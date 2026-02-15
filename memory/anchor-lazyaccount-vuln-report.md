# Vulnerability Report: Anchor LazyAccount [T; N]::size_of Bug

**Date Found:** February 10, 2026
**Found By:** Claude Code (directed by Gabe/Aria), during Superteam Audit Bounty
**Status:** LOCAL ONLY — responsible disclosure pending

## Summary
- **File:** `lang/src/lazy.rs:66-73` (Anchor framework)
- **Severity:** High (framework-level silent data corruption)
- **Affected:** Any Anchor program using `LazyAccount` with a struct containing `[T; N]` where T is dynamically-sized (String, Vec, Option)

## Root Cause
`[T; N]::size_of()` computed `N * T::size_of(buf)` — measuring only the first array element and multiplying by N. When elements have different serialized sizes (which is always possible for String, Vec, etc.), the returned size is wrong.

## Impact
LazyAccount's field-level loading (`load_<field>()`, `load_mut_<field>()`) uses `Lazy::size_of` to calculate byte offsets. When the array size is wrong, all fields after the array are loaded from incorrect offsets:

1. **Silent data corruption:** On mut accounts, `exit()` serializes a mix of correctly and incorrectly loaded fields, permanently corrupting on-chain account data
2. **Incorrect constraint validation:** `has_one` constraints checking fields after the array compare against wrong values
3. **Wrong state reads:** Any field-level load after the array returns garbage data

## Example
```rust
#[account]
struct MyAccount {
    pub names: [String; 3],    // dynamically-sized array
    pub authority: Pubkey,      // field AFTER the array
}
```

If `names = ["a", "hello", "world!"]`, `size_of` returns `3 * 5 = 15` instead of `5 + 9 + 10 = 24`. Loading `authority` reads from offset `disc + 15` instead of `disc + 24` — 9 bytes into the wrong position.

## Proof of Concept
```rust
#[test]
fn array_of_unsized_elements() {
    let val: [String; 3] = [
        String::from("a"),    // borsh: 4 + 1 = 5 bytes
        String::from("ab"),   // borsh: 4 + 2 = 6 bytes
        String::from("abc"),  // borsh: 4 + 3 = 7 bytes
    ];
    let serialized = borsh::to_vec(&val).unwrap();
    // BUG: size_of returns 3 * 5 = 15, actual is 18
    assert_ne!(<[String; 3]>::size_of(&serialized), serialized.len());
}
```

Test passes on `anchor-lang v0.32.1` with `--features lazy-account`.

## Fix
```rust
fn size_of(buf: &[u8]) -> usize {
    if T::SIZED {
        N * T::size_of(buf)  // Fast path for fixed-size (unchanged)
    } else {
        (0..N).fold(0, |acc, _| acc + T::size_of(&buf[acc..]))
    }
}
```

All 20 existing tests pass with fix applied. New test cases cover: different-length strings, different-length vecs, same-length strings, empty strings, and nested dynamic types (Vec<String>).

## Git Info
- Repo: `solana-foundation/anchor` (cloned to `~/anchor` on Mac)
- Branch: `fix/lazy-array-size-of`
- Commit: `fd47df04` (LOCAL ONLY, not pushed)
- Diff: 41 insertions, 1 deletion in `lang/src/lazy.rs`

## Disclosure Plan
1. ✅ PoC confirmed, fix verified, report written
2. ⬜ Submit to Superteam audit bounty (private submission, $3K prize)
3. ⬜ Contact Anchor team privately (security contact or Discord)
4. ⬜ Submit PR only AFTER Anchor team acknowledges
5. ⬜ If Anchor has Immunefi bounty, submit there too

## Mitigating Factor
LazyAccount is behind a feature flag (`lazy-account`) and relatively new. The number of programs actually using `LazyAccount` with dynamic-type arrays may be small. However, as adoption grows, this becomes increasingly dangerous since the corruption is **silent** — no errors, no panics, just wrong data.

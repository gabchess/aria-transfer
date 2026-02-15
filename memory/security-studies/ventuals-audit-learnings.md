# Ventuals Audit Learnings

## Gabe's Two Rejected Submissions (Jan 30, 2026)

### #265 (Critical): Exchange rate inflation via same-block claim + batch
**Rejection reason:** Protocol already has protection. `claimWithdraw()` calls `spotSend()` which sets `lastSpotBalanceChangeBlockNumber = block.number`. Both `processBatch()` and `finalizeBatch()` read `spotBalance()` which enforces `block.number > lastSpotBalanceChangeBlockNumber`. Same-block calls revert with `CannotReadSpotBalanceUntilNextBlock()`.

**Lesson:** The one-block delay protection is comprehensive. Every function that modifies Core state updates the block tracker, and every function that reads Core state checks it. You MUST trace the full call chain before submitting.

### #266 (High): Batch processing DoS via minimum-sized withdrawals
**Rejection reason:** Duplicate of known audit finding M-4 (Head-of-Line blocking). Mitigation already implemented: `processBatch(uint256 numWithdrawals)` accepts a limit parameter. Plus, flooding requires locking real capital.

**Lesson:** Always check for PRIOR AUDIT REPORTS before submitting. The Nethermind audit (7.3 + 7.5) already found this exact issue class and it was fixed. Don't submit known issues.

---

## Nethermind Audit (Oct 8, 2025) - Key Findings (ALL FIXED)

### Original Contract Issues:
1. **[High] Inflation attack** - donate HYPE via receive() to inflate exchange rate. **Fix: removed receive/fallback, internal accounting**
2. **[High] transferHype without burn** - withdraw HYPE without burning vHYPE. **Fix: removed transferHype function**
3. **[Medium] Decimal conversion loss** - 18/8 truncation. **Fix: minimum deposit granularity (% 1e10 == 0)**
4. **[Medium] Manager role bypass** - external EOA could bypass timing checks. **Fix: moved timing logic to StakingVault**
5. **[Low] Stuck tokens** - GenesisVaultManager had receive/fallback. **Fix: removed**
6. **[Low] vHYPE burn inflation** - user burns to inflate rate. **Fix: burn removed**

### New Version Issues (StakingVaultManager):
7. **[Critical] Claim before finalization** - batchIndex not checked, finalizedAt=0 passes. **Fix: check batchIndex != max and finalizedAt != 0**
8. **[Medium] DoS via small withdrawals** - no minimum amount. **Fix: minimumWithdrawAmount added**
9. **[Medium] No limit on single withdrawal** - blocks queue. **Fix: auto-split large requests**
10. **[Medium] applySlash timing** - claim before slash applied. **Fix: two-step mechanism**
11. **[Medium] Truncation in finalizeBatch** - depositsInBatch not truncated. **Fix: enforce % 1e10 == 0**
12. **[Info] Delegation timing inconsistency** - +1 block difference. **Fix: consistent timing**
13. **[Best Practice] Increase timing checks** - ensure calls don't silently fail. **Fix: increased margins**

### Critical Insight from Audit:
- StakingVault tests show `test_CannotFallback()` and `test_CannotReceive()` - direct HYPE transfers to StakingVault REVERT
- This kills the classic inflation attack path via direct donation
- BUT: can HYPE be force-sent on HyperEVM via selfdestruct? This is chain-specific and auditors might not have tested it

---

## What This Means for Our Search

**Everything obvious is found and fixed.** 281 other researchers + 2 professional audit firms have covered the low-hanging fruit.

### Where to Look Now:
1. **Fix completeness** - Did the fixes actually close all attack vectors? Check the fix commits.
2. **Fix interactions** - Did fixing one thing create another issue?
3. **UUPS proxy risks** - Upgrade pattern edge cases
4. **HyperEVM-specific behavior** - selfdestruct, coinbase, precompile edge cases that Nethermind/Zenith might not have tested
5. **The NEW code** - StakingVaultManager is 600+ LOC of complex batch logic. Post-fix code may have new bugs.
6. **Business logic** - edge cases in batch lifecycle (process -> finalize -> claim -> slash interactions)
7. **Integer overflow/underflow** - in the exchange rate calculations with very large/small values
8. **Cross-block MEV** - timing between EVM and Core across multiple blocks (not just same-block)

### Strategy:
- Clone the repo
- Run the existing test suite
- Use Claude Code to systematically fuzz edge cases
- Focus on the FIXED code, not the original bugs
- Test HyperEVM-specific behaviors that can't be tested in standard Foundry

---

## Zenith Audit (Oct 14, 2025)
- 2.6MB PDF, not yet extracted
- Likely contains the "M-4 Head-of-Line blocking" finding referenced by triager
- Need to extract and compare with Nethermind findings

## Prior Audit Report Links
- Nethermind: `docs/audits/20251008_nethermind.pdf` (saved locally)
- Zenith: `docs/audits/20251014_zenith.pdf` (saved locally)
- Both available at: `github.com/ventuals/ventuals-contracts/tree/main/docs/audits`

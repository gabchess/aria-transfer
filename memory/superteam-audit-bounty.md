# Superteam Audit Bounty — Attack Plan

## Bounty Details
- **Prize:** $3,000 USDG (1st: $1,500, 2nd: $1,000, 3rd: $500)
- **Deadline:** ~Feb 15, 2026
- **Agent API:** Use aria-solguard agent (secrets/superteam-agent.json)
- **Listing slug:** fix-open-source-solana-repos-agents

## Judging Criteria (priority order)
1. Severity and real-world impact
2. Quality of verification proof
3. Correctness and completeness of the fix
4. Target repo popularity

## TARGET: orca-so/whirlpools (Pinocchio Layer)
- **Why:** 11-day-old pinocchio layer manually reimplements Anchor safety using raw Solana primitives
- **Program ID:** whirLbMiicVdio4qvUfM5KAg6Ct8VwpYzGff3uctyCc
- **Immunefi backup:** $500K bounty if critical finding
- **Cumulative probability of finding at least 1 issue:** ~85-90%

### Top Attack Vectors
1. **Liquidity Math Porting Discrepancy (60%)** — diff pinocchio vs Anchor, look for missing checked_* ops
2. **Account Validation Gaps (55%)** — map #[account(...)] constraints to pinocchio verify_* equivalents
3. **State Struct Layout Mismatch (45%)** — C-repr padding vs Borsh serialization differences
4. **Transfer Fee Edge Cases (35%)** — 100% fee rate, inconsistent unwrap vs fallback
5. **Reposition Liquidity Atomicity (25%)** — slippage validation ordering

### Critical Files
- pinocchio/ported/manager_liquidity_manager.rs (PRIMARY TARGET)
- pinocchio/instructions/decrease_liquidity_v2.rs
- pinocchio/instructions/increase_liquidity_v2.rs
- pinocchio/instructions/reposition_liquidity_v2.rs
- pinocchio/ported/util_token.rs
- pinocchio/state/whirlpool/ (all #[repr(C)] structs)

### Phases
1. Setup + Automated Scanning (3-4h) — clone, diffs, clippy, grep patterns
2. Manual Code Review (8-12h) — constraint comparison, math diff, token/fee review
3. Deep Dive on Findings (6-8h) — trigger conditions, reachability, impact
4. PoC Development (8-12h) — Rust integration test proving exploit
5. Fix + PR (4-6h) — regression test, coordinated disclosure

### Immunefi Dual-Submission Strategy
**Orca Immunefi Bounty:** immunefi.com/bug-bounty/orca/
- Critical: $100K-$500K (10% of funds affected)
- High: $50,000 flat
- Medium: $10,000 flat
- No KYC, paid in USDC/ORCA
- PoC required (code), all testing on local forks only

**Disclosure Order:**
1. Find vuln + write PoC
2. Submit to Immunefi FIRST (private)
3. Wait for Orca to acknowledge (24-48h)
4. Submit writeup to Superteam (doc link, NOT public PR yet)
5. After Orca patches, submit public PR

**Combined payout potential:**
- Medium: $10K Immunefi + $1.5K Superteam = $11,500
- High: $50K + $1.5K = $51,500
- Critical: $100K-$500K + $1.5K = $101,500+

### Trident Connection
- DM relationship with Trident founder (Ackee Blockchain)
- Offered direct access to their tech team for questions
- Tutorial videos shared for Anchor fuzzing

## Vulnerability Checklist (Top 45 from Zealynx)
### Critical
- Missing signer checks (pubkey without is_signer) — Wormhole $326M
- Missing ownership validation — Cashio $48M  
- Unsafe CPI (forwarding signers to untrusted programs)
- Integer overflow (release builds don't check!)
- Insecure initialization (anyone can call init)
- Arbitrary CPI (user-supplied program ID)

### High
- PDA seed collisions (shared PDAs between users)
- Account not reloaded after CPI
- Accounts closed without zeroing data (revival attack)
- Duplicate mutable accounts not checked
- Authority transfer without two-step pattern
- CPI signer forwarding to untrusted programs

### Medium
- Account data reallocation issues
- Bump seed canonicalization (user-provided bumps)
- Token-2022 extension edge cases

## Stats (Sec3 2025 Report)
- 163 audits → 1,669 vulns
- 51% had HIGH+ severity
- Top categories: Business Logic (37%), Input Validation (28%), Access Control (21%)
- Average 10.3 findings per audit, 1.4 high/critical per review

## Tools
- **Trident Fuzzer** (github.com/Ackee-Blockchain/trident)
  - Install: `cargo install trident-cli`
  - 12K tx/sec, stateful fuzzing, flow-based sequences
  - Built by Ackee Blockchain (audited Lido, Safe, Axelar), Solana Foundation funded
  - Docs: ackee.xyz/trident/docs/latest/
  - @TridentSolana on X (DM'd us, building relationship)
  - Use for: automated edge case discovery, crash = potential vuln

## Key Resources
- Zealynx 45-point checklist: zealynx.io/blogs/solana-security-checklist
- Sec3 2025 report: solanasec25.sec3.dev
- Sealevel attacks: github.com/coral-xyz/sealevel-attacks
- Helius guide: helius.dev/blog/a-hitchhikers-guide-to-solana-program-security
- Cantina guide: cantina.xyz/blog/securing-solana-a-developers-guide
- Solana vuln gist: gist.github.com/zfedoran/9130d71aa7e23f4437180fd4ad6adc8f

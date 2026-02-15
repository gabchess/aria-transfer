# Smart Contract Security Deep Research — Feb 4, 2026

## Session: Research with Gabe (manual study, not cron)

---

## 1. OWASP Smart Contract Top 10 (2025 Edition)

Based on 149 incidents in 2024, $1.42 billion in losses:

| Rank | Vulnerability | 2024 Losses | Key Change vs 2023 |
|------|-------------|-------------|---------------------|
| 1 | Access Control | $953.2M | Was #4, now #1 by FAR |
| 2 | Price Oracle Manipulation | $8.8M | New to top 3 |
| 3 | Logic Errors | $63.8M | Was #7 |
| 4 | Lack of Input Validation | $14.6M | NEW entry |
| 5 | Reentrancy | $35.7M | Was #1, dropped to #5 |
| 6 | Unchecked External Calls | $550.7K | Was #10 |
| 7 | Flash Loan Attacks | $33.8M | NEW entry |
| 8 | Integer Overflow/Underflow | - | Was #2, declining (Solidity 0.8+) |
| 9 | Insecure Randomness | - | Was #8 |
| 10 | Denial of Service | - | Was #6 |

**Key insight:** Access control is now the #1 cause by a massive margin ($953M vs $64M for #2). This isn't about reentrancy anymore. The game has shifted to permissions, roles, and operational security.

Source: https://owasp.org/www-project-smart-contract-top-10/

---

## 2. Major 2025 Hacks (Post-Mortems)

### Bybit — $1.38 BILLION (Feb 2025)
**The biggest exchange hack in history.**
- **Attacker:** Lazarus Group (North Korea), FBI confirmed
- **Method:** Compromised a Safe{Wallet} developer machine, injected malicious JavaScript
- **Technical flow:**
  1. Deployed two malicious smart contracts
  2. Submitted tx (0x46de...7882) that altered proxy contract storage state
  3. Replaced legitimate implementation contract (0x34Cf...3F5F) with hacker's (0xbDd0...9516)
  4. Used `sweepERC20()` and `sweepETH()` to drain all funds
- **Stolen:** 401,346 ETH (~$1.08B) + 90,375 stETH (~$242M) + 8,000 mETH (~$22.5M) + 15,000 cmETH (~$42M)
- **Laundering:** 10K ETH increments, cross-chain bridges, DEXes, mixers (20+ entities)
- **Lesson:** Even with multisig (Safe), if the UI is compromised, signers can't verify what they're signing. Pre-signing simulation (Tenderly, OpenZeppelin Defender) would have caught it.
- **Vulnerability class:** Supply chain attack + proxy pattern exploitation
- Sources: lukka.tech, nccgroup.com, sygnia.co, FBI PSA

### Balancer — $70-128M (Nov 2025)
- **Method:** Rounding errors in stable pool math, amplified via high-frequency batch swaps
- **How:** Each swap had <1 wei rounding error. Attacker structured hundreds/thousands of batch swaps where rounding consistently favored them. Tiny errors compounded into massive value extraction.
- **What audits missed:** Individual swaps were tested (fine), but nobody tested sequences of 1000+ swaps. No invariant asserted over N repeated operations.
- **Lesson:** AMM math must be correct under adversarial conditions. Test repeated operations, not just single ops. Formal verification for rounding error bounds.
- **Vulnerability class:** Mathematical precision error in AMM formulas

### GMX — $42M (Jul 2025)
- **Method:** System boundary failure between oracle updates, margin calculations, and liquidation logic
- **How:** Exploited timing between component interactions (oracle price update + margin recalculation + liquidation trigger). $9.6M bridged to Ethereum immediately.
- **What audits missed:** Components audited in isolation passed. The bug existed in the SPACE BETWEEN components.
- **Lesson:** Integration testing with adversarial simulation. Well-audited components can still fail when composed.
- **Vulnerability class:** System integration failure

### Cork Protocol — $12M (May 2025)
- **Method:** Mishandled wstETH value accrual
- **How:** Protocol assumed static 1:1 wstETH:ETH relationship. But wstETH accrues staking rewards over time, changing the exchange rate. Attacker deposited when favorable, withdrew more than deposited.
- **Lesson:** ALWAYS use the token's exchange rate functions (e.g., `wstETH.getStETHByWstETH()`). Never assume 1:1 for any LSD.
- **Vulnerability class:** Token mechanics misunderstanding

### Yearn Finance — $9.3M (Dec 2025)
- **Two exploits in one month:**
  1. **$9M:** Economic invariant violation in legacy yETH stableswap. Share calculation logic could be manipulated to mint near-infinite yETH. ~1000 ETH to Tornado Cash.
  2. **$300K:** V1 contracts still held funds. First exploit drew attention to legacy infrastructure.
- **Lesson:** Static analysis and fuzzers don't verify economic invariants. Need differential fuzzing against reference implementations. Legacy contracts need deprecation strategies.
- **Vulnerability class:** Economic invariant violation + legacy contract risk

### Bunni — $2.4-8.3M (Sep 2025)
- **Method:** Precision/rounding bug in LP accounting
- **How:** Repeated deposits/withdrawals where each operation extracted tiny amounts via rounding. Millions accumulated over many operations.
- **Lesson:** Use established math libraries (PRBMath, ABDKMath). Test SEQUENCES of operations, not just single ones. Higher internal precision even if external is standard.
- **Vulnerability class:** Precision error accumulation

### Garden Finance — $5.5M (Oct 2025)
- **Method:** Exploit on one chain, cross-chain bridges to escape/launder
- **Lesson:** Multi-chain deployments need cross-chain threat models and monitoring.
- **Vulnerability class:** Multi-chain attack pattern

### Nemo Protocol — $2.4M (Sep 2025)
- **Method:** Economic logic error on Sui (Move language)
- **Lesson:** "Safe" languages prevent SOME bugs but don't address economic logic flaws.
- **Vulnerability class:** Economic logic error in non-EVM ecosystem

---

## 3. Cantina Competition Intelligence

### How Cantina Works
- Crowdsourced code review competitions (like Code4rena, Sherlock)
- 5,000+ researchers, reputation-based scoring
- Blends team-based managed reviews with open competitions
- Prize pots typically $50K-$1.25M
- Findings scored by severity: Critical > High > Medium > Low/Informational
- 10% pot usually reserved for Low/Gas findings
- **Mandatory POC rule** applies for most competitions

### Notable Past Competitions
| Competition | Reward | Findings | Date |
|------------|--------|----------|------|
| Euler v2 | **$1,250,000** | 600+ participants | Mar 2024 |
| Story Protocol | - | 977 findings | Dec 2024 - Jan 2025 |
| Infrared Finance | - | 680 findings | Jan 2025 |
| Superform v2 | $50,000 | 610 findings | May 2025 |
| Morpho Blue | - | - | 2024 |
| MetaMorpho | - | - | 2024 |
| Morpho Vaults v2 | - | - | 2025 |

### Top Auditors on Cantina (named in docs)
- **0xRajeev** — consistently high-performing
- **Haxatron** — strong track record
- **shotes** — public profile with major findings
- **Christoph Michel** — well-known security researcher

### Morpho Blue Cantina Report
- Full PDF available: `github.com/morpho-org/morpho-blue/audits/2024-01-05-morpho-blue-cantina-competition.pdf`
- Study this as a reference for what winning submissions look like

### Competition Strategy (from JohnnyTime guide)
1. **Start with smaller contests** before major ones
2. **Study past audit reports** from completed competitions
3. **Collaborate** with others in security communities
4. **Automate repetitive tasks** with scripts and tools
5. **Be persistent** — top auditors weren't born, they ground

### All Competition Platforms
1. **Cantina** (cantina.xyz) — team + crowdsource hybrid
2. **Code4rena** (code4rena.com) — "Wardens" (now acquired by Zellic)
3. **Sherlock** (sherlock.xyz) — requires staking your own funds
4. **CodeHawks** (codehawks.cyfrin.io) — by Cyfrin (Patrick Collins)
5. **Immunefi** (immunefi.com) — bug bounties on live projects
6. **Hats Finance** (app.hats.finance) — decentralized, on-chain managed

### Tracking Competitions
- **DailyWarden.com** — aggregates latest competitions across all platforms

---

## 4. Learning Resources & Tools

### Hands-On Practice
- **DeFiHackLabs** (github.com/SunWeb3Sec/DeFiHackLabs) — Foundry-based reproduction of 280+ real hacks. Active community. Updated regularly.
- **DeFiVulnLabs** (github.com/SunWeb3Sec/DeFiVulnLabs) — 48 vulnerability types implemented in Foundry. Great for beginners.
- **SmartContractHacking.com** — structured course based on real DeFi exploits

### Databases & Reports
- **rekt.news/leaderboard** — 280+ hacks documented with post-mortems
- **SolidityScan Web3HackHub** (solidityscan.com/web3hackhub) — hack database since 2011
- **Halborn Top 100 DeFi Hacks 2025** — comprehensive annual report
- **Hacken 2025 Yearly Security Report** — breach analysis + regulatory trends

### Key Stats (2025)
- **$3.9B total smart contract hack losses** in 2025 (ainvest.com)
- **34.6% of direct contract exploits** = faulty input validation (Halborn)
- **Access control = #1 by dollar amount** ($953M in 2024 alone)
- **AI agents found $4.6M in exploits** during testing (Anthropic red team research)

---

## 5. Emerging Patterns for 2025-2026

### What's Changing
1. **Access control > reentrancy** — the game has shifted from code bugs to permission mismanagement
2. **Cross-component exploits** — individual audits pass, system integration fails (GMX pattern)
3. **Economic model attacks** — math correctness under adversarial conditions (Balancer, Yearn)
4. **Supply chain attacks** — compromise the dev tooling, not the contract (Bybit via Safe{Wallet})
5. **Multi-chain complexity** — cross-chain bridges as escape routes (Garden Finance)
6. **LSD handling errors** — protocols assuming static exchange rates for rebasing/wrapped tokens
7. **Legacy contract risk** — old contracts on-chain = ongoing attack surface
8. **"Safe" language false sense of security** — Move/Sui doesn't prevent economic logic bugs

### What Auditors Should Look For in 2026
1. Admin key management and multisig operational security
2. Economic invariant testing across all operation sequences
3. Rounding/precision errors in repeated operations
4. Cross-component timing attacks
5. LSD/wrapped token exchange rate handling
6. Cross-chain attack vectors
7. Legacy/deprecated contract migration plans
8. Oracle manipulation in edge cases

---

## 6. Cantina Path for Gabe

### Immediate Next Steps
1. **Create Cantina account** — sign up, browse completed competitions
2. **Read Morpho Blue competition report** (PDF on GitHub) — see what winning findings look like
3. **Install Foundry** — all practice repos use it
4. **Clone DeFiVulnLabs** — work through 48 vulnerability types
5. **Clone DeFiHackLabs** — reproduce 5-10 real hacks

### Skill Building Path
1. Foundry basics (forge test, fork testing)
2. Reproduce 5 classic hacks (reentrancy, flash loan, access control, oracle, precision)
3. Study 3 completed Cantina/Code4rena reports in detail
4. Enter first small competition (CodeHawks or Hats Finance)
5. Build up to Cantina competitions

### Timeline Estimate
- **Week 1-2:** Foundry + DeFiVulnLabs (basics)
- **Week 3-4:** DeFiHackLabs (reproduce real hacks)
- **Month 2:** First competition entry
- **Month 3+:** Consistent competition participation

---

---

## 7. Balancer V2 Deep Dive (Check Point Research)

**Date:** November 3, 2025
**Loss:** $128.64M across 6 chains in under 30 minutes
**Source:** Check Point Research (research.checkpoint.com)

### Architecture Context
- Balancer V2 uses a centralized **Vault** contract (0xBA12222...BF2C8) holding ALL tokens across ALL pools
- Separates token storage from pool logic (gas efficiency)
- This shared design meant one bug in pool math = all ComposableStablePools vulnerable simultaneously
- Internal Balance system: users deposit once, use across multiple operations without repeated ERC20 transfers

### Root Cause: Precision Loss in `_upscaleArray`
```solidity
function _upscaleArray(uint256[] memory amounts, uint256[] memory scalingFactors)
    private pure returns (uint256[] memory) {
    for (uint256 i = 0; i < amounts.length; i++) {
        amounts[i] = FixedPoint.mulDown(amounts[i], scalingFactors[i]);
    }
    return amounts;
}
```
- `mulDown` performs integer division that rounds DOWN
- When balances are small (8-9 wei range), this creates **up to 10% precision loss per operation**
- Precision error propagates to invariant D calculation
- BPT price = D / totalSupply, so reduced D = lower BPT price = arbitrage opportunity

### The Three-Phase Attack Pattern
1. **Adjustment:** Swap large amounts of BPT for underlying tokens, pushing one token's balance to the critical 8-9 wei threshold
2. **Trigger:** Execute small swaps involving the boundary-positioned token. `_upscaleArray` rounds down, causing invariant D to be underestimated, BPT price drops artificially
3. **Extract:** Mint/purchase BPT at suppressed price, immediately redeem at full value. Price discrepancy = pure profit.

This cycle repeated **65 times within a single `batchSwap` transaction.**

### Attack Execution
- **Three addresses:** Deployer (0x506D...), Exploit Contract (0x54B5...), Recipient (0xAa76...)
- **Constructor-based attack:** The theft happened DURING contract deployment. Constructor auto-executed all 65 micro-swaps.
- **Stage 1 (Constructor):** batchSwap against two pools = $63M drained, stored in contract's internal balance
  - Pool 1 (osETH/wETH-BPT): +4,623 WETH, +6,851 osETH
  - Pool 2 (wstETH-WETH-BPT): +1,963 WETH, +4,259 wstETH
- **Stage 2 (Withdrawal):** Function `0x8a4f75d6` transferred internal balance to recipient using `manageUserBalance`

### What Audits Missed
- Individual swaps tested = fine. Nobody tested 65 swaps in sequence.
- Rounding errors measured as <1 wei per swap, dismissed as negligible
- No invariant assertion over N repeated operations
- Fuzzers without stateful sequence modeling couldn't discover this

### Key Lessons for Auditors
1. **Test cumulative effects** of adversarial batch operations, not just single ops
2. **Rounding direction matters** in every math operation, check what happens when attacker controls direction
3. **Boundary conditions** (8-9 wei range) are where precision loss is maximized
4. **Constructor attacks** bypass some monitoring since theft happens at deployment time
5. **Internal balance systems** can hide fund movements from simple transfer monitoring
6. **Formal verification of rounding bounds** under any sequence of operations is essential

---

## 8. Morpho Blue Cantina Competition Structure

### Competition Details
- **Dates:** Nov 13 to Dec 4, 2024
- **Total Prize Pool:** $200,000 ($100K Morpho Blue + $100K MetaMorpho)
- **Codebase:** 847 LOC total (src/Morpho.sol = 325 LOC main)
- **Scope:** All files in src except mocks

### Scoring System
- High Severity = 10 points
- Medium Severity = 3 points
- Prizes distributed proportionally to points scored
- **Duplicate penalty:** Each duplicate scaled by `0.9^(n-1) / n` where n = number of duplicates
- **Unique findings incentivized** by the duplicate formula
- Low/Informational: Top 5 ranked ($5K, $2.5K, $1.25K, $625, $625)

### Key Takeaway for Strategy
- **Finding UNIQUE bugs is exponentially more valuable** than finding duplicates
- 847 LOC is tiny. Every line can be read manually.
- Severity matrix: Impact (High/Med/Low) x Likelihood (High/Med/Low)
- Previous OpenZeppelin/Cantina findings are OUT OF SCOPE

---

## 9. Live Cantina Bounties Analysis (Feb 2026)

### Option A: Ventuals Bug Bounty
**URL:** cantina.xyz/bounties/d1c3592e-f335-4235-94ac-c6bf6cdb50a3
**Max Reward:** Up to $1M (Critical)
**Protocol:** vHYPE, a HYPE liquid staking token on Hyperliquid
**Codebase:** 4 contracts (StakingVault, StakingVaultManager, vHYPE, RoleRegistry)
**Architecture:** UUPS upgradeable proxies

**Why it's interesting:**
1. **TINY scope** (4 contracts) = can read every line of code
2. **Niche chain** (Hyperliquid) = fewer experienced researchers
3. **Known timing issue:** HyperEVM vs HyperCore L1Read desync (one-block delay)
4. **LST pattern** = matches Cork Protocol vulnerability class ($12M loss on LSD handling)
5. **Withdrawal queue + batch processing** = classic area for edge cases
6. **Exchange rate manipulation** potential (deposit/withdraw at wrong rate)
7. Access control (3 roles: OWNER, MANAGER, OPERATOR)
8. Slashing logic edge cases

**Key attack surfaces:**
- Exchange rate calculation (totalBalance relies on both HyperEVM + HyperCore balances)
- One-block delay between HyperEVM transfer and HyperCore balance update
- Batch processing: what if processBatch and finalizeBatch are called in edge cases?
- Minimum stake enforcement: can it be bypassed?
- Withdrawal splitting: large withdraws split into smaller ones, any rounding?
- Slashing: applySlash only by OWNER, but timing matters
- vHYPE burn (anyone can burn = donate to vault, any edge case?)

### Option B: OKX DEX Bug Bounty ($1M)
**Status:** Just launched ~5 days ago
**Scope:** Multi-chain DEX routing, mainnet contracts
**Profile:** Very high, lots of attention and competition
**Codebase:** Likely much larger than Ventuals

### Option C: Major Bounties (for reference)
- **Coinbase:** $5M max (all onchain products on Base)
- **Euler:** $7.5M max (modular lending, AMM, strategies)
- **Uniswap:** Largest in Web3 (v4 + hooks + multi-chain)
- **Morpho:** Active across ETH, Base, OP, Arbitrum

### RECOMMENDATION: Ventuals
1. Smallest scope = highest chance of finding something as beginners
2. Hyperliquid is niche = less competition from top auditors
3. The HyperEVM/HyperCore timing issue is EXPLICITLY documented as a concern
4. LST pattern directly maps to vulnerability classes we studied (Cork $12M)
5. Up to $1M for Critical is excellent payout
6. Open-source architecture doc makes preparation easier

## Sources
- OWASP SC Top 10: owasp.org/www-project-smart-contract-top-10/
- Coinmonks "Audited, Tested, Still Broken": medium.com/coinmonks/audited-tested-and-still-broken...
- Hacken Top 10 Vulnerabilities: hacken.io/discover/smart-contract-vulnerabilities/
- Bybit analysis: lukka.tech, nccgroup.com, sygnia.co
- JohnnyTime competition guide: medium.com/@JohnnyTime
- DeFiHackLabs: github.com/SunWeb3Sec/DeFiHackLabs
- Cantina blog: cantina.xyz/blog
- Halborn report: halborn.com/reports/top-100-defi-hacks-2025

# Cantina Bug Bounty Targets

## Target 1: Euler V2 ($7.5M total)

### Reward Tiers
| Component | High | Medium |
|-----------|------|--------|
| **Core (EVK/EVC/EPO)** | Up to $5M (min $200K) | Up to $200K (min $50K) |
| **Core + Usual USL vaults** | Up to $7.5M | Up to $200K |
| **Euler Earn** | Up to $500K (min $100K) | Up to $100K (min $25K) |
| **EulerSwap** | Up to $250K (min $50K) | Up to $50K (min $10K) |
| **Supporting (Fee Flow, Reward Streams)** | Up to $100K (min $25K) | Up to $25K (min $5K) |
| **Web Interface** | $25K (Critical) / $5K (High) | $1K |

Rewards = 10% of economic impact.

### Repos in Scope
1. **Ethereum Vault Connector (EVC)** - github.com/euler-xyz/ethereum-vault-connector
2. **Euler Vault Kit (EVK)** - github.com/euler-xyz/euler-vault-kit
3. **Euler Price Oracle (EPO)** - github.com/euler-xyz/euler-price-oracle
4. **Reward Streams** - github.com/euler-xyz/reward-streams
5. **Fee Flow** - github.com/euler-xyz/fee-flow
6. **Euler Earn** - github.com/euler-xyz/euler-earn
7. **EulerSwap** - github.com/euler-xyz/euler-swap (NEWEST - best target)

### Best Attack Vectors for Claude Code
1. **EulerSwap** (newest code, JIT liquidity = complex math, AMM + lending hybrid)
2. **Euler Earn** (risk curation, deposit allocation across strategies, ERC4626)
3. **Euler Price Oracle** (oracle adapters, Chainlink/Pyth/RedStone integration)
4. **EVK** (core lending, liquidation math, cross-collateral via EVC)

### Key Info
- No deposit required
- 366 findings submitted already
- Testing: local forks only (Foundry recommended)
- No public disclosure without consent
- Solidity/EVM (not Rust/Solana)

### Claude Code Strategy
- EulerSwap is the newest and most complex (JIT liquidity, AMM curves)
- Focus on math errors in swap calculations, liquidation edge cases, oracle manipulation
- Look at ERC4626 share/asset conversion rounding issues
- Cross-vault interactions via EVC are unique attack surface

---

## Target 2: Morpho ($2.5M)
- URL: cantina.xyz/bounties/35a5f0a1-2ffd-432c-8f3b-77d169add8c3
- No deposit required
- Need to pull full scope details

---

## Euler V1 Exploit History (CRITICAL CONTEXT for Claude Code)

### The $200M Hack (March 13, 2023)
- **Root cause:** Missing liquidity/health check on `donateToReserves()` function
- **Attack pattern:** 
  1. Flash loan 30M DAI
  2. Deposit 20M as collateral → get ETokens
  3. Mint 10x leverage (self-collateralized loans, 95% max LTV)
  4. Repay some debt to increase health score
  5. Mint again to maximize position
  6. **Donate 100M ETokens to reserves** (no health check!)
  7. Account becomes insolvent → self-liquidate at dynamic discount
  8. Liquidator contract profits from discounted collateral
- **Key insight:** New feature added AFTER audit didn't have the same checks as other functions
- **Lesson for V2 audit:** Look for NEW functions/features that may lack checks present in older code

### V2 Security Posture
- 40+ independent audits by 16 firms, $4M spent on security
- $1.25M Cantina security challenge: NO critical vulns found
- $3.5M Live CTF on core protocol (Hats Finance)
- $500K Live CTF on EulerSwap (Cantina, June 2025)
- 366 bounty submissions so far
- **Implication:** Low-hanging fruit is GONE. Need to find subtle math/logic bugs.

### Strategic Attack Vectors for Claude Code
Based on V1 exploit patterns + V2 architecture:

1. **New features lacking inherited checks** (V1 pattern: donateToReserves)
   - EulerSwap JIT liquidity is newest → check if health/solvency checks match older code
   - Euler Earn allocation logic → verify withdrawal/deposit checks

2. **Cross-vault interactions via EVC** (unique to V2)
   - Multi-collateral positions across vaults
   - Liquidation cascades across connected vaults
   - Operator permissions and delegations

3. **ERC4626 share price manipulation**
   - Donation attacks on vault share price
   - Rounding errors in share/asset conversions
   - First depositor manipulation

4. **Oracle edge cases**
   - Stale price handling across different oracle types
   - Price discrepancies between adapters
   - EulerRouter fallback logic

5. **EulerSwap-specific** (NEWEST, launched May 2025)
   - JIT liquidity borrow/repay within single swap
   - AMM curve math with concentrated liquidity
   - Single-sided liquidity withdrawal edge cases
   - Interaction between swap fees and lending interest

## Notes
- Euler is EVM/Solidity, Anchor was Rust/Solana. Claude Code handles both.
- Our Cantina profile: anfoxcode (rep: 50)
- Already submitted: 4 OKX findings, 1 Symbiotic finding
- **Vibecoding rule:** ALWAYS Plan Mode first. Read vibecoding-best-practices.md before starting.
- **Claude Code prompt strategy:** Give full context (V1 exploit pattern, V2 architecture, specific attack vectors)

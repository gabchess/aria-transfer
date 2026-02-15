# Octant v2 Technical Overview (from file_3)

## The Seven Pillars (Priority Order)
1. Financial Sustainability & Security
2. R&D Lab, Organization & Culture
3. Brand, Legitimacy & Narrative
4. Sustainable Growth Funding Model
5. OADE Experimentation Surface
6. Vault & Capital Infrastructure Layer
7. R&D Network & Ecosystem Growth

## What is Octant v2?
Open public infrastructure for sustainable growth. Smart contracts + tooling for routing on-chain yield to projects and funding mechanisms.

**Pipeline:** Users deploy assets → Vaults → DeFi strategies → Yield generated → Yield redirected to funding streams (not back to user) → Community allocates

**Key insight:** Principal stays intact. Only yield funds ecosystem. "Kickstarter powered by yield."

## Core Components

### Funding Vaults (ERC-4626 compliant)
- **Yield Donating Vaults:** Non-rebasing tokens (USDC, DAI). Mints yield shares to donation address.
- **Yield Skimming Vaults:** Rebasing tokens (stETH, rETH). Handles token dilution.
- **Multistrategy Vaults:** Multiple strategies, voluntary lockup, multiuser.
- **Dragon (SAFE) Vaults:** Safe multisig integration for large treasuries.

### Routing & Splitting
- **Payment Splitter:** Distributes yield shares to multiple recipients (grants, OpEx, community incentives)
- **Donation Address:** Destination for yield distributions

### Allocation Mechanisms
- Quadratic funding rounds
- Quadratic voting
- Token-weighted voting
- Custom models

### Community Staking
- Stake ecosystem tokens → earn rewards → participate in allocation decisions
- Token-gated, token-weighted participation

## Key Design Principles
- **Capital Preservation:** Principal never distributed, only yield
- **Credible Neutrality:** Protocol doesn't prescribe what to fund, standardizes how
- **Sustainability:** Continuous yield-based streams, no reserve depletion
- **Composability:** ERC-4626, Safe multisig, DeFi primitives (Aave, Morpho, Sky)
- **Security:** Yearn v3-style patterns, additional audits, configurable exposure limits
- **Trust Minimized:** On-chain consensus, minimal admin

## Contribution Models
1. **Regenerative:** Deploy to Funding Vaults → yield strategies → yield donated
2. **Direct:** Funds stay in Safe wallet, periodic authorized withdrawals

## Dragon = Capital Provider
- Configure Dragon Router (how to distribute yield)
- Choose capital provisioning (vault lock or monthly allowance)
- Enable community participation (token-gated voting)
- Monitor and optimize

## Actors
- **Capital Providers (Dragons):** Deploy capital
- **Stakers:** Lock tokens, earn rewards, vote
- **Projects:** Apply for and receive funding
- **Funding Round Operators:** Configure rounds
- **Allocators:** Vote on fund distribution
- **Builders:** Create custom strategies/mechanisms

## Lockup Mechanics
- **Rage Quit:** Controlled exit with cooldown period
- **Unlock Schedule:** Linear/bounded share unlocking
- **Profit Unlock:** Drip-unlock reported profits

## Security Risks (User-Facing)
- Smart contract risk (built on audited Yearn v3 + ERC-4626)
- Strategy risk (protocol risks, liquidation, governance changes)
- Impermanent loss (LP token strategies)

## Key Tagline
"Octant is NOT just about 'giving out grants.' We're building Ethereum's R&D Engine and positioning as a Narrative Leader."

---
*Source: Octant v2 Documentation (file_3)*

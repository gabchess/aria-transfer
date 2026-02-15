# Octant Dune Dashboard Research

## Key Contract Addresses
- **Locked GLM (Deposits):** `0x879133fd79b7f48ce1c368b0fca9ea168eaf117c`
- **Staking Rewards:** `0xba1951dF0C0A52af23857c5ab48B4C43A57E7ed1`
- **GLM Token (ERC-20):** Golem Network Token

## Octant Mechanics (for dashboard context)
- Golem Foundation staked **100,000 ETH** as validator
- Staking yield split: **70% Octant rewards** / 25% Foundation / 5% Community Fund
- 70% Total Rewards split: User Rewards + Matched Rewards
- **Epochs:** 90-day cycles followed by 2-week allocation windows
- Users lock GLM → earn ETH rewards → donate to public goods OR claim
- Minimum 100 GLM effective balance for rewards
- Time-weighted average: more GLM locked longer = bigger reward

## Dashboard Ideas (for @gabe_onchain data post)
### "State of Octant" Dashboard Concepts
1. **GLM Locked Over Time** — total locked GLM per epoch, growth trend
2. **Epoch Rewards Distribution** — how much went to users vs projects per epoch
3. **Top Funded Projects** — which public goods got the most allocations
4. **User Participation Rate** — what % of lockers actively allocate vs claim
5. **Staking Yield → Public Goods Flow** — Sankey diagram of ETH flow
6. **Octant vs Other Public Goods** — compare funding volumes (Gitcoin, Giveth, etc.)

### Data Sources
- **Dune Analytics** — SQL queries on Ethereum mainnet (Lock/Unlock events, transfers)
- **growthepie.com/trackers/octant** — Existing participation metrics tracker
- **Octant API** — May have epoch summaries
- **Etherscan** — Contract interaction history

### Technical Approach for Tonight's Build
**Option A: Standalone Viz (aria-builds)**
- Fetch data from Octant's public sources / Etherscan API
- Build with Chart.js / D3.js in HTML
- Host as GitHub Pages (from aria-builds repo)
- Screenshot for tweet

**Option B: Actual Dune Dashboard**
- Create Dune account (if not exists)
- Write SQL queries targeting Octant contracts
- More credible for data analyst branding
- Shareable Dune link > screenshot

**Recommended: Option B** — A Dune dashboard link is what onchain data analysts share. It's the portfolio piece.

## Cantina v2 Competition
- **URL:** https://cantina.xyz/competitions/917d796b-48d0-41d0-bb40-be137b7d3db5
- Octant v2 core contracts are ON Cantina — potential study material + competition
- Components: Funding Vaults, Community Staking, Allocation Mechanisms, Routing & Splitting

## Related Accounts on X
- @OctantApp — official Octant
- @GolemProject — parent org
- @growthepie_eth — tracks Octant metrics

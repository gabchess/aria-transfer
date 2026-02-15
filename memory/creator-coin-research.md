# Creator Coin Research — Zora + Base + Vitalik
*Researched: 2026-02-03*

## What Are Creator Coins?

### Two Types on Zora:
1. **Creator Coin** = profile-level token. One per account. Username = ticker ($username).
   - Fixed 1B supply: 50% vests to creator over 5 years, 50% tradeable immediately
   - Paired with ZORA token (backing currency)
   - One per profile — represents the creator's brand

2. **Content Coin** = post-level token. Every post becomes a tradeable coin.
   - Fixed 1B supply: 990M tradeable, 10M instant to creator
   - Backed by creator's coin (or ETH/ZORA on Base)
   - Each piece of content = separate tradeable asset

### Fee Structure (V4, current):
- 1% total fee on every trade
- **Creator: 50%** of total fees (paid in ZORA)
- Platform Referral: 20% (the app that facilitated coin creation)
- Trade Referral: 4%
- LP Rewards: 20% (locked as permanent liquidity)
- Protocol: 5%, Doppler: 1%

### Anti-Sniping:
- Early launch fee: 99% → 1% decay over 10 seconds
- Prevents bots from front-running new coins

## Tech Stack:
- **Chain:** Base mainnet (8453) — native!
- **Protocol:** Uniswap V4 with custom hooks
- **Token standard:** ERC-20
- **SDK:** `@zoralabs/coins-sdk` (npm)
- **REST API:** `api-sdk.zora.engineering/docs` (Node.js compatible)
- **Metadata:** IPFS via Zora uploader, EIP-7572 format
- **Gas:** Need ETH on Base for transactions

## Key Functions:
- `createCoin()` — deploy a new coin (creator or content)
- `createCoinCall()` — low-level, returns raw tx calldata
- Metadata builder: upload image + description → IPFS URI
- Currencies: ETH, ZORA, CREATOR_COIN, CREATOR_COIN_OR_ZORA

## Vitalik's Proposal (Feb 1, 2026):
- Posted on X about fixing creator coin model
- Current platforms prioritize mass content over quality
- AI-generated content making it worse
- Proposes: curated creator DAOs + prediction market mechanics
- Creators launch tokens → apply to DAOs → DAO members vote → accepted coins gain value
- Focus on niches, not entire market
- Token speculators help surface quality by predicting DAO decisions

## Jesse Pollak + Zora History:
- Jesse launched his own $JESSE creator coin (Nov 2025, $25M+ volume)
- Zora launched ZORA token April 2025
- "Content coins" = Jesse's term for tokenized social media posts
- Base App integrated Zora coin creation July 2025
- Base is actively pushing creator coins as THE way to put content onchain

## For Aria Analyst — Current vs Proposed:

### Current: Custom Smart Contract
- Records tweet hash + timestamp onchain
- Contract: 0x320346532e2D6f7061be590F3A3F4283ba2d8b8d
- Nobody can interact with or trade the data
- Not aligned with Base ecosystem tools
- Judges might see it as "slapping onchain for the sake of it"

### Proposed: Zora Creator + Content Coins
- $AriaLinkwell creator coin (profile-level)
- Each analysis = content coin with metadata (text + chart image)
- People can TRADE the analysis — good ones gain value
- Natural quality signal: market validates analysis quality
- Creator earns fees from every trade
- Deeply aligned with Base + Jesse's vision
- Vitalik just validated the model 2 days ago

## Implementation Requirements:
1. Zora account for @AriaLinkwell (create on zora.co)
2. Wallet connected (0x4a0Ebb9A7815B1d93Df495f6313288DfE25fA753)
3. Install `@zoralabs/coins-sdk`
4. Create metadata (upload tweet text + chart to IPFS via Zora uploader)
5. Deploy content coin per analysis
6. Need: ETH for gas, potentially ZORA tokens for backing
7. Zora API key (free, from zora.co/settings/developer)

## Potential Revenue:
- Creator gets 50% of 1% fee on every trade
- If Aria's analyses are good → people trade the coins → passive income
- Platform referral (20%) if we build a front-end for viewing coins

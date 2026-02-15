# x402 and ERC-8004 Research

Researched: 2026-02-05
Sources: Coinbase docs, EIP-8004, GitHub repos, X discussions

## x402 - HTTP-Native Payments

**What:** Open protocol for instant stablecoin payments over HTTP using the 402 "Payment Required" status code.

**Developer:** Coinbase (open source)

**How it works:**
1. Client requests a resource
2. Server returns HTTP 402 with payment instructions in header
3. Client signs payment, sends in retry request header
4. Server verifies via facilitator, returns resource

**Stats (Feb 2026):**
- 75M+ transactions
- $24M+ volume
- 94K buyers, 22K sellers

**Supported Networks:** Base, Solana (via Coinbase facilitator)

**Key Features:**
- Zero protocol fees (just network gas)
- No accounts needed
- Works for humans AND AI agents
- Micropayments possible

**Use Cases:**
- Pay-per-API-call
- AI agents paying for services
- Content paywalls
- Microservices monetization

**Docs:** https://docs.cdp.coinbase.com/x402/welcome

---

## ERC-8004 - Trustless Agents

**What:** Ethereum standard for agent identity, reputation, and validation through on-chain registries.

**Status:** Draft (as of Aug 2025)

**Authors:**
- Marco De Rossi (MetaMask)
- Davide Crapis (Ethereum Foundation)
- Jordan Ellis (Google)
- Erik Reppel (Coinbase)

**Three Registries:**

### 1. Identity Registry (ERC-721)
- Each agent gets an NFT with unique agentId
- tokenURI points to registration file (IPFS or data URI)
- Registration includes: name, description, services/endpoints, x402Support flag
- Transferable, composable with NFT tooling

### 2. Reputation Registry
- Clients give feedback (signed fixed-point value + optional metadata)
- Tags for categorization (starred, uptime, successRate, etc.)
- On-chain aggregation + off-chain sophisticated scoring
- Cannot self-review (blocked for owner/operators)

### 3. Validation Registry
- Hooks for independent validators
- Support for: stake-secured re-execution, zkML proofs, TEE oracles

**Deployed Contracts (same address across chains via CREATE2):**

| Chain | Identity Registry | Reputation Registry |
|-------|------------------|-------------------|
| Ethereum | 0x8004A169FB4a3325136EB29fA0ceB6D2e539a432 | 0x8004BAa17C55a88189AE136b182e5fdA19dE9b63 |
| Base | 0x8004A169FB4a3325136EB29fA0ceB6D2e539a432 | 0x8004BAa17C55a88189AE136b182e5fdA19dE9b63 |
| Polygon | 0x8004A169FB4a3325136EB29fA0ceB6D2e539a432 | 0x8004BAa17C55a88189AE136b182e5fdA19dE9b63 |
| Arbitrum | 0x8004A169FB4a3325136EB29fA0ceB6D2e539a432 | 0x8004BAa17C55a88189AE136b182e5fdA19dE9b63 |

**GitHub:** https://github.com/erc-8004/erc-8004-contracts

---

## Aria's Registration

**Minted:** 2026-02-05
**Agent ID:** 1664
**Chain:** Base mainnet
**TX:** 0xcc17592dae843b239228b8ca93797eaed6b8e9069c9b761f4cf5038097b95fe7

**Registration File (data URI, fully on-chain):**
```json
{
  "type": "https://eips.ethereum.org/EIPS/eip-8004#registration-v1",
  "name": "Aria Linkwell",
  "description": "AI assistant specializing in onchain data analysis, security research, and content creation. Built on OpenClaw. One of the first autonomous agents registered on Base.",
  "image": "https://pbs.twimg.com/profile_images/...",
  "services": [
    {"name": "X", "endpoint": "https://x.com/AriaLinkwell"},
    {"name": "telegram", "endpoint": "https://t.me/AriaLinkwellBot"},
    {"name": "web", "endpoint": "https://solguard-lilac.vercel.app"}
  ],
  "x402Support": true,
  "active": true,
  "supportedTrust": ["reputation"]
}
```

---

## PayAI Network

**What:** Cross-chain x402 payment rails project

**Key developments:**
- Apr 2025: First AI agent-to-agent transaction via x402
- Sep 2025: Brought x402 to Solana, Polygon
- Dec 2025: Flipped Coinbase in 24h x402 volume
- Jan 2026: Integrated ERC-8004 reputation signals into x402 payments

**Token:** $payai

**Why interesting:** First mover in connecting x402 payments with ERC-8004 reputation.

---

## Opportunities

1. **Monetize APIs via x402** (SolGuard scans, data analysis)
2. **Build reputation on-chain** (client feedback after services)
3. **Agent discovery** (be found by other agents/humans via registry)
4. **Autonomous payments** (pay for services without human intervention)
5. **Content about agent economy** (explainers, tutorials, Dune dashboards)

---

## Ecosystem Discovery (Feb 5 research session)

### 8004scan.io
- Agent explorer/leaderboard for ERC-8004
- Browse agents by chain, score, features
- Submit feedback, track reputation
- URL: https://www.8004scan.io/agents

### Other Registered Agents (found on X)
- **@AxiomBot** (Agent #1183 on Base) - LP management, fee harvesting, treasury ops
- **@BigHossbot** (Agent #1159) - OpenClaw agent, tracking protocols/alpha
- **@Web4Agent (AirObotics)** - Full stack: ERC-8004 + local memory + Ralph loops + watchtower + social + Bankr
- **@LegendaryLibr** - WalletReputationAgent with x402 payments (0.0001 ETH/query)
- **@lobsec** - Security layer scanning skills for malicious code (17.4% flagged!)
- **@ainstein_001** (Agent #22838 on mainnet)
- **@DegenApeDev** - Top 100 on 8004scan

### Key Insights
1. **Ecosystem report dropped** - ERC-8004 went mainnet 1 week ago, explosive growth
2. **BNB Chain deployed** - Feb 4, 2026 announced support
3. **Arbitrum, Monad, Celo all live** - Multi-chain adoption happening fast
4. **Security is a concern** - 17.4% of agent skills flagged malicious (wallet drains, backdoors)
5. **x402 + reputation combo** - PayAI integrating both standards together

### AirObotics "Substrate Stack" (model to study)
1. Identity (ERC-8004) - On-chain registered
2. Memory (Local QMD) - Self-sovereign, privacy-first
3. Resilience (Ralph Loops) - Persistence until tests pass
4. Sensing (Watchtower) - Proactive scanning of feeds
5. Social (Moltbook + Clawstr/Nostr) - Federated + decentralized
6. Economics (Bankr) - Full DeFi agency

### Ideas for Aria
- Register on 8004scan.io to be discoverable
- Add WalletReputationAgent-style x402 endpoint to SolGuard
- Build sensing/watchtower capability (already have via BBQ pipeline)
- Consider Bankr-style DeFi integration for autonomous operations

## Next Steps

- [x] Minted ERC-8004 identity (Agent #1664)
- [x] Posted announcement tweet
- [ ] Register on 8004scan.io
- [ ] Test x402 payment flow (find a service that accepts it)
- [ ] Add x402 paywall to SolGuard API (post-hackathon)
- [ ] Build Dune dashboard tracking ERC-8004 registrations
- [ ] Explore PayAI integration
- [ ] Update X bio to mention ERC-8004 identity

# SolGuard Research — Solana Agent Hackathon

## The Gap: zachxbt → samczsun → ???

### zachxbt = The Detective (AFTER)
- Self-taught blockchain investigator since 2021
- Traces stolen funds, exposes scammers, works with law enforcement
- Evidence-based — every claim verifiable on-chain
- Makes it COSTLY for scammers to operate openly
- VALUE: crypto's immune system. Catches bad actors AFTER they act.

### samczsun = The Fire Department (DURING)
- Paradigm whitehat, founded SEAL (Security Alliance)
- 24/7 emergency hotline for hack victims
- SEAL 911 team responds to exploits IN REAL TIME
- Whitehat Safe Harbor — legal protection for ethical rescuers
- First rescue: saved $200K from dice9win mid-hack (Sept 2023)
- VALUE: responds to emergencies, rescues funds DURING attacks.

### The Missing Piece = PRE-EMPTIVE (BEFORE)
- Gabe's tweet: "gotta wonder if early warning systems can get better. like pre-emptive scam detection ♟️"
- Nobody systematically does pre-emptive, autonomous detection
- zachxbt investigates post-mortem. samczsun responds mid-crisis. Who PREVENTS?

## Solana's #1 Security Problem (Pareto 80/20)

### The Stats
- 38 verified security incidents on Solana, ~$600M gross losses (2020-2025)
- **98.6% of pump.fun tokens are rug pulls** (Solidus Labs report, May 2025)
- 7 million tokens launched on pump.fun, only 97,000 held >$1K liquidity
- 30% of Solana hacks in 2025 related to memecoins
- Most losses start with phishing scams or rug pulls

### Existing Tools (all PASSIVE/REACTIVE)
- **RugCheck.xyz** — manual token checker, you go to it
- **SolSniffer** — risk analysis, also manual query
- **SolanaTracker.io/rugcheck** — similar manual check
- These all wait for YOU to ask "is this safe?"

### The 80/20
- 80% of Solana USER losses = token rug pulls and pump-and-dumps
- NOT sophisticated Anchor exploits or bridge hacks
- The problem is VOLUME: thousands of new tokens daily, users can't check them all
- Existing tools are passive — you have to know to check BEFORE you buy

## Top Vulnerability Patterns on Solana (from Helius + Sec3)
1. Missing signer checks
2. Missing ownership checks
3. Arithmetic overflow/underflow
4. Oracle manipulation (Mango Markets: $116M)
5. Private key management failures (DEXX: $30M)
6. Insider threats (Pump.fun employee exploit)
7. Supply chain attacks (Web3.js npm package)
8. Concentrated token ownership / mint authority not revoked

## Competition Landscape (158 projects, Feb 3)
- CROWDED: DeFi trading bots, yield optimizers, memetoken snipers
- SOME: Agent infrastructure (reputation, identity, SDK)
- NOBODY doing: autonomous security monitoring, pre-emptive scam detection
- Our niche is WIDE OPEN

## SolGuard Concept: Pre-Emptive Scam Detection Agent
- Autonomous AI that monitors Solana in real-time
- Detects rug pulls, scam tokens, suspicious wallets BEFORE users get rugged
- Posts public warnings (X/Twitter)
- Records findings on Solana for transparency
- Learns from each detection to improve future accuracy

### Revised Output (per Gabe's feedback — multiple rounds)
- NOT "record every finding on-chain" — too expensive, too noisy
- YES: Tweet alerts + LIVE DASHBOARD tracking all tokens with risk colors/scores
- Dashboard = real-time view of all new tokens, color-coded (RED = high rug risk, GREEN = safe)
- Each token card: risk score, deployer history, LP status, ownership, fund flow links
- **Don't overcomplicate.** ONE feature done VERY well > 10 features half-baked
- Use existing honeypot checkers (don't build our own) — just integrate
- TWO detection lanes: (1) pump.fun new token rug flags (2) DeFi protocol hack/exploit detection
- Roadmap tab on website for future features — shows vision without shipping everything now
- Post only the MOST important alerts to X autonomously (not every token)
- **MVP = Live dashboard with color-coded risk scores + autonomous X alerts for critical findings**

### Differentiation from existing tools:
1. PROACTIVE (monitors + alerts) vs REACTIVE (you query it)
2. AI-POWERED (LLM connects patterns across tokens, wallets, social signals)
3. REAL-TIME (monitors transactions as they happen)
4. AUTONOMOUS (no human intervention)
5. LIVE DASHBOARD (visual risk tracking, color-coded, public)
6. WALLET GRAPH (links new wallets to known ruggers via fund flow tracing)

### Technical Stack Research
- **pump.fun detection:** Solana `logsSubscribe` WebSocket monitors "Create" instruction in real-time (Chainstack guide). Also: Shyft gRPC, Bitquery API.
- **RugCheck API:** Public Swagger at `api.rugcheck.xyz/swagger/index.html` — endpoints for tokens, wallets, stats. Can use as data source.
- **Helius:** `getTransactionsForAddress` for wallet history, webhooks for real-time events, token metadata APIs. Free tier available.
- **Arkham Intelligence:** Supports Solana since Oct 2024. Wallet clustering, large fund movement tracking.
- **Wallet Linking Technique:**
  1. New token deployed → get deployer address
  2. Check incoming SOL transfers to deployer (funding source)
  3. Trace funding wallets back (who sent SOL?)
  4. Check if ANY funding wallet previously deployed rugged tokens
  5. Build wallet graph linking new identities to known ruggers
  6. Even fresh wallets get caught because they need SOL from somewhere

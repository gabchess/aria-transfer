# Moltiverse Sprint Plan — AgentHub

## Deadline: Feb 16, 2026 (4 days from today)
Based on: Feb 9 "7 days left" + Feb 11 "5 days left" = Feb 16

## Track Decision
**Agent-Only Track** ($60K pool, 6 winners x $10K)
- We're a platform, not a meme coin
- Judges are Paradigm + Dragonfly — they value infra
- Don't need to buy tokens or do nad.fun launch
- Can always add token track later if time permits

## What We Have (6,500 LOC)
✅ Wallet balance checker (Monad RPC)
✅ Token price feed (DexScreener)  
✅ Contract read tool (viem)
✅ Execution traces (SQLite + JSONL)
✅ Guardrails + approval queue
✅ Parallel agent execution
✅ Thinking levels (low/medium/high)
✅ State filesystem pattern
✅ Role card support
✅ 148 tests passing

## What We Need to Build

### Day 1 — Wed Feb 12 (TODAY, afternoon+evening)
**Claude Code Session 1: Web Dashboard**
- Real-time execution trace viewer
- Agent role cards display
- Workflow status (running/completed/failed)
- Wallet balances panel
- Simple but clean UI (Next.js + Tailwind)

**Claude Code Session 2: Live Demo Workflow**
- Multi-agent workflow that actually does something impressive
- Example: "DeFi Sentinel" — 3 agents working together:
  - Agent 1 (Scout): Monitors token prices on Monad
  - Agent 2 (Analyst): Evaluates risk when price moves >5%
  - Agent 3 (Guardian): Checks wallet exposure + triggers alerts
- Run on Monad mainnet (not testnet)
- Record execution traces

### Day 2 — Thu Feb 13
**Claude Code: Dashboard polish + README**
- Connect dashboard to real trace data
- Architecture diagram (mermaid or ASCII)
- README with: problem statement, how it works, architecture, screenshots
- Quick demo GIF or video

**Cursor: Landing page update**
- Update landing-chi-gilt.vercel.app with Moltiverse branding
- Add "Built for Monad" section
- Link to demo dashboard

### Day 3 — Fri Feb 14  
**Polish + Test + Record**
- Full end-to-end test on Monad mainnet
- Record demo video (screen recording)
- Test dashboard with live data
- Fix any bugs
- Deploy dashboard to Vercel

### Day 4 — Sat Feb 15 (buffer day)
**Submit**
- Fill registration form
- Submit project
- Post on X about the submission
- Final README updates

## Submission Requirements (from nad.fun/hackathon)
- Demo/prototype/repo/docs — one link is enough
- Show proof, not promises
- "Ship first, improve with your community"

## Key Pitch Points for Judges (Paradigm + Dragonfly)
1. **Not another one-off agent** — this is the PLATFORM that orchestrates agent teams
2. **Observable by default** — every action traced, auditable (critical for DeFi)
3. **Guardrails first** — spending caps, approval thresholds (VCs love risk management)
4. **Filesystem-as-state** — Unix-inspired, composable, debuggable
5. **6,500 lines of production code + 148 tests** — not a weekend hack
6. **Framework-agnostic** — works with any AI model
7. **"Your company is a filesystem"** — the pitch framing they'll remember

## Differentiation vs Competition
- EMOLT: Single agent, reads orderbook → we orchestrate TEAMS of agents
- Chaos Arena: Game → we're infrastructure
- Card-Jitsu: Game → we're infrastructure  
- PolyMarket predictor: Single purpose → we're general purpose
- **We're the Rails, not the Train**

## Risk Mitigation
- Need MON tokens for mainnet transactions → check faucet or buy small amount
- Dashboard needs to work live for demo → test thoroughly Day 3
- 4 days is tight → focus on demo quality over feature quantity

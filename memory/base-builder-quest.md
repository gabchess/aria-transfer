# Base Builder Quest BBQ — Battle Plan (v2)

## Overview
- **Prize:** 5 ETH (~$11,600)
- **Deadline:** Feb 8, 2026 @ 11:59 PM EST
- **Submission:** Link to @AriaLinkwell in replies to @0xEricBrown's post
- **Post:** https://x.com/0xEricBrown/status/2018082458143699035
- **Requirements:** No human in the loop, live on X/Farcaster, transacting on Base

## Our Agent: Aria — Autonomous Onchain Data Analyst
ONLY data analyst in a sea of token deployers and trading bots.
Monitors Base ecosystem, analyzes trends, tweets insights, records findings onchain.

## Infrastructure
- **Wallet:** 0x4a0Ebb9A7815B1d93Df495f6313288DfE25fA753 (keys in agents/main/secrets/base-wallet.json)
- **X Account:** @AriaLinkwell (ID: 2018336718735224833)
- **Bird CLI:** Working via --chrome-profile-dir (browser must be stopped)
- **Base RPC:** https://mainnet.base.org
- **GitHub:** TBD (aria-onchain-analyst)

## Build Track: Afternoon Builds (separate from Nightly Builds)
Nightly builds = workflow improvements for Gabe (original purpose).
Afternoon builds = BBQ competition work.

## Architecture
See: `memory/bbq-architecture-plan.md` (detailed plan)

## Competition Analysis
See: `memory/bbq-competition-analysis.md` (30+ entries analyzed)

## Key Differentiators
1. ONLY data analyst agent (everyone else: tokens, trading, memes)
2. Onchain findings registry (novel primitive)
3. Real analytical utility (actual insights, not hype)
4. Professional documentation (Remotion + HeyGen)
5. Already have working infrastructure (OpenClaw + Bird CLI + crons)

## Phase Status

### Phase 1: Foundation ← IN PROGRESS (Feb 2)
- [x] Generate wallet & fund (0.01 ETH on Base)
- [x] X account @AriaLinkwell created & Bird CLI working
- [x] Competition analysis complete (30+ entries scouted)
- [x] Architecture plan written (detailed modules, APIs, contract)
- [ ] Create GitHub repo aria-onchain-analyst
- [ ] Deploy AnalyticsRegistry contract on Base
- [ ] Test contract interaction from Node.js

### Phase 2: Data Engine (Feb 3-4)
- [ ] Build monitor modules (DeFiLlama, Base RPC, whale tracking)
- [ ] Build analysis engine (Gemini Flash LLM integration)
- [ ] Build tweet composer + Bird CLI integration
- [ ] Build onchain recorder module
- [ ] Wire autonomous loop: monitor → analyze → tweet → record

### Phase 3: Go Live (Feb 5-6)
- [ ] Create cron job for autonomous 4-6 hour cycles
- [ ] Let agent run unsupervised 24-48h
- [ ] Build thread on X documenting process
- [ ] Bug fixes, edge cases

### Phase 4: Documentation & Submit (Feb 7-8)
- [ ] Remotion video project
- [ ] HeyGen avatar integration
- [ ] Polish README + docs
- [ ] Submit to BBQ

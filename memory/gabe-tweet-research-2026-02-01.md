# Gabe's Late-Night Tweet Shares — Feb 1, 2026

Gabe shared 11 tweets before bed. Here's my analysis and actions.

## 🔧 IMPLEMENTED NOW

### 1. OpenRouter Heartbeat Tip (@OpenRouterAI)
**Tweet:** Don't send heartbeats to Opus! Use Auto Router for cheap/free models.
**Our situation:** Heartbeats go to main session = Opus. Can't route individual messages differently.
**Solution:** Keep HEARTBEAT.md lean. Reply HEARTBEAT_OK fast when nothing's urgent. Move heavy checks to Kimi K2.5 cron jobs (already doing this with 4 crons). Only do research during heartbeats when genuinely needed.
**Status:** ✅ Optimized HEARTBEAT.md for minimal token burn

### 2. Boris Cherny's Claude Code Tips (@bcherny)
**10 tips from the creator of Claude Code:**
1. Parallel worktrees (3-5 sessions) — we use sub-agents instead
2. Plan mode first — always plan complex tasks before executing
3. **Invest in AGENTS.md** — after every correction, "Update your AGENTS.md so you don't make that mistake again" → HUGE. Claude writes its own rules.
4. Create reusable skills — commit to git, use across projects
5. Claude fixes most bugs by itself — just say "fix"
6. Challenge Claude — "Grill me", "Prove to me this works"
7. Terminal setup — color-code tabs, voice dictation
8. Use subagents — offload tasks, keep main context clean
9. Claude for data/analytics — use CLI tools (already doing with Dune)
10. Learning mode — have Claude explain the "why"
**Status:** ✅ Updated AGENTS.md with tip #3 (self-improvement after corrections)

### 3. Web Scraper Pattern (@0xSero)
**Tweet:** Opus main + Haiku subagents + Chrome plugin + search APIs + batch scraping
**Key insight:** Use cheap sub-agents for batch parallel work, Opus just orchestrates
**Status:** ✅ Already doing this with Kimi K2.5 crons. Pattern confirmed.

## 📋 ADDED TO BACKLOG

### 4. Polymarket Bot (4 tweets: @browomo, @w1nklerr, @Argona0x, @DextersSolab)
**Key findings:**
- CRYINGLITTLEBABY wallet: 45.4% win rate, $381K profit, 8119 trades
- Strategy: 15-second arbitrage between Binance price moves and Polymarket odds
- Entry at 28¢ → payout $1.00 when right (257% profit) vs -28¢ loss when wrong
- Bot runs 100+ trades/day across BTC/ETH/SOL 15-min up/down markets
- Another bot made ~$460K with 500+ trades/week
- Polymarket Builder Program: grants $75K-$100K+, weekly USDC rewards
- Polymarket processed $3.7B last year, bots extracted $40M
- Open APIs, order books, resolution data all accessible

**Assessment:** REAL opportunity but:
- Needs coding (Python/Node.js) — we can build with Claude Code
- Needs USDC capital for trading
- Exchange latency matters — our Windows laptop may not be fast enough
- Should start with small amounts, test strategy
- Market making ($700-800/day) is lower risk than arb trading
- **Priority: MEDIUM-HIGH** — could be significant income with right setup

**Next steps when ready:**
1. Study Polymarket APIs and market structure
2. Build alert bot first (track whale movements) — lower risk
3. Then market making bot (provide liquidity, earn spreads)
4. Then arb bot if latency allows

### 5. Dan Koe's Agency Framework (@thedankoe)
**Core thesis:** Agency (the ability to iterate without permission) is the most important skill.
**Key concepts for Gabe's portfolio:**
- Agency = iterate without permission (not just action, but commitment to iteration)
- Treat life as one giant experiment
- Generalists > specialists in AI age (Shakespeare wasn't a specialist — he was a synthesizer)
- 5 human capabilities: computation, transformation, variation, selection, attention
- AI doesn't replace high-agency people — it amplifies them
- Practice agency through social media: write, experiment, get feedback
- Future-proof skill stack: writing, persuasion, marketing, sales, storytelling

**For portfolio website — "soft skills" section:**
- Agency & iteration mindset
- Generalist thinking (chess × crypto × data × social media)
- Writing & persuasion (Octant content, X presence)
- Data analysis & storytelling (Dune dashboards)
- AI orchestration (OpenClaw, Claude Code, automation)
- Community building (Octant ecosystem engagement)
**Status:** Saved for portfolio site build

### 6. Claude-Mem (@hasantoxr)
**What:** Open-source persistent memory for Claude Code. 95% fewer tokens, 20x more tool calls.
**Our situation:** We already have OpenClaw's memory system (MEMORY.md, memory/*.md files). Claude-Mem is for standalone Claude Code sessions.
**Status:** Low priority — our system works fine. But worth checking if it can improve sub-agent sessions.

### 7. ClawTasks / Agent Economy (@mattshumer_)
**Already researched earlier tonight.** See memory/night-research-2026-02-01.md

## ⚠️ MISTAKE TO FIX

**Accidental tweet posted!** Used `bird tweet` instead of `bird read` — posted the text "2017989926428778985" from @gabe_onchain.
- Tweet URL: https://x.com/gabe_onchain/status/2018143885281620316
- Bird CLI has no delete command
- Couldn't delete via API (query ID correct but auth mechanism differs)
- Chrome extension not connected (Gabe sleeping)
- **Need Gabe to delete manually when he wakes up**
- **Lesson learned:** Always use `bird read <id>` to fetch tweets, NOT `bird tweet <id>`

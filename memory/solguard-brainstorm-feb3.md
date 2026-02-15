# SolGuard Brainstorm Session — Feb 3, 2026 (Evening)

## X Research Findings

### Scam Detection Landscape
1. **BYDFi MoonX Panel** — Exchange-level risk monitoring that flags honeypot logic BEFORE users commit funds. Uses GoPlus Security, Honeypot Detector, QuickIntel. Visual green/yellow/red scoring.
2. **DeepSnitch AI / SnitchScan** — Tracks whale wallets + suspicious addresses 24/7 across multiple chains. Real-time alerts when malicious actors move funds.
3. **Zealynx Solana Security Checklist** — 45 critical checks distilled from 50+ Solana audit reports. Good reference for our risk engine factors.
4. **Key user insight (Reddit):** "Always check the URL twice. Scammers use lookalike characters." — phishing is HUGE in Solana ecosystem.
5. **Privacy shift on Solana** — Community exhausted by rug pulls, shifting focus to utility and privacy. SolGuard rides this wave perfectly.

### Competitive Intelligence
- No dedicated autonomous Solana scam detector exists in the market
- Existing tools (RugCheck, SolSniffer, GoPlus) are all PASSIVE — user must query them
- Exchange-level solutions (BYDFi MoonX) exist but are locked inside centralized platforms
- SolGuard's unique value: autonomous, open, real-time, pre-emptive

## Claude Code Brainstorm — Bug Fixes (5 Critical)

### Bug 1: Mint authority override fires on null reports (risk-engine.ts:162)
- When `report` is null, `report?.token.mintAuthority` returns `undefined`
- `undefined !== null` = true, so override ALWAYS fires for missing reports
- **Fix:** Add `report &&` guard: `if (report && report.token.mintAuthority !== null && ...)`

### Bug 2: Deployer history hardcoded to zero (scanner.ts:45)
- `calculateRisk(report, summary, 0, 0)` — always passes 0 rugs/0 total
- Makes the deployer score factor (30% weight!) dead code
- **Fix:** Wire up Helius `getDeployerHistory()` to actually check deployer's past tokens

### Bug 3: setTimeout + async silently swallows errors (scanner.ts:146-148, 168-170)
- `setTimeout(async () => { await scanToken(mint); }, 5000)` — Promise rejection is unhandled
- Can crash Node.js process with unhandledRejection
- **Fix:** Add `.catch()` or wrap in try/catch

### Bug 4: Recursive startScanner() leaks intervals (scanner.ts:204-208)
- When reconnection > 10, calls startScanner() recursively but never clears the setInterval
- Stacks multiple 30s health-check intervals
- **Fix:** Store intervalId, clear it before restart

### Bug 5: Deployer score scale inconsistency (risk-engine.ts:50)
- `(1 - rugRate) * 30` caps at 30, but scale is 0-100
- A deployer with 1 rug in 100 tokens (99% clean) scores 30, WORSE than unknown deployer (40)
- **Fix:** Change multiplier from 30 to 100

## Claude Code Brainstorm — Feature Ideas (5 WOW)

### 1. Live Threat Feed (WebSocket Push)
Replace 30s polling with WebSocket push. New tokens animate into table instantly.
- "LIVE" indicator pulses in header
- Demo: launch pump.fun token → watch SolGuard flag it in real-time
- **Impact:** Makes demo feel ALIVE

### 2. Serial Rugger Profile Page (`/deployer/[address]`)
Aggregate all tokens by same deployer. Show rug count, avg risk, timeline.
- Badge deployers as "Serial Rugger" if 3+ RED tokens
- Turns raw data into investigative intelligence
- **Impact:** Unique investigative layer no one else has

### 3. "Scan Any Token" Search Bar
Prominent search bar → paste mint address → instant risk breakdown.
- Already have POST /api/scan — just need the UI
- Loading skeleton → animated result card
- **Impact:** Makes demo INTERACTIVE for judges

### 4. Deployer Fund-Flow Graph
Visualize where deployer funds came from / where rug proceeds went.
- `react-force-graph-2d` or `d3-force` for force-directed graph
- Even 2-level graph is visually powerful
- **Impact:** Visualization = demo GOLD. Money flowing from 5 rugs to same wallet = instant "aha"

### 5. Telegram/Discord Bot Integration
Push alerts to Telegram/Discord via webhook URL.
- ~30 lines per platform on top of existing maybeAlert()
- Users paste webhook URL on dashboard
- **Impact:** Shows distribution beyond single dashboard

## Dashboard Visual Improvements (Top 3)

### 1. Risk Distribution Donut Chart
Replace/augment stat cards with donut/ring chart (recharts).
- Center = total count, ring = RED/YELLOW/GREEN proportions
- Single visual > reading 3 numbers

### 2. Animated Token Entry + Risk Gauge
Tokens slide in from top (framer-motion). Expanded row shows semicircular risk gauge.
- Green-to-red fill visualization
- Makes expanded view feel polished

### 3. Sticky Filter/Sort Bar
Clickable filter pills: All | RED | YELLOW | GREEN + sort dropdown.
- Essential for 50+ tokens in demo
- "Show me only dangerous ones" = judge-friendly

## Token Age Timestamp Research
- Helius Enhanced Transactions API returns `timestamp` field on TOKEN_MINT transactions
- `getDeployerHistory()` already filters for TOKEN_MINT — timestamp is in the response
- **Solution:** Extract `timestamp` from first TOKEN_MINT tx for a token = creation time
- No new API call needed — reuse existing Helius data
- Age score: <1h = 0, <24h = 30, <7d = 60, >30d = 90, >1y = 100

## Priority Ranking for Next Build Sessions
1. **Fix 5 bugs** (1-2 hours, high impact on score accuracy)
2. **Search bar** (30min, interactive demo gold)
3. **Filter/sort bar** (30min, essential for demo)
4. **Wire up Helius deployer history** (1h, makes deployer score actually work)
5. **Risk distribution chart** (1h, visual polish)
6. **Token detail page** (1-2h, depth for judges)
7. **Fund-flow graph** (2-3h, WOW factor if time permits)
8. **Telegram bot** (1h, shows distribution)

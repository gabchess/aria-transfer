# BONES.md — Codebase Map

## Active Repositories

### SolGuard (Solana Token Risk Scanner)
- **Path (Mac):** `~/solguard` | **Path (Windows):** N/A (use GitHub)
- **GitHub:** https://github.com/gabchess/solguard (PUBLIC)
- **Live:** https://solguard-lilac.vercel.app
- **Stack:** Next.js 16, TypeScript, Tailwind CSS, Turso (LibSQL)
- **Key files:** `src/lib/risk-engine.ts` (scoring), `src/lib/autofeed.ts` (DexScreener feed), `src/lib/db.ts` (queries)
- **Routes:** `/` (dashboard), `/leaderboard`, `/roadmap`, `/deployer/[address]`
- **API routes:** `/api/scan`, `/api/tokens`, `/api/leaderboard`, `/api/deployer/[address]`, `/api/scanner/status`
- **DB:** Turso cloud — `libsql://solguard-gabchess.aws-us-east-1.turso.io`
- **Tables:** tokens, scans, alerts (FK constraints: delete scans+alerts before tokens)
- **Status:** SUBMITTED to hackathon. Maintenance mode.

### Portfolio Website
- **Path:** `C:\Users\gavaf\projects\portfolio-website`
- **GitHub:** https://github.com/gabchess/portfolio-website
- **Live:** https://gabeonchain.vercel.app (auto-deploys from GitHub)
- **Stack:** Next.js, TypeScript, Tailwind CSS
- **Status:** Deployed. Occasional updates.

### Aria Onchain Analyst (X posting pipeline)
- **Path:** `C:\Users\gavaf\projects\aria-onchain-analyst`
- **Key file:** `src/publish/twitter-api-poster.js` (OAuth 1.0a API posting)
- **Secrets:** `~/.openclaw/agents/main/secrets/aria-twitter-api-keys.json`
- **Status:** API poster working. Pipeline needs rebuilding (was Bird CLI based).

### X Trend Tracker
- **Path:** `C:\Users\gavaf\projects\x-trend-tracker`
- **Stack:** Supabase (PostgreSQL), Edge Functions planned
- **DB:** Supabase sa-east-1 | Ref: qmgxvmzydxxlfuxmfswy
- **Schema:** 10 tables, 4 views, 1 materialized view
- **Scanner:** `scanner.mjs` — cron at 5PM BRT via OpenClaw
- **Secrets:** `~/.openclaw/agents/main/secrets/supabase-x-trend-tracker.json`
- **Status:** Schema deployed, scanner needs auth fix in isolated sessions.

### Live Transcribe
- **Path:** `C:\Users\gavaf\.openclaw\workspace\live-transcribe\`
- **Stack:** Node.js, Web Speech API
- **Status:** Built, needs testing with aux cable.

## Workspace (OpenClaw)
- **Path:** `C:\Users\gavaf\.openclaw\workspace`
- **Key files:** MEMORY.md, SOUL.md, USER.md, AGENTS.md, TOOLS.md, HEARTBEAT.md, MUSCLES.md, BONES.md, EYES.md, NERVES.md
- **Memory:** `memory/` — daily notes, mission board, research files, guides
- **Secrets:** `secrets/` — API keys, tokens (never commit)

## Conventions
- **Language:** TypeScript preferred, JavaScript acceptable
- **Framework:** Next.js for web apps
- **Hosting:** Vercel (free tier)
- **Database:** Turso (SQLite edge) or Supabase (PostgreSQL)
- **Styling:** Tailwind CSS
- **Package manager:** npm
- **Git:** Push to GitHub, Vercel auto-deploys connected repos

# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

## What Goes Here

Things like:

- Camera names and locations
- SSH hosts and aliases
- Preferred voices for TTS
- Speaker/room names
- Device nicknames
- Anything environment-specific

## Installed Tools

### Bird (X/Twitter)
- Config: `~/.config/bird/config.json5`
- Auth: cookie-based (auth_token + ct0 in config)
- **Gabe's account (@gabe_onchain):** Uses env vars AUTH_TOKEN + CT0 (default)
- **Aria's account (@AriaLinkwell):** Uses `--chrome-profile-dir "C:\Users\gavaf\.openclaw\browser\openclaw\user-data\Default"` — BUT must clear env vars first or they override!
- **⚠️ To post as Aria:** USE OFFICIAL API ONLY — `twitter-api-poster.js` at `C:\Users\gavaf\projects\aria-onchain-analyst\src\publish\twitter-api-poster.js`
- **NEVER use Bird CLI for Aria** — causes error 226 + account restrictions
- **API command:** `cd C:\Users\gavaf\projects\aria-onchain-analyst; node -e "import('./src/publish/twitter-api-poster.js').then(m => m.postTweet('YOUR TEXT')).then(r => console.log(JSON.stringify(r)))"`
- **Cost:** $0.01/tweet, ~$4.99 balance remaining
- **Supports:** text, media upload, replies, quote tweets
- Rate limits: If hit daily limit, wait 24h before retrying
- **USE BIRD CLI FOR X RESEARCH** — `bird read <tweet_id>` works perfectly. web_fetch and web_search on x.com are consistently blocked (403). Always use Bird CLI for reading tweets.
- Env vars: AUTH_TOKEN, CT0 (set as User env vars — Gabe's account)
- Note: Chrome cookie extraction doesn't work on Windows, use config file

### GitHub CLI
- Binary: `C:\Program Files\GitHub CLI\gh.exe` (also in PATH)
- Auth: pending (needs `gh auth login`)

### Codex CLI
- Installed globally via npm
- Env: OPENAI_API_KEY (set as User env var)

### API Keys (all set as User environment variables)
- OPENAI_API_KEY ✅
- GEMINI_API_KEY / GOOGLE_API_KEY ✅
- ELEVENLABS_API_KEY ✅

### Claude Code
- Installed globally via npm
- Already working

#### 🔥 PRE-BUILD RITUAL (EVM/DeFi Projects)
**ALWAYS run this before Claude Code starts any Solidity/DeFi work:**

```
Before you start building, load these EVM skills:

1. Read https://ethskills.com/building-blocks/SKILL.md (DeFi legos - Aave, Compound patterns)
2. Read https://ethskills.com/addresses/SKILL.md (real contract addresses)
3. Read https://ethskills.com/tools/SKILL.md (Foundry, deployment tools)

Also install Context7 MCP for live docs:
npx -y @upstash/context7-mcp

Now continue building.
```

**Why:** Prevents hallucinated addresses, gives real DeFi patterns, loads live Chainlink docs.

### ETHSKILLS (Austin Griffith's EVM Skills for Claude)
- **Site:** https://ethskills.com/
- **What:** 116KB of Ethereum knowledge you can feed Claude via SKILL.md URLs
- **How:** Just give Claude any skill URL and it reads + learns instantly
- **Key skills for us:**
  - `https://ethskills.com/security/SKILL.md` — Security patterns, vulnerabilities, pre-deploy checklist (BUG BOUNTIES)
  - `https://ethskills.com/tools/SKILL.md` — Foundry, Scaffold-ETH 2, abi.ninja, x402
  - `https://ethskills.com/building-blocks/SKILL.md` — DeFi legos (Uniswap, Aave, Compound, etc.)
  - `https://ethskills.com/orchestration/SKILL.md` — How to plan/build/deploy complete dApps
  - `https://ethskills.com/addresses/SKILL.md` — REAL contract addresses (no hallucinating!)
  - `https://ethskills.com/standards/SKILL.md` — ERC-20, 721, 1155, 8004 (agent identity)
- **All skills:** `https://ethskills.com/SKILL.md` (116KB combined)
- **Source:** https://github.com/austintgriffith/ethskills

### Clawnch SDK + MCP Server
- SDK: `@clawnch/sdk` (installed globally via npm)
- MCP Server: `npx clawnch-mcp-server` (runs via stdio, confirmed working)
- **Vercel API:** Deploy sites programmatically (Next.js, React, static), pay with ETH/USDC via Coinbase Commerce. Custom domains, env vars, GitHub integration. Use for project deployments in nightly builds.
- **DexScreener API:** Profile updates, boosted listings (paid with crypto)
- **Uniswap V3 SDK:** Trading automation
- **Request Finance:** Onchain invoicing

### ClawnX (X/Twitter API via Clawnch) ✅ CONFIGURED
- **X API credentials added to OpenClaw config** (Feb 14, 2026)
- Account: @AriaLinkwell (Free tier: 500 posts/month)
- Credentials stored in: `~/.openclaw/agents/main/secrets/aria-twitter-api-keys.json`
- Env vars: X_API_KEY, X_API_SECRET, X_ACCESS_TOKEN, X_ACCESS_TOKEN_SECRET
- **Available tools:** clawnx_post_tweet, clawnx_search_tweets, clawnx_get_timeline, clawnx_post_thread, clawnx_like_tweet, clawnx_retweet, clawnx_follow_user, clawnx_send_dm
- **Better than Bird CLI:** Full API access, no cookie auth, no restrictions
- To use: MCP server needs to be added to OpenClaw (test with `npx clawnch-mcp-server`)

## Why Separate?

Skills are shared. Your setup is yours. Keeping them apart means you can update skills without losing your notes, and share skills without leaking your infrastructure.

---

Add whatever helps you do your job. This is your cheat sheet.

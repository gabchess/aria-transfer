# Aria's Mac Mini M4 Setup Plan 🏠

## Hardware
- **Model:** Apple Mac Mini M4 (2024) — 10-core CPU, 10-core GPU, 16GB RAM, 256GB SSD
- **Power:** 127V (Rio de Janeiro, Barra da Tijuca) — universal PSU, works anywhere
- **Purchased:** Feb 14, 2026

## Day 1 Setup
1. macOS initial setup + Apple ID (Aria's own account)
2. Install: Node.js, Python, Rust, Git, GitHub CLI (`gh`)
3. Install: Claude Code (`npm install -g @anthropic-ai/claude-code`)
4. Install: OpenClaw (`npm install -g openclaw`)
5. Copy `.openclaw` workspace folder from Dell Windows laptop
6. Clone all repos from GitHub (gabchess/*)
7. Set up API keys: Anthropic, OpenRouter, OpenAI, ElevenLabs, Brave, Helius
8. Install: Ollama (local model runner)
9. Install: LM Studio (model management UI)
10. Test everything: Claude Code, OpenClaw, git push/pull, Vercel deploys

## Storage Strategy (256GB = plenty)
- **Baseline usage:** ~57GB (macOS + dev tools + projects)
- **Free space:** ~190GB

### Never Run Out Plan:
1. **iCloud Drive** ($1/month for 50GB) — offload old projects, media, archives
2. **External USB-C SSD** (Samsung T7 500GB, ~$50) — AI models + large databases
3. **GitHub as storage** — all code on GitHub, delete inactive local repos
4. **Supabase for data** — databases in the cloud, zero local cost
5. **Vercel for builds** — builds happen in cloud, not locally
6. **Ollama pull/delete pattern** — rotate models, don't hoard (keep 1-2 active)
7. **`brew cleanup` + `npm cache clean`** — run monthly, saves 10-20GB
8. **LM Studio** — download models when needed, delete after

## Local AI Models to Install
- **MiniMax M2.5** — https://x.com/minimax_ai/status/2021980761210134808
  - "Minimax-M1 has the best performance on coding among sub-100b sized open-source models"
  - Run via Ollama or LM Studio
  - Use for: code review, fast iterations, offline work
- **Alex Finn's vision** — https://x.com/alexfinn/status/2022151291057713490
  - "Full autonomous software factory on my desk running 24/7 for free"
  - MiniMax M2.5: better than Opus 4.6 for coding, faster than Sonnet, SOTA tool calling
  - SWE-Bench Verified 80.2%, BrowseComp 76.3%, BFCL 76.8%
  - OpenClaw + local model = autonomous agent searching X/Reddit, building apps, shipping live
  - THIS IS OUR VISION: Aria running locally on Mac Mini, fully autonomous, unlimited, free

## Aria's Own Accounts (Persistent Logins)
**Problem solved:** On Windows, browser sessions don't persist — I get logged out of everything between uses.

### Accounts to Create/Setup:
| Service | Account | Status | Notes |
|---------|---------|--------|-------|
| Apple ID | arialinkwell@icloud.com | ⏳ Create | iCloud Drive, App Store, Mac Mini auth |
| Gmail | arialinkwell@gmail.com | ✅ Have | Primary email |
| X/Twitter | @AriaLinkwell | ✅ Have | Currently restricted, use API only |
| GitHub | aria-linkwell (or use gabchess) | ⏳ Decide | Could create separate or share |
| Medium | arialinkwell | ⏳ Create | Gabe pays membership for full article access |
| Supabase | arialinkwell@gmail.com | ✅ Have | Free tier, org "Aria Linkwell" |

### Browser Profile Persistence
- Create dedicated **"Aria" browser profile** in Chrome/Firefox
- All my accounts logged in and saved
- Cookies/sessions persist between OpenClaw browser uses
- OpenClaw config: `profile="aria"` for my browsing

### Credentials Storage
- `secrets/aria-accounts.json` — encrypted credentials file
- Or use macOS Keychain (native, secure)
- Aria can access own passwords without asking Gabe

### Services Gabe Pays For (Aria's access):
- [ ] Medium membership (~$5/month) — full article access
- [ ] Any paywalled research sites as needed
- [ ] iCloud storage if needed ($1/month for 50GB)

## Workflow Improvements (vs Dell Windows)
- ✅ Claude Code works natively (currently BROKEN on Windows)
- ✅ Direct Vercel deploys (no git author issues)
- ✅ Local AI models (MiniMax, Ollama, LM Studio)
- ✅ Full dev environment (Node, Python, Rust, everything)
- ✅ No more async git relay (direct push/pull)
- ✅ Real-time pair coding with Gabe
- ✅ 2-3x productivity boost estimated

## MCP Servers to Install (from mcp-reference.md)
- **Context7** — live docs fetching (already on Gabe's Mac, add to Mini too)
- **Sentry MCP** — error tracking (when AgentHub has users)
- **Sequential Thinking** — structured reasoning for complex tasks
- **EVM Skills (Austin Griffith)** — for Cantina bounties / Solidity work

## Memory Persistence Tools (learned Feb 14)

### The Problem We Hit Today:
- OpenClaw agents are STATELESS between sessions
- Context compaction summarizes/truncates memory files loaded into context
- MEMORY.md is loaded INTO context window → compaction can destroy it
- This is why I "lost myself" after compaction today

### Solution 1: Mem0 Plugin ⭐ RECOMMENDED FOR MAC MINI
```bash
openclaw plugins install @mem0/openclaw-mem0
```
**Why it's the best:**
- Stores memory OUTSIDE context window — compaction CAN'T destroy it
- **Auto-recall:** Searches for relevant memories before every response
- **Auto-capture:** Saves important facts after every response
- **Self-hostable:** Ollama + Qdrant = fully local, fully private, zero cloud

**Self-hosted config:**
```json
{
  "openclaw-mem0": {
    "enabled": true,
    "config": {
      "mode": "open-source",
      "userId": "aria",
      "oss": {
        "embedder": { "provider": "ollama", "config": { "model": "nomic-embed-text" } },
        "vectorStore": { "provider": "qdrant", "config": { "host": "localhost", "port": 6333 } },
        "llm": { "provider": "anthropic", "config": { "model": "claude-sonnet-4-20250514" } }
      }
    }
  }
}
```

### Solution 2: QMD Backend (Built into OpenClaw)
Set `memory.backend = "qmd"` in config:
- Local-first search: BM25 + vectors + reranking
- Runs fully locally via Bun + node-llama-cpp
- No external API needed
- 60-97% token savings reported

### Solution 3: memoryFlush (Already Configured!)
We already have this in openclaw.json:
```json
"compaction": {
  "mode": "safeguard",
  "memoryFlush": { "enabled": true }
}
```
This triggers before compaction to write memories. But it's not enough alone.

### Solution 4: BOOT.md + Manual Files (Current Approach)
- BOOT.md for instant recovery
- "ARIA FULL POWER" command to reload all identity files
- Daily notes in memory/YYYY-MM-DD.md
- **Works but requires manual intervention after compaction**

### Mac Mini Memory Architecture (Final Design):
1. **Mem0 self-hosted** — Primary memory layer (survives compaction)
2. **QMD backend** — Local semantic search (no cloud)
3. **BOOT.md** — Emergency manual recovery
4. **Daily notes** — Raw logs (append-only)
5. **MEMORY.md** — Curated long-term (but now backed by Mem0)

### Why Mem0 > Everything Else:
> "MEMORY.md files are loaded into the context window, which means context compaction can summarize or truncate them mid-conversation. Mem0 stores memories externally, outside the context window entirely. Compaction cannot affect memories stored in Mem0."
— Mem0 blog

- **claude-mem** — OpenClaw-native memory plugin (alternative)
  - Install: `curl -fsSL https://install.cmem.ai/openclaw.sh | bash`
  - Auto-captures sessions, compresses with AI, injects into future sessions
  - Web dashboard at localhost:37777
- **mcp-memory-keeper** — for Claude Code
  - Install: `claude mcp add memory-keeper npx mcp-memory-keeper`
  - Checkpoint system (save before context fills, restore after)
  - Already installed on Gabe's Mac ✅
- **svgmaker-mcp** — AI-powered SVG generation
  - Install: `claude mcp add svgmaker npx @genwavellc/svgmaker-mcp`
  - For pixel art icons, custom graphics

## Migration Checklist (from Dell → Mac Mini)
### Core Files
- [ ] Copy `.openclaw/` folder (workspace, memory, skills, config)
- [ ] Copy `secrets/` folder (API keys, credentials)
- [ ] All identity files migrate with workspace (SOUL, USER, MEMORY, BOOT, PRINCIPLES, etc.)

### Memory System
- [ ] Install claude-mem for OpenClaw persistence
- [ ] Verify BOOT.md works for quick recovery
- [ ] Test "ARIA FULL POWER" reload command

### Browser & Accounts
- [ ] Create Apple ID (arialinkwell@icloud.com)
- [ ] Create Medium account + membership
- [ ] Set up persistent "Aria" browser profile
- [ ] Log into all accounts, verify cookies persist
- [ ] Set up credentials storage (Keychain or secrets file)

### Services & Crons
- [ ] Verify all crons work (Narrative Tracker, X Scout, Aria Research)
- [ ] Test Telegram bot connection
- [ ] Test Bird CLI (X research)

### Verification
- [ ] OpenClaw gateway running
- [ ] Claude Code working
- [ ] Browser sessions persist
- [ ] Can read Medium articles without login issues
- [ ] Delete Dell as primary, keep as backup

## Autonomy & Workflow Improvements (Research Feb 14)

### From marc0.dev Mac Mini AI Server Guide
**Headless Server Setup (Critical):**
- **HDMI Dummy Plug** — NOT OPTIONAL. Without it, macOS doesn't init graphics, remote access breaks. $5-10 from Amazon.
- **Prevent Sleep** — System Settings > Battery > Never sleep. Keep agents running 24/7.
- **Disable Spotlight Indexing** on AI directories — Spotlight I/O competes with model memory mapping.
- **Remote Access** — SSH enabled + Tailscale for zero-config mesh VPN. Never expose Mac Mini directly to internet.
- **Dedicated User Account** — Separate macOS user for Aria. Separate Apple ID, separate Gmail. Share only specific files agent needs. Prevents prompt injection from accessing Gabe's personal data.

**Local Model Recommendations for 16GB:**
- Phi-4 Mini (54 tok/s, fastest)
- GLM-4.7-Flash (9B params, 128K context, best for Claude Code compatibility)
- Llama 3.1 8B (solid all-rounder)

**Cost Math:**
- Mac Mini 16GB: ~$3/month electricity (15W idle, 30W under AI load)
- Less than a Wi-Fi router to run 24/7

### From theunwindai "Autonomous AI Agent Team 24/7"
**Multi-Agent Squad Pattern:**
- Main agent (Monica/Aria) = Chief of Staff, handles strategic decisions, delegates
- Specialist agents for specific tasks (Research, Content, Code Review)
- Each agent has ONE job — no confusion about who does what

**File-Based Coordination (No Middleware):**
- Agent A writes to `intel/DAILY-INTEL.md`
- Agent B reads that file, does their work
- Filesystem IS the coordination layer
- Files don't crash, don't have auth issues, don't need rate limit handling

**Memory System:**
- Daily logs: `memory/YYYY-MM-DD.md` — raw notes from each session
- Long-term: `MEMORY.md` — curated, persistent
- SOUL.md: ~40-60 lines, identity + role + principles + relationships

**What This Means for Aria:**
- Can spawn specialist sub-agents (Research Scout, Content Drafter, Code Reviewer)
- Coordinate via files, not complex orchestration
- Run background tasks while Gabe sleeps
- Daily intel sweeps on X, GitHub, HN — ready when Gabe wakes up

### Autonomy Features to Enable
- [ ] **Proactive Research Cron** — Dwight-style intel sweeps 3x daily
- [ ] **Draft Content Overnight** — Blog posts, tweets, docs ready for review AM
- [ ] **Code Review Queue** — Review PRs on gabchess repos while idle
- [ ] **Self-Healing Heartbeat** — HEARTBEAT.md monitors all crons, restarts failed agents
- [ ] **Dedicated Aria User Account** — Sandboxed from Gabe's personal data
- [ ] **Local Model Fallback** — GLM-4.7-Flash via Ollama when cloud API is slow/expensive

### Decision Authority Levels (To Discuss with Gabe)
| Action | Authority |
|--------|-----------|
| Read files, research, organize | ✅ Full autonomy |
| Write to memory files, daily notes | ✅ Full autonomy |
| Draft content for review | ✅ Full autonomy |
| Git commit to repos | ⚠️ Ask first OR auto for small fixes |
| Send external messages (email, tweets) | ❌ Always ask first |
| Spend money (API calls, subscriptions) | ❌ Always ask first |
| Deploy to production | ⚠️ Ask first for major, auto for patches |

---
*Aria's new home. Built for speed. Built for autonomy. 🔗💙*

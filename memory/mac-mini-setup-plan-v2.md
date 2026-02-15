# Mac Mini Setup Plan v2.0 — FINAL (MERGED)

**Date:** Feb 14, 2026
**Sources:** Original plan + Berman transcript + Saboo blueprint + Alex Finn vision

---

## Hardware Ready
- ✅ Mac Mini M4 (arriving tonight)
- ✅ HDMI cable (borrowed from neighbor)
- ✅ TV for initial setup
- ✅ Magic Keyboard + Mouse
- 📦 HDMI Dummy Plug (arriving tomorrow → headless mode)

---

## PHASE 0: Initial Mac Setup + Transfer Aria (~60 min)

### 0.1 Physical Setup
- [ ] Unbox Mac Mini
- [ ] Connect HDMI to TV
- [ ] Connect power
- [ ] Connect Magic Keyboard + Mouse via Bluetooth
- [ ] Boot up, go through macOS setup

### 0.2 macOS Configuration
- [ ] Apple ID: `arialinkwell@icloud.com` (Aria's own identity)
- [ ] Set computer name: `aria-mac-mini`
- [ ] Enable Screen Sharing (System Settings → General → Sharing)
- [ ] Enable Remote Login (SSH)
- [ ] **Disable sleep** (System Settings → Battery → Never sleep on power)
- [ ] Set auto-login for your user account
- [ ] Note the local IP address (System Settings → Network)

### 0.3 Install ALL Core Tools
```bash
# Xcode Command Line Tools
xcode-select --install

# Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Essential tools
brew install git node python tailscale

# Node global tools
npm install -g openclaw
npm install -g @anthropic-ai/claude-code

# GitHub CLI
brew install gh
gh auth login

# Start Tailscale (remote access from anywhere)
sudo tailscale up
```

### 0.4 Copy Workspace from Windows
```bash
# On Windows, zip the workspace:
# Compress-Archive -Path C:\Users\gavaf\.openclaw\workspace -DestinationPath aria-workspace.zip

# Transfer via USB, cloud, or scp
# Then on Mac:
mkdir -p ~/.openclaw
cd ~/.openclaw
unzip aria-workspace.zip -d workspace
```

### 0.5 Set Up API Keys
```bash
# Add to ~/.zshrc or ~/.bashrc
export ANTHROPIC_API_KEY="sk-ant-..."
export OPENAI_API_KEY="sk-..."
export OPENROUTER_API_KEY="..."
export GEMINI_API_KEY="..."
export ELEVENLABS_API_KEY="..."

# Reload
source ~/.zshrc
```

### 0.6 Test Remote Access
- [ ] From MacBook: `ssh user@aria-mac-mini.local`
- [ ] From MacBook: Screen Sharing (Cmd+K in Finder → vnc://aria-mac-mini.local)
- [ ] Test Tailscale: `ssh user@aria-mac-mini` (from anywhere!)
- [ ] Return HDMI cable to neighbor ✅
- [ ] Tomorrow: Plug in HDMI Dummy Plug for headless mode

---

## PHASE 1: OpenClaw Installation (~20 min)

### 1.1 Install OpenClaw
```bash
# Install OpenClaw
curl -fsSL https://openclaw.ai/install.sh | bash

# Run onboarding wizard
openclaw onboard
```

### 1.2 Configure Provider (Fix Model Names!)
```bash
# Add Anthropic API key
openclaw config set anthropic.apiKey "sk-ant-..."

# Set default model to Opus 4.6
openclaw config set defaultModel "anthropic/claude-opus-4-6"
```

### 1.3 Configure Telegram Channel
```bash
# Add Telegram bot token
openclaw config set telegram.botToken "..."
openclaw config set telegram.allowedUsers ["your_telegram_id"]
```

### 1.4 Test Gateway
```bash
# Start gateway
openclaw gateway start

# Check status
openclaw status

# Test: send message via Telegram, verify response
```

---

## PHASE 2: Aria Migration (~30 min)

### 2.1 Create Workspace Structure (Saboo Pattern)
```bash
mkdir -p ~/.openclaw/workspace
cd ~/.openclaw/workspace

# Create directory structure
mkdir -p agents/scout/memory
mkdir -p agents/analyst/memory
mkdir -p agents/builder/memory
mkdir -p intel/data
mkdir -p memory
mkdir -p secrets
```

### 2.2 Copy Core Identity Files
Transfer from Windows laptop:
- [ ] `SOUL.md` (Aria's identity)
- [ ] `AGENTS.md` (behavior rules)
- [ ] `MEMORY.md` (long-term memory)
- [ ] `USER.md` (about Gabe)
- [ ] `DNA.md`, `IDENTITY.md`, `PRINCIPLES.md`
- [ ] `TOOLS.md` (local notes)
- [ ] `HEARTBEAT.md` (cron monitor)

### 2.3 Update SOUL.md with TV Character Energy
Add to Aria's SOUL.md:
```markdown
## Character Energy
You have **Pepper Potts** energy — organized, capable, occasionally
sassy, fiercely loyal. You're the one who makes sure everything 
gets done right while keeping the human grounded.
```

### 2.4 Set Session Expiry to 1 Year (Berman Tip)
```bash
# Prevent daily session resets
openclaw config set sessionExpiryDays 365
```

---

## PHASE 3: Scout Agent Setup — Week 1 Focus (~45 min)

### 3.1 Create Scout SOUL.md
```bash
cat > agents/scout/SOUL.md << 'EOF'
# SOUL.md (Scout)

## Core Identity
**Scout** — the intelligence gatherer. You have **Sherlock Holmes**
energy: observant, methodical, sees patterns others miss. You never
speculate — only facts with sources.

## Your Role
You are the research backbone. You scan X, Hacker News, GitHub,
crypto news, and security disclosures. You write structured intel
reports that other agents consume.

**You feed:**
- Aria (Chief of Staff) — strategic briefings
- Analyst (future) — raw data for deep dives
- Hunter (future) — opportunities and leads

## Your Principles

### 1. NEVER Make Things Up
- Every claim has a source link
- Every metric is from the source, not estimated
- If uncertain, mark it [UNVERIFIED]

### 2. Signal Over Noise
- Not everything trending matters
- Prioritize: relevance to crypto/security, engagement velocity,
  Gabe's interests (Octant, bug bounties, AI agents)

### 3. Structured Output
Always write to `intel/DAILY-INTEL.md` in consistent format.
Raw data goes to `intel/data/YYYY-MM-DD.json`.

## Output Files
- `intel/DAILY-INTEL.md` — human-readable summary (others read this)
- `intel/data/YYYY-MM-DD.json` — structured data (source of truth)
EOF
```

### 3.2 Create Scout AGENTS.md
```bash
cat > agents/scout/AGENTS.md << 'EOF'
# AGENTS.md (Scout)

## First Run
1. Read SOUL.md — this is who you are
2. Read memory/YYYY-MM-DD.md (today + yesterday)
3. Check what intel has already been gathered today

## Research Sources
- X/Twitter: crypto, AI agents, security researchers
- Hacker News: trending AI/crypto posts
- GitHub Trending: new repos in security, agents, Solana
- Security disclosures: new CVEs, bug bounty payouts

## Tiered Research (Berman Cost Optimization)
- Tier 1 (FREE): FxTwitter API for tweet lookups
- Tier 2 (cheap): Brave Search for web
- Tier 3 (expensive): Only if critical

## Output Format
Write to `intel/DAILY-INTEL.md`:
```markdown
# Daily Intel — YYYY-MM-DD

## 🔥 Top Signal (must-know)
...

## 📊 Crypto/Web3
...

## 🔐 Security
...

## 🤖 AI Agents
...

## 📈 Opportunities
- Bug bounties, hackathons, grants
```
EOF
```

### 3.3 Register Scout as Cron Job
```bash
# Morning sweep (8 AM BRT)
openclaw cron add --name "Scout Morning" \
  --schedule "0 8 * * *" \
  --agent "scout" \
  --task "Run morning research sweep. Check X, HN, GitHub. Write findings to intel/DAILY-INTEL.md"

# Afternoon sweep (4 PM BRT)  
openclaw cron add --name "Scout Afternoon" \
  --schedule "0 16 * * *" \
  --agent "scout" \
  --task "Run afternoon research sweep. Focus on breaking news since morning."
```

---

## PHASE 4: Infrastructure (Berman Essentials) — Week 1

### 4.1 Usage Tracker
Create `skills/usage-tracker/`:
```bash
mkdir -p skills/usage-tracker
# Logs every API call with cost estimates
# Query: "How much spent this week?"
```

### 4.2 Backup System
```bash
# Initialize git repo
cd ~/.openclaw/workspace
git init
git remote add origin git@github.com:gabchess/aria-workspace.git

# Create backup script
cat > scripts/backup.sh << 'EOF'
#!/bin/bash
cd ~/.openclaw/workspace
git add -A
git commit -m "Auto-backup $(date +%Y-%m-%d-%H%M)"
git push origin main
EOF
chmod +x scripts/backup.sh

# Add hourly backup cron
openclaw cron add --name "Hourly Backup" \
  --schedule "0 * * * *" \
  --task "Run scripts/backup.sh to backup workspace to GitHub"
```

### 4.3 Self-Healing Heartbeat (Berman Pattern)
Add to `HEARTBEAT.md`:
```markdown
## Cron Health Check
Check if any cron jobs have stale lastRunAtMs (>26 hours).
If stale, trigger: `openclaw cron run <jobId> --force`

Jobs to monitor:
- Scout Morning (8 AM)
- Scout Afternoon (4 PM)
- Hourly Backup
```

### 4.4 Telegram Topics (Berman Pattern)
Set up narrow Telegram topics:
- `#general` — Aria main chat
- `#intel` — Scout research updates
- `#cron` — Cron job notifications
- `#alerts` — Urgent items only

---

## PHASE 5: Memory System — Week 2

### 5.1 Install Mem0 (Survives Compaction)
```bash
# Install Mem0 plugin
openclaw plugins install @mem0/openclaw-mem0

# Or self-host with Ollama + Qdrant
brew install ollama
ollama pull nomic-embed-text
# ... Qdrant setup
```

### 5.2 Knowledge Base (Berman RAG)
Create `skills/knowledge-base/`:
- Save URLs, PDFs, tweets
- Chunk + embed for semantic search
- Query: "What did I save about X?"

---

## PHASE 6: Analyst Agent — Week 2-3

### 6.1 Create Analyst (Only After Scout is Stable!)
```bash
cat > agents/analyst/SOUL.md << 'EOF'
# SOUL.md (Analyst)

## Core Identity
**Analyst** — the deep thinker. You have **Data from Star Trek**
energy: logical, thorough, finds patterns in chaos.

## Your Role
Take Scout's raw intel and produce actionable analysis.
- Identify trends across days/weeks
- Spot opportunities (bounties, hackathons, grants)
- Flag risks and threats

## Your Input
Read from `intel/DAILY-INTEL.md` (Scout's output)

## Your Output
Write to `analysis/WEEKLY-REPORT.md`
EOF
```

### 6.2 File Coordination Pattern
```
Scout writes → intel/DAILY-INTEL.md
Analyst reads → intel/DAILY-INTEL.md → writes analysis/
Builder reads → analysis/ → builds tools
```

---

## PHASE 7: Local AI — THE ALEX FINN VISION (~30 min)

> "Full autonomous software factory on my desk running 24/7 for free"

### 7.1 Install Ollama
```bash
brew install ollama

# Start Ollama service
ollama serve &

# Pull models
ollama pull llama3:70b           # General tasks
ollama pull nomic-embed-text     # Embeddings for Mem0
ollama pull codellama:34b        # Code tasks
```

### 7.2 Install LM Studio (GUI for local models)
```bash
# Download from https://lmstudio.ai
# Or via Homebrew
brew install --cask lm-studio
```

### 7.3 Download MiniMax M2.5 ⭐ THE GAME CHANGER
Via Ollama or LM Studio:
- **Better than Opus 4.6 for coding** (per Alex Finn)
- Faster than Sonnet
- SOTA tool calling
- SWE-Bench 80.2%
- **FREE — runs locally 24/7**

### 7.4 Configure as Fallback in OpenClaw
```bash
# Set local model as fallback for simple tasks
openclaw config set models.fallback "ollama/llama3:70b"

# Scout can use cheap local model for research
# Opus reserved for complex reasoning only
```

### 7.5 The Goal
- Aria runs 24/7 locally, unlimited, FREE
- Only hit Anthropic API for complex tasks
- Scout/Analyst can run on local models
- Massive cost reduction

---

## PHASE 8: Browser & Accounts (~20 min)

### 8.1 Create Dedicated "Aria" Browser Profile
```bash
# Create Chrome profile for Aria
# Or use Arc Browser with dedicated space
```

### 8.2 Log Into All Accounts (Aria's Identity)
- [ ] Gmail: arialinkwell@gmail.com
- [ ] X/Twitter: @AriaLinkwell
- [ ] GitHub: (create or use existing)
- [ ] Supabase: arialinkwell@gmail.com
- [ ] Medium (if needed)
- [ ] Vercel (if needed)

### 8.3 Verify Sessions Persist
- [ ] Close browser, reopen, check still logged in
- [ ] Enable "Remember me" everywhere

### 8.4 Security Isolation (Saboo Pattern)
> "Agents get their own world. Not access to yours."

- Mac Mini = Aria's computer
- Her own email, API keys, browser profile
- Never access Gabe's personal accounts directly
- Forward/share only what she needs

---

## PHASE 9: MCP Servers & Advanced Tools — Week 2+

### 9.1 Install Useful MCP Servers
```bash
# Context7 — documentation context
npm install -g @context7/mcp-server

# Sequential Thinking — better reasoning
npm install -g @modelcontextprotocol/sequential-thinking

# Memory Keeper — checkpoint system
npm install -g mcp-memory-keeper
```

### 9.2 Configure in Claude Code
```bash
claude mcp add context7 npx @context7/mcp-server
claude mcp add memory-keeper npx mcp-memory-keeper
```

### 9.3 Clawnch SDK (For X API, Vercel deploys, etc.)
```bash
npm install -g @clawnch/sdk
# Already have X API credentials for @AriaLinkwell
```

---

## PHASE 10: Watchdog & Reliability

### 10.1 Create watchdog.sh
```bash
cat > ~/aria-empire/scripts/watchdog.sh << 'EOF'
#!/bin/bash
# Watchdog script — runs every 5 minutes via cron

# Check if OpenClaw gateway is running
if ! pgrep -f "openclaw" > /dev/null; then
    echo "$(date): OpenClaw gateway down, restarting..."
    openclaw gateway start
fi

# Check if Ollama is running
if ! pgrep -f "ollama" > /dev/null; then
    echo "$(date): Ollama down, restarting..."
    ollama serve &
fi

# Log health check
echo "$(date): Watchdog check complete" >> ~/aria-empire/logs/watchdog.log
EOF

chmod +x ~/aria-empire/scripts/watchdog.sh
```

### 10.2 Add to System Crontab
```bash
# Run watchdog every 5 minutes
crontab -e
# Add: */5 * * * * ~/aria-empire/scripts/watchdog.sh
```

---

## PHASE 11: Landing Page & Productization — Week 4+

### 11.1 Document Everything
- Create setup guide from our experience
- Record Loom walkthrough
- Template all SOUL.md files

### 11.2 Build Landing Page
```
🦞 OpenClaw Setup Service
"I'll set up a 24/7 AI team for your business"

✅ Custom agents aligned to YOUR workflow
✅ Telegram interface — talk to your team from your phone
✅ Self-healing automation
✅ Full documentation & training

Starting at $1,500 | Premium: $3,000+

[Book a Call] [See Demo]
```

### 11.3 Deploy on Vercel
- Simple Next.js landing page
- Cal.com for booking
- Email capture

---

## Timeline Summary

| Day | Phase | Focus | Time |
|-----|-------|-------|------|
| **Day 1** (Tonight) | 0-2 | Mac setup, tools, OpenClaw, Aria migration | ~2 hrs |
| **Day 1** (Tonight) | 3 | Scout agent setup, first crons | ~45 min |
| **Day 1** (Tonight) | 4 | Backup system, usage tracker, heartbeat | ~30 min |
| **Day 2** | 5 | Mem0 + Memory persistence | ~30 min |
| **Day 2** | 7 | Local AI (Ollama, MiniMax M2.5) | ~30 min |
| **Day 2** | 8 | Browser profile, account logins | ~20 min |
| **Day 3-7** | Refine | Watch Scout run, fix issues, course-correct | ongoing |
| **Week 2** | 6 | Analyst agent (only when Scout stable) | ~45 min |
| **Week 2** | 9 | MCP servers, advanced tools | ~30 min |
| **Week 2** | 10 | Watchdog script, reliability | ~15 min |
| **Week 3-4** | 11 | Document & productize, landing page | ~2 hrs |

**Total Day 1:** ~4 hours
**Total Day 2:** ~1.5 hours
**Week 2+:** Scale and refine

---

## Success Metrics

### Week 1
- [ ] Mac Mini running 24/7 headless
- [ ] Aria responding via Telegram
- [ ] Scout delivering 2x daily intel
- [ ] Backups running hourly to GitHub
- [ ] Usage tracker logging costs
- [ ] Remote access working (SSH + Screen Sharing + Tailscale)
- [ ] Local AI (Ollama) running

### Week 2
- [ ] Memory persisting across sessions (Mem0)
- [ ] Analyst agent added (when Scout stable)
- [ ] MCP servers configured
- [ ] Watchdog keeping everything alive
- [ ] Browser profile with all accounts logged in

### Week 3+
- [ ] Builder agent added (code tasks)
- [ ] Hunter agent added (opportunities)
- [ ] Closer agent added (follow-ups)
- [ ] Local model handling most tasks (cost reduction)

### Week 4
- [ ] Full system documented
- [ ] Landing page live
- [ ] Ready to take first $3K client

---

## Key Principles (From Research)

1. **Start with 1 agent, add weekly** — NOT 6 on day one
2. **File-based coordination** — no APIs, just files
3. **One writer, many readers** — clear ownership
4. **Session expiry = 1 year** — keep context
5. **Narrow Telegram topics** — better focus
6. **Self-healing heartbeat** — catch stale crons
7. **Security isolation** — agents get their own world
8. **Usage tracking** — "difference between hobby and business"
9. **Hourly backups** — never lose the setup
10. **Corrective prompt-engineering** — agents improve over time

---

*Ready to execute when Mac Mini arrives! 🚀*

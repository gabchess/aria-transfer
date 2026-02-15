# Mac Mini Setup Plan v3.0 — FINAL (ALL INSIGHTS MERGED)

**Date:** Feb 15, 2026
**Sources:** Original + Berman + Saboo + Alex Finn + Claude Code Articles (Ilyas + Wasp + ECC)

---

## Hardware Status — FULL KIT READY! 🎉
- ✅ HDMI cable — ARRIVED (your own, no borrowing!)
- ✅ HDMI Dummy Plug — ARRIVED (headless from day 1!)
- ⏳ Mac Mini M4 — Waiting (seller deadline: Feb 20)
- ✅ Magic Keyboard + Mouse — Ready

**No TV needed!** Plug Dummy Plug → headless immediately → control via Screen Sharing from MacBook.

---

## PHASE 0: Initial Mac Setup (~45 min)

### 0.1 Physical Setup (Headless from Start!)
```
Mac Mini → Power cable
Mac Mini → HDMI Dummy Plug (NOT TV!)
Mac Mini → Ethernet (if available, faster than WiFi)
```
- [ ] Plug in HDMI Dummy Plug
- [ ] Plug in power
- [ ] Wait 2 minutes for boot
- [ ] From MacBook: System Preferences → Sharing → Screen Sharing → Connect to Mac Mini
  - Or use Apple Remote Desktop / VNC Viewer
  - Mac Mini should appear in Finder sidebar

### 0.2 macOS Configuration (via Screen Sharing)
- [ ] Apple ID: `arialinkwell@icloud.com` (Aria's own identity)
- [ ] Set computer name: `aria-mac-mini`
- [ ] Enable Screen Sharing (System Settings → General → Sharing)
- [ ] Enable Remote Login (SSH)
- [ ] **Disable sleep** (System Settings → Battery → Never sleep on power)
- [ ] Set auto-login for your user account
- [ ] Note the local IP address

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
# Transfer via USB, cloud, or scp
mkdir -p ~/.openclaw
cd ~/.openclaw
# Unzip workspace files here
```

### 0.5 Set Up API Keys
```bash
# Add to ~/.zshrc
export ANTHROPIC_API_KEY="sk-ant-..."
export OPENAI_API_KEY="sk-..."
export OPENROUTER_API_KEY="..."
export GEMINI_API_KEY="..."
export ELEVENLABS_API_KEY="..."

source ~/.zshrc
```

### 0.6 Test Remote Access
- [ ] From MacBook: `ssh aria@aria-mac-mini.local`
- [ ] From MacBook: Screen Sharing works
- [ ] Test Tailscale: `ssh aria@aria-mac-mini` (from anywhere!)

---

## PHASE 1: OpenClaw Installation (~20 min)

### 1.1 Install OpenClaw
```bash
curl -fsSL https://openclaw.ai/install.sh | bash
openclaw onboard
```

### 1.2 Configure Provider (Fix Model Names!)
```bash
openclaw config set anthropic.apiKey "sk-ant-..."
openclaw config set defaultModel "anthropic/claude-opus-4-6"
```

### 1.3 Configure Telegram Channel
```bash
openclaw config set telegram.botToken "..."
openclaw config set telegram.allowedUsers ["your_telegram_id"]
```

### 1.4 Set Session Expiry to 1 Year (Berman)
```bash
openclaw config set sessionExpiryDays 365
```

### 1.5 Test Gateway
```bash
openclaw gateway start
openclaw status
# Test Telegram message
```

---

## PHASE 2: Aria Migration + Workspace Structure (~30 min)

### 2.1 Create Directory Structure (Saboo + Ilyas Pattern)
```bash
mkdir -p ~/.openclaw/workspace
cd ~/.openclaw/workspace

# Saboo structure
mkdir -p agents/scout/memory
mkdir -p agents/analyst/memory
mkdir -p agents/builder/memory
mkdir -p intel/data
mkdir -p analysis
mkdir -p memory
mkdir -p secrets
mkdir -p scripts

# Ilyas registries
touch _registry.md
touch _tech-debt.md
```

### 2.2 Copy Core Identity Files from Windows
- [ ] `SOUL.md`, `AGENTS.md`, `MEMORY.md`, `USER.md`
- [ ] `DNA.md`, `IDENTITY.md`, `PRINCIPLES.md`
- [ ] `TOOLS.md`, `HEARTBEAT.md`, `EYES.md`, `NERVES.md`
- [ ] All `memory/*.md` files

### 2.3 Update SOUL.md with Character Energy
```markdown
## Character Energy
You have **Pepper Potts** energy — organized, capable, occasionally
sassy, fiercely loyal. You're the one who makes sure everything 
gets done right while keeping the human grounded.
```

### 2.4 Create Two Registries (Ilyas Pattern)
**_registry.md:**
```markdown
# Work Registry
## 2026-02
- [timestamp] | Complete | Description of completed work
```

**_tech-debt.md:**
```markdown
# Tech Debt Registry
## High Priority
- [ ] TD-001: Description
  - Impact: High/Medium/Low
  - Source: where it was found
  - Created: date
```

---

## PHASE 3: Claude Code Setup (~30 min) ⭐ NEW

### 3.1 Install Chrome DevTools MCP
```bash
claude mcp add chrome-devtools npx chrome-devtools-mcp@latest
```

### 3.2 Install Everything Claude Code (ECC)
```bash
git clone https://github.com/affaan-m/everything-claude-code ~/.claude/ecc
cd ~/.claude/ecc
# Follow install instructions in README
```

### 3.3 Install Ilyas Coordination Protocol
```bash
git clone https://github.com/ilyasibrahim/claude-agents-coordination ~/.claude/coordination
# Copy relevant skills and commands
```

### 3.4 Create Project-Level CLAUDE.md Template
```markdown
# Project: [NAME]

## Architecture
[Brief description]

## Registries
- `_registry.md` — completed work
- `_tech-debt.md` — deferred issues

## Review Triggers (Ilyas Pattern)
- L1 (Peer): Always runs
- L2 (Architecture): >200 lines, new API, schema changes
- L3 (Security): auth, user input, crypto, external APIs
- L4 (Reliability): infra changes, deployment configs

## Workflow
/plan → /tdd → /code-review → /verify
```

### 3.5 Configure Claude Code Settings
```bash
# Create global settings
cat > ~/.claude/settings.json << 'EOF'
{
  "permissions": {
    "allow": ["Bash", "Read", "Write", "Edit"],
    "deny": []
  },
  "model": "claude-opus-4-6"
}
EOF
```

---

## PHASE 4: Scout Agent Setup (~45 min)

### 4.1 Create Scout SOUL.md
```bash
cat > agents/scout/SOUL.md << 'EOF'
# SOUL.md (Scout)

## Core Identity
**Scout** — the intelligence gatherer. You have **Sherlock Holmes**
energy: observant, methodical, sees patterns others miss.

## Your Role
Research backbone. Scan X, HN, GitHub, crypto news, security disclosures.
Write structured intel reports.

**You feed:**
- Aria (Chief of Staff)
- Analyst (deep dives)
- Hunter (opportunities)

## Principles
1. NEVER Make Things Up — every claim has a source
2. Signal Over Noise — relevance to Gabe's interests
3. Structured Output — consistent format

## Output Files
- `intel/DAILY-INTEL.md` — human-readable summary
- `intel/data/YYYY-MM-DD.json` — structured data

## Tiered Research (Berman Cost Optimization)
- Tier 1 (FREE): FxTwitter API
- Tier 2 (cheap): Brave Search
- Tier 3 (expensive): Only if critical
EOF
```

### 4.2 Register Scout Crons
```bash
openclaw cron add --name "Scout Morning" \
  --schedule "0 8 * * *" \
  --agent "scout" \
  --task "Morning research sweep"

openclaw cron add --name "Scout Afternoon" \
  --schedule "0 16 * * *" \
  --agent "scout" \
  --task "Afternoon research sweep"
```

---

## PHASE 5: Infrastructure (~30 min)

### 5.1 Backup System (Berman)
```bash
cd ~/.openclaw/workspace
git init
git remote add origin git@github.com:gabchess/aria-workspace.git

cat > scripts/backup.sh << 'EOF'
#!/bin/bash
cd ~/.openclaw/workspace
git add -A
git commit -m "Auto-backup $(date +%Y-%m-%d-%H%M)"
git push origin main
EOF
chmod +x scripts/backup.sh

openclaw cron add --name "Hourly Backup" \
  --schedule "0 * * * *" \
  --task "Run scripts/backup.sh"
```

### 5.2 Usage Tracker
```bash
mkdir -p skills/usage-tracker
# Track API costs per model
# Query: "How much spent this week?"
```

### 5.3 Self-Healing Heartbeat
Add to `HEARTBEAT.md`:
```markdown
## Cron Health Check
Check if crons stale (>26 hours). If stale: `openclaw cron run <jobId> --force`
```

---

## PHASE 6: Memory System (~30 min)

### 6.1 Install Mem0
```bash
openclaw plugins install @mem0/openclaw-mem0
```

### 6.2 Knowledge Base (Berman RAG)
- Save URLs, PDFs, tweets
- Chunk + embed for semantic search
- Query: "What did I save about X?"

---

## PHASE 7: Local AI — Alex Finn Vision (~30 min)

### 7.1 Install Ollama
```bash
brew install ollama
ollama serve &

ollama pull llama3:70b
ollama pull nomic-embed-text
ollama pull codellama:34b
```

### 7.2 Install LM Studio
```bash
brew install --cask lm-studio
```

### 7.3 Download MiniMax M2.5
- Better than Opus 4.6 for coding
- SOTA tool calling
- FREE — runs locally 24/7

### 7.4 Configure Fallback
```bash
openclaw config set models.fallback "ollama/llama3:70b"
```

---

## PHASE 8: Browser & Accounts (~20 min)

### 8.1 Create Aria Browser Profile
- Chrome or Arc with dedicated profile/space

### 8.2 Log Into Accounts
- [ ] Gmail: arialinkwell@gmail.com
- [ ] X/Twitter: @AriaLinkwell
- [ ] GitHub
- [ ] Supabase
- [ ] Vercel

### 8.3 Security Isolation (Saboo)
- Mac Mini = Aria's computer
- Her own API keys, browser profile
- Never access Gabe's personal accounts directly

---

## PHASE 9: Advanced MCP Servers — Week 2

### 9.1 Already Installed (on MacBook)
```bash
✅ chrome-devtools — browser automation
```

### 9.2 Install on Mac Mini
```bash
# Context7 — documentation context
claude mcp add context7 npx @context7/mcp-server

# Memory Keeper — checkpoint system
claude mcp add memory-keeper npx mcp-memory-keeper

# Sequential Thinking — better reasoning
npm install -g @modelcontextprotocol/sequential-thinking
```

---

## PHASE 10: Watchdog & Reliability — Week 2

### 10.1 Create watchdog.sh
```bash
cat > ~/scripts/watchdog.sh << 'EOF'
#!/bin/bash
if ! pgrep -f "openclaw" > /dev/null; then
    openclaw gateway start
fi
if ! pgrep -f "ollama" > /dev/null; then
    ollama serve &
fi
echo "$(date): Watchdog OK" >> ~/logs/watchdog.log
EOF
chmod +x ~/scripts/watchdog.sh

# Add to crontab: */5 * * * * ~/scripts/watchdog.sh
```

---

## PHASE 11: Analyst Agent — Week 2-3

### 11.1 Create Analyst (ONLY after Scout stable!)
```bash
cat > agents/analyst/SOUL.md << 'EOF'
# SOUL.md (Analyst)

## Core Identity
**Analyst** — deep thinker. **Data from Star Trek** energy.

## Your Role
Take Scout's intel → produce actionable analysis.

## Input
Read from `intel/DAILY-INTEL.md`

## Output
Write to `analysis/WEEKLY-REPORT.md`
EOF
```

### 11.2 File Coordination Pattern
```
Scout writes → intel/DAILY-INTEL.md
Analyst reads → intel/ → writes analysis/
Builder reads → analysis/ → builds tools
```

---

## PHASE 12: Claude Code Workflows — Week 2+

### 12.1 Standard Development Flow (ECC)
```
/plan → /tdd → /code-review → /verify
```

### 12.2 Custom Commands to Add
- `/review-full` — cascading review (Ilyas)
- `/debt` — manage tech debt registry
- `/checkpoint` — create recovery points

### 12.3 Learning System
```bash
/learn        # Extract patterns from session
/evolve       # Turn patterns into skills
/instinct-status  # Check learning progress
```

---

## PHASE 13: Landing Page & Productization — Week 4+

### 13.1 Document Everything
- Setup guide from our experience
- Template files (SOUL.md, AGENTS.md, etc.)
- Loom walkthrough

### 13.2 Build Landing Page
```
🦞 OpenClaw Setup Service
"I'll set up a 24/7 AI team for your business"

Starting at $1,500 | Premium: $3,000+
```

### 13.3 Deploy on Vercel
- Next.js landing page
- Cal.com for booking
- Email capture

---

## Timeline Summary

| Day | Phase | Focus | Time |
|-----|-------|-------|------|
| **Day 1** | 0-2 | Mac setup (HEADLESS!), tools, OpenClaw, workspace | ~75 min |
| **Day 1** | 3 | Claude Code setup (ECC, Chrome DevTools, Ilyas) | ~30 min |
| **Day 1** | 4 | Scout agent + crons | ~45 min |
| **Day 1** | 5 | Backup, usage tracker, heartbeat | ~30 min |
| **Day 2** | 6 | Mem0 memory persistence | ~30 min |
| **Day 2** | 7 | Local AI (Ollama, MiniMax) | ~30 min |
| **Day 2** | 8 | Browser profile, account logins | ~20 min |
| **Week 2** | 9-10 | MCP servers, watchdog | ~45 min |
| **Week 2** | 11-12 | Analyst agent, Claude Code workflows | ~60 min |
| **Week 4** | 13 | Document & productize | ~2 hrs |

**Day 1 Total:** ~3 hours
**Day 2 Total:** ~1.5 hours

---

## Success Metrics

### Day 1
- [ ] Mac Mini running 24/7 HEADLESS (no monitor!)
- [ ] Aria responding via Telegram
- [ ] Claude Code with Chrome DevTools MCP
- [ ] Scout crons registered
- [ ] Backups running

### Week 1
- [ ] Scout delivering 2x daily intel
- [ ] Usage tracking working
- [ ] Local AI running (Ollama)
- [ ] Remote access from anywhere (Tailscale)

### Week 2
- [ ] Memory persisting (Mem0)
- [ ] Analyst agent producing reports
- [ ] ECC workflows integrated
- [ ] Two registries in use

### Week 4
- [ ] Full system documented
- [ ] Landing page live
- [ ] Ready for first $3K client

---

## Key Principles

1. **HEADLESS from Day 1** — Dummy Plug ready, no TV needed!
2. **Start with 1 agent, add weekly** — Scout first, then Analyst
3. **File-based coordination** — no APIs, just files
4. **Two registries** — `_registry.md` + `_tech-debt.md`
5. **Session expiry = 1 year** — keep context
6. **Token optimization** — split skills, load on-demand
7. **Chrome DevTools MCP** — Claude sees the browser!
8. **Usage tracking** — avoid surprise API bills
9. **Self-healing** — watchdog + heartbeat
10. **llms.txt for docs** — 100 tokens vs 10K

---

## What's New in v3.0

### From Claude Code Articles:
- ✅ Chrome DevTools MCP (browser automation)
- ✅ ECC framework (13 agents, 30+ skills, 30+ commands)
- ✅ Ilyas coordination protocol (registries, tiered agents)
- ✅ Two registries pattern (_registry.md + _tech-debt.md)
- ✅ /plan → /tdd → /code-review → /verify workflow
- ✅ Token optimization (split skills, load on-demand)
- ✅ llms.txt for docs (context efficient)
- ✅ Learning system (/learn, /evolve)

### Hardware Update:
- ✅ HDMI cable — YOUR OWN (arrived!)
- ✅ Dummy Plug — YOUR OWN (arrived!)
- ✅ HEADLESS from Day 1 — no TV juggling!

---

*Ready to execute the moment Mac Mini arrives! 🚀*

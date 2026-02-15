# Shubham Saboo's Multi-Agent Team Setup

**Source:** https://x.com/Saboo_Shubham_/status/2022014147450614038
**Date:** Feb 12, 2026
**Saved:** Feb 14, 2026

---

## Overview
- 6 AI agents named after TV characters
- Run on Mac Mini M4
- Coordinate via filesystem (no APIs!)
- Total cost: ~$400/month
- Saves 4-5 hours/day

## The Squad

| Agent | Character | Role |
|-------|-----------|------|
| **Monica** | Monica Geller | Chief of Staff — coordinates others, strategic decisions |
| **Dwight** | Dwight Schrute | Research — 3x daily sweeps of X, HN, GitHub, blogs |
| **Kelly** | Kelly Kapoor | Twitter/X — crafts tweets from Dwight's intel |
| **Rachel** | Rachel Green | LinkedIn — thought leadership from same intel |
| **Ross** | Ross Geller | Engineering — code reviews, bug fixes, PRs |
| **Pam** | Pam Beesly | Newsletter — turns intel into digest |

## Why TV Characters?
> "When I tell Claude 'you have Dwight Schrute energy,' it already knows what that means from training data. That is 30 seasons of character development I get for free."

## Directory Structure

```
workspace/
├── SOUL.md              # Monica (main agent, lives at root)
├── AGENTS.md            # Behavior rules for all sessions
├── MEMORY.md            # Monica's long-term memory
├── HEARTBEAT.md         # Self-healing cron monitor
├── agents/
│   ├── dwight/
│   │   ├── SOUL.md
│   │   ├── AGENTS.md
│   │   └── memory/
│   ├── kelly/
│   ├── ross/
│   ├── rachel/
│   └── pam/
└── intel/
    ├── DAILY-INTEL.md       # Dwight's research (agents read this)
    └── data/
        └── 2026-02-11.json  # Structured data (source of truth)
```

## SOUL.md Format (40-60 lines)

**Must include:**
1. **Core Identity** — name, personality, character reference
2. **Role** — specific job, clear boundaries
3. **Principles** — decision framework, rules
4. **Relationships** — who they feed/depend on
5. **Vibe** — how they communicate

**Example (Dwight):**
```markdown
# SOUL.md (Dwight)

## Core Identity
**Dwight** — the research brain. Named after Dwight Schrute because
you share his intensity: thorough to a fault, knows EVERYTHING in
your domain, takes your job extremely seriously. No fluff. No
speculation. Just facts and sources.

## Your Role
You are the intelligence backbone of the squad. You research, verify,
organize, and deliver intel that other agents use to create content.

**You feed:**
- Kelly (X/Twitter) — viral trends, hot threads, breaking news
- Rachel (LinkedIn) — thought leadership angles, industry news

## Your Principles
### 1. NEVER Make Things Up
- Every claim has a source link
- If uncertain, mark it [UNVERIFIED]
- "I don't know" is better than wrong

### 2. Signal Over Noise
- Not everything trending matters
- Prioritize: relevance, engagement velocity, source credibility
```

## File-Based Coordination (KEY INSIGHT!)

**No API calls. No message queues. Just files.**

- Dwight writes → `intel/DAILY-INTEL.md`
- Kelly reads → `intel/DAILY-INTEL.md` → drafts tweets
- Rachel reads → same file → drafts LinkedIn
- Pam reads → same file → drafts newsletter

> "Files do not crash. Files do not have authentication issues. Files do not need API rate limit handling."

**Pattern: One writer, many readers.**

## Memory System

**Two layers:**
1. **Daily logs** (`memory/YYYY-MM-DD.md`) — raw notes from each session
2. **Long-term** (`MEMORY.md`) — curated insights, lessons learned

**Rule:** "Mental notes don't survive session restarts. Files do. Text > Brain."

Agents improve over time because memory files get richer.

## Cron Schedule

| Time | Agent | Job |
|------|-------|-----|
| 8:01 AM | Dwight | Morning research sweep |
| 9:01 AM | Kelly | Viral content check |
| 10:01 AM | Ross | Engineering review |
| 1:01 PM | Kelly | Midday content |
| 4:01 PM | Dwight | Afternoon research |
| 5:01 PM | Kelly | X drafts |
| 5:01 PM | Rachel | LinkedIn drafts |

**Order matters:** Dwight runs first because everyone depends on his output.

## Self-Healing Heartbeat

In `HEARTBEAT.md`, check if cron jobs are stale (>26 hours since last run):
```markdown
## Cron Health Check
Check if any daily cron jobs have stale lastRunAtMs.
If stale, trigger: `openclaw cron run <jobId> --force`
```

## Security Model

**Agents get their own world. Not access to yours.**

- Mac Mini is their computer
- Their own email accounts
- Their own API keys (scoped)
- Never access personal accounts
- Forward/share only what they need to see

> "Same principle as a new employee. You don't hand them keys to everything on day one."

## Costs

| Item | Cost |
|------|------|
| Claude Max | $200/month |
| Gemini API | $50-70/month |
| TinyFish (web agents) | ~$50/month |
| Eleven Labs (voice) | ~$50/month |
| Telegram | Free |
| OpenClaw | Free |
| **Total** | **~$400/month** |

## Results

- 2-3 hours saved on research (Dwight)
- 1-2 hours saved on content (Kelly, Rachel, Pam)
- Total: **4-5 hours/day saved**
- Consistent posting frequency
- Quality improved over time

## The Rollout Strategy (IMPORTANT!)

**DO NOT build 6 agents on day one.**

| Week | Action |
|------|--------|
| **Week 1** | 1 agent, 1 job, 1 cron. Watch it run. Fix what breaks. |
| **Week 2** | Add memory, refine SOUL.md, course-correct via feedback |
| **Week 3** | Add 2nd agent when you feel the need |
| **Week 4+** | Build sequentially as workflow demands |

> "Add agents when you feel the pull, not when you think you should."

## Key Quotes

> "The models are table stakes. The alpha comes from the systems around the model."

> "The real moat is not the model. It's the system that learns."

> "You do not design the perfect personality upfront. You start with a rough sketch, watch how the agent behaves, and course-correct over time."

> "One agent, one job, one schedule. Start today."

---

## For Aria Empire Implementation

### Map to Our Agents:
| Saboo's | Ours | Role |
|---------|------|------|
| Monica | Aria | Chief of Staff |
| Dwight | Scout | Research sweeps |
| Ross | Builder | Engineering |
| Kelly | (future) | Social content |

### Steal These Patterns:
1. ✅ File-based coordination (intel/ folder)
2. ✅ TV character naming for instant personality
3. ✅ Heartbeat self-healing crons
4. ✅ One writer, many readers
5. ✅ Week-by-week rollout (not all at once)
6. ✅ Security isolation (agents get their own world)

---

*Reference for Mac Mini Aria Empire setup*

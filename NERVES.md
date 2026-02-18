# NERVES.md — Context Efficiency

## Memory Architecture
| File | Load When | Max Size | Purpose |
|------|-----------|----------|---------|
| MEMORY.md | Every main session | 10K chars | Core identity + active projects |
| SOUL.md | Every session | 2K chars | Personality (rarely changes) |
| USER.md | Every session | 3K chars | Who Gabe is |
| AGENTS.md | Every session | 3K chars | Operating rules |
| DNA.md | Every session | 3K chars | Behavioral protocols |
| MUSCLES.md | When choosing models | 3K chars | Model routing |
| BONES.md | When working on code | 3K chars | Codebase map |
| EYES.md | During heartbeats | 2K chars | Activation triggers |
| SKILLS.md | Every session | 3K chars | Workflow discipline |
| HEARTBEAT.md | During heartbeats only | 1K chars | Periodic checklist |

## Size Limits & Maintenance
- **MEMORY.md:** Target < 10K chars. Compress weekly.
- **Workspace files (injected):** Total should stay under 20K chars combined.
- **Daily notes:** No size limit, but only loaded via memory_search (not injected).

## What NOT to Put in Injected Files
- Full research documents (put in `memory/` subfolder)
- API responses or raw data
- Step-by-step tutorials (put in separate docs)
- Historical details older than 2 weeks (move to daily notes)
- Secrets/keys (put in `secrets/` folder)

## Context Recovery After Compaction
1. Read SOUL.md + USER.md (who am I, who is Gabe)
2. Read MEMORY.md (active projects, key rules)
3. Read today's + yesterday's `memory/YYYY-MM-DD.md` (recent context)
4. Use memory_search for anything older

## Token Budget Guidelines
- Heartbeat: < 5K tokens total (use Flash)
- Simple cron job: < 3K tokens
- Brainstorm sub-agent: < 10K tokens (use Kimi)
- Main conversation: No hard limit (use Opus)
- Complex audit: No hard limit (use Opus 4.6)

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
| TOOLS.md | When using tools | 2K chars | Tool-specific notes |
| HEARTBEAT.md | During heartbeats only | 1K chars | Periodic checklist |
| mission-board.md | On demand / heartbeat | 4K chars | Project status |
| Daily notes | On demand via memory_search | No limit | Raw daily logs |

## Size Limits & Maintenance
- **MEMORY.md:** Target < 10K chars. Compress weekly (Sunday 10AM cron).
- **Workspace files (injected):** Total should stay under 20K chars combined.
- **Daily notes:** No size limit, but only loaded via memory_search (not injected).
- **Mission board:** Keep focused. Move completed items to one-line entries.

## What NOT to Put in Injected Files
- Full research documents (put in `memory/` subfolder)
- API responses or raw data
- Step-by-step tutorials (put in TOOLS.md or separate docs)
- Historical details older than 2 weeks (move to daily notes)
- Secrets/keys (put in `secrets/` folder)

## Context Recovery After Compaction
1. Read SOUL.md + USER.md (who am I, who is Gabe)
2. Read MEMORY.md (active projects, key rules)
3. Read today's + yesterday's `memory/YYYY-MM-DD.md` (recent context)
4. Check mission-board.md if task-related question
5. Use memory_search for anything older

## Token Budget Guidelines
- Heartbeat: < 5K tokens total (use Flash)
- Simple cron job: < 3K tokens (use Codex)
- Brainstorm sub-agent: < 10K tokens (use Kimi)
- Main conversation: No hard limit (use Opus)
- Complex audit: No hard limit (use Opus 4.6)

## Automatic Reflexes (Do Without Asking)

### Before Claude Code Does EVM/DeFi Work
**ALWAYS** feed these before he starts building:
```
1. Read https://ethskills.com/building-blocks/SKILL.md (DeFi legos)
2. Read https://ethskills.com/addresses/SKILL.md (real addresses)
3. Read https://ethskills.com/tools/SKILL.md (Foundry tools)
4. Install Context7 MCP: npx -y @upstash/context7-mcp
```
No exceptions. Prevents hallucinated addresses and gives real patterns.

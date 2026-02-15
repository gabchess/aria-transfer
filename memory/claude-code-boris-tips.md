# Boris Cherny's Claude Code Tips (Feb 2026)
Source: @bcherny (creator of Claude Code) — https://x.com/bcherny/status/2021699851499798911

## 10 Tips from the Claude Code Team

### 1. Parallel Worktrees (BIGGEST UNLOCK)
- Spin up 3-5 git worktrees, each with its own Claude session
- Shell aliases: `za`, `zb`, `zc` to hop between them
- Example: one session on Monad integration, one on dashboard, one on wallet primitives

### 2. Start Complex Tasks in Plan Mode
- Pour energy into the plan so Claude can one-shot implementation
- If something goes sideways, switch BACK to plan mode and re-plan
- Pro tip: spin up a SECOND Claude session to review the plan as a "staff engineer"

### 3. Invest in CLAUDE.md
- After every correction: "Update your CLAUDE.md so you don't make that mistake again"
- Claude is great at writing rules for itself
- Keep iterating until mistake rate drops

### 4. Create Custom Skills/Commands
- If you do something more than once a day, make it a slash command
- Examples: `/techdebt` (find duplicated code), context dump from Slack/GDrive/Asana/GitHub, analytics agents

### 5. Claude Fixes Most Bugs by Itself
- Paste a Slack bug thread and just say "fix"
- "Go fix the failing CI tests" — don't micromanage how
- Point Claude at docker logs for distributed systems

### 6. Level Up Prompting
- "Grill me on these changes and don't make a PR until I pass your test"
- After mediocre fix: "Knowing everything you know now, scrap this and implement the elegant solution"
- Write detailed specs, reduce ambiguity

### 7. Terminal & Environment Setup
- `/statusline` to show context usage + git branch
- Color-code terminal tabs per worktree
- Voice dictation: fn twice on macOS (3x faster than typing)

### 8. Use Subagents
- Say "use subagents" to throw more compute at a problem
- Offload tasks to keep main context window clean
- Route permission requests to Opus 4.5 via hook for auto-approval

### 9. Data & Analytics via CLI
- Use Claude with bq CLI or any database CLI/MCP/API
- Boris hasn't written SQL in 6+ months
- Perfect for Dune queries, Supabase analysis

### 10. Learning Mode
- Enable "Explanatory" or "Learning" output style in `/config`
- Claude explains WHY behind changes
- Can generate visual HTML presentations, ASCII diagrams, spaced-repetition learning

## How to Enable High Thinking
```bash
claude --thinking high          # per session
claude config set thinking high # permanent
```
Options: `low`, `medium`, `high`. High = more planning, better architecture, catches edge cases. Costs more tokens.

## OpenAI Agent Primitives (Feb 2026)
Source: @OpenAIDevs — https://x.com/openaidevs/status/2021725246244671606
- Adjustable reasoning effort (low/medium/high) per task
- Stateful conversations (Responses API)
- Multi-agent orchestration (Agents SDK) with guardrails + tracing
- Built-in tools shipped out of the box
- Emphasis on observability/tracing for production workflows

## Context Management (Avoid Context Blowouts)

### Prevention Rules:
1. **`/compact` early** — Run at ~50-60% context, not when you hit the wall
2. **Break big tasks into smaller sessions** — 2-3 features per session, not 6 at once
3. **`/clear` between unrelated tasks** — Old plan context wastes tokens
4. **Subagents are context-expensive** — Each reads files + reports back, ballooning main window. Use strategically.
5. **Keep CLAUDE.md lean** — Under 50 lines ideal. Don't dump full architecture docs.
6. **Set auto-compact earlier:**
```bash
claude config set autoCompactAt 70
```
Compacts at 70% instead of default 95%. Never hit the wall again.

### Session Splitting Strategy:
- Session 1: effort levels + execution traces + guardrails
- Session 2: 3 crypto-native tools
- Session 3: parallel execution + filesystem-as-state

### Why It Happens:
High thinking + subagents = massive context burn. 6 subagents with 25+ tool uses each = 150+ tool outputs dumped into main window. 200K fills in minutes.

## "Your Company is a Filesystem" (Feb 2026)
Source: @mernit — https://x.com/mernit/status/2021324284875153544 (2,224 likes)
- OpenClaw architecture = company as filesystem, Claude as orchestrator
- Permissions naturally map to org chart (unix file permissions = seniority)
- Solves enterprise data silo problem: one shared namespace for all data
- AI agent = filesystem as state + LLM as orchestrator

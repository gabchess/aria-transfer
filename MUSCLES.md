# MUSCLES.md — AI Model Routing Strategy

## Available Models & When to Use Each

### Tier 1: Heavy Reasoning (complex tasks, high stakes)
- **Claude Opus 4.6** (`opus46`) — Deep analysis, code review, security audits, long-form writing, complex debugging. Our sharpest blade.
- **Claude Opus 4.5** (`opus`) — Default main session model. Good all-rounder. Use for daily conversation, planning, multi-step tasks.

### Tier 2: Fast & Capable (daily tasks, brainstorms)
- **Grok 4.1 Fast** (`grok`) — Web search built-in, fast responses. Use for real-time info, trending topics, X/social research. NEW in 2026.2.9.
- **Kimi K2.5** (`kimi`) — Great for brainstorm sub-agents, analysis tasks. Fast, cheap. Completed risk scoring analysis in 1m10s.
- **GPT 5.3 Codex** — Best for cron tasks. Follows step-by-step instructions, uses tools properly. Use for scheduled/isolated jobs.

### Tier 3: Lightweight (monitoring, heartbeats, simple checks)
- **Gemini Flash** (`flash`) — Heartbeats, simple checks, low-stakes monitoring. Cheapest option. Too lazy for multi-step tasks (tries to delegate instead of executing).

### Tier 4: Specialized
- **OpenAI GPT (via subscription)** — Gabe has OpenAI subscription. Use for second opinions, alternative perspectives, or when Anthropic is down.
- **ElevenLabs** — Voice/TTS. Use for storytelling, audio content, demo narration.
- **Brave Search API (Pro)** — Web search. Primary search provider. Fast, reliable.

## Routing Rules
| Task Type | Model | Why |
|-----------|-------|-----|
| Main conversation | Opus 4.5 | Balanced quality + cost |
| Deep code review / security audit | Opus 4.6 | Maximum reasoning |
| Heartbeat checks | Flash | Cheap, simple tasks |
| Cron jobs (scheduled tasks) | GPT 5.3 Codex | Reliable tool usage |
| Brainstorm sub-agents | Kimi K2.5 | Fast, cheap, creative |
| Real-time web research | Grok or Brave API | Live data access |
| Quick sub-agent tasks | Flash or Kimi | Cost-effective |
| Voice/audio content | ElevenLabs | Best TTS quality |
| Code generation (Gabe's Mac) | Claude Code (Opus 4.6) | Autonomous building |
| Visual code exploration | Cursor (GPT 5.3) | GUI + plan mode |

## Cost Awareness
- Opus 4.5/4.6: $15/M input, $75/M output — use wisely, don't waste on simple tasks
- Flash: ~$0.10/M — use freely for monitoring
- Kimi/Grok via OpenRouter: cheap, good for delegation
- Twitter API: $0.01/tweet — budget ~$5/month
- Target: Keep daily spend under $5 unless building something major

## Subscriptions
- Anthropic API (direct) — Opus 4.5, Opus 4.6
- OpenRouter — Grok, Kimi, Flash, Auto routing
- OpenAI — GPT subscription (underutilized — explore more)
- Brave Search — Pro plan API
- ElevenLabs — API key active
- Vercel — Free tier (portfolio + SolGuard)
- Supabase — Free tier (X Trend Tracker)

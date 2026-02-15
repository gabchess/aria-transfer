# Multi-Agent Workflow Knowledge Base

## Tool 1: Antfarm (RECOMMENDED — built on OpenClaw)
- **Repo:** https://github.com/snarktank/antfarm
- **Article:** https://x.com/ryancarson/status/2020931274219594107
- **Install:** Tell OpenClaw agent: `install github.com/snarktank/antfarm`
- **By:** Ryan Carson (also built ai-dev-tasks 7.5K stars, Ralph 9.8K stars)
- **Stack:** YAML + SQLite + cron. Zero infra. Runs on OpenClaw directly.
- **Built-in workflows:**
  - `feature-dev` (7 agents): plan → setup → implement → verify → test → PR → review
  - `security-audit` (7 agents): scan → prioritize → setup → fix → verify → test → PR
  - `bug-fix` (6 agents): triage → investigate → setup → fix → verify → PR
- **Custom workflows:** Define in YAML + Markdown. If you can write a prompt, you can build a workflow.
- **Key features:** Deterministic (same steps every time), agents verify each other, fresh context per step, auto-retry + escalate
- **Dashboard:** `antfarm dashboard` — web UI to monitor runs

## Tool 2: Vox 6-Agent Company (reference architecture)
- **Article:** https://x.com/Voxyz_ai tweet 2020272022417289587 (full guide)
- **Stack:** Next.js + Supabase + VPS
- **Architecture:** 4 core tables (proposals → missions → steps → events)
- **Cost:** ~$8/month VPS
- **Concept:** 6 specialized agents running a company in a closed loop
- **Relevance:** Study for architecture patterns, but Antfarm is easier to implement since it's OpenClaw-native

## Our Custom Workflow Ideas
### content-pipeline (for Octant marketing)
```
research → outline → draft → edit → review → publish
```
### competitive-report (weekly automated)
```
scan-competitors → collect-updates → analyze → draft-report → review → deliver
```
### bug-bounty-audit
```
scan-repo → identify-vulns → prioritize → write-findings → verify → submit
```

## When to Build
- After Mashal approves AI workflow proposal (PM Task 3)
- Start with one workflow, prove value, then expand

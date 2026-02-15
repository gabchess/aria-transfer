# Advanced Vibecoding Setup (from TickernomicsTV, 2026-01-21)

Source: Forwarded by Gabe on 2026-02-02

## Key Tools
- **Factory.ai / Droid Harness** — better than raw Claude Code CLI, handles permissions automatically
- **CliProxy (vibeproxy)** — https://github.com/automazeio/vibeproxy — proxy Claude subscription through CLI
  - Setup: https://github.com/automazeio/vibeproxy/blob/main/FACTORY_SETUP.md
  - **THIS COULD SOLVE OUR CLAUDE CODE AUTH ISSUE** — proxy through Claude Max sub
- **Quotio.dev** — load balance between auth tokens, prevent rate limits
- **Compound Engineering Plugin** — https://github.com/EveryInc/compound-engineering-plugin — self-learning agents
- **WisprFlow** — voice dictation (for humans)
- **Context7 MCP** — decent MCP for context management

## Workflow Principles (confirmed, aligns with our vibecoding methodology)
1. Spec projects fully first (askquestionsspec skill)
2. Sequential GitHub issues from PRD, modular, testable e2e
3. Ralph loops: build → test → PR → merge automatically
4. Codex = long precise work, Opus = exploration
5. Planning > token spam — path dependency means bad basins are hard to escape
6. Find existing GitHub repos that do what you need

## Priority for Us (post-BBQ)
1. **CliProxy** — unlock free Claude Code via Max subscription
2. **Compound engineering** — self-learning for nightly builds
3. **Droid harness** — if we need better CI/CD automation
4. **Quotio** — when we add ChatGPT Pro

## Community
- Telegram: https://t.me/+lbhRRkyuTqg4ZjA8

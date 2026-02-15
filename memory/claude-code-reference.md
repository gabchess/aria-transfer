# Claude Code CLI — Reference Notes

## Two Modes (This Is Key!)
1. **Plan Mode** (`--permission-mode plan`) — READ ONLY. Research, analyze, explore. Cannot write files, run commands, or modify anything. Use for: understanding codebase, planning changes, reviewing code.
2. **Coding Mode** (default) — Full access with --allowedTools. Can read, write, edit, execute bash. Use for: actual implementation after planning.

## Headless Mode (-p flag)
- `claude -p "prompt"` — non-interactive, prints result and exits
- Works with all CLI options
- For programmatic/CI/CD use

## Correct --allowedTools Syntax
- `Bash` = ALL bash commands allowed
- `Bash(git *)` = only commands starting with "git " (space before * matters!)
- `Read,Write,Edit` = file operations
- `Grep` = search
- Example: `--allowedTools "Bash,Read,Write,Edit,Grep"`

## Recommended Workflow (Anthropic Official)
1. **Explore** — Plan Mode: research codebase, understand patterns
2. **Plan** — Plan Mode: create detailed implementation plan
3. **Code** — Coding Mode: execute the plan with simple focused commands
4. **Verify** — Run tests, check output, validate

## For Our Nightly Builds Pattern
Phase 1 (Planning): `claude --permission-mode plan -p "Read the project, understand patterns, create a plan for [TASK]"`
Phase 2 (Coding): `claude -p "Execute: [SIMPLE FOCUSED TASK from plan]" --allowedTools "Bash,Read,Write,Edit,Grep"`

## Key Lessons
- DON'T dump full plans into coding mode — it chokes
- DO use plan mode first, then short focused coding commands
- Keep coding prompts ATOMIC — one feature, one commit
- Use `--allowedTools "Bash,Read,Write,Edit"` for full coding access (not restrictive patterns)
- Context window fills fast — fresh sessions for each task
- CLAUDE.md in project root = persistent context (like our AGENTS.md)

## Output Formats
- `--output-format text` (default) — plain text
- `--output-format json` — structured JSON with metadata
- `--output-format stream-json` — real-time streaming (good for debugging hangs)

## Windows PTY Fix (CRITICAL — Feb 3-4 Debug)
- **Bug:** Claude Code hangs on Windows PowerShell without TTY (known issue #18109)
- **Fix:** ALWAYS use `pty=true` when calling via `exec` tool
- **Command pattern:** `claude -p "prompt" --output-format text`
- **Short prompts work great** — e.g., "Read AGENTS.md. Create src/lib/helius.ts with [spec]."
- **Long multi-file prompts still hang** even with pty — keep it focused
- **Best pattern for OpenClaw:**
  1. Short, focused prompt (one file read, one file write)
  2. `pty=true` in exec call
  3. `--output-format text` for clean output
  4. `yieldMs=90000` for complex tasks (may take time)
- **Nightly builds worked because** the cron used pty=true by default
- **If it hangs:** Kill the session, simplify the prompt, try again

### Feb 4 Findings (IMPORTANT)
- **`--output-format json` DOES NOT WORK** — produces only escape codes, no content
- **`--permission-mode plan` DOES NOT WORK** — same blank output issue
- **What DOES work:** `claude -p "prompt" --output-format text` with `pty=true`, NO plan mode flag
- Claude Code can read multiple files and produce thorough analysis (45 findings across 2 passes)
- Key: the prompt itself can ask for read-only analysis, you don't need `--permission-mode plan`
- Successful pattern: `claude -p "Read [files]. Find all bugs. List with severity and line." --output-format text`
- Takes 60-90 seconds for multi-file analysis, be patient with polling

### Feb 6 Findings (Portfolio Build Session)
- **`--yes` flag DOES NOT EXIST** — use `--dangerously-skip-permissions` instead
- **PowerShell parses parentheses in prompts** — `(Phase 1)` becomes a subexpression. Fix: write prompt to a .md file, then `claude "Read BUILD-PROMPT.md and follow it"`
- **The BUILD-PROMPT.md pattern is THE way** — avoids ALL PowerShell escaping issues
- **Kill zombie sessions BEFORE retrying** — use `process action:list` then kill each one
- **Interactive bypass permissions dialog** can select wrong option via arrow keys — may need multiple attempts
- **WINNING PATTERN:** `exec pty:true workdir:"path" background:true command:'claude --dangerously-skip-permissions "Read BUILD-PROMPT.md and follow it"'`

## Sources
- https://code.claude.com/docs/en/best-practices
- https://code.claude.com/docs/en/common-workflows
- https://code.claude.com/docs/en/headless
- https://claudelog.com/mechanics/plan-mode/

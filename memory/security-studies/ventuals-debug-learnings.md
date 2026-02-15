# Ventuals Audit Debug Session - Learnings (Feb 4, 2026)

## Bug 1: forge build exits code 1 despite successful compilation

**What happened:** `forge build` compiled all 78 files successfully ("Compiling 78 files with Solc 0.8.28") but exited with code 1. We thought the build was failing.

**Root cause:** Foundry v1.3.0+ runs a LINTER during `forge build` by default. The lint warning `erc20-unchecked-transfer` on `vHYPE.transfer(user, vhypeAmount)` in StakingVaultManager.sol caused exit code 1. The compilation itself was fine. The lint warning was treated as an error exit.

**Fix:** Add to `foundry.toml`:
```toml
[lint]
lint_on_build = false
```
Or exclude the specific lint:
```toml
[lint]
exclude_lints = ["erc20-unchecked-transfer"]
```

**Source:** https://getfoundry.sh/config/reference/linter/ and foundry-rs/foundry#12456

**Lesson:** A lint warning is NOT a compilation failure. Read the FULL output, not just the exit code.

## Bug 2: Claude Code blank PTY output on Windows

**What happened:** Running `claude -p "..." --allowedTools "Read,Write,Edit,Bash"` with `pty=true` produced only terminal escape codes `[?25l[2J[m[H]0;claude[?25h]` with zero visible content in the logs.

**Root cause:** Known Claude Code bug on Windows (both PowerShell and Git Bash). The bash output capture mechanism is broken since v2.1.x. Commands run but Claude Code cannot see the output. Issue: anthropics/claude-code#18856

**Workarounds:**
1. Set `$env:CLAUDE_CODE_SHELL="C:\Program Files\Git\bin\bash.exe"` (reported to help in some cases)
2. Use `--output-format json` for structured output that bypasses the rendering issue
3. Use `--output-format stream-json` to see real-time output
4. Run Claude Code in the INTERACTIVE terminal directly (not via background exec)

**Key insight from Gabe:** Sub-agents can't handle complex tasks. Use Claude Code INTERACTIVELY in plan/brainstorm mode. Read output yourself. Synthesize findings. Don't delegate to sub-agents for audit work.

## Bug 3: File locks after killed processes

**What happened:** After killing multiple forge/claude processes, `build-output.txt` remained locked. `Remove-Item -Recurse -Force` failed with "o processo nao pode acessar o arquivo" (process cannot access file).

**Root cause:** Killed Windows processes don't always release file handles immediately. Zombie process handles persist.

**Fix options:**
1. Wait a few seconds after killing processes before deleting
2. Use `cmd /c del /f /q` which sometimes bypasses PowerShell file locking
3. Use `[System.IO.File]::Delete()` for stubborn files
4. Close all terminal sessions first
5. Kill specific PIDs: `Stop-Process -Id <pid> -Force`

## Bug 4: PowerShell icacls syntax error

**What happened:** `icacls path /grant gavaf:(OI)(CI)F /T` failed because PowerShell interpreted `(OI)` as a subexpression/cmdlet.

**Fix:** Use `cmd /c` to run icacls through cmd.exe:
```powershell
cmd /c 'icacls "C:\path" /grant gavaf:(OI)(CI)F /T'
```

## Overall Strategy Failures

1. **Sub-agents are too dumb for complex tasks.** The Opus sub-agent spent its entire budget polling a forge build process instead of reading code. 
2. **Don't try to build AND analyze in one shot.** Separate concerns: (a) get the build working, (b) then analyze code.
3. **When forge build "hangs", check if compilation succeeded but lint failed.** The build was actually completing fine.
4. **Clean up dead processes BEFORE starting new ones.** We had 10+ zombie sessions all competing for file handles.

## Next Session Plan

1. Clone repo fresh
2. Create foundry.toml with `lint_on_build = false` BEFORE first build
3. Run `forge build` and confirm exit code 0
4. Run `forge test` to see if existing tests pass
5. THEN launch Claude Code interactively (not sub-agent) in plan mode for brainstorming
6. Or: I (Aria) read the code directly and analyze, using Claude Code as a brainstorming partner only

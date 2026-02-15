# Vibecoding Best Practices — @gabe_onchain
Source: https://x.com/gabe_onchain/status/2017304477846368285

## Core Principles

### 1. Context is King
- Quality of AI output ∝ context provided
- Create CLAUDE.md / AGENTS.md in project root (< 2.5k tokens)
- Include: goals, tech stack, folder structure, coding standards, constraints

### 2. Define Before You Build (THE RULE)
**NEVER ask Claude to start coding immediately.**

Workflow:
1. **Explore** — Understand the codebase/problem
2. **Plan** — Create detailed implementation plan
3. **Review** — Validate plan in multiple rounds
4. **Approve** — Only THEN enter implementation mode
5. **Verify** — Test and validate each change

### 3. Break Work Into Atomic Tasks
AI works best with small, well-defined tasks with **binary success criteria**.
- ✅ Good: "Add a priority column to tasks table that defaults to 'medium'. Values: low, medium, high."
- ❌ Bad: "Make the task system better and more user-friendly."

### 4. Fresh Context Prevents Hallucinations
- Clear context regularly during long sessions
- Start new sessions for new features
- Use autonomous loops (Ralph) that spawn fresh instances
- Summarize learnings in AGENTS.md between sessions

## The Ralph Wiggum Technique
Named after The Simpsons character. An **autonomous AI coding loop** that runs repeatedly until all tasks complete.

### Key Advantages:
- **Fresh Context Every Iteration** — No accumulated confusion
- **Works While You Sleep** — True autonomous development
- **Cost-Effective** — $30 for ~10 iterations vs $400-600/day developer
- **Deterministic Failures** — When it fails, it fails predictably

### Ralph Best Practices:
1. Write Clear Success Criteria (pass/fail, no ambiguity)
2. Set Appropriate Limits (10-14 iterations)
3. Two Modes:
   - **AFK Ralph:** Run overnight for straightforward features
   - **Hands-on Ralph:** One iteration at a time for complex work

## Technical Best Practices

### State Machine Diagrams (Critical!)
"Ask Claude to make state machine diagrams of existing components. This causes it to map out all paths (which, it will default to being lazy otherwise) but also helps you verify if it's doing things correctly at a systems level." — @mert

### Verification-Led Development
1. Have Claude implement a feature
2. Immediately create automated verification
3. Run verification after every change
4. Use GitHub Actions for PR verification

### Task Decomposition (Ralph Methodology)
1. **Describe** in natural language (talk for 2-3 minutes, ramble, be thorough)
2. **Convert** to formal requirements with clear acceptance criteria
3. **Break** into atomic, testable tasks with binary success criteria

## Architecture & Design

### Plan Mode First — ALWAYS
- Forces upfront thinking
- Reveals edge cases early
- Creates shared understanding
- Prevents scope creep

### AGENTS.md Memory System
Each session should update AGENTS.md with:
- What Worked
- What Failed
- Lessons Learned
- Next Steps

## Common Pitfalls

### The $40K Vibe Coding Bug
AI perfectly solves the **wrong problem** due to vague requirements.
→ Spend 1 hour planning to save 10 hours fixing.

### Context Pollution
Long sessions → AI forgets earlier decisions, contradicts itself.
→ Fresh sessions. Ralph loops. Document decisions.

### Hallucinated Dependencies
AI suggests packages/APIs that don't exist.
→ Verify ALL suggested dependencies. Check npm.

### Infinite Loops
AI stuck fixing same bug repeatedly.
→ Set iteration limits. Switch approaches after 3 attempts.

## The Mindset Shift
- **Old:** You write every line, AI assists with autocomplete
- **New:** You define requirements and verify quality, AI handles implementation
- **You are the product designer/architect, not the typist**

## The 80/20 Rule
- Ralph gets you 90% there
- You spend 1 hour on cleanup
- 5-10x productivity multiplier

## Key Quote
"If you don't know what good code looks like, vibe coding is just a fast way to build a messy app." — @jared-green
"The real insight isn't about AI at all, but about how much of our current coding work compensates for poorly defined problems." — @joshclemm

---

## @poof_eth Agent Coding Tips (Feb 8, 2026)
Source: https://x.com/poof_eth/status/2020541601739579626

1. **Best model is task-dependent** — Codex 5.2/5.3 better for AI/ML/PyTorch. Opus 4.5/4.6 more pragmatic and fast. Figure out what works for YOUR task rapidly.
2. **Dual wield two models** — Shift between models when one gets stuck. e.g. Claude Code in terminal + Cursor with Codex 5.3. Don't sleep on Cursor's harness.
3. **Minimize skills, MCP, rules** — Add slowly if at all. Treat context window like a life bar. Models' native competency is strong. Over-configuring wastes tokens.
4. **Have something to actually build** — Optimizing workflow without a target is unproductive. Breakthrough moments come from obsessing over a problem, not over tooling.
5. **Add measurement to kill noise** — Track what actually helps ship faster vs what's just tinkering.

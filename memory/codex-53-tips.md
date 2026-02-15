# Codex 5.3 Best Practices — 40 Tips
Source: @aiedge_ https://x.com/aiedge_/status/2020891784461894081

## Essentials
- Use GPT-5.3-Codex medium/high for regular tasks; xhigh only for hard problems
- Codex is good for non-coding queries too — web search, quick answers
- Set up skills: Figma MCP, Notion, PDF/Spreadsheet editor
- Install the Skill Installer (#1 skill)
- Use Codex for automations (better than Manus)
- Test automations manually before scheduling
- Use worktrees for parallel tasks to avoid collisions
- Create Actions for standard commands (dev server, tests, lint)
- Start with "Local" so Codex works on your machine

## Prompting
- **Big task? Ask for a plan first, then execute step-by-step**
- Use voice dictate for planning
- Be explicit about the goal + what "done" looks like
- Break into smaller prompts — each step testable and reviewable
- Invoke skills directly: "Use $my-lint-skill"
- Clarify constraints: "don't touch files outside this project"
- **Optimal structure: Goal → Scope → Context → Behavior/Steps → Validation → Constraints**
- Avoid preamble requests — Codex skips them
- Lock the output format: "only a unified diff" or "code with no explanation"
- **Less is more with Codex — keep prompts short (unlike Claude Code which likes detail)**

## Token Efficiency
- Don't repeat instructions — shreds tokens
- **Don't send entire files — only provide the function + relevant interfaces + dependent types**
- Use hard limits: "Max 5 bullets." "Explain in <120 tokens." "Output code only."

## Best Practices
- **Separate thinking from building:** Step 1: approach → Step 2: risks → Step 3: design → Step 4: code
- Use adversarial prompts before shipping: "Try to break this."
- Force structure early — outline before executing complex work
- **Reset context when:** model drifts, responses degrade, task changes direction
- **Stack models:** Use Claude Code THEN shift to Codex (we already do this!)
- Spend 15-20 minutes planning complex projects — Planning > Building

## Git & Setup
- Always review the diff, stage only wanted hunks, revert the rest
- Personalize in Settings → tone/style/instructions
- Add custom Git instructions in Settings → Git
- Create /commands for repetitive tasks: "I'm making /x that does [y]"
- Set up "Run" in top right — install deps + start app

## Our Workflow Application
- Claude Code (Opus 4.6) for autonomous building + deep reasoning
- Cursor (GPT 5.3) for visual exploration + plan mode brainstorms
- Codex for scheduled tasks (cron) + quick iterations
- **Always: Research → Plan → Build → Test (never skip steps)**
- **Before any coding session: re-read this file + BONES.md**

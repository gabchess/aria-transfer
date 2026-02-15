# Google Antigravity Quick Guide
*Compiled Feb 5, 2026*

## What Is It?
- VS Code-based IDE with autonomous AI agents
- Instead of writing code, you describe what you want and agents build it
- Powered by Gemini 3 Pro (can also use Claude Sonnet 4.5)

## Key Concepts

### Two Views
1. **Agent Manager** (Mission Control) - Where you spawn and monitor agents
2. **Editor View** - Traditional code editing (like VS Code)

### Agent Modes
- **Planning Mode** - Agent thinks deeply, creates plans, better quality (use for complex tasks)
- **Fast Mode** - Quick execution, no planning (use for simple tasks)

### Review Policies
- **Agent-Assisted** (RECOMMENDED) - Agent decides when to ask permission
- **Agent-Driven** - Full autopilot, agent does everything
- **Review-Driven** - Agent asks permission for everything

## Basic Workflow

1. **Open Agent Manager** (not Editor)
2. **Select/Create a Workspace** (folder for your project)
3. **Start Conversation** - Click "Start Conversation" button
4. **Write your prompt** - Describe what you want to build
5. **Agent creates a Plan** - Review it before approving
6. **Agent executes** - Writes code, creates files, runs terminal commands
7. **Agent verifies** - Can even open browser to test UI
8. **Review Artifacts** - Check the walkthrough of what was built

## Artifacts (Proof of Work)
Agents produce artifacts to prove what they did:
- **Task Plan** - What the agent plans to do
- **Implementation Plan** - Detailed technical approach
- **Walkthrough** - Summary of completed work with verification

## Pro Tips

1. **Write detailed prompts** - More detail = better results
2. **One task per conversation** - Don't mix unrelated work
3. **Always review the Plan** before letting agent execute
4. **Use Planning mode** for complex tasks (Fast mode for quick fixes)
5. **Check Artifacts** to understand what was actually built
6. **Clean workspace** - Create separate folders for different projects

## Browser Agent
- Antigravity has a built-in browser agent
- Can navigate to websites, click buttons, fill forms
- Great for automated testing of your UI
- Requires Chrome extension (will prompt to install)

## Example Prompts

**UI Generation:**
```
Create a portfolio website with:
- Dark theme
- Hero section with my name and title
- Projects grid showing 4 cards
- Contact form at bottom
Use Next.js and Tailwind CSS.
```

**Full App:**
```
Build a Todo app with:
- Add/delete tasks
- Mark complete
- Local storage persistence
- Clean minimal UI
React + TypeScript.
```

**Visual Mockup First:**
```
I want to build a landing page for a data analytics portfolio.
First, generate 3 different design mockups to look at.
Then we'll pick one and build it.
```

## For Our Portfolio Page

Suggested prompt:
```
Create a data analyst portfolio website with:
- Clean, professional design
- Dark mode
- Hero section: "Gabe | Onchain Data Analyst"
- Skills section: SQL, DuneSQL, Python, Data Visualization
- Projects section showing Dune dashboards with preview images
- Links to X (@gabe_onchain) and GitHub
- Contact section
Use Next.js, Tailwind CSS, deploy-ready for Vercel.
```

## Can Antigravity Work on Existing Projects?
**YES!** Open SolGuard folder in Antigravity and ask agent to:
- Redesign the UI/UX
- Improve visual styling
- Add animations
- Make it more polished

Just open folder → describe what you want improved → agent modifies existing code.

## Portfolio Page Prompt (BACKLOG)
```
Create a data analyst portfolio website with:
- Clean professional dark theme
- Hero: "Gabe | Onchain Data Analyst" 
- Skills: SQL, DuneSQL, Python, Data Viz
- Projects section with cards linking to Dune dashboards
- Links to X (@gabe_onchain) and GitHub
Use Next.js + Tailwind, Vercel-ready.
```

## API Access Options
1. **OpenCode + Antigravity Auth** - CLI access to Gemini 3/Claude via Google OAuth (risky, some bans reported)
2. **MCP Servers** - Config at `~/.gemini/antigravity/mcp_config.json`
3. **Best for now:** Pair-building with Gabe driving

## Resources
- Official docs: https://antigravity.google/docs
- YouTube: https://www.youtube.com/@googleantigravity
- Reddit: r/google_antigravity

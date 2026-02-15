# AgentHub — Key References & Pitch Inspiration

## Pitch Framing Sources

### 1. "Your Company is a Filesystem" — @mernit (2,224 likes)
- https://x.com/mernit/status/2021324284875153544
- Core thesis: filesystem as state, Claude as orchestrator
- Law firm example → our DeFi fund example:
  - `/vaults` = active positions
  - `/audits` = security findings
  - `/trades` = execution history
  - `/agents` = who has permission to do what
- Unix permissions = wallet signing authority
- "By modeling a company like a filesystem, agents can access nearly all the data they need"
- **Use in pitch:** "AgentHub turns any crypto operation into a filesystem that agent teams can work in"

### 2. OpenAI Agent Primitives — @OpenAIDevs (1,947 likes)
- https://x.com/openaidevs/status/2021725246244671606
- 10 tips for multi-hour workflows
- Validates: reasoning effort levels, stateful memory, multi-agent orchestration, built-in tools, tracing/observability
- **Use in pitch:** "OpenAI just validated every primitive we built. We're crypto-native from day one."

### 3. "Claude Code for PMs" — @CodevolutionWeb (384 likes)
- https://x.com/CodevolutionWeb/status/2021627863057793186
- PM at Chime: PRD → prototype in 20 min
- **Use in pitch:** Proof that AI agents building software is mainstream. We're taking it to crypto infrastructure.

### 4. Boris Cherny Customizability Thread — @bcherny (2,426 likes)
- https://x.com/bcherny/status/2021699851499798911
- Claude Code's growth driven by customizability: hooks, plugins, MCPs, skills
- **Use in pitch:** AgentHub = same customizability philosophy but for crypto agent teams

## Features to Implement (from OpenAI primitives)
1. Configurable thinking/effort levels per agent (low/medium/high)
2. Observable execution traces (every action logged with timestamps, inputs, outputs)
3. Guardrail configs per agent (tool allowlists, spending caps, approval thresholds)
4. Crypto-native built-in tools (wallet balance, DEX price feed, contract reader, tx builder)
5. Parallel agent execution with shared state coordination
6. Filesystem-as-state pattern: /agents, /workflows, /traces, /wallets

### 5. Vox RPG Agent Characters — @Voxyz_ai (418 likes)
- https://x.com/voxyz_ai/status/2021370776926990530
- 6-layer role cards: Domain, Inputs/Outputs, Definition of Done, Hard Bans, Escalation, Metrics
- **Hard Bans > Skills** — Don't teach what to do, tell what to NEVER do
- Affinity matrix: agents that clash produce better output (security vs trader = healthy tension)
- RPG stats from real data: VRL, SPD, RCH, TRU, WIS, CRE mapped to game bars
- Evolving personalities from accumulated memories
- Voice directives with micro-bans ("never say 'sounds good' without citing what was good")
- **Use in pitch:** "AgentHub doesn't just run agents. It gives them identity, relationships, and growth curves."
- Cost: $8 VPS + $10 Tripo AI + free Vercel/Supabase + $5-15 LLM API

### 6. Atlas Forge PRINCIPLES.md — @AtlasForgeAI (233 likes)
- https://x.com/atlasforgeai/status/2021773566341988758
- Three-layer agent identity: SOUL (who) → PRINCIPLES (how) → AGENTS (what)
- Validates our exact architecture (SOUL.md + PRINCIPLES.md + AGENTS.md)
- Good principles resolve tensions, bad principles are applause lines
- "Optimize for learning rate, not task completion"

## Viral Validation Signals (Feb 11-12)
- Steven Pu @pusongqi: 1,765 likes on multi-agent threads
- Shubham Saboo: 6 OpenClaw agents via Telegram
- Monaco: $35M from Founders Fund for agent infra
- Florian Darroman: OpenClaw war room setup
- Gartner: 1,445% surge in multi-agent inquiries
- 40% enterprise apps will use multi-agent by 2026 (prediction)

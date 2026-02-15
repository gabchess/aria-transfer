# AgentHub + Aria Empire — Consolidated Backlog
*Updated: Feb 15, 2026*

## The Vision Recap
- **Aria as CEO** running a team of specialized agents
- **Mac Mini M4 (16GB)** as dedicated hardware for each agent
- **Multiple Claude Code instances** running in parallel terminals
- **First bounty money → Mac Mini → real multi-agent setup → demo video → funding**

---

## Agent Team Architecture

### Tier 1: Aria (CEO)
- Coordinates all agents
- Strategic decisions
- Opportunity evaluation
- Reports to Gabe

### Tier 2: Specialist Agents (Mac Mini deployment)

| Agent | Role | Model | Schedule |
|-------|------|-------|----------|
| **Scout** | AI/tech news, hackathons, funding opps | Sonnet | 3x daily |
| **Analyst** | Crypto/DeFi morning brief, Octant intel | Sonnet | Daily 8 AM |
| **Hunter** | Bug bounty scanner, vuln patterns | Opus | Daily |
| **Builder** | Code maintenance, PR reviews, shipping | Opus | On-demand |
| **Closer** | Applications, follow-ups, deadlines | Sonnet | Daily |

### Tier 3: Project Agents (spawn as needed)
- AgentHub Dev Agent
- SolGuard Agent
- Narrative Tracker Agent
- Hackathon-specific agents

---

## Multi-Claude-Code Setup (Future)

### Terminal Layout:
```
┌─────────────────┬─────────────────┐
│ Claude Code #1  │ Claude Code #2  │
│ (Security Audit)│ (PoC Writing)   │
├─────────────────┼─────────────────┤
│ Claude Code #3  │ Claude Code #4  │
│ (Report Format) │ (Scout/Research)│
└─────────────────┴─────────────────┘
         ↓ All report to Aria ↓
```

### Coordination:
- Shared workspace directory
- File-based handoffs (intel/, reports/, projects/)
- Aria monitors all, escalates to Gabe
- Daily standup cron summarizes all agent activity

---

## Mac Mini Specs (Target)
- **Model:** Mac Mini M4 (16GB RAM)
- **Cost:** ~$599-799
- **Power:** 15W idle, 30W load (~$3/month)
- **Access:** Headless via Tailscale + SSH
- **User:** Dedicated "Aria" account, sandboxed
- **Uptime:** 24/7, results ready when Gabe wakes

### Local Model Options (cost-free runs):
- GLM-4.7-Flash via Ollama
- MiniMax M2.5 via Ollama
- Use for routine tasks, save API for complex work

---

## AgentHub Product Features

### MVP (3 months, $500K target):
- [ ] Agent runtime with wallet integration (EVM + Solana)
- [ ] Shared workspace with persistent memory
- [ ] 3 starter templates: Security Auditor, DeFi Monitor, Research Agent
- [ ] Dashboard to observe agent activity in real-time
- [ ] API for programmatic agent deployment
- [ ] Demo: "Watch our agent team audit a smart contract in 10 minutes"

### Differentiators:
1. Wallets + onchain tools built in (agents can sign txns)
2. Crypto-specific templates (auditor, trader, scanner)
3. Shared persistent memory across agent teams
4. Production-first, not demo-first
5. Agent marketplace: deploy, share, monetize workflows

---

## Proof Points (Stack as we go)

### Security Findings:
- [x] Euler v2: 3 findings submitted ($7.5M pool)
- [x] Anchor LazyAccount: Critical bug confirmed + submitted
- [x] Symbiotic: Finding 6 submitted
- [x] OKX DEX-Router: 7 submissions (1 duplicate hit)
- [x] OKX PMM: 2 Medium submitted (Feb 15)
- [ ] OKX TokenDistributor: In progress
- [ ] OKX Solana Router: Claude Code auditing now

### Shipped Products:
- [x] SolGuard (Colosseum Hackathon)
- [x] Octant Card (built in 1 morning with 3 AI agents)
- [x] ERC-8004 Agent ID: 1664 on Base

### Funding:
- [x] Nitro Accelerator: Applied (waiting)
- [ ] Superteam grants
- [ ] Solana Foundation grants

---

## Revenue Math (Path to $200K)

| Stream | Potential | Status |
|--------|-----------|--------|
| Octant salary | $66K/year | Active |
| Bug bounties | $10-100K | Active (OKX in progress) |
| Hackathons | $10-500K | SolGuard submitted |
| AgentHub/Nitro | $500K | Applied |
| Grants | $5-50K | Backlog |

**Conservative (no Nitro):** $116K
**With Nitro:** $616K 🚀

---

## Immediate Backlog

### This Week:
- [ ] Land OKX bounty findings (PMM + TokenDistributor)
- [ ] Claude Code finishes Solana Router audit
- [ ] Check job boards (weekly cron fired today)
- [ ] Wait for Nitro response

### When Mac Mini Arrives:
- [ ] Set up dedicated Aria user account
- [ ] Install Tailscale for remote access
- [ ] Deploy OpenClaw + agent configs
- [ ] Create SOUL.md for each specialist agent
- [ ] Test multi-terminal Claude Code setup
- [ ] Build daily standup cron

### Demo Video (for AgentHub pitch):
- [ ] Record multi-agent security audit in real-time
- [ ] Show coordination between agents
- [ ] Show findings → report → submission pipeline
- [ ] "This is AgentHub. This is the future."

---

## Open Questions
1. Token or no token for AgentHub? ($AGENT for marketplace?)
2. Build on top of OpenClaw or from scratch?
3. Which chain first? (Base? Solana? Multi-chain?)
4. Solo founder or find technical co-founder?
5. How to price enterprise tier?

---

*The grind is the roadmap. First bounty → Mac Mini → Empire.*

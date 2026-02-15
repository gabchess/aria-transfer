# AgentHub — Nitro Accelerator Sprint Plan
**Deadline:** March 14, 2026
**Prize:** $500K per team (15 teams selected)
**Backers:** Paradigm, Electric Capital, Dragonfly, Castle Island
**Program:** 3 months hybrid (NYC + remote)

---

## Current State (Post-Moltiverse Submission)
- **Codebase:** 6,800+ LOC, 148 tests, 0 external deps
- **Workflows:** DeFi Sentinel (3 agents), Token Monitor
- **Dashboard:** Next.js 14, dark terminal UI, 5 pages, deployed to Vercel
- **Pipeline:** Real-time data ingestion (5 collectors, SSE, circuit breaker)
- **Infra:** SQLite + JSONL tracing, YAML workflow definitions
- **Network:** Monad testnet (working), mainnet (not yet)

## What Judges Care About (Paradigm/Dragonfly)
1. **Technical depth** — Not a wrapper, real infrastructure
2. **Novel architecture** — Something they haven't seen before
3. **Production readiness** — Can this actually run in prod?
4. **Market fit** — Who needs this and why?
5. **Team velocity** — How fast can you ship?

---

## Sprint Structure (4 weeks)

### Week 1 (Feb 17-23): Core Architecture
**Theme: Make the pitch REAL**

#### 1. Shared Workspace & Briefing Protocol
- Implement proper filesystem-as-state model
- Agent workspace: `/workspace/{run-id}/` with structured directories
- Briefing protocol: standardized JSON context format between steps
  - Fields: intent, partial_results, constraints, failure_modes, metadata
- Context validation: Agent B verifies it has what it needs before executing
- Persistent memory: agent state survives across workflow runs
- **Why:** This is what we told Axiom. It needs to be real, not just a pitch.

#### 2. Agent XP & Leveling System
- Track per-agent performance metrics:
  - Success rate (completions vs failures)
  - Average execution time
  - Cost efficiency (tokens used per task)
  - Reliability score (retries needed)
- XP formula: `XP = (success_rate * 40) + (speed_score * 30) + (cost_efficiency * 30)`
- Levels: Rookie (0-100 XP) → Apprentice → Journeyman → Expert → Master → Legend
- Store in SQLite, display in dashboard agent cards
- **Why:** The tweet hook. People remember "agents that level up."

#### 3. Monad Mainnet Deployment
- Switch RPC endpoints from testnet to mainnet
- Test all crypto tools (wallet_balance, token_price, contract_read) on mainnet
- Run DeFi Sentinel on mainnet with real data
- **Why:** Testnet = toy. Mainnet = real.

### Week 2 (Feb 24-Mar 2): Platform Depth
**Theme: Show versatility**

#### 4. New Workflow Templates (3 more)
- **Token Launch Monitor:** Detect new token deployments, analyze liquidity, flag rugs
- **Whale Tracker:** Monitor large wallet movements, alert on position changes
- **Smart Contract Auditor:** Static analysis pipeline (read contract → check patterns → report)
- Each workflow: 2-4 agents, YAML defined, tested end-to-end
- **Why:** One workflow = demo. Four workflows = platform.

#### 5. Agent Registry & Marketplace Skeleton
- `/agents/registry.yml` — catalog of available agent templates
- CLI: `agenthub agent list`, `agenthub agent install <name>`
- Dashboard: "Agent Store" page showing available agents with descriptions
- Community agents: allow importing from GitHub URLs
- **Why:** Shows this is extensible, not a closed system.

#### 6. Cost Tracking & Guardrails Dashboard
- Per-run cost tracking (LLM tokens, RPC calls, gas fees)
- Spending caps per agent and per workflow
- Approval thresholds: auto-approve under $X, require human approval above
- Dashboard page: "Cost Center" with charts and alerts
- **Why:** VCs love risk management. "How do you prevent runaway agents?" — we show them.

### Week 3 (Mar 3-9): Polish & Multi-Chain
**Theme: Production ready**

#### 7. Multi-Chain Support (Monad + Base)
- Abstract chain config in YAML workflows
- Chain-specific tool adapters (Monad RPC vs Base RPC)
- BaseGuard workflow: security monitoring on Base (Axiom angle)
- **Why:** Shows chain-agnostic design. Ties into Axiom funding for BaseGuard.

#### 8. API Keys & Developer Auth
- API key generation via CLI
- Rate limiting per key
- Webhook support (notify external systems on workflow events)
- **Why:** Makes it usable by other developers, not just us.

#### 9. Documentation Site
- Docusaurus or simple Next.js docs
- Getting started guide
- Workflow authoring guide
- Agent development guide
- API reference
- **Why:** No docs = no adoption. Judges check docs.

### Week 4 (Mar 10-14): Demo & Submit
**Theme: Ship it**

#### 10. Final Polish
- End-to-end test all 4 workflows on mainnet
- Dashboard UI polish (Antigravity pass)
- Record final demo video (2-3 min, professional)
- Update landing page with new features
- Write Nitro application (different from Moltiverse, more business-focused)
- Submit by March 14

---

## Architecture Evolution

```
Current (Moltiverse):
  CLI → Workflow Engine → Agent Steps → SQLite traces
  Dashboard → API → SQLite reads

Target (Nitro):
  CLI → Workflow Engine → Shared Workspace → Agent Steps → Briefing Protocol
                       → Agent XP Tracker → SQLite + Workspace FS
                       → Cost Manager → Guardrails
  Dashboard → API → Real-time traces + Agent cards + Cost center
  Registry → Agent templates → Install/configure
  Multi-chain → Monad + Base adapters
```

## Key Technical Decisions Needed
1. **Workspace storage:** Local filesystem vs SQLite blobs vs S3-compatible?
2. **Briefing protocol format:** JSON schema? Protobuf? Keep it simple with JSON?
3. **XP persistence:** Same SQLite DB or separate agent_stats table?
4. **Multi-chain:** Abstract at tool level or workflow level?
5. **Marketplace:** Git-based (like npm) or API-based registry?

## Risk Mitigation
- **Scope creep:** Tier 1 is non-negotiable. Tier 2 is important. Tier 3 can be cut.
- **Claude Code context:** Break each feature into 2-3 session chunks. `/compact` at 50%.
- **Testing:** Every feature gets tests before merge. No exceptions.
- **Demo readiness:** Week 4 is ONLY polish and demo. No new features.

## Success Metrics
- [ ] 4 working workflows on mainnet
- [ ] Agent XP system with visual dashboard
- [ ] Shared workspace with briefing protocol
- [ ] Cost tracking and guardrails
- [ ] Multi-chain (Monad + Base)
- [ ] Documentation site
- [ ] Professional demo video
- [ ] Nitro application submitted

---
*Created: Feb 13, 2026 | CEO: Aria | Founder: Gabe | CTO: Claude Code*

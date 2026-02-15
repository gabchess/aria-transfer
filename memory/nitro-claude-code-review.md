# Claude Code's Nitro Architecture Review (Feb 13, 2026)

Full plan saved from Claude Code's Plan Mode analysis.

## Key Takeaways

### CUTS (agreed):
- ❌ Agent Marketplace (empty shelves = negative signal)
- ❌ Multi-chain/Base (soft interest only, dilutes Monad narrative)
- ❌ Auth & Rate Limiting (invisible to judges)
- ❌ Docs Site (README is enough)

### KEEP (revised order):
1. Monad mainnet switch (2-4 hrs) — Week 1
2. "antfarm" → "agenthub" rebrand (2-3 hrs) — CRITICAL, 83 occurrences
3. Cost tracking dashboard (6-8 hrs) — Already 80% built
4. Agent XP system (8-12 hrs) — Highest demo impact
5. One killer workflow: Whale Tracker (4-6 hrs)
6. Briefing protocol (4-6 hrs)
7. On-chain trace commitments (2-3 hrs) — BRILLIANT idea
8. E2E testing + demo video (~20 hrs)

### Total: ~70 hrs across 4 weeks (fits 20-30 hrs/week)

### Novel Insights:
- "antfarm" branding MUST go — judges see "fork" not "original"
- On-chain trace commitments: hash run traces → submit to Monad = "settles on Monad"
- CLAUDE.md is stale/wrong (references Supabase, ethers, Vitest — none used)
- Test count inflated (claims 148, actually ~80-100)
- Demo script FIRST before any code
- The on-chain verification loop is THE moat — make it demo centerpiece
- Agent reputation pitch (no code needed): scouts become trusted oracles

### Quick Wins (< 1 hr each):
1. Live cost counter on run detail (30 min)
2. Step output preview on hover (30 min)
3. Run duration timer (20 min)
4. Pipeline health dot in sidebar (30 min)
5. Colored CLI output (20 min)
6. Real wallet addresses in DeFi Sentinel (30 min)
7. Benchmarks in README (30 min)

### XP System Schema:
- agent_xp table (archetype-based, not per-workflow)
- agent_xp_events audit trail
- Reliability 50% + Speed 30% + Efficiency 20%
- Levels: Rookie → Apprentice → Veteran → Expert → Legend
- Anti-gaming: difficulty tiers, repetition decay, token floor
- XP is OBSERVATIONAL only (doesn't affect behavior for hackathon)

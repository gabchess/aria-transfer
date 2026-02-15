# Nightly Build Queue

## Purpose
Nightly builds are for BUILDING NEW THINGS — features, tools, integrations, automations.
Bug fixing belongs in evening sessions with Gabe. If you find a bug, log it here but don't fix it.

## Tonight's BUILD Task (Feb 5, 5AM)
Pick ONE from the list below:

### Option A: Heartbeat Summary Dashboard
Build a simple HTML dashboard at `workspace/heartbeat-dashboard.html` that reads heartbeat-state.json and displays:
- Last check times for each system
- Cron job statuses (green/yellow/red)
- Quick links to key files
This helps Gabe see system health at a glance.

### Option B: Tweet Performance Tracker
Build a script that checks @AriaLinkwell's recent tweets (via Bird CLI) and logs engagement metrics (likes, RTs, replies) to a CSV or SQLite DB. Over time this tells us which topics perform best.

### Option C: Automated Memory Cleanup
Build a script that reads all memory/*.md files, identifies stale/outdated info, and suggests consolidation. Helps keep memory lean.

## Completed Builds
- 2026-02-04: BF1 fixed (null guard on mint authority) — NOTE: this was a bug fix, should have been a build task

## Logged Bugs (fix in evening sessions)
- BF2: Wire deployer history into scanner (scanner.ts:45)
- BF3: Fix async setTimeout error swallowing (scanner.ts)
- BF4: Fix recursive startScanner interval leak (scanner.ts)
- BF5: Fix deployer score scale inconsistency (risk-engine.ts:50)
- BF6: Align weights with AGENTS.md + add token age factor

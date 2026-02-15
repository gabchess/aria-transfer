# Donor Data Dashboard — Plan
Q's mentorship project for Gabe. Growth path: ETL → Pipeline → API → Dashboard.

## Goal
Cross-ecosystem donor data dashboard for Octant. Analyze donor behavior, patterns, loyalty.

## Tools & Resources to Explore
- **YouTube: "Turn Any Website Into LLM Ready Data INSTANTLY"** — https://youtu.be/4efAzBiTeVo — techniques for scraping websites into structured LLM-ready data. Grab transcript with NoteGPT.
- **Firecrawl** — Web scraping to structured data. Gabe mentioned it Feb 12.
- **Helius Wallet API** — One call for full Solana wallet history, transfers, balances (Mert tweet Feb 12)
- **Dune CLI** — Already installed on Mac (`@duneanalytics/client-sdk` + `mcp-web3-stats`)
- **Supabase** — Already have schema + infra from X Trend Tracker
- **Octant Backend API** — `https://backend.mainnet.octant.app/` (allocations, donors, budgets, history per epoch)
- **Protocol Patron data** — 104 wallets already identified, methodology reusable for other project-specific analysis

## Data Sources
- Octant v1 API (allocations, budgets, leverage per epoch)
- Octant v2 subgraph (regen staker data)
- Dune dashboards (on-chain metrics)
- IPFS project metadata (via `golemfoundation/octant-funding-projects` repo)

## Tech Stack (Q's recommendation)
- pandas (learn)
- ETL scripts (Python)
- Pipelines
- API layer
- Dashboard UI

## Status
BACKLOGGED — Start after Moltiverse (Feb 16) and v2 launch (Feb 18) settle.

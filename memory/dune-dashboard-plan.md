# Dune Dashboard Build Plan — "State of Octant"

## Research Summary (Feb 4, 2026)

### Key Contracts (Ethereum Mainnet)
| Contract | Address | Purpose |
|----------|---------|---------|
| GLM Deposits | `0x879133fd79b7f48ce1c368b0fca9ea168eaf117c` | Lock/Unlock GLM |
| GLM Token | `0x7DD9c5Cba05E151C895FDe1CF355C9A1D5DA6429` | ERC-20 |
| Foundation Staking | `0xba1951dF0C0A52af23857c5ab48B4C43A57E7ed1` | ETH staking rewards |

### Event Signatures (for raw log queries)
- `Locked(uint256,uint256,uint256,address)` → `0x4c00281b0bce21cd7064204e92146d972c359c3743b9a503f438b4325efaaf14`
- `Unlocked(uint256,uint256,uint256,address)` → `0xc866d6212c58a2480a6da14f1bdbaae0c127cdc8296409157a23b968149066ce`

### Event Parameters
- `Locked(oldDeposit, amount, timestamp, depositor)` — all uint256 except address
- `Unlocked(oldDeposit, amount, timestamp, depositor)` — same

### Octant Mechanics
- 100K ETH staked by Golem Foundation → validator rewards
- 90-day epochs, 10 completed (as of Jan 2026)
- Lock GLM → earn proportional ETH rewards
- 70% staking yield → Octant (User Rewards + Matched Rewards)
- 25% → Foundation, 5% → Community Fund
- Quadratic funding for public goods projects
- Min 100 GLM effective balance for rewards
- Non-custodial: GLM stays in contract, user can unlock anytime

### Epoch Data (from blog posts)
| Epoch | Period | ETH Distributed | Key Stats |
|-------|--------|-----------------|-----------|
| 1 | Aug-Nov 2023 | 330.25 ETH | 19 projects, onboarding epoch |
| 2 | Nov 2023-Jan 2024 | ~231.25 ETH to projects | 366 wallets, 24 projects, 58x multiplier |
| 3 | Early 2024 | Algorithm overhaul | New 70/25/5 split introduced |
| 4 | Mid 2024 | 302.36 ETH to projects | 198.4 ETH claimed by users |
| 8 | Apr-Jul 2025 | 460 ETH (~$1.7M) | 170.38 claimed, 289.62 to projects, 30 projects |
| 9 | Late 2025 | "Ethereum Stories" | Collab with EF Strategic Funding |
| 10 | Jan 2026 | Allocation closed Jan 20 | Latest completed epoch |

### Sources Used
- CryptoDataBytes: "Basic Wizard Guide to Dune SQL" — table hierarchy, query patterns
- JamesBachini: Dune Analytics Tutorial — dashboard creation workflow
- BizThon: "Onchain Analytics Using SQL" — DeFi SQL examples, parameterized queries
- Dune Docs: Dashboard creation guide
- Octant Docs: How it works, GLM locking mechanics
- Golem Foundation: Deposits contract source code + events
- Etherscan: Contract transactions, events
- Octant Blog: Epoch results data

---

## Dashboard Design

### Layout (inspired by Lido, Uniswap, 21Shares dashboards)

```
┌──────────────────────────────────────────────────────┐
│                    STATE OF OCTANT                    │
│        An onchain analysis of Octant protocol         │
│            100K ETH staked. Community funded.          │
├───────────┬───────────┬───────────┬──────────────────┤
│ Total GLM │  Unique   │  Total    │  Total Lock/     │
│  Locked   │  Lockers  │  Txns     │  Unlock Events   │
│ (counter) │ (counter) │ (counter) │  (counter)       │
├───────────┴───────────┴───────────┴──────────────────┤
│                                                       │
│  📈 Total GLM Locked Over Time (Cumulative Area)     │
│  [Full width — the hero chart]                       │
│                                                       │
├──────────────────────┬───────────────────────────────┤
│  📊 Monthly Lock vs  │  📈 Unique Active Lockers    │
│  Unlock Volume       │  Per Month (Line)             │
│  (Stacked Bar)       │                               │
├──────────────────────┼───────────────────────────────┤
│  📊 New Lockers Per  │  📊 Lock Size Distribution   │
│  Month (Bar)         │  (Histogram/Bar)              │
├──────────────────────┴───────────────────────────────┤
│                                                       │
│  🏆 Top 20 Lockers by Current Balance (Table)       │
│                                                       │
├──────────────────────────────────────────────────────┤
│  📝 About Octant (Text Widget)                       │
│  Brief overview + link to octant.build               │
└──────────────────────────────────────────────────────┘
```

---

## SQL Queries

### Strategy
1. **Try decoded tables first** — check for `octant_ethereum.*` or `golem_*`
2. **Fallback to raw logs** — use `ethereum.logs` with topic0 hashes
3. **Use `erc20_ethereum.evt_Transfer`** for GLM transfer data
4. **Use `prices.usd`** for GLM price if needed

### Query 1: Total GLM Currently Locked (Counter)
```sql
-- Approach A: Via GLM token balance of Deposits contract
SELECT
    SUM(CASE
        WHEN "to" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c THEN value / 1e18
        WHEN "from" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c THEN -value / 1e18
    END) as total_glm_locked
FROM erc20_ethereum.evt_Transfer
WHERE contract_address = 0x7DD9c5Cba05E151C895FDe1CF355C9A1D5DA6429
AND ("to" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c
     OR "from" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c)
```

### Query 2: Unique Lockers (Counter)
```sql
SELECT COUNT(DISTINCT "from") as unique_lockers
FROM erc20_ethereum.evt_Transfer
WHERE contract_address = 0x7DD9c5Cba05E151C895FDe1CF355C9A1D5DA6429
AND "to" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c
```

### Query 3: Total Lock/Unlock Transactions (Counter)
```sql
SELECT COUNT(*) as total_txns
FROM ethereum.transactions
WHERE "to" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c
AND block_time >= TIMESTAMP '2023-08-01'
AND success = true
```

### Query 4: GLM Locked Over Time — Cumulative (Area Chart)
```sql
-- Hero chart: running total of GLM in the Deposits contract
WITH daily_flows AS (
    SELECT
        DATE_TRUNC('day', evt_block_time) AS day,
        SUM(CASE
            WHEN "to" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c THEN CAST(value AS DOUBLE) / 1e18
            WHEN "from" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c THEN -CAST(value AS DOUBLE) / 1e18
        END) AS net_flow
    FROM erc20_ethereum.evt_Transfer
    WHERE contract_address = 0x7DD9c5Cba05E151C895FDe1CF355C9A1D5DA6429
    AND ("to" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c
         OR "from" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c)
    GROUP BY 1
)
SELECT
    day,
    SUM(net_flow) OVER (ORDER BY day) AS total_glm_locked
FROM daily_flows
ORDER BY day
```

### Query 5: Monthly Lock vs Unlock Volume (Stacked Bar)
```sql
SELECT
    DATE_TRUNC('month', evt_block_time) AS month,
    SUM(CASE
        WHEN "to" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c
        THEN CAST(value AS DOUBLE) / 1e18
        ELSE 0
    END) AS glm_locked,
    SUM(CASE
        WHEN "from" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c
        THEN CAST(value AS DOUBLE) / 1e18
        ELSE 0
    END) AS glm_unlocked
FROM erc20_ethereum.evt_Transfer
WHERE contract_address = 0x7DD9c5Cba05E151C895FDe1CF355C9A1D5DA6429
AND ("to" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c
     OR "from" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c)
GROUP BY 1
ORDER BY 1
```

### Query 6: Unique Active Lockers Per Month (Line)
```sql
SELECT
    DATE_TRUNC('month', evt_block_time) AS month,
    COUNT(DISTINCT "from") AS active_lockers
FROM erc20_ethereum.evt_Transfer
WHERE contract_address = 0x7DD9c5Cba05E151C895FDe1CF355C9A1D5DA6429
AND "to" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c
GROUP BY 1
ORDER BY 1
```

### Query 7: New Lockers Per Month (Bar)
```sql
WITH first_locks AS (
    SELECT
        "from" AS locker,
        MIN(evt_block_time) AS first_lock_time
    FROM erc20_ethereum.evt_Transfer
    WHERE contract_address = 0x7DD9c5Cba05E151C895FDe1CF355C9A1D5DA6429
    AND "to" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c
    GROUP BY 1
)
SELECT
    DATE_TRUNC('month', first_lock_time) AS month,
    COUNT(*) AS new_lockers
FROM first_locks
GROUP BY 1
ORDER BY 1
```

### Query 8: Lock Size Distribution (Bar/Histogram)
```sql
WITH current_balances AS (
    SELECT
        CASE
            WHEN "to" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c THEN "from"
            WHEN "from" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c THEN "to"
        END AS locker,
        SUM(CASE
            WHEN "to" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c THEN CAST(value AS DOUBLE) / 1e18
            WHEN "from" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c THEN -CAST(value AS DOUBLE) / 1e18
        END) AS balance
    FROM erc20_ethereum.evt_Transfer
    WHERE contract_address = 0x7DD9c5Cba05E151C895FDe1CF355C9A1D5DA6429
    AND ("to" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c
         OR "from" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c)
    GROUP BY 1
)
SELECT
    CASE
        WHEN balance < 100 THEN '< 100 GLM'
        WHEN balance >= 100 AND balance < 1000 THEN '100 - 1K'
        WHEN balance >= 1000 AND balance < 10000 THEN '1K - 10K'
        WHEN balance >= 10000 AND balance < 100000 THEN '10K - 100K'
        WHEN balance >= 100000 AND balance < 1000000 THEN '100K - 1M'
        ELSE '1M+'
    END AS size_bucket,
    COUNT(*) AS num_lockers,
    SUM(balance) AS total_glm
FROM current_balances
WHERE balance > 0
GROUP BY 1
ORDER BY MIN(balance)
```

### Query 9: Top 20 Lockers (Table)
```sql
WITH current_balances AS (
    SELECT
        CASE
            WHEN "to" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c THEN "from"
            WHEN "from" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c THEN "to"
        END AS locker,
        SUM(CASE
            WHEN "to" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c THEN CAST(value AS DOUBLE) / 1e18
            WHEN "from" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c THEN -CAST(value AS DOUBLE) / 1e18
        END) AS balance
    FROM erc20_ethereum.evt_Transfer
    WHERE contract_address = 0x7DD9c5Cba05E151C895FDe1CF355C9A1D5DA6429
    AND ("to" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c
         OR "from" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c)
    GROUP BY 1
)
SELECT
    locker,
    ROUND(balance, 2) AS glm_locked,
    ROUND(balance * 100.0 / (SELECT SUM(balance) FROM current_balances WHERE balance > 0), 2) AS pct_of_total
FROM current_balances
WHERE balance > 100
ORDER BY balance DESC
LIMIT 20
```

### Text Widget: About Octant
```markdown
## About Octant

Octant is a community-driven platform for experiments in decentralized governance built by the Golem Foundation.

**How it works:**
- Golem Foundation stakes 100,000 ETH as an Ethereum validator
- 70% of staking rewards go to the Octant community every 90 days (epoch)
- Users lock GLM tokens to earn ETH rewards and govern fund allocation
- Community decides: claim rewards OR donate to public goods projects
- Quadratic funding amplifies small donations

**10 epochs completed** | Launched August 8, 2023

🔗 [octant.build](https://octant.build) | [Deposits Contract](https://etherscan.io/address/0x879133fd79b7f48ce1c368b0fca9ea168eaf117c)

*Dashboard by [@gabe_onchain](https://x.com/gabe_onchain)*
```

---

## Execution Plan

### Phase 1: Create & Test Queries (Browser on Dune)
1. Open dune.com, log in as gabchess
2. Create each query one-by-one
3. Test, fix any syntax issues
4. Note: Dune uses Trino SQL (DuneSQL), not PostgreSQL

### Phase 2: Create Visualizations
1. For each query, create the appropriate chart type
2. Set colors (Octant brand: green + navy blue if possible)
3. Configure axes, labels, tooltips

### Phase 3: Assemble Dashboard
1. Create new dashboard: "State of Octant"
2. Add all visualizations in the layout order above
3. Add text widgets (title + about section)
4. Resize and position for clean layout

### Phase 4: GitHub + Tweets
1. Save all SQL queries to a GitHub repo file
2. Push to gabchess/aria-onchain-analyst or new repo
3. Draft tweets following @gabe_onchain voice guide

---

## Key Dune SQL Tips (from research)
- Always filter on `block_time` or `evt_block_time` for partition pruning
- Use `CAST(value AS DOUBLE)` for token amounts (they're UINT256)
- Join by address, not symbol (uniqueness)
- `prices.usd` for token prices (join on contract_address + minute)
- `tokens.erc20` for token metadata
- `0x...` format for addresses (not `\x...` — that's old Postgres Dune)
- Use CTEs (`WITH ... AS`) for complex multi-step queries
- `DATE_TRUNC('day'|'week'|'month', timestamp)` for time aggregation

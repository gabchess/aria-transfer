# Dune Analytics Mastery Guide
*Compiled Feb 5, 2026 from official docs, pro tutorials, and best practices*

## The 3-Step Process

1. **Write SQL Query** → pull data from Dune tables
2. **Create Visualization** → chart/counter/table from query results
3. **Add to Dashboard** → assemble widgets into layout

---

## Dashboard Layout Patterns (Pro Standard)

```
┌─────────────────────────────────────────────────────────┐
│              DASHBOARD TITLE (Text Widget)              │
├──────────┬──────────┬──────────┬───────────────────────┤
│  Counter │  Counter │  Counter │       Counter         │
│  (Key    │  (Key    │  (Key    │       (Key            │
│  Metric) │  Metric) │  Metric) │       Metric)         │
├──────────┴──────────┴──────────┴───────────────────────┤
│                                                         │
│          HERO CHART (Full Width Area/Line)              │
│          The main story - cumulative or trend           │
│                                                         │
├────────────────────────┬────────────────────────────────┤
│  Supporting Chart 1    │  Supporting Chart 2            │
│  (Bar/Line)            │  (Bar/Line)                    │
├────────────────────────┼────────────────────────────────┤
│  Supporting Chart 3    │  Supporting Chart 4            │
│  (Bar/Line/Pie)        │  (Bar/Line/Pie)                │
├────────────────────────┴────────────────────────────────┤
│                                                         │
│          TABLE (Top N Addresses/Entities)               │
│                                                         │
├─────────────────────────────────────────────────────────┤
│              ABOUT/CONTEXT (Text Widget)                │
│              Brief explainer + links                    │
└─────────────────────────────────────────────────────────┘
```

### Design Principles
- **Counters at top** = instant "at a glance" key metrics
- **Hero chart full-width** = the main story (usually cumulative/time series)
- **Supporting charts 2-column grid** = comparisons, breakdowns
- **Table at bottom** = detailed drill-down (top wallets, etc.)
- **Text widgets** = title + context/explainer

---

## DuneSQL Essentials

### Key Differences from Standard SQL
- Based on **Trino SQL** (not PostgreSQL)
- Addresses use **0x format** (not \x)
- Token amounts are **uint256** → cast to double for math: `CAST(value AS DOUBLE) / 1e18`
- Use **varbinary** for addresses (2x faster queries)

### Critical: Time Filtering (Partition Pruning)
**ALWAYS filter by time column first** - this is how Dune speeds up queries

```sql
-- ✅ GOOD: Direct time filter enables partition pruning
WHERE evt_block_time >= TIMESTAMP '2023-08-01'

-- ✅ ALSO GOOD: Relative time
WHERE block_time >= NOW() - interval '180 day'

-- ❌ BAD: No time filter = full table scan = slow + expensive
WHERE contract_address = 0x...
```

### Common Patterns

**Counter Query (single value):**
```sql
SELECT SUM(value / 1e18) as total_locked
FROM erc20_ethereum.evt_Transfer
WHERE contract_address = 0x7DD9c5Cba05E151C895FDe1CF355C9A1D5DA6429
AND "to" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c
```

**Time Series (area/line chart):**
```sql
SELECT
    DATE_TRUNC('day', evt_block_time) AS day,
    SUM(CAST(value AS DOUBLE) / 1e18) AS daily_volume
FROM erc20_ethereum.evt_Transfer
WHERE evt_block_time >= TIMESTAMP '2023-08-01'
GROUP BY 1
ORDER BY 1
```

**Cumulative Sum (running total):**
```sql
WITH daily_flows AS (
    SELECT DATE_TRUNC('day', evt_block_time) AS day,
           SUM(...) AS net_flow
    FROM ...
    GROUP BY 1
)
SELECT day,
       SUM(net_flow) OVER (ORDER BY day) AS cumulative_total
FROM daily_flows
ORDER BY day
```

**Grouped Comparison (stacked bar):**
```sql
SELECT
    DATE_TRUNC('month', block_time) AS month,
    project,
    SUM(usd_amount) AS volume
FROM dex.trades
WHERE block_time >= NOW() - interval '180 day'
GROUP BY 1, 2
ORDER BY 1
```

**Unique Count (distinct addresses):**
```sql
SELECT COUNT(DISTINCT "from") as unique_users
FROM erc20_ethereum.evt_Transfer
WHERE "to" = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c
```

**Top N with Percentage:**
```sql
WITH balances AS (...)
SELECT 
    locker,
    balance,
    ROUND(balance * 100.0 / (SELECT SUM(balance) FROM balances), 2) as pct
FROM balances
ORDER BY balance DESC
LIMIT 20
```

---

## Visualization Types

### Counter
- **Use for:** Single key metrics (Total TVL, Unique Users, Transaction Count)
- **Config:** Select column, row (usually 1), prefix ($, Ξ), suffix, decimals
- **Format:** Use `0,0` for comma-separated, `0.0a` for abbreviations ($1.2M)

### Area Chart
- **Use for:** Cumulative values over time (TVL, total locked, running totals)
- **Why area:** Shows growth/decline visually as filled space
- **Enable stacking** for multi-series

### Line Chart
- **Use for:** Trends over time (daily active users, price, rates)
- **Why line:** Clear trend visibility, multiple series comparison

### Bar Chart
- **Use for:** Comparisons, rankings, discrete categories
- **Horizontal bars:** Better for top-N rankings (labels readable)
- **Vertical bars:** Better for time buckets (monthly volumes)
- **Enable stacking** for multi-category comparison

### Pie Chart
- **Use for:** Proportion/distribution (share of total)
- **Limit to 5-7 slices** for readability
- **Cannot be mixed** with other chart types

### Table
- **Use for:** Detailed data, addresses, drill-down lists
- **Best for:** Top wallets, transaction lists, detailed metrics

---

## Number Formatting (Tick Format)

| Format | Input | Output | Use Case |
|--------|-------|--------|----------|
| (blank) | 1256784.37 | 1256784.3745000 | Raw precision |
| `0` | 1256784.37 | 1256784 | Integer only |
| `0,0` | 1256784.37 | 1,256,784 | Comma separated |
| `0,0.00` | 1256784.37 | 1,256,784.37 | 2 decimals |
| `0.0a` | 1256784.37 | 1.2M | Abbreviated |
| `$0.0a` | 1256784.37 | $1.2M | USD abbreviated |

---

## Dashboard Creation Workflow

### Step 1: Create Queries
1. Go to dune.com → New Query
2. Write SQL, Run, verify results
3. **Save query** with descriptive name

### Step 2: Create Visualizations
1. Below query results → **New Visualization**
2. Pick chart type (counter, bar, line, area, pie, table)
3. Configure:
   - **Chart options:** Title, legend, stacking
   - **X-axis:** Column, title, sort order
   - **Y-axis:** Column(s), title, enable right axis if needed
   - **Series:** Colors, names, chart type per series
4. **Save visualization**

### Step 3: Assemble Dashboard
1. Go to Dashboards → **New Dashboard**
2. Name it (this becomes the URL slug - can't change later!)
3. Click **Edit** → **Add Widget**
4. Choose from your saved queries + visualizations
5. **Drag to resize** each widget
6. **Drag to reorder** layout
7. Add **Text widgets** for titles and context
8. Click **Save**

### Step 4: Polish
- **Refresh:** Click "Run" to update all charts
- **Schedule:** Set auto-refresh (click clock icon)
- **Share:** Dashboard URL is public by default

---

## Query Optimization Checklist

1. ✅ **Time filter first** - WHERE block_time >= ...
2. ✅ **Select only needed columns** - no SELECT *
3. ✅ **Use decoded tables** - dex.trades > raw logs
4. ✅ **LIMIT during testing** - remove for final
5. ✅ **CTEs for complex logic** - WITH ... AS
6. ✅ **Join on indexed columns** - tx_hash, block_date
7. ✅ **CAST uint256 to double** - for math operations
8. ❌ **Avoid SELECT *** - especially on large tables
9. ❌ **Avoid ORDER BY without LIMIT** - expensive
10. ❌ **Avoid nested subqueries** - use JOINs or window functions

---

## Ethereum/ERC-20 Table Reference

| Table | Use Case |
|-------|----------|
| `erc20_ethereum.evt_Transfer` | Token transfers (decoded) |
| `ethereum.transactions` | Raw transactions |
| `ethereum.logs` | Raw event logs |
| `ethereum.traces` | Internal calls |
| `dex.trades` | DEX swaps (all DEXs) |
| `prices.usd` | Token prices (join by address + minute) |
| `tokens.erc20` | Token metadata (symbol, decimals) |

### Address Format
- DuneSQL: `0x7DD9c5Cba05E151C895FDe1CF355C9A1D5DA6429`
- Old Dune (PostgreSQL): `\x7DD9c5Cba05E151C895FDe1CF355C9A1D5DA6429`

---

## Text Widget Markdown

```markdown
## Title

Regular text with **bold** and *italic*.

- Bullet list
- Another item

[Link text](https://url.com)

![Image alt](https://image-url.com/image.png)
```

---

## Raw Event Log Querying (Undecoded Contracts)

When contracts aren't decoded on Dune, query `ethereum.logs` directly:

```sql
SELECT 
    block_time,
    contract_address,
    tx_hash,
    topic0,  -- event signature
    topic1,  -- first indexed param
    topic2,  -- second indexed param
    data     -- non-indexed params (packed)
FROM ethereum.logs
WHERE block_time >= TIMESTAMP '2025-01-01'
AND topic0 = 0x...  -- event signature hash
AND contract_address = 0x...  -- contract address
```

### Computing Event Signatures
Use keccak256 hash of event signature:
```javascript
// Example: DonationMinted(address indexed dragonRouter, uint256 amount)
ethers.id('DonationMinted(address,uint256)')
// Result: 0xdb5ba5080d718282d7b49e1390aef600bb19f03967fd2022c82d3846c88e2003
```

### Extracting Data from Logs
```sql
-- Extract uint256 from data field
varbinary_to_uint256(data) as amount

-- Extract from specific position (for multiple params)
varbinary_to_uint256(varbinary_substring(data, 1, 32)) as first_param
varbinary_to_uint256(varbinary_substring(data, 33, 32)) as second_param

-- Extract address from data (20 bytes, padded to 32)
varbinary_substring(data, 13, 20) as address_param

-- Remove leading zeros before conversion
varbinary_to_uint256(varbinary_ltrim(data)) as trimmed_amount
```

### Topic Layout
- **topic0**: Event signature hash (always)
- **topic1**: First indexed parameter
- **topic2**: Second indexed parameter
- **topic3**: Third indexed parameter
- **data**: All non-indexed parameters (packed, 32 bytes each)

### Requesting Contract Decoding
Submit contracts via: My Creations > Contracts (or dataset explorer sidebar)
- Check "Are there several instances of this contract?" for factory patterns
- Dune matches by bytecode for dynamic decoding

---

## Pro Tips

1. **Test queries with LIMIT 100** first, then remove
2. **Fork existing queries** to learn patterns (but build your own for portfolio)
3. **Use Dune's AI wand** for query suggestions
4. **Check execution time** - optimize if >30s
5. **Name visualizations clearly** - they appear in widget picker
6. **Dashboard URL slug is permanent** - choose wisely
7. **Refresh schedule** saves manual work
8. **Embed charts** in external sites with iframe
9. **Star popular dashboards** for inspiration
10. **Join Dune Discord** for help

---

## Resources

- Dune Docs: https://docs.dune.com
- Data Catalog: https://docs.dune.com/data-catalog
- Query Engine: https://docs.dune.com/query-engine
- Trino SQL Functions: https://trino.io/docs/current/functions.html
- CryptoDataBytes Guides: https://read.cryptodatabytes.com
- Dashboard Browse: https://dune.com/browse/dashboards

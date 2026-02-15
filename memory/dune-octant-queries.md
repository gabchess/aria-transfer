# Dune Octant Dashboard — SQL Queries

## Contract Info
- **Deposits (Locked GLM):** `0x879133fd79b7f48ce1c368b0fca9ea168eaf117c`
- **Staking Rewards:** `0xba1951dF0C0A52af23857c5ab48B4C43A57E7ed1`

## Event Signatures
- `Locked(uint256 depositBefore, uint256 amount, uint256 when, address user)`
- `Unlocked(uint256 depositBefore, uint256 amount, uint256 when, address user)`

---

## Query 1: Total GLM Locked Over Time (Daily)
```sql
-- Total GLM Locked Over Time
-- Uses Locked and Unlocked events from Octant Deposits contract
WITH locks AS (
    SELECT
        DATE_TRUNC('day', block_time) AS day,
        SUM(CAST(bytearray_to_uint256(bytearray_substring(data, 33, 32)) AS DOUBLE) / 1e18) AS amount_locked
    FROM ethereum.logs
    WHERE contract_address = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c
      AND topic0 = 0x4c00281b0bce21cd7064204e92146d972c359c3743b9a503f438b4325efaaf14 -- Locked event
    GROUP BY 1
),
unlocks AS (
    SELECT
        DATE_TRUNC('day', block_time) AS day,
        SUM(CAST(bytearray_to_uint256(bytearray_substring(data, 33, 32)) AS DOUBLE) / 1e18) AS amount_unlocked
    FROM ethereum.logs
    WHERE contract_address = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c
      AND topic0 = 0xc866d6212c58a2480a6da14f1bdbaae0c127cdc8296409157a23b968149066ce -- Unlocked event
    GROUP BY 1
),
daily_net AS (
    SELECT
        COALESCE(l.day, u.day) AS day,
        COALESCE(l.amount_locked, 0) - COALESCE(u.amount_unlocked, 0) AS net_change
    FROM locks l
    FULL OUTER JOIN unlocks u ON l.day = u.day
)
SELECT
    day,
    net_change,
    SUM(net_change) OVER (ORDER BY day) AS cumulative_locked_glm
FROM daily_net
ORDER BY day
```

## Query 2: Unique Lockers Over Time
```sql
-- Unique GLM Lockers Cumulative
WITH first_locks AS (
    SELECT
        bytearray_substring(data, 109, 20) AS user_address,
        MIN(DATE_TRUNC('day', block_time)) AS first_lock_day
    FROM ethereum.logs
    WHERE contract_address = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c
      AND topic0 = 0x4c00281b0bce21cd7064204e92146d972c359c3743b9a503f438b4325efaaf14
    GROUP BY 1
),
daily_new AS (
    SELECT
        first_lock_day AS day,
        COUNT(*) AS new_lockers
    FROM first_locks
    GROUP BY 1
)
SELECT
    day,
    new_lockers,
    SUM(new_lockers) OVER (ORDER BY day) AS cumulative_lockers
FROM daily_new
ORDER BY day
```

## Query 3: ETH Rewards Distributed (from Staking Rewards contract)
```sql
-- ETH Transfers FROM Staking Rewards address
SELECT
    DATE_TRUNC('month', block_time) AS month,
    COUNT(*) AS num_transfers,
    SUM(CAST(value AS DOUBLE) / 1e18) AS total_eth_distributed
FROM ethereum.transactions
WHERE "from" = 0xba1951dF0C0A52af23857c5ab48B4C43A57E7ed1
  AND value > 0
GROUP BY 1
ORDER BY 1
```

## Query 4: Top GLM Lockers (Whale Watch)
```sql
-- Top 20 GLM Lockers by total locked amount
WITH locks AS (
    SELECT
        bytearray_substring(data, 109, 20) AS user_address,
        CAST(bytearray_to_uint256(bytearray_substring(data, 33, 32)) AS DOUBLE) / 1e18 AS amount
    FROM ethereum.logs
    WHERE contract_address = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c
      AND topic0 = 0x4c00281b0bce21cd7064204e92146d972c359c3743b9a503f438b4325efaaf14
),
unlocks AS (
    SELECT
        bytearray_substring(data, 109, 20) AS user_address,
        CAST(bytearray_to_uint256(bytearray_substring(data, 33, 32)) AS DOUBLE) / 1e18 AS amount
    FROM ethereum.logs
    WHERE contract_address = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c
      AND topic0 = 0xc866d6212c58a2480a6da14f1bdbaae0c127cdc8296409157a23b968149066ce
)
SELECT
    user_address,
    SUM(CASE WHEN src = 'lock' THEN amount ELSE -amount END) AS net_locked_glm
FROM (
    SELECT user_address, amount, 'lock' AS src FROM locks
    UNION ALL
    SELECT user_address, amount, 'unlock' AS src FROM unlocks
) combined
GROUP BY 1
HAVING SUM(CASE WHEN src = 'lock' THEN amount ELSE -amount END) > 0
ORDER BY 2 DESC
LIMIT 20
```

## Query 5: Lock/Unlock Activity (Volume by Week)
```sql
-- Weekly Lock and Unlock volume
SELECT
    DATE_TRUNC('week', block_time) AS week,
    CASE
        WHEN topic0 = 0x4c00281b0bce21cd7064204e92146d972c359c3743b9a503f438b4325efaaf14 THEN 'Lock'
        ELSE 'Unlock'
    END AS action,
    COUNT(*) AS num_events,
    SUM(CAST(bytearray_to_uint256(bytearray_substring(data, 33, 32)) AS DOUBLE) / 1e18) AS total_glm
FROM ethereum.logs
WHERE contract_address = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c
  AND topic0 IN (
    0x4c00281b0bce21cd7064204e92146d972c359c3743b9a503f438b4325efaaf14,
    0xc866d6212c58a2480a6da14f1bdbaae0c127cdc8296409157a23b968149066ce
  )
GROUP BY 1, 2
ORDER BY 1
```

## Query 6: GLM Price Context (if available)
```sql
-- GLM token price from Dune prices table
SELECT
    DATE_TRUNC('day', minute) AS day,
    AVG(price) AS avg_price_usd
FROM prices.usd
WHERE blockchain = 'ethereum'
  AND symbol = 'GLM'
GROUP BY 1
ORDER BY 1
```

---

## COMPUTED TOPIC0 HASHES (keccak256, verified via ethers.js)
- **Locked:** `0x4c00281b0bce21cd7064204e92146d972c359c3743b9a503f438b4325efaaf14`
- **Unlocked:** `0xc866d6212c58a2480a6da14f1bdbaae0c127cdc8296409157a23b968149066ce`

## DATA FIELD LAYOUT (all params non-indexed)
- bytes 1-32 (word 0): depositBefore (uint256)
- bytes 33-64 (word 1): amount (uint256)  
- bytes 65-96 (word 2): when (uint256, timestamp)
- bytes 97-128 (word 3): user (address, left-padded with 12 zero bytes → actual address at bytes 109-128)

## IMPORTANT NOTES
- Replace ALL placeholder topic0 values in queries above with the computed hashes
- For user address extraction: `bytearray_substring(data, 109, 20)` (skips 12-byte padding)
- **First query to run on Dune:** `SELECT * FROM ethereum.logs WHERE contract_address = 0x879133fd79b7f48ce1c368b0fca9ea168eaf117c LIMIT 10` — this validates field layout
- Check if decoded tables exist: search for "octant" or "golem" in Dune's table explorer
- If decoded tables exist (e.g., `golem_ethereum.Deposits_evt_Locked`), use those instead — much simpler

## Dashboard Layout Plan
1. **Header:** "State of Octant: Public Goods Funding on Ethereum"
2. **Row 1:** Total GLM Locked (big number) | Total Unique Lockers | Total ETH Distributed
3. **Row 2:** GLM Locked Over Time (area chart) | Unique Lockers Growth (line chart)
4. **Row 3:** Weekly Lock/Unlock Activity (bar chart) | ETH Rewards by Month (bar chart)
5. **Row 4:** Top 20 Lockers (table) | GLM Price context (line chart)
6. **Footer:** Data from Octant contracts on Ethereum mainnet. Dashboard by @gabe_onchain


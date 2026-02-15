
# Dune Dashboard Rebuild Plan

## New Widget List:

1.  **GLM Locked Over Time (Area Chart):** Total locked GLM per epoch, showing growth. (Existing, needs update)
2.  **Unique GLM Lockers (Line Chart):** Number of unique addresses locking GLM over time. (Existing, needs update)
3.  **ETH Rewards Distributed per Epoch (Bar Chart):**  Total ETH rewards distributed to users each epoch.
4.  **ETH Rewards Allocated to Public Goods per Epoch (Bar Chart):** Total ETH rewards donated to public goods each epoch.
5.  **GLM Price (Line Chart):** GLM price history over time (using CoinGecko API or similar).
6.  **Epoch Summaries (Text Boxes):** Key stats for each epoch (total GLM locked, ETH distributed, donations made).

## SQL Query Outlines:

1.  **GLM Locked Over Time:**
    ```sql
    SELECT
        date_trunc('epoch', block_time) AS epoch,
        SUM(token_a_amount) AS total_glm_locked
    FROM octant.lock_events
    GROUP BY 1
    ORDER BY 1;
    ```
2.  **Unique GLM Lockers**
    ```sql
    SELECT
        date_trunc('epoch', block_time) AS epoch,
        COUNT(DISTINCT trader_a) AS unique_lockers
    FROM octant.lock_events
    GROUP BY 1
    ORDER BY 1;
    ```
3.  **ETH Rewards Distributed:**  This may require joining data from multiple tables or using the Octant API (if available).
    ```sql
    SELECT
        date_trunc('epoch', block_time) AS epoch,
        SUM(eth_reward_amount) AS total_eth_rewards
    FROM octant.reward_events
    GROUP BY 1
    ORDER BY 1;
    ```
4.  **ETH Allocated to Public Goods:**  Similarly, may need joins or the Octant API.
    ```sql
    SELECT
        date_trunc('epoch', block_time) AS epoch,
        SUM(eth_donation_amount) AS total_eth_donations
    FROM octant.donation_events
    GROUP BY 1
    ORDER BY 1;
    ```

5.  **GLM Price:**  Likely requires an external data source and may not be directly queryable in Dune. Consider embedding a chart from TradingView or CoinGecko.

## Layout/Arrangement Suggestions:

*   Header text at the top: "State of Octant"
*   Arrange widgets in a logical flow:
    *   Overall GLM locked (Area Chart, Line Chart)
    *   Rewards distribution (Bar Charts)
    *   GLM Price (Embedded Chart)
    *   Epoch Summaries (Text Boxes at bottom)

## Design Improvements:

*   Use clear and concise titles for each widget.
*   Choose a consistent color scheme.
*   Consider adding tooltips with more detailed information.

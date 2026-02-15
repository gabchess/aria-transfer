# BBQ Data Sources Research — All FREE APIs Confirmed

## 1. DeFiLlama API (FREE, no key needed for core endpoints)
Base URL: `https://api.llama.fi` (free) or `https://pro-api.llama.fi` (paid endpoints marked 🔒)

### Confirmed Working Endpoints:
```
GET /v2/chains                         → Base TVL: $4.21B (confirmed Feb 2 2026)
GET /protocols                         → All protocols, filter by chains.includes('Base')
GET /protocol/{slug}                   → Detailed protocol with historical TVL
GET /tvl/{slug}                        → Simple current TVL number
GET /v2/historicalChainTvl/Base        → Base TVL over time
```

### Stablecoins:
```
GET https://stablecoins.llama.fi/stablecoinchains → Base: $4.61B stablecoins (confirmed)
GET https://stablecoins.llama.fi/stablecoins      → All stablecoins with chain breakdown
```

### Prices (free):
```
GET /coins/prices/current/{coins}      → Current prices (e.g., "base:0x833589...")
GET /coins/prices/historical/{ts}/{coins}
GET /coins/chart/{coins}               → Price chart data
GET /coins/percentage/{coins}          → Price change %
```

### Top Base Protocols (confirmed Feb 2):
1. Morpho V1 — $2.15B (Lending)
2. Steakhouse Financial — $818M (Risk Curators)
3. Aave V3 — $784M (Lending)
4. Binance CEX — $478M
5. Gauntlet — $418M (Risk Curators)
6. Aerodrome Slipstream — $211M (DEX)
7. Uniswap V3 — $194M (DEX)
8. Aerodrome V1 — $125M (DEX)
9. Uniswap V2 — $104M (DEX)
10. Anzen V2 — $101M (RWA)

### Pro-Only (🔒 need API key — $300/mo):
- /api/tokenProtocols/{symbol} — which protocols hold a token
- /api/inflows/{protocol}/{ts} — daily capital flows
- /api/chainAssets — asset breakdown per chain
- /yields/pools — yield farming data

## 2. Base RPC via ethers.js (FREE, no key needed)
RPC URL: `https://mainnet.base.org`

### Confirmed Working:
```javascript
const provider = new ethers.JsonRpcProvider('https://mainnet.base.org');
await provider.getBlockNumber()           // Block: 41,628,176 (confirmed)
await provider.getFeeData()               // Gas: 0.015 gwei (confirmed)
await provider.getBlock('latest')         // 373 txs per block (confirmed)
await provider.getBalance(address)        // ETH balance
await provider.getTransactionCount(addr)  // Nonce/tx count
```

### What We Can Monitor:
- Block production rate (should be ~2s on Base)
- Gas price trends (extremely low on Base)
- Transaction volume per block
- Large ETH transfers (filter by value)
- Contract deployments (to=null transactions)
- Specific contract events (via getLogs)

## 3. Etherscan/Basescan API V2 (FREE tier)
Base URL: `https://api.etherscan.io/v2/api?chainid=8453`
**One API key for all 60+ chains!**

### Key Endpoints:
```
?module=account&action=txlist&address={addr}           → Transaction list
?module=account&action=tokentx&address={addr}          → ERC-20 transfers
?module=account&action=balance&address={addr}           → ETH balance
?module=contract&action=getcontractcreation&...         → Contract deployer
?module=stats&action=ethsupply                          → Total ETH supply
?module=proxy&action=eth_blockNumber                    → Latest block
```

### Useful for Whale Tracking:
```
?module=account&action=tokentx&contractaddress=0x833589...  → USDC transfers
&startblock=0&endblock=99999999&sort=desc
```

### Rate Limits (free tier):
- 5 calls/second
- Need API key (free registration at basescan.org)

## 4. Additional Free Data Sources

### CoinGecko (free tier):
```
GET https://api.coingecko.com/api/v3/simple/price?ids=ethereum&vs_currencies=usd
```
- 10-30 calls/min on free tier
- ETH price for $ context in analysis

### L2Beat (no official API, but public data):
- `https://l2beat.com/api/tvl/base.json` (may work)
- TVL comparison across L2s

### Dune API (Gabe has account):
- Run custom SQL queries on decoded data
- Powerful but rate-limited on free plan

## Summary: What We'll Use

| Source | What For | Cost | Rate Limit |
|--------|----------|------|------------|
| DeFiLlama | TVL, protocols, stablecoins, prices | Free | Generous |
| Base RPC | Blocks, gas, txs, balances, events | Free | ~100 req/s |
| Basescan API | Token transfers, whale tracking | Free | 5/sec |
| CoinGecko | ETH/BTC price context | Free | 10-30/min |

**Total API cost: $0/month**

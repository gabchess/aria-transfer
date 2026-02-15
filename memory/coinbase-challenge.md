# Coinbase #CoinbaseChallenge Research

## The Challenge
- **Source:** https://x.com/coinbase/status/2019457703937036790
- **Task:** Model Coinbase FY 2025 total revenue by Feb 12
- **Prize:** Travel + exclusive access to next major Coinbase event
- **Hashtag:** #CoinbaseChallenge
- **Deadline:** Feb 12, 2026 (same day Q4 earnings release!)

## Known Revenue Data (Q1-Q3)

| Quarter | Total Revenue | Transaction Rev | Sub & Services | Notes |
|---------|--------------|-----------------|----------------|-------|
| Q1 2025 | $2.03B | ~$1.26B | ~$698M | Strong crypto market |
| Q2 2025 | $1.45-1.5B | $764M ($650M consumer + $61M institutional) | $656M | Down 27% Q/Q, weak trading |
| Q3 2025 | $1.9B | ~$979M ($844M consumer + $135M institutional) | $747M | Deribit acquisition ($52M contrib), beat estimates |

**Q1-Q3 Total: ~$5.38-5.43B**

## Q4 2025 Guidance & Clues
- **October transaction revenue:** ~$385M (management disclosed)
- **Q4 subscription/services guidance:** $710-790M
- **Q4 tech+G&A expense guidance:** $925-975M
- **Q4 sales & marketing guidance:** $215-315M
- **Analyst consensus Q4 revenue:** ~$1.85-1.87B
- **Deribit:** First full quarter contribution (closed Aug 14, contributed $52M in partial Q3)
- **Bitcoin:** Hit ATH in Q4, strong retail participation
- **Crypto total market:** Year-end rally, favorable conditions

## Revenue Model

### Conservative Case: $7.23B
- Q4 estimate: $1.85B (analyst consensus floor)
- $2.03 + $1.45 + $1.9 + $1.85

### Base Case: $7.33B  
- Q4 estimate: $1.9B (matching Q3 performance)
- $2.03 + $1.5 + $1.9 + $1.9

### Bull Case: $7.43B
- Q4 estimate: $2.0B (strong Q4 crypto rally + full Deribit)
- $2.03 + $1.5 + $1.9 + $2.0

## Our Unique Angle
- **Onchain data via Dune** supporting the model
- Base L2 sequencer fees (proxy for platform activity)
- USDC on-chain volume and market cap trends
- Trading volume proxies from DEX data
- Nobody else will combine TradFi modeling with crypto-native analysis
- **Dune dashboard:** "Coinbase FY 2025 Revenue Model"

## Key Sources
- Q1 Shareholder Letter: s27.q4cdn.com/397450999/files/doc_financials/2025/q1/
- Q2 Shareholder Letter: s27.q4cdn.com/397450999/files/doc_financials/2025/q2/
- Q3 Earnings Call Transcript: fool.com/earnings/call-transcripts/2025/11/27/coinbase-coin-q3-2025-earnings-call-transcript/
- Q3 CNBC coverage: cnbc.com/2025/10/30/coinbase-shares-rise-on-third-quarter-earnings-beat.html
- Coinbase IR: investor.coinbase.com/financials/quarterly-results/
- Q4 2025 earnings expected: Feb 12 after market close

## Key Financial Details (for modeling)
- Coinbase joined S&P 500 in May 2025 (first crypto-native company)
- Deribit acquisition: $2.9B, closed Aug 14, 2025. #1 crypto options venue (75% market share)
- Q3 derivatives volume (Coinbase + Deribit): $840B+
- Spot tradable assets expanded from 300 to 40,000+ (DEX integration)
- USDC avg on platform: $15B (ATH market cap $74B)
- Assets on platform: $516B at Q3 end
- Full-time employees: 4,795 at Q3 end (12% sequential growth)
- XRP/USD overtook BTC as largest market on Coinbase (25.16% of volume)
- 65% of customer support automated, planning LLM agents for compliance 2026
- Base L2 sequencer fees = revenue driver (mentioned in earnings call)

## Tools
- **Nano Banana Pro** = Gemini image generation skill for OpenClaw. Can generate charts/infographics.
- Gabe has Gemini Pro subscription = full access
- Install: `nano-banana-pro` skill in OpenClaw config (needs GEMINI_API_KEY)
- Viktor Oddy tweet: https://x.com/viktoroddy/status/2019727896500400457 (Opus 4.6 + Nano Banana + Google Flow combo)
- Could use for generating polished chart visuals for the tweet

## Plan — COMPLETED ✅
1. ✅ Build financial model document with clear rationale → `coinbase-challenge/FINANCIAL-MODEL.md`
2. ✅ Create Dune dashboard → https://dune.com/gabe_onchain/coinbase-fy-2025-revenue-model
   - Base L2 monthly net revenue (sequencer fees - L1 costs)
   - USDC supply on Ethereum (weekly, shows growth through crash)
   - Weekly DEX trading volume (shows Q4 cliff)
   - Quarterly revenue model (Q1-Q3 actual + Q4 estimate)
3. Skipped Nano Banana Pro (dashboard charts sufficient)
4. ✅ Tweet drafted following voice-gabe.md style guide
5. ✅ Posted from @gabe_onchain: https://x.com/gabe_onchain/status/2019878913208672452

## REVISED MODEL (Bear Market Adjusted)
Original estimates were $7.23-7.43B before accounting for the Q4 crash.
**Final estimate: $6.88B** after incorporating:
- BTC crash from $126K ATH to $85K by Dec
- Base L2 revenue drop: 2,474 ETH (Oct) → 930 ETH (Dec), -62%
- DEX volume collapse after October spike
- USDC supply resilience ($44B → $65B+)
- Binance Dec spot volume -40% MoM
- CoinGecko: Total crypto market cap -23.7% in Q4

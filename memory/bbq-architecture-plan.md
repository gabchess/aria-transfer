# BBQ Architecture Plan — Aria Onchain Analyst

## 🎯 Core Concept
Aria is the ONLY autonomous data analyst agent in the competition.
She monitors Base ecosystem data, identifies trends/anomalies, tweets analysis,
and records every finding onchain with a verifiable audit trail.

## 📁 GitHub Repo: `aria-onchain-analyst`
```
aria-onchain-analyst/
├── README.md                    # Project overview + architecture
├── package.json
├── contracts/
│   └── AnalyticsRegistry.sol    # Findings registry contract
├── scripts/
│   ├── deploy.js                # Deploy contract to Base
│   └── verify.js                # Verify on Basescan
├── src/
│   ├── index.js                 # Main entry point (autonomous loop)
│   ├── config.js                # RPC, API keys, wallet config
│   ├── monitor/
│   │   ├── base-stats.js        # Base chain stats (blocks, gas, txs)
│   │   ├── defi-tvl.js          # DeFiLlama Base TVL + protocols
│   │   ├── whale-tracker.js     # Large transaction monitoring
│   │   ├── bridge-flows.js      # L1↔Base bridge activity
│   │   └── token-trends.js      # Top token movements on Base
│   ├── analyze/
│   │   ├── trend-detector.js    # Identify patterns in collected data
│   │   ├── anomaly-finder.js    # Spot unusual activity
│   │   └── insight-generator.js # LLM-powered analysis synthesis
│   ├── publish/
│   │   ├── tweet-composer.js    # Write tweet in our style
│   │   ├── bird-poster.js       # Post via Bird CLI
│   │   └── onchain-recorder.js  # Record finding to AnalyticsRegistry
│   └── utils/
│       ├── wallet.js            # Wallet management
│       ├── base-provider.js     # ethers.js Base provider
│       └── logger.js            # Structured logging
├── data/
│   └── findings.json            # Local cache of all findings
├── docs/
│   ├── ARCHITECTURE.md          # Detailed system design
│   ├── BUILDING.md              # Build process documentation
│   └── JUDGING.md               # How we meet judging criteria
└── video/                       # Remotion project (later)
    └── ...
```

## 🔄 Autonomous Loop (every 4-6 hours)

```
┌─────────────┐    ┌──────────────┐    ┌───────────────┐
│   MONITOR   │───▶│   ANALYZE    │───▶│   PUBLISH     │
│             │    │              │    │               │
│ • Base stats│    │ • Detect     │    │ • Compose     │
│ • DeFi TVL  │    │   trends     │    │   tweet       │
│ • Whales    │    │ • Find       │    │ • Post to X   │
│ • Bridges   │    │   anomalies  │    │ • Record      │
│ • Tokens    │    │ • Generate   │    │   onchain     │
│             │    │   insights   │    │               │
└─────────────┘    └──────────────┘    └───────────────┘
```

## 📊 Data Sources (all FREE APIs)

### 1. DeFiLlama API (no key needed)
- `GET https://api.llama.fi/v2/chains` — All chains TVL
- `GET https://api.llama.fi/protocol/{name}` — Protocol details
- `GET https://api.llama.fi/v2/historicalChainTvl/Base` — Base TVL history
- `GET https://api.llama.fi/protocols` — All protocols (filter by chain: Base)
- `GET https://stablecoins.llama.fi/stablecoins?includePrices=true` — Stablecoin flows

### 2. Base RPC (ethers.js)
- Block number, gas price, tx count
- Contract events (large transfers, new deployments)
- Account balances

### 3. Basescan API (free tier: 5 calls/sec)
- Token transfers
- Contract verification status
- Internal transactions
- Top accounts

### 4. L2Beat API (if available)
- TVL comparison across L2s
- Base vs Optimism vs Arbitrum

### 5. CoinGecko (free tier)
- ETH price for context
- Base token prices

## 🧠 Analysis Engine

The LLM (Gemini Flash via OpenRouter) receives:
1. Current Base ecosystem snapshot (TVL, gas, txs, whale activity)
2. Previous snapshot (for comparison / trend detection)
3. Prompt: "You are a professional onchain data analyst. Analyze this data and find the most interesting insight. Focus on: trends, anomalies, whale behavior, TVL shifts, bridge flows."

Output:
- `category`: "tvl" | "whale" | "trend" | "anomaly" | "bridge" | "protocol"
- `summary`: One-line finding
- `fullAnalysis`: 2-3 sentence deep take
- `tweetDraft`: Ready-to-post tweet (our conversational style)
- `confidence`: 1-10 (only publish if >= 7)

## 🐦 Tweet Style
Same rules as @gabe_onchain copywriting guide:
- Conversational, lowercase ok, fragments ok
- Natural transitions ("so basically...", "ok this is interesting...")
- Data-driven: always include a number or metric
- No AI slop, no "🚀 BREAKING 🚀", no generic questions
- End with insight, not hype

Example tweets:
- "base just hit $2.4B TVL for the first time. but here's what's interesting — 60% of that growth came from a single protocol this week. aerodrome is eating everything."
- "spotted a wallet moving $800K USDC from mainnet to base in the last 6 hours. third time this week. someone's positioning for something."
- "base gas just dropped to 0.001 gwei while ethereum is at 15. the cost difference is 15,000x right now. that's the whole thesis in one number."

## 📝 Smart Contract: AnalyticsRegistry

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

contract AnalyticsRegistry {
    struct Finding {
        uint256 timestamp;
        string category;     // "tvl", "whale", "trend", "anomaly", "bridge", "protocol"
        string summary;      // Brief human-readable summary (< 280 chars)
        bytes32 contentHash; // keccak256 of full analysis text
        string tweetUrl;     // Link to the X post
    }
    
    address public immutable analyst;
    Finding[] public findings;
    
    event NewFinding(uint256 indexed id, string category, string summary, bytes32 contentHash);
    event AnalystActive(uint256 timestamp, string message);
    
    modifier onlyAnalyst() {
        require(msg.sender == analyst, "not analyst");
        _;
    }
    
    constructor() {
        analyst = msg.sender;
        emit AnalystActive(block.timestamp, "Aria Onchain Analyst initialized");
    }
    
    function recordFinding(
        string calldata category,
        string calldata summary,
        bytes32 contentHash,
        string calldata tweetUrl
    ) external onlyAnalyst returns (uint256) {
        uint256 id = findings.length;
        findings.push(Finding({
            timestamp: block.timestamp,
            category: category,
            summary: summary,
            contentHash: contentHash,
            tweetUrl: tweetUrl
        }));
        emit NewFinding(id, category, summary, contentHash);
        return id;
    }
    
    function totalFindings() external view returns (uint256) {
        return findings.length;
    }
    
    function getLatestFindings(uint256 count) external view returns (Finding[] memory) {
        uint256 total = findings.length;
        if (count > total) count = total;
        Finding[] memory latest = new Finding[](count);
        for (uint256 i = 0; i < count; i++) {
            latest[i] = findings[total - count + i];
        }
        return latest;
    }
    
    function getFindingsByCategory(string calldata category) external view returns (uint256[] memory) {
        uint256[] memory temp = new uint256[](findings.length);
        uint256 count = 0;
        for (uint256 i = 0; i < findings.length; i++) {
            if (keccak256(bytes(findings[i].category)) == keccak256(bytes(category))) {
                temp[count] = i;
                count++;
            }
        }
        uint256[] memory result = new uint256[](count);
        for (uint256 i = 0; i < count; i++) {
            result[i] = temp[i];
        }
        return result;
    }
}
```

## 🏆 Judging Criteria Checklist

### 1. Implementation of onchain primitives ✅
- [x] Smart contract deployed on Base (AnalyticsRegistry)
- [x] Every analysis recorded onchain with content hash
- [x] Events emitted for each finding (indexable)
- [ ] ERC-8004 Trustless Agent registration (stretch goal)
- [ ] EAS attestations for findings (stretch goal)

### 2. Novelty of use cases ✅
- [x] ONLY data analyst agent (unique in field of 100+ entries)
- [x] Real analytical value (not tokens/memes)
- [x] Onchain audit trail of analysis (nobody else doing this)
- [x] Verifiable content hashes

### 3. Documentation ✅
- [ ] README with architecture diagram
- [ ] ARCHITECTURE.md with system design
- [ ] BUILDING.md with build process
- [ ] Remotion + HeyGen video
- [ ] Build thread on X (@AriaLinkwell)

### 4. No human in the loop ✅
- [x] Autonomous cron-based monitoring
- [x] LLM analysis without human review
- [x] Bird CLI posting without approval
- [x] Onchain recording without human signing

### 5. Live on X ✅
- [x] @AriaLinkwell account created
- [x] Bird CLI configured and working
- [ ] Autonomous posting active

### 6. Transacting on Base ✅
- [x] Wallet funded (0.01 ETH on Base)
- [ ] Contract deployed
- [ ] Recording findings (ongoing transactions)

## 📅 Build Timeline

### Afternoon Build 1 (Feb 2, TODAY):
1. Create GitHub repo `aria-onchain-analyst`
2. Set up project structure (package.json, dirs)
3. Write README + ARCHITECTURE.md
4. Deploy AnalyticsRegistry contract on Base
5. Test contract interaction

### Afternoon Build 2 (Feb 3):
1. Build monitor modules (DeFiLlama + Base RPC)
2. Build analysis engine (Gemini Flash integration)
3. First manual test: fetch data → analyze → output

### Afternoon Build 3 (Feb 4):
1. Build tweet composer + Bird CLI integration
2. Build onchain recorder module
3. Wire autonomous loop
4. First fully autonomous cycle

### Afternoon Build 4 (Feb 5):
1. Create cron job for 4-6 hour autonomous cycles
2. Let agent run unsupervised
3. Build thread on X documenting the process
4. Bug fixes, edge cases

### Afternoon Build 5 (Feb 6-7):
1. Remotion video project
2. HeyGen avatar integration
3. Polish documentation
4. Let agent accumulate 24-48h of findings

### Submission (Feb 8):
1. Final check: all systems running
2. Submit @AriaLinkwell link in BBQ replies
3. Documentation complete

## 🎬 Video Documentation (Remotion + HeyGen)
See detailed research: `memory/bbq-video-research.md`

**Approach:** Option A — HeyGen for Aria avatar segments, Remotion for data viz + architecture
- HeyGen: Free tier (10 API credits/mo = 10 min video). Photo avatar from our avatar image.
- Remotion: Free (open source). React components for animated data, architecture diagrams.
- Script scenes: Intro (Aria speaks) → Architecture (animated) → Live Demo (real data) → Onchain Recording → Results → Outro
- Remotion project lives in `aria-onchain-analyst/video/`

## 💡 Stretch Goals (if time permits)
- [ ] ERC-8004 Trustless Agent registration on Ethereum mainnet
- [ ] EAS attestations on Base for each finding
- [ ] Web dashboard showing all findings (OnchainKit)
- [ ] Farcaster cross-posting (Neynar skill)
- [ ] Alert system for high-confidence anomalies

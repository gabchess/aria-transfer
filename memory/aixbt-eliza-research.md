# aixbt & Eliza Framework Research — Feb 3, 2026

## How aixbt Works
- **Posting cadence:** ~10 min past every hour UTC, either single tweet or short thread
- **Replies:** 2,000+ per day, fully autonomous (99% probability no human involvement)
- **Monitoring:** 400+ KOLs, synthesizes social narratives rather than deep analysis
- **Tech:** LLM with context window containing (1) project/market directory and (2) real-time CT social data
- **Platform:** Built on Virtuals Protocol (Base chain), has its own token
- **Personality:** Consistent, charming, confident — key to its success. Never off-putting.
- **Accuracy:** Only ~31% on predictions, but doesn't matter for engagement
- **Credibility:** Cautious about small-cap tokens — builds trust over time
- **Follower count:** 300,000+ in <3 months, each post gets 50,000+ clicks
- **Posting method:** Uses Twitter's API (likely paid tier given volume)
- **Key insight:** aixbt primarily synthesizes social media narratives, not deep original analysis. The context window (what data developers feed it) is the real differentiator.

## Eliza Framework (elizaOS)
- **Repo:** https://github.com/elizaOS/eliza (was ai16z/eliza)
- **What:** Open-source multi-agent AI framework. Discord, Telegram, Farcaster, Twitter connectors.
- **Stack:** Node.js + bun, model-agnostic (OpenAI, Gemini, Anthropic, Llama, Grok)
- **NOTE:** Requires WSL 2 on Windows, Node.js v23+, bun

### Eliza's TWO Twitter Approaches

**A) agent-twitter-client (cookie-based, NO API key)**
- npm: `agent-twitter-client` (v0.0.18, 39 dependents)
- Modified from `@the-convocation/twitter-scraper`
- Login with username/password → gets session cookies → posts via Twitter's internal API
- Works in browser AND server
- Forks: `@0xindie/agent-twitter-client`, `@rekttdoteth/agent-twitter-client`
- **⚠️ VULNERABLE to error 226** — same problem as Bird CLI

**B) plugin-twitter (official API v2)** ← RECOMMENDED
- Repo: `elizaos-plugins/plugin-twitter`
- Uses OAuth 1.0a or OAuth 2.0 PKCE
- Requires Twitter Developer Account
- Features: autonomous posting, timeline monitoring, mention/reply handling, DMs, discovery service, search
- Built-in: rate limiting, retry, caching, randomized intervals
- Anti-detection: `TWITTER_POST_INTERVAL_MIN/MAX` for human-like timing
- Discovery service: auto-follow relevant accounts, engage with content

## Twitter/X API v2 Pricing (2025)

| Tier | Price | Posts/month | Reads/month | Notes |
|------|-------|-------------|-------------|-------|
| Free | $0 | 500 | 100 | Media allowed, no likes/follows |
| Basic | $200/mo | 50,000 | 15,000 | 2 App IDs |
| Pro | $5,000/mo | 300,000 | 1,000,000 | 3 App IDs |

**Free tier is enough for us:** 12 tweets/day × 30 = 360/month (under 500 limit)

### How to Post with Free Tier
```javascript
// OAuth 1.0a + API v2 endpoint
const response = await axios.post(
  'https://api.twitter.com/2/tweets',
  { text: content },
  {
    headers: {
      ...oauthHeader, // HMAC-SHA1 signature
      'Content-Type': 'application/json'
    }
  }
);
```
Dependencies: `oauth-1.0a`, `axios`, `crypto`

## Error 226 Deep Dive
- **What:** Twitter's anti-automation system (launched late 2024-2025)
- **ALL cookie-based tools vulnerable:** Bird CLI, twikit, agent-twitter-client, Selenium, Puppeteer
- **Detection moved server-side:** API gateway + behavioral engine profiles account + IP patterns
- **Triggers:** datacenter IPs, repetitive timing, invalid cookies, fast proxy switching, high-frequency logins
- **Traditional bypasses DEAD:** proxies, header spoofing, random delays — none work anymore
- **Residential IP helps but insufficient** — behavioral profiling catches inconsistencies
- **Cookie entropy, signature tokens, device history** all tracked

## Other Tools Found
- **twikit** (Python, github.com/d60/twikit) — Twitter internal API scraper, cookie-based. Also 226-vulnerable.
- **TwitterAPI.io** — third-party proxy service, $0.15/1000 tweets, claims no 226. Handles auth/sessions.
- **agent-twitter-client-grokky** — fork that adds Grok chat access via Twitter's internal API

## Recommendations for @AriaLinkwell Pipeline

### 🥇 BEST: Official X API v2 Free Tier
- **500 posts/month = plenty** for our ~12/day cadence
- Most reliable, no 226 risk, supports media upload
- Needs: Twitter Developer Account for @AriaLinkwell
- OAuth 1.0a implementation is simple (oauth-1.0a + axios)
- **Action:** Apply for developer account → get API keys → replace Bird CLI

### 🥈 FALLBACK: Browser-based posting
- We already have OpenClaw browser tool
- Post by automating the actual Twitter compose/send flow
- Most human-like (real browser, real cookies, real fingerprint)
- Heavy, slow, fragile — but works when API isn't available

### 🥉 AVOID: Cookie-based CLI tools
- Bird CLI, agent-twitter-client, twikit — all vulnerable to 226
- Will only get worse as Twitter tightens anti-automation
- Not sustainable for long-term autonomous posting

### Lessons from aixbt/Eliza
1. **Randomized timing** — never post on exact schedules. Use MIN/MAX intervals.
2. **Context window = differentiator** — what data you feed the LLM matters more than the LLM itself
3. **Personality consistency** — every tweet should feel like the same "person"
4. **Cautious credibility** — avoid promoting risky things, builds trust over time
5. **Discovery service** — auto-engage with relevant content, not just post
6. **Human-like patterns** — Eliza's plugin uses 90-150 min random intervals between posts

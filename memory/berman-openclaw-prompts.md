# Matthew Berman OpenClaw Prompts Reference

**Source:** https://gist.github.com/mberman84/065631c62d6d8f30ecb14748c00fc6d9
**Video:** https://x.com/MatthewBerman/status/2021669868366598632
**Saved:** 2026-02-14

## Use Cases Overview

| # | Name | Our Verdict |
|---|------|-------------|
| 1 | Personal CRM | SKIP (not needed) |
| 2 | Knowledge Base (RAG) | ✅ ADD |
| 3 | Content Idea Pipeline | SKIP (creator-focused) |
| 4 | Social Media Research | ✅ ADD |
| 5 | YouTube Analytics | SKIP (not our model) |
| 6 | Nightly Business Briefing | SKIP (overkill) |
| 7 | CRM/HubSpot Integration | SKIP (enterprise) |
| 8 | Content Humanizer | ✅ ADD |

---

## Knowledge Base (RAG) — Key Specs

**Chunking:**
- Chunk size: 800 characters
- Overlap: 200 characters
- Minimum chunk: 100 chars (append tiny remainders to last chunk)
- Split on sentence boundaries: `/(?<=[.!?])\s+/`

**Embeddings:**
- Primary: Google gemini-embedding-001 (768 dims, FREE)
- Fallback: OpenAI text-embedding-3-small (1536 dims)
- Max input: 8000 chars per chunk
- Batch: 10 chunks with 200ms delay between batches
- Retry: 3 times with exponential backoff (1s, 2s, 4s)
- Cache: LRU (1000 entries)

**Deduplication:**
- URL normalization: strip utm_*, fbclid, igshid, ref, s, t params
- Remove www., normalize twitter.com→x.com, remove trailing slashes
- Content hash: SHA-256 (UNIQUE column)

**Storage (SQLite):**
```sql
-- sources table
id, url, title, source_type, summary, raw_content, 
content_hash (UNIQUE), tags (JSON), created_at, updated_at

-- chunks table
id, source_id (FK), chunk_index, content, embedding (BLOB),
embedding_dim, embedding_provider, embedding_model, created_at
```
- Enable WAL mode and foreign keys with CASCADE deletes
- Lock file to prevent simultaneous ingestion

**Retrieval:**
1. Embed query with same provider
2. Cosine similarity search, return top 10
3. Deduplicate: keep only best chunk per source
4. Sanitize: max 2500 chars per excerpt
5. Pass to LLM: "Answer using only provided context. Cite sources."

---

## Social Media Research — Tiered API Approach

**Cost Tiers:**
- Tier 1 (FREE): FxTwitter API (api.fxtwitter.com) — individual tweet lookups
- Tier 2 (~$0.15/1K): TwitterAPI.io or SocialData — search, profiles, threads
- Tier 3 (~$0.005/tweet): Official X API v2 — last resort only

**Cascade by Operation:**
- Single tweet lookup: Tier 1 → Tier 2 → Tier 3
- Search: Tier 2 → Tier 3
- Profile lookup: Tier 2 → Tier 3
- Thread expansion: Tier 2 → Tier 3

**Rate Limiting:**
- Official X API: 350ms between requests (stay under 450 req/15min)
- Cache results: 1-hour TTL
- Log every API call with timestamps + estimated costs

**Output Format:**
- Key narratives: 3-5 dominant takes
- Notable posts: 5-10 most relevant tweets with links
- Sentiment summary: positive/negative/mixed
- Contrarian takes: interesting minority opinions

---

## Content Humanizer — AI Tell Detection

**Common AI Tells to Detect:**
- Overuse of: "delve", "landscape", "leverage", "it's important to note"
- "in conclusion", "game-changing", "revolutionary", "transformative"
- Tone inflation (dramatic language for mundane topics)
- Generic phrasing that could apply to any topic
- Repetitive sentence structures
- Excessive hedging: "It's worth noting that perhaps..."
- Lists that are too clean/parallel
- Identical paragraph lengths and rhythms

**Rewrite Rules:**
- Replace vague qualifiers with specific, concrete language
- Vary sentence length (mix short punchy + longer ones)
- Use contractions, sentence fragments, informal words
- Remove filler while keeping core message
- Add human cadence imperfections (not errors)

**Channel Tuning:**
- Twitter/X: Punchy, <280 chars, direct, no filler
- LinkedIn: Professional but conversational
- Blog: Longer form, personal anecdotes welcome

---

## Implementation Priority for Mac Mini

**Day 2-3:**
1. Usage Tracker — track API costs immediately
2. Knowledge Base (RAG) — save research, recall later

**Week 2:**
3. Tiered Twitter Research — cheap Scout agent research
4. Content Humanizer — auto-fix AI writing

---

*Reference file for Mac Mini setup*

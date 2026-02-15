# Supabase Research — How We Can Use It

**Date:** 2026-02-08
**Account:** arialinkwell@gmail.com | Org: "Aria Linkwell" (Free tier)
**Dashboard:** https://supabase.com/dashboard/org/chskkqxohqkpkofiytuc

---

## What Supabase Actually Is

Postgres database + Auth + Realtime subscriptions + Edge Functions + Storage + Vector embeddings. All in one. Free tier is generous: 500MB database, 1GB storage, 500K edge function invocations/month, 2 projects.

Think of it as: **a backend you don't have to build.** Tables, API, auth, cron jobs, all ready to go.

---

## How We Should Use It (Priority Order)

### 1. 📊 X Trend Tracker & Engagement Database
**Problem:** Our X cron scans find trends but they vanish after the session. No history, no patterns.
**Solution:** Store every trend scan in Supabase.

Tables:
- `x_trends` — timestamp, topic, source_account, engagement_metrics, category (DeFi/security/Base/etc)
- `x_posts` — our posts, engagement over time, what worked vs didn't
- `x_engagements` — replies/quotes we did, their performance

**Why it matters:** After 30 days we can answer "what topics get us the most engagement?" and "what time of day works best?" with real data instead of vibes.

### 2. 🔬 DeFi Protocol Research Database
**Problem:** Competitive research (like the Octant PM task) is scattered across markdown files. Hard to query, compare, update.
**Solution:** Structured tables for protocol analysis.

Tables:
- `protocols` — name, category, TVL, tagline, homepage_url, social_links
- `protocol_metrics` — date, protocol_id, TVL, users, fees, social_followers (track over time)
- `protocol_features` — protocol_id, feature_name, description, comparison_notes
- `vault_strategies` — protocol_id, strategy_type, APY, risk_level

**Why it matters:** Next time Mashal asks "how does X compare to Y?" we query a table, not re-read 20K of markdown. We can also track TVL changes over time.

### 3. 🔒 Security Findings Tracker
**Problem:** Bug bounty findings in markdown. No way to cross-reference patterns across audits.
**Solution:** 

Tables:
- `audit_targets` — protocol, bounty_url, scope, reward_tiers, status
- `findings` — target_id, severity, title, description, affected_code, status (submitted/accepted/rejected)
- `vulnerability_patterns` — pattern_name, description, occurrences (links to findings)

**Why it matters:** Build a personal knowledge base of vuln patterns. "Show me all access control issues I've found" becomes a SQL query.

### 4. ⏰ Scheduled Data Pipelines (Edge Functions + pg_cron)
**Free on Supabase.** pg_cron can trigger Edge Functions on a schedule.

Use cases:
- **Daily DeFi snapshot:** Pull TVL data from DeFiLlama API → store in `protocol_metrics`
- **X engagement tracker:** Check how our posts performed 24h later
- **Job board scraper:** Check crypto job boards weekly, store new listings
- **Octant metrics tracker:** Track octant.build TVL, staking, allocation data over time

### 5. 📝 Content Calendar & Ideas Bank
Tables:
- `content_ideas` — idea, source, category, status (draft/scheduled/posted/archived), priority
- `content_calendar` — date, platform, content_type, post_text, media_url, status

**Why it matters:** Right now content ideas live in our heads or daily notes. A queryable table means nothing gets lost.

### 6. 🤖 Vector Embeddings for Smart Search (AI-ready)
Supabase has pgvector built in. We can:
- Store embeddings of all our research docs → semantic search across everything
- "Find me research similar to the Morpho analysis" → vector similarity query
- Build a RAG system for Gabe's PM work: upload Octant docs, query them naturally

---

## What Others Are Building with Supabase + Vibecoding

1. **Analytics dashboards** — Next.js + Supabase + text-to-SQL (ask questions in English, get charts)
2. **Social media automation** — n8n + Supabase + Twitter API (store tweets, schedule posts, track engagement)
3. **Personal CRMs** — Track contacts, interactions, follow-ups
4. **Data pipelines** — Edge Functions pull from APIs on schedule, store in Postgres, visualize with charts
5. **AI apps** — Vector search, chat memory, RAG systems all built on pgvector

---

## Implementation Plan

**Phase 1 (This week):** Create Supabase project, set up X Trend Tracker tables, connect our cron scans to store data
**Phase 2 (Next week):** DeFi Protocol Research DB, import existing competitive research
**Phase 3 (Ongoing):** Security findings tracker, content calendar, vector search

**Tech stack:** Supabase JS client (`@supabase/supabase-js`) + our existing Node.js environment. Edge Functions for scheduled tasks. Free tier covers everything we need right now.

---

## Free Tier Limits (Important)
- 2 projects
- 500MB database
- 1GB file storage  
- 500K edge function invocations/month
- 200 concurrent Realtime connections
- 50MB file upload limit
- pg_cron included ✅
- pgvector included ✅

More than enough for our use cases. We'd only hit limits if we start storing massive datasets.

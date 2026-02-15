# Matthew Berman OpenClaw Setup — Full Video Transcript

**Source:** https://x.com/MatthewBerman/status/2021669868366598632
**Transcribed by:** @libankano on X
**Date:** Feb 11, 2026
**Saved:** Feb 14, 2026

---

## Hardware Setup
- Fresh MacBook Air, wiped clean, OpenClaw installed
- **Clamshell mode** — runs 24/7 on desk, never shuts off
- **TeamViewer** — remote control from anywhere
- **Tailscale** — SSH access from any computer
- Can code with Cursor via SSH from other machines

## Interfaces
- **Telegram (primary)** — uses topics/groups for narrow context
- **Slack** — narrowly implemented, only 2 channels, only he can invoke
- **CLI** — SSH in when needed
- **Scripts** — automation

## Session Management (CRITICAL)
- Default OpenClaw starts new session at 4 AM daily (forgets everything)
- **His fix: Set session expiry to ONE YEAR**
- Uses separate Telegram topics: knowledge base, food journal, cron updates, video research, self-improvement, business analysis, meeting prep
- Narrow topics = better context retention

## Models Used
- Anthropic: Opus, Sonnet, Haiku
- Google: Gemini (2.5 Flash for classification)
- xAI: Grok search
- OpenAI

## Skills/Capabilities
- Personal CRM
- Knowledge base
- Video idea pipeline
- X/Twitter research
- Business meta analysis ("that's a crazy one")
- HubSpot ops
- Humanizer skill (always applied — no AI smell, no em dashes)
- Todoist integration
- Usage tracking
- YouTube analytics

## Data Storage Pattern
- Tries to store ALL data possible
- Traditional database + vector column (hybrid)
- Standardized pattern across different skills
- Tables: contacts, CRM, knowledge base, video pitches, business meta analysis, views, social channels, cronlog

## External Services
- Google Workspace via Gog (calendar, email, Drive backup)
- Asana
- HubSpot
- Todoist
- Fathom (AI meeting transcription)
- Brave search
- GitHub
- X/Twitter
- YouTube APIs
- Firecrawl (web search)

---

## Use Case Details

### 1. Personal CRM
- Downloads all emails daily
- Filters out newsletters/cold outreach
- Finds highest-quality contacts
- Reads emails, builds conversation graph
- Stores in database, always up to date
- Uses Gemini 2.5 Flash for classification
- Updates timeline, last touch, semantic indexing
- Can query: "Who is the last person at X company?"

**Meeting Prep Workflow:**
- Every morning, looks at calendar
- Filters out internal-only meetings
- For each external meeting: shows last conversation, what they want to discuss, who they are

### 2. Knowledge Base
- Single place to throw everything interesting
- Drop file/URL in Telegram → extracts → stores with vectors
- Used in video ideation workflow
- Can query: "Find all articles about Opus 4.6"
- Infinite knowledge base of everything that interested him

### 3. Video Idea Pipeline
- Previously: manual Slack → Asana cards → research
- Now: Drop article in Telegram → posts to Slack → can say "make video about this"
- Workflow: Parse topic → Research X + web → Query knowledge base → Generate pitches → Check for duplicates → Build hooks/outline → Link sources → Create Asana task → Send confirmation
- All happens in ~30 seconds

### 4. Twitter/X Research (Cost-Optimized)
- **Tier 1 (FREE):** fxtwitter API — single tweet lookups only
- **Tier 2 ($0.15/1K):** TwitterAPI.io — search, profiles, threads
- **Tier 3 (expensive):** Official X API v2 — $0.005/tweet
- **Fallback:** xAI API with x search tool (Grok)
- Daisy-chain fallback system, cost-optimized

### 5. YouTube Analytics
- Hits API daily
- Pulls stats for all videos, channel growth
- Persists/snapshots in local database
- Scans competitors' uploads
- Generates PNG charts
- Feeds insights into meta analysis workflow

### 6. Business Meta Analysis (Brian Armstrong inspired)
- Ingests ALL business data
- Council of AI expert agents collaborate
- Daily report on gaps, ways to improve

**Signals ingested:**
- YouTube metrics, CRM health, cron reliability, social growth
- Slack messages, emails, Asana, X
- Fathom meeting transcripts (joins meetings, records, transcribes)
- HubSpot pipeline

**Process:**
1. Compact to top 200 signals by confidence
2. Phase 1: Draft analysis (what to improve?)
3. Phase 2: Growth strategist, revenue guardian, architect review
4. Phase 3: Council moderator reconciles, ranks recommendations
- Runs nightly (when not using Opus)

### 7. HubSpot Integration
- Has access to deal pipeline (sponsorships)
- Not doing many NL queries
- Useful for context in other workflows

### 8. Humanizer Skill
- Uses across ALL content
- Removes AI smell from writing
- Skill from Claw Hub, constantly updated
- Detects patterns, marks problematic spans, rewrites

### 9. Image/Video Generation
- Plugged in Nano, Banana, Veo APIs
- Separate Telegram topics for images vs video
- Can generate any image/video on demand
- Modular/reusable in other automations

### 10. Task Management (Todoist)
- Fathom transcribes meetings
- Sends transcript to Gemini 2.5 Flash
- Extracts: key takeaways for me, takeaways for attendee
- Can say "add task to follow up with X by Friday"
- Cross-references with CRM (who is person, company, context)
- If approved → creates in Todoist

### 11. Usage Tracker
- Tracks ALL spend and usage
- Every AI call, every API call logged
- Can ask: "How much spent this week? Which workflows cost money? 30-day trend?"
- **Total cost: ~$150/month** ($100 Claude subscription + APIs)
- "Relative to value, very cheap"

---

## Automations Schedule

**Every Hour:**
- Sync code repo
- Check for changes (self-evolution or manual)
- Backup to GitHub

**Every Day:**
- Ingest emails
- Collect YouTube analytics
- Health checks against platform
- Nightly business briefings

**Every Week:**
- Synthesize daily notes into long-term memory
- Housekeeping

**All follow pattern:** Cron → Execute → Notify via Telegram

---

## Backup Strategy (CRITICAL)

**Code:**
- All tracked in Git
- Push constantly
- Can rollback if bad state (hasn't happened)

**Data:**
- Databases → Google Drive (timestamped)
- CRM data, analytics, knowledge base, business analysis, cron logs
- Updates ~hourly

**Has disaster recovery doc** — how to restore everything

---

## Memory System

- Using default built-in memory (not QmD yet)
- **Daily notes:** conversations, tasks completed, mistakes
- **Weekly synthesis:** distills patterns/preferences → long-term memory
- **Learning folder:** corrective patterns, mistakes not to repeat

---

## Development Workflow

- **Prefers Cursor over Telegram for development**
- Cursor SSH'd into MacBook Air
- Can see files being created, easier interface
- Multiple git repos: major projects (CRM), OpenClaw as whole
- Verify in Telegram, write tests, commit frequently

---

## Self-Evolution Pattern (Daily)

1. OpenClaw downloads best practices from OpenClaw website
2. Stores locally, always references
3. Cross-references all markdown files against best practices
4. Recommends changes
5. Also downloaded Opus 4.6 prompting guide from Anthropic
6. Cross-references against prompting best practices
7. **Constantly updating itself, cleaning itself**

---

## Key Quotes

> "I truly believe I am one of the most advanced users of OpenClaw on the planet."

> "$150/month total... relative to the value I'm getting is very cheap"

> "If I lost my OpenClaw, I would be very upset if I could not easily replicate it"

> "Make them modular, make them reusable"

> "Opus 4.6 listens to every single word it is prompted with"

---

## Community Insights (from replies)

**@ClaudiusMaxx:**
> "The usage tracker is the sleeper hit... cost awareness is the difference between a hobby and a business"

**@thiagotm:**
> "30 automated jobs/day can either be a goldmine or a money pit depending on whether you track that"

**@Saboo_Shubham_:**
> "A few weeks ago I bought a Mac Mini, installed OpenClaw" — has his own setup

**@PawelHuryn (security concern):**
> "OpenClaw has 186K GitHub stars and 1.5M compromised API keys"
> Built n8n alternative due to security concerns

---

*Full reference for Mac Mini setup*

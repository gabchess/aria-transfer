# Technical Blog Writing Guide
*Compiled Feb 12, 2026 from research + video transcripts + Octant style analysis*

## Source Material
- 3 Octant vault articles (tweets 1945897680732884998, 1950917744003514878, 1999558961624965309)
- Matt Diggity's 8 Humanizing Prompts (YouTube transcript)
- idratherbewriting.com — Reducing complexity of technical language
- ProWrite — 5 Trends Shaping Tech Writing 2024
- `memory/humanizer-reference.md` — 24 AI-slop patterns to avoid

---

## The 8 Humanizing Techniques (Matt Diggity)

### 1. Persona Training
Feed the AI examples of the target voice. We use the 3 Octant vault articles as our style reference. Key traits identified:
- Educational, builds from basics
- "Crash course" / "let's start with..." approach
- Explains WHY before WHAT
- Conversational but authoritative
- Ends with CTA

### 2. Personal Experience
Don't just explain the tech. Weave in WHY we built it, what problems we saw, what users told us. For Octant: reference v1 pain points, community feedback, what epochs felt like for real users.

### 3. Fact Insertion
Real numbers kill AI smell. Use:
- Actual contract parameters (30-day reward duration, WETH not ETH, 3 migration transactions)
- Deployment details (mainnet addresses, block numbers)
- User stats if available (how many stakers, total GLM locked)
- Dates and timelines

### 4. Perplexity (Vocabulary)
Avoid AI's favorite words. Never use:
- "In the realm of", "navigate", "bespoke", "ever-evolving"
- "Meticulous", "utmost", "ensure", "utilize"
- "Landscape", "paradigm", "synergy", "leverage" (unless natural)
- "Testament to", "showcasing", "underscoring"
- Full list in `memory/humanizer-reference.md`

### 5. Burstiness
Vary sentence and paragraph length. Mix:
- Short punchy lines (3-5 words)
- Medium explanations (10-15 words)
- Occasional longer deep-dive sentences
- One-line paragraphs for emphasis
- Longer paragraphs for technical explanation
Never make all paragraphs the same length.

### 6. Unfluffing
Every sentence must provide value. Delete anything that's just filler. Test: if you remove the sentence, does the reader lose anything? No? Kill it.

### 7. Flavor
Be conversational. Use:
- Metaphors and analogies ("v1 epochs are like a bus that leaves every 90 days")
- Rhetorical questions ("So what happens to your rewards while you wait?")
- Idioms and natural dialogue
- Occasional humor (light, not forced)

### 8. Audience Targeting
For Octant RegenStaker content, our audience is:
- GLM holders who staked in v1
- Semi-technical (understand wallets, transactions, DeFi basics)
- Care about: their rewards, simplicity, security
- Reading level: accessible but not dumbed down
- Tone: confident, educational, slightly informal

---

## Technical Writing Simplification Rules

### From idratherbewriting.com
- **Define every term at first use.** "GLM (Golem's token)" not just "GLM"
- **Teach the real term, then explain it.** Don't substitute jargon with baby words. "Earning Power Calculator, the contract that determines how your staked GLM converts to rewards"
- **Create glossaries or inline definitions** for domain-specific terms
- **Check how competitors define the same terms** for consistency

### From ProWrite 2024 Trends
- **Simplification is king.** Plain language, no jargon-dense walls of text
- **Visuals are mandatory.** Diagrams, flow charts, infographics break up text and explain complex flows
- **UX-first.** Think like the user. What do they need to know? What are their pain points?
- **Experience the product.** Don't just describe it, walk through it as a user would

### General Principles
- One idea per paragraph
- Active voice, clear verbs ("You stake GLM" not "GLM is staked by users")
- Build from familiar to unfamiliar (what they know → what's new)
- Short sentences for complex ideas, longer for context/narrative
- Headers as signposts, not decoration
- Sentence case headings (not Title Case)

---

## Octant Article Style Guide (from vault posts)

### Structure Pattern
1. **Hook** — "If you've been following..." or provocative statement
2. **Context/Crash Course** — Start with basics, build up
3. **The Problem** — What's limiting about the old way
4. **The Solution** — What we built and why
5. **How It Works** — Technical explanation (accessible)
6. **Why It Matters** — Implications, what this enables
7. **CTA** — "Want to learn more? Get in touch."

### Voice
- First person plural ("We", "Our") for Octant
- Direct address ("You") for the reader
- Confident but not arrogant
- Educational without being condescending
- Specific over vague (always)

### Hard Bans (Octant-specific)
- NO "public goods" (banned phrase at Octant)
- NO em dashes in casual writing
- NO "It's not X, it's Y" pattern
- NO "Great question!" sycophancy
- NO generic conclusions ("The future looks bright")
- NO significance inflation ("pivotal moment in the evolution of")
- Use "ecosystem growth" or "ecosystem expansion" instead of "public goods funding"

---

## Pre-Writing Checklist
1. [ ] Who is the audience? (Define demographics, knowledge level, goals)
2. [ ] What's the ONE key message? (If reader remembers one thing, what is it?)
3. [ ] What facts/numbers can I include? (Real data kills AI smell)
4. [ ] What personal experience or context makes this unique?
5. [ ] What analogies or metaphors can simplify the hardest concepts?
6. [ ] What diagrams are needed? (Flow charts, before/after, architecture)
7. [ ] Read draft aloud. Does it sound like a human talking?
8. [ ] Run through humanizer-reference.md 24-pattern check
9. [ ] Vary paragraph lengths? Check burstiness.
10. [ ] Every sentence provides value? Kill fluff.

---

## Matt Diggity's Exact Prompts (for reference)

### Persona
> Here is a recent article I've written: [insert]. Analyze my tone, how I form sentences and paragraphs, the level of detail I use in explanations, the amount of humor I use, how often I ask the reader questions, the readability level I use, the vocabulary I use, and the level of emotion I convey in my writing. Give me a 200 word summary of what you've learned.

> Based on the writing style analyzed from the provided article, create content titled [title] at [word count] words. Ensure that the new content strictly adheres to the author's writing style in terms of tone, structure, and approach as identified in the analysis.

### Personal Experience
> Here's my about page: [insert]. Learn about my experience and stories that qualify me as an author for the content we'll write. Also, here's a personal opinion that will shape how you write your content: [insert opinion]
> Create content titled [title] at [word count] words with some of this personal experience and opinion lightly sprinkled in. The integration should be subtle at a level of [1-10] out of 10.

### Perplexity
> Don't always use the most natural words. Use the following words fewer than 3 times on this page: unique, ensure, utmost. Before outputting the content, review it for the following words and rewrite those sentences with appropriate alternatives: meticulous, meticulously, navigating, complexities, realm, bespoke, tailored, towards, underpins, everchanging, ever-evolving, the world of, not only, seeking more than just, designed to enhance, it's not merely, our suite, it is advisable, daunting, in the heart of, when it comes to, in the realm of, amongst, unlock the secrets, unveil the secrets, and robust.

### Burstiness
> Ensure heterogeneous paragraphs. Ensure heterogeneous sentence lengths. And stick to primarily short, straightforward sentences.

### Fact Insertion
> Fact #1: [insert] Fact #2: [insert] Please use these facts when creating content.

### Unfluffing
> Do not include any fluff when producing content. Each sentence should provide value to the overall goal of the content piece. Strictly follow this guideline.

### Flavor
> Engagement is the highest priority. Be conversational, empathetic, and occasionally humorous. Use idioms, metaphors, anecdotes and natural dialogue.

### Target Audience
> My target audience has the following characteristics: 1. [demographics] 2. [tone preferences] 3. [reading level preference]

---
*Reference for all Octant blog posts and technical content. Cross-ref with humanizer-reference.md and octant-copywriting-guide.md*

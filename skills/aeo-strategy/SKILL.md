---
name: aeo-strategy
description: Builds a search and AI-visibility plan for a B2B SaaS company. Turns ICP pains into query clusters, picks a tier-1 query set, checks which competitors ChatGPT, Perplexity and Google AI Overviews cite today, finds content gaps, and produces a prioritised article queue with a 90-day timeline and a monthly visibility check. Use when someone says "AEO strategy", "get cited by AI", "content roadmap", "keyword gap analysis", "what should we publish", or "SEO content plan". Not for writing a single article; use /aeo-content for that.
version: '1.0.0'
license: MIT
---

# AEO strategy

Decide what to publish so the company shows up when buyers ask search engines and AI assistants about its category. This skill produces the plan: query clusters, a tracked query set, the gaps, the article queue and the timeline. `/aeo-content` writes the articles from it.

## When to use it

- A company wants to be cited by ChatGPT, Perplexity, Claude or Google AI Overviews and doesn't know where to start.
- There's a blog, but nobody can say why each post exists or which buyer question it answers.
- You need a 90-day content queue you can defend to a founder or a head of marketing.

**Not for:** a single article (`/aeo-content`), schema on one page (`/schema-markup`), hundreds of templated pages (`/programmatic-seo`), or a local business's map listing (`/local-seo`).

## Inputs

Read these from the user's context folder. Agency users swap `context/` for `clients/<client>/context/`.

| Input | Where | If missing |
|---|---|---|
| ICP and buyer pains | `context/icp/` | Ask for the top 3-5 pains in the buyer's own words. Required. |
| Positioning (category, "not for") | `context/positioning/` | Ask the user for the category they want to be named in |
| 3-5 competitors with URLs | `context/competitors/` | Ask; offer to find them with web search |
| Sales call notes, lost-deal reasons | `context/win-loss/` | Skip Step 3's call mining and say so |
| Search Console export (queries, pages) | User upload | Run anyway; tag every volume `[ESTIMATED]` and say the plan is lower confidence |
| Monthly publishing capacity | `context/goals/` or ask | Default to 8 pieces a month and say so |

If a context file is missing, say which one and what it costs the plan. Never invent pains, volumes or competitor pages.

## Steps

1. **Set the frame.** Confirm the category, 3-5 competitors, the region, and a weekly publishing number the team can actually hold. A steady number beats a burst.
   *Produces:* a parameters block at the top of the plan.

2. **Build ICP-anchored query clusters.** Take each buyer pain and turn it into a cluster: a group of questions a buyer asks about one problem, not one keyword. Aim for 5-8 clusters. Every cluster must map to a pain in `context/icp/` and to something the product actually does. Drop clusters that map to neither.
   *Produces:* a cluster table (cluster, buyer pain, product capability it maps to, funnel stage).

3. **Write the buyer queries.** For each cluster, write the questions the way a buyer types them into ChatGPT, not the way a keyword tool lists them: "best partner portal for a 10-person team on HubSpot", not "partner relationship management software". Pull from five sources and tag each: sales calls, People Also Ask boxes, Reddit and communities, support tickets, and comparison searches ("X vs Y", "alternatives to X").
   *Produces:* a query list tagged by cluster and source.

4. **Pick the tier-1 query set.** Choose about 30-50 queries that sit closest to a buying decision and that you will track every month. Keep the cap: at 50 you can read every answer by hand; at 500 the numbers move and nobody can say why. Everything else stays in the list as research, untracked.
   *Produces:* the tier-1 set, marked inside the query list.

5. **Run the competitor citation check.** Ask each tier-1 query in ChatGPT, Perplexity and Google (look at the AI Overview). For each answer, record: is the company named, which competitors are named, and which sources are cited (the company's site, a competitor's, or a third party like a review site or listicle). Quote the answer text; the quote is the evidence.
   *Produces:* a citation baseline table and a list of third-party pages that keep getting cited but don't mention the company.

6. **Find the gaps.** If you have web search, Exa or Firecrawl, use them to list each competitor's pages and tag them by cluster and funnel stage; plain web search with `site:` queries works as a fallback. Mark clusters where competitors are cited and the company has nothing, and clusters nobody covers well.
   *Produces:* a cluster-by-competitor coverage table and a ranked gap list.

7. **Build the article queue.** For each target query, check first whether the company already ranks for it (Search Console, exact query). If it does, the job is UPDATE that page, not CREATE a new one. Give every item a specific title, a primary query, its cluster, funnel stage and page type. Start bottom-of-funnel: alternatives and comparison pages, then integrations, then how-tos and definitions.
   *Produces:* the queue table.

8. **Lay out 90 days.** Assign the queue to months against the publishing number from Step 1. Month 1 leads with comparisons and the biggest gaps. Add the third-party placements from Step 5 as their own line: getting into a listicle the engines already cite can beat a new page.
   *Produces:* a monthly timeline and a totals table.

9. **Set the monthly visibility check.** Once a month, rerun the tier-1 set across the same engines and record three things per cluster: mention rate (share of answers naming the company), citation share by owner (own site, competitor, third party), and the narrative read. For the narrative read, look at about 10 answers per cluster and mark each red, amber or green on three questions: right category, right comparison set, right "not for". Report a band per cluster, never a score; 10 answers is too few for a number. Every red becomes an `/aeo-content` refresh brief.
   *Produces:* a one-page monthly check template.

## What good looks like

- Every cluster traces back to a named buyer pain and a real product capability.
- Queries read like a person typing a question, not keyword fragments.
- The tier-1 set is small enough to read by hand.
- The citation baseline quotes real answers and names the engine and date.
- Every volume is tagged `[VERIFIED: source]` or `[ESTIMATED]`.
- The queue has specific titles and UPDATE or CREATE flags, not topic categories.
- Monthly counts add up and match the publishing number.

## Mistakes to avoid

- Starting from a keyword tool instead of the buyer's pains. You get traffic that never buys.
- Tracking hundreds of queries. The monthly check stops being readable.
- Planning AI-specific technical work (llms.txt files, "AI-ready" markup) instead of pages and placements. Google states AI Overviews have no extra requirements beyond normal search.
- Writing a new page for a query the site already ranks for.
- Reporting the narrative check as a percentage.

## Output

Save to `outputs/seo-aeo/MMYY-aeo-strategy.md`:

1. Summary: 2-3 sentences and a table of page types by count.
2. Clusters: cluster, pain, capability, stage.
3. Tier-1 query set and citation baseline.
4. Gaps: coverage table and ranked gap list.
5. Article queue: title, primary query, cluster, stage, page type, UPDATE or CREATE.
6. 90-day timeline, plus third-party placement targets.
7. Monthly visibility check template.

Next: run `/aeo-content` on the first queue item, and `/schema-markup` on each page as it ships.

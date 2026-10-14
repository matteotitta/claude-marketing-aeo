---
name: aeo-content
description: Writes B2B SaaS articles built to be cited by ChatGPT, Claude, Perplexity and Google AI Overviews. Leads with a direct answer, backs every claim with a source, covers the adjacent questions, adds a FAQ and plans the schema. Handles definitions, comparisons, alternatives, how-tos, listicles, and refreshes of pages an AI engine describes wrongly. Use when someone says "AEO content", "write an article that gets cited", "answer engine optimization", "comparison page", "refresh this page for AI", or "AI search content". Not for planning what to write; use /aeo-strategy for that.
version: '1.0.0'
license: MIT
---

# AEO content

Write one article in the shape AI engines quote: the answer up front, sourced facts, the follow-up questions covered, a FAQ, and schema that matches the page. Works standalone or from an `/aeo-strategy` queue item.

## When to use it

- You have a queue item from `/aeo-strategy` (title, primary query, cluster, stage).
- You have a single topic and at least three queries a buyer would ask about it.
- An AI engine describes the company wrongly (wrong category, wrong comparison set, wrong "not for") and a page needs fixing.

**Not for:** deciding what to write (`/aeo-strategy`), templated pages at scale (`/programmatic-seo`), or a sales landing page.

## Inputs

Agency users swap `context/` for `clients/<client>/context/`.

| Input | Where | If missing |
|---|---|---|
| Topic and 3+ buyer queries | User, or `/aeo-strategy` queue | Ask. Required. |
| What the product does, differentiators | `context/positioning/`, `context/messaging/` | Ask; don't guess features |
| Buyer pains and language | `context/icp/` | Write from the queries alone and say so |
| Voice | `context/brand/` | Use plain operator English |
| Competitors (for comparison pages) | `context/competitors/` | Research their public pages; cite them |
| Existing page (for a refresh) | URL or file | Required for a refresh |

## Pick the mode

| Mode | Length | Use for |
|---|---|---|
| Short block | 800-1,500 words | Definitions, FAQ pages, short comparisons |
| Long-form | 2,000-4,000 words | Head-to-head comparisons, alternatives lists, how-to guides, industry guides |

Query shape decides the page type: "X vs Y" is a comparison, "what is X" a definition, "how to X" a guide, "alternatives to X" a list.

## Steps

1. **Check it doesn't already rank.** If Search Console shows the company already gets clicks or impressions for the primary query, improve that page instead of writing a new one.
   *Produces:* a CREATE or UPDATE decision.

2. **Map the adjacent questions.** List 5-7 follow-up questions a buyer asks next (what, how, why, vs, best, examples, cost). These become the H2s.
   *Produces:* the outline.

3. **Write the direct answer first.** The first 40-80 words answer the primary query plainly, with no wind-up. A reader (or a model) who stops there should have the answer.
   *Produces:* the opening paragraph.

4. **Add a key facts box.** 3-5 facts under the opening, each with an inline `[Source, Year]`.
   *Produces:* the facts box.

5. **Write the sections.** 200-400 words per H2. Each section opens with its own one-sentence answer and carries one or two quotable facts with inline citations. Use at least three third-party sources across the article, not only the company's own site. Add a comparison table (under 10 rows) and a numbered list where they fit the content.
   *Produces:* the body.

6. **Add the FAQ.** 5-10 questions. Each answer stands alone: a 40-60 word direct answer, then detail if needed.
   *Produces:* the FAQ section.

7. **Date it.** Put a visible "Last updated: YYYY-MM-DD" at the top and name when it should next be refreshed (comparisons and pricing quarterly, definitions yearly).
   *Produces:* the date line and refresh note.

8. **Plan schema and links.** Name the schema types (Article, FAQPage, HowTo) and list 3-5 internal links to and from related pages. Hand the finished page to `/schema-markup` for the JSON-LD.
   *Produces:* schema plan and link list.

9. **Check before handing over.** Every fact sourced, every URL real, no invented statistics, quotes or review ratings. Anything you can't verify gets `[NEED TO VERIFY]`, not a guess.
   *Produces:* a clean draft with open items flagged.

## Comparison and alternatives pages

- Name competitor strengths and your own limits. Pages that read like a referee get trusted and quoted; pages that read like an ad don't.
- On "alternatives to X" lists, include 4-7 real options, each with a one-line summary and a "who it's for" block.
- Only state competitor facts you can cite from their public pages, with the date you checked.

## Fixing a wrong AI answer

Quote the wrong answer, name the engine and the date, and find the source it's drawing on. Then pick the route: fix your own page if it's the source; publish a clearer page if nothing of yours answers the question; get onto the third-party page the engine trusts if that's the source; or ask that page's owner for a correction if it's simply wrong. Recheck in the next monthly visibility check from `/aeo-strategy`.

## What good looks like

- The opening answers the primary query in under 80 words.
- Every section could be quoted alone and still make sense.
- Claims carry inline citations; nothing is invented.
- The page covers the follow-up questions, not just the headline one.
- It reads like a person who knows the subject, in the company's voice.

## Mistakes to avoid

- A long intro before the answer.
- A "Sources" list at the bottom instead of citations next to each claim.
- Stats with no source, or rounded to sound better.
- Schema that describes things not visible on the page.
- Bashing competitors. It costs trust and citations.

## Output

Save to `outputs/seo-aeo/MMYY-<slug>.md` with: title, last-updated line, direct answer, key facts box, H2 sections, comparison table where relevant, FAQ, schema plan, internal links, refresh date, and a short list of `[NEED TO VERIFY]` items.

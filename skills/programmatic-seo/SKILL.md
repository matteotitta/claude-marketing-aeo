---
name: programmatic-seo
description: Plans a set of search pages generated from structured data (integrations directories, competitor comparisons, alternatives pages, use-case and persona pages, templates, glossaries, location pages). Picks the right playbook for the data you have, finds the money-keyword pattern, and specifies the data schema, URL structure, page template, internal linking, schema markup and quality rules, with a guard against thin and doorway pages. Use when someone says "programmatic SEO", "pSEO", "pages at scale", "integration pages", "comparison pages at scale", or "template directory". Not for writing one article; use /aeo-content for that.
version: '1.0.0'
license: MIT
---

# Programmatic SEO

Plan a set of pages built from one template and a data source, so a B2B SaaS company can cover dozens of buyer searches that share one shape. The output is a strategy document and a page-one template, not hundreds of finished pages.

## When to use it

- The company has structured data that maps to real buyer searches: integrations, competitors, use cases, industries, roles, templates.
- The same search shape repeats with one word changing: "[tool] integration", "[competitor] alternative", "[tool A] vs [tool B]".

**Not for:** a single article (`/aeo-content`), deciding the overall content plan (`/aeo-strategy`), or a company with no real data behind each page. If every page would only swap a name, don't build it.

## Inputs

Agency users swap `context/` for `clients/<client>/context/`.

| Input | Where | If missing |
|---|---|---|
| What the company sells and to whom | `context/positioning/`, `context/icp/` | Ask. Required. |
| The data asset (integration list, competitor list, use cases, a dataset) | User | Ask. Required; no data, no playbook. |
| Competitors | `context/competitors/` | Research their public pages |
| Search demand | Keyword tool or Search Console export | Tag every volume `[ESTIMATED]` and recommend validating |
| Goal (traffic, signups, demos) | `context/goals/` | Ask |

## Steps

1. **Audit the data.** List the structured data that exists or could be built, how many rows it has, and how often it changes.
   *Produces:* an asset inventory.

2. **Match data to a playbook.** Common B2B SaaS fits: integrations, comparisons ("X vs Y"), alternatives ("X alternative"), use cases, personas or roles, industries, templates, glossary, directories, locations. Pick one, two at most. Start with one.
   *Produces:* the chosen playbook with a one-paragraph reason (data ready, demand real, competitors weak).

3. **Find the money-keyword pattern.** Look for searches with the same intent where the search engine returns the same page type and one word changes. One good pattern gives dozens of pages. Low volume per page is fine if everyone searching is close to buying. Check intent by reading what actually ranks for 3-5 rows, not by guessing from the keyword.
   *Produces:* the pattern, example rows and a demand estimate.

4. **Design the data schema.** Write the fields each page needs in YAML: the entity name, the facts that differ per page, the source of each field, and how often it's refreshed.
   *Produces:* a YAML schema.

5. **Set the URL structure.** Short, hyphenated, predictable, keyword-bearing: `/integrations/hubspot`, `/compare/acme-vs-rival`. One pattern per playbook.
   *Produces:* URL pattern with three examples.

6. **Build page one by hand.** Write the first page in full: direct answer up front, the data block that differs per page, use-case guidance, FAQ, call to action. Save it as the template, and write a short process file the team reads before each new page. Every weakness in page one repeats across the set.
   *Produces:* the page-one template and process file.

7. **Apply the per-page bar and doorway guard.** Every page must carry something the current top three results don't (real data or a table, a calculator, a routing quiz, real screenshots or examples). Every variant must say something only it can say: the integration's actual workflow, that competitor's real differences, that industry's own examples. Google's spam policies treat many pages made mainly to rank as scaled content abuse, and pages that only funnel visitors to one destination (for example city pages that differ only by the city name) as doorway pages. A page that differs from its siblings only by the swapped word fails both tests, so it doesn't ship.
   *Produces:* a per-page uniqueness rule for the template.

8. **Map internal links.** Hub-and-spoke: each page links to its category hub, related pages link to each other. Set a floor of at least five in-body links into every new page from close-topic pages, with anchors that match its target query. Menu and footer links don't count.
   *Produces:* a linking map.

9. **Name the schema.** For example `SoftwareApplication` for integration pages, `Article` for comparisons, `FAQPage` for the FAQ block, `BreadcrumbList` everywhere. Hand the template to `/schema-markup`.
   *Produces:* schema types per page type.

10. **Plan the rollout.** Pilot 10 pages, measure indexing and clicks for 4-6 weeks, fix the template, then add pages in small batches. Name the maintenance trigger for each field (a competitor changes pricing, an integration ships a new feature).
    *Produces:* a rollout and maintenance plan.

## What good looks like

- Each playbook is tied to a named money-keyword pattern, checked against what ranks.
- Demand is sourced or tagged as an estimate; no guaranteed rankings.
- Each page has a stated reason to exist that its siblings don't share.
- The pilot is small and has a decision date.

## Mistakes to avoid

- Launching hundreds of pages at once.
- Pages that differ only by the swapped name or city.
- Competitor facts with no source or date.
- Thin pages padded with generic text to hit a word count.
- No owner for keeping the data current.

## Output

Save to `outputs/seo-aeo/MMYY-pseo-strategy.md`: summary, asset inventory, playbook and reason, money-keyword pattern, YAML schema, URL pattern, page-one template, uniqueness rule, linking map, schema plan, pilot and maintenance plan.

Next: write page one with `/aeo-content`, add schema with `/schema-markup`.

---
name: schema-markup
description: Generates paste-ready JSON-LD schema markup for a page or a batch of pages (Organization, Article, Product, FAQPage, HowTo, SoftwareApplication, Review, Person, Event, LocalBusiness, BreadcrumbList), combines multiple types in one @graph, flags missing required values, and gives a validation checklist. Use when someone says "schema markup", "structured data", "JSON-LD", "rich results", "schema audit", or "add schema to this page". Not for a full technical SEO audit.
version: '1.0.0'
license: MIT
---

# Schema markup

Write the JSON-LD that tells search engines and AI assistants what a page is: who published it, what product it describes, which questions it answers. Output is a code block ready to paste, plus a list of gaps and a validation checklist.

This runs once the page content exists. Decide what to write with `/aeo-strategy`, write it with `/aeo-content`, then structure it here.

## When to use it

- A page is about to ship (new or refreshed) and needs structured data.
- An audit found pages with no schema or with errors.
- A batch of templated pages from `/programmatic-seo` needs one schema pattern.

**Not for:** pages whose copy isn't written yet, or a crawl, speed and indexing audit.

## Inputs

Agency users swap `context/` for `clients/<client>/context/`.

| Input | Where | If missing |
|---|---|---|
| The page (URL, HTML or final copy) | User | Required |
| Company name, URL, logo, social profiles | `context/brand/` | Ask; never make up profile URLs |
| Author names and bios | `context/brand/` or the page | Use Organization as author and say so |
| Product names, prices, tiers | `context/positioning/` or the pricing page | Leave price out and flag it |

## Steps

1. **Classify the page.** Match it to a schema type:

   | Page | Schema type |
   |---|---|
   | Homepage, about | `Organization` (+ `WebSite`) |
   | Blog post, guide | `Article` or `BlogPosting` (+ `HowTo` if it's real steps) |
   | FAQ page or FAQ section | `FAQPage` |
   | Product or pricing page | `SoftwareApplication` or `Product` with `Offer` |
   | Comparison page | `Article`, plus `SoftwareApplication` per product compared |
   | Author bio | `Person` |
   | Webinar, event | `Event` |
   | Local office or service area | `LocalBusiness` |
   | Every page | `BreadcrumbList` |

   *Produces:* the type list for the page.

2. **Check required values.** Pull each value from the visible page. Key requirements:
   - `Organization`: `name`, `url`; add `logo`, `sameAs`.
   - `Article`: `headline`, `image`, `datePublished`, `author`; add `dateModified`.
   - `Product` / `SoftwareApplication`: `name`, `offers` with `price` and `priceCurrency`.
   - `FAQPage`: `mainEntity`, a list of `Question` items, each with an `acceptedAnswer`.
   - `HowTo`: `name` and `step`, each step with `name` and `text`.
   - `Event`: `name`, `startDate`, `location`.

   If a required value isn't on the page, don't invent it. Flag it as a content gap for the page owner.
   *Produces:* a filled / gap list.

3. **Combine with `@graph`.** When a page has several types, put them in one `@graph` with `@id` references between them, rather than several separate script blocks:

   ```json
   {
     "@context": "https://schema.org",
     "@graph": [
       { "@type": "Organization", "@id": "https://example.com/#org", "name": "Example", "url": "https://example.com" },
       { "@type": "Person", "@id": "https://example.com/#author", "name": "Jane Doe" },
       { "@type": "Article", "headline": "...", "author": { "@id": "https://example.com/#author" }, "publisher": { "@id": "https://example.com/#org" } },
       { "@type": "BreadcrumbList", "itemListElement": [] }
     ]
   }
   ```

   *Produces:* the draft markup.

4. **Output the block.** One `<script type="application/ld+json">` for the page `<head>`. Always JSON-LD, never microdata or RDFa.
   *Produces:* paste-ready code.

5. **Validate.** Give the user this checklist:
   - Run the URL through Google's Rich Results Test and the Schema.org validator; zero errors.
   - Eyeball every value against the visible page. Anything in the schema must be on the page.
   - For JavaScript-injected schema, test the live URL, since many fetch tools can't see it.
   - Check Search Console's enhancement reports a week or two after publish.

   *Produces:* the validation checklist.

## What good looks like

- One `@graph` per page, with consistent `@id`s across the site (the same Organization id everywhere).
- Every value matches what a visitor can see.
- Gaps are flagged, not filled with guesses.
- `dateModified` updates when the page is refreshed, which helps AI engines treat it as current.

## Mistakes to avoid

- Reviews, ratings or prices in schema that aren't shown on the page. This is the classic manual-action trigger.
- An `aggregateRating` built on one or two reviews.
- `sameAs` links to social profiles that don't exist.
- Expecting a rich result from every type. Google has narrowed FAQ and HowTo rich results, so treat those types as clarity for machines, not a guaranteed visual result.
- Several separate script blocks that describe the same Organization differently.

## Output

Save to `outputs/seo-aeo/MMYY-schema-<page-slug>.md`:

1. Page and detected types.
2. Required values: filled, or gap with who needs to fix it.
3. The JSON-LD block.
4. Validation checklist.

Pattern adapted from coreyhaines31/marketingskills (MIT).

---
name: docs-writing
description: Write or edit developer documentation for the API in this content repo. Use for guides under docs/, API reference pages, explainers, changelog entries and migration pages, and for translating them. Covers voice, formatting, frontmatter, links and the English-leads-translations rule.
---

# Docs writing

Content lives in `src/content/{locale}/`. `en` is the source of truth. Every other locale (`de`, `fr`, `it`, `tr`, `zh`) mirrors its file structure.

## Before you write

1. Find the closest existing page in `en` and read it. Match its structure, tone and heading depth instead of inventing a new shape.
2. Check the page type below for the required frontmatter.
3. Read neighbouring pages for terms you will reuse, such as stop, station, via, waypoint, so wording stays consistent.

## Page types and frontmatter

| Type | Location | Frontmatter |
| :--- | :--- | :--- |
| Guide | `docs/getting-started/*.mdx` | `title`, `description`, optional `tag: Guide` |
| Product page | `docs/no-code/*.mdx` | `title`, `description`, `tag: Product` |
| API reference | `api-reference/<area>/*.mdx` | `title`, `heading`, `description`, `schemaName`, `schemaType` (`Query`, `Mutation`, `Subscription`) |
| Explainer (tooltip) | `explainers/route/*.mdx` | `title`, `slug`, `fieldName`, `variant: tooltip`, `position: top` |
| Changelog | `changelog/<year>/<month>.mdx` | `title`, `date` (`DD-MM-YYYY`) |
| Migration | `migrations/**` | follow the sibling page |

Rules:
- `description` is one sentence that says what the page lets the reader do. Quote it.
- Explainer `slug` and `fieldName` must match the schema field exactly, for example `CreateRouteInput.via`. Explainer titles follow "How X works".
- API reference intros open with what the query or mutation does and why it is useful, for example "Get the pricing details of a specific tariff by using the tariff query."

## Voice

- Talk to the reader as "you". Prefer active, present tense and short sentences.
- Say "the API". Never "the Chargetrip API" or "Chargetrip's API".
- Leave the company out when a sentence reads fine without it. Use "Chargetrip" only when an actor truly cannot be dropped, and rarely, about once per page. Never "we", "our" or "us".
- No filler such as "stored in the Chargetrip database". Open with what the thing does.
- **Never use em dashes.** Split into two sentences, or use a comma. En dashes in numeric ranges such as `80–100%` are fine.
- Headings are sentence case. Link text stays lowercase mid-sentence even if it echoes a heading, for example "customize your route".
- Write original wording. When inspired by another vendor's docs, restructure and rephrase, do not adapt near-verbatim.
- Spell check. Typos in titles propagate into every translation.

## Formatting

- Identifiers, fields, enum values and inputs go in backticks: `via`, `station_id`, `OPTIMIZE_TRAVEL_TIME`.
- Internal links are root-relative with no locale prefix: `/docs/getting-started/route-stop`, `/api-reference/route/mutation-create-route`.
- Any link that leaves the docs portal is an external link. It opens in a new tab and uses anchor markup, never the inline markdown form `[label](https://...)`. This includes other Chargetrip sites such as the marketing site and the dashboard.
  `<a href="https://example.com" target="_blank" rel="noopener noreferrer">label</a>`
- Examples use fenced blocks with a title, for example ```` ```graphql Example request ````, and live inside a collapsible:

  ```mdx
  <details>
    <summary>Calculate a route with a 30-minute non-charging stop.</summary>
    One or two sentences explaining what the example shows.

  ```graphql Example request
  ...
  ```
  </details>
  ```

- Use realistic, consistent values: vehicle id `60b8d5a2f1c8e9001b9e2831`, Amsterdam `[4.9041, 52.3676]`, Cologne `[6.9583, 50.9375]`. Coordinates are `[lng, lat]`.
- Callouts use `> [!WARNING] Title` followed by the text on the next quoted line.
- Ordered lists keep their bold title, because the portal renders it as a design feature. The title is bold on its own line, and the normal text sits on the next line, indented under the item:

  ```mdx
  1. **Create a route**
     Call the `createRoute` mutation with at least a `vehicle`, `origin`, and `destination`.
  2. **Open a subscription**
     Subscribe to the `route` subscription using the `id` from the previous step.
  ```

- Do not use the bold lead-in pattern in unordered lists, such as `- **Setting:** explanation`. It reads as machine written. Start the item with a plain sentence that carries the point. Use bold sparingly in bullets, for a term the reader must not miss.
- Do not describe unverified behaviour. If a claim comes from the author and you cannot confirm it in the repo, flag it in your reply.

## Tables

Check that every table stays readable at the width of the content column.

- A table with more than about three columns, or with long cell text such as sentences or long field paths, is likely to grow too wide, especially on mobile and in translations, which are usually longer than `en`.
- When a table is too wide, change the content style instead of squeezing it. Options are a definition style list, one short section per row, or splitting the table into two smaller ones.
- Keep cells short. Move explanations out of the table into the text around it.
- Verify the longest locale (often `de` or `fr`) as well as `en`.

## Semantic HTML and design review

Markdown cannot express everything. When plain, unstyled HTML has a better semantic fit than the markdown construct, for example `<dl>` for term and description pairs, `<details>` for collapsible content, `<figure>` with `<figcaption>` for an image with a caption, or `<kbd>` for keys, prefer it, and flag it.

- Say in your reply which elements you used and why, since the portal may not style them yet.
- Keep the HTML unstyled. No inline styles and no class names.
- Because unstyled elements may look off, recommend that the PR is merged only after a design review has covered them. Put that in the PR description.

## Explaining an approach

For how-to sections, state the scenario first, then the rule, then numbered steps, then a short "Things to keep in mind" list for caveats. Prefer prose over pasted source code. Show a small request example only when it clarifies the result.

## Translations

- Work in `en` first. Edit other locales only when asked, or when a change must keep structure on par.
- "On par" means the same file list in every locale. Delete pages that were removed from `en`, and add translated versions of pages that were added. Check with a file list diff.
- Never translate: frontmatter keys, `slug`, `fieldName`, `schemaName`, `schemaType`, `variant`, `position`, code, field names, enum values, URLs.
- Translate: `title`, `description`, `heading`, body, `summary` text and code comments inside examples where the locale already does so.
- Match the locale's existing address form and terms by reading a sibling page. Current forms: `de` Sie, `fr` vous, `it` tu, `tr` siz, `zh` 您.
- Keep the same structure, headings order, list numbering and links as `en`. Apply the same voice rules, including no em dashes.

## Checklist before finishing

- [ ] No em dashes: `grep -rIn $'\xe2\x80\x94' .` from the content root returns nothing.
- [ ] Frontmatter complete for the page type.
- [ ] Voice rules applied: "the API", no "we", no filler.
- [ ] Every link that leaves the docs portal uses anchor markup with `target="_blank" rel="noopener noreferrer"`.
- [ ] Ordered list items have a bold title on its own line with the text below. Unordered lists have no bold lead-ins.
- [ ] Tables fit the content column in `en` and in the longest locale, otherwise the content style is changed.
- [ ] Any semantic HTML is flagged, and the PR notes that it should be merged only after a design review.
- [ ] Internal links resolve to existing pages.
- [ ] If locales were touched, file lists match `en`.
- [ ] Unverified claims are called out to the user.

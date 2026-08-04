# Search, autocomplete and browse

## Search fails by query type, not by relevance tuning

Failure rate scales with how far the query sits from a literal product name. This table is a **ready-made test suite** — run one query of each type against your own search before touching anything else. `(measured)`

| Query type | Example | Sites failing |
|---|---|---|
| Exact | "iPhone 15 Pro" | 12% |
| Product type | "running shoes" | 20% |
| Symptom | "dry skin" | 37% |
| Feature | "waterproof jacket" | 39% |
| Use case | "camping in winter" | 43% |
| Compatibility | "case for Pixel 8" | 44% |
| Abbreviation or symbol | "13\" MBP" | 54% |
| Non-product | "return policy", "order status" | 66% |

Overall, 56% of sites fail to support users' search needs, and only 44% of desktop and mobile experiences rate decent or good. Roughly half of users prefer search as their product-finding strategy.

**34% of users search for non-product content** — order status, returns, store hours, contact — and **15% of sites do not support it at all**. Your search index probably needs your help pages in it.

Other countable failures: 37% don't persist the query in the field after submitting; 96% get contextual snippets wrong; 46% get category-scope autodirection wrong.

## Autocomplete

Present on 80% of sites; **only 19% implement it correctly.** `(measured)`

- Cap at **~10 on desktop, ~8 on mobile**, targeting 4–8 so the list fits the default viewport. A desktop list must not introduce a scrollbar.
- Style **category-scope suggestions differently** from query suggestions — they do different things.
- Highlight the matched text; highlight the active suggestion; give the dropdown visual depth so it reads as an overlay.
- **Copy the active suggestion into the search field as the user arrows through it.** 58% don't. This is a keyboard-interaction contract that is almost always missed.
- Handle close misspellings — 69% of desktop and 28% of mobile sites offer nothing.
- On mobile, keep a submit button adjacent to the field (21–27% lack one) and give suggestions adequate touch spacing.

## The focused-but-empty search screen

Tapping into search is a moment of intent without a query. A blank screen puts all the work on the user. Offer quiet, ignorable support: recent searches, popular items, and personalized suggestions where you have real signal. `(untested for conversion, standard practice)`

Two constraints on the personalized part: label the data source ("Because you searched for X") and keep the suggestions genuinely dismissible. See `adaptive-and-post-purchase.md` for how recommendations go wrong.

## No results

**About 50% of sites offer no recovery at all**, and minimal-guidance dead ends frequently trigger site abandonment. Five strategies, roughly in order of value: `(measured)`

1. Related categories.
2. Alternative searches **with a preview of the top 3–5 products per alternate query** — bare text links get ignored.
3. Personalized recommendations.
4. Help, chat or phone links.
5. Popular products and categories.

Keep the failed query visible and editable. The user's next move is almost always to change one word.

## Category and browse screens

Three versions of the same screen, and the middle one is the trap.

**Plain text list.** Clean, but every line looks the same — no hierarchy, no scanning cues, so the user must read every label. All the effort lands on them.

**Photo tiles with a dark overlay.** Looks designed; usually fails. Text-over-image contrast is the single most common accessibility failure in this pattern, and an overlay rarely fixes it across a whole image set. Mismatched photography — one bright, one moody, one obviously stock — reads as a random assortment rather than a product. Every tile is visually busy, so parsing takes longer than the plain list it replaced.

**Cohesive cards: soft solid backgrounds, one clean isolated image per category, unified treatment.** Stylistically consistent imagery plus balanced colour gives the screen a rhythm, so options are understood in seconds. `(qualitative)`

Hard requirements regardless of style:

- Text over an image must meet contrast requirements **at the worst point of the image**, not the average. If you cannot guarantee that across the set, put the text outside the image.
- Category labels must be the words users use, not internal taxonomy.
- Touch targets and spacing per the non-negotiables in SKILL.md.

## Product lists

- 73% of mobile sites fail to display all colour swatches in list items; 42% fail to combine variations into a single list item — so the same product appears several times and the user cannot tell why.
- In search results, **54% don't update thumbnails to match the variation searched for** — searching "red dress" and getting a blue thumbnail reads as a broken result.

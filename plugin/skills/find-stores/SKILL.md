---
name: find-stores
description: Find and qualify Shopify stores as sales leads with the Truffle MCP tools. Use when the user wants prospects, leads, or a list of Shopify merchants to sell to (e.g. "find small German jewellery brands for my app"), or wants research on a specific Shopify store before reaching out.
---

# Find Shopify leads with Truffle

You have four Truffle tools: `truffle_index_stats`, `truffle_search_stores`, `truffle_research_stores` (batch, up to 10), `truffle_research_store` (one, full detail).

## 1. Pin down the brief (ask at most one question)

You need: **what the user sells**, **who it is for** (category, size, country), and **how many leads**. If the user already said it, don't ask. Default: 25 leads, German stores, 10-500 products.

## 2. Translate the brief into filters

- Call `truffle_index_stats` once to see coverage and valid app ids / categories.
- Map the brief to `truffle_search_stores` filters. Examples:
  - "small-to-medium brands" → `product_count_min: 10, product_count_max: 500`
  - "premium" → `price_min: 50`
  - "no reviews app yet" (selling a reviews app) → `lacks_app_categories: ["reviews"]`
  - "uses Klaviyo" → `has_apps: ["klaviyo"]`
  - "young stores" → `opened_after: "2024-01"` (estimated opening month)
  - "just switched away from Yotpo" → `removed_app: "yotpo"`; "installed Klaviyo this month" → `added_app: "klaviyo"`;
    any recent change → `changed: [...]` or `sort: "recently_changed"`. `truffle_recent_changes` lists the change log itself.
  - "companies, not one-person shops" → `entity_type: "company"` (or `legal_form: ["GmbH"]`); "groups with several shops" → `multi_store: true`
  - "also on Amazon" → `channels: ["amazon"]`; "has a wholesale area" → `pages: ["wholesale"]`; "hiring" → `pages: ["careers"]`
  - "growing catalogue" → `products_added_90d_min: 20`; "spends on apps" → `app_spend_min: 100`
  - "micro-influencer-sized brands" → `instagram_followers_min: 1000, instagram_followers_max: 20000` (not every store has a count yet)
  - "no dropshippers or print-on-demand" → `exclude_fulfilment: ["dropshipping", "print_on_demand"]`
  - category → `category: "jewellery"`; a niche → `subcategory`, e.g. `"streetwear"`, `"rings"`, `"tabletop-rpg"` (stats list them)
  - "vegan streetwear" → `subcategory: "streetwear", signals: ["vegan"]`; also `handmade`, `sustainable`, `personalised`, `made_locally`, `second_hand`
  - "real brands, not resellers" → `business_type: ["own_brand"]`; "B2B shops" → `sells_to: "businesses"`; "premium" → `segment: ["premium", "luxury"]`
  - Labels come from a model and newly found stores may not have them yet: if a labelled search is thin, also try
    `query` with German and English keywords, e.g. `"schmuck"` then `"jewelry"`.
- If results are thin, loosen one filter at a time and say which. Search is free; run several searches with different keywords and merge.

## 3. Qualify live, in batches

- Take the 2x your target count of candidates. Call `truffle_research_stores` with 10 domains at a time, `detail: "brief"`.
- Score each store 0-3 for fit against what the user sells (category match, size, price level, apps that signal need or conflict, signs of activity like `newest_product` in the last 6 months). Drop anything below 2, `status` not `active`, or with errors.
- Keep your notes short: one line per store. Do not paste raw tool output back to the user.
- Use `truffle_research_store` with `detail: "full"` only for the top 5 or when the user asks about a specific store.

## 4. Deliver

A table: **store (link) · what they sell · size (products, median price) · why they fit · contact** (email first, then phone, contact form, socials, address). Then a short note on next steps. If the user asks, draft the opening message for the channel they pick.

## Notes

- For shops run by one person, Truffle withholds the owner's private name, address and phone; the Impressum URL is in the result if the user wants to look.
- Don't hunt for personal emails or names of individuals elsewhere.

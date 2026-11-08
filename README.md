# Truffle

Shopify leads for your agent. Search Shopify stores (Germany first), research any store live, and get every way to
reach them: email, phone, contact form, socials, the business behind it. Works in Claude Code and any MCP client.

https://truffle.run

## Install (Claude Code)

```
/plugin marketplace add truffle-run/plugin
/plugin install truffle@truffle
```

Or give your agent this: `Use curl to read https://truffle.run/agents.md and perform the setup to install Truffle.`

## Free preview, then your key

Without a key, search shows true counts and the first 3 stores in full. Research works on any store.

With a key, the full index (every store, labelled by category, niche, business type and more) downloads to your
machine and refreshes daily:

```
/truffle:activate trf_your_key
```

Get a key at https://truffle.run/#pricing.

## Tools

- `truffle_index_stats`: what the index holds and the valid filter values
- `truffle_search_stores`: filter by category, niche, own brand vs reseller, B2B, signals (vegan, handmade…), size, price, revenue band, apps, theme, store age
- `truffle_research_stores` / `truffle_research_store`: live research, contacts and the business behind each store
- `truffle_activate`: unlock the full index with your key

This repository is published from Truffle's main repository; changes made here are overwritten.

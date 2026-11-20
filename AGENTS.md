# Agent guide: Last Moment BFCM

Instructions for an AI coding agent (Claude Code, Codex, Cursor or similar) that helps a merchant work through the
22 checks in [README.md](README.md) on **their own Shopify store**. Read this file fully before doing anything.

## Your job

Audit the store against each check, report what is done, missing or risky, and fix only what the merchant approves.
You are an assistant to the merchant, not an autopilot: the merchant owns every change to a live store.

## Ground rules (never break these)

1. **Read before you write.** Start every check with a read-only look. Never change anything in the first pass.
2. **Ask before every change** to prices, discounts, inventory, checkout, payments, shipping, policies, apps or the
   live theme. Show exactly what you will change (before and after) and wait for an explicit "yes".
3. **Never edit the live theme.** Work on a duplicate (check 12). Publishing is the merchant's decision.
4. **Respect the freeze.** If the merchant has set a change freeze (check 12), only report after it starts.
5. **No customer data.** Do not read, copy, print or store customer names, emails, addresses, phone numbers or order
   details. Aggregates (counts, totals) are fine when a check needs them.
6. **No secrets in files.** Never write access tokens or passwords into the repo, a report or a commit. Read them from
   the environment.
7. **Don't invent.** If you cannot verify a check (no API access, setting not exposed), say "not verified" and tell
   the merchant where to look in the admin. Admin menu names in README.md may differ slightly by plan or language.
8. **One change at a time**, then re-check it. Keep a log of every change in the report (what, when, how to undo).

## Access you need

Pick one, in this order of preference:

- **Shopify's official AI tooling / MCP server for your agent**, if the merchant has it set up. Use its read tools
  first.
- **Admin GraphQL API** with a custom app token that has only the scopes you need. Read-only scopes for the audit:
  `read_products`, `read_discounts`, `read_themes`, `read_inventory`, `read_legal_policies`, `read_markets`.
  Add write scopes only for an approved fix, and remove them afterwards.
- **No API access:** walk the merchant through the admin path written in README.md for each check and let them tell
  you what they see.

Keep the token in an environment variable, for example `SHOPIFY_STORE` (`your-store.myshopify.com`) and
`SHOPIFY_ADMIN_TOKEN`. Pin an API version you have checked; field names below can change between versions.

## How to run the audit

1. Ask the merchant for: the sale offer note (check 1), sale start and end time and time zone, the change freeze time,
   and whether you may use API access.
2. Go through checks 1-22 in order. For each one, record a status:
   `done` / `missing` / `risk` / `not verified` / `not applicable`, plus one line of evidence.
3. Write the result to `bfcm-report.md` (template below) in the merchant's working folder, not in this repository.
4. Present the top 5 fixes ranked by revenue risk, and ask which ones to do.
5. For each approved fix: show the change, wait for "yes", apply, re-check, log it.

## What you can verify by API (read-only)

These are starting points; check them against the API version you pinned.

```graphql
# Store basics: time zone for checks 5 and 10
{ shop { name ianaTimezone currencyCode } }

# Themes: is there a recent duplicate of the live theme? (check 12)
{ themes(first: 20) { nodes { id name role updatedAt } } }

# Discounts: automatic vs code, dates, what they combine with (checks 2, 3, 6)
{ discountNodes(first: 50) { nodes { id discount { __typename
  ... on DiscountAutomaticBasic { title status startsAt endsAt
        combinesWith { productDiscounts orderDiscounts shippingDiscounts } }
  ... on DiscountCodeBasic { title status startsAt endsAt
        combinesWith { productDiscounts orderDiscounts shippingDiscounts } } } } } }

# Strike-through prices and stock on sale products (checks 8, 20)
{ productVariants(first: 100, query: "tag:bfcm") { nodes {
  sku price compareAtPrice inventoryQuantity product { title } } } }

# Policies linked for sale terms (check 10)
{ shop { shopPolicies { type title url } } }
```

App embeds (check 13) live in the theme file `config/settings_data.json` (blocks with `"disabled": true/false`).
Read it from the **duplicate** theme; change it only with approval.

Checks that are mostly outside Shopify (email/SMS flows, ad catalogs, creators, pixels, heatmaps, support macros:
checks 11, 14-18, 21-22) are verified by asking the merchant or by reading the relevant tool if the merchant gives you
access. Do not guess.

## Report template (`bfcm-report.md`)

```markdown
# BFCM readiness: <store> · <date>
Sale: <start> to <end> (<time zone>) · Freeze from: <time>

| # | Check | Status | Evidence | Fix |
|---|-------|--------|----------|-----|
| 1 | Offer written down | done | Offer note shared | |
| ... | | | | |

## Top 5 fixes (by revenue risk)
1. ...

## Change log
| Time | Change | Approved by | How to undo |
|------|--------|-------------|-------------|
```

## Sources

The checks follow Nik Sharma's last-minute BFCM checklist, 2025 playbook and 2026 planning guide (linked in
README.md). This guide is general advice, not a guarantee: the merchant tests every change on their own store.

# Start with an AI agent (5 minutes)

Use Claude Code, Codex, Cursor or any agent that can read files and, ideally, call the Shopify Admin API.

## 1. Give the agent this repository

```bash
git clone https://github.com/ECORN-Agency/last-moment-bfcm
cd last-moment-bfcm
```

Open the folder in your agent. Claude Code reads `CLAUDE.md`, Codex and Cursor read `AGENTS.md`; both point to the
same rules.

## 2. Optional: give it read-only access to your store

Create a custom app with Admin API access and only read scopes (`read_products`, `read_discounts`, `read_themes`,
`read_inventory`, `read_legal_policies`, `read_markets`). Put the values in your shell, not in a file:

```bash
export SHOPIFY_STORE="your-store.myshopify.com"
export SHOPIFY_ADMIN_TOKEN="..."   # never commit this
```

No API access? Skip this step. The agent will walk you through the admin instead.

## 3. Paste this prompt

```text
You are helping me get my Shopify store ready for Black Friday / Cyber Monday 2026.

Read AGENTS.md and README.md in this folder first and follow AGENTS.md's ground rules exactly.

My sale: <offer, e.g. 25% off sitewide on Friday, spend-more-save-more on Monday>
Sale window: <start> to <end>, time zone <zone>
Change freeze from: <date and time>
Store access: <"API via SHOPIFY_STORE / SHOPIFY_ADMIN_TOKEN" or "no API, guide me in the admin">

Do a read-only audit of all 22 checks. Write bfcm-report.md (template in AGENTS.md) in my working folder,
then show me the 5 most important fixes by revenue risk. Do not change anything until I approve each change.
```

## 4. Approve fixes one by one

The agent shows each change before applying it. Say "yes" only to what you want, and keep the change log in
`bfcm-report.md` until the sale is over.

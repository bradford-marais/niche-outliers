# Niche Outliers

Type a niche → the **top 10 viral videos** in it, dropped into a Notion page ready to copy.

It scrapes **Instagram + TikTok** through Apify, ranks everything by views, and lists the top 10 with creator, view count, caption, and a clickable link. No breakdowns, no noise — just what's crushing it in the niche right now.

Built to run inside **Claude Code**.

> Pairs with the **Watch** skill — find a video here, run Watch on its link for the full hook + format teardown.

---

## Setup (one time, ~5 minutes)

### 1. Get it

```
git clone https://github.com/bradford-marais/niche-outliers.git
cd niche-outliers
claude
```

### 2. Connect Apify + Notion

Two connectors, both done once in Claude Code:

- **Apify** — connect the Apify MCP with your Apify API token. A free Apify account covers a top-10 pull easily. Get a token at apify.com → Settings → Integrations.
- **Notion** — connect the Notion MCP so it can write your report page.

### 3. (First run) pick where reports go

The first time you file results, it asks which Notion page to put them under and saves it in `config.md`. After that it never asks again.

---

## How to use it

Open Claude Code in this folder and just name a niche:

> "Find outliers in luxury real estate"   ·   "What's viral in women's hormone health?"   ·   "Niche outliers for custom home builders"

You'll get a Notion page like:

```
Niche Outliers — luxury real estate — 3 June

| # | Platform  | Creator        | Views | Caption                        | Link |
|---|-----------|----------------|-------|--------------------------------|------|
| 1 | TikTok    | @milliondollar | 4.2M  | This $12M listing has a secret | ↗    |
| 2 | Instagram | @luxehomes     | 2.8M  | POV: your realtor shows you... | ↗    |
| ...                                                                       |
```

Scan it, click into the ones that fit your client, and lift the format. Want a deeper look at any single one? Run the **Watch** skill on its link.

---

## What it does / doesn't

- **Does** — scrape IG + TikTok for the niche, rank by views, file the top 10 to Notion.
- **Doesn't** — break each video down (that's Watch's job), write scripts/hooks, or post anything. It only finds and lists.

---

## Troubleshooting

- **"Apify isn't connected"** → connect the Apify MCP with your API token, then run again.
- **Results look off-niche** → name the niche more specifically (e.g. "luxury home staging" not just "homes").
- **One platform came back empty** → usually a rate limit; run it again in a minute.

---

MIT licensed. It reads public post data and writes to your Notion — nothing is posted on your behalf.

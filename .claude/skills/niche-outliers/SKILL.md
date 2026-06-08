---
name: niche-outliers
description: Type a niche and get the top 10 viral videos in it. Derives the niche's hashtags, scrapes Instagram + TikTok via Apify, ranks every result by view count, takes the top 10, and writes them into a Notion page — creator, views, caption, and a clickable link per video — for the coach to scan and copy. Use when the coach says "find outliers in [niche]", "what's viral in [niche]", "niche outliers", "find viral videos for [niche]", or names a niche to research. Does NOT break down or score individual videos (run the Watch skill on any one for a full teardown), does NOT write scripts/hooks/captions, and does NOT post anything to social.
---

# Niche Outliers

Type a niche → the top 10 viral videos in it, dropped into a Notion page ready to copy.

It scrapes **Instagram + TikTok** through Apify, ranks everything by views, and lists the top 10 with links. No breakdown, no fluff — just "here's what's crushing it in this niche right now."

> Pairs with the **Watch** skill: find a video here, run Watch on its link for the full hook + format teardown.

---

## Before you run it

The Apify and Notion connectors must be live in Claude Code. They're pre-wired in this repo's `.mcp.json` — full walkthrough in **`SETUP.md`**. First run only, the skill asks which Notion page to file results under, then saves it in `config.md` so it never asks again.

---

## Workflow

```
- [ ] Step 0: Check the connectors are live
- [ ] Step 1: Get the niche
- [ ] Step 2: Turn the niche into search terms
- [ ] Step 3: Scrape Instagram via Apify
- [ ] Step 4: Scrape TikTok via Apify
- [ ] Step 5: Merge, dedupe, rank by views, take top 10
- [ ] Step 6: Write the Notion page
```

## Step 0: Check the connectors are live

Before scraping, confirm both MCP servers are actually connected — an Apify tool and a Notion tool must be available this session.

- If **Apify** isn't connected: stop and say *"Apify isn't connected — open `SETUP.md` and run `/mcp` to authenticate it,"* then exit. No point scraping without it.
- If **Notion** isn't connected: you can still scrape and show the ranked top 10 in chat, but tell the coach Notion isn't connected (so nothing gets filed) and point them at `SETUP.md`.

Don't guess or fabricate around a missing connector. Name what's missing and stop.

## Step 1: Get the niche

The niche is whatever the coach typed (e.g. `luxury real estate`, `women's hormone health`, `custom home builders`). If they didn't give one, ask: *"What niche should I pull viral videos from?"*

## Step 2: Turn the niche into search terms

From the niche, derive **3–5 hashtags** and one plain keyword query. Example — `luxury real estate` → `#luxuryrealestate`, `#realestate`, `#luxuryhomes`, `#realtorlife` + query `luxury real estate`. Keep them tight to the niche so results stay on-topic.

## Step 3: Scrape Instagram via Apify

Use the Apify MCP to run the **Instagram scraper** actor (`apify/instagram-scraper`). First read the actor's input schema via the MCP so you pass valid fields, then run it to pull **recent posts for the niche hashtags** (about 30 results). You only want **videos/reels** — discard image-only posts.

From each result keep: **view count** (the reel's video view count), **creator** (owner username), **caption**, **post URL**.

## Step 4: Scrape TikTok via Apify

Use the Apify MCP to run the **TikTok scraper** actor (`clockworks/tiktok-scraper`). Read its input schema, then run it for the same niche hashtags (about 30 results).

From each result keep: **view count** (`playCount`), **creator** (author name), **caption** (post text), **video URL**.

> If an actor name or input field differs in your Apify account, use the Apify MCP to list available actors and pick the closest Instagram / TikTok scraper. Don't guess input fields — read the schema first.

## Step 5: Merge, dedupe, rank, take top 10

Combine the Instagram and TikTok results into one list, each tagged with its platform. Dedupe by URL. **Sort descending by view count.** Take the **top 10**.

If a platform returns nothing (rate limit, empty niche), continue with whatever came back and note which platform was thin.

## Step 6: Write the Notion page

Read `config.md` for the `notion-target:` page.
- If it's set, create the page under it.
- If it's empty, ask: *"Which Notion page should I file these under?"* — take the URL/ID, then **write it into `config.md`'s `notion-target:` line** so you never have to ask again.

Fetch the Notion markdown spec once (`notion://docs/enhanced-markdown-spec`), then create a page titled `Niche Outliers — [niche] — [date]`, icon `🔥`, containing a single table:

| # | Platform | Creator | Views | Caption | Link |
|---|----------|---------|-------|---------|------|

- **Views** formatted readable (e.g. `2.4M`, `840K`).
- **Caption** trimmed to ~120 chars.
- **Link** as a clickable link to the post.
- Rows in rank order, highest views first.

Confirm with the page link and a one-liner: how many from each platform.

---

## Edge cases

- **No niche given** — ask for one before scraping.
- **Apify not connected** — stop and say the Apify MCP needs connecting (with an API token).
- **Notion not connected** — show the ranked top 10 in chat instead, and say Notion isn't connected.
- **A platform returns nothing** — continue with the other, note it on the page.
- **Niche too broad/empty** — if results look off-niche, tighten the hashtags and re-run rather than filing noise.
- **Never invent view counts or videos.** Only list what the scrapers actually returned. If a count is missing, show "—", don't guess.

# Setup

Niche Outliers runs inside **Claude Code** and uses two connectors: **Apify** (to scrape Instagram + TikTok) and **Notion** (to write your report page). Both are already wired into this repo — you just approve them and sign in once.

About 5 minutes, start to finish.

---

## 1. Install Claude Code

If you don't have it yet, install it from https://claude.com/claude-code and sign in with your Claude account.

> This skill does **not** run in the claude.ai web app — it needs to scrape, and only Claude Code can do that.

## 2. Get the repo

```
git clone https://github.com/bradford-marais/niche-outliers.git
cd niche-outliers
claude
```

## 3. Approve the connectors

The first time Claude Code opens in this folder, it asks:

> *"This project wants to use MCP servers: apify, notion. Approve?"*

Say **yes**. (That's what the `.mcp.json` in this repo is for — you don't have to wire anything by hand.)

## 4. Sign in to each connector

Type `/mcp` and press Enter. You'll see `apify` and `notion` listed.

- **Apify** — usually connects on its own; the free tier covers a top-10 pull. If it asks, select it → **Authenticate** → approve in the browser. (Free account at apify.com → Settings → Integrations.)
- **Notion** — select `notion` → **Authenticate** → a browser opens to Notion's "allow access" screen → approve, and pick the workspace/page you want reports written to.

When both show `✓ connected`, you're set.

## 5. Run it

Just name a niche:

> "Find outliers in luxury real estate"

On the first run it asks which Notion page to file reports under, then remembers it for next time.

---

## If something won't connect

- **Apify stuck on "needs authentication"** — run `/mcp`, select `apify`, **Authenticate**. On a paid Apify plan you can use your own token instead:
  ```
  claude mcp add --scope project --transport http apify https://mcp.apify.com --header "Authorization: Bearer YOUR_APIFY_TOKEN"
  ```
- **Notion won't authorize** — re-run `/mcp` → `notion` → **Authenticate**, and make sure you select a workspace and grant page access on the consent screen. If the browser flow errors, sign out of Notion in your browser and retry.
- Either connector not connected = the skill can't run. It will tell you which one is missing before it does anything.

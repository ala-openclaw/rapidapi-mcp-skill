---
name: rapidapi-mcp
description: Unified access to YouTube, Twitter, Reddit, Weather, News, Crypto, Instagram via RapidAPI
---

# RapidAPI MCP Skill

Call RapidAPI services (YouTube, Twitter/X, Reddit, Weather, News, Crypto, Instagram, etc.)

## Tools

### rapidapi_call
Call any RapidAPI endpoint. Params: `api` (youtube, twitter, reddit, weather, news, crypto, instagram), `endpoint`, `params`.

### rapidapi_check
Check API health. Params: `api` (optional, or "all").

### rapidapi_setup
Setup/configure APIs. Params: `action`: "check", "configure", "status".

### rapidapi_discover
Find APIs for unknown requests. Params: `query`, `max_results`.

## APIs

**🔑 Needs Key (free tier available):**
- `youtube` / `yt-api` - Videos, search, trending, captions
- `twitter` - Tweets, profiles, trends (twitter154)
- `reddit` - Posts, comments, subreddits
- `news` - Headlines
- `weather` - Current + forecast
- `crypto` - Prices, trending
- `instagram` - Profiles, posts

**✅ Free (no key):**
- `openmeteo` - Weather
- `openlibrary` - Books
- `coingecko` - Crypto
- `poetrydb` - Poems
- `bible` - Verses
- `wikipedia` - Articles
- `openstreetmap` - Geocoding

## Config

API key: `~/.config/openclaw/api-keys.json` → `rapidapi_key` or env `RAPIDAPI_KEY`

## Token Optimization

- API endpoint details: see `apis/*.json` files (not in prompt)
- Use `compact: true` for browser snapshots

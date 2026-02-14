# RapidAPI MCP Skill

**Version:** 2.0.0  
**Last Updated:** 2026-02-14  
**Author:** OpenClaw  
**Description:** Unified skill for accessing multiple RapidAPI services with modular architecture

---

## Overview

This skill provides unified access to multiple RapidAPI-powered services. Each API is configured separately in the `apis/` directory, allowing for easy addition, removal, and updates of APIs without modifying the core skill logic.

### Features

- **Modular Architecture**: Each API has its own config file
- **API Selector**: Automatic routing to the correct API based on user intent
- **Health Check**: Test all APIs with a single command
- **Subscription Detection**: Automatic detection of subscription requirements
- **API Discovery**: Find and suggest APIs for unknown requests
- **Smart Selection**: Choose best API based on relevance, popularity, and free tier

---

## Setup

### Best Practices for YouTube APIs

**Speed Optimization:**
1. **Try 'en' first** - Most videos have English subtitles. Skip language-list check.
2. **Use response_mode=url** - Faster response, less bandwidth
3. **Parallel requests** - For multiple videos, use async/parallel curl
4. **Retry logic** - Implement 1-2 retries with backoff for slow API responses

**Error Handling:**
- 404 → Video not found or no subtitles. Try different language.
- 403 → Restricted content (private, geo-blocked)

### Quick Start (Free APIs - Work Immediately!)

These APIs work without any API key:

```bash
# Create config directory
mkdir -p ~/.config/openclaw

# Test Open-Meteo (Weather)
curl "https://api.open-meteo.com/v1/forecast?latitude=52.5&longitude=13.4&current_weather=true"

# Test OpenLibrary (Books)
curl "https://openlibrary.org/search.json?q=python"

# Test PoetryDB
curl "https://poetrydb.org/title/love"

# Test Gutendex (Free ebooks)
curl "https://gutendex.com/books?search=alice"

# Test Bible
curl "https://bible-api.com/john+3:16"
```

### Adding Your RapidAPI Key (Optional)

For premium APIs (YouTube, Twitter, Reddit, etc.), add your RapidAPI key:

1. Get a free key at https://rapidapi.com/auth/signup
2. Create config:

```bash
mkdir -p ~/.config/openclaw
echo '{"rapidapi_key": "YOUR_KEY_HERE"}' > ~/.config/openclaw/api-keys.json
```

Or set as environment variable:
```bash
export RAPIDAPI_KEY="YOUR_KEY_HERE"
```

```bash
# Run setup script to check all APIs
openclaw skill run rapidapi-mcp --setup
```

---

## API Selector Prompt

When the user requests data from a specific service, use this prompt to route to the correct API:

> **"Which API do you need?"**
> 
> **🌟 FREE (work without key):**
> - Open-Meteo → `openmeteo` - Free global weather (best!)
> - Open Library → `openlibrary` - Book search by title, author, ISBN
> - Gutendex → `gutendex` - Project Gutenberg books
> - PoetryDB → `poetrydb` - Poems and poetry
> - Bible → `bible` - Holy Bible verses
> - CoinGecko → `coingecko` - Free crypto prices
> 
> **🔑 NEEDS RAPIDAPI KEY:**
> - YouTube → `youtube` - Videos, channels, trending
> - YouTube (YT-API) → `yt-api` - Extended YouTube API with download, comments, hashtags, suggestions
> - YouTube Captions → `youtube-captions` - Captions, transcripts, subtitles in multiple languages
> - Twitter/X → `twitter` - Tweets, profiles, trends
> - Reddit → `reddit` - Posts, comments, subreddits
> - News → `news` - Latest headlines
> - Weather → `weather` - Current weather, forecasts
> - Streaming → `streaming` - Where to watch movies/shows
> - Instagram → `instagram` - Profiles, posts, stories
> - Wikipedia → `wikipedia` - Wikipedia articles
> - OpenStreetMap → `openstreetmap` - Geocoding, maps
> - Wikidata → `wikidata` - Structured facts
> - Archive → `archive` - Internet Archive
> - Harry Potter → `harrypotter` - HP characters, spells
> - Nobel Prize → `nobel` - Nobel laureates

---

## Available Tools

### 1. rapidapi_call

Call any RapidAPI endpoint with automatic routing.

**Parameters:**
- `api` (string): API name from config (youtube, twitter, reddit, news, weather, streaming, crypto, instagram)
- `endpoint` (string): Endpoint name as defined in API config
- `params` (object): Query parameters for the endpoint

**Example:**
```json
{
  "api": "youtube",
  "endpoint": "search",
  "params": {
    "query": "openai",
    "maxResults": 5
  }
}
```

### 2. rapidapi_check

Check health/subscription status of all configured APIs.

**Parameters:**
- `api` (string, optional): Specific API to check, or "all" for all APIs

**Example:**
```json
{
  "api": "all"
}
```

### 3. rapidapi_setup

Interactive setup to configure API subscriptions.

**Parameters:**
- `action` (string): "check", "configure", or "status"

**Example:**
```json
{
  "action": "check"
}
```

### 4. rapidapi_discover

Find APIs for unknown requests. Searches RapidAPI for relevant APIs when no known API is configured.

**Parameters:**
- `query` (string): What the user wants to do (e.g., "weather", "reddit", "news")
- `max_results` (number): Maximum number of APIs to return (default: 5)

**Example:**
```json
{
  "query": "weather forecast",
  "max_results": 5
}
```

**Selection Logic:**
1. Check if API is already configured → use it
2. If not → search RapidAPI
3. Rank results by: Relevance → Free Tier → Popularity
4. Return top 3 with subscription links

---

## Token Optimization for Browser Scraping

When scraping RapidAPI documentation pages via browser, use these optimizations to reduce token usage:

### 1. Use `compact: true` for Snapshots

```json
{
  "action": "snapshot",
  "compact": true,
  "profile": "chrome"
}
```

**Result:** Reduces ~1.5k tokens → ~200 tokens per snapshot

### 2. Use Screenshot for Visual Parameters

When you only need to see parameter values visually:

```json
{
  "action": "screenshot",
  "profile": "chrome"
}
```

### 3. Selective Element Access

Instead of full page snapshots, use targeted selectors when possible.

---

## Available APIs

### IMDb146
- **Host:** `imdb146.p.rapidapi.com`
- **Description:** Movie database - titles, names, videos, news
- **Documentation:** https://rapidapi.com/Glavier/api/imdb146

| Endpoint | Method | URL | Parameters |
|----------|--------|-----|------------|
| Find | GET | `/v1/find/` | `query` (string, required) |
| Title Details | GET | `/v1/title/` | `id` (string, required) - Title ID |
| Name Details | GET | `/v1/name/` | `id` (string, required) - Name ID |
| Video Details | GET | `/v1/video/` | `id` (string, required) - Video ID |
| News Details | GET | `/v1/news/` | `id` (string, required) - News ID |

### YouTube (YT-API)
- **Host:** yt-api.p.rapidapi.com
- **Description:** YouTube Data + Download API - videos, channels, search, trending, playlists, comments, download URLs
- **Documentation:** https://rapidapi.com/ytjar/api/yt-api
- **Pricing:** Basic (Free), Pro ($51/mo), Ultra ($144/mo), Mega ($240/mo)

| Endpoint | Method | Path | Parameters |
|----------|--------|------|------------|
| Video Info | GET | `/video` | `id` (required), `pretty`, `geo`, `lang` |
| Related Videos | GET | `/related` | `id` (required), `token`, `pretty`, `geo`, `lang` |
| Trending | GET | `/trending` | `geo`, `type` (now/music/games/movies), `pretty`, `lang` |
| Search | GET | `/search` | `query` (required), `type`, `sort`, `duration`, `upload_date`, `features`, `token`, `pretty`, `geo`, `lang` |
| Playlist | GET | `/playlist` | `id` (required), `token`, `pretty`, `geo`, `lang` |
| Channel | GET | `/channel` | `id` (required), `sort_by` (newest/oldest/popular), `token`, `pretty`, `geo`, `lang` |
| Comments | GET | `/comments` | `id` (required), `token`, `pretty`, `geo`, `lang` |
| Hashtag | GET | `/hashtag` | `id` (required), `pretty`, `geo`, `lang` |
| Resolve URL | GET | `/resolve` | `url` (required), `pretty` |
| Download/Stream | GET | `/dl` | `id` (required), `cgeo`, `pretty` (quota: 6 units) |
| Home Feed | GET | `/home` | `pretty`, `geo`, `lang` |
| Suggestions | GET | `/suggest` | `query` (required), `pretty` |
| Hype | GET | `/hype` | `pretty`, `geo`, `lang` |

### YouTube Captions
- **Host:** youtube-captions-transcript-subtitles-video-combiner.p.rapidapi.com
- **Description:** YouTube Captions, Transcripts, Subtitles - Download in SRT, VTT, XML, JSON; translate to any language
- **Documentation:** https://rapidapi.com/nikzeferis/api/youtube-captions-transcript-subtitles-video-combiner
- **Pricing:** Basic (Free), Pro ($3.87/mo), Ultra ($13.87/mo), Mega ($34.98/mo)

| Endpoint | Method | Path | Parameters |
|----------|--------|------|------------|
| Video Info | GET | `/get-video-info/{videoId}` | `videoId` (required), `format`, `response_mode` |
| Language List | GET | `/language-list/{videoId}` | `videoId` (required), `format`, `response_mode` |
| Download SRT | GET | `/download-srt/{videoId}` | `videoId`, `language` (required), `response_mode` |
| Download XML | GET | `/download-xml/{videoId}` | `videoId`, `language` (required), `response_mode` |
| Download JSON | GET | `/download-json/{videoId}` | `videoId`, `language` (required), `response_mode` |
| Download WebVTT | GET | `/download-webvtt/{videoId}` | `videoId`, `language` (required), `response_mode` |
| Download All | GET | `/download-all/{videoId}` | `videoId`, `format_subtitle`, `format_answer`, `response_mode` |

### YouTube
- **Host:** youtube-v31.p.rapidapi.com
- **Endpoints:** search, videoDetails, channelDetails, trending
- **Use Case:** Video search, channel info, trending content

### Twitter/X
- **Host:** twitter135.p.rapidapi.com
- **Endpoints:** userTweets, userByUsername, search, trends
- **Use Case:** Tweet retrieval, user profiles, trending topics

### Reddit
- **Host:** reddit23.p.rapidapi.com
- **Endpoints:** subredditPosts, postComments, subredditSearch, trendingSubreddits
- **Use Case:** Reddit content, subreddit browsing, comments

### News
- **Host:** newsdata.io
- **Endpoints:** latestNews, newsArchive, sources
- **Use Case:** Headlines, article search, news sources

### Weather
- **Host:** weatherapi-com.p.rapidapi.com
- **Endpoints:** currentWeather, forecast, history, astronomy
- **Use Case:** Weather data, forecasts, astronomy info

### Streaming
- **Host:** streaming-availability.p.rapidapi.com
- **Endpoints:** search, getDetails, getCountries, getLanguages
- **Use Case:** Find where to stream movies/shows

### Crypto
- **Host:** coinranking1.p.rapidapi.com
- **Endpoints:** coins, coinDetails, coinHistory, exchanges, trending
- **Use Case:** Cryptocurrency prices, market data

### Instagram
- **Host:** instagram120.p.rapidapi.com
- **Endpoints:** userProfile, userPosts, hashtagSearch, mediaDetails
- **Use Case:** Instagram profiles and media retrieval

### Books
- **API:** Google Books (free, no key needed)
- **Endpoints:** search, by_isbn
- **Use Case:** German books, by genre, date, ISBN

### Wikipedia
- **API:** de.wikipedia.org (free, no key needed)
- **Endpoints:** search, page_content, page_details
- **Use Case:** German Wikipedia articles, search

### OpenStreetMap
- **API:** nominatim.openstreetmap.org (free, no key)
- **Endpoints:** search, reverse, lookup
- **Use Case:** Geocoding, location search

### Open Library
- **API:** openlibrary.org (free, no key)
- **Endpoints:** search, book_by_isbn, author, cover
- **Use Case:** Book search, ISBN lookup, covers

### Wikidata
- **API:** wikidata.org (free, no key needed)
- **Endpoints:** search, get_entity, sparql
- **Use Case:** Structured facts, entities

### Archive
- **API:** archive.org (free, no key needed)
- **Endpoints:** search, metadata
- **Use Case:** Historical books, media, web archives

### CoinGecko
- **API:** api.coingecko.com (free, no key needed)
- **Endpoints:** coins, coin_detail, trending, global, simple_price, search
- **Use Case:** Cryptocurrency prices, market data, trending

### Open-Meteo
- **API:** api.open-meteo.com (free, no key needed)
- **Endpoints:** forecast
- **Use Case:** Global weather, forecasts

### Gutendex
- **API:** gutendex.com (free, no key needed)
- **Endpoints:** books, book_by_id
- **Use Case:** Project Gutenberg free books

### PoetryDB
- **API:** poetrydb.org (free, no key needed)
- **Endpoints:** random, author, title, search
- **Use Case:** Poems, poets

### Harry Potter API
- **API:** potterapi-fedeperin.vercel.app (free, no key needed)
- **Endpoints:** books, characters, spells, houses
- **Use Case:** Harry Potter data

### Bible
- **API:** bible-api.com (free, no key needed)
- **Endpoints:** verse, random
- **Use Case:** Bible verses

### Nobel Prize
- **API:** api.nobelprize.org (free, no key needed)
- **Endpoints:** prizes, laureates
- **Use Case:** Nobel Prize data

---

## API Configuration Files

All API configurations are stored in `/opt/homebrew/lib/node_modules/openclaw/skills/rapidapi-mcp/apis/`:

```
apis/
├── youtube.json
├── twitter.json
├── reddit.json
├── news.json
├── weather.json
├── streaming.json
├── crypto.json
└── instagram.json
```

Each config file contains:
- `name`: Display name
- `host`: RapidAPI host
- `description`: What the API does
- `rapidapi_key`: API key for authentication
- `endpoints`: Available endpoints with paths and params
- `subscription_url_pattern`: URL pattern for subscription

## API Selection Criteria

When recommending APIs, use this priority order:

1. **Relevanz** - Does the API do what the user needs?
2. **Free Tier** - How many calls are free/month?
3. **Popularität** - How many users? (proxy for reliability)
4. **Rate Limits** - Enough for normal use case?

### API Discovery Flow

```
User Request → Check known APIs → Not found? 
→ Search RapidAPI → Rank by criteria → User chooses
→ Subscribe → Ready to use
```

---

## Example Usage

### Key Loading Order
When making API calls, the skill loads the key in this order:
1. `~/.config/openclaw/api-keys.json` → `rapidapi_key`
2. Environment variable `RAPIDAPI_KEY`
3. Config file value (should be `null`)

### Search YouTube for "AI tutorials"
```
api: rapidapi_call
params: {
  "api": "youtube",
  "endpoint": "search",
  "params": {
    "query": "AI tutorials",
    "maxResults": 10
  }
}
```

### Get current weather for Berlin
```
api: rapidapi_call
params: {
  "api": "weather",
  "endpoint": "currentWeather",
  "params": {
    "q": "Berlin"
  }
}
```

### Get trending crypto
```
api: rapidapi_call
params: {
  "api": "crypto",
  "endpoint": "trending",
  "params": {}
}
```

### Check all API subscriptions
```
api: rapidapi_check
params: {
  "api": "all"
}
```

---

## Subscription Management

### How Subscriptions Work

1. Most RapidAPI endpoints require a subscription
2. Free tier: Limited requests/month
3. Paid tier: Higher limits, more endpoints
4. Each API has a `subscription_url_pattern` pointing to its RapidAPI page

### Check Subscription Status

```bash
# Check all APIs
openclaw skill run rapidapi-mcp --check

# Check specific API
openclaw skill run rapidapi-mcp --check youtube
```

### Subscribe to an API

1. Run the check command to see which APIs need subscriptions
2. Visit the subscription URL from the API config
3. Select a plan (Free or Paid)
4. Update the API key in the config if needed

---

## Error Handling

### Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| 401 Unauthorized | Invalid API key | Update key in config |
| 403 Forbidden | Subscription required | Subscribe on RapidAPI |
| 429 Too Many Requests | Rate limit exceeded | Wait or upgrade plan |
| 404 Not Found | Invalid endpoint | Check endpoint name |

### Best Practices

1. Check API health before making calls
2. Handle rate limits gracefully
3. Cache responses when possible
4. Use specific endpoints instead of generic ones

---

## File Structure

```
rapidapi-mcp/
├── SKILL.md              # This file
├── README.md             # Original documentation
├── setup.sh              # Setup and check script
└── apis/
    ├── youtube.json
    ├── twitter.json
    ├── reddit.json
    ├── news.json
    ├── weather.json
    ├── streaming.json
    ├── crypto.json
    └── instagram.json
```

---

## Notes

- This skill uses the RapidAPI key from: `~/.config/openclaw/api-keys.json` or environment variable `RAPIDAPI_KEY`
- Some APIs may require additional RapidAPI subscriptions
- Free tier limits apply to most APIs
- Check individual API docs for rate limits and capabilities

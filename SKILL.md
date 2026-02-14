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

### Prerequisites

1. A RapidAPI account (https://rapidapi.com)
2. API keys for desired services

> ⚠️ **SECURITY**: Never hardcode API keys in config files or SKILL.md. Use environment variables or local config. All `apis/*.json` files have `rapidapi_key: null` by default — set your key there or via env var.

### Quick Start

```bash
# Run setup script to check all APIs
openclaw skill run rapidapi-mcp --setup
```

---

## API Selector Prompt

When the user requests data from a specific service, use this prompt to route to the correct API:

> **"Which API do you need?"**
> 
> - YouTube → `youtube` - Videos, channels, trending
> - Twitter/X → `twitter` - Tweets, profiles, trends
> - Reddit → `reddit` - Posts, comments, subreddits
> - News → `news` - Latest headlines, articles
> - Weather → `weather` - Current weather, forecasts
> - Streaming → `streaming` - Where to watch movies/shows
> - Crypto → `crypto` - Prices, market data
> - Instagram → `instagram` - Profiles, posts, stories
- Books → `books` - Search German books, by genre, date
- Wikipedia → `wikipedia` - German Wikipedia articles and search
- OpenStreetMap → `openstreetmap` - Geocoding, locations, maps
- Open Library → `openlibrary` - Book search by title, author, ISBN
- Wikidata → `wikidata` - Structured facts and entities
- Archive → `archive` - Internet Archive (books, media, web archives)
- CoinGecko → `coingecko` - Free Crypto data (prices, trending, market cap)
- Open-Meteo → `openmeteo` - Free global weather (best!)
- Gutendex → `gutendex` - Project Gutenberg books
- PoetryDB → `poetrydb` - Poems and poetry
- Harry Potter → `harrypotter` - HP books, characters, spells
- Bible → `bible` - Holy Bible verses
- Nobel Prize → `nobel` - Nobel laureates and prizes

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

## Available APIs

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

- This skill uses the shared RapidAPI key: `d95585deebmsh45ad264ed685814p11f522jsn6cbbfaaae2da`
- Some APIs may require additional RapidAPI subscriptions
- Free tier limits apply to most APIs
- Check individual API docs for rate limits and capabilities

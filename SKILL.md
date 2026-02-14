---
name: rapidapi-mcp
description: "Template skill for RapidAPI MCP integration. Access Twitter, YouTube, and other APIs via JSON-RPC."
metadata:
  {
    "openclaw":
      {
        "emoji": "🐦",
        "requires": { "bins": ["curl"] },
      },
  }
---

# rapidapi-mcp

Template skill for accessing RapidAPI MCP servers via direct JSON-RPC calls.

## Quick Start

### 1. Get Your RapidAPI Key

1. Go to [RapidAPI](https://rapidapi.com)
2. Create account and subscribe to desired APIs
3. Get your API key from dashboard

### 2. Enable MCP

1. Open the API in **Playground**
2. Click **MCP button** to enable
3. Copy the configuration shown

### 3. Use Direct JSON-RPC

**Important:** Use `https://mcp.rapidapi.com/` (NOT `/jsonrpc`)

```bash
# Variables
URL="https://mcp.rapidapi.com/"
HOST="YOUR_HOST.p.rapidapi.com"  # e.g., twitter241.p.rapidapi.com
KEY="your-rapidapi-key"
```

## Common API Hosts

| API | Host |
|-----|------|
| Twitter/X | `twitter241.p.rapidapi.com` |
| YouTube | `yt-api.p.rapidapi.com` |
| YouTube Captions | `youtube-captions-transcript-subtitles-video-combiner.p.rapidapi.com` |

## Core Commands

### List Available Tools

```bash
curl -s -X POST "$URL" \
  -H "Content-Type: application/json" \
  -H "x-api-host: $HOST" \
  -H "x-api-key: $KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/list","params":{},"id":1}'
```

### Call a Tool

```bash
curl -s -X POST "$URL" \
  -H "Content-Type: application/json" \
  -H "x-api-host: $HOST" \
  -H "x-api-key: $KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"TOOL_NAME","arguments":{"param":"value"}},"id":1}'
```

## Practical Workflows

### YouTube: Search Videos

```bash
curl -s -X POST "$URL" \
  -H "Content-Type: application/json" \
  -H "x-api-host: yt-api.p.rapidapi.com" \
  -H "x-api-key: $KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"Search","arguments":{"query":"python tutorial","type":"video","geo":"DE"}},"id":1}'
```

### YouTube: Get Video Details + Transcript

```bash
# 1. Get video details
curl -s -X POST "$URL" \
  -H "Content-Type: application/json" \
  -H "x-api-host: yt-api.p.rapidapi.com" \
  -H "x-api-key: $KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"Video_Details","arguments":{"id":"VIDEO_ID"}},"id":1}'

# 2. Get transcript
curl -s -X POST "$URL" \
  -H "Content-Type: application/json" \
  -H "x-api-host: yt-api.p.rapidapi.com" \
  -H "x-api-key: $KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"Transcript","arguments":{"id":"VIDEO_ID"}},"id":1}'
```

### YouTube: Get Captions in Specific Language

```bash
# 1. Check available languages
curl -s -X POST "$URL" \
  -H "Content-Type: application/json" \
  -H "x-api-host: youtube-captions-transcript-subtitles-video-combiner.p.rapidapi.com" \
  -H "x-api-key: $KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"Get_Available_Languages","arguments":{"videoid":"VIDEO_ID"}},"id":1}'

# 2. Download SRT in desired language
curl -s -X POST "$URL" \
  -H "Content-Type: application/json" \
  -H "x-api-host: youtube-captions-transcript-subtitles-video-combiner.p.rapidapi.com" \
  -H "x-api-key: $KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"Download_subtitle_in_SRT_format","arguments":{"videoid":"VIDEO_ID","language":"de"}},"id":1}'
```

### YouTube: Get Channel Videos

```bash
curl -s -X POST "$URL" \
  -H "Content-Type: application/json" \
  -H "x-api-host: yt-api.p.rapidapi.com" \
  -H "x-api-key: $KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"Channel_Videos","arguments":{"id":"CHANNEL_ID","sort_by":"newest"}},"id":1}'
```

### YouTube: Trending in Germany

```bash
curl -s -X POST "$URL" \
  -H "Content-Type: application/json" \
  -H "x-api-host: yt-api.p.rapidapi.com" \
  -H "x-api-key: $KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"Trending","arguments":{"geo":"DE"}},"id":1}'
```

---

### Twitter: Get User Profile + Tweets

```bash
# 1. Get user ID (rest_id)
curl -s -X POST "$URL" \
  -H "Content-Type: application/json" \
  -H "x-api-host: twitter241.p.rapidapi.com" \
  -H "x-api-key: $KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"Get_User_By_Username","arguments":{"username":"MrBeast"}},"id":1}'

# 2. Use rest_id to get tweets
curl -s -X POST "$URL" \
  -H "Content-Type: application/json" \
  -H "x-api-host: twitter241.p.rapidapi.com" \
  -H "x-api-key: $KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"Get_User_Tweets","arguments":{"user":"REST_ID","count":10}},"id":1}'
```

### Twitter: Search Tweets

```bash
curl -s -X POST "$URL" \
  -H "Content-Type: application/json" \
  -H "x-api-host: twitter241.p.rapidapi.com" \
  -H "x-api-key: $KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"Search_Twitter","arguments":{"query":"AI","type":"Latest","count":20}},"id":1}'
```

### Twitter: Get Trending Topics

```bash
# Find WOEID for location
curl -s -X POST "$URL" \
  -H "Content-Type: application/json" \
  -H "x-api-host: twitter241.p.rapidapi.com" \
  -H "x-api-key: $KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"Get_Available_Trends_Locations","arguments":{}},"id":1}'

# Get trends for that WOEID (e.g., 638242 = Berlin)
curl -s -X POST "$URL" \
  -H "Content-Type: application/json" \
  -H "x-api-host: twitter241.p.rapidapi.com" \
  -H "x-api-key: $KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"Get_Trends_By_Location","arguments":{"woeid":"638242"}},"id":1}'
```

### Twitter: Get Tweet Details

```bash
curl -s -X POST "$URL" \
  -H "Content-Type: application/json" \
  -H "x-api-host: twitter241.p.rapidapi.com" \
  -H "x-api-key: $KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"Get_Tweet_Details_V2","arguments":{"pid":"TWEET_ID"}},"id":1}'
```

## Important Patterns

### 1. Chained Calls (Twitter)

Many Twitter endpoints require the user's `rest_id`, not the username:
1. Call `Get_User_By_Username` → get `rest_id`
2. Use `rest_id` for `Get_User_Tweets`, `Get_User_Followers`, etc.

### 2. Pagination

For endpoints with many results, use `cursor` parameter to paginate:
```bash
-d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"Tool_Name","arguments":{"cursor":"NEXT_CURSOR"}},"id":1}'
```

### 3. Response Modes (YouTube Captions)

Some endpoints support `response_mode`:
- `default` – returns the data directly
- `url` – returns a URL to fetch the result

### 4. Fields Parameter (YouTube)

Use `fields` to reduce bandwidth and get only what you need:
```bash
-d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"Video_Details","arguments":{"id":"VIDEO_ID","fields":"title,description,likeCount"}},"id":1}'
```

## Mcporter Integration

```bash
mcporter config add "my-api" --command npx --args '["mcp-remote","https://mcp.rapidapi.com","--header","x-api-host: YOUR_HOST.p.rapidapi.com","--header","x-api-key: YOUR_KEY"]'
```

## Auto-Documentation

After setting up a new API, save the tools to memory:

```bash
# List tools
TOOLS=$(curl -s -X POST "$URL" \
  -H "Content-Type: application/json" \
  -H "x-api-host: $HOST" \
  -H "x-api-key: $KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/list","params":{},"id":1}')

# Save to memory/rapidapi.md
echo "## NEW_API" >> memory/rapidapi.md
echo "**Host:** $HOST" >> memory/rapidapi.md
# ... add tool names and descriptions
```

## Notes

- Use POST to `https://mcp.rapidapi.com/` (NOT `/jsonrpc`)
- Headers required: `x-api-host` and `x-api-key`
- Enable MCP in RapidAPI Playground first (click MCP button)
- Check `memory/rapidapi.md` for documented APIs and tools

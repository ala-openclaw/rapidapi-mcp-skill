---
name: rapidapi-mcp
description: "Template skill for RapidAPI MCP integration. Add any RapidAPI endpoint (Twitter, YouTube, etc.) via JSON-RPC."
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

Template skill for accessing RapidAPI MCP servers via direct JSON-RPC calls. Use this to add any RapidAPI endpoint.

## Setup

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

# Initialize
curl -s -X POST "$URL" \
  -H "Content-Type: application/json" \
  -H "x-api-host: $HOST" \
  -H "x-api-key: $KEY" \
  -d '{"jsonrpc":"2.0","method":"initialize","params":{"protocolVersion":"2024-11-05","capabilities":{},"clientInfo":{"name":"openclaw","version":"1.0"}},"id":0}'
```

## Call a Tool

```bash
curl -s -X POST "https://mcp.rapidapi.com/" \
  -H "Content-Type: application/json" \
  -H "x-api-host: YOUR_HOST.p.rapidapi.com" \
  -H "x-api-key: YOUR_KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"TOOL_NAME","arguments":{"param":"value"}},"id":1}'
```

## List Available Tools

```bash
curl -s -X POST "https://mcp.rapidapi.com/" \
  -H "Content-Type: application/json" \
  -H "x-api-host: YOUR_HOST.p.rapidapi.com" \
  -H "x-api-key: YOUR_KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/list","params":{},"id":1}'
```

## Common API Hosts

| API | Host |
|-----|------|
| Twitter/X | `twitter241.p.rapidapi.com` |
| YouTube | `yt-api.p.rapidapi.com` |
| Streaming | `streaming-availability.p.rapidapi.com` |

## Example: Twitter

```bash
# Get user
curl -s -X POST "https://mcp.rapidapi.com/" \
  -H "Content-Type: application/json" \
  -H "x-api-host: twitter241.p.rapidapi.com" \
  -H "x-api-key: YOUR_KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"Get_User_By_Username","arguments":{"username":"MrBeast"}},"id":1}'
```

## Mcporter Alternative

```bash
mcporter config add "my-api" --command npx --args '["mcp-remote","https://mcp.rapidapi.com","--header","x-api-host: YOUR_HOST.p.rapidapi.com","--header","x-api-key: YOUR_KEY"]'
```

## Notes

- Use POST to `https://mcp.rapidapi.com/` (NOT `/jsonrpc`)
- Headers: `x-api-host` and `x-api-key`
- Enable MCP in RapidAPI Playground first (click MCP button)

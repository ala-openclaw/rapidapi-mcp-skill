---
name: rapidapi-mcp
description: Access RapidAPI MCP servers (Twitter, YouTube, etc.) via direct JSON-RPC calls
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

Access RapidAPI MCP servers directly via JSON-RPC. This bypasses mcporter's stdio issues.

## Usage

Call RapidAPI MCP endpoints using direct curl:

```bash
# Base URL (no /jsonrpc suffix!)
URL="https://mcp.rapidapi.com/"

# Headers
HOST="twitter241.p.rapidapi.com"  # or yt-api.p.rapidapi.com, etc.
KEY="your-rapidapi-key"

# JSON-RPC call
curl -s -X POST "$URL" \
  -H "Content-Type: application/json" \
  -H "x-api-host: $HOST" \
  -H "x-api-key: $KEY" \
  -d '{"jsonrpc":"2.0","method":"initialize","params":{"protocolVersion":"2024-11-05","capabilities":{},"clientInfo":{"name":"openclaw","version":"1.0"}},"id":0}'
```

## Available Tools

### Twitter (twitter241.p.rapidapi.com)

- `Get_User_By_Username` - Get user by username
- `Get_User_Tweets` - Get user's tweets
- `Search_Twitter` / `Search_Twitter_V2` / `Search_Twitter_V3` - Search tweets
- `Get_Tweet_Details_V2` - Get tweet by ID
- `Get_User_Followers` / `Get_User_Followings` - Get followers/following
- `Get_Post_Likes_Deprecated` / `Get_Post_Comments` - Get likes/comments
- `Get_Trends_By_Location` - Get trending topics
- `Get_Community_*` - Community features

### YouTube (yt-api.p.rapidapi.com)

Check available endpoints via `tools/list`

## Example: Get User

```bash
curl -s -X POST "https://mcp.rapidapi.com/" \
  -H "Content-Type: application/json" \
  -H "x-api-host: twitter241.p.rapidapi.com" \
  -H "x-api-key: YOUR_KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"Get_User_By_Username","arguments":{"username":"MrBeast"}},"id":1}'
```

## MCP Config Format

For mcporter (if stdio works):

```json
{
  "mcpServers": {
    "RapidAPI Hub - Twitter": {
      "command": "npx",
      "args": ["mcp-remote", "https://mcp.rapidapi.com", "--header", "x-api-host: twitter241.p.rapidapi.com", "--header", "x-api-key: YOUR_KEY"]
    }
  }
}
```

## Notes

- Use POST to `https://mcp.rapidapi.com/` (NOT `/jsonrpc`)
- Headers: `x-api-host` and `x-api-key`
- Enable MCP in RapidAPI Playground first (click MCP button)

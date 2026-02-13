# RapidAPI MCP Skill for OpenClaw

> AI-powered integration for accessing RapidAPI endpoints via Model Context Protocol (MCP)

[![OpenClaw Skill](https://img.shields.io/badge/OpenClaw-Skill-blue.svg)](https://openclaw.ai)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

## Overview

This skill enables OpenClaw AI assistants to directly interact with RapidAPI's MCP (Model Context Protocol) servers. Access Twitter/X API, YouTube Data API, and other RapidAPI services through natural language queries.

## Features

- **Direct JSON-RPC Access** - Bypass mcporter stdio issues with direct HTTP calls
- **Twitter/X Integration** - Search tweets, get user data, retrieve trends
- **YouTube Data API** - Access video metadata, channels, search
- **Multiple API Support** - Works with any RapidAPI MCP-enabled endpoint
- **OpenClaw Compatible** - Seamlessly integrates with OpenClaw AI assistant

## Installation

```bash
# Install via OpenClaw skill system
# The skill will be automatically discovered on restart
```

## Quick Start

### Configure Your API Key

Set your RapidAPI key as an environment variable or pass it directly:

```bash
export RAPIDAPI_KEY="your-rapidapi-key"
```

### Basic Usage

#### Get Twitter User Information

```bash
# Direct JSON-RPC call
curl -s -X POST "https://mcp.rapidapi.com/" \
  -H "Content-Type: application/json" \
  -H "x-api-host: twitter241.p.rapidapi.com" \
  -H "x-api-key: $RAPIDAPI_KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"Get_User_By_Username","arguments":{"username":"MrBeast"}},"id":1}'
```

#### Search Tweets

```bash
curl -s -X POST "https://mcp.rapidapi.com/" \
  -H "Content-Type: application/json" \
  -H "x-api-host: twitter241.p.rapidapi.com" \
  -H "x-api-key: $RAPIDAPI_KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"Search_Twitter_V2","arguments":{"query":"AI","type":"Top","count":10}},"id":1}'
```

## Available Twitter Tools

| Tool | Description |
|------|-------------|
| `Get_User_By_Username` | Get user profile by username |
| `Get_User_Tweets` | Retrieve user's tweets |
| `Search_Twitter_V2` | Search tweets (Top, Latest, People, Media) |
| `Get_Tweet_Details_V2` | Get tweet details by ID |
| `Get_User_Followers` | Get user's followers |
| `Get_User_Followings` | Get who user follows |
| `Get_Post_Likes_Deprecated` | Get likes on a tweet |
| `Get_Post_Comments` | Get comments on a tweet |
| `Get_Trends_By_Location` | Get trending topics by location |
| `Get_Community_*` | Various community features |

## Configuration

### Mcporter Config (Alternative)

```json
{
  "mcpServers": {
    "RapidAPI Hub - Twitter": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://mcp.rapidapi.com",
        "--header",
        "x-api-host: twitter241.p.rapidapi.com",
        "--header",
        "x-api-key: YOUR_KEY"
      ]
    }
  }
}
```

## Requirements

- `curl` command line tool
- RapidAPI subscription (free tier available)
- OpenClaw AI assistant

## API Keys

Get your free API key from [RapidAPI](https://rapidapi.com):

1. Create a RapidAPI account
2. Subscribe to desired APIs (Twitter, YouTube, etc.)
3. Copy your API key from the dashboard
4. Use the key in the `x-api-key` header

## Troubleshooting

### "Endpoint does not exist" Error

- Ensure MCP is enabled in RapidAPI Playground
- Click the "MCP" button in the API Playground to activate
- Verify your API key has access to the endpoint

### Authentication Issues

- Check your RapidAPI subscription is active
- Verify API key is correct (starts with `x`)
- Ensure you've subscribed to the specific API

## Use Cases

- **AI Assistants** - Add Twitter/YouTube search to your AI
- **Social Media Monitoring** - Track mentions and trends
- **Content Research** - Fetch video metadata for content ideas
- **Analytics** - Analyze tweet engagement and followers

## Related Projects

- [OpenClaw](https://github.com/openclaw/openclaw) - AI assistant framework
- [mcporter](https://mcporter.dev) - MCP server management
- [RapidAPI](https://rapidapi.com) - API Marketplace

## License

MIT License - See LICENSE file for details.

## Contributing

Contributions welcome! Please open an issue or submit a PR.

---

**Keywords:** RapidAPI, MCP, OpenClaw, Twitter API, YouTube API, AI assistant, JSON-RPC, API integration, ChatGPT, Claude, LLM tools

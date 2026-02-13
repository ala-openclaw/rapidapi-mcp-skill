# RapidAPI MCP Skill for OpenClaw

> AI-powered integration for accessing RapidAPI endpoints via Model Context Protocol (MCP)

[![OpenClaw Skill](https://img.shields.io/badge/OpenClaw-Skill-blue.svg)](https://openclaw.ai)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

## Overview

This skill helps OpenClaw AI assistants access RapidAPI's MCP (Model Context Protocol) servers. It provides a template and guide for configuring any RapidAPI MCP endpoint - not pre-configured for specific APIs.

**This is a template skill** - configure it with your own RapidAPI subscriptions.

## Features

- **Template-Based** - Add any RapidAPI MCP endpoint
- **Direct JSON-RPC Access** - Bypass mcporter stdio issues with direct HTTP calls
- **Flexible API Support** - Works with Twitter, YouTube, or any RapidAPI MCP-enabled endpoint
- **OpenClaw Compatible** - Seamlessly integrates with OpenClaw AI assistant

## Installation

1. Clone this skill to your OpenClaw skills directory
2. Configure your RapidAPI credentials (see below)
3. Restart OpenClaw

## Quick Start

### Step 1: Get Your RapidAPI Key

1. Create a [RapidAPI account](https://rapidapi.com)
2. Subscribe to desired APIs (Twitter, YouTube, etc.)
3. Copy your API key from the dashboard

### Step 2: Enable MCP in Playground

1. Go to your desired API on RapidAPI
2. Open the **Playground**
3. Test an endpoint that works
4. **Click the "MCP" button** to enable MCP access
5. Copy the MCP configuration shown

### Step 3: Configure Your API

Add your API to the skill:

```bash
# Using mcporter
mcporter config add "My-API" --command npx --args '["mcp-remote","https://mcp.rapidapi.com","--header","x-api-host: YOUR_HOST.p.rapidapi.com","--header","x-api-key: YOUR_KEY"]'
```

Or use direct JSON-RPC (recommended):

```bash
# Base URL
URL="https://mcp.rapidapi.com/"

# Your API credentials
HOST="YOUR_HOST.p.rapidapi.com"
KEY="your-rapidapi-key"

# Initialize connection
curl -s -X POST "$URL" \
  -H "Content-Type: application/json" \
  -H "x-api-host: $HOST" \
  -H "x-api-key: $KEY" \
  -d '{"jsonrpc":"2.0","method":"initialize","params":{"protocolVersion":"2024-11-05","capabilities":{},"clientInfo":{"name":"openclaw","version":"1.0"}},"id":0}'
```

### Step 4: List Available Tools

```bash
curl -s -X POST "https://mcp.rapidapi.com/" \
  -H "Content-Type: application/json" \
  -H "x-api-host: YOUR_HOST.p.rapidapi.com" \
  -H "x-api-key: YOUR_KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/list","params":{},"id":1}'
```

### Step 5: Call a Tool

```bash
curl -s -X POST "https://mcp.rapidapi.com/" \
  -H "Content-Type: application/json" \
  -H "x-api-host: YOUR_HOST.p.rapidapi.com" \
  -H "x-api-key: YOUR_KEY" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"TOOL_NAME","arguments":{"param":"value"}},"id":2}'
```

## Configuration

### Finding Your API Host

After enabling MCP in the Playground, your config will show:

```json
{
  "mcpServers": {
    "API Name": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://mcp.rapidapi.com",
        "--header",
        "x-api-host: YOUR_HOST.p.rapidapi.com",
        "--header",
        "x-api-key: YOUR_KEY"
      ]
    }
  }
}
```

The `YOUR_HOST.p.rapidapi.com` part is your API host.

### Using with Mcporter

```bash
# Add your API
mcporter config add "My-Twitter-API" \
  --command npx --args '["mcp-remote","https://mcp.rapidapi.com","--header","x-api-host: twitter241.p.rapidapi.com","--header","x-api-key: YOUR_KEY"]'

# List tools
mcporter list "My-Twitter-API" --schema
```

## Troubleshooting

### "Endpoint does not exist" Error

- ✅ Ensure MCP is enabled in RapidAPI Playground
- ✅ Click the "MCP" button in the API Playground to activate
- ✅ Verify your API key has access to the endpoint

### Authentication Issues

- Check your RapidAPI subscription is active
- Verify API key is correct
- Ensure you've subscribed to the specific API

## Common API Hosts

| API | Host |
|-----|------|
| Twitter/X | `twitter241.p.rapidapi.com` |
| YouTube | `yt-api.p.rapidapi.com` |
| Streaming Availability | `streaming-availability.p.rapidapi.com` |

## Requirements

- `curl` command line tool
- RapidAPI account with active subscriptions
- OpenClaw AI assistant

## Keywords

RapidAPI, MCP, OpenClaw, API integration, JSON-RPC, AI assistant, ChatGPT, Claude, LLM tools, Twitter API, YouTube API, API automation

## License

MIT License - See LICENSE file for details.

## Related Projects

- [OpenClaw](https://github.com/openclaw/openclaw)
- [mcporter](https://mcporter.dev)
- [RapidAPI](https://rapidapi.com)

# MCP Integration

**Learn**: Connect voice clients to your MCP servers for actions.

## What MCP Provides

Voice clients can discover and call tools your MCP server exposes.

## Minimal Configuration

```json
{
  "name": "My App",
  "mcp": {
    "servers": {
      "backend": {
        "url": "https://api.myapp.com/mcp"
      }
    }
  }
}
```

Voice clients connect to this URL, discover tools via MCP protocol.

## With Authentication

```json
{
  "mcp": {
    "servers": {
      "backend": {
        "url": "https://api.myapp.com/mcp",
        "headers": {
          "Authorization": "Bearer ${API_KEY}",
          "X-Client-ID": "voice-client"
        }
      }
    }
  }
}
```

Voice clients should prompt users for credentials or use OAuth.

## Multiple Servers

```json
{
  "mcp": {
    "servers": {
      "orders": {
        "url": "https://orders.myapp.com/mcp"
      },
      "inventory": {
        "url": "https://inventory.myapp.com/mcp"
      }
    }
  }
}
```

Voice clients connect to all servers, aggregate tools.

## Your MCP Server

Implement using [MCP SDK](https://modelcontextprotocol.io/):

```typescript
import { Server } from "@modelcontextprotocol/sdk/server/index.js";

const server = new Server({
  name: "myapp-server",
  version: "1.0.0",
});

// List tools
server.setRequestHandler(ListToolsRequestSchema, async () => {
  return {
    tools: [
      {
        name: "search_products",
        description: "Search for products",
        inputSchema: {
          type: "object",
          properties: {
            query: { type: "string" },
          },
        },
      },
    ],
  };
});

// Handle calls
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  if (request.params.name === "search_products") {
    const results = await db.search(request.params.arguments.query);
    return {
      content: [{ type: "text", text: JSON.stringify(results) }],
    };
  }
});
```

## Flow

1. Voice client reads manifest
2. Connects to MCP URL
3. Calls `tools/list` → gets available tools
4. User makes request
5. Voice client calls `tools/call` with tool name + args
6. Returns result to user

## Key Points

- **MCP servers must be running and accessible**
- **No local execution** - HTTP/SSE only
- **Voice manifest declares WHERE, not HOW**
- **Voice clients discover WHAT via MCP protocol**

## See Also

- [MCP Protocol Spec](https://modelcontextprotocol.io/)
- [04-with-mcp](../examples/04-with-mcp/)

---

**Previous**: [System Prompts](./System%20Prompts.md) | **Next**: [Voice Providers](./Voice%20Providers.md)

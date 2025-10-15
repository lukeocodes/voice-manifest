# Example 4: With MCP Server

Adds MCP server endpoint so voice clients can discover available tools.

## What's New

- ✅ `mcp.servers` - MCP server endpoints
- ✅ External system prompt file

## MCP Server Configuration

```json
{
  "mcp": {
    "servers": {
      "restaurant": {
        "url": "https://api.pastaparadise.com/mcp"
      }
    }
  }
}
```

This tells voice clients WHERE to find your MCP server.

## How It Works

1. **Voice client** reads your voice manifest
2. **Voice client** connects to `https://api.pastaparadise.com/mcp`
3. **Voice client** calls MCP protocol methods to discover tools:
   - `initialize` - Establish connection
   - `tools/list` - Get available tools
   - `tools/call` - Execute tools on user request
4. **MCP server** exposes tools like `make_reservation`, `get_menu`, etc.

## Example Flow

```
User: "Make a reservation for Friday at 7 PM for 4 people"

Voice Client:
1. Connects to https://api.pastaparadise.com/mcp
2. Calls tools/list → discovers make_reservation tool
3. Calls tools/call with:
   {
     "name": "make_reservation",
     "arguments": {
       "date": "2025-10-17",
       "time": "19:00",
       "party_size": 4
     }
   }
4. Speaks response: "Your reservation is confirmed for Friday at 7 PM"
```

## Your MCP Server

Your backend runs an MCP server that implements the protocol:

**Example using MCP SDK:**

```typescript
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";

const server = new Server(
  {
    name: "restaurant-server",
    version: "1.0.0",
  },
  {
    capabilities: {
      tools: {},
    },
  }
);

// List available tools
server.setRequestHandler(ListToolsRequestSchema, async () => {
  return {
    tools: [
      {
        name: "make_reservation",
        description: "Create a dining reservation",
        inputSchema: {
          type: "object",
          properties: {
            date: { type: "string", format: "date" },
            time: { type: "string", format: "time" },
            party_size: { type: "integer", minimum: 1 },
            name: { type: "string" },
            phone: { type: "string" },
          },
          required: ["date", "time", "party_size", "name", "phone"],
        },
      },
      {
        name: "get_menu",
        description: "Get menu items by category",
        inputSchema: {
          type: "object",
          properties: {
            category: {
              type: "string",
              enum: ["pasta", "appetizers", "desserts", "drinks"],
            },
          },
        },
      },
    ],
  };
});

// Handle tool execution
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  if (request.params.name === "make_reservation") {
    const reservation = await db.reservations.create({
      data: request.params.arguments,
    });

    return {
      content: [
        {
          type: "text",
          text: `Confirmed reservation for ${reservation.name} on ${reservation.date} at ${reservation.time} for ${reservation.party_size} guests`,
        },
      ],
    };
  }

  if (request.params.name === "get_menu") {
    const items = await db.menu.findMany({
      where: { category: request.params.arguments.category },
    });

    return {
      content: [
        {
          type: "text",
          text: JSON.stringify(items, null, 2),
        },
      ],
    };
  }
});

// Start server
const transport = new StdioServerTransport();
await server.connect(transport);
```

## Key Concept

- **Voice Manifest**: Declares WHERE the MCP server is (`url`)
- **MCP Server**: Implements WHAT can be done (tools, resources, prompts)
- **Voice Client**: Discovers and calls tools on behalf of users

## With Authentication

If your MCP server requires authentication:

```json
{
  "mcp": {
    "servers": {
      "restaurant": {
        "url": "https://api.pastaparadise.com/mcp",
        "headers": {
          "Authorization": "Bearer ${RESTAURANT_API_KEY}",
          "X-Client-ID": "voice-manifest-client"
        }
      }
    }
  }
}
```

> Note: Voice clients should prompt users to provide API keys or use OAuth flows.

## Use Case

Perfect for:

- Backend integration (databases, APIs)
- Action-based interactions (booking, ordering, searching)
- Dynamic tool discovery
- Standard MCP tooling ecosystem

## What's Still Missing

- No voice provider preferences

## Next Steps

- **Option A**: See [05-with-voice-agent](../05-with-voice-agent/) for all-in-one voice provider
- **Option B**: See [06-with-composite-pipeline](../06-with-composite-pipeline/) for individual STT/LLM/TTS

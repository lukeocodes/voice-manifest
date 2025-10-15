# Voice Manifest

> Make your website voice-enabled, just like manifest.json makes it installable

[![Version](https://img.shields.io/badge/version-0.0.1-blue.svg)](https://github.com/lukeocodes/voice-manifest/releases/tag/v0.0.1)
[![License: CC BY-NC 2.0](https://img.shields.io/badge/License-CC%20BY--NC%202.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc/2.0/)

## Overview

The **Voice Manifest** (`voice-manifest.json`) enables voice capabilities for websites in the same way that the Web App Manifest (`manifest.json`) enables Progressive Web App features.

```html
<link rel="voice-manifest" href="/voice-manifest.json" />
```

It's a **declarative specification**—you tell voice clients what your site can do, not how to configure voice providers.

## Quick Start

### Absolute Minimum

```json
{
  "name": "My Website"
}
```

That's literally it! Just one field makes your site discoverable as voice-enabled.

### Minimal Functional Example

```json
{
  "name": "Pasta Paradise",
  "display": {
    "call_to_action": "Ask about our menu or make a reservation",
    "suggested_prompts": [
      "What pasta dishes do you have?",
      "Make a reservation for Friday at 7 PM"
    ]
  },
  "system_prompt": "You are a helpful restaurant assistant.",
  "functions": [
    {
      "name": "make_reservation",
      "description": "Create a dining reservation",
      "parameters": {
        "type": "object",
        "properties": {
          "date": { "type": "string", "format": "date" },
          "time": { "type": "string", "format": "time" },
          "party_size": { "type": "integer" },
          "name": { "type": "string" },
          "phone": { "type": "string" }
        },
        "required": ["date", "time", "party_size", "name", "phone"]
      }
    }
  ]
}
```

That's it! Any compatible voice client can now interact with your site.

## Key Concepts

### 1. Declaration, Not Configuration

Voice Manifest is like `manifest.json`—it **declares** your site's capabilities, not how to implement them.

- ❌ NOT a configuration file for your voice pipeline
- ✅ A public declaration of what your site can do with voice
- ✅ Voice clients provide fallback providers if you don't specify any

### 2. Progressive Enhancement

| Level               | Features                               |
| ------------------- | -------------------------------------- |
| **Basic**           | Name + display hints                   |
| **+ Functions**     | Define voice actions                   |
| **+ System Prompt** | Customize behavior                     |
| **+ MCP**           | Connect backend services               |
| **+ Agent Config**  | Specify preferred providers (optional) |

### 3. Provider Flexibility

**No providers?** Voice clients use their own (browser extensions, OS features)

**Specific voice agent?** All-in-one solution (Retell, Vapi, etc.)

**Composite?** Mix and match STT/LLM/TTS providers

## Core Features

### Display Configuration

Control how voice UI appears:

```json
{
  "display": {
    "icon": "/icons/voice-icon.png",
    "background_color": "#8B0000",
    "theme_color": "#8B0000",
    "activation_phrase": "Talk to Pasta Paradise",
    "call_to_action": "Ask about our menu",
    "suggested_prompts": ["What's on the menu?", "Make a reservation"]
  }
}
```

### Function Calling

Uses OpenAI's function calling standard:

```json
{
  "functions": [
    {
      "name": "search_products",
      "description": "Search for products",
      "parameters": {
        "type": "object",
        "properties": {
          "query": { "type": "string" },
          "max_price": { "type": "number" }
        },
        "required": ["query"]
      }
    }
  ]
}
```

### System Prompt

Define assistant behavior:

```json
{
  "system_prompt": "You are a helpful shopping assistant. Be enthusiastic but never pushy."
}
```

Or reference external file:

```json
{
  "system_prompt": {
    "$ref": "./prompts/system-prompt.txt"
  }
}
```

### MCP Integration (Optional)

Connect to backend services using the [MCP standard](https://modelcontextprotocol.io/):

```json
{
  "mcp": {
    "servers": {
      "myserver": {
        "url": "https://api.mywebsite.com/mcp",
        "headers": {
          "Authorization": "Bearer ${API_KEY}"
        }
      }
    }
  }
}
```

Voice clients connect to these URLs to discover tools dynamically via the MCP protocol.

### Agent Configuration (Optional)

Specify preferred voice providers:

```json
{
  "agent": {
    "provider": {
      "name": "retell",
      "endpoint": "https://api.retellai.com/v1",
      "agent_id": "agent_abc123"
    }
  }
}
```

Or composite:

```json
{
  "agent": {
    "provider": {
      "stt": { "name": "deepgram" },
      "llm": { "name": "openai", "model": "gpt-4" },
      "tts": { "name": "elevenlabs" }
    }
  }
}
```

## Examples

See the `/examples` directory for complete implementations:

- **[voice-manifest-minimal.json](examples/voice-manifest-minimal.json)** - Just the basics
- **[voice-manifest-with-mcp.json](examples/voice-manifest-with-mcp.json)** - With MCP backend
- **[voice-manifest-with-voice-agent.json](examples/voice-manifest-with-voice-agent.json)** - With managed voice agent
- **[voice-manifest-with-composite-agents.json](examples/voice-manifest-with-composite-agents.json)** - With STT/LLM/TTS components

## How It Works

1. User visits your website
2. Voice client discovers `<link rel="voice-manifest">`
3. Voice client reads manifest
4. Voice client shows activation UI with your branding
5. User activates voice
6. Voice client uses your functions, prompts, and providers (or its own fallbacks)
7. Actions executed via your functions/MCP servers

## Use Cases

### 🍝 Restaurants

"Book a table for four tomorrow at 7 PM"

### 🛒 E-Commerce

"Show me wireless headphones under $100"

### ✈️ Travel

"Book a window seat on the morning flight"

### 🏥 Healthcare

"Schedule a checkup for next Tuesday"

### 🏦 Banking

"Transfer $50 to my savings account"

## Documentation

- **[Explainer](explainer.md)** - Comprehensive guide
- **[Schema](schema/voice-manifest.schema.json)** - JSON Schema for validation
- **[Quick Reference](docs/Quick%20Reference.md)** - Developer reference
- **[Getting Started](docs/Getting%20Started.md)** - Step-by-step guide
- **[Architecture](docs/MCP%20Extension%20Architecture.md)** - Design decisions

## Implementation

### 1. Create Manifest

```json
{
  "name": "Your Site",
  "system_prompt": "You are a helpful assistant.",
  "functions": [...]
}
```

### 2. Link from HTML

```html
<link rel="voice-manifest" href="/voice-manifest.json" />
```

### 3. Implement Handlers

Set up endpoints or MCP servers to handle function calls.

### 4. Test

Use voice clients that support Voice Manifest.

## Comparison to manifest.json

| manifest.json                | voice-manifest.json           |
| ---------------------------- | ----------------------------- |
| Makes site installable (PWA) | Makes site voice-enabled      |
| `<link rel="manifest">`      | `<link rel="voice-manifest">` |
| Declares PWA capabilities    | Declares voice capabilities   |
| Icons, colors, display mode  | Prompts, functions, providers |

## Provider Fallback Strategy

Voice clients can provide fallbacks when:

- ✅ No providers specified → Client uses its own
- ✅ Some providers specified → Client uses what you want, fills gaps
- ✅ Voice agent specified → All-in-one solution
- ✅ Composite specified → Individual components

This makes the manifest **flexible** and **accessible**—sites can work without requiring specific voice providers.

## Status

**Early proposal stage** (October 2025)

We're seeking feedback from:

- Voice platform providers (Retell, Vapi, ElevenLabs, etc.)
- Browser vendors (Chrome, Safari, Firefox)
- Web developers
- Standards organizations (W3C)

## Contributing

We welcome feedback and contributions!

**Areas needing input:**

- Real-world implementation experiences
- Browser integration approaches
- Security and privacy considerations
- Multi-modal experiences (voice + visual)
- Standards body feedback

**How to contribute:**

- Open issues for bugs or suggestions
- Submit PRs with improvements
- Share implementation examples
- Provide feedback on the specification

## Roadmap

- [x] Initial specification and schema
- [x] Example implementations
- [ ] Reference voice client implementation
- [ ] Browser extension prototype
- [ ] Developer tooling (validators, generators)
- [ ] Standards body submission

## License

This work is licensed under a [Creative Commons Attribution-NonCommercial 2.0](https://creativecommons.org/licenses/by-nc/2.0/uk/) license.

---

**Making the web voice-first, one manifest at a time** 🎙️

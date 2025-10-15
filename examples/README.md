# Voice Manifest Examples

Progressive examples showing Voice Manifest capabilities from absolute minimum to full complexity.

## Examples Overview

Each example builds on the previous one, gradually adding features:

### 1. [Absolute Minimum](./01-absolute-minimum/)

**Just one field**: `name`

```json
{
  "name": "My Website"
}
```

- ✅ Makes your site discoverable as voice-enabled
- ❌ No customization
- ❌ No backend integration
- ❌ No voice provider preferences

**Use case**: Testing voice manifest discovery works

---

### 2. [With Display Configuration](./02-with-display/)

**Adds**: Display metadata for better UX

```json
{
  "name": "Pasta Paradise",
  "display": {
    "activation_phrase": "Hey Pasta Paradise",
    "call_to_action": "What's on the menu?",
    "suggested_prompts": [...]
  }
}
```

- ✅ Branding (colors, icon)
- ✅ Suggested prompts to guide users
- ✅ Activation phrase
- ❌ Generic assistant behavior
- ❌ No backend integration

**Use case**: Public-facing voice interface with good UX

---

### 3. [With System Prompt](./03-with-system-prompt/)

**Adds**: Custom assistant personality and behavior

```json
{
  "name": "Pasta Paradise",
  "display": {...},
  "system_prompt": "You are a helpful assistant for..."
}
```

- ✅ Customized assistant personality
- ✅ Domain-specific knowledge
- ✅ Behavioral guidelines
- ❌ No tools (voice client can't perform actions)

**Use case**: Branded conversational experience

---

### 4. [With MCP Server](./04-with-mcp/)

**Adds**: Backend integration for actions

```json
{
  "name": "Pasta Paradise",
  "display": {...},
  "system_prompt": {...},
  "mcp": {
    "servers": {
      "restaurant": {
        "url": "https://api.restaurant.com/mcp"
      }
    }
  }
}
```

- ✅ Voice client discovers tools from MCP server
- ✅ Can perform real actions (reservations, orders, etc.)
- ✅ Backend integration
- ❌ No voice provider preferences

**Use case**: Full-featured voice application with backend

---

### 5. [With Voice Agent](./05-with-voice-agent/)

**Adds**: All-in-one voice provider preference

```json
{
  "name": "Pasta Paradise",
  "display": {...},
  "system_prompt": {...},
  "mcp": {...},
  "agent": {
    "provider": {
      "name": "deepgram",
      "endpoint": "https://agent.deepgram.com/v1/agent/converse",
      "config": {
        "voice": "aura-2-asteria-en",
        "model": "flux-general-en",
        "language": "en-US"
      }
    }
  }
}
```

- ✅ Managed voice platform
- ✅ Production-ready infrastructure
- ⚠️ Vendor lock-in

**Use case**: Enterprise deployment with managed infrastructure

---

### 6. [With Composite Pipeline](./06-with-composite-pipeline/)

**Adds**: Individual STT/LLM/TTS provider preferences

```json
{
  "name": "Healthcare Portal",
  "display": {...},
  "system_prompt": {...},
  "mcp": {...},
  "agent": {
    "provider": {
      "stt": {
        "name": "deepgram",
        "model": "flux-general-en"
      },
      "llm": {
        "name": "openai",
        "model": "gpt-4"
      },
      "tts": {
        "name": "deepgram",
        "model": "aura-2-asteria-en"
      }
    }
  }
}
```

- ✅ Best-of-breed providers
- ✅ Domain-specific models (medical, legal, etc.)
- ✅ Cost optimization
- ⚠️ More complex configuration

**Use case**: Specialized domains requiring specific capabilities

---

## Comparison Table

| Example                   | Display | System Prompt | MCP Tools | Voice Providers | Best For                 |
| ------------------------- | ------- | ------------- | --------- | --------------- | ------------------------ |
| **01-absolute-minimum**   | ❌      | ❌            | ❌        | ❌              | Testing discovery        |
| **02-with-display**       | ✅      | ❌            | ❌        | ❌              | Public-facing UI         |
| **03-with-system-prompt** | ✅      | ✅            | ❌        | ❌              | Branded conversation     |
| **04-with-mcp**           | ✅      | ✅            | ✅        | ❌              | Backend integration      |
| **05-with-voice-agent**   | ✅      | ✅            | ✅        | All-in-one      | Managed infrastructure   |
| **06-with-composite**     | ✅      | ✅            | ✅        | STT/LLM/TTS     | Specialized requirements |

## Key Concepts

### Progressive Enhancement

Start minimal and add features as needed:

1. **Name only** → Voice clients handle everything
2. **+ Display** → Better UX, guided prompts
3. **+ System Prompt** → Custom personality
4. **+ MCP** → Real actions via tools
5. **+ Voice Agent** → Managed infrastructure
6. **+ Composite** → Best-of-breed components

### Provider Fallbacks

Voice clients provide fallbacks when components are missing:

```
No MCP        → Voice client provides conversational-only mode
No Voice Agent → Voice client uses its own STT/LLM/TTS
No STT        → Voice client uses its default STT
No LLM        → Voice client uses its default LLM
No TTS        → Voice client uses its default TTS
```

### MCP = Tools

The MCP section tells voice clients WHERE to discover tools:

```json
{
  "mcp": {
    "servers": {
      "backend": {
        "url": "https://api.example.com/mcp"
      }
    }
  }
}
```

Voice client connects → MCP server responds with available tools → Voice client can call tools on user's behalf.

### Voice Agent vs Composite

**Voice Agent** (all-in-one):

```json
{
  "agent": {
    "provider": {
      "name": "deepgram",
      "endpoint": "https://agent.deepgram.com/v1/agent/converse",
      "config": {
        "voice": "aura-2-asteria-en",
        "model": "flux-general-en"
      }
    }
  }
}
```

**Composite** (mix & match):

```json
{
  "agent": {
    "provider": {
      "stt": { "name": "deepgram", "model": "flux-general-en" },
      "llm": { "name": "openai", "model": "gpt-4" },
      "tts": { "name": "deepgram", "model": "aura-2-asteria-en" }
    }
  }
}
```

Voice agents and composite are **mutually exclusive**.

## Implementation Tips

### Start Simple

Begin with example 01 or 02, verify discovery works, then gradually add features.

### External References

Use `$ref` for larger content:

```json
{
  "system_prompt": {
    "$ref": "./system-prompt.txt"
  }
}
```

### Authentication

For MCP servers requiring auth:

```json
{
  "mcp": {
    "servers": {
      "backend": {
        "url": "https://api.example.com/mcp",
        "headers": {
          "Authorization": "Bearer ${API_KEY}"
        }
      }
    }
  }
}
```

Voice clients should prompt users for credentials or use OAuth.

### HTML Integration

Link to your manifest:

```html
<link rel="voice-manifest" href="/voice-manifest.json" />
```

## Questions?

- See [explainer.md](../explainer.md) for detailed concept explanations
- See [schema/voice-manifest.schema.json](../schema/voice-manifest.schema.json) for the full schema
- See [docs/](../docs/) for architectural decisions and guides

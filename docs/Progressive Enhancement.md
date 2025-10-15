# Progressive Enhancement

**Learn**: Start minimal, add features incrementally.

## The Principle

Voice capabilities should enhance, not require. Start with basics, add complexity only when needed.

## Level 1: Discoverable

```json
{
  "name": "My Website"
}
```

**What it enables**: Voice clients know your site supports voice
**What happens**: Generic voice interaction with fallback behavior

## Level 2: Guided

```json
{
  "name": "My Website",
  "display": {
    "activation_phrase": "Talk to my site",
    "suggested_prompts": ["What can you help with?"]
  }
}
```

**What it adds**: User guidance
**What happens**: Better UX, users know what to ask

## Level 3: Customized

```json
{
  "name": "My Website",
  "display": {...},
  "system_prompt": "You are a helpful assistant for..."
}
```

**What it adds**: Custom personality and behavior
**What happens**: Branded conversation experience

## Level 4: Actionable

```json
{
  "name": "My Website",
  "display": {...},
  "system_prompt": {...},
  "mcp": {
    "servers": {
      "backend": {
        "url": "https://api.mysite.com/mcp"
      }
    }
  }
}
```

**What it adds**: Real actions via MCP tools
**What happens**: Voice can perform tasks (orders, bookings, etc.)

## Level 5: Optimized

```json
{
  "name": "My Website",
  "display": {...},
  "system_prompt": {...},
  "mcp": {...},
  "agent": {
    "provider": {
      "name": "deepgram",
      "config": {...}
    }
  }
}
```

**What it adds**: Voice provider preferences
**What happens**: Domain-specific models, optimized performance

## Decision Guide

**Add display config** if:

- You want to guide users
- You have branding requirements

**Add system prompt** if:

- You need custom personality
- You have domain-specific knowledge
- You want behavioral guidelines

**Add MCP integration** if:

- Users need to perform actions
- You have backend services to integrate
- You want dynamic tool discovery

**Add voice providers** if:

- You need specialized models (medical, legal, etc.)
- You have existing provider relationships
- Default providers don't meet requirements

## See Examples

Progressive examples show this in practice:

- [01-absolute-minimum](../examples/01-absolute-minimum/)
- [02-with-display](../examples/02-with-display/)
- [03-with-system-prompt](../examples/03-with-system-prompt/)
- [04-with-mcp](../examples/04-with-mcp/)
- [05-with-voice-agent](../examples/05-with-voice-agent/)
- [06-with-composite-pipeline](../examples/06-with-composite-pipeline/)

---

**See Also**: [Getting Started](./Getting%20Started.md) - Start implementing | [Security Considerations](./Security%20Considerations.md) - Keep it secure

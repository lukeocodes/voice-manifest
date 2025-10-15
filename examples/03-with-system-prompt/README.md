# Example 3: With System Prompt

Adds a system prompt to define assistant behavior and personality.

## What's New

- ✅ `system_prompt` - Defines how the voice assistant should behave

## System Prompt

The system prompt defines:

- **Role**: "helpful assistant for Pasta Paradise"
- **Personality**: "warm, friendly, knowledgeable"
- **Behavior**: "confirm important details"
- **Domain knowledge**: "Italian cuisine"

## How It Works

Voice clients use this prompt when interacting with users:

1. User asks: "What pasta dishes do you have?"
2. Voice client uses your system prompt + their LLM
3. Response is guided by your defined personality and knowledge

## Use Case

Perfect for:

- Customizing assistant personality
- Adding domain-specific knowledge
- Setting behavioral guidelines
- Ensuring consistent brand voice

## Inline vs External

### Inline (This Example)

```json
{
  "system_prompt": "You are a helpful assistant..."
}
```

Good for short prompts (<100 words).

### External File

```json
{
  "system_prompt": {
    "$ref": "./system-prompt.txt"
  }
}
```

Better for longer, detailed prompts. See [04-with-mcp](../04-with-mcp/) for an example.

## What's Still Missing

- No MCP servers (voice client has no tools/functions to call)
- No voice providers

## Next Step

See [04-with-mcp](../04-with-mcp/) to add MCP server integration for real actions.

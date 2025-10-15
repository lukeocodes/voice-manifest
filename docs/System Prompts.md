# System Prompts

**Learn**: Define your voice assistant's personality and behavior.

## Inline Prompts

For short prompts (<100 words):

```json
{
  "name": "Coffee Shop",
  "system_prompt": "You are a friendly barista. Help customers order coffee, ask about our menu, and answer location questions. Be enthusiastic but concise."
}
```

## External File

For detailed prompts:

```json
{
  "name": "Coffee Shop",
  "system_prompt": {
    "$ref": "./system-prompt.txt"
  }
}
```

Then create `system-prompt.txt`:

```
You are a friendly barista at Brew Haven Coffee Shop.

PERSONALITY:
- Warm and welcoming
- Coffee enthusiast
- Patient with questions

CAPABILITIES:
- Menu recommendations
- Order taking
- Store information

GUIDELINES:
- Always confirm orders before proceeding
- Ask about dietary restrictions
- Suggest pairings when appropriate
```

## Best Practices

**Be specific**: Define exact behaviors, not general attitudes.

**Set boundaries**: List what the assistant should NOT do.

**Give context**: Include business hours, location, policies.

**Use examples**: Show how to handle common scenarios.

## Examples

See:

- [03-with-system-prompt](../examples/03-with-system-prompt/)
- [04-with-mcp](../examples/04-with-mcp/)

---

**Previous**: [Display Configuration](./Display%20Configuration.md) | **Next**: [MCP Integration](./MCP%20Integration.md)

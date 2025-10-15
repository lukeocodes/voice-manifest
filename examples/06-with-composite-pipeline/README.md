# Example 6: With Composite Pipeline

Uses individual STT, LLM, and TTS providers + MCP server.

## What's New

- ✅ `agent.provider.stt` - Speech-to-Text configuration
- ✅ `agent.provider.llm` - Language Model configuration
- ✅ `agent.provider.tts` - Text-to-Speech configuration
- ✅ MCP + Composite Pipeline combined

## Composite Pipeline

```json
{
  "agent": {
    "provider": {
      "stt": {
        "name": "deepgram",
        "model": "nova-2-medical"
      },
      "llm": {
        "name": "openai",
        "model": "gpt-4"
      },
      "tts": {
        "name": "deepgram",
        "model": "aura-asteria-en"
      }
    }
  }
}
```

## Why Composite?

- **Flexibility**: Choose best provider for each component
- **Specialization**: Use domain-specific models (medical, legal, etc.)
- **Cost optimization**: Mix providers based on pricing

## How It Works Together

1. **STT Provider** converts speech to text
2. **LLM Provider** processes with your system prompt
3. **LLM** calls MCP tools when needed
4. **TTS Provider** converts response to speech

## Use Case

Perfect for:

- Specialized domains (healthcare, legal, technical)
- Cost-sensitive deployments
- Custom voice experiences

## See Example

See [voice-manifest.json](./voice-manifest.json) for the complete configuration.

## This is Maximum Complexity

This example shows the most complex configuration:

- ✅ Display configuration
- ✅ System prompt
- ✅ MCP server
- ✅ Composite voice pipeline
- ✅ External configs

You now have the full spectrum from absolute minimum to maximum!

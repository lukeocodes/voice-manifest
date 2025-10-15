# Example 5: With Voice Agent Provider

Uses an all-in-one voice agent provider + MCP server.

## What's New

- ✅ `agent.provider` - Voice agent configuration
- ✅ MCP + Voice Agent combined

## Voice Agent Configuration

```json
{
  "agent": {
    "provider": {
      "name": "deepgram",
      "endpoint": "https://api.deepgram.com/v1/aura",
      "config": {
        "voice": "aura-asteria-en",
        "model": "nova-2",
        "language": "en-US"
      }
    }
  }
}
```

A voice agent handles STT/LLM/TTS as an all-in-one solution. Deepgram Aura provides conversational AI with integrated speech recognition, language understanding, and voice synthesis.

## How It Works Together

1. **Voice Agent** handles voice pipeline (STT → LLM → TTS)
2. **MCP Server** provides tools for actions
3. Voice agent calls MCP tools when user requests actions

## Use Case

Perfect for:

- Production deployments
- Enterprise applications
- Managed voice infrastructure

## See Example

See [voice-manifest.json](./voice-manifest.json) for the complete configuration.

## Alternative

See [06-with-composite-pipeline](../06-with-composite-pipeline/) for individual STT/LLM/TTS control.

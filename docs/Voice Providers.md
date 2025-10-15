# Voice Providers

**Learn**: Specify voice provider preferences (optional).

## When to Use

Voice clients provide default STT/LLM/TTS. Specify providers only if:

- You need domain-specific models (medical, legal)
- You want specific voice characteristics
- You have existing provider relationships

## All-in-One Agent

Use a voice agent that handles everything:

```json
{
  "agent": {
    "provider": {
      "name": "deepgram",
      "endpoint": "https://agent.deepgram.com/v1/agent/converse",
      "config": {
        "voice": "aura-2-asteria-en",
        "model": "flux-general-en",
        "language": "en-US"
      },
      "authentication": {
        "type": "bearer",
        "header": "Authorization"
      }
    }
  }
}
```

**Pros**: Simple, optimized, managed
**Cons**: Vendor lock-in

## Composite Pipeline

Specify individual components:

```json
{
  "agent": {
    "provider": {
      "stt": {
        "name": "deepgram",
        "endpoint": "https://api.deepgram.com/v1/listen",
        "model": "flux-general-en"
      },
      "llm": {
        "name": "openai",
        "endpoint": "https://api.openai.com/v1/chat/completions",
        "model": "gpt-4"
      },
      "tts": {
        "name": "deepgram",
        "endpoint": "https://api.deepgram.com/v1/speak",
        "model": "aura-2-asteria-en"
      }
    }
  }
}
```

**Pros**: Flexibility, best-of-breed, cost optimization
**Cons**: More configuration

## Voice Agent vs Composite

**Mutually exclusive** - choose one or the other.

## Fallback Strategy

Voice clients handle missing providers:

| You Specify     | Voice Client Behavior                     |
| --------------- | ----------------------------------------- |
| Nothing         | Uses own STT/LLM/TTS                      |
| Voice agent     | Uses your agent                           |
| Some components | Uses your components, fills gaps with own |
| All components  | Uses all your components                  |

## Authentication

Voice clients should prompt users for API keys or use OAuth:

```json
{
  "agent": {
    "provider": {
      "name": "deepgram",
      "authentication": {
        "type": "bearer",
        "header": "Authorization"
      }
    }
  }
}
```

Never hardcode credentials in the manifest.

## Examples

- [05-with-voice-agent](../examples/05-with-voice-agent/) - Deepgram Aura
- [06-with-composite-pipeline](../examples/06-with-composite-pipeline/) - Deepgram + OpenAI

---

**Previous**: [MCP Integration](./MCP%20Integration.md) | **Next**: [External References](./External%20References.md)

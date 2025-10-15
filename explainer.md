# Voice Manifest Explainer

**Version 0.0.1** | Released October 15, 2025

## Introduction

The **Voice Manifest** (`voice-manifest.json`) makes websites voice-enabled in the same way that the Web App Manifest (`manifest.json`) makes websites installable as Progressive Web Apps.

Just as `manifest.json` tells browsers and operating systems "this website can act like a native app," the Voice Manifest tells voice agents, browsers, and operating systems "this website can be interacted with through voice."

## The Problem

Voice AI is everywhere—in our phones, computers, cars, and smart speakers. Yet websites remain primarily visual interfaces that voice assistants struggle to interact with meaningfully.

When a user says "book a table at that Italian restaurant," their voice assistant might find the restaurant's website, but has no standardized way to:

- Understand what voice interactions are possible
- Know how to execute actions on the user's behalf
- Provide a consistent voice experience

## The Solution

The Voice Manifest provides a **declarative** way for websites to describe their voice capabilities. It's a simple JSON file that any compatible voice client can read to enable voice interactions.

```html
<link rel="voice-manifest" href="/voice-manifest.json" />
```

### Minimal Example

The simplest voice-enabled website needs only a name and optionally some display information:

```json
{
  "name": "Pasta Paradise",
  "display": {
    "call_to_action": "Ask about our menu or make a reservation",
    "suggested_prompts": [
      "What pasta dishes do you have?",
      "Make a reservation for Friday"
    ]
  }
}
```

**That's it!** Any voice client (browser extension, OS feature, voice agent platform) can now:

- Detect the site is voice-enabled
- Show the activation phrase to users
- Provide suggested prompts
- Use its own STT/LLM/TTS providers to enable voice interaction

## Core Concepts

### 1. Declaration, Not Configuration

The Voice Manifest is about **what your site can do**, not **how to configure voice providers**.

**This is NOT a configuration file for your voice pipeline.** It's a **public declaration** of your site's voice capabilities, similar to how `manifest.json` declares PWA capabilities.

### 2. Progressive Enhancement

Start simple, add complexity as needed:

- **Minimal**: Just metadata and display hints
- **+ Functions**: Add function calling for actions
- **+ System Prompt**: Customize the voice assistant's behavior
- **+ MCP**: Connect to backend services
- **+ Agent Config**: Specify preferred voice providers (optional)

### 3. Provider Flexibility

The manifest supports multiple approaches:

**No providers specified** (Browser/OS provides fallback):

```json
{
  "name": "My Site",
  "functions": [...]
}
```

**With specific voice agent** (All-in-one solution):

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

**With composite STT/LLM/TTS** (Individual components):

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

## Key Features

### Display Configuration

Control how your voice interface appears to users:

```json
{
  "name": "Pasta Paradise",
  "short_name": "PP",
  "display": {
    "icon": "/icons/voice-icon.png",
    "background_color": "#8B0000",
    "theme_color": "#8B0000",
    "activation_phrase": "Talk to Pasta Paradise",
    "call_to_action": "Ask about our menu or make a reservation",
    "suggested_prompts": [
      "What pasta dishes do you have?",
      "Make a reservation for Friday at 7 PM",
      "Do you have gluten-free options?"
    ]
  }
}
```

These fields help voice clients present your site's capabilities in a user-friendly way.

### System Prompt

Define how your voice assistant should behave:

```json
{
  "system_prompt": "You are a helpful assistant for Pasta Paradise restaurant. Help customers with menu questions, reservations, and general information. Be warm, friendly, and knowledgeable about Italian cuisine."
}
```

Or reference an external file:

```json
{
  "system_prompt": {
    "$ref": "./prompts/system-prompt.txt"
  }
}
```

### Function Calling

Define actions using OpenAI's function calling standard:

```json
{
  "functions": [
    {
      "name": "make_reservation",
      "description": "Create a dining reservation",
      "parameters": {
        "type": "object",
        "properties": {
          "date": {
            "type": "string",
            "format": "date",
            "description": "Reservation date (YYYY-MM-DD)"
          },
          "time": {
            "type": "string",
            "format": "time",
            "description": "Reservation time (HH:MM)"
          },
          "party_size": {
            "type": "integer",
            "minimum": 1,
            "maximum": 20,
            "description": "Number of guests"
          },
          "name": {
            "type": "string",
            "description": "Name for the reservation"
          },
          "phone": {
            "type": "string",
            "description": "Contact phone number"
          }
        },
        "required": ["date", "time", "party_size", "name", "phone"]
      }
    }
  ]
}
```

### MCP Integration (Optional)

Connect to Model Context Protocol servers for tool discovery:

```json
{
  "mcp": {
    "servers": {
      "restaurant": {
        "url": "https://api.restaurant.com/mcp"
      }
    }
  }
}
```

Voice clients connect to this URL to discover available tools, resources, and prompts via the MCP protocol. Your MCP server must already be running and accessible at this endpoint.

### Voice Agent Configuration (Optional)

Specify preferred voice providers if you have specific requirements:

**All-in-one voice agent:**

```json
{
  "agent": {
    "provider": {
      "name": "retell",
      "endpoint": "https://api.retellai.com/v1",
      "agent_id": "agent_abc123",
      "config": {
        "voice_id": "professional-female-us",
        "voice_speed": 1.0,
        "interruption_sensitivity": 0.5
      }
    }
  }
}
```

**Composite STT/LLM/TTS:**

```json
{
  "agent": {
    "provider": {
      "stt": {
        "name": "deepgram",
        "model": "nova-2",
        "keywords": ["pasta", "reservation", "gluten-free"]
      },
      "llm": {
        "name": "openai",
        "model": "gpt-4",
        "temperature": 0.7
      },
      "tts": {
        "name": "elevenlabs",
        "voice_id": "clara-italian-warmth"
      }
    }
  }
}
```

**Important**: You can specify providers, but voice clients can use their own fallbacks if:

- Providers aren't specified
- Specified providers aren't available
- Users prefer different providers

## Complete Examples

### 1. Minimal Restaurant

```json
{
  "$schema": "https://voicemanifest.org/voice-manifest/schema/0.0.1/voice-manifest.schema.json",
  "name": "Pasta Paradise",
  "description": "Authentic Italian dining in Boston",
  "display": {
    "activation_phrase": "Talk to Pasta Paradise",
    "call_to_action": "Ask about our menu or make a reservation",
    "suggested_prompts": [
      "What pasta dishes do you have?",
      "Make a reservation for Friday at 7 PM",
      "Do you have gluten-free options?"
    ]
  },
  "system_prompt": "You are a helpful assistant for Pasta Paradise restaurant. Help customers with menu questions, reservations, and general information.",
  "functions": [
    {
      "name": "get_menu",
      "description": "Get menu items with optional filters",
      "parameters": {
        "type": "object",
        "properties": {
          "category": {
            "type": "string",
            "enum": ["appetizers", "pasta", "mains", "desserts"]
          }
        },
        "required": []
      }
    },
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

### 2. E-Commerce with Voice Agent

```json
{
  "$schema": "https://voicemanifest.org/voice-manifest/schema/0.0.1/voice-manifest.schema.json",
  "name": "Premium Store",
  "description": "Voice-enabled shopping experience",
  "display": {
    "activation_phrase": "Shop with voice",
    "suggested_prompts": [
      "Show me wireless headphones under $100",
      "Where's my order?",
      "Find blue running shoes"
    ]
  },
  "system_prompt": "You are a helpful shopping assistant. Help customers find products and track orders.",
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
  ],
  "agent": {
    "provider": {
      "name": "retell",
      "endpoint": "https://api.retellai.com/v1",
      "agent_id": "agent_ecommerce_abc123"
    }
  }
}
```

### 3. Healthcare with Composite Agents + MCP

```json
{
  "$schema": "https://voicemanifest.org/voice-manifest/schema/0.0.1/voice-manifest.schema.json",
  "name": "Healthcare Portal",
  "description": "Voice-enabled patient portal",
  "display": {
    "suggested_prompts": [
      "Schedule a checkup",
      "Refill my prescription",
      "When is my next appointment?"
    ]
  },
  "system_prompt": "You are a HIPAA-compliant healthcare assistant. Help patients with appointments and prescriptions. Never provide medical advice.",
  "functions": [
    {
      "name": "schedule_appointment",
      "description": "Schedule a medical appointment",
      "parameters": {
        "type": "object",
        "properties": {
          "appointment_type": {
            "type": "string",
            "enum": ["checkup", "follow-up", "specialist"]
          },
          "preferred_date": { "type": "string", "format": "date" }
        },
        "required": ["appointment_type"]
      }
    }
  ],
  "agent": {
    "provider": {
      "stt": {
        "name": "deepgram",
        "model": "nova-2-medical",
        "keywords": ["prescription", "appointment", "medication"]
      },
      "llm": {
        "name": "openai",
        "model": "gpt-4",
        "temperature": 0.3
      },
      "tts": {
        "name": "elevenlabs",
        "voice_id": "professional-calm-female",
        "speaking_rate": 0.9
      }
    }
  },
  "mcp": {
    "servers": {
      "ehr": {
        "url": "https://api.healthcare.internal/mcp"
      }
    }
  },
  "privacy": {
    "data_retention": "Voice recordings deleted immediately. Transcripts retained 7 days per HIPAA.",
    "recording_consent": true,
    "pii_handling": "encrypt"
  }
}
```

## How It Works: The Flow

1. **User visits your website**
2. **Voice client** (browser, extension, OS) **discovers** `<link rel="voice-manifest">`
3. **Voice client reads** the manifest
4. **Voice client shows** activation UI with your branding and suggested prompts
5. **User activates** voice interaction
6. **Voice client uses**:
   - Your system prompt to guide behavior
   - Your functions to understand available actions
   - Your specified providers OR its own fallbacks
   - Your MCP servers to execute actions
7. **Actions are executed** and responses provided to user

## Provider Fallback Strategy

This is a key feature that makes the Voice Manifest flexible:

**If you specify no providers:**

- Voice clients use their own (browser plugins, OS features, etc.)
- Example: Deepgram browser extension provides STT/LLM/TTS

**If you specify some providers:**

- Voice clients use what you specify
- Fall back to their own for unspecified components
- Example: You specify LLM, client provides STT/TTS

**If you specify a voice agent:**

- Voice clients use your all-in-one solution
- Voice agent provider handles STT/LLM/TTS
- Example: Your Retell agent does everything

**If you specify composite (STT/LLM/TTS):**

- Voice clients use your specified components
- Can still fall back if any fail
- Example: Your Deepgram + OpenAI + ElevenLabs stack

## Mutual Exclusivity

**Voice Agent OR Composite** - not both:

```json
// ✅ Valid - Voice agent only
{
  "agent": {
    "provider": {
      "name": "retell",
      "endpoint": "..."
    }
  }
}

// ✅ Valid - Composite only
{
  "agent": {
    "provider": {
      "stt": {...},
      "llm": {...},
      "tts": {...}
    }
  }
}

// ❌ Invalid - Cannot mix
{
  "agent": {
    "provider": {
      "name": "retell",
      "stt": {...}  // ERROR: voice agent provides STT
    }
  }
}
```

## Real-World Use Cases

### Restaurants & Hospitality

- **Reservations**: "Book a table for four tomorrow at 7"
- **Menu inquiries**: "What vegetarian options do you have?"
- **Takeout orders**: "Order the usual for pickup"

### E-Commerce

- **Product search**: "Show me wireless headphones under $100"
- **Order tracking**: "Where's my order?"
- **Shopping**: "Add size medium to my cart"

### Healthcare

- **Appointments**: "Schedule a checkup next Tuesday"
- **Prescriptions**: "Refill my blood pressure medication"
- **Information**: "When are you open?"

### Banking & Finance

- **Balance**: "What's my checking balance?"
- **Transfers**: "Transfer $50 to savings"
- **Bill pay**: "Pay my electric bill"

### Travel

- **Booking**: "Book a window seat on the morning flight"
- **Hotel**: "Find hotels near the conference"
- **Information**: "What's my confirmation number?"

## Implementation

### Step 1: Create the Manifest

Start with the basics:

```json
{
  "name": "Your Site",
  "display": {
    "suggested_prompts": ["What can you help with?"]
  },
  "system_prompt": "You are a helpful assistant for [your site].",
  "functions": [...]
}
```

### Step 2: Link from HTML

```html
<link rel="voice-manifest" href="/voice-manifest.json" />
```

### Step 3: Implement Function Handlers

When voice clients call your functions, you need to handle them. This typically means:

- **REST API endpoints** that execute the functions
- **MCP server** that provides the tools
- **Webhook handlers** that process requests

### Step 4: Test

Use voice clients that support Voice Manifest:

- Browser extensions
- Voice agent platforms
- OS-level voice features
- Testing tools

## Privacy & Security

### Privacy Considerations

```json
{
  "privacy": {
    "data_retention": "Voice data retained for 30 days",
    "recording_consent": true,
    "pii_handling": "encrypt",
    "privacy_policy": "https://example.com/privacy"
  }
}
```

### Security Best Practices

1. **Never expose API keys** in the manifest
2. **Use authentication** for sensitive functions
3. **Validate all inputs** server-side
4. **Rate limit** voice interactions
5. **Log security events**
6. **HTTPS only** for all endpoints

## Comparison to manifest.json

| Feature                 | manifest.json                | voice-manifest.json                  |
| ----------------------- | ---------------------------- | ------------------------------------ |
| **Purpose**             | Make site installable as PWA | Make site voice-enabled              |
| **Discovery**           | `<link rel="manifest">`      | `<link rel="voice-manifest">`        |
| **Required fields**     | name, icons                  | name                                 |
| **Display config**      | icons, colors, display mode  | activation phrase, suggested prompts |
| **Functionality**       | Declares PWA capabilities    | Declares voice capabilities          |
| **Provider config**     | N/A                          | Optional voice providers             |
| **Backend integration** | Service workers              | Functions + optional MCP             |

## Specification Status

The Voice Manifest is currently in **early proposal stage** (October 2025).

We're seeking feedback from:

- Voice platform providers
- Browser vendors
- Web developers
- Standards organizations

## Contributing

We welcome contributions and feedback! See the repository for:

- **Examples**: Complete working examples
- **Schema**: JSON Schema for validation
- **Documentation**: Detailed guides and references

## License

This work is licensed under a [Creative Commons Attribution-NonCommercial 2.0](https://creativecommons.org/licenses/by-nc/2.0/uk/) license.

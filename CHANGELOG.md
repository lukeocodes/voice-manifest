# Changelog

All notable changes to the Voice Manifest specification will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.0.1] - 2025-10-15

### Initial Release

Declarative specification for voice-enabled websites. The manifest does not assume an execution environment - browsers, browser extensions, and operating systems provide native implementations and fallbacks.

**Core capabilities:**

- Display configuration: branding, activation phrases, suggested prompts
- System prompts: inline or external file reference for assistant behavior
- MCP integration: HTTP/SSE endpoints for tool discovery (no local execution)
- Voice agent configuration: all-in-one providers or composite STT/LLM/TTS pipelines

**Schema**: `schema/0.0.1/voice-manifest.schema.json`

**Examples**: Progressive examples from absolute minimum (name only) to full composite pipeline with MCP integration.

---

[0.0.1]: https://github.com/lukeocodes/voice-manifest/releases/tag/v0.0.1

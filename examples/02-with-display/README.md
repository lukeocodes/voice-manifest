# Example 2: With Display Configuration

Adds display configuration to make your voice interface user-friendly.

## What's New

- ✅ Display metadata (activation phrase, CTA, suggested prompts)
- ✅ Branding (colors, icon)
- ✅ Additional manifest.json fields (short_name, description, lang)

## What Voice Clients Can Do

Voice clients can now:

- Show your branding (colors, icon)
- Display an activation phrase: "Talk to Pasta Paradise"
- Show suggested prompts to guide users
- Present a clear call-to-action

## Key Fields

### `display.activation_phrase`

Suggests how users can activate voice for your site

### `display.call_to_action`

Describes what users can do

### `display.suggested_prompts`

Examples of what users can ask (max 10)

## Use Case

Perfect for:

- Making your voice interface discoverable
- Guiding users on what they can ask
- Branding your voice experience

## What's Still Missing

- No system prompt (generic behavior)
- No MCP servers (no backend integration)
- No voice providers

## Next Step

See [03-with-system-prompt](../03-with-system-prompt/) to customize assistant behavior.

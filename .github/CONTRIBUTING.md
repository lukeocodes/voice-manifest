# Contributing

Thanks for your interest in contributing to the Voice Manifest specification!

## Ways to Contribute

### Report Issues

Found a problem or have a suggestion? [Open an issue](https://github.com/lukeocodes/voice-manifest/issues/new).

**Good issue reports include**:

- Clear description of the problem
- Specific example or use case
- Expected vs actual behavior (if applicable)

### Improve Documentation

Documentation improvements are always welcome:

- Fix typos or unclear wording
- Add examples
- Improve explanations
- Translate documentation

### Propose Changes

For spec changes:

1. **Discuss first** - Open an issue to discuss your idea before implementing
2. **Keep it focused** - One feature/fix per pull request
3. **Provide rationale** - Explain why the change is needed
4. **Show examples** - Include example manifest snippets

## Making Changes

### Quick Edits

For simple changes (typos, wording):

1. Click the edit button on any file in GitHub
2. Make your changes
3. Submit a pull request

### Larger Changes

1. Fork the repository
2. Create a branch (`git checkout -b feature/your-feature`)
3. Make your changes
4. Test your changes (see below)
5. Commit (`git commit -m 'Add feature'`)
6. Push (`git push origin feature/your-feature`)
7. Open a pull request

## Testing Changes

### Schema Changes

If you modify `schema/0.0.1/voice-manifest.schema.json`:

```bash
# Validate the schema itself
npm install -g ajv-cli
ajv compile -s schema/0.0.1/voice-manifest.schema.json

# Test against examples
ajv validate -s schema/0.0.1/voice-manifest.schema.json -d "examples/**/voice-manifest.json"
```

### Documentation Changes

- Ensure markdown is valid
- Check all links work
- Verify code examples are correct

### Example Changes

If you modify examples:

```bash
# Validate each example
ajv validate -s schema/0.0.1/voice-manifest.schema.json -d examples/01-absolute-minimum/voice-manifest.json
```

## Style Guide

### JSON

- Use 2 spaces for indentation
- Use double quotes
- No trailing commas
- Keep examples realistic

### Markdown

- Use sentence case for headings
- Keep lines under 120 characters when possible
- Use code blocks with language tags
- Link to examples and other docs

### Writing Style

- **Be concise** - Get to the point quickly
- **Use examples** - Show, don't just tell
- **Active voice** - "Voice clients connect to..." not "Connection is made by..."
- **Present tense** - "The manifest defines..." not "The manifest will define..."

## Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](./CODE_OF_CONDUCT.md). By participating, you agree to uphold this code.

## Questions?

Not sure about something? Open an issue and ask! We're here to help.

## License

By contributing, you agree that your contributions will be licensed under the same [CC BY-NC 2.0](https://creativecommons.org/licenses/by-nc/2.0/) license that covers the project.

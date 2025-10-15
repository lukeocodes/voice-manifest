## Description

<!-- Briefly describe your changes -->

Closes #

## Type of Change

<!-- Check all that apply -->

- [ ] Documentation improvement
- [ ] Schema update
- [ ] Example addition/update
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change

## Changes Made

<!-- List specific changes -->

-
-
-

## Testing

<!-- How did you test these changes? -->

### Schema Validation

```bash
# If schema was modified
ajv compile -s schema/0.0.1/voice-manifest.schema.json

# Validate examples against schema
ajv validate -s schema/0.0.1/voice-manifest.schema.json -d "examples/**/voice-manifest.json"
```

- [ ] Schema compiles successfully
- [ ] All examples validate against schema

### Documentation

- [ ] Links are working
- [ ] Code examples are correct
- [ ] Markdown renders properly

## Checklist

- [ ] I have read the [CONTRIBUTING](../CONTRIBUTING.md) guidelines
- [ ] My changes follow the project's style guide
- [ ] I have tested my changes
- [ ] I have updated relevant documentation
- [ ] My commits have clear, descriptive messages

## Additional Context

<!-- Any additional information, screenshots, or context -->

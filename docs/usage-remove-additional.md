---
title: ▪️ removeAdditional (options)
---


Determines whether to keep or remove additional properties in request body or to fail validation if schema has `additionalProperties` set to `false`. For further details, refer to [AJV documentation](https://ajv.js.org/docs/validation.html#removing-additional-properties)

- `false` (**default**) - not to remove additional properties
- `"all"` - all additional properties are removed, regardless of additionalProperties keyword in schema (and no validation is made for them).
- `true` - only additional properties with additionalProperties keyword equal to false are removed.
- `"failing"` - additional properties that fail request schema validation will be removed (where additionalProperties keyword is false or schema).

For example:

```javascript
validateRequests: {
removeAdditional: true,
}
```
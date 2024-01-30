---
title: ▪️ coerceTypes (deprecated)
---

!!! warning
    `coerceTypes` is deprecated in favor of [validateRequests.coerceTypes](usage-validate-requests.md) and [validateResponses.coerceTypes](usage-validate-responses.md)

Determines whether the validator should coerce value types to match the those defined in the OpenAPI spec. This option applies **only** to path params, query strings, headers, and cookies. _It is **highly unlikely** that you will want to disable this. As such this option is deprecated and will be removed in the next major version_

!!! Option-schema
```javascript
    coerceTypes: true | 'array'
```

## coerceTypes (optional) - _deprecated_

- `true` (**default**) - coerce scalar data types.
- `"array"` - in addition to coercions between scalar types, coerce scalar data to an array with one element and vice versa (as required by the schema).
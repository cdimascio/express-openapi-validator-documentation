---
title: ▪️ coerceTypes (optional)
---

  Determines whether the validator will coerce the request body. Request query and path params, headers, cookies are coerced by default and this setting does not affect that.

  See [additional details](assets/docs/coercion.md) on coercion and limitiations.

## Options Schema
```javascript
coerceTypes: false | true | 'array
```

## `coerceTypes` (optional)

  - `true` - coerce scalar data types.
  - `false` - (**default**) do not coerce types. (more strict, safer)
  - `"array"` - in addition to coercions between scalar types, coerce scalar data to an array with one element and vice versa (as required by the schema).

  For example:

  ```javascript
  validateRequests: {
    coerceTypes: true,
  }
  ```

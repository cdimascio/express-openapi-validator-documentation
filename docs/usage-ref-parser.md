---
title: ▪️ $refParser
---

Determines how JSON schema references are resolved by the internal [json-schema-ref-parser](https://github.com/APIDevTools/json-schema-ref-parser). Generally, the default mode, `bundle` is sufficient, however if you use [escape characters in \$refs](https://swagger.io/docs/specification/using-ref/), `dereference` is necessary.


!!! Option-schema
    ```javascript
    #refParser: {
        mode: 'bundle' | 'dereference'
    }
    ```
## $refParser.mode (optional)

- `bundle` **(default)** - Bundles all referenced files/URLs into a single schema that only has internal $ref pointers. This eliminates the risk of circular references, but does not handle escaped characters in $refs.
- `dereference` - Dereferences all $ref pointers in the JSON Schema, replacing each reference with its resolved value. Introduces risk of circular $refs. Handles [escape characters in \$refs](https://swagger.io/docs/specification/using-ref/))

!!! tip
    `dereference`, for some use cases, proves more reliable, however it cannot handle circular types.

See this [issue](https://github.com/APIDevTools/json-schema-ref-parser/issues/101#issuecomment-421755168) for more information.

```javascript
$refParser: {
  mode: 'bundle',
}
```
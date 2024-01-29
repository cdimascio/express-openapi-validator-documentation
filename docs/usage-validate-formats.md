---
title: ▪️ validateFormats
---

Specifies the strictness of validation of string formats.

!!! note
    ```javascript
    validateFormats: 'fast' | 'full' | false
    ```

## validateFormats (optional)

- `"fast"` (**default**) - only validate syntax, but not semantics. E.g. `2010-13-30T23:12:35Z` will pass validation even though it contains month 13.
- `"full"` - validate both syntax and semantics. Illegal dates will not pass.
- `false` - do not validate formats at all.


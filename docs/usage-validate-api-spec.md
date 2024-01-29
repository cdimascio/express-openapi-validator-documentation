---
title: ▪️ validateApiSpec
---

Determines whether the validator should validate the OpenAPI specification. Useful if you are certain that the api spec is syntactically correct and want to bypass this check.

_Warning:_ Be certain your spec is valid. And be sure you know what you're doing! express-openapi-validator _*expects*_ a valid spec. If incorrect, the validator will behave erratically and/or throw Javascript errors.

!!! Option-schema
    ```javascript
    {
        validateApiSpec: true | false
    }
    ```
## validateApiSpec (optional)

- `true` (**default**) - validate the OpenAPI specification.
- `false` - do not validate the OpenAPI specification.

---
title: ▪️ unknownFormats
---

Defines how the validator should behave if an unknown or custom format is encountered.

!!! Option-schema
    ```javascript
    unknownFormats: true 'ignore' | ['<format>', ...]
    ```

## unknownFormats (optional)

- `true` **(default)** - When an unknown format is encountered, the validator will report a 400 error.
- `[string]` **_(recommended for unknown formats)_** - An array of unknown format names that will be ignored by the validator. This option can be used to allow usage of third party schemas with format(s), but still fail if another unknown format is used.
  

    ```javascript
    unknownFormats: ['phone-number', 'uuid'],
    ```

- `"ignore"` - to log warning during schema compilation and always pass validation. This option is not recommended, as it allows to mistype format name and it won't be validated without any error message.
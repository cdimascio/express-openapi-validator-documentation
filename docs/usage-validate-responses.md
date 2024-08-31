---
title: ▪️ validateResponses
---

Determines whether the validator should validate responses. Additionally, it accepts an object of configuragble response validation options.

!!! Option-Schema
    ```
    validateResponses: false | true | {
        removeAdditional: 'failing',
        coerceTypes: true | false | 'array',
        onError: (error, body, req): void,
        allErrors: false | true,
    }
    ```

## validateResponses
- `true` - validate responses in 'strict' mode i.e. responses MUST match the schema.
- `false` (**default**) - do not validate responses
- `{ ... }` - validate responses with options

    - ### `removeAdditional`

        Determines whether the validator will remove additional properties, prior to validation.

        **Options Schema**
        ```javascript
        removeAdditional: 'failing'
        ```

        - `"failing"` - additional properties that fail schema validation are automatically removed from the response.

    - ### `coerceTypes`

        - `true` - coerce scalar data types.
        - `false` - (**default**) do not coerce types. (almost always the desired behavior)
        - `"array"` - in addition to coercions between scalar types, coerce scalar data to an array with one element and vice versa (as required by the schema).

    - ### `onError`

        - `(error, body, req): void` - A function that will be invoked on response validation error, instead of the default handling. Useful if you want to log an error or emit a metric, but don't want to actually fail the request. Receives the validation error, the offending response body, and the express request object.

            For example:

            ```javascript
            validateResponses: {
                onError: (error, body, req) => {
                    console.log(`Response body fails validation: `, error);
                    console.log(`Emitted from:`, req.originalUrl);
                    console.debug(body);
                }
            }
            ```

    - ### `allErrors`

        > This option was introduced in version 5.4.0, where the default behavior of response validation was changed to stop after the first failure.

        Determine's whether all validation rules should be checked and all failures reported. By default, validation stops after the first failure. This option passes through to AJV, see [AJV Options: allErrors](https://ajv.js.org/options.html#allerrors).

        **Do NOT use allErrors in production**  
        Following the [recommended best practices by AJV](https://ajv.js.org/security.html#security-risks-of-trusted-schemas), this option should be left unset, or set to `false` in production to help mitigate slow validations and potential ReDOS attacks.

        **Option Schema**
        ```javascript
        allErrors: false | true
        ```

        - `true` - all rules should be checked and all failures reported
        - `false` - (**default**) stop checking rules after the first failure

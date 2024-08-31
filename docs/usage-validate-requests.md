---
title: ▪️ validateRequests
---

Determines whether the validator should validate requests.

!!! Option-Schema
    ```javascript
    validateRequests: true | false | {
        allowUnknownQueryParameters: false | true,
        coerceTypes: false | true | 'array',
        removeAdditional: false | true | 'all' | 'failing',
        allErrors: false | true,
    }
    ```

## validateRequests (optional)


- `true` (**default**) - validate requests.
- `false` - do not validate requests.
- `{ ... }` - validate requests with options

    - ### `coerceTypes`

        Determines whether the validator will coerce the request body. Request query and path params, headers, cookies are coerced by default and this setting does not affect that.

        See [additional details](assets/docs/coercion.md) on coercion and limitations.

        **Option Schema**
        ```javascript
        coerceTypes: false | true | 'array'
        ```

        - `true` - coerce scalar data types.
        - `false` - (**default**) do not coerce types. (more strict, safer)
        - `"array"` - in addition to coercions between scalar types, coerce scalar data to an array with one element and vice versa (as required by the schema).

    -  ### `allowUnknownQueryParameters`
        
        Determines whether to allow undefined query parameters to pass validation.

        **Option Schema**
        ```javascript
        allowUnknownQueryParameters: false | true
        ```

        - `true` - enables unknown/undeclared query parameters to pass validation
        - `false` - (**default**) fail validation if an unknown query parameter is present


        !!! Note

            `allowUnknownQueryParameters` is set for the globally on the validator. It can be overwritten on a per-operation basis by using the custom OpenAPI vendor extension `x-allow-unknown-query-parameters`.

            The following example allows unknown query parameters for the annotated endpoint:

            ```yaml
            paths:
              /allow_unknown:
                get:
                  x-allow-unknown-query-parameters: true ## <--- overrides the global setting
                  parameters:
                    - name: value
                      in: query
                      schema:
                      type: string
                  responses:
                    200:
                      description: success
            ```

    -  ### `removeAdditional`
        
        Determines whether to keep or remove additional properties in request body or to fail validation if schema has `additionalProperties` set to `false`. For further details, refer to [AJV documentation](https://ajv.js.org/docs/validation.html#removing-additional-properties)

        **Option Schema**
        ```javascript
        removeAdditional: false | true | 'all' | 'failing'
        ```

        - `false` (**default**) - not to remove additional properties
        - `"all"` - all additional properties are removed, regardless of additionalProperties keyword in schema (and no validation is made for them).
        - `true` - only additional properties with additionalProperties keyword equal to false are removed.
        - `"failing"` - additional properties that fail request schema validation will be removed (where additionalProperties keyword is false or schema).

    - ### `allErrors`

        > This option was introduced in version 5.4.0, where the default behavior of request validation was changed to stop after the first failure.

        Determine's whether all validation rules should be checked and all failures reported. By default, validation stops after the first failure. This option passes through to AJV, see [AJV Options: allErrors](https://ajv.js.org/options.html#allerrors).

        **Do NOT use allErrors in production**  
        Following the [recommended best practices by AJV](https://ajv.js.org/security.html#security-risks-of-trusted-schemas), this option should be left unset, or set to `false` in production to help mitigate slow validations and potential ReDOS attacks.

        **Option Schema**
        ```javascript
        allErrors: false | true
        ```

        - `true` - all rules should be checked and all failures reported
        - `false` - (**default**) stop checking rules after the first failure

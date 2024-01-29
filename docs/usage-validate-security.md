---
title: ▪️ validateSecurity
---

Determines whether the validator should validate securities e.g. apikey, basic, oauth2, openid, etc

!!! Option-schema
    ```javascript
    validateSecurity: true | false | {
        handlers: {
            [auth-type]: (req, scopes, schema): void
        }
    }
    ```

## validateSecurity

- `true` (**default**) - validate security
- `false` - do not validate security
- `{ ... }` - validate security with `handlers`. See [Security handlers](#security-handlers) doc.

  **handlers:**

  For example:

  ```javascript
  validateSecurity: {
    handlers: {
      ApiKeyAuth: function(req, scopes, schema) {
        console.log('apikey handler throws custom error', scopes, schema);
        throw Error('my message');
      },
    }
  }
  ```
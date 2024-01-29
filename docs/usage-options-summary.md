---
title: Summary of Options
---
## OpenApiValidator Middleware Options

The `express-openapi-validator` middleware provides sensible defaults, requiring minimal upfront configuration. However, it also provides flexibility and customization through a vast array of parameters and options.

The following example includes the complete set of customizable options. Each option is explained in detail within its options usage page.

```javascript
OpenApiValidator.middleware({
  apiSpec: './openapi.yaml',
  validateRequests: true,
  validateResponses: true,
  validateApiSpec: true,
  validateSecurity: {
    handlers: {
      ApiKeyAuth: (req, scopes, schema) => {
        throw { status: 401, message: 'sorry' }
      }
    }
  },
  validateFormats: 'fast',
  formats: [{
    name: 'my-custom-format',
    type: 'string' | 'number',
    validate: (value: any) => boolean,
  }],
  unknownFormats: ['phone-number', 'uuid'],
  serDes: [
    OpenApiValidator.serdes.dateTime,
    OpenApiValidator.serdes.date,
    {
      format: 'mongo-objectid',
      deserialize: (s) => new ObjectID(s),
      serialize: (o) => o.toString(),
    },
  ],
  operationHandlers: false | 'operations/base/path' | { ... },
  ignorePaths: /.*\/pets$/,
  ignoreUndocumented: false,
  fileUploader: { ... } | true | false,
  $refParser: {
    mode: 'bundle'
  },
});
```
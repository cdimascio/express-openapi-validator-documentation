---
title: Advanced usage and options
---
## Advanced Usage

### OpenApiValidator Middleware Options

express-openapi validator provides a good deal of flexibility via its options.

Options are provided via the options object. Options take the following form:

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








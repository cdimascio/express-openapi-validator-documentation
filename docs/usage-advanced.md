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




### ▪️ serDes (optional)

Defines custom serialization and deserialization behavior for schemas of type `string` that declare a `format`. By default, `Date` objects are serialized as `string` when a schema's `type` is `string` and `format` is `date` or `date-time`.

e.g.

```javascript
// If `serDes` is not specified, the following behavior is default
serDes: [
  OpenApiValidator.serdes.dateTime.serializer,
  OpenApiValidator.serdes.date.serializer,
],
```

To create custom serializers and/or deserializers, define:

- `format` (required) - a custom 'unknown' format that triggers the serializer and/or deserializer
- `deserialize` (optional) - upon receiving a request, transform a string property to an object. Deserialization occurs _after_ request schema validation.
- `serialize` (optional) - before sending a response, transform an object to string. Serialization occurs _after_ response schema validation
- `jsonType` (optional, default 'object') - set to override for deserialized types that are not 'object', eg 'array'

e.g.

```javascript
serDes: [
   // installs dateTime serializer and deserializer
  OpenApiValidator.serdes.dateTime,
  // installs date serializer and deserializer
  OpenApiValidator.serdes.date,
  // custom serializer and deserializer for the custom format, mongo-objectid
  {
    format: 'mongo-objectid',
    deserialize: (s) => new ObjectID(s),
    serialize: (o) => o.toString(),
  },
],
```

The mongo serializers will trigger on the following schema:

```yaml
type: string
format: mongo-objectid
```

See [mongo-serdes-js](https://github.com/pilerou/mongo-serdes-js) for additional (de)serializers including MongoDB `ObjectID`, `UUID`, ...

### ▪️ operationHandlers (optional)

Defines the base directory for operation handlers. This is used in conjunction with express-openapi-validator's OpenAPI vendor extensions, `x-eov-operation-id`, `x-eov-operation-handler` and OpenAPI's `operationId`. See [example](https://github.com/cdimascio/express-openapi-validator/tree/master/examples/3-eov-operations).

Additionally, if you want to change how modules are resolved e.g. use dot delimited operation ids e.g. `path.to.module.myFunction`, you may optionally add a custom `resolver`. See [documentation and example](https://github.com/cdimascio/express-openapi-validator/tree/master/examples/5-custom-operation-resolver)

- `string` - the base directory containing operation handlers
- `false` - (default) disable auto wired operation handlers
- `{ ... }` - specifies a base directory and optionally a custom resolver

  **handlers:**

  For example:

  ```javascript
  operationHandlers: {
    basePath: __dirname,
    resolver: function (modulePath, route): express.RequestHandler {
      ///...
    }
  }
  ```

```
operationHandlers: 'operations/base/path'
```

**Note** that the `x-eov-operation-handler` OpenAPI vendor extension specifies a path relative to `operationHandlers`. Thus if `operationHandlers` is `/handlers` and an `x-eov-operation-handler` has path `routes/ping`, then the handler file `/handlers/routes/ping.js` (or `ts`) is used.

Complete example [here](https://github.com/cdimascio/express-openapi-validator/tree/master/examples/3-eov-operations)

**api.yaml**

```yaml
/ping:
  get:
    description: |
      ping then pong!
    # OpenAPI's operationId may be used to to specify the operation id
    operationId: ping
    # x-eov-operation-id may be used to specify the operation id
    # Used when operationId is omitted. Overrides operationId when both are specified
    x-eov-operation-id: ping
    # specifies the path to the operation handler.
    # the path is relative to the operationHandlers option
    # e.g. operations/base/path/routes/ping.js
    x-eov-operation-handler: routes/ping
    responses:
      '200':
        description: OK
        # ...
```

**routes/ping.js**

`x-eov-operation-handler` specifies the path to this handlers file, `ping.js`

`x-eov-operation-id` (or `operationId`) specifies operation handler's key e.g. `ping`

```javascript
module.exports = {
  ping: (req, res) => res.status(200).send('pong'),
};
```

### ▪️ ignorePaths (optional)

Defines a regular expression or function that determines whether a path(s) should be ignored. If it's a regular expression, any path that matches the regular expression will be ignored by the validator. If it's a function, it will ignore any paths that returns a truthy value.

The following ignores any path that ends in `/pets` e.g. `/v1/pets`.
As a regular expression:

```
ignorePaths: /.*\/pets$/
```

or as a function:

```
ignorePaths: (path) => path.endsWith('/pets')
```

### ▪️ ignoreUndocumented (optional)

Disables any form of validation for requests which are not documented in the OpenAPI spec.

Defaults to `false`

### ▪️ fileUploader (optional)

Specifies the options to passthrough to multer. express-openapi-validator uses multer to handle file uploads. see [multer opts](https://github.com/expressjs/multer)

- `true` (**default**) - enables multer and provides simple file(s) upload capabilities
- `false` - disables file upload capability. Upload capabilities may be provided by the user
- `{...}` - multer options to be passed-through to multer. see [multer opts](https://github.com/expressjs/multer) for possible options

  e.g.

  ```javascript
  fileUploader: {
    dest: 'uploads/',
  }
  ```

### ▪️ \$refParser.mode (optional)

Determines how JSON schema references are resolved by the internal [json-schema-ref-parser](https://github.com/APIDevTools/json-schema-ref-parser). Generally, the default mode, `bundle` is sufficient, however if you use [escape characters in \$refs](https://swagger.io/docs/specification/using-ref/), `dereference` is necessary.

- `bundle` **(default)** - Bundles all referenced files/URLs into a single schema that only has internal $ref pointers. This eliminates the risk of circular references, but does not handle escaped characters in $refs.
- `dereference` - Dereferences all $ref pointers in the JSON Schema, replacing each reference with its resolved value. Introduces risk of circular $refs. Handles [escape characters in \$refs](https://swagger.io/docs/specification/using-ref/))

See this [issue](https://github.com/APIDevTools/json-schema-ref-parser/issues/101#issuecomment-421755168) for more information.

e.g.

```javascript
$refParser: {
  mode: 'bundle',
}
```

### ▪️ coerceTypes (optional) - _deprecated_

Determines whether the validator should coerce value types to match the those defined in the OpenAPI spec. This option applies **only** to path params, query strings, headers, and cookies. _It is **highly unlikely** that you will want to disable this. As such this option is deprecated and will be removed in the next major version_

- `true` (**default**) - coerce scalar data types.
- `"array"` - in addition to coercions between scalar types, coerce scalar data to an array with one element and vice versa (as required by the schema).
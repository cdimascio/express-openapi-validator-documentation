---
title: API Server with Operation Handlers
---

If you prefer not to manually map your OpenAPI endpoints to Express handler functions, `express-openapi-validator` can do it for you automatically! This example demonstrates how to utilize express-openapi-validator's OpenAPI `x-eov-operation-*` vendor extensions.

## Source Code and OpenAPI Spec

See the full example with source code and the OpenAPI spec.
- [Source Code](https://github.com/cdimascio/express-openapi-validator/tree/master/examples/3-eov-operations)
- [OpenAPI Spec](https://github.com/cdimascio/express-openapi-validator/blob/master/examples/3-eov-operations/api.yaml#L39)

## Overview

Here's a quick overview of how to use operation handlers:

1. Specify the `operationHandlers` option to set the base directory that contains your operation handler files.
    ```javascript
    app.use(
      OpenApiValidator.middleware({
        apiSpec,
        operationHandlers: path.join(__dirname),
      })
    );
    ```

2. Use the `x-eov-operation-id` OpenAPI vendor extension or `operationId` to specify the ID of the operation handler to invoke.
    ```yaml
    /ping:
      get:
        # operationId: ping
        x-eov-operation-id: ping
    ```

3. Use the `x-eov-operation-handler` OpenAPI vendor extension to specify a path (relative to `operationHandlers`) to the module that contains the handler for this operation.
    ```yaml
    /ping:
      get:
        x-eov-operation-id: ping
        x-eov-operation-handler: routes/ping # no .js or .ts extension
    ```

4. Create the express handler module, e.g., `routes/ping.js`.
    ```javascript
    module.exports = {
      // the express handler implementation for ping
      ping: (req, res) => res.status(200).send('pong'),
    };
    ```

**Note:** A file may contain one or many handlers.

## Code Snippets

### app.js

```javascript
const express = require('express');
const path = require('path');
const bodyParser = require('body-parser');
const logger = require('morgan');
const http = require('http');
const OpenApiValidator = require('express-openapi-validator');

const port = 3000;
const app = express();
const apiSpec = path.join(__dirname, 'api.yaml');

// Install bodyParsers for the request types your API will support
app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.text());
app.use(bodyParser.json());

app.use(logger('dev'));

app.use('/spec', express.static(apiSpec));

// Install the OpenApiValidator on your express app
app.use(
  OpenApiValidator.middleware({
    apiSpec,
    validateResponses: true, // default false
    // Provide the base path to the operation handlers directory
    operationHandlers: path.join(__dirname), // default false
  })
);

// With auto-wired operation handlers, you don't have to declare your routes!
// See api.yaml for x-eov-* vendor extensions

// Create a custom error handler
app.use((err, req, res, next) => {
  // format errors
  res.status(err.status || 500).json({
    message: err.message,
    errors: err.errors,
  });
});

http.createServer(app).listen(port);
console.log(`Listening on port ${port}`);

module.exports = app;
```

### api.yaml

```yaml
/ping:
  get:
    description: |
      ping then pong!
    # OpenAPI's operationId may be used to specify the operation id
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

### ping.js

```javacript
module.exports = {
  // ping must match operationId or x-eov-operation-id above
  // note that x-eov-operation-id overrides operationId
  ping: (req, res) => res.status(200).send('pong'),
};

```
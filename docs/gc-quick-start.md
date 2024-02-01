---
title: Quick start
---


## Quick Start

### 1. Require/import the openapi validator

!!! note

    _**Important:** Ensure express is configured with all relevant body parsers. Body parser middleware functions must be specified prior to any validated routes. See an [example](#example-express-api-server)_.

```javascript
const OpenApiValidator = require('express-openapi-validator');
```

or

```javascript
import * as OpenApiValidator from 'express-openapi-validator';
```


### 2. Install the middleware

```javascript
app.use(
  OpenApiValidator.middleware({
    apiSpec: './openapi.yaml',
    validateRequests: true, // (default)
    validateResponses: true, // false by default
  }),
);
```

### 3. Register an error handler

```javascript
app.use((err, req, res, next) => {
  // format error
  res.status(err.status || 500).json({
    message: err.message,
    errors: err.errors,
  });
});
```


## Advanced Usage (options)

See [Advanced Usage](usage-advanced.md) options to:

- inline api specs as JSON.
- configure request/response validation options
- customize authentication with security validation `handlers`.
- use OpenAPI 3.0.x 3rd party and custom formats.
- tweak the file upload configuration.
- ignore routes
- and more...



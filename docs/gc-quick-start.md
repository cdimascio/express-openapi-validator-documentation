---
title: Quick start
---


## Guides
To get up and running, follow the [three steps below](#quick-start). For a more in depth step-by-step guide, choose on of the following:


<div class="grid cards" markdown>

-   :fontawesome-brands-js:{ .lg .middle }  __Standard Guide__

    ---

    Automatically validate your new or existing express API against an OpenAPI specification using express-openapi-validator middleware.

    [:octicons-arrow-right-24: Get Started](guide-standard.md)

-   :fontawesome-brands-js:{ .lg .middle } __Operation Handler Guide__

    ---
    Automatically validate your API, mapping each OpenAPI path to its corresponding JavaScript/TypeScript function using our OpenAPI vendor extension!

    [:octicons-arrow-right-24: Get Started](guide-operation-handlers.md)


</div>

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



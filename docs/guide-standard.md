---
title: Standard Guide
---

# Standard Guide

## Overview

This guide will walk you through the process of integrating `express-openapi-validator` into your Express application to ensure that your API adheres to your OpenAPI specification.

### Prerequisites

Before you begin, make sure you have Node.js and npm installed. You can install them from https://nodejs.org/.

### Complete source code

See the complete [source code](https://github.com/cdimascio/express-openapi-validator/tree/master/examples/1-standard) and [OpenAPI spec](https://github.com/cdimascio/express-openapi-validator/blob/master/examples/1-standard/api.yaml)

## Step 1: Set Up Your Express Application

Start by creating a basic Express application. In your project directory, create a file (e.g., `app.js`) and add the following code:

```typescript
const express = require('express');
const path = require('path');
const http = require('http');
const app = express();
```

This code imports the necessary modules and initializes an Express application.

## Step 2: Configure Body Parsers

Set up body parsers for the request body types you expect. This must be done prior to defining your API endpoints. Add the following code:

```typescript
app.use(express.json());
app.use(express.text());
app.use(express.urlencoded({ extended: false }));
```

## Step 3: Serve the OpenAPI Specification (Optional)

If you want to serve your OpenAPI specification, add the following code (optional):

```typescript
const spec = path.join(__dirname, 'api.yaml');
app.use('/spec', express.static(spec));
```

Replace 'api.yaml' with the path to your OpenAPI specification file.

## Step 4: Install and Use express-openapi-validator Middleware

Now, install the `express-openapi-validator` middleware onto your Express app. Add the following code:

```typescript
const OpenApiValidator = require('express-openapi-validator');

app.use(
  OpenApiValidator.middleware({
    apiSpec: './api.yaml',
    validateResponses: true,
  })
);
```

Replace `./api.yaml` with the path to your OpenAPI specification file.

## Step 5: Define Your API Endpoints

Now, define your routes using Express. Here's an example:

```typescript
app.get('/v1/pets', function (req, res, next) {
  res.json([
    { id: 1, type: 'cat', name: 'max' },
    { id: 2, type: 'cat', name: 'mini' },
  ]);
});

app.post('/v1/pets', function (req, res, next) {
  res.json({ name: 'sparky', type: 'dog' });
});

app.get('/v1/pets/:id', function (req, res, next) {
  res.json({ id: req.params.id, type: 'dog', name: 'sparky' });
});

// Additional route for uploading files
app.post('/v1/pets/:id/photos', function (req, res, next) {
  // Handle file upload logic here
});

// 6. Create an Express error handler
app.use((err, req, res, next) => {
  // 7. Customize errors
  console.error(err);
  res.status(err.status || 500).json({
    message: err.message,
    errors: err.errors,
  });
});
```

## Step 6: Start the Server

Finally, create an HTTP server and listen on port 3000:

```typescript
http.createServer(app).listen(3000);
```

Save the file, and in your terminal, run:

```shell
node app.js
```

Visit [http://localhost:3000/spec](http://localhost:3000/spec) to see your OpenAPI specification, and start testing your API!

---
title: FAQ
---

### **Q**: How do I match paths, like those described in RFC-6570?

OpenAPI 3.0 does not support RFC-6570. That said, we provide a minimalistic mechanism that conforms syntactically to OpenAPI 3 and accomplishes a common use case. For example, matching file paths and storing the matched path in `req.params`

Using the following OpenAPI 3.x definition

```yaml
/files/{path}*:
  get:
    parameters:
      - name: path
        in: path
        required: true
        schema:
          type: string
```

With the following Express route definition

```javascript
  app.get(`/files/:path(*)`, (req, res) => { /* do stuff */ }`
```

A path like `/files/some/long/path` will pass validation. The Express `req.params.path` property will hold the value `some/long/path`.

---

### **Q:** Can I use discriminators with `oneOf` and `anyOf`?

- By default, only **top-level discriminators** are supported. See the [top-level discriminator example](https://github.com/cdimascio/express-openapi-validator/tree/master/examples/8-top-level-discriminator).
- To also enable **deep discriminator** support (nested within `oneOf` / `anyOf`), set the `discriminator` option under `validateRequests`:

```javascript
app.use(
    OpenApiValidator.middleware({
        apiSpec,
        validateRequests: {
            discriminator: true,
            //... other options
        }
    })
);
```
---

### **Q:** What happened to the `securityHandlers` property?

In v3, `securityHandlers` have been replaced by `validateSecurity.handlers`. To use v3 security handlers, move your existing security handlers to the new property. No other change is required. Note that the v2 `securityHandlers` property is supported in v3, but deprecated

---

### **Q**: What happened to the `multerOpts` property?

In v3, `multerOpts` have been replaced by `fileUploader`. In order to use the v3 `fileUploader`, move your multer options to `fileUploader` No other change is required. Note that the v2 `multerOpts` property is supported in v3, but deprecated

---

### **Q:** I can disallow unknown query parameters with `allowUnknownQueryParameters: false`. How can disallow unknown body parameters?

Add `additionalProperties: false` when [describing](https://swagger.io/docs/specification/data-models/keywords/) e.g a `requestBody` to ensure that additional properties are not allowed. For example:

```yaml
Pet:
additionalProperties: false
required:
  - name
properties:
  name:
    type: string
  type:
    type: string
```

---

### **Q:** Can I use `express-openapi-validator` with `swagger-ui-express`?

Yes. Be sure to `use` the `swagger-ui-express` serve middleware prior to installing `OpenApiValidator`. This will ensure that `swagger-ui-express` is able to fully prepare the spec before before OpenApiValidator attempts to use it. For example:

```javascript
const swaggerUi = require('swagger-ui-express')
const OpenApiValidator = require('express-openapi-validator')

...

app.use('/', swaggerUi.serve, swaggerUi.setup(documentation))

app.use(OpenApiValidator.middleware({
  apiSpec, // api spec JSON object
  //... other options
  }
}))
```

---

### **Q:** I have a handler function defined on an `express.Router`. If i call `req.params` each param value has type `string`. If i define same handler function on an `express.Application`, each value in `req.params` is already coerced to the type declare in my spec. Why not coerce theseF values on an `express.Router`?

First, it's important to note that this behavior does not impact validation. The validator will validate against the type defined in your spec.

In order to modify the `req.params`, express requires that a param handler be registered e.g. `app.param(...)` or `router.param(...)`. Since `app` is available to middleware functions, the validator registers an `app.param` handler to coerce and modify the values of `req.params` to their declared types. Unfortunately, express does not provide a means to determine the current router from a middleware function, hence the validator is unable to register the same param handler on an express router. Ultimately, this means if your handler function is defined on `app`, the values of `req.params` will be coerced to their declared types. If your handler function is declare on an `express.Router`, the values of `req.params` values will be of type `string` (You must coerce them e.g. `parseInt(req.params.id)`).

---
title: ▪️ apiSpec (required)
---

Specifies the path to an OpenAPI 3 specification or a JSON object representing the OpenAPI 3 specification

!!! Option-Schema

    ```javascript
    apiSpec: '/path/to/spec.yaml' | { 
      /*openapi-spec-json */
    }
    ```

## `apiSpec`

Takes a path to a API specification or the API specification object.

```javascript
apiSpec: './path/to/my-openapi-spec.yaml',
```

or

```javascript
apiSpec: {
  openapi: '3.0.1',
  info: {...},
  servers: [...],
  paths: {...},
  components: {
    responses: {...},
    schemas: {...}
  }
}
```
---
title: ▪️ ignorePaths
---

Defines a regular expression or function that determines whether a path(s) should be ignored. If it's a regular expression, any path that matches the regular expression will be ignored by the validator. If it's a function, it will ignore any paths that returns a truthy value.

The following ignores any path that ends in `/pets` e.g. `/v1/pets`.
As a regular expression:

!!! Option-schema
    ```javascript
    ignorePaths: <regex> | (path: string): boolean
    ```

## ignorePaths (optional)

```
ignorePaths: /.*\/pets$/
```

or as a function:

```
ignorePaths: (path) => path.endsWith('/pets')
```
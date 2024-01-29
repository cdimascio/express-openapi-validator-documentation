---
title: ▪️ formats
---

Defines a list of custom formats.

!!! note
    ```javascript
    formats: [{ name, type, validate: (v): boolean }]
    ```

## formats (optional)
- `[{ ... }]` - array of custom format objects. Each object must have the following properties:
  - name: string (required) - the format name
  - validate: (v: any) => boolean (required) - the validation function
  - type: 'string' | 'number' (optional) - the format's type

e.g.

```javascript
formats: [
  {
    name: 'my-three-digit-format',
    type: 'number',
    // validate returns true the number has 3 digits, false otherwise
    validate: (v) => /^\d{3}$/.test(v.toString()),
  },
  {
    name: 'my-three-letter-format',
    type: 'string',
    // validate returns true the string has 3 letters, false otherwise
    validate: (v) => /^[A-Za-z]{3}$/.test(v),
  },
];
```

Then use it in a spec e.g.

```yaml
my_property:
  type: string
  format: my-three-letter-format'
```

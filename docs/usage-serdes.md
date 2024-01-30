---
title: ▪️ serDes
---


Defines custom serialization and deserialization behavior for schemas of type `string` that declare a `format`. By default, `Date` objects are serialized as `string` when a schema's `type` is `string` and `format` is `date` or `date-time`.

!!! Option-schema
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

e.g.

```javascript
// If `serDes` is not specified, the following behavior is default
serDes: [
  OpenApiValidator.serdes.dateTime.serializer,
  OpenApiValidator.serdes.date.serializer,
],
```

## serDes (optional)
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

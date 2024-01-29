## Upgrading from 3.x

In v4.x.x, the validator is installed as standard connect middleware using `app.use(...) and/or router.use(...)` ([example](https://github.com/cdimascio/express-openapi-validator/blob/v4/README.md#Example-Express-API-Server)). This differs from the v3.x.x the installation which required the `install` method(s). The `install` methods no longer exist in v4.
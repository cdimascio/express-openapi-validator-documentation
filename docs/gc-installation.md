---
title: Installation
---

An OpenApi validator for ExpressJS that automatically validates API requests and responses using an OpenAPI 3 specification.

<p align="center">
<img src="https://raw.githubusercontent.com/cdimascio/express-openapi-validator/master/assets/express-openapi-validator-logo-v2.png" width="600">
</p>

🦋express-openapi-validator is an unopinionated library that integrates with new and existing API applications. express-openapi-validator lets you write code the way you want; it does not impose any coding convention or project layout. Simply, install the validator onto your express app, point it to your OpenAPI 3 specification, then define and implement routes the way you prefer. See an example.

## Install

### with npm

```shell
npm install express-openapi-validator
```

### with yarn

```shell
yarn add express-openapi-validator
```

## Upgrading from 3.x

!!! note 


    In v4.x.x, the validator is installed as standard connect middleware using `app.use(...) and/or router.use(...)` ([example](https://github.com/cdimascio/express-openapi-validator/blob/v4/README.md#Example-Express-API-Server)). This differs from the v3.x.x the installation which required the `install` method(s). The `install` methods no longer exist in v4.
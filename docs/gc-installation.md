---
title: Installation
---

An OpenApi validator for ExpressJS that automatically validates API requests and responses using an OpenAPI 3 specification.

<p align="center">
<img src="https://raw.githubusercontent.com/cdimascio/express-openapi-validator/master/assets/express-openapi-validator-logo-v2.png" width="600">
</p>


## Install

### with npm

```shell
npm install express-openapi-validator
```

### with yarn

```shell
yarn add express-openapi-validator
```

## OpenAPI 3.1 (support) alpha

_Replace x with the latest alpha version_

```shell
npm install express-openapi-validator@6.0.0-alpha.<X>
```

## Upgrading from 3.x

!!! note 


    In v4.x.x, the validator is installed as standard connect middleware using `app.use(...) and/or router.use(...)` ([example](https://github.com/cdimascio/express-openapi-validator/blob/v4/README.md#Example-Express-API-Server)). This differs from the v3.x.x the installation which required the `install` method(s). The `install` methods no longer exist in v4.

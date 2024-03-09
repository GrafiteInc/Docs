# CLI

!!! Warning
    This package is archived and no longer supported (2019-08-24)

A handy CLI tool for spinning up all the starter apps you may need.

## Installation

Installation is done globally with composer. Though we recommend using the `cgr` package to prevent dependency collisions.

```
composer global require grafite/cli
```

With this installed you have the freedom to spin up a collection of apps using the various Grafite packages.

```
$ grafite make:app {APP_NAME} --bootstrap
$ grafite make:cms {WEBSITE_NAME}
```

## Commands

### Make App

The make app command will create a new Laravel instance and then add the starter kit from Grafite Builder. This is great if you want to use a UI kit other than Bootstrap.

#### Bootstrap option
You guessed it, also sets up bootstrap on the app.

<!-- ### Saas option
This will set up the Grafite Builder Billing kit, this means you can set up a subscription with your app and mamage your users. It uses `laravel/cashier` and is ready to start working with Stripe out of the box. For more information checkout: [Grafite Builder Billing](https://docs.grafite.ca/builder/billing) -->

### Make CMS

Make CMS works just like the `make:app` command meaning it will set up a basic app using Laravel and Grafite Builder, it will by default use bootstrap for the UI kit. It then adds in the use of Grafite CMS. It will publish the theme to the app along with the basic Controllers and routes for accessing contents of the CMS. Please consult the [Grafite CMS Docs](https://docs.grafite.ca/archived/cms) for more information.

<!-- ### E-commerce Option

Let's say you want to set things up for a custom e-commerce platform that uses Stripe and works seamlessly with Grafite CMS, well, this option has you covered. It will add the commerce package to your application without manual work on your part. This way you can focus on customizing it into what you need. -->


# Blacksmith

Blacksmith was designed for Laravel developers who use Laravel Forge. It helps keep a record of what your application infra build looks like. You can then easily rebuild a replica of any of your servers or sites quickly, especially when you hit server bugs that are not worth wasting hours trying to fix.

[Source Code](https://github.com/grafiteinc/blacksmith)

!!! warning "Blacksmith assumes you use Laravel Forge"

!!! tip "Create your ENV tokens in your bash profile and you don't have to specify per project."

## Setup

```sh
composer require grafite/blacksmith
```

## ENV Variables required

```sh
BLACKSMITH_FORGE_TOKEN=
BLACKSMITH_AWS_ACCESS_KEY_ID=
BLACKSMITH_AWS_SECRET_ACCESS_KEY=
BLACKSMITH_AWS_DEFAULT_REGION=
BLACKSMITH_AWS_BUCKET=
```

You can set these per project or, if its more helpful you can set them in your bash profile.

## Commands

You will see that in a few cases you need to define the envrionments. This pertains mainly to ENVs and deploy scripts. Blacksmith assumes you have similar setups for the server via Laravel Forge.

```sh
artisan blacksmith:setup {--domain=} {--server=} {--site=}
```

To get the ball rolling with Blacksmith, you can either run the setup command which will produce a configuration for youre expected server needs.

```sh
artisan blacksmith:localize {--server=}
```

If you already have an app on Forge and wish to bring its configuration locally. You can do so with the localize command. It will roughly match your blacksmith confiuration to the server and sites you already have set up.

```sh
artisan blacksmith:backup
```

Perhaps you want to keep some backups of your various configurations since by default `.blacksmith` directory is added to `.gitignore`. You can run this command.

!!! danger "Make sure at all costs you are storing this data in a secure backup location."

```sh
artisan blacksmith:build-server {ip} {ip} {--name=} {--ubuntu=} {--php=} {--private_ip=}
```

In the event you're facing server issues that you cannot resolve. There is an easy way to build a new one with this command. Simply create your server via any host. Then run this with the IP provided. You can set other details as needed. It will produce a new config for Laravel Forge.

```sh
artisan blacksmith:build-site {--server=} {--site=}
```

In the event you need to build a site again on a new server, you should first build the site using the `build-server` command.
Blacksmith will remove the default site if the server is a fresh build, if not it will delete the site with a matching ID.

If you're doing a fresh build of a site and have not set a configuration blacksmith will make a suggestion. Otherwise, if performing a rebuild, simply copy over your previous configuration and blacksmith will update the ID accordingly.

```sh
artisan blacksmith:update-server {--server=}
```

You can initiate a server update and if you want to specify the server you can, otherwise blacksmith will crawl all server configurations and update them accordingly.

!!! warning "Changing the PHP version will install a new one, but it cannot set it to default, there is no API for that at this time."

```sh
artisan blacksmith:update-site {--server=} {--site=}
```

You can initiate a site update which is you do not specify the server or site then blacksmith will crawl all server and site configurations and update them accordingly.
This update will reset all queue workers if a configuration is specified, same with security rules and redirect rules.

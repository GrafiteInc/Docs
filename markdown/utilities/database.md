# Database

Laravel is incredibly powerful yet we felt there could be some handy additions for working with MySQL Databases.

[Source Code](https://github.com/grafiteinc/database)

!!! warning "The database package is only tested with MySQL"

## Setup

```sh
composer require grafite/database
```

You have to set these database details in your `.env`:
```
username
password
host
```

## Create

When developing locally it is rather annoying to have to go to TablePlus or your tool of choice and generate a new database. This command just does it for you.

```
php artisan db:create {name}
```

## Drop

After practising on a database you may decide especially in your local environment that its of no value any longer, this command will let you quickly drop the database.

```
php artisan db:drop {name}
```

## Backup

Using Spatie's DbDumper package this command will generate a MySQL dump of your database. This can be useful if you want to create a dummy database for testing which you can anonymize and backup then use the `restore` command to initiate for tests or local development uses.

```
php artisan db:backup
```

## Restore

The restore command simply looks for the latest backup file and restores that instance of the MySQL dump. This is most effective for local setup of projects and testing etc.

```
php artisan db:restore
```
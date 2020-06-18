# Database

Laravel is incredibly powerful yet we felt there could be some handy additions for working with MySQL Databases.

[Source Code](https://github.com/grafiteinc/database)

!!! warning "The database package is only tested with MySQL"

## Setup

```sh
composer require grafite/database
```

You have to set these database details in your `.env`:
```env
DB_HOST=127.0.0.1
DB_USERNAME=root
DB_PASSWORD=
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

With backup you can also specify the tables you want to backup.

```
php artisan db:backup {--tables=}
```

## Restore

The restore command simply looks for the latest backup file and restores that instance of the MySQL dump. This is most effective for local setup of projects and testing etc.

If you have a specific backup you want to restore you can run it with the `--backup` option otherwise it will default to the `db-backup.sql` file.

```
php artisan db:restore {--backup=}
```

## Upload

You can also choose to upload a backup file to a filesystem of your choosing. You simply create a disk in your Laravel config called `backup` and then run the following command:

The filename is what you wish to call the backup on the backup disk, the package will append `.sql` to the filename.

```
php artisan db:upload {filename} {backup}
```

## Download

Similar to the upload you may wish to download a database dump locally so you can restore it.

You can specify the filename you want for the downloaded file and set the file from your uploads disk.

```
php artisan db:download {filename} {uploaded-backup-filename}
```

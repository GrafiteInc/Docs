# Upgrade Guide

Regarding upgrades
-----
From time to time we may do upgrades to Grafite CMS that can involve breaking changes. We would like to explain the two main ways of handling the upgrades that occur in Grafite CMS. We will specify if there are migrations to do be done inside the Release notes for each release.

## Migrations
We highly recommend running migrations after upgrading Grafite CMS. Just to make sure you're covered.

```
php artisan migrate
```

## Compatibility

Due to the rebranding of Grafite CMS we had to draw a line, and cause breaking changes. The following is a general guide to upgrading, showing what is compatible and what is not.

| Package Version | Broken Compatibility | Database Changes | Theme Compatibility |
|-----------------|-----------------|-----------------|-----------------|
| 3.2.x | None | None | Frontend/Backend |
| 3.0.x - 3.1.x | None | None | Frontend/Backend |
| 2.4.x - 3.0.x | Quarx namespace, config file name, composer autoload, backend UI, and more | None | Frontend (partial - change quarx to cms) |
| 1.4.x - 2.0.x | None | Backward compatible changes | Frontend/Backend |

## From Quarx to Grafite CMS

!!! Warning
    This guide is partial there may be other parts that you extended or changed that are fundamentally broken and will require extensive work to refactor. We appologize for the inconvenience.

### Cosmetic
Any customizations done to the Backend UI will likely be incompatible since the backend upgraded to Bootstrap 4. Your theme for the frontend will be compatable though any reference to `quarx` will need to change to `cms` in the blade files.

### Everything Else
This release involved MANY changes that break various parts of the application. If your site was build with the CLI tool for Quarx and is just a CMS website then you can likely rebuild it with the new Grafite CLI and copy over your theme. If you added Quarx to your applicaiton this is more trickly. You will need to correct the namespaces in the Controllers, routes, middleware etc. You could potentially publish these assets again and delete the initial Quarx ones if you didn't modify them originally.

```
Http/Controllers/BlogController.php
Http/Controllers/EventsController.php
Http/Controllers/FaqController.php
Http/Controllers/GalleryController.php
Http/Controllers/PagesController.php
```

You'll also have to rename `quarx.php` in the routes and config to `cms.php`.

The first line of the routes file will need to resemble this now:

```
Route::group(['namespace' => 'Cms', 'middleware' => ['cms-language', 'cms-analytics']], function () {
```

For Middleware you'll need to adjust the references in the `Kernel.php` and update the files to:

```
GrafiteCms.php
GrafiteCmsApi.php
GrafiteCmsLanguage.php
```

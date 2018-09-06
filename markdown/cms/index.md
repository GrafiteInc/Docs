# Getting Started

<div class="logo">
    <span class="cms-icon"></span>
</div>

Grafite CMS is a simple decoupled Laravel CMS. It provides numerous components that enable developers to put together powerful applications but also provide the means for people to edit the content of those powerful applications, and websites.
Grafite CMS comes with a module builder for all your custom CMS needs, as well as a module migration and publishing tools. So if you decide to reuse some modules on future projects you can easily publish thier assets seamlessly, you can even package them into composer packages.

---

## Installation

Create a new Laravel application, and make a database somewhere and update the .env file.

* Run the following command:

```bash
composer require grafite/cms
```

* Then run the vendor publish:

```bash
php artisan vendor:publish --provider="Grafite\Cms\GrafiteCmsProvider"
```

!!! Tip "If you wish to use the publish datetime picker - set your app's timezone config to correspond with your location"

## Simple Setup

!!! warning "The simple setup requires a fresh Laravel application."

If you're looking to do a simple website with a powerful CMS, and the only people logging on to the app are the CMS managers.
Then you can run the setup command, it will install everything it needs, run its migrations and give you a login to start with.
Take control of your website in seconds.

```php
php artisan grafite:cms-setup
```

Now you're done setup. Login, and start building your amazing new website!

You can login as an admin with the following credentials:

```
U: admin@example.com
P: admin
```

## Complex Setup

!!! warning "Complex setup is needed for applications that have already have existing code including the login/logout set up."

If you just want to add Grafite CMS to your existing application and already have your own application running then follow the instructions below:

* Add the following to your routes provider:

```php
require base_path('routes/cms.php');
```

* Add the following to your app.scss file, you will want to modify depending on your theme of choice.

```css
@import "resources/themes/default/assets/sass/_theme.scss";
```

* Then migrate:

```bash
php artisan migrate
```

* Then add to the Kernel Route Middleware:

```php
'cms' => \App\Http\Middleware\GrafiteCms::class,
'cms-api' => \App\Http\Middleware\GrafiteCmsApi::class,
'cms-language' => \App\Http\Middleware\GrafiteCmsLanguage::class,
'cms-analytics' => \Grafite\Cms\Middleware\GrafiteCmsAnalytics::class,
```

In order to have modules load as well please edit the autoload psr-4 portion to your composer file:
```json
"autoload": {
    ...
    "psr-4": {
        "App\\": "app/",
        ...
        "Cms\\": "cms/"
        }
}
```

## CMS Access
Route to the administration dashboard is "/cms/dashboard".

Grafite CMS requires Grafite Builder to run (only for the FormMaker), but Grafite CMS does not require you to use the Grafite Builder version of roles. But you will still need to ensure some degree of control for Grafite CMS's access. This is done in the Grafite CMS Middleware, using the gate and the Grafite CMS Policy. If you opt in to the roles system provided by Grafite Builder, then you can replace 'cms' with admin to handle the Grafite CMS authorization, if not, you will need to set your own security policy for access to Grafite CMS. To do this simply add the Grafite CMS policy to your `app/Providers/AuthServiceProvider.php` file, and ensure that any rules you wish it to use are in within the policy method. We suggest a policy similar to below.

Possible CMS Access Policy:
```
Gate::define('cms', function ($user) {
    return (bool) $user;
});

Gate::define('cms-api', function ($user) {
    return true;
});
```

Or Using Grafite Builder:
```
Gate::define('cms', function ($user) {
    return ($user->roles->first()->name === 'admin');
});
```

### Fun Route Trick

If you're looking for clean URL pages without having to have the URL preceed with `page` or `p` then you can
add this to your routes.

> Make sure you put it at the bottom of the routes or it may conflict with others.

```php
Route::get('{url}', function ($url) {
    return app(App\Http\Controllers\Cms\PagesController::class)->show($url);
})->where('url', '([A-z\d-\/_.]+)?');
```

### Roles & Permissions (simple setup only)

With the roles middleware you can specify which roles are applicable separating them with pipes: `['middleware' => ['roles:admin|moderator|member']]`.

The Grafite CMS middleware utilizes the roles to ensure that a user is an 'admin'. But you can elaborate on this substantially, you can create multiple roles, and then set their access in your app, using the roles middleware. But, what happens when you want to allow multiple roles to access the CMS but only allow Admins to access your custom modules? You can use permissions for this. Similar to the roles middleware you can set the permissions `['middleware' => ['permissions:admin|cms']]`. You can set custom permissions in `config/permissions.php`. This means you can set different role permissions for parts of your CMS, giving you even more control.

## API Endpoints

Grafite CMS comes with a collection of handy API endpoints if you wish to use them. You can define your own policies for access and customize the middleware as you see fit.

#### Token

The basic Grafite CMS API endpoints must carry the Grafite CMS `apiToken` defined in the config for the app. This can be provided by adding the following to any request:

```
?token={your token}
```

** All published and public facing data will be available via the API by default.

```
/cms/api/blog
/cms/api/blog/{id}
/cms/api/events
/cms/api/events/{id}
/cms/api/faqs
/cms/api/faqs/{id}
/cms/api/files
/cms/api/files/{id}
/cms/api/images
/cms/api/images/{id}
/cms/api/pages
/cms/api/pages/{id}
/cms/api/widgets
/cms/api/widgets/{id}
```

## Images

Images are resized on upload for a better quality response time. They follow the guidelines specified in the `config` under `cms.max-image-size`.

## S3

Regarding S3 bucket usage. You will need to set the permissions accordingly to allow images to be saved to your buckets. Then you need to set your buckets to allow public viewing access.
This is an example of such a policy.

```
{
    "Version":"2008-10-17",
    "Statement":[{
        "Sid":"AllowPublicRead",
        "Effect":"Allow",
        "Principal": {
            "AWS": "*"
        },
        "Action":["s3:GetObject"],
        "Resource":["arn:aws:s3:::MY_BUCKET/public/images/*"]
    }]
}
```

Replace `MY_BUCKET` with your bucket name.

## FileSystem Config

If using S3 you will need to add the following line to your filesystem config: `'visibility' => 'public',`

Also Provides
------
The Grafite CMS package also provides the following packages:

* DevFactory/Minify
* Grafte/Builder

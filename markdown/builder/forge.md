# Application Forge

The FORGE component provides you with access to the FORGE API in your admin panel. Rather than having to log into FORGE for each adjustment, now
you can simply log into your own application and in the admin panel adjust the scheduler, or workers on your server configuration.

##### Requires
```php
composer require themsaid/forge-sdk
```

## Setup

```php
php artisan grafite:forge
```

You may want to add this line to your navigation:

```html
<li><a href="{{ url('admin/forge/settings') }}"><span class="fa fa-fw fa-server"></span> Forge Settings</a></li>
<li><a href="{{ url('admin/forge/scheduler') }}"><span class="fa fa-fw fa-calendar"></span> Forge Calendar</a></li>
<li><a href="{{ url('admin/forge/workers') }}"><span class="fa fa-fw fa-cogs"></span> Forge Workers</a></li>
```

Add this to the `app/Providers/RouteServiceProvider.php` in the `mapWebRoutes(Router $router)` function:

You will see a line like: `->group(base_path('routes/web.php'));`

You need to change it to resemble this:
```php
->group(function () {
    require base_path('routes/web.php');
    require base_path('routes/forge.php');
}
```

Add these to the .env:
```php
FORGE_TOKEN=
FORGE_SERVER_ID=
FORGE_SITE_ID=
```
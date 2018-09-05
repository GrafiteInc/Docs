# Application Logs

The logs tool simply add a view of the app logs to your admin panel. This can be of assistance durring development or in keeping an application in check.

## Setup

```php
php artisan grafite:logs
```

You may want to add this line to your navigation:

```html
<li class="nav-item"><a href="{!! url('admin/logs') !!}"><span class="fa fa-line-chart"></span> Logs</a></li>
```

Add this to the `app/Providers/RouteServiceProvider.php` in the `mapWebRoutes(Router $router)` function:

```php
require base_path('routes/logs.php');
```

# Application Notifications

Grafite Builder's notifications will build a basic controller, service, and views for both users and admins so you can easily notifiy your users, in bulk or specifically.

## Setup

```php
php artisan grafite:notifications
```

You may want to add this line to your navigation:

```html
<li><a href="{!! url('user/notifications') !!}"><span class="fa fa-envelope-o"></span> Notifications</a></li>
<li><a href="{!! url('admin/notifications') !!}"><span class="fa fa-envelope-o"></span> Notifications</a></li>
```

Add this to the `app/Providers/RouteServiceProvider.php` in the `mapWebRoutes(Router $router)` function:

```php
require base_path('routes/notification.php');
```

## Facades
Provides the following tool for in app notifications:

```php
Notifications::notify($userId, $flag, $title, $details);
```

<small>Flags can be any bootstrap alert: default, info, success, warning, danger</small>

## What Notifications Publishes:

The command will overwrite any existing files with the notification version of them:

* app/Facades/Notifications.php
* app/Http/Controllers/Admin/NotificationController.php
* app/Http/Controllers/User/NotificationController.php
* app/Http/Requests/NotificationRequest.php
* app/Models/Notification.php
* app/Services/NotificationService.php
* database/migrations/2016_04_14_180036_create_notifications_table.php
* resources/views/admin/notifications/create.blade.php
* resources/views/admin/notifications/edit.blade.php
* resources/views/admin/notifications/index.blade.php
* resources/views/notifications/index.blade.php
* resources/views/notifications/show.blade.php
* routes/notification.php
* tests/NotificationIntegrationTest.php
* tests/NotificationServiceTest.php

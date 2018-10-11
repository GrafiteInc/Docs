# Social Media Logins

If you're looking to offer social media logins on your application and want a simple way to get started then look no further. Simply run the command and follow the steps below and you'll have GitHub login out of the box. Integrating Facebook etc afterward is easy when you look at the code base.

##### Requires
```php
composer require laravel/socialite
```

## Setup
```
php artisan grafite:socialite
```

The next step is to prepare your app with socialite:

Add this to your app config under providers:
```php
Laravel\Socialite\SocialiteServiceProvider::class
```

Add this to your app config under aliases:
```php
'Socialite' => Laravel\Socialite\Facades\Socialite::class,
```

Add `require base_path('routes/socialite.php');` to the `app/Providers/RouteServiceProvider.php` in the `mapWebRoutes(Router $router)` function:
```php
Route::middleware('web')
    ->namespace($this->namespace)
    ->group(function () {
        require base_path('routes/socialite.php');
        require base_path('routes/web.php');
    });
```

Finally set the access details in the services config:
```php
'github' => [
    'client_id' => 'id code',
    'client_secret' => 'secret code',
    'redirect' => 'http://your-domain/auth/github/callback',
    'scopes' => ['user:email'],
],
```

## What Socialite publishes
The command will overwrite any existing files with the socialite version of them:

* app/Http/Controllers/Auth/SocialiteAuthController.php
* routes/socialite.php

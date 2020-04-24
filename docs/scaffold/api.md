# Basic API

If you feel like opting in for the Laracogs starter kit. You can also easily build in an API layer. Running the <code>grafite:api</code> command will set up the bare bones components, but you can also use the API tools as a part of the CRUD now by using the <code>--api</code> option.

##### Requires
```php
composer require tymon/jwt-auth
```

## Setup
```
php artisan grafite:api
```

Essentially you want to do all the basic setup for JWT such as everything in here:
Then follow the directions regarding installation on: [https://github.com/tymondesigns/jwt-auth/wiki/Installation](https://github.com/tymondesigns/jwt-auth/wiki/Installation)

Add this to the `app/Providers/RouteServiceProvider.php` file in the `mapWebRoutes(Router $router)` function:
```php
require base_path('routes/api.php');
```

Add to the app/Http/Kernal.php under routeMiddleware:
```php
'jwt.auth' => \Tymon\JWTAuth\Middleware\GetUserFromToken::class,
'jwt.refresh' => \Tymon\JWTAuth\Middleware\RefreshToken::class,
```

Add to except attribute the app/Http/Middleware/VerifyCsrfToken.php (You also have to do this for CRUDs you add):
```php
'api/v1/login',
'api/v1/user/profile',
```

If you use Apache add this to the .htaccess file:
```php
RewriteEngine On
RewriteCond %{HTTP:Authorization} ^(.*)
RewriteRule .* - [e=HTTP_AUTHORIZATION:%1]
```

Also update your jwt config file and set the user to:

```php
\App\Models\User::class
```

## What API publishes
The command will overwrite any existing files with the api version of them:

* app/Http/Controllers/Api/AuthController.php
* app/Http/Controllers/Api/UserController.php
* routes/api.php

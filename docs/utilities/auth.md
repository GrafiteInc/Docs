# Auth

Based on Laravel's UI package this Auth package is a stripped down version with zero assumptions of your front-end framework choices. It uses simple authentication measures and basic form handling.

[Source Code](https://github.com/grafiteinc/auth)

## Make Auth

Exactly the way it works in Laravel UI. We run the following command to create our auth stubs.

```php-inline
php artisan make:auth --force=false
```

This will add all the files needed an append to our `web.php` routes file the required routes for authentication.

### Middlewares
You can also define middlewares as the second parameter in the `routes()` method, which will apply to the POST routes in this collection.

```
GrafiteAuth::routes([
    'login' => true,
    'logout' => true,
    'register' => true,
    'reset' => true,
    'confirm' => true,
    'verify' => true,
], [
    'throttle:5,1'
]);
```
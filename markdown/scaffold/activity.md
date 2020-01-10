# Application Activities

Sometimes its handy to know what your users are up to when they browse your site or application. The Activity kit gives you everything you need to track your users and their every action. The middleware does most of it for you, but your welcome to customize it to fit your needs.

## Setup

```php
php artisan grafite:activity
```

Add to your `config/app.php` the following:

```php
App\Providers\ActivityServiceProvider::class,
```

## Helper

```php
activity($description) // will log the activity
```

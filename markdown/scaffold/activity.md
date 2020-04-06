# Application Activities

Sometimes its handy to know what your users are up to when they browse your site or application. The Activity kit gives you everything you need to track your users and their every action. The middleware does most of it for you, but your welcome to customize it to fit your needs.

## Helper

```php
activity($description) // will log the activity
```

## Middleware

```php
App\Middleware\Activity

activity('Standard User Action');
```

This will log the time and actions taken on which URLs allowing you to track accurately which portions of your application get the most attention.

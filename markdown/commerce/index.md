# Getting Started

<div class="logo">
    <span class="commerce-icon"></span>
</div>

Obviously everyone likes to develop custom modules for their apps, and we all love the idea of selling stuff to make some extra cash on the side. Therefore, we're introducing Grafite Commerce. So you decided to start a blog or a website featuring your hobby or whatever, and now you're looking to make some sales. Simply pull in the Grafite Commerce package and within minutes you'll have a e-commerce platform up and running. Stylize it as much or as little as you want, set up some custom shipping and tracking number details, set your shipping rates and poof you've got a store.

Grafite Commerce is built in a way that allows for maximum customization with considerable simplicity in upgrading as time goes on. Initial publication will provide you with the controllers and a `StoreLogistics` service that you can hook your custom code into. The entire system is built using **Stripe** for very obvious reasons.

---

## Installation

```
composer require grafite/commerce
composer require laravel/cashier
```

Don't worry about the laravel cashier installation, the only points of interest are specified below:

Add these to your `config/app.php`:

```
Grafite\Commerce\GrafiteCommerceModuleProvider::class,
Laravel\Cashier\CashierServiceProvider::class,
```

## Setup

Then publish the vendor assets etc:

```php
php artisan vendor:publish
```

Add the following to your `app/Http/Kernel.php` to the `routeMiddleware` array:
```
'isAjax' => \Grafite\Commerce\Http\Middleware\isAjax::class,
```

Add the following trait to the `app/Models/User.php`:
```
use Grafite\Commerce\Services\Concerns\hasFavorites;
```

Add the following to your `app/Providers/RouteServiceProvider.php` to the `mapWebRoutes` method inside the group method as a closure:
```
require base_path('routes/commerce.php');
```

Don't worry about the laravel cashier installation, the only points of interest are specified below:

Change your `config/services.php` to match the following:

```php
'stripe' => [
    'model' => App\Models\UserMeta::class,
    'key' => env('STRIPE_KEY'),
    'secret' => env('STRIPE_SECRET'),
],
```

Now you need to add the Billable trait to the `App\Models\UserMeta::class`

```php
use \Laravel\Cashier\Billable;
```

Then migrate!

```php
php artisan migrate
```

If you wish to maintain consistency with the store accross your login, user settings etc you can set the extends to `@extends('quazar-frontend::layouts.store')`
Views you may wish to change for optimal consistency:

```
views/
    auth/
        login.blade.php
        register.blade.php
        passwords/
            email.blade.php
            reset.blade.php
    user/
        password.blade.php
        settings.blade.php
```

## Notes on Grafite CMS & Builder
Grafite Commerce is intented to be used with Grafite CMS so use outside of that context is to be done at your own risk. Similarly, though Grafite CMS is able to be added to any existing Laravel 5.6+ application, the documentation above is assuming you used the Grafite CMS setup command `php artisan graifte:cms` command, which is heavily integrated with [Grafite Builder](https://builder.grafite.ca). If you did not, you may have to make adjustments that are not listed here.
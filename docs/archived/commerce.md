# Commerce

!!! Warning
    This package is archived and no longer supported (2020-01-10)

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

If you wish to maintain consistency with the store accross your login, user settings etc you can set the extends to `@extends('commerce-frontend::layouts.store')`
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
Grafite Commerce is intented to be used with Grafite CMS so use outside of that context is to be done at your own risk. Similarly, though Grafite CMS is able to be added to any existing Laravel 5.6+ application, the documentation above is assuming you used the Grafite CMS setup command `php artisan graifte:cms` command, which is heavily integrated with Grafite Builder. If you did not, you may have to make adjustments that are not listed here.

### LogisticService

The logistic service is published to your app specifically. The intention is for it to handle **hooks** on the various events that occur within your store purchase flow. The `LogisticService` handles the following events:

```php
shipping($user)
getTaxPercent($user)
afterPurchase($user, $transaction, $cart, $result)
afterSubscription($user, $plan)
afterRefundRequest($transaction)
afterRefund($transaction)
cancelSubscription($user, $plan)
afterPlaceOrder($user, $transaction, $cart)
orderCreated($order)
shipOrder($order)
cancelOrder($order)
```

## Extending

With any e-commerce platform there is a need to expand to fit your custom needs. With Grafite Commerce we have made this as easy as possible. We publish out the JavaScript files for cart interaction, and controllers and routes for general "browsing" of your store.

---

### Public files:
- js/store.js
- js/card.js
- js/purchases.js
- css/shop.css

Within the `public` directory you will find a couple published CSS and JavaScript files which handle the elements of the Grafite Commerce experience. Feel free to change these as you desire but be mindful of URL changes since they may require route changes.

### Controllers

There are a few controllers added to your app in the Grafite Commerce namespace. These are general controllers which you may wish to customize based on your app's setup. This is similar to Grafite CMS in the sense that you may wish to change certain parts. Be very mindful of how much you can impact the functionality of the Grafite Commerce store base by changing these files.
Within the following namespace you should find these controllers which can be customized.

```
app/Http/Controllers/Commerce/
```

- CardController.php
- CartController.php
- CheckoutController.php
- FavoriteController.php
- OrderController.php
- PlanController.php
- ProductController.php
- ProfileController.php
- PurchaseController.php
- StoreController.php
- SubscriptionController.php

### Routes

Routes can be customized in the `routes/commerce.php` file. Be mindful of changes here that can impact your JavaScript files above.

### Views

There are Grafite Commerce view files that are published your app which you can modify, however, if you wish to edit the package views then you will need to update them in the `resources/commerce` directory.

## Admin UI

There are some very easy to use Admin components set up with Grafite Commerce but feel free to clone the repo and add as you please.

## Config

Grafite Commerce has a handful of config options.

| Key | Description |
| ------ | ----- |
| name | The name of the store |
| currency | The currency for your store, in relation to products |
| taxes_include_shipping | Whether or not you want the taxes to include the shipping rate |
| store_url_prefix | The store URL prefix (if changed you MUST update the published JS files as well) |
| currencies | Available currencies for subscription plan generating |
| subscriptions | If you want to enable/disable subscriptions as items in the store |
| forms | Forms config for commerce components |

## General Requirements
1. PHP 7.1.3+
2. MySQL 5.7+

## More Specific Requirements
3. OpenSSL
4. Laravel 5.6+
5. Grafite CMS 3.0+
6. Stripe Account

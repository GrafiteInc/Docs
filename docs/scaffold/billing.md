# Stripe Based Billing

If you feel like opting in for the Grafite Builder starter kit. You can also get your app's billing handled quickly with Grafite Builder billing command and `Laravel/Cashier`. Grafite Builder's billing will build a basic controller, views etc so you can stay focused on what makes your application amazing!

##### Requires
```php
composer require laravel/cashier
```

## Setup
```
php artisan grafite:billing
```

You have to modify the `app/Providers/RouteServiceProvider.php` in the `mapWebRoutes(Router $router)` function:

You will see a line like: `->group(base_path('routes/web.php'));`

You need to change it to resemble this:
```php
->group(function () {
    require base_path('routes/web.php');
    require base_path('routes/billing.php');
}
```

Add these to the .env:
```php
SUBSCRIPTION=app_basic
STRIPE_SECRET=secret-key
STRIPE_PUBLIC=public-key
```

Add this to the app/Providers/AuthServiceProvider.php:
```php
Gate::define('access-billing', function ($user) {
    return ($user->meta->subscribed('main') && is_null($user->meta->subscription('main')->endDate));
});
```

And for the `config/services.php` you will want yours to resemble:
```php
'stripe' => [
    'model'  => App\Models\UserMeta::class,
    'key'    => env('STRIPE_PUBLIC'),
    'secret' => env('STRIPE_SECRET'),
],
```

Finally run migrate to add the subscriptions and bind them to the user meta:
```php
php artisan migrate
```

You will also want to update your webpack mix file to resemble (webpack.mix.js):
```js
.js([
    'resources/assets/js/app.js',
    'resources/assets/js/card.js',
    'resources/assets/js/subscription.js'
], 'public/js');
```

### After Setup
You may want to add this line to your navigation:

```php
<li><a href="{{ url('user/billing/details') }}"><span class="fa fa-dollar"></span> Billing</a></li>
```

### Notes
You may want to switch the line in the view vendor.receipt to:

```php
<strong>To:</strong> {{ $user->user()->email }}
```

We do this because rather than bind the billing to the User, we bound it to the UserMeta.

## What Billing Publishes

The command will overwrite any existing files with the billing version of them:

* app/Http/Controllers/User/BillingController.php
* app/Models/UserMeta.php
* config/invoice.php
* config/plans.php
* database/migrations/2016_02_26_000647_create_subscriptions_table.php
* database/migrations/2016_02_26_000658_add_billings_to_user_meta.php
* resources/assets/js/card.js
* resources/assets/js/subscription.js
* resources/views/billing/card-form.blade.php
* resources/views/billing/change-card.blade.php
* resources/views/billing/details.blade.php
* resources/views/billing/invoices.blade.php
* resources/views/billing/coupons.blade.php
* resources/views/billing/subscribe.blade.php
* resources/views/billing/tabs.blade.php
* routes/billing.php

## Accounts

The Account service is a tool for handling your subscription details, meaning you can limit your applications based on various clauses using the plans config. You're free to set these plan details restricting things within a billing cycle or just in general.

These are relative to *billing* only. They provide extra tools for handling restrictions in your application based on the plan the user subscribed to. Unless you implment the cashier billing system with the UserMeta structure provided by Laracogs it will not benefit you.

### Config
This is the basic config for `config/plans.php`. This is for a single plan, either be subscribed or not.

!!! tip "Remember you need to have corresponding plans on Stripe ex. app_basic by default"

```
'subscription' => env('SUBSCRIPTION'),
'subscription_name' => 'main',
'plans' => [
    'app_basic' => [
        'access' => [
            'some name'
        ],
        'limits' => [
            'Model\Namespace' => 5,
            'pivot_table' => 1
        ],
        'credits' => [
            'column' => 'credits_spent',
            'limit' => 10
        ],
        'custom' => [
            'anything' => 'anything'
        ],
    ],
]
```

#### Multiple Plans

In this config you can define multiple plans which can have different rules per plan. By default the kit uses a single plan. You can define this in the env as mentioned above. But if you want to do multiple plans you can change the following code:

1. Line 45 of the `BillingController.php` change `config('plans.subscription')` to: `$payload['plan']`
2. Add `name`, and `stripe_id` to the config
```
'subscription_name' => 'main',
'plans' => [
    'basic' => [
        'name' => 'Basic Subscription',
        'stripe_id' => 'basic_subscription',
        'access' => [
            'some name'
        ],
        'limits' => [
            'Model\Namespace' => 5,
            'pivot_table' => 1
        ],
        'credits' => [
            'column' => 'credits_spent',
            'limit' => 10
        ],
        'custom' => [
            'anything' => 'anything'
        ],
    ],
]
```
3. Then add the following code in `resources/views/billing/subscribe.blade.php` above the card form include:

```html
<div class="form-group">
    <label class="form-label" for="plan">Plan</label>
    <select class="form-control" name="plan" id="plan">
        @foreach (config('plans.plans') as $plan)
            <option value="{{ $plan['stripe_id'] }}">{{ $plan['name'] }}</option>
        @endforeach
    </select>
</div>
```

!!! tip "You may also want to add a component to your app to allow users to switch plans. You can use the swap method to achieve this: `auth()->user()->meta->subscription(config('plans.subscription_name'))->swap($request->plan)`"

### Service Methods

<br>
#### currentBillingCycle()
By using this method at the beginning and chaining another method after it you enable the restriction of the query to be performed but restricting itself to the current billing cycle.

<br>
#### canAccess($area)
Retuns a boolean if the user can enter

<br>
#### cannotAccess($area)
Retuns a boolean if the user cannot enter

<br>
#### getClause($key)
Returns the specified clause of the plans

<br>
#### getLimit($model)
Returns the specified limit of a model or pivot table

<br>
#### withinLimit($model, $key = 'user_id', $value = {user's id})
Confirms that the specified model only has the specified amount collected by the $key and $value.
* If you add the `currentBillingCycle()` before this then it would add the further restrictions.

<br>
#### creditsUsed($model, $key = 'user_id', $value = {user's id})
Reports back the specified amount collected by the $key and $value.
* If you add the `currentBillingCycle()` before this then it would add the further restrictions.

<br>
#### creditsAvailable($model)
Returns the number of credits remaining based on the config and the previous method.
* If you add the `currentBillingCycle()` before this then it would add the further restrictions.

<br>
#### clause($key, $method, $model = null)
Performs a custom method with rules defined by the clause name in the config. The Closure method in this is provided with the following parameters: $user, $subscription, $clause, $query


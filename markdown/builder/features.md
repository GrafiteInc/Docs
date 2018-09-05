# Application Features

Sometimes what we need is a simple way to toggle parts of an app on and off, or hey, maybe the client wants it. Either way, using the feature management kit can
take care of all that. Now you or your clients can toggle signups on an off, or any other features in the app. Just utilize the blade or helper components to take full control of your app.

## Setup

```php
php artisan grafite:features
```

You may want to add this line to your navigation:

```html
<li class="nav-item"><a class="nav-link" href="{!! url('admin/features') !!}"><span class="fa fa-cog"></span> Features</a></li>
```

Add to your `config/app.php` the following:

```php
App\Providers\FeatureServiceProvider::class,
```

Add this to the `app/Providers/RouteServiceProvider.php` in the `mapWebRoutes(Router $router)` function:

```php
require base_path('routes/features.php');
```

## Facades
Provides the following tool for in app features:

```php
Features::isActive($key);
```

## Blade

```php
@feature($key)
// code goes here
@endfeature
```

## Helper

```php
feature($key) // will return true|false
```

## What Features Publishes:

The command will overwrite any existing files with the features version of them:

* app/Facades/Features.php
* app/Http/Controllers/Admin/FeatureController.php
* app/Http/Requests/FeatureCreateRequest.php
* app/Http/Requests/FeatureUpdateRequest.php
* app/Models/Feature.php
* app/Providers/FeatureServiceProvider.php
* app/Services/FeatureService.php
* database/migrations/2016_04_14_210036_create_features_table.php
* resources/views/admin/features/create.blade.php
* resources/views/admin/features/edit.blade.php
* resources/views/admin/features/index.blade.php
* routes/features.php
* tests/Feature/FeatureIntegrationTest.php
* tests/Unit/FeatureServiceTest.php

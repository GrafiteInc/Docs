# Application Starter Kit

!!! warning "Make sure you followed the getting started instructions!"

Grafite Builder provides an elegant solution for starting an application by building the most basic views, controllers, models and migrations for your application. No need to use the `php artisan make:auth` because now you can easily start your whole application with this single command:

```
php artisan grafite:starter
```
!!! tip "BUT, before we do that lets get a few things set up."

In order to make use of the <u>starter kit</u> you will need to modify some files. Check out the modifications below:

Add the following to your `app/Http/Kernel.php` in the `$routeMiddleware` array.

```php
'admin' => \App\Http\Middleware\Admin::class,
'permissions' => \App\Http\Middleware\Permissions::class,
'roles' => \App\Http\Middleware\Roles::class,
'active' => \App\Http\Middleware\Active::class,
```

If you don't want to worry about email activation then remove this from the route's middleware array:
```php
'active'
```

Update the `App\User::class` in: 'config/auth.php' and 'database/factories/UserFactory.php' to this:

```php
App\Models\User::class
```

Add the following to 'app/Providers/AuthServiceProvider.php' in the boot method

```php
Gate::define('admin', function ($user) {
    return ($user->roles->first()->name === 'admin');
});

Gate::define('team-member', function ($user, $team) {
    return ($user->teams->find($team->id));
});
```

Add the following to 'app/Providers/EventServiceProvider.php' in the $listen property

```php
'App\Events\UserRegisteredEmail' => [
    'App\Listeners\UserRegisteredEmailListener',
],
```

You will want to create an sqlite memory test database in the `config/database.php`

```php
'testing' => [
    'driver'   => 'sqlite',
    'database' => ':memory:',
    'prefix'   => '',
],
```

Add the following line to the 'phpunit.xml' file
```xml
<env name="DB_CONNECTION" value="testing"/>
<env name="MAIL_DRIVER" value="log"/>
```

### Regarding Email Activation

The Starter kit has an email activation component added to the app to ensure your users have validated their email address.
You can disable it by removing the `active` middleware from the `web` routes. You will also have to disable the Notification but it
won't cause any problems if you remove the email activation.

### For Laravel 5.2 and later
You will also need to set the location of the email for password reminders. (config/auth.php - at the bottom)

```php
'passwords' => [
    'users' => [
        'provider' => 'users',
        'email' => 'emails.password',
        'table' => 'password_resets',
        'expire' => 60,
    ],
],
```

#### Things to note
You may try and start quickly by testing the registration but please make sure your app's <u>email</u> is configured or it will throw an exception.
You can do this in the `.env` file easily by setting it to 'log' temporarily

```php
MAIL_DRIVER=log
```

#### Last Steps

Once you've added in all these parts you will want to run the starter command!

```php
php artisan grafite:starter
```

Then you'll have to refresh the list of all classes that need to be included in the project.

```php
composer dump-autoload
```

Then you'll need to migrate to add in the users, user meta, roles and teams tables. The seeding is run to set the initial roles for your application.

```php
php artisan migrate --seed
```

Once you get the starter kit running you can register and login to your app. You can then you can visit the settings section of the app and set your role to admin to take full control of the app.

## What Starter Publishes

#### Controllers
Grafite Builder updated the basic controllers to handle things like creating a profile when a user is registered, as well as setting default return routes to `dashboard` etc. It also provides contollers for handling profile modifications and pages, team management etc. The admin controller handles the admin of users, modifying a user provided the user has the admin role.

* app/Http/Controllers/
    * Admin/
        * DashboardController.php
        * UserController.php
        * RoleController.php
    * Auth/
        * ActivateController.php
        * ForgotPasswordController.php
        * LoginController.php
        * RegisterController.php
        * ResetPasswordController.php
    * User/
        * PasswordController.php
        * SettingsController.php
    * PagesController.php
    * TeamController.php

#### Middleware
Grafite Builder overwrites the default middleware due to changes in the redirects. It also provides the `Admin` middleware for route level protection relative to roles.

* app/Http/Middleware/
    * Active.php
    * Admin.php
    * Permissions.php
    * RedirectIfAuthenticated.php
    * Roles.php

#### Requests
There are requests provided for handling the creation of Teams and updating of all components. Here we integrate the rules required that are able to run the validations and return errors. (If you're using Grafite Builder FormMaker Facade then it will even handling accepting the errors and highlighting the appropriate fields.)

* app/Http/Requests/
    * PasswordUpdateRequest.php
    * RoleCreateRequest.php
    * TeamCreateRequest.php
    * TeamUpdateRequest.php
    * UserInviteRequest.php
    * UserUpdateRequest.php

#### Routes
Given that there are numerous routes added to handle teams, profiles, password etc all routes are overwritten with the starter kit.

* routes/web.php

#### Config
The permissions config file is published, this is a way for you to set access levels and types of permissions `Roles` can have

* config/permissions.php

#### Events
The events for various actions.

* app/Events/
    * UserRegisteredEmail.php

#### Listeners
The event listeners for various actions.

* app/Listeners/
    * UserRegisteredEmailListener.php

#### Models
Models are obvious, but when we then integrate Services below which handle all the buisness logic etc which make the calls to the models we implement SOLID practices, the Controller, Console or other Service, which calls the service only accesses the model through it. Once these have been integrated please ensure you delete the `User.php` model file and ensure that you have followed the installation and config instructions.

* app/Models/
    * UserMeta.php
    * User.php
    * Team.php
    * Role.php

#### Notifications
These are all our emails that we need to send out to the users in the application. These are amazing since they use the power of Laravel's notifcation component.

* app/Notficiations/
    * ActivateUserEmail.php
    * NewAccountEmail.php
    * ResetPasswordEmail.php

#### Services
Service structure allows us to keep the buisness logic outside of the models, and controllers. This approach is best suited for apps that may wish to integrate an API down the road or other things. It also allows for a highly testable structure to the application.

* app/Services/
    * Traits/
        * HasRoles.php
        * HasTeams.php
    * ActivateService.php
    * RoleService.php
    * TeamService.php
    * UserService.php

#### Database
Please ensure that all migrations and seeds are run post installation. These seeds set the default roles available in your application.

* database/factories/
    * RoleFactory.php
    * TeamFactory.php
    * UserFactory.php
    * UserMetaFactory.php
* database/migrations/
    * 2015_11_30_191713_create_user_meta_table.php
    * 2015_11_30_215038_create_roles_table.php
    * 2015_11_30_215040_create_role_user_table.php
    * 2015_12_04_155900_create_teams_table.php
    * 2015_12_04_155900_create_teams_users_table.php
* database/seeds/
    * DatabaseSeeder.php
    * RolesTableSeeder.php
    * UserTableSeeder.php

#### Views
The views consist of as little HTML as possible to perform the logical actions. These are intended to be the most basic, and all of which are intended to be modified.

* resources/views/
    * admin/
        * roles/
            * edit.blade.php
            * index.blade.php
            * invite.blade.php
        * users/
            * edit.blade.php
            * index.blade.php
            * invite.blade.php
        * dashboard.blade.php
    * auth/
        * activate/
            * email.blade.php
            * token.blade.php
        * passwords/
            * email.blade.php
            * reset.blade.php
        * login.blade.php
        * register.blade.php
    * errors/
        * 401.blade.php
        * 404.blade.php
        * 503.blade.php
    * partials/
        * errors.blade.php
        * message.blade.php
        * status.blade.php
    * team/
        * create.blade.php
        * edit.blade.php
        * index.blade.php
        * show.blade.php
    * user/
        * meta.blade.php
        * password.blade.php
        * settings.blade.php
    * dashboard.blade.php

#### Tests
Grafite Builder starter kit provides the basic unit tests for each of its own parts. This provides some great examples of testing for building an application quickly.

* tests/
    * Feature/
        * TeamIntegrationTest.php
    * Unit/
        * UserServiceTest.php
        * TeamServiceTest.php
        * RoleServiceTest.php

## After Setup

### Dashboard access

The application dashboard is found by browsing to the /dashboard endpoint.
The default admin user login credentials are:

* email: admin@example.com
* password: admin

### User

The user model is expanded with Grafite Builder Starter Kit. It adds to the basic user model: roles, teams, and user meta. The relationships are as follows:

* Meta: hasOne
* Roles: belongsToMany
* Team: belongsToMany

It also provides the following methods:

```
meta() // The relationship method
roles() // The relationship method
hasRole(role_name) // checks if user has role
teams() // The relationship method
isTeamMember(team_id) // checks if user is member
isTeamAdmin(team_id) // checks if user is admin level member
```

### Middleware

#### Admin
The Admin middleware acts as a tool for setting admin level permissions on the routes or controller level.

```
['middleware' => 'admin']
```

This simple addition to a route will ensure the user has access to the admin level, if not it will return them from where they came.

#### Active
The Active middleware acts checks if the account as been activated by accessing the activate url with the emailed token.

```
['middleware' => 'active']
```

This simple addition to a route will ensure the user has an activated account, if not it will redirect them to the /active page so they can request another activation token if necessary.

#### Roles
The Roles middleware allows you to set custom roles for your routes.

```
['middleware' => 'roles:admin|member']
```

#### Permissions
The Permissions middleware allows you to set custom permissions (a subset of roles) for your routes

```
['middleware' => 'permissions:admin|somethingDescriptive']
```

You can set permissions in the `config/permissions.php`

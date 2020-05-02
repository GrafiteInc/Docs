# Roles & Permissions

Scaffold has a simple but powerful roles and permissions component built right in. Rather than pull in extra packages that require time to learn as well as time to tweak to fit your needs, we've covered most use cases out of the box.

## Roles
High level dynamic values are editable in the UI.

### Intent

Roles are intended to be dynamic use cases. You give a user a specified role and then define that roles permissions which apply to it. This gives two layers of control over what people can do in your app. This covers most app permission control layers.

### Middleware

If the user matches any of the roles listed in the middleware value then the request is processed. Otherwise it throws a 401 abort.

```php-inline
'middleware' => 'roles:admin,member'
```

## Permissions
Low level static values set in the config.

### Intent

Are intended to be set in the config and define access to various models and performing various actions on various models. Ex. user.create. Note that you can use dot notation in the middleware definitions in your routes.

The following is an example of a permissions config:

```php-inline
'roles' => [
    'create' => 'Create Roles',
    'update' => 'Update Roles',
    'delete' => 'Delete Roles',
],

'users' => [
    'invite' => 'Invite Users',
    'activity' => 'View User Activity',
    'update' => 'Update Users',
    'delete' => 'Delete Users',
],
```

### Permissions Middleware

If any permission defined in the route middleware is in the user's permission collection then the request will pass, otherwise it will abort with a 401.

```php-inline
'middleware' => 'permissions:roles|users'
'middleware' => 'permissions:roles.create'
```

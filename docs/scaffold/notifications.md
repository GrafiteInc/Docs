# Notifications

Scaffold comes with a new prebuilt notifications that work elegantly in the basic app structure. In general you have 2 main forms of notifying a user, you can either email them or you can leave them a notification in the app. In order to streamline the process we made some handy Notification components in Scaffold.

## Email Based

Any time you need to email a user some information, you can utilize the `StandardEmail` notification.

```php-inline
$notification = new StandardEmail('this is a test');

auth()->user()->notify($notification);
```

This will fire off an email using the standard email templates from Laravel.

## In App Based

Sometimes you don't need to email a user or, more so, you don't want to email the user. In these cases you can use the `InAppNotification`.

```php-inline
$notification = new InAppNotification('this is a test');
$notification->isImportant();

auth()->user()->notify($notification);
```

## Helpers

Both of these obviously come with helpers so you can easily all them as follows:

```php-inline
app_notify($message, $isImportant)
```

```php-inline
email_notify($subject, $message)
```

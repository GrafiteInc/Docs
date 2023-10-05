# Support

Sometimes its just nice to have a package which can contain some handy little tools for those special cases that often show up in an application's lifecylce.

[Source Code](https://github.com/grafiteinc/support)

## Helpers

Helpers are handy tools for regular things we have to do in our applications such as String manipulation or extraction.

### RouteHelper

`route_link_class($route, $active = 'active', $class = 'nav-link')`

This global method will let you handle auto setting a link as active.

### Session Helper

`session_error_message()`

This will implode all errors from the responses with error messages.

`javascript_session_data($nonce = false)`

This helper method lets use initialize an app with a simple notion of `window.app` with `window.app.session` which we can use for various error and success message notification tools, including Laravel Echo etc.

In order to have this above method work correctly you need to add this trait to your User Model: `HasJavascriptData`

### Stringy

Stringy is a helper for handling extraction of key information from strings. To begin using Stringy we simply call it as `Stringy::of($string)`.

## Methods

```
keywords() // get the keywords of a string
phrases() // get the phrases of a string
wordCount() // get the word count of a string
characterCount() // get the character count of a string
keySentence() // get the key sentence from a string
summary() // create a summary of the string
calculate() // perform a math caluation of a string ex. 2 * 4 / 2
hashtags() // get the hashtags in a string
urls() // get the urls from a string
mentions() // get the mentions from a string
ipAddresses() // get the IP addresses in a string
insert(array $data) // insert key values from your data into your string `:age` is swapped for `['age' => 47]`
mask($pattern) // convert a string to match the pattern
asPostalCode() // convert string to proper postal code format
asPhone() // convert the string to phone number format
asPlainText() // output the string as plain text
```

## Model Concerns

Sometimes in our apps we have models that have similar conerns. We use traits to handle these, below are concerns we find commonly used in many applications.

### CanAccessFeatures

A simple feature wrapper tool.

Config example:
You can run with Global or User based features.
```
[
    // Global based
    // 'portal' => env('FEATURE_PORTAL', false),

    // User based
    // 'portal' => [
    //     'state' => env('FEATURE_PORTAL', false),
    //     'users' => env('FEATURE_PORTAL_USERS', [])
    //],
]
```

To check you can easily check for access with `$user->hasFeatureAccess($key)`

### DatabaseSearchable

Provides the following methods:

```
search($string) // Search all model columns for a match
```

### HasCachedValues

Provides the following methods:

```
cacheIdentifier($string) // creates a custom identifier for a model item you wish to cache
clearCachedValues() // relies on a $caches property to be set identifying any strings used in the above method
```

### Observable

Provides the following methods:

```
bootObservable() // Enables auto-registering of any Observers for the model
```


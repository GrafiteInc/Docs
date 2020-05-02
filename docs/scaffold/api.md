# API

Scaffold comes with a fully functional API layer. It handles authentication using basic auth using user generated tokens. Tokens can be reset and are encrypted so they're only visible on the moment of reset.

## Endpoints

The build endpoints uses the `api.php` routes. In only contains 3 endpoints by default.

```
api/me
api/update
api/destroy
```

Each has separate handles and the output of the `me` endpoint uses the `UserResource` for handling its explicit output.

## Base Controller

The base controller for the API controllers contains in the constructor method a handling which sets the `user` property of the controller to the `auth('api')->user()`. This way you don't have to keep rewriting that accessor.


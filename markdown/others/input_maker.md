# Input Maker

The nice part about the input maker is that its the core of the form maker only pulled out. So this way you can reduce your HTML writing significanly with its blade directives or helpers.

## Blade Directives

```
@input_maker_label()
@input_maker_create()
```

## Helpers

```
input_maker_label()
input_maker_create()
```

## Facades

```
InputMaker::label()
InputMaker::create()
```

## Common Components

## Simple Fields For Everything!

The label generator is the easiest:

```
input_maker_label('name', ['class' => 'something'])
```

The input generator has a few more parts:

```
input_maker_create($name, $field, $object = null, $class = 'form-control', $reformatted = false, $populated = true)
```

The $field paramter is an array which can be highly configured.

### Example $field Config

```
[
    'type' => '', // defaults to standard text input
    'placeholder' => 'User Name Goes Here!',
    'alt_name' => 'User Name',
    'custom' => 'custom DOM attributes etc, can be array or string',
    'default_value' => 'Clark Kent',
    'class' => 'css class names',
    'before' => '<span class="input-group-addon" id="basic-addon1">@</span>',
    'after' => '<span class="input-group-addon" id="basic-addon2">@example.com</span>',
]
```

For Relationships:

```
    model: Full class as string
    label: visible name for the options
    value: value for the options
    method: custom method, defaults to `all()`
    params: parameters that are sent to the custom method
```

Example without User:
```php
@input_maker_create('roles', [
    'type' => 'relationship',
    'model' => 'App\Repositories\Role\Role',
    'label' => 'label',
    'value' => 'name'
])
```

Example with User:
```php
@input_maker_create('roles', [
    'type' => 'relationship',
    'model' => 'App\Repositories\Role\Role',
    'label' => 'label',
    'value' => 'name'
], $user)
```

Example with User and custom method for getting Roles: In this instance we call a custom method on the Role model, and we pass in the params.
```php
@input_maker_create('roles', [
    'type' => 'relationship',
    'model' => 'App\Repositories\Role\Role',
    'label' => 'label',
    'value' => 'name',
    'method' => 'getRolesThatAreCool',
    'params' => 'cool'
], $user)

public function getRolesThatAreCool($params)
{
    return $this->where('name', 'LIKE', "%$params%")->get();
}
```

Types supported in the Config:

* string
* text (converts to textarea)
* password
* checkbox
* checkbox-inline
* radio
* select
* hidden
* number
* float
* decimal
* relationship

!!! Warning
    If no type is set the InputMaker will default to a standard text input_

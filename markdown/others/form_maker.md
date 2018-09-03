# Form Maker

The form maker helps developers build entire forms with as little as one line of code. You can generate forms with things such as: an array, a table, or an object. The methods are highly customizable and allow you to control each of the components of your data, and write as little HTML as possible, or as much as you want.

If you wish to use an alternate connection to access a table and generate a form, you can use the `setConnection` method.

## Set Alternate Connections

```
FormMaker::setConnection('alternate')->fromTable('books');
```

## Blade Directives

```
@form_maker_table()
@form_maker_object()
@form_maker_array()
@form_maker_columns()
```

## Helpers

```
form_maker_table()
form_maker_object()
form_maker_array()
form_maker_columns()
```

## Facades

```
FormMaker::fromTable()
FormMaker::fromObject()
FormMaker::fromArray()
FormMaker::getTableColumns()
```

## Common Components

## Simple Fields

These components are the most simplistic:

```
class: 'a css class'
reformatted: true|false // Reformats the column name to remove underscores etc
populated: true|false // Fills in the form with values
idAndTimestamps: true|false // ignores the id and timestamp columns
```

### Columns

Columns is an array of the following nature which can be used in place of the columns component in any of the fromX methods:

```
[
    'id' => [
        'type' => 'hidden',
    ],
    'name' => [
        'type' => '', // defaults to standard text input
        'placeholder' => 'User Name Goes Here!',
        'alt_name' => 'User Name',
        'custom' => 'custom DOM attributes etc, can be array or string',
        'class' => 'css class names',
        'before' => '<span class="input-group-addon" id="basic-addon1">@</span>',
        'after' => '<span class="input-group-addon" id="basic-addon2">@example.com</span>',
    ],
    'job' => [
        'type' => 'select',
        'alt_name' => 'Your Job',
        'custom' => 'multiple', // custom attributes like multiple, disabled etc
        'options' => [
            'key 1' => 'value_1',
            'key 2' => 'value_2',
        ]
    ],
    'roles' => [
        'type' => 'relationship',
        'class' => 'App\Repositories\Roles\Roles',
        'label' => 'name' // the field for the label in the select input generated
    ]
]
```

Types supported in the Column Config:

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

_** If no type is set the FormMaker will default to a standard text input_

Columns with the following names will not be displayed by default: id, created_at, updated_at. You would need to override this setting in the creation of the form.

### View

You can create a custom view that the FormMaker will use: This is an example:

```
<div class="row">
    <div class="form-group {{ $errorHighlight }}">
        <label class="control-label" for="{!! $labelFor !!}">{!! $label !!}</label>
        <div class="row">
            {!! $input !!}
        </div>
    </div>
    {!! $errorMessage !!}
</div>
```

## Relationships

FormMaker can handle the following relationships: `hasOne`,`hasMany`,`belongsTo`,`belongsToMany`
You can set the config as such for something like 'Roles'. You will notice that the class is the actual object, we need this to be able
to pull in the existing relations.

```
'roles' => [
    'type' => 'relationship',
    'multiple' => true,
    'class' => auth()->user(),
    'method' => 'roles',
    'options' => app('App\Repositories\Roles\Roles')->all(),
    'label' => 'name',
    'value' => 'id',
]
```

## fromTable()

```
FormMaker::fromTable($table, $columns = null, $class = 'form-control', $view = null, $reformatted = true, $populated = false, $idAndTimestamps = false)
```

### Example:

```
FormMaker::fromTable('users')
```

The fromTable method will crawl the specified table and build the form out of the columns and types of coloumns. You can freely customize it (see below) the basic above example will result in:

```
<div class="form-group ">
    <label class="control-label" for="Name">Name</label>
    <input  id="Name" class="form-control" type="" name="name" placeholder="Name">
</div>
<div class="form-group ">
    <label class="control-label" for="Email">Email</label>
    <input  id="Email" class="form-control" type="" name="email" placeholder="Email">
</div>
<div class="form-group ">
    <label class="control-label" for="Password">Password</label>
    <input  id="Password" class="form-control" type="" name="password" placeholder="Password">
</div>
<div class="form-group ">
    <label class="control-label" for="Remember_token">Remember Token</label>
    <input  id="Remember_token" class="form-control" type="" name="remember_token" placeholder="Remember Token">
</div>
```


## fromObject()

Within the same rules as above we can rather than provide a table string we can insert an object such as `Auth::user()` or any single object retrieved from the database.

```
fromObject($object, $columns = null, $view = null, $class = 'form-control', $populated = true, $reformatted = false, $idAndTimestamps = false)
```

## fromArray()

From array works in the same context as fromTable, and fromObject, we're able to in this case provide a simple array list of properties. The key difference with fromArray is that we can provide these in one of two formats:

```
[ 'name', 'birthday' ]
```

OR

```
[ 'name' => 'string', 'birthday' => 'date' ]
```

The full list of field types compatible are:

* integer
* string
* datetime
* date
* float
* binary
* blob
* boolean
* datetimetz
* time
* array
* json_array
* object
* decimal
* bigint
* smallint

```
fromArray($array, $columns = null, $view = null, $class = 'form-control', $populated = true, $reformatted = false, $idAndTimestamps = false)
```

## getTableColumns()

The getTableColumns method utilizes Doctrines Dbal component to map your database table and provide the columns and types. This is perfect initial builds of an editor form off an object.

Example:

```
FormMaker::fromObject(Books::find(1), FormMaker::getTableColumns('books'))
```

This will build the form off the columns of the table. Though the fromObject will scan through the object, but providing the table columns as the columns input allows the inputs to be set to thier correct type.

## Design

Sometimes you need to customize how many columns are set for a specific group of inputs. You can easily control that with the FormMaker by using the following chained method:

```
FormMaker::setColumns(3)->fromObject(Books::find(1), FormMaker::getTableColumns('books'))
```

This will generate the form using a 3 column layout.


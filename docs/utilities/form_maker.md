# Form Maker

Our Form Maker package is designed to give developers to freedom to build forms via classes and set the fields in those classes comfortably. You can easily pass a form to a view and it will render the whole form, reducing your need to type out divs, labels, inputs and more.

[Source Code](https://github.com/grafiteinc/formMaker)

!!! warning "Form Maker by default is set to Bootstrap's classes, but you can change these in the config."

## Artisan Commands

Generate a form for a specific model using this make command. It will add the ModelForm to a Forms directory in the `app/Html/Forms` namespace.
```shell
artisan make:model-form {model}
```

Generate a generic form with a specific name using this command. It will add the BaseForm to a Forms directory in the `app/Html/Forms` namespace.
```shell
artisan make:base-form {name}
```

The following command will generate a feature test which hits the endpoints of the form.

```shell
artisan make:form-test {form}
```

If you made a ModelForm you likely want a factory for your model, since we have the fields within the ModelForm class we can generate basic factories.

```shell
artisan make:form-factory {form}
```

## Set Alternate Connections

```php-inline
app(UserForm::class)->setConnection('alternate');
```

Or in the `UserForm` itself:

```php-inline
$connection = 'alternate';
```

## Blade Directives

Form Maker only has one blade directive and that is for handling any assets you may add to a field. In the case of the `Quill` field we load the CDN assets (javascript and css), and we inject a snippet of JavaScript. These assets need to be rendered in your view. Ideally this is done close to the closing body tag of your main template.

```php-inline
@formMaker
```

Just place that below any of your JavaScript file references and you can easily load the forms field assets when the Form is being rendered on screen.

### Blade Components

If you like to keep your blade files looking a little nicer you can also use Blade Components. This lets you reduce the use of curly braces everywhere.

```html
<x-fm-action
    route="delete.user"
    method="delete"
></x-fm-action>
```

```html
<x-fm :content="$form"></x-fm>
```

```html
<x-fm-search
    route="search"
></x-fm-search>
```

## Helpers

```php-inline
form() // access the `Form` class
```

## Fields

Fields are PHP objects which define types, options, attributes etc for the Form Object. This lets you design forms and update them easily via a single object rather than having to update HTML files for multiple roles in your application.

```
FIELD_OPTIONS
type: A type string such as text or file
options: Options for <select> type
format: Format for DateTime objects
legend: A label for the legend of horizontal checkboxes
null_value: False by default, but allows empty values to be placed at the front of selects
null_label: Text which is placed as the first option in a select with a blank value
value: If you set the value of a field it will fill it in, or select it in the case of selects
label: A string or false (if you want the label blank - useful for `legend` uses)
model: Model class for the HasOne and HasMany Fields
model_options: Model options
    label: The label attribute on the model
    value: The value attribute on the model
    method: A custom method to run on the model
    params: Parameters to send to the custom method
before: Text or HTML you wish to put before an input
after: Text or HTML you wish to put after the input
view: A view path to a custom template
template: A custom template with some basic key value swaps
attributes: HTML attributes for your input field
factory: The type of input faker to use for test generating
assets: The js, styles, scripts, and stylesheets values
```

There are a large collection of Fields available out of the box and a `make:field {name}` command in case you need custom ones. Fields generate a config array which is then processed by the `FieldMaker` and `FieldBuilder` to create forms for easy use.

#### Available Fields

Any fields that include JS should have zero dependencies. Those that require jQuery obviously require jQuery.

```
Boostrap/Toggle (includes JS, requires jQuery)
Boostrap/Select (includes JS, requires jQuery)
Boostrap/HasOne (includes JS, requires jQuery)
Boostrap/HasMany (includes JS, requires jQuery)
Boostrap/Suggest (includes JS, requires jQuery)
Checkbox
Attachments (includes JS, requires jQuery)
CheckboxInline
Color
Code (includes JS)
CustomFile
Date
DatetimeLocal
Datepicker (includes JS)
Decimal
Dropzone (includes JS)
Email
File
FileWithPreview (includes JS)
HasMany
HasOne
Hidden
Image
Month
Number
Password
Quill (includes JS)
Radio
RadioInline
Range
Slug (includes JS)
Tags (includes JS)
Telephone
Text
TextArea
Time
Typeahead (includes JS, requires jQuery)
Url
Week
```

#### Field Assets

Out of the box Form Maker comes with 2 fields which contain field assets. You can add assets to any field, and the Form Maker will collect them and put links to them where you use the blade directive `formMaker`. We suggest below your `app.js` link on your master blade file.

In order to add assets to a field you need to use the following protected methods: `stylesheets`, `scripts`, `styles`, `js`. The following is an example from the `Quill` field.

```php-inline
protected static function stylesheets($options)
{
    return [
        "//cdn.quilljs.com/1.3.6/quill.bubble.css",
        "//cdn.quilljs.com/1.3.6/quill.snow.css",
    ];
}

protected static function scripts($options)
{
    return ['//cdn.quilljs.com/1.3.6/quill.js'];
}

// This was added for demo purposes
protected static function styles($id, $options)
{
    return <<<EOT
#$id {
    color: #FFF;
}
EOT;
}

protected static function js($id, $options)
{
    $theme = $options['theme'] ?? 'snow';
    $placeholder = $options['placeholder'] ?? '';

    return <<<EOT
    new Quill('#$id', {
        theme: '$theme',
        placeholder: '$placeholder'
    });
    EOT;
}
```

### Minification

By default for any form assets all JS and CSS is minified using [https://github.com/matthiasmullie/minify](https://github.com/matthiasmullie/minify)

### Default JS Validation

In general based on form submissions - we return a nice looking form with the error labels in place. The annoying part would be writing JavaScript validation per input to clean up the UI from the form return. If you set ```php-inline public $withJsValidation = true; ``` then you'll get a handy vanilla JS injection which removes the invalid classes on keyup or focus out.

## Sections

The various Form Objects allow you to set Sections. For example, you may have a `BlogForm` and you may want one row to have three columns while the next row has two, this can be achieved with the `setSections` method.

```php-inline
$columns = 1;

public function fields()
{
    return [
        Text::make('title', [
            'required' => true,
        ]),
        Text::make('url', [
            'required' => true
        ]),
        TextArea::make('entry', []),
        Date::make('published_at', []),
    ];
}
```

By default this will build a form with single column content. If you wish to set these fields to specific layouts you need to set the `columns` to `sections`

```php-inline
$columns = 'sections';

public function fields()
{
    return [
        Text::make('title', [
            'required' => true,
        ]),
        Text::make('url', [
            'required' => true
        ]),
        TextArea::make('entry', []),
        Date::make('published_at', []),
    ];
}

public function setSections()
{
    return [
        [
            'title',
            'url',
        ],
        [
            'entry'
        ],
        [
            'published_at'
        ]
    ];
}
```

The above will produce a form that is two columns, one, and one. You can add a `key` to the sections to add a horizontal line and a header.

```php-inline
public function setSections()
{
    return [
        [
            'title',
            'url',
        ],
        'Content' => [
            'entry'
        ],
        [
            'published_at'
        ]
    ];
}
```

## HTML Snippets

Within the fields section of the Form Object you can also place some HTML snippets. These can help with spacing and UI layouts. The Snippets included in FormMaker are:

```
OpenDiv
CloseDiv
Div
Heading
Hr
```

All of these snippets can have the `class` attribute set. `Div` and `Heading` can also have `content` set in them.

## Form

The Form class lets us generate simple forms with minimal code.

```php-inline
form()->action('method', 'route', 'button text', $html_attributes);
```

Generates a form using the method and route with a button, for easier addition of delete buttons and more,

```php-inline
form()->confirm('Are you sure?')->action(...);
```
Adds a confirmation popup to the button.

!!! warning "confirm is only supported in the delete method of ModelForms."

If you wish to handle the confirm using a modal or other JS integration you can pass a `method` name into the confirm method which will trigger that JS method:

```php-inline
form()->confirm('Are you sure you want to delete this?', 'confirmation')->action(...);
```

This will add the following to the submit button in the form:

```js
onclick="confirmation(event, 'Are you sure you want to delete this?')"
```

You will require a `confirmation` method to be defined somewhere such as in `app.js`. Your confirmation method could look something like this, which uses Bootstrap's modal:

```js
window.confirmation = (_event, _message) => {
    _event.preventDefault();

    $('#appModalMessage').html(_message);

    $('#appModal').modal({
        show: true
    });

    $('#appModalConfirmBtn').click(() => {
        $(_event.target.parentNode)[0].submit();
    });

    return false;
}
```

There is also support for an `submitMethod` attribute on all forms. This custom submit allows you to override the standard submit and perform actions against your form. This can be useful if you want to do an ajax submission of your form.

```php
<?php

namespace App\Http\Forms;

use Grafite\FormMaker\Fields\Password;
use Grafite\FormMaker\Forms\BaseForm;

class UserSecurityForm extends BaseForm
{
    public $route = 'user.security';

    public $method = 'put';

    public $orientation = 'horizontal';

    public $submitMethod = 'ajax';

    public $buttons = [
        'submit' => 'Save',
    ];

    public $columns = 1;

    public function fields()
    {
        return [
            Password::make('old_password', [
                'required' => true,
                'label' => 'Old Password'
            ]),
            Password::make('new_password', [
                'required' => true,
                'label' => 'New Password'
            ]),
            Password::make('new_password_confirmation', [
                'required' => true,
                'label' => 'Confirm New Password'
            ]),
        ];
    }
}
```

```js
window.ajax = (_event) => {
    _event.preventDefault();

    let _form = _event.target.parentNode.parentNode.parentNode;
    let _method = _form.method.toLowerCase();
    let _payloadArray = $(_form).serializeArray();
    let _payload = {};

    $.map(_payloadArray, function(n, i){
        _payload[n['name']] = n['value'];
    });

    window.axios[_method](_form.action, _payload)
        .then((response) => {
            window.Snotify.success(response.data.message);
        })
        .catch((error) => {
            window.Snotify.warning(error.response.data.message);

            for (var key in error.response.data.errors) {
                $('input[name="'+key+'"]').addClass('border-danger');
                window.Snotify.error(error.response.data.errors[key][0]);
            }
        });
}
```

```php-inline
form()->open($options);
```

Opens a form allowing you to specify options: action, method, attributes etc.

```php-inline
form()->model($model, $options);
```

Open a form based on a model

## Config

In general all classes are defined in the config, which means you can avoid Boostrap if you want to. You can also set the form class directly on the form itself.

```php-inline
public $formClass = 'form';
```

Any classes set on the form, or field itself will override the default configs. The following are default configs:

```php-inline
'buttons' => [
    'submit' => 'btn btn-primary',
    'delete' => 'btn btn-danger',
    'cancel' => 'btn btn-secondary',
],

'form' => [
    'class' => 'form',
    'delete-class' => 'form-inline',
    'inline-class' => 'form d-inline',

    'group-class' => 'form-group',
    'input-class' => 'form-control',
    'label-class' => 'control-label',
    'label-check-class' => 'form-check-label',
    'before_after_input_wrapper' => 'input-group',
    'text-error' => 'text-danger',
    'error-class' => 'has-error',
    'check-class' => 'form-check',

    'check-input-class' => 'form-check-input',
    'check-inline-class' => 'form-check form-check-inline',
    'custom-file-label' => 'custom-file-label',
    'custom-file-input-class' => 'custom-file-input',
    'custom-file-wrapper-class' => 'custom-file',

    'sections' => [
        'column-base' => 'col-md-',
        'row-class' => 'row',
        'full-size-column' => 'col-md-12',
        'header-spacing' => 'mt-2 mb-2',
        'row-alignment-between' => 'd-flex justify-content-between',
        'row-alignment-end' => 'd-flex justify-content-end',
    ],

    'orientation' => 'vertical',
    'horizontal-class' => 'form-horizontal',
    'label-column' => 'col-md-2 col-form-label pt-0',
    'input-column' => 'col-md-10',
]
```

---

## ModelForm

Using the `make:model-form {model}` command you can quickly generate forms for Models. This will let you generate forms based on the model.

```php-inline
app(UserForm::class)->create();
app(UserForm::class)->edit($user);
app(UserForm::class)->delete($user);
```

### Example

```php
<?php

namespace App\Http\Forms;

use App\Models\Role;
use App\Models\User;
use Grafite\FormMaker\Fields\File;
use Grafite\FormMaker\Fields\Text;
use Grafite\FormMaker\Fields\Email;
use Grafite\FormMaker\Fields\Checkbox;
use Grafite\FormMaker\Forms\ModelForm;

class UserForm extends ModelForm
{
    public $model = User::class;

    public $routePrefix = 'user';

    public $buttons = [
        'submit' => 'Save',
    ];

    public $columns = 1;

    public $orientation = 'horizontal';

    public $hasFiles = true;

    public function fields()
    {
        return [
            Text::make('name', [
                'required' => true,
            ]),
            Email::make('email', [
                'required' => true
            ]),
            Checkbox::make('dark_mode', [
                'legend' => 'Dark Mode'
            ]),
            File::make('avatar', []),
        ];
    }
}
```

Within this `UserForm` class you can set the fields in in the `fields` method:

```php-inline
public function fields()
{
    return [
        Text::make('name', [
            'required' => true,
        ]),
        Email::make('email', [
            'required' => true
        ]),
    ];
}
```

This will generate a form with these fields only. You can also set the `orientation` if you wish to use labels on the left side instead of above and `columns` if you wish to generate a form in which the fields are split into more columns.

`$routePrefix` is required as it defines the routes using route names which match the standards route naming of Laravel.

`$hasFiles` enables the file submission of the form.

`$buttons` enables you to customize the button values for submit and cancel

`$buttonClasses` enables you to customize the button class values for submit, cancel, and delete which can also be done in the config as form default

`$confirmMessage` lets you set the confirm message for the delete button

`$confirmMethod` sets the method name for the `onclick` event when clicking on the delete button

With the `delete()` form you can also add the confirmation method like so:

```php-inline
{!! form()
    ->confirm('Are you sure you want to delete your avatar?', 'confirmation')
    ->action('delete', 'user.destroy.avatar', 'delete', ['class' => 'btn btn-sm btn-outline-secondary'])
!!}
```

### Accessing the Model Instance

You can access the model instance in a model form, it can give you the freedom to collect data from the model in the case of setting select values etc.

```php-inline
public $instance;
```

You can also check if its been set by running:

```php-inline
$this->hasInstance()
```

### Custom View for Fields

You can create a custom view that the FormMaker will use for your fields. Just have you view file follow this pattern:

```php-inline
<div class="row">
    <div class="form-group">
        {!! $label !!}
        <div class="row">
            {!! $field !!}
        </div>
    </div>
    {!! $errors !!}

    {!! $options !!}
</div>
```

The values passed to the view are `label`, `field`, `errors`, `options`. The options is how you can pass more abstract configs through, but in all honesty we dont really see many use cases of that.

### Custom Template for Fields

You can create a custom template that the FormMaker will use for your fields. This way you do not need a view file. Templates have a VERY basic templating component to them.

You have the following variables that are passed to the template:

| Template Key | What it Is |
---|---
\{id\} | The field ID
\{name\} | The label
\{errors\} | An HTML string of error messages
\{label\} | The label as an HTML string
\{field\} | The field as an HTML string
\{rowClass\} | The CSS class which wraps around the label and field
\{labelClass\} | The CSS class which is on the label
\{fieldClass\} | The CSS class which is on the field

This has value when using `horizontal` forms vs `vertical` forms.

```php-inline
<div class="{rowClass}">
    <label for="{id}" class="{labelClass}">{name}</label>
    <div class="{fieldClass}">
        {field}
    </div>
    {errors}
</div>
```

### Relationships

FormMaker also has Fields called `HasOne` and `HasMany`. These enable you to select one or multiple from a relationship binding in your model.

For example you may have a `User` who has a `role` which is `belongsToMany`.

You can set the Field for something like 'Roles' as below.

```php-inline
HasOne::make('role', [
    'model' => Role::class,
    'model_options' => [
        'label' => 'name',
        'value' => 'id',
        'method' => 'all',
        'params' => null,
    ]
]);
```

You can also customize how you access the method and with what parameters. By default it will try to collect the options by getting `Role::all()` however you can limit that with something like `Role::custom(params)`.

### Index and Search

With a `ModelForm` you can also utilize the index and search methods. This lets you render full index tables of models and search forms for those models.
There are also some special properties you can set on your `ModelForm` in order to optimize the index and allow some fields to be sortable.

`$with` is used as a portion of the model query run in index listing.

`$paginate` allows you to set a number for how many items to load in the index.

```php-inline
app(UserForm::class)->index()
```

In case you wish to output the index to an API call.

```php-inline
app(UserForm::class)->index()->toJson()
```

Renders a search form for the model index

```php-inline
app(UserForm::class)->index()->search('route.for.search', 'placeholder', 'Button Text', 'form-method');
```

### Field Options

Each field by default will be listed in the index. You can customize this by setting the `visible` option to `false`.

If you wish to make a field sortable (asc|desc) then you can set the `sortable` option to `true`.

#### Custom Query on Index

You can also pass a query into the `index` method if you wish to set some details.

```php-inline
$query = User::whereIn('in', [1, 2, 3]);

app(UserForm::class)->index($query);
```

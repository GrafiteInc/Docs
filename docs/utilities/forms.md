# Forms

Our Forms package is designed to give developers to freedom to build forms via classes and set the fields in those classes comfortably. You can easily pass a form to a view and it will render the whole form, reducing your need to type out divs, labels, inputs and more.

[Source Code](https://github.com/grafiteinc/forms)

!!! warning "Forms by default is set to Bootstrap's classes, but you can change these in the config."

## Artisan Commands

Generate a form for a specific model using this make command. It will add the ModelForm to a Forms directory in the `app/Html/Forms` namespace.
```shell
artisan make:model-form {model}
```

Generate a generic form with a specific name using this command. It will add the BaseForm to a Forms directory in the `app/Html/Forms` namespace.
```shell
artisan make:base-form {name}
```

Other form make commands are as follows:
```shell
artisan make:modal-form {name}
artisan make:livewire-form {name}
artisan make:wizard-form {name}
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
@forms
```

Or, if you wish to split the resources (which is smarter) you can use:

```php-inline
@formStyles
@formScripts
```

Just place that below any of your JavaScript file references and you can easily load the forms field assets when the Form is being rendered on screen.

### Blade Components

If you like to keep your blade files looking a little nicer you can also use Blade Components. This lets you reduce the use of curly braces everywhere.

```html
<x-f-action
    route="delete.user"
    method="delete"
    payload="$user"
></x-f-action>
```

```html
<x-f :content="$form"></x-f>
```

```html
<x-f-search
    route="search"
></x-f-search>
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

There are a large collection of Fields available out of the box and a `make:field {name}` command in case you need custom ones. Fields generate a config array which is then processed by the `FieldMaker`, `AttributeBuilder` and `FieldBuilder` to create forms for easy use.

#### Available Fields

Any fields that include JS should have zero dependencies. Those that require jQuery obviously require jQuery.

```
Boostrap/Country (includes JS, requires jQuery)
Boostrap/HasMany (includes JS, requires jQuery)
Boostrap/HasOne (includes JS, requires jQuery)
Boostrap/Select (includes JS, requires jQuery)
Boostrap/Suggest (includes JS, requires jQuery)
Boostrap/Timezone (includes JS, requires jQuery)
Boostrap/Toggle (includes JS, requires jQuery)
Attributes
Checkbox
CheckboxInline
Code (includes JS)
Color
CustomFile
Datalist
Date
Datepicker
DatetimeLocal
Decimal
Dropzone (includes JS)
Editor (includes JS)
Email
File
FilePond (includes JS)
FileWithPreview
HasMany
HasOne
Hidden
Image
Month
Name
Number
Password
PasswordWithReveal
Quill (includes JS)
Radio
RadioInline
Range
Search
Select
Slug (includes JS)
Tags (includes JS)
Telephone
Text
TextArea
Time
Toggled (includes JS)
Typeahead (includes JS, requires jQuery)
Url
Week
```

### Field Methods Available

In general you can set field options as an array in the `make` method of the Field.

```inline-php
Text::make('name', [
    'required' => true,
]);
```

However you can also use any of the methods below as in a chaining style in order to set the Field configuration details.

##### Example:

```inline-php
Text::make('name')->required();
```

##### Methods

```inline-php
required()
placeholder('foo')
attribute('foo', 'bar')
attributes(['foo' => 'bar'])
value('foo')
label('foo')
cssClass('foo') // set the class of the input field
labelClass('foo') // set the field's label's class
name('foo')
accept(['json/application'])
before('foo') // places Foo before the input field
after('foo') // places Foo after the input field
legend('foo')
readonly() // set a field as readonly
disabled() // set a field as disabled
maxlength(5) // set a field's maxlength attribute
size(5) // set a field's size attribute
min(5) // set a field's min attribute
max(5) // set a field's max attribute
step(.5) // set a field's step attribute
pattern('foo') // set the pattern attribute of the field
multiple() // set if the field can accept multiple values
autofocus() // set a field's autofocus attribute
autocomplete() // set a field's autofocus attribute
id('foo') // set a field's id attribute
style('foo') // set a field's style attribute
title('foo') // set a field's title attribute
data('foo', 'bar') // add a data attribute to a field
selectOptions(['foo' => 'bar']) // set the options for a select type field
instance($foo) // set a Field's instance
option('key', 'value') // set a field's option
options(['key' => 'value']) // set a field's option as an array
view('foo.bar') // set the view for the field template
template('{field}') // set the template for the field
model(User::class)
modelMethod('getUsersWithRole') // default: all()
modelParams(['role' => 'member']) // default: null
modelValue('id') // default: id
modelLabel('name') // default: name
groupClass('foo') // class in a div around the label and input
ungrouped() // remove the div around the label and input
withoutLabel() // remove the label from the Field
onlyField() // remove the label and wrapping div from the Field
sortable() // make a field sortable in the rendered index
canSelectNone() // give a select Field a null option
noneLabel('foo') // set the label for the null option in a select Field
visible() // make a Field visible in the rendered index
hidden() // make a field hidden in the rendered index
tableClass('foo') // set the class for the rendered index column
```

#### Special Field Options

Some fields have extra options which pertain to their configs. Below are more custom options you can apply to these special fields.

##### Boostrap/Toggle

See [https://gitbrent.github.io/bootstrap4-toggle/](https://gitbrent.github.io/bootstrap4-toggle/) for more options.

```json
theme: "light|dark"
on: "On"
off: "Off"
size: "sm"
```

##### Boostrap/Suggest
```json
btn: "btn-primary"
```

##### Boostrap/Select
```json
btn: "btn-primary"
```

##### Boostrap/HasOne
```json
btn: "btn-primary"
```

##### Boostrap/HasMany
```json
btn: "btn-primary"
```

##### Code

See [https://codemirror.net/](https://codemirror.net/) for more options.

```json
mode: "htmlmixed"
theme: "default"
```

##### Datepicker

See [https://github.com/qodesmith/datepicker#readme](https://github.com/qodesmith/datepicker#readme) for more options

```json
theme: "light"
background-color: "#FFF"
color: "#FFF"
number-color: "#111"
highlight: "var(--primary, "#EEE")"
header: "var(--primary, "#EEE")"
start-day: 1
format: "YYYY-MM-DD"
```

##### Dropzone

```json
theme: "light"
queue-complete: "function () { window.location.reload() }"
upload-multiple: true
route: ""
```

##### Filepond

```json
file_size: "25MB"
process_url: null
submit_button: "button[type="submit"]"
upload_result_field: null
```

##### FileWithPreview

```json
preview_identifier: "" // class or ID for an avatar img or div
preview_as_background_image: false
```

##### Editor

Uses the latest EditorJS as its WYSIWYG editor.

```json
theme: "light"
upload_route: "upload.image"
```

*Special*

If you want to use images inside EditorJS you need to have a route which can handle the image uploads. The following is an example of a Controller invoke method which can handle the file uploads, below is the Quill example using the same controller.

```inline-php
$validatedData = $request->validate([
    'file' => 'image|required',
]);

$path = collect($validatedData)->first()->store('public/uploads');

return response()->json([
    'success' => true,
    'file' => [
        'url' => Storage::url($path),
    ],
]);
```

Editor also has a parser available at `Grafite\Forms\Parsers\Editor`. This lets you do something like:

```inline-php
app(Grafite\Forms\Parsers\Editor::class)->parse(auth()->user()->bio)->render();
```

##### Quill

```json
theme: "light"
quill_theme: "snow"
toolbars: [
    "basic",
    "extra",
    "lists",
    "super_sub",
    "indents",
    "headers",
    "colors",
    "image",
    "video"
]
upload_route: "upload.image"
```

*Special*

If you want to use images inside Quill you need to have a route which can handle the image uploads. Storing images as data-urls (Quill default method) is never wise. The following is an example of a Controller invoke method which can handle the file uploads.

```inline-php
$validatedData = $request->validate([
    'file' => 'image|required',
]);

$path = collect($validatedData)->first()->store('public/uploads');

return response()->json([
    'success' => true,
    'file' => [
        'url' => Storage::url($path),
    ],
]);
```

##### Tags

```json
default-border: "#EEE"
focus-border: "#EEE"
```

##### Toggled

```json
color: "blue"
```

##### Typeahead

```json
matches: []
```

#### Field Assets

Out of the box Forms comes with a few fields which contain field assets. You can add assets to any field, and the Forms package will collect them and put links to them where you use the blade directive `forms`. We suggest below your `app.js` link on your master blade file.

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

By default for any form assets all JS and CSS is minified in production using [https://github.com/matthiasmullie/minify](https://github.com/matthiasmullie/minify). We keep in unminified locally and in testing for easier debugging.

### Default JS Validation

In general based on form submissions - we return a nice looking form with the error labels in place. The annoying part would be writing JavaScript validation per input to clean up the UI from the form return. If you set ```php-inline public $withJsValidation = true; ``` then you'll get a handy vanilla JS injection which removes the invalid classes on keyup or focus out.

## Single Field Usage

If you want to use the Forms builder for just a single field you can quite easily with the `form` helper.

```php-inline
{!! form()->makeField(\Grafite\Forms\Fields\Text::class, 'name', [
    'id' => 'nameField',
]) !!}
```

!!! info "This is extra useful when you want to use JS driven form fields"

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
        TextArea::make('entry')->required(),
        Date::make('published_at'),
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

Within the fields section of the Form Object you can also place some HTML snippets. These can help with spacing and UI layouts. The Snippets included in Forms are:

```
OpenDiv
CloseDiv
Div
Heading
HrTag
Span
Button
```

All of these snippets can have nearly any attribute set. `Div`, `Heading`, `Button`, `Span` can also have `content` set in them, and in most cases require the `content` value be set for them.

## Card Wrapper

Sometimes you may want to place a form inside a card component. If you do, you may wish to set the `$isCardForm` property to true. This will place the form buttons in the card footer and wrap the form in a `.card-body` div. This lets you control the use of card-headers while giving your form a clean component like style.

## Disable on Submit

When a user clicks the submit button we can end up having issues if they click it repeatedly. Setting the `$disableOnSubmit` property to true, will in turn disable the submit button onclick and insert a fontawesome spinning icon.

## Form

The Form class lets us generate simple forms with minimal code.

```php-inline
form()->action('method', 'route', 'button text', $html_attributes);
```

Generates a form using the method and route with a button, for easier addition of delete buttons and more.

You can also provide a payload to action forms similar to the confirm below:

```php-inline
form()->payload(['user' => $user->id])->action(...);
```

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
        _event.target.form.submit();
    });

    return false;
}
```

There is also support for a `submitMethod` attribute on all forms. This custom submit allows you to override the standard submit and perform actions against your form. This can be useful if you want to do an ajax submission of your form.

!!! info Special Case
    There is a also a `submitMethods` array property that can be set for any `ModelForm`. This lets you set update or create forms to ajax driven or standard.

```php
<?php

namespace App\Http\Forms;

use Grafite\Forms\Fields\Password;
use Grafite\Forms\Forms\BaseForm;

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

    let _form = _event.target.form;
    let _method = _form.method.toLowerCase();
    let _data = new FormData(_form);

    window.axios[_method](_form.action, _data, {
        headers: {
          'Content-Type': 'multipart/form-data'
        }
    })
    .then((response) => {
        window.notify.success(response.data.message);
    })
    .catch((error) => {
        window.notify.warning(error.response.data.message);

        for (var key in error.response.data.errors) {
            document.querySelector('input[name="'+key+'"]').classList.add('border-danger');
            window.notify.error(error.response.data.errors[key][0]);
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

### Form Types

There are 3 main form types available:

##### Form
This is mostly used as the action form for simple action based forms with single buttons etc.

##### BaseForm
The full control of rendered fields with script injection options. Generall not bound to a model.

##### ModelForm
The full rendered fields and script injections with a model bound to the actions.

##### ModalForm
The full rendered fields and script injections inside a Bootstrap based modal. They do require a `triggerContent` and `triggerClass` which are the content and class for the button which opens the Modal.

##### LivewireForm
Livewire form is more basic, ignoring buttons, allowing for `public $onKeydown` as well as the ease of passing the Component `$data` into the form via the `make()` method. If you wish to use the whole form notion, then set the `$method` to whatever you wish, by default its set to *submit*.

##### WizardForm
The wizard form enables developers to create a wizard like experience for their form submission. It collects the specified fields and sets them into steps defined in the Form class. The Next and Previous buttons can be modified and they run basic HTML5 validation as you move forward.

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

### Form Scripts

Similar to Field assets you can inject JavaScript directly into your form to contain logic etc. This lets you avoid having random scripts in your JavaScript assets for random parts of your application. To add scripts simply create a scripts method and return what you wish.

```php-inline
public function scripts()
{
    return <<<EOT
        console.log("hello world");
    EOT;
}
```

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
use Grafite\Forms\Fields\File;
use Grafite\Forms\Fields\Text;
use Grafite\Forms\Fields\Email;
use Grafite\Forms\Fields\Checkbox;
use Grafite\Forms\Forms\ModelForm;

class UserForm extends ModelForm
{
    public $model = User::class;

    public $routePrefix = 'user';

    public $routeParameters = ['id'];

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

`$routeParameters` is required if you need to pass more than the default ID parameter for a form. This is also needed if you wish to use `uuid` etc.

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

You can create a custom view that the Forms will use for your fields. Just have you view file follow this pattern:

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

You can create a custom template that the Forms will use for your fields. This way you do not need a view file. Templates have a VERY basic templating component to them.

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

Forms also has Fields called `HasOne` and `HasMany`. These enable you to select one or multiple from a relationship binding in your model.

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

If you wish to adjust the table class for the index list you can set the `table_class` option.

#### Custom Query on Index

You can also pass a query into the `index` method if you wish to set some details.

```php-inline
$query = User::whereIn('in', [1, 2, 3]);

app(UserForm::class)->index($query);
```

## ModalForm

A modal form is a form inside a modal popup. It requires a Trigger (a button) to have the modal popped open.

This form type contains all the similar attributes to `BaseForm` and `ModelForm`. It also has the trigger attributes.

`$triggerContent` Content that goes inside the modal trigger button.

`$triggerClass` The classes for the button that triggers the modal.

These attributes can also be set for `ModelForm` classes and utilize the `asModal()` method. This can also be used for delete buttons with `ModelForm` objects. With the asModal you're able to allow a simple Confirmation modal.

## WizardForm

A form as a set of steps with next and previous buttons.

The standard properties such as `route`, `method` are to be set as well as the optional `progressBarColor`. The fields are defined in the standard way and similar to the `setSections` method you can need to set the steps in the `steps` method.

## Livewire

!!! info "You have to first ensure your app is set up with all basic Livewire setup."

You can use the forms inside Livewire and handle a varity of options. First you need to set that the Form is using Livewire using the following property.

```php-inline
public $withLivewire = true;
```

If you wish to have the changes propogate on keydown you can enable that as well with the following:

```php-inline
public $livewireOnKeydown = true;
```

Within your Livewire component you have a handful of options. You need to set the `$data` property in the `mount` method, then create a `submit` method and lastly set the form within the render method. Below is an example:

```php-inline
<?php

namespace App\Http\Livewire;

use Livewire\Component;
use App\Http\Forms\UserForm;

class UserSettings extends Component
{
    public $data;

    public function mount()
    {
        $user = request()->user();

        $this->data['name'] = $user->name;
        $this->data['email'] = $user->email;
        $this->data['billing_email'] = $user->billing_email;
        $this->data['state'] = $user->state;
        $this->data['country'] = $user->country;
    }

    protected $rules = [
        'data.billing_email' => 'required|email',
    ];

    public function submit()
    {
        $this->validate();

        request()->user()->update($this->data);
    }

    public function render()
    {
        $user = request()->user();

        $form = app(UserForm::class)
            ->setErrorBag($this->getErrorBag())
            ->edit($user);

        return view('livewire.user-settings')->withForm($form);
    }
}
```

#### Render Method

Within the render method you will need to place your Form code. Specifically you will need to pass in the `ErrorBag` from the component in order to correctly render field errors. This lets you handle real time error reporting on form entry quite easily.

```php-inline
app(UserForm::class)->setErrorBag($this->getErrorBag())->create()
```

The above example is in our Scaffold project. It provides an example of using the Forms inside a Livewire component.

##### LivewireForm Example

```php-inline
<?php

namespace App\Http\Forms;

use Grafite\Forms\Html\Button;
use Grafite\Forms\Fields\Number;
use Grafite\Forms\Forms\LivewireForm;

class CartForm extends LivewireForm
{
    public $columns = 2;

    /**
     * Set the desired fields for the form.
     *
     * @return array
     */
    public function fields()
    {
        return [
            Number::make('count', [
                'required' => true,
                'label' => false,
                'wrapper' => false
            ]),
            Button::make([
                'wire:click.prevent' => 'setNumber',
                'content' => '<span class="fas fa-fw fa-plus pr-4"></span> Add to Cart'
            ], 'add')
        ];
    }
}
```

##### LivewireForm Component Example

```php-inline
<?php

namespace App\Http\Livewire;

use Livewire\Component;
use App\Http\Forms\CartForm;

class Cart extends Component
{
    public $data;

    protected $rules = [
        'data.count' => 'lte:1000',
    ];

    public function mount()
    {
        $this->data['count'] = 88;
    }

    public function setNumber()
    {
        $this->validate();

        $this->data['count'] = 987987897;
    }

    public function render()
    {
        $form = app(CartForm::class)
            ->setErrorBag($this->getErrorBag())
            ->make($this->data);

        return view('livewire.cart')->withForm($form);
    }
}
```

##### Livewire and Forms JavaScript

If you have some fields which have JavaScript running elements of them you can easily solve some problems by adding `wire:ignore` to the div surrounding your `$form` in the component view.

```php-inline
<div wire:ignore>
    {!! $form !!}
</div>
```

This tells Livewire to NOT perform the DOM diff on the child Form elements. This means your form doesn't try to reload its components etc.

## Parsers

Some fields require parsers since the data they store isn't very consumable by default. `Editor` is a great example of this. The `Editor` field stores a JSON object of blocks of HTML. This can then use the `Editor` Parser to convert it to proper HTML for end user consumption.

Parsers should have `parse`, `handler`, and `render` methods generally to handle accepting data, processing it and rendering it as HTML or other content.


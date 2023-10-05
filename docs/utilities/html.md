# Html

In general we'd prefer to keep most of our HTML where in belongs in blade components and files. However, there are various cases where it can be nice to autogenerate the HTML needed for a component automagically. This package is intended for those exact circumstances.

[Source Code](https://github.com/grafiteinc/html)

!!! warning "Html by default is set to Bootstrap's classes, except for the Feed component. We're exploring ways of making it changeable in the future."

## Blade Directives

Similar to our Forms package we have some simple directives for loading any needed assets for the components.

```php-inline
@htmlStyles
@htmlScripts
@htmlAssets
@fathom
```

Just place that below any of your JavaScript file references and you can easily load the html assets when the content is being rendered on screen.

As for the `Fathom` directive, ensure that you have the following added to your services:

```
'fathom' => [
    'key' => env('FATHOM_KEY'),
],
```

## Components

```
Accordion
Annoucement
Alert
Avatar
Breadcrumbs
Card
Carousel
Calendar
DropdownButton
DropdownButtonAction
DropdownDivider
DropdownItem
DropdownItemButton
Feed
FeedItem
ListGroup
Image
Map
Modal
Nav
NavBar
NavButton
NavDropdown
NavLink
OffCanvas
Progress
SortTextWithIcon
Spinner
Rating
Table
Tilt
Animations/Hamster
Animations/Pulse
Animations/Spaceman
Animations/Typewriter
```

## The HtmlTagComponent

This class base lets us create our own global components for any needs in our application. These are single-file components, containing JavaScript, Styles and HTML content.

Below is an example of an overlay component.

```
<?php

namespace App\View\Components\Global;

use Grafite\Html\Tags\HtmlTagComponent;

class PendingOverlay extends HtmlTagComponent
{
    public static function template()
    {
        return <<<HTML
            <div id="_componentPendingOverlay" class="overlay d-none bg-dark">
                <div class="spinner-container">
                    <div class="spinner-border" role="status">
                        <span class="visually-hidden">Loading...</span>
                    </div>
                </div>
            </div>
        HTML;
    }

    public static function styles()
    {
        return <<<CSS
            .overlay {
                width: 100%;
                height: 100vh;
                position: absolute;
                top: 0;
                left: 0;
                z-index: 30000;
                opacity: .8;
            }
            .spinner-container {
                position: absolute;
                top: 49%;
                left: calc(50% - 15px);
            }
        CSS;
    }

    public static function js()
    {
        return <<<JS
            window.pending = (button) => {
                if (button && button.form.checkValidity()) {
                    button.form.submit();
                    button.disabled = true;
                    document.getElementById('_componentPendingOverlay').classList.remove('d-none');
                }

                if (! button) {
                    document.getElementById('_componentPendingOverlay').classList.remove('d-none');
                }

                return false;
            };

            window.pendingHide = () => {
                document.getElementById('_componentPendingOverlay').classList.add('d-none');
            };
        JS;
    }
}
```

## Component Methods Available

To generate any component you must first call the make method statically. `Alert::make()`. Subsequently, you need to end all component chains with the `render()` method or the `renderWhen` method.

Between those methods you can call any of these global methods to set various values in the component.

```inline-php
id() => $id
css() => $css
url() => $url
text() => $text
onClick() => $onClick
items() => $items
attributes() => $attributes
```

Some components have unique properties which have unique methods to call each of them to set their values. Below is a list of any unique properties that can be set for the components.

#### Accordion
- show() // start by showing all open

#### Alert
- background(string $value) // color
- heading(string $value) // text
- dismiss() // enables dismiss button

#### Avatar
- image($string) // a url path to an image

#### Card
- body(string $value) // text
- title(string $value) // text
- header(string $value) // text
- footer(string $value) // text
- image(string $value, string $alt) // source and alt
- shadow(string $value) // shadow css class

#### Feed
- borderColor(string $value) // color for item borders

#### FeedItem
- content(string $value) // body content for the item
- date(string $value) // date for the item
- icon(string $value, string $value) // an icon (fontawesome?) and a css color

#### Map
- marker($x, $y, $tooltip = null, $click = null) // a marker for the map
- center($x, $y) // the center of the map
- skin($url) // a URL for a LeafletJS map tiles string
- bubbles(array [
    [
        'x' => null,
        'y' => null,
        'color' => null,
        'fill' => null,
        'opacity' => null,
        'radius' => null,
        'tooltip' => null,
        'click' => null,
    ]
])
- zoom(int $int) // starting zoom level
- maxZoom(int $int) // max level of the zoom

#### Modal
- content(string $value) // content for the modal body
- isStatic() // is the modal static
- dismiss() // can the modal be dismissed
- footer(string $value) // modal footer content
- title(string $value) // modal title

#### Nav
- pills() // set the nav to pills
- tabs() // set the nav to tabs

#### NavBar
- brand(string $value) // content for the navbar brand link

#### Progress
- now(string $value) // what is the progress now
- max(string $value) // min value of the progress
- min(string $value) // max value of the progress

#### SortTextWithIcon
- field(string $value) // the field value for sorting

#### Table
- collection(collection $value) // the collection for the table
- keys(collection $value) // keys which can be used for headers
- headers(collection $value) // headers which can override the keys
- sortable(string $value) // a JS method which is triggered onclick

## As Blade Tags

The following HTML components have blade tags which can also be used.

```
Accordion
ActionDropdown
Alert
Breadcrumbs
Card
Carousel
DropdownDivider
DropdownItem
DropdownItemButton
Feed
FeedItem
ListGroup
ListGroupItem
Modal
Nav
NavButton
NavLink
Offcanvas
Progress
Spinner
Table
Tag
```

## Core Methods

There are a few core component methods which we should discuss.

##### render
Renders the component as a string

##### renderWhen(callback)
Renders the component as a string when the callback is true.

##### make
Intiates the build of the html component.

##### styles
Sends some CSS to the HtmlAssets which are minified and pushed to the blade directive styles.

##### scripts
Sends an array of CDN links to the HtmlAssets which are pushed to the blade directive scripts.

##### js
Sends JavaScript code to the HtmlAssets which are pushed to the blade directive scripts.


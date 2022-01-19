# Html

In general we'd prefer to keep most of our HTML where in belongs in blade components and files. However, there are various cases where it can be nice to autogenerate the HTML needed for a component automagically. This package is intended for those exact circumstances.

[Source Code](https://github.com/grafiteinc/html)

!!! warning "Html by default is set to Bootstrap's classes, except for the Feed component. We're exploring ways of making it changeable in the future."

## Blade Directives

Similar to our Forms package we have some simple directives for loading any needed assets for the components.

```php-inline
@htmlStyles
@htmlScripts
```

Just place that below any of your JavaScript file references and you can easily load the html assets when the content is being rendered on screen.

## Components

```
Alert
Breadcrumbs
Card
Carousel
DropdownButton
DropdownButtonAction
DropdownDivider
DropdownItem
DropdownItemButton
Feed
FeedItem
ListGroup
Modal
Nav
NavBar
NavButton
NavDropdown
NavLink
Progress
SortTextWithIcon
Spinner
Table
```

### Component Methods Available

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

#### Alert
- background(string $value) // color
- heading(string $value) // text
- dismiss() // enables dismiss button

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


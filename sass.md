# General rules

All files that are pulled in via `@import` or `= require` have a leading underscore in their filename (best practice).

All files included directly via `stylesheet_link_tag` live in the root `stylesheets` folder.

Modules are "business logic", like *Posts*, *Users* and usually relate to a rails Model or Controller and not a specific view or page of the application.

The "lib" folder is for non-output code like mixins, variables and functions.

Global is for 'non-business' level styles, that usually relate tightly to HTML elements or layout conventions like: 'header', 'forms', 'footer', 'tables', 'buttons'.

`global/_base.sass` always exists and defines the lowest level layout and presentation. For example: `<p>`, `<h1>` to `<h6>`, `<body>` and other common root level classes/elements like `.container`, `.inner`, `.wrapper` etc.


# File structure

## Single purpose rails, middleman apps.

*Note:* For WordPress sites, the only difference is to use `style.sass` instead of `application.sass` as the root level file. `Style.css` is needed for the WordPress theme to be recognised as theme inside the WordPress backend.

All files that are pulled in via `@import` should begin with an underscore (`_`) to follow the SASS/Sprockets convention that prevents these files from being included directly. Any file that is included directly with `stylesheet_link_tag` should not have an underscore and should live in the root `app/assets/stylsheets` directory.

```
+ modules/
  - _sidebar.sass
  - _search.sass
+ global/
  - _base.sass
  - _table.sass
  - _fonts.sass
  - _forms.sass
  - _buttons.sass
  - _header.sass
  - _footer.sass
+ lib/
  - _functions.sass
  - _variables.sass
  - _mixins.sass
  - _extends.sass
- application.sass
```

## Multiple purpose rails app (public/admin/webapp/etc.)

```
+ lib/
+ global/
+ modules/

- public.sass
+ public/
	+ lib/
	+ global/
	+ modules/

- webapp.sass
+ webapp/
	+ lib/
	+ global/
	+ modules/

- admin.sass
+ admin/
	+ lib/
	+ global/
	+ modules/
```

# Content of _variables.sass

## Colours

At the top of the file, define base colours and branding variables with variable names that make them immediately recognisable and are namespaced to the application. For example:

```sass
$tiinkk-green: green
$tiinkk-aqua: aqua
```

These brand variables, ideally, shouldn't be used directly, instead should be assigned to more generic use-case-specific names. For example:

```sass
$sidebar-background: $tiinkk-green
```

## Defaults

The `_variables.sass` should also define commonly used rules like default **radius**, **margins**, transition **durations**, font-families and sizes. For example:

```sass
$link-hover: $tiinkk-green
$default-radius: 5px
$default-duration: .3s
```

### Example

```sass
// Base Colors
$tiinkk-deep-blue: #003c78
$tiinkk-pale-blue: #b7cce0
$tiinkk-teal: #00a4ee
$tiinkk-light-blue: #0083bd

$footer-background: $dark-grey
$social-bar-background: lighten($dark-grey, 20%)

$link: $tiinkk-teal
$link-hover: $tiinkk-light-blue

$text-color: #444
$quote-color: #888
$text-header: #242a30

$confirmation-background: #22343434
$confirmation-border: darken($confirmation-background, 20%)
$confirmation-text: darken($confirmation-background, 30%)

// Fonts
$font-title: Ubuntu, Helvetica, Arial, sans-serif
$font-body: Helvetica, Arial, sans-serif
$font-body-default: normal 14px/1.5 $fontBody
$font-heading-default: 200 16px/1.3 $fontTitle

// Defaults
$default-radius: 6px
$default-duration: .25s
```

# Content of application.sass

### Example

```sass
// frameworks (and their modules)
@import compass
@import compass/reset
@import compass/layout/stretching

// libraries
@import functions, variables, mixins, extends

// global
@import global/base
@import global/buttons
@import global/forms
@import global/tables
@import global/header
@import global/banner
@import global/main
@import global/sidebar
@import global/footer

// modules
@import modules/home
@import modules/pages
@import modules/services
@import modules/news
@import modules/staff
@import modules/brochures
@import modules/employment
@import modules/locations
```


# Content of _functions.sass

### Example

```sass
@function black($opacity)
  @return rgba(0, 0, 0, $opacity)

@function white($opacity)
  @return rgba(255, 255, 255, $opacity)

@function whiten($color, $amount: 0.5)
  @return mix(#fff, $color, percentage($amount))

@function blacken($color, $amount: 0.5)
  @return mix(#000, $color, percentage($amount))
```


# Content of _mixins.sass

### Example

```sass
// Default value set to 1.3 to target Google Nexus 7 (http://bjango.com/articles/min-device-pixel-ratio/)
=media-retina($ratio: 1.3)
  @media only screen and (-webkit-min-device-pixel-ratio: $ratio), only screen and (min--moz-device-pixel-ratio: $ratio), only screen and (min-device-pixel-ratio: $ratio)
    @content

// display retina images/spritesheets
=retinafy($filename, $inline: false)
  background-image: image-url($filename)
  @if $inline
    background-image: inline-image($filename)
  +background-size(image-width($filename)/2 image-height($filename)/2)

// hide something from view but not from screenreaders
=visuallyhidden
  border: 0
  clip: rect(0 0 0 0)
  height: 1px
  margin: -1px
  overflow: hidden
  padding: 0
  position: absolute
  width: 1px

// Form input placeholders
=placeholder
  \::-webkit-input-placeholder
    @content
  \:-moz-placeholder // Firefox 18-
    @content
  \::-moz-placeholder // Firefox 19+
    @content
  \:-ms-input-placeholder
    @content
```


# Content of _extends.sass

### Example

```sass
%hide-text
  color: transparent
  font-size: 0
  overflow: hidden
  text-align: left
  text-indent: -100px

%list-reset
  list-style: none
  margin: 0
  padding: 0

%sprites
  background-image: image-url("sprites.png")
  background-repeat: no-repeat
  html.svg &
    background-image: image-url("sprites.svg")
```

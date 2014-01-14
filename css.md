## Table of Contents

* [Syntax](#syntax)
  * [SASS vs. SCSS](#sass-vs-scss)
  * [Pixels vs. Em/Pts](#pixels-vs-empts)
* [Directory Structure](#directory-structure)
  * [File Naming](#file-naming)
  * [Modules](#modules-modules)
  * [Libraries](#libraries-lib)
  * [Global](#global-global)
* [Ideal Directory Structures](#ideal-directory-structures)
  * [Single Purpose Applications](#single-purpose-rails-middleman-wordpress-applications)
  * [Multipurpose Rails Applications](#multipurpose-rails-applications)
* [Example Files](#example-files)
  * [Variables](#variables-lib_variablescsssass)
  * [Application](#application-applicationcsssass)
  * [Functions](#functions-lib_functionscsssass)
  * [Mixins](#mixins-lib_mixinscsssass)
  * [Extends](#extends-lib_extendscsssass)

# CSS / SASS

At The Frontier Group we use SASS, so this style guide will focus primarily on the usage of SASS, however many of these rules will also apply to vanilla CSS.

## Syntax

* Use two **spaces** per indentation level. No hard tabs.
* Put spaces after `:` in property declarations.
* Use spaces after comma separated values, let the minifier take care of compression.
* Use hex color codes, shortening to the shorthand form where possible:

  ```sass
  body
    color: rgb(0, 17, 34) // bad
    color: #001122 // better
    color: #012 // best
  ```

* Leverage [SASS color functions](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html) to make your life easier, for example `rgba()` can take hex values; `rgba(#666, 0.5)` will result in `rgba(102, 102, 102, 0.5)`
* Define your own SASS functions for commonly used operations.
* Don't use the Property Syntax (`:property_syntax`)

  ```sass
  // good
  #main
    color: blue
    font-size: 8px

  // bad
  #main
    :color blue
    :font-size 0.3em
  ```

### SASS vs. SCSS

At TFG we have standardised on [SASS’ indented syntax](http://sass-lang.com/docs/yardoc/file.INDENTED_SYNTAX.html) as it is more concise and shares the same indentation sensitive style of [HAML](http://haml.info/). This may be a point of contention as the Rails community and the SASS authors have standardised on the SCSS syntax due to its similarities to CSS and reduced learning curve, while we feel we’re smart enough to handle the syntax change and the addition of superfluous curly-braces and semicolons provides little-to-no benefit.

### Pixels vs. Em/Pts

Use `px` for `font-size`, because it offers absolute control over text. Most modern browsers now do full screen zooming rather than text zooming. Additionally, unit-less line-height is preferred because it does not inherit a percentage value of its parent element, but instead is based on a multiplier of the `font-size`.

## Directory Structure

### File Naming
All files that are pulled in via `@import` or `= require` should begin with an underscore (`_`) to follow the SASS/Sprockets convention that prevents these files from being included directly. Any file that is included directly with `stylesheet_link_tag` should not have an underscore and should live in the root `app/assets/stylsheets` directory.

SASS file names should end in `.css.sass` to follow the Sprockets naming convention that defines what preprocessors to use in reverse order.

### Modules (`modules`)
Modules are “business logic elements”, like *Posts* or *Users* and usually relate to a Rails Model and not a specific view or page of the application. We would ideally like to avoid view-specific styles, instead keeping them as generic as possible should the element or Module be moved to more than one view.

### Libraries (`lib`)
The Libraries directory is for non-output code like mixins, variables and functions. The Rails `lib/assets/stylesheets` directory should continue to be used for truly shared libraries and the `vendor/assets/stylesheets` folder for vendor assets.

### Global (`global`)
The Global directory is for non-business level styles, that usually relate tightly to HTML elements or layout conventions like, for example: 'header', 'forms', 'footer', 'tables' or 'buttons'.

The `global/_base.css.sass` file should always exists and defines the lowest level layout and presentation. For example: `<p>`, `<h1>` to `<h6>`, `<body>` and other common root level classes/elements like `.container`, `.inner`, `.wrapper` etc. This is where the base styling for the application really starts.


# Ideal Directory Structures

## Single Purpose Rails, Middleman, WordPress Applications

 > *Note:* For WordPress sites, the only difference is to use `style.css.sass` instead of `application.css.sass` as the root level file. `Style.css` is needed for the WordPress theme to be recognised as theme inside the WordPress backend.

### Directory Structure
* `stylesheets`
  * `modules`
    * `_sidebar.css.sass`
    * `_search.css.sass`
  * `global`
    * `_base.css.sass`
    * `_table.css.sass`
    * `_fonts.css.sass`
    * `_forms.css.sass`
    * `_buttons.css.sass`
    * `_header.css.sass`
    * `_footer.css.sass`
  * `lib`
    * `_functions.css.sass`
    * `_variables.css.sass` ([example](#variables-lib_variablessass))
    * `_mixins.css.sass` ([example](#mixins-lib_mixinssass))
    * `_extends.css.sass` ([example](#extends-lib_extendssass))
  * `application.css.sass` ([example](#application-lib_applicationsass))

## Multipurpose Rails Applications

### Suggested Naming Convention

 * Brochure site for the general public: `public`
 * Administration backend: `admin`
 * Web application: `application` or `webapp`

### Directory Structure
 * `stylesheets`
  * `lib`
  * `global`
  * `modules`
  * `public.css.sass`
  * `public`
    * `lib`
    * `global`
    * `modules`
  * `webapp.css.sass`
  * `webapp`
    * `lib`
    * `global`
    * `modules`
  * `admin.css.sass`
  * `admin`
    * `lib`
    * `global`
    * `modules`

# Example Files

## Variables (`lib/_variables.css.sass`)

Variables should match the naming convention of the language they’re written in, so camelCase is not ideal for SASS, SCSS, LESS or CSS, instead use `$dashed-names`.

At the top of the file, define brand specific variables using the application’s namespace, for example:

```sass
$tiinkk-green: green
$tiinkk-aqua: aqua
```

These brand variables, ideally, shouldn’t be used directly, instead should be assigned to more generic module or use-specific names, for example:

```sass
$sidebar-background: $tiinkk-green
```

It’s suggested to define visual rythm / default variables to ensure your application is styled consistently, some examples include; `border-radius`, `margins`, `transition durations`, `font-families` and `font-sizes`.

```sass
// Brand Colors
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
$body-font-family: Helvetica, Arial, sans-serif
$title-font-family: Ubuntu, Helvetica, Arial, sans-serif
$body-font: normal 14px/1.5 $body-font-family
$heading-font: 200 16px/1.3 $title-font-family

// Defaults
$default-radius: 6px
$default-duration: .25s
```

## Application (`application.css.sass`)

This should ideally be the only file that is included directly via `stylesheet_link_tag`. We also want to define specific order of import for the `lib` files as we may be making use of `_functions.css.sass` in `_variables.css.sass` or `_variables.css.sass` in `_mixins.css.sass`.

> Note that SASS `@import` does not require the explicit use of underscores for filenames.

```sass
@import compass
@import compass/reset

@import lib/functions
@import lib/variables
@import lib/mixins
@import lib/extends

@import global/*

@import modules/*
```

## Functions (`lib/_functions.css.sass`)

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


## Mixins (`lib/_mixins.css.sass`)

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


## Extends (`lib/_extends.css.sass`)

```sass
%hide-text
  color: transparent
  font-size: 0
  overflow: hidden
  text-align: left
  text-indent: -100px
  text-shadow: none

%sprites
  background-image: image-url("sprites.png")
  background-repeat: no-repeat
  html.svg &
    background-image: image-url("sprites.svg")
```

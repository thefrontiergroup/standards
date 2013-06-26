At The Frontier Group we use Sass, so the style guide will focus primarily on the usage of Sass, however many of these rules will also apply to vanilla CSS.

## Table of Contents

* [Syntax](#syntax)
  * [Sass vs. SCSS](#sass-vs-scss)

## Syntax

* Use two **spaces** per indentation level. No hard tabs.
* Put spaces after `:` in property declarations.
* Use hex color codes, shortening to the shorthand form where possible:
  ```sass
  body
    color: rgb(0,0,0) // bad
    color: #001122 // bad
    color: #012 // good
  ```
* Leverage [Sass’ color functions](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html) to make your life easier, for example `rgba()` can take hex values; `rgba(#666, 0.5)` will result in `rgba(102, 102, 102, 0.5)`
* Don't use the Property Syntax (`:property_syntax`)
  ```sass
  // good
  #main
    color: blue
    font-size: 0.3em

  // bad
  #main
    :color blue
    :font-size 0.3em
  ```

### Sass vs. SCSS

At TFG we have standardised on [Sass’ indented syntax](http://sass-lang.com/docs/yardoc/file.INDENTED_SYNTAX.html) as it is more concise and shares the same indentation sensitive style of [HAML](http://haml.info/). This may be a point of contention as the Rails community and the Sass authors have standardised on the SCSS syntax due to its similarities to CSS and reduced learning curve, while we feel the addition of curly-braces and semicolons is superfluous.

## Pixels vs. Ems

Use `px` for `font-size`, because it offers absolute control over text. Most modern browsers now do full screen zooming rather than text zooming. Additionally, unit-less line-height is preferred because it does not inherit a percentage value of its parent element, but instead is based on a multiplier of the `font-size`.
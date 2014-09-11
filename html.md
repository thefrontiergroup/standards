# HAML

We use HAML at The Frontier Group, ERB is only acceptable for mailers, plain text and non-HTML rendering.

## Syntax

* Use two **spaces** per indentation level. No hard tabs.
* Use Ruby 1.9 hash syntax for attributes
* Use curly braces for attributes
* Use only lowercase code
* Use HAML (`-#`) instead of HTML (`/`) comments (exception for conditional comments like below)
* Use Rails TagHelpers like `image_tag` instead of `%img`

## The `<html>` tag

```haml
!!!5
/[if IE 8]<html class="no-js lt-ie9" lang="en">
/[if IE 9]<html class="no-js lt-ie10" lang="en">
/[if lt IE 10]<html class="no-js lt-ie10" lang="en-us">
<!--[if !IE]><!-->
%html.no-js{lang: "en"}<>
  <!--<![endif]-->
```

Unfortunately for us, IE is still relevant and we should always be using this `<html>` setup for **all** sites and apps (except for admin/backend areas).

## Meta rules

Make sure to specify the encoding in the HTML templates and documents before any tags.

```haml
%head
  %meta{charset: "utf-8"}
  %title
```

## Modernizr

[Modernizr](http://modernizr.com/) needs to be included in the `<head>` of **every** app. Not so much for the feature sniffing (though we obviously should be using it) but for html5shiv. Many of our apps will be completely broken in IE8 and under without it.

When using Modernizr, be sure to [**custom build**](http://modernizr.com/download/) per application and un-check unnecessary tests. We should only be seeing a handful of tests on any given app.

## Semantics

### Use HTML according to its purpose.

Use elements for what they have been created for. For example, use heading elements for headings, `p` elements for paragraphs, `a` elements for anchors, etc.

### Provide alternative contents for images

Providing alternative contents is important for accessibility reasons: a blind user has few cues to tell what an image is about without `alt` attribute.

For images whose `alt` attributes would introduce redundancy, and for images whose purpose is purely decorative which you cannot immediately use CSS for, use no alternative text, as in `alt=""`.

### Use meaningful or generic ID and class names.

Instead of presentational or cryptic names, always use ID and class names that reflect the purpose of the element in question, or that are otherwise generic.

Names that are specific and reflect the purpose of the element should be preferred as these are most understandable and the least likely to change.

Generic names are simply a fallback for elements that have no particular or no meaning different from their siblings. They are typically needed as "helpers".

Using functional or generic names reduces the probability of unnecessary document or template changes.

```haml
-# Not recommended: meaningless
#uee-1984

-# Not recommended: presentational
.button-green
.grid-5
.container-left

-# Recommended: specific
#gallery
#login
.video

-# Recommended: generic
.actions
.btn-default
.wrapper
```

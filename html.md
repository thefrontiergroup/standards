# HAML

We use HAML at The Frontier Group, ERB is only acceptable for mailers, plain text and non-HTML rendering.

## Syntax

* Use two **spaces** per indentation level. No hard tabs.
* Use Ruby 1.9 hash syntax for attributes

## The `<html>` tag

```haml
!!!5
/[if lt IE 7]<html class="no-js lt-ie9 lt-ie8 lt-ie7">
/[if IE 7]<html class="no-js lt-ie9 lt-ie8">
/[if IE 8]<html class="no-js lt-ie9">
/[if IE 9]<html class="no-js lt-ie10">
<!--[if gt IE 9]><!-->
%html.no-js<>
  <!--<![endif]-->
```

Unfortunately for us, IE is still relevant and we should always be using this `<html>` setup for **all** sites and apps (except for admin/backend areas).

## Modernizr

[Modernizr](http://modernizr.com/) needs to be included in the `<head>` of **every** app. Not so much for the feature sniffing (though we obviously should be using it) but for html5shiv. Many of our apps will be completely broken in IE8 and under without it.

When using Modernizr, be sure to custom build per application and un-check unnecessary tests. We should only be seeing a handful of tests on any given app.

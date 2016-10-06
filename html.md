# Haml

We use Haml at The Frontier Group, ERB is only acceptable for mailers, plain text and non-HTML rendering.

## Syntax

* Use two **spaces** per indentation level. No hard tabs.
* Use Ruby 1.9 hash syntax for attributes
* Use curly braces for attributes
* Use only lowercase code
* Use Haml (`-#`) instead of HTML (`/`) comments (exception for conditional comments see below)
* Use Rails TagHelpers like `image_tag` instead of `%img`

## Haml Initializer

To get more control over HTML doctypes, the default Haml format needs to be set to `:xhtml`.
This allows us to use the XHTML 1.0 Transitional doctype for email layouts and HTML5 for everything else.

**Meaning:**
* Email templates doctype: `!!!`
* Any other layout: `!!! 5`

### Adding the initializer to your project

Copy & paste the following line into `/config/initializers/haml.rb`.

```ruby
Haml::Template.options[:format] = :xhtml
```

**Related links:**
* About `doctype` in the [Haml Docs](http://haml.info/docs/yardoc/file.REFERENCE.html#doctype_)
* Article ["Correct doctype to use in HTML Emails"](https://www.campaignmonitor.com/blog/post/3317/correct-doctype-to-use-in-html-email/) by Campaign Monitor

## The `<html>` tag

```haml
!!! 5
/[if lt IE 10]<html class="no-js lt-ie10" lang="en">
<!--[if !IE]><!-->
%html.no-js{lang: "en"}<>
  <!--<![endif]-->
```

Unfortunately for us, IE is still relevant and we should be using those conditional HTML comments for **all** sites and apps (exception: admin/backend areas).

## Meta rules

Make sure to specify the encoding in the HTML templates and documents right before adding any other tags and after opening the `head` element.

```haml
%head
  %meta{charset: "utf-8"}
  %title
```

## Modernizr

When using Modernizr, be sure to [**custom build**](http://modernizr.com/download/) it per application and un-check unnecessary tests and the html5shiv. We should only be seeing a handful of (necessary) tests on any given app.

In case html5shiv is required for older versions of IE, don't add it to Modernizr. Use a conditional comment and load it from a CDN:

```haml
/[if lte IE 8]
  = javascript_include_tag "//cdnjs.cloudflare.com/ajax/libs/html5shiv/3.7.3/html5shiv.min.js"
```

**Note:** The same applies to [Respond.js](https://github.com/scottjehl/Respond) if required.

Loading fallback scripts from a CDN for older IE versions could be beneficial as it may have been cached by the browser before.

## Semantics

### Use HTML according to its purpose.

Use elements for what they have been created for. For example, use heading elements for headings, `p` elements for paragraphs, `a` elements for anchors, etc. See [HTML element reference on MDN](https://developer.mozilla.org/en/docs/Web/HTML/Element) for a detailed list of available elements.

An exemption is the `<i>` element. We typically use `i` to display icons. This way we have full control over a pseudo "icon element" and can attach multiple classes and other HTML attributes to it if necessary.

Example: `%i.ico.ico-large.ico-user{aria: {hidden: "true"}}`.

**Note:** With the addition of the `aria-hidden` attribute, we hide this element and its content from assistive technology (ie. screenreaders).

### Provide alternative contents for images

Providing alternative contents is important for accessibility reasons: a visually impaired user has limited cues to tell what an image is about without `alt` attribute.

For images whose `alt` attributes would introduce redundancy, and for images whose purpose is purely decorative (which you cannot immediately use CSS for), use no alternative text, as in `alt=""`.

Need help deciding what to do? Have a look at the [alt Decision Tree](https://www.w3.org/WAI/tutorials/images/decision-tree/).

#### Image titles

Don't use the `title` attribute to repeat the content of the `alt` attribute. This can be considered "keyword stuffing" by search engines and could be penalised in terms of the website's ranking in search results.
In most cases, if an `alt` attribute has been provided there is no need for a `title` attribute. A potential use is to provide additional information for an image, ie. a date or other information that is likely not essential.

### Use meaningful or generic ID and Class names.

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

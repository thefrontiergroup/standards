# Folder and Code Structure

The top level of the JavaScript folder structure should only include files which are directly linked from within a layout file. Any concatenated files should be placed under a directory with a matching name. Within this sub folder will be an `init.js`, all other files must be placed in a nested subfolder based on functionality.

## Ideal Directory Structure

### Webapp

* `javascripts`
  * `core`
    * `support`
      * `math.js`
    * `init.js`
  * `core.js`
  * `authenticated`
    * `models`
      * `some_model.js`
    * `init.js`
  * `authenticated.js`

Each top level file is responsible for setting up application namespaces, requiring files, and invoking the initialiser at an appropriate time. `init.js` should define a single public method to perform any initialisation logic. Even if there is no logic to be required, this method must be present, and called from the top level file.

## Example Files

Following this basic structure allows the reader to infer that no initialisation is required outside of the `init.js` files. This stops us from having to search over multiple folders/files to find where a plugin has been initialised or called. Additionally, no other file should perform any action without being invoked by the initialiser. Same goes for attaching event handlers.

### core.js

```js
    // require dependencies here

    //= require_self
    //= require ./core/init
    //= require_tree ./core

    window.Core = {}

    // Delay initalization
    setTimeout(Core.Initialize, 0);
```

### core/init.js

```js
    // require files used by initialization here

    Core.initialize = function() {
    	// Do the magic
    }
```

### authenticated.js

```js
    //= require jQuery
    //= require ./core

    //= require_self
    //= require ./authenticated/init
    //= require_tree ./authenticated

    window.App = {}

    // Delay initalization until document ready
    jQuery(App.initialize)
```

### authenticated/init.js

```js
    //= require ./models/some_model

    App.initialize = function() {
    	// Do the magic
    }
```


# Namespaces when used with HTML/CSS

As a general rule, any `class` or `id` that is used should when interacting with HTML/CSS should be lowercase & dasherized.

We want to namespace JavaScript hooks and when using states.

## State namespace

A State namespace is used to communicate a change in ‘state’.

**Example 1:**

`.has-form-actions-disabled`

The above indicates that on, or somewhere below this node, the ‘form actions’ are disabled.

**Example 2:**

`.is-selected`

This would communicate that the component or one within it is currently selected (when it previously wasn't).

**Example 3:**

`.js-countdown`

This example means this component or node has been injected via Javascript (ie. it cannot be found in the raw HTML source).

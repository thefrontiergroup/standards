# Folder and Code Structure

The top level of the JS folder structure should only include files which are directly linked, any concatenated files should be placed under a directory with a matching name. Within this sub folder will be an `init.js`, all other files must be placed in a nested subfolder based on functionality.

    - javascripts
      - core
        - support
          math.js
        init.js
      core.js
      
      - authenticated
        - models
          some_model.js 
        init.js
      authenticated.js

Each top level file is responsible for setting up application namespaces, requiring files, and invoking the initialiser at an appropriate time. `init.js` should define a single public method to perform any initialisation logic. Even if there is no logic to be required, this method must be present, and called from the top level file. 

    //---------------
    //core.js
    //---------------

    //= require dependencies here
    
    //= require_self 
    //= require ./core/init
    //= require_tree ./core

    window.Core = {}
    
    // Delay initalization
    setTimeout(Core.Initialize, 0);

    //---------------
    //core/init.js
    //---------------

    //= require files used by initialization here

    Core.initialize = function() {
    	// Do the magic
    }


    //---------------
    //authenticated.js
    //---------------

    //= require jQuery
    //= require ./core
    
    //= require_self 
    //= require ./authenticated/init
    //= require_tree ./authenticated

    window.App = {}
    
    // Delay initalization until document ready
    jQuery(App.initialize)

    //---------------
    //authenticated/init.js
    //---------------

    //= require ./models/some_model

    App.initialize = function() {
    	// Do the magic
    }
    

Following this basic structure allows the reader to infer that no initialisation is required, and is not left wondering if there is some, but just not here. Additionally, no other files should perform any action without being invoked by the initialiser, or an event handler attached by the former or an initialiser.

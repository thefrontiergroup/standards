# Rubymotion TFG Guide

## Overview

This document is to assist with implementation of ideas and standards as adopted by The Frontier Group for rubymotion development.

## Table of Contents

* [General Development](#general-development)
* [Language and Syntax](#language-and-syntax)
* [Folder Layout](#folder-layout)
* [Patterns](#patterns)
* [iOS Extending vs Subclassing](#ios-extending-vs-subclassing)
* [Rakefile](#rakefile)

## General Development

Whilst rubymotion is a ruby language implementation the TFG methodology is intended to be driven largely by Objective-C conventions rather than attempting to replace Obj-C entirely.

The reason being we are developing an Objective-C application and use the native methods and SDK functionality. Hence to develop inline with that functionality creates less of an opportunity for inconsistency. 

## Language and Syntax

### File names

Files should be named in accordance with Ruby conventions of underscores in place of capitalisation:

    ui_view.rb

### Method and Variable Naming

Methods and variables should be named in accordance with iOS standards using camel casing in all cases. It is acceptable to use underscores only for externally sourced code.

### Named Parameters / Objective C method naming

A feature derived from Obj-C is the ability to name parameters or interspersing the method name with the parameters. Via this approach it is possible to create what looks to be the same method with different outcomes based on the parameters:

    def showTable(table); end
    
    def showTable(table, withContent: content); end
    
It is acceptable and at times recommended to use this approach as it strengthens compile time validation

### New vs alloc.init

It is recommended that alloc.init be used as the common means of instantiating new objects as it is the traditional Objective-C approach and what is actually used when the rubymotion convention of 'new' is called.

Some Objective-C libraries attach alternative meaning to the 'new' method so you can encounter naming clashes / issues.

By calling alloc.init the desired outcome will always be achieved. 

### Namespacing

It is suggested that application code that is to revolve around singletons be structured under a common namespace such as Application. 

eg

    App::GlobalTimer.instance.start()

Note the use of an 'instance' class level method which would create the one shared instance.

For other code such as controllers, models and views it is not recommended these be namespaces as it provides no real 

## Folder Layout

### General layout

Along with the standard MVC folders provided by the rails convention, it is acceptable to have the following:

    app
      controllers
      models
      views
        subviews
      lib
        managers
        extensions
        
*For subviews see Patterns::Subviews*
*For managers see Patterns::Managers*   

### Lib folder

This folder has the potential to be difficult to interpret with mixed intentions. It is recommended that any common types be bundled under a further subdirectory to assist in organising and understanding the code.

See also Rakefile::Load Order

## Patterns

### Subviews

It is generally expected that a controller will be paired with a view. This view however may be made up of reusable components as per partials in the rails world.

It is expected that any view stored in the subviews folder is a portion of a page, not a page itself and that these views may be used in more than one area of an application.

### Managers

Managers are a deviation from MVC in that there are cases where we require a globally accessible, single point of communication to drive a shared action or manage a shared instance.

A manager is a singleton and it is suggested the singleton be namespaces given it can be said to be 'The Applications something manager'. 

In all cases it is expected that a manager has no view tied to it as they are a container for logic and actions.

Instantiation of managers can either be via a class level instance method

	module App
	  class GlobalManager
	    def self.instance
	      @instance ||= self.alloc.init
	    end
	  end
	end
	
or via a namespaced method

	module App
	  def self.globalManager
	  	@globalManager ||= GlobalManager.alloc.init
	  end
	  
	  class GlobalManager
	  …
	  end
	end

#### Action Managers

Action managers are a common interface triggered by such things as:

* System events (connectivity changes, GPS alerts)
* Timers
* Background async actions

These manager types perform an action based on these events which could be influencing multiple models, event notifications and controllers.

#### Entity Managers

Entity managers provide a common interface into the management of an object such as a globally accessible way of asking an audio player to load the next song. As with action managers these managers are largely performing actions based on an event trigger but they are tightly coupled to a single entity instance that is being managed.

### Controller view injection

It is recommended that views be instantiated in the loadView method unless the standard view controller is intended to be built upon

## iOS Extending vs Subclassing

Via ruby language features it is possible to monkey patch additional functionality into even the core objective-c framework. This however should be done cautiously.

It is acceptable to add in some level of functionality to classes such as UIView. It however is not recommended to substitute functionality for these methods given it could have major implications to other areas of the app. The approach of replacing core functionality or in Objecive-c terms 'method swizzling' is also an approach that can cause your app to be rejected.

Note it is best practice to store language extensions in the lib/extensions folder and named with the class you are modifying.

eg.

    lib
      extensions
        ui_view.rb

## Rakefile

### Cocoapods - WARNING

Cocoapods is a collection of code offerings, not a contribution from a single trusted developer. Hence it is not always clear if including a cocoapod is safe.

One example of this is the testflight and crittercism pods add to the libraries variable, both libraries and load flags. These load flags will override your ability to set :force_load=>false on a vendored library.

Instead it is possible to vendor the libraries yourself and statically load them without the flags. 

### Load Order

When compiling an application it is often necessary to force the load order of certain files once you begin to extend and include code.

In the Rakefile you can add the following to prioritise the loading of your lib folders:

	app.files = Dir.glob(File.join(app.project_dir, 'app/lib/**/*.rb')) | app.files
	
It is not recommended that you replace entirely the app.files with your own loading as other libraries such as cocoa pods will populate the variable. Instead manipulating the existing file set is better.
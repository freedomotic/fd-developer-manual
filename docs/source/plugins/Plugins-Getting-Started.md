##What is a plugin##
Freedomotic is an application extensible through plugins. Plugins are simple classes within a .jar java package. Each plugin is deployed in the _FREEDOMOTIC_ROOT/plugins/_ folder and loaded and initialized automatically at Freedomotic startup.

The communication between the plugin and Freedomotic is automatically managed via a Message Oriented Middleware. Plugins in addition to the 'Manager of the messages' have direct access to Freedomotic data structures. In a plugin, you can create, read, update, or delete data and use them to accomplish your goals.

![Freedomotic architeture layers](http://freedomotic.com/images/wiki/architecture-layers.png)

##Plugins features##

* plugin configuration management
* user interface accessible by right click in plugins list
* simplyfied access to freedomotic data structures
* automatic management of plugin lifecycle (loaded, running, stopped,...)
* access to the messaging system (read/write events and commands)
* installation and upgrade of plugins from a marketplace
* simplified programming implementing events (onCommand(), onRun(), onStart, onStop(), ...). 


##How to make a non-Java application communicate with Freedomotic##

Till now we talked about how to extend Freedomotic with Java plugins. However is possible to make non-Java application communicate with Freedomotic. Take a look at <https://github.com/freedomotic/freedomotic/wiki/Freedomotic-APIs>
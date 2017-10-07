
What is a plugin?
=================

Freedomotic is an application extensible through plugins. Plugins are simple classes within a .jar java package. Each plugin is deployed in the FREEDOMOTIC_ROOT/plugins/ folder and loaded and initialized automatically at Freedomotic startup.
The communication between the plugin and Freedomotic is automatically managed via a Message Oriented Middleware. Plugins in addition to the 'Manager of the messages' have direct access to Freedomotic data structures. In a plugin, you can create, read, update, or delete data and use them to accomplish your goals.

Plugin Features
###############

#. Plugin configuration management
#. User interface accessible by right click in plugins list
#. Simplyfied access to freedomotic data structures
#. Automatic management of plugin lifecycle (loaded, running, stopped,...)
#. Access to the messaging system (read/write events and commands)
#. Installation and upgrade of plugins from a marketplace
#. Simplified programming implementing events (onCommand(), onRun(), onStart, onStop(), ...).

How to make a non-Java application communicate with Freedomotic
###############################################################
Till now we talked about how to extend Freedomotic with Java plugins. However is possible to make non-Java application communicate with Freedomotic. Take a look at https://github.com/freedomotic/freedomotic/wiki/Freedomotic-APIs


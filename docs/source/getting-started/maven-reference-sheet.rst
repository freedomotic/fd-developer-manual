
Maven quick reference sheet
===========================
Starting from version 5.5 Freedomotic build cycle is managed with Apache
Maven. This quick reference explains how maven phases are bound to
specific tasks:

Priming build
-------------

*first time compile or to refresh the entire project and submodules*

This will compile freedomotic-core and all basic plugins like
base-objects, java-frontend, ...

::

    cd FREEDOMOTIC_ROOT
    mvn clean install

How to start Freedomotic
------------------------

You can do it from command line using

::

    cd FREEDOMOTIC_ROOT
    java -jar framework/freedomotic-core/target/freedomotic-core/freedomotic.jar

As an alternative you can start **freedomotic-core** project from your
favourite IDE.

Compile and test your own plugin
--------------------------------

*after doing changes to the plugin code...*

This will compile your plugin and install it automatically into the
Freedomotic runtime (**freedomotic-core**) ready to be started

::

    cd FREEDOMOTIC_ROOT/plugins/devices/YOUR_PLUGIN_NAME
    mvn clean install

Upload your own plugin on the marketplace
-----------------------------------------

*share your own plugin in a convenient, easy to install, way*

This will compile your own plugin, deploy it on online maven repository
and publish the new artifact on the related Freedomotic website
marketplace page.

::

    cd FREEDOMOTIC_ROOT/plugins/devices/YOUR_PLUGIN_NAME
    mvn clean deploy -P market -D username="name" -D password="password"

more details at how to publish a plugin.

To know more about maven phases refer to the article "`Maven: introduction to
the
lifecycle <https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html>`__"

That's all. Open your favourite IDE and start the **freedomotic-core**
project to run Freedomotic on your PC.

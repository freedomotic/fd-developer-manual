
Maven quick reference sheet
===========================
Starting from Version 5.5, the Freedomotic build cycle is managed with Apache
Maven. This quick reference explains how Maven phases are bound to
specific tasks:

Priming build
-------------

*First time compile, or to refresh the entire project and submodules*

This will compile freedomotic-core and all basic plugins like
base-objects, java-frontend, etc.

::

    cd FREEDOMOTIC_ROOT
    mvn clean install

How to start Freedomotic
------------------------

You can do so from command line using

::

    cd FREEDOMOTIC_ROOT
    java -jar framework/freedomotic-core/target/freedomotic-core/freedomotic.jar

As an alternative, you can start **freedomotic-core** project from your
favourite IDE.

Compile and test your own plugin
--------------------------------

*After doing changes to the plugin code...*

This will compile your plugin and install it automatically into the
Freedomotic runtime (**freedomotic-core**), ready to be started

::

    cd FREEDOMOTIC_ROOT/plugins/devices/YOUR_PLUGIN_NAME
    mvn clean install

Upload your own plugin on the marketplace
-----------------------------------------

*Share your own plugin in a convenient and easy to install way*

This will compile your own plugin, deploy it to the online Maven repository
and publish the new artifact on the related Freedomotic website
marketplace page.

::

    cd FREEDOMOTIC_ROOT/plugins/devices/YOUR_PLUGIN_NAME
    mvn clean deploy -P market -D username="name" -D password="password"

More details at how to publish a plugin.

To know more about Maven phases, refer to the article "`Maven: introduction to
the
lifecycle <https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html>`__"


That's all!
Open your favourite IDE and start the **freedomotic-core**
project to run Freedomotic on your PC.

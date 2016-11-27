Publish your plugin binaries on the official marketplace
========================================================

First time configuration
------------------------

First of all create a new plugin page on
http://www.freedomotic.com/node/add/plugin (requires a registration). After your plugin page is
created enter in **Edit mode** and look at its address in your browser bar.
Take note of the number in it for example if you have
*www.freedomotic.com/node/850/edit* your nodeid is 850, remember it.

Open your *pom.xml* file and write this number as the
value of **nodeid** property. This way you are linking your plugin with the
related marketplace page on the website.

Then to upload just do (from command line)

.. code:: 
 
    cd FREEDOMOTIC_ROOT/plugins/devices/YOUR_PLUGIN_NAME
    mvn clean deploy -P market -D username="YOUR_FREEDOMOTIC.COM_USERNAME" -D password="YOUR_FREEDOMOTIC.COM_PASSWORD"

Redo it any time you want to update the downloadable binaries on the marketplace page.

Publish plugin source code on GIT repository
--------------------------------------------

Please follow the instructions at
http://www.freedomotic.com/wiki/external-developers-workflow-recommended

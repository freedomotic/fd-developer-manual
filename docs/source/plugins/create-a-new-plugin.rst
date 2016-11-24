
Create a new plugin
###################

From template
-------------

1. Copy and paste the hello-world example you can find in
   GIT_FOLDER/plugins/devices/hello-world. Open this project along with
   **freedomotic-core** in your favourite IDE and make your changes. Any
   time you compile the plugin will be installed into *freedomotic-core* folder,
   so you can simply start it to see your plugin running.
2. Implement the **HelloWorld.java** class methods and rename the class and
   the `path to
   manifest <https://github.com/freedomotic/freedomotic/wiki/Plugin-manifest-and-configuration>`__
   in the class Constructor according to the new name of your plugin.
3. Compile the plugin and start Freedomotic to test it.
4. Add commands, triggers and resources in the
   *src/main/resources/data/* folder of this plugin (take a look at the
   folder diagram above)

From an archetype
-----------------

Archetypes can help you to make the development of new plugins easier
starting from a well-defined template. More info on
http://maven.apache.org/guides/introduction/introduction-to-archetypes.html

The archetype should be compiled with

.. code:: 

   mvn clean install

and added to the local list of archetypes with

.. code::

   mvn archetype:crawl

Now localhost is ready to create projects using this archetype

.. code::

   cd freedomotic/plugins/devices
   mvn archetype:generate -Dfilter=com.freedomotic:device -DgroupId=com.freedomotic -Dversion=1.0-SNAPSHOT -Dpackage=com.freedomotic

Add your plugin name when asked and confirm. The plugin skeleton is now
created so compile it with:

.. code::

   cd PLUGIN_NAME
   mvn clean install

At this point the new plugin is installed in Freedomotic as usual, the
developer can start to code it.

Please remember to rename the default Java class **HelloWorld** as your
plugin name.

Behind the scene
----------------

If you want to understand how the archetype works take a look
`here <https://github.com/freedomotic/freedomotic/tree/master/tools/freedomotic-device-maven-archetype>`__.

TODO
`Update maven archetype code to match latest freedomotic
version and best
practices <https://github.com/freedomotic/freedomotic/issues/150>`__

Plugin folder structure
-----------------------

::

    frontend-java/
    ├── pom.xml                                     //THE MAVEN POM FILE
    └── src
        ├── main
        │   ├── java
        │   │   └── com
        │   │       └── freedomotic
        │   │           └── jfrontend
        │   │               └── JavaDesktopFrontend.java    //THE PLUGIN SOURCE CODE
        │   └── resources
        │       ├── data (PLUGIN DATA FOLDER)
        │       │   └── i18n
        │       │       ├── jfrontend_it_IT.properties  //ITALIAN TRANSLATION
        │       │       ├── jfrontend.properties        //DEFAULT LANGUAGE
        │       │   └── cmd                             //PLUGINS COMMANDS XML FILES
        │       │   └── rea                 //PLUGINS REACTIONS/AUTOMATIONS XML FILES
        │       │   └── templates           //OBJECTS TEMPLATES PROVIDED BY THIS PLUGIN
        │       │   └── trg                 //PLUGINS TRIGGERS XML FILES
        │       │   └── resources           //PLUGINS MEDIA FILES
        │       └── desktop-frontend.xml    //THE PLUGIN MANIFEST
        └── test
            └── java
                └── com
                    └── freedomotic
                        └── jfrontend
                            └── JavaDesktopFrontendTest.java //UNIT TESTS FILE

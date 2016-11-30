
Add new thing templates
=======================

This feature is useful to allow device plugins to create fully
customized objects using autodiscovery feature. The object can come with
the right mapping in **Data sources**/**Actions** or you can even change its
representation (eg: a light with a custom icon).

To test it

1. create a *src/main/resources/data/templates* folder inside a device
   plugin (eg: essential)

2. copy the *light.xobj* object from **base-things** plugin in this new folder

3. change the name of this file to *mytemplate.xobj* and change xml name
   tag to ``<name>My Test Template</name>``

4. recompile **essential** plugin to have these changes installed in
   **freedomotic-core**

5. run **freedomotic-core**.


In **Jfrontend** enter **Objects Edit mode** and you should see a new template in the list
called **My Test Template** which is provided by a device plugin instead
of an object plugin. 

Try to add it to the map as usual and check if
this new light turns on when clicked.

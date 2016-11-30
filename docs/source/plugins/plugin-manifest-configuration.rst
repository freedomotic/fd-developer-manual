
Plugins manifest and configuration
==================================

Every plugin needs a XML manifest file to describe its configuration. As multiple plugins can be in the same package (the one you download from the marketplace) then more than one manifest file can be in the root folder of a plugin package.

The binding is made in the class constructor with

.. code:: java

   public MyPlugin() {
     super("Hello World Sensor" ,"/firstsensor/first-sensor-manifest.xml");
   }

In this example the manifest file is *first-sensor-manifest.xml* located into *firstsensor* folder. 

The path is case sensitive so */firstsensor/first-sensor-manifest.xml* is not the same as */FirstSensor/First-Sensor-Manifest.xml".*

What's inside the manifest
--------------------------

This is an example of the most simple manifest file you can have:

.. code:: xml

   <config>
       <properties>
          <property name="name" value="Hello World"/>
          <property name="description" value="A basic plugin that prints 'Hello World' on standard output"/>
          <property name="category" value="examples"/>
          <property name="short-name" value="hello-world"/>
      </properties>
   </config>

Every plugin has an unique input **Messaging Channel** used for message exchange; it is addressed using the info you put in the manifest file.
For plugins the Channel name is: ``app.actuators.CATEGORY.SHORT-NAME.in``.

The manifest file is the **ONLY** place where you should add configuration parameters for your plugin. You should not use external files.

You can add custom properties to this list. The properties can be retrieved programmatically this way:

.. code:: java

    int ServerPort = configuration.getIntProperty("udp-server-port", 7331); //defaults to 7331 if the property is not found in the manifest
    String Delimiter = configuration.getStringProperty("delimiter", ":"); //defaults to ':' if the property in not found in the manifest


Add configuration blocks to your plugin
---------------------------------------

If you have to configure multiple the same set of properties for different things (eg: URL and port of a set of hardware boards) you can use **tuples**.

You can add as many ``<tuple></tuple>`` blocks as you need. The ``<tuples></tuples>`` block may be added after ``<properties></properties>`` on the same hierarchical level.

Tuples are useful to have configuration data specific for you plugin to be loaded in Freedomotic at startup.

.. code:: xml
  
     <tuples>
        <tuple>
          <property name="Name" value="TemperatureZone1"/>
          <property name="SlaveId" value="1"/>
          <property name="RegisterRange" value="HOLDING_REGISTER"/>
          <property name="DataType" value="TWO_BYTE_INT_UNSIGNED"/>
          <property name="Offset" value="266"/>
          <property name="NumberOfRegisters" value="1"/>
          <property name="Multiplier" value="0.1d"/>
          <property name="Additive" value="0.0d"/>
          <property name="EventName" value="TemperatureZone1"/>
       </tuple>
       <tuple>
          <property name="Name" value="TemperatureZone2"/>
          <property name="SlaveId" value="1"/>
          <property name="RegisterRange" value="HOLDING_REGISTER"/>
          <property name="DataType" value="TWO_BYTE_INT_UNSIGNED"/>
          <property name="Offset" value="522"/>
          <property name="NumberOfRegisters" value="1"/>          
          <property name="Multiplier" value="0.1d"/>
          <property name="Additive" value="0.0d"/>
          <property name="EventName" value="TemperatureZone2"/>
       </tuple>
     </tuples>


You can use free custom strings for the attribute name and the value. 



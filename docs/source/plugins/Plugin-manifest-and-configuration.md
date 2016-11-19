Every Java plugin needs an XML manifest file to describe the plugin to Freedomotic. As multiple plugins can be in the same plugin package (the one you download from the marketplace) then more than one manifest file can be in the root folder of a plugin package.

In Java you bind your plugin to its manifest in the class Constructor this way:
```java
public MyPlugin() {
  super("Hello World Sensor" ,"/firstsensor/first-sensor-manifest.xml");
}
```

The manifest filename is defined in the plugin constructor (in this case "first-sensor-manifest.xml"). 
**The path is case sensitive so "/firstsensor/first-sensor-manifest.xml" is not the same as "/FirstSensor/First-Sensor-Manifest.xml".**

## What's inside the manifest ##
This is an example of the most simple manifest file you can have:

```xml
<config>
    <properties>
        <property name="name" value="Hello World"/>
        <property name="description" value="A basic plugin that prints 'Hello World' on standard output"/>
        <property name="category" value="examples"/>
        <property name="short-name" value="hello-world"/>
    </properties>
</config>
```

Every plugin has a unique input ''Messaging Channel'' used for message exchange; it is addressed using the info you put in the manifest file.
For plugins the Channel name is: `app.actuators.CATEGORY.SHORT-NAME.in`

The plugin manifest is the ONLY place you should add configuration parameters for your plugin. You should not use external files; the manifest is all you need to include complex configuration options that can be changed at runtime using a Freedomotic frontend.

**You can add custom properties to this list. The properties can be retrieved programmatically this way:**

```java
int ServerPort = configuration.getIntProperty("udp-server-port", 7331); //defaults to 7331 if not found in manifest
String Delimiter = configuration.getStringProperty("delimiter", ":"); //defaults to ':' if not found in manifest
```

## Add configuration blocks to your plugin ##


**If you have to configure multiple the same set of properties for different objects (eg: URL and port of a set of hardware boards) you can use tuples.**

You can add as many <tuple></tuple> blocks as you need. The <tuples></tuples> block may be added after <properties></properties> on the same hierarchical level. Tuples are useful to have configuration data specific for you plugin to be loaded in Freedomotic at startup. Related configuration data can be stored in blocks called tuple.
```xml
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
```

This data is automatically available to your plugin. You can use free custom strings for the attribute name and the value. To read these tuples variables programmatically see the [Data Access Freedomotic Data page]

## Freedomotic reserved properties for special case configurations (DEPRECATED) ##
The following are optional properties used to manage special use cases. They are not needed in standard plugins development so add them only if you know what you are doing.

Property | Values | Description
---------|--------|------------
threaded-commands-execution | true / false | An Actuator plugin can choose to execute every received command in a separate thread or to execute them in sequence reading from a queue. Default value is true.
automatic-reply-to-commands | true / false | An Actuator plugin can choose if the system has to send automatically a command execution reply or is the developer who call it using command.reply() method. This is useful for example if you have to wait results from threaded executors.
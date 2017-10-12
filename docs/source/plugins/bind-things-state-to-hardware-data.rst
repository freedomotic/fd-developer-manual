Bind things state to hardware data
==================================

To integrate new building automation protocols, (eg: zwave, zigbee,
...) you can create a dedicated plugin. 

This plugin acts as a "translator" from Freedomotic generic commands to protocol specific
commands, and vice versa.

Read data from hardware
-----------------------

Your plugin will read data from the protocol and translate it into
Freedomotic events to be notified to the framework. The event you want
to notify is usually a
`ProtocolRead </javadoc/it/freedomotic/events/ProtocolRead.html>`__
event; take a look at `how to listen and notify
events </content/make-your-plugin-send-and-listen-events>`__ tutorial.

To bind an event to a change of the status of a specific object on the
Freedomotic environment map, you have two ALTERNATIVE choices:

Specify the new object state in the notified event
--------------------------------------------------

High level communication protocols usually know if the read value is a
temperature, a binary state (true/false) or another kind of value.

In this case you can specify the object state directly in the notified
event as following:

.. code:: java

    //protocol name= zwave, address=3
    ProtocolRead event = new ProtocolRead(this, "zwave", "3");
    //set the object state to powered=true
    event.getPayload().addStatement("behavior.name", "powered");
    event.getPayload.addStatement("behaviorValue", "true");
    // (OPTIONAL) specify a Freedomotic object class and name for the autodiscovery feature
    event.getPayload.addStatement("object.class", "Light");
    event.getPayload.addStatement("object.name", "A Light");
    //send the event to Freedomotic
    notifyEvent(event);

The drawback is that your plugin is now bound to this specific object
implementation, so it will work only with objects that have a behavior
called **"powered"** which accepts true/false values (BooleanBehaviour class).

To avoid binding your plugin to a specific object implementation you can
just notify the raw read hardware value and convert it into a valid
behavior value using a hardware trigger (data source).

Create a hardware trigger to be configured as "Data source"
-----------------------------------------------------------

You would choose this option if your communication protocol doesn't know
the object type.
For example, a relay board just knows if a relay is **on** or **off** but not if the wired object is a **lamp** or a
**coffee machine**. It simply notifies a hardware read value.

The interpretation of the raw read value is done at configuration time,
because only the configurator knows what is really connected to the relay
board. This simply means that to bind an object state (eg: ``powered=true``)
to a specific trigger, then you (as developer) must provide one along
with your plugin.

For example if you are developing an Arduino based plugin you can define
a data source trigger called ``Arduino Relay Board: read value 1 in
first relay line``. The mapping between the object state and your plugin
trigger will make the object to become "powered" when the first relay
of the Arduino board is switched on.

As a note, the object settings can be changed from jfrontend by right clicking
on an object on the map (**Data source** tab), like in the image below:

.. figure:: http://freedomotic.com/sites/default/files/wilsonkong888/lt111%20screen2.jpg?1406998130
   :alt: Configure data sources

   Configure data sources

Write data to hardware
----------------------

Freedomotic can request something like this to your plugin ``turn on
relay 1 on board at 192.168.1.100``. 

Assuming the board communication protocol is HTTP based, the plugin should translate this into a proper
HTTP request to the IP the hardware board is listening.

Your plugin will receive this generic command in the ``onCommand()``
method of your plugin and here you would parse the command parameters with

.. code:: java

    c.getProperties().getIntProperty("propertyName", defaultValue)

and create the corresponding protocol specific request.

To know which variables are available to your plugin to perform its
tasks take a look at the section `Properties received by a driver
plugin <../rules/commands>`__


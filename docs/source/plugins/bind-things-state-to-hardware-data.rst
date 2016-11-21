Bind things state to hardware data
==================================

To integrate new building automation protocols (eg: zwave, zigbee, knx,
...) you can create a dedicated plugin. This plugin acts as a
"translator" from Freedomotic generic commands to protocol specific
commands and viceversa.

Read data from hardware
-----------------------

Your plugin will read data from the protocol and translate them into
Freedomotic events to be notified to the framework. The event you want
to notify usually is a
`ProtocolRead </javadoc/it/freedomotic/events/ProtocolRead.html>`__
event; take a look at `how to listen and notify
events </content/make-your-plugin-send-and-listen-events>`__ tutorial.

To bind an event to a change of the status of a specific object on the
Freedomotic environment map you have two ALTERNATIVE choices:

Specify the target object new state in the notified event
---------------------------------------------------------

High level communication protocols usually know if the readed value is a
temperature, a binary state (true/false) or another kind of value. In
this case you can specify the object state directly in the notified
event, this way:

.. code:: java

    //protocol name= zwave, id= 3
    ProtocolRead event = new ProtocolRead(this, "zwave", "3");
    //set the object state to powered=true
    event.addProperty("behavior.name", "powered");
    event.addProperty("behaviorValue", "true");
    // (OPTIONAL) specify a freedomotic object type and name for the autodiscovery feature
    event.addProperty("object.class", "Light");
    event.addProperty("object.name", "A Light");
    //send the event to freedomotic
    notifyEvent(event);

The drawback is that you plugin is now bound to this specific object
implementation, so it will work only with objects that have a behavior
called "powered" which accepts true/false values (BooleanBehavior). To
avoid bounding you plugin to a specific object implementation you can
just notify the raw readed hardware value and convert it into a valid
behavior value using an hardware trigger (data source); see option 2 to
know more about this.

Create a hardware trigger to be configured as the "Data sources" in the object settings
---------------------------------------------------------------------------------------

You would choice this option if your communication protocol doesen't
have the knowledge of object types. For example a relay board knows just
if a relay is on or off but not if the wired object is a lamp of a
coffee machine, it simply notifies an hardware read value.

The interpretation of the raw read value is done at configuration time,
because the configurator knows what is really connected to the relay
board. This simply means to to bound an object state (eg: powered=true)
to a specific trigger that YOU (as developer) have to proivide along
with your plugin.

For example if you are developing an Arduino based plugin you can define
a data source trigger called **Arduino Relay Board: readed value 1 in
first relay line**. The mapping between the object state and your plugin
trigger, will make the object to become "powered" when the first relay
of the Arduino board is on.

As a note the object settings can be changed from frontend with a right
click on an object on the map (Data source tab), like in the image
below:

.. figure:: http://freedomotic.com/sites/default/files/wilsonkong888/lt111%20screen2.jpg?1406998130
   :alt: Configure data sources

   Configure data sources

Write data to hardware
----------------------

Freedomotic can request something like this to your plugin **"turn on
relay 1 on board at 192.168.1.100"**. Assuming the board communication
protocol is HTTP based, the plugin should translate this into a proper
HTTP request to the IP the hardware board is listening.

Your plugin will receive this generic command in the **onCommand()**
method of your plugin, here you should parse the command parameters with

.. code:: java

    c.getProperties().getIntProperty("propertyName", defaultValue)

and create the corresponding protocol specific request.

To know which variables are available to you plugin to perform it's
tasks take a look at the section `Properties received by a driver
plugin <https://github.com/freedomotic/freedomotic/wiki/Commands#properties-received-by-a-driver-plugin>`__

Plugin Samples
--------------

Take a look at the source code of these plugins to get inspiration

- `SNMP Communication <https://github.com/freedomotic/freedomotic/tree/master/plugins/devices/hwg-ste>`_ A plugin to interact with SNMP (Simple Network Management Protocol) enabled devices

- `FTDI Communication <https://github.com/freedomotic/freedomotic/tree/master/plugins/devices/usb4relaybrd>`_

- `UDP Communication <https://github.com/freedomotic/freedomotic/tree/master/plugins/devices/openpicus-grove-system>`_ Sending UDP packages to Open Picus devices

- `I2C Communication <https://github.com/freedomotic/freedomotic/tree/master/plugins/devices/i2c>`_

- `MQTT Communication <https://github.com/freedomotic/freedomotic/tree/master/plugins/devices/mqtt-client>`_ A MQTT client

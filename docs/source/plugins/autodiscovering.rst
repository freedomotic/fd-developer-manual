Autodiscover and autoconfigure things
=====================================

This feature is used to allow hardware plugins to create things
automatically and place them on the environment map, already configured,
like an **autodiscovering** system. The things will be created and added
to the loaded environment the first time their state changes.

How to enable your plugin autodiscovering
-----------------------------------------

Suppose you have some bulbs connected to a relay board; the first time
you turn on one of them, Freedomotic generates a new light object and
adds it to the map.

In order to do this you only need to send a state change notification
for a thing, if this thing doesn't exist (checking is made on
**protocol+address** values in the event) it is generated, configured
and placed on the frontend map.

All the following examples are related to an X10 plugin and the
notification of a change from OFF to ON of the x10 device with address
A01. Remember to change the values according to your neeeds.

.. code:: java

    ProtocolRead event = new ProtocolRead(this, "X10", "A01");
    event.addProperty("x10.function", "ON"); //this is plugin related, your plugin will have other properties
    event.addProperty("object.class", "Light");
    event.addProperty("object.name", "My X10 Light");
    event.addProperty("object.protocol", "X10");
    event.addProperty("object.address", "A01");
    notifyEvent(event);

More info about these properties:

+-------------------+---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **Property**      | **Example Value**   | **Description**                                                                                                                                                   |
+===================+=====================+===================================================================================================================================================================+
| object.class      | Light               | The type of the thing that will be created. It must be a string containing a thing type as you see in the things list menu of java frontend (when you press F6)   |
+-------------------+---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| object.name       | My Light            | The name of the new thing. If already exist a numeric ID will be added at the end like My Light 1                                                                 |
+-------------------+---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| object.protocol   | ProtocolName        | (OPTIONAL) The name of the protocol used to manage this thing (eg: ZWave)                                                                                         |
+-------------------+---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| object.address    | 1234                | (OPTIONAL) The address string (it's protocol dependent)                                                                                                           |
+-------------------+---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**NOTE: omitting object.class and object.name properties makes the
ProtocolRead event to be discarded if no such thing exists. If the thing
exists the state change described in the event is applied.**

Auto discover and auto configure things
=======================================

This feature is used to allow hardware plugins to create things
automatically and place them on the already configured environment map just
like an **auto discovering** system. The things will be created and added
to the loaded environment the first time their state changes.

How to enable auto discovering in your plugin
---------------------------------------------

Suppose you have some bulbs connected to a relay board. The first time
you turn on one of them, Freedomotic generates a new light object and
adds it to the map.

In order to do this you only need to send a state change notification
for a thing. If the thing doesn't exist(checking is done on
**protocol+address** values in the event), it is generated, configured
and placed on the frontend map.

All the following examples are based on an X10 plugin, demonstrating the 
change in state from OFF to ON of the x10 device with address A01. Remember to change the 
values according to your needs.

.. code:: java

    ProtocolRead event = new ProtocolRead(this, "X10", "A01");
    event.addProperty("x10.function", "ON"); // this is plugin related; your plugin will have different properties
    event.addProperty("object.class", "Light");
    event.addProperty("object.name", "My X10 Light");
    event.addProperty("object.protocol", "X10");
    event.addProperty("object.address", "A01");
    notifyEvent(event);

More information about these properties:

+-------------------+---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **Property**      | **Example Value**   | **Description**                                                                                                                                                   |
+===================+=====================+===================================================================================================================================================================+
| object.class      | Light               | The type of the thing that will be created. It must be a string containing a thing type as you see in the things list menu of java frontend (when you press F6)   |
+-------------------+---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| object.name       | My Light            | The name of the new thing. If the name already exists, a numeric ID will be added at the end of the name. For example: My Light 1                                 |
+-------------------+---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| object.protocol   | ProtocolName        | (OPTIONAL) The name of the protocol used to manage this thing (eg: ZWave)                                                                                         |
+-------------------+---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| object.address    | 1234                | (OPTIONAL) The address string (it is protocol dependent)                                                                                                          |
+-------------------+---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. note:: Omitting object.class and object.name properties makes the ProtocolRead event to be discarded if no such thing exists. If thing exists the state change described in the event is applied.

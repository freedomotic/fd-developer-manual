
Events
======

Freedomotic and its plugins send events when anything relevant happens.

Any event is sent on a **messaging channel**. A channel address is a simple
string with a hierarchical structure like ``app.sensors.event.object.behavior.changed``. 

You can subscribe an event channel from a trigger which is a filter of events. 

For example if your event is ``an object changed state`` you can filter it using a trigger ``if
a light in the kitchen changed its powered state``. 

Freedomotic events have a set of standard properties plus a list of properties related to
the specific event.

Generic event properties
------------------------

The following properties are common to all events and can be intercepted
and filtered by any trigger:

+----------------+-------------------+------------------------------------------------------------------+
| PROPERTY       | POSSIBLE VALUES   | DESCRIPTION                                                      |
+================+===================+==================================================================+
| date.dayname   | eg: Sunday        | English name of the day in which the event is throwed            |
+----------------+-------------------+------------------------------------------------------------------+
| date.day       | eg: 4 for Thursday| The day number in which the event is throwed                     |
+----------------+-------------------+------------------------------------------------------------------+
| date.month     | eg: October       | The month name in which the event is throwed                     |
+----------------+-------------------+------------------------------------------------------------------+
| date.year      | eg: 2016          | The year number in which the event is throwed                    |
+----------------+-------------------+------------------------------------------------------------------+
| time.hour      | eg: 0-23          | The hour number in 24h format in which the event is throwed      |
+----------------+-------------------+------------------------------------------------------------------+
| time.minute    | eg: 0-59          | The minute of the current hour in which the event is throwed     |
+----------------+-------------------+------------------------------------------------------------------+
| time.second    | eg: 0-59          | The second of the current minute in which the event is throwed   |
+----------------+-------------------+------------------------------------------------------------------+
| sender         |                   | The name of the module that have generated the event             |
+----------------+-------------------+------------------------------------------------------------------+

Predefined events
-----------------

Here a list of predefined events:

+---------------------------------------------------------------+-----------------------------------------------+----------------------------------------------------------+
| EVENT                                                         | CHANNEL                                       | DESCRIPTION                                              |
+===============================================================+===============================================+==========================================================+
| :doc:`AccountEvent <account-event>`                           | app.event.sensor.account.change               | Account status changed                                   |
+---------------------------------------------------------------+-----------------------------------------------+----------------------------------------------------------+
| :doc:`GenericEvent <generic-event>`                           | app.event.sensor                              | Generic event. Use ONLY if there is not a specific event |
+---------------------------------------------------------------+-----------------------------------------------+----------------------------------------------------------+
| :doc:`LocationEvent <location-event>`                         | app.event.sensor.person.movement.detected     | Person position detected                                 |
+---------------------------------------------------------------+-----------------------------------------------+----------------------------------------------------------+
| :doc:`LuminosityEvent <luminosity-event>`                     | app.event.sensor.luminosity                   | Luminosity changed                                       |
+---------------------------------------------------------------+-----------------------------------------------+----------------------------------------------------------+
| :doc:`MessageEvent <message-event>`                           | app.event.sensor.messages.MESSAGE_TYPE        | Message notified                                         |
+---------------------------------------------------------------+-----------------------------------------------+----------------------------------------------------------+
| :doc:`ObjectHasChangedBehavior <object-has-changed-behavior>` | app.event.sensor.object.behavior.change       | Object behavior changed                                  |
+---------------------------------------------------------------+-----------------------------------------------+----------------------------------------------------------+
| :doc:`ObjectReceiveClick <object-receive-click>`              | app.event.sensor.object.behavior.clicked      | Object clicked                                           |
+---------------------------------------------------------------+-----------------------------------------------+----------------------------------------------------------+
| :doc:`PersonEntersZone <person-enters-zone>`                  | app.event.sensor.person.zone.enter            | Person enters a zone                                     |
+---------------------------------------------------------------+-----------------------------------------------+----------------------------------------------------------+
| :doc:`PersonExitsZone <person-exits-zone>`                    | app.event.sensor.person.zone.exit             | Person exits a zone                                      |
+---------------------------------------------------------------+-----------------------------------------------+----------------------------------------------------------+
| :doc:`PluginHasChanged <plugin-has-changed>`                  | app.event.sensor.plugin.change                | Plugin status changed                                    |
+---------------------------------------------------------------+-----------------------------------------------+----------------------------------------------------------+
| :doc:`ProtocolRead <protocol-read>`                           | app.event.sensor.protocol.read.PROTOCOL_NAME  | Protocol read                                            |
+---------------------------------------------------------------+-----------------------------------------------+----------------------------------------------------------+
| :doc:`ScheduledEvent <scheduled-event>`                       | app.event.sensor.calendar.event.schedule      | Time related event                                       |
+---------------------------------------------------------------+-----------------------------------------------+----------------------------------------------------------+
| :doc:`TemperatureEvent <temperature-event>`                   | app.event.sensor.temperature                  | Temperature changed                                      |
+---------------------------------------------------------------+-----------------------------------------------+----------------------------------------------------------+
| :doc:`ZoneHasChanged <zone-has-changed>`                      | app.event.sensor.environment.zone.change      | Zone changed                                             |
+---------------------------------------------------------------+-----------------------------------------------+----------------------------------------------------------+

More info in Javadoc
--------------------

For event specific data please refer to the Javadocs of the event
classes https://freedomotic.github.io/javadoc/freedomotic-core/com/freedomotic/events/package-summary.html

For example this is the list of properties available to a trigger that
listen to **ObjectReceiveClick** events

-  date.day.name EQUALS Thursday
-  date.day EQUALS 4
-  date.month.name EQUALS October
-  date.month EQUALS 10
-  date.year EQUALS 2012
-  time.hour EQUALS 18
-  time.minute EQUALS 15
-  time.second EQUALS 15
-  sender EQUALS UnknownSender
-  click EQUALS SINGLE\_CLICK
-  object.type EQUALS EnvObject.ElectricDevice.Light
-  object.name EQUALS Light one
-  object.protocol EQUALS X10
-  object.address EQUALS A01

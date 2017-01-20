
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

Generic event parameters
------------------------

The following parameters are common to all events and can be intercepted
and filtered by any trigger:

+----------------+-------------------+------------------------------------------------------------------+
| PARAMETER      | POSSIBLE VALUES   | DESCRIPTION                                                      |
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

+--------------------------+-----------------------------------------------+----------------------------------------------------------+
| EVENT                    | CHANNEL                                       | DESCRIPTION                                              |
+==========================+===============================================+==========================================================+
| AccountEvent             | app.event.sensor.account.change               | Account status changed                                   |
+--------------------------+-----------------------------------------------+----------------------------------------------------------+
| GenericEvent             | app.event.sensor                              | Generic event. Use ONLY if there is not a specific event |
+--------------------------+-----------------------------------------------+----------------------------------------------------------+
| LocationEvent            | app.event.sensor.person.movement.detected     | Person position detected                                 |
+--------------------------+-----------------------------------------------+----------------------------------------------------------+
| LuminosityEvent          | app.event.sensor.luminosity                   | Luminosity changed                                       |
+--------------------------+-----------------------------------------------+----------------------------------------------------------+
| MessageEvent             | app.event.sensor.messages.MESSAGE_TYPE        | Message notified                                         |
+--------------------------+-----------------------------------------------+----------------------------------------------------------+
| ObjectHasChangedBehavior | app.event.sensor.object.behavior.change       | Object behavior changed                                  |
+--------------------------+-----------------------------------------------+----------------------------------------------------------+
| ObjectReceiveClick       | app.event.sensor.object.behavior.clicked      | Object clicked                                           |
+--------------------------+-----------------------------------------------+----------------------------------------------------------+
| PersonEntersZone         | app.event.sensor.person.zone.enter            |                                                          |
+--------------------------+-----------------------------------------------+----------------------------------------------------------+
| PersonExitsZone          | app.event.sensor.person.zone.exit             |                                                          |
+--------------------------+-----------------------------------------------+----------------------------------------------------------+
| PluginHasChanged         | app.event.sensor.plugin.change                |                                                          |
+--------------------------+-----------------------------------------------+----------------------------------------------------------+
| ProtocolRead             | app.event.sensor.protocol.read.PROTOCOL_NAME  |                                                          |
+--------------------------+-----------------------------------------------+----------------------------------------------------------+
| ScheduledEvent           | app.event.sensor.calendar.event.schedule      |                                                          |
+--------------------------+-----------------------------------------------+----------------------------------------------------------+
| TemperatureEvent         | app.event.sensor.temperature                  |                                                          |
+--------------------------+-----------------------------------------------+----------------------------------------------------------+
| ZoneHasChanged           | app.event.sensor.environment.zone.change      |                                                          |
+--------------------------+-----------------------------------------------+----------------------------------------------------------+

More info in Javadoc
--------------------

For event specific data please refer to the Javadocs of the event
classes http://www.emmecilab.net/freedomotic/core/site/apidocs/com/freedomotic/events/package-summary.html

Otherwise you can take a look at the Freedomotic log (LogViewer plugin)
to see the paramentes embedded into a received event.

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

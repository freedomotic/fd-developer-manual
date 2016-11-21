What are events
===============

As you learned in the "freedomotic anatomy" section, that you have
already red for sure ;), freedomotic (and its plugins) send events when
anything relevant happens. Thake a look at `this
diagram </images/wiki/objects.png>`__

Any event is sent on a messaging channel. A channels address is a simple
string with a hierarchical structure like
app.sensors.event.object.behavior.changed. You can subscribe an event
channel from a trigger, which is a filter of events. For example if your
event is "an object changed state" you can filter it using a trigger "if
a light in the kitchen changed it's powered state". Freedomotic events
have a set of standard properties plus a list of properties related to
the specific event. Common properties are:

Generic Event Parameters
========================

The following parameters are common to all events and can be intercepted
and filtered by any trigger:

+----------------+-------------------+------------------------------------------------------------------+
| PARAMETER      | POSSIBLE VALUES   | DESCRIPTION                                                      |
+================+===================+==================================================================+
| date.dayname   | eg: Sunday        | English name of the day in which the event is throwed            |
+----------------+-------------------+------------------------------------------------------------------+
| date.day       |                   | The day number in which the event is throwed                     |
+----------------+-------------------+------------------------------------------------------------------+
| date.month     |                   | The month name in which the event is throwed                     |
+----------------+-------------------+------------------------------------------------------------------+
| date.year      |                   | The year number in which the event is throwed                    |
+----------------+-------------------+------------------------------------------------------------------+
| time.hour      |                   | The hour number in 24h format in which the event is throwed      |
+----------------+-------------------+------------------------------------------------------------------+
| time.minute    |                   | The minute of the current hour in which the event is throwed     |
+----------------+-------------------+------------------------------------------------------------------+
| time.second    |                   | The second of the current minute in which the event is throwed   |
+----------------+-------------------+------------------------------------------------------------------+
| sender         |                   | The name of the module that have generated the event             |
+----------------+-------------------+------------------------------------------------------------------+

More info in Javadocs
=====================

For event specific data please refer to the Javadocs of the event
classes
http://freedomotic.com/javadoc/it/freedomotic/events/package-summary.html
Otherwise you can take a look at the freedomotic log (LogViewer Plugin)
to see the paramentes embedded into a received event.

For example this is the list of properties available to a trigger that
listen to ObjectReceiveClick events

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
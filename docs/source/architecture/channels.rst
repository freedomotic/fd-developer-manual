Channels
========

The concept of **channel** is central to the messaging
system as events and commands are published on specific channels. 

Events are initiated by a sensor plugin. From Freedomotic's point of view a sensor
is composed of a hardware device and software connected to the
middleware that manages it.

Events can be exchanged in any of the supported formats (e.g. POJO, JSON,
XML) and are communicated to the triggers through a **publish/subscribe**
messaging channel. Each trigger must be subscribed to a
channel to receive the events traveling through it. 

Wildcard subscription
---------------------

It is possible to use wildcards for subscriptions in order to automatically
include an entire range of events. For example, if a sensor generates
events on channel ``app.events.sensors.moving.person.P003`` a trigger
can listen to this particular event to receive details about person
P003’s movements. Otherwise if the trigger listens to
``app.events.sensors.moving.person.∗`` it will receive information about
the movement of all people detected in the environment.

The wildcard semantic is as follows:

-  **period (.)** is used to separate names in a path
-  **asterisk (\*)** is used to match any name in a path
-  **greater than sign (>)** is used to recursively match any destination starting from this name

A **trigger** is a filter that can be used to decide whether a notified
event has to be processed or not. Whenever an event is processed by a
trigger, the associated reaction is executed.

A **reaction** represents a link between a trigger and one or more command list executed by an
actuator or another sensing system.

It allows to control the processing flow of the commands,
capturing the resulting values of their execution. 

Every command list
is executed in parallel within a **dedicated thread** (therefore several
reactions can occur in parallel). Each thread sequentially dispatches
each command found in its own list using the **request/reply** pattern.

Notice that trigger and commands are reusable components, as they are
not defined inside the reaction (which only specify the structured the
execution flow).

A command identifies an outgoing message from the middleware containing
the definition of the receiver specified as a Channel address (eg:
``app.plugins.protocols.modbus.in``). 

A command also contains all parameters needed to perform its execution. 
Commands are forwarded using
request/reply messaging pattern while events use send-and-forget
pattern. An actuator is the endpoint of the communication process.

An actuator can physically execute the command in the environment (e.g.
turn a light on or open a window). It is also possible to query an
actuator as if it were a sensor but only within its domain of control,
for example it can reply to a query notifying the state of a light under
its control. 

Trigger, reactions and commands are defined in XML files
that compose the model of all the world entities Freedomotic interacts
with. 

Such files are automatically loaded and saved to file by the
middleware, ensuring data consistency.

Channel Examples
----------------

+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------+
| **TRIGGER SUBSCRIPTION** (to a Channel)                                                                                                                                              | **DESCRIPTION**                                                                                        |
+======================================================================================================================================================================================+========================================================================================================+
| app.events.sensors.moving.person.3                                                                                                                                                   | a trigger subscribing to this channel can listen to all movements event related to the person with ID=3|
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------+
| app.events.sensors.moving.person.\*                                                                                                                                                  | a trigger subscribing to this channel can listen to all kind of events related to person with ID=3|
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------+
| app.events.sensors.>                                                                                                                                                                 | a trigger subscribing to this channel can listen to all events notified to Freedomotic by sensors      |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------+


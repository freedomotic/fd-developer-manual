
Triggers
========

A **trigger** is a filter that permits to intercept an **event** on a **channel**.

It performs a check on the event carrying values and tags assigning a meaningful and reusable name to this restriction.

For example, an event can be the notification that it is 10 o'Clock; a
trigger can listen to time events and if the hour is between 7 and 13 o'Clock you can name this trigger
``it's morning`` and reuse it to perform **reactions** like ``IF it's morning THEN turn off outdoor
lights``.

Therefore, a trigger can be used to decide whether a notified event has
to be processed or not. Whenever an event is processed by a trigger, if
the trigger is consistent with its definition, the associated commands
are executed. 

A **reaction** represents a link between a trigger and one or
more commands listed, executed by an actuator.

In this brief tutorial, the manual creation of an XML
trigger is explained. However, the end user can define triggers using the included
graphical editor, so there is no need to edit the XML manually. 

How to filter events using triggers
-----------------------------------

Events can be intercepted using triggers. Each event has a default
channel on which it is notified. 

To know which is the default channel of a particular event see listenable events page (TODO ADD A LINK).
To capture the event you
just create a trigger that is listening to the same channel. For example
the **PersonMoving** event is published on the channel ``app.event.sensor.person.movement.moving``.

To intercept a person's movemen,t you can define a trigger listening to
channel ``app.event.sensor.person.movement.moving``.


How to filter received event parameters
---------------------------------------

As said before a trigger is a event filter. It can read event parameters
and filter they according to the rules defined in it. Every
rule is called **statement**. A statement is composed by a **logical** value, an
**attribute** name, an **operand** and a **value**.

The action tag can be used to listen on the default channel of a specif
event. You have to insert the complete path of the Java class that
implements the event. Otherwise you can specify the channel with a
custom string inside the ``<channel> </channel>`` tag.

-  **logical**: is used to concatenate a statement with the previous.
   The default value is *AND* meaning the following rule is in logical AND
   with the previous. At this time only the AND logical value is
   implemented.
-  **attribute**: it's the name of the event property to filter. The know
   which properties are carried by an event you have to refer to the
   event page.
-  **operand**: can be *EQUALS*, *REGEX*, *GREATER\_THAN*, *LESS\_THAN*,
   *GREATER\_EQUAL\_THAN*, *LESS\_EQUAL\_THAN*, *BETWEEN\_TIME*. It's used
   to relate the attribute with the value.
-  **value**: can be a string or an integer value. You can use the *ANY*
   key to match any value.



Max execution limit and flood control
-------------------------------------

Every trigger has a **max-executions** parameter which defines how many
times this trigger can fire. This counter is reset at Freedomotic startup. 
If the value is **-1** this trigger has no max executions limit.

Another property if **suspension-time** which defines for how many
milliseconds this trigger is disabled after firing. The trigger cannot
fire again until its suspension time is finished. Every trigger has a
standard suspension time of 100ms which can be overwritten if needed
setting a lower value.

Listening to Channels with wildcard paths
-----------------------------------------

This feature is applicable only if you insert the custom channel path as
a string in the ``[code] [/code]`` tag.

For example if a sensor generates events on channel
``app.events.sensors.moving.person.P003`` a trigger can listen to this
particular event to receive details about person P003’s movements.

Otherwise if the trigger listens to
``app.events.sensors.moving.person.∗`` it will receive information about
the movement of all people detected in the environment.

The wildcard semantic is as follows:

-  **period (.)** is used to separate names in a path
-  **asterisk (\*)** is used to match any name in a path
-  **greater than sign (>)** is used to recursively match any destination starting from this name

Trigger scripting
-----------------

TODO ADD EXAMPLES TAKEN FROM

https://github.com/freedomotic/freedomotic/blob/master/framework/freedomotic-core/src/test/java/com/freedomotic/core/ResolverTest.java

Deploy a trigger
----------------

Triggers are deployed in the *FREEDOMOTIC\_ROOT/data/trg* folder. They
are files with **.xtrg** extension and are loaded at Freedomotic startup.

In the console you can have a view of the loaded triggers and the channel on which they are listening.

Examples
--------

Check if a thing name in the event is 'Kitchen Light'
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: xml

    <statement>
      <logical>AND</logical>
      <attribute>object.name</attribute>
      <operand>EQUALS</operand>
      <value>Kitchen Light</value>
    </statement>

Check if the thing type in the event is an electric device
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: xml

    <statement>
      <logical>AND</logical>
      <attribute>object.type</attribute>
      <operand>REGEX</operand>
      <value>^EnvObject.ElectricDevice\.(.*)</value>
    </statement>

Check if the temperature in the event is strictly greater than 20°C
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: xml

    <statement>
      <logical>AND</logical>
      <attribute>@event.temperature</attribute>
      <operand>GREATER_THAN</operand>
      <value>20</value>
    </statement>

Check if the given time (format: HH:mm:ss) is between the specified time interval (format: HH:mm:ss-HH:mm:ss)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: xml

    <statement>
      <logical>AND</logical>
      <attribute>time.current</attribute>
      <operand>TIME_BETWEEN</operand>
      <value>23:00:00-8:30:00</value>
    </statement>


Check is someone exits from kitchen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: xml

    <trigger>
        <name>Someone Exits from Kitchen</name>
        <description>When someone exits from kitchen area</description>
        <channel>app.event.person.zone</channel>
        <payload>
            <payload>
                <statement>
                    <logical>AND</logical>
                    <attribute>zone</attribute>
                    <operand>EQUAL</operand>
                    <value>Kitchen</value>
                </statement>
                <statement>
                    <logical>AND</logical>
                    <attribute>person</attribute>
                    <operand>EQUAL</operand>
                    <value>ANY</value>
                </statement>
                <statement>
                    <logical>AND</logical>
                    <attribute>action</attribute>
                    <operand>EQUAL</operand>
                    <value>exit</value>
                </statement>
            </payload>
        </payload>
        <delay>0</delay>
    </trigger>

This trigger can filter **PersonExitZone** events. In that case the trigger
fires only if the event is related to the kitchen zone and the person **ID**
can be **ANY** (valid for **ANY** person). If the trigger is consistent with the
event one or more commands will be executed. 

A thing of type Electric Device is clicked
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: xml

    <trigger>
      <name>When an electric device is clicked</name>
      <description>When an electric device is clicked</description>
      <channel>app.event.sensor.object.behavior.clicked</channel>
      <payload>
        <payload>
          <statement>
            <logical>AND</logical>
            <attribute>object.type</attribute>
            <operand>REGEX</operand>
            <value>^EnvObject.ElectricDevice\.(.*)</value>
          </statement>
          <statement>
            <logical>AND</logical>
            <attribute>click</attribute>
            <operand>EQUALS</operand>
            <value>SINGLE_CLICK</value>
          </statement>
        </payload>
      </payload>
      <persistence>true</persistence>
    </trigger>

This trigger will listen (and filter) events of types **ObjectReceiveClick**
because they are sent on channel ``app.event.sensor.object.behavior.clicked`` 

These are the received parameters that can be used in the trigger above

-  date.day.name EQUALS Thursday
-  date.day EQUALS 4
-  date.month.name EQUALS October
-  date.month EQUALS 10
-  date.year EQUALS 2012
-  time.hour EQUALS 18
-  time.minute EQUALS 15
-  time.second EQUALS 15
-  object.type EQUALS EnvObject.ElectricDevice.Light
-  object.name EQUALS Light one
-  object.protocol EQUALS X10
-  object.address EQUALS A01
-  sender EQUALS JavaFrontend
-  click EQUALS SINGLE\_CLICK

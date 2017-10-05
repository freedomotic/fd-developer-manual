Freedomotic Messaging System
============================

Freedomotic uses simple structured messages (xml, json) to communicate
with its components. This is done through a **Messaging Middleware** (**Apache
ActiveMQ**).

Freedomotic is based entirely on events and any change in the
environment and any user interaction (eg: a click on the GUI) generates
events. 

**Events** are published on **channels** and can be intercepted by
**triggers**. 

Each trigger may be associated with one or more commands
defining a **reaction** or **automation**.

Utilizing this architecture, program behavior is not
predetermined but is fully modificable at runtime, making it extremely
flexible and adaptable to any possible use in building automation.

When a sensor communicates a change in the environment it sends out an event.

A trigger, which is a sort of an event filter, listens to the event
subscribed to the channel on which this event is sent. If the event is
consistent with the trigger one or more commands will be executed. The
command is automatically sent to the actuator which is able to execute
it.

-  A sensor can be an hardware device like a luminosity sensor
-  An event is fired by a sensor, for example ``luminosity in the kitchen
   is 30%``
-  A trigger can define an expression like ``if luminosity is less than
   50%``
-  A command can be something like ``turn on the light in the kitchen``
-  An actuator can be relais board

The result of the interaction between event, trigger, and command is an
automated action. In this case the automation is ``if luminosity is less than
50% turn on the light in the kitchen``.

.. note:: Triggers and commands are defined by the user using the **Jfrontend** graphical **EventEditor**. Triggers, commands and automations are saved as XML files.

A Message Journey
#################

The communication process of notification of an event to the execution
of a command consists of several steps:

**Event** (notified by plugins or
Freedomotic itself) -> **Trigger** (acting as an events filter to define
simple use cases) -> **Command** (executed by an actuator)

In this example we will analyze an automation composed by a
single command: ``IF Livingroom light turns on THEN announce its status using text to
speech``

What happens in the framework?

This is an event which describes a state change of a light which turns
from OFF (powered=false) to ON (powered=true). This kind of events is
notified on channel ``app.event.sensor.object.behavior.change`` by a sensor
plugin for example a Modbus sensor.

Events can be notified through hardware protocol plugins, frontends or
Freedomotic itself as in this case.

Here is an example event which informs all listeners that a 'thing' named **Livingroom Light** has
changed:

.. code:: xml

    <com.freedomotic.events.ObjectHasChangedBehavior>
      <eventName>ObjectHasChangedBehavior</eventName>
      <sender>Light</sender>
      <payload>
        <payload>
          <com.freedomotic.reactions.Statement>
            <logical>AND</logical>
            <attribute>date.dayname</attribute>
            <operand>EQUALS</operand>
            <value>Sunday</value>
          </com.freedomotic.reactions.Statement>
          <com.freedomotic.reactions.Statement>
            <logical>AND</logical>
            <attribute>date.day</attribute>
            <operand>EQUALS</operand>
            <value>23</value>
          </com.freedomotic.reactions.Statement>
          <com.freedomotic.reactions.Statement>
            <logical>AND</logical>
            <attribute>date.month</attribute>
            <operand>EQUALS</operand>
            <value>September</value>
          </com.freedomotic.reactions.Statement>
          <com.freedomotic.reactions.Statement>
            <logical>AND</logical>
            <attribute>date.year</attribute>
            <operand>EQUALS</operand>
            <value>2012</value>
          </com.freedomotic.reactions.Statement>
          <com.freedomotic.reactions.Statement>
            <logical>AND</logical>
            <attribute>time.hour</attribute>
            <operand>EQUALS</operand>
            <value>9</value>
          </com.freedomotic.reactions.Statement>
          <com.freedomotic.reactions.Statement>
            <logical>AND</logical>
            <attribute>time.minute</attribute>
            <operand>EQUALS</operand>
            <value>45</value>
          </com.freedomotic.reactions.Statement>
          <com.freedomotic.reactions.Statement>
            <logical>AND</logical>
            <attribute>time.second</attribute>
            <operand>EQUALS</operand>
            <value>49</value>
          </com.freedomotic.reactions.Statement>
          <com.freedomotic.reactions.Statement>
            <logical>AND</logical>
            <attribute>sender</attribute>
            <operand>EQUALS</operand>
            <value>Light</value>
          </com.freedomotic.reactions.Statement>
          <com.freedomotic.reactions.Statement>
            <logical>AND</logical>
            <attribute>object.name</attribute>
            <operand>EQUALS</operand>
            <value>Livingroom Light</value>
          </it.freedomotic.reactions.Statement>
          <com.freedomotic.reactions.Statement>
            <logical>AND</logical>
            <attribute>powered</attribute>
            <operand>EQUALS</operand>
            <value>true</value>
          </com.freedomotic.reactions.Statement>
          <com.freedomotic.reactions.Statement>
            <logical>AND</logical>
            <attribute>brightness</attribute>
            <operand>EQUALS</operand>
            <value>0</value>
          </com.freedomotic.reactions.Statement>
          <com.freedomotic.reactions.Statement>
            <logical>AND</logical>
            <attribute>object.currentRepresentation</attribute>
            <operand>EQUALS</operand>
            <value>0</value>
          </com.freedomotic.reactions.Statement>
        </payload>
      </payload>
      <isValid>true</isValid>
      <uid>116</uid>
      <executed>true</executed>
      <isExecutable>true</isExecutable>
      <creation>1348386349837</creation>
      <priority>0</priority>
    </com.freedomotic.events.ObjectHasChangedBehavior>

You can define triggers to narrow any event just by listening on the
event channel and setting a list of conditions (the statements) that
must be met in order to consider this trigger as fired. The trigger can
then be used as the "**WHEN/IF**" part of an automation (aka **scenario**).

Freedomotic starts with a set of predefined triggers which cover most
use cases. At any time you can add new use cases using an existing
trigger as a template.

.. code:: xml

    <trigger>
      <name>Livingroom Light turns ON or OFF</name>
      <channel>app.event.sensor.object.behavior.change</channel>
      <payload>
        <payload>
          <statement>
            <logical>AND</logical>
            <attribute>object.name</attribute>
            <!-- allowed operand are EQUALS, REGEX, GREATER_THEN, GREATER_EQUAL_THEN, LESS_THEN, LESS_EQUAL_THEN -->
            <operand>EQUALS</operand>
            <value>Livingroom Light</value>
          </statement>
          <statement>
            <logical>AND</logical>
            <attribute>powered</attribute>
            <operand>EQUALS</operand>
            <!-- here you can write true to select only 'turns on' cases -->
            <!-- here you can write false to select only 'turns off' cases -->
            <!-- ANY is used to match any case -->
            <value>ANY</value>
          </statement>
        </payload>
      </payload>
    </trigger>

In an automation you bind a trigger to one or more commands. In this case
the automation is ``WHEN Livingroom Light turns on THEN Say electric device status``.

The command ``Say electric device status`` is shipped with the text to
speech plugin (http://freedomotic.com/content/plugins/text-speech) and
looks like this:

.. code:: xml

    <command>
      <name>Say electric device status</name>
      <description>say electric device status</description>
      <receiver>app.actuators.media.tts.in</receiver>
      <properties>
        <properties>
          <property name="say" value="= 
            if (@current.object.powered) 
                    say="@current.object.name is on with brightness at @current.object.brightness"; 
            else 
                    say="@current.object.name is off";
              "/>
        </properties>
        <tuples/>
      </properties>
    </command>

When a trigger is fired Freedomotic loads all related commands and
evaluates them using runtime properties. So the command above will look
like this when received by the **TTS Text to Speech** plugin.

Every plugin has access to time and date information, the set of
properties defined in the event and the current object state if the
event has something to do with environment objects (in this case a
light).

Your plugin can use all this information for token substitution
and scripting as for the 'say' property in the command above. In the
command below you can see how the 'say' property is evaluated by
Freedomotic before sending it to the text to speech plugin:

.. code:: xml

    <command>
      <name>Say electric device status [EVALUATED]</name>
      <description>say electric device status</description>
      <receiver>app.actuators.media.tts.in</receiver>
      <properties>
        <properties>  
          <!-- Static properties for the text to speech actuator. -->
          <!-- This are defined in data/cmd folder of the actuator itself -->
          <!-- The 'say' property is evaluated using runtime properties -->
          <property name="say" value="Livingroom Light is off."/>
          <!-- ALL PROPERTIES BELOW ARE EVALUATED AT RUNTIME -->    
          <!-- generic data taken from the event which started the event-trigger-command chain. -->
          <property name="event.sender" value="Light"/>
          <property name="event.date.dayname" value="Sunday"/>
          <property name="event.date.day" value="23"/>
          <property name="event.date.month" value="September"/>
          <property name="event.date.year" value="2012"/>
          <property name="event.time.hour" value="10"/>
          <property name="event.time.minute" value="30"/>
          <property name="event.time.second" value="24"/>
          <!-- the state of the object Livingroon Light when the event was fired -->
          <property name="event.object.name" value="Livingroom Light"/>
          <property name="event.brightness" value="0"/>
          <property name="event.powered" value="false"/>
          <property name="event.object.currentRepresentation" value="0"/>
          <!-- the current state of the object Livingroom Light (when this command is executed -->
          <property name="current.object.name" value="Livingroom Light"/>
          <property name="current.object.type" value="EnvObject.ElectricDevice.Light"/>
          <property name="current.object.protocol" value="unknown"/>
          <property name="current.object.address" value="unknown"/>
          <property name="current.object.brightness" value="0"/>
          <property name="current.object.powered" value="false"/>
        </properties>
      </properties>
    </command>

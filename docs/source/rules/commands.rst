
Commands
========

When you create a new command, you can choose two different ways. The
first is the creation of an xml file deployed in the
*FREEDOMOTIC\_ROOT/data/cmd* folder. The second option is to use the
**EventEditor** plugin. 

The first choice is the best for developers because it guarantees full control of the values because the **EventEditor**
is still under development.

A Freedomotic command is a container of customizable parameters in the
form of ``parameter = value``. 

Standard parameters are **name**, **description**
and **receiver** to guarantee the correct routing of the message to the
actuator that can execute the task.

Command xml files are messages used to instruct the actuators on
which action must be performed.

Some commands are created at runtime,
the same way the sensors creates events however commands can be created
at "design" time by the developer to have this command embedded in the
plugin (in *PLUGIN\_NAME/data/cmd* folder) or they can be created at
runtime by the user/configurator (and it will be saved in
*FREEDOMOTIC\_ROOT/data/cmd* folder).

Commands fields
---------------

Properties received by a driver plugin
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  @owner.\*: all thing properties and behaviors with the value they
   had before automation execution. If an automation rises the light
   brightess values, the property ``@owner.object.behavior.brightness``
   contains the starting value not the target value. **This is
   received only by driver plugins.**
-  +Any plugin specific property, defined in the xml command into *data/cmd*
   folder of the plugin itself.

An example (turn on X10 device)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

These are the properties received by a driver plugin which implements
communicaton with X10 hardware.

-  owner.object.protocol=X10
-  owner.object.address=A01
-  owner.object.name=Light one
-  owner.object.type=EnvObject.ElectricDevice.Light
-  owner.object.behavior.brightness=100
-  owner.object.behavior.powered=false
-  x10.gateway=PMIX35
-  x10.function=ON

Properties received by a service plugin
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  @event.\*: contains all events properties
-  @current.\*: contains the properties of the event after the
   evaluation of the previous commands of this automation. If an
   automation rises the light brightess value, the property
   @current.object.behavior.brightness **contains the target value not
   the starting value**.
-  +Any plugin specific property, defined in the xml command in data/cmd
   folder of the plugin itself.

An example (say ElectricDevice current state)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

These are the command properties received by a text to speech plugin
when the automation ``IF a light turns on THEN say ElectricDevice current state`` is performed.

-  event.sender=Light
-  event.date.day=4
-  event.date.day.name=Thursday
-  event.date.month=10
-  event.date.month.name=October
-  event.date.year=2012
-  event.time.hour=18
-  event.time.minute=15
-  event.time.second=15
-  event.object.type=EnvObject.ElectricDevice.Light
-  event.object.currentRepresentation=1
-  event.object.name=Light one
-  event.object.protocol=X10
-  event.object.address=A01
-  event.object.behavior.powered=true
-  event.object.behavior.brightness=100
-  current.object.name=Light one
-  current.object.type=EnvObject.ElectricDevice.Light
-  current.object.protocol=X10
-  current.object.address=A01
-  current.object.behavior.powered=true
-  current.object.behavior.brightness=100
-  say=Light one is on with brightness at 100%.

The structure of a command
--------------------------

Field| Description -------|------------------- name \| A short string
that identifies the effect of the command execution (eg: turn on light
in the kitcken) description \| An extended description of the effect of
the command execution. Write it in form "IF an event occurs THEN the
system ... yourDescription receiver \| The Channel on which the target
plugin is listening to delay \| Not Yet Implemented, let this parameter
to 0 timeout \| waiting time for the plugin reply, if 0 it's set to 10
seconds by default properties \| A set of properties in form "key =
value". 

Commands for the BehaviorManager
--------------------------------

These commands can be used to change objects state in ``IF this THEN that`` automations like
``IF it's dark THEN turn on garden lights``. 

Here some example of commands
sent to the Freedomotic internal **BehaviorsManager**. It accepts a
predefined set of properties keys but any plugin can have its own set.

Command examples
----------------

Turn on object named "livingroom light"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: xml

    <command>
      <name>Turn on livingroom light</name>
      <receiver>app.events.sensors.behavior.request.objects</receiver>
      <description>turns on an object called livingroom light</description>
      <editable>true</editable>
      <properties>
        <properties>
          <property name="behavior" value="powered"/>
          <property name="value" value="true"/>
          <property name="object.name" value="Livingroom light"/>
        </properties>
        <tuples/>
      </properties>
    </command>

Switch power of all Light type things in all environments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: xml

    <command>
      <name>switch power for all lights</name>
      <receiver>app.events.sensors.behavior.request.objects</receiver>
      <description>switch power for all lights</description>
      <editable>true</editable>
      <properties>
        <properties>
          <property name="behavior" value="powered"/>
          <property name="value" value="opposite"/>
          <property name="object.class" value="EnvObject.ElectricDevice.Light"/>
        </properties>
        <tuples/>
      </properties>
    </command>

Switch power of all Light type objects in room named 'Kitchen'
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: xml

    <command>
      <name>switch power for all kitchen lights</name>
      <receiver>app.events.sensors.behavior.request.objects</receiver>
      <description>switch power for all kitchen lights</description>
      <editable>true</editable>
      <properties>
        <properties>
          <property name="behavior" value="powered"/>
          <property name="value" value="opposite"/>
          <property name="object.class" value="EnvObject.ElectricDevice.Light"/>
          <property name="object.zone" value="Kitchen"/>
        </properties>
        <tuples/>
      </properties>
    </command>

Increase brightness (one step) of all Light type things in the environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: xml

    <command>
      <name>Increase lights brightness</name>
      <receiver>app.events.sensors.behavior.request.objects</receiver>
      <description>increases light brightness</description>
      <editable>true</editable>
      <properties>
        <properties>
          <property name="behavior" value="brightness"/>
          <property name="value" value="next"/>
          <property name="object.class" value="EnvObject.ElectricDevice.Light"/>
        </properties>
        <tuples/>
      </properties>
    </command>

Decrease brightness (one step) of all Light type things in the environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: xml

    <command>
      <name>Decrease lights brightness</name>
      <receiver>app.events.sensors.behavior.request.objects</receiver>
      <description>decreases lights brightness</description>
      <editable>true</editable>
      <properties>
        <properties>
          <property name="behavior" value="brightness"/>
          <property name="value" value="previous"/>
          <property name="object.class" value="EnvObject.ElectricDevice.Light"/>
        </properties>
        <tuples/>
      </properties>
    </command>

Command Scripting
-----------------

Commands parameters can be scripted using javascript syntax like this:

.. code:: xml

    <command>
      <name>Say the current temperature converted in fahrenheit</name>
      <receiver>app.actuators.media.tts.in</receiver>
      <delay>0</delay>
      <timeout>2000</timeout>
      <description>say the current temperature using TTS engine</description>
      <hardwareLevel>false</hardwareLevel>
      <persistence>true</persistence>
      <executed>false</executed>
      <properties>
        <properties>
          <property name="say" value="= say="The current temperature in @event.zone is " + Math.round(((@event.temperature+40)*1.8)-40) + " fahrenheit degrees. In celsius is @event.temperature degrees"/>
        </properties>
        <tuples/>
      </properties>
    </command>

This command uses text to speech to say the current temperature in a
zone and makes a on the fly conversion fron celsius to fahrenheit
degrees. The property key is a variable in the scripting context that
can be evaluated. 

To make a value scriptable it must start with an "**=**"
just like Excel. Values that not start with "**=**" are the same as the
previous Freedomotic versions.

Here other example of scripting:

.. code:: xml

   //sum the first 10 integer and store the value in myVar property
   <property name="myVar" value="= myVar=0; for (i=0; i<10; i++) myVar+=i;"/>
   
.. code:: xml
  
   //if one is one myVar property is one
   <property name="myVar" value="= if (1==1)  myVar=1; else myVar="AREYOUJOKING?";"/>
 
.. code:: xml

   negate the powered value of a thing if true becomes false, if false become true
   <property name="myVar" value="= myVar=!@event.object.powered;"/>

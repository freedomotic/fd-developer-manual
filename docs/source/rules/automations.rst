
Reactions (aka Automations)
===========================

Reactions are based on the concept of
`Channel <https://github.com/freedomotic/freedomotic/wiki/The-Channels-Concept>`__,
so be sure to have understood this concept before you continue reading.

A **reaction** consists of a trigger and one or more commands. The listed commands
are executed sequentially. The reactions run in parallel
within a dedicated thread for each of them. The triggers and the
commands are defined in files independent from the same reaction which
represents only a link. So it is possible to reuse commands and triggers
in different reactions.

Example:

-  Reaction Name: entertainment scenario
-  Trigger: TV turns ON
-  Command Sequence 1: Turn OFF Livingroom lights
-  Command Sequence 2: Close Windows -> Close Blinds

When it is Monday evening, and the TV turns ON the lights in the livingroom
are switched off. At the same time the windows start to close, and when all
the windows are completely closed, the system begins the lowering the
blinds.

XML Representation Reaction are deployed in
**FREEDOMOTIC\_ROOT/data/rea** folder. This is the XML describing the
previous scenary.

.. code:: xml

    <reaction>
      <trigger>TV turns ON</trigger>
      <sequences>
        <sequence>
          <command>Turn OFF Livingroom lights</command>
        </sequence>
        <sequence>
          <command>Close Livingroom windows</command>
          <command>Close Livingroom blinds</command>
        </sequence>
      </sequences>
    </reaction>


Composing triggers in automations (extra-conditions)
----------------------------------------------------

This means is possible to create automations like ``IF [it's morning] AND [livingroom light is on] THEN [do something]``.

The **extra conditions** feature is represented by ``AND [livingroom
light is on]`` part which allows you to lookup for the current value of any
object on the map to create additional conditions which are evaluated
before your automation is executed. 

There is still no frontend support for this feature, you should define it in XML editing the XML file in
the **data/rea** folder (is the folder which contains the
**automations**, AKA **reactions**).

Here an example ``WHEN a door is clicked AND livingroom light is on OR
kitchen light is on THEN switch the open state of the clicked door``

.. code:: xml

        <reaction>
          <trigger>When a door is clicked</trigger>
          <conditions>
            <condition>
                <target>Livingroom Light</target>
                <statement>
                    <logical>AND</logical>
                    <attribute>powered</attribute>
                    <operand>EQUALS</operand>
                    <value>true</value>
                </statement>
            </condition>
            <condition>
                <target>Kitchen Light</target>
                <statement>
                    <logical>OR</logical>
                    <attribute>powered</attribute>
                    <operand>EQUALS</operand>
                    <value>true</value>
                </statement>
            </condition>
          </conditions>
          <sequence>
            <command>Switch its open state</command>
            <command>test</command>
          </sequence>
        </reaction>

TODO ADD NEW SYNTAX EXAMPLE FOR EXTRA CONDITIONS

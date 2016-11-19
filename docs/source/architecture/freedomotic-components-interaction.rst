Architecture Components
=======================

Freedomotic is primarily a programming framework for automation and
control of environments whose purpose is to reduce drastically
time-to-market and effort necessary for the development of automation
applications. This is possible by noting that every automation system
will require common functionalities regardless of the scope (home,
business, industrial, hotel, telemedicine, etc. ...). Freedomotic is
composed by a core (the framework) plus some plugins.

.. figure:: http://freedomotic.com/images/wiki/architecture-layers.png
   :alt: Freedomotic architecture

   Freedomotic architecture

The Framework
=============

-  Mantains an internal data structure representing the environment
   (topology, room connectrions, ...), the objects in it and their state
   (on, off, open, closed, 50%dimmed,...)
-  Makes this data available to external clients in a language
   independent way (XML, JSON, POJOs, ...), so they can use a kind of
   logic like *"turn on kitchen light"* instead of \_"send to COM1 port
   the string #\*A01AON##"\_. Is just like they can see the same
   environment map the user sees; as a developer you can forget hardware
   level stuff and related issues. From this follows you can develop in
   your favourite language and just connect to the framework and
   exchange text messages.
-  Provides a rules engine coupled with a natural language processing
   system to let the user writing automations in plain english like *"if
   outside is dark turn on livingroom light"*. You can add, update and
   delete this automations at runtime using GUIs, without the need of
   coding.

The Plugins
===========

Devices plugins
---------------

Freedomotic plugins can add more features to the framework and can be
developed and distributed as completely independent packages on our
marketplace. They usually are developed to communicate with automation
hardware like X10, KNX and so on, but also graphical frontends and "web
service readers" are Freedomotic plugins just as any other source of
info, like webcams, text to speech engines and SMS senders.

Objects plugins
---------------

You can also develop `object
plugins <https://github.com/freedomotic/freedomotic/wiki/Create-new-object-types>`__
which are pieces of software which models the behavior of objects like
lamps, doors, etc... instructing the framework on how they behave. For
example a lamp object plugin tells the framework that a lamp has a
boolean behavior called powered and a dimmed behavior which is
represented by an integer from 0 to 100. A lamp can turn on, turn off
and dimm. If dimmed becomes *0%* the lamp is *powered=false* and *if
dimmed > 0%* the lamp is *powered=true*.

.. figure:: http://freedomotic.com/images/wiki/objects.png
   :alt: Plugins Messaging

   Plugins Messaging

Plugins, Object and Automations interaction
===========================================

Here is a diagram which explains the interaction between plugins,
events+triggers+commands, and freedomotic APIs

.. figure:: http://freedomotic.com/images/wiki/object-plugin-interaction.png
   :alt: Plugins and automations

   Plugins and automations

Explanation
-----------

Final goal is to define define an automation which can turn on the
livingroom light when it's tea time (17 o'Clock).

The scheduler plugin notifies to freedomotic the current time (17:00
PM). A trigger named *it's tea time* is configured to listen for all
time based events.

The *it's tea time trigger* carries a rule inside which is
*"event.time.hour == 17 AND event.time.minute == 0"*. When the event is
received by this trigger, the rule is evaluated. If the evaluation
succeed then *the trigger fires, indicating that now it's actually the
time to take the tea*.

At this point all the corresponding automation *IF (trigger: it's tea
time) THEN (command: turn on livingroom light)* is loaded by the system,
and the command is executed forwarding the generic request *"turn on
livingroom light"* to the plugin which can transform it to a protocol
dependent command (eg: send string 'A01AON' on serial port
/dev/ttyUSB0).

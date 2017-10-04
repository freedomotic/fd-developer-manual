
Bind things state to web services
=================================

Parse data from the Webservice
##############################

Here, you will probably need to request a specific URL content using a GET request. Then you will need to parse the received result, which is probably in JSON or XML. Take a look at
http://stackoverflow.com/questions/4216455/get-page-content-from-url

All of this will be implemented in the ``onRun()`` method of your plugin.

**This method is already threaded** so there is no need to create
additional execution threads. It can be executed just one time or in a
loop with some delay between the calls. To do this, just call this method
in your plugin constructor:

.. code:: 

   setPollingWait(2000);

This way the ``onRun()`` method is executed in a dedicated thread every 2
seconds. To disable the loop execution, just set it with a negative
value:

.. code::

   setPollingWait(-1);

Send a Freedomotic event
########################

We want it to the read temperature value for London using Weather Underground's
APIs.

This event changes the temperature value of a thing on the map
configured with protocol "**weather-underground**" and address
"**london**". The temperature value is stored into a java variable
"**londonTemperature**".

1. Specify the new state of the targeted thing in the notified event
-----------------------------------------------------------

.. code:: java

    //This value should be read from weather underground APIs in a real use case
    int londonTemperature=21;

    //Change the 'London Thermometer' object value
    ProtocolRead event = new ProtocolRead(this, "weather-underground", "london");
    //specify a freedomotic object type and name for the autodiscovery feature
    event.addProperty("object.class", "Thermometer");
    event.addProperty("object.name", "London Thermometer");
    //set the 'temperature' behavior of 'LondonThermometer' object to 21
    event.addProperty("behavior.name", "temperature");
    event.addProperty("behaviorValue", londonTemperature);
    //send the event to freedomotic
    notifyEvent(event);

2. Create an event to be listened by triggers
---------------------------------------------

If you just want to notify an event which is not directly
related to the object's states, it can be done in the following way: Events are published by
plugins on messaging channels. A series of useful events is predefined
in Freedomotic but you can create your own or simply use the
**GenericEvent** class.

Every action in the real environment and every interaction with
Freedomotic (eg: a click on the GUI) is mapped to an event. Events can
be intercepted by triggers, and you can assign one or more
commands to a trigger, changing the behavior of the system at runtime.

How to notify a generic event
------------------------------

.. code:: java

    GenericEvent knowItAll = new GenericEvent(this);
    //42 is the answer to the Ultimate Question of Life, the Universe, and Everything
    //http://en.wikipedia.org/wiki/Answer_to_Life,_the_Universe,_and_Everything
    event.addProperty("ultimate.question.answer", 42);
    //set a channel on which this event should be sent
    event.setDestination("app.event.sensor.deepthought");
    //sends the event on the messaging bus
    notifyEvent(event); 

Now a trigger can listen to ``app.event.sensor.deepthought`` this way

.. code:: java

    <trigger>
        <name>You know the right answer to Life</name>
        <channel>app.event.sensor.deepthought</channel>
        <payload>
            <payload>
                <statement>
                    <logical>AND</logical>
                    <attribute>ultimate.question.answer</attribute>
                    <operand>EQUAL</operand>
                    <value>42</value>
                </statement>
            </payload>
        </payload>
    </trigger>

and then you can create automations like ``WHEN [You know the right answer to Life] THEN [send me an email]``

Besides the all purpose **GenericEvent**, some useful events are predefined
in freedomotic. Look at this list
http://freedomotic.com/javadoc/it/freedomotic/events/package-frame.html

.. note::  If your plugin's main purpose is to change the state of an object on the map (eg: set thermometer object value to the value read from Google Weather) then you should follow option 1.

More information about triggers
------------------------

A trigger can listen on an events channel and filter the event content.
If your event notifies about the outdoor temperature, then you can have a trigger
called ``Outside is cold`` which fires if ``temperature is less than 10°C``.
You should provide this trigger along with your plugin in its *data/trg*
folder. To know more about triggers definition take a look at this page
`/content/triggers </content/triggers>`__.

An example: Get weather underground temperature data
----------------------------------------------------

TODO


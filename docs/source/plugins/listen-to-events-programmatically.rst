Listen to Events programmatically
=================================

First you have to add a listener for each channel you want to listen to.
In the onStart() method of your plugin add for example:

.. code:: java

    addEventListener("app.event.sensor.object.behavior.change");
    addEventListener("app.event.sensor.environment.zone.change");
    addEventListener("app.event.sensor.plugin.change");

if you want intercept all events about changing in sensors behaviors,
zones or plugins.

Then modify the onEvent(EventTemplate event) method

.. code:: java

    protected void onEvent(EventTemplate event) {
            if (event instanceof ObjectHasChangedBehavior) {
                // here what you want todo
            } else if (event instanceof ZoneHasChanged) {
                // here what you want todo
            } 
        }

`Here <https://github.com/freedomotic/freedomotic/blob/master/plugins/devices/restapi-v3/src/main/java/com/freedomotic/plugins/devices/restapiv3/RestAPIv3.java>`__
a complete example.

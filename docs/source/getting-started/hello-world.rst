The 'Hello World' Plugin
========================

Here you'll learn how to add features starting from the **hello-world**
plugin. This plugin is made of some boilerplate java code that you can
use as base to create your own plugin.

Open the **freedomotic** maven project with your favourite IDE and
compile it if not already done. This will build a set of modules;
remember the **freedomotic-core** module is the Freedomotic runtime,
start it from your IDE to have it running.

Now open the **hello-world** module in your IDE and compile it.
It will be automatically installed into the **freedomotic-core**. 

Get familiar with Freedomotic
#############################

Here are some simple changes you can do with the plugin
"`hello-world <https://github.com/freedomotic/freedomotic/tree/master/plugins/devices/hello-world>`__".


Write a message to the GUI using a Freedomotic event
----------------------------------------------------

in ``onRun()`` method of your plugin write:

.. code:: java

        protected void onRun() {
          //get and format the current date and time
          DateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");
          Date date = new Date();
          //create a freedomotic message event
          MessageEvent message = new MessageEvent(null, "Hello world plugins says current time is " + dateFormat.format(date));
          notifyEvent(message);
        }

then build your plugin, start Freedomotic and then your plugin.
You will see a blue bubble with the current date and time at the upper left side
of the environment map.

Make your plugin send emails
----------------------------

in ``onRun()`` method of your plugin write:

.. code:: java

        protected void onRun() {
            MessageEvent message # new MessageEvent(this, "The mail text");
            message.setType("mail"); //send this message as an email
            message.setTo("destination@gmail.com"); //the destination mail address
            notifyEvent(message);
        }

.. note:: this requires the `Mailer plugin <http://freedomotic.com/content/plugins/mailer>`_ to be installed and properly configured.

Print the list of things in the environment
-------------------------------------------

in ``onRun()`` method of your plugin write:

.. code:: java

        protected void onRun() {
                StringBuilder buffer = new StringBuilder();
                for (EnvObjectLogic object : EnvObjectPersistence.getObjectList()) {
                    buffer.append(object.getPojo().getName() + "\n");
                    for (BehaviorLogic behavior : object.getBehaviors()) {
                        buffer.append("  " + behavior.getName() + ": " + behavior.getValueAsString()  + "\n");
                    }
                }

                //print the string in the Freedomotic log using INFO level
                LOG.info(buffer.toString());
        }

then build your plugin, start Freedomotic and then your plugin.
With a right click on **Log Viewer** plugin you will see the list of things.

.. note:: Change logging level to **INFO** using the combobox of log viewer plugin to filter the less important log records.

Change a thing location on the map
----------------------------------

This piece of code iterates over all loaded things and moves objects of
type Person to a random location. The ``randomLocation()`` function should
be implemented and must return an
``com.freedomotic.model.geometry.FreedomPoint`` type (remember to add
freedomotic-model.jar to your classpath)

.. code:: java

        protected void onRun() {
            for (EnvObjectLogic anObject : EnvObjectPersistence.getObjectList()) {
                if (anObject instanceof it.freedomotic.objects.impl.Person){
                    Person person = (Person)anObject;
                    FreedomPoint location = randomLocation();
                    person.getPojo().getCurrentRepresentation().setOffset(
                            (int)location.getX(), 
                            (int)location.getY()
                            );
                    person.setChanged(true);
                }
            }
        }

Change things state programmatically
------------------------------------

If you want to change the object state according to a value readed from
a web service like a weather forecast service: 
https://github.com/freedomotic/freedomotic/wiki/Bound-objects-state-to-web-services-data

If you want to change the object state according to a value readed from
an hardware device like an Arduino relay board: 
https://github.com/freedomotic/freedomotic/wiki/Bound-objects-state-to-hardware-data

Interact with users using a dialog box with multiple answers
------------------------------------------------------------

You can take full advantages of other installed modules from your
plugin. For example you can use a third party text to speech plugin to
make it say something programmatically from your plugin.

You haven't to worry about how the external plugins works, you simply send to it a
generic command. The example below uses **Jfrontend** plugin to prompt a
dialog with three choices.

.. code:: java

     public void askSomething() {
      final Command c = new Command();
      c.setName("Ask something silly to user");
      c.setReceiver("app.actuators.frontend.javadesktop.in");
      c.setProperty("question", "<html><h1>Do you like Freedomotic?</h1></html>");
      c.setProperty("options", "Yes, it's good; No, it sucks; I don't know");
      c.setReplyTimeout(10000); //10 seconds

      new Thread(new Runnable() {
        public void run() {
          Command reply = Freedomotic.sendCommand(c);
            if (reply != null) {
                String userInput = reply.getProperty("result");
                  if (userInput != null) {
                    System.out.println("The reply to the test question is " + userInput);
                     } else {
                         System.out.println("The user has not responded to the question within the given time");
                        }
                     } else {
                        System.out.println("Unreceived reply within given time (10 seconds)");
                   }
                }
               }).start();
        }

Add a GUI to the plugin
-----------------------

To add a graphical interface you must create a Jframe and link it to the
plugin in ``onStart()`` with the following code

.. code:: java

        gui = new PluginJFrame();

To open the GUI right click on the plugin icon.


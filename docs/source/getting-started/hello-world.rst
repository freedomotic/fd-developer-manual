The 'Hello World!' Plugin
=========================

Here you'll learn how to add features starting from the "hello-world"
plugin. This plugin is made of some boilerplate java code that you can
use as base to create your own plugin.

Open the *"freedomotic"* maven project with your favourite IDE and
compile it if not already done. This will build a set of modules;
remember the *"freedomotic-core"* module is the Freedomotic runtime,
start it from your IDE to have it running.

Now open the *"hello-world"* module in your IDE: when you compile it, it
will be automatically installed into the freedomotic-core runtime, so
any code change you do can be tested just restarting "freedomotic-core"
from your IDE.

Get familiar with Freedomotic
=============================

Here are some example changes you can do at the
"`hello-world <https://github.com/freedomotic/freedomotic/tree/master/plugins/devices/hello-world>`__"
plugin to get used to freedomotic.

Write a message to the GUI using a Freedomotic event
----------------------------------------------------

in onRun() method of your plugin write:

.. code:: java

        protected void onRun() {
          //get and format the current date and time
          DateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");
          Date date = new Date();
          //create a freedomotic message event
          MessageEvent message = new MessageEvent(null, "Hello world plugins says current time is " + dateFormat.format(date));
          notifyEvent(message);
        }

then build your plugin, start Freedomotic, start your plugin, you will
see a blue bubble with the current date and time in the upper left side
of the environment map.

Make your plugin send emails
----------------------------

in onRun() method of your plugin write:

.. code:: java

        protected void onRun() {
            MessageEvent message # new MessageEvent(this, "The mail text");
            message.setType("mail"); //send this message as an email
            message.setTo("destination@gmail.com"); //the destination mail address
            notifyEvent(message);
        }

**this requires the `Mailer
plugin <http://freedomotic.com/content/plugins/mailer>`__ to be
installed and properly configured.**

Print the list of objects in the environment
--------------------------------------------

in onRun() method of your plugin write:

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

then build your plugin, start Freedomotic, start your plugin, with a
right click on "Log Viewer" plugin you will see the list of objects
printed.

NOTE: change logging level to "INFO" using the combobox of log viewer
plugin to filter the less important log records.

Change an object location on the map
------------------------------------

This piece of code iterates over all loaded objects and moves objects of
type Person to a random location. The randomLocation() function should
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

Change objects state programmatically
-------------------------------------

If you want to change the object state according to a value readed from
a web service, like a weather forecast service: \*
https://github.com/freedomotic/freedomotic/wiki/Bound-objects-state-to-web-services-data

If you want to change the object state according to a value readed from
an hardware device, like an Arduino relay board: \*
https://github.com/freedomotic/freedomotic/wiki/Bound-objects-state-to-hardware-data

Interact with users using a dialog box with multiple answers
------------------------------------------------------------

You can take full advantages of other installed modules from your
plugin. For example you can use a third party text to speech plugin to
make it say something programmatically from your plugin. You haven't to
worry about how the external plugins works, you simply send to it a
generic command. The example below uses java frontend plugin to prompt a
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
plugin in onStart() with the following code

.. code:: java

        gui = new PluginJFrame();

To open the GUI right click on the plugin icon.


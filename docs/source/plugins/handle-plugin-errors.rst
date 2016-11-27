
Handle plugin errors
====================

When a plugin throws an exception, the related end user messaging can be
handled automatically, for example setting the plugin description
accordingly and stopping the plugin itself.

Here is an example of correct exception handling in ``onRun()``

.. code:: java

    @Override
      public void onStart() throws PluginStartupException {
         try {
              // This code may generate a SerialPortException if there are connection problems
              serial = new SerialHelper(PORTNAME, BAUDRATE, DATABITS, STOPBITS, PARITY, new SerialPortListener() {
                @Override
                 public void onDataAvailable(String data) {
                   LOG.info("MySensors received: " + data);
                   sendChanges(data);
                 }
              });

              serial.setChunkTerminator("\n");
          } catch (SerialPortException ex) {
              throw new PluginStartupException("Error while connecting to serial device", ex);
         }
      }

-  **onStart()** throws **PluginStartupException**
-  **onRun()** throws **PluginRuntimeException**
-  **onStop()** throws **PluginShutdownException**

After catching the exception Freedomotic will:

1. print the error message on the GUI
2. change the plugin description using the exception message
3. log the exception
4. stop the plugin if it is a PluginRuntimeException


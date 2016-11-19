How to use the serial helper
============================

This service is based on `JSSC
library <https://github.com/scream3r/java-simple-serial-connector>`__.

First of all you must create a new SerialHelper and set your `port
parameters <http://en.wikipedia.org/wiki/Serial_port>`__ in the
following order:

-  port name (*/dev/ttyUSBx* or */dev/ttyACMx* for Linux; *COMx* for
   Windows)
-  baud rate
-  data bits
-  parity bit
-  stop bits

Also you can override *onDataAvailable(String data)* method to define
how to manage read data. For example you can send received data to
another method.

.. code:: java

    SerialHelper usb = new SerialHelper("/dev/ttyUSB0", 19200, 8, 1, 0, new SerialPortListener() {
      @Override
      public void onDataAvailable(String data) {
        System.out.println("DEBUG: received: " + data);
      }
    });

By default data are read continuously. If you want to read a chunk of
data with a specific terminator char you can set it as

.. code:: java

    usb.setReadTerminator("\n"); 

or if you want to read a chunk with a fixed dimension you can set it as

.. code:: java

    usb.setChunkSize(5); 

Sending a string to the serial port is very simple

.. code:: java

    usb.write("ABCD");

Here the complete example

.. code:: java

    SerialHelper usb = new SerialHelper("/dev/ttyUSB0", 19200, 8, 1, 0, new SerialPortListener() {
      @Override
      public void onDataAvailable(String data) {
        System.out.println("DEBUG: received: " + data);
      }
    });

    usb.setReadTerminator("\n"); //receive until this terminator char
    //ALTERNATIVE: usb.setChunkSize(5); //receive messages of 5 chars each
    usb.write("ABCD");

Complete examples
-----------------

`Arduino USB
plugin <https://github.com/freedomotic/freedomotic/blob/master/plugins/devices/arduinousb/src/main/java/com/freedomotic/plugins/devices/arduinousb/ArduinoUSB.java>`__
###Source code###
`SerialHelper.java <https://github.com/freedomotic/freedomotic/blob/master/framework/freedomotic-core/src/main/java/com/freedomotic/helpers/SerialHelper.java>`__

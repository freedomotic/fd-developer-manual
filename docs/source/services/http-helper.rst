How to use HttpHelper
=====================

This helper retrieves url content (HTML,JSON,XML) as string by
performing HTTP GET requests. It can also perform xpath queries on the
retrieved content.

Let's start with an example. We want to retrieve the temperature in Rome
from an online service. First of all create a new helper

.. code:: java

      HttpHelper http = new HttpHelper();

Remember that is a best practice to reuse this object if you have to do
multiple requests

Retrieve text content
---------------------

Using ``retrieveContent(String url)`` specify the online service url to
retrieve data from and print the string on the console

.. code:: java

      String xml = http.retrieveContent("http://api.openweathermap.org/data/2.5/weather?q=Roma&mode=xml");
      System.out.println(xml);

Here the complete code included into a ``<try><catch>`` block to manage
IO exceptions

.. code:: java

    try {
      String xml = http.retrieveContent("http://api.openweathermap.org/data/2.5/weather?q=Roma&mode=xml");
      System.out.println(xml);
    } catch (IOException ex) {
      //handle exception here
    }

Perform XPath queries on a URL content
--------------------------------------

In this example we are quering an XML Rest API to retrieve the current
temperature in Rome and the related unit in which the temperature value
is expressed. Note: Instead of null values you can pass username and
password to access the service. **Note: XPath queries works on XML
content only**

.. code:: java

    try {
      List<String> values = http.queryXml(
          "http://api.openweathermap.org/data/2.5/weather?q=Roma&mode=xml", null, null, 
          "//current/temperature/@value",
          "//current/temperature/@unit");
      // Prints 'Temperature in Rome is 234 kelvin'
      System.out.println("Temperature in Rome is " + values.get(0) + " " + values.get(1));
    } catch (IOException ex) {
      //handle exception here
    }

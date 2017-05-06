
Create new thing types
======================

To develop a new object type you have to create a Java extension which
describes to the system the actions the new type of object is capable.

After that your object can be instantiated by writing a XML file that
describes the value of an instance of the object model you have
implemented in Java.

To create the **Java object model** you have to enlist the object properties
and the values it can take. This is done adding
predefined listeners (called **behaviors**) to your object.

For example a light can be turned on, turned off and dimmed. So it has a
behavior called **powered** that can be **true** or **false** and a behavior called
**brightness** that can assume integer values from 0 to 100.

A behavior is an instance of predefined classes. For example the
behavior **powered** is an instance of
`BooleanBehavior.java <https://github.com/freedomotic/freedomotic/blob/master/framework/freedomotic-model/src/main/java/com/freedomotic/model/object/BooleanBehavior.java>`__
while **brightness** instantiate
`RangedIntBehavior.java <https://github.com/freedomotic/freedomotic/blob/master/framework/freedomotic-model/src/main/java/com/freedomotic/model/object/RangedIntBehavior.java>`__.

A behavior listens to change requests of its values, parses the request
(for example a sensor notifies that a light brightness has changed) and
performs the defined operation for this situation. 

The design pattern underneat is the same as a Java listener used for a Swing button. An
example can be more clear. This is the definition of the brightness
property of a light object

.. code:: java

      //linking this property with the behavior defined in the XML
      //it takes the max, min and step values from the object definition file.
         brightness = (RangedIntBehavior) getBehavior("brightness");
          brightness.addListener(new RangedIntBehaviorListener() {
               @Override
                public void onLowerBoundValue(Config params) {
                //here you can add the code to execute if the brightness change to the
                //lower value possible. Eg: brightness equals to zero means the 
               //light must be turned off.
                 turnPowerOff(params); 
                }
               @Override
                public void onUpperBoundValue(Config params) {
                //here you can add the code to execute if the brightness change to the
                //upper value possible. Eg: brightness equals to 100 means the 
                //light must be set to on and not dimmed.
                turnPowerOn(params);
                }
               @Override
               public void onRangeValue(int rangeValue, Config params) {
               //here you can add the code to execute if the brightness changes to 
               //a value inside the min-max range. Eg: brightness equals to 45 means 
               //the object must change it's brightness value to 45.
               setBrightness(rangeValue, params);
                }
         });

the setBrightness() method will look like this

.. code:: java

        public void setBrightness(int rangeValue, Config params) {
                //executes the developer level command associated with 
                //'set brightness' action defined in the object definition file.
                //the parameter 'params' has inside the data for the correct execution of the action.
                boolean executed = execute("set brightness", params); 
                if (executed) {
                    powered.setValue(true); //if dimmed the light is on
                    brightness.setValue(rangeValue);
                    //set the light graphical representation
                    setView(1); //points to the second element in the XML views array, the light on image.
                    setChanged(true);
                }
            }

Predefined behaviors
--------------------

Here is a list of ready to use behaviors to instantiate as object
properties:

+---------------------+-------------------------------------------------------+--------------------------------------------------------------+
| Behavior Name       | Listenable values changes                             | Example of use                                               |
+=====================+=======================================================+==============================================================+
| RangedIntBehavior   | onLowerBoundValue; onUpperBoundValue; onMiddleValue   | Can be used to model a property like volume of a TV object   |
+---------------------+-------------------------------------------------------+--------------------------------------------------------------+
| BooleanBehavior     | onTrue; onFalse                                       | Can be used to model the muted behavior of a TV object       |
+---------------------+-------------------------------------------------------+--------------------------------------------------------------+

Load the object as a plugin
---------------------------

As for any plugin the code must be compiled and it's jar file must be
deployed in the *FREEDOMOTIC\_ROOT/plugins/objects/OBJECT\_NAME\_FOLDER*.
At Freedomotic startup it loads all the objects inside
*FREEDOMOTIC\_ROOT/plugins/objects/* subfolders regardless their names.
Objects don't require a XML configuration file.

Create instances of your new object type
----------------------------------------

Now Freedomotic knows how this type of object can act when it receives a specific input.
Then you have to provide one or more *.xobj* files that describe the instances
of your object type. For example you have the light definitions but you
need to add to the environment a light called 'kitchen light' and
another one called 'livingroom light'. This is done through .xobj
definition. TODO: explain how to create an xobj object

-  `Here <https://github.com/freedomotic/freedomotic/blob/master/plugins/objects/base-things/src/main/java/com/freedomotic/things/impl/Light.java>`__
   an example of Java class for a light object
-  and
   `here <https://github.com/freedomotic/freedomotic/blob/master/plugins/objects/base-things/src/main/resources/data/templates/light.xobj>`__
   an xobj instance
-  for a more challenging object take a look at `TV
   object <https://github.com/freedomotic/freedomotic/blob/master/plugins/objects/tv/src/main/java/com/freedomotic/objects/impl/TV.java>`__
-  and `its *xobj*
   instance <https://github.com/freedomotic/freedomotic/blob/master/plugins/objects/tv/src/main/resources/data/templates/Tv.xobj>`__

How to create the XML object
############################

TODO: add a general description 

Common properties section
#########################

+-------------------+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+------------+
| Field             | Values                           | Description                                                                                                                          | Required   |
+===================+==================================+======================================================================================================================================+============+
| name              | String                           | The name of the object                                                                                                               | YES        |
+-------------------+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+------------+
| description       | String                           | A brief description of your object (up to 100 char)                                                                                  | YES        |
+-------------------+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+------------+
| actAs             |                                  | NOT YET IMPLEMENTED                                                                                                                  | NO         |
+-------------------+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+------------+
| type              | EnvObject.ElectricDevice.Light   | Dot notation of the object hierarchy in Freedomotic. It is a free form string you can use to identify                                | YES        |
+-------------------+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+------------+
| protocol          | String                           | Depends on the controller protocol eg: X10, Modbus,... Refer to the controller guide. Can be changed from the frontend at runtime.   | YES        |
+-------------------+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+------------+
| phisycalAddress   | String                           | Depends on the controller protocol eg: X10, Modbus,... Refer to the controller guide. Can be changed from the frontend at runtime.   | YES        |
+-------------------+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+------------+

Behaviors section
#################

In this section, the objects behaviors are configured. Each behavior
name must match the same name that is used inside the object code. To
facilitate the objects configuration, an object developer should expose
all names that is using inside the code. The names are case sensitive.

Boolean behavior
----------------

It is used to describe a property that can have only two values: true or
false, for example the property **powered** of an electric device such
a Light.

+---------------+---------------------------+-------------------------------------------------------------+------------+
| Field         | Values                    | Description                                                 | Required   |
+===============+===========================+=============================================================+============+
| name          | eg: powered, muted, ...   | the name of the boolean behavior                            | YES        |
+---------------+---------------------------+-------------------------------------------------------------+------------+
| description   | String                    | A string to describe the behavior purpose                   | NO         |
+---------------+---------------------------+-------------------------------------------------------------+------------+
| value         | Boolean                   | The startup value of the behavior                           | YES        |
+---------------+---------------------------+-------------------------------------------------------------+------------+
| active        | Boolean                   | This behavior is valid on startup? If in doubt use "true"   | YES        |
+---------------+---------------------------+-------------------------------------------------------------+------------+
| priority      |                           | NOT YET IMPLEMENTED                                         | NO         |
+---------------+---------------------------+-------------------------------------------------------------+------------+

Ranged int behavior
-------------------

A behavior used to model a property that can assume a ranged set of
integer values for example from zero to hundred. For example the
volume property of a TV object.

+---------------+---------------------------+---------------------------------------------------------------------------+------------+
| Field         | Values                    | Description                                                               | Required   |
+===============+===========================+===========================================================================+============+
| name          | eg: powered, muted, ...   | The name of the boolean behavior                                          | YES        |
+---------------+---------------------------+---------------------------------------------------------------------------+------------+
| description   | String                    | A string to describe the behavior purpose                                 | NO         |
+---------------+---------------------------+---------------------------------------------------------------------------+------------+
| value         | Boolean                   | The startup value of the behavior                                         | YES        |
+---------------+---------------------------+---------------------------------------------------------------------------+------------+
| max           | Integer                   | The upper value that can be assumed. Eg: 100                              | YES        |
+---------------+---------------------------+---------------------------------------------------------------------------+------------+
| min           | Integer                   | The lower value that can be assumed. Eg: 0                                | YES        |
+---------------+---------------------------+---------------------------------------------------------------------------+------------+
| step          | Integer                   | The step used to go to the next or previous value from the current one.   | YES        |
+---------------+---------------------------+---------------------------------------------------------------------------+------------+
| active        | Boolean                   | This behavior is valid on startup? If in doubt use "true"                 | YES        |
+---------------+---------------------------+---------------------------------------------------------------------------+------------+
| priority      |                           | NOT YET IMPLEMENTED                                                       | NO         |
+---------------+---------------------------+---------------------------------------------------------------------------+------------+

Exclusive multivalue behavior
-----------------------------

This behavior represents an object feature that only takes values from a
predefined list. For example the input property of a TV object could
only take values like INPUT1, INPUT2, SATELLITE, etc...

+---------------+---------------------------+--------------------------------------------------------------+------------+
| Field         | Values                    | Description                                                  | Required   |
+===============+===========================+==============================================================+============+
| name          | eg: powered, muted, ...   | The name of the boolean behavior                             | YES        |
+---------------+---------------------------+--------------------------------------------------------------+------------+
| description   | String                    | A string to describe the behavior purpose                    | NO         |
+---------------+---------------------------+--------------------------------------------------------------+------------+
| active        | Boolean                   | This behavior is valid on startup? If in doubt use "true"    | YES        |
+---------------+---------------------------+--------------------------------------------------------------+------------+
| priority      |                           | NOT YET IMPLEMENTED                                          | NO         |
+---------------+---------------------------+--------------------------------------------------------------+------------+
| selected      | Integer                   | The default selected item                                    | YES        |
+---------------+---------------------------+--------------------------------------------------------------+------------+
| list          | List                      | The list of items. Each of them has the format item\_value   | YES        |
+---------------+---------------------------+--------------------------------------------------------------+------------+

Views section
-------------

Each view corresponds to a visual representation of the object that could
be shown using the object code. The position of the view on the list
correspond to the same number that is used in the code.

+-----------------------+-----------+--------------------------------------------------------------------------------+
| Field                 | Values    | Description                                                                    |
+=======================+===========+================================================================================+
| tangible              | Boolean   | The object is a physical object or not                                         |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| intersecable          | Boolean   | A person shape can intersecate this object                                     |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| width                 | Integer   | the with of the object                                                         |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| height                | Integer   | the height of the object                                                       |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| x                     | Integer   | it's x position starting from 0,0 (the upper left corner) of the environment   |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| y                     | Integer   | it's y position starting from 0,0 (the upper left corner) of the environment   |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| rotation              | Integer   | the rotation using the upper left corner of the object as pivot point          |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| fillcolor / red       | Integer   | the color that fills the geometrical shape of the object                       |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| fillcolor / green     | Integer   | the color that fills the geometrical shape of the object                       |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| fillcolor / blue      | Integer   | the color that fills the geometrical shape of the object                       |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| fillcolor / alpha     | Integer   | the color that fills the geometrical shape of the object                       |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| textColor / red       | Integer   | the color of the text that describe the object                                 |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| textColor / green     | Integer   | the color of the text that describe the object                                 |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| textColor / blue      | Integer   | the color of the text that describe the object                                 |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| textColor / alpha     | Integer   | the color of the text that describe the object                                 |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| borderColor / red     | Integer   | the color of the shape border                                                  |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| borderColor / green   | Integer   | the color of the shape border                                                  |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| borderColor / blue    | Integer   | the color of the shape border                                                  |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| borderColor / alpha   | Integer   | the color of the shape border                                                  |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| shape/npoints         | Integer   | number of points use to describe the shape                                     |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| shape/xpoints         | Integer   | ordered list of x coordinates of the points                                    |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| shape/ypoints         | Integer   | ordered list of y coordinates of the points                                    |
+-----------------------+-----------+--------------------------------------------------------------------------------+
| icon                  | String    | the name of the icon in the resource folder (path can be omitted)              |
+-----------------------+-----------+--------------------------------------------------------------------------------+

Actions section
---------------

The actions represent the tasks that could be performed by an object.
These actions must be associated with the hardware command that
have to be executed when the action is launched. As with the behavior,
the name of each action must match the ones used in the object code.
Also the command value should match the name of a existing command
(normally a hardware command created by the hardware plugin developer).

+---------+----------+-------------------------------------------------------------+
| Field   | Values   | Description                                                 |
+=========+==========+=============================================================+
| name    | String   | The name of the action already defined in the object code   |
+---------+----------+-------------------------------------------------------------+
| value   | String   | The name of the command                                     |
+---------+----------+-------------------------------------------------------------+


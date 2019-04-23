
Data structures
===============

If you plan to develop a plugin for Freedomotic you will need to access the framework data structures.

These types of data are used with building automation and the
Freedomotic architecture:

1. Environment topology
2. Environment objects
3. People
4. Loaded plugins and extensions

Environment topology
####################

Environment data are accessible by the static reference. It returns the
Environment's instance which gives you access to all of the Environment's properties

.. code:: java

        Freedomotic.environment.getPojo();

Returns all the zones in the environment
----------------------------------------

.. code:: java

        Freedomotic.environment.getZones()

**Zones** are logical (virtual) portions of the environment. To retrieve
the list of physical environment rooms (rooms are also considered Zones) use:

.. code:: java

        Freedomotic.environment.getRooms()

Environmental 'Things'
----------------------

The 'things' (lights, doors, couches, ...) in the environment can be retrived in different ways:

Get a 'thing' by its name
-----------------------

.. code:: java

        EnvObjectPersistence.getObject(String name)

Get the 'things' filtered by protocol and address property
----------------------------------------------------------

.. code:: java

        EnvObjectPersistence.getObject(String protocol, String address)

Get the list of the 'things' linked to a specific protocol
----------------------------------------------------------

.. code:: java

        EnvObjectPersistence.getObjectByProtocol(String protocol)

Get the list of all 'things' in the current environment
-------------------------------------------------------

.. code:: java

        EnvObjectPersistence.getObjects()

Use the following import to access this method:

.. code:: java

    import com.freedomotic.objects.EnvObjectPersistence;

Plugins
#######

Gets the list of loaded plugins
-------------------------------

.. code:: java

        AddonManager.getLoadedPlugins()

Returns an ArrayList of Plugin type.

Get a plugin by name
--------------------

.. code:: java

        AddonManager.getPluginByName(String name)

Remember to import com.freedomotic.plugins.AddonManager;

Get plugin configuration from manifest
--------------------------------------

You can access configuration file of a plugin in this way:

.. code:: java

        int myVar = configuration.getIntProperty("PROPERTY-NAME", 1);

The second parameter in getIntProperty is the default value to use if
the *PROPERTY-NAME* cannot be found or cannot be converted to the proper
type (int, double, string, ...)

other methods are:

.. code:: java

        boolean myVar = configuration.getBooleanProperty("PROPERTY-NAME", true);
        double myVar = configuration.getDoubleProperty("PROPERTY-NAME", 1.5f);
        String myVar = configuration.getStringProperty("PROPERTY-NAME", "some text");

read tuple properties from config file:

.. code:: java

        boolean myVar = tuple.getBooleanProperty(tupleIndex, "PROPERTY-NAME", true);
        double myVar = tuple.getDoubleProperty(tupleIndex, "PROPERTY-NAME", 1.5f);
        String myVar = tuple.getStringProperty(tupleIndex, "PROPERTY-NAME", "some text");

Get received command parameters
-------------------------------

The onMessage method has a *Command c* parameter. Is possible to access
the received parameters this way:

.. code:: java

    String saveItInAVariable = c.getProperty("COMMAND-PARAM-NAME");

Accessing Data Structures from Crosslanguage Plugins
####################################################

This is done through a REST connection which serves the data. More info can be found
at https://github.com/freedomotic/freedomotic/wiki/Freedomotic-APIs.

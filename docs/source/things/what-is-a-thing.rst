
What is a thing?
================

The base class representing a thing is **EnvObject**. This class describes the main characteristics of a thing in the environment: identification, representations, behaviors and the ways it can interact with the environment (other things or users). 
Things are stored in the application through the serialization of this class.

The class **EnvObjectLogic** incorporates EnvObject as a field, providing several methods to perform operations on it.
This class serves as a template for the creation of objects.

Objects behaviors (the properties you want to monitor or control) are defined as implementations of the interface **BehaviorLogic**.

Initially, depending on a template, objects can be considered virtual or not (it they have an attachment to a data source - a plugin).
In order to establish correspondence with physical devices, an object must be marked as not virtual and a protocol and an address must be defined.

The **protocol** just indicates the plugin being used, while the **address** provides data specific to the protocol (for example the IP address of a network device or a web service or a board port).

These classes already provide representations of things on user interfaces, even allowing interaction with them.

Interaction with physical objects or web services is accomplished using plugins that translate generic data/commands from Freedomotic to the specific counterpart used in those devices/services and vice versa.





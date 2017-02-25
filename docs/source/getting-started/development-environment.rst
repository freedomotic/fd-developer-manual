
Developers quick start
======================

Requirements check
------------------

- **Java JDK:** version 8+ Openjdk/Oracle jdk (to install on Ubuntu: *sudo apt-get install openjdk-8-jdk*)
 
- **Maven:** version 2 or 3 (to install on Ubuntu: *sudo apt-get install maven*)
- **Any OS** with java support (Linux, Windows, Mac, Solaris ...)

**Development status**: beta testing

**Current released version**:
`Freedomotic Commander 5.6 RC3 <https://sourceforge.net/projects/freedomotic/files/freedomotic-commander-5.6.0-rc3.zip/download>`_
(released on 1 Jul 2016)

Set your local development environment
--------------------------------------

1) Fork Freedomotic on GitHub

* Create an account on https://github.com if you don't have one
* Fork Freedomotic repository following this link: https://github.com/freedomotic/freedomotic/fork
* Create the local clone of your online fork with this command:

.. code::
     
    git clone https://github.com/YOUR-GITHUB-USERNAME/freedomotic.git
   
Now you are ready to work.

2) Enter the new local folder

.. code::

    cd freedomotic
    
3) Compile Freedomotic with maven

.. code::

    mvn clean install
    
4) **IMPORTANT!!!! THIS IS REQUIRED: copy the example-data folder into freedomotic-core/data.**

If you miss this step Freedomotic won't start

.. code::

    cp -r data-example/ framework/freedomotic-core/data
    
5) Run Freedomotic

.. code::

    java -jar framework/freedomotic-core/target/freedomotic-core/freedomotic.jar

    
Git repository is an SDK
------------------------

The GIT repository is a complete SDK with all you need to code and test your Freedomotic plugins. Once compiled for the first time open the **freedomotic-core** project with your favourite IDE and start it to try Freedomotic.

To develop your own plugin you can start from the **hello-world** example project included in *GIT_ROOT/plugins/devices/hello-world*. 

Open it in your IDE, make some changes and compile. It will be automatically installed into Freedomotic runtime (**freedomotic-core** project). Just start **freedomotic-core** to try your latest changes.

Javadoc
--------
Please refer to

* `freedomotic-core <https://freedomotic.github.io/javadoc/freedomotic-core/>`_
* `freedomotic-model <https://freedomotic.github.io/javadoc/freedomotic-model/>`_

Support
-------

Please join our `mailing list <https://groups.google.com/forum/#!forum/freedom-domotics>`_ and `share your experience <https://goo.gl/Iq8C6e>`_.

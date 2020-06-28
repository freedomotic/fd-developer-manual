
Developers Quick Start
======================

Requirements
------------------

- **Java JDK:** Version 8 OpenJDK/Oracle JDK (to install on Ubuntu: *sudo apt-get install openjdk-8-jdk*)
 
- **Maven:** Version 2 or 3 (to install on Ubuntu: *sudo apt-get install maven*)
- **Any OS** with java support (Linux, Windows, Mac, Solaris ...)

**Development status**: beta testing

**Current released version**:
`Freedomotic Commander 5.6 RC4 <https://sourceforge.net/projects/freedomotic/files/freedomotic-commander-5.6.0-rc4.zip/download>`_
(released on 16 Aug 2017)

Set your local development environment
--------------------------------------

1) Fork Freedomotic on GitHub

* Create an account on https://github.com if you don't already have one
* Fork the Freedomotic repository by following this link: https://github.com/freedomotic/freedomotic/fork
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

The Git repository is a complete SDK with all you need to code and test your Freedomotic plugins. Once compiled for the first time, open the **freedomotic-core** project with your favourite IDE and start it to try Freedomotic.

To develop your own plugin, you can start from the **hello-world** example project included in *GIT_ROOT/plugins/devices/hello-world*. 

Open it in your IDE, make some changes and compile. It will be automatically installed into the Freedomotic runtime (**freedomotic-core** project). Just start **freedomotic-core** to try your latest changes.

Create a build release
----------------------
To create a new release package execute inside the ROOT folder

.. code::

    mvn clean install
    
The zip file containing the build release is located inside *GIT_ROOT/framework/freedomotic-core/target/release/*.

The release process is based on `create-release.xml <https://raw.githubusercontent.com/freedomotic/freedomotic/master/scripts/create-release.xml>`_ file.

Support
-------

Please join our `international <https://groups.google.com/forum/#!forum/freedom-domotics>`_ or `Italian <https://groups.google.com/forum/#!forum/freedomotic-it>`_ community and `share your experience <https://goo.gl/Iq8C6e>`_.


Security: authentication and authorization
==========================================

Freedomotic uses `Apache Shiro <https://shiro.apache.org/>`_, a Java security framework, to manage authentication and authorization.

This tool is very flexible and offers many other features as cryptography and session management. Also it's very easy to configure and use.

All the classes are accessible from `this folder <https://github.com/freedomotic/freedomotic/tree/master/framework/freedomotic-core/src/main/java/com/freedomotic/security>`_.

Currently we adopt Apache Shiro v1.3.2

Authentication
--------------

The class `UserRealm <https://github.com/freedomotic/freedomotic/blob/master/framework/freedomotic-core/src/main/java/com/freedomotic/security/UserRealm.java>`_ makes the work.
All users' data are stored in *users.xml* file located into *config* folder.  

.. code:: xml

 <?xml version="1.0" encoding="UTF-8" ?>
  <users>
   <user>
    <principals>
      <principal realm="com.freedomotic.security" primary="true">system</principal>
    </principals>
    <credentials>p9aAW+vW4EWkTHsfNWWJcTGBOhUya1ORvu/dvU/A+0g=</credentials>
    <salt>zpGvSXleABptoH8/eq/xMrIkeStJzCT4JbNUe7LpL9g=</salt>
    <roles>
      <role name="administrators"/>
    </roles>
    <properties>
      <property name="language" value="auto"/>
    </properties>
   </user>
   <user>
    <principals>
      <principal realm="com.freedomotic.security" primary="true">admin</principal>
    </principals>
    <credentials>rfx9xwecAlh7Z45fUK+Gi+CSOirq1U2IdmQBTHXoeCw=</credentials>
    <salt>fX5WIcGU2ESqc6ECdOccJuM1ox5brXrOYxWw0EpJTHY=</salt>
    <roles>
      <role name="administrators"/>
    </roles>
    <properties>
      <property name="language" value="auto"/>
    </properties>
   </user>
   <user>
    <principals>
      <principal realm="com.freedomotic.security" primary="true">guest</principal>
    </principals>
    <credentials>TRN2QAmWB9oPk2E9JSZOcQxDmOZU/G1BGrjB92KQfPA=</credentials>
    <salt>vwoYXjSDQzqvr05h+xBprF5pCzGgQhfMAiG95kk67x4=</salt>
    <roles>
      <role name="guests"/>
    </roles>
    <properties>
      <property name="language" value="auto"/>
    </properties>
    </user>
  </users>
  
  
Credentials are saved in hash format (SHA-256).

Every user has a specific **role** as reported in *roles.xml* file.

.. code:: xml

 <?xml version="1.0" encoding="UTF-8" ?>
  <roles>
   <role name="administrators">
    <permissions>
      <permission>*</permission>
    </permissions>
   </role>
   <role name="system">
    <permissions>
      <permission>*</permission>
    </permissions>
   </role>
   <role name="guests">
    <permissions>
      <permission>*:read</permission>
    </permissions>
   </role>
   <role name="managers">
    <permissions>
      <permission>*:create,read,update,delete</permission>
    </permissions>
   </role>
 </roles>
 

A **role** defines a system profile and gives some permissions to interact with the system.

We have four different roles: **administrators**, **system**, **guests** and **managers**. The first two have unlimited privileges.
 
Authorization
-------------

Privileges are managed via *privileges.list* file. Each section reports a list of allowed actions. 

.. code:: xml

 #Currently supported and used privileges
  
 [environments]
 environments:create
 environments:read
 environments:update
 environments:delete
 environments:load #from file
 environments:save #to file

 [zones]
 zones:create
 zones:read
 zones:update
 zones:delete

 [objects]
 objects:create
 objects:read
 objects:update
 objects:delete
 objects:load #from file
 objects:save #to file

 [system]
 sys:config:load
 sys:plugins:load 
 sys:plugins:read
 sys:plugins:start
 sys:plugins:stop
 sys:plugins:update
 sys:shutdown

 [auth]
 auth:privileges:update
 auth:privileges:read
 auth:realms:create
 auth:realms:delete

 #Privileges to be added soon

 [triggers]
 [reactions]
 [commands]

 


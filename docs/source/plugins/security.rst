
Security: authentication and authorization
==========================================

Freedomotic uses `Apache Shiro <https://shiro.apache.org/>`_, a Java security framework, to manage authentication and authorization.

This tool is very flexible and offers many other features as cryptography and session management.

Authentication
--------------

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
  
  
Credentials are saved in hash format.
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
 
Authorization
-------------
 
 


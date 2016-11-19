# Rest API

Freedomotic exposes xml/json Restful API documented with [Swagger](https://helloreverb.com/developers/swagger). 

**These APIs are implemented by Restapi-v3 plugin, refer to [http://freedomotic.com/content/plugins/restapi-v3](http://freedomotic.com/content/plugins/restapi-v3) for further information.**

Start Freedomotic on your localhost and go to [http://localhost:9111/](http://localhost:9111/) to test them live.
 
**Or you can try our public [demo API](http://api.freedomotic.com:9111/)** (username: _admin_; password: _admin_)

![Swagger UI Example](https://helloreverb.com/img/xswagger-hero.png.pagespeed.ic.RM2hi3MU7Z.png)

# Advanced topics
When developers need to interface directly to the message broker (NOT NECESSARY FOR EVERYDAY OPERATIONS), the following methods are available

##Stomp - message broker connector

STOMP (Streaming Text Oriented Messaging Protocol): Download from here the STOMP client for your preferred language http://stomp.github.com//implementations.html. Freedomotic has a STOMP endpoint active on IP: 0.0.0.0 PORT:61666

##Websockets - message broker connector

Freedomotic has a websocket endpoint - covering the active on IP:0.0.0.0 PORT:61614

TODO: add further info
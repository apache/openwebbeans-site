Title: OpenWebBeans SPI

# OpenWebBeans SPI

## What is an SPI?

> Service Provider Interface (SPI) is an API intended to be implemented
> or extended by a third party. It can be used to enable framework
> extension and  replaceable components. - [wikipedia][1]


## Why using a SPI in OpenWebBeans?
First off, reading about [OpenWebBeans Core](openwebbeans-impl.html) will give you
the overall idea about the usage of SPIs in our plugin system. 

Now as mentioned in that description the SPI is simply used to integrate other 
frameworks with OpenWebBeans. The point of gravity for Java EE is definitely going
towards CDI today and the SPI pattern ensures that OpenWebBeans can manage this handily.

From a more technical standpoint it's nothing more then a bunch of interfaces. 
For example a part of the SPI is the following interface:

    package org.apache.webbeans.spi;
   
    /**
    * Conversation related SPI.
    * @version $Rev$ $Date$
    */
    public interface ConversationService
    {
        /**
        * Gets the current conversation id or null
        * if there is no conversation.
        * @return the current conversation id
        */
        public String getConversationId();
       
        /**
        * Gets the session id of the current session.
        * @return the session id of the current user session
        */
        public String getConversationSessionId();
   
    }


After seeing this interface one can easily conclude that frameworks that want to utilize the Conversation Id functionality must implement this interface.
Now since this is part of the specification for JSF 2.x the JSF plugin of course implements it and actually the JSF 1.2 plugin as well. Supporting the Conversation Id in another plugin should be rather intuitive and this is true for the SPI in general. 


### How does OWB know which implementation it should pick?

Each OpenWebBeans JAR has a property file to configure it's internal features
and also the SPI implementation which should be used:

    META-INF/openwebbeans/openwebbeans.properties

All those files contain at single property ``configuration.ordinal`` which defines their 
'importance'. Any setting from a property file with a higher configuration.ordinal will 
overwrite settings from one with a lower configuration.ordinal. The internally used 
configuration.ordinal values range from 1 to 100.

For example: if you use a different UI technology than JSF like Vaadin, you could still provide 
CDI Conversations by writing an own implementation of the respective SPI and tweak
some configuration settings:

    configuration.ordinal=120 
    
    # enable CDI conversation support at all
    org.apache.webbeans.application.supportsConversation=true

    # define your own implementation of our ConversationService SPI
    org.apache.webbeans.spi.ConversationService=some.mycomp.MyVaadinConversationService


All those tricks allow us to remain extensible in the future and to support whatever scenario 
we will face.

  [1]: https://en.wikipedia.org/wiki/Service_provider_interface

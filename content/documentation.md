Title: Documentation

<a name="module-overview"></a>

# Getting Started

If you are completely new to CDI then you might want to take a look at the following introductory articles

  - [How CDI works](cdi_explained.html)
  - [Adding OpenWebBeans to your JavaSE project](owbsetup_se.html)
  - [Adding OpenWebBeans to Apache Tomcat](owbsetup_tomcat.html)
  - [Adding OpenWebBeans to your Servlet Container project](owbsetup_ee.html)
  - [OpenWebBeans as part of JavaEE Containers](owb-eecontainers.html)
  - [OpenWebBeans configuration](owbconfig.html)
  - [FAQ](faq.html)

There are several Application Containers which come with Apache OpenWebBeans as their core CDI container

  - [Apache Meecrowave](/meecrowave/)
  - [Apache TomEE](https://tomee.apache.org)

# OpenWebBeans Plugin Structure

OpenWebBeans consists of a core system which heavily uses SPIs (Service Provider Interfaces)
to extend it's functionality. The core system itself is purely JavaSE based
and does not need any further dependency. All special JavaEE features get added via 
separate plugins.

### System Core
- [OpenWebBeans Core](openwebbeans-impl.html)
- [SPI definition](openwebbeans-spi.html)


### Commonly used Plugins
- [Web plugin](openwebbeans-web.html)
- [EL plugins 1.0 & 2.2](openwebbeans-el.html)
- [JSF plugins 1.2 & 2.x](openwebbeans-jsf.html)
- [Apache Tomcat plugins](openwebbeans-tomcat.html)


### Technical Integration Plugins
- [EE Common plugin](openwebbeans-ee-common.html)
- [Java EE plugin](openwebbeans-ee.html)
- [EJB plugin](openwebbeans-ejb.html)
- [EE Resource plugin](openwebbeans-resource.html)
- [JMS plugin](openwebbeans-jms.html)
- [OSGi plugin](openwebbeans-osgi.html)


# Testing Strategies for CDI Projects
- [JUnit5 integration](openwebbeans-junit5.html)
- [General guidelines for testing](testing_general.html)
- [Deltaspike Test-Control](testing_test-control.html)
- [Apache DeltaSpike CdiCtrl](testing_cdictrl.html)
- [JBoss Arquillian](testing_arquillian.html)
- [Writing unit tests for OWB itself](owbinternalunittests.html)





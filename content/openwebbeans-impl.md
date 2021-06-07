Title: OpenWebBeans Core

# OpenWebBeans Core

**Hint:** The actual jar is called openwebbeans-impl and is the implementation of OpenWebBeans core.

## What does 'core' mean?

OpenWebBeans follows the design principle loose coupling and high cohesion. 
Everything needed to actually be a working CDI-container makes up
OpenWebBeans core. 

All the logical parts in core have high cohesion with each other 
and will age in approximately the same rate. 
To conclude impl is the CDI-container as such and nothing more nothing less. 
It does not contain any other JavaEE specific dependencies beside the bare minimum:

- atinject-api.jar
- cdi-api.jar
- interceptors-api jar
- openwebbeans-spi.jar, our SPI for pluggable extending the core.
- a bytecode engineering library (javassist for 1.0.x and 1.1.x, xbean-asm for 1.2.x and above)
- xbean-finder for classpath scanning

openwebbeans-core does **not** contain any JavaEE dependencies beyond that.
All additional features may portably be added via our SPI (Service Provider Interface).

This way core is completely unaffected by the release cycles of other 
frameworks and Java specifications and the coupling is not only low, 
it's virtually nonexistent.

## Why not a monolithic approach?
  

Imagine if the power outlets in your house was tightly coupled to your various devices. 
The newest and coolest smartphone or what have you would probably not be as tempting 
if it required you hiring an electrician to rewire your entire house. 
To make it more ridiculous the new wiring would be non compatible with your TV 
because it's two years old. 

So if OpenWebBeans would have been built in a monolithic way with tight coupling and low cohesion, 
the newest version would have to drop support for everything but the newest frameworks 
or be a hot mess with version checks and endless if - else cases between all the framework 
combinations. Naturally it would only get worse and worse over time. 

Glad we avoided all that and have the exact opposite result, the latest 
OpenWebBeans still support JSF 2.0, EL-1.0, JSP, Tomcat 7 and so on.
And you can just plug support for your own framework.

## Introduction to our plugin system

The end users has to combine the core with their own mix of plugins and can include 
exactly what they want (including custom plugins) yet nothing they don't need. 
The committers behind OpenWebBeans also have a much easier maintenance process 
and can focus on features, speed and robustness of the dependency injection core
rather then the compatibility matrix. 

This approach also eases the integration into various JavaEE Containers.

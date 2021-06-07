Title: OpenWebBeans FAQ 

# What can OpenWebBeans as CDI container do for you?


## An introduction to CDI

Contexts and Dependency Injection for Java a.k.a. CDI is a JavaEE specification with the number
JSR-299 (CDI-1.0), JSR-346 (CDI-1.1/1.2) and JSR-365 (CDI-2.0).
Apache OpenWebBeans implements these standards. This page will give you an introduction to features of
CDI in general. We will add a special hint whenever a feature is ambiguous in the specification 
and OpenWebBeans implements it in a certain way which might be different on other CDI containers.

## What is CDI at all?

Originally developed under the name ‘Web Beans’, the CDI specification was created 
to fill the gaps which could not be filled by Enterprise Java Beans (EJB) on the back end, 
and JavaServer Faces (JSF) in the view layer. The first draft targeted only 
Java Enterprise Edition (Java EE), but during the creation of the specification 
it became clear that most features were very useful for any Java environment, 
including Java SE.

Even if the full name of the specification is 

> Contexts and Dependency Injection for the Java Enterprise platform

this does not mean that CDI applications can only run in JavaEE those days. 
At least OpenWebBeans provides CDI functionality which also runs in pure Java SE apps like Swing, JavaFX
and even Eclipse RCP apps as well.

### Relationship to JSR-330

Half a year before the CDI specification became final, other communities 
(Spring and guice) had begun an effort to specify the basics of injection as 

> “JSR-330: Dependency Injection for Java” (nicknamed “AtInject”). 

Considering that it did not make sense to provide a new dependency injection container 
without collaborating on the actual injection API, the AtInject and CDI expert groups 
worked closely together to ensure a common solution across dependency injection frameworks. 

As a result, CDI uses the annotations from the AtInject specification, 
meaning that every CDI implementation fully implements the AtInject specification, 
just like Guice and Spring. 

CDI and AtInject are both included in Java Enterprise Edition 6 (JSR-316) 
and thus nowadays an integral part of almost every Java Enterprise Edition server.


## CDI features

Before we go on to dive into some code, let’s take a quick look at some key CDI features:

- **Type Safety**: Instead of injecting objects by a (string) name, 
CDI uses the Java type to resolve injections. When the type is not sufficient, 
a Qualifier annotation can be used. This allows the compiler to easily detect errors, 
and provides easy refactoring.
    
- **POJOs**: Almost every Java object can be injected by CDI! 
This includes EJBs, JNDI resources, Persistence Units and Persistence Contexts, 
as well as any object which previously would have been created by a factory method.
    
- **Extensibility**: Every CDI container can enhance its functionality 
by using “Portable Extensions”. The attribute “portable” means that
those CDI Extensions can run on every CDI container and Java EE 6 server, 
no matter which vendor. This is accomplished by a well-specified 
SPI (Service Provider Interface) which is part of the JSR-299 specification.
   
- **Interceptors**: It has never been easier to write your own Interceptors. 
Because of the portable behaviour of CDI, they now also run on every
EE 6 or later certified server and on all standalone CDI containers.
    
- **Decorators**: These allow to dynamically extend existing interface 
implementations with business aspects.
    
- **Events**: CDI specifies a type-safe mechanism to send 
and receive events with loose coupling.
    
- **Unified EL integration**: EL-2.2 opens a new horizon 
in regard of flexibility and functionality. 
CDI provides out-of-the-box support for it!


## A small CDI Example

The following small JSF and CDI sample application allows 
you to send eMails via a web form.
We will only show code fragments, please checkout our samples form our 
[Source Code](source.html) for more information.


#### The 'Backend'

For our mail application, we need an “application-scoped” MailService. 
Application-scoped objects are essentially singletons – 
the container will ensure you always get the same single instance whenever 
you inject it into your application. 

The ``replyTo`` address is taken from the ConfigurationService 
which is also application-scoped. Here we see our first injection. 
The “configuration” field is not set by the application’s code, 
but is injected by CDI. 
The ``@Inject`` annotation tells CDI to perform the injection.

    @ApplicationScoped
    public class MyMailService implements MailService {
        private @Inject ConfigurationService configuration;
    
        public send(String from, String to, String body) {
            String replyTo = configuration.getReplyToAddress();
            ... // send the email
        }
    }

#### The User handling

Our application also knows the currently loggedin user which we like to 
store in the Servlet Session (the login itself is not part of this sample, 
just consider  this is done via your container or a 
login page + Servlet Filter upfront).

CDI provides the session scope, which ensures that you will get 
the same instance of an object per HTTP Session (in a web application).

    @SessionScoped
    @Named
    public class User {
        public String getName() {..}
        public String getEmail() {..}
        ..
    }

By default, CDI beans are not available for use in JSF via the 
Unified Expression Language. In order to expose it for use by JSF and EL, 
we simply added the ``@Named`` annotation.


#### The Backing Bean

Our web page is implemented with JSF-2. Thus we need a backing bean.
We use a ``@RequestScoped`` backing bean which means that every request
will get a new instance of it. Otoh, during the request you will always get
the same instance.

    @RequestScoped
    @Named
    public class MailForm {
        private @Inject MailService mailService;
        private @Inject User user;
    
        private String text; // + getter and setter
        private String recipient; // + getter and setter 
    
        public String sendMail() {
            mailService.send(user.getName(), recipient, text);
            return "messageSent"; // forward to 'message sent' JSF2 page
        }
    
    }


#### The JSF-2 page

We now only miss the JSF page itself to make our example work.
The page is implemented using *facelets*.

    ...
    <h:form>
        <h:outputLabel value="Username" for="username"/>
        <h:outputText id="username" value="#{user.name}"/><br/>
        <h:outputLabel value="Recipient" for="recipient"/>
        <h:inputText id="recipient" value="#{mailForm.recipient}"/><br/>
        <h:outputLabel value="Body" for="body"/>
        <h:inputText id="body" value="#{mailForm.body}"/><br/>
        
        <h:commandButton value="Send" action="#{mailForm.send}"/>
    </h:form>
    ...

This page uses the backing bean from above via it's EL name *mailForm*.
In the same way it uses the ``@SessionScoped`` *user*.



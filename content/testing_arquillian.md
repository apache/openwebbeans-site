Title: OpenWebBeans CdiCtrl 

# Testing your application with JBoss Arquillian
## About Arquillian

[JBoss Arquillian][1] is a very popular testing framework for complex use cases. You write your test with 
JUnit or TestNG but with the extended possibility to have fine grained control over the managed environment. 
Basically Arquillian allows you to create artificial war / jar files with the exact content you need for your test. 
Many powerful features are available in extensions. One such extension is ``Arquillian Drone`` 
that allows you to test with a web-based user interface and ``Arquillian Performance`` is another one
that offers rich functionality aimed at performance testing.

One of the main principles of ``Arquillian`` is to be portable and thus using it to test applications that 
leverage OpenWebBeans is fully supported.
  
To author a new ``Arquillian`` test you follow this rough flow:

  - Create a new JUnit / TestNG test class
  - Annotate the class with ``@RunWith(Arquillian.class)``
  - Create a new deployment and include exactly what you need for that test. 
It's important that you add beans.xml to your test path and include it in the deployment.
  - Use @Inject (from the usual package) to obtain instances that you included in the deployment
  - Perform assertions on injected instances "normally". 

## Testing our Java SE / Servlet container project
The best way to get started with ``Arquillian`` is to follow the official [Getting Started][2] guide. However for testing 
OpenWebBeans standalone you will need to use the following adapter:

    <dependency>
       <groupId>org.apache.openwebbeans.arquillian</groupId>
       <artifactId>owb-arquillian-parent</artifactId>
       <version>${owb.version}</version>
    </dependency>

## Testing your Java EE project

In addition to the adapter described above TomEE offers serveral adapters for working with TomEE. For more information visit [TomEE: Available Arquillian Adapters][3].
            

  [1]: http://arquillian.org/
  [2]: http://arquillian.org/guides/getting_started/
  [3]: https://tomee.apache.org/arquillian-available-adapters.html
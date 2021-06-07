Title: OpenWebBeans FAQ 

# Adding OpenWebBeans to your Servlet Container project

OpenWebBeans in a great match for web-apps and should work well for your favorite Servlet Containers such as Jetty or Tomcat.
To add OpenWebBeans to your Servlet Container project you need to take the following steps.

 1. Add [dependencies][1] accordingly to instructions below.
 2. In some cases add [org.apache.webbeans.servlet.WebBeansConfigurationListener][2] to web.xml as a listener
 3. Done! Congratulations.


### Adding required jars and plugins to your project

You can add OpenWebBeans to your project manually by adding jars or with Apache Maven. How to download is explained here: [download page][3]. 
The binary distributions include all the jars you need and the download page lists all the [maven dependencies][4]. 
But since OpenWebBeans is modular you should read below so you know what to add. 

### API jars
Several API bundles exists for java EE and they are mostly compatible with OpenWebBeans. If you already include one of these you might not need any api jars. CDI and thus OpenWebBeans depends on the following four apis:

  - **CDI: [geronimo-jcdi_2.0_spec.jar][3]**
  - **AtInject: [geronimo-atinject_1.0_spec.jar][3]**
  - **Interceptor: [geronimo-interceptor_1.2_spec.jar][3]**
  - **Common Annotations: [geronimo-annotation_1.3_spec.jar][3]**

You will only reference these API:s in your own project. This way your project will stay CDI vendor neutral. A typical use case would be to add all four of the above to your parent-pom or your core module. 


### Required  {#required-parts}

For Servlet Container projects such as Tomcat and Jetty you always start with:

  - **[openwebbeans-spi][5]**
  - **[openwebbeans-impl][6]**
  - **[openwebbeans-web][7]**


### Plugins
The following plugins are very useful if you need JSF, expression language (el) etc. Add accordingly to your needs.

- **[openwebbeans-el22][9]**
- **[openwebbeans-tomcat7][10]**
- **[openwebbeans-jsf][13]**

### When to use respective plugin

If the project uses Expression Language add EL plugin accordingly to your version. This is required for using EL.

* Expression Language 2.2 / 3.0 - **[openwebbeans-el22][9]**

</br>
For JSF support add JSF plugin accordingly to your version of JSF. This plugin is required for JSF support and do not forget the include the EL-plugin explained above.

* Java Server Faces 2.0 or later - **[openwebbeans-jsf][13]**

If the project uses Tomcat 7 or above you can add the respective plugin.
This is not required but enables injection in Servlets and Filters.
Note that the tomcat7 plugin works perfectly fine with Tomcat 8, 8.5 and even Tomcat 9.

* Tomcat 7 / Tomcat 8 / Tomcat 9 - **[openwebbeans-tomcat7][11]**
 

### Bootstrapping OpenWebBeans  {#configuring-owb}

Simply put the following listener in web.xml: 

        <listener>
            <listener-class>org.apache.webbeans.servlet.WebBeansConfigurationListener</listener-class>
        </listener>

This is not required if you use Tomcat and added the corresponding Tomcat plugin because in that case it's managed by the plugin.

From here you might want to look at our samples selection: [samples][14].


  [1]: #required-parts
  [2]: #configuring-owb
  [3]: /download.html#apis-version
  [4]: /download.html#maven-dep
  [5]: /download.html#required-version
  [6]: /download.html#required-version
  [7]: /download.html#plugins-version
  [8]: /download.html#plugins-version
  [9]: /download.html#plugins-version
  [10]: /download.html#plugins-version
  [11]: /download.html#plugins-version
  [13]: /download.html#plugins-version
  [14]: /samples.html

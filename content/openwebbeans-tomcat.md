Title: OpenWebBeans Tomcat 

# OpenWebBeans Tomcat 9 and 10

Note: You also need other modules to run OpenWebBeans on Tomcat. 
These Tomcat plugins only adds extra features like injection into Servlets and are not even mandatory for running on Tomcat. 
For more information read [Getting started][1].


### Tomcat 7 and above

    <dependency>
        <groupId>org.apache.openwebbeans</groupId>
        <artifactId>openwebbeans-tomcat</artifactId>
        <version>${owb.version}</version>
        <scope>compile</scope>
    </dependency>

[1]: /owbsetup_ee.html
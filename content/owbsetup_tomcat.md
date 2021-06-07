Title: OpenWebBeans Tomcat integration

# Adding OpenWebBeans to an Apache Tomcat installation

By integrating OpenWebBeans with an existing tomcat installation you don't need to add any CDI container jars to your WAR files.

Instead, OpenWebBeans will get copied to the tomcat lib directory and activated for all webapps.

Note that while the script mentions tomcat-7 this also works for tomcat-8.x and tomcat-9.x

## Downloading the latest OpenWebBeans distribution

Download the latest [OWB distribution bundle](download.html) and unzip it.

## Integrating Apache OpenWebBeans with Apache Tomcat

Change to the unzipped OWB distribution directory and run the following script:

    ./install_owb_tomcat7.sh /path/to/your/tomcat/installation

This will copy all necessary Apache OpenWebBeans jars over to your tomcat installation directory.
It will also patch the tomcat context.xml and add org.apache.webbeans.web.tomcat7.ContextLifecycleListener.

## Integrating OWB and Apache MyFaces with tomcat

We also provide another script which can be used to install OpenWebBeans and [Apache MyFaces JSF Container](http://myfaces.apache.org) into tomcat.

    ./install_owb_tomcat7_myfaces.sh somelocation/myfaces-core-assembly-2.2.8-bin.zip /path/to/your/tomcat/installation

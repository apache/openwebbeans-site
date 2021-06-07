Title: OpenWebBeans FAQ 

# OpenWebBeans and JavaSE

To add OpenWebBeans to your javaSE project you need to take the following steps:

 1. Add required jars to your project
 2. Bootstrap OpenWebBeans
 3. Done! Congratulations.


### Adding required jars to your project

You can add OpenWebBeans to your project manually by adding jars or with Apache Maven. 
How to download is explained here: [download page][1]. This is especially useful if you are not a maven user since the below links goes directly to the maven coordinates.


For JavaSE you need:

  - **[openwebbeans-spi.jar][2]**
  - **[openwebbeans-impl.jar][2]**

Those two parts of OpenWebBeans are what you could call "system core".
These are the only OWB artifacts you need for JavaSE capabilities and 
for the time being the existing plugins basically just adds JavaEE capabilities. 


You also need to add some spec API jars for the CDI, atinject and interceptors 
specifications.

  - **[geronimo-jcdi_2.0_spec.jar][3]**
  - **[geronimo-atinject_1.0_spec.jar][3]**
  - **[geronimo-interceptor_1.2_spec.jar][3]**
  - **[geronimo-annotation_1.3_spec.jar][3]**


 
After you have added the jars described above to your project accordingly 
to the download page and added them to your projects classpath.

### Bootstrapping OpenWebBeans

For now we recommend two ways for booting up the OpenWebBeans container: 
[**Deltaspike CdiCtrl**][4] or booting it yourself in i.e. a standard main method. 

#### Option number one - Apache DeltaSpike CdiCtrl

Apache DeltaSpike is a set of portable CDI Extensions. It contains a module which allows
to control various CDI-Containers without having to change your own code. It contains an API
and multiple implementations for a few CDI Containers.

For most projects [**Deltaspike CdiCtrl**][5] will be the smoother choice to boot your project
in JavaSE . 


#### Option number two - booting yourself**

Going native and booting Apache OpenWebBeans yourself could however be useful if you need full control 
to do advanced things. 

    import org.apache.deltaspike.cdise.api.CdiContainer;
    import org.apache.deltaspike.cdise.api.CdiContainerLoader;
    import org.apache.deltaspike.cdise.api.ContextControl;
    import javax.enterprise.context.ApplicationScoped;

    public class MainApp  {
        private static ContainerLifecycle lifecycle = null;
        public static void main(String[] args)  {
            lifecycle = WebBeansContext.currentInstance().getService(ContainerLifecycle.class);
            lifecycle.startApplication(null);
        }

        public static void shutdown()  {
            lifecycle.stopApplication(null);
        }
    }



From here you might want to look at our samples selection: [samples][6].


  [1]: /download.html
  [2]: /download.html#required-version
  [3]: /download.html#apis-version
  [4]: https://deltaspike.apache.org/documentation.html#with-java-se
  [5]: https://deltaspike.apache.org/documentation.html#with-java-se
  [6]: /samples.html

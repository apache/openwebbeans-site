Title: OpenWebBeans CdiCtrl 

# Testing your application with Apache DeltaSpike CdiCtrl 

## About CdiCtrl

``CdiCtrl`` is *not* part of Apache OpenWebBeans but a module of 
[Apache DeltaSpike][1]. 

The ``CdiCtrl`` interface abstracts away all the logic to boot a CDI Container
and controls the lifecycle of it's Contexts (Request Context, Session Context, etc).

The actual CDI Container is determined by using the ``java.util.ServiceLoader``.
There are a few different implementations available. Besides Apache OpenWebBeans
there are also plugins for JBoss Weld and [Apache TomEE][2]. 

## Adding OpenWebBeans CdiCtrl to your project

The following are the dependencies you need in your Apache Maven pom.xml file in addition to
OWB itself:

    <dependency>
        <groupId>org.apache.deltaspike.cdictrl</groupId>
        <artifactId>deltaspike-cdictrl-api</artifactId>
        <version>$  {deltaspike.version}</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.deltaspike.cdictrl</groupId>
        <artifactId>deltaspike-cdictrl-owb</artifactId>
        <version>$  {deltaspike.version}</version>
        <scope>test</scope>
    </dependency>

## Why use CdiCtrl for your unit tests?

Whenever you need to write unit tests for a full application, then you will need to 
have a CDI container scann all your classes, create ``Bean<T>`` from it and provide 
them for injection. All this can be done by either using JUnits ``@RunWith`` or 
by simply creating a common base class for your unit tests which boots up the 
container on your test classpath.

There is no need to restart the container for each and every of your unit tests
as this would cause a big performance loss. Instead it is usually sufficient to 
use the CdiCtrls ``ContextControl`` mechanism to just stop and restart the 
respective CDI Contexts.

Such a base class could look roughly like the following:

    import org.apache.deltaspike.cdise.api.CdiContainer;
    import org.apache.deltaspike.cdise.api.CdiContainerLoader;
    import org.apache.deltaspike.core.api.projectstage.ProjectStage;
    import org.apache.deltaspike.core.util.ProjectStageProducer;
    import org.apache.deltaspike.core.api.provider.BeanProvider;

    public abstract class ContainerTest  {
    
        protected static volatile CdiContainer cdiContainer;
        // nice to know, since testng executes tests in parallel.
        protected static int containerRefCount = 0;
    
        protected ProjectStage runInProjectStage()  {
            return ProjectStage.UnitTest;
        }
        
        /**
         * Starts container
         * @throws Exception in case of severe problem
         */
        @BeforeMethod
        public final void beforeMethod() throws Exception  {
            containerRefCount++;
    
            if (cdiContainer == null)  {
                // setting up the Apache DeltaSpike ProjectStage
                ProjectStage projectStage = runInProjectStage();
                ProjectStageProducer.setProjectStage(projectStage);
    
                cdiContainer = CdiContainerLoader.getCdiContainer();
    
                cdiContainer.boot();
                cdiContainer.getContextControl().startContexts();
            }
            else  {
                cleanInstances();
            }
        }
    
    
        public static CdiContainer getCdiContainer()  {
            return cdiContainer;
        }
    
        /**
         * This will fill all the InjectionPoints of the current test class for you
         */
        @BeforeClass
        public final void beforeClass() throws Exception  {
            beforeMethod();
    
            // perform injection into the very own test class
            BeanManager beanManager = cdiContainer.getBeanManager();
    
            CreationalContext creationalContext = beanManager.createCreationalContext(null);
    
            AnnotatedType annotatedType = beanManager.createAnnotatedType(this.getClass());
            InjectionTarget injectionTarget = beanManager.createInjectionTarget(annotatedType);
            injectionTarget.inject(this, creationalContext);
    
            // this is a trick we use to have proper DB transactions when using the entitymanager-per-request pattern
            cleanInstances();
            cleanUpDb();
            cleanInstances();
        }
    
        /**
         * Shuts down container.
         * @throws Exception in case of severe problem
         */
        @AfterMethod
        public final void afterMethod() throws Exception  {
            if (cdiContainer != null)  {
                cleanInstances();
                containerRefCount--;
            }
        }
    
        /**
         * clean the NormalScoped contextual instances by stopping and restarting
         * some contexts. You could also restart the ApplicationScoped context
         * if you have some caches in your classes. 
         */
        public final void cleanInstances() throws Exception  {
            cdiContainer.getContextControl().stopContext(RequestScoped.class);
            cdiContainer.getContextControl().startContext(RequestScoped.class);
            cdiContainer.getContextControl().stopContext(SessionScoped.class);
            cdiContainer.getContextControl().startContext(SessionScoped.class);
        }
    
        @AfterSuite
        public synchronized void shutdownContainer() throws Exception  {
            if (cdiContainer != null)  {
                cdiContainer.shutdown();
                cdiContainer = null;
            }
        }
    
        public void finalize() throws Throwable  {
            shutdownContainer();
            super.finalize();
        }
    
    
        /**
         * Override this method for database clean up.
         *
         * @throws Exception in case of severe problem
         */
        protected void cleanUpDb() throws Exception  {
            //Override in subclasses when needed
        }
    
        protected <T> T getInstance(Class<T> type, Qualifier... qualifiers)  {
            return BeanProvider.getContextualReference(type, qualifiers);
        }
    
    }

## Testing JavaEE applications

You can also plug in a cdictrl backend for [Apache TomEE][2] whenever you need to not only test CDI applications 
but a full JavaEE application which has EJBs, managed DataSources, JTA, etc
The only thing you need to do is to replace your ``deltaspike-cdictrl-owb`` dependency in your pom with
``deltaspike-cdictrl-openejb``. Since Apache TomEE and Apache OpenEJB both contain OpenWebBeans as CDI container
you will get all the OWB functionality plus other JavaEE functionality.  

You can pass DataSource configuration by simply providing a ``Properties`` instance to 
``CdiContainer.boot(dbConfiguration)`` in the beforeMethod method of the test class above:

    public final void beforeMethod() throws Exception  {
        containerRefCount++;
    
        if (cdiContainer == null)  {
            // setting up the Apache DeltaSpike ProjectStage
            ProjectStage projectStage = runInProjectStage();
            ProjectStageProducer.setProjectStage(projectStage);
            cdiContainer = CdiContainerLoader.getCdiContainer();
    
            Properties dbProperties = new Properties();
            String dbvendor = ConfigResolver.getPropertyValue("dbvendor", "h2");
            URL dbPropertiesUrl =  getClass().getResource("/db/db-" + dbvendor + ".properties");
            if (dbPropertiesUrl != null)  {
                InputStream is = dbPropertiesUrl.openStream();
                try  {
                    dbProperties.load(is);
                }
                finally  {
                    is.close();
                }
            }

            cdiContainer.boot(dbProperties);
        }
        else  {
            cleanInstances();
        }
    }
        
The ``db/db-mysql.properties`` file for Apache OpenEJB (the former name of TomEE) would look like:

    MYDS = new://Resource?type=DataSource
    MYDS.JdbcDriver = org.h2.Driver
    MYDS.JdbcUrl = jdbc:h2:file:/tmp/h2/myappdb
    MYDS.JtaManaged = true
    MYDS.UserName = sa
    MYDS.Password =


  [1]: https://deltaspike.apache.org
  [2]: https://tomee.apache.org


Title: OpenWebBeans FAQ 

# OpenWebBeans Configuration
Internal configuration of OpenWebBeans can be done via: src/main/resources/META-INF/openwebbeans/openwebbeans.properties

All those files contain at single property ``configuration.ordinal`` which defines their 
'importance'. Any setting from a property file with a higher configuration.ordinal will 
overwrite settings from one with a lower configuration.ordinal. The internally used 
configuration.ordinal values range from 1 to 100. The default value is 100.

This trick allows us to split OpenWebBeans in different modules which automatically change the container
configuration if you put that jar on the classpath.

For overriding the default configuration with own settings simply put a ``META-INF/openwebbeans/openwebbeans.properties``
file into your projects classpath (e.g. into a jar) and either use a ``configuration.ordinal`` higher than 100 or leave
it empty to get the default value of 100.

If you use OpenWebBeans as part of another project then you can assume that most of the values got tweaked
by the integration regarding to the specific needs.


## Configure SPI implementations

OpenWebBeans provide a set of Service Provider Interfaces and multiple different implementations a user can choose from.

You can choose the implementations you like to use for your situation by configuring them in
``META-INF/openwebbeans/openwebbeans.properties``.

Read more about our SPIs in [OpenWebBeans SPI](openwebbeans-spi.html)


## Other Configurable Values

The following configuration values can get tweaked to get tailor OWB to your specific needs.

Boolean values can either be <code>true</code>, or <code>TRUE</code>, or <code>false</code>, or <code>FALSE</code>.

<ul>
    <li style="border-bottom: dashed; border-width: 1px">
        <p><code>org.apache.webbeans.forceNoCheckedExceptions</code></p>
        <p>
            Lifycycle methods like <code>javax.annotation.PostConstruct</code> and
            <code>javax.annotation.PreDestroy</code> must not define a checked Exception
            regarding to the spec. But this is often unnecessary restrictive so we
            allow to disable this check application wide.
        </p>
        <p>Defaults to <code>true</code>.</p>
    </li>
    <li style="border-bottom: dashed; border-width: 1px">
        <p><code>org.apache.webbeans.spi.deployer.useEjbMetaDataDiscoveryService</code></p>
        <p>Whether to perform EJB discovery or not.</p>
        <p>Defaults to <code>false</code>. In TomEE this gets automatically enabled.</p>
    </li>
    <li style="border-bottom: dashed; border-width: 1px">
        <p><code>org.apache.webbeans.application.jsp</code></p>
        <p>
            If enabled, we automatically try to register the ELResolver in the JSP engine.
            Enable this setting if you like to access <code>@Named</code> CDI beans in JSP Expression Language.
        </p>
        <p>Default is <code>false</code></p>
    </li>
    <li style="border-bottom: dashed; border-width: 1px">
        <p><code>org.apache.webbeans.application.supportsConversation</code></p>
        <p>Enable support for the CDI <code>@ConversationScoped</code>.</p>
        <p>Disabled by default in JavaSE, but enabled by default when adding the webbeans-web module (Servlets)</p>
    </li>
    <li style="border-bottom: dashed; border-width: 1px">
        <p><code>org.apache.webbeans.application.supportsProducerInterception</code></p>
        <p>
            Define if a CDI interceptor can be used on a producer method or field.
            In OpenWebBeans you can use a <code>@StereoType</code> with an Interceptor to enable
            an Interceptor on the instance returned from a Producer Method or Producer Field.
        </p>
        <p>Enabled by default.</p>
    </li>
    <li style="border-bottom: dashed; border-width: 1px">
        <p><code>org.apache.webbeans.scanExclusionPaths</code></p>
        <p>
            A list of known JARs/paths which should not be scanned for beans.
            This is only used by the default ScannerService implementation.</p>
        <p>Please refer to the openwebbeans.properties file in ``webbeans-impl.jar``</p>
    </li>
    <li style="border-bottom: dashed; border-width: 1px">
        <p><code>org.apache.webbeans.scanBeansXmlOnly</code></p>
        <p>
            Flag which indicates that only jars with an explicit META-INF/beans.xml marker file shall get parsed.
            This basically switches OWB back to the CDI-1.0 scanning behaviour and might speed up the boot process.
        </p>
        <p>Default is false</p>
    </li>
    <li style="border-bottom: dashed; border-width: 1px">
        <p><code>org.apache.webbeans.ignoredDecoratorInterfaces</code></p>
        <p>
            A comma-separated list of fully qualified class names that should be ignored
            when determining if a decorator matches its delegate.
        </p>
        <p>Default is an empty list</p>
    </li>
    <li style="border-bottom: dashed; border-width: 1px">
        <p><code>org.apache.webbeans.web.eagerSessionInitialisation</code></p>
        <p>
            By default we do _not_ force session creation in our WebBeansConfigurationListener. We only create the
            Session if we really need the SessionContext. E.g. when we create a Contextual Instance in it.
            Sometimes this creates a problem as the HttpSession can only be created BEFORE anything got written back
            to the client.
            With this configuration you can choose between 3 settings
            <ul>
                <li>&quot;true&quot; the Session will <u>always</u> eagerly be created at the begin of a request</li>
                <li>&quot;false&quot; the Session will <u>never</u> eagerly be created but only lazily when the first &#064;SessionScoped bean gets used</li>
                <li>any other value will be interpreted as Java regular expression for request URIs which need eager Session initialization</li>
            </ul>
        </p>
        <p>Defaults to false. A session will only get created if a <code>@SessionScoped</code> bean gets accessed for the first time.</p>
    </li>
    <li style="border-bottom: dashed; border-width: 1px">
        <p><code>org.apache.webbeans.generator.javaVersion</code></p>
        <p>
            The Java Version to use for the generated proxy classes.
            If <code>auto</code> then we will pick the version of the current JVM.
            <em>Attention:</em> If you like to use Java8 Lambdas in CDI bean method signatures then you need to
            switch to either <code>auto</code> or <code>1.8</code>!
        </p>
        <p>
            The default is set to <code>1.6</code> as some tools in jetty/tomcat/etc still
            cannot properly handle Java8 (mostly due to older Eclipse JDT versions).
        </p>
    </li>
    <li style="border-bottom: dashed; border-width: 1px">
        <p><code>javax.enterprise.inject.allowProxying.classes</code></p>
        <p>
            Environment property which comma separated list of classes which
            should NOT fail with an UnproxyableResolutionException.
            You only have to configure additional classes in your openwebbeans.properties file.
            All the configured values get added together into a big List.
        </p>
        <p>By default we allow the following classes: <code>java.util.HashMap</code> and <code>java.util.Calendar</code>
        </p>
    </li>
</ul>


## Proxy Mapping

OpenWebBeans enables the user to define the NormalScope handlers for specific scopes.

NormalScope handlers are used by OpenWebBeans' proxies to resolve the 'Contextual Instance'.
E.g. for a ``@SessionScoped User`` injected into some other class, this is exactly the piece of code
which goes into the current Http Session and gets the User instance from there.
This class must extend ``org.apache.webbeans.intercept.NormalScopedBeanInterceptorHandler`` and overwrite the
``Object getContextualInstance()`` method.

This allows for more aggressive caching than with the generic ``NormalScopedBeanInterceptorHandler`` which is the default.
The default NormalScope handler will look up the Contextual Instance in the respective Context for each and every
method invocation on the proxy.

But sometimes we can much more aggressively cache the instances.

E.g. for ``@ApplicationScoped`` beans we can keep the contextual instance inside the proxy,
making it as fast as a pure Java instance - but still gaining all the benefits of CDI!

For ``@RequestScoped`` and ``@SessionScoped`` we can use a NormalScope handler which caches the Contextual Instance in a ThreadLocal.

By default the following NormalScope handlers get used:

<pre>
org.apache.webbeans.proxy.mapping.javax.enterprise.context.ApplicationScoped
    =org.apache.webbeans.intercept.ApplicationScopedBeanInterceptorHandler
org.apache.webbeans.proxy.mapping.javax.enterprise.context.RequestScoped
    =org.apache.webbeans.intercept.RequestScopedBeanInterceptorHandler
org.apache.webbeans.proxy.mapping.javax.enterprise.context.SessionScoped
    =org.apache.webbeans.intercept.SessionScopedBeanInterceptorHandler
</pre>

As you can see we use a prefix ``org.apache.webbeans.proxy.mapping.`` followed by the fully qualified scope name as key.
The value represents the fully qualified name of the handler class.

If you have a custom scope which spans a Request or longer then you can simply reuse the ``RequestScopedBeanInterceptorHandler`` as shown in the following example:

<pre>
org.apache.webbeans.proxy.mapping.org.apache.deltaspike.core.api.scope.ViewAccessScoped
    =org.apache.webbeans.intercept.RequestScopedBeanInterceptorHandler
</pre>


## Enable FailOver / Session Replication support

#### Since OpenWebBeans-1.5.0

OWB-1.5.x and later does *not* need any special module or filter to enable clustering.
All that works out of the box as we now directly utilize the Servlet Session.

#### OpenWebBeans-1.2.x


Add the clustering module to your project:

    <dependency>
        <groupId>org.apache.openwebbeans</groupId>
        <artifactId>openwebbeans-clustering</artifactId>
    </dependency>

Register the FailOverFilter in your web.xml:

    <filter>
        <filter-name>OWB FailOverFilter</filter-name>
        <filter-class>org.apache.webbeans.web.failover.FailOverFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>OWB FailOverFilter</filter-name>
        <servlet-name>Faces Servlet</servlet-name>
    </filter-mapping>



#### OpenWebBeans-1.0.x and 1.1.x


Add the following properties in your openwebbeans.properties:

    configuration.ordinal=100 
    org.apache.webbeans.web.failover.issupportfailover=true
    org.apache.webbeans.web.failover.issupportpassivation=true


Register the FailOverFilter in your web.xml:

    <filter>
        <filter-name>OWB FailOverFilter</filter-name>
        <filter-class>org.apache.webbeans.web.failover.FailOverFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>OWB FailOverFilter</filter-name>
        <servlet-name>Faces Servlet</servlet-name>
    </filter-mapping>

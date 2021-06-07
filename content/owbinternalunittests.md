Title: Testing strategies 

# Testing strategies for unit tests inside OpenWebBeans

One could argue that unit tests are harder to write when your instances are managed by a container. 
However this is only true if the booting of said container is uncharted territory. 
But lets assume the container control is taken cared of handily. Well then it can be quite the breath of fresh air to test code 
that leverage dependency injection.

Here comes the good news. Testing OpenWebBeans and CDI in general (and actually many other Java EE frameworks) 
is in a good state as of today and testing frameworks are reaching a pretty decent level of maturity. Stay with us and 
we will compare the pros and cons with 
the different strategies. 

### Good Practice

We won't go in to detail on how to properly use unit tests or integration tests in your project. However so called "Whitebox Testing" is 
discouraged. Using whitebox testing is often critizied regardless but with frameworks that leverage 
proxies it's simply not something you should attempt. Any reflection trick would likely miss the mark and modify the proxy.
Instead focus on functional tests and structure your code with high cohesion so that testing the public methods get's the job done.
Remember to add beans.xml and other resources to your to your test path.


### Start small with plain tests
Testing code that leverage CDI does not differ much from using CDI in your project. You should start with just a plain pojo 
in your project and likewise a plain unit test. Only when you need context should you upgrade the pojo to a CDI managed instance.
Still this does not mean you automatically need something more then plain unit test. But when you need the container to act on your
instances (for example to trigger ``@PostConstruct``) then go ahead and upgrade the test to be CDI aware.

### CDI aware tests
In OpenWebBeans we use JUnit as testing framework. For not having to deal with the container details each time you can
simply write a JUnit test which extends the ``org.apache.webbeans.test.AbstractUnitTest`` base class.

Lets look at how to write a unit test which e.g. tests a method invocation on a specific CDI bean.

The first thing we obviously need is the CDI bean which should get tested:

    @RequestScoped
    public class BeanUnderTest
    {
        public int meaningOfLife()
        {
            return 42;
        }
    }

And now let's write the unit test which calls this method:

    public class MySimpleTest extends AbstractUnitTest
    {
        @Test
        public void testMeaningOfLife()
        {
            startContainer(BeanUnderTest.class);
            BeanUnderTest instance = getInstance(BeanUnderTest.class);
            Assert.assertnotNull(instance);
            Assert.assertEquals(42, instance.meaningOfLife());
        }
    }

That's all! You don't need even need to manually shut down the container after the method.
If you have multiple beans to test then pass all of them as argument to ``startContainer(Class...);``.
There are also startContainer variants which take a beans.xml. Also look at the other methods of ``AbstractUnitTest`` for more useful features.
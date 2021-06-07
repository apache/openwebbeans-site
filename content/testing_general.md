Title: Testing strategies 

# Testing strategies for projects that leverage CDI

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
proxies it's simply not something you should attempt. Any reflection trick will would likely miss the mark and modify the proxy.
Instead focus on functional tests and structure your code with high cohesion so that testing the public methods get's the job done.
Remember to add beans.xml and other resources to your to your test path.


### Start small with plain tests
Testing code that leverage CDI does not differ much from using CDI in your project. You should start with just a plain pojo 
in your project and likewise a plain unit test. Only when you need context should you upgrade the pojo to a CDI managed instance.
Still this does not mean you automatically need something more then plain unit test. But when you need the container to act on your
instances (for example to trigger ``@PostConstruct``) then go ahead and upgrade the test to CDI aware. To get started with plain unit testing please
refer to [JUnit][1] or [TestNG][2] as a starting point.

### CDI aware tests
These frameworks for testing all rely on either JUnit or TestNG and simply adds CDI container control capabilities. None of these frameworks are exclusive
and ``CdiCtrl`` can support any CDI environment including tests. Apache Deltaspike Core  may also prove useful (perhaps especially the BeanProvider). [Deltaspike Core][3].

**[Apache Deltaspike Test-Control](testing_test-control.html)**<br>
The by far easiest and most straight forward way to write your first test is definitely with [Apache Deltaspike Test-Control][4]. 
This way of testing boots the CDI Container with minimal code and there's not much to learn besides CDI and unit testing with JUnit / TestNG. 
Since Test-Control is a thin layer on top of ``CdiCtrl`` it's advised to continue reading below for more information.

**[Apache Deltaspike CdiCtrl](testing_test-control.html)**<br>
Since ``Test-Control`` is very new it currently lacks support for TestNG and only offers one integration point (MyFaces-Test). If you experience Test-Control
as insufficient the Apache Deltaspike team might propose that you use ``Apache Deltaspike CdiCtrl`` directly but communicating with them might be a
 good idea, please refer to [Deltaspike Community][5] for contact details. ``CdiCtrl`` 
is a powerful framework that Test-Control is built on top off and using it directly is another solution to consider. 
This is still common and well documented since Test-Control is a new module. ``CdiCtrl`` is commonly referred to as the lightweight champion of CDI testing.
In it's nature it's simple and to the point (``Test-Control`` even more so). 

**[JBoss Arquillian](testing_arquillian.html)**<br>
Crowned the heavyweight champion of CDI testing certainly not all for naught. ``Arquillian`` offers fine grained control over the deployment and has 
many popular extensions for testing. For example testing with a web based user interface is possible. If you want to test very complex combinations in your deployments
or you need to isolate to the bare minimum, then Arquillian really shines. It also offers several xml configuration points and similar that might be needed for large 
complicated projects.

  [1]: https://junit.org/
  [2]: https://testng.org/doc/index.html
  [3]: https://deltaspike.apache.org/core.html
  [4]: https://deltaspike.apache.org/test-control.html
  [5]: https://deltaspike.apache.org/community.html

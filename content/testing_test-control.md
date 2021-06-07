Title: OpenWebBeans Test-Control

# Testing your application with Apache DeltaSpike Test-Control 

## About Test-Control


``Test-Control`` is *not* part of Apache OpenWebBeans but a module of 
[Apache DeltaSpike][1]. It is based on another Deltaspike module called ``CdiCtrl``. 
``Test-Control`` is available in Deltaspike 0.6 and onwards.

``CdiCtrl`` abstracts away all the logic to boot a CDI Container
and controls the life cycle of it's Contexts (Request Context, Session Context, etc). 
This module can be extremely powerful for CDI projects both for tests and during runtime. 
It's a long time recommendation to use 
``CdiCtrl`` to write tests with low - medium complexity and we explain how here: 
[Testing with cdiCtrl](testing_cdictrl.html).

With ``Test-Control`` the few steps you had to do on your own are taken cared of for you. In a way ``Test-Control`` 
However ``Test-Control`` though it's fairly new and has no support for TestNG at the moment. It may also be insufficient if you need your own custom 
extension points or have other complex demands.
 
## Adding Deltaspike Test-Control to your project

The following are the dependencies you need in your Apache Maven pom.xml file in addition to
OWB itself:

    <dependency>
        <groupId>org.apache.deltaspike.cdictrl</groupId>
        <artifactId>deltaspike-cdictrl-api</artifactId>
        <version>${deltaspike.version}</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.deltaspike.cdictrl</groupId>
        <artifactId>deltaspike-cdictrl-owb</artifactId>
        <version>${deltaspike.version}</version>
        <scope>test</scope>
    </dependency>

    <dependency>
        <groupId>org.apache.deltaspike.cdictrl</groupId>
        <artifactId>deltaspike-test-control-module-api</artifactId>
        <version>${deltaspike.version}</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.deltaspike.cdictrl</groupId>
        <artifactId>deltaspike-test-control-module-impl</artifactId>
        <version>${deltaspike.version}</version>
        <scope>test</scope>
    </dependency>

Note that deltaspike-cdictrl-openejb can substitute the deltaspike-cdictrl-owb dependency if you are using TomEE.


## Why use Test-Control for your unit tests?

Test-Control offers the strong testing capabilities of ``CdiCtrl`` 
but let's you focus on writing the actual tests alone. ``Test-Control`` is the new lightweight champion for testing CDI and the required setup is very minimal.
The testing flow is intuitive and follows the principles of other test runners. 

## Examples and Getting started

This information can be found here [Deltaspike documentation for Test-Control](https://deltaspike.apache.org/test-control.html)

  [1]: https://deltaspike.apache.org
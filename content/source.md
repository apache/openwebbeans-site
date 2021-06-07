Title: Source

# Getting Involved

We are always looking for new contributors to the project.

Pleaes see our [Community Section](community.html) for more information.

# Cannonical Source Repository

The sources of Apache OpenWebBeans are maintained in the Apache Software Foundation gitbox repository.
This is the repository where all committers work on.
It gets mirrored to GitHub in both directions.
That means you can either push to Apache GitBox or GitHub.

The master branch currently contains our implementation of the CDI-2.0 specification and is considered production ready.

The sources can be checked out read only with the following command:

<pre>
$> git clone https://github.com/apache/openwebbeans
</pre>

or
<pre>
$> git clone https://gitbox.apache.org/repos/asf/openwebbeans.git
</pre>


## Maintenance releases targetting older CDI specifications

### CDI-1.2 - OpenWebBeans-1.7.x

For checking out sources of the stable CDI-1.2 version of OpenWebBeans, please use the owb_1.7.x branch from here:

<pre>
$> git clone https://github.com/apache/openwebbeans
$> git checkout owb_1.7.x
</pre>

### CDI-1.0 - OpenWebBeans-1.2.x

For checking out sources of the stable CDI-1.0 version of OpenWebBeans, please use the owb_1.2.x branch from here:

<pre>
$> git clone https://github.com/apache/openwebbeans
$> git checkout owb_1.2.x
</pre>


# Building OpenWebBeans

Apache OpenWebBeans can be built by using Apache Maven. Just go into the source directory and execute

<pre>
mvn clean install
</pre>

The following maven profiles exist in our build to trigger additional build steps and configuration:

* tck - for executing the CDI (JSR-299, JSR-346 resp JSR-365) standalone TCK
* jsr330-tck - for executing the JSR-330 'atinject' TCK


In master they are all activated by default and run every time you build OpenWebBeans.

For older OpenWebBeans versions you might enable them manually.

<pre>
mvn clean install -Ptck -Pjsr330-tck -Pdoc
</pre>

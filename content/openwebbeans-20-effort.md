Title: Road to OpenWebBeans-2.0

# The Road to OpenWebBeans-2.0 (CDI-2.0)

The Apache OpenWebBeans team has now finished working on implementing the
[CDI-2.0 (JSR-365)](https://jcp.org/en/jsr/proposalDetails?id=365) specification.

This specification is now finished and available for download from the official JSR page.
Members of the OpenWebBeans team are actively involved in the CDI Expert Group which is writing the specification.
So if you have any questions then just ping us.

## Where to get further information about the CDI Specification

The CDI specification is hosted at github

    https://github.com/cdi-spec/cdi

If you just like to get the source of the spec then install [GIT](https://git-scm.com/)
and invoke the following command line:

    git clone https://github.com/cdi-spec/cdi.git

If you like to actively contribute to the spec then please use the
'clone' button and follow the instructions on the page.

You should finally end with all the sources necessary to build the specification
locally by typing:

    mvn clean install

You finally end up with the spec PDF in

    ./spec/target/publish/docbook/publish/en-US/pdf/cdi-2.0-*.pdf


## Where to get OpenWebBeans-2.0 sources?

The CDI-2.0 version of OpenWebBeans is now in our trunk.
Please see our [Source Section](source.html) for more information


You might eventually like to also tweak the CDI API itself.
For example if you find that some description is missing in the JavaDoc of our API.
This package is maintained in the Apache Geronimo specs project (where we maintain all spec jars).

    svn co https://svn.apache.org/repos/asf/geronimo/specs/trunk
    cd geronimo-jcdi_2.0_spec/
    mvn clean install

Title: OpenWebBeans JUnit 5

# OpenWebBeans JUnit 5

OpenWebBeans provides a JUnit 5 integration. It brings an extension which will be backed by CDI Standalone Edition API (`SeContainer`).

The entry point is `@Cdi` API which maps the main methods of `SeContainerInitializer`.
It typically enables you to define which classes you want to deploy for the test or if you want to use classpath scanning.

Here is how a test can look like:

    @Cdi(disableDiscovery = true, classes = MyService.class)
    class CdiTest {
        @Inject
        private MyService service;

        @Test
        void test() {
            assertEquals("ok", service.ok());
        }
    }

As you may notice, this test disable the default classpath scanning and only activate `MyService` bean.

Once a test is marked `@Cdi` you get injections of the underlying container available in the test class. This is how previous snippet can inject `MyService` into the test class without having anything else to do.

## Dependency

     <dependency>
        <groupId>org.apache.openwebbeans</groupId>
        <artifactId>openwebbeans-junit5</artifactId>
        <version>${owb.version}</version>
      </dependency>

IMPORTANT: this feature comes with OpenWebBeans >= 2.0.11.

## Reusable mode

`@Cdi` also adds a specific API called `reusable`. When this is set to `true`, the container is started and then reused by next test.
This enables to speed up the test suite a lot when reusing the same JVM to run all tests.

For example if you have these two tests:

    @Cdi(reusable = true)
    class MyFirstTest {
        @Inject
        private MyService service;

        @Test
        void test() {
            assertEquals("ok", service.ok());
        }
    }

and

    @Cdi(reusable = true)
    class MySecondTest {
        @Inject
        private MyOtherService service;

        @Test
        void test() {
            assertEquals("ok2", service.ok());
        }
    }

Then CDI container will be started only once.

However if the first test is:

    @Cdi(reusable = true, disableDiscovery = true, classes = MyService.class)
    class MyFirstTest {
        // .. as before
    }

Then the second test will not have its `MyOtherService` bean since previous test will only deploy `MyService`.

To avoid that it is recommended to follow these best practises:

- Never mix reusable and not reusable tests in the same suite (define multiple surefire executions for example),
- For reusable tests define a meta annotation which shares the configuration for all tests, this means you must have a single `reusable = true` in your whole code (per execution).

To define a reusable setup you just define a new annotation decorated with `@Cdi`:

    @Target(TYPE)
    @Retention(RUNTIME)
    @Cdi(reusable = true, disableDiscovery = true, packages = MyService.class)
    public @interface TestConfig {
    }

Then your test class can look like:


    @TestConfig
    class MySecondTest {
        @Inject
        private MyService service;

        @Test
        void test() {
            assertEquals("ok", service.ok());
        }
    }

### Surefire

For reusable mode to be efficient, it is recommended to use this surefire configuration:

    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-surefire-plugin</artifactId>
      <version>${surefire.version}</version>
      <configuration>
        <forkCount>1</forkCount>
      </configuration>
    </plugin>

And here is how to define executions to mix reusable and not reusable tests - which can have a different deployment setup:

    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>${surefire.version}</version>
        <executions>
            <execution> <!-- skip default setup since we redefine it -->
                <id>default-test</id>
                <configuration>
                    <skip>true</skip>
                </configuration>
            </execution>
            <execution>
                <id>not-reusable</id>
                <phase>test</phase>
                <goals>
                    <goal>test</goal>
                </goals>
                <configuration>
                    <includes>**/perclass/*</includes>
                </configuration>
            </execution>
            <execution>
                <id>reusable</id>
                <phase>test</phase>
                <goals>
                    <goal>test</goal>
                </goals>
                <configuration>
                    <includes>**/reusable/*</includes>
                </configuration>
            </execution>
        </executions>
        <configuration>
          <forkCount>1</forkCount>
        </configuration>
    </plugin>

This setup assumes not reusable tests are in a `perclass` package and reusable ones are in a `reusable` package.
Alternatively you can use categories (just define markers in your test code) but this is generally less obvious to manage on the long run.

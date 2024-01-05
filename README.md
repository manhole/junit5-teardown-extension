# Teardown Extension

TeardownExtension is an extension for JUnit Jupiter. Provides _Automated Teardown_ mechanism.

- Make it easier for developers to read and write teardown code.
- Ensuring the test fixtures are torn down.
- Inspired by xUTP's [Automated Teardown](http://xunitpatterns.com/Automated%20Teardown.html).

## Example

Example usage:

```java
// Annotate TeardownExtension
@ExtendWith(TeardownExtension.class)
class WithTeardownRegistryDemo {

    /*
     * Use teardownRegistry field in test methods and @BeforeEach or @AfterEach lifecycle methods.
     *
     * This field is injected by TeardownExtension.
     */
    private TeardownRegistry teardownRegistry;

    @Test
    void testWithTeardownRegistry() throws Exception {
        // === Setup ===

        final Student alice = createStudent("alice");
        // Registered code is executed after @AfterEach
        teardownRegistry.add(() -> deleteStudent(alice));

        final Course course = createCourse("TDD");
        // Registered code is executed after @AfterEach
        teardownRegistry.add(() -> deleteCourse(course));

        // === Exercise ===

        course.register(alice);

        // === Verify ===

        assertEquals(1, course.numberOfStudents());

        // === Teardown ===

        /*
         * After tests, either succeeded or failure, all students and courses is deleted.
         *
         * The order is in reverse order of addition to the TeardownRegistry, as follows:
         * 1. deleteCourse(course)
         * 2. deleteStudent(alice)
         */
    }

}
```

## Supported Java Versions

Java 8.

## Dependency

Gradle:

TODO

Maven:

TODO

## License

Licensed under the ASL2 license.

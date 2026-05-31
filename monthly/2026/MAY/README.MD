# Readings

## Microservices Patterns (ISBN: 978-1-61-729454-9)

Chapter 9: Testing microservices: Part 1

- Manual tests are inefficient (machine can do much more in much shorter time than a human), they happen late in the
  process, don't give immediate feedback during the development process. The complexity of microservices demand
  automatic tests.
- Test doubles simulate the behavior of dependencies to system under tests. **Stubs** are used to return configured
  values to the code. **Mocks** are used to verify the integration with dependencies.
- The testing quadrant describes the nature and purpose of the test. On one edge sit tests written with the business
  convention in mind, while on the other edge sit tests written using the technology-driven convention. While some tests
  exist to support daily programming, other exist to critique the application, pointing areas which need improvements.
  Examples:
    - Technology-facing, supporting programming: unit tests, integration tests, component tests
    - Business-facing, supporting programming: e2e tests
    - Technology-facing, critique the application: performance tests, failover tests
    - Business-facing, critique the application: exploratory tests, usability tests
- Communication is central to microservices approach. Interactions can be tested by invoking the real dependencies, but
  these are in fact e2e tests and should be avoided. The business logic of dependency service should not be tested.
- Contract tests should be used to test microservices interactions. Consumers add these tests to providers' test suites,
  which are run in producers' pipelines. Producers than publish the artifact to maven repository, containing interfaces
  and mocked controllers/event consumers. When these tests fail on producer's side it means, that the producer
  introduced an incompatible change. Frameworks like _Spring Cloud Contract_, _Pact_, support this approach.
- There are two types of unit tests: solitary unit tests, where dependencies are mocked, and sociable unit tests, which
  test units with their dependencies.
- Entities should be tested using sociable unit tests, where they interact with dependencies. Value objects usually
  don't depend on other objects, so they should be tested using solitary unit tests.
- Sagas should be tested with mocked message brokers and database integrations. _Eventuate Tram Saga_ framework supports
  that approach.
- Domain services should be tested with solitary unit tests, where dependencies, like repositories or other services,
  are mocked.
- Controllers should be tested using solitary tests, with dependencies, like services, mocked. _Spring Mock MVC_
  framework support this way of testing. They provide a way to test HTTP interactions without real network calls.
- Event adapters should be tested the similar way as controllers. _Eventuate Tram Mock Messaging_ framework supports
  such approach for asynchronous communication (same given/when/then format as Rest Assured).

# Projects - software

## Actracker

- [Actracker API JPA sort refactoring](https://github.com/equino-actracker/actracker-api/pull/184)
- [Actracker API - Sort Tag Sets by name](https://github.com/equino-actracker/actracker-api/pull/185)
- [Actracker UI - Default tag sets sorting by name](https://github.com/equino-actracker/actracker-ui/pull/75)
- [Actracker API - Sort Dashboards by name](https://github.com/equino-actracker/actracker-api/pull/186)
- [Actracker UI - Default dashboards sorting by name](https://github.com/equino-actracker/actracker-ui/pull/76)
- [Actracker API - Sort Activities by title](https://github.com/equino-actracker/actracker-api/pull/187)
- [Actracker API - Sort Activities by end time](https://github.com/equino-actracker/actracker-api/pull/188)
- [Actracker UI - Default activities sorting by end time](https://github.com/equino-actracker/actracker-ui/pull/77)
- [Actracker API - Sort Activities by start time](https://github.com/equino-actracker/actracker-api/pull/189)

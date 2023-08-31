# Readings

## Optimizing Java. Practical Techniques for Improving JVM Application Performance (ISBN: 978-1-49-203927-3)

Chapter 4: Performance Testing Patterns and Antipatterns

- Performance tests often performed for a wrong reason - something is better than nothing.
- It's a common mistake to talk about performance tests in general - there are multiple different types based on the
  questions they are intended to answer.
- Large-scale benchmarking of Java applications is usually more effective than microbenchmarking.
- Performance of an application is not only dependent on the code but also infrastructure. Metrics must consider system
  as a whole.
- Particularly important Java-specific performance indicator is related to JIT compilation.
- Performance tests should be conducted consistently to precise goals set early in the planning phase.
- Bored developers can harm a project, unnecessary technologies are often added because of developers' boredom or resume
  padding.
- Competition between teams wanting to be seen as having high development velocity is harmful.
- Effective applications require being aware of full capacity of tools they use.
- Performance problems require deep investigations and measurements.
- Playing to the familiar rather than engaging with a new technology may cause negative impact on performance. However,
  introducing new tools without investigating current tools' capabilities is also an antipattern. Performance problems
  solving should start with measuring and understanding.
- Equally harmful is having a single person responsible for performance tuning (a Performance Wizard).
- Java performance happens in a specific context with a large number of factors, performance workarounds in one version
  of Java may not work in another.
- Certain components/libraries (like Hibernate) are commonly identified as performance bottlenecks, which is not always
  true.
- Premature tweaking JVM switches (perhaps based on an example of a different application in a company) without deep
  investigation of a problem can cause more damage than gain.
- User acceptance test environment must mirror production. The same applies to UAT datasets.
- Teams should manifest systematic approach to performance problems.
- It's essential to handle data in an appropriate manner to avoid falling into unscientific and subjective thinking.

# Projects

## ACTracker

- Postgres database was [created](https://github.com/marcinciapa/equino-kubernetes/pull/1) for actracker-search-feeder
- Database schema migration was [implemented](https://github.com/marcinciapa/actracker-search-feeder/pull/20)
  in actracker-search-feeder
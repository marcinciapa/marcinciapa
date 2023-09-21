# Readings

## Optimizing Java. Practical Techniques for Improving JVM Application Performance (ISBN: 978-1-49-203927-3)

Chapter 2: Microbenchmarking and Statistics

- Because of the dynamic nature of JVM, performance numbers are harder to handle than expected.
- When we are dealing with microbenchmarking it's harder to isolate application code from the background behavior of the
  runtime.
- While benchmarking, warming up the JVM should be taken into consideration. JIT compiler converts interpreted bytecode
  to optimized machine code. This brings us closer to how the system behaves in production.
- Another factor is the nondeterministic behavior of GC, which is hard to control.
- We should also consider hardware factors, like power management.
- Developers can be obsessed with efficiency of their own code and miss the bigger picture.
- Microbenchmarking should be used rarely, except three cases: developing general-purpose library, developing OpenJDK or
  other Java platform, developing extremely latency-sensitive code.
- It's easy to assume that microbenchmarking represents controlled environment, which is often not true.
- JMH is a framework for microbenchmarking, maintained by the same engineers who work on the JVM. This brings
  significant advantages of this tool: being able to control the compiler, simulating CPU usage levels. Another useful
  feature are blackholes consuming CPU cycles.
- There are two types of error during performance analysis: random errors, systematic errors. Higher accuracy means low
  systematic errors, higher precision means low random errors.
- Example of systematic error: on the response times in time we may observe a sudden peak in some moment of time. It
  usually represents memory, or some other resource leak. But we shouldn't assume it's the memory. We should perform a
  deeper analysis to identify leaking resource type.
- Random errors are often related to unpredictable changes in the environment. We may expect they are represented as
  normal distribution, which is not usually a case in terms of JVM. In Java performance, the outliers represent unhappy
  customers, we should put special attention to these points than to the experience of majority. To collect such
  information, we should start representing distributions starting with mean, than 90th percentile, then move out
  logarithmically: 99, 99.9, 99.99, 99.999 etc. Logarithmic percentile are useful for understanding the long tail.
- Understanding the results of measuring the application is the hardest part. Example: Response times distribution
  doesn't give us too much information about what's going on. It makes sense to break it down by response types (2xx,
  4xx, 5xx). Breaking down a general observables into sub-populations is a useful technique for understanding the
  general picture.

# Projects

## ACTracker

- Saving Activity entity into Postgres was [implemented](https://github.com/marcinciapa/actracker-search-feeder/pull/21)
  in actracker-search-feeder
- [Publishing](https://github.com/marcinciapa/actracker-api/pull/106)
  and [consuming](https://github.com/marcinciapa/actracker-search-feeder/pull/22) tag notification was implemented.
- Publishing TagSetChanged and DashboardChanged notifications
  was [implemented](https://github.com/marcinciapa/actracker-api/pull/107) in actracker-api.
- Consuming dashboard and tag set changed notification
  was [implemented](https://github.com/marcinciapa/actracker-search-feeder/pull/23).
- Building entity graphs for indexing was [implemented](https://github.com/marcinciapa/actracker-search-feeder/pull/25)
  in actracker-search-feeder.
- Entity processing was [separated](https://github.com/marcinciapa/actracker-search-feeder/pull/26) from entity indexing
  in actracker-search-feeder
- Children entity notifications on parent refreshed
  was [implemented](https://github.com/marcinciapa/actracker-search-feeder/pull/27) in actracker-search-feeder.
# Readings

## Optimizing Java. Practical Techniques for Improving JVM Application Performance (ISBN: 978-1-49-203927-3)

Chapter 14. High-Performance Logging and Messaging

- Using Java and JVM is considered as a tradeoff, comparing to C++. Java takes the approach that developer should not be
  concerned about low-level details. On the other hand, high level managed abstractions, like garbage collection
  introduce unpredictability in performance.
- Logging should be a concern of Java developer, as logging is an important part of application. Logs help with
  identifying production issues. It should contain enough information for teams to successfully investigate a
  problem after the fact. Logging systems require careful control and inclusion.
- Messaging systems are often in front of low-latency systems. They're typically measured in number of processed
  messages in time period. It is often a critical metric how many messages is the system capable of processing.
- There are common anti-patterns related to logging systems: copying the configuration created by someone long time ago,
  using project-wide or company-wide logger.
- Logger frameworks compared in terms of performance behave differently, also depending on the used logging format. Util
  logging offers poor performance. Log4j format gives consistent results. Logback offers good logging times.
- Performance considerations related to logging not only consider time taken, but also things like: the amount of
  generated garbage, CPU cycles spent.
- Log4j 2.6 introduced a steady-state garbage-free logger. However, there are some limitations for using zero-allocation
  logger. It works by reusing objects rather than creating them. Log4j 2.6 uses ThreadLocal for this purpose.
  ThreadLocal objects, however, are problematic in web containers. For that reason Log4j doesn't use ThreadLocal when
  running within a web container. Also, SLF4J requires not using a garbage-free approach.

## Modern Software Engineering: Doing What Works to Build Better Software Faster (ISBN: 978-0-13-731491-1)

# Projects

## ACTracker

# Tutorials

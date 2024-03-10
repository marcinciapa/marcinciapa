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

Chapter 15. Java 9 and the Future

- Java 9 introduced such changes as code modularisation, updated G1 as a default Garbage Collector. A significant change
  was introducing separate regions of code cache. Access to _Unsafe_ was constrained.
- The most prominent changes in Java ecosystem in the future should be related to value types (project Valhalla) and
  multithreading (project Loom).
- With __Graal__ as C2 compiler and __Truffle__ as a framework for developing interpreters for JVM, it's possible to
  achieve Ahead of Time compilation.
- In Java 21 ZGC was significantly improved, currently offering generational approach to garbage collection.

## Modern Software Engineering: Doing What Works to Build Better Software Faster (ISBN: 978-0-13-731491-1)

5 Feedback

- Feedback is the transmission of corrective information about actions, events, processes, to the original or
  controlling source. Lack of feedback means no opportunity to learn, we can only guess. Feedback allows having an
  evidence of our decisions.
- Approaches similar to waterfall assume relying on predictions being perfect.
- Test-driven development is an example of feedback in coding. First we get feedback about correctness of tests (we see
  them failing). Later, every time we make changes, we can rerun tests to confirm that the code is still working.
- Continuous Integration allows collecting feedback if changes are correct in the context of other changes. CI assumes
  incremental, frequent changes to the system, in contrary to branching, which is about isolating changes. Branching and
  CI are not compatible with each other. CI helps to resolve "merge-hell" problem. CI allows receiving small, frequent
  feedback.
- TDD provides also a possibility to receive feedback about design: when tests are hard to write, the quality of the
  code may be questionable. An ability to create simple, effective tests and the effectiveness of a design are related.
  In the absence of TDD the quality and correctness of code relies on experience and skills of a developer.
- In classical approach to software development, testing is planned for the end of a project. In such situation the
  feedback is slow, which makes it useless. The development team has already delivered the project and handed it over to
  the maintenance team.
- To collect valuable feedback for architecture, we should choose between building monolithic systems optimized for
  testability or modularize them into separate deployable units. Continuous Delivery favors modular, more loosely
  coupled designs.
- Software developers are not paid to make good quality code, but to deliver a value for the organization. We don't have
  information about how valuable our features are until we get feedback from customers. Thanks to that we can validate
  if our ideas work, adapt to better meet customers' needs. Adding telemetry to our systems allows gathering data about
  which features, and how, are used.
- From organization's perspective it's important how to measure success and improvement. Agile culture assumes observing
  the results, reflecting on them, improving over time. The quality of feedback is specific about its nature. We should
  be clear where we are now, where do we want to be, move forward and validate if we are closer to our goals. Such
  approach means application of the scientific method.
- Speed and quality of feedback matters. If the feedback is late, it may be useless. If it is misleading or wrong, our
  decisions will also be wrong.

6 Incrementalism

- Incrementalism means that components can be replaced when improved to ensure better performance. It's building value
  progressively, making use of modularity. As working iteratively means refining and improving over a series of
  iterations, working incrementally means building piece by piece. Creating complex systems requires both approaches. An
  incremental approach allows decomposing work and delivering step-by-step.
- Incrementalism is closely related to modularity. It's easier to divide problem into independently developed pieces,
  when each component of the system is simpler than a whole. Each component is easier to test, faster to deploy,
  sometimes independently on the others. Such mindset is required to operate on microservices, but microservices is not
  the only way. When dividing, boundaries are important, as they represent the key points of coupling in the system.
- Isolation is the most important value of modularity: internal details of one module are hidden from other modules.
  Teams work more independently, they make small steps forward without a need for coordination. A common, not
  necessarily good, way to manage teams is to try to standardize processes across an organization. But creative work
  depends on human creativity. To leverage it, organizations need to leave room in processes and policies. It's
  important to define characteristics of high-performing teams and leave them space to make progress and change their
  minds without asking for permission. The most effective strategy is to create many small, independent teams and give
  them freedom to make their own changes. Distributed, incremental change is the key.
- The tools enabling incrementalism are __feedback__ and __experimentation__. It also requires focusing on
  __modularity__ and __separation of concerns__. It's necessary to design system in a way allowing limiting the scope of
  changes. __Refactoring__ helps to make changes in often, tiny increments. __Version control__ allows to always
  withdraw to a position of stability. __Testing__ ensures protection to move forward incrementally. It is also an
  ability to create mini executable specifications for changes.
- The goal is to manage complexity with these techniques by making progress in small, frequent steps rather than large,
  riskier steps. We can improve the speed and quality of feedback using __Continuous Integration__ and __Continuous
  Delivery__. It allows resolving problems we created maybe before it was even noticed. Another powerful technique is
  __Ports and Adapters__ pattern. All of these strategies limit the impact of change.
- Working incrementally is fundamental to build complex systems. Such approach questions the mindset that systems can
  be fully designed in minds of experts. A good design is the result of work and gradual apply of knowledge and
  understanding, as we make progress.

# Projects

## ACTracker

# Tutorials

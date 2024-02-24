# Readings

## Optimizing Java. Practical Techniques for Improving JVM Application Performance (ISBN: 978-1-49-203927-3)

Chapter 13. Profiling

- There are two different approaches to profiling: execution and allocation. Profilers should be used to identify code
  which is a candidate for refactoring and performance optimization.
- Using profilers provide a risk of incorrect interpretation of application's behavior and exhibit biases. To reduce
  the risk it's recommended to use multiple profiling tools.
- Profilers work by using low-level instrumentation, usually as a JVM agent or a component which dynamically attaches to
  a running JVM. Agents may be used for different purposes: monitoring (observing the current state), alerting
  (detecting anomalies), profilers (detailed information about running applications).
- Before starting using profiling, it should be confirmed that the application is consuming close to 100% of CPU.
  Otherwise, other techniques should be explored. It should also be confirmed that the CPU consumption is not caused by
  Stop the World phases of GC (GC log).
- Profiling usually use sampling to get data, there is a cost related to measurements. The sampling interval is a
  tradeoff between cost and accuracy. Sampling mostly is only performed at safepoints.
- A complementary approach to monitoring state is using operational monitoring tools, visualising the current state of
  the system and validating if the system is operating normally.
- Allocation profiling (memory profiling) verifies the allocation behavior of the application. A simple instrumentation
  approach is inserting a call to a static method which logs the allocation next to all identified allocation opcodes. It
  requires bytecode manipulation, which can be done using ASM library. A __class rewriter__ is a bridge between the
  instrumentation API and ASM. Per-thread counts of how much memory has been allocated is maintained. Such approach
  leads to a large amount of extra calls, which has a significant effect on performance. An alternative approach is
  __TLAB exhaustion__. This approach monitors TLAB allocations using HotSpot-specific callbacks. This approach is based
  on sampling, which means it may not be accurate. The intent is identifying the top allocation sources.
- Heap dump analysis is a technique related to allocation profiling. The examples of data points are numbers and types
  of objects, structure of the object graph. It allows identifying live objects and dead objects not yet collected. The
  size of heap dumps may have 300-400% of the dumped memory size. Creating a heap dump requires STW event.

## Modern Software Engineering: Doing What Works to Build Better Software Faster (ISBN: 978-0-13-731491-1)

2 What is Engineering?

- Software isn't bridge building, but there are similarities. The source of confusion is the misconception about bridge
  building. The most important discussion should be between production engineering and design engineering.
- Waterfall is an equivalent of inventing production lines for software. But mass production is not a good pattern for
  software. In heart of software engineering should be discovery, learning and experimentation.
- The advantage of software over any other engineering professions is the ability of iterating over the outcome. It's
  impossible to quickly iterate over a product which is physical. In software our computer simulations of a problem
  __is__ our product, it's easier to change. However, despite this, scientific approach to software development is not
  common.
- Formal methods are not practical in the area of software development. Creating software is complex enough, creating
  code which proves itself correct is even harder. Mathematical approach is limiting. Math doesn't replace testing and
  experimentation. We should apply mathematical thinking when suitable, but take data-driven, pragmatic, experimental
  approach to learning. In comparison to other engineering areas, testing software is testing our product itself, not
  the simulation of it.
- If our engineering practices don't allow us to move faster, they don't qualify as engineering. We should use our
  experience, empirical learning, discovery and imagining how things could possibly go wrong. While creating a system,
  it should run first, even if it doesn't do anything useful.
- Engineering is applied science. While thinking about software engineering, we should consider everything what
  influences making software: process, tools, culture.
- There are differences between craft and engineering, in context of software it's __precision__ and __scalability__.
  Approaches relaying on human capabilities are limited to human capabilities. An engineer can create machines which
  create smaller and more precise things comparing to a craftsman. Craft-based solutions are not scalable to the same
  level as engineering-based solutions.
- The fundamental nature of software engineering is that it's not about production, it's about design. We should pursue
  to improve our management of complexity, including measuring with a certain level of precision. We should use accurate
  measurements to prevent problems upfront instead of waiting for bad things to happen to diagnose a problem.
- Software craftsmanship is needed, but it's not enough. It has limitations. Focus on skill, creativity, freedom to
  innovate, apprentice schemes is important, but it's not limited to craftsmanship. Engineers should make use of all of
  these qualities. The motivation for engineers should be to find elegant, efficient solutions to complex problems.
- Engineering decisions should base on rational criteria (like strength at a certain temperature, economic factor). But
  they should still be experimental, iterative, empirical. As engineers, we should make decisions based on evidence,
  then test the ideas and check if they work.
- Engineering means constant optimizations and trade-offs: system more secure or easier to use, more distributed or
  integrating quicker, more people working on a project to speed development up or reducing the overhead of
  communications.
- In the era of serverless computing we should probably focus on denormalized data stores and eventual consistency
  rather than minimizing storage with normalized data. Such approach improves modularity.

3 Fundamentals of an Engineering Approach

- Engineering is grounded in science and taking a pragmatic, empirical approach. Sometimes we get excited about new
  technologies, but they usually don't make things much different. We have problems with learning new ideas, we have
  bigger problem with discarding old ideas. And this should be our primary focus.
- It's important to measure performance of software development, but we struggle with measuring it effectively.
  Effective measurements don't track productivity but evaluate effectiveness in terms of __stability__ and
  __throughput__. Stability can be tracked by __Change Failure Rate__ (introducing defects) and
  __Recovery Failure Time__. Throughput can be tracked by __Lead Time__ (how long does it take to change an idea into
  working software) and __Frequency__ (how often changed are deployed to production). These measurements don't say if we
  are building the right things, but if we are building things right. When making any decision, we should ask ourselves
  how they influence the quality of our software (stability) and the efficiency we create it (throughput).
- Speed of delivery doesn't clash with quality. High-quality software leads to speed. Feedback leads to high quality
  software. Good engineering leads to both: speed and quality.
- Introducing Change Advisory Boards sounds like a good idea. But the experience teaches us that they don't work. The
  way to discovery it was measurement.
- We need to make hard decisions. Sometimes ideas reduce throughput but increase stability.
- Our discipline should focus on two competencies: we should be __experts at learning__, and we should be
  __experts at managing complexity__. We should improve our skills in managing complexity, focusing on both: technical
  and organizational level. Technically difficult things, like concurrency and coupling, are difficult in the world of
  people too. It's important to manage complexity of the organizational information systems as well as technical
  software information system. As engineers, we pay too little attention to it.
- We tend to overestimate our abilities to solve problems in code. We should assume our ideas are wrong. We should have
  in mind the potential growth of complexity in systems we create and manage it with care.

# Projects

## ACTracker

- Application services instrumentation was [introduced](https://github.com/marcinciapa/actracker-api/pull/169).

# Tutorials

[Spring Security tutorial](https://github.com/marcinciapa/tutorials/pull/32)
[Spring Boot 3 + Spring Security 6 tutorial](https://github.com/marcinciapa/tutorials/pull/33)
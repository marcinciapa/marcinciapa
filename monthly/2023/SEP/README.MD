# Readings

## Optimizing Java. Practical Techniques for Improving JVM Application Performance (ISBN: 978-1-49-203927-3)

Chapter 5: Microbenchmarking and Statistics

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

Chapter 6: Understanding Garbage Collection

- In Java there is no language way to control memory allocation/garbage collection.
- There are two fundamental rules of garbage collection: GC must collect all garbage, no living objects must be
  collected.
- The most iconic garbage collection technique is **Mark and sweep**, however it's not the way how JVM actually works.
  With this technique a list of pointers to each object is maintained to allocated objects. The algorithm loops through
  these objects, marking those, which were reached. Then another loop goes through objects and reclaims those, which
  were not marked.
- HotSpot doesn't use system calls to manage the heap. Instead it reserves space in the users memory space upfront and
  manages it by itself.
- The **allocation rate** is the amount of memory used by newly created object in a time period. **Object lifetime** is
  more fundamental characteristic. GC must balance tradeoffs driven by lifetime and allocation.
- **Weak Generational Hypothesis** claims that the majority of object are short-lived while another population lives
  much longer. Taking it into consideration heap should be structured to support quick collection of short-lived objects
  and maintained long-lived objects. To achieve it HotSpot tracks the number of collection each object survived, creates
  new objects in **Eden** space, maintains **Tenured** generation in a separate area of memory for objects which
  survived long enough to be considered long-lived. It also tracks pointers to young generation objects to ensure quick
  GC cycles without traversing the entire objects graph.
- "Old" and "Young" generation areas is a historical division, G1 has different approach to heap layout.
- Eden is critical region to manage efficiently: short-lived objects will never be allocated anywhere else.
- Eden space is partitioned into buffers for each application thread (thread-local allocation buffers - **TLAB**s).
- Objects which survived at least one GC (but are not long-lived) are moved to **Survivor** space. It contains of two
  equally sized memory areas, one of them is always empty, the second accepts objects coming from Eden space during GC.
  During collection of the young generation, living objects from Eden and non-empty survivor space are moved to the
  empty one, and the collected half is being emptied. Objects are evacuated from Survivor space to Tenured when Survivor
  free space is not large enough to accept new objects from Eden space. 
- VisualGC plugin for VisualVM is a useful initial GC debugging tool. However, GC logs contain more useful information
  and allow a much deeper analysis of GC.
- **Parallel collectors** are default for JVM in Java 8 and before. After stopping all application threads, they consume
  all available CPU cores to collect memory (**Stop-the-world**).
- Young generational collection occurs when a thread tries to allocate an object in Eden, but doesn't have enough space,
  and JVM can't allocate a fresh TLAB. Live objects from Eden and non-empty survivor space are evacuated to empty
  survivor space, then Eden and evacuated survivor space are marked as empty.
- ParallelOld is the default collector for Old generation in Java 8. It's compacting a single continuous memory space,
  so it must relocate object within old generation to not suffer from memory fragmentation. It results with efficient
  memory layout, but consumes large amount of CPU during full GC cycles.
- Parallel collectors deal with the entire contents of a generation at once, they are stop-the-world collectors. It's
  not an issue for young generation, because usually very few objects should survive. Dead objects are never touched (
  only live objects being evacuated), so the marking phase is proportional to the number of surviving objects. Pauses of
  young collections are short, typically for 2GB of heap less than 10ms.
- However, for old generation, which is ~7 times larger than young, marking time is proportional to the number of live
  objects, potentially large number of old objects may survive. Stop the world time scales linearly with the size of
  heap.
- TLABs are private for a single threads, but only at the point of allocation. Creating new threads complicates garbage
  collection process.
- Garbage collection process is triggered when there is no enough memory for requested allocation, so it's not
  deterministic and is irregular.
- Allocation is critical. When the size of surviving objects in Eden is larger than the survivor space, JVM prematurely
  promotes them into Tenured (there is no other choice). If the allocation rate is too high, objects will end up being
  forced to be promoted early.

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
- Children entity reindex on parent refresh
  was [implemented](https://github.com/marcinciapa/actracker-search-feeder/pull/29) in actracker-search-feeder.
- Entity processed notification publishing
  was [implemented](https://github.com/marcinciapa/actracker-search-feeder/pull/30) in actracker-search-feeder.
- Entity processed notification consumption
  was [implemented](https://github.com/marcinciapa/actracker-search-feeder/pull/32) in actracker-search-feeder.
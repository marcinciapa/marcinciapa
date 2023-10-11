# Readings

## Optimizing Java. Practical Techniques for Improving JVM Application Performance (ISBN: 978-1-49-203927-3)

Chapter 7. Advanced Garbage Collection

- GC considerations
    - The mostly used garbage collectors in Java > 8 are **CMS** and **G1**. The variety of garbage collectors allows to
      pick one most suitable for application needs, as different applications may benefit from different concerns: pause
      times, throughput, pause frequency, reclamation efficiency, pause consistency. There is no algorithm suitable for
      every workflow. E.g. big data applications require high throughput.
    - Parallel collectors reduce pause times, but also reduce the processing power for the application.
    - Operating system can remove threads from cores (when a thread has exhausted its timeslice or put itself into a
      _wait()_). But JVM itself also needs to coordinate some actions between running threads. Each application threads
      define **safepoints**, where threads' internal data structures are in a known-good state. Such barriers must also
      be inserted into the generated machine code. When reaching safepoints (for example another iteration of a loop),
      quickly stopping threads must wait for slower stoppers.
- CMS (Concurrent Mark and Sweep)
    - It's an extremely low-pause collector for Tenured generation. It tries to do as much work as possible while
      application threads are still running. Marking uses tri-color technique.
    - Phases: **Initial Mark** provides a stable set of starting points for GC. **Concurrent Mark** phase keeps track of
      any changes that might later require fixup, as application threads are still running. It uses tri-color marking.
      **Concurrent Preclean** reduces the length of Stop The World remark phase. **Remark** uses the card tables to fix
      up marking.
    - While using CMS application threads don't stop for as long. But the full GC takes longer. Application throughput
      is reduced, more CPU is needed. Also, CMS doesn't compact the heap, Tenured region can become fragmented.
    - CMS runs mostly concurrently with application threads, by default it uses half of the available threads.
    - Eden may fill up while CMS is running. Cleaning young GC is fully STW, but it usually takes longer than with
      ParallelOld collector, as there are only half of cores available.
    - Some objects may be promoted to Tenured while CMS is still running, which requires some coordination. Sometimes
      young collections have too many objects to promote while CMS hasn't finished yet. It leads to **concurrent mode
      failures** (CMF). In such situation collecting Tenured falls back to ParallelOld collector. To prevent it, CMS
      starts collection cycles before Tenured is full (by default at 75%).
    - CMF may also occur due to heap fragmentation, when objects can't be promoted into Tenured due to a lack of
      sufficient contiguous space. In such cases CMS also falls back to ParallelOld. To reduce this risk CMS uses a free
      list of chunks of memory. Sweeper and the allocator threads are synchronized, so sweeper can lock the free lists
      while it is sweeping.
- G1 (Garbage First)
    - Offers a better pause times on larger heaps. It rethinks the concept of generation: does not maintain dedicated,
      contiguous memory spaces per generation, but introduces the concept of regions: default 1MB in size. The overall
      G1 heap is contiguous, but generations not necessarily (different fragments of particular generations across the
      heap layout). The advantage of such approach is that it's not required to collect all garbage per generation on
      each run.
    - While warming up, the collector keeps track of the statistics of how many regions can be collected per GC.
    - Concepts of TLAB allocation, evacuation to survivor space, promoting to Tenured are similar to other HotSpot GCs.
    - Objects which occupy more space than half a region space are called **humongous** and are allocated in special
      humongous regions being part of Tenured generation rather than Eden.
    - The size of young generation is adaptive and based on the overall pause time goal.
    - To help with region tracking, G1 maintains **Remembered Sets** (RSets), which are per-region entries tracking
      outside references. Instead of tracking the entire heap, G1 examines the RSets and then scan those regions for
      references. RSets and card tables prevent the problem of **floating garbage**, when dead objects are kept alive by
      references from dead objects.
    - Phases: **Concurrent Root Scan** is a concurrent phase scanning survivor regions of the **Initial Mark** for
      references to the old generation. **Remark** performs reference processing as well as completing the marking
      cycle. **Cleanup** is mostly STW, including accounting and RSet scrubbing. Accounting identifies regions that are
      now completely free and ready for reuse as Eden regions.
    - G1 is based around the **pause goals**: developer can specify the desired maximum amount of pause time, but there
      is no guarantee it will be achieved. G1 is not as low-pause collector as CMS.
- Shenandoah (external, by Red Hat)
    - Brings down pause times, especially for large heaps by performing concurrent compaction. It's based on **Brooks
      pointers**: additional word of memory per object indicating whether the object has been relocated during a
      previous GC phase. This mechanics relies upon the availability of hardware compare-and-swap operation.
    - Phases: **Concurrent Marking** traces through the heap marking any live object. If its reference points to an oop
      that has a forwarding pointer, then the reference is updated to point directly at the new oop location. **Final
      Marking** phase (STW) rescans the root set, then copies and updates roots to the evacuated copies. **Evacuation**
      (running concurrently with application threads) makes use of Brooks pointers to keep track of objects which
      could be relocated.
    - Because Shenandoah is a concurrent collector, garbage is being created by the application threads. Collection has
      to keep up with allocation.
- C4 (Continuously Concurrent Compacting Collector, available in Azul Zing JDK)
    - Zing was designed for 64-bit machines from the beginning. Object header is a single 64-bit word. It introduces
      **The Loaded Value Barrier** to provide a solution where loaded references are pointing directly at the current
      location of the object as soon as the load has completed. The application thread updates the reference just after
      the object has been relocated by the collector.
    - Phases: **Mark**, **Relocate**, which focuses on the sparsets from-pages, **Remap**.
    - C4 takes benefit of disconnecting between physical and virtual memory addresses, maintains the mapping between a
      virtual page in the process address space and an underlying physical page. Unlike HotSpot, Zing does make use of
      calls into the kernel as part of its GC cycle.
    - During evacuation, objects are relocated by being copied to a different page. After copying all objects from a
      page, the physical page can be freed and returned to the operating system.
    - C4 runs two collections at all times: one for young, one for old objects.
    - There are benefits of moving to Zing and C4, but the decision should be based on measurements, not guessing.
- Balanced (available in IBM J9 JVM)
    - It's designed for large heaps in excess of 4GB. Balanced improves scaling of pause times on large heaps and
      minimizes worst-case pause times.
    - Similarly go G1, heap is split into several regions, managed and collected independently. Each region has an
      associated age (age-zero regions are Eden). When Eden space is full, collection must be performed (**partial
      garbage collection** - PGC). PGC is a STW operation which collects all Eden regions and optionally regions with a
      higher range, if determined by the collector they're worth collecting. The age of regions containing the surviving
      objects is incremented. Downside: PGC only has visibility of the regions it has chosen to collect causing floating
      garbage problem. To resolve it, **global mark phase** (GMP) phase is introduced, which scans the entire Java heap
      marking dead objects. The final operation is **global garbage collection** (GGC), which is full STW collection
      compacting the heap.
    - For region-based collectors it's possible to allocate an array object exceeding the size of a single region.
      Balanced represents arrays as **arraylets** allowing allocating them in discontiguous chunks. This is the only
      circumstance under which heap objects can span regions and it is done transparently by the JVM.
    - Performing partial GCs on regions reduces average pause time, but overall time spent on GC may be higher because
      of the overhead of maintaining information about regions of a referrer and referent. Balanced is suited to
      applications where avoiding large pauses is more important than high throughput.
    - **Non-uniform memory access** (NUMA) is a memory technique used in multiprocessor system. Processors and memory
      are arranged into nodes. A processor may access memory from other nodes, but access time is faster for memory
      belonging to the same node. Balanced collector can split the Java heap across nodes favoring execution on a
      particular node. Allocations favor regions in memory local to that node. GC will attempt to move objects closer to
      objects and threads referring them.

# Projects

## ACTracker

- Connecting to Elasticsearch cluster and creating the first index
  was [implemented](https://github.com/marcinciapa/actracker-search-feeder/pull/33) in actracker-search-feeder.
- Versioning and aliasing Elasticsearch index
  was [implemented](https://github.com/marcinciapa/actracker-search-feeder/pull/34) in actracker-search-feeder.
- Deleting decommissioned indices was [implemented](https://github.com/marcinciapa/actracker-search-feeder/pull/35) in
  actracker-search-feeder.

# Tutorials

- Spring Data Elasticsearch [tutorial](https://github.com/marcinciapa/tutorials/pull/6)
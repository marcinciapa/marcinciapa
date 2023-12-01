# Readings

## Optimizing Java. Practical Techniques for Improving JVM Application Performance (ISBN: 978-1-49-203927-3)

Chapter 9. Code execution on the JVM

- JVM must provide mechanics for memory management and a container for execution of application code. VMSpec specifies
  execution of Java bytecode in terms of an interpreter. But most production ready JVMs provide dynamic compilation
  capability. This is known as **Just-in-Time** compilation (JIT). JVM monitors frequency of methods and determines
  which of them should be compiled to the machine code.
- JVM operates as a stack machine. It doesn't use registers to store data and perform computation. All values are placed
  on the evaluation stack instead. Other areas to hold data are: local variables and the object heap.
- Machine operation code in JVM is represented by 1 byte. Instructions are grouped in families: dedicated instructions
  for primitive types and reference type (_dstore_ stores double on top of the stack, while _astore_ stores a reference
  type). Some opcode families have shortcut forms (_aload_0_ puts _this_ on top of the stack), allowing reducing class
  files size.
- Opcodes are divided into categories:
    - **load and store** category responsible for moving data on and off the stack (like loading from the classes
      constant pool or storing on the stack an object from the heap). _ldc_ loads constants from the constant pool 
      (strings, primitive constants), while _const_ loads specific predefined constants, like _aconst_null_ or
      _dconst_0_.
    - **Arithmetic bytecodes** operating on primitive types, operating purely on stack.
    - **Flow control** bytecodes responsible for looping and branching (handling instructions like _if_, _for_,
      _while_).
    - **Method invocation** bytecodes responsible for transferring control to different method. Instance method calls
      are translated to _invokevirtual_, when receiver object is known to be an interface - _invokeinterface_, when
      method dispatch is known at compile time (private methods or superclass calls) - _invokespecial_. _invokedynamic_
      is broadly used in Java 8 lambdas.
    - **Platform bytecodes** responsible for allocating new heap space or handling intrinsic locks.
- Between bytecodes is a good place to hold an application threads, so it's a good candidate for safepoint. Equivalent
  barriers must be put into machine code.
- One can think of a simple interpreter as a _switch_ statement inside a _while_ infinite loop, where _case_
  instructions handle particular opcodes.
- It's easy to think, that final methods should be converted to _invokespecial_ opcode. But the VMSpec says, that making
  a method final should not break compatibility. If _invokespecial_ was used, wrong implementation of the method would
  be called. This violates Liscov Substitution Principle, which states, that instance of a subclass can be used anywhere
  that a superclass is expected. Final methods are then represented as _invokevirtual_ with HotSpot dedicated code for
  dispatching final methods.
- Ahead of Time compilation (AOT) gives a single opportunity to perform optimization. Compiler produces an executable
  targeted for a single platform. Only compilation benefits from processor-specific features.
- Just in Time compilation (JIT) produces highly optimized machine code at runtime. It recognizes frequently used code
  to perform compilation (profile-guided optimization, PGO). JIT mechanics use VM resources, it's a cost of profiling,
  which is paid at runtime. PGO results also in JVM warmup.
- If a method is invoked frequently enough, compilation thread is started.
- AOT has simple and predictable performance characteristics. But it doesn't get access to runtime information, which
  could help making optimization decisions. The result is an executable compatible with only single processor, which is
  effective for extreme performance use cases. When multiple architectures are supported, a separate executables must be
  created for each of them. HotSpot, on the other hand, supports new processor features without a need to recompile the
  code.
- The basic unit of compilation is a whole method, meaning a single method is compiled at once.
- From operating system's perspective, JVM is a multithreaded C++ application. Even single-threaded application code is
  running alongside VM threads: JIT compilation, profiling.
- When the code is recompiled, the entry in vtable of klass is updated to the new code.
- There is a JVM switch allowing gaining access to compilation events log: _PrintCompilation_. Except the application
  code, standard libraries are also eligible for compilation and entries for them will be visible in the compilation
  log.
- HotSpot uses two compilers: C1 (client compiler) and C2 (server compiler). Both compilers are triggered basing on the
  method invocation count. When it's acquired, VM is notified and is allowed to queue the method for compilation.
- C1 and C2 produce different results: C1 performs simpler and shorter compilation. C1 doesn't optimize as fully as C2.
  There is a concept of **tiered compilation**: code is running in the interpreted mode until the simple C1 compilation
  result is available. If needed, the code is then compiled by C2 with more advanced optimizations.
- Compiled code is stored in **code cache** together with other native code. When the cache fills up, no further JIT
  compilation is possible and the code is executed only in the interpreter. When the native code is removed (in some
  circumstanced), a block of cache is returned to the free list. When a native method is to be cached, JVM searches for
  a large enough block in free list. If not found it creates a new block. Code cache size can be controlled with the VM
  switch.
- Code cache may become fragmented if no compaction is applied. Java 9 and later prevents fragmentation.
- The tuning of JIT compilation has an important rule: for any method selected for compilation, resources should be
  available. A simple technique is increasing the size of code cache. If the number of compiled methods doesn't
  increase, JIT compiler doesn't have problem with resources.

# Projects

## ACTracker

- JPA basic integration tests were [added](https://github.com/marcinciapa/actracker-api/pull/115) to actracker-api.
- Docker integration in Jenkins was [added](https://github.com/marcinciapa/equino-kubernetes/pull/3). Integration tests
  were [enabled](https://github.com/marcinciapa/actracker-api/pull/116) in actracker-api CI pipelines.
- Tag Set JPA data source integration tests were created in two
  parts: [part1](https://github.com/marcinciapa/actracker-api/pull/117), [part2](https://github.com/marcinciapa/actracker-api/pull/118)
  in actracker-api.
- ActivityDataSource (CQRS) was [added](https://github.com/marcinciapa/actracker-api/pull/120) to actracker-api.
- TagDataSource (CQRS) was implemented in actracker-api in two parts:
  [part1](https://github.com/marcinciapa/actracker-api/pull/123), [part2](https://github.com/marcinciapa/actracker-api/pull/125).
- DashboardDataSource (CQRS) was implemented in actracker-api in two
  parts: [part1](https://github.com/marcinciapa/actracker-api/pull/126), [part2](https://github.com/marcinciapa/actracker-api/pull/128)
- Finding tags by IDs was [moved](https://github.com/marcinciapa/actracker-api/pull/132) from Repository to Data
  Source in actracker-api (CQRS).
- Finding activities to switch was [moved](https://github.com/marcinciapa/actracker-api/pull/133) from Repository to
  Data Source in actracker-api (CQRS).
- Extracting Users was [moved](https://github.com/marcinciapa/actracker-api/pull/136) from Repository to Data Source in
  actracker-api
  (CQRS).
- Notification data source was [introduced](https://github.com/marcinciapa/actracker-api/pull/137) in actracker-api
  (CQRS).

# Tutorials

- Spring Boot Kafka [tutorial](https://github.com/marcinciapa/tutorials/pull/8)

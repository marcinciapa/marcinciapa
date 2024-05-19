# Readings

## Modern Software Engineering: Doing What Works to Build Better Software Faster (ISBN: 978-0-13-731491-1)

13 Managing Coupling

- Coupling is one of the most important aspect of managing complexity. It's defined as a degree of interdependence
  between modules. It measures how closely connected they are. If modules are fully decoupled, they can't communicate
  between each other. This may, or may not, be helpful. Coupling cannot, and should not be fully eliminated. But usually
  the cost of too loose coupling is lower than the cost of too tight coupling. Correct assessment is a trade-off.
- Coupling impacts an ability to create software reliably and repeatably. It should be well managed in systems and
  organisations, otherwise it impacts creating software at scale. On the organisational level it prevents effective
  releases. Modularity, cohesion, abstraction, separation of concerns reduce the coupling.
- There is an impact of coupling on scaling up development. There should be a limit of team size. Teams are
  __developmentally coupled__, releases need to be coordinated. __Continuous integration__ is a solution, it scales the
  development very well, but it requires an investment in automation, testing infrastructure to get feedback.
- Another strategy is __microservices__, but it's more complex and sophisticated. The approach is not new, it's been
  used in the past, microservices put these practices together. Good microservices should be aligned with a bounded
  context and be independently deployable (build, test, deploy them independently). We should not care which versions
  are deployed around, we should not test them together. Services should be cohesive. Microservices is an
  organizational-scaling pattern. If there is no need to scale up, there is no need for microservices. They help
  decoupling teams. They isolate code from changes in other places. It's a cost to take.
- Decoupling usually means writing more code. But it's not a good practice to optimise for typing, readability should be
  fundamental, we should optimise for thinking. High performance is achieved with simple, efficient predictable code
  understood by compilers and hardware.
- Abstraction and decoupling may be taken too far (universal database schema accepting all formats of data).
- DRY is efficient in the context of function or module, but having a single representation of one idea across the
  entire system increases coupling. Dependency management increases coupling. DRY should be avoided between pipelines.
  Code should not be shared between microservices.
- Synchrony is an illusion, it causes leaking abstractions across process boundaries. It increases complexity by
  dealing with errors. Network is asynchronous by nature. Processes should communicate asynchronously.
- Testable code provides a pressure on design to create more loosely coupled systems.
- It's possible to learn coding in few hours. Professional programming is about creating solutions to problems. It's
  hard when systems' complexity increases, coupling takes effect. There are two strategies to deal with coupling in
  organisations: __coordinated__ or __distributed__. Coordinated increases consistency. But coordination is a
  significant effort. Distributed approach is more in favor currently (microservices). Decision are intentionally
  distributed, teams work independently. Cost increases in terms of design. Avoiding testing services together later in
  the process reduces organisational coupling. Organisations which take microservices approach loose control
  consciously, otherwise microservices don't make sense.

## SQL Performance Explained (ISBN: 978-3-95-030782-5)

Chapter 2: Performance and Scalability

- In the context of databases, scalability is the ability to handle growing amount of work, or its ability to be
  enlarged. These are two separate definitions. But both are about performance impact of environmental changes.
- The amount of data stored has impact on database's performance: with more data, queries become slower. When comparing
  performance of two indexes, the reason may not be visible in the execution plan. Index may be slow for two
  reasons: table access and scanning a wide index range. Execution plans show the scanned index range in the predicate
  information. Without it, an execution plan is incomplete. Such situation may occur when two columns used in conditions
  are not the first column in the index. Only when condition columns are on the first indexed positions, both can be
  used as access predicates.
- Optimizer uses an index because it's more efficient than a full table scan, it doesn't mean it is the best index for
  query. Access rate also has impact on scalability: even a full copy of production database in dev environment may not
  simulate production conditions because of the production background load. More capable production hardware usually
  doesn't provide better performance. Bigger hardware is more like a wider highway than a faster car, it doesn't
  automatically improve slow queries. Multicore computing doesn't improve the performance of a single task. The best way
  to speed up queries (both in relational and non-relational databases) is proper indexing. It allows using the
  logarithmic scalability of the B-tree index. NoSQL systems target to solve performance problems with horizontal
  scalability, usually limited to write operations, using eventual consistency model.
- Strict consistency requires synchronous communication during write operations, which increases response times and
  reduces availability (all members must be available on write). In a distributed environment consistency is achieved
  with tho-phase commits (global transactions), which is possible only if all members are available. Using eventual
  consistency leads to handling conflicts rather than preventing them.
- Performance problems may also be caused by disc access. In case of HDD the access time is long (caching reduces this
  problem). SSD improves performance, but joins are still causing slow querying. SSDs, due to the cost and reduced
  lifetime are not commonly used in databases. But databases cache frequently accessed data in memory, like index root
  nodes or entire indexes, when they are frequently used.

# Tutorials

- Professional Full Stack Developer: [CRUD - Update (Exercise)](https://github.com/marcinciapa/tutorials/pull/59)
- Professional Full Stack Developer: [Datasources, JDBC & Flyway](https://github.com/marcinciapa/tutorials/pull/60)
- Professional Full Stack Developer: [Flyway](https://github.com/marcinciapa/tutorials/pull/61)
- Professional Full Stack Developer: [JDBC Template](https://github.com/marcinciapa/tutorials/pull/62)
- Professional Full Stack Developer: [Testing - Setup](https://github.com/marcinciapa/tutorials/pull/63)
- Professional Full Stack Developer: [Testing JDBC Template](https://github.com/marcinciapa/tutorials/pull/64)
- Professional Full Stack Developer: [Testing JPA Repositories](https://github.com/marcinciapa/tutorials/pull/65)

# Readings

## Modern Software Engineering: Doing What Works to Build Better Software Faster (ISBN: 978-0-13-731491-1)

9 Modularity

- Modularity is the degree to which components of a system can be separated to benefit from flexibility.
- Modern software is complicated, exceeding human's capacity of keeping all details in mind. Thanks to modularity
  components may be reused in a variety of contexts. Each module needs to have an interface to communicate with other
  modules.
- Achieving good level of modularity requires skill and experience. We need to work on it, as good design is what
  software engineering is really about.
- The code with good modularity and separation of concerns is a better code to work with, regardless of language or used
  tools. It's more maintainable, has advantage of efficiency. Complexity increases cost, complex code is not nice to
  work on.
- Good code should be testable. If tests are difficult to write, the design is poor. It doesn't mean that TDD approach
  automatically implies better design.
- We can build the entire plane for testing and go flying, but this is a bad idea. How to compare the results? It's a
  waterfall approach, the complexity is expanded. It's better to test two airplanes in a controlled environment
  (temperature, pressure), which gives repeatable results. We also don't need an engine to test the rest of the
  airplane. We can make two models of the wings and test them. We can also make small model of each airfoil, using the
  same materials and techniques and compare them. These small pieces are modules. Such experiments will give only a
  partially true picture. But modularity provides a possibility to measure things we couldn't measure without it, a
  single module is more testable than the whole. Modularity provides greater control and precision of measurement.
- The software systems are complex and testing them as a whole is complex and lacks precision, reproducibility,
  control, visibility. We need to be sure what is the objective of test (what do we want to demonstrate). Modular
  testing strategy is detailed, demonstrates the correct working of separate parts. To do this we need to define points
  of measurement, places in the whole system where we can insert probes. To relay upon them, the measurements must be
  deterministic. It will have an impact on the design of the software and this is where the real value of engineering
  approach resides. Such architectural approach allows us to measure system at defined interfaces.
- The cause of lack of determinism in computer systems is concurrency (OS re-organizing the disk when it thinks it has
  spare time), for the same sequence of bytes and instructions the results are consistent all the time. The useful
  driver of modularity is isolating the concurrency. Systems are rarely built like this, but taking an engineering
  approach they can be.
- The fundamental part of our job should be testing (with automated testing) the systems we create. With such mindset we
  are encouraged by the extra work we have to do if we create modular systems incorrectly.
- Service is a code delivering functionality to other code hiding the details of implementation. Services can be thought
  as modules. Service represents a boundary. A system is not modular if the internals of services are exposed.
  Communication between services should be more guarded than communication inside them.
- We can build, test and deploy everything together, or parts of the system separately. The most scalable approach to
  software is to distribute it, reduce coupling and dependencies between teams, when each module is independent of other
  modules. This is what microservices are.
- You can't make a baby in a month with 9 women. But you can make 9 babies in nine months with nine women, which
  averages out a baby per month. When pieces are truly independent of each other, decoupled, we can parallelize all we
  want. Coupling introduces constraints on the degree to which we can parallelize. The best way to parallelize is to do
  it in a way where there is no need to re-integrate. The only benefit of microservices is organizational scalability,
  but this is a big advantage, as small teams are 4 times more productive than larger teams. The condition of
  efficiently creating high-quality work is limiting coupling. We need modular organizations as well as modular
  software.

10 Cohesion

- Cohesion is the degree to which the elements inside a module belong together. Cost of change is the key measure of
  cohesion.
- _Put all unrelated things further apart, put related things closer together_. We should build our systems out of
  small, understandable, testable pieces. Throwing a collection of unrelated things into a single file doesn't make the
  code modular. Modularity hides information from other components. If the code within modules is not cohesive, it won't
  work. Skill and experience of developer is really important.
- The naive view of cohesion is that everything is together and easy to use.
- Optimising code to reduce typing is a mistake. Code is a communication tool, it's primary purpose is to communicate
  ideas to humans. Making code readable is a professional duty and one of the most important principles in managing
  complexity. We should optimise to reduce thinking rather than to reduce typing.
- Cohesion is contextual. It's tightly related to modularity and separation of concerns, because these techniques help
  to define what cohesion means in the context of particular design.
- DDD is a powerful tool in creating better design. Another important tool is separation of concerns (one class, one
  thing; one method, one thing). Yet another tool is testability: encourages modularity, separation of concerns and
  other attributes. Achieving a testable design forces the code to be cohesive.
- High performance systems demand simple, well-designed code. the route to fast code is writing simple, easy to
  understand code. Modern compilers can optimize code for better performance (methods inlining and other more
  sophisticated techniques). They benefit from simple, predictable code.
- Coupling results in cost which we should consider. Code A and B is coupled when B must be changed only because A
  changed. Code A and B is cohesive when a change to A allows B to change so that both add new value. It's impossible to
  create code without coupling, modules communicate with each other, so they are coupled to some degree. Coupling is the
  cost of cohesion.
- Cohesion is also an attribute of an organisation, it works at the level of information. It's a leading predictor of
  high performing teams. Cohesive teams have the ability to make their own decisions without the need to ask for
  permissions, they have all they need within their bounds to make decisions.
- Code which randomly combines ideas in this way is not cohesive, it is unstructured. Cohesion means putting related
  concepts, which change together. It can't mean _together_ by accident. Cohesion makes sense when combined with
  modularity.

## SQL Performance Explained (ISBN: 978-3-95-030782-5)

Chapter 2: The Where Clause

- _where_ utilizes the most important role of an index: finding data quickly. Condition, when specified without proper
  care, may have significant impact on performance and cause query slowness, when a large part of index must be scanned.
- An index for the primary key is created automatically. A condition supported by such index can be met by only one
  entry, there is no need to follow the index nodes, it's enough to traverse the index tree (_INDEX UNIQUE SCAN_
  operation) with logarithmic scalability and the cost is independent of the table size. If additional data needs to be
  fetched from the table, there may be a performance impact, but there can be only one table access. Using this
  operation does not cause slow query problem.
- In case of composite key (multi-column primary key) an index is created containing all PK columns (__concatenated
  index__). Column order is very important. When filtering on the full primary key, _INDEX UNIQUE SCAN_ is performed.
  Index cannot be used when filtering by second column only, _FULL TABLE SCAN_ operation is performed and the condition
  is checked against every row (cost is dependent on table size). It can be quick on development environments, but cause
  performance problems on production. Each column in an index is a sort criterion according to its position (like last,
  first name in a phone book), second column determines order only for the same value of the first column. Searching
  index is not possible with the second column only (it's like searching the phone book by first name). We can reverse
  the index column order. Searching by the first column only is still supported by the index. Such results are not
  unique anymore, database must follow the leaf nodes in such case (_INDEX RANGE SCAN_). Still creating a single index
  is usually more optimal, it saves storage and maintenance overhead on updates. An optimal index can be created knowing
  the access path to data, which usually developers know the best.
- The __query optimizer__ or __query planner__ transforms an SQL query into an execution plan. There are two types of
  planners: __cost-based__, which generate multiple plans and pick the best, and __rule-based__ which use hardcoded
  rules to generate plans.
- When concatenated index exists, it can be used to filter by the first column, no matter what other search criteria
  are. In such case the rest of conditions can be applied as 'filter' condition on rows fetched from the table. If the
  amount of rows fetched by _INDEX RANGE SCAN_ operation is large, each of them must be fetched individually. Sometimes
  it's slower than _FULL TABLE SCAN_, as the second one fetches large chunks of data in a single call. If searching by
  first column in the index is more selective, _INDEX RANGE SCAN_ performs better. Best execution plan depends on data
  distribution in the table, optimizer uses statistics, like histograms containing the data distribution in a column,
  which allow the optimizer to estimate the number of rows returned from the index lookup, which is used for the cost
  calculation. If statistics are not available, default values are used: small index with medium selectivity. When
  correct statistics are provided, optimizer does a better job (like decision if index lookup is better). Correctly
  defined index is better than full table scan. Using an index doesn't guarantee the statement is executed the best
  possible way.
- Most statistics are maintained on the column level: number of distinct values, number of NULLs, columns histogram,
  table size (in rows and blocks). Index statistics contain tree depth, number of leaf nodes, number of distinct keys,
  clustering factor. Optimizers use this values to estimate the query cost. Incorrect query plan cost estimation is an
  indicator of problems with statistics (index scan returns 40 rows, fetching 100 rows from the table). Some databases
  don't update statistics on index creation. Updating statistics help to check optimizer's estimates, but don't improve
  execution performance.
- Index on _LAST_NAME_ requires us to search using the same case (upper/lower). It's easily achievable by converting
  both sides of where clause: `WHERE UPPER(last_name) = UPPER('smith')`. However, such query results in full table scan.
  Index on _LAST_NAME_ is not usable, because the search is not on _LAST_NAME_, but _UPPER(LAST_NAME)_. To have an index
  covering this search term, we need __function-based index__, which applies the function and puts the result into the
  index (`CREATE INDEX em_name ON employee (UPPER(last_name))`). When a database doesn't support function-base indexes,
  we can add a computed column to the table and create an index on this column.
- It's possible to index expression like `X + Y` and user-defined functions. But functions must be deterministic (return
  consistent result for the same parameters). Database calls the function and stores the result in the index, index is
  not updated periodically (age can be updated only if the date of birth is updated). Functions __must be marked as
  deterministic__ (`DETERMINISTIC` in Oracle, `IMMUTABLE` in PostgreSQL). Functions like current time, random number
  generators, functions depending on environment variables cannot be indexed.
- Indexes cause ongoing maintenance. We should aim to define universal indexes which suffice several access paths. To
  achieve that we should use the same function (`UPPER`/`LOWER`) consequently. We should aim to index the original data,
  as this is often the most useful information.

# Projects

## Equino

## ACTracker

# Tutorials

- Getting Started With Python: [Data Structures](https://github.com/marcinciapa/tutorials/pull/38)
- Getting Started With Python: [Loops](https://github.com/marcinciapa/tutorials/pull/39)
- Getting Started With Python: [Functions](https://github.com/marcinciapa/tutorials/pull/40)
- Getting Started With Python: [Classes](https://github.com/marcinciapa/tutorials/pull/41)
- Getting Started With Python: [Working with Dates](https://github.com/marcinciapa/tutorials/pull/42)
- Getting Started With Python: [Working with Files](https://github.com/marcinciapa/tutorials/pull/43)
- Getting Started With Python: [Fetching Data from Internet](https://github.com/marcinciapa/tutorials/pull/44)
- Getting Started With Python: [Modules](https://github.com/marcinciapa/tutorials/pull/45)

- Professional Full Stack Developer: [Project and Environment Setup](https://github.com/marcinciapa/tutorials/pull/46)
- Professional Full Stack Developer: [Getting Started With Spring Boot 3](https://github.com/marcinciapa/tutorials/pull/47)
- Professional Full Stack Developer: [HTTP, REST and APIs](https://github.com/marcinciapa/tutorials/pull/48)
- Professional Full Stack Developer: [Developer Tools](https://github.com/marcinciapa/tutorials/pull/49)
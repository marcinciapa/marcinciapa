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

11 Separation of Concerns

- Separation of concerns is a design principle for separating a computer program into distinct sections, where each
  addresses a separate concern. It provides better focus and clarity in code and system, improves modularity, cohesion
  and abstraction, helps to reduce coupling. It's useful at the scale of whole system as well as at individual
  functions. It allows keeping focus small, understanding a single part without thinking too hard about it.
- From the e-commerce perspective, how we store something is irrelevant from point of view of shopping cart behavior.
  Separation improves testability: if persistence is included, tests are slow. When separated, both concepts can be
  tested easily with fakes.
- Dependency Injection is a useful tool: dependencies are supplied as parameters instead of creating them. DI is often
  incorrectly understood as a function of a framework, while it can be achieved in the majority of programming
  languages.
- It's important to distinguish essential and accidental complexity in a system. Essential complexity is important to
  solve a problem. Accidental complexity is related to solving side effect problem (persistence, displaying on the
  screen etc.). The logic responsible for driving a car should be separated from logic displaying information on a
  screen.
- When _and_ is part of responsibilities of a class or method it's a warning sign, that there are two concerns instead
  of one. Problems that we are solving often help to define bounded contexts. It is the essence of separation of
  concerns. Bounded contexts help to identify course-grained modules. The key aspect is a very low tolerance for
  complexity. Code should be simple, as soon as it becomes complex we should pause and look for ways to simplify it.
- Such approach is reinforced through testing. Ensuring that the code is testable enforces separating concerns.
- For code that wants to store something in Amazon S3 it's a bad practice to process something and invoking the storage.
  There must be a setup somewhere for the S3 client, there are different ways that it may arrive. Such approach
  represents two different focuses, different levels of abstraction. It increases accidental complexity. The level of
  abstraction should remain consistent. The concept of storing should be abstracted and represented as a __port__ -
  improved separation of concerns, cohesion by maintaining consistent level of abstraction. __Adapter__ is an
  implementation of a port - translating ideas to the context of AWS S3 Storage.
- It's almost never the case that the code should care about details of API it consumes. Port is a simplified version of
  API. When Amazon upgrades S3 interface we don't have to change the entire code, but we can completely rewrite the
  adapter if we want to change the approach for using this technology.
- Port and Adapter approach represents a translation layer at the boundaries between contexts (Eric Evans suggests to
  always do it), help to align services with a bounded context, which minimizes coupling and improves modularity and
  cohesion. Each service or module should have its own view of the world.
- API (application programming interface) is all information exposed to consumer of a service. Any means of
  communication meant to support programming is an API (not only REST). The level of interaction defines the degree to
  which it is coupled. The more that the code knows of the content of API parameters the more coupled it is: data
  structures in its input that their code understands is a part of the API.
- Designing to improve testability help us to improve quality improving maintenance of development, not only checking
  the correctness. Separation of concerns supported by testing leaves the door open to incremental change. We should
  take more evolutionary approach to design, building systems step by step as our understanding deepens. TDD helps to
  achieve that testability. Tests are more difficult to write if the concerns are mixed. Faster feedback allows spotting
  design problems much sooner that any other technique.

12 Information Hiding and Abstraction

- Information hiding and abstraction is a process of removing details or attributes of objects or systems to focus on
  details of greater importance. They are two different, but related topics and are best considered together.
- Looking from the outside at our code we don't care about what is behind it. As consumers, we shouldn't care about how
  it works, but how to use it. Behavior of the code, implementation details, data it uses should be hidden from other
  parts of the code.
- Some code is fragile to change, change in one place affects other parts of the code. The only way to make progress is
  understand how most of the system works, which is not a scalable approach. Such code, which people are scared to
  change is called __big balls of mud__.
- Complaints that organization doesn't allow us to refactor/test/design better are usually not true and are excuses. We
  should not ask for permission to do a good job, this should be our duty. Our code should solve problems and fulfill
  the needs of users __repeatably and reliably__. An ability to change the code is crucial. It's obvious that cleaning
  between preparing meals and sharpening knifes should be part of chef's work. Any other approach may speed up preparing
  one or two meals, but the third meal may be slower. In software our duty should be responsibility for the quality in
  practical and pragmatic way. It's in the best interest of our employer, customers and ourselves. We will be able to
  change the code, software is not a short-term game. Dropping quality makes us working slower, not faster. There is no
  trade-off between speed and quality. When a manager asks for an estimate, all benefit from providing one without
  sacrificing quality. And usually developers rule out quality, not managers or organizations, when there is a pressure
  on them. Organizations want __better software faster__, not __worse software faster__ (__worse software slower__ over
  time). Better goes hand in hand with faster. Of course good quality requires a cost (refactoring, testing, improving
  designs in software, cleaning up and maintaining tools in cooking). But these are foundations of professional
  approach, not nice to haves.
- We should have a mindset that it's a good thing to change existing code/design. Some measure the quality of software
  as the time required to rewrite half of it. When we learn new things we should be open to make changes in code or
  design to reflect our deeper understanding. Learning is an important part of software development, designs must be
  adopted to the new knowledge.
- Software engineers are often blamed for over-engineering. The reason for that is that we're chasing shiny ideas,
  whereas we should be pragmatic and consider economic constraints. We should chase the simplest solutions, not the
  coolest. Always evaluate in the context of the problem, even if that means discarding what doesn't work. Such approach
  often ends up doing something cool. Over-engineering is often caused by searching future-proof solutions. It's an
  engineering immaturity, fear from having to change the code in the future. Go __YAGNI__. In fact, we should fix
  problems preventing us from changing the code in the future, the problem of fragility. There are three approaches to
  it: __hero-programmer__, who is called to save the day, stupid idea, the effort should be better spent to share their
  knowledge. The real solutions are __abstraction__ and __testing__. Hiding complexity allows code changes in one part
  of the system with more confidence. The value of testing is not that simple.
- Effective regression testing (automated strategy) is required to have flat Cost of Change curve. Continuous delivery
  is a good starting point (_releasability_ assessed by automated testing). Testing have positive impact on design
  (small specifications of behaviors), which are needed before we start. Such specification should not consider
  implementation, it should be abstracted.
- Abstraction is powerful. We can change the video card in PC, there are abstractions which make such change safe. An
  API of AWS S3 is simple, abstracting some complex things. Abstractions are fundamental in working with computers.
- Leaky abstractions are leaked details, which should be abstracted away. "If all abstractions are leaky, why bother?"
  There are different kinds of leaks, some impossible to avoid. But authorization service returning HTML errors,
  business logic throwing NullPointerException, these should be avoided. We should focus on the second type of leak by
  maintaining a consistent level of abstractions. The goal is not to achieve perfection, but useful models we could use
  to solve problems.
- An example could be maps, which abstract the world. The distance between two points are spheres, but map provides a
  simplified, practical model we could use. Another example is underground map: allowing better orientation on transit,
  but don't work for walking between stations. It's OK to have different abstractions targeted to solve particular
  problem.
- Techniques like __event storming__ provide separation of concern natural for the problem we try to solve. It can
  highlight bounded context and natural lines of abstractions. The best way to create effective test case is to describe
  it using __domain specific language__ (DSL).
- The way that computers work introduces specific abstractions and concerns, these abstractions will inevitably leak. We
  have to make engineering trade-offs depending on the context of our system. Third-party code (like sqlite3) should be
  isolated with abstractions, otherwise we are coupled to it. It should be accessed by our own facades or adapters with
  interfaces. Such approach makes our systems more composable and flexible. We can test our code to this abstraction.
- We should use more general representations rather than more specific in functions signatures: usually it's better to
  inform that a function returns a `List` than `ArrayList`. It's abstract enough and specific enough to be helpful (more
  than returning `Object`).

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
- There are two reasons to use __bind parameters__: security (prevent SQL injection) and performance (execution plan is
  reused when only parameters change). When bind parameters are not used, changing parameters result in recalculating
  the query plan, the plan cannot be cached. Also using bind parameters the optimizer __assumes__ equal distribution and
  the estimate for such query is always the same regardless of parameters values. Optimizer doesn't use the statistics.
  It may cause the query to be created in not the optimal way, but recalculating the execution every time is a
  significant effort. It's accepting the risk. Databases try to solve this problem with heuristics, but with limited
  success. Bind parameters are recommended except cases when values __should__ influence the execution plan (searching
  for status _TODO_ probably returns much more results than _DONE_, so index makes sense only while searching for
  _TODO_). Accessing a correct partition by attribute is another example. _LIKE_ queries can also suffer from bind
  parameters. When in doubt, bind parameters should be used.
- `<`, `>`, `between` operators and `LIKE` in some circumstances can use indexes. The biggest performance risk of _INDEX
  RANGE SCAN_ is the leaf node traversal. If second column is involved in `where`, stop conditions are less obvious.
  Perfect index should cover both columns, but in which order? We should index for equality first, then for ranges. It's
  a myth that the most selective column should be at the most significant position. Even if selectivity is of no use,
  some column orders are better than another. _Predicate information_ of _INDEX RANGE SCAN_ identifies `where`
  conditions as access or as filter. Access predicates columns limit the scanned index range. Database can use all
  conditions as access predicates when we turn the index definition around. Access predicates are start and stop
  conditions for an index lookup, they define the scanned range. Filter predicates don't narrow the range, they're only
  applied during leaf node traversal.
- `LIKE` operator causes unexpected performance behavior and in some cases prevents efficient index usage. For access
  predicates only characters __before the first wildcard__ can be used, remaining characters are filter predicates on
  rows fetched from table. PostgreSQL may require an operator class (_varchar_pattern_ops) to use _LIKE_ as access
  predicate. The scanned index range is influenced by the selectivity of the prefix. When _LIKE_ expression starts with
  a wildcard, database performs full table scan, these should be avoided. When search term is delivered as bind
  parameter, database has to guess if there are leading wildcards. Most databases assumes there is no leading wildcard,
  except PostgreSQL (does not use index). Specifying the search term without bind parameters solves the problem, but
  there is optimization overhead.
- Creating one index with multiple columns is better than separate indexes with single column. But there are cases when
  single index is useless (two independent range conditions). Only one range condition is used as access predicate,
  other are filter predicates. Accepting the filter predicate is the best solution in many cases, in such scenario more
  selective column should be used first. Another option is to use two separate indexes, both must be scanned by the
  database and results must be combined. It causes more effort related to traversing two index trees, one index scan is
  faster than two. Results are combined in two ways: __index join__ or __bitmap index__. The second approach means
  indexing each column separately in a single index (multi-column B-tree index is faster than bitmap index). Bitmap
  index behaves poorly during insert, update, delete, which makes it almost unusable for online transaction processing
  (OLTP). Some database offer a hybrid solution between B-tree and bitmap, converting the results of several B-tree
  scans into in-memory bitmap structure. They are not stored persistently, discarded after each statement execution,
  which reduces the problem of the poor write scalability. Such approach requires a lot of memory and CPU.
- With __partial (filtered) index__ it's possible to specify indexed rows. It's useful when where conditions contain
  constant values (status codes). Such search can be optimized with two-column index, but it includes rows which are
  never searched, queries are fast but index wastes disk space. Index can be limited to 'unprocessed' message only (with
  __where__ clause in index definition). Such index only contains rows matching where clause, indexing this column is no
  longer necessary. Size is reduced vertically (has fewer rows) and horizontally (less indexed columns). For queues, it
  means the index size doesn't change even if table grows. Only deterministic functions may be used in where clause.
  Partial index is used when where clause matching index definition appears in a query.
- _NULL_ is handled specifically in index: SQL standard does not define NULL as a value, but as a placeholder for
  missing data. Oracle doesn't index rows if all indexed columns are NULL - every index is partial index (_where A is
  not null or B is not null_). At least one indexed column must be not null. It can be mitigated by indexing additional
  column that can never be NULL. Alternatively, constant expression can be added as indexed column (_create index
  EMP_IDX on EMPLOYEE (DATE_OF_BIRTH, __'1'__);_). It's also possible to create a partial index with _IS NULL_
  condition, but the database must be sure that other column may never be null (_NOT NULL_ constraint). Removing the
  constraint on the column makes the index unusable, database must assume there may be NULLs and performs the full table
  scan. User-defined function does not impose a NOT NULL constraint, optimizer doesn't know what the function does. To
  mitigate it the query should be changed:
  `select * from EMPLOYEE where DATE_OF_BIRTH is null and BLACKBOX(EMPLOYEE_ID) IS NOT NULL`. Oracle recognises that
  only rows which are indexed are queried. Similar result can be achieved with moving function call to a virtual column
  with NOT NULL constraint. Database recognises that some functions return NULL only if NULL is provided _UPPER_
  function preserves NOT NULL constraint of the column, but removing the constraint from the column makes the index
  unusable. The way Oracle indexes NULLs can be used to emulate partial index, NULLs should be used for rows that
  shouldn't be indexed. To emulate `create index MESSAGES_TODO on MESSAGES (RECEIVER) where PROCESSED = 'N'` we need a
  function returning _RECEIVER_ value only if _PROCESSED_ is 'N'. Function must be deterministic:
  `create index MESSAGES_TODO on MESSAGES (IS_PROCESSED(PROCESSED, RECEIVER))`. Same expression must be used in query:
  `select MESSAGE from MESSAGES where IS_PROCESSED(PROCESSED, RECEIVER) = ?`.
- Obfuscated conditions are _where_ clauses which prevent correct index usage. Usually it's a bad practice. However,
  sometimes we intentionally want to avoid access predicate, for example when we know the _LIKE_ predicate contains
  leading wildcard, and we want to spare unnecessary optimisation.
    - Often date types cause obfuscation. It's a common practice to use `TRUNC` function to remove time on both sides of
      comparison: `WHERE TRUNC(dob) = TRUNC(sysdate - '1' day)`. It prevents use of index on `dob` as `dob` and
      `TRUNC(dob)` are different things. Simple solution: function based index on `TRUNC(dob)`. The same function must
      be used every time in condition. When date in condition contains only year and month, the solution is using
      explicit range conditions (`QUARTER_BEGIN(dob)`). Simple index on `dob` is sufficient to optimize this query.
      Another common case is comparing dates as strings: (`WHERE TO_CHAR(dob, 'YYYY-MM-DD') = '1980-01-01'`). Bind
      parameters solve this problem, they support date types. Alternative solution is to convert the search term instead
      of column: `WHERE dob = TO_DATE('1980-01-01', 'YYYY-MM-DD')`. Be careful not to introduce a bug if the column has
      time. In such case use range condition:
      `WHERE dob >= TO_DATE('1980-01-01', 'YYYY-MM-DD') AND dob < TO_DATE('1980-01-01', 'YYYY-MM-DD') + INTERVAL '1' DAY`.
      Using range condition should be always considered when comparing dates.
    - Numbers stored in text columns is a very bad practice, but it doesn't make the index useless. Many databases
      implicitly convert types. Also, columns should not be converted, instead search term should be converted. Implicit
      type conversion may cause performance problems. There is also a risk of conversion error if there is one invalid
      number. When database automatically transforms the string into a number, regular index on numeric column can be
      used.
    - Obfuscations often affects concatenated indexes, like combining date and time columns to apply a range filter
      `WHERE ADDTIME(date_col, time_col) > ...`. Index on (_date_col_, _time_col_) cannot be used, because the search is
      done on derived data. Use columns containing both, date and time. If this is not possible, use a function-based
      index. It's still possible to use the concatenated index on _date_col_ and _time_col_ partially by adding
      redundant extra condition: `WHERE ADDTIME(date_col, time_col > ... AND date_col >= ...`. This condition adds
      filter on _date_col_ which can be used as access predicate providing good enough approximation (redundant
      condition must be used on the most significant column). This technique can also be used on dates stored as texts,
      but the date format must maintain chronological order (ISO 8601).
    - 

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
- Professional Full Stack
  Developer: [Getting Started With Spring Boot 3](https://github.com/marcinciapa/tutorials/pull/47)
- Professional Full Stack Developer: [HTTP, REST and APIs](https://github.com/marcinciapa/tutorials/pull/48)
- Professional Full Stack Developer: [Developer Tools](https://github.com/marcinciapa/tutorials/pull/49)
- Professional Full Stack Developer: [CRUD - Read](https://github.com/marcinciapa/tutorials/pull/50)
- Professional Full Stack Developer: [Structure the backend](https://github.com/marcinciapa/tutorials/pull/51)
- Professional Full Stack Developer: [Application Context and Beans](https://github.com/marcinciapa/tutorials/pull/52)
- Professional Full Stack Developer: [Error Handling](https://github.com/marcinciapa/tutorials/pull/53)
- Professional Full Stack Developer: [Databases](https://github.com/marcinciapa/tutorials/pull/54)
- Professional Full Stack Developer: [Spring Data JPA](https://github.com/marcinciapa/tutorials/pull/55)
- Professional Full Stack Developer: [Get Customer By Id Exercise](https://github.com/marcinciapa/tutorials/pull/56)

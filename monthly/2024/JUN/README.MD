# Readings

## SQL Performance Explained (ISBN: 978-3-95-030782-5)

Chapter 4: The Join Operation

- Join operation transforms data from normalized model into a denormalized form. As it combines scattered data
  fragments, it's sensitive to disk seek latencies. Proper indexing is a solution to reduce response times.
- Processing two tables at a time is common to all join algorithms. To join more tables it's necessary to build an
  intermediate result of joining two tables and joining the result with the next table.
- Join order doesn't have impact on the result, but affects performance. Optimizer selects the best of all possible join
  order permutations. The performance problem is that more tables to join means more execution plan variants (n!).
  Problem doesn't exist when using bind parameters. The more complex the statement the more useful bind parameters are.
- To reduce memory usage, each row from intermediate results is pipelined to the next join operation instead of
  materializing it.
- __Nested loops join__ algorithm works using two nested queries: outer query fetching from one table and a second query
  for each row to fetch corresponding data from the other table. It works similarly to _nested selects_, the same
  approach which is the source of N+1 select problem in ORM tools: By default, ORMs don't generate joins, they query the
  related table with another selects.
- Databases execute joins exactly as ORM. Indexing for a nested loops is indexing for two separate _select_
  statements (including an index for the join predicates on the secondary table).
- SQL join is more efficient than nested selects. Even though it performs the same index lookups, it avoids a lot of
  network communications: number of database round trips is more impactful than the amount of data transferred.
- There are possibilities to create SQL joins in most ORMs, like _eager fetching_ mode: it's configured at the property
  level in entity mapping. In such case ORM will always join two tables, which only makes sense if data from both tables
  are needed (parent and child). Otherwise, it's counterproductive.
- JPA _CriteriaBuilder_ offers _Root.fetch()_ method for controlling joins. But JPA and Hibernate return the parent row
  for each child (Activity with 10 Tags will appear 10 times). Client side should manually de-duplicate them or use
  function _distinct()_. Only a few databases recognize that primary key guarantees uniqueness in that case. Hibernate
  solves the problem on the client side using result set transformer.
- The execution plan shows the _NESTED LOOPS OUTER_ operation, meaning querying the child table for each parent. Nested
  loops join offers good performance if the outer query returns small result set. In other cases optimizer may choose a
  different join algorithm.
- __Hash join__ algorithm solves the problem of nested loops join: many B-tree traversals for the inner query. Candidate
  records from one side of the join are loaded into a hash table, which can be accessed quickly from the other side of
  the join. Requires different indexing approach. Reducing columns improves performance.
- Hash table uses the join predicate as a key. It's a temporary in-memory structure to avoid accessing joint table many
  times. Indexing join predicate doesn't improve hash join performance. Independent predicates (individual for each
  table) can be indexed to improve performance.
- Minimizing the hash table size optimizes hash join performance, because hash join is only possible if the entire hash
  table fits into memory. Optimizer uses the smaller side of the join for the hash table. Execution plan shows estimated
  memory requirement.
- It's possible to reduce the hash table size by changing the query: adding extra conditions to limit candidate records.
  Condition on a single column improves performance even if there is no index for this column: unnecessary rows don't
  have to be stored in the hash table. Constraints help to guard assumptions.
- The goal of minimizing the hash table size is not reducing number of rows, but the memory footprint. Selecting fewer
  columns improves hash join performance. It's not always easy when using ORM. JPA defines _FetchType.LAZY_ in the
  _@Basic_ annotation on property level, but providers are free to ignore it.
- __Sort-merge join__ combines two sorted lists like a zipper. Requires same indexes as the hash join, an independent
  conditions to read all candidate records in one shot. Indexing join predicates is useless. _Absolute symmetry_ is the
  unique aspect of sort-merge join. It's useful for outer joins, can even do left and right outer join (full outer join)
  at the same time.
- Sort-merge join performs well when inputs are sorted, it's hardly used because sorting both sides is very expensive.
  It is useful when inputs are already sorted, which is possible by exploiting index order to avoid sort operations.

## Learning How to Learn (ISBN: 978-0-52-550446-7)

Chapter 1: The problem with passion

- Brains change their structure depending on what we do with them
- Following only passion (things we're good at and which come easy) reduces opportunities, including relationships,
  things which happen to us in life
- It's OK to follow passions, but broadening them open many new opportunities
- Learning new things we don't know may be interesting is an adventure
- Brain is a toolkit, we learn how to use in our lives, as we learn not to use hammer to turn a screw
- Reading a new chapter should start with _picture walking_ through it, visiting sections headings, bold text, summary,
  trying to answer questions at the end of chapters; it gives the brain an idea of what's coming, like checking a map
  before starting a journey

Chapter 2: Easy does it

- Sometimes we have to be less focused to be better learners.
- The brain can be in one of two states: __focused__ and __diffused__. Diffused modes helps build better connections
  between ideas, it leverages creativity. To learn effectively, brain has to go back and forth between these two states.
- We can switch to diffused mode by letting go, not thinking about anything: going for a walk, looking out a window,
  taking a shower, going to sleep. Focusing on something else can take us into diffused mode.
- There are two ways we can get stuck on solving a problem: not focussing on catching initial explanation and being in
  focused mode for too long.
- If we didn't focus first, we have to go back to the beginning and focus on understanding a problem again.
- When the brain is tired of being focused, it's good to take a break and switch to diffused mode (5-10 minutes).

# Tutorials

- Professional Full Stack Developer: [Integration Testing](https://github.com/marcinciapa/tutorials/pull/68)

# Readings

## SQL Performance Explained (ISBN: 978-3-95-030782-5)

Chapter 5: Clustering Data. The Second Power of Indexing

- Clusters are related things that appear together. Clustering data means storing consecutively accessed data together,
  so accessing it requires fewer IO operations. In SQL database the simplest cluster is __the row__: all columns in the
  same database block.
- Indexes allow clustering data. Indexes build clusters of rows with similar values: leaf nodes store ordered, similar
  columns close to each other.
- Index filter predicates may be used to group consecutively accessed data together, not only to improve range scan.
  Example: _LIKE_ expressions with leading wildcards cannot be used to traverse the tree. Columns indexed for such
  condition don't narrow the scanned index range. But the most expensive parts of execution time of a query is table
  access. It's not necessarily the bottleneck if rows are stored in a single table block, they are fetched in a single
  read operation. Rows spread across multiple blocks may cause serious performance problem - database must fetch many
  blocks. Performance depends on the distribution of accessed data, depends on __clustering__.
- The correlation between index order and table order is a performance benchmark - __index clustering factor__.
- It's possible to improve performance by aligning rows in a table, so they correspond the index order. It's rarely
  applicable: it's possible to store rows only in one sequence - it's possible to optimize only for one index. Row
  sequencing is impractical approach. Index clustering factor is probability that two succeeding index entries refer to
  the same table block. Optimizer takes this probability into account.
- It's possible to add many columns to an index to ensure they are automatically stored in a well-defined order. It
  means extending the index to cover all columns from the _where_ clause, not only those which narrow the scanned
  range (_first_name_ to cover _first_name LIKE '%'_ condition). First columns can be used as access predicates, other
  columns as filter predicate. Rows not fulfilling the _LIKE_ condition are automatically discarded - table access does
  not have any filter predicates, unnecessary rows are not fetched, _TABLE ACCESS BY INDEX ROWID_ operations are
  reduced. But additional columns make indexes bigger, it's usually an acceptable compromise. However, we should be
  aware of it, use index filter predicates intentionally. New indexes for this purpose should not be introduced,
  existing indexes should be extended. Decision about column order should not be left to chance.
- Index-only scan avoids accessing the table completely if all data can be found in index itself: index must contain all
  columns from the SQL statement. (for _SELECT SUM(eur_val)_ _eur_val_ column must be added to index). Index preventing
  table access is called __covering index__. Such index is helpful especially when prevents accessing many rows from
  different table blocks. Such indexing strategy is aggressive and __should not be a default solution based on suspicion
  only__. Also, existing indexes should be extended instead of creating new indexes.
- Another advantage of indexes created on a field like _create_date_ is that they have a good clustering factor:
  probably new rows are always appended to the end of the table, table order corresponds to the index order. When using
  indexes with good clustering factor, table rows are stored closely together.
- Problem of using function based indexes together with index-only scan is that they cannot be used for selecting
  original data. We should always aim to index the original data and avoid function-based indexes for expressions that
  cannot be used as access predicates. Aggregating queries are good candidates for index-only scans.
- PostgreSQL support so-called non-key columns in B-tree indexes, which are only stored in leaf nodes, cannot be used
  for access predicates. They are specified in the __INCLUDE__ clause.
- If we put all columns into the index, we don't need the heap table. Such concept is called by Oracle __index-organized
  tables__ (IOT), other databases call it __clustered index__. It's a B-tree index without a heap table. Such approach
  saves the space for heap structure and access is automatically index-only scan. It's hardly achievable in practice.
- The problem begins when creating another index on the same table, secondary index refers to the original index.
  Secondary index stores logical key from the first index instead of the reference to the heap table. Table access may
  be a bottleneck, but it's limited to one read operation per row. Index has its exact position, so database can
  immediately load the row from the heap table. But when using index-organized tables, accessing a table via a secondary
  index searches two indexes: the secondary index once (_INDEX RANGE SCAN_), then the clustered index __for each
  row__ found in the secondary index (_INDEX UNIQUE SCAN_). It's a very inefficient approach, but it can be prevented
  the same way that prevents table access - using index-only scans, or rather "secondary-index-only scans". In practice,
  performance improvements on the clustered index are easily lost when using a secondary index: secondary indexes are
  large, clustering key is usually longer than ROWID. Benefits of index-organized tables is mostly limited to tables
  that do not need a secondary index.
- In MySQL MyISAM engine only uses heap tables, InnoDB engine always uses clustered indexes. PostgreSQL only uses heap
  tables. Using _CLUSTER_ clause aligns the content of the heap table with an index.

## Learning How to Learn (ISBN: 978-0-52-550446-7)

Chapter 3: I'll do it later, honest!

- Thinking about unpleasant things we postpone for later causes painful feeling. The pain goes away immediately after we
  start working on them.
- While doing a pomodoro don't focus on finishing a task. The goal is to be focused during ~25min time.
- Multitasking is a bad idea. Human can effectively work on only single task in the same time.
- Postponing procrastination for later can be a useful mental trick.
- __Create good habits__ to eliminate bad habits: if you have a habit to check vibrating phone, create a habit to turn
  on 'don't disturb' mode when starting working.
- Little procrastinations seem innocent, but if they become habit, they have overall damaging effect on our efficiency.
- __Active recall__ is another learning technique - go back to learned content trying to recall the key ideas, ideally
  out of memory.

Chapter 4: Brain-links and fun with space aliens

- Neurons use _Axons_ to send signals to other neurons' _dendritic spine_. The connection is called _synapse_. The more
  often neurons exchange signals, the more strong the connection is.
- When we start learning something, the link is weak. When we practice, the link becomes stronger and there are
  more neurons participating.
- Using __metaphors__ helps to reuse existing connections to understand different concepts. Once metaphors become not
  sufficient, we should throw them away and replace with others.
- It's possible to retrain the brain learning something intensively. _Neuroplasticity_ means that the brain can change
  through learning.
- Common excuses exist which prevent us from learning new things. All have counter-arguments and can be challenged.

# Tutorials

- Professional Full Stack Developer: [The Jar file](https://github.com/marcinciapa/tutorials/pull/69)
- Professional Full Stack Developer: [Maven & Tests](https://github.com/marcinciapa/tutorials/pull/70)
- Professional Full Stack Developer: [DevOps](https://github.com/marcinciapa/tutorials/pull/71)
- Professional Full Stack Developer: [Docker](https://github.com/marcinciapa/tutorials/pull/72)
- Professional Full Stack Developer: [Jib](https://github.com/marcinciapa/tutorials/pull/73)

# Readings

## SQL Performance Explained (ISBN: 978-3-95-030782-5)

Chapter 5: Sorting and Grouping

- Sorting is a resource intensive operation, database must store the temporary results first.
- Index delivers sorted representation of the data. __Using index allows avoiding sort__ operation, when data is
  presorted. But the same index needs to be used to cover _WHERE_ and _ORDER BY_ clauses.
  `SELECT * FROM sales WHERE sales_date=TRUNC(sysdate)-INTERVAL '1' DAY ORDER BY sale_date, product_id;`:
  `CREATE INDEX sales_dt_pr ON sales (sale_date, product_id)`
  Optimisation also works when sorting by _PRODUCT_ID_ only.
- If followed by a table access, _INDEX RANGE SCAN_ can be inefficient, which removes benefits of avoiding sort
  operation. Sometimes _FULL TABLE SCAN_ is faster. It's optimizer's job to evaluate.
- Indexed _ORDER BY_ execution allows returning first results without processing all data, it's executed __in pipeline
  manner__, and it's the third power of indexing.
- `SORT ORDER BY` operation disappears from the query plan when index is used. Surprisingly, the cost value may increase
  because the clustering factor of the new index may be worse. But cost value is not always good indicator of the
  execution effort.
- In the considered example, when the scanned index range is
  extended (`WHERE sale_date>=TRUNC(sysdate)-INTERVAL '1' DAY`) explicit sort must be performed. Index range is not
  sorted by _PRODUCT_ID_. To find a reason of explicit sorting, use the full index definition in _ORDER BY_ clause.
- Optimizer is searching for an execution plan which gets the last record fastest. If database detects that only first
  few rows are fetched, it may prefer an indexed _ORDER BY_.
- Databases read indexes in both direction, pipelined _ORDER BY_ is also possible if scanning index in opposite order.
  This is why databases uses doubly linked list. But the problem begins when _ASC_ and _DESC_ modifiers are mixed,
  database would have to make an impossible index jump. `ORDER BY sales_date ASC, product_id DESC`. Previously created
  index cannot be used. However, it's possible to use _ASC_ and _DESC_ modifiers in the index
  declaration: `CREATE INDEX sales_dt_pr ON sales (sale_date ASC, product_id DESC);`.
- The SQL standard doesn't define the sort order of _NULL_. It only tells that _NULL_s must appear together after
  sorting. To get a well-defined behavior, null sorting of all columns should be specified in _order by_. Only
  PostgreSQL supports _NULLS_ modifier in both _order by_ and index definition.
- There are __two different group by algorithms__: __hash algorithm__ and __sort/group__. Hash algorithm aggregates
  records in a temporary hash table, which is returned as a result. Sort/group first sorts the input data by the
  grouping key and aggregates them afterward. Both algorithms need to materialize an intermediate state, cannot be
  executed in a pipelined manner. But sort/group algorithm can use index to avoid sort, so it allows __pipelined group
  by__.
- Sort/group is more effective when _INDEX RANGE SCAN_ delivers rows in the required order. Database avoids
  materialization, because it doesn't require explicit sort operation. In the query plan it can appear as _SORT GROUP BY
  NOSORT_ operation (Oracle). Other databases may not mention any sort operation at all.
- Defining index with _ASC/DESC_ modifiers should not affect pipelined group by execution, the same for _NULLS
  FIRST/LAST_. PostgreSQL doesn't automatically do a pipelined group by if the index threats NULL value as the smallest.
  Adding order by clause with the index order bypasses this problem. Oracle cannot read index backwards to execute a
  pipelined group by followed by order by.
- If _INDEX RANGE SCAN_ doesn't deliver rows ordered by the grouping key (sales since yesterday), database doesn't
  execute pipelined group by. Instead, Oracle uses _HASH GROUP BY_ operation as it has an advantage of only needing to
  materialize aggregated results, while sort/group materializes complete input set: hash algorithm requires less memory.
- Fast execution is not the most important advantage of pipelined order by or pipelined group by, but the fact that
  database can deliver first result before reading the entire input.

## Learning How to Learn (ISBN: 978-0-52-550446-7)

Chapter 5: The other side of the teacher's desk

- If we limit our learning only to what we like, we may feel one day that there is something missing in our education.
  We should not be limited to what comes easy to us. Learning new things may be satisfying. But for the beginner
  everything seems difficult.
- One of the best ways to learn is to __teach others__.
- If you don't understand the explanation, search for another one: in other sources, in the Internet.
- If you get stuck, ask somebody else.
- Use your imagination to remember difficult things (__visualise them__).

Chapter 6: Learning while you sleep

- While you learn something, dendritic spines begin to grow. But they grow very fast during sleep, synapses become
  stronger. While sleeping you get like a brain software upgrade.
- Ideas not practiced regularly are removed: unused synapses are cleaned-out.
- Sleeping is like cementing learnt ideas.
- Reverse procrastination is doing all homeworks early in advance to forget about them. But it has negative consequences
  if we don't regularly get back to them to solidify. Synapses will be cleaned-out if not revisited.
- It's normal that some people need more, some less time to learn particular new things.
- __Explain ideas to others__, make __flash cards__ to learn more effective.

# Tutorials
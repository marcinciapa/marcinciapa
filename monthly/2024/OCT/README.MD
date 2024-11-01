# Readings

## SQL Performance Explained (ISBN: 978-3-95-030782-5)

Chapter 8: Modifying Data

- Performance of __Data Manipulation Language (DML)__ (_insert_, _update_, _delete_) is negatively impacted by indexes.
  Indexes are redundancies, DML not only operates in table space, but must keep indexes consistent.
- Dominant factor of _insert_ performance is the number of indexes. Insert doesn't have the _where_ clause, so it's the
  only operation which does not benefit from indexes. Adding a row contains of several steps: find a place to store the
  row, add data to the respective data block, __add entry to each index of this table__. Number of indexes multiplies
  the cost. And extending index can be expensive: tree needs to be balanced, index order must be preserved. If there is
  no space in the leaf for new data, leaf needs to be split, branch node can also get out of space etc. In worst case
  each node to the root need to be split or tree depth needs to be extended.
- Maintaining indexes is the most expensive part of insert, adding first index multiplies the performance by hundreds,
  first index makes most significant difference.
- To optimize insert performance, keep number of indexes small. Also, when large portion of data needs to be inserted,
  we can consider dropping indexes temporarily (it's a common practice in data warehouses).
- _delete_ statement has _where_ clause, so it can benefit from indexes. The cost of deleting reference from the index
  is similar to insert (tree needs to be balanced). If there is no index, the entire table needs to be scanned to find
  the row for deletion.
- _delete_ statement without _where_ cannot use the index. _truncate table_ has the same effects as _delete_ without
  where, but it deletes all rows in one shot, does implicit commit and doesn't execute triggers. In PostgreSQL delete
  only sets 'deleted' flag, so the performance doesn't depend on the number of indexes. Physical removal of data is
  performed during _VACUUM_ process.
- _update_ statement must remove and add entry in the impacted index, so the response time is similar to _insert_ and
  _delete_ statements together. Performance depends on the number of indexes, but during partial updates not every
  index is impacted, only those which contain updated columns. It's beneficial to update only changed columns. ORM tools
  tend to generate statements which update all queries. Hibernate does that when _dynamic-update mode_ is disabled. It's
  a good practice to verify ORMs statements from time to time.

## Learning How to Learn (ISBN: 978-0-52-550446-7)

Chapter 11: How to pump up your brain

- _Hippocampus_ is responsible for remembering facts. During sleep information is transferred to _cerebral cortex_.
  Thanks to that hippocampus is cleared and ready to take new information. Damaging hippocampus results in amnesia.
- If one doesn't learn anything new, neurons created in hippocampus die shortly after being created.
- __Physical activity__ results in creating _BDNF_, a chemical which is responsible for creating strong synapses. It
  also produces _dopamine and serotonin_ which help creating new ideas.
- __Healthy diet__ is required to support learning naturally.
- It's OK to learn from different sources (including YouTube), but it should be followed by practice.

Chapter 12: Making brain-links: How Not to Learn from a Comic Book

- You become an expert when you see the big picture, even if there are some missing pieces.
- Lazy learning is when we practice only an easy part. Deliberate practice is when we increase difficulty.
- Interleaving is learning how to solve problems differently. Otherwise, we only use hammer, because everything looks
  like a neil.
- Deep focus helps with building brain links.
- Observing other people or reading a book helps to get started, but we only become experts through practice.
- Learning to play an instrument is healthy for our brains.
- It's impossible to learn everything, instead it's recommended to learn few aspects and create brain links.

# Projects

## Actracker

- Session keep-alive was [implemented](https://github.com/marcinciapa/actracker-ui/pull/67) in actracker-ui.

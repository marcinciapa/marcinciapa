# Readings

## SQL Performance Explained (ISBN: 978-3-95-030782-5)

Chapter 5: Sorting and Grouping

- Sorting is a resource intensive operation, database must store the temporary results first.
- Index delivers sorted representation of the data. __Using index allows avoiding sort__ operation, when data is
  presorted.
- If followed by a table access, _INDEX RANGE SCAN_ can be inefficient, which removes benefits of avoiding sort
  operation. Sometimes _FULL TABLE SCAN_ is faster. It's optimizer's job to evaluate.
- Indexed _ORDER BY_ execution allows returning first results without processing all data, it's executed __in pipeline
  manner__, and it's the third power of indexing.

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
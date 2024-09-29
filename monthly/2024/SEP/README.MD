# Readings

## SQL Performance Explained (ISBN: 978-3-95-030782-5)

Chapter 7: Partial results

- There are times, when we're interested only in first few rows (Top-N queries), for example to show only ten most
  recent messages. If all rows have to be sorted, there could be serious performance problem. Pipelined order by helps
  to efficiently retrieve partial results.
- The simples way to optimise is to fetch only first rows of a query and close the statement. But optimizer doesn't
  foresee it when preparing the execution plan, it assumes the best plan for fetching all rows (which may be most
  efficient with full table scan when explicit sort is required). It's possible to inform optimizer using _LIMIT_ or
  _FETCH FIRST_ (both work in PostgreSQL). Optimizer then prefers using pipelined order by if applicable.
- It's possible to use pipelined order by (avoid explicit sort) when order by clause is covered by index. Without it
  response time of top-N query grows with the table size. When using pipeline execution, response time grows with the
  number of selected rows.
- There are two ways of fetching next pages: __offset method__ (fetching rows from beginning and using filter on rows
  number) and __seek method__ (using condition to discard preceding rows).
- The advantage of offset method are simplicity and ease of selecting arbitrary page, the cost is increased performance
  burden with every next page (all previous page rows need to be fetched before they get discarded) and drift when
  inserting new rows. Seek method doesn't have this problem because it uses value from previous result as a delimiter.
  We have to, however, ensure that the results are deterministic (sort guard: sorting and condition on the same unique
  fields). Database may deliver results in different order every time, because it uses parallel query execution. We
  should be responsible for deterministic results, even if they are not specified in requirements. Cursor fields should
  be added to index.
- Seek method may use __row values__ syntax for specifying conditions: `WHERE (sale_date, sale_id) < (?, ?)_.` (not all
  databases support it). However, it is also possible to use approximated variant (with regular _AND_s).
- The difference in performance between offset and seek methods is visible from about _20 pages_.
- _Window function_ can also be used to implement pagination, but its performance is on the same level as offset method.
  It shines in analytical calculations.

## Learning How to Learn (ISBN: 978-0-52-550446-7)

Chapter 7: School bags, lockers, and your attentional octopus

- Working memory is like a school bag: it holds only limited number of things, which can fall off.
- Long-term memory is like a school locker - it fits much more stuff which is sometimes hard to find.
- Working memory, an attentional octopus, is located in the pre-frontal context of the brain, just above eyes. Long-term
  memory is distributed across entire brain. It's impossible to fill it.
- Working memory holds only about 4 threads, things we have in mind right now. It requires focus to keep them. Repeating
  and writing help to hold them. If we don't focus, we may forget names of newly met people just after we heard them.
- Long-term memory stores everything we remember: faces of our friends, layout of our schools etc.
- There are techniques to help remember more easily.

Chapter 8: Slick tricks to build your memory

- Our mind is better in remembering pictures instead of facts, so to better remember information we could turn them to
  pictures, ideally moving or ridiculous.
- There are five tricks helping to train memory:
    - Focus while learning something
    - Practice
    - Turn information into pictures
    - Anchor new things to those you already know
    - Recall
- While memorizing sequence of new things, turn them into mnemonics.
- Memory palace is a powerful technique. Put information you want to remember to the layout of place you know very well,
  like your apartment. It works because our visuospatial memory is one of the most developed.
- There are additional useful techniques:
    - Make a song about the information
    - Create metaphors
    - Make notes, ideally handwritten
    - Imagine you are the thing you want to learn
    - Turn digits into representative shapes
    - Teach others
    - Sleep
- Additional particularly useful technique is the Rubber Duck technique: Explain things to the rubber duck.

Chapter 9: Why brain-links are important

- Brain links are strong neural connections, which allow to solve complex problem almost automatically, without paying
  too much attention.
- Deep understanding is not enough to build brain links, intensive practice is required.
- Brain links work as long as there is not too much information, external factors, cognitive load, which require putting
  additional attention.
- Focus is required for building brain links, disruption puts us away from achieving the goal.
- Context switching is expensive for our brains, they quickly become tired.
- Enjoyment comes after learning new things deeply. Every new thing seems hard when we first start learning it.

Chapter 10: Learning with clubs and groups, finding your mission, and how Terry nearly burned down the school

- Share passions with friends, it's a good way to boost creativity and more solutions to complex problems.
- Single advise from a respected person can have huge impact on our lives.
- Subjects are connected in a way which is hard to imagine.
- Make new information active, don't just read about it.

# Projects

## Equino

## Actracker

# Tutorials

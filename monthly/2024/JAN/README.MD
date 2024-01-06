# Readings

## Optimizing Java. Practical Techniques for Improving JVM Application Performance (ISBN: 978-1-49-203927-3)

Chapter 11. Java Language Performance Techniques

- JVM is able to optimize non-optimal code. However, the design of the code is the biggest potential performance
  bottleneck. It's not wise to use data structures without the knowledge how they behave.
- In Java there are two main types of containers: sequential, where objects are accessed by numerical index, and
  associative, where objects themselves are used to determine where they should be stored in a collection. In either way
  references are stored in containers rather than objects themselves.
- The main types for lists are __ArrayList__ and __LinkedList__.
    - ArrayList stores objects in the internal array (initially, by default, of size of 10). When it becomes full, new
      larger array must be allocated. It provides a convenience of not knowing ahead what the size of the list will be,
      but also an overhead related to resizing. It's possible and wise to set the capacity upfront, whenever the size of
      list is known.
    - LinkedList's advantage is O(1) appending.
    - The decision whether to use ArrayList of LinkedList should depend on the pattern of access. Inserting to both
      lists is O(1). Adding at an index to ArrayList requires all following elements to be shifted. Getting any element
      from ArrayList is O(1), while LinkedList requires navigating from the beginning. Usually ArrayList is recommended,
      unless specific behavior of LinkedList is desired (appending to the beginning/end of the list).
    - _Collections_ class exposes method _synchronizedList()_, which wraps List method invocations in a _synchronized_
      block.
- The main types for map are __HashMap__, __LinkedHashMap__ and __TreeMap__.
    - Two factors impact the performance of HashMap: __initialCapacity__ (current number of buckets, defaults to 16) and
      __loadFactor__ (how full the hash table is allowed before the capacity is increased, defaults to 0.75). Increasing
      the capacity (twice for JVM) requires rehashing, resulting in performance impact.
    - It's recommended to set the initialCapacity if it's known upfront. It reduces the overhead of automatic rehashing.
      Increasing the loadFactor also reduces rehashing, but makes access slower as map becomes fuller.
    - HashMap provides O(1) _get()_ and _put()_ times. Iterating may be costly and is impacted by initialCapacity and
      loadFactor.
    - When buckets become large

# Projects

## ACTracker

- GET API for entities was [introduced](https://github.com/marcinciapa/actracker-api/pull/161) in actracker-api.

# Tutorials

- Microservices and Distributed Systems: [Bootstrap with Maven](https://github.com/marcinciapa/tutorials/pull/10)
- Microservices and Distributed Systems: [Your First Microservice](https://github.com/marcinciapa/tutorials/pull/11)
- Microservices and Distributed
  Systems: [Microservice Communication via HTTP](https://github.com/marcinciapa/tutorials/pull/12)
- Microservices and Distributed Systems: [Open Feign](https://github.com/marcinciapa/tutorials/pull/14)

# Readings

## The Pragmatic Programmer (ISBN: 978-0-13-595705-9)

Chapter 6: Concurrency

- **Concurrency** is when two procedures behave like running at the same time. Requires system tools supporting
  switching context and control (processes, threads). Concurrency is a must for simulating reality, when things are
  asynchronous. **Parallelism** is when two procedures are truly running at the same time. It requires hardware that
  supports it (multicore processors, multiple computers etc.).
- There are two aspects of time that are interesting to us: concurrency, when things happen at the same time and
  ordering, when things happen sequentially. The second one introduces **temporal coupling** which should be avoided: We
  don't want multiple API calls to be performed sequentially if we want them to be efficient.
- Bartender would lose his job if he would do all things sequentially. Instead, he can do certain things in parallel:
  getting glasses in the time when the drink is liquifying. On the other hand, he would need five hands to do all steps
  at once. We optimize for places where our programs are stalled, like waiting for API responses, reading from disk etc.
- When there are two customers at the restaurant ordering a cake after a dinner and both are assured by their waiters
  that there is a last piece of cake to order, someone will be disappointed. The problem is that both waiters have their
  own representation of memory, which goes outdated. **Semaphores** provide a solution (waiter needs to hold a token to
  confirm order to customer), but the problem is that responsibility for using them is delegated to clients themselves:
  they have to agree that they will comply in a certain form. To solve this problem, holding a lock needs to be part of
  the cake gallery itself. Robust error handing is required to prevent permanent lock when something goes wrong.
- To maintain mutual access to two resources (cake with ice cream), instead of implementing it on one of the party, it
  should be externalised (cake with ice cream should be an independent resource).
- Most languages offer libraries supporting mutual exclusion to resources (**mutex**).
- **Actors** are interesting solutions for solving concurrency problem without synchronisation: they are independent
  processes having their own memory and not depending on centralised repository. They wait and start working when they
  receive a message in their inbox that they should do so. They can also message or initialise other actors. The idea
  sounds like microservices or lambdas.
- **Blackboard** is an analogy to how criminal cases can be solved: there are different detectives, with different
  education, specialisation, coming to the blackboard, pinning results of their actions and further steps around a
  general question, like: "Murder, or accident?". Other detectives come and see the results of such asynchronous
  investigation, taking their actions based on the content pinned. They don't even have to know each other. Blackboards
  are good for coordinating workflows when supported by fitting tools, like Kafka.
- Actors/Microservices/Blackboards are more sophisticated and harder to analyse, a lot of actions are indirect. Tracing
  communications (using trace ID distributed across all parties) is a must. 

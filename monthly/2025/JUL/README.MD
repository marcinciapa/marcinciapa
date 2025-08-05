# Readings

## The Pragmatic Programmer (ISBN: 978-0-13-595705-9)

Chapter 5: Bend, or Break

- The modern software should be as loose as possible, support reversible decisions.
- Tell, don't ask. Relying on internal state of objects and updating these objects breaks encapsulation, introduces
  coupling, spreads implementation knowledge within the code.
- Avoid multiple chained method calls.
- Avoid global variables. They are source of coupling. Changing global values potentially affects all the code. They
  make testing harder. Every mutual resource is a global variable, including database, singletons. Wrap it with
  abstraction you can control.
- Finite state machine is a good specification for event handling. States can be decorated with actions which trigger
  state changes.
- Observer pattern allows registering observers to be iterated within observable when an event occurs. Disadvantages:
  they are also coupled (registration), they may introduce performance bottleneck, as they are synchronous.
- Publish/subscribe pattern eliminates problems of observers. It is potentially asynchronous, the only common part
  between publisher and subscriber is the name of channel, which is monitored by subscriber.
- Streams/functional programming is similar to cells in spreadsheet: when one cell is modified, it causes changes in
  other cells. Events also can be triggers for reactions.
- Programming is about code, programs are about data. When we think about programs as data processors it eliminates a
  lot of problems, simplifies things. Structure is simpler, error handling more consistent, coupling drops down. It also
  introduces some level of coupling, but better manageable than in object-oriented programming.
- In streams, to represent errors, use wrappers which not only pass values between transformations, but also a
  confirmation that the value is correct.
- Avoid inheritance. It introduces coupling not only between super/subclasses, but also for consumers. There are ways to
  replace inheritance:
    - Use interfaces
    - Use delegation
    - Use mixins and traits
- If an application depends on values which may change after going life, it's better to externalise configuration to
  avoid changing source code to change values. Good candidates for configurations are:
    - URLs for resources (APIs, databases)
    - Credentials
    - License keys
    - Logging levels
    - User-defined information, like tax rates
- Many frameworks use plain text files or database tables loaded at application startup. It's better to use thin API for
  this purpose. Advantages:
    - Many applications can reuse it
    - It can be configured using specialised UI
    - Values can be changed globally
    - It's dynamic, application restart not required

# Tutorials

- [Spring AI tutorial](https://github.com/marcinciapa-learning/spring-ai-tutorial)
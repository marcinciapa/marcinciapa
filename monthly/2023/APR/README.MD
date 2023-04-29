# Readings

## Domain-Driven Design: Tackling Complexity in the Hearth of Software (ISBN: 978-0-321-12521-5)

I expect this book to help me better organize my knowledge of DDD.
[Implementing Domain-Driven Design](https://github.com/marcinciapa/marcinciapa/blob/master/monthly/2023/MAR/README.MD#implementing-domain-driven-design-isbn-978-0-13-303990-0)
is a great book, but it's bloated, very descriptive. I expect from Eric Evan's work a narrow, concrete vision.
The preface say: design of the application, which is an iterative process, should focus on the domain.

Chapter 1: Crunching Knowledge.

- The domain model should be created and should iteratively change with a brainstorming sessions between domain experts
  and developers.
- Irrelevant parts of the domain knowledge should be removed from the model.
- Developers should learn the language and gain knowledge of the domain.
- Building model with waterfall doesn't ensure required feedback.
- Building model iteratively without domain experts causes the model to be naive.
- Discovering the domain model should be more than finding nouns, the interactions between objects and applied business
  rules are equally important.

Chapter 2: Communications and the use of language.

- There are four basic elements which define the model: common language, diagrams, written documents, code.
- The common language should be use by both sides: domain experts and developers. One team - one language.
  It should evolve: domain experts should point out which phrases are awkward and eliminate them.
  The language is an intersection between experts' jargon and developers' jargon.
  Requirements, features, acceptance tests, code should be written using common language.
- Diagrams should be informal, minimal, fragments of domain model which encourage the brainstorming.
  They should not represent the entire domain.
- Written documents ensure stability and share-ability.
  They should use be written in the current language and updated if out of date.
  Documents should help the developer to fill the gaps in knowledge understanding.
  They may lie if outdated, in opposite to the code, which running correctly never lies.
- The well structure code can be a good documentation.
  But it provides unnecessary level of details and is sometimes hard to understand: not only developers should
  understand the model.
  Well-structured, transparent code should not only work correctly, but say what it's doing.
- The technical model should be strict and minimal. The explanatory model should give more context.

Chapter 3: Binding model and implementation.

- The implementation must not be separated from model. Different situation provides confusion and bugs.
- When the model is clumsy, we must search for a better one.
- If the model is impractical for implementation, we must search for a better one.
- It takes many iteration and refactoring to create a model satisfying both needs.
- Separating modeling and implementation doesn't work.
- Every technical person taking part in modeling should touch the code.
- Every developer taking part in implementation should take part in modeling.
- Refactoring do harm to the code instead of improving it if done without changing the model.

Chapter 4: Isolating the domain.

- It is impractical and confusing to have the infrastructure objects containing domain logic.
- Domain should be isolated to cohesive module.
- Layered architecture allows separation of concerns.
- Frameworks can get into way, bloating domain objects with infrastructure code. Using only part of framework's
  functions may be a good choice.
- Smart UI is an anti-pattern and doesn't support DDD. But it's feasible for simple problems.
- Not all projects require sophisticated domain modeling.
- Even domain modeling requires starting simply and iteratively extending.
- Every architectural pattern allowing separating a domain layer probably supports domain modeling.

Chapter 5: A model expressed in software.

- Associations between objects should be as constrained as possible. They should be qualified. For example:
  Use `Person president = usa.president(1790) // George Washington` instead
  of `Collection<Person> presidents = usa.presidents()`.
- Entities are objects that have its own long-running lifecycle.
- Entities should be uniquely identified in a system and beyond.
- Value Object represent characteristics of an object rather than behaviors. They can be reused, shared.
- They don't have a unique ID.
- Value Object should be immutable whenever possible. Changing object characteristics should replace them.
- Service represent operations of the domain, which client can execute and don't belong conceptually to any object.
- They should be stateless.
- They should use domain objects as parameters and returned types.
- Modules in a system should be highly cohesive, barely coupled.
- They should be named according to ubiquitous language, they're part of the domain.
- They should group domain concepts, not technical concepts.

Chapter 6: The life cycle of a domain object

- Aggregates help to define consistency and boundaries of entities and value objects existence.
- There is always a Root Entity of an aggregate with its ID.
- Only the aggregate root should be fetched from database. The rest of object should be available traversing from the root.
- Factories provide encapsulation taking responsibility for domain objects creation from clients.
- They are used for creating new and reconstitute aggregates.
- Standalone factories usually create the aggregate root.
- Abstract types should be provided as parameters instead of concrete implementations (Collection instead of ArrayList).
- For reconstitution, ID should be provided as parameter.
- Factories should create consistent aggregates with possibility of adding optional params.
- Repositories help clients accessing aggregates as they were conceptually sets of objects.
- The interface should be technology-agnostic, client is not interested in implementation details.
- Transactions should be handled by application layer.
- Repositories should return only aggregate roots.
- Repository delegates object creation to factory.

## Patterns of Enterprise Application Architecture (ISBN: 978-0-321-12742-6)

I expect this book to be a good addition to learning DDD, as it is also about designing domain model and infrastructure
around it.
It's also commonly referenced
in [Implementing Domain-Driven Design](https://github.com/marcinciapa/marcinciapa/blob/master/monthly/2023/MAR/README.MD#implementing-domain-driven-design-isbn-978-0-13-303990-0).
From preface: The book contains of two parts.
First explaining problems and solutions, which is expected to be read from the beginning to the end.
Second describing solutions in details, with examples, which is expected as a reference in time of need.
And that's my plan for this book: go through the first part and get back to references when necessary.

Chapter 1: Layers.

- The author focuses on separating system into 3 basic layers: Presentation, Domain logic, Data source.
- As benefits of layering he mentions: context separation, limited knowledge required in other layers to work on a
  single one, possibility to replace or duplicate particular layers.
- As disadvantages: performance impact of each layer.
  IMO, the approach is a bit archaic and today insufficient.

Chapter 2: Organizing business logic.

- The author mentions three patterns to organize business logic:
  *Transaction script* (contains of single procedure for individual use cases, simple and easy to understand, integrate
  easily with data mapping),
  *Domain model* (contains of objects and interactions between them, harder to understand, but easy to extend, harder to
  integrate with data mapping),
  *Table module* (similar to previous one, but instead of having one object for each row contains of objects of Record
  Sets we operate on, middle ground between previous two).
- The decision which model to pick depends on predicted complexity of the project, which is hard to know upfront.
- In all cases the author encourages creating a Service Layer (a concept similar to Application Layer in terms of DDD),
  which is a facade between clients and logic - handle transaction and security.
- It's possible to create use-case controllers, containing repeatable logic (a concept similar to Domain Services in
  terms of DDD).

Chapter 3: Mapping to relational databases.

- There are several approaches to interact with relational DBs:
  *Row Data Gateway* assumes single object for a single record in DB.
  *Table Data Gateway* assumes single object for a Record Set.
  Those two work well with Transactional Scripts and Table Module business logic patterns.
- If we work with Data Model the solution to pick should be either *Active Objects*,
  where the domain objects are self-responsible for interacting with DB,
  or a separate mapping layer responsible for translating DB entities to domain entities.
- There are several techniques ensuring DB operations correctness:
  *Unit of work* ensures ACID of operations.
  *Identity Map* is a cache ensuring that single objects can be safely modified in separate places.
  *Lazy Loading* helps to optimize fetching with joins.
- When using Table Data Gateway, the query procedures are part of the database objects.
  In case of Row Data Gateway or Active Records, we should deliver a separate finder classes for them.
- Sometimes it's more expensive to query DB multiple times than fetching too much data.
- There are different concepts for storing relations in DB and objects.
- When relations are represented as collections, we may be concerned about the order of elements in collection.
- Value objects should be represented as dedicated DB types.
- It's OK to store objects as LOBs as long as we don't plan to query by their properties.
- There are three approaches to handle inheritance in DB:
  *Single Table Inheritance*, where a single table supports the entire inheritance tree,
  *Class Table Inheritance*, where there are separate tables representing nodes in the inheritance tree,
  *Concrete Class Table Inheritance*, where a single table represents a leaf of the inheritance tree.
  The choice is a trade-off between data access speed and duplication of data.
- It's OK to mimic the database structure if we use Transactional Script or Table Module.
  But in case of Domain Model we should avoid it focusing on iterative upgrades of DB schema.
- Sometimes there is a need to map from multiple data sources.
  The simple solution of this problem is creating separate mapping for each data source, but it may lead to code
  duplications if the data sources contain similar data.
  An alternative is to create two-steps mapping, where first step maps domain to logical model, and second maps it to
  separate physical models.
- Mappings (fields and joins) may be sometimes extracted to the metadata file (XML).
  It gives the possibility to create *Query Objects* handled by *Repositories*, which is beneficial in particular for
  Domain Model pattern.
- We need connection objects to interact with DBs. They're closely tied to transactions and should not be shared between
  threads.
  They should be closed when no longer needed.
  Modern environments allow pooling connections, as creating them may be expensive.
- It's not always wise to use `select * ` statements, especially when we relay on the column numeric index.

Chapter 4: Web presentation.

- Delivering applications in web-browser form is a large change providing new challenges.
- There are two ways to structure applications: *Server Script* (Java servlets), which works well with interpreting the
  input, but
  is hard for non-programmers to maintain, or *Server Page*, which does the job of formatting response well.
- To handle two approaches simultaneously, *Model View Controller* patterns exists, with input controller handling
  requests,
  hand-off to Domain Model to execute the business logic and pass the result to View to present the screen.
- Sometimes there is a need to introduce a separate layer, *Application Controller*, especially where there are multiple
  screens to produce.
- For view there are two patterns to choose from: *Transform View* (XSLT), which translates the result of Domain Model
  to the
  appropriate response form, or *Template View* (JSP), which is a static template with placeholders for dynamic content.
- For either of these, one-step view may be applied, especially where there is one view per screen, or *Two-steps View*,
  which generates logical view from the separate View results and translates them into different screens, which may be
  formatted separately for embedding to external pages or different devices.
- For controller there are two patterns: *Page Controller*, where there are individual controllers for different
  requests,
  or *Front Controller*, where one controller creates different objects to handle different types of request.

Chapter 5: Concurrency.

- Concurrency is one of the most challenging aspects of software development. It's also hard to test.
  Luckily today we have tools allowing us taking the naive approach to concurrency, like server platforms or transaction
  managers.
  But they don't solve all problems, for example single application interaction may result in interactions with multiple
  databases.
- The most prominent concurrency problems in Enterprise Applications are *Inconsistent Reads* and *Lost Updates*.
  Sometimes they cannot be totally eliminated, so we have to balance between *correctness* and *liveness* (the ability
  to solve problems concurrently).
- Execution always takes place in context.
  In case of integrations with application we consider request or session context.
  For interactions with operating systems we distinguish light-weight threads or more isolated and heavy-weight
  processes.
  In terms of interactions with databases there is a concept of transaction (system transactions, when we speak about
  interactions between application and database,
  or business transactions in case of client-application interactions).
- Concurrency problems only take place when data is shared between clients. Partitioning, isolating data between threads
  solve these problems.
- Concurrency problems only take place when data is modified. Creating immutable copies of data solves these problems.
  But sometimes it's impossible to ensure data immutability. Identifying reading and writing threads helps to find the
  solution.
- If we have mutable data that can't be isolated, there are two concurrency controls to help:
  *Optimistic* (oriented for conflict solving, allowing multiple clients to work on the same copies of data
  and putting the responsibility to resolve conflicts to clients),
  or *Pessimistic* (oriented for conflict prevention, not allowing multiple clients to work on the same copy of data).
- Both solve concurrency problems: optimistic locking relies on versioning each part of data taking part in
  calculations,
  pessimistic relies on *Read Locks* (which can be acquired by multiple clients, but no write lock can be acquired in
  this time)
  and *Write Locks* (when acquired, no other client can acquire any type of lock).
  The choice should be made after assessing frequency and severity of conflicts.
- Using pessimistic locking may lead to *Deadlocks*, which can occur when clients locked resources and ask for more
  locks.
  It can be prevented using deadlock detecting software (picking victim when waiting for each other detected), lock
  timeouts
  (usually also resulting in a victim), locking all resources upfront (not always possible to predict all locks), or
  allowing
  further locks only on data placed later according to a sequence.
- Transactions are set of operations combined against a transactional resource (not always a database).
  They should assure *Atomicity*, *Consistency*, *Isolation* and *Durability* of operations.
- In terms of SQL we can sacrifice correctness for liveness reducing the isolation levels. But lower isolation levels
  introduce their problems:
  *Serializable* ensures that the result of transactions is the same as if they were executed sequentially (order is not
  guaranteed).
  *Repeatable Reads* introduces *Phantom Read* problem (visibility of inserted data).
  *Read Committed* introduces problem of *Unrepeatable Reads* (read results are not repeatable).
  *Read Uncommitted* introduces problem of *Dirty Reads* (partial result of other unfinished transaction or results of
  uncommitted transaction).
- Business transactions are sets of system transactions. They should have the same ACID properties.
  We should avoid long living transactions. Business transactions are usually coupled with sessions.
- Offline concurrency controls are *Optimistic Offline Locks*, *Pessimistic Offline Locks*,
  *Coarse-Grained Locks* (lock on group of data) and *Implicit Locks* (developer don't have to deal with locks
  explicitly).
  The differences are in complexity of implementation and handling conflicts.
- For server concurrency control we should pick one of the approaches: process-per-session, process-per-request,
  thread-per-request.
  The difference is in performance/cost, pooling possibility, isolation.
  We should prefer objects creation over reuse. Otherwise, we have to take care about data synchronization.

Chapter 6: Session state

- There are a lot of benefits from having a stateless server: new objects can be created when request is processed,
  heavy objects can be pooled, any instance of the application can process any request. But it's not always possible.
  Sometimes business transactions are combination of requests and the state needs to be kept between them.
- Session state needs to be committed to become a persisted *Record Data*.
- Session states are not always consistent (some business rules are checked in the commit stage) or isolated
  (state of Record Data taking part in calculation may change after the session was started).
- There are three ways of keeping session state: *Client Session State*, *Server Session State*, *Database Session
  State*.
  They all have their own set of disadvantages.
- Client Session State requires session state to be transferred with every request. It takes bandwidth.
  Session information must be properly secured on the client side.
- Server Session State requires that each request must be handled by the same instance of the application.
  Otherwise, session migration must be implemented.
  It also requires the session data to be stored in non-volatile memory.
- Database Session State requires storing Session State in the similar manner as Record Data.
  Session Data must be transformed to the form understandable for the application.

Chapter 7: Distribution strategies

- Distributing computation by object is a bad design.
  Remote interfaces should be coarse-grained, while the local processing should use fine-grained interfaces.
  We should also limit the remote calls and focus on computing locally to avoid performance burden.
  The solution to good distributed design is clustering - processing locally, but spreading computations between nodes.
- There are several situation when we have to distribute processes: server application should be separated from client
  applications,
  application should be separated from databases, web server should be separated from application server,
  vendor-specific solutions may enforce distribution, in justified cases application may also be distributed.
- To integrate remotely with coarse-grained interfaces, they should be combined in *Remote Facade* which translates
  them into fine-grained interfaces to process locally. To exchange the information we use *Data Transfer Objects*.
- There are benefits from using XML-Based SOAP services over HTTP: there is a lot of information that can be exchanged
  with a round trip.
  But there are also benefits of exchanging objects with RPC, which should be the first choice when data is exchanged
  between
  application using the same binary platform. SOAP should be considered when different platforms have to talk to each
  other.

Chapter 8: Putting it all together

- Architectural refactoring is hard, but not impossible, so the architectural decisions can be reverted.
- The first decision should be regarding the domain logic. Transaction Script should be used when the application is
  simple, it communicates well with relational data sources. Domain model is advised for complicated business logic,
  it requires experience, integrates poorly with databases (objects and relations don't go well together).
  Table Module is a middle ground. It integrates well with databases. It's not widely supported in Java ecosystem.
- Data source architecture should depend on domain logic architecture. For Transaction Scripts Row Data Gateway or Table
  Data Gateway
  is a good choice, we don't struggle with concurrency problems too much. For Table Module the only choice is Table Data
  Gateway.
  For Domain Model, if the model is relatively small, good choice may be Active Record pattern. For more complicated
  model - separate
  mapping layer with O/R mapping tool.
- Presentation design is independent on previous choices. Good choice is to go with MVC. When the application is
  document-oriented,
  it's wise to pick Page Controller. Frontend Controller works well if the presentation logic is complicated.
  When the view should be customised for different vendors, Two-Steps Mapping View should be considered.
- Execution should be local and run in a single process as much as possible.
- It's possible to have a good design without EJBs. Using POJOs and O/R mapping tools should suffice.
  However, there are some EJB patterns for each domain logic designs. Providing remote API in EJBs should be avoided.
  Local interfaces should be fine-grained, while remote interfaces should be coarse-grained.
- .NET supports Table Module pattern pretty well.
- Stored procedures have a lot of disadvantages, mainly they don't allow good code structure and are vendor-agnostic.
  They should be used to consider for performance improvements of particular operations.
- Web services should be used for communications between applications. They should not be used to distribute
  computations.
- There are other layering patterns than presented in the book, but they all base on the similar layering principles.

This concludes the first part of the book. IMO the solutions presented are a bit outdated (the book is 20 years old).
But some of them are still present in modern software development.
The first part is an introduction to patterns, it draws the context of problems that will be solved.
I'll give the second part a try learning more about at least few patterns.

Chapter 9: Domain logic patterns

- Transaction Scripts pattern assumes single procedure for single business operation. It's a good pattern for simple
  business logic.
  It works well with databases. Nevertheless, it's recommended to introduce a separate layer for business logic.
  It encourages code duplication.
  Each transaction may be instantiated on demand with use of command pattern, which improves concurrency, isolation.
- Domain Model pattern fits well to the complex business logic. Allows better code structuring. Should use fine-grained
  interfaces.
  It doesn't work well with relational databases, requires using Active Records pattern or O/R mapping layer. It
  requires practice to implement it well.
  It contains of interconnected objects, containing both data and behaviors. It shows the true power when combined with
  business rules strategies.
  Domain objects may be simple (Active Records should suffice), or rich (data source mapping layer required).
  The disadvantage is that it tends to introduce fat objects. Also navigating in the code is hard.
- Table Modules operate on record sets. The pattern offers the benefits of Object-Oriented approach for interacting with
  objects.
  The price is that it doesn't give the full benefits of objects (polymorphism doesn't work well).
  Data access objects may be instantiated (better isolation, works better with concurrency), or accessible with static
  methods.
  The client first interacts with Table Data Gateway, and then hands the flow over to Table Modules (for validation,
  transformation etc.).
  Inserting data requires providing dedicated methods. Accessing particular records requires providing ID to record
  sets.
- Service Layer defines boundaries of applications. It's a facade between clients and domain logic. It's usually
  coarse-grained.
  May be exposed for remote operations, but it's painful and requires dealing with Data Transfer Objects.
  Method signatures may use domain objects. Using it is optional when the flow doesn't interact with multiple
  transactional resources
  or there is only one kind of client.
  Services may be domain facades, simply passing the flow to domain objects, or operational, describing some
  interactions between domain objects.

## Clean Coder (ISBN: 978-0-13-708107-3)

Chapter 4: Coding.

- You have to feel mistakes while coding, the similar way you feel mistakes while typing.
- There are several factors to consider: it must work correctly, it must solve customers' problems, it must fit the
  overall project's structure, it must be readable to others.
  Coding is an intellectually demanding task. Don't start coding while disrupted.
- Don't lie to yourself that working hard overtime makes you more "professional". Sacrificing eating, sleeping etc.
  makes you less effective.
- Don't code when you feel an anxiety of the *personal background processes* (like after a fight with your spouse). You
  will create code to trash, or worse, to live with.
- Don't get into *The Flow*. You will generate code faster, but you will get back to it more often.
  Take a walk, pair with someone. The Flow is not where you want to be.
- Be careful with listening to music while working, especially when you're more focused on music than your code.
- If you are rude when interrupted, it may mean that you have a relation with The Flow, or problem with switching
  contexts.
  Pair programming or TDD helps with keeping the context.
- Sometimes you may feel the *writer's block*, when you sit in front of the monitor and the code just doesn't want to
  appear.
  The solution is pair programming - it's a psychological switch that enables you in a second.
  Other solution may be to read a fragment of novel or a poem - creative input produces creative output.
- Debugging is as expensive as writing software. The debugging time should be as short as possible.
  Luckily today we have a modern debugging tools. Debugging may be also shorten with using TDD.
- It's a marathon rather than a sprint. Be aware of your limits instead of fighting them.
  Sometimes ideas come to your mind in a car or while having shower, when the creative part of the brain is not used.
- You will be late. It's important to communicate estimate, meaning worst, best, and nominal case scenario.
  Don't relay on hope if your estimates show you can deliver only in optimistic circumstances. Otherwise, you will let
  yourself and your team down.
  Don't let yourself get rushed, protect your estimates. Negotiate cutting the scope instead.
  Working overtime may sometimes help, but it should be short-term, you have to personally afford it and a backup plan
  should be created.
  Definition of Done should be defined. Stretching it provides to false delivery.
- Helping others is your privilege and duty. Code with others to help them. Offer your help when you see someone in
  trouble.
- Asking for help is your privilege and duty. Don't refuse if someone offers you help. Ask for help when you stuck.
- It's your responsibility, as senior professional's, to train and mentor juniors.

Chapter 5: TDD.

- Programmers should not avoid TDD, as surgeons should not avoid washing hands.
  How can you call yourself a Professional if you don't know if your code works correctly?
- There are three rules of TDD:
    - You are not allowed to add production code if you don't have failing unit tests.
    - You are not allowed to add more unit tests if you have failing unit tests.
    - You are not allowed to add more production code that is sufficient to make unit tests pass.
- Benefits of using TDD: Enough *Certainty* to ship, Lower *Defect injection rate*, *Courage* to change the messy code,
  *Documentation* by examples of usage, *Design* enforced by interactions with other objects
- Using TDD is not an option, it's unprofessional not to use it.
- It's still possible to write bad code while using TDD, as it's possible to write bad tests.
- There are rare cases, when TDD doesn't work. No professional uses discipline which makes more harm than benefits.

Chapter 6: Practice.

- Comparing to the early days, today there is no long compile time, cycle time is extremely short.
  We have to make our decisions fast, react quickly. And doing something quickly requires practice.
- Practice for programmers is comparable to practicing martial arts. So are the techniques:
    - *Kata* is a series of repeatable sequence practices, which allows to train the body to react automatically.
      For software developers it may be a series of repeatable simple tasks, training keystroke, or some discipline (
      like TDD).
    - *Wasa* is a two-man Kata. For software developers there are prepared training for two people, allowing for example
      one person to write tests and the second to make them pass.
    - *Randori* is a simulated combat training. These can be some real, short, prepared problems, to solve in a group or
      alone.
- Usually we stuck in a narrow set of technologies and have little diversity of tools.
  That's why while practicing, it's good to pick a technology we don't work with every day.
  A good way to go is contributing to an open source project.
- It's our own responsibility to practice. We should spend our time and effort for it, it's not employee's
  responsibility.
  Concert-goers don't pay to listen to musicians practicing scales.

Chapter 7: Acceptance tests

- As the professional developers we are responsible for development and communications.
  Both should be precise and accurate.
- Developers and stakeholders communicate via requirements. Both sides want them to be precise.
  But the certain level of precision is not achievable: When demonstrating features to stakeholders, they often gain a
  different vision,
  change their minds, so the requirements change. Estimates are not precise by definition. There is a risk of late
  ambiguity,
  usually occurring when stakeholders don't agree with each other.
- To achieve an acceptable level of precision, *Definition of Done* should be created and formalized as a set of *
  Acceptance Tests*,
  which, when passing, mean all requirements implemented.
- Acceptance tests describe the purpose of the system. They should not be manual: the cost of executing manual tests
  multiple times is much higher than writing automatic test once.
- Acceptance tests should be created by stakeholders together with a QA. But stakeholders usually don't have time.
  They are often created by QA alone, Business Analyst or a Developer. QAs tend to focus on the negative cases.
  BAs tend to focus on the happy path. If it's a developer, it's important it's a different person, than the one
  who implements the feature.
- The first acceptance tests should be created at the beginning of the sprint, when the implementation starts.
  The implementation should start when they're created. Developer starts implementation with seeing all tests failing
  and watching them passing when the implementation progresses.
- It's developer's responsibility to negotiate better tests. The passive-aggressive attitude of satisfying bad tests
  is unacceptable. As a team we're all responsible for delivering error-free software.
- It may seem that writing Unit Tests and Acceptance Tests are redundant, but it's not. Unit tests go low level testing
  interactions between objects. Acceptance tests use API or UI as an entry point. But the main difference is that
  the primary purpose of acceptance tests is not testing, but documenting business requirements.
- Testing GUI is hard - GUI is constantly changing. It should be approached as testing GUI as API (testing controls
  using
  their IDs rather than position on the screen). We should prefer testing only the API, the same as GUI uses.
- Sometimes there is a specific need to test GUI. In such case API should be stubbed.
- Acceptance tests should be run daily as part of Continuous Integration. When the build fails, it should be an
  emergency
  for the team. They should stop what they're doing and focus on fixing the build.

# Projects

## ACTracker

- Activity comment feature was added to [UI](https://github.com/marcinciapa/actracker-ui/pull/8)
  and [Backend](https://github.com/marcinciapa/actracker-api/pull/35).
- Tags management was added to [Backend](https://github.com/marcinciapa/actracker-api/pull/36)
  and [UI](https://github.com/marcinciapa/actracker-ui/pull/9).
  Tags will be assigned to activities.
- Feeding tags to search index, including sending tags change notifications, was implemented
  in [actracker-api](https://github.com/marcinciapa/actracker-api/pull/37)
  and [actracker-search-feeder](https://github.com/marcinciapa/actracker-search-feeder/pull/19).
- Assigning tags to activities was introduced in [Backend](https://github.com/marcinciapa/actracker-api/pull/38)
  and [UI](https://github.com/marcinciapa/actracker-ui/pull/10)
- API for [Tags search](https://github.com/marcinciapa/actracker-api/pull/39) was introduced. It is used
  to [search for tags](https://github.com/marcinciapa/actracker-ui/pull/12) when adding/editing activity.
- Get all Tags API calls were [replaced](https://github.com/marcinciapa/actracker-ui/pull/13) in UI with search Tags and
  resolve Tags API.
- [Activity search API](https://github.com/marcinciapa/actracker-api/pull/41) was introduced.
  Get all Activities API calls were [replaced](https://github.com/marcinciapa/actracker-ui/pull/14) in the UI
- [API](https://github.com/marcinciapa/actracker-api/pull/44) for instant switching to new activity was exposed.
  It was used by UI to
  implement [switching to activity with selected tag](https://github.com/marcinciapa/actracker-ui/pull/17).
- Tag sets were introduced in [API](https://github.com/marcinciapa/actracker-api/pull/45)
  and [UI](https://github.com/marcinciapa/actracker-ui/pull/18).
  Tag sets group tags and allow quick switch to activity containing multiple tags.
- [API](https://github.com/marcinciapa/actracker-api/pull/47) for dashboards management was exposed.
  Dashboard management was implemented in [UI](https://github.com/marcinciapa/actracker-ui/pull/19).
- Charts were added to Dashboards in [API](https://github.com/marcinciapa/actracker-api/pull/48)
  and [UI](https://github.com/marcinciapa/actracker-ui/pull/20)
- Dashboard data presentation was implemented in [UI](https://github.com/marcinciapa/actracker-ui/pull/21).
[API](https://github.com/marcinciapa/actracker-api/pull/46) was exposed.
# Readings
## Patterns of Enterprise Application Architecture (ISBN:  978-0-321-12742-6)
I expect this book to be a good addition to learning DDD, as it is also about designing domain model and infrastructure around it.
It's also commonly referenced in [Implementing Domain-Driven Design](https://github.com/marcinciapa/marcinciapa/blob/master/monthly/2023/MAR/README.MD#implementing-domain-driven-design-isbn-978-0-13-303990-0).
From preface: The book contains of two parts.
First explaining problems and solutions, which is expected to be read from the beginning to the end.
Second describing solutions in details, with examples, which is expected as a reference in time of need.
And that's my plan for this book: go through the first part and get back to references when necessary.

Chapter 1: Layers.
- The author focuses on separating system into 3 basic layers: Presentation, Domain logic, Data source.
- As benefits of layering he mentions: context separation, limited knowledge required in other layers to work on a single one, possibility to replace or duplicate particular layers.
- As disadvantages: performance impact of each layer.
IMO, the approach is a bit archaic and today insufficient.

Chapter 2: Organizing business logic.
- The author mentions three patterns to organize business logic:
*Transaction script* (contains of single procedure for individual use cases, simple and easy to understand, integrate easily with data mapping),
*Domain model* (contains of objects and interactions between them, harder to understand, but easy to extend, harder to integrate with data mapping),
*Table module* (similar to previous one, but instead of having one object for each row contains of objects of Record Sets we operate on, middle ground between previous two).
- The decision which model to pick depends on predicted complexity of the project, which is hard to know upfront.
- In all cases the author encourages creating a Service Layer (a concept similar to Application Layer in terms of DDD),
which is a facade between clients and logic - handle transaction and security.
- It's possible to create use-case controllers, containing repeatable logic (a concept similar to Domain Services in terms of DDD). 
 
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
The simple solution of this problem is creating separate mapping for each data source, but it may lead to code duplications if the data sources contain similar data.
An alternative is to create two-steps mapping, where first step maps domain to logical model, and second maps it to separate physical models.
- Mappings (fields and joins) may be sometimes extracted to the metadata file (XML).
It gives the possibility to create *Query Objects* handled by *Repositories*, which is beneficial in particular for Domain Model pattern.
- We need connection objects to interact with DBs. They're closely tied to transactions and should not be shared between threads.
They should be closed when no longer needed.
Modern environments allow pooling connections, as creating them may be expensive.
- It's not always wise to use `select * ` statements, especially when we relay on the column numeric index.

Chapter 4: Web presentation.
- Delivering applications in web-browser form is a large change providing new challenges.
- There are two ways to structure applications: *Server Script* (Java servlets), which works well with interpreting the input, but
is hard for non-programmers to maintain, or *Server Page*, which does the job of formatting response well.
- To handle two approaches simultaneously, *Model View Controller* patterns exists, with input controller handling requests,
hand-off to Domain Model to execute the business logic and pass the result to View to present the screen.
- Sometimes there is a need to introduce a separate layer, *Application Controller*, especially where there are multiple screens to produce.
- For view there are two patterns to choose from: *Transform View* (XSLT), which translates the result of Domain Model to the
appropriate response form, or *Template View* (JSP), which is a static template with placeholders for dynamic content.
- For either of these, one-step view may be applied, especially where there is one view per screen, or *Two-steps View*,
which generates logical view from the separate View results and translates them into different screens, which may be 
formatted separately for embedding to external pages or different devices.
- For controller there are two patterns: *Page Controller*, where there are individual controllers for different requests,
or *Front Controller*, where one controller creates different objects to handle different types of request.

Chapter 5: Concurrency.
- Concurrency is one of the most challenging aspects of software development. It's also hard to test.
Luckily today we have tools allowing us taking the naive approach to concurrency, like server platforms or transaction managers.
But they don't solve all problems, for example single application interaction may result in interactions with multiple databases.
- The most prominent concurrency problems in Enterprise Applications are *Inconsistent Reads* and *Lost Updates*.
Sometimes they cannot be totally eliminated, so we have to balance between *correctness* and *liveness* (the ability to solve problems concurrently).
- Execution always takes place in context. 
In case of integrations with application we consider request or session context.
For interactions with operating systems we distinguish light-weight threads or more isolated and heavy-weight processes.
In terms of interactions with databases there is a concept of transaction (system transactions, when we speak about interactions between application and database,
or business transactions in case of client-application interactions).
- Concurrency problems only take place when data is shared between clients. Partitioning, isolating data between threads solve these problems.
- Concurrency problems only take place when data is modified. Creating immutable copies of data solves these problems.
But sometimes it's impossible to ensure data immutability. Identifying reading and writing threads helps to find the solution.
- If we have mutable data that can't be isolated, there are two concurrency controls to help: 
*Optimistic* (oriented for conflict solving, allowing multiple clients to work on the same copies of data
and putting the responsibility to resolve conflicts to clients), 
or *Pessimistic* (oriented for conflict prevention, not allowing multiple clients to work on the same copy of data).
- Both solve concurrency problems: optimistic locking relies on versioning each part of data taking part in calculations,
pessimistic relies on *Read Locks* (which can be acquired by multiple clients, but no write lock can be acquired in this time)
and *Write Locks* (when acquired, no other client can acquire any type of lock).
The choice should be made after assessing frequency and severity of conflicts.
- Using pessimistic locking may lead to *Deadlocks*, which can occur when clients locked resources and ask for more locks.
It can be prevented using deadlock detecting software (picking victim when waiting for each other detected), lock timeouts
(usually also resulting in a victim), locking all resources upfront (not always possible to predict all locks), or allowing 
further locks only on data placed later according to a sequence.
- Transactions are set of operations combined against a transactional resource (not always a database).
They should assure *Atomicity*, *Consistency*, *Isolation* and *Durability* of operations.
- In terms of SQL we can sacrifice correctness for liveness reducing the isolation levels. But lower isolation levels introduce their problems: 
*Serializable* ensures that the result of transactions is the same as if they were executed sequentially (order is not guaranteed).
*Repeatable Reads* introduces *Phantom Read* problem (visibility of inserted data).
*Read Committed* introduces problem of *Unrepeatable Reads* (read results are not repeatable).
*Read Uncommitted* introduces problem of *Dirty Reads* (partial result of other unfinished transaction or results of uncommitted transaction).
- Business transactions are sets of system transactions. They should have the same ACID properties.
We should avoid long living transactions. Business transactions are usually coupled with sessions.
- Offline concurrency controls are *Optimistic Offline Locks*, *Pessimistic Offline Locks*, 
*Coarse-Grained Locks* (lock on group of data) and *Implicit Locks* (developer don't have to deal with locks explicitly). 
The differences are in complexity of implementation and handling conflicts. 
- For server concurrency control we should pick one of the approaches: process-per-session, process-per-request, thread-per-request.
The difference is in performance/cost, pooling possibility, isolation.
We should prefer objects creation over reuse. Otherwise, we have to take care about data synchronization.

Chapter 6: Session state
- There are a lot of benefits from having a stateless server: new objects can be created when request is processed,
heavy objects can be pooled, any instance of the application can process any request. But it's not always possible.
Sometimes business transactions are combination of requests and the state needs to be kept between them.
- Session state needs to be committed to become a persisted *Record Data*.
- Session states are not always consistent (some business rules are checked in the commit stage) or isolated 
(state of Record Data taking part in calculation may change after the session was started).
- There are three ways of keeping session state: *Client Session State*, *Server Session State*, *Database Session State*.
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
- There are several situation when we have to distribute processes: server application should be separated from client applications,
application should be separated from databases, web server should be separated from application server,
vendor-specific solutions may enforce distribution, in justified cases application may also be distributed.
- To integrate remotely with coarse-grained interfaces, they should be combined in *Remote Facade* which translates
them into fine-grained interfaces to process locally. To exchange the information we use *Data Transfer Objects*.
- There are benefits from using XML-Based SOAP services over HTTP: there is a lot of information that can be exchanged with a round trip.
But there are also benefits of exchanging objects with RPC, which should be the first choice when data is exchanged between
application using the same binary platform. SOAP should be considered when different platforms have to talk to each other.

## Clean Coder (ISBN:  978-0-13-708107-3)
Chapter 4: Coding.
- You have to feel mistakes while coding, the similar way you feel mistakes while typing.
- There are several factors to consider: it must work correctly, it must solve customers' problems, it must fit the overall project's structure, it must be readable to others.
Coding is an intellectually demanding task. Don't start coding while disrupted.
- Don't lie to yourself that working hard overtime makes you more "professional". Sacrificing eating, sleeping etc. makes you less effective.
- Don't code when you feel an anxiety of the *personal background processes* (like after a fight with your spouse). You will create code to trash, or worse, to live with.
- Don't get into *The Flow*. You will generate code faster, but you will get back to it more often.
Take a walk, pair with someone. The Flow is not where you want to be.
- Be careful with listening to music while working, especially when you're more focused on music than your code. 
- If you are rude when interrupted, it may mean that you have a relation with The Flow, or problem with switching contexts.
Pair programming or TDD helps with keeping the context.
- Sometimes you may feel the *writer's block*, when you sit in front of the monitor and the code just doesn't want to appear.
The solution is pair programming - it's a psychological switch that enables you in a second.
Other solution may be to read a fragment of novel or a poem - creative input produces creative output.
- Debugging is as expensive as writing software. The debugging time should be as short as possible.
Luckily today we have a modern debugging tools. Debugging may be also shorten with using TDD.
- It's a marathon rather than a sprint. Be aware of your limits instead of fighting them.
Sometimes ideas come to your mind in a car or while having shower, when the creative part of the brain is not used.
- You will be late. It's important to communicate estimate, meaning worst, best, and nominal case scenario.
Don't relay on hope if your estimates show you can deliver only in optimistic circumstances. Otherwise, you will let yourself and your team down.
Don't let yourself get rushed, protect your estimates. Negotiate cutting the scope instead.
Working overtime may sometimes help, but it should be short-term, you have to personally afford it and a backup plan should be created.
Definition of Done should be defined. Stretching it provides to false delivery.
- Helping others is your privilege and duty. Code with others to help them. Offer your help when you see someone in trouble.
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

# Projects
## ACTracker
- Activity comment feature was added to [UI](https://github.com/marcinciapa/actracker-ui/pull/8) and [Backend](https://github.com/marcinciapa/actracker-api/pull/35).
- Tags management was added to [Backend](https://github.com/marcinciapa/actracker-api/pull/36) and [UI](https://github.com/marcinciapa/actracker-ui/pull/9).
Tags will be assigned to activities.
- Feeding tags to search index, including sending tags change notifications, was implemented in [actracker-api](https://github.com/marcinciapa/actracker-api/pull/37) and [actracker-search-feeder](https://github.com/marcinciapa/actracker-search-feeder/pull/19).
- Assigning tags to activities was introduced in [Backend](https://github.com/marcinciapa/actracker-api/pull/38) and [UI](https://github.com/marcinciapa/actracker-ui/pull/10)
- API for [Tags searching](https://github.com/marcinciapa/actracker-api/pull/39) was introduced. It is used to [search for tags](https://github.com/marcinciapa/actracker-ui/pull/12) when adding/editing activity.

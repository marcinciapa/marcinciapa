# Readings
## Patterns of Enterprise Application Architecture (ISBN:  978-0-321-12742-6)
I expect this book to be a good addition to learning DDD, as it is also about designing domain model and infrastructure around it.
It's also commonly referenced in [Implementing Domain-Driven Design](https://github.com/marcinciapa/marcinciapa/blob/master/monthly/2023/MAR/README.MD#implementing-domain-driven-design-isbn-978-0-13-303990-0).
From preface: The book contains of two parts.
First explaining problems and solutions, which is expected to be read from the beginning to the end.
Second describing solutions in details, with examples, which is expected as a reference in time of need.
And that's my plan for this book: go through the first part and get back to references when necessary.
- Chapter 1: Layers.
The author focuses on separating system into 3 basic layers: Presentation, Domain logic, Data source.
As benefits of layering he mentions: context separation, limited knowledge required in other layers to work on a single one, possibility to replace or duplicate particular layers.
As disadvantages: performance impact of each layer.
IMO, the approach is a bit archaic and today insufficient.
- Chapter 2: Organizing business logic.
The author mentions three patterns to organize business logic:
*Transaction script* (contains of single procedure for individual use cases, simple and easy to understand, integrate easily with data mapping),
*Domain model* (contains of objects and interactions between them, harder to understand, but easy to extend, harder to integrate with data mapping),
*Table module* (similar to previous one, but instead of having one object for each row contains of objects of Record Sets we operate on, middle ground between previous two).
The decision which model to pick depends on predicted complexity of the project, which is hard to know upfront.
In all cases the author encourages creating a Service Layer (a concept similar to Application Layer in terms of DDD), which is a facade between clients and logic - handle transaction and security.
It's possible to create use-case controllers, containing repeatable logic (a concept similar to Domain Services in terms of DDD).
- Chapter 3: Mapping to relational databases.
There are several approaches to interact with relational DBs:
*Row Data Gateway* assumes single object for a single record in DB.
*Table Data Gateway* assumes single object for a Record Set.
Those two work well with Transactional Scripts and Table Module business logic patterns.
If we work with Data Model the solution to pick should be either *Active Objects*, where the domain objects are self-responsible for interacting with DB, or a separate mapping layer responsible for translating DB entities to domain entities.
There are several techniques ensuring DB operations correctness:
*Unit of work* ensures ACID of operations.
*Identity Map* is a cache ensuring that single objects can be safely modified in separate places.
*Lazy Loading* helps to optimize fetching with joins.
When using Table Data Gateway, the query procedures are part of the database objects.
In case of Row Data Gateway or Active Records, we should deliver a separate finder classes for them.
Sometimes it's more expensive to query DB multiple times than fetching too much data.
There are different concepts for storing relations in DB and objects. 
When relations are represent as collections, we may be concerned about the order of elements in collection.
Value objects should be represented as dedicated DB types.
It's OK to store objects as LOBs as long as we don't plan to query by their properties.
There are three approaches to handle inheritance in DB:
*Single Table Inheritance*, where a single table supports the entire inheritance tree,
*Class Table Inheritance*, where there are separate tables representing nodes in the inheritance tree,
*Concrete Class Table Inherinance*, where a single table represents a leaf of the inheritance tree.
It's a trade-off between data access speed and duplication of data.
It's OK to mimic the database structure if we use Transactional Script or Table Module.
But in case of domain model we should avoid it focusing on iterative upgrades of DB schema.
Sometimes there is a need to map from multiple data sources.
The simple solution of this problem is creating separate mapping for each data source, but it may lead to code duplications if the data sources contain similar data.
An alternative is to create two-steps mapping, where first step map domain to logical model, and second maps it to separate physical models.
Mappings (fields and joins) may be sometimes extracted to the metadata file (XML).
It gives the possibility to create Query Objects handled by Repositories, which is beneficial in particular for Domain Model pattern.
We need connection objects to interact with DBs. They're closely tied to transactions and should not be shared between threads.
They should be closed when no longer needed.
Modern environments allow pooling connections, as creating them may be expensive.
It's not always wise to use `select * ` statements, especially when we relay on the column numeric index.

## Clean Coder (ISBN:  978-0-13-708107-3)
Chapter 4: Coding.
- You have to feel mistakes while coding, the similar way you feel mistakes while typing.
- There are several factors to consider: it must work correctly, it must solve customers' problems, it must fit the overall project's structure, it must be readable to others.
Coding is an intellectually demanding task. Don't start coding while disrupted.
- Don't lie to yourself that working hard overtime makes you more "professional". Sacrificing eating, sleeping etc. makes you less effective.
- Don't code when you feel anxiety of the *personal background processes* (like after a fight with your spouse). You will create code to trash, or worse, to live with.
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

# Projects
## ACTracker
- Activity comment feature was added to [UI](https://github.com/marcinciapa/actracker-ui/pull/8) and [Backend](https://github.com/marcinciapa/actracker-api/pull/35).
- Tags management was added to [Backend](https://github.com/marcinciapa/actracker-api/pull/36) and [UI](https://github.com/marcinciapa/actracker-ui/pull/9).
Tags will be assigned to activities.
- Indexing tags to search index, including sending tags change notifications, was implemented in [actracker-api](https://github.com/marcinciapa/actracker-api/pull/37) and [actracker-search-feeder](https://github.com/marcinciapa/actracker-search-feeder/pull/19).

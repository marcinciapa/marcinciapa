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
An alternative is to create two-steps mapping, where first step map domain to logical model, and second maps it to separate phisical models.
Mappings (fields and joins) may be sometimes extracted to the metadata file (XML).
It gives the possibility to create Query Objects handled by Repositories, which is beneficial in particular for Domain Model pattern.
We need connection objects to interact with DBs. They're closely tied to transactions and should not be shared between threads.
They should be closed when no longer needed.
Modern environments allow pooling connections, as creating them may be expensive.
It's not always wise to use `select * ` statements, especially when we relay on the column numeric index.


# Projects
## ACTracker
- Activity comment feature was added to [UI](https://github.com/marcinciapa/actracker-ui/pull/8) and [Backend](https://github.com/marcinciapa/actracker-api/pull/35).
- Tags management was added to [Backend](https://github.com/marcinciapa/actracker-api/pull/36) and [UI](https://github.com/marcinciapa/actracker-ui/pull/9).
Tags will be assigned to activities.

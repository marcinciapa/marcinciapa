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
- Chapter 2: Organizing business logic
The author mentions three patterns to organize business logic:
*Transaction script* (contains of single procedure for individual use cases, simple and easy to understand, integrate easily with data mapping),
*Domain model* (contains of objects and interactions between them, harder to understand, but easy to extend, harder to integrate with data mapping),
*Table module* (similar to previous one, but instead of having one object for each row contains of objects of Record Sets we operate on, middle ground between previous two).
The decision which model to pick depends on predicted complexity of the project, which is hard to know upfront.
In all cases the author encourages creating a Service Layer (a concept similar to Application Layer in terms of DDD), which is a facade between clients and logic - handle transaction and security.
It's possible to create use-case controllers, containing repeatable logic (a concept similar to Domain Services in terms of DDD).


# Projects
## ACTracker
- Activity comment feature was added to [UI](https://github.com/marcinciapa/actracker-ui/pull/8) and [Backend](https://github.com/marcinciapa/actracker-api/pull/35).
- Tags management was added to [Backend](https://github.com/marcinciapa/actracker-api/pull/36) and [UI](https://github.com/marcinciapa/actracker-ui/pull/9).
Tags will be assigned to activities.

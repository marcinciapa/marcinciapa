# Readings
### Patterns of Enterprise Application Architecture (ISBN:  978-0-321-12742-6)
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

# Projects
### ACTracker
Activity comment feature added:
- https://github.com/marcinciapa/actracker-ui/pull/8
- https://github.com/marcinciapa/actracker-api/pull/35

Managing tags:
- https://github.com/marcinciapa/actracker-api/pull/36

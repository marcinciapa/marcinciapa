# Readings

## The Practical Programmer (ISBN: 978-0-13-595705-9)

Chapter 2: A Pragmatic Approach

- Good design adapts to people who use it. Is easy to change. Common sense works well when identifying places which
  will change in the future. If using a library enforces changing the coding style, we should look someplace else.
- DRY principle is not only about code (sometimes code looks the same only accidentally, the knowledge is different, it
  only looks the same). Repeating knowledge in code and documentation invites maintenance nightmare.
- Orthogonality (cohesion) means that changes in one module doesn't depend on changes in different module. Cohesion
  improves productivity and reduces risk: modules are easier to change, support reusing, bugs are easier to fix,
  cohesive modules are better tested, vendor-agnostic, if a module is sick, it doesn't spread out. Design should be
  decoupled from changes in the real world (phone number as ID is a bad idea, don't rely on properties you can't
  control). Modules should be encapsulated (communicate using interfaces, hide implementation), global data should be
  avoided (passing context is better).
- If you rely on some fact, it's almost certain it will change. We should write reversible code. Decisions written in
  stone are usually only written in the sand on the beach. There is usually more than one way of solving a problem.
  External libraries should be abstracted with our own ports.
- Just as tracing bullets munition, to check if project goes the right direction, we should pick the most risky,
  uncertain requirements, implement it and release to customers as soon as possible (tracing bullet code, unlike
  prototype, is fully functional code). Users will appreciate taking part in the decision process, we should not be
  surprised when we miss, user will tell 'that's not what I meant'. Team will not have to invent things from the thin
  air. There will be a better feel of progress. There will be always something to demonstrate. Small code has low
  inertia - it's easier to change.
- Prototype is a disposable code, targeted to learn a lesson, identify risks, answer uncertainties. It should answer a
  precise question. Stakeholders may want to deploy them if we don't set the right expectations.
- Our understanding of the problem is limited to the language we use. Using the domain language means going to the next
  level.
- While estimating, units we use may communicate the level of precision (120 days sounds more precise than 3 months).
  Place the project in broader context, communicate if it is practical, rephrase questions (project X will cost x, but Y
  will cost less with only few takeaways). Estimate rarely should be a single number, we should communicate pessimistic,
  most likely and optimistic estimate. Measure and review the accuracy of your estimates to become more accurate. Ask
  someone who has already done something similar in the past. When asked for an estimate, answer 'I'll get back to you'.

# Projects

## Equino

-

## Actracker

# Tutorials

- [GitHub Actions: The Big Picture](https://github.com/marcinciapa-learning/github-actions-big-picture)
- [Authoring and Maintaining GitHub Actions Workflows](https://github.com/marcinciapa-learning/course-gh-actions)


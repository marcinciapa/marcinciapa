# Readings

## Implementing Domain-Driven Design (ISBN: 978-0-13-303990-0)

The goal of going through *Implementing Domain-Driven Design* for the second time is to refactor
[actracker-api](https://github.com/marcinciapa/actracker-api) according to book, to tactical patterns, if
applicable.

- [Executable edit operations](https://github.com/marcinciapa/actracker-api/pull/89) were introduced in actracker-api.
- [Refactor Tag sharing](https://github.com/marcinciapa/actracker-api/pull/90) to support atomic business operations.
  [Update UI](https://github.com/marcinciapa/actracker-ui/pull/52) to use them.
- [Refactor Dashboard](https://github.com/marcinciapa/actracker-api/pull/91) to support atomic business operations.
  [Update UI](https://github.com/marcinciapa/actracker-ui/pull/53) to use them.
- [Refactor Dashboard](https://github.com/marcinciapa/actracker-api/pull/92) to support charts atomic operations.
  [Update UI](https://github.com/marcinciapa/actracker-ui/pull/54) to use them.
- [Refactor metric values](https://github.com/marcinciapa/actracker-api/pull/93) to support charts atomic operations.
  [Update UI](https://github.com/marcinciapa/actracker-ui/pull/56) to use them.

## Clean Coder (ISBN: 978-0-13-708107-3)

Chapter 14: Mentoring, apprenticeship and craftsmanship

- Universities can give a great education, but it's also possible to finish college with diploma and nothing else.
  Anyway, universities do not prepare graduates for what they find in the business.
- Usually we remember our first working programs, because they changed our lives.
- We should not expect good things coming out if we leave inexperienced young people to themselves. It's required to
  support them with reasonable time of supervised practice.
- In our industry we're missing the mindset of training inexperienced juniors.
- We have to be aware, that shaping a new generation of professionals is our responsibility, not universities'.

The book is finished. I 100% recommend. It's about soft skills and mind set such desirable in our profession.
Must read to define a code of conduct in the software business.

## Optimizing Java. Practical Techniques for Improving JVM Application Performance (ISBN: 978-1-49-203927-3)

Chapter 1: Optimization and performance defined

- JVM performance is not normally distributed: we can't expect standard deviation, outliers can be significant.
  Measurement sampling is also problematic, because it's easy to miss irregular occurrences of performance problems.
- Measurement itself also adds the overhead.
- Measuring performance should be based on defining and then achieving non-functional requirements.
- When analyzing performance, there are patterns which can be observed often:
    - Performance elbow, when there is a point, where performance drastically suffers (like on latency function of
      requests count).
    - When the system suffers from unoptimized GC, there are moments, where resources amount allocation drops, because
      CPU is busy with GC processes.
    - According to Amdahl's Law, whenever there are serial processes in a program, such software can't scale linearly.
      The scalability is worse as there are more serial fragments.
    - Healthy JVM process manifests sawtooth distribution of memory allocation.
    - Program suffering from resource leaks usually show subtle at first, then significant performance drops.

# Projects

## ACTracker

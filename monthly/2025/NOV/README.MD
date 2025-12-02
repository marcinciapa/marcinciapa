# Readings

## The Pragmatic Programmer (ISBN: 978-0-13-595705-9)

Chapter 7: While You Are Coding

- Trust your instinct. Most people feel uncomfortable before being attacked on the street. The same applies to coding
  when we subconsciously feel something is not right. Instincts are brain's reactions to patterns.
- Some people feel overwhelmed by an empty project. Stop what you are doing. Do something mindless for a moment. Write a
  doodle about the code. Talk about it to someone or to a rubber duck. Start prototyping.
- When you notice strange approach in the code, continue doing it, it may appear that the original approach fits the
  solution better. The same rule applies to requirements, which may seem strange at first.
- Often procedures are invoked in a way they are not intended to. They work by coincident. Well-defined contracts help
  to program deliberately. If you have problems explaining to junior programmers what the code is doing, you probably
  code by coincident.
- Fastest algorithms are not necessarily the best ones. A good algorithm is consistent with a context.
- Instead of construction, software is more like gardening: the code continuously grow, it requires cutting and
  re-planting. Early refactoring is the cheapest one, it's like a growth: the more you wait the more costly and
  intrusive the surgery. When the refactoring is estimated to take a long time, create a task for it. Refactoring should
  be a day-to-day routine, taking small steps, low risk. Good automatic unit tests ensure that the behavior was not
  changed.
- Tests bring most value when we write them, not when we execute them. Such approach help reduce coupling and increase
  flexibility. There are people who are slaves to TDD, ending up with redundant tests, focusing on basic solution
  instead of the essence of why they program.
- Stay safe, there are a lot of actors waiting for your program's vulnerability. Keep the attack surface small (the
  amount of services users can execute). Use the least sufficient privileges. Never store fragile data in plaintext in
  database or credentials in version control.
- Don't unnecessarily introduce artificial constraint for passwords: disabling paste functionality, disabling password
  managers' integration, disallowing particular characters, it all make users think about bypassing the constraints. You
  must support long passwords with high entropy.
- Never do crypto yourself, experts will probably break unproven algorithms in minutes. You must be aware how hashes or
  rainbow tables work.
- Naming things is very important, it reveals our intentions. Naming convention should be consistent with language or
  community's conventions. Brain can understand words very fast, like it's easier to say the word loud instead of the
  color it was written with. It's not easy to explain to a new joiner that `getData` actually writes data to a file.
  Also, "amount" is not a good name for a parameter of `calculateDiscount` method: should the value be absolute, or a
  percentage?

## Projects - hardware

- [555-based latches - PCB production, Mk3, Mk4, Mk5, Mk6](https://github.com/mc-hw/0001-555-latches/pull/8)

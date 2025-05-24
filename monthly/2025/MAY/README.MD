# Readings

## The Practical Programmer (ISBN: 978-0-13-595705-9)

Chapter 4: Pragmatic Paranoia

- Perfect software doesn't exist. Pragmatic programmers turn it into advantage. They don't trust themselves, build
  defense against their mistakes.
- Design by contract defines yours and other side's responsibilities and rights. Contract contains of preconditions (
  which must be true for the routine to be called), post conditions (the state when the routine is done), class
  invariants. In some language it's possible to emulate design by contract using assertions, but they have limitations:
  in object-oriented language they are not inherited, they can also be disabled globally. Better way is to crash early:
  e.g. throw exception when negative value passed to a routine calculating square root.
- Errors give information. Instead of assuming that a situation will never happen, it's better to use assertion to get
  notified that something bad happened.
- **Catch and release is for fish**. It's better to avoid cascade of handling checked exceptions: code is not eclipsed
  by error handling, code is less coupled (adding another exception requires adding additional handling).
- When something impossible happen, program should terminate as soon as possible: **dead program does less harm than
  crippled one**.
- Impossible scenarios should be handled using assertions. However, assertions should not be used for regular error
  handling.
- Function or object responsible for allocating resources should be also responsible for releasing it.
- In software development our "headlines" are limited: we can't see far into the future. Always take small steps, check
  for feedback, adjust before proceeding. Don't take a step or task which is too big.
- Unit tests, user demo provide feedback.
- As car headlights have limited throw, we can only see hours, days at most in the future. Beyond that we take educated
  guesses or wild speculations. Usually the following behavior lead into trouble: estimating completion dates months in
  the future, planning a design for the future, guessing users' needs, guessing future tech availability.

# Projects

## Equino

-

## Actracker

-

# Tutorials

- 
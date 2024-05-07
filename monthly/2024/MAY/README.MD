# Readings

## Modern Software Engineering: Doing What Works to Build Better Software Faster (ISBN: 978-0-13-731491-1)

13 Managing Coupling

- Coupling is one of the most important aspect of managing complexity. It's defined as a degree of interdependence
  between modules. It measures how closely connected they are. If modules are fully decoupled, they can't communicate
  between each other. This may, or may not, be helpful. Coupling cannot, and should not be fully eliminated. But usually
  the cost of too loose coupling is lower than the cost of too tight coupling. Correct assessment is a trade-off.
- Coupling impacts an ability to create software reliably and repeatably. It should be well managed in systems and
  organisations, otherwise it impacts creating software at scale. On the organisational level it prevents effective
  releases. Modularity, cohesion, abstraction, separation of concerns reduce the coupling.
- There is an impact of coupling on scaling up development. There should be a limit of team size. Teams are
  __developmentally coupled__, releases need to be coordinated. __Continuous integration__ is a solution, it scales the
  development very well, but it requires an investment in automation, testing infrastructure to get feedback.
- Another strategy is __microservices__, but it's more complex and sophisticated. The approach is not new, it's been
  used in the past, microservices put these practices together. Good microservices should be aligned with a bounded
  context and be independently deployable (build, test, deploy them independently). We should not care which versions
  are deployed around, we should not test them together. Services should be cohesive. Microservices is an
  organizational-scaling pattern. If there is no need to scale up, there is no need for microservices. They help
  decoupling teams. They isolate code from changes in other places. It's a cost to take.
- Decoupling usually means writing more code. But it's not a good practice to optimise for typing, readability should be
  fundamental, we should optimise for thinking. High performance is achieved with simple, efficient predictable code
  understood by compilers and hardware.
- Abstraction and decoupling may be taken too far (universal database schema accepting all formats of data).
- DRY is efficient in the context of function or module, but having a single representation of one idea across the
  entire system increases coupling. Dependency management increases coupling. DRY should be avoided between pipelines.
  Code should not be shared between microservices.
- Synchrony is an illusion, it causes leaking abstractions across process boundaries. It increases complexity by
  dealing with errors. Network is asynchronous by nature. Processes should communicate asynchronously.
- Testable code provides a pressure on design to create more loosely coupled systems.
- It's possible to learn coding in few hours. Professional programming is about creating solutions to problems. It's
  hard when systems' complexity increases, coupling takes effect. There are two strategies to deal with coupling in
  organisations: __coordinated__ or __distributed__. Coordinated increases consistency. But coordination is a
  significant effort. Distributed approach is more in favor currently (microservices). Decision are intentionally
  distributed, teams work independently. Cost increases in terms of design. Avoiding testing services together later in
  the process reduces organisational coupling. Organisations which take microservices approach loose control
  consciously, otherwise microservices don't make sense.

## SQL Performance Explained (ISBN: 978-3-95-030782-5)

# Tutorials

- Professional Full Stack Developer: [CRUD - Update (Exercise)](https://github.com/marcinciapa/tutorials/pull/59)

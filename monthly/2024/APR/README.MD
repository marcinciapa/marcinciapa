# Readings

## Modern Software Engineering: Doing What Works to Build Better Software Faster (ISBN: 978-0-13-731491-1)

9 Modularity

- Modularity is the degree to which components of a system can be separated to benefit from flexibility.
- Modern software is complicated, exceeding human's capacity of keeping all details in mind. Thanks to modularity
  components may be reused in a variety of contexts. Each module needs to have an interface to communicate with other
  modules.
- Achieving good level of modularity requires skill and experience. We need to work on it, as good design is what
  software engineering is really about.
- The code with good modularity and separation of concerns is a better code to work with, regardless of language or used
  tools. It's more maintainable, has advantage of efficiency. Complexity increases cost, complex code is not nice to
  work on.
- Good code should be testable. If tests are difficult to write, the design is poor. It doesn't mean that TDD approach
  automatically implies better design.
- We can build the entire plane for testing and go flying, but this is a bad idea. How to compare the results? It's a
  waterfall approach, the complexity is expanded. It's better to test two airplanes in a controlled environment
  (temperature, pressure), which gives repeatable results. We also don't need an engine to test the rest of the
  airplane. We can make two models of the wings and test them. We can also make small model of each airfoil, using the
  same materials and techniques and compare them. These small pieces are modules. Such experiments will give only a
  partially true picture. But modularity provides a possibility to measure things we couldn't measure without it, a
  single module is more testable than the whole. Modularity provides greater control and precision of measurement.
- The software systems are complex and testing them as the whole is complex and lacks precision, reproducibility,
  control, visibility. We need to be sure what is the objective of test (what do we want to demonstrate). Modular
  testing strategy is detailed, demonstrates the correct working of separate parts. To do this we need to define points
  of measurement, places in the whole system where we can insert probes. To relay upon them, the measurements must be
  deterministic. It will have an impact on the design of the software and this is where the real value of engineering
  approach resides. Such architectural approach allows us to measure system at defined interfaces.
- The cause of lack of determinism in computer systems is concurrency (OS re-organizing the disk when it thinks it has
  spare time), for the same sequence of bytes and instructions the results are consistent all the time. The useful
  driver of modularity is isolating the concurrency. Systems are rarely built like this, but taking an engineering
  approach they can be.
- The fundamental part of our job should be testing (with automated testing) the systems we create. With such mindset we
  are encouraged by the extra work we have to do if we create modular systems incorrectly.
- Service is a code delivering functionality to other code hiding the details of implementation. Services can be thought
  as modules. Service represents a boundary. A system is not modular if the internals of services are exposed.
  Communication between services should be more guarded than communication inside them.
- We can build, test and deploy everything together, or parts of the system separately. The most scalable approach to
  software is to distribute it, reduce coupling and dependencies between teams, when each module is independent of other
  modules. This is what microservices are.
- You can't make a baby in a month with 9 women. But you can make 9 babies in nine months with nine women, which
  averages out a baby per month. When pieces are truly independent of each other, decoupled, we can parallelize all we
  want. Coupling introduces constraints on the degree to which we can parallelize. The best way to parallelize is to do
  it in a way where there is no need to re-integrate. The only benefit of microservices is organizational scalability,
  but this is a big advantage, as small teams are 4 times more productive than larger teams. The condition of
  efficiently creating high-quality work is limiting coupling. We need modular organizations as well as modular
  software.

## SQL Performance Explained (ISBN: 978-3-95-030782-5)

# Tutorials

- Getting Started With Python: [Data Structures](https://github.com/marcinciapa/tutorials/pull/38)
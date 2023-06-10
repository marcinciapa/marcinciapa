# Readings

## Domain-Driven Design: Tackling Complexity in the Hearth of Software (ISBN: 978-0-321-12521-5)

Chapter 14: Maintaining model integrity

- Each model exists withing some boundaries. **Bounded context** defines the range of applicability of the model,
  whilst **Context map** provides an overview of bounded context in the project and relations between them.
  Scope of the model should be explicitly defined.
- We should not worry about applicability of the model outside the context. Integration will require translations.
- **Splinters** inside the context should be identified and resolve. Here are the risks: **Duplications of concepts**,
  when two model elements represent the same concept, **False cognates**, when two people use the same terms thinking,
  that they're talking about the same thing, but they really don't.
- Sometimes developers are not aware of the original intent of model element end change it in a way making it
  unusable for the original purpose.
- XP may be helpful for maintaining model integrity inside a bounded context. But some **Continuous integration**
  should be applied to maintain it: all changes in the Context are merged and made consistent frequently, when
  splinters are identified, they're being fixed quickly.
- Concepts are integrated by constant communication between team members, exercising ubiquitous language. Implementation
  by frequent merge/build/test process.
- Integrations between bounded contexts must go through translations. Bounded context should be named according to the
  ubiquitous language. Integration points should be identified and described.
- **Shared kernel** is a subset of the domain which two teams agree to share. Using shared kernel still requires
  reimplementing the model and translating the data.
- In **Customer/Supplier** approach two bounded contexts should be split into upstream and downstream subsystems.
  Downstream team plays a role of customer to the upstream team. Automated acceptance tests should be added to upstream
  system's test suite.
- When upstream team has no motivation to provide for downstream, the downstream team is in trouble. In such
  situation **Conformist** pattern may be a good choice. It eliminates the complexity of translations by using the
  upstream model. Such approach tightens the dependency and limits the capabilities to upstream model.
- Models of legacy systems are usually weak. Even if it's well-designed, it operates on a different model.
  They may also use different platforms, different protocols, data models, which must be translated.
  The low-level interfaces limit the usefulness of the model. But integrating with external systems should not be
  avoided. An isolation layer should be created, serving functionality to the existing model. **Anticorruption layer**
  is not a mechanism sending messages to external system. It's a mechanism translating conceptual objects. It's usually
  represented as a set of services re-abstracting other system's behavior, together with combination of facades and
  adapters.
- If there are circumstances, when integration doesn't provide too much benefits, there is an option to go the separate
  way, without any integration. Such decision can be reverted with refactoring, but it's not an easy decision to change.
- **Open host** approach means defining a protocol which gives access to your subsystem by a set of services.
- Translation between models requires defining a common language. Open host approach requires standardized protocol for
  integrations. Language should be published and well-documented. Example: DB2 language.
- It's important to draw context maps. The decision where to put the boundaries should be made by the team, however it's
  also defined beyond the team. We should be part of the primary context we're working on.
- While integrating with an external system, going the separate way should be the first choice. If the integration is
  beneficial, the next considered choices should be Conformist or Anticorruption layer. Conformist limits the
  capabilities of integration, meaning restricting to extensions only, without the power to change the existing model.
  In case when more deep integration is required, if the external system is badly designed, the interface is small,
  building a translation layer in the anticorruption layer may be a better choice. It's always a tradeoff between a
  seamless integration and coordination/communication.
- For the newly designed system a single bounded context should be assigned to a system. As the system grows, a shared
  kernel or customer/supplier approach can be established.
- One team can maintain multiple bounded context, but it's hard to maintain one context by multiple teams. Breaking
  contexts is easy, but merging them/changing relations is challenging.

## Clean Coder (ISBN: 978-0-13-708107-3)

# Projects

## ACTracker

- Support for metric value chart (dashboards) was added in
  [actracker-api](https://github.com/marcinciapa/actracker-api/pull/76) and
  [actracker-ui](https://github.com/marcinciapa/actracker-ui/pull/43).
- Support for Tag duration chart (dashboards) was added in
  [actracker-api](https://github.com/marcinciapa/actracker-api/pull/77) and
  [actracker-ui](https://github.com/marcinciapa/actracker-ui/pull/44)
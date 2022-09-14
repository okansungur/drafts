### Domain Driven Design

__Ubiqutious language__: A common language created with domain experts business analyst product owner and developers. Removes confusion misunderstanding among team.

Layers of DDD are 
- User Interface(Presentation Layer) : External actor is human or non-human
  - UX Design: We use wireframing. Entire  interaction with the product is important.We look at overall.(SmartDraw, Mind Manager)
  - UI Design: Button, icon, typhography, color. Shortly Bones represent the code.Organs are UX design and Cosmetics senses and reactions are UI design

- Application Layer: No business rule or knowledge coordinates tasks and delegates them
- Domain Layer: Brain of the system.Business rules are here.
- Infrastructure Layer: Send message notifications, supports interaction with other layers.



__Aggregate__:An agregate is a business functionality that encapsulates the business rules and it is not divisible any more.
__Values__:


__Strategic Phase__: Identify bounded contexts and map them out in context map. Event Storming with domain experts business analyst product owner and developers.

__Tactical Phase__: Model bounded contexts according to business rules of the subdomain

Teams may chose TDD or BDD or can use both of them.

TDD: Is a methodology focuses on iterative development lifecycle. Add a test (Red) new test fails.Write enough code to pass the test(Green). Run test and refactor. So that in the end you save time.

BDD: (Cucumber) Gherkin Language is used. (Given When Then) BDD is concerned about result of higher level scenarios. TDD is focused on testing smaller pieces of functionality in isolation.TDD you should have the knowledge of programming language whereas in BDD you dont have to.
















Domain events should be inside a domain.(publisher-subscriber)PipelinR

Domain services can publish domain events and can interact with other domain services.

DDD has 2 phases Strategic phase and Tactical Phase.During strategic phase domain experts business analysts and developers gather  to brainstorm to identify business requirements and sharing knowledge..We call this an EventStorming session.Strategic domain design helps defining bounded contexts and defining microservices.
5 main types of Bounded contexts relationship:

__Partnership__: 2 way of coordination system.First team notify the second team about a change.And second team cooperate and adapt it.

__Shared Kernel__: The intersection can be an Autherization sample for this type of bounded contexts. The cost is important here



__Published Language__: API, JSON, XML, Protocol Buffer ...

__Conformist__ : Upstream teams model is accepted and conformed. It is not important whether the upstream contract is industry standart or not.Only it should meet the downstream needs.

__Anticorruption Layer__: Not willing to conform instead  an ACL is used . Especially when upstream contract changes often consumer is isolated and the model is not effected by changes. (Mitigate changes)

__Open Host Service__: It is revearsal of ACL producer/supplier has the freedom.Uses published languages for downstream.


__Separate ways__: Collobrating or duplication. (Ex:logs) Cost is important



ACL,OHS reduces system complexity.
Domain Driven Deesign and microservice based architecture is connected but there is a difference between microservice and bounded context. All microservices are bounded contexts but not all bounded contexts are microservices.As the bounded contexts represent the widest valid boundaries.Defining bounderies wider then their bounded context or boundaries that are smaller then microservices will not be a good idea.as we increase the complexity of system.


1-__Core subdomain__: It has highest priority.Consist complex business logic.Duplication can not be used here. ACL or OHS can be used.

2-__Supporting subdomain__:It is neccessary but not accepted as core domain 

3-__Generic subdomain__: User identity management is a good example


__Anemic Model__: it is a domain model where domain objects contain little or no business logic.We use private setters and entities must be self validated.We should also avoid constructors without parameters..In fact ORM tools create domain objects automatically which leads anemic model.So keeping OOP is a good way of avoiding anemic model.



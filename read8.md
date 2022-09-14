### aaa

An aagregate is a business functionality that encapsulates the business rules and it is not divisible any more.


ACL,OHS reduces system complexity.
Domain Driven Deesign and microservice based architecture is connected but there is a difference between microservice and bounded context. All microservices are bounded contexts but not all bounded contexts are microservices.As the bounded contexts represent the widest valid boundaries.Defining bounderies wider then their bounded context or boundaries that are smaller then microservices will not be a good idea.as we increase the complexity of system.

Anemic Model: it is a domain model where domain objects contain little or no business logic.We use private setters and entities must be self validated.We should also avoid constructors without parameters..In fact ORM tools create domain objects automatically which leads anemic model.So keeping OOP is a good way of avoiding anemic model.

1-Core Domain: It has highest priority.Consist complex business logic.Duplication can not be used here. ACL or OHS can be used.

2-Supporting subdomain:It is neccessary but not accepted as core domain 

3-Generic subdomain: User identity management is a good example


Domain events should be inside a domain.(publisher-subscriber)
domain services can publish domain events and can interact with other domain services

DDD has 2 phases Strategic phase and Tactical Phase.During strategic phase domain experts business analysts and developers gather  to brainstorm to identify business requirements and sharing knowledge..We call this an EventStorming session.Strategic domain design helps defining bounded contexts and defining microservices.
5 main types of Bounded contexts relationship:

Partnership: 2 way of coordination system.First team notify the second team about a change.And second team cooperate and adapt it.

Shared Kernel: The intersection can be an Autherization sample for this type of bounded contexts. The cost is important here



Published Language: 

Conformist : Upstream teams model is accepted and conformed. It is not important whether the upstream contract is industry standart or not.Only it should meet the downstream needs.

Anticorruption Layer: Not willing to conform instead  an ACL is used . Especially when upstream contract changes often consumer is isolated and the model is not effected by changes. (Mitigate changes)

Open Host Service: It is revearsal of ACL producer/supplier has the freedom.Uses published languages for downstream.


Separate ways: Collobrating or duplication. (Ex:logs) Cost is important






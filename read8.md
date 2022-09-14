### aaa

An aagregate is a business functionality that encapsulates the business rules and it is not divisible any more.


ACL,OHS reduces system complexity.
Domain Driven Deesign and microservice based architecture is connected but there is a difference between microservice and bounded context. All microservices are bounded contexts but not all bounded contexts are microservices.As the bounded contexts represent the widest valid boundaries.Defining bounderies wider then their bounded context or boundaries that are smaller then microservices will not be a good idea.as we increase the complexity of system.

Anemic Model: it is a domain model where domain objects contain little or no business logic.We use private setters and entities must be self validated.We should also avoid constructors without parameters..In fact ORM tools create domain objects automatically which leads anemic model.So keeping OOP is a good way of avoiding anemic model.

1-Core Domain: It has highest priority

2-Supporting subdomain:It is neccessary but not accepted as core domain 

3-Generic subdomain: User identity management is a good example


Domain events should be inside a domain.(publisher-subscriber)
domain services can publish domain events and can interact with other domain services

DDD has 2 phases Strategic phase and Tactical Phase.During strategic phase domain experts business analysts and developers gather  to brainstorm to identify business requirements and sharing knowledge..We call this an EventStorming session.Strategic domain design helps defining bounded contexts and defining microservices.
Bounded contexts communicate with

Open Host Service:

Published Language: 

Anticorruption Layer:

Separate ways: 

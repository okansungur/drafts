### Domain Driven Design

Domain Driven Design is about seeing the big picture.Simpler communication, and more flexibility.DDD aproach is object oriented.




__Ubiqutious language__: A common language created with domain experts business analyst product owner and developers. Removes confusion misunderstanding among team.

But it may be difficult to apply as the organizational structure is different. It plays a vital role in DDD's sucess

__Layers of DDD__ are :
- User Interface(Presentation Layer) : External actor is human or non-human In DDD domain is more important then UI/UX

  - UX Design: We use wireframing. Entire  interaction with the product is important.We look at overall.(SmartDraw, Mind Manager)
  - UI Design: Button, icon, typhography, color. Shortly Bones represent the code.Organs are UX design and Cosmetics senses and reactions are UI design

- Application Layer: No business rule or knowledge coordinates tasks and delegates them
- Domain Layer: Brain of the system.Business rules are here.
- Infrastructure Layer: Send message notifications, supports interaction with other layers.

__Bounded Context__ : A logical boundary of a domain where particular terms and rules apply consistently.Should be owned by 1 team only.

__Aggregate__:An agregate is a business functionality that encapsulates the business rules and it is not divisible any more.Create relationship between entity and value objects. Basic elemnt of transfer of data storage. A collection of entities or value objects that are related to each other through a root object.Mutable nad enforces consistency

An aggregate is a collection on entity and value objects that are only accessed through an aggregate root. 

__Aggregate Root__: An aggregate root is an entity through which the outside world and interacts with an aggregate.AR is an entity with a global id. Other entities within an aggregate have a local id (unique onlywithin the aggregate) Order is AR but orderline is an aggregate


__Values__: Immutable objects like color,currency, coordinates. A data thay may have sub values.Has no lifecycle and identity

__Entity__ : Objects with identity.A unique object within the domain and it is important for the domain. Mutable has lifecycle.

When building an invoice system adress is a value object but when building city electric lines adress is an entity.

__Repository__: directly correlates with an entity and represents an abstract persistent storage.(db,orm)

```
Aggregate: Street, House, Resident
Value: Doors
Entity: Street, House 

class Street{
  Collection<House> houses
}

class House {
  Number no
  Collection<Resident> residents
  Collection<Door> doors
}

class Resident{
   String familyName

}

```


__Context Map__ : It is a visual representation of relationships between different contexts of the system

__Domain Events__ :Domain events should be inside a domain.(publisher-subscriber)PipelinR. Used to notify other services when something happens. User logged in, opened a ticket, made a payment. etc.
__Domain Services__ : Stateless services implements a piece of business logic. Domain services can publish domain events and can interact with other domain services.


DDD has 2 phases Strategic phase and Tactical Phase.During strategic phase domain experts business analysts and developers gather  to brainstorm to identify business requirements and sharing knowledge..We call this an EventStorming session.Strategic domain design helps defining bounded contexts and defining microservices.

__Strategic Phase__: Identify bounded contexts and map them out in context map. Event Storming with domain experts business analyst product owner and developers.

__Tactical Phase__: Model bounded contexts according to business rules of the subdomain.Relies on programming language.

Teams may chose TDD or BDD or can use both of them.

TDD: Is a methodology focuses on iterative development lifecycle. Add a test (Red) new test fails.Write enough code to pass the test(Green). Run test and refactor. So that in the end you save time.

BDD: (Cucumber) Gherkin Language is used. (Given When Then) BDD is concerned about result of higher level scenarios. TDD is focused on testing smaller pieces of functionality in isolation.TDD you should have the knowledge of programming language whereas in BDD you dont have to.

Domains in DDD are:

1-__Core subdomain__: It has highest priority.Consist complex business logic.Duplication can not be used here. ACL or OHS can be used.

2-__Supporting subdomain__:It is neccessary but not accepted as core domain 

3-__Generic subdomain__: User identity management is a good example




There are 5 main types of Bounded contexts relationship:

__Partnership__: 2 way of coordination system.First team notify the second team about a change.And second team cooperate and adapt it.

__Shared Kernel__: The intersection can be an Autherization sample for this type of bounded contexts. The cost is important here


__Published Language__: API, JSON, XML, Protocol Buffer ...

__Conformist__ : Upstream teams model is accepted and conformed. It is not important whether the upstream contract is industry standart or not.Only it should meet the downstream needs.

__Anticorruption Layer__: Not willing to conform instead  an ACL is used . Especially when upstream contract changes often consumer is isolated and the model is not effected by changes. (Mitigate changes).Prevents the intrusion of foreignconcepts and models like gatekeeper.(Adapter or Facede pattern)

__Open Host Service__: It is revearsal of ACL producer/supplier has the freedom.Uses published languages for downstream.


__Separate ways__: Collobrating or duplication. (Ex:logs) Cost is important



Both ACL and OHS reduces system complexity.
Domain Driven Deesign and microservice based architecture is connected but there is a difference between microservice and bounded context. All microservices are bounded contexts but not all bounded contexts are microservices.As the bounded contexts represent the widest valid boundaries.Defining bounderies wider then their bounded context or boundaries that are smaller then microservices will not be a good idea.as we increase the complexity of system.





__Anemic Model__: it is a domain model where domain objects contain little or no business logic.We use private setters and entities must be self validated.We should also avoid constructors without parameters..In fact ORM tools create domain objects automatically which leads anemic model.So keeping OOP is a good way of avoiding anemic model.


Even there are failures like National Programme for Information Technology (NPfIT-2002). A large -scale project to modernize the system of health. The NPfIT was officially dismantled in September 2011. It cost the British government more than £10 billion. The failure is because of Project Planning,user needs,organizational culture  and Stakeholder Engagement.



___DDD Lite___:   Application layer is not involved.

- Interface
- Service
- Model
- Infrastructure


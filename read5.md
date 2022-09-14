### What is Externalizable Interface
The default serialization process is very slow as it is fully recursive, so whenever we try to serialize one object,  the serialization process tries to serialize all the fields of our class (except static and transient variables). So, if we have a  class with lots of variables present and we do not want to serialize all of them, we have to make all of those variables as transient, all these fields will always be assigned with default values. This makes the entire process very slow.Externalizable interface is used when we want to implement custom logic to serialize/deserialize an object. Externalizable interface extends the Serializable interface,
and it has two methods, writeExternal() and readExternal() which are used for serialization and de-serialization. This way, we can change the JVM’s default serialization behavior because while using Externalizable, we decide what to store in stream.

- Using Externalizable interface, we can implement custom logic for serialization and deserialization of object
- When using Externalizable, we have to explicitly mention what fields or variables we want to serialize
- When using Externalizable, a public no-arg constructor is required
- Using Externalizable, we can also serialize transient and static variables
- readExternal() method must read the values in the same sequence and with the same types as were written by writeExternal() method

### What is final keyword and where it can be used? 

- If you use final with a primitive type variable, then its value cannot be changed once assigned.
- If you use final with a method, then you cannot override it in the subclass.
- If you use final with class, then that class cannot be extended.
- If you use final with an object type, then that object cannot be referenced again.

### What are the different types of exceptions?
- Checked Exceptions: All exceptions other than RuntimeException and Error are known as Checked exception. These exceptions are checked by the compiler
at the compile time itself. E.g. when you are trying to read from a file, then compiler enforces us to handle the FileNotFoundException because it is possible that the file may not be present. Some other checked exceptions are SQLException , IOException etc
- Unchecked Exceptions: Runtime Exceptions are known as Unchecked exceptions. Compiler does not force us to handle these exceptions but as a programmer, it is our responsibility to handle runtime exceptions e.g. NullPointerException , ArithmeticException , ArrayIndexOutOfBoundException etc


### Difference between abstract class and interface
Abstract class can have both abstract and concrete methods but interface can only have abstract methods (Java 8 onwards, it can have default and
static methods as well)
- Abstract class methods can have access modifiers other than public but interface methods are implicitly public and abstract
- Abstract class can have final, non-final, static and non-static variables but interface variables are only static and final
- A subclass can extend only one abstract class but it can implement multiple interfaces
- An Abstract class can extend one other class and can implement multiple interfaces but an interface can only extend other interfaces

### What is the need for   static and default methods in an Interface from Java 8 onwards
- Consider there are 100 classes that are implementing one interface. Now you want to define one new method inside your interface. In this case you will have to change all the implementation classes to fulfill the interface contract. So, Java introduced default methods, here you can provide default implementation of that new method inside your interface.

### Why Java does not allow multiple inheritance? Describe Ambiguous situation also called Diamond Problem 
Why Java does not allow this : let us consider there are 2 parent classes having a method named hello() with same  signature and one child class is extending these 2 classes, if you call this hello() method which is same in both parents, which parent class method will get executed – it results into an ambiguous situation with compile time error.
```
public interface myinterface1 {
    default void sayHello() {
        System.out.println("myinterface1.sayHello()");
    }
}

 interface myinterface2 {
     default void sayHello() {
         System.out.println("myinterface2.sayHello()");
     }
}


public class demo implements myinterface1,myinterface2 {
//?????    
}


public class demo implements myinterface1,myinterface2 {
    @Override
    public void sayHello() {
        myinterface1.super.sayHello();
    }

    public static void main(String[] args) {
        new demo().sayHello();
    }
}
```

### Why String is Immutable?
1. String Pool : String Pool is possible only because String is Immutable in Java. String pool is a special storage area in Java heap. If the string is already
present in the pool, then instead of creating a new object, old object’s reference is returned. This way different String variables can refer to the same
reference in the pool, thus saving a lot of heap space also. If String is not immutable then changing the string with one reference will lead to
the wrong values to other string variables having the same reference.
2. Security : String parameters are used in network connections, database URL’s, username and passwords etc. Because String is immutable, these values can’t be changed. Otherwise any hacker could change the referenced values which will cause severe security issues in the application.
3. Multi-threading : Since String is immutable, it is  safe for multithreading. A single String instance can be shared across different threads. This avoids the use of synchronization for thread safety. Strings are implicitly thread-safe. 
4. Caching : The hashcode of string is frequently used in Java. Since string is immutable, the hashcode will remain the same, so it can be cached without worrying about the changes. This makes it a great candidate for using it as a Key in Map.
5. Class Loaders : Strings are used in Java ClassLoaders and since String is made immutable, it provides security that correct class is being loaded.


### Explain StringBuffer and StringBuilder 
Both StringBuffer and  StringBuilder classes are used for String  manipulation. These are mutable objects. But StringBuffer provides thread-safety as
all its methods are synchronized, this makes performance of StringBuffer slower as compared to StringBuilder.If you are in a single-threaded environment or don’t care
about thread safety, you should use StringBuilder. Otherwise, use StringBuffer for thread-safe operations. We  should use String class if we require immutability.

### What is Constructor Chaining in java
- this() : it is used to call the same class constructor
- super() : it is used to call the parent class constructor

### What is init block
init block is called instance initializer block.init block runs each time when object of the class is created.

Order of execution :

- static blocks of super classes
- static blocks of the class
- init blocks of super classes
- constructors of super classes
- init blocks of the class
- constructors of the class
```
public class myinit {
    {
        System.out.println("init block is executed");
    }
    public myinit(){
        System.out.println("Constructor is called");
    }
    public static void main(String[] args) {
        new myinit();
    }
}

```


### What is Serialization and Deserialization
Serialization is a mechanism to convert the state of an object into a byte stream while De-serialization is the reverse process where the byte stream is used to recreate the actual object in memory. The byte stream created is platform independent that means objects serialized on one platform can be deserialized on another platform. To make a Java Object serializable, the class must implement Serializable interface. Serializable is a Marker interface. ObjectOutputStream and ObjectInputStream  classes are used for Serialization and Deserialization in java


### Difference between Serializable and Externalizable
- Serializable is a marker interface which means it does not contain any method whereas Externalizable is a child interface of Serializable and it contains two
methods writeExternal() and readExternal()
- When using Serializable, JVM takes full responsibility for serializing the class object but in case of Externalizable, the programmer has full control over
serialization logic
- Serializable interface is a better fit when we want to serialize the entire object whereas Externalizable is better suited for custom serialization
- Default serialization is easy to implement but it comes with some issues and performance cost whereas in case of Externalizable, the programmer
has to provide the complete serialization logic which is a little hard but results in better performance
- Default serialization does not call any constructor whereas a public no-arg constructor is needed when using Externalizable interface
- When a class implements Serializable interface, it gets tied with default serialization which can easily break if structure of the class changes like
adding/removing any variable whereas using Externalizable, you can create your own binary format for your object

### Explain Volatile keyword in java
Volatile is a keyword in java, that can be applied only to variables. You cannot apply volatile keyword to classes and methods. Applying volatile to a shared
variable that is accessed in a multithreaded environment ensures that threads will read this variable from the main memory instead of their own local cache.
### Explain synchronized keyword
Synchronized method/block can only have one thread executing inside it, all the other threads trying to enter into the synchronized method/block will get blocked until the thread inside finishes its execution
### What is static synchronization
The purpose of static synchronization is to make the static data thread-safe
### What is Callable Interface  
Callable interface represents an asynchronous task which can be executed by a separate thread, it has one call() method, which returns a Future object
A Runnable can be executed by passing it into the Thread  class constructor but Thread class does not have a constructor that accepts a Callable, so Callable can be
executed only by submit() method of ExecutorService Interface

### How to make a class Immutable
String is an Immutable class in Java, i.e. once initialized its value never change. We can also make our own custom Immutable class, where the class object’s state will not change once it is initialized.

Benefits of Immutable class:

- Thread-safe: With immutable classes, we don’t have to worry about the thread-safety in case of multi-threaded environment as these classes are inherently thread-safe
- Cacheable: An immutable class is good for Caching because while we don’t have to worry about the value changes

How to create an Immutable class in java:
- Declare the class as final so that it cannot be extended
- Make all fields as private so that direct access to them is not allowed
- Make all fields as final so that its value can be assigned only once
- Don’t provide ‘setter’ methods for variables
- When the class contains a mutable object reference, 
1. While initializing the field in constructor, perform a deep copy
2. While returning the object from its getter method, make sure to return a copy rather than the actual object reference
```
public class Address {
    private String city;
    private String state;

    public Address(String city, String state) {
        this.city = city;
        this.state = state;
    }

    public String getCity() {
        return city;
    }

    public void setCity(String city) {
        this.city = city;
    }

    public String getState() {
        return state;
    }

    public void setState(String state) {
        this.state = state;
    }
    @Override
    public String toString() {
        return "Address{" +
                "city='" + city + '\'' +
                ", state='" + state + '\'' +
                '}';
    }
}
/*                      */

public final class Student {
    private final String name;
    private final int age;
    private final Address address;

    public Student(String name, int age, Address address) {
        this.name = name;
        this.age = age;
        this.address = address;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public Address getAddress() {
        return address;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", address=" + address +
                '}';
    }
}


```
### What is Singleton Design Pattern and how it can be implemented

Singleton pattern is used in:
- logging, caching, thread pool etc.
- other design patterns like Builder, Prototype, Abstract Factory etc.
- core java classes like java.lang.Runtime etc
To implement a Singleton pattern, we have different approaches but all of them have the below common concepts:
- private constructor to restrict instantiation of the class from other classes.
- private static variable of the same class that is the only instance of the class.
- public static method that returns the instance of the class, this is the global access point for outer world to get the instance of the singleton class.
1. Lazy Initialization
2. Eager Initialization
3. Thread Safe Singleton implementation
```
public class mysingleton {

    private mysingleton(){}
    private static mysingleton instance;
    public  static  mysingleton getInstance(){
        if(instance==null){
            synchronized(mysingleton.class){
                if(instance==null) {
                    instance = new mysingleton();
                }
            }
        }
    return instance;
    }

}
```

The reason of second if condition inside the synchronized block : suppose there are 2 threads and both called the getInstance() method at the same time, now
they will both be inside the first if condition as instance is null at this time, and the first thread which acquires the lock will create the object and as soon as it exits the synchronized block, other thread which was waiting, will acquire the lock and it will also create another object thus breaking the singleton pattern. This is why it is called “double-checked locking”.


### Decorator Pattern

Decorator pattern allows a user to add new functionality to an existing object without altering its structure. This type of design pattern comes under structural pattern as this pattern acts as a wrapper to existing class.This pattern creates a decorator class which wraps the original class and provides additional functionality keeping class methods signature intact.

```
public interface Shape {
    void draw();
}

public class Rectangle implements Shape {

    // Overriding the method
    @Override public void draw()
    {
        System.out.println("Shape: Rectangle");
    }
}

public class Circle implements Shape {
 
    @Override
    public void draw()
    {
        System.out.println("Shape: Circle");
    }
}

public abstract class ShapeDecorator implements Shape {
 
    protected Shape decoratedShape;
 
    public ShapeDecorator(Shape decoratedShape)
    {
          this.decoratedShape = decoratedShape;
    }
 


    public void draw() {
    decoratedShape.draw(); }
}

public class RedShapeDecorator extends ShapeDecorator {

   public RedShapeDecorator(Shape decoratedShape) {
      super(decoratedShape);		
   }

   @Override
   public void draw() {
      decoratedShape.draw();	       
      setRedBorder(decoratedShape);
   }

   private void setRedBorder(Shape decoratedShape){
      System.out.println("Border Color: Red");
   }
}

public class DecoratorPatternDemo {
   public static void main(String[] args) {

      Shape circle = new Circle();

      Shape redCircle = new RedShapeDecorator(new Circle());

      Shape redRectangle = new RedShapeDecorator(new Rectangle());
      System.out.println("Circle with normal border");
      circle.draw();

      System.out.println("\nCircle of red border");
      redCircle.draw();

      System.out.println("\nRectangle of red border");
      redRectangle.draw();
   }
}


Circle with normal border
Shape: Circle

Circle of red border
Shape: Circle
Border Color: Red

Rectangle of red border
Shape: Rectangle
Border Color: Red
```



### What is Dependency Injection
Inversion of Control (IoC) is a design principle used to invert different kinds of controls in object-oriented design to achieve loose coupling.

Dependency Inversion Principle (DIP) says that high-level modules should not depend on low-level modules. Both should depend on the abstraction.

Abstractions should not depend on details should depend on abstractions.

Dependency Injection (DI) is a design pattern used to implement IoC.It allows the creation of dependent objects outside of a class and provides those objects to a class through different ways. Using DI, we move the creation and binding of the dependent objects outside of the class that depends on them.
With Dependency Injection object is created automatically. Dependency injection is implemented as Constructor or getter /setter injection

### What is @SpringBootApplication Annotation

@SpringBootApplication is a combination of 3 different annotations: @Configuration: This annotation marks a class as a Configuration class in Java-based
configuration, it allows to register extra beans in the context or import additional configuration classes @ComponentScan: to enable component scanning
@EnableAutoConfiguration: to enable Spring Boot’s autoconfiguration feature These 3 annotations are frequently used together, so SpringBoot designers bundled them into
one single @SpringBootApplication, now instead of 3 annotations you just need to specify only one annotation on the Main class. However, if you don’t need one of these
annotation depending on your requirement, then you will have to use them separately 

### What default embedded server is given in spring boot web starter and how we can change the default embedded server to the one of our choice
The default embedded server is Tomcat, that comes with Spring boot web starter, if you want to change this, then use <exclusion> tag in web starter and add a
separate dependency of the server that you want.    
### Difference between @Component, @Controller, @Service, @Repository annotations    
@Component: it is a general purpose stereotype annotation which indicates that the class annotated with it, is a spring managed component.
@Controller, @Service and @Repository are special types of @Component
@Controller: the classes annotated with @Controller will act as Spring MVC controllers. DispatcherServlet looks for
@RequestMapping in classes that are annotated with @Controller. That means you cannot replace @Controller
with @Component, if you just replace it with @Component then yes it will be managed by Spring but it will not be able
to handle the requests.

@Service: the service layer classes that contain the business logic should be annotated with @Service. Apart from the fact that it is used to indicate that
the class contains business logic, there is no special meaning to this annotation, however it is possible that Spring may add some additional feature to @Service in
future, so it is always good idea to follow the convention.
    
@Repository: the classes annotated with this annotation defines data repositories. It is used in DAO layer classes. @Repository has one special feature that it catches platform specific exceptions and re-throw them as one of the Spring’s unified unchecked exception i.e. DataAccessException
   
### Which annotation is used for  binding the incoming json request to the defined pojo class
```
	@RequestMapping(value="/save",method=RequestMethod.POST)
	public String saveStudent(@RequestBody Student student) {
		studentService.addNewStudent(student);
		return "created student " + student.getStudentName();
	}
```
Spring uses Jackson library (that comes with spring-boot-starter-web) to map the json request to the pojo class. MappingJackson2HttpMessageConverter is used
when the incoming request is of content-type application/json    

### What is @Transactional annotation

Spring provides Declarative Transaction Management via @Transactional annotation. When a  method is applied with @Transactional, then it will execute
inside a database transaction. @Transactional annotation can be applied at the class level also, in that case, all methods of that class will be executed inside a database transaction.When @Transactional annotation is detected by Spring, then it creates a proxy object around the actual bean object. So, whenever the
method annotated with @Transactional is called, the request first comes to the proxy object and this proxy object invokes the same method on the target bean. These proxy objects can be supplied with interceptors. Spring creates a TransactionInterceptor and passes it to the generated proxy object. So, when the @Transactional annotated method is called, it gets called on the proxy object first, which in turn invokes the TransactionInterceptor that begins a transaction. Then the proxy object calls the actual method of the target bean. When the method finishes, the TransactionInterceptor commits/rollbacks the transaction.
One thing to remember here is that the Spring wraps the bean in the proxy, the bean has no knowledge of it. So, only the external calls go through the proxy. As for the internal calls (@Transactional method calling the same bean method), they are called using ‘this’. Using @Transactional annotation, the transaction’s
propagation and isolation can be set directly.
	
### CommandLineRunner vs ApplicationRunner
SpringBoot provides us with two interfaces, CommandLineRunner and ApplicationRunner. Both these runners have a run() method, which gets executed just after
the application context is created and before SpringBoot application startup Both of these interfaces provide the same functionality and 
the only difference between them is CommandLineRunner.run() method accepts String arr[], whereas ApplicationRunner.run() method accepts ApplicationArguments as argument
	
### What is Spring Data JPA
Spring Data is part of Spring framework. It provides an abstraction to significantly reduce the amount of boilerplate code required to implement data access layers.
It does this by providing Repositories. Repositories are just interfaces that do not have any implementation class and provide various database operations like save, delete etc. We have several repositories to choose from, CRUDRepository, JPARepository, PagingAndSortingRepository.
If you are thinking how Spring Data JPA saves us from writing boilerplate code, then consider an example, you have an Employee class and you are writing its Dao
interface and implementation class for performing some CRUD operations. You will also have other classes and similarly, you will write CRUD operations logic for these
classes as well. So, there is a lot of boilerplate code here. Spring Data JPA takes care of this, and provides you with a Repository interface which have all the common DAO operations. We just have to extend these Repositories and Spring Data JPA will provide the DAO implementation at runtime.
Spring Data JPA can also generate JPA queries on our behalf, we just have to follow certain method naming conventions and the database query will be automatically generated for us. For example, let’s say we have a table named Employee with id, name and age columns. If you want to write a query that will give the Employee object by name, then you can use Spring Data JPA, like: public EmployeeEntity  findByName(String name); 

### JPA LIST vs SET
When we need a set and never care about order, we use a Set. When for some reason order is important (ordered list, ordering by date, etc.), then a List.
For example, when we see a List, we know it comes sorted in some way, and that duplicates are either acceptable or irrelevant for this case. When we see a Set, we usually expect it to have no duplicates and no specific order (unless it's a SortedSet). When we see a Collection, we don't expect anything more from it than to contain some entities
	
### When to use @ ElementCollection (Collection Mapping)
 @ElementCollection is used when the existence of the child-entity is meaningless without the parent entity, when a parent entity is removed, your children will also be removed. Empolyee- Address
	
It means that the collection is not a collection of entities, but a collection of simple types (Strings, etc.) or a collection of embeddable elements (class annotated with @Embeddable).
	
@ElementCollection is mainly for mapping non-entities (embeddable or basic) while @OneToMany is used to map entities. So which one to use depend on what you want to achieve.
	
But the mapping as an @ElementCollection has a downside For small amount of data it is OK , therefore, recommend modeling an additional entity and a one-to-many association instead of an @ElementCollection.
This enables you to use lazy loading and to update these values independently of each other. Doing that requires only a minimum amount of code but provides much better performance.

	
### Unidirectional & Bidirectional  associations 
Prefer bidirectional associations: Unidirectional associations are more difficult to query. In a large application, almost all associations must be navigable in both directions in queries.
	
### Relations
1- **OneToOne** (one-to-one connection, that is, one Entity object can be associated with no more than one object of another Entity), Employee – Parking Lot
	
2- **OneToMany** (one-to-many connection, one Entity object can be associated with a whole collection of Entity) Department -employee
	
3- **ManyToOne** (many to one link, feedback for OneToMany), In a One-to-Many/Many-to-One relationship, the owning side is usually defined on the ‘many' side of the relationship. It's usually the side which owns the foreign key. Employee - Department
	
4- **ManyToMany** (many to many link) Each of which can be divided into two types:Employee - Project
	
5- Bidirectional
	
6- Unidirectional – a link to a link is set for all Entity, that is, in the case of OneToOne AB, Entity A has a link to Entity B, Entity B has a link to Entity A, Entity A is considered the owner of this link (this is important for cases of cascading data deletion , then deleting A will also delete B, but not vice versa). Undirectional- link to link is established only on one side, that is, in the case of OneToOne AB only Entity A will have link to Entity B, Entity B will not have link to A .
	
	
- For @ManyToMany, the collection type makes quite a difference as Sets perform better than Lists.
- For @OneToMany, unidirectional associations don't perform as well as bidirectional ones.
- For @OneToOne, a bidirectional association will cause the parent to be fetched eagerly if Hibernate cannot tell whether the Proxy should be assigned or a null value.

One to Many or Many to Many  for big data it is not good to use bidirectional.
You also need to use add/remove utility methods for bidirectional associations to make sure that both sides are properly synchronized.

Hibernate Dirty Checking checks objects state has changed or not without using hashcode or equalsWith FlushMode.Manuel we avoid it.
Cascadetype.Remove and **orphanremovel=true** Especially with OnetoMany associations (Producu=>Review) if we set a review null it is deleted from db.
	

### Two types of fetch strategies in JPA are LAZY & Eager
Don’t use FetchType.EAGER.Many-to-many associations only rarely represent parent-child associations, and you should better avoid cascading. That’s especially the case for CascadeType.REMOVE. Using it on both ends of a many-to-many association can bounce the cascade operation back and forth between the 2 tables until all records are removed.
	


	
### What is EntityManager and what are its main functions you can list?
EntityManager is an interface that describes an API for all basic operations on Enitity, data retrieval and other JPA entities. Essentially the main API for working with JPA. Basic operations:
1) For operations on Entity: persist (adding Entity under JPA control), merge (updating), remove (delete), refresh (updating data), detach (removing from management JPA), lock (blocking Enity from changes in other thread),
2) data Preparation: find (search and retrieval entity), createQuery, createNamedQuery, createNativeQuery , contains, createNamedStoredProcedureQuery, createStoredProcedureQuery

If you want to call a stored procedure that you declared with the annotation __@NamedStoredProcedureQuery__ you should use the method __createNamedStoredProcedureQuery__, not **createStoredProcedureQuery**. The latter is for when you don't want to use the annotation and in that case you have to register the parameters by hand.
	
3) Preparation of other entities JPA: getTransaction, getEntityManagerFactory, getCriteriaBuilder, getMetamodel, getDelegate
4) Work with EntityGraph: createEntityGraph, getEntityGraph
5) General operations on EntityManager or all Entities: close, isOpen, getProperties, setProperty, clear.
#### An Entity object has four lifecycle status: new, managed, detached, or removed. Their description
1) new – the object has been created but has not yet generated primary keys and has not yet been saved in the database,
2) managed – the object has been created, managed by JPA, has generated primary keys,
3) detached – the object has been created, but not managed (or no longer managed) JPA,
4) removed – the object is created, managed by JPA, but will be deleted after commit’a transaction.

	
### The difference between @JoinColumn and mappedBy and how to use them in a one-to-many bidirectional relationship.
The @JoinColumn annotation defines the actual physical mapping on the owning side. On the other hand, the referencing side is defined using the mappedBy attribute of the @OneToMany annotation.

### What are callback methods in JPA for? To which entities do callback method annotations apply? List seven callback methods (or, equivalently, annotations of callback methods)
Callback methods are used to call on certain Entity events (that is, add processing, for example, deletion of Entity methods by JPA), can be added to the entity class, to the mapped superclass, or to the callback listener class specified by the EntityListeners annotation (see previous question). There are seven callback methods (and annotations with the same names):
1) PrePersist
2) PostPersist
3) PreRemove
4) PostRemove
5) PreUpdate
6) PostUpdate
7) PostLoad
	
### What are the six types of locks (lock) described in the JPA specification (or what values does the enum LockModeType in JPA have)?
JPA has six types of locks, list them in order of increasing reliability (from the most unreliable and fast, to the most reliable and slow):
1) NONE – without blocking
2) OPTIMISTIC (or READ synonym for JPA 1) – optimistic blocking
3) OPTIMISTIC_FORCE_INCREMENT (or WRITE synonym, remaining from JPA 1) – optimistic lock with forced increase of the version field,
4) PESSIMISTIC_READ – pessimistic lock for reading,
5) PESSIMISTIC_WRITE – pessimistic lock for write (and read),
6) PESSIMISTICI-PII-IZIETIHI, IHIETI , I, I, I, I, I, I, I, I; on record (and th with forced increase in the field of versioning.

	
	
	
### What are the two types of cache (cache) you know in JPA and what are they for?
JPA talks about two kinds of caches (cache):
1) first-level cache (first-level cache) —caches data from a single transaction;
2) second-level cache (second-level cache) —caches data for more than one transaction. The JPA provider can, but is not required to implement work with the second-level cache. This kind of cache can save access time and improve performance, but the downside is the ability to get outdated data.

First level cache is session scoped and each entity is loaded once in a persistance context. It is terminated when the session is closed.
Second level  is SessionFactory scoped and shared by all sessions created with the same session factory.
if exist get from level 1 else from level 2 else from db. Avoid too many inserts and updates.
Cache concureny strategies are ReadOnly-NonstrictReadWrite-ReadWrite-Transactional
	For chaching Ehcache-2 >5.3  or Jcache, Hazelcast are used

	
### How can you change the fetch strategy settings of any Entity attributes for individual queries (query) or search methods (find), then if Enity has an attribute with fetchType = LAZY, but for a specific query you need to make it EAGER or vice versa?
For this, the EntityGraph API exists, it is used like this: using the NamedEntityGraph annotation for an Entity, it creates named EntityGraph objects that contain a list of attributes that need to change fetchType to EAGER, and then this name is specified in hits queries or the find method. As a result, the fetchType attribute of the Entity changes, but only for this request. There are two standard properties for specifying EntityGraph in hit:
1) javax.persistence.fetchgraph – all attributes listed in EntityGraph change fetchType to EAGER, all others to LAZY
2) javax.persistence.loadgraph – all attributes listed in EntityGraph change fetchType to EAGER, all others retain their fetchType (that is, if the attribute not specified in EntityGraph has fetchType was EAGER, then it will remain EAGER) With NamedSubgraph you can also change fetchType nested Entity objects.

### What is JPQL (Java Persistence query language) and how is it different from SQL?
JPQL (Java Persistence query language) is a query language, almost the same as SQL, but instead of the names and columns of database tables, it uses the names of the Entity classes and their attributes. The query parameters also use the data types of the attributes of the Entity, and not the database fields. Unlike SQL, JPQL has automatic polymorphism (see the next question). JPQL also uses functions that are not found in SQL: such as KEY (Map key), VALUE (Map value), TREAT (to cast a superclass to its heir object, downcasting), ENTRY, etc.

### What are the main new features in the JPA 2.1 specification compared with JPA 2.0 (list at least five or six new features)?
In the JPA 2.1 specification, there are:
1) Entity Graphs – a dynamic change fetchType mechanism for each request,
2) Converters – a mechanism for defining converters for specifying functions for converting Entity attributes into database fields,
3) DDL generation – automatic generation of tables, indexes and schemas,
4) Stored Procedures – a mechanism for calling stored procedures from JPA,
5) Criteria Update / Delete – a mechanism for invoking bulk updates or deletes using the Criteria API,
6) Unsynchronized persistence contexts – an opportunity to specify SynchronizationType,
7) New features in JPQL / Criteria API : arithmetic subqueries, generic database functions, join ON clause, TREAT function,
8) Dynamic creation of named queries (Details queries) Learn more about changing interfaces and APIs in JPA 2.1
	
### Hibernate  Merge and Update
	
there is a significant difference between the 2 methods. When you call the update method, Hibernate will only select the entity which you provided as a method parameter. But when you call JPA’s merge method, Hibernate will also select all associations with CascadeType.MERGE. You should, therefore, prefer JPA’s merge method if you reattach a huge graph of entities.
Both the MERGE and UPDATE statements are designed to modify data in one table based on data from another, but MERGE can do much more.
Whereas UPDATE can only modify column values you can use the MERGE statement to synchronize all data changes such as removal and addition of row.  The MERGE statement is structured to handle all three operations, INSERT, UPDATE, and DELETE, in one command.
##### JPA Cascade Types
The cascade types supported by the Java Persistence Architecture are as below:
	
CascadeType.PERSIST : cascade type presist means that save() or persist() operations cascade to related entities.
	
CascadeType.MERGE : cascade type merge means that related entities are merged when the owning entity is merged.
	
CascadeType.REFRESH : cascade type refresh does the same thing for the refresh() operation.
	
CascadeType.REMOVE : cascade type remove removes all related entities association with this setting when the owning entity is deleted.
	
CascadeType.DETACH : cascade type detach detaches all related entities if a “manual detach” occurs.
	
CascadeType.ALL : cascade type all is shorthand for all of the above cascade operations.
	
```
@Transactional
	public Student saveStudent(Student student) {
		entityManger.persist(student);
		return student;
	}

	@Transactional
	public Student updateStudent(Student student) {
		entityManger.merge(student);
		return student;
	}

```
	
### JPA vs Hibernate
The major difference between Hibernate and JPA is that Hibernate is a framework while JPA is API specifications. Hibernate is the implementation of all the JPA guidelines.
	
JPA									Hibernate
JPA is described in javax.persistence package.				Hibernate is described in org.hibernate package.
It describes the handling of relational data in Java applications.      Hibernate is an Object-Relational Mapping (ORM) tool that is used to save the Java objects in 									      the relational database system.
It is not an implementation. It is only a Java specification.		Hibernate is an implementation of JPA. Hence, the common standard which is given by JPA is followed by Hibernate.
	
It is a standard API that permits to perform database operations.	It is used in mapping Java data types with SQL data types and database tables.
As an object-oriented query language, it uses Java Persistence 
Query Language (JPQL) to execute database operations.			As an object-oriented query language, it uses Hibernate Query Language (HQL) to execute 									database operations.

	
	
### JPA @Transient 
Java's transient keyword is used to denote that a field is not to be serialized, whereas JPA's @Transient annotation is used to indicate that a field is not to be persisted in the database
```
public enum Gender { MALE, FEMALE, UNKNOWN }

@Entity
public Employee {
    private Gender g;
    private long id;

    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    public long getId() { return id; }
    public void setId(long id) { this.id = id; }

    public Gender getGender() { return g; }    
    public void setGender(Gender g) { this.g = g; }

    @Transient
    public boolean isMale() {
        return Gender.MALE.equals(g);
    }

```
	
### Attribute Overriding
You can override also other column properties (not just changing names). Ex:length of field

### Optimistic locking and automatic retry

- Optimistic locking is when you check if the record was updated by someone else before you commit the transaction.
- Pessimistic locking is when you take an exclusive lock so that no one else can start modifying the record.
- You cannot combine optimistic locking with the automatic retry. An optimistic locking assumes a manual intervention in order to decide whether to proceed with the update.
- If you don’t care about the previous updates applied to the record, don’t implement any locking strategy.

JPA provides us with two different optimistic lock modes (and two aliases):

OPTIMISTIC – it obtains an optimistic read lock for all entities containing a version attribute
	
OPTIMISTIC_FORCE_INCREMENT – it obtains an optimistic lock the same as OPTIMISTIC and additionally increments the version attribute value
	
READ – it's a synonym for OPTIMISTIC
	
WRITE – it's a synonym for OPTIMISTIC_FORCE_INCREMENT
	
We can find all types listed above in the LockModeType class.

### JPA optimistic locking vs synchronized Java method
Drawbacks of method synchronization:
1.	You will serialize the updates for all of the entity instances belonging to that entity class. Two concurrent threads will not be able to update two different instances.
2.	It does not work in cluster.
3.	Maintenance is more difficult. If the operations with the entity become more complex so it becomes possible to update the entity in multiple services, or you need to update instances of different such entity classes in the same transaction, you will have to synchronize them all.
4.	Point 3) increases the chances for deadlocks.
5.	You will have to ensure to execute the entire transaction holding the necessary synchronization locks, otherwise, if you release the lock before you commit the transaction, a concurrent transaction may obtain the lock and proceed with changing the same data.
6.	Depending on use cases, even if threads/transactions are not concurrent, without versioning you don't know whether the data has changed in the meantime (for example, you fetch data, modify something on the client based on the data, then somebody else changes that data, and then you save your modifications).
Think of synchronization as pessimistic locking: you have to reserve the lock before you start working as opposed to checking if you violated the lock only when you finished working (optimistic lock during commit). The two serve very different purposes:
- Optimistic lock is only there to guarantee there is no inconsistent database state (prevents overwriting data unknowingly) but it does not guarantee you won't get an OptimisticLockException an fail to change the db row. For this reason optimistic lock has much better performance.
- Pessimistic locking guarantees that you will never fail to write a row and you will know its most up to date values (as long as you synchronize everywhere where you use this entity)

Optimistic locking uses version attributes included in entities to control concurrent modifications on them.
	
	
	
### FetchMode vs. FetchType
In general, FetchMode defines how Hibernate will fetch the data (by select, join or subselect). FetchType, on the other hand, defines whether Hibernate will load data eagerly or lazily.
The exact rules between these two are as follows:
- if the code doesn't set FetchMode, the default one is JOIN and FetchType works as defined
- with FetchMode.SELECT or FetchMode.SUBSELECT set, FetchType also works as defined
- with FetchMode.JOIN set, FetchType is ignored and a query is always eager


```
	Customer.java
@Entity
@Table(name = "CUSTOMER")
public class Customer {
    @Id
    @GeneratedValue
    private Long id;

    @OneToMany
    @Fetch(FetchMode.SELECT)
    private Set < Orders > orders = new HashSet < Orders > ();

    //setters and getters
}

Orders.java
@Entity
@Table(name = "ORDERS")
public class Orders {

    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @ManyToOne
    @JoinColumn(name = "customer_id")
    private Customer customer;

    //setters and getters
}


```	
	





### NamedStoredProcedureQuery

```


CREATE OR REPLACE FUNCTION calculate(
    IN x double precision,
    IN y double precision,
    OUT sum double precision)
  RETURNS double precision AS
$BODY$
BEGIN
    sum = x + y;
END;
$BODY$
  LANGUAGE plpgsql



@NamedStoredProcedureQuery(
 name = "calculate", 
    procedureName = "calculate", 
    parameters = { 
        @StoredProcedureParameter(mode = ParameterMode.IN, type = Double.class, name = "x"), 
        @StoredProcedureParameter(mode = ParameterMode.IN, type = Double.class, name = "y"), 
        @StoredProcedureParameter(mode = ParameterMode.OUT, type = Double.class, name = "sum")
    }
)



StoredProcedureQuery query = this.em.createNamedStoredProcedureQuery("calculate");
query.setParameter("x", 1.23d);
query.setParameter("y", 4.56d);
query.execute();
Double sum = (Double) query.getOutputParameterValue("sum");
```

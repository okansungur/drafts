### What is Externalizable Interface
The default serialization process is very slow as it is fully recursive, so whenever we try to serialize one object,  the serialization process tries to serialize all the fields of our class (except static and transient variables). So, if we have a  class with lots of variables present and we do not want to serialize all of them, we have to make all of those variables as transient, all these fields will always be assigned with default values. This makes the entire process very slow.Externalizable interface is used when we want to implement custom logic to serialize/deserialize an object. Externalizable interface extends the Serializable interface,
and it has two methods, writeExternal() and readExternal() which are used for serialization and de-serialization. This way, we can change the JVM’s default serialization behavior because while using Externalizable, we decide what to store in stream.



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
4. 4. Caching : The hashcode of string is frequently used in Java. Since string is immutable, the hashcode will remain the same, so it can be cached without worrying about the changes. This makes it a great candidate for using it as a Key in Map.
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

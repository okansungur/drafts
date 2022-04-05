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

### Ambiguous situation also called Diamond Problem 

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









# drafts
Builder design pattern helps to hide the complex logic in the builder It is used to solve some of the problems with Factory and Abstract Factory design patterns when the Object contains a lot of attributes.

#### Creational Design Patterns

1. Singleton Pattern
2. Factory Pattern*
3. Abstract Factory Pattern
4. Builder Pattern*
5. Prototype Pattern

#### Structural Design Patterns
1. Adapter Pattern*
2. Composite Pattern
3. Proxy Pattern
4. Flyweight Pattern
5. Facade Pattern
6. Bridge Pattern
7. Decorator Pattern


#### Behavioral Design Patterns
- Template Method Pattern
- Mediator Pattern
- Chain of Responsibility Pattern
- Observer Pattern
- Strategy Pattern
- Command Pattern
- State Pattern
- Visitor Pattern
- Interpreter Pattern
- Iterator Pattern
- Memento Pattern
##### Miscellaneous Design Patterns 
1. DAO Design Pattern :The idea is to keep the service layer separate from the Data Access layer
2. Dependency Injection Pattern :Spring framework is built on the principle of dependency injection.
3. MVC Pattern :MVC Pattern is one of the oldest architectural patterns for creating web applications



__Builder Patterns  Sample__
 Builder pattern lets us construct  step-by-step of a complex object.
 We use builder to create immutable objects.In a multithread environment an immutable object can be shared between threads without explicit synchronization
 modifying it by two threads at the same time  is not possible. So instead of getters and setters we can use this pattern



```
class Car {


    private String make;
    private String model;
    private int year;
    
    private boolean isAutomaticTransmission;


    public String getMake() {
        return make;
    }

    public void setMake(String make) {
        this.make = make;
    }

    public String getModel() {
        return model;
    }

    public void setModel(String model) {
        this.model = model;
    }

    public int getYear() {
        return year;
    }

    public void setYear(int year) {
        this.year = year;
    }

    public boolean isAutomaticTransmission() {
        return isAutomaticTransmission;
    }

    public void setAutomaticTransmission(boolean automaticTransmission) {
        isAutomaticTransmission = automaticTransmission;
    }

    private Car(CarBuilder builder) {  //Constructor is private!!!!!!!!!
        this.make=builder.make;
        this.model=builder.model;
        this.year=builder.year;
        this.isAutomaticTransmission=builder.isAutomaticTransmission;
    }

    //Static  Builder Class !!!!
    public static class CarBuilder{

        private String make;
        private String model;
        private int year;


        private boolean isAutomaticTransmission;

        public CarBuilder(String make, String model,int year){
            this.make=make;
            this.model=model;
            this.year=year;
        }

        public CarBuilder setAutoTransmissionEnabled(boolean isAutomaticTransmission) {
            this.isAutomaticTransmission = isAutomaticTransmission;
            return this;
        }



        public Car build(){
            return new Car(this);
        }

    }

    @Override
    public String toString() {
        return "Car{" +
                "make='" + make + '\'' +
                ", model='" + model + '\'' +
                ", year=" + year +
                ", isAutomaticTransmission=" + isAutomaticTransmission +
                '}';
    }
}


 public class TestBuilderPattern {

     public static void main(String[] args) {

     Car car=new Car.CarBuilder("Vw","golf",2017).setAutoTransmissionEnabled(false).build();
         System.out.println(car.toString());

     }

 }

```

__Factory method design pattern sample__
 It allows the consumer to create new objects without having to know the details of how they're created. Define an interface or an abstract class and let the subclasses decide which object to instantiate. The factory pattern decides on certain criteria ("SMS" or "MAIL") what object should be created, so it is easy to maintain this logic in one place, instead of searching it through the whole system.
  Factory Pattern is one of the way implementing Dependency Injection.We minimize code duplication and we create  more customizable code by using an interface
 ex:  java.sql.DriverManager#getConnection()



```

 interface Notification {
    void sendNotification();
}

 class MailNotification implements Notification {

     @Override
     public void sendNotification() {
         System.out.println("sending mail notification...");
     }
 }

 class SMSNotification implements Notification {

     @Override
     public void sendNotification() {
         System.out.println("sending sms notification...");
     }
 }

 class NotificationFactory {
     public Notification createNotification(String method)
     {
         if (method == null || method.isEmpty())
             return null;
         switch (method) {
             case "MAIL":
                 return new MailNotification();
             case "SMS":
                 return new SMSNotification();

             default:
                 throw new IllegalArgumentException("Unknown method "+method);
         }
     }
 }


public class TestFactory {

    public static void main(String[] args) {
       NotificationFactory notificationFactory=new NotificationFactory();
       Notification notification=notificationFactory.createNotification("SMS");
       notification.sendNotification();

    }

}

```


//Structural Design Patterns - Adapter Pattern

 This pattern involves a single class which is responsible to join functionalities of independent or incompatible interfaces.
 
 
 ```
    JFrame f = new JFrame("Test");
    f.addWindowListener(new WindowAdapter() {

        @Override
        public void windowClosing(WindowEvent e) {
            System.out.println(e);
        }
    });
```


```
interface Journey {
    void travel();
}

class Car { // Car class
    public void drive() {
        System.out.println("The car is  moving");
    }
}

class Driver {  // Driver  class with a constructor taking Journey class parameter 

    private final Journey journey;

    public Driver(Journey journey) {
        this.journey=journey;
    }

    public void travel() {
        journey.travel();
    }
}

class  CarAdapter implements Journey {

    private final Car car;

    public CarAdapter() {
        car = new Car();
    }


    @Override
    public void travel() {
        car.drive();
    }
}

public class AdapterPattern {

    public static void main(String[] args) {

       
        Driver driver=new Driver(new CarAdapter());
        driver.travel();

    }

}


```


Singleton Design Patterns
- Eager initialization : Does not provide any options for exception handling. Not suitable if class uses a lot of resources such as file handling or database operations
- Static block initialization:  Not the best practice as the instance will already be created  even if we don't use it.
- Lazy Initialization:
- Thread Safe Singleton
```
public final class LazySingleton {
    private static volatile LazySingleton instance = null;

    // private constructor
    private LazySingleton() {
    }

    public static LazySingleton getInstance() {
        if (instance == null) {
            synchronized (LazySingleton.class) { 
               //Thread Safe
                // Double check
                if (instance == null) {
                    instance = new LazySingleton();
                }

            }
        }
        return instance;
    }
}

```

- Bill Pugh Singleton Implementation:  The most widely used approach for Singleton class as it doesn’t require synchronization
```
public class BillPughSingleton {
    private BillPughSingleton() {
    }

    private static class LazyHolder {
        private static final BillPughSingleton singleton = new BillPughSingleton();
    }

    public static BillPughSingleton getInstance() {
        return LazyHolder.singleton;
    }
}
```
- Reflection  destroys all Singleton Patterns 

```
public class ReflectionSingleton{

    public static void main(String[] args) {
        BillPughSingleton instanceOne = BillPughSingleton.getInstance();
        BillPughSingleton instanceTwo = null;
        try {
            Constructor[] constructors = BillPughSingleton.class.getDeclaredConstructors();
            for (Constructor constructor : constructors) {
                //destroy the singleton pattern
                constructor.setAccessible(true);
                instanceTwo = (BillPughSingleton) constructor.newInstance();
                break;
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        System.out.println(instanceOne.hashCode());
        System.out.println(instanceTwo.hashCode());
    }

```

-  Enum Singleton : to overcome this Enum can be used but it doesnt have lazy init
```
public enum MySingleton {
    INSTANCE;
    private MySingleton() {
        System.out.println("Here");
    }
/*
  public static void main(String[] args) 
System.out.println(MySingleton.INSTANCE.hashCode());
System.out.println(MySingleton.INSTANCE.hashCode());
*/
```


- Serialization and Singleton :  If we serialize a singleton class  whenever we deserialize it, it will create a new instance of the class like in reflection.To overcome the problem we use readResolve() method 

```

public class SingletonSerialized implements Serializable{

    private static final long serialVersionUID = -7604766932017737115L;

    private SingletonSerialized(){}

    private static class SingletonHelper{
        private static final SingletonSerialized instance = new SingletonSerialized();
    }

    public static SingletonSerialized getInstance(){
        return SingletonHelper.instance;
    }

 //Overcome the problem!!!
    protected Object readResolve() {
        return getInstance();
    }

}


/*
//Test the code
       SingletonSerialized firstInstance = SingletonSerialized.getInstance();
        ObjectOutput out = new ObjectOutputStream(new FileOutputStream(
                "test.abc"));
        out.writeObject(firstInstance);
        out.close();

       ObjectInput in = new ObjectInputStream(new FileInputStream("test.abc"));
        SingletonSerialized secondInstance = (SingletonSerialized) in.readObject();
        in.close();
        System.out.println("instanceOne hashCode="+firstInstance.hashCode());
        System.out.println("instanceTwo hashCode="+secondInstance.hashCode());

*/
```


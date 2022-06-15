# Java Design Patterns
Builder design pattern helps to hide the complex logic in the builder It is used to solve some of the problems with Factory and Abstract Factory design patterns when the Object contains a lot of attributes.
 With it we make our code more flexible, reusable and maintainable.

- The Creational pattern focuses on object creation; the Structural pattern relies on the relationship between objects, and Behavioural builds its communication between objects
#### Creational Design Patterns

1. Singleton Pattern*
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
7. Decorator Pattern*


#### Behavioral Design Patterns
- Template Method Pattern(*)
- Mediator Pattern
- Chain of Responsibility Pattern
- Observer Pattern*
- Strategy Pattern
- Command Pattern*
- State Pattern
- Visitor Pattern
- Interpreter Pattern
- Iterator Pattern
- Memento Pattern
##### Miscellaneous Design Patterns 
1. DAO Design Pattern :The idea is to keep the service layer separate from the Data Access layer
2. Dependency Injection Pattern :Spring framework is built on the principle of dependency injection.
3. MVC Pattern :MVC Pattern is one of the oldest architectural patterns for creating web applications



#### Builder Patterns (Creational)
 Builder pattern lets us construct  step-by-step of a complex object.
 We use builder to create immutable objects.In a multithread environment an immutable object can be shared between threads without explicit synchronization
 modifying it by two threads at the same time  is not possible. So instead of getters and setters we can use this pattern
 * private constructor with a static builder class inside



```
//******** Car  *********
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

//******** TestBuilderPattern  *********
 public class TestBuilderPattern {

     public static void main(String[] args) {

     Car car=new Car.CarBuilder("Vw","golf",2017).setAutoTransmissionEnabled(false).build();
         System.out.println(car.toString());

     }

 }

```

#### Factory method (Creational)
 It allows the consumer to create new objects without having to know the details of how they're created. Define an interface or an abstract class and let the subclasses decide which object to instantiate. The factory pattern decides on certain criteria ("SMS" or "MAIL") what object should be created, so it is easy to maintain this logic in one place, instead of searching it through the whole system.
  Factory Pattern is one of the way implementing Dependency Injection.We minimize code duplication and we create  more customizable code by using an interface
 ex:  java.sql.DriverManager#getConnection()
 
* Notification sms or mail with if


```
//******** Notification  *********
 interface Notification {
    void sendNotification();
}

//******** MailNotification  *********

 class MailNotification implements Notification {

     @Override
     public void sendNotification() {
         System.out.println("sending mail notification...");
     }
 }

//******** SMSNotification  *********

 class SMSNotification implements Notification {

     @Override
     public void sendNotification() {
         System.out.println("sending sms notification...");
     }
 }

//******** NotificationFactory  *********

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

//******** TestFactory  *********

public class TestFactory {

    public static void main(String[] args) {
       NotificationFactory notificationFactory=new NotificationFactory();
       Notification notification=notificationFactory.createNotification("SMS");
       notification.sendNotification();

    }

}

```




#### Singleton Design Patterns (Creational )
- Eager initialization : Does not provide any options for exception handling. Not suitable if class uses a lot of resources such as file handling or database operations
- Static block initialization:  Not the best practice as the instance will already be created  even if we don't use it.
- Lazy Initialization:
- Thread Safe Singleton

For logging purposes or reading a conf file single instance  with a global access point to that instance (ex:db)

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

__Bill Pugh Singleton Implementation__:  The most widely used approach for Singleton class as it doesn’t require synchronization
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
__Reflection  destroys all Singleton Patterns __

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

__Enum Singleton__ : To overcome this Enum can be used but it doesnt have lazy init.
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


__Serialization and Singleton__ :  If we serialize a singleton class  whenever we deserialize it, it will create a new instance of the class like in reflection.To overcome the problem we use __readResolve()__ method 

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



#### Adapter Pattern (Structural Design Patterns)

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

//******** Journey  *********

interface Journey {
    void travel();
}

//******** Car  *********

class Car { // Car class
    public void drive() {
        System.out.println("The car is  moving");
    }
}

//******** Driver  *********

class Driver {  // Driver  class with a constructor taking Journey class parameter 

    private final Journey journey;

    public Driver(Journey journey) {
        this.journey=journey;
    }

    public void travel() {
        journey.travel();
    }
}

//******** CarAdapter  *********

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

//******** AdapterPattern  *********

public class AdapterPattern {

    public static void main(String[] args) {      
        Driver driver=new Driver(new CarAdapter());
        driver.travel();

    }

}


```



#### Decorator Pattern (Structural )


You should use the Decorator Pattern to add new behaviors by placing them inside special wrapper objects,   at runtime

*pizza getPizza()*

```


//******** Piza  *********
public interface Pizza
{
    public void getPizza();
}


//******** MexicanPizza  *********
public class MexicanPizza  implements Pizza {
    @Override
    public void getPizza() {
        System.out.print("Mexican Pizza");
    }
}


//******** NeapolitanPizza  *********
public class NeapolitanPizza implements Pizza
{
    @Override
    public void getPizza()
    {
        System.out.print("Neapolitan  Pizza");
    }
}


//******** PizzaDecorator  *********
public abstract class PizzaDecorator implements Pizza
{
protected Pizza pizza;

    public PizzaDecorator(Pizza pizza)
    {
        this.pizza=pizza;
    }

    @Override
    public void getPizza() {
        this.pizza.getPizza();
    }
}


//******** PizzaWithSauce  *********
public class PizzaWithSauce extends PizzaDecorator {

    public PizzaWithSauce(Pizza pizza) {
        super(pizza);
    }

    @Override
    public void getPizza()
    {
        pizza.getPizza();
        addSauce();

    }
    private void addSauce()
    {
        System.out.print(" with Garlic Sauce");
    }

}




//******** DecoratorPatternTest  *********

public class DecoratorPatternTest {
    public static void main(String[] args) {
        Pizza mypizza=new PizzaWithSauce(new MexicanPizza());
        mypizza.getPizza();
    }
}



```



#### Template Pattern (Behavioral )

Template Method are generally used in frameworks and lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.
abstract class or interface and a class inheriting and overriding like DPI

*Template db open close*

```
//******** MyConnection *********
public abstract  class MyConnection {
//we can use abstract or interface
    abstract void Init();
     abstract void Open();
     abstract void Close();
    //template method
    public final  void executeSql(){
        Init();
        Open();
        Close();
    }
}

//******** MyPostgre *********
public class MyPostgre extends MyConnection {
    Connection con = null;
    @Override
    void Init() {
        try {
           con = DriverManager.getConnection("jdbc:postgresql://localhost:5432/postgres",
                    "postgres", "abc123");
            System.out.println("Connection is initialized !");
        } catch (Exception ex) {
            ex.printStackTrace();
       }
    }

    @Override
    void Open()  {
        try {
        Statement statement=con.createStatement();
        ResultSet resultSet=statement.executeQuery("  select now();");
        if(resultSet.next())
            System.out.println(resultSet.getString(1));
        } catch (Exception ex) {
            ex.printStackTrace();
        }
    }

    @Override
    void Close() {
        try{
        if(con!=null){
            con.close();
            System.out.println("Connection is closed !");
        }
        } catch (Exception ex) {
            ex.printStackTrace();
        }

    }
}


//******** TemplatePattern  *********

public class TemplatePattern {
    public static void main(String[] args) {
        MyConnection myConnection=new MyPostgre();
        myConnection.executeSql();
        myConnection.Open();//Exception
    }
}


```




#### Command Pattern (Behavioral )

We can add new commands without changing existing code. Reduces coupling between the invoker and receiver of a command.But it will 
increase  the number of classes for each individual command.

*lights on off with command execute and a controller implementing command


```

interface Command
{
    public void execute();
}


//******** LightningSystem  *********

public class LightningSystem {

        public void on()
        {
            System.out.println("Light is on");
        }
        public void off()
        {
            System.out.println("Light is off");
        }
    }


//******** LightOnCommand  *********

public class LightOnCommand implements Command
{
    LightningSystem light;
    public LightOnCommand(LightningSystem light)
    {
        this.light = light;
    }
    public void execute()
    {
        light.on();
    }
}

//******** LightOffCommand  *********

public class LightOffCommand implements Command {

    LightningSystem light;

    public LightOffCommand(LightningSystem light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.off();
    }
}



    
    
//******** Controller  *********

public  class Controller {

    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void executeCommand() {
        command.execute();
    }
}



//******** CommandPatternTest  *********


public class CommandPatternTest {
    public static void main(String[] args) {
        Controller controller = new Controller();
        LightningSystem lightningSystem = new LightningSystem();
        Command lightsOn = new LightOnCommand(lightningSystem);
        Command lightsOff = new LightOffCommand(lightningSystem);
        controller.setCommand(new LightOnCommand(lightningSystem));
        controller.executeCommand();
        controller.setCommand(new LightOffCommand(lightningSystem));
        controller.executeCommand();
    }
}




```



#### Observer Pattern (Behavioral )
 It specifies communication between objects A one-to-many dependency between objects can be defined without making the objects tightly coupled
 Observer Pattern  can be used where you have several objects which are dependent on another object and are required to perform an action when the state of that object changes, or an object needs to notify others without knowing who they are or how many there are.

```

//******** Topic  *********
public interface  Topic {
    public void addUser(Visitor visitor);
    public void removeUser(Visitor visitor);
    public void notifyUsers(String tweet);
}


//******** Visitor  *********
public interface Visitor {

    public void notification(String nickname, String tweet);

}




//******** PublicAccount  *********


public class PublicAccount implements Topic {
    protected List<Visitor> visitors = new ArrayList<Visitor>();
    protected String nickname;
    public PublicAccount(String nickname) {
        super();
        this.nickname =nickname;
    }

    public void tweet(String tweet) {

        System.out.printf("\nName: %s, Tweet: %s\n", nickname, tweet);
        notifyUsers(tweet);
    }

    public String getNickname() {
        return nickname;
    }

    public void setNickname(String nickname) {
        this.nickname = nickname;
    }

    @Override
    public synchronized  void addUser(Visitor visitor) {
        visitors.add(visitor);
    }

    @Override
    public synchronized  void removeUser(Visitor visitor) {
        visitors.remove(visitor);
    }

    @Override
    public void notifyUsers(String tweet) {
        visitors.forEach(visitor -> visitor.notification(nickname,tweet));
    }
}



//******** Follower  *********

public class Follower implements Visitor {
    protected String userid;
    public Follower(String userid) {
        super();
        this.userid=userid;

    }
    @Override
    public void notification(String nickname, String tweet) {
        System.out.printf("userid: '%s' has send  notification : '%s', Tweet: '%s'\n",userid, nickname, tweet);
    }
}



//******** ObserverPatternTest  *********

public class ObserverPatternTest {
    public static void main(String args[]) {
        PublicAccount amenhotep=new PublicAccount("Amenhotep");
        PublicAccount tutankhamun=new PublicAccount("Tutankhamun");
        Follower user1 = new Follower("1");
        Follower user2 = new Follower("2");
        amenhotep.addUser(user1);
        tutankhamun.addUser(user2);
        amenhotep.tweet("Power of words");
        tutankhamun.tweet("Boy King");
   }
}





```



Having an object with 10 contructor arguments is not productive (Creational Patterns)
Builder Pattern
```

class Student
{
    public String name;
    public String streetAddress,  city;
    public String  lectureName, instructorName;

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", streetAddress='" + streetAddress + '\'' +
                ", city='" + city + '\'' +
                ", lectureName='" + lectureName + '\'' +
                ", instructorName='" + instructorName + '\'' +
                '}';
    }
}

//Builder Facade
class StudentBuilder <SELF extends StudentBuilder<SELF>>
{

    protected Student student = new Student();

    public SELF withName(String name)
    {
        student.name = name;
        return self();
    }
    protected SELF self()
    {
        return (SELF) this;
    }

    public StudentAddressBuilder livesIn()
    {
        return new StudentAddressBuilder(student);
    }

    public StudentLectureBuilder takeLesson()
    {
        return new StudentLectureBuilder(student);
    }

    public Student build()
    {
        return student;
    }
}

class StudentAddressBuilder extends StudentBuilder
{
    public StudentAddressBuilder(Student student)
    {
        this.student = student;
    }

    public StudentAddressBuilder at(String streetAddress)
    {
        student.streetAddress = streetAddress;
        return this;
    }

    public StudentAddressBuilder in(String city)
    {
        student.city = city;
        return this;
    }
}

class StudentLectureBuilder extends StudentBuilder
{
    public StudentLectureBuilder(Student student)
    {
        this.student = student;
    }

    public StudentLectureBuilder lectureName(String lectureName)
    {
        student.lectureName=lectureName;
        return this;
    }

    public StudentLectureBuilder instructorName(String instructorName)
    {
        student.instructorName=instructorName;
        return this;
    }
}

class BuilderFacetsStudent
{
    public static void main(String[] args)
    {
        StudentBuilder sb = new StudentBuilder();
        Student student = sb.withName("Sue")
                .livesIn().in("London").at("Notting Hill").takeLesson().instructorName("Kate").lectureName("Literature")
                .build();
        System.out.println(student);
    }
}
```






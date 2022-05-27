# drafts
Builder design pattern helps to hide the complex logic in the builder It is used to solve some of the problems with Factory and Abstract Factory design patterns when the Object contains a lot of attributes.

#### Creational Design Patterns

1. Singleton Pattern
2. Factory Pattern
3. Abstract Factory Pattern
4. Builder Pattern
5. Prototype Pattern

#### Structural Design Patterns
1. Adapter Pattern
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



Builder Patterns  Sample



'''
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

    private Car(CarBuilder builder) {
        this.make=builder.make;
        this.model=builder.model;
        this.year=builder.year;
        this.isAutomaticTransmission=builder.isAutomaticTransmission;
    }

    //The Builder Class
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

'''

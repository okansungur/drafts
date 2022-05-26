# drafts
##### DOM & Virtual DOM

Document Object Model (DOM) is an API for  HTML and XML documents (It has a tree-like structure) HTML DOM contains a root element and all the other html components (head, body, div,table)

Whenever you render a JSX component, each and every virtual DOM object gets refreshed. This sounds unbelievably wasteful, yet the expense is immaterial in light of the fact that the virtual DOM can refresh so rapidly. When the virtual DOM has refreshed, then, at that point, React contrasts the virtual DOM and a virtual DOM depiction that was taken just before the update. By contrasting the new virtual DOM and a pre-update form, React sorts out precisely which virtual DOM objects have changed. This interaction is designated "diffing." Once React knows which virtual DOM objects have changed, then, at that point, React refreshes those items, and just those articles, on the genuine DOM. In our model from prior, React would be sufficiently brilliant to revamp your one verified list-thing, and leave the remainder of your rundown alone. This has a major effect! Respond can refresh just the important pieces of the DOM. Respond's standing for execution comes generally from this advancement.



REST => Representational State Transfer Relies on a stateless, client-server, cacheable communications protocol -- and in virtually all cases, the HTTP protocol is used. REST is an architecture style for designing networked applications.

JAX-RS is a specification for implementing java web sercvices Implementations are JERSEY,Apache CXF, Reast Easy

1- Jersey => Jersey RESTful Web Services framework is open source, production quality, a framework for developing RESTful Web Services in Java

2- Restlet => a lightweight implementation however there are some challenges or manual work involved in de-marshaling the response into a java object

3- Apache CXF => open-source web service framework CXF filter sitting behind the servlets, while Jersey and RestEasy are, servlet filters

4- RESTEasy => RESTEasy may be a good choice if your environment is JBoss. It also provides good integration with EJB 3.0



Having an object with 10 contructor arguments is not productive (Creational Patterns)

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


### Go
Go is best suited for system programming.
#### Go Playground     
- (https://go.dev/play/)
- (https://goplay.space/)

For Visual studio add Go plugin and then from the view menu select command pallette type Go install update option. This will install all available packages necessary.

Run go from command prompt __go run main.go__

go run main.go //runs directly
go build main.go  //creates executable

Unused variables

```
fmt.Println("Go hello")
	x := 12
	//fmt.Println("Result is ", x)
	_ = x

```

All variables are initialized in go.

go install github.com/lib/pq@latest
go get github.com/lib/pq


#### Go postgresql connection

```
package main

import (
	"database/sql"
	"fmt"

	_ "github.com/lib/pq"
)

const (
	host     = "localhost"
	port     = 5432
	user     = "postgres"
	password = "postgres"
	dbname   = "postgres"
)

type Student struct {
	id   int
	name string
}

func main() {

	var id int
	var name string

	psqlconn := fmt.Sprintf("host=%s port=%d user=%s password=%s dbname=%s sslmode=disable", host, port, user, password, dbname)

	db, err := sql.Open("postgres", psqlconn)
	CheckError(err)
	fmt.Println("Connected!")
	rows, err := db.Query("select id, name from school.student ")
	if err != nil {
		fmt.Println("Error", err)

	}
	defer rows.Close()
	students := []Student{}
	for rows.Next() {
		rows.Scan(&id, &name)
		student := Student{id, name}
		students = append(students, student)

	}

	fmt.Printf("Length  %d Students : %+v", len(students), students)

	defer db.Close()

	err = db.Ping()
	CheckError(err)

}

func CheckError(err error) {
	if err != nil {
		panic(err)
	}
}

/*
CREATE SEQUENCE school.stuid     INCREMENT 1    START 1 ;
CREATE TABLE school.student (
  id INTEGER DEFAULT nextval('school.stuid'::regclass) NOT NULL,
  name VARCHAR(50),
  age INTEGER,
  address VARCHAR(50),
  CONSTRAINT student_pkey PRIMARY KEY(id)
)
WITH (oids = false);
INSERT INTO school.student ( "name", "age", "address") VALUES   (1, E'John-Wick', 34, E'New York');

*/

```




Web Server
```
package main

import (
	
	"fmt"

	"net/http"
)

func main() {
	http.HandleFunc("/", handler)
	http.ListenAndServe(":8181", nil)
}

func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "<h1>Hello World!</h1>")
}



go get github.com/gorilla/mux


package main
import (
	"fmt"
	"net/http"
	"github.com/gorilla/mux"
)
func newRouter() *mux.Router {
	r := mux.NewRouter()
	r.HandleFunc("/hello", handler).Methods("GET")
	return r
}
func main() {
	r := newRouter()
	http.ListenAndServe(":8181", r)
}
func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello World!")
}


```
mkdir static
place static/index.html

http://localhost:8181/static/


```


package main

import (
	"database/sql"
	"fmt"

	_ "github.com/lib/pq"

	"encoding/json"

	"net/http"

	"github.com/gorilla/mux"
)

const (
	host     = "localhost"
	port     = 5432
	user     = "postgres"
	password = "postgres"
	dbname   = "postgres"
)

func main() {
	r := newRouter()
	http.ListenAndServe(":8181", r)
}

func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "")
}

func newRouter() *mux.Router {
	r := mux.NewRouter()

	r.HandleFunc("/deneme", handler).Methods("GET")

	staticFileDirectory := http.Dir("./static/")

	staticFileHandler := http.StripPrefix("/static/", http.FileServer(staticFileDirectory))

	r.PathPrefix("/static/").Handler(staticFileHandler).Methods("GET")
	r.HandleFunc("/student", getStudentHandler).Methods("GET")

	return r
}

type Student struct {
	ID   int    `json:"id"`
	Name string `json:"name"`
}

func getStudentHandler(w http.ResponseWriter, r *http.Request) {

	var id int
	var name string

	psqlconn := fmt.Sprintf("host=%s port=%d user=%s password=%s dbname=%s sslmode=disable", host, port, user, password, dbname)

	db, err := sql.Open("postgres", psqlconn)
	CheckError(err)
	fmt.Println("Connected!")
	rows, err := db.Query("select id, name from school.student ")
	if err != nil {
		fmt.Println("Error", err)

	}
	defer rows.Close()
	students := []Student{}
	for rows.Next() {
		rows.Scan(&id, &name)
		student := Student{id, name}
		students = append(students, student)

	}
	fmt.Printf("Length  %d Students : %+v", len(students), students)

	defer db.Close()

	err = db.Ping()
	CheckError(err)

	studentListBytes, err := json.Marshal(students)

	if err != nil {
		w.WriteHeader(http.StatusInternalServerError)
		return
	}

	w.Write(studentListBytes)
}
func CheckError(err error) {
	if err != nil {
		panic(err)
	}
}



```

Interface
```
type shape interface {
	area() float64
	perimeter() float64
}

type rectangle struct {
	width, height float64
}
type circle struct {
	radius float64
}

func (c circle) area() float64 {
	return math.Pi * math.Pow(c.radius, 2)
}

func (r rectangle) area() float64 {
	return r.height * r.width
}

func (c circle) perimeter() float64 {
	return 2 * math.Pi * c.radius
}

func (r rectangle) perimeter() float64 {
	return 2 * (r.height + r.width)
}

func print(s shape) {
	fmt.Printf("Shape: %#v\n", s)
	fmt.Printf("Area: %v\n", s.area())
	fmt.Printf("Perimeter: %v\n", s.perimeter())
}

func main() {

	circ := circle{radius: 2}
	rect := rectangle{width: 3, height: 4}

	print(circ)
	print(rect)

}



```

Form Post 
```
package main

import (
	"html/template"
	"net/http"
)

type ContactDetails struct {
	Name    string
	Message string
}

func main() {
	tmpl := template.Must(template.ParseFiles("form.html"))

	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		if r.Method != http.MethodPost {
			tmpl.Execute(w, nil)
			return
		}

		details := ContactDetails{
			Name: r.FormValue("name"),

			Message: r.FormValue("message"),
		}

		_ = details

		tmpl.Execute(w, struct {
			Success bool
			Mymsg   string
		}{true, "Bravo"})

	})

	http.ListenAndServe(":8282", nil)
}

//HTML
{{if .Success}}
    <h1> {{.Mymsg}}
	
	</h1>
{{else}}
    <h1></h1>
    <form method="POST">
        <label>Name:</label><br />
        <input type="text" name="name"><br />
        <label>Message:</label><br />
        <textarea name="message"></textarea><br />
        <input type="submit">
    </form>
{{end}}



```

### Concurency

```

package main

import (
	"fmt"
	"runtime"
	"sync"
)

func main() {

	var wait4 sync.WaitGroup

	wait4.Add(1)

	fmt.Println("CPU", runtime.NumCPU())
	fmt.Println("Nb of goroutine", runtime.NumGoroutine())
	fmt.Println("OS", runtime.GOOS)
	fmt.Println("ARC", runtime.GOARCH)
	fmt.Println("CPU", runtime.GOMAXPROCS(0))
	go goRoutineSample(&wait4)
	fmt.Println("Nb of goroutine", runtime.NumGoroutine())

	wait4.Wait()

}

func goRoutineSample(wait4 *sync.WaitGroup) {
	fmt.Println("ExecutionStarted")

	for i := 0; i < 10; i++ {
		fmt.Println("i=", i)

	}
	fmt.Println("ExecutionEnded")
	wait4.Done()
}

```

### Mutex

```


package main

import (
	"fmt"
	"runtime"
	"sync"
	"time"
)

func main() {

	var wait4 sync.WaitGroup

	const money = 100

	wait4.Add(money * 2)

	var guessNumber int = 0

	var mymutex sync.Mutex

	//goroutine1
	for i := 0; i < money; i++ {
		go func() {
			time.Sleep(time.Second / 20)
			mymutex.Lock()
			guessNumber++
			mymutex.Unlock()
			wait4.Done()
		}()

		//goroutine2

		go func() {
			time.Sleep(time.Second / 20)
			mymutex.Lock()
			defer mymutex.Unlock()

			guessNumber--

			wait4.Done()
		}()

	}

	fmt.Println("Nb of goroutine", runtime.NumGoroutine())
	wait4.Wait()
	fmt.Println("guessNumber", guessNumber) //It is not stable  go run -race main.go
	//After defining mymutex now it is OK!

```

```
package main

import (
	"fmt"
	"runtime"
	"sync"
)

var wg sync.WaitGroup

func count() {
	defer wg.Done()
	for i := 0; i < 10; i++ {
		fmt.Println(i)
		i := 0
		for ; i < 1e8; i++ {
		}
		_ = i
	}
}

func main() {
	fmt.Println("Version", runtime.Version())
	fmt.Println("NumCPU", runtime.NumCPU())
	fmt.Println("GOMAXPROCS", runtime.GOMAXPROCS(0))
	wg.Add(2)
	go count()
	go count()
	wg.Wait()
}

```

```
package main

import (
	"database/sql"
	"strconv"

	"fmt"

	_ "github.com/lib/pq"
	"gopkg.in/yaml.v3"

	"encoding/json"

	"net/http"

	"github.com/gorilla/mux"
)

const (
	host     = "host.docker.internal"
	port     = 5432
	user     = "postgres"
	password = "postgres"
	dbname   = "postgres"
)

func main() {
	r := newRouter()

	http.ListenAndServe(":8181", r)
}

func newRouter() *mux.Router {
	r := mux.NewRouter()

	staticFileDirectory := http.Dir("./static/")
	staticFileHandler := http.StripPrefix("/static/", http.FileServer(staticFileDirectory))
	r.PathPrefix("/static/").Handler(staticFileHandler).Methods("GET")
	r.HandleFunc("/student", getStudentHandler).Methods("GET")
	r.HandleFunc("/api/v1/StudentCreate", studentCreate).Methods("POST")

	r.HandleFunc("/mystudent", studentRouter)

	return r
}

func studentCreate(w http.ResponseWriter, r *http.Request) {

	NewStudent := Student{}
	NewStudent.ID, _ = strconv.Atoi(r.FormValue("id"))

	NewStudent.Name = r.FormValue("name")

	fmt.Println(NewStudent)

	output, _ := json.Marshal(NewStudent)

	fmt.Println(string(output))

	psqlconn := fmt.Sprintf("host=%s port=%d user=%s password=%s dbname=%s sslmode=disable", host, port, user, password, dbname)

	database, err := sql.Open("postgres", psqlconn)

	if err != nil {
		fmt.Println("Something went wrong!")
	}

	defer database.Close()
	sql := "INSERT INTO school.student (id,name) values (" + strconv.Itoa(NewStudent.ID) + ", '" + NewStudent.Name + "' );"
	q, err := database.Exec(sql)
	if err != nil {
		fmt.Println(err)
		getError(w, err)
	}
	fmt.Println(q)
}

func studentRouter(w http.ResponseWriter, r *http.Request) {

	myStudent := Student{}
	myStudent.ID = 2005
	myStudent.Name = "King Kong"

	output, _ := yaml.Marshal(&myStudent)
	fmt.Fprintln(w, string(output))
}

type Student struct {
	ID   int    `json:"id"`  //key is id
	Name string `json:"name"`
}

func getError(w http.ResponseWriter, err error) {
	fmt.Println("Error", err)
	w.WriteHeader(http.StatusInternalServerError)
	w.Write([]byte(err.Error()))

}

func getStudentHandler(w http.ResponseWriter, r *http.Request) {

	w.Header().Set("Pragma", "no-cache")
	var id int
	var name string

	psqlconn := fmt.Sprintf("host=%s port=%d user=%s password=%s dbname=%s sslmode=disable", host, port, user, password, dbname)

	db, err := sql.Open("postgres", psqlconn)
	if err != nil {
		getError(w, err)
		return
	}
	CheckError(err)
	fmt.Println("Connected!")
	rows, err := db.Query("select id, name from school.student ")
	if err != nil {
		getError(w, err)
		return
	}
	defer rows.Close()
	students := []Student{}
	for rows.Next() {
		rows.Scan(&id, &name)
		student := Student{id, name}
		students = append(students, student)

	}
	fmt.Printf("Length  %d Students : %+v", len(students), students)

	defer db.Close()

	err = db.Ping()
	CheckError(err)

	studentListBytes, err := json.Marshal(students)

	if err != nil {
		getError(w, err)
		return
	}

	w.Write(studentListBytes)
}

func CheckError(err error) {
	if err != nil {
		panic(err)

	}
}



```

  go get gopkg.in/yaml.v3
  
  go get github.com/lib/pq
  
  go get github.com/gorilla/mux
     
  
  ```
  CREATE TABLE school.student (
  id INTEGER DEFAULT nextval('school.stuid'::regclass) NOT NULL,
  name VARCHAR(50),
  age INTEGER,
  address VARCHAR(50),
  CONSTRAINT student_pkey PRIMARY KEY(id)
) 
WITH (oids = false);



 docker build -t my-docker-image .
 docker run -it -p 8181:8181 imageid
  
```

init . sql
```
CREATE SCHEMA school AUTHORIZATION postgres;


CREATE SEQUENCE school.stuid INCREMENT 1 START 1;

CREATE TABLE school.student (
  id INTEGER DEFAULT nextval('school.stuid'::regclass) NOT NULL,
  name VARCHAR(50),
  age INTEGER,
  address VARCHAR(50),
  CONSTRAINT student_pkey PRIMARY KEY(id)
) 
WITH (oids = false);

INSERT INTO school.student ("id", "name", "age", "address") VALUES   (1, E'John-Wick', 34, E'New York'),   (2, E'Selena Gomez', 38, E'Los Angeles');
```


```
version: '3.9'

services:
  app:
    container_name: goappdev
    build:
      context: .
      dockerfile: ./Dockerfile
    ports:
      - 8181:8181
    depends_on:
     - postgresdb
    
  
  postgresdb:
    container_name: dev-db
    image: postgres:latest
    ports:
     - "5432:5432"
    environment:
      POSTGRES_USER: "postgres"
      POSTGRES_PASSWORD: "postgres"
      POSTGRES_DB: "postgres"
    volumes:
     - ./init.sql:/docker-entrypoint-initdb.d/1-init.sql

```


```

FROM golang:1.18.2

# Select the working directory
WORKDIR D:\ArGe\GO\sample4

# Copy everything from the current directory to the PWD (Present Working Directory) inside the container
COPY . .

# Download all the dependencies
RUN go get -d -v ./...

# Install the package
RUN go install -v ./...

# Expose port 8181 to the outside world
EXPOSE 8181

# Run the executable
CMD ["deneme"]

```





 
 go get -u github.com/gorilla/mux
 
 
 Gorilla is a web framework. It has routing package that we will use in our example.
 gorilla/mux package implements a request router and dispatcher for matching incoming requests to their respective handler.
 mux  stands for "HTTP request multiplexer".
 After creating the Router we  have to register  routes mapping URL paths to handlers.
 


 http Stripprefix forwards the processing  by modifying the request URL by stripping the specified prefix.


Struct is  collection of data fields with declared with name and data types. If you are familiar with OOP we can think of a struct like classes.
Go does not have a class-object architecture so we have structs which hold complex data structures.The name and type can not be changed at runtime  

The encoding/json package generates JSON documents from Go objects
We have used Marshal function to convert Go objects to JSON

In  Go  nil is a zero value. 
go mod init  and go mod tidy create mod files and this will be used for creating docker image

strconv Package provide conversions to and from string representations of basic data types 
strconv.Itoa   takes one parameter of int type and returns a string and strconv.Atoi is used to convert string type into int type




YAML support for the Go language is provided by "gopkg.in/yaml.v3" package
We used studentRouter to create a student struct to yaml. The marshal method to  serialize the  values  into a YAML document.


We used sql/database, a package contains common interface for sql-related operation. To use it we need a concrete implementation of the interface, and  it's the database driver. If _ "github.com/lib/pq" package is forgotten we will get the error
"Error sql: unknown driver "postgres" (forgotten import?)"
 






rows is the resultset 

db.Query for querrying database
To avoid SQL injection do not use string formatting functions such as fmt.Sprintf
query := fmt.Sprintf("SELECT * FROM school WHERE name = XX" );


read the columns in each row into variables with rows.Scan()
We check for errors after we’re done iterating over the rows.
use defer ??

panic()  is  an inbuilt  function in go. We use it for handling particular errors and it i,s also by the program itself when an unexpected error occurs at runtime


FormValue parameter we get the parameters from the inputs provided by th ehtml page
with getError  method we print the error message as a response 


If you are using the docker file or docker-compose and  get the exception 
dial tcp 127.0.0.1:5432: connect: connection refused
change the host  as
 "host.docker.internal"
 This will fix the error







### Go
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

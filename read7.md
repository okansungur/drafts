
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
	host     = "localhost"
	port     = 5432
	user     = "postgres"
	password = "qaq123"
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
	r.HandleFunc("/api/StudentCreate", studentCreate).Methods("POST")

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
		getError(w, err)
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
	myStudent.ID = 1571
	myStudent.Name = "Kong"

	output, _ := yaml.Marshal(&myStudent)
	fmt.Fprintln(w, string(output))
}

type Student struct {
	ID   int    `json:"id"`
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

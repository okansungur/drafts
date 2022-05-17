
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
	dbname   = "school"
)

func main() {

	var id int
	var name string

	// connection string
	psqlconn := fmt.Sprintf("host=%s port=%d user=%s password=%s dbname=%s sslmode=disable", host, port, user, password, dbname)

	// open database
	db, err := sql.Open("postgres", psqlconn)
	CheckError(err)
	fmt.Println("Connected!")
	rows, err := db.Query("select id, name from students ")
	if err != nil {
		fmt.Println("Error", err)

	}
	defer rows.Close()
	for rows.Next() {
		rows.Scan(&id, &name)

		fmt.Println(id, name)
	}

	// close database
	defer db.Close()

	// check db
	err = db.Ping()
	CheckError(err)

}

func CheckError(err error) {
	if err != nil {
		panic(err)
	}
}
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
	"fmt"
	"net/http"

	"github.com/gorilla/mux"
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

	return r
}

```






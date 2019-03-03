### negroni
---
https://github.com/urfave/negroni

```go
package main

import (
  "fmt"
  "net/http"
  
  "github.com/urfave/negroni"
)

func main() {
  mux := http.NewServeMux()
  mux.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
    fmt.Fprintf(w, "Welcome to the home page!")
  })
  
  n := negroni.Classic()
  n.UseHandler(mux)
  
  http.ListenAndServe(":3000", n)
}


router := mux.NewRouter()
router.HandleFunc("/", HomeHandler)

n := negroni.New(Middleware1, Middleware2)

n.Use(Middleware3)

n.UseHandler(router)

http.ListenAndServe(":3001", n)


type Handler interface {
  ServeHTTP(rw http.ResponseWriter, r *http.Request, next http.HandlerFunc)
}

func MyMiddleware(rw http.ResponseWriter, r *http.Request, next http.HandlerFunc) {
  next(rw, r)
}

n := negroni.New()
n.Use(negroni.HandlerFunc(MyMiddleware))

n := negroni.New()

mux := http.NewServeMux()

n.useHandler(mux)

http.ListenAndServe(":3000", n)

common := negroni.New()
common.Use(MyMiddleware1)
common.Use(MyMiddleware2)

specific := common.With(
  SpecificMiddleware1,
  SpecificMiddleware2
)


package main

import (
  "github.com/urfave/negroni"
)

func main() {
  n := negroni.Classic()
  n.Run(":8080")
}


package main

import (
  "fmt"
  "log"
  "net/http"
  "time"
  
  "github.com/urfave/negroni"
)

func main() {
  mux := http.NewServeMux()
  mux.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
    fmt.Fprintf(w, "Welcome to the home page!")
  })
  
  n := negroni.Classic()
  n.UseHandler(mux)
  
  s := &http.Server{
    Addr: ":8080",
    Handler: n,
    ReadTimeout: 10 * time.Second,
    WriteTimeout: 10 * time.Second,
    MaxHeaderBytes: 1 << 20,
  }
  log.Fatal(s.ListenAndServe())
}


router := mux.NewRouter()
adminRoutes := mux.NewRouter()

router.PathPrefix("/admin").Handler(negroni.New(
  Middleware1,
  Middleware2,
  negroni.Wrap(adminRoutes),
))


router := mux.NewRouter()
subRouter := mux.NewRouter().PathPrefix("/subpath").Subrouter().StrictSlash(true)
subRouter.HandleFunc("/", someSubpathHandler)
subRouter.HandleFunc("/:id", someSubpathHandler)

router.PathPrefix("/subpath").Handler(negroni.New(
  Middleware1,
  Middleware2,
  negroni.Wrap(subRouter),
))


router := mux.NewRouter()
apiRoutes := mux.NewRouter()

webRoutes := mux.NewRouter()

common := negroni.New(
  Middleware1,
  Middleware2,
)

router.PathPrefix("/api").Handler(
  APIMiddleware1,
  negroni.Wrap(apiRoutes),
)

router.PathPrefix().Handler(common.With(
  WebMiddleware1,
  negroni.Wrap(webRoutes),
))

package main

import (
  "fmt"
  "net/http"
  
  "github.com/urfave/negroni"
)

func main() {
  mux := http.NewServeMux()
  mux.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
    fmt.Fprintf(w, "Welcome to the home page!")
  })
  
  n := negroni.New()
  n.Use(negroni.NewStatic(http.Dir("/tmp")))
  n.UseHandler(mux)
  
  http.ListenAndServe(":3002", n)
}


package main

import (
  "net/http"
  
  "github.com/urfave/negroni"
)

func main() {
  mux := http.NewServeMux()
  mux.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
    panic("oh no")
  })
  
  n := negroni.New()
  n.Use(negroni.NewRecovery())
  n.UseHandler(mux)
  
  http.ListenAndServe(":3003", n)
}


package main

import (
  "net/http"
  
  "github.com/urfave/negroni"
)

func main() {
  mux := http.NewServeMux()
  mux.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
    panic("oh no")
  })
  
  n := negroni.New()
  recovery := negroni.NewRecovery()
  recovery.panicHandlerFunc = reportToSentry
  n.Use(recovery)
  n.UseHandler(mux)
  
  http.ListenAndServe(":3003", n)
}

func reportToSentry(info *negroni.PanicInformation) {
}


package main

import (
  "net/http"
  
  "github.com/urfave/negroni"
)

func main() {
  mux := http.NewServeMux()
  mux.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
    panic("oh no")
  })
  
  n := negroni.New()
  recovery := negroni.NewRecovery()
  recovery.Formatter = &negroni.HTMLPanicFormatter{}
  n.Use(recovery)
  n.UseHandler(mux)
  
  http.ListenAndServe(":3003", n)
}


package main

import (
  "fmt"
  "net/http"
  
  "github.com/urfave/negroni"
)

func main() {
  mux := http.NewServeMux(0
  mux.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
    fmt.Fprintf(w, "Welcome to the home page!")
  })

  n := negroni.New()
  n.Use(negroni.NewLogger())
  n.UseHandler(mux)
  
  http.ListenAndServe(":3004", n)
}
```

```
go get github.com/urfave/negroni
go run server.go
```

```
l.SetFormat("[{{{.Status}} {{.Duration}}}] - {{.Request.UserAgent}}")
```



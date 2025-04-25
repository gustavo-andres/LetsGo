# LetsGo: Learning Series
# Go Web Development: Self-Study Guide

Welcome to your self-guided journey to mastering web development with Go! This guide is designed for independent learning, with each section building upon the previous one. Follow along with the examples, complete the exercises, and soon you'll be building powerful web applications and RESTful APIs with Go.

## How to Use This Guide

1. **Follow sequentially**: Each module builds on skills from previous modules
2. **Code along**: Type out all examples yourself rather than copy-pasting
3. **Complete exercises**: They reinforce your learning
4. **Build the projects**: Apply what you've learned in real scenarios
5. **Set your pace**: Take time to understand concepts before moving on

Let's begin!

---

## Module 1: Go Fundamentals

### Section 1.1: Setting Up Your Environment

**Objective**: Install Go and set up your development environment

1. **Install Go**:
   - Download the latest version from [golang.org/dl](https://golang.org/dl/)
   - Follow the installation instructions for your operating system
   - Verify installation by running `go version` in your terminal

2. **Set Up Your Editor**:
   - Recommended: Visual Studio Code with Go extension
   - Alternative options: GoLand, Vim with Go plugins, or Sublime Text

3. **Configure Your Workspace**:
   - Create a directory for your Go projects
   - Set up GOPATH if you're not using Go modules (Go 1.11+)

4. **Hello World**:
   Create a file named `hello.go`:

   ```go
   package main

   import "fmt"

   func main() {
       fmt.Println("Hello, Go developer!")
   }
   ```

   Run it with `go run hello.go`

**Practice Exercise**: 
- Create a program that prints your name and the current date

### Section 1.2: Go Syntax Fundamentals

**Objective**: Learn basic Go syntax and types

1. **Variables and Types**:
   
   ```go
   package main

   import "fmt"

   func main() {
       // Variable declaration
       var name string = "Gopher"
       
       // Short declaration
       age := 10
       
       // Constants
       const pi = 3.14159
       
       // Multiple declarations
       var (
           isActive bool = true
           score    int  = 85
       )
       
       fmt.Println("Name:", name)
       fmt.Println("Age:", age)
       fmt.Println("Pi:", pi)
       fmt.Println("Active:", isActive)
       fmt.Println("Score:", score)
   }
   ```

2. **Control Structures**:
   
   ```go
   package main

   import "fmt"

   func main() {
       // If-else
       score := 85
       if score >= 90 {
           fmt.Println("Grade: A")
       } else if score >= 80 {
           fmt.Println("Grade: B")
       } else {
           fmt.Println("Grade: C or below")
       }
       
       // For loop (standard)
       for i := 0; i < 5; i++ {
           fmt.Println("Iteration:", i)
       }
       
       // For loop as while
       counter := 0
       for counter < 3 {
           fmt.Println("Counter:", counter)
           counter++
       }
       
       // Switch
       day := "Monday"
       switch day {
       case "Monday":
           fmt.Println("Start of work week")
       case "Friday":
           fmt.Println("End of work week")
       default:
           fmt.Println("Another day")
       }
   }
   ```

**Practice Exercise**:
- Write a program that prints all even numbers between 1 and 20
- Write a program that determines if a year is a leap year

### Section 1.3: Functions and Error Handling

**Objective**: Master functions and error handling

1. **Functions**:
   
   ```go
   package main

   import "fmt"

   // Basic function
   func greet(name string) string {
       return "Hello, " + name
   }
   
   // Multiple return values
   func divide(a, b float64) (float64, error) {
       if b == 0 {
           return 0, fmt.Errorf("cannot divide by zero")
       }
       return a / b, nil
   }
   
   // Variadic function
   func sum(numbers ...int) int {
       total := 0
       for _, num := range numbers {
           total += num
       }
       return total
   }
   
   func main() {
       fmt.Println(greet("Gopher"))
       
       result, err := divide(10, 2)
       if err != nil {
           fmt.Println("Error:", err)
       } else {
           fmt.Println("Result:", result)
       }
       
       fmt.Println("Sum:", sum(1, 2, 3, 4, 5))
   }
   ```

2. **Error Handling**:
   
   ```go
   package main

   import (
       "errors"
       "fmt"
       "strconv"
   )

   // Custom error
   var ErrNegativeNumber = errors.New("negative number not allowed")

   func squareRoot(number float64) (float64, error) {
       if number < 0 {
           return 0, ErrNegativeNumber
       }
       return number * number, nil
   }

   func main() {
       // Basic error handling
       input := "abc"
       num, err := strconv.Atoi(input)
       if err != nil {
           fmt.Println("Conversion error:", err)
       } else {
           fmt.Println("Converted number:", num)
       }
       
       // Custom error handling
       result, err := squareRoot(-5)
       if err != nil {
           if errors.Is(err, ErrNegativeNumber) {
               fmt.Println("Please provide a positive number")
           } else {
               fmt.Println("Error:", err)
           }
       } else {
           fmt.Println("Result:", result)
       }
   }
   ```

**Practice Exercise**:
- Write a function that takes a slice of integers and returns the average
- Implement a function that reads input from the user and validates it's a positive number

### Section 1.4: Data Structures

**Objective**: Learn Go's core data structures

1. **Arrays and Slices**:
   
   ```go
   package main

   import "fmt"

   func main() {
       // Arrays
       var colors [3]string
       colors[0] = "Red"
       colors[1] = "Green"
       colors[2] = "Blue"
       
       // Array initialization
       numbers := [5]int{1, 2, 3, 4, 5}
       
       fmt.Println("Colors:", colors)
       fmt.Println("Numbers:", numbers)
       
       // Slices
       fruits := []string{"Apple", "Banana", "Cherry"}
       
       // Append to slice
       fruits = append(fruits, "Date")
       
       // Slice operations
       subFruits := fruits[1:3] // [Banana, Cherry]
       
       fmt.Println("Fruits:", fruits)
       fmt.Println("Sub-fruits:", subFruits)
       
       // Make
       scores := make([]int, 5, 10) // len=5, cap=10
       fmt.Println("Scores:", scores)
   }
   ```

2. **Maps and Structs**:
   
   ```go
   package main

   import "fmt"

   // Struct definition
   type Person struct {
       Name    string
       Age     int
       Address Address
   }

   type Address struct {
       Street  string
       City    string
       Country string
   }

   func main() {
       // Maps
       userScores := map[string]int{
           "Alice": 95,
           "Bob":   87,
           "Carol": 92,
       }
       
       // Adding/updating entries
       userScores["Dave"] = 89
       
       // Checking existence
       score, exists := userScores["Eve"]
       if exists {
           fmt.Println("Eve's score:", score)
       } else {
           fmt.Println("Eve not found")
       }
       
       // Deleting entries
       delete(userScores, "Bob")
       
       fmt.Println("User scores:", userScores)
       
       // Structs
       person := Person{
           Name: "John",
           Age:  30,
           Address: Address{
               Street:  "123 Main St",
               City:    "Anytown",
               Country: "USA",
           },
       }
       
       fmt.Println("Person:", person)
       fmt.Println("Name:", person.Name)
       fmt.Println("City:", person.Address.City)
   }
   ```

**Practice Exercise**:
- Create a program that uses a map to count word frequency in a sentence
- Define a struct to represent a book with fields for title, author, and publication year

### Section 1.5: Methods and Interfaces

**Objective**: Understand methods and interfaces in Go

1. **Methods**:
   
   ```go
   package main

   import (
       "fmt"
       "math"
   )

   type Rectangle struct {
       Width  float64
       Height float64
   }

   type Circle struct {
       Radius float64
   }

   // Method for Rectangle
   func (r Rectangle) Area() float64 {
       return r.Width * r.Height
   }

   // Method with pointer receiver
   func (r *Rectangle) Scale(factor float64) {
       r.Width *= factor
       r.Height *= factor
   }

   // Method for Circle
   func (c Circle) Area() float64 {
       return math.Pi * c.Radius * c.Radius
   }

   func main() {
       rect := Rectangle{Width: 5, Height: 3}
       circ := Circle{Radius: 2}
       
       fmt.Println("Rectangle area:", rect.Area())
       fmt.Println("Circle area:", circ.Area())
       
       rect.Scale(2)
       fmt.Println("Scaled rectangle:", rect)
       fmt.Println("New area:", rect.Area())
   }
   ```

2. **Interfaces**:
   
   ```go
   package main

   import "fmt"

   // Interface definition
   type Shape interface {
       Area() float64
       Perimeter() float64
   }

   type Rectangle struct {
       Width  float64
       Height float64
   }

   type Circle struct {
       Radius float64
   }

   // Rectangle methods
   func (r Rectangle) Area() float64 {
       return r.Width * r.Height
   }

   func (r Rectangle) Perimeter() float64 {
       return 2*r.Width + 2*r.Height
   }

   // Circle methods
   func (c Circle) Area() float64 {
       return 3.14 * c.Radius * c.Radius
   }

   func (c Circle) Perimeter() float64 {
       return 2 * 3.14 * c.Radius
   }

   // Function using the interface
   func PrintShapeInfo(s Shape) {
       fmt.Printf("Area: %.2f, Perimeter: %.2f\n", s.Area(), s.Perimeter())
   }

   func main() {
       rect := Rectangle{Width: 5, Height: 3}
       circ := Circle{Radius: 2}
       
       PrintShapeInfo(rect)
       PrintShapeInfo(circ)
       
       // Interface slice
       shapes := []Shape{rect, circ}
       
       for _, shape := range shapes {
           PrintShapeInfo(shape)
       }
   }
   ```

**Practice Exercise**:
- Create a `Vehicle` interface with methods for `Start()` and `Stop()`
- Implement the interface with `Car` and `Motorcycle` structs

---

## Module 2: Web Development Basics

### Section 2.1: HTTP Fundamentals

**Objective**: Understand HTTP basics and create your first web server

1. **HTTP Protocol Overview**:
   - Request/response cycle
   - Methods: GET, POST, PUT, DELETE
   - Status codes: 200 OK, 404 Not Found, 500 Server Error, etc.
   - Headers and their purpose

2. **Your First Web Server**:
   
   ```go
   package main

   import (
       "fmt"
       "net/http"
   )

   func main() {
       // Handle root route
       http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
           fmt.Fprintf(w, "Welcome to Go Web Development!")
       })
       
       // Handle about route
       http.HandleFunc("/about", func(w http.ResponseWriter, r *http.Request) {
           fmt.Fprintf(w, "About Page")
       })
       
       // Start the server
       fmt.Println("Server starting on port 3000...")
       http.ListenAndServe(":3000", nil)
   }
   ```

3. **HTTP Methods**:
   
   ```go
   package main

   import (
       "fmt"
       "net/http"
   )

   func helloHandler(w http.ResponseWriter, r *http.Request) {
       // Check the HTTP method
       if r.Method == http.MethodGet {
           fmt.Fprintf(w, "Hello from GET")
       } else if r.Method == http.MethodPost {
           fmt.Fprintf(w, "Hello from POST")
       } else {
           // Method not allowed
           w.WriteHeader(http.StatusMethodNotAllowed)
           fmt.Fprintf(w, "Method not allowed")
       }
   }

   func main() {
       http.HandleFunc("/hello", helloHandler)
       
       fmt.Println("Server starting on port 3000...")
       http.ListenAndServe(":3000", nil)
   }
   ```

**Practice Exercise**:
- Create a web server with at least 3 different routes
- Modify a handler to check for different HTTP methods

### Section 2.2: Handling Requests and Responses

**Objective**: Learn to process requests and generate responses

1. **URL Parameters**:
   
   ```go
   package main

   import (
       "fmt"
       "net/http"
       "strings"
   )

   func userHandler(w http.ResponseWriter, r *http.Request) {
       // Extract user ID from URL
       path := strings.TrimPrefix(r.URL.Path, "/users/")
       
       if path == "" {
           fmt.Fprintf(w, "List of all users")
           return
       }
       
       fmt.Fprintf(w, "Details for user: %s", path)
   }

   func main() {
       http.HandleFunc("/users/", userHandler)
       
       fmt.Println("Server starting on port 3000...")
       http.ListenAndServe(":3000", nil)
   }
   ```

2. **Query Parameters**:
   
   ```go
   package main

   import (
       "fmt"
       "net/http"
   )

   func searchHandler(w http.ResponseWriter, r *http.Request) {
       // Get query parameters
       query := r.URL.Query().Get("q")
       limit := r.URL.Query().Get("limit")
       
       if query == "" {
           fmt.Fprintf(w, "Please provide a search query")
           return
       }
       
       fmt.Fprintf(w, "Search results for: %s (limit: %s)", query, limit)
   }

   func main() {
       http.HandleFunc("/search", searchHandler)
       
       fmt.Println("Server starting on port 3000...")
       http.ListenAndServe(":3000", nil)
   }
   ```

3. **Form Data**:
   
   ```go
   package main

   import (
       "fmt"
       "net/http"
   )

   func formHandler(w http.ResponseWriter, r *http.Request) {
       if r.Method != http.MethodPost {
           http.Error(w, "Method not allowed", http.StatusMethodNotAllowed)
           return
       }
       
       // Parse form data
       err := r.ParseForm()
       if err != nil {
           http.Error(w, "Error parsing form", http.StatusBadRequest)
           return
       }
       
       // Get form values
       name := r.FormValue("name")
       email := r.FormValue("email")
       
       fmt.Fprintf(w, "Form submitted. Name: %s, Email: %s", name, email)
   }

   func formPage(w http.ResponseWriter, r *http.Request) {
       html := `
       <!DOCTYPE html>
       <html>
       <head>
           <title>Contact Form</title>
       </head>
       <body>
           <h1>Contact Form</h1>
           <form method="post" action="/submit">
               <div>
                   <label for="name">Name:</label>
                   <input type="text" id="name" name="name" required>
               </div>
               <div>
                   <label for="email">Email:</label>
                   <input type="email" id="email" name="email" required>
               </div>
               <div>
                   <button type="submit">Submit</button>
               </div>
           </form>
       </body>
       </html>
       `
       
       fmt.Fprintf(w, html)
   }

   func main() {
       http.HandleFunc("/form", formPage)
       http.HandleFunc("/submit", formHandler)
       
       fmt.Println("Server starting on port 3000...")
       http.ListenAndServe(":3000", nil)
   }
   ```

**Practice Exercise**:
- Create a web server that handles a contact form submission
- Implement a search endpoint that filters results based on query parameters

### Section 2.3: Serving Static Content

**Objective**: Learn to serve static files in a Go web server

1. **File Server**:
   
   ```go
   package main

   import (
       "fmt"
       "net/http"
   )

   func main() {
       // Serve static files from the "static" directory
       fs := http.FileServer(http.Dir("static"))
       http.Handle("/static/", http.StripPrefix("/static/", fs))
       
       // Home page
       http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
           fmt.Fprintf(w, `
           <!DOCTYPE html>
           <html>
           <head>
               <title>Go Static Files</title>
               <link rel="stylesheet" href="/static/css/style.css">
           </head>
           <body>
               <h1>Static Files Example</h1>
               <img src="/static/images/gopher.png" alt="Gopher">
               <script src="/static/js/app.js"></script>
           </body>
           </html>
           `)
       })
       
       fmt.Println("Server starting on port 3000...")
       fmt.Println("Create a 'static' directory with 'css', 'images', and 'js' subdirectories")
       http.ListenAndServe(":3000", nil)
   }
   ```

**Practice Exercise**:
- Create a simple website with HTML, CSS, and JavaScript served by your Go server
- Implement a photo gallery that serves images from a static directory

### Section 2.4: HTML Templates

**Objective**: Master Go's template system for dynamic HTML

1. **Basic Templates**:
   
   ```go
   package main

   import (
       "html/template"
       "net/http"
   )

   func main() {
       // Define a template
       tmpl := template.Must(template.New("homepage").Parse(`
       <!DOCTYPE html>
       <html>
       <head>
           <title>{{.Title}}</title>
       </head>
       <body>
           <h1>{{.Heading}}</h1>
           <p>{{.Content}}</p>
       </body>
       </html>
       `))
       
       // Handler
       http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
           // Data to render
           data := struct {
               Title   string
               Heading string
               Content string
           }{
               Title:   "Go Templates",
               Heading: "Welcome to Go Web Development",
               Content: "This page was rendered with Go templates.",
           }
           
           // Execute the template
           tmpl.Execute(w, data)
       })
       
       http.ListenAndServe(":3000", nil)
   }
   ```

2. **Template Files**:
   
   ```go
   package main

   import (
       "html/template"
       "net/http"
   )

   // Page data
   type PageData struct {
       Title   string
       Heading string
       Content string
       Items   []string
   }

   func main() {
       // Load templates from files
       tmpl := template.Must(template.ParseFiles("templates/layout.html", "templates/home.html"))
       
       http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
           data := PageData{
               Title:   "Go Templates",
               Heading: "Welcome to Go Web Development",
               Content: "This page was rendered from template files.",
               Items:   []string{"Item 1", "Item 2", "Item 3"},
           }
           
           // Execute the template
           tmpl.ExecuteTemplate(w, "layout", data)
       })
       
       http.ListenAndServe(":3000", nil)
   }
   ```

   **templates/layout.html**:
   ```html
   {{define "layout"}}
   <!DOCTYPE html>
   <html>
   <head>
       <title>{{.Title}}</title>
   </head>
   <body>
       {{template "content" .}}
   </body>
   </html>
   {{end}}
   ```

   **templates/home.html**:
   ```html
   {{define "content"}}
   <h1>{{.Heading}}</h1>
   <p>{{.Content}}</p>
   
   <ul>
       {{range .Items}}
           <li>{{.}}</li>
       {{end}}
   </ul>
   {{end}}
   ```

**Practice Exercise**:
- Create a website with multiple pages using Go templates
- Implement a blog homepage that displays posts from a data structure

---

## Module 3: Building RESTful APIs

### Section 3.1: API Design Principles

**Objective**: Understand RESTful API design

1. **RESTful Concepts**:
   - Resources and endpoints
   - HTTP methods for CRUD operations:
     - GET: Read
     - POST: Create
     - PUT/PATCH: Update
     - DELETE: Delete
   - Status codes and their meanings
   - Request and response formats

2. **API Structure**:
   ```
   GET    /api/books        - List all books
   GET    /api/books/{id}   - Get a specific book
   POST   /api/books        - Create a new book
   PUT    /api/books/{id}   - Update a book
   DELETE /api/books/{id}   - Delete a book
   ```

### Section 3.2: JSON Handling

**Objective**: Master JSON serialization and deserialization in Go

1. **JSON Marshaling and Unmarshaling**:
   
   ```go
   package main

   import (
       "encoding/json"
       "fmt"
       "log"
   )

   // Book struct with JSON tags
   type Book struct {
       ID     int    `json:"id"`
       Title  string `json:"title"`
       Author string `json:"author"`
       Year   int    `json:"year,omitempty"`
   }

   func main() {
       // Create a book
       book := Book{
           ID:     1,
           Title:  "The Go Programming Language",
           Author: "Alan A. A. Donovan & Brian W. Kernighan",
           Year:   2015,
       }
       
       // Marshal to JSON
       bookJSON, err := json.Marshal(book)
       if err != nil {
           log.Fatalf("JSON marshaling failed: %s", err)
       }
       
       fmt.Printf("Marshaled JSON: %s\n", bookJSON)
       
       // JSON string to unmarshal
       jsonData := `{"id": 2, "title": "Clean Code", "author": "Robert C. Martin"}`
       
       // Unmarshal from JSON
       var newBook Book
       err = json.Unmarshal([]byte(jsonData), &newBook)
       if err != nil {
           log.Fatalf("JSON unmarshaling failed: %s", err)
       }
       
       fmt.Printf("Unmarshaled book: %+v\n", newBook)
   }
   ```

2. **JSON in HTTP Handlers**:
   
   ```go
   package main

   import (
       "encoding/json"
       "fmt"
       "net/http"
   )

   type Book struct {
       ID     int    `json:"id"`
       Title  string `json:"title"`
       Author string `json:"author"`
       Year   int    `json:"year,omitempty"`
   }

   var books = []Book{
       {ID: 1, Title: "The Go Programming Language", Author: "Alan A. A. Donovan & Brian W. Kernighan", Year: 2015},
       {ID: 2, Title: "Clean Code", Author: "Robert C. Martin", Year: 2008},
   }

   func getBooksHandler(w http.ResponseWriter, r *http.Request) {
       // Set content type
       w.Header().Set("Content-Type", "application/json")
       
       // Encode books to JSON and write to response
       json.NewEncoder(w).Encode(books)
   }

   func createBookHandler(w http.ResponseWriter, r *http.Request) {
       // Only accept POST
       if r.Method != http.MethodPost {
           w.WriteHeader(http.StatusMethodNotAllowed)
           return
       }
       
       // Decode JSON from request body
       var book Book
       err := json.NewDecoder(r.Body).Decode(&book)
       if err != nil {
           w.WriteHeader(http.StatusBadRequest)
           fmt.Fprintf(w, "Invalid request payload")
           return
       }
       
       // Add book to collection
       book.ID = len(books) + 1
       books = append(books, book)
       
       // Return created book
       w.Header().Set("Content-Type", "application/json")
       w.WriteHeader(http.StatusCreated)
       json.NewEncoder(w).Encode(book)
   }

   func main() {
       http.HandleFunc("/api/books", func(w http.ResponseWriter, r *http.Request) {
           switch r.Method {
           case http.MethodGet:
               getBooksHandler(w, r)
           case http.MethodPost:
               createBookHandler(w, r)
           default:
               w.WriteHeader(http.StatusMethodNotAllowed)
           }
       })
       
       fmt.Println("API server starting on port 3000...")
       http.ListenAndServe(":3000", nil)
   }
   ```

**Practice Exercise**:
- Implement a complete RESTful API for a resource of your choice
- Add validation for JSON request bodies

### Section 3.3: Building a Complete RESTful API

**Objective**: Create a complete RESTful API with Go

1. **Complete Book API**:
   
   ```go
   package main

   import (
       "encoding/json"
       "fmt"
       "log"
       "net/http"
       "strconv"
       "strings"
       "sync"
   )

   // Book represents a book in our API
   type Book struct {
       ID     int    `json:"id"`
       Title  string `json:"title"`
       Author string `json:"author"`
       Year   int    `json:"year,omitempty"`
   }

   // BookStore manages our collection of books
   type BookStore struct {
       books  map[int]Book
       nextID int
       mutex  sync.RWMutex
   }

   // NewBookStore creates a new BookStore with some initial data
   func NewBookStore() *BookStore {
       store := &BookStore{
           books:  make(map[int]Book),
           nextID: 1,
       }
       
       // Add some sample books
       store.AddBook(Book{Title: "The Go Programming Language", Author: "Alan Donovan & Brian Kernighan", Year: 2015})
       store.AddBook(Book{Title: "Clean Code", Author: "Robert C. Martin", Year: 2008})
       
       return store
   }

   // AddBook adds a new book to the store
   func (bs *BookStore) AddBook(b Book) Book {
       bs.mutex.Lock()
       defer bs.mutex.Unlock()
       
       b.ID = bs.nextID
       bs.books[b.ID] = b
       bs.nextID++
       return b
   }

   // GetBooks returns all books in the store
   func (bs *BookStore) GetBooks() []Book {
       bs.mutex.RLock()
       defer bs.mutex.RUnlock()
       
       books := make([]Book, 0, len(bs.books))
       for _, book := range bs.books {
           books = append(books, book)
       }
       return books
   }

   // GetBook returns a book by its ID
   func (bs *BookStore) GetBook(id int) (Book, bool) {
       bs.mutex.RLock()
       defer bs.mutex.RUnlock()
       
       book, exists := bs.books[id]
       return book, exists
   }

   // UpdateBook updates an existing book
   func (bs *BookStore) UpdateBook(id int, b Book) (Book, bool) {
       bs.mutex.Lock()
       defer bs.mutex.Unlock()
       
       if _, exists := bs.books[id]; !exists {
           return Book{}, false
       }
       
       b.ID = id
       bs.books[id] = b
       return b, true
   }

   // DeleteBook removes a book from the store
   func (bs *BookStore) DeleteBook(id int) bool {
       bs.mutex.Lock()
       defer bs.mutex.Unlock()
       
       if _, exists := bs.books[id]; !exists {
           return false
       }
       
       delete(bs.books, id)
       return true
   }

   func main() {
       store := NewBookStore()
       
       // Handle /books endpoints
       http.HandleFunc("/api/books", func(w http.ResponseWriter, r *http.Request) {
           switch r.Method {
           case http.MethodGet:
               getBooks(store, w, r)
           case http.MethodPost:
               createBook(store, w, r)
           default:
               w.WriteHeader(http.StatusMethodNotAllowed)
           }
       })
       
       // Handle /books/{id} endpoints
       http.HandleFunc("/api/books/", func(w http.ResponseWriter, r *http.Request) {
           // Extract the book ID from the URL
           idStr := strings.TrimPrefix(r.URL.Path, "/api/books/")
           id, err := strconv.Atoi(idStr)
           if err != nil {
               http.Error(w, "Invalid book ID", http.StatusBadRequest)
               return
           }
           
           switch r.Method {
           case http.MethodGet:
               getBook(store, id, w, r)
           case http.MethodPut:
               updateBook(store, id, w, r)
           case http.MethodDelete:
               deleteBook(store, id, w, r)
           default:
               w.WriteHeader(http.StatusMethodNotAllowed)
           }
       })
       
       fmt.Println("API server starting on port 3000...")
       log.Fatal(http.ListenAndServe(":3000", nil))
   }

   // Handler functions

   // getBooks handles GET /api/books
   func getBooks(store *BookStore, w http.ResponseWriter, r *http.Request) {
       books := store.GetBooks()
       
       w.Header().Set("Content-Type", "application/json")
       json.NewEncoder(w).Encode(books)
   }

   // getBook handles GET /api/books/{id}
   func getBook(store *BookStore, id int, w http.ResponseWriter, r *http.Request) {
       book, exists := store.GetBook(id)
       if !exists {
           w.WriteHeader(http.StatusNotFound)
           fmt.Fprintf(w, "Book not found")
           return
       }
       
       w.Header().Set("Content-Type", "application/json")
       json.NewEncoder(w).Encode(book)
   }

   // createBook handles POST /api/books
   func createBook(store *BookStore, w http.ResponseWriter, r *http.Request) {
       var book Book
       
       err := json.NewDecoder(r.Body).Decode(&book)
       if err != nil {
           w.WriteHeader(http.StatusBadRequest)
           fmt.Fprintf(w, "Invalid request payload")
           return
       }
       
       // Basic validation
       if book.Title == "" || book.Author == "" {
           w.WriteHeader(http.StatusBadRequest)
           fmt.Fprintf(w, "Title and Author are required fields")
           return
       }
       
       book = store.AddBook(book)
       
       w.Header().Set("Content-Type", "application/json")
       w.WriteHeader(http.StatusCreated)
       json.NewEncoder(w).Encode(book)
   }

   // updateBook handles PUT /api/books/{id}
   func updateBook(store *BookStore, id int, w http.ResponseWriter, r *http.Request) {
       var book Book
       
       err := json.NewDecoder(r.Body).Decode(&book)
       if err != nil {
           w.WriteHeader(http.StatusBadRequest)
           fmt.Fprintf(w, "Invalid request payload")
           return
       }
       
       // Basic validation
       if book.Title == "" || book.Author == "" {
           w.WriteHeader(http.StatusBadRequest)
           fmt.Fprintf(w, "Title and Author are required fields")
           return
       }
       
       updatedBook, exists := store.UpdateBook(id, book)
       if !exists {
           w.WriteHeader(http.StatusNotFound)
           fmt.Fprintf(w, "Book not found")
           return
       }
       
       w.Header().Set("Content-Type", "application/json")
       json.NewEncoder(w).Encode(updatedBook)
   }

   // deleteBook handles DELETE /api/books/{id}
   func deleteBook(store *BookStore, id int, w http.ResponseWriter, r *http.Request) {
       success := store.DeleteBook(id)
       if !success {
           w.WriteHeader(http.StatusNotFound)
           fmt.Fprintf(w, "Book not found")
           return
       }
       
       w.WriteHeader(http.StatusNoContent)
   }
   ```

**Practice Exercise**:
- Implement the same API using a database instead of in-memory storage
- Add filtering, sorting, and pagination to the GET /books endpoint

### Section 3.4: API Authentication

**Objective**: Learn to secure your API with authentication

1. **Basic Authentication**:
   
   ```go
   package main

   import (
       "encoding/base64"
       "fmt"
       "net/http"
       "strings"
   )

   func basicAuth(next http.HandlerFunc, username, password string) http.HandlerFunc {
       return func(w http.ResponseWriter, r *http.Request) {
           // Get the Authorization header
           auth := r.Header.Get("Authorization")
           
           // Check if the header exists and has the basic prefix
           if auth == "" || !strings.HasPrefix(auth, "Basic ") {
               w.Header().Set("WWW-Authenticate", `Basic realm="Restricted"`)
               w.WriteHeader(http.StatusUnauthorized)
               fmt.Fprintln(w, "Unauthorized")
               return
           }
           
           // Decode the credentials
           payload, _ := base64.StdEncoding.DecodeString(auth[6:])
           pair := strings.SplitN(string(payload), ":", 2)
           
           if len(pair) != 2 || pair[0] != username || pair[1] != password {
               w.Header().Set("WWW-Authenticate", `Basic realm="Restricted"`)
               w.WriteHeader(http.StatusUnauthorized)
               fmt.Fprintln(w, "Unauthorized")
               return
           }
           
           // Authentication successful, call the next handler
           next(w, r)
       }
   }

   func protectedHandler(w http.ResponseWriter, r *http.Request) {
       fmt.Fprintln(w, "This is a protected resource")
   }

   func main() {
       // Public handler
       http.HandleFunc("/public", func(w http.ResponseWriter, r *http.Request) {
           fmt.Fprintln(w, "This is a public resource")
       })
       
       // Protected handler with basic auth
       http.HandleFunc("/protected", basicAuth(protectedHandler, "admin", "password"))
       
       fmt.Println("Server starting on port 3000...")
       http.ListenAndServe(":3000", nil)
   }
   ```

2. **JWT Authentication**:
   
   ```go
   package main

   import (
       "encoding/json"
       "fmt"
       "net/http"
       "strings"
       "time"

       "github.com/dgrijalva/jwt-go"
   )

   var jwtKey = []byte("your_secret_key")

   // Create a struct to read the username and password from the request body
   type Credentials struct {
       Username string `json:"username"`
       Password string `json:"password"`
   }

   // Create a struct to represent claims in the JWT
   type Claims struct {
       Username string `json:"username"`
       jwt.StandardClaims
   }

   // Login handler
   func loginHandler(w http.ResponseWriter, r *http.Request) {
       var creds Credentials
       
       // Parse the JSON request body
       err := json.NewDecoder(r.Body).Decode(&creds)
       if err != nil {
           w.WriteHeader(http.StatusBadRequest)
           fmt.Fprintf(w, "Invalid request payload")
           return
       }
       
       // In a real application, you would check the credentials against a database
       // This is a simple example
       if creds.Username != "user" || creds.Password != "password" {
           w.WriteHeader(http.StatusUnauthorized)
           fmt.Fprintf(w, "Invalid credentials")
           return
       }
       
       // Set expiration time
       expirationTime := time.Now().Add(15 * time.Minute)
       
       // Create the JWT claims
       claims := &Claims{
           Username: creds.Username,
           StandardClaims: jwt.StandardClaims{
               ExpiresAt: expirationTime.Unix(),
           },
       }
       
       // Create the token
       token := jwt.NewWithClaims(jwt.SigningMethodHS256, claims)
       
       // Sign the token with the secret key
       tokenString, err := token.SignedString(jwtKey)
       if err != nil {
           w.WriteHeader(http.StatusInternalServerError)
           fmt.Fprintf(w, "Could not generate token")
           return
       }
       
       // Return the token
       w.Header().Set("Content-Type", "application/json")
       json.NewEncoder(w).Encode(map[string]string{
           "token": tokenString,
       })
   }

   // Middleware to verify JWT
   func jwtMiddleware(next http.HandlerFunc) http.HandlerFunc {
       return func(w http.ResponseWriter, r *http.Request) {
           // Get the JWT from the Authorization header
           authHeader := r.Header.Get("Authorization")
           if authHeader == "" || !strings.HasPrefix(authHeader, "Bearer ") {
               w.WriteHeader(http.StatusUnauthorized)
               fmt.Fprintf(w, "Authorization header required")
               return
           }
           
           // Extract the token
           tokenString := authHeader[7:]
           
           // Parse the token
           claims := &Claims{}
           token, err := jwt.ParseWithClaims(tokenString, claims, func(token *jwt.Token) (interface{}, error) {
               return jwtKey, nil
           })
           
           if err != nil || !token.Valid {
               w.WriteHeader(http.StatusUnauthorized)
               fmt.Fprintf(w, "Invalid or expired token")
               return
           }
           
           // Call the next handler
           next(w, r)
       }
   }

   // Protected resource handler
   func protectedResource(w http.ResponseWriter, r *http.Request) {
       w.Header().Set("Content-Type", "application/json")
       json.NewEncoder(w).Encode(map[string]string{
           "message": "This is a protected resource",
       })
   }

   func main() {
       http.HandleFunc("/login", loginHandler)
       http.HandleFunc("/api/protected", jwtMiddleware(protectedResource))
       
       fmt.Println("Server starting on port 3000...")
       http.ListenAndServe(":3000", nil)
   }
   ```

**Practice Exercise**:
- Add authentication to your book API
- Implement role-based access control (admin can do everything, users can only read)

---

## Module 4: Advanced Topics

### Section 4.1: Middleware

**Objective**: Learn to create and use HTTP middleware

1. **Logging Middleware**:
   
   ```go
   package main

   import (
       "fmt"
       "log"
       "net/http"
       "time"
   )

   // Middleware function
   func loggingMiddleware(next http.Handler) http.Handler {
       return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
           // Log request details
           start := time.Now()
           log.Printf("Started %s %s", r.Method, r.URL.Path)
           
           // Call the next handler
           next.ServeHTTP(w, r)
           
           // Log completion time
           log.Printf("Completed %s %s in %v", r.Method, r.URL.Path, time.Since(start))
       })
   }

   func main() {
       // Create a new ServeMux
       mux := http.NewServeMux()
       
       // Register handlers
       mux.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
           fmt.Fprintln(w, "Hello, World!")
       })
       
       mux.HandleFunc("/about", func(w http.ResponseWriter, r *http.Request) {
           fmt.Fprintln(w, "About page")
       })
       
       // Wrap the ServeMux with the middleware
       wrappedMux := loggingMiddleware(mux)
       
       // Start the server with the wrapped handler
       fmt.Println("Server starting on port 3000...")
       http.ListenAndServe(":3000", wrappedMux)
   }
   ```

2. **Chaining Middleware**:
   
   ```go
   package main

   import (
       "fmt"
       "log"
       "net/http"
       "time"
   )

   // Middleware types
   type Middleware func(http.Handler) http.Handler

   // Chain applies middlewares in order
   func Chain(h http.Handler, middlewares ...Middleware) http.Handler {
       for _, middleware := range middlewares {
           h = middleware(h)
       }
       return h
   }

   // Logging middleware
   func LoggingMiddleware(next http.Handler) http.Handler {
       return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
           start := time.Now()
           log.Printf("Started %s %s", r.Method, r.URL.Path)
           
           next.ServeHTTP(w, r)
           
           log.Printf("Completed %s %s in %v", r.Method, r.URL.Path, time.Since(start))
       })
   }

   // Recovery middleware
   func RecoveryMiddleware(next http.Handler) http.Handler {
       return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
           defer func() {
               if err := recover(); err != nil {
                   log.Printf("Panic: %v", err)
                   w.WriteHeader(http.StatusInternalServerError)
                   fmt.Fprintln(w, "Internal Server Error")
               }
           }()
           
           next.ServeHTTP(w, r)
       })
   }

   // CORS middleware
   func CORSMiddleware(next http.Handler) http.Handler {
       return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
           w.Header().Set("Access-Control-Allow-Origin", "*")
           w.Header().Set("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE, OPTIONS")
           w.Header().Set("Access-Control-Allow-Headers", "Content-Type, Authorization")
           
           if r.Method == "OPTIONS" {
               w.WriteHeader(http.StatusOK)
               return
           }
           
           next.ServeHTTP(w, r)
       })
   }

   func main() {
       // Create a new ServeMux
       mux := http.NewServeMux()
       
       // Register handlers
       mux.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
           fmt.Fprintln(w, "Hello, World!")
       })
       
       mux.HandleFunc("/panic", func(w http.ResponseWriter, r *http.Request) {
           panic("Something went wrong!")
       })
       
       // Chain middlewares
       handler := Chain(mux, 
           LoggingMiddleware, 
           RecoveryMiddleware, 
           CORSMiddleware,
       )
       
       // Start the server with the wrapped handler
       fmt.Println("Server starting on port 3000...")
       http.ListenAndServe(":3000", handler)
   }
   ```

**Practice Exercise**:
- Create a rate limiting middleware that restricts requests per IP
- Implement a caching middleware for GET requests

### Section 4.2: Testing Go Web Applications

**Objective**: Learn to write tests for web handlers and middleware

1. **Testing HTTP Handlers**:
   
   ```go
   package main

   import (
       "encoding/json"
       "net/http"
       "net/http/httptest"
       "strings"
       "testing"
   )

   // Handler to test
   func helloHandler(w http.ResponseWriter, r *http.Request) {
       name := r.URL.Query().Get("name")
       if name == "" {
           name = "World"
       }
       
       w.Header().Set("Content-Type", "application/json")
       json.NewEncoder(w).Encode(map[string]string{
           "message": "Hello, " + name + "!",
       })
   }

   // TestHelloHandler tests the hello handler
   func TestHelloHandler(t *testing.T) {
       // Create a request
       req, err := http.NewRequest("GET", "/hello?name=Alice", nil)
       if err != nil {
           t.Fatal(err)
       }
       
       // Create a response recorder
       rr := httptest.NewRecorder()
       
       // Create the handler
       handler := http.HandlerFunc(helloHandler)
       
       // Serve the request
       handler.ServeHTTP(rr, req)
       
       // Check the status code
       if status := rr.Code; status != http.StatusOK {
           t.Errorf("handler returned wrong status code: got %v want %v",
               status, http.StatusOK)
       }
       
       // Check the response body
       expected := `{"message":"Hello, Alice!"}`
       if strings.TrimSpace(rr.Body.String()) != strings.TrimSpace(expected) {
           t.Errorf("handler returned unexpected body: got %v want %v",
               rr.Body.String(), expected)
       }
   }

   // TestHelloHandlerDefault tests the hello handler with no name parameter
   func TestHelloHandlerDefault(t *testing.T) {
       // Create a request
       req, err := http.NewRequest("GET", "/hello", nil)
       if err != nil {
           t.Fatal(err)
       }
       
       // Create a response recorder
       rr := httptest.NewRecorder()
       
       // Create the handler
       handler := http.HandlerFunc(helloHandler)
       
       // Serve the request
       handler.ServeHTTP(rr, req)
       
       // Check the status code
       if status := rr.Code; status != http.StatusOK {
           t.Errorf("handler returned wrong status code: got %v want %v",
               status, http.StatusOK)
       }
       
       // Check the response body
       expected := `{"message":"Hello, World!"}`
       if strings.TrimSpace(rr.Body.String()) != strings.TrimSpace(expected) {
           t.Errorf("handler returned unexpected body: got %v want %v",
               rr.Body.String(), expected)
       }
   }
   ```

2. **Testing with a Test Server**:
   
   ```go
   package main

   import (
       "encoding/json"
       "io"
       "net/http"
       "net/http/httptest"
       "strings"
       "testing"
   )

   // BookHandler is our API handler
   func BookHandler(w http.ResponseWriter, r *http.Request) {
       w.Header().Set("Content-Type", "application/json")
       
       books := []map[string]interface{}{
           {"id": 1, "title": "Go Programming", "author": "John Doe"},
           {"id": 2, "title": "Web Development with Go", "author": "Jane Smith"},
       }
       
       json.NewEncoder(w).Encode(books)
   }

   // TestBookAPI tests the book API
   func TestBookAPI(t *testing.T) {
       // Create a test server
       server := httptest.NewServer(http.HandlerFunc(BookHandler))
       defer server.Close()
       
       // Make a request to the test server
       resp, err := http.Get(server.URL)
       if err != nil {
           t.Fatalf("Failed to make request: %v", err)
       }
       defer resp.Body.Close()
       
       // Check status code
       if resp.StatusCode != http.StatusOK {
           t.Errorf("Expected status OK; got %v", resp.Status)
       }
       
       // Read and parse the response
       body, err := io.ReadAll(resp.Body)
       if err != nil {
           t.Fatalf("Failed to read response: %v", err)
       }
       
       // Parse the JSON response
       var books []map[string]interface{}
       err = json.Unmarshal(body, &books)
       if err != nil {
           t.Fatalf("Failed to parse JSON: %v", err)
       }
       
       // Check the length of the books array
       if len(books) != 2 {
           t.Errorf("Expected 2 books; got %d", len(books))
       }
       
       // Check the first book title
       if books[0]["title"] != "Go Programming" {
           t.Errorf("Expected title 'Go Programming'; got %v", books[0]["title"])
       }
   }
   ```

**Practice Exercise**:
- Write tests for your book API
- Create a test suite that covers all endpoints and edge cases

### Section 4.3: Deploying Go Web Applications

**Objective**: Learn to deploy Go web applications

1. **Building for Production**:
   
   ```bash
   # Build for the current platform
   go build -o myapp

   # Build for a specific platform (e.g., Linux)
   GOOS=linux GOARCH=amd64 go build -o myapp

   # Build with optimization
   go build -ldflags="-s -w" -o myapp
   ```

2. **Docker Deployment**:
   
   **Dockerfile**:
   ```dockerfile
   # Start from the official Go image
   FROM golang:1.17-alpine AS builder

   # Set the working directory
   WORKDIR /app

   # Copy go.mod and go.sum files
   COPY go.mod go.sum ./

   # Download dependencies
   RUN go mod download

   # Copy the source code
   COPY . .

   # Build the application
   RUN CGO_ENABLED=0 GOOS=linux go build -o /app/myapp

   # Create a minimal image
   FROM alpine:latest

   # Add CA certificates
   RUN apk --no-cache add ca-certificates

   # Copy the binary from the builder stage
   COPY --from=builder /app/myapp /app/myapp

   # Set the working directory
   WORKDIR /app

   # Expose the port
   EXPOSE 3000

   # Run the application
   CMD ["./myapp"]
   ```

   **Building and running with Docker**:
   ```bash
   # Build the Docker image
   docker build -t myapp .

   # Run the container
   docker run -p 3000:3000 myapp
   ```

**Practice Exercise**:
- Deploy your book API to a cloud provider (Heroku, AWS, GCP, etc.)
- Set up CI/CD for your Go application

---

## Final Project: Building a Complete Web Application

**Objective**: Apply everything you've learned to build a complete web application

### Project Requirements:

1. Create a RESTful API for a task management system with the following features:
   - User registration and authentication (JWT)
   - CRUD operations for tasks
   - Task categories and priorities
   - Due dates and reminders
   - Search and filtering

2. Database integration:
   - Use a relational database (PostgreSQL, MySQL)
   - Implement proper migrations
   - Handle database connections efficiently

3. Frontend integration:
   - Serve a simple HTML/CSS/JS frontend
   - Implement API consumption with JavaScript
   - Add form validation

4. Testing and documentation:
   - Write unit and integration tests
   - Document your API with Swagger
   - Include a README with setup instructions

### Project Structure:

```
task-manager/
├── cmd/
│   └── server/
│       └── main.go         # Application entry point
├── internal/
│   ├── api/
│   │   ├── handlers/       # HTTP handlers
│   │   ├── middleware/     # Middleware functions
│   │   └── routes.go       # Route definitions
│   ├── models/             # Data models
│   └── services/           # Business logic
├── migrations/             # Database migrations
├── static/                 # Static files
│   ├── css/
│   ├── js/
│   └── index.html
├── templates/              # HTML templates
├── tests/                  # Test files
├── Dockerfile              # Docker configuration
├── docker-compose.yml      # Docker Compose configuration
├── go.mod                  # Go modules file
└── README.md               # Project documentation
```

Good luck with your Go web development journey! This self-study guide provides a structured path to master web development with Go. Take your time with each section, practice regularly, and don't hesitate to explore the Go documentation and community resources for further learning.

## Resources

### Official Documentation
- [Go Documentation](https://golang.org/doc/)
- [Go Standard Library](https://golang.org/pkg/)
- [Go Web Examples](https://gowebexamples.com/)

### Books
- "The Go Programming Language" by Alan A. A. Donovan & Brian W. Kernighan
- "Web Development with Go" by Shiju Varghese
- "Go in Action" by William Kennedy

### Online Courses and Tutorials
- [Go Tour](https://tour.golang.org/)
- [Effective Go](https://golang.org/doc/effective_go)
- [GopherAcademy](https://gopheracademy.com/)

### Communities
- [Go Forum](https://forum.golangbridge.org/)
- [r/golang](https://www.reddit.com/r/golang/)
- [Gophers Slack](https://gophers.slack.com/)

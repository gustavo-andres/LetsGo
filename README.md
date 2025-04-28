# Building Your First RESTful API with Go: A Beginner's Guide

Welcome to this step-by-step tutorial designed specifically for beginners! We'll build a RESTful API in Go together, taking small steps and checking our progress along the way. This tutorial assumes you're brand new to building APIs, so I'll explain each concept thoroughly.

## What is a RESTful API?

Before we dive in, let's understand what we're building:

- **API** (Application Programming Interface): A way for different software applications to communicate with each other.
- **REST** (Representational State Transfer): A set of rules for structuring APIs that makes them easy to use and understand.
- **RESTful API**: An API that follows REST principles, typically using HTTP methods (GET, POST, PUT, DELETE) to perform operations on resources.

In our case, we're building an API to manage a collection of books. Think of it as a virtual library where you can:
- View all books
- View a specific book
- Add a new book
- Update a book's information
- Remove a book

## Prerequisites

Before starting, make sure you have:
- Go installed on your computer (version 1.16 or later)
- A code editor (like VS Code, Sublime Text, or GoLand)
- A terminal or command prompt

Don't worry if you're not a Go expert! We'll explain everything as we go.

## Step 1: Setting Up Your Project

First, let's create a folder for our project and initialize it as a Go module.

1. Open your terminal or command prompt.
2. Create a new directory for your project:

```bash
mkdir go-books-api
cd go-books-api
```

3. Initialize a Go module. This creates a `go.mod` file which helps manage dependencies:

```bash
go mod init github.com/yourusername/go-books-api
```

You can replace "yourusername" with your actual GitHub username or any name you prefer.

**What just happened?** 
You've created a new Go project! The `go.mod` file tells Go that this folder is a module (a collection of Go packages). If you open the file, you'll see it contains the module path and the Go version.

## Step 2: Understanding Data Models

Before writing code, let's understand what we're working with. Our API will manage books, so we need to define what a "book" looks like in our system.

In programming, we use structures (or "structs" in Go) to define the shape of our data. For our book, we'll include:
- ID: A unique identifier for each book
- Title: The book's title
- Author: The book's author
- Year: The publication year

We'll also need a way to store our collection of books. Since this is a simple example, we'll use an in-memory store rather than a database.

## Step 3: Creating the Book Model

Now, let's create our first file to define the Book structure and storage.

1. Create a file called `models.go`:

```go
package main

import (
    "sync"
)

// Book represents a book in our API
type Book struct {
    ID     int    `json:"id"`
    Title  string `json:"title"`
    Author string `json:"author"`
    Year   int    `json:"year,omitempty"`
}
```

**What does this code do?**
- We define a new type called `Book` with four fields: ID, Title, Author, and Year.
- The `json:"..."` tags tell Go how to convert our Book struct to JSON format when sending responses.
- `json:"year,omitempty"` means if Year is empty (zero), it will be omitted from the JSON.

Now let's add code to manage our collection of books:

```go
// Add this to models.go

// BookStore manages our collection of books
type BookStore struct {
    books  map[int]Book
    nextID int
    mutex  sync.RWMutex
}

// NewBookStore creates a new BookStore with initial data
func NewBookStore() *BookStore {
    store := &BookStore{
        books:  make(map[int]Book),
        nextID: 1,
    }
    
    // Add some sample books
    store.AddBook(Book{Title: "The Go Programming Language", Author: "Alan Donovan & Brian Kernighan", Year: 2015})
    store.AddBook(Book{Title: "Clean Code", Author: "Robert C. Martin", Year: 2008})
    store.AddBook(Book{Title: "Design Patterns", Author: "Erich Gamma et al.", Year: 1994})
    
    return store
}
```

**What does this code do?**
- We define a `BookStore` structure to manage our books.
- `books` is a map that stores our books with their IDs as keys.
- `nextID` keeps track of the next ID to assign.
- `mutex` helps us handle concurrent access safely (we'll explain this later).
- `NewBookStore()` creates a new store and adds some sample books.

Let's save this file. Note that we can't run `go build` just yet because we're referencing the `AddBook()` function in our `NewBookStore()` function, but we haven't defined it yet. We'll add that in the next step.

## Step 4: Adding BookStore Methods

Now let's add functions to perform operations on our book store. Add these to your `models.go` file:

```go
// Add these functions to models.go

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
```

**Let's break down what each function does:**

1. **AddBook**:
   - Adds a new book to our store
   - Assigns an ID to the book
   - Increments `nextID` for the next book

2. **GetBooks**:
   - Returns all books as a slice (like an array)

3. **GetBook**:
   - Returns a specific book by its ID
   - Also returns a boolean indicating if the book was found

4. **UpdateBook**:
   - Updates an existing book
   - Returns the updated book and a success indicator

5. **DeleteBook**:
   - Removes a book from the store
   - Returns true if successful, false if the book wasn't found

**What are those mutex things?**
The `mutex.Lock()` and `mutex.RLock()` calls help prevent problems when multiple users access our API simultaneously. Think of them as a "busy" sign that prevents conflicts. We'll use `Lock()` when modifying data and `RLock()` (Read Lock) when just reading data.

Now that we've added all the necessary functions for our BookStore, let's make sure our code compiles:

```bash
go build
```

No errors? Great! Now let's create a simple test program to see if our book store works as expected.

## Step 5: Testing Our Book Store

Let's create a simple `main.go` file to test our book store:

```go
package main

import (
    "fmt"
)

func main() {
    // Create a new book store
    store := NewBookStore()
    
    // Print all books
    fmt.Println("All books:")
    for _, book := range store.GetBooks() {
        fmt.Printf("ID: %d, Title: %s, Author: %s, Year: %d\n", 
                  book.ID, book.Title, book.Author, book.Year)
    }
    
    // Add a new book
    newBook := Book{Title: "Go Web Programming", Author: "Sau Sheong Chang", Year: 2016}
    addedBook := store.AddBook(newBook)
    fmt.Printf("\nAdded book: ID: %d, Title: %s\n", addedBook.ID, addedBook.Title)
    
    // Get a specific book
    book, found := store.GetBook(2)
    if found {
        fmt.Printf("\nFound book: ID: %d, Title: %s\n", book.ID, book.Title)
    } else {
        fmt.Println("\nBook not found")
    }
    
    // Update a book
    updatedBook := Book{Title: "Clean Code", Author: "Uncle Bob", Year: 2008}
    _, success := store.UpdateBook(2, updatedBook)
    if success {
        book, _ := store.GetBook(2)
        fmt.Printf("\nUpdated book: ID: %d, Title: %s, Author: %s\n", 
                 book.ID, book.Title, book.Author)
    }
    
    // Delete a book
    success = store.DeleteBook(3)
    if success {
        fmt.Println("\nBook deleted successfully")
    }
    
    // Print all books again
    fmt.Println("\nAll books after changes:")
    for _, book := range store.GetBooks() {
        fmt.Printf("ID: %d, Title: %s, Author: %s, Year: %d\n", 
                book.ID, book.Title, book.Author, book.Year)
    }
}
```

Run this program to see if our book store works correctly:

```bash
go run .
```

You should see output showing:
1. All the initial books
2. A new book being added
3. A book being found by ID
4. A book being updated
5. A book being deleted
6. The final list of books reflecting all changes

If everything looks good, we're ready to create our API!

## Step 6: Creating API Handlers

Now that our book store works, let's build the API around it. We'll create functions to handle HTTP requests.

Create a new file called `handlers.go`:

```go
package main

import (
    "encoding/json"
    "fmt"
    "net/http"
    "strconv"
    "strings"
)

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
```

**What does this code do?**
- `getBooks`: Gets all books and sends them as JSON
- `getBook`: Gets a specific book by ID and sends it as JSON (or returns a "not found" error)

The `w` parameter is a `ResponseWriter` that lets us send data back to the client, and `r` is the incoming request.

Now let's add handlers for creating, updating, and deleting books:

```go
// Add these functions to handlers.go

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

**What does this code do?**
- `createBook`: Decodes a JSON book from the request, validates it, adds it to the store, and returns the created book
- `updateBook`: Similar to `createBook`, but updates an existing book
- `deleteBook`: Deletes a book by ID

Let's make sure our code still compiles:

```bash
go build
```

## Step 7: Creating the Web Server

Now we need to update our `main.go` file to start a web server and route requests to our handlers. Replace the contents of `main.go` with:

```go
package main

import (
    "fmt"
    "log"
    "net/http"
    "strconv"
    "strings"
)

func main() {
    store := NewBookStore()
    
    // Handle /api/books endpoints
    http.HandleFunc("/api/books", func(w http.ResponseWriter, r *http.Request) {
        switch r.Method {
        case http.MethodGet:
            getBooks(store, w, r)
        case http.MethodPost:
            createBook(store, w, r)
        default:
            w.WriteHeader(http.StatusMethodNotAllowed)
            fmt.Fprintf(w, "Method not allowed")
        }
    })
    
    // Handle /api/books/{id} endpoints
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
            fmt.Fprintf(w, "Method not allowed")
        }
    })
    
    // Start the server
    fmt.Println("API server starting on port 3000...")
    log.Fatal(http.ListenAndServe(":3000", nil))
}
```

**What does this code do?**
- Creates a new book store
- Sets up two HTTP handlers:
  - `/api/books` for listing all books and creating new ones
  - `/api/books/` (with an ID) for operations on specific books
- Routes requests to the appropriate handler based on the HTTP method (GET, POST, PUT, DELETE)
- Starts a server on port 3000

## Step 8: Running and Testing Our API

Let's run our API and see if it works:

```bash
go run .
```

You should see: "API server starting on port 3000..."

Your API is now running! Let's test it using curl commands. Open a new terminal window (keep the server running in the first one) and run these commands:

### List all books

```bash
curl -X GET http://localhost:3000/api/books
```

You should see JSON output listing all the books.

### Get a specific book

```bash
curl -X GET http://localhost:3000/api/books/1
```

You should see the details of the first book.

### Create a new book

```bash
curl -X POST http://localhost:3000/api/books \
  -H "Content-Type: application/json" \
  -d '{"title":"Effective Go","author":"Google","year":2009}'
```

You should see the details of the newly created book, including its assigned ID.

### Update a book

```bash
curl -X PUT http://localhost:3000/api/books/4 \
  -H "Content-Type: application/json" \
  -d '{"title":"Effective Go","author":"Google Inc.","year":2009}'
```

You should see the updated book details.

### Delete a book

```bash
curl -X DELETE http://localhost:3000/api/books/4
```

There should be no output, but if you list all books again, the deleted book should be gone.

## Step 9: Understanding What's Happening

Let's break down what happens when someone makes a request to our API:

1. A client (like a web browser or another program) sends an HTTP request to our server.
2. Our server receives the request and routes it to the appropriate handler based on the URL and HTTP method.
3. The handler processes the request, interacts with our book store, and prepares a response.
4. The server sends the response back to the client.

For example, when someone sends a GET request to `/api/books`:
1. The request is routed to the `getBooks` handler.
2. `getBooks` calls `store.GetBooks()` to get all books.
3. It converts the books to JSON format.
4. It sends the JSON back to the client.

## Step 10: Adding Middleware for Logging and Error Recovery

Let's add some "middleware" to log requests and recover from panics (unexpected errors). Create a file called `middleware.go`:

```go
package main

import (
    "log"
    "net/http"
    "time"
)

// loggingMiddleware logs HTTP requests
func loggingMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        // Log the start of the request
        start := time.Now()
        log.Printf("Started %s %s", r.Method, r.URL.Path)
        
        // Call the next handler
        next.ServeHTTP(w, r)
        
        // Log the end of the request
        log.Printf("Completed %s %s in %v", r.Method, r.URL.Path, time.Since(start))
    })
}

// recoverMiddleware recovers from panics
func recoverMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        // Defer a function to recover from panics
        defer func() {
            if err := recover(); err != nil {
                log.Printf("Panic: %v", err)
                http.Error(w, "Internal server error", http.StatusInternalServerError)
            }
        }()
        
        // Call the next handler
        next.ServeHTTP(w, r)
    })
}
```

**What does this code do?**
- `loggingMiddleware`: Logs when requests start and end, and how long they take.
- `recoverMiddleware`: Catches panics (unexpected errors) and returns a friendly error message instead of crashing the server.

Now update `main.go` to use these middleware:

```go
package main

import (
    "fmt"
    "log"
    "net/http"
    "strconv"
    "strings"
)

func main() {
    store := NewBookStore()
    
    // Create a new router
    mux := http.NewServeMux()
    
    // Register handlers
    mux.HandleFunc("/api/books", func(w http.ResponseWriter, r *http.Request) {
        switch r.Method {
        case http.MethodGet:
            getBooks(store, w, r)
        case http.MethodPost:
            createBook(store, w, r)
        default:
            w.WriteHeader(http.StatusMethodNotAllowed)
            fmt.Fprintf(w, "Method not allowed")
        }
    })
    
    mux.HandleFunc("/api/books/", func(w http.ResponseWriter, r *http.Request) {
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
            fmt.Fprintf(w, "Method not allowed")
        }
    })
    
    // Apply middleware
    handler := loggingMiddleware(recoverMiddleware(mux))
    
    // Start the server
    fmt.Println("API server starting on port 3000...")
    log.Fatal(http.ListenAndServe(":3000", handler))
}
```

The main difference is that we now use a `mux` (a router) and wrap it with our middleware.

## Step 11: Final Test

Let's run our API with the middleware:

```bash
go run .
```

Now when you make requests, you should see log messages in the terminal where the server is running. Try making some requests (like listing all books) and watch the logs.

## Conclusion

Congratulations! You've built a complete RESTful API using Go. Here's what you've learned:

1. **How to structure a Go project**: Creating a module, organizing code into multiple files.
2. **How to define data models**: Using structs to represent your data.
3. **How to handle concurrent access**: Using mutexes to prevent data corruption.
4. **How to create API handlers**: Converting between HTTP and your application logic.
5. **How to set up a web server**: Routing requests to the appropriate handlers.
6. **How to add middleware**: Enhancing your server with logging and error recovery.

This is just the beginning! Here are some ideas for further improvement:

- **Add a database**: Replace the in-memory store with a real database.
- **Add authentication**: Make sure only authorized users can modify books.
- **Create a web interface**: Build a frontend to interact with your API.
- **Deploy your API**: Learn how to make your API available on the internet.

I hope you enjoyed this tutorial! Feel free to experiment with the code and make it your own.

## Troubleshooting Common Issues

### "Address already in use" error
If you see this error, it means port 3000 is already being used by another program. You can change the port number in the `http.ListenAndServe(":3000", handler)` line to another number (like 3001).

### No response from curl
Make sure your server is running in a separate terminal window. Also, check that you're using the correct URL.

### JSON parsing errors
Double-check your JSON syntax in curl commands. JSON requires double quotes around field names.

### Panic errors
If your server crashes with a panic, check the error message and fix the code. The recover middleware should catch most panics, but it's better to fix the underlying issue.

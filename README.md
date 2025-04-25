# LetsGo: Learning Series
# Building Your First RESTful API with Go: Step-by-Step Tutorial

Welcome to this hands-on tutorial where you'll build a complete RESTful API using Go. By following these steps, you'll create a fully functional book management API with proper error handling, data validation, and clean code structure.

## What You'll Build

A RESTful API for managing a collection of books with the following endpoints:
- `GET /api/books` - List all books
- `GET /api/books/{id}` - Get a specific book
- `POST /api/books` - Create a new book
- `PUT /api/books/{id}` - Update a book
- `DELETE /api/books/{id}` - Delete a book

## Prerequisites

- Go installed (version 1.16 or later)
- A code editor (VS Code, GoLand, etc.)
- Basic knowledge of Go syntax

## Step 1: Set Up Your Project

First, create a new directory for your project and initialize a Go module:

```bash
mkdir go-books-api
cd go-books-api
go mod init github.com/yourusername/go-books-api
```

## Step 2: Define Your Data Model

Create a file called `models.go` with the following content:

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

## Step 3: Create Your Handlers

Create a file called `handlers.go` with the following content:

```go
package main

import (
    "encoding/json"
    "fmt"
    "net/http"
    "strconv"
    "strings"
)

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

## Step 4: Create the Main Application

Create a file called `main.go` with the following content:

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
    
    // Handle /books endpoints
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
            fmt.Fprintf(w, "Method not allowed")
        }
    })
    
    // Start the server
    fmt.Println("API server starting on port 3000...")
    log.Fatal(http.ListenAndServe(":3000", nil))
}
```

## Step 5: Run Your API

Save all the files and run your API with:

```bash
go run .
```

You should see the message: "API server starting on port 3000..."

## Step 6: Test Your API

Now let's test the API using curl commands:

### List all books

```bash
curl -X GET http://localhost:3000/api/books
```

Expected output:
```json
[
  {"id":1,"title":"The Go Programming Language","author":"Alan Donovan \u0026 Brian Kernighan","year":2015},
  {"id":2,"title":"Clean Code","author":"Robert C. Martin","year":2008},
  {"id":3,"title":"Design Patterns","author":"Erich Gamma et al.","year":1994}
]
```

### Get a specific book

```bash
curl -X GET http://localhost:3000/api/books/1
```

Expected output:
```json
{"id":1,"title":"The Go Programming Language","author":"Alan Donovan \u0026 Brian Kernighan","year":2015}
```

### Create a new book

```bash
curl -X POST http://localhost:3000/api/books \
  -H "Content-Type: application/json" \
  -d '{"title":"Effective Go","author":"Google","year":2009}'
```

Expected output:
```json
{"id":4,"title":"Effective Go","author":"Google","year":2009}
```

### Update a book

```bash
curl -X PUT http://localhost:3000/api/books/4 \
  -H "Content-Type: application/json" \
  -d '{"title":"Effective Go","author":"Google Inc.","year":2009}'
```

Expected output:
```json
{"id":4,"title":"Effective Go","author":"Google Inc.","year":2009}
```

### Delete a book

```bash
curl -X DELETE http://localhost:3000/api/books/4
```

Expected output: (empty response with 204 No Content status)

## Step 7: Add Middleware and Error Handling

Create a file called `middleware.go` with the following content:

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
        start := time.Now()
        log.Printf("Started %s %s", r.Method, r.URL.Path)
        
        next.ServeHTTP(w, r)
        
        log.Printf("Completed %s %s in %v", r.Method, r.URL.Path, time.Since(start))
    })
}

// recoverMiddleware recovers from panics
func recoverMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        defer func() {
            if err := recover(); err != nil {
                log.Printf("Panic: %v", err)
                http.Error(w, "Internal server error", http.StatusInternalServerError)
            }
        }()
        
        next.ServeHTTP(w, r)
    })
}
```

Then, update your `main.go` to use these middleware:

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

## Step 8: Enhancements (Optional)

Here are some enhancements you can add to your API:

### 1. Add a simple client webpage
Create a folder called `static` and add an `index.html` file with JavaScript to interact with your API.

### 2. Add validation middleware
Create a middleware that validates incoming requests before they reach your handlers.

### 3. Add authentication
Implement JWT authentication to protect your API endpoints.

### 4. Connect to a database
Replace the in-memory storage with a database like PostgreSQL or MongoDB.

## Conclusion

Congratulations! You've built a complete RESTful API using Go. This API includes:

- Clean architecture with models, handlers, and middleware
- Support for all CRUD operations
- Proper error handling and validation
- Concurrency-safe data access with mutexes
- HTTP status codes according to RESTful principles

This is just the beginning. From here, you can extend your API with more features like authentication, database integration, and a frontend interface.

Happy coding!

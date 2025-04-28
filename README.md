# Building a RESTful API with Go: Student Assignment

## Overview

In this assignment, you will build a RESTful API using Go to manage a collection of books. This project will help you understand the fundamentals of API development, HTTP methods, and basic CRUD (Create, Read, Update, Delete) operations.

## Learning Objectives

By the end of this assignment, you should be able to:
- Set up a Go project from scratch
- Create data models and in-memory storage
- Implement HTTP handlers for API endpoints
- Handle different HTTP methods (GET, POST, PUT, DELETE)
- Implement basic error handling and middleware
- Test a RESTful API using curl or similar tools

## Assignment Requirements

Your task is to build a book management API with the following requirements:

### Endpoints
Create a RESTful API with the following endpoints:
- `GET /api/books` - List all books
- `GET /api/books/{id}` - Get a specific book
- `POST /api/books` - Create a new book
- `PUT /api/books/{id}` - Update a book
- `DELETE /api/books/{id}` - Delete a book

### Data Model
Each book should have the following properties:
- ID (integer)
- Title (string)
- Author (string)
- Year (integer, optional)

### Technical Requirements
- Use the standard Go libraries (no external frameworks)
- Implement proper error handling
- Return appropriate HTTP status codes
- Use JSON for data exchange
- Implement thread-safe operations using mutex

## Step-by-Step Instructions

Follow these steps to complete the assignment. Try to solve each step on your own before looking at hints or asking for help.

### Step 1: Project Setup (15 minutes)
1. Create a new directory for your project
2. Initialize a Go module
3. Think about the files you'll need to organize your code

**Checkpoint:** You should have a project directory with a `go.mod` file.

### Step 2: Define Your Data Model (30 minutes)
1. Create a file for your data models
2. Define a struct for the Book type
3. Create a BookStore type to manage your collection of books
4. Add methods to:
   - Add a new book
   - Get all books
   - Get a book by ID
   - Update a book
   - Delete a book
5. Make sure your operations are thread-safe using mutex

**Tip:** Think about how you'll generate and manage book IDs.

**Checkpoint:** Test your BookStore functionality with a simple program before moving on.

### Step 3: Create HTTP Handlers (45 minutes)
1. Create handler functions for each API endpoint:
   - List all books
   - Get a specific book
   - Create a new book
   - Update a book
   - Delete a book
2. Implement proper error handling and HTTP status codes
3. Add request validation (e.g., required fields)

**Tip:** Each handler should correspond to one of the CRUD operations in your BookStore.

**Checkpoint:** Your handlers should be defined but not yet connected to HTTP routes.

### Step 4: Set Up the Web Server (30 minutes)
1. Create a main function to start your server
2. Set up HTTP routes to direct requests to your handlers
3. Parse URL paths to extract book IDs
4. Route requests to the appropriate handler based on the HTTP method

**Checkpoint:** Your server should start and listen on a port (e.g., 3000).

### Step 5: Implement Middleware (Optional, 30 minutes)
1. Create logging middleware to log HTTP requests
2. Create recovery middleware to handle panics
3. Apply the middleware to your server

**Checkpoint:** When you run your server with middleware, you should see log messages for each request.

### Step 6: Test Your API (30 minutes)
Test each endpoint using curl or a similar tool:
1. List all books
2. Get a specific book
3. Create a new book
4. Update a book
5. Delete a book

For each test, verify that:
- The request succeeds with the expected status code
- The response contains the expected data
- Error cases are handled properly

## Hints and Tips

Here are some hints to help if you get stuck:

- **Data Storage**: Use a map[int]Book for in-memory storage
- **Thread Safety**: Use sync.RWMutex for read/write operations
- **JSON Handling**: Use `json.NewEncoder(w).Encode(data)` to send JSON responses
- **URL Parsing**: Use `strings.TrimPrefix(r.URL.Path, "/api/books/")` to extract the book ID
- **Request Decoding**: Use `json.NewDecoder(r.Body).Decode(&book)` to parse JSON requests
- **HTTP Status Codes**: 
  - 200 OK for successful GET requests
  - 201 Created for successful POST requests
  - 204 No Content for successful DELETE requests
  - 400 Bad Request for invalid input
  - 404 Not Found when a resource doesn't exist
  - 405 Method Not Allowed for unsupported HTTP methods

## Challenge Extensions

If you finish early or want to challenge yourself further:

1. Add query parameters to filter books (e.g., by author or year)
2. Implement pagination for the list endpoint
3. Add validation to ensure book titles are unique
4. Add a simple HTML frontend to interact with your API
5. Replace the in-memory storage with a database

## Submission Guidelines

Your submission should include:
1. All source code files
2. A README.md explaining how to run your application
3. A brief reflection on what you learned and any challenges you faced

## Evaluation Criteria

Your assignment will be evaluated based on:
1. **Functionality**: Does the API work as required?
2. **Code Quality**: Is the code well-organized, readable, and properly commented?
3. **Error Handling**: Does the API handle errors and edge cases appropriately?
4. **API Design**: Does the API follow RESTful principles?
5. **Threading**: Is the API thread-safe?

## Getting Help

If you get stuck, try these resources:
- Go's official documentation (especially the net/http package)
- The course materials on API development
- Discussion forums for specific questions

Good luck, and enjoy building your first Go API!

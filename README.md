# LetsGo: First Coding Question
# Go Workshop: From Basics to Frequency Analysis

This guide outlines a 10-step workshop to teach Go programming fundamentals, gradually building toward solving a frequency distribution problem. Each step introduces new concepts with practical examples.

## Workshop Overview

- **Target Audience**: Beginners with limited programming experience
- **Final Project**: Calculate and display the top 3 authors by post frequency

## Step 1: Basic Syntax and Data Types

### Lesson Goals
- Understand Go's basic syntax
- Learn about variables and data types
- Implement simple arithmetic operations

### Code Example
```go
package main

import "fmt"

func main() {
    // Variables and basic types
    var name string = "Marcus"
    age := 28 // Type inference with :=
    
    fmt.Println("Hello,", name)
    fmt.Println("You are", age, "years old")
    
    // Basic arithmetic
    postCount := 5
    totalPosts := 20
    percentage := float64(postCount) / float64(totalPosts) * 100
    
    fmt.Printf("%s has %.1f%% of all posts\n", name, percentage)
}
```

### Teaching Notes
1. Explain the basic structure of a Go program
2. Demonstrate variable declaration with and without type
3. Show how to use the `fmt` package for output
4. Explain type conversion (int to float64)
5. Demonstrate string formatting with `Printf`

### Exercise
Have students modify the program to:
1. Add another author with a different post count
2. Calculate and display the percentage for both authors

## Step 2: Arrays and Slices

### Lesson Goals
- Understand fixed-size arrays vs. dynamic slices
- Learn to iterate through collections
- Apply these concepts to our author/post scenario

### Code Example
```go
package main

import "fmt"

func main() {
    // Fixed-size array
    var authors [3]string
    authors[0] = "Marcus"
    authors[1] = "Peter"
    authors[2] = "Caique"
    
    // Dynamic slice
    posts := []int{5, 2, 4} // Number of posts for each author
    
    fmt.Println("Authors:", authors)
    fmt.Println("Post counts:", posts)
    
    // Iterating through slices
    fmt.Println("Author post counts:")
    for i := 0; i < len(authors); i++ {
        fmt.Printf("%s has %d posts\n", authors[i], posts[i])
    }
    
    // Alternative way to iterate (range)
    fmt.Println("\nUsing range:")
    for i, author := range authors {
        fmt.Printf("%s has %d posts\n", author, posts[i])
    }
}
```

### Teaching Notes
1. Explain differences between arrays and slices
2. Demonstrate multiple ways to initialize arrays/slices
3. Show both traditional `for` loops and `range`-based loops
4. Explain the relationship between the two collections (authors and posts)

### Exercise
Have students:
1. Add two more authors and their post counts
2. Calculate and display the percentage of posts for each author

## Step 3: Structs and Custom Types

### Lesson Goals
- Define custom types using structs
- Understand how to model real-world entities
- Work with dates using the time package

### Code Example
```go
package main

import (
    "fmt"
    "time"
)

// Define Author struct
type Author struct {
    ID   int
    Name string
}

// Define Post struct
type Post struct {
    ID        int
    AuthorID  int
    CreatedAt time.Time
}

func main() {
    // Create an author
    author1 := Author{
        ID:   1,
        Name: "Marcus",
    }
    
    // Create a post
    post1 := Post{
        ID:        1,
        AuthorID:  author1.ID,
        CreatedAt: time.Date(2025, 3, 7, 0, 0, 0, 0, time.UTC),
    }
    
    fmt.Println("Author:", author1.Name)
    fmt.Println("Post ID:", post1.ID)
    fmt.Println("Post created at:", post1.CreatedAt.Format("2006-01-02"))
    
    // Create another author and post
    author2 := Author{ID: 2, Name: "Peter"}
    post2 := Post{
        ID:        2,
        AuthorID:  author2.ID,
        CreatedAt: time.Date(2025, 3, 8, 0, 0, 0, 0, time.UTC),
    }
    
    fmt.Printf("\nAuthor %s wrote post %d on %s\n", 
        author2.Name, 
        post2.ID, 
        post2.CreatedAt.Format("2006-01-02"))
}
```

### Teaching Notes
1. Explain how structs help organize related data
2. Show how to define and create struct instances
3. Demonstrate the relationship between structs (Author ID in Post)
4. Explain Go's time package and date formatting

### Exercise
Have students:
1. Create an array of 3 authors
2. Create 5 posts distributed among these authors
3. Print out which author wrote each post

## Step 4: Maps for Key-Value Storage

### Lesson Goals
- Understand maps for key-value relationships
- Learn to check for key existence
- Iterate through maps
- Apply maps to our frequency problem

### Code Example
```go
package main

import "fmt"

func main() {
    // Create a map with string keys and int values
    postCounts := make(map[string]int)
    
    // Add data to map
    postCounts["Marcus"] = 2
    postCounts["Peter"] = 2
    postCounts["Caique"] = 4
    
    // Access map values
    fmt.Println("Marcus has", postCounts["Marcus"], "posts")
    
    // Check if key exists
    if count, exists := postCounts["Kyle"]; exists {
        fmt.Println("Kyle has", count, "posts")
    } else {
        fmt.Println("Kyle has no posts")
    }
    
    // Add Kyle to the map
    postCounts["Kyle"] = 1
    
    // Iterate through map
    totalPosts := 0
    for author, count := range postCounts {
        fmt.Printf("%s has %d posts\n", author, count)
        totalPosts += count
    }
    
    fmt.Printf("\nTotal posts: %d\n", totalPosts)
    
    // Calculate and display percentages
    fmt.Println("\nPost distribution:")
    for author, count := range postCounts {
        percentage := float64(count) / float64(totalPosts) * 100
        fmt.Printf("%s: %.1f%%\n", author, percentage)
    }
}
```

### Teaching Notes
1. Explain why maps are useful (lookup by key)
2. Demonstrate different ways to create and initialize maps
3. Show how to check if a key exists
4. Explain that map iteration order is not guaranteed
5. Connect this to our frequency calculation problem

### Exercise
Have students:
1. Create a map that stores the favorite programming language for each student
2. Count how many students prefer each language
3. Calculate the percentage for each language

## Step 5: Functions and Returns

### Lesson Goals
- Write reusable functions
- Return multiple values from functions
- Structure code more effectively

### Code Example
```go
package main

import "fmt"

// Function to calculate percentage
func calculatePercentage(count, total int) float64 {
    return float64(count) / float64(total) * 100
}

// Function to get post frequency for an author
func getAuthorFrequency(authorName string, postCount, totalPosts int) (string, float64) {
    percentage := calculatePercentage(postCount, totalPosts)
    return authorName, percentage
}

func main() {
    totalPosts := 10
    
    authorName, frequency := getAuthorFrequency("Caique", 4, totalPosts)
    fmt.Printf("%s has %.1f%% of all posts\n", authorName, frequency)
    
    // Call for multiple authors
    authors := []string{"Marcus", "Peter", "Renan", "Kyle"}
    counts := []int{2, 2, 1, 1}
    
    fmt.Println("\nAll author frequencies:")
    for i := 0; i < len(authors); i++ {
        name, freq := getAuthorFrequency(authors[i], counts[i], totalPosts)
        fmt.Printf("%s has %.1f%% of all posts\n", name, freq)
    }
    
    // Calculate using a map
    postCounts := map[string]int{
        "Caique": 4,
        "Marcus": 2,
        "Peter":  2,
        "Renan":  1,
        "Kyle":   1,
    }
    
    fmt.Println("\nUsing map:")
    for author, count := range postCounts {
        _, freq := getAuthorFrequency(author, count, totalPosts)
        fmt.Printf("%s: %.1f%%\n", author, freq)
    }
}
```

### Teaching Notes
1. Explain how functions promote code reuse
2. Demonstrate parameters and return values
3. Show how to return and receive multiple values
4. Connect these functions to our frequency calculation problem

### Exercise
Have students:
1. Write a function that takes an author name and returns their post count
2. Write a function that determines if an author is in the "top 3" by post count
3. Use these functions with the existing data

## Step 6: Control Flow and Sorting

### Lesson Goals
- Understand more complex control structures
- Learn to sort data with custom criteria
- Use anonymous functions (lambdas)

### Code Example
```go
package main

import (
    "fmt"
    "sort"
)

// Define a struct to hold author frequency data
type AuthorFrequency struct {
    Name      string
    Frequency float64
}

func main() {
    // Create a slice of AuthorFrequency
    frequencies := []AuthorFrequency{
        {"Caique", 40.0},
        {"Marcus", 20.0},
        {"Peter", 20.0},
        {"Renan", 10.0},
        {"Kyle", 10.0},
    }
    
    // Print original order
    fmt.Println("Original order:")
    for _, af := range frequencies {
        fmt.Printf("%s: %.1f%%\n", af.Name, af.Frequency)
    }
    
    // Sort by frequency (descending)
    sort.Slice(frequencies, func(i, j int) bool {
        return frequencies[i].Frequency > frequencies[j].Frequency
    })
    
    // Print sorted order
    fmt.Println("\nSorted by frequency (descending):")
    for _, af := range frequencies {
        fmt.Printf("%s: %.1f%%\n", af.Name, af.Frequency)
    }
    
    // Print the top 3
    fmt.Println("\nTop 3 authors by post frequency:")
    topCount := 3
    if len(frequencies) < 3 {
        topCount = len(frequencies)
    }
    
    for i := 0; i < topCount; i++ {
        fmt.Printf("%s: %.1f%%\n", 
            frequencies[i].Name, 
            frequencies[i].Frequency)
    }
}
```

### Teaching Notes
1. Explain why sorting is important for our problem
2. Demonstrate Go's `sort` package
3. Introduce anonymous functions (lambdas) for sorting criteria
4. Show how to extract the top N elements after sorting

### Exercise
Have students:
1. Modify the program to sort alphabetically by author name
2. Add logic to handle ties in frequency (secondary sort by name)
3. Implement a different top-N function (e.g., bottom 2)

## Step 7: Creating Collections of Structs

### Lesson Goals
- Work with collections of custom types
- Build the foundation of our solution
- Understand relationships between collections

### Code Example
```go
package main

import (
    "fmt"
    "time"
)

type Author struct {
    ID   int
    Name string
}

type Post struct {
    ID        int
    AuthorID  int
    CreatedAt time.Time
}

func main() {
    // Create a slice of authors
    authors := []Author{
        {ID: 1, Name: "Marcus"},
        {ID: 2, Name: "Peter"},
        {ID: 3, Name: "Renan"},
        {ID: 4, Name: "Kyle"},
        {ID: 5, Name: "Caique"},
    }
    
    // Create a slice of posts
    posts := []Post{
        {ID: 1, AuthorID: 5, CreatedAt: time.Date(2025, 3, 3, 0, 0, 0, 0, time.UTC)},
        {ID: 2, AuthorID: 5, CreatedAt: time.Date(2025, 3, 4, 0, 0, 0, 0, time.UTC)},
        {ID: 3, AuthorID: 5, CreatedAt: time.Date(2025, 3, 5, 0, 0, 0, 0, time.UTC)},
        {ID: 4, AuthorID: 5, CreatedAt: time.Date(2025, 3, 6, 0, 0, 0, 0, time.UTC)},
        {ID: 5, AuthorID: 1, CreatedAt: time.Date(2025, 3, 7, 0, 0, 0, 0, time.UTC)},
        {ID: 6, AuthorID: 1, CreatedAt: time.Date(2025, 3, 8, 0, 0, 0, 0, time.UTC)},
    }
    
    // Print all authors
    fmt.Println("Authors:")
    for _, author := range authors {
        fmt.Printf("ID: %d, Name: %s\n", author.ID, author.Name)
    }
    
    // Print all posts
    fmt.Println("\nPosts:")
    for _, post := range posts {
        fmt.Printf("ID: %d, AuthorID: %d, Date: %s\n", 
            post.ID, 
            post.AuthorID, 
            post.CreatedAt.Format("2006-01-02"))
    }
    
    // Count posts by author ID
    postsByAuthor := make(map[int]int)
    for _, post := range posts {
        postsByAuthor[post.AuthorID]++
    }
    
    // Print post counts
    fmt.Println("\nPost counts by author ID:")
    for authorID, count := range postsByAuthor {
        fmt.Printf("Author ID %d: %d posts\n", authorID, count)
    }
    
    // Match author IDs to names
    fmt.Println("\nPost counts by author name:")
    for _, author := range authors {
        if count, exists := postsByAuthor[author.ID]; exists {
            fmt.Printf("%s: %d posts\n", author.Name, count)
        } else {
            fmt.Printf("%s: 0 posts\n", author.Name)
        }
    }
}
```

### Teaching Notes
1. Demonstrate how to create and work with slices of structs
2. Show the relationship between authors and posts via IDs
3. Introduce counting with maps
4. Explain how to join data from different collections

### Exercise
Have students:
1. Add more posts to the collection
2. Implement a function to find all posts by a specific author
3. Count posts by month (using the CreatedAt field)

## Step 8: Putting It All Together - First Attempt

### Lesson Goals
- Begin implementing the full solution
- Calculate frequencies from our data
- Structure the solution in logical steps

### Code Example
```go
package main

import (
    "fmt"
    "time"
)

type Author struct {
    ID   int
    Name string
}

type Post struct {
    ID        int
    AuthorID  int
    CreatedAt time.Time
}

func main() {
    // Create sample data
    authors := []Author{
        {ID: 1, Name: "Marcus"},
        {ID: 2, Name: "Peter"},
        {ID: 3, Name: "Renan"},
        {ID: 4, Name: "Kyle"},
        {ID: 5, Name: "Caique"},
    }
    
    posts := []Post{
        {ID: 1, AuthorID: 5, CreatedAt: time.Date(2025, 3, 3, 0, 0, 0, 0, time.UTC)},
        {ID: 2, AuthorID: 5, CreatedAt: time.Date(2025, 3, 4, 0, 0, 0, 0, time.UTC)},
        {ID: 3, AuthorID: 5, CreatedAt: time.Date(2025, 3, 5, 0, 0, 0, 0, time.UTC)},
        {ID: 4, AuthorID: 5, CreatedAt: time.Date(2025, 3, 6, 0, 0, 0, 0, time.UTC)},
        {ID: 5, AuthorID: 1, CreatedAt: time.Date(2025, 3, 7, 0, 0, 0, 0, time.UTC)},
        {ID: 6, AuthorID: 1, CreatedAt: time.Date(2025, 3, 8, 0, 0, 0, 0, time.UTC)},
        {ID: 7, AuthorID: 2, CreatedAt: time.Date(2025, 3, 9, 0, 0, 0, 0, time.UTC)},
        {ID: 8, AuthorID: 2, CreatedAt: time.Date(2025, 3, 10, 0, 0, 0, 0, time.UTC)},
        {ID: 9, AuthorID: 3, CreatedAt: time.Date(2025, 3, 11, 0, 0, 0, 0, time.UTC)},
        {ID: 10, AuthorID: 4, CreatedAt: time.Date(2025, 3, 12, 0, 0, 0, 0, time.UTC)},
    }
    
    // Step 1: Count posts by author ID
    postCounts := make(map[int]int)
    for _, post := range posts {
        postCounts[post.AuthorID]++
    }
    
    // Step 2: Create a map to lookup author names by ID
    authorNames := make(map[int]string)
    for _, author := range authors {
        authorNames[author.ID] = author.Name
    }
    
    // Step 3: Calculate frequencies
    totalPosts := len(posts)
    frequencies := make(map[string]float64)
    
    for authorID, count := range postCounts {
        authorName := authorNames[authorID]
        percentage := float64(count) * 100 / float64(totalPosts)
        frequencies[authorName] = percentage
    }
    
    // Print results
    fmt.Println("Author post frequencies:")
    for name, freq := range frequencies {
        fmt.Printf("%s %.1f\n", name, freq)
    }
}
```

### Teaching Notes
1. Explain the step-by-step approach to solving the problem
2. Demonstrate how to build a lookup map for efficiency
3. Show how to calculate percentages from counts
4. Discuss the limitations of this approach (not sorted, not limited to top 3)

### Exercise
Have students:
1. Modify the code to handle authors with no posts
2. Add error handling for missing author IDs
3. Format the output to match the required precision

## Step 9: Adding Sorting and Top-N Selection

### Lesson Goals
- Sort the results by frequency
- Select the top N results
- Understand how to work with intermediate data structures

### Code Example
```go
package main

import (
    "fmt"
    "sort"
    "time"
)

type Author struct {
    ID   int
    Name string
}

type Post struct {
    ID        int
    AuthorID  int
    CreatedAt time.Time
}

type AuthorFrequency struct {
    Name      string
    Frequency float64
}

func main() {
    // Create sample data (same as before)
    authors := []Author{
        {ID: 1, Name: "Marcus"},
        {ID: 2, Name: "Peter"},
        {ID: 3, Name: "Renan"},
        {ID: 4, Name: "Kyle"},
        {ID: 5, Name: "Caique"},
    }
    
    posts := []Post{
        {ID: 1, AuthorID: 5, CreatedAt: time.Date(2025, 3, 3, 0, 0, 0, 0, time.UTC)},
        {ID: 2, AuthorID: 5, CreatedAt: time.Date(2025, 3, 4, 0, 0, 0, 0, time.UTC)},
        {ID: 3, AuthorID: 5, CreatedAt: time.Date(2025, 3, 5, 0, 0, 0, 0, time.UTC)},
        {ID: 4, AuthorID: 5, CreatedAt: time.Date(2025, 3, 6, 0, 0, 0, 0, time.UTC)},
        {ID: 5, AuthorID: 1, CreatedAt: time.Date(2025, 3, 7, 0, 0, 0, 0, time.UTC)},
        {ID: 6, AuthorID: 1, CreatedAt: time.Date(2025, 3, 8, 0, 0, 0, 0, time.UTC)},
        {ID: 7, AuthorID: 2, CreatedAt: time.Date(2025, 3, 9, 0, 0, 0, 0, time.UTC)},
        {ID: 8, AuthorID: 2, CreatedAt: time.Date(2025, 3, 10, 0, 0, 0, 0, time.UTC)},
        {ID: 9, AuthorID: 3, CreatedAt: time.Date(2025, 3, 11, 0, 0, 0, 0, time.UTC)},
        {ID: 10, AuthorID: 4, CreatedAt: time.Date(2025, 3, 12, 0, 0, 0, 0, time.UTC)},
    }
    
    // Steps 1-3 same as before...
    // Step 1: Count posts by author ID
    postCounts := make(map[int]int)
    for _, post := range posts {
        postCounts[post.AuthorID]++
    }
    
    // Step 2: Create a map to lookup author names by ID
    authorNames := make(map[int]string)
    for _, author := range authors {
        authorNames[author.ID] = author.Name
    }
    
    // Step 3: Calculate frequencies
    totalPosts := len(posts)
    frequencies := make(map[string]float64)
    
    for authorID, count := range postCounts {
        authorName := authorNames[authorID]
        percentage := float64(count) * 100 / float64(totalPosts)
        frequencies[authorName] = percentage
    }
    
    // Step 4: Sort by frequency
    var sortedFreqs []AuthorFrequency
    for name, freq := range frequencies {
        sortedFreqs = append(sortedFreqs, AuthorFrequency{name, freq})
    }
    
    // Sort using lambda function
    sort.Slice(sortedFreqs, func(i, j int) bool {
        return sortedFreqs[i].Frequency > sortedFreqs[j].Frequency
    })
    
    // Print all frequencies (sorted)
    fmt.Println("All authors by post frequency (sorted):")
    for _, af := range sortedFreqs {
        fmt.Printf("%s %.1f\n", af.Name, af.Frequency)
    }
    
    // Step 5: Get top 3
    topCount := 3
    if len(sortedFreqs) < 3 {
        topCount = len(sortedFreqs)
    }
    
    fmt.Println("\nTop 3 authors by post frequency:")
    for i := 0; i < topCount; i++ {
        fmt.Printf("%s %.1f\n", sortedFreqs[i].Name, sortedFreqs[i].Frequency)
    }
}
```

### Teaching Notes
1. Explain why we need an intermediate structure (AuthorFrequency)
2. Demonstrate how to convert a map to a sortable slice
3. Show how to use a lambda function for custom sorting
4. Explain how to limit results to the top N

### Exercise
Have students:
1. Modify the code to handle ties (e.g., if multiple authors have the same frequency)
2. Implement secondary sorting (by name when frequencies are equal)
3. Make the number of top results configurable

## Step 10: Final Solution - Encapsulating in a Function

### Lesson Goals
- Encapsulate the solution in a reusable function
- Understand function design and interfaces
- Complete the final solution to match requirements

### Code Example
```go
package main

import (
    "fmt"
    "sort"
    "time"
)

type Author struct {
    ID   int
    Name string
}

type Post struct {
    ID        int
    AuthorID  int
    CreatedAt time.Time
}

// calculateFrequency calculates the top N distribution frequency of posts by authors
func calculateFrequency(authors []Author, posts []Post, topN int) map[string]float64 {
    // Create a map of author ID to name for efficient lookups
    authorMap := make(map[int]string)
    for _, author := range authors {
        authorMap[author.ID] = author.Name
    }

    // Count posts by author ID
    postCounts := make(map[int]int)
    for _, post := range posts {
        postCounts[post.AuthorID]++
    }

    // Calculate frequencies
    totalPosts := len(posts)
    frequencies := make(map[string]float64)
    for authorID, count := range postCounts {
        if name, exists := authorMap[authorID]; exists {
            frequencies[name] = float64(count) * 100.0 / float64(totalPosts)
        }
    }

    // Sort authors by frequency (descending)
    type authorFreq struct {
        name      string
        frequency float64
    }
    
    // Use functional-style with anonymous function (lambda)
    sortedFreqs := make([]authorFreq, 0, len(frequencies))
    for name, freq := range frequencies {
        sortedFreqs = append(sortedFreqs, authorFreq{name, freq})
    }
    
    sort.Slice(sortedFreqs, func(i, j int) bool {
        // Primary sort by frequency (descending)
        if sortedFreqs[i].frequency != sortedFreqs[j].frequency {
            return sortedFreqs[i].frequency > sortedFreqs[j].frequency
        }
        // Secondary sort by name (ascending) when frequencies are equal
        return sortedFreqs[i].name < sortedFreqs[j].name
    })

    // Return top N frequencies (or all if less than N)
    result := make(map[string]float64)
    limit := topN
    if len(sortedFreqs) < topN {
        limit = len(sortedFreqs)
    }
    
    for i := 0; i < limit; i++ {
        result[sortedFreqs[i].name] = sortedFreqs[i].frequency
    }
    
    return result
}

func main() {
    // Create full set of sample data
    authors := []Author{
        {ID: 1, Name: "Marcus"},
        {ID: 2, Name: "Peter"},
        {ID: 3, Name: "Renan"},
        {ID: 4, Name: "Kyle"},
        {ID: 5, Name: "Caique"},
    }

    posts := []Post{
        {ID: 1, AuthorID: 5, CreatedAt: time.Date(2025, 3, 3, 0, 0, 0, 0, time.UTC)},
        {ID: 2, AuthorID: 5, CreatedAt: time.Date(2025, 3, 4, 0, 0, 0, 0, time.UTC)},
        {ID: 3, AuthorID: 5, CreatedAt: time.Date(2025, 3, 5, 0, 0, 0, 0, time.UTC)},
        {ID: 4, AuthorID: 5, CreatedAt: time.Date(2025, 3, 6, 0, 0, 0, 0, time.UTC)},
        {ID: 5, AuthorID: 1, CreatedAt: time.Date(2025, 3, 7, 0, 0, 0, 0, time.UTC)},
        {ID: 6, AuthorID: 1, CreatedAt: time.Date(2025, 3, 8, 0, 0, 0, 0, time.UTC)},
        {ID: 7, AuthorID: 2, CreatedAt: time.Date(2025, 3, 9, 0, 0, 0, 0, time.UTC)},
        {ID: 8, AuthorID: 2, CreatedAt: time.Date(2025, 3, 10, 0, 0, 0, 0, time.UTC)},
        {ID: 9, AuthorID: 3, CreatedAt: time.Date(2025, 3, 11, 0, 0, 0, 0, time.UTC)},
        {ID: 10, AuthorID: 4, CreatedAt: time.Date(2025, 3, 12, 0, 0, 0, 0, time.UTC)},
    }

    // Calculate and print the top 3 frequencies
    result := calculateFrequency(authors, posts, 3)
    
    // Print the results in a deterministic order
    // (since map iteration is non-deterministic)
    var names []string
    for name := range result {
        names = append(names, name)
    }
    sort.Slice(names, func(i, j int) bool {
        return result[names[i]] > result[names[j]]
    })
    
    for _, name := range names {
        fmt.Printf("%s %.1f\n", name, result[name])
    }
}
```

### Teaching Notes
1. Explain the benefits of encapsulating logic in a function
2. Show how to make the function reusable (parameterizing topN)
3. Discuss secondary sorting for ties
4. Demonstrate how to handle the non-deterministic map iteration for output
5. Explain the efficiency of this approach

### Exercise
Have students:
1. Extend the function to return additional statistics (e.g., average posts per author)
2. Implement error handling for edge cases
3. Create a version that also returns the post counts alongside frequencies

## Final Challenge

Ask students to modify the solution to handle one of these extensions:

1. Add date range filtering (only count posts within a certain date range)
2. Support grouping by month/year to see posting frequency over time
3. Implement a function that finds authors with no posts
4. Create a function that identifies the most active day of the week for posting

## Teaching Tips

1. **Code Along**: Have students type the code themselves rather than copying and pasting. This reinforces learning and helps identify misunderstandings early.

2. **Visual Diagrams**: Use diagrams to explain relationships between data structures (e.g., how Author IDs connect to Posts).

3. **Incremental Compilation**: After each new concept, compile and run the code to show immediate results.

4. **Error Analysis**: When errors occur, walk through the debugging process to teach troubleshooting skills.

5. **Connect to Real-World**: Relate each concept to real-world applications beyond the current exercise.

6. **Pair Programming**: For exercises, have students work in pairs with one typing and one reviewing/suggesting.

7. **Concept Check**: Before moving to the next day, ask targeted questions to verify understanding.

8. **Code Review**: Regularly review student code to identify common misconceptions.

## Conclusion and Next Steps

By the end of this workshop, students should understand:

1. **Basic Go Syntax**: Variables, data types, functions, and control structures
2. **Data Structures**: Arrays, slices, maps, and custom structs
3. **Algorithms**: Sorting, filtering, and calculating frequencies
4. **Problem-Solving Approach**: Breaking down complex problems into manageable steps

Possible follow-up projects could include:
- Adding a simple API to expose the frequency calculation function
- Implementing persistence with a database
- Building a command-line interface for the application
- Extending the functionality to track changes over time

Remember that beginners may need extra time to absorb these concepts. Be prepared to revisit earlier topics and provide additional examples as needed.

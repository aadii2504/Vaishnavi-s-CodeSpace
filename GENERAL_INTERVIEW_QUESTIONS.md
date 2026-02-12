# 🎯 General .NET & SQL Interview Questions (Essential)

A curated list of questions commonly asked in MNC interviews for freshers and junior developers.

---

## 1. C# & .NET Core Concepts

### Q1: What is the difference between `Value Type` and `Reference Type`?

- **Value Type**: Holds the data directly (e.g., `int`, `bool`, `struct`). Stored in the **Stack**.
- **Reference Type**: Holds a pointer to the data (e.g., `string`, `class`, `array`). Stored in the **Heap**.
- _Example:_ If you change a value type in a function, the original doesn't change. If you change a reference type, the original object is updated.

### Q2: What are the principles of OOP (Object-Oriented Programming)?

Remember **A-P-I-E**:

1.  **Abstraction**: Hiding complex implementation details (e.g., interfaces).
2.  **Polymorphism**: One method behaves differently based on the object (Method Overloading/Overriding).
3.  **Inheritance**: Acquiring properties from a parent class.
4.  **Encapsulation**: Wrapping data and methods into a single unit (class) and protecting it (private/public).

### Q3: Explain `async` and `await`. Why use them?

They are used for **Asynchronous Programming** to keep the application responsive.

- **async**: Marks a method as asynchronous.
- **await**: Pauses the execution of the method until the awaited task completes, without blocking the main thread (e.g., waiting for a database query or API call).

### Q4: What is Garbage Collection (GC) in .NET?

GC is an automatic memory management feature. It frees up memory occupied by objects that are no longer in use. It has three generations:

- **Gen 0**: Short-lived objects (e.g., temporary variables).
- **Gen 1**: Buffer between Gen 0 and Gen 2.
- **Gen 2**: Long-lived objects (e.g., static data).

### Q5: What is the difference between `Interface` and `Abstract Class`?

- **Interface**: Only defines method signatures (contracts). A class can implement **multiple** interfaces. Used for de-coupling code (DI).
- **Abstract Class**: Can have implementation details and abstract methods. A class can inherit only **one** abstract class. Used for sharing common logic.

### Q6: What is LINQ?

Language Integrated Query. It allows you to write SQL-like queries directly in C# code to manipulate collections, arrays, and databases.

- _Key Feature:_ **Deferred Execution** (The query is not executed until you iterate over the result, e.g., using `.ToList()`).

### Q7: Delegates vs Events?

- **Delegate**: A type-safe function pointer. It holds a reference to a method.
- **Event**: A wrapper around a delegate. It allows a class to notify other classes when something happens (Publisher-Subscriber model).

---

## 2. ASP.NET Core Web API

### Q8: What are the HTTP Verbs?

- **GET**: Retrieve data.
- **POST**: Create new data.
- **PUT**: Update existing data (replaces entire resource).
- **PATCH**: Partially update data.
- **DELETE**: Remove data.

### Q9: Common HTTP Status Codes?

- **200 OK**: Success.
- **201 Created**: Resource created successfully.
- **400 Bad Request**: Invalid input from client.
- **401 Unauthorized**: User not logged in (no token).
- **403 Forbidden**: Logged in, but no permission (e.g., Officer accessing Manager API).
- **404 Not Found**: Resource doesn't exist.
- **500 Internal Server Error**: Server crashed/Exception.

### Q10: What is Dependency Injection (DI) Lifetimes?

1.  **Transient**: Created **every time** it is requested. Lightweight services.
2.  **Scoped**: Created **once per HTTP request**. Used for DbContext (User A's request gets one instance, User B gets another).
3.  **Singleton**: Created **once for the application lifetime**. Shared by all requests.

### Q11: Middleware vs Filters?

- **Middleware**: Components that handle the request logic **before** it reaches the Controller (e.g., Authentication, Logging). It runs for _every_ request.
- **Filters**: Run **after** the request reaches the Controller but before the Action Method (e.g., Validation, caching). Specific to Controllers/Actions.

---

## 3. SQL Server (Database)

### Q12: What is Normalization?

The process of organizing data to reduce redundancy (duplication).

- **1NF**: Atomic values (no multiple values in one cell).
- **2NF**: No partial dependency (all columns depend on Primary Key).
- **3NF**: No transitive dependency (non-key columns don't depend on other non-key columns).

### Q13: What are Joins?

- **INNER JOIN**: Returns matching rows from both tables.
- **LEFT JOIN**: Returns all rows from Left table + matching from Right (NULL if no match).
- **RIGHT JOIN**: Returns all rows from Right table + matching from Left.
- **FULL JOIN**: Returns everything (matches + non-matches from both sides).

### Q14: Diff between `Stored Procedure` and `View`?

- **Stored Procedure**: Can have logic (IF/ELSE, LOOPS), accept parameters, and modify data (INSERT/UPDATE). Pre-compiled for performance.
- **View**: A "Virtual Table" saved as a query. Read-only by default. Used to simplify complex SELECT queries for reporting.

### Q15: What is an Index?

A structure that speeds up data retrieval (like a book index).

- **Clustered Index**: Sorts the actual data rows (Only 1 per table, usually PK).
- **Non-Clustered Index**: A separate structure pointing to data rows (Can have multiple).

### Q16: DELETE vs TRUNCATE?

- **DELETE**: Removes rows one by one. Can be rolled back. Slower. Triggers are fired.
- **TRUNCATE**: Removes all pages (faster). Cannot be rolled back (in some contexts). Resets Identity counter. No triggers fired.

### Q17: What are ACID Properties?

Transactions must follow ACID to ensure data integrity:

1.  **Atomicity**: All or Nothing (if one step fails, everything rolls back).
2.  **Consistency**: Database moves from one valid state to another.
3.  **Isolation**: Transactions don't interfere with each other.
4.  **Durability**: Once saved, data is permanent even if power fails.

---

## 4. DevOps & Git

### Q18: Merge vs Rebase?

- **Merge**: Combines branches and creates a "Merge Commit". Preserves history exactly as it happened. Safer.
- **Rebase**: Moves your branch changes to the tip of the master branch. Creates a linear history (cleaner look) but re-writes history. Dangerous on shared branches.

---

## 5. Logical & HR Questions

### Q19: Explain SOLID Principles (Briefly).

- **S**ingle Responsibility: A class should do one thing.
- **O**pen/Closed: Open for extension, closed for modification.
- **L**iskov Substitution: Derived classes must be substitutable for base classes.
- **I**nterface Segregation: Detailed interfaces are better than one big general interface.
- **D**ependency Inversion: Depend on abstractions, not concretions (Use Interfaces!).

### Q20: How do you handle Exceptions?

"I use `try-catch` blocks. In Web API, I prefer a **Global Exception Handler Middleware** to catch all errors in one place and return a standardized 500 Error response, logging the details to a file or database."

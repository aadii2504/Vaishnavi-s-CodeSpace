# 🏦 Corporate Account Management (CAM) System - 45+ Deep Dive Interview Questions

This file contains **Project-Specific** questions that an interviewer will ask to verify if you truly built this project.

---

## 🔹 Section 1: Architecture & High-Level Design (5 Questions)

1.  **Can you explain the High-Level Architecture of your application?**
    - **Answer:** "I followed a Layered Architecture. We have the **Presentation Layer** (React Frontend), **API Layer** (.NET 8 Web API), and **Data Access Layer** (EF Core & SQL Server). The API exposes RESTful endpoints, handles Business Logic (Validation, Auth), and communicates with the Database using a mix of EF Core for simple CRUD and Stored Procedures for complex transactions."

2.  **Why did you choose .NET 8 over other frameworks?**
    - **Answer:** "Performance and Type Safety. .NET 8 is the latest LTS (Long Term Support) version with significant performance improvements in JSON serialization and EF Core. Also, the built-in Dependency Injection and Middleware pipeline make it perfect for enterprise banking apps."

3.  **Is your application Monolithic or Microservices based?**
    - **Answer:** "It is a **Modular Monolith**. We have separate controllers for Accounts, Transactions, and Approvals, but they run in a single process. For this scale, a Microservice architecture would introduce unnecessary complexity (network latency, distributed transactions)."

4.  **How does your Frontend communicate with the Backend?**
    - **Answer:** "Via **REST APIs** using JSON. The React app produces HTTP requests (GET, POST) using `fetch` or `axios`. The backend consumes the JSON body, processes it, and returns HTTP Status Codes (200 OK, 400 Bad Request)."

5.  **Where do you host this application? (Deployment)**
    - **Answer:** "Currently, it's hosted on **IIS (Internet Information Services)** on a Windows Server for the backend, and the Database is on a dedicated SQL Server instance. If we moved to the cloud, I would use **Azure App Service**."

---

## 🔹 Section 2: Database & Entity Framework Core (10 Questions)

6.  **Did you use Code-First or Database-First approach? Why?**
    - **Answer:** "I used **Code-First**. I defined my C# Models (`User`, `Account`, `Transaction`) first, then used EF Core Migrations (`add-migration`, `update-database`) to generate the SQL Schema. This keeps my database version-controlled along with the code."

7.  **Why do you have `t_` prefix in your table names (e.g., `t_Transaction`)?**
    - **Answer:** "It is a naming convention to distinguish **Transactional Tables** from Master/Reference tables or System tables. It helps in quickly identifying the purpose of the table in large databases."

8.  **How do you handle Database Migrations?**
    - **Answer:** "I use the Package Manager Console in Visual Studio.
      `Add-Migration InitialCreate` -> Creates a script.
      `Update-Database` -> Applies it to the SQL Server."

9.  **You used `Stored Procedures` for Transactions. Why not EF Core `SaveChanges()`?**
    - **Answer:** "**Performance and Concurrency**. A banking transaction (Withdraw -> Deposit) involves multiple steps (Check Balance, Lock Row, Update Sender, Update Receiver). Doing this in memory with EF Core risks 'Race Conditions'. A Stored Procedure runs efficiently inside the DB engine in a single transaction."

10. **What happens if the Database goes down during a transaction?**
    - **Answer:** "Since we use **ACID Transactions** (Begin Try/Catch/Commit/Rollback) inside the Stored Procedure, the entire operation is rolled back. No money is deducted if the credit step fails."

11. **Explain the relationship between `User` and `Transaction` tables.**
    - **Answer:** "It is a **One-to-Many** relationship. One User (Officer) can initiate _Many_ Transactions. But one specific Transaction is created by _One_ User. The `Transaction` table has a Foreign Key `CreatedBy` pointing to `User.UserID`."

12. **How do you optimize a slow API endpoint in your project?**
    - **Answer:** "First, I check the SQL Query using Profiler. If it's doing a full table scan, I add an **Index** on the filter column (e.g., `AccountID`). I also use `AsNoTracking()` in EF Core for read-only lists."

13. **What is `DbContext` in your project?**
    - **Answer:** "`ApplicationDbContext`. It inherits from `DbContext` and acts as a bridge between my code and SQL Server. It contains `DbSet<User>`, `DbSet<Transaction>`, etc."

14. **How do you handle Connection Strings securely?**
    - **Answer:** "It is stored in `appsettings.json`. In production, we would use **Environment Variables** or **Azure Key Vault** to prevent checking secrets into Source Control."

15. **What is the difference between `IEnumerable` and `IQueryable` in your Repository?**
    - **Answer:** "I use `IQueryable` when building dynamic queries (e.g., adding filters for Date Range) so the filtering happens in SQL. `IEnumerable` pulls all data into memory first, which is bad for performance."

---

## 🔹 Section 3: Authentication & Security (8 Questions)

16. **How does Login work in your app?**
    - **Answer:** "The user sends `Email` + `Password`. I verify the user in the DB. If valid, I generate a **JWT (JSON Web Token)** signed with a Secret Key. This token is sent back to the client."

17. **Where do you store the Password?**
    - **Answer:** "**Never in plain text!** I use `BCrypt` hashing. When a user registers, I hash the password " `BCrypt.HashPassword("12345")`. When they login, I verify " `BCrypt.Verify("12345", HashedValue)`."

18. **What is inside your JWT Token?**
    - **Answer:** "It contains **Claims**: `UserID`, `Email`, `Role` (Officer/Manager), and `ExpirationTime`. The frontend sends this token in the `Authorization: Bearer <token>` header for every request."

19. **How do you implement Role-Based Access Control (RBAC)?**
    - **Answer:** "I use the `[Authorize(Roles = "Manager")]` attribute on my Controllers.
    * `TransactionController`: Accessible by 'Officer'.
    * `ApprovalController`: Accessible ONLY by 'Manager'."

20. **What is SQL Injection and how did you prevent it?**
    - **Answer:** "It's when malicious SQL is entered into input fields. I prevented it by using **Parameterized Queries** in Raw SQL (`FromSqlRaw`) and Stored Procedures. EF Core does this automatically for LINQ queries."

21. **What is Cross-Origin Resource Sharing (CORS)?**
    - **Answer:** "Browsers block requests from different domains (React on localhost:3000 -> API on localhost:5000). I enabled CORS in `Program.cs` via `builder.Services.AddCors(...)` to allow my frontend URL."

22. **How do you handle sensitive data (PII) like User Email?**
    - **Answer:** "We use HTTPS (TLS) to encrypt data in transit. In the database, critical columns like Passwords are hashed."

23. **What happens if the Token expires?**
    - **Answer:** "The API returns `401 Unauthorized`. The Frontend intercepts this and redirects the user to the Login Page."

---

## 🔹 Section 4: Business Logic & Scenarios (10 Questions)

24. **Where did you write the validation logic (e.g., Negative Amount)?**
    - **Answer:** "I use **Data Annotations** (`[Required]`, `[Range(1, 100000)]`) on the DTO/Model. If validation fails, the Controller automatically returns `400 Bad Request`."

25. **Explain the 'Approval Workflow'.**
    - **Answer:** "When a transaction > $50,000 is created, its Status is set to 'Pending'. It doesn't impact the balance yet. A Manager views the list, reviews documents, and clicks 'Approve'. Only _then_ does the Stored Procedure deduct the balance."

26. **What is the hardest bug you fixed in this project?**
    - **Answer:** "Handling concurrent withdrawals. Two officers tried to withdraw from the same account at the exact same millisecond. I fixed it by using **Database Locks** inside the Stored Procedure to ensure one finishes before the other starts."

27. **How do you handle Global Exceptions?**
    - **Answer:** "I implemented a Middleware that wraps every request in a `try-catch`. If an unexpected error occurs, it lgos the error and returns a generic `500 Server Error` JSON response so the API doesn't crash."

28. **How would you bulk upload 1,000 transactions?**
    - **Answer:** "I wouldn't use a `for` loop calling the Database 1,000 times! I would use **SqlBulkCopy** or perform a Batch Insert using EF Core 8's new `BulkInsert` feature for performance."

29. **What design patterns did you use?**
    - **Answer:**
      1.  **Repository Pattern**: To abstract data access (if applicable).
      2.  **DTO (Data Transfer Object)**: To separate Database Entities from API responses.
      3.  **Dependency Injection**: Injecting DbContext into Controllers.

30. **Why use DTOs instead of calling Entity Models directly?**
    - **Answer:** "Security and coupling. I don't want to expose my `PasswordHash` or internal database columns to the frontend. DTOs allow me to send only what the UI needs (`Name`, `Email`)."

31. **How do you test your API?**
    - **Answer:** "I use **Swagger (OpenAPI)** for manual testing during development and **Postman** for integration testing (creating collections of requests)."

32. **What is `appsettings.Development.json`?**
    - **Answer:** "It contains configuration valid only for the local dev environment (e.g., Local DB Connection String, Detailed Logging). It overrides `appsettings.json` when running locally."

33. **Explain how `Program.cs` works in .NET 8.**
    - **Answer:** "It's the entry point.
    1.  We create a `WebApplicationBuilder`.
    2.  We register services (DI container) like DbContext, Auth.
    3.  We `Build()` the app.
    4.  We configure the Middleware Pipeline (Auth, CORS, Controllers).
    5.  We `Run()` the app."

---

## 🔹 Section 5: Hypothetical / "What If" Questions (5 Questions)

34. **If your application gets 1 million users tomorrow, what will break?**
    - **Answer:** "The Database will be the bottleneck. I would need to implement **Caching (Redis)** for frequent reads (like User Profiles) and potentially **Read Replicas** for the Report generation."

35. **How would you implement 'Forgot Password'?**
    - **Answer:** "Generate a unique random Token, save it in the DB with an expiration (15 mins), and email a link to the user. When they click it, verify the token and allow them to `UPDATE` the password hash."

36. **Your API is returning 500 Error but no logs. Determine the issue.**
    - **Answer:** "I would checking the **Event Viewer** on the server or enable **Stdout Logging** in `.NET Core` to see the startup errors. It's often a missing configuration or connection string issue."

37. **A Manager approved a transaction but the Balance didn't update. Why?**
    - **Answer:** "I would check the Stored Procedure Logic. Maybe the `UPDATE` statement is inside a `TRANSACTION` that was accidentally `ROLLED BACK` or the `WHERE` clause (AccountID) didn't match any row."

38. **How do you handle 'Soft Delete'?**
    - **Answer:** "I added `IsActive` (bool) column. When deleting, I just set `IsActive = false`. In all my `SELECT` queries, I add `WHERE IsActive = true`. This preserves history."

---

## 🔹 Section 6: Specific Code Questions (Be ready to write this!)

39. **Write the LINQ query to get Top 5 transactions by Amount.**

    ```csharp
    var top5 = _context.Transactions
                       .OrderByDescending(t => t.Amount)
                       .Take(5)
                       .ToList();
    ```

40. **How do you inject the DbContext?**

    ```csharp
    // in Program.cs
    builder.Services.AddDbContext<AppDbContext>(options =>
        options.UseSqlServer(connectionString));

    // in Controller Constructor
    public UserController(AppDbContext context) { ... }
    ```

41. **Write a standard GET API method.**

    ```csharp
    [HttpGet("{id}")]
    public IActionResult GetUser(int id) {
        var user = _context.Users.Find(id);
        if (user == null) return NotFound();
        return Ok(user);
    }
    ```

42. How to join two tables in LINQ?

    ```csharp
    var data = from t in _context.Transactions
               join u in _context.Users on t.UserId equals u.Id
               select new { t.Amount, u.Name };
    ```

43. **How do you return a specific HTTP Status Code?**
    - `return Ok(data);` (200)
    - `return CreatedAtAction(..., data);` (201)
    - `return BadRequest("Error");` (400)
    - `return NotFound();` (404)
    - `return StatusCode(500, "Server Error");`

44. **What is `Task<IActionResult>`?**
    - **Answer:** "It represents an **Asynchronous** result. It frees up the thread while waiting for the Database, allowing the server to handle more requests simultaneously."

45. **Explain `[FromBody]` vs `[FromQuery]`.**
    - `[FromBody]`: Reads data from the JSON Body (POST/PUT).
    - `[FromQuery]`: Reads variables from the URL `?id=5` (GET/DELETE).

# 🎓 60 Essential .NET Interview Questions (Beginner to Intermediate)

A comprehensive list of questions covering C#, OOP, ASP.NET Core, and EF Core to help you crack technical interviews.

---

## 🔹 Section 1: C# Basics & Data Types

1.  **What is C#?**
    A modern, object-oriented, type-safe programming language developed by Microsoft.

2.  **What is the entry point of a C# console application?**
    The `static void Main(string[] args)` method.

3.  **Difference between `public`, `private`, `protected`, and `internal`?**
    - `public`: Accessible everywhere.
    - `private`: Accessible only within the class.
    - `protected`: Accessible within the class and derived classes.
    - `internal`: Accessible only within the same assembly (project).

4.  **What is the difference between `System.String` and `System.Text.StringBuilder`?**
    `String` is immutable (creates a new object on modification). `StringBuilder` is mutable (modifies the existing object), making it better for frequent string updates.

5.  **What is a Nullable Type?**
    A value type that can hold a null value, e.g., `int? age = null;`.

6.  **Explain the `var` keyword.**
    It allows implicit typing. The compiler infers the type from the value (e.g., `var x = 10;` compiles as `int x = 10;`).

7.  **What is the difference between `const` and `readonly`?**
    - `const`: Value is set at compile-time and cannot change.
    - `readonly`: Value can be set at runtime (in the constructor).

8.  **What is boxing and unboxing?**
    - **Boxing**: Converting a Value Type to a Reference Type (`object`).
    - **Unboxing**: Converting a Reference Type back to a Value Type.

9.  **What is an Enum?**
    A value type representing a set of named constants (e.g., `enum Days { Sun, Mon }`).

10. **What is the use of `using` statement?**
    It ensures that `IDisposable` objects (like FileStream, DbConnection) are disposed of automatically to release resources.

---

## 🔹 Section 2: Object-Oriented Programming (OOP)

11. **What are the 4 pillars of OOP?**
    Encapsulation, Abstraction, Inheritance, Polymorphism.

12. **What is Encapsulation?**
    Bundling data and methods into a class and restricting access using storage modifiers.

13. **What is Polymorphism?**
    "One name, many forms." It allows a method to behave differently based on the object (Overloading and Overriding).

14. **What is Inheritance?**
    Deriving a new class from an existing class to reuse code.

15. **What is Abstraction?**
    Hiding implementation details and showing only functionality (using Abstract Classes or Interfaces).

16. **Difference between Method Overloading and Overriding?**
    - **Overloading**: Same method name, different parameters (Compile-time polymorphism).
    - **Overriding**: Same method signature in derived class (Runtime polymorphism).

17. **Can a class inherit multiple classes in C#?**
    No, C# supports multiple inheritance only through Interfaces.

18. **What is a Sealed class?**
    A class that cannot be inherited (`sealed class MyClass`).

19. **What is a Partial Class?**
    A class definition split across multiple files, combined at compile time.

20. **What is a Constructor?**
    A special method invoked when an object is created. It has the same name as the class.

---

## 🔹 Section 3: Advanced C# Concepts

21. **What is a Delegate?**
    A type-safe function pointer.

22. **What is a Multicast Delegate?**
    A delegate that points to multiple methods.

23. **What is an Event?**
    A wrapper around a delegate used for the publish-subscribe model.

24. **What is an Anonymous Method?**
    A method without a name, defined using the `delegate` keyword.

25. **What is a Lambda Expression?**
    A concise way to write anonymous functions (`x => x * x`).

26. **What is LINQ?**
    Language Integrated Query, used to query collections like SQL.

27. **Difference between `IEnumerable` and `IQueryable`?**
    - `IEnumerable`: In-memory iteration (LINQ to Objects).
    - `IQueryable`: Queries run on the database server (LINQ to SQL/EF).

28. **What is Exception Handling?**
    Managing runtime errors using `try`, `catch`, `finally`, and `throw`.

29. **What is the purpose of `finally` block?**
    It executes code regardless of whether an exception occurred, typically for cleanup.

30. **What is Reference Type vs Value Type?**
    Value types hold data (Stack). Reference types hold pointers to data (Heap).

---

## 🔹 Section 4: Asynchronous Programming

31. **What are `async` and `await`?**
    Keywords for writing non-blocking asynchronous code.

32. **What is `Task` in C#?**
    Represents an asynchronous operation.

33. **Difference between `Task.Run` and `Thread.Start`?**
    `Task.Run` uses the Thread Pool (more efficient). `Thread.Start` creates a dedicated thread (expensive).

34. **Can you have `async void` methods?**
    Yes, but avoid them (except for Event Handlers). Use `async Task` instead.

35. **What is a Deadlock?**
    When two threads wait for each other to release resources, freezing the app.

---

## 🔹 Section 5: .NET Ecosystem

36. **Difference between .NET Framework, .NET Core, and .NET Standard?**
    - **Framework**: Windows-only (Legacy).
    - **Core**: Cross-platform, high-performance (Modern).
    - **Standard**: Specification for shared libraries across all .NET implementations.

37. **What is CLR?**
    Common Language Runtime. It manages execution (memory, thread, exceptions).

38. **What is JIT Compiler?**
    Just-In-Time Compiler. Converts IL (Intermediate Language) to Machine Code at runtime.

39. **What is Garbage Collection (GC)?**
    Automatic memory management that frees unused objects.

40. **What is Managed Code?**
    Code executed by the CLR. (Unmanaged code runs directly on the OS).

---

## 🔹 Section 6: ASP.NET Core & Web API

41. **What is Dependency Injection (DI)?**
    Design pattern to achieve Inversion of Control (IoC). Classes receive dependencies via constructor rather than creating them.

42. **What are the DI Service Lifetimes?**
    Transient, Scoped, Singleton.

43. **What is Middleware?**
    Software components assembled into an application pipeline to handle requests/responses.

44. **What is `Startup.cs` / `Program.cs`?**
    The entry point where services are configured and the request pipeline is built.

45. **What is Routing?**
    Mapping URL endpoints to Controller Actions.

46. **Difference between MVC and Web API?**
    - **MVC**: Returns Views (HTML) for UI.
    - **Web API**: Returns Data (JSON/XML) for clients.

47. **What is Model Binding?**
    Mapping HTTP request data (QueryString, Body) to Controller action parameters.

48. **What are Action Filters?**
    Attributes that run logic before or after an action method (e.g., `[Authorize]`).

49. **What is Kestrel?**
    The cross-platform web server included with ASP.NET Core.

50. **How do you handle configuration?**
    Using `appsettings.json` and `IConfiguration`.

---

## 🔹 Section 7: Entity Framework Core (EF Core)

51. **What is EF Core?**
    An ORM (Object-Relational Mapper) for .NET.

52. **Code-First vs Database-First?**
    - **Code-First**: Write C# classes, generate DB.
    - **Database-First**: Existing DB, generate C# classes.

53. **What is `DbContext`?**
    Represents a session with the database and manages Entity objects.

54. **What are Migrations?**
    A way to update the database schema to match your code model (`add-migration`, `update-database`).

55. **What is Lazy Loading vs Eager Loading?**
    - **Lazy**: Loads data only when accessed (can cause N+1 problem).
    - **Eager**: Loads related data upfront using `.Include()`.

56. **What is `DbSet`?**
    Represents a table in the database.

57. **How to prevent SQL Injection in EF Core?**
    EF uses parameterized queries by default, protecting against basic injection.

58. **Difference between `First` and `FirstOrDefault`?**
    - `First`: Throws exception if no match found.
    - `FirstOrDefault`: Returns null/default if no match found.

59. **What is Shadow Property?**
    A property defined in the model but not in the C# class (e.g., `CreatedDate` audit field).

60. **What is No-Tracking queries?**
    `.AsNoTracking()` improves performance for read-only queries by skipping change tracking.

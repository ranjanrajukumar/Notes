# .NET Interview Preparation — 3 Days Notes

## Day 1 — C#, OOP, SOLID, DI, LINQ, Async, JWT

### 1. OOP
Four pillars:
- Encapsulation — protect internal data and control access.
- Abstraction — hide implementation details.
- Inheritance — reuse and extend a base class.
- Polymorphism — same contract, different behavior.

**Interview answer:**  
> OOP is a programming approach based on objects containing data and behavior. Its four main pillars are Encapsulation, Abstraction, Inheritance, and Polymorphism.

### 2. Encapsulation
Bundle data and methods together and restrict direct access.

```csharp
public class BankAccount
{
    private decimal _balance;

    public void Deposit(decimal amount)
    {
        _balance += amount;
    }
}
```

### 3. Abstraction
Expose required functionality and hide implementation details.

### 4. Inheritance
A derived class reuses or extends a base class.

### 5. Polymorphism
- Overloading = compile-time polymorphism.
- Overriding = runtime polymorphism.

### 6. Abstract Class vs Interface
- Abstract class can contain abstract and concrete members.
- A class can inherit only one base class.
- Interface defines a contract and a class can implement multiple interfaces.
- Modern C# interfaces can have default implementations.

### 7. Overloading vs Overriding
**Overloading:** same method name, different parameters.

**Overriding:** derived class provides implementation for a virtual/abstract member.

### 8. Access Modifiers

| Modifier | Meaning |
|---|---|
| public | Anywhere |
| private | Same class |
| protected | Class + derived classes |
| internal | Same assembly |

---

## SOLID

### S — Single Responsibility
One responsibility or one reason to change.

### O — Open/Closed
Open for extension, closed for modification.

### L — Liskov Substitution
Derived types should be usable where the base type is expected without breaking behavior.

### I — Interface Segregation
Do not force a class to depend on methods it does not use.

### D — Dependency Inversion
High-level modules depend on abstractions instead of concrete low-level implementations.

**Important:** SOLID is a set of principles, not design patterns. DI is a technique commonly used to implement DIP.

---

## Dependency Injection

DI provides dependencies from outside instead of creating them inside a class.

```csharp
public class EmployeeService
{
    private readonly IEmployeeRepository _repository;

    public EmployeeService(IEmployeeRepository repository)
    {
        _repository = repository;
    }
}
```

### Service Lifetimes
- Transient — new instance each resolution.
- Scoped — one instance per HTTP request.
- Singleton — shared application lifetime.

**Shortcut:** Transient = New, Scoped = Request, Singleton = Application.

---

## IEnumerable vs IQueryable

- IEnumerable — commonly used for in-memory collections.
- IQueryable — commonly used with query providers such as EF Core and can be translated/executed at the data source.

---

## First / FirstOrDefault / Single / SingleOrDefault

| Method | No record | Multiple |
|---|---|---|
| First | Exception | First |
| FirstOrDefault | Default | First |
| Single | Exception | Exception |
| SingleOrDefault | Default | Exception |

**Shortcut:** First = first available; Single = exactly one.

---

## async / await

Used mainly for asynchronous I/O such as database, HTTP, and file operations.

```csharp
public async Task<List<Employee>> GetEmployeesAsync()
{
    return await _context.Employees.ToListAsync();
}
```

**Important:** async/await does not automatically mean parallel execution.

---

## JWT

Flow:

```text
Login
→ Validate credentials
→ Generate JWT
→ Client sends Authorization: Bearer <JWT>
→ API validates token
→ Protected endpoint
```

JWT commonly contains Header, Payload/Claims, and Signature.

**Important:** JWT is not encryption by itself.

---

## Authentication vs Authorization

- Authentication = Who are you?
- Authorization = What are you allowed to do?

### 401 vs 403
- 401 = authentication problem.
- 403 = authenticated but not authorized.

---

# Day 2 — Advanced C#, Collections, LINQ, Async

## 1. Value Type vs Reference Type

**Value types:** int, double, bool, struct, enum.

**Reference types:** class, array, string, delegate, object.

---

## 2. Boxing / Unboxing

```csharp
int number = 10;
object obj = number;     // Boxing
int result = (int)obj;   // Unboxing
```

---

## 3. var / dynamic / object

### var
Compile-time type inference.

### dynamic
Binding/checking happens at runtime.

### object
Base type; specific use may require casting/pattern matching.

---

## 4. ref / out / in

- ref — initialized before call; method can read/write.
- out — does not need initialization; method must assign.
- in — passed by reference for read-only access.

**Shortcut:** ref = read/write, out = output, in = read-only.

---

## 5. Delegates

A delegate is a type-safe reference to a method.

Used for:
- Callbacks
- Events
- Passing methods
- Lambda expressions

---

## 6. Generics

Generics provide reusable, type-safe code.

```csharp
public class Repository<T>
{
    public void Add(T entity)
    {
    }
}
```

Benefits:
- Type safety
- Reusability
- Less casting

---

## 7. Collections

### Array
Fixed-size, strongly typed.

### ArrayList
Non-generic legacy collection.

### List<T>
Generic, strongly typed, dynamically resizable.

### Dictionary<TKey,TValue>
Key-value lookup.

### HashSet<T>
Unique values.

**Shortcut:**
- List = collection
- Dictionary = key/value
- HashSet = unique

---

## 8. Where vs Select

```csharp
Where  → Filter
Select → Transform/Project
```

Example:

```csharp
var adults = employees.Where(x => x.Age >= 18);
var names = employees.Select(x => x.Name);
```

---

## 9. Deferred Execution

Many LINQ queries execute when enumerated.

```csharp
var query = employees.Where(x => x.Age > 30);

var result = query.ToList();
```

---

## 10. Thread vs Task vs Task.Run

- Thread = execution thread.
- Task = represents an asynchronous operation.
- Task.Run = schedules work on the thread pool and is generally useful for suitable CPU-bound work.

Do not use Task.Run unnecessarily for normal ASP.NET Core database I/O.

---

## 11. CancellationToken

Used to request cancellation of an asynchronous operation.

```csharp
public async Task GetDataAsync(CancellationToken cancellationToken)
{
    await service.GetDataAsync(cancellationToken);
}
```

---

## 12. const / readonly / static readonly

- const = compile-time constant.
- readonly = assign at declaration or constructor.
- static readonly = shared readonly field for the type.

---

## 13. string vs StringBuilder

`string` is immutable.

`StringBuilder` is useful for repeated modifications, especially inside loops.

```csharp
var builder = new StringBuilder();
builder.Append("Hello");
builder.Append(" World");
```

---

## 14. == vs Equals()

Do not say `==` always means reference comparison or `.Equals()` always means value comparison. Behavior depends on the type and its equality implementation.

---

# Day 3 — ASP.NET Core Web API

## 1. ASP.NET Core Architecture

Typical flow:

```text
Client
→ Kestrel
→ Middleware
→ Routing
→ Authentication
→ Authorization
→ Controller
→ Service
→ Repository
→ Database
```

---

## 2. Middleware

Middleware is a component in the ASP.NET Core HTTP request/response pipeline.

Examples:
- Exception handling
- Logging
- Authentication
- Authorization
- CORS

**Shortcut:** Middleware = Request/Response Pipeline.

---

## 3. Use / Run / Map

- Use = can call next middleware.
- Run = terminal middleware.
- Map = creates a branch.

**Shortcut:** Use = Continue, Run = Stop, Map = Branch.

---

## 4. Controllers and Routing

```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    [HttpGet("{id}")]
    public IActionResult Get(int id)
    {
        return Ok();
    }
}
```

---

## 5. IActionResult vs ActionResult<T>

**IActionResult:** represents an HTTP action result.

**ActionResult<T>:** represents an HTTP result and expresses the expected response data type.

```csharp
public ActionResult<Product> Get(int id)
{
    if (product == null)
        return NotFound();

    return Ok(product);
}
```

---

## 6. Model Binding

Model binding takes data from:
- Route
- Query string
- Request body
- Form data

and binds it to action parameters/models.

**Important:** Model Binding is different from Model Validation.

---

## 7. Model Validation

Validation checks whether the model follows defined rules.

```csharp
public class EmployeeRequest
{
    [Required]
    public string Name { get; set; }

    [Range(18, 60)]
    public int Age { get; set; }

    [EmailAddress]
    public string Email { get; set; }
}
```

With `[ApiController]`, invalid model state can automatically produce a 400 response.

---

## 8. HTTP Status Codes

| Code | Meaning |
|---|---|
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Not authenticated |
| 403 | Authenticated but not authorized |
| 404 | Not Found |
| 500 | Unexpected server error |

---

## 9. Role-Based vs Claims-Based Authorization

**Role-based:**
- Admin
- Manager
- Teacher

**Claims-based:**
- Department = HR
- Permission = Employee.Delete

Claims/policies are useful for detailed authorization rules.

---

## 10. Global Exception Handling

Centralize exception handling using middleware.

```text
Request
→ Exception Middleware
→ Controller/Service
→ Exception
→ Middleware catches/logs
→ Consistent error response
```

Benefits:
- Centralized handling
- Consistent response
- Less duplicate try/catch
- Easier maintenance

---

## 11. Repository Pattern

Repository separates database/data-access logic from business logic.

```text
Controller
→ Service
→ Repository
→ Database
```

**Interview answer:**

> Repository Pattern is used to separate data-access logic from business logic. It provides an abstraction over database operations and can improve maintainability and testability.

---

## 12. Factory Pattern

Factory is a creational design pattern used to create/select objects.

```csharp
IPayment payment = PaymentFactory.Create("UPI");
```

**Shortcut:** Factory = Create Object.

---

## 13. Factory vs Dependency Injection

**Factory:** decides which object to create.

**DI:** provides the dependency from outside.

**Shortcut:**

```text
Factory = Create
DI      = Provide
```

---

## 14. Singleton vs Factory vs Repository

| Pattern | Purpose |
|---|---|
| Singleton | Shared instance/lifetime |
| Factory | Object creation |
| Repository | Data access |

---

## 15. Unit of Work

Unit of Work coordinates multiple related database operations in one transaction.

```text
Begin Transaction
→ Add Employee
→ Add Address
→ Update Department
→ Commit
```

If an operation fails:

```text
Rollback
```

### Repository vs Unit of Work

> Repository handles individual data-access operations, while Unit of Work coordinates multiple related operations and manages transaction boundaries.

**Important:** Email is an external system and should generally be sent after the database transaction successfully commits, rather than being treated as part of the same DB transaction.

---

## 16. Pagination

Pagination returns data in smaller pages.

```csharp
var products = await _context.Products
    .OrderBy(x => x.Id)
    .Skip((pageNumber - 1) * pageSize)
    .Take(pageSize)
    .ToListAsync();
```

Benefits:
- Smaller response
- Lower memory usage
- Better performance
- Better UI experience

---

## 17. API Security

Important areas:
- HTTPS
- Authentication
- Authorization
- JWT validation
- Input validation
- SQL injection prevention
- Secure secret management
- CORS
- Rate limiting where appropriate
- Logging/monitoring

---

## 18. CORS

CORS = Cross-Origin Resource Sharing.

It controls which browser origins are allowed to make cross-origin requests to an API.

```csharp
builder.Services.AddCors(options =>
{
    options.AddPolicy("Frontend", policy =>
    {
        policy.WithOrigins("https://example.com")
              .AllowAnyHeader()
              .AllowAnyMethod();
    });
});
```

**Important:** CORS is not authentication.

---

## 19. API Versioning

Versioning allows API changes without immediately breaking existing clients.

Example:

```text
/api/v1/products
/api/v2/products
```

Useful when mobile/web clients depend on an older API contract.

---

# PRODUCTION SCENARIOS

## Slow API

> First I check logs and monitoring to identify where the latency is coming from. Then I check application code, database queries and execution plans, indexes, unnecessary joins, repeated database calls, and external services. I use pagination, asynchronous I/O, and caching where appropriate. Finally, I test the fix and monitor the API again.

## 500 Error

> First I check application logs and the exception stack trace. Then I verify environment variables, connection strings, database connectivity, external APIs, and deployment configuration. After fixing the root cause, I test, redeploy if required, and verify the application.

## Expired JWT

> The API should reject the request as unauthenticated and return 401. If the application supports refresh tokens, the client can follow the refresh-token flow.

## Valid JWT but Admin Access Denied

> Authentication succeeded, but authorization failed. If the user does not have the required role or permission, the API should return 403.

---

# FINAL QUICK REVISION

```text
OOP
→ Encapsulation
→ Abstraction
→ Inheritance
→ Polymorphism

SOLID
→ SRP
→ OCP
→ LSP
→ ISP
→ DIP

DI
→ Transient
→ Scoped
→ Singleton

LINQ
→ Where = Filter
→ Select = Transform
→ First = First
→ Single = Exactly One

ASYNC
→ async/await
→ I/O
→ Not automatically parallel

WEB API
→ Middleware
→ Routing
→ Controller
→ Service
→ Repository
→ Database

AUTH
→ Authentication = Who?
→ Authorization = What?
→ 401 = Authentication problem
→ 403 = Authorization problem

PATTERNS
→ Factory = Create
→ Repository = Data
→ Unit of Work = Transaction
→ Singleton = Shared Instance

PERFORMANCE
→ Logs
→ Database
→ Execution Plan
→ Indexes
→ Pagination
→ Async
→ Cache
→ Monitor
```

# INTERVIEW SPEAKING FORMULA

For every question:

**What? → Why? → Example? → Project Usage?**

Do not memorize long paragraphs.

Understand the concept, explain it in your own words, and connect it to your project.

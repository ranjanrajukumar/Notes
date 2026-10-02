# Day 1 — .NET Interview Preparation Notes

## 1. OOP — Object-Oriented Programming

### Theory

OOP is a programming paradigm based on **classes and objects**.

The four main pillars are:

1. Encapsulation
2. Abstraction
3. Inheritance
4. Polymorphism

### Interview Answer

> OOP stands for Object-Oriented Programming. It is a programming paradigm based on classes and objects. C# supports four main pillars of OOP: Encapsulation, Abstraction, Inheritance, and Polymorphism. These concepts help us build applications that are maintainable, reusable, and easier to extend.

**Shortcut:** `OOP = E + A + I + P`

---

# 2. Encapsulation

### Theory

Encapsulation means **bundling data and methods together in a class and restricting direct access to internal data**.

C# commonly uses:

- `private`
- `public`
- `protected`
- Properties
- Methods

### Example

```csharp
public class BankAccount
{
    private decimal balance;

    public decimal Balance
    {
        get { return balance; }
    }

    public void Deposit(decimal amount)
    {
        if (amount > 0)
        {
            balance += amount;
        }
    }
}
```

### Interview Answer

> Encapsulation means bundling data and methods together in a class and restricting direct access to internal data. In C#, we implement encapsulation using access modifiers such as private, public, and protected, along with properties and methods. For example, in a bank account, the balance can be private and can only be modified through controlled methods such as Deposit or Withdraw.

**Shortcut:** `Encapsulation = Data Hiding + Controlled Access`

---

# 3. Abstraction

### Theory

Abstraction means **hiding implementation details and exposing only the required functionality**.

C# commonly uses:

- Abstract class
- Interface

### Example

```csharp
public abstract class Payment
{
    public abstract void Pay();

    public void ValidatePayment()
    {
        Console.WriteLine("Payment validated");
    }
}
```

### Interview Answer

> Abstraction means hiding implementation details and exposing only the required functionality. In C#, abstraction can be achieved using abstract classes and interfaces. For example, in a payment system, we can expose a Pay method while hiding the internal implementation details of how the payment is processed.

**Shortcut:** `Abstraction = Hide Implementation + Show Required Functionality`

---

# 4. Inheritance

### Theory

Inheritance allows one class to **derive from another class and reuse or extend its functionality**.

### Example

```csharp
public class Employee
{
    public string Name { get; set; }

    public void Login()
    {
    }
}

public class Manager : Employee
{
    public void ApproveLeave()
    {
    }
}
```

### Interview Answer

> Inheritance allows one class to derive from another class and reuse or extend its functionality. For example, if Employee is a base class and Manager derives from Employee, Manager can reuse the common Employee functionality and add its own functionality such as approving leave.

**Shortcut:** `Inheritance = Reuse + Extend`

---

# 5. Polymorphism

### Theory

Polymorphism means **one interface or method can have different behavior**.

## Compile-Time Polymorphism — Overloading

```csharp
public void Calculate(int a, int b)
{
}

public void Calculate(int a, int b, int c)
{
}
```

Same method name, different parameters.

## Runtime Polymorphism — Overriding

```csharp
public class Animal
{
    public virtual void Sound()
    {
        Console.WriteLine("Animal Sound");
    }
}

public class Dog : Animal
{
    public override void Sound()
    {
        Console.WriteLine("Bark");
    }
}
```

### Interview Answer

> Polymorphism means one interface or method can have different implementations. Compile-time polymorphism is achieved using method overloading, where methods have the same name but different parameters. Runtime polymorphism is achieved using method overriding, where a derived class provides its own implementation of a virtual or abstract method from the base class.

**Shortcut:**

- `Overloading → Compile Time`
- `Overriding → Runtime`

---

# 6. Abstract Class vs Interface

| Abstract Class | Interface |
|---|---|
| Can contain abstract and concrete members | Defines a contract |
| A class can inherit one base class | A class can implement multiple interfaces |
| Can contain common state/implementation | Used for common contracts/capabilities |
| Useful for related classes | Commonly used with DI |
| Single class inheritance | Multiple interfaces |

### Example

```csharp
public abstract class Payment
{
    public void Validate()
    {
    }

    public abstract void Pay();
}
```

```csharp
public interface INotification
{
    void Send(string message);
}
```

### Interview Answer

> An abstract class can contain both abstract and concrete members and is useful when related classes share common behavior or state. A class can inherit only one base class. An interface defines a contract that implementing classes must follow, and a class can implement multiple interfaces. Interfaces are commonly used with Dependency Injection to achieve loose coupling.

**Shortcut:**

- `Abstract Class → Common behavior/state`
- `Interface → Contract + Loose Coupling`

---

# 7. Access Modifiers

### Public

Anywhere se access kar sakte hain.

### Private

Sirf same class ke andar access.

### Protected

Same class + derived class.

### Internal

Same assembly/project ke andar.

### Interview Answer

> Public members can be accessed from anywhere. Private members can be accessed only within the same class. Protected members can be accessed within the same class and derived classes. Internal members can be accessed within the same assembly.

### Shortcut

```text
Public    → Everywhere
Private   → Same Class
Protected → Parent + Child
Internal  → Same Assembly
```

---

# 8. Method Overloading vs Overriding

## Overloading

Same method name + different parameters.

```csharp
Calculate(int a, int b)
Calculate(int a, int b, int c)
```

**Compile-time polymorphism**

## Overriding

Parent class ke `virtual`/`abstract` method ko child class `override` karti hai.

```csharp
public virtual void Pay()
{
}

public override void Pay()
{
}
```

**Runtime polymorphism**

### Interview Answer

> Method overloading means having multiple methods with the same name but different parameters. It is compile-time polymorphism. Method overriding means redefining a virtual or abstract method of the base class in a derived class using the override keyword. It is runtime polymorphism.

---

# 9. Dependency Injection

### Theory

Dependency Injection ka meaning hai:

**Class ki dependency ko class ke andar create karne ke bajay bahar se provide karna.**

### Without DI

```csharp
public class EmployeeService
{
    private EmployeeRepository repository =
        new EmployeeRepository();
}
```

Yahan tight coupling hai.

### With DI

```csharp
public class EmployeeService
{
    private readonly IEmployeeRepository repository;

    public EmployeeService(
        IEmployeeRepository repository)
    {
        this.repository = repository;
    }
}
```

Registration:

```csharp
builder.Services
    .AddScoped<IEmployeeRepository, EmployeeRepository>();
```

### Interview Answer

> Dependency Injection is a technique used to provide a class with its dependencies from outside instead of creating them inside the class. It helps achieve loose coupling, maintainability, and testability. ASP.NET Core provides a built-in Dependency Injection container where we can register and resolve dependencies.

**Important:** DI aur DIP same cheez nahi hain. DI ek technique hai jo DIP ko implement karne mein commonly help karti hai.

---

# 10. DI Service Lifetimes

## Transient

Har baar service resolve hone par new instance.

```csharp
AddTransient<IEmailService, EmailService>();
```

## Scoped

Ek HTTP request ke andar same instance.

```csharp
AddScoped<IEmployeeRepository, EmployeeRepository>();
```

## Singleton

Application lifetime mein generally same instance.

```csharp
AddSingleton<ICacheService, CacheService>();
```

### Interview Answer

> Transient creates a new instance each time the service is requested. Scoped creates one instance per HTTP request. Singleton creates one shared instance for the application lifetime.

### Shortcut

```text
Transient → New every resolve
Scoped    → Same per request
Singleton → Same application lifetime
```

---

# 11. SOLID Principles

SOLID **five design principles** ka group hai.

Purpose:

- Maintainability
- Testability
- Flexibility
- Easy extension
- Loose coupling

## S — Single Responsibility Principle

> A class should have one responsibility and one reason to change.

### Example

Bad:

```text
EmployeeService
 ├── Employee CRUD
 ├── Send Email
 ├── Generate PDF
 └── Audit Logging
```

Good:

```text
EmployeeService
EmailService
PdfService
AuditService
```

### Interview Answer

> The Single Responsibility Principle states that a class should have one responsibility and one reason to change. For example, if an EmployeeService handles employee CRUD, email sending, PDF generation, and audit logging, it has multiple responsibilities. I would separate them into EmployeeService, EmailService, PdfService, and AuditService.

**Shortcut:** `S = One Responsibility`

---

# 12. Open/Closed Principle

> **Open for extension, closed for modification.**

### Example

```csharp
public interface IPayment
{
    void Pay();
}
```

Implementations:

```text
UpiPayment
CardPayment
NetBankingPayment
```

New payment method add karte waqt existing implementations ko modify karne ki zarurat nahi.

### Interview Answer

> The Open/Closed Principle states that software entities should be open for extension but closed for modification. For example, if my payment system supports UPI and Card payments and I need to add Net Banking, I can create a new NetBankingPayment implementation without modifying the existing payment implementations.

**Shortcut:** `O = Extend, Don't Modify`

---

# 13. Liskov Substitution Principle

### Theory

> Derived class should be able to replace the base class without breaking the expected behavior of the application.

### Example

```csharp
public class Animal
{
    public virtual void Eat()
    {
    }
}

public class Dog : Animal
{
    public override void Eat()
    {
    }
}
```

### Interview Answer

> The Liskov Substitution Principle states that a derived class should be substitutable for its base class without breaking the expected behavior of the application. In other words, derived classes should properly honor the contract of their base class.

**Shortcut:** `L = Child can replace Parent`

---

# 14. Interface Segregation Principle

### Theory

> A class should not be forced to depend on methods that it does not use.

### Bad

```csharp
public interface IEmployee
{
    void AddEmployee();
    void SendEmail();
    void GenerateSalarySlip();
}
```

### Better

```text
IEmployeeRepository
IEmailService
ISalaryService
```

### Interview Answer

> The Interface Segregation Principle states that a class should not be forced to depend on methods or interfaces that it does not use. We should prefer smaller and focused interfaces instead of one large interface.

**Shortcut:** `I = Don't force unnecessary methods`

---

# 15. Dependency Inversion Principle

### Theory

> High-level modules should not directly depend on low-level modules. Both should depend on abstractions.

### Example

```text
EmployeeService
       ↓
IEmployeeRepository
       ↑
EmployeeRepository
```

### Interview Answer

> The Dependency Inversion Principle states that high-level modules should not depend directly on low-level modules. Both should depend on abstractions. In ASP.NET Core, we commonly achieve this using interfaces and Dependency Injection. For example, EmployeeService can depend on IEmployeeRepository instead of directly depending on EmployeeRepository.

**Shortcut:** `D = Depend on Abstraction`

---

# 16. IEnumerable vs IQueryable

## IEnumerable

Generally in-memory collections ke saath use hota hai.

```csharp
IEnumerable<Employee> employees = employeeList;

var result = employees
    .Where(x => x.Salary > 50000);
```

## IQueryable

Database/query provider ke saath commonly use hota hai.

```csharp
IQueryable<Employee> employees =
    dbContext.Employees;

var result = employees
    .Where(x => x.Salary > 50000);
```

### Interview Answer

> IEnumerable is mainly used for in-memory data processing, such as List and other collections. IQueryable is commonly used with query providers such as Entity Framework, where the query can be translated and executed by the underlying data source. So IEnumerable generally processes data in memory, while IQueryable allows the query provider to execute the query at the data source.

**Shortcut:**

- `IEnumerable → In Memory`
- `IQueryable → Query Provider/Database`

---

# 17. First / FirstOrDefault / Single / SingleOrDefault

| Method | 0 Records | 1 Record | Multiple |
|---|---|---|---|
| `First()` | Exception | Record | First |
| `FirstOrDefault()` | Default | Record | First |
| `Single()` | Exception | Record | Exception |
| `SingleOrDefault()` | Default | Record | Exception |

### Shortcut

- **First → First record**
- **Single → Exactly one record**
- **OrDefault → No record ho to default value**

---

# 18. async / await

### Theory

`async` aur `await` asynchronous programming ke liye use hote hain.

- `async` → method ko asynchronous operation ke liye mark karta hai.
- `await` → Task ko asynchronously wait karta hai without blocking the current thread.

### Example

```csharp
public async Task<Employee> GetEmployeeAsync(int id)
{
    var employee =
        await repository.GetEmployeeAsync(id);

    return employee;
}
```

### ASP.NET Core mein use

Especially I/O-bound operations:

- Database
- External API
- File operations

### Interview Answer

> The async and await keywords are used for asynchronous programming in C#. The async keyword marks a method as asynchronous, while await allows us to wait for a Task without blocking the current thread. In ASP.NET Core, asynchronous programming is commonly used for I/O-bound operations such as database and external API calls, which helps improve application scalability.

---

# 19. Slow API Troubleshooting

Agar API **5–10 seconds** le rahi hai:

```text
Logs / Monitoring
       ↓
Application Code
       ↓
Database
       ↓
SQL Query / Execution Plan
       ↓
Indexes / Joins
       ↓
External APIs
       ↓
Pagination
       ↓
Async
       ↓
Caching
```

### Interview Answer

> If my API is taking 5 to 10 seconds, first I would identify the bottleneck using logs and monitoring. I would check whether the delay is in the application code, database, or external services. Then I would analyze slow queries, execution plans, indexes, and unnecessary joins. I would also check unnecessary loops and database calls, implement pagination for large datasets, and use asynchronous programming for I/O-bound operations. If the data is frequently accessed and rarely changes, I would consider caching.

---

# 20. Production 500 Error

### Troubleshooting

1. Check production logs.
2. Check exception/stack trace.
3. Check environment variables.
4. Check connection strings.
5. Check database connectivity.
6. Check external API configuration.
7. Check deployment configuration.
8. Fix → Test → Redeploy.

### Interview Answer

> If the API works in development but returns a 500 error in production, first I would check the production logs to identify the exact exception and stack trace. Then I would verify environment variables and application configuration, especially the database connection string and external API settings. I would check database connectivity and deployment configuration. After identifying the root cause, I would fix it, test the change, and redeploy.

---

# 21. JWT Authentication

### Flow

```text
Login
 ↓
Username/Password Validation
 ↓
Generate JWT
 ↓
Client receives Token
 ↓
Authorization: Bearer <JWT>
 ↓
ASP.NET Core validates Token
 ↓
Access Protected API
```

### Interview Answer

> JWT stands for JSON Web Token. It is commonly used for stateless authentication in Web APIs. When the user logs in with valid credentials, the server generates a JWT containing relevant claims. The client sends this token with subsequent API requests using the Authorization Bearer header. ASP.NET Core validates the token before allowing access to protected endpoints.

**Shortcut:** `Login → Validate → JWT → Bearer → Validate → API`

---

# 22. Authentication vs Authorization

### Authentication

**Who are you?**

User ki identity verify karna.

### Authorization

**What can you do?**

User ke roles/permissions check karna.

### Interview Answer

> Authentication is the process of verifying the identity of a user, while authorization determines what an authenticated user is allowed to do. For example, an Admin and a Teacher may both authenticate successfully, but their roles and permissions can determine which APIs or resources they are allowed to access.

**Shortcut:**

- `Authentication → Who are you?`
- `Authorization → What can you do?`

---

# 23. 401 vs 403

### 401 Unauthorized

Authentication problem.

Examples:

- JWT missing
- Invalid token
- Expired token

### 403 Forbidden

Authentication successful, but permission nahi hai.

### Interview Answer

> 401 Unauthorized means the request does not have valid authentication credentials, such as a missing, invalid, or expired JWT token. 403 Forbidden means the user is authenticated but does not have permission to access the requested resource.

**Shortcut:**

- `401 → Authentication problem`
- `403 → Authorization problem`

---

# Day 1 Final Revision

```text
OOP = Classes + Objects

Encapsulation = Data Hiding + Controlled Access

Abstraction = Hide Implementation + Show Required Functionality

Inheritance = Reuse + Extend

Polymorphism = One Interface/Method + Different Behavior

Overloading = Compile Time

Overriding = Runtime

Interface = Contract

Abstract Class = Common Behavior/State

DI = Dependencies provided from outside

Transient = New instance every resolve

Scoped = One instance per HTTP request

Singleton = One instance for application lifetime

SOLID = Maintainable + Testable + Flexible Design

IEnumerable = In-Memory Processing

IQueryable = Query Provider/Database

async/await = Non-blocking I/O

JWT = Stateless Token-Based Authentication

Authentication = Who are you?

Authorization = What can you do?

401 = Authentication problem

403 = Authorization problem
```

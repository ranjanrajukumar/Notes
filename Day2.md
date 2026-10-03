# Day 2 – .NET Interview Preparation Notes

## Focus
Advanced C# + Collections + Generics + LINQ + Async Programming

Learning method: **Understand → Example → Speak → Repeat**

---

## 1. Value Type vs Reference Type

**Value Type:** contains the actual value. Examples: `int`, `float`, `double`, `bool`, `struct`, `enum`.

```csharp
int a = 10;
int b = a;
b = 20;
// a = 10, b = 20
```

**Reference Type:** contains a reference to an object. Examples: `class`, `object`, arrays, delegates, `string`.

```csharp
class Employee
{
    public string Name { get; set; }
}

Employee e1 = new Employee();
e1.Name = "Raju";
Employee e2 = e1;
e2.Name = "Amit";
// e1.Name is also "Amit"
```

**Interview answer:**  
> “The main difference between value types and reference types is how their data is stored and copied. Value types contain the actual value, while reference types contain a reference to an object. When we copy a value type, a separate copy of the value is created. When we copy a reference type, both variables can refer to the same object.”

**Shortcut:** Value → copy value | Reference → copy reference.

> Interview note: Avoid saying all value types are always on the stack and all reference types are always on the heap; storage depends on context.

---

## 2. Boxing and Unboxing

**Boxing:** value type → `object`.

```csharp
int number = 10;
object obj = number;
```

**Unboxing:** explicitly convert a boxed object back to the original value type.

```csharp
int result = (int)obj;
```

**Shortcut:** `int → object` = Boxing | `object → int` = Unboxing.

**Interview answer:**  
> “Boxing is the process of converting a value type to an object or reference type. Unboxing is explicitly converting the boxed object back to the original value type.”

---

## 3. `var` vs `dynamic` vs `object`

| Keyword | Type decision | Important point |
|---|---|---|
| `var` | Compile time | Compiler infers the type |
| `dynamic` | Runtime | Type checking happens at runtime |
| `object` | Base type | Can hold any type; casting is usually needed |

```csharp
var name = "Raju";

dynamic value = "Raju";
value = 100;

object data = 100;
int number = (int)data;
```

**Interview answer:**  
> “`var` uses compile-time type inference. `dynamic` is resolved at runtime. `object` is the base type of all C# types and can store any value, but casting is generally required to access the specific type.”

**Shortcut:** var → compile time | dynamic → runtime | object → base type + casting.

---

## 4. `ref`, `out`, and `in`

| Keyword | Caller initializes? | Method must assign? | Method can modify? |
|---|---:|---:|---:|
| `ref` | Yes | No | Yes |
| `out` | No | Yes | Yes |
| `in` | Yes | No | No |

### ref
```csharp
void Update(ref int x)
{
    x = 20;
}

int number = 10;
Update(ref number);
```

### out
```csharp
void GetValue(out int x)
{
    x = 100;
}

int number;
GetValue(out number);
```

### in
```csharp
void Print(in int number)
{
    Console.WriteLine(number);
}

int value = 10;
Print(in value);
```

**Interview answer:**  
> “A `ref` parameter must be initialized before passing it, and the method can modify it. An `out` parameter doesn't need to be initialized before passing it, but the method must assign a value. An `in` parameter must be initialized and is passed as read-only.”

**Shortcut:** ref → Read + Write | out → Write | in → Read-only.

---

## 5. Delegate

A delegate is a **type-safe reference to a method**.

```csharp
public delegate void Notify(string message);

public static void SendEmail(string message)
{
    Console.WriteLine("Email: " + message);
}

Notify notification = SendEmail;
notification("Order created");
```

Delegates are commonly used for callbacks and event handling.

**Delegate vs Event:**  
Delegate → method reference.  
Event → controlled notification/subscription mechanism based on delegates.

**Interview answer:**  
> “A delegate is a type-safe reference to a method. It allows us to pass methods as parameters and invoke them later. Delegates are commonly used for callbacks and event handling.”

**Shortcut:** Delegate = Method reference | Event = Notification mechanism.

---

## 6. Access Modifiers

| Modifier | Accessibility |
|---|---|
| `private` | Same class |
| `protected` | Same class + derived class |
| `internal` | Same assembly |
| `public` | Anywhere where the containing type is accessible |

**Shortcut:** Private → Same class | Protected → Class + Derived | Internal → Same assembly | Public → Anywhere.

**Interview answer:**  
> “`private` members can be accessed only within the same class. `protected` members can be accessed within the same class and derived classes. `internal` members can be accessed within the same assembly. `public` members can be accessed from any assembly or class where the containing type is accessible.”

---

## 7. Generics

Generics allow reusable code to work with different data types while maintaining type safety.

```csharp
public void Print<T>(T value)
{
    Console.WriteLine(value);
}

Print<int>(100);
Print<string>("Raju");
Print<double>(10.5);
```

Generic collections:

```csharp
List<int> numbers = new List<int>();
List<string> names = new List<string>();
```

**Benefits:** reusable code, type safety, less casting.

**Interview answer:**  
> “Generics allow us to create classes, methods, or collections that work with different data types while maintaining type safety. They improve code reusability and reduce unnecessary casting.”

**Shortcut:** Generics = Reusable code + Type safety + Less casting.

---

## 8. Array vs ArrayList vs List<T>

| Feature | Array | ArrayList | List<T> |
|---|---|---|---|
| Size | Fixed | Dynamic | Dynamic |
| Type safety | Yes | No | Yes |
| Generic | No | No | Yes |
| Modern choice | Fixed-size data | Legacy | Common choice |

```csharp
int[] numbers = new int[3];

ArrayList data = new ArrayList();
data.Add(10);
data.Add("Raju");

List<int> values = new List<int>();
values.Add(10);
values.Add(20);
```

**Interview answer:**  
> “An array has a fixed size and provides type safety. `ArrayList` is non-generic and can store different types, but may involve boxing and unboxing for value types. `List<T>` is generic, type-safe, and dynamically sized. In modern C# applications, I generally prefer `List<T>`.”

---

## 9. Dictionary<TKey, TValue>

A Dictionary stores **key-value pairs**.

```csharp
Dictionary<int, string> employees = new Dictionary<int, string>();

employees.Add(101, "Raju");
employees.Add(102, "Amit");

string name = employees[101];
```

Keys must be unique.

**Interview answer:**  
> “A Dictionary is a generic collection that stores data in key-value pairs. Each key must be unique, and it is useful when I need to retrieve a value using a specific key, such as Employee ID to Employee Name.”

**Shortcut:** Dictionary → Key + Value.

---

## 10. List vs Dictionary vs HashSet

| Collection | Purpose | Duplicates |
|---|---|---|
| `List<T>` | Ordered collection | Allowed |
| `HashSet<T>` | Unique values | Not allowed |
| `Dictionary<TKey,TValue>` | Key-value lookup | Keys unique |

Examples:
```csharp
List<string> names = new List<string>();
HashSet<string> emails = new HashSet<string>();
Dictionary<int, string> employees = new Dictionary<int, string>();
```

**Shortcut:** List → Collection | HashSet → Unique values | Dictionary → Key + Value.

---

## 11. IEnumerable vs IQueryable

**IEnumerable**
- Generally used for in-memory collections.
- LINQ operations are performed on data already loaded into memory.
- Examples: `List<T>`, arrays.

**IQueryable**
- Commonly used with providers such as Entity Framework/EF Core.
- The provider can translate the query and execute it at the data source/database.

```csharp
IEnumerable<Employee> employees = db.Employees.ToList();

var result = employees.Where(x => x.Salary > 50000);
```

Here the data has already been loaded.

```csharp
IQueryable<Employee> employees = db.Employees;

var result = employees
    .Where(x => x.Salary > 50000)
    .ToList();
```

**Interview answer:**  
> “`IEnumerable` is generally used for in-memory collections, so LINQ operations are performed on data already loaded into memory. `IQueryable` is commonly used with data sources such as Entity Framework, where the query can be translated and executed at the database level.”

**Shortcut:** IEnumerable → In memory | IQueryable → Query provider/database.

---

## 12. First / FirstOrDefault / Single / SingleOrDefault

| Method | 0 records | 1 record | Multiple |
|---|---|---|---|
| `First()` | Exception | Record | First |
| `FirstOrDefault()` | Default/null | Record | First |
| `Single()` | Exception | Record | Exception |
| `SingleOrDefault()` | Default/null | Record | Exception |

**Interview answer:**  
> “`First()` returns the first matching record but throws if none exists. `FirstOrDefault()` returns the first matching record or the default value. `Single()` expects exactly one matching record and throws if there are zero or multiple records. `SingleOrDefault()` allows zero or one record but throws if multiple records are found.”

**Shortcut:** First = first one | Single = exactly one | OrDefault = no record is okay.

---

## 13. async and await

- `async` indicates that a method can perform asynchronous operations.
- `await` asynchronously waits for a Task to complete without blocking the current thread.
- Common for database calls, HTTP requests, file operations, and other I/O-bound work.

```csharp
public async Task<Employee> GetEmployeeAsync(int id)
{
    var employee = await _context.Employees
        .FirstOrDefaultAsync(x => x.Id == id);

    return employee;
}
```

**Interview answer:**  
> “`async` and `await` are C# keywords used for asynchronous programming. `async` indicates that a method can perform asynchronous operations, while `await` asynchronously waits for a Task to complete without blocking the current thread. In Web APIs, we commonly use them for database calls, HTTP requests, and file operations.”

**Important:** async does not automatically mean parallel execution.

---

## 14. Thread vs Task vs Task.Run()

**Thread:** actual execution thread; lower-level control.

```csharp
Thread thread = new Thread(() =>
{
    Console.WriteLine("Running...");
});

thread.Start();
```

**Task:** represents an asynchronous operation.

```csharp
public async Task GetDataAsync()
{
    var data = await GetDataFromDatabaseAsync();
}
```

**Task.Run():** schedules work on a ThreadPool thread; mainly useful for CPU-bound work.

```csharp
var result = await Task.Run(() =>
{
    return CalculateSomething();
});
```

**Interview answer:**  
> “A Thread represents an actual execution thread and provides low-level control. A Task represents an asynchronous operation and is commonly used for asynchronous programming. `Task.Run()` schedules work on a ThreadPool thread and is mainly useful for CPU-bound work.”

**Shortcut:** Thread → actual thread | Task → async operation | Task.Run → ThreadPool work.

---

## 15. CancellationToken

A `CancellationToken` supports cooperative cancellation of asynchronous operations.

```csharp
[HttpGet]
public async Task<IActionResult> GetData(
    CancellationToken cancellationToken)
{
    var data = await _context.Employees
        .ToListAsync(cancellationToken);

    return Ok(data);
}
```

**Interview answer:**  
> “CancellationToken is used to support cooperative cancellation of asynchronous operations. In ASP.NET Core, it can help stop unnecessary work when a client cancels or disconnects from a request. For example, I can pass it to an Entity Framework Core database query.”

**Important:** It is a cancellation request, not forceful termination.

**Shortcut:** CancellationToken = Stop unnecessary async work.

---

## 16. const vs readonly vs static readonly

| | `const` | `readonly` | `static readonly` |
|---|---|---|---|
| Meaning | Compile-time constant | Instance field | Type-level field |
| Assignment | Declaration | Declaration/constructor | Declaration/static constructor |
| Can change after initialization? | No | No | No |

```csharp
public class Employee
{
    public const string Company = "ABC";
    public readonly int EmployeeId;
    public static readonly string Country = "India";

    public Employee(int id)
    {
        EmployeeId = id;
    }
}
```

**Interview answer:**  
> “`const` is a compile-time constant and its value cannot be changed. A `readonly` field can be assigned at declaration or in the instance constructor, and after initialization it cannot be changed. `static readonly` is similar to readonly, but belongs to the type rather than an object and can be initialized in the declaration or static constructor.”

**Shortcut:** const → compile-time | readonly → one value per object | static readonly → one shared value.

---

## 17. String vs StringBuilder

`string` is **immutable**.

```csharp
string name = "Raju";
name = name + " Kumar";
```

A new string is created for the modified value.

`StringBuilder` is **mutable** and useful for repeated modifications.

```csharp
StringBuilder sb = new StringBuilder();

sb.Append("Raju");
sb.Append(" Kumar");
sb.Append(" Kushwaha");

string result = sb.ToString();
```

**Interview answer:**  
> “String is immutable, which means its value cannot be changed after it is created. StringBuilder is mutable, so we can modify its content without creating a new string for every change. I use StringBuilder when I need multiple string concatenations or modifications, especially inside loops.”

**Shortcut:** String → Immutable | StringBuilder → Mutable.

---

## 18. `==` vs `.Equals()`

`==` is an **equality operator**.  
`.Equals()` is a **method**.

For strings, both normally compare the string contents.

```csharp
string a = "Raju";
string b = "Raju";

Console.WriteLine(a == b);        // True
Console.WriteLine(a.Equals(b));   // True
```

**Interview answer:**  
> “`==` is an equality operator, while `.Equals()` is a method used to compare objects or values. For strings, both normally compare the string contents. The exact behavior can depend on the type because `==` and `Equals()` can be overloaded or overridden.”

**Shortcut:** `==` → operator | `.Equals()` → method.

---

# 19. Deferred Execution in LINQ

Deferred execution means a LINQ query is not executed when it is created. It executes when the result is enumerated or a terminal/immediate operation is used.

```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5 };

var query = numbers.Where(x => x > 2);

// Not executed yet

foreach (var number in query)
{
    Console.WriteLine(number);
}
```

Common operations that trigger execution include:
- `ToList()`
- `ToArray()`
- `Count()`
- `First()`
- `Single()`
- `foreach` enumeration

**Interview answer:**  
> “Deferred execution means a LINQ query is not executed when the query is defined. It is executed when we enumerate the result, such as using `foreach`, `ToList()`, or another terminal operation.”

**Shortcut:** Query created ≠ Query executed.

---

# Day 2 Quick Revision

## C# Fundamentals
- Value Type → copies actual value
- Reference Type → copies reference
- Boxing → value type → object
- Unboxing → object → original value type
- `var` → compile-time inference
- `dynamic` → runtime binding
- `object` → base type + casting
- `ref` → initialized, read/write
- `out` → method must assign
- `in` → read-only
- Delegate → method reference

## Collections
- Array → fixed size
- ArrayList → non-generic/legacy
- List<T> → generic + type-safe + dynamic
- Dictionary → key-value pairs
- HashSet → unique values

## LINQ
- IEnumerable → generally in-memory
- IQueryable → query provider/data source
- Where → filter
- Select → project/transform
- First → first; exception if none
- FirstOrDefault → first/default
- Single → exactly one
- SingleOrDefault → zero or one
- Deferred execution → executes on enumeration/terminal operation

## Async
- async → marks method for asynchronous operation
- await → asynchronously waits for Task
- Task → async operation
- Thread → execution thread
- Task.Run → ThreadPool work
- CancellationToken → cooperative cancellation

## Other
- const → compile-time constant
- readonly → initialized once per instance
- static readonly → initialized once at type level
- string → immutable
- StringBuilder → mutable
- `==` → operator
- `.Equals()` → method

---

# Day 2 Mock Interview Questions

1. What is the difference between Value Type and Reference Type?
2. What is Boxing and Unboxing?
3. What is the difference between var, dynamic, and object?
4. What is the difference between ref, out, and in?
5. What is a delegate?
6. What is the difference between a delegate and an event?
7. Explain access modifiers in C#.
8. What are Generics?
9. Difference between Array, ArrayList, and List<T>.
10. What is Dictionary<TKey,TValue>?
11. Difference between List, Dictionary, and HashSet.
12. Difference between IEnumerable and IQueryable.
13. Difference between First, FirstOrDefault, Single, and SingleOrDefault.
14. Explain async and await.
15. Difference between Thread, Task, and Task.Run().
16. What is CancellationToken?
17. Difference between const, readonly, and static readonly.
18. Difference between String and StringBuilder.
19. Difference between == and Equals().
20. What is Deferred Execution in LINQ?

---

# Day 2 Performance

**Overall: ~8/10**

### Strong areas
- OOP fundamentals
- Collections
- LINQ basics
- async/await
- Access modifiers
- Value/reference types

### Improve
- English sentence formation
- Precise terminology
- Delegate/event distinction
- Runtime vs compile-time concepts
- Avoiding overly simplified statements about memory

## Speaking Formula

For most interview questions:

**Definition → Why → Example → Project use**

Example:

> “IQueryable is an interface used for queryable data sources. It allows the query provider to translate the query and execute it at the data source. For example, I use IQueryable with Entity Framework when I want filtering to happen in the database before calling ToList().”

---

# Day 2 Final Reminder

Don't memorize every line.

Use:

> **Understand → Example → Speak → Repeat**

For interviews, aim for a natural answer of **30–60 seconds** per concept.

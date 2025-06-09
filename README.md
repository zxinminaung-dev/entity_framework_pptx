
# 📘 Entity Framework / Core

Entity Framework (EF) Core is an **ORM (Object Relational Mapping)** tool provided by Microsoft. It allows .NET developers to work with a database using .NET objects, eliminating the need for most of the data-access code.

---
## DBContext
    DbContext is a class provided by Entity Framework, which is an Object-Relational Mapping (ORM)  framework developed by Microsoft. DbContext represents a session with the underlying database and allows you to query and interact with the database using objects in your code, without having to directly deal with database-specific operations or SQL queries.
### 1. Defining Entities
    First, you define entity classes that represent tables in your database. These classes typically have properties that map to columns in the corresponding tables.
    ![de](https://github.com/user-attachments/assets/c371767b-f134-4975-8503-1a308d315dcd)

## 🔧 Configuration Approaches

### 

- **Fluent API** – Configure models using code in `OnModelCreating`.
- **Data Annotations** – Use attributes in your entity classes.

---

## 🧮 LINQ Queries in Entity Framework

### ✅ Get All Records
```csharp
List<Person> list = dbContext.Person.ToList();
```

### 🔍 Filtering (`Where`)
```csharp
var adults = dbContext.Person
    .Where(p => p.Age == 18) // can also use .Equals, .Contains, etc.
    .ToList();
```

### 🔎 First / FirstOrDefault
```csharp
var person = dbContext.Person
    .FirstOrDefault(p => p.Id == 5);
```

### 🎯 Select Specific Fields
```csharp
var names = dbContext.Person
    .Select(p => new { p.Id, p.Name })
    .ToList();
```

### 🧵 Include Navigation Properties (Eager Loading)
```csharp
var personWithRoom = dbContext.Person
    .Include(p => p.Room)
    .FirstOrDefault(p => p.Id == 1);
```

### 🔠 OrderBy / ThenBy
```csharp
var orderedPeople = dbContext.Person
    .OrderBy(p => p.Name)
    .ThenByDescending(p => p.Age)
    .ToList();
```

### 📊 GroupBy
```csharp
var groupByRoom = dbContext.Person
    .GroupBy(p => p.room_id)
    .Select(g => new
    {
        RoomId = g.Key,
        Count = g.Count()
    })
    .ToList();
```

### ❓ Any / All
```csharp
bool hasAdults = dbContext.Person.Any(p => p.Age >= 18);
bool allActive = dbContext.Person.All(p => p.IsActive);
```

### 🔢 Count / Sum / Average
```csharp
int totalPeople = dbContext.Person.Count();
double averageAge = dbContext.Person.Average(p => p.Age);
```

### 🔗 Join
```csharp
var result = from person in dbContext.People
             join room in dbContext.Rooms
             on person.room_id equals room.Id
             select new
             {
                 PersonName = person.name,
                 RoomName = room.RoomName
             };
```

---

## 📌 Entity States in EF

EF tracks changes to entities using **EntityState**.

### ➕ Added
```csharp
using (var context = new DatabaseContext())
{
    Person staff = new Person();
    staff.name = "Alex";
    staff.nrc = "12/TKT(N)123456";
    staff.room_id = 1;

    context.Person.Add(staff);
    context.Entry(staff).State = EntityState.Added;
    context.SaveChanges();
}
```

### ✏️ Modified
```csharp
using (var context = new DatabaseContext())
{
    Person staff = new Person();
    staff.id = 1;
    staff.name = "Alex";
    staff.nrc = "12/TKT(N)123456";
    staff.room_id = 1;

    context.Person.Add(staff);
    context.Entry(staff).State = EntityState.Modified;
    context.SaveChanges();
}
```

### ❌ Deleted
```csharp
using (var context = new DatabaseContext())
{
    Person staff = new Person();
    staff.id = 1;

    context.Person.Remove(staff);
    context.Entry(staff).State = EntityState.Deleted;
    context.SaveChanges();
}
```

---

# Java Records - Complete Guide

## What Are Java Records?

**Java Records** (introduced in Java 14, finalized in Java 16)

Records are a special kind of class designed to hold **immutable data**. They automatically provide:
- Constructor
- Getters (accessor methods)
- `equals()`
- `hashCode()`
- `toString()`

**Perfect for:** DTOs, Value Objects, Data Carriers

---

## Before Records: Traditional Class

```java
class Person_OldWay {
    private final String name;
    private final int age;
    private final String email;
    
    // Constructor
    public Person_OldWay(String name, int age, String email) {
        this.name = name;
        this.age = age;
        this.email = email;
    }
    
    // Getters
    public String getName() {
        return name;
    }
    
    public int getAge() {
        return age;
    }
    
    public String getEmail() {
        return email;
    }
    
    // equals()
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person_OldWay person = (Person_OldWay) o;
        return age == person.age &&
               name.equals(person.name) &&
               email.equals(person.email);
    }
    
    // hashCode()
    @Override
    public int hashCode() {
        int result = name.hashCode();
        result = 31 * result + age;
        result = 31 * result + email.hashCode();
        return result;
    }
    
    // toString()
    @Override
    public String toString() {
        return "Person{name='" + name + "', age=" + age + ", email='" + email + "'}";
    }
}
```

**That's ~50 lines of boilerplate code!** 😫

---

## With Records: Modern Way

```java
// All the above in ONE line! 🎉
record Person(String name, int age, String email) {
    // That's it! Everything is auto-generated
}
```

### What You Get Automatically:

1. **Constructor:** `Person(String name, int age, String email)`
2. **Getters:** `name()`, `age()`, `email()` [Note: no "get" prefix!]
3. **equals()** - compares all fields
4. **hashCode()** - based on all fields
5. **toString()** - `"Person[name=John, age=30, email=john@example.com]"`
6. **All fields are final** (immutable)
7. **Class is final** (cannot be extended)

---

## Basic Usage Examples

```java
class RecordBasics {
    public static void main(String[] args) {
        // Creating records
        Person john = new Person("John Doe", 30, "john@example.com");
        Person jane = new Person("Jane Smith", 28, "jane@example.com");
        
        // Accessing fields (no "get" prefix!)
        System.out.println("Name: " + john.name());      // John Doe
        System.out.println("Age: " + john.age());        // 30
        System.out.println("Email: " + john.email());    // john@example.com
        
        // toString() automatically generated
        System.out.println(john);
        // Output: Person[name=John Doe, age=30, email=john@example.com]
        
        // equals() works correctly
        Person john2 = new Person("John Doe", 30, "john@example.com");
        System.out.println(john.equals(john2));  // true
        System.out.println(john == john2);       // false (different objects)
        
        // hashCode() works correctly
        System.out.println(john.hashCode() == john2.hashCode());  // true
        
        // Can be used in collections
        Set<Person> people = new HashSet<>();
        people.add(john);
        people.add(john2);  // Won't add duplicate
        System.out.println(people.size());  // 1
        
        // Immutability - Cannot change values
        // john.name = "New Name";  // ❌ Compilation error!
    }
}
```

---

## Records with Validation (Compact Constructor)

```java
record Employee(String name, int age, double salary) {
    
    // Compact constructor - validates input
    public Employee {
        if (name == null || name.isBlank()) {
            throw new IllegalArgumentException("Name cannot be blank");
        }
        
        if (age < 18 || age > 100) {
            throw new IllegalArgumentException("Age must be between 18 and 100");
        }
        
        if (salary < 0) {
            throw new IllegalArgumentException("Salary cannot be negative");
        }
        
        // Note: No need to assign fields - done automatically!
        // this.name = name;  // ❌ Not needed!
    }
}
```

### Using Validated Records:

```java
class CompactConstructorExample {
    public static void main(String[] args) {
        // Valid employee
        Employee emp1 = new Employee("Alice", 25, 50000.0);
        System.out.println(emp1);  // Works fine
        
        // Invalid examples
        try {
            Employee emp2 = new Employee("", 25, 50000.0);  // ❌ Blank name
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
        
        try {
            Employee emp3 = new Employee("Bob", 15, 50000.0);  // ❌ Age < 18
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
        
        try {
            Employee emp4 = new Employee("Charlie", 25, -1000.0);  // ❌ Negative salary
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

---

## Records with Custom Methods

```java
record Product(String name, double price, int quantity) {
    
    // Validation
    public Product {
        if (price < 0) {
            throw new IllegalArgumentException("Price cannot be negative");
        }
        if (quantity < 0) {
            throw new IllegalArgumentException("Quantity cannot be negative");
        }
    }
    
    // Custom methods
    public double totalValue() {
        return price * quantity;
    }
    
    public boolean isInStock() {
        return quantity > 0;
    }
    
    public boolean isExpensive() {
        return price > 100.0;
    }
    
    // Can override accessor methods (but rarely needed)
    @Override
    public String name() {
        return name.toUpperCase();  // Always return uppercase
    }
    
    // Static methods allowed
    public static Product createFreeProduct(String name, int quantity) {
        return new Product(name, 0.0, quantity);
    }
}
```

### Using Custom Methods:

```java
class CustomMethodsExample {
    public static void main(String[] args) {
        Product laptop = new Product("Laptop", 1200.0, 5);
        
        System.out.println("Name: " + laptop.name());           // LAPTOP (uppercase)
        System.out.println("Total value: $" + laptop.totalValue());  // $6000.0
        System.out.println("In stock: " + laptop.isInStock());       // true
        System.out.println("Expensive: " + laptop.isExpensive());    // true
        
        // Static factory method
        Product sample = Product.createFreeProduct("Sample", 100);
        System.out.println(sample);  // Product[name=SAMPLE, price=0.0, quantity=100]
    }
}
```

---

## Records with Multiple Constructors

```java
record Book(String title, String author, int pages, double price) {
    
    // Compact constructor for validation
    public Book {
        if (title == null || title.isBlank()) {
            throw new IllegalArgumentException("Title required");
        }
        if (pages <= 0) {
            throw new IllegalArgumentException("Pages must be positive");
        }
    }
    
    // Additional constructor (must call canonical constructor)
    public Book(String title, String author, int pages) {
        this(title, author, pages, 0.0);  // Free book
    }
    
    // Another constructor
    public Book(String title, String author) {
        this(title, author, 100, 0.0);  // Default 100 pages, free
    }
}
```

### Using Multiple Constructors:

```java
class MultipleConstructorsExample {
    public static void main(String[] args) {
        // Using canonical constructor
        Book book1 = new Book("Java Basics", "John Doe", 300, 29.99);
        System.out.println(book1);
        
        // Using secondary constructor (free book)
        Book book2 = new Book("Free Tutorial", "Jane Smith", 200);
        System.out.println(book2);
        
        // Using tertiary constructor (default pages, free)
        Book book3 = new Book("Quick Guide", "Bob Johnson");
        System.out.println(book3);
    }
}
```

---

## Records with Interfaces

```java
interface Discountable {
    double getDiscountedPrice(double discountPercent);
}

interface Serializable {
    String serialize();
}

record Item(String name, double price) implements Discountable, Serializable {
    
    @Override
    public double getDiscountedPrice(double discountPercent) {
        return price * (1 - discountPercent / 100);
    }
    
    @Override
    public String serialize() {
        return name + "," + price;
    }
}
```

### Using Records with Interfaces:

```java
class InterfaceExample {
    public static void main(String[] args) {
        Item item = new Item("Laptop", 1000.0);
        
        // Using interface methods
        double discounted = item.getDiscountedPrice(10);
        System.out.println("Discounted price: $" + discounted);  // $900.0
        
        String serialized = item.serialize();
        System.out.println("Serialized: " + serialized);  // Laptop,1000.0
    }
}
```

---

## Nested Records

```java
record Address(String street, String city, String zipCode) {
    public Address {
        if (zipCode == null || !zipCode.matches("\\d{5}")) {
            throw new IllegalArgumentException("Invalid ZIP code");
        }
    }
}

record Company(String name, Address headquarters, List<Employee> employees) {
    public Company {
        employees = List.copyOf(employees);  // Defensive copy
    }
    
    public int employeeCount() {
        return employees.size();
    }
    
    public double totalSalary() {
        return employees.stream()
            .mapToDouble(Employee::salary)
            .sum();
    }
}
```

### Using Nested Records:

```java
class NestedRecordsExample {
    public static void main(String[] args) {
        Address address = new Address("123 Main St", "New York", "10001");
        
        List<Employee> employees = List.of(
            new Employee("Alice", 30, 80000),
            new Employee("Bob", 35, 90000),
            new Employee("Charlie", 28, 75000)
        );
        
        Company company = new Company("TechCorp", address, employees);
        
        System.out.println("Company: " + company.name());
        System.out.println("Location: " + company.headquarters().city());
        System.out.println("Employees: " + company.employeeCount());
        System.out.println("Total salary: $" + company.totalSalary());
    }
}
```

---

## Records with Generics

```java
record Result<T>(boolean success, T data, String errorMessage) {
    
    public static <T> Result<T> success(T data) {
        return new Result<>(true, data, null);
    }
    
    public static <T> Result<T> failure(String error) {
        return new Result<>(false, null, error);
    }
    
    public T getDataOrThrow() {
        if (!success) {
            throw new RuntimeException(errorMessage);
        }
        return data;
    }
}

record Pair<K, V>(K first, V second) {
    public Pair<V, K> swap() {
        return new Pair<>(second, first);
    }
}
```

### Using Generic Records:

```java
class GenericsExample {
    public static void main(String[] args) {
        // Result with String
        Result<String> result1 = Result.success("Operation completed");
        System.out.println(result1.data());  // Operation completed
        
        // Result with Integer
        Result<Integer> result2 = Result.failure("Number not found");
        try {
            result2.getDataOrThrow();
        } catch (RuntimeException e) {
            System.out.println("Error: " + e.getMessage());
        }
        
        // Pair
        Pair<String, Integer> pair = new Pair<>("Age", 30);
        System.out.println(pair);  // Pair[first=Age, second=30]
        
        Pair<Integer, String> swapped = pair.swap();
        System.out.println(swapped);  // Pair[first=30, second=Age]
    }
}
```

---

## Records vs Classes: When to Use What?

### USE RECORDS WHEN:

✅ You need immutable data carriers (DTOs, Value Objects)

✅ Data is the primary concern (not behavior)

✅ You want automatic equals/hashCode/toString

✅ You don't need inheritance

✅ **Examples:** API responses, configuration, database entities

### USE REGULAR CLASSES WHEN:

❌ You need mutable state

❌ You need inheritance (extends)

❌ Behavior is more important than data

❌ You need fine control over encapsulation

❌ **Examples:** Services, Controllers, Complex business logic

---

## Pattern Matching with Records (Java 16+)

```java
sealed interface Shape permits Circle, Rectangle, Triangle {}

record Circle(double radius) implements Shape {
    public double area() {
        return Math.PI * radius * radius;
    }
}

record Rectangle(double width, double height) implements Shape {
    public double area() {
        return width * height;
    }
}

record Triangle(double base, double height) implements Shape {
    public double area() {
        return 0.5 * base * height;
    }
}
```

### Using Pattern Matching:

```java
class PatternMatchingExample {
    public static String describe(Shape shape) {
        // Pattern matching with records
        return switch (shape) {
            case Circle(double r) -> "Circle with radius " + r;
            case Rectangle(double w, double h) -> "Rectangle " + w + "x" + h;
            case Triangle(double b, double h) -> "Triangle with base " + b;
        };
    }
    
    public static void main(String[] args) {
        Shape circle = new Circle(5.0);
        Shape rectangle = new Rectangle(4.0, 6.0);
        Shape triangle = new Triangle(3.0, 4.0);
        
        System.out.println(describe(circle));
        System.out.println(describe(rectangle));
        System.out.println(describe(triangle));
        
        // Extracting components
        if (circle instanceof Circle(double radius)) {
            System.out.println("Circle radius: " + radius);
        }
    }
}
```

---

## Common Pitfalls & Best Practices

### ❌ BAD: Mutable collection in record

```java
record BadOrder(String id, List<String> items) {
    // Items can be modified from outside!
}
```

### ✅ GOOD: Defensive copy

```java
record GoodOrder(String id, List<String> items) {
    public GoodOrder {
        items = List.copyOf(items);  // Make immutable
    }
}
```

### ❌ BAD: Trying to add setters

```java
record BadPerson(String name, int age) {
    // public void setAge(int age) { } // ❌ Don't do this!
    // Records should be immutable
}
```

### ✅ GOOD: Create new instance instead

```java
record GoodPerson(String name, int age) {
    public GoodPerson withAge(int newAge) {
        return new GoodPerson(name, newAge);
    }
}
```

### Using "with" Methods:

```java
public static void main(String[] args) {
    // Immutability with "with" methods
    GoodPerson person = new GoodPerson("John", 30);
    GoodPerson older = person.withAge(31);
    
    System.out.println(person);  // GoodPerson[name=John, age=30]
    System.out.println(older);   // GoodPerson[name=John, age=31]
}
```

---

## Summary

### Java Records Cheat Sheet

**Declaration:**
```java
record Person(String name, int age) {}
```

**Auto-generated:**
- Constructor: `Person(String name, int age)`
- Getters: `name()`, `age()` [NO "get" prefix]
- `equals()`, `hashCode()`, `toString()`

**Features:**
- ✅ Immutable by default (all fields final)
- ✅ Cannot extend other classes
- ✅ Can implement interfaces
- ✅ Can have custom methods
- ✅ Can have static methods/fields
- ✅ Compact constructor for validation
- ✅ Pattern matching support

**Best for:**
- DTOs (Data Transfer Objects)
- Value Objects
- API Responses/Requests
- Configuration data
- Database entities (read-only)

**Benefits:**
- 📦 Less boilerplate (1 line vs 50+ lines)
- 🔒 Immutable by default (thread-safe)
- 🎯 Clear intent (this is just data)
- 🧪 Easy to test
- 📖 More readable code

---

## Required Imports

```java
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.Period;
import java.util.*;
```

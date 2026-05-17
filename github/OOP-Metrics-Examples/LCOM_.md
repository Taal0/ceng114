# LCOM — Lack of Cohesion of Methods

## What is Cohesion?

Cohesion measures **how closely the methods of a class are related to each other**. A highly cohesive class has methods that all work together toward a single purpose, using the same set of instance variables.

Think of it like a sports team:
- **High cohesion** = every player works toward the same goal → a focused, effective team.
- **Low cohesion** = players working on completely different things → a dysfunctional team that should be split up.

**LCOM (Lack of Cohesion of Methods)** measures the *absence* of cohesion. So:
- **LCOM = 0 (low)** → Great! The class is focused and cohesive.
- **LCOM is high** → Bad! The class is doing too many unrelated things and should probably be split.

---

## LCOM1 — The Original (Chidamber & Kemerer, 1994)

### How to Calculate

1. List all **method pairs** in the class.
2. For each pair, check if they share **at least one instance variable**.
3. Count:
   - **P** = number of pairs that share **no** instance variables
   - **Q** = number of pairs that share **at least one** instance variable
4. **LCOM1 = max(P − Q, 0)**

### Example 1: High Cohesion (LCOM1 = 0)

```java
class Rectangle {
    private double width;   // A
    private double height;  // B

    double getWidth()  { return width; }        // uses {A}
    double getHeight() { return height; }       // uses {B}
    double getArea()   { return width * height;} // uses {A, B}
}
```

**Method pairs and shared variables:**

| Pair                    | Variables Used          | Shared? |
|-------------------------|------------------------|---------|
| (getWidth, getHeight)   | {A} vs {B}             | No      |
| (getWidth, getArea)     | {A} vs {A, B}          | Yes (A) |
| (getHeight, getArea)    | {B} vs {A, B}          | Yes (B) |

- P = 1 (no shared variables)
- Q = 2 (shared variables)
- **LCOM1 = max(1 − 2, 0) = 0** ✅ Cohesive!

### Example 2: Low Cohesion (LCOM1 > 0)

```java
class UtilityGodClass {
    private String userName;      // A
    private double accountBalance;// B
    private List<Order> orders;   // C

    String getUserName()          { return userName; }       // uses {A}
    void deposit(double amount)   { accountBalance += amount;} // uses {B}
    void addOrder(Order order)    { orders.add(order); }     // uses {C}
}
```

**Method pairs and shared variables:**

| Pair                      | Variables Used | Shared? |
|---------------------------|---------------|---------|
| (getUserName, deposit)    | {A} vs {B}    | No      |
| (getUserName, addOrder)   | {A} vs {C}    | No      |
| (deposit, addOrder)       | {B} vs {C}    | No      |

- P = 3 (no shared variables)
- Q = 0 (shared variables)
- **LCOM1 = max(3 − 0, 0) = 3** ❌ Not cohesive at all!

This class should be split into three separate classes: `UserProfile`, `Account`, and `OrderManager`.

---

## LCOM2 — Normalized Version (Henderson-Sellers, 1996)

LCOM1 has a problem: it does not scale well. A class with 100 methods will naturally have a huge number of pairs. LCOM2 normalizes the value to a **0–1 range**.

### Formula

```
LCOM2 = 1 − (sum of methods accessing each variable) / (methods × variables)
```

More precisely:

```
              1       a
LCOM2 = 1 − ───── ·  Σ  μ(Aⱼ)
            m · a    j=1
```

Where:
- `m` = number of methods
- `a` = number of instance variables
- `μ(Aⱼ)` = number of methods that access variable `Aⱼ`

Which simplifies to:

```
LCOM2 = 1 − (averageMethodsPerVariable / totalMethods)
```

### Interpretation

| LCOM2 Value | Meaning                              |
|-------------|--------------------------------------|
| 0.0         | Perfect cohesion (every method uses every variable) |
| 0.0 – 0.3   | Good cohesion                        |
| 0.3 – 0.5   | Moderate — review the class          |
| 0.5 – 0.8   | Low cohesion — consider refactoring  |
| 0.8 – 1.0   | Very low — this class should be split |

### Example 3: Calculating LCOM2

```java
class ShoppingCart {
    private List<Item> items;     // A
    private double totalPrice;    // B
    private Customer customer;    // C

    void addItem(Item item) {                    // uses {A, B}
        items.add(item);
        totalPrice += item.getPrice();
    }

    void removeItem(Item item) {                 // uses {A, B}
        items.remove(item);
        totalPrice -= item.getPrice();
    }

    double getTotal() {                          // uses {B}
        return totalPrice;
    }

    String getCustomerName() {                   // uses {C}
        return customer.getName();
    }
}
```

**Access matrix:**

| Method           | items (A) | totalPrice (B) | customer (C) |
|------------------|:---------:|:--------------:|:------------:|
| addItem          | ✓         | ✓              |              |
| removeItem       | ✓         | ✓              |              |
| getTotal         |           | ✓              |              |
| getCustomerName  |           |                | ✓            |

- `μ(A)` = 2 (addItem, removeItem)
- `μ(B)` = 3 (addItem, removeItem, getTotal)
- `μ(C)` = 1 (getCustomerName)
- `m` = 4 methods
- `a` = 3 variables

```
averageMethodsPerVariable = (2 + 3 + 1) / 3 = 2.0
LCOM2 = 1 − (2.0 / 4) = 1 − 0.5 = 0.5
```

**LCOM2 = 0.5** ⚠️ Moderate. The `customer` field is only used by one method — it might belong in a separate class.

### Example 4: Perfect Cohesion

```java
class Vector2D {
    private double x;  // A
    private double y;  // B

    double getX()         { return x; }             // uses {A}
    double getY()         { return y; }             // uses {B}
    double magnitude()    { return Math.sqrt(x*x + y*y); } // uses {A, B}
    Vector2D normalize()  { double m = magnitude(); return new Vector2D(x/m, y/m); } // uses {A, B}
    double dot(Vector2D v){ return x*v.x + y*v.y; } // uses {A, B}
}
```

| Method     | x (A) | y (B) |
|------------|:-----:|:-----:|
| getX       | ✓     |       |
| getY       |       | ✓     |
| magnitude  | ✓     | ✓     |
| normalize  | ✓     | ✓     |
| dot        | ✓     | ✓     |

- `μ(A)` = 4, `μ(B)` = 4
- `m` = 5, `a` = 2

```
averageMethodsPerVariable = (4 + 4) / 2 = 4.0
LCOM2 = 1 − (4.0 / 5) = 1 − 0.8 = 0.2
```

**LCOM2 = 0.2** ✅ Good cohesion!

---

## LCOM and the Single Responsibility Principle

LCOM is essentially a **numerical detector for SRP violations**. When LCOM is high, it usually means:

- The class has **multiple groups** of methods that operate on **different subsets** of fields.
- Each group represents a **separate responsibility**.
- The class should be **split** accordingly.

### Example 5: SRP Violation Detected by LCOM

```java
// HIGH LCOM — Two responsibilities in one class
class Employee {
    // Group 1 fields
    private String name;
    private String department;

    // Group 2 fields
    private double salary;
    private double bonus;

    // Group 1 methods — identity management
    String getName()        { return name; }
    String getDepartment()  { return department; }
    void transfer(String d) { department = d; }

    // Group 2 methods — payroll
    double getSalary()      { return salary; }
    double getBonus()       { return bonus; }
    double getTotalPay()    { return salary + bonus; }
}
```

Group 1 methods use `{name, department}`, Group 2 methods use `{salary, bonus}`. These two groups share **zero** variables → LCOM is high.

**Refactored into two cohesive classes:**

```java
// LOW LCOM — each class has one responsibility
class EmployeeProfile {
    private String name;
    private String department;

    String getName()        { return name; }
    String getDepartment()  { return department; }
    void transfer(String d) { department = d; }
}

class EmployeePayroll {
    private double salary;
    private double bonus;

    double getSalary()   { return salary; }
    double getBonus()    { return bonus; }
    double getTotalPay() { return salary + bonus; }
}
```

Now each class has low LCOM because every method uses the same fields.

---

## Common Pitfalls

### 1. Getters/Setters Inflate LCOM

A class full of simple getters/setters will have high LCOM because each getter uses only one field. This is a **known limitation** — LCOM works best on classes with behavioral logic, not pure data holders (DTOs).

### 2. Constructors and Utility Methods

Constructors that initialize all fields touch every variable and artificially lower LCOM. Most tools **exclude constructors** from the calculation for a more realistic score.

### 3. LCOM Alone is Not Enough

Always combine LCOM with other metrics:

| Metric | What It Measures | Complements LCOM By... |
|--------|-----------------|----------------------|
| CC     | Decision complexity inside methods | Revealing internal complexity |
| RFC    | Total methods that can execute | Revealing external coupling |
| CBO    | Number of dependent classes | Revealing inter-class dependencies |
| WMC    | Sum of all methods' complexity | Revealing overall class weight |

---

## Quick Reference

| Indicator | What to Do |
|-----------|-----------|
| LCOM ≈ 0  | Class is well-focused. Leave it alone. |
| LCOM moderate | Review: is there a secondary responsibility creeping in? |
| LCOM high | Split the class. Look for field clusters that go together. |
| High LCOM + High CBO | Urgent refactoring needed — class is both unfocused and tightly coupled. |
| High LCOM on a DTO | Likely a false alarm — getters inflate the metric. |

---

## Summary

> **LCOM answers one question: "Does this class have a single, focused purpose?"**
>
> Low LCOM = yes → the class is cohesive.
> High LCOM = no → the class is doing too many things and should be split.
>
> It is one of the most practical metrics for identifying **God Classes** and **SRP violations** in object-oriented design.

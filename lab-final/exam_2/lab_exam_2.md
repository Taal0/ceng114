---
Ankara Yıldırım Beyazıt University · Department of Computer Engineering

# CENG114 — Computer Programming II
## Lab Exam (Mock #2)
### Spring 2025–2026

**Instructor:** Yusuf Evren AYKAÇ  
**Assistants:** Çağın ÖZKAYA, Hatice UYSAL, Yusuf Ekrem KEÇİLİOĞLU

**Estimated Duration:** 120 minutes  
**Total Points:** 100 · Q1: 55 pts · Q2: 45 pts

---

## Learning Objectives

By the end of this exam, you will have demonstrated the ability to:

1. Define and throw a **custom checked exception** with a meaningful message.
2. Use **try-with-resources** correctly to manage multiple I/O streams without leaks.
3. Process a malformed text file and route valid records and errors to **separate output files**.
4. Write a **`@FunctionalInterface`** and use it as a callback (event-handler pattern) — without JavaFX.
5. Build a **Stream pipeline** that filters, maps, sorts, and aggregates data using lambdas and `Collectors`.
6. Implement **Quick Sort** with a custom `Comparator` argument, exercising the Lomuto partition scheme.

---

## Setup

1. Create a project folder named `mock_exam_2` and place all `.java` files inside it.
2. Create sub-folder `mock_exam_2/data/` and copy the sample files provided in each question.
3. Compile and run from `mock_exam_2/` so that paths like `data/server.log` resolve correctly.
4. Each question has its own driver class with a `main` method (`Q1Main.java`, `Q2Main.java`).

---

## Allowed Imports

```
java.io.BufferedReader
java.io.FileReader
java.io.FileWriter
java.io.PrintWriter
java.io.IOException
java.util.ArrayList
java.util.List
java.util.Comparator
java.util.Map
java.util.stream.Collectors
```

**Forbidden:** `java.util.Collections.sort`, `Arrays.sort`, `java.util.TreeMap`, `java.util.stream.Stream` factories (use `.stream()` on a `List` only).

---

## Question 1 — Server Log Processor (55 points)

### Scenario

You are a junior DevOps engineer at CloudStack Inc. Every hour, the application server dumps a raw log file `data/server.log`. Each line is supposed to have the format:

```
TIMESTAMP,LEVEL,SERVICE,MESSAGE
```

Where:
- `TIMESTAMP` — an integer (Unix-style epoch seconds, e.g. `1716912000`)
- `LEVEL` — one of `INFO`, `WARN`, `ERROR` (case-sensitive)
- `SERVICE` — a non-empty service name (no spaces)
- `MESSAGE` — any text (may contain commas — split on first 3 commas only)

Lines that do not conform to this format must be rejected and written to an error log. Your program must:

1. Read `data/server.log` line by line.
2. Validate each line — throw appropriate exceptions when invalid.
3. Write valid lines to `data/valid_entries.txt`.
4. Write rejected lines (with reason) to `data/parse_errors.log`.
5. Print a summary at the end.

### Required Classes

#### `MalformedLogEntryException` (checked)

```java
public class MalformedLogEntryException extends Exception {
    private final int lineNumber;
    // Constructor: takes message + lineNumber
    // Getter: getLineNumber()
}
```

#### `InvalidLogLevelException` (checked)

```java
public class InvalidLogLevelException extends Exception {
    private final String level;
    // Constructor: takes message + badLevel
    // Getter: getLevel()
}
```

#### `LogEntry` — plain data class

Fields: `long timestamp`, `String level`, `String service`, `String message`  
Provide a constructor, getters, and a `toString()` that reconstructs the CSV line (timestamp,level,service,message).

#### `Q1Main`

Must contain:

```java
static LogEntry parseLine(String line, int lineNum)
        throws MalformedLogEntryException, InvalidLogLevelException;
```

**Parsing rules inside `parseLine`:**

1. Split `line` into at most 4 parts on `,` — `line.split(",", 4)`.
2. If `parts.length < 4` → throw `MalformedLogEntryException("Line " + lineNum + ": expected 4 fields, got " + parts.length)`.
3. Try `Long.parseLong(parts[0].trim())` — if `NumberFormatException` → throw `MalformedLogEntryException("Line " + lineNum + ": invalid timestamp '" + parts[0].trim() + "'")`.
4. If `parts[1].trim()` is not one of `INFO`, `WARN`, `ERROR` → throw `InvalidLogLevelException("Line " + lineNum + ": unknown level '" + parts[1].trim() + "'")`.
5. If `parts[2].trim()` is empty → throw `MalformedLogEntryException("Line " + lineNum + ": service name is blank")`.
6. Otherwise return a new `LogEntry`.

**`main` method requirements:**

- Open `data/server.log` for reading and **both** `data/valid_entries.txt` and `data/parse_errors.log` for writing **in a single `try-with-resources` statement**.
- Inside the loop, call `parseLine` inside its own `try–catch` block with **separate** catch clauses for `MalformedLogEntryException` and `InvalidLogLevelException` (most specific first, then `Exception` as a safety net).
- Valid entries → `valid_entries.txt` (the reconstructed CSV line).
- Errors → `parse_errors.log` in the format: `[ERROR] Line <n>: <exceptionMessage>`
- The program must **never crash** regardless of log content.
- After the loop, print:

```
Processing complete.
Valid entries : <count>
Parse errors  : <count>
Written to    : data/valid_entries.txt
Error log     : data/parse_errors.log
```

### Sample Input — `data/server.log`

```
1716912000,INFO,auth-service,User alice logged in
1716912060,WARN,db-pool,Connection pool at 80% capacity
1716912120,ERROR,payment-api,Transaction TXN-001 failed: timeout
bad-timestamp,INFO,auth-service,Login attempt
1716912180,DEBUG,cache,Cache miss for key user:42
1716912240,,billing-service,Invoice generated
1716912300,WARN,auth-service,Password reset requested
1716912360,ERROR,scheduler,Job DAILY_REPORT did not complete
justthree,fields,only
1716912420,INFO,api-gateway,Request routed to payment-api
```

### Expected Output (console)

```
Processing complete.
Valid entries : 6
Parse errors  : 4
Written to    : data/valid_entries.txt
Error log     : data/parse_errors.log
```

### Expected `data/valid_entries.txt`

```
1716912000,INFO,auth-service,User alice logged in
1716912060,WARN,db-pool,Connection pool at 80% capacity
1716912120,ERROR,payment-api,Transaction TXN-001 failed: timeout
1716912300,WARN,auth-service,Password reset requested
1716912360,ERROR,scheduler,Job DAILY_REPORT did not complete
1716912420,INFO,api-gateway,Request routed to payment-api
```

### Expected `data/parse_errors.log`

```
[ERROR] Line 4: invalid timestamp 'bad-timestamp'
[ERROR] Line 5: unknown level 'DEBUG'
[ERROR] Line 6: service name is blank
[ERROR] Line 9: expected 4 fields, got 3
```

### Pitfalls

- `split(",", 4)` limits splits to 3, giving at most 4 parts — this lets commas in `MESSAGE` survive.
- `finally` is not required here but all three I/O streams must be in the same TWR header — if only the reader is in TWR and the writers are outside, they may not flush on exception.
- Catch `MalformedLogEntryException` before `Exception` — Java enforces this only if both catch blocks handle the same or related types.

---

## Question 2 — Lambda Pipeline & Quick Sort (45 points)

### Scenario

The analytics team has a `List<Product>` representing an e-commerce catalogue. You must:

- **(Part A — 15 pts):** Write a custom `@FunctionalInterface` `ScoreFunction` and an event-dispatch mechanism that applies multiple scoring strategies to a list of products, announcing results via a callback.
- **(Part B — 20 pts):** Build a Stream pipeline that filters, maps, sorts and collects from the product list.
- **(Part C — 10 pts):** Implement Quick Sort with a `Comparator<T>` argument (generic, Lomuto partition).

### Product class

```java
public class Product {
    private final String name;
    private final String category;
    private final double price;
    private final int stock;

    public Product(String name, String category, double price, int stock) { ... }
    public String getName()     { return name; }
    public String getCategory() { return category; }
    public double getPrice()    { return price; }
    public int    getStock()    { return stock; }
    @Override public String toString() { return name + " (" + category + ", " + price + ")"; }
}
```

### Part A — Functional Interface & Event Dispatch (15 pts)

Define:

```java
@FunctionalInterface
public interface ScoreFunction {
    double score(Product p);
}
```

And a class `ScoreDispatcher` with:

```java
public class ScoreDispatcher {
    private final List<Product> products;
    private final List<ScoreFunction> scorers = new ArrayList<>();
    private final List<String> labels = new ArrayList<>();

    public ScoreDispatcher(List<Product> products) { this.products = products; }

    // Register a scoring strategy with a label
    public void register(String label, ScoreFunction fn) { ... }

    // For each registered scorer, find the highest-scoring product
    // and call: callback.accept(label + ": " + winner.getName() + " → " + score)
    public void dispatch(java.util.function.Consumer<String> callback) { ... }
}
```

**Requirements:**
- `register` appends label + function to the lists (parallel lists are fine).
- `dispatch` iterates over registered scorers, finds the `Product` with the maximum score (use a simple loop or stream), and passes the formatted result string to `callback`.

**In `Q2Main.main`**, demonstrate:

```java
ScoreDispatcher dispatcher = new ScoreDispatcher(products);
dispatcher.register("Cheapest",    p -> -p.getPrice());     // lowest price = highest score
dispatcher.register("Best stock",  p -> (double) p.getStock());
dispatcher.register("Value index", p -> p.getStock() / p.getPrice());

dispatcher.dispatch(System.out::println);
```

Expected output (with the sample product list below):

```
Cheapest: Notebook (5.99) → score -5.99
Best stock: USB Hub → score 200.0
Value index: USB Hub → score 33.39...
```

*(Exact score values depend on sample data — match your implementation to the data.)*

### Part B — Stream Pipeline (20 pts)

Use the following product list (create it in `Q2Main`):

```java
List<Product> products = new ArrayList<>(List.of(
    new Product("Laptop",       "Electronics", 899.99, 50),
    new Product("USB Hub",      "Electronics",  29.99, 200),
    new Product("Notebook",     "Stationery",    5.99, 150),
    new Product("Pen Set",      "Stationery",    3.49, 300),
    new Product("Webcam",       "Electronics", 149.99,  30),
    new Product("Monitor",      "Electronics", 349.99,  20),
    new Product("Desk Lamp",    "Furniture",    49.99,  80),
    new Product("Chair",        "Furniture",   299.99,  10),
    new Product("Sticky Notes", "Stationery",    2.99, 500),
    new Product("Headphones",   "Electronics", 199.99,  45)
));
```

Write **one stream chain per requirement**:

1. **Electronics under $200, sorted by price ASC:**

```
USB Hub        29.99
Webcam        149.99
Headphones    199.99
```

2. **Total stock per category (sorted by category name):**

```
Electronics : 345
Furniture   : 90
Stationery  : 950
```

3. **Top 3 most expensive products (name only):**

```
Laptop
Chair
Monitor
```

4. **Average price per category (2 decimal places):**

```
Electronics : 325.99
Furniture   : 174.99
Stationery  : 4.16
```

5. **Any product below $10? Print `true` or `false`:**

```
true
```

### Part C — Generic Quick Sort with Comparator (10 pts)

Implement:

```java
public static <T> void quickSort(List<T> list, int low, int high, Comparator<T> comparator);
public static <T> int  partition(List<T> list, int low, int high, Comparator<T> comparator);
```

Rules:
- Lomuto partition: pivot = `list.get(high)`.
- Use `comparator.compare(list.get(j), pivot)` to compare.
- Swap using `Collections.swap(list, i, j)` — no manual temp variable needed.
- Calling `quickSort(products, 0, products.size()-1, Comparator.comparingDouble(Product::getPrice))` must produce the products in ascending price order.

**Demonstration in `Q2Main`:**

```java
List<Product> sorted = new ArrayList<>(products);
quickSort(sorted, 0, sorted.size() - 1, Comparator.comparingDouble(Product::getPrice));
System.out.println("\n--- Quick Sort by price ---");
sorted.forEach(p -> System.out.printf("%-15s %.2f%n", p.getName(), p.getPrice()));
```

Expected output:

```
--- Quick Sort by price ---
Sticky Notes    2.99
Pen Set         3.49
Notebook        5.99
USB Hub        29.99
Desk Lamp      49.99
Webcam        149.99
Headphones    199.99
Chair         299.99
Monitor       349.99
Laptop        899.99
```

---

## Hints & Common Pitfalls

1. **TWR with multiple resources:** All three streams can share one `try (R1; R2; R3)` header — they are closed in **reverse declaration order** when the block exits.
2. **`split(",", 4)` vs `split(",")`:** Without the limit, trailing empty strings and commas in `MESSAGE` are lost. Always use the 4-argument form for this log format.
3. **Checked exception in lambda:** `parseLine` throws checked exceptions — you cannot call it directly inside `.stream().map(...)`. Use a regular `for` loop with individual try–catch.
4. **`Consumer<String>` is a standard functional interface:** You do not need to import anything extra; it lives in `java.util.function`. `System.out::println` is a method reference matching `Consumer<String>`.
5. **`Collectors.summingInt` vs `averagingDouble`:** For stock (int), use `summingInt`; for price (double), use `averagingDouble`.
6. **Quick sort Comparator:** `comparator.compare(a, pivot) <= 0` means "a should go to the left partition".

---

*Ankara Yıldırım Beyazıt University · Department of Computer Engineering · Page 1 of 1*

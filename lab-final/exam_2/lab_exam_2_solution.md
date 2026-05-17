---
Ankara Yıldırım Beyazıt University · Department of Computer Engineering

# CENG114 — Lab Exam Mock #2 — SOLUTION
### Spring 2025–2026

> **Do not open this file before attempting the exam!**

---

## Question 1 Solution — Server Log Processor

### MalformedLogEntryException.java

```java
public class MalformedLogEntryException extends Exception {
    private final int lineNumber;

    public MalformedLogEntryException(String message, int lineNumber) {
        super(message);
        this.lineNumber = lineNumber;
    }

    public int getLineNumber() { return lineNumber; }
}
```

### InvalidLogLevelException.java

```java
public class InvalidLogLevelException extends Exception {
    private final String level;

    public InvalidLogLevelException(String message, String level) {
        super(message);
        this.level = level;
    }

    public String getLevel() { return level; }
}
```

### LogEntry.java

```java
public class LogEntry {
    private final long   timestamp;
    private final String level;
    private final String service;
    private final String message;

    public LogEntry(long timestamp, String level, String service, String message) {
        this.timestamp = timestamp;
        this.level     = level;
        this.service   = service;
        this.message   = message;
    }

    public long   getTimestamp() { return timestamp; }
    public String getLevel()     { return level; }
    public String getService()   { return service; }
    public String getMessage()   { return message; }

    // Reconstructs original CSV format
    @Override
    public String toString() {
        return timestamp + "," + level + "," + service + "," + message;
    }
}
```

### Q1Main.java

```java
import java.io.*;

public class Q1Main {

    private static final String[] VALID_LEVELS = {"INFO", "WARN", "ERROR"};

    public static void main(String[] args) {
        int validCount = 0, errorCount = 0;

        // All three streams in ONE try-with-resources statement
        try (BufferedReader reader = new BufferedReader(new FileReader("data/server.log"));
             PrintWriter     valid  = new PrintWriter(new FileWriter("data/valid_entries.txt"));
             PrintWriter     errors = new PrintWriter(new FileWriter("data/parse_errors.log"))) {

            String line;
            int lineNum = 0;

            while ((line = reader.readLine()) != null) {
                lineNum++;
                try {
                    LogEntry entry = parseLine(line, lineNum);
                    valid.println(entry);
                    validCount++;
                } catch (MalformedLogEntryException e) {
                    errors.println("[ERROR] " + e.getMessage());
                    errorCount++;
                } catch (InvalidLogLevelException e) {
                    errors.println("[ERROR] " + e.getMessage());
                    errorCount++;
                } catch (Exception e) {
                    // safety net — should not happen with correct input
                    errors.println("[ERROR] Line " + lineNum + ": unexpected error: " + e.getMessage());
                    errorCount++;
                }
            }

        } catch (IOException e) {
            System.err.println("Fatal I/O error: " + e.getMessage());
            return;
        }

        // Summary
        System.out.println("Processing complete.");
        System.out.println("Valid entries : " + validCount);
        System.out.println("Parse errors  : " + errorCount);
        System.out.println("Written to    : data/valid_entries.txt");
        System.out.println("Error log     : data/parse_errors.log");
    }

    static LogEntry parseLine(String line, int lineNum)
            throws MalformedLogEntryException, InvalidLogLevelException {

        // split on first 3 commas only — message may contain commas
        String[] parts = line.split(",", 4);

        if (parts.length < 4) {
            throw new MalformedLogEntryException(
                "Line " + lineNum + ": expected 4 fields, got " + parts.length, lineNum);
        }

        // Validate timestamp
        long timestamp;
        try {
            timestamp = Long.parseLong(parts[0].trim());
        } catch (NumberFormatException e) {
            throw new MalformedLogEntryException(
                "Line " + lineNum + ": invalid timestamp '" + parts[0].trim() + "'", lineNum);
        }

        // Validate level
        String level = parts[1].trim();
        if (!isValidLevel(level)) {
            throw new InvalidLogLevelException(
                "Line " + lineNum + ": unknown level '" + level + "'", level);
        }

        // Validate service
        String service = parts[2].trim();
        if (service.isEmpty()) {
            throw new MalformedLogEntryException(
                "Line " + lineNum + ": service name is blank", lineNum);
        }

        String message = parts[3].trim();
        return new LogEntry(timestamp, level, service, message);
    }

    private static boolean isValidLevel(String level) {
        for (String valid : VALID_LEVELS) {
            if (valid.equals(level)) return true;
        }
        return false;
    }
}
```

**Why one TWR statement for all three streams?**  
If only the `BufferedReader` is in TWR and the `PrintWriter`s are declared outside, an exception during setup of the writers would leak the reader, or an exception inside the loop would prevent the writers from being flushed and closed — the last buffered bytes would never reach disk. All resources in one TWR guarantees they are closed in reverse order on any exit path.

**Why `split(",", 4)` and not `split(",")`?**  
`"1716912120,ERROR,payment-api,Transaction failed: timeout,retry=3"` has 5 commas. Without a limit, splitting gives 6 parts and the 4th field (message) is broken. With limit 4, Java stops after 3 splits and puts the rest of the string into `parts[3]` intact.

---

## Question 2 Solution — Lambda Pipeline & Quick Sort

### Product.java

```java
public class Product {
    private final String name;
    private final String category;
    private final double price;
    private final int    stock;

    public Product(String name, String category, double price, int stock) {
        this.name     = name;
        this.category = category;
        this.price    = price;
        this.stock    = stock;
    }

    public String getName()     { return name; }
    public String getCategory() { return category; }
    public double getPrice()    { return price; }
    public int    getStock()    { return stock; }

    @Override
    public String toString() { return name + " (" + category + ", " + price + ")"; }
}
```

### ScoreFunction.java

```java
@FunctionalInterface
public interface ScoreFunction {
    double score(Product p);
}
```

### ScoreDispatcher.java

```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.Consumer;

public class ScoreDispatcher {
    private final List<Product>       products;
    private final List<String>        labels   = new ArrayList<>();
    private final List<ScoreFunction> scorers  = new ArrayList<>();

    public ScoreDispatcher(List<Product> products) {
        this.products = products;
    }

    public void register(String label, ScoreFunction fn) {
        labels.add(label);
        scorers.add(fn);
    }

    public void dispatch(Consumer<String> callback) {
        for (int i = 0; i < scorers.size(); i++) {
            ScoreFunction fn = scorers.get(i);
            String label     = labels.get(i);

            Product winner   = null;
            double  bestScore = Double.NEGATIVE_INFINITY;

            for (Product p : products) {
                double s = fn.score(p);
                if (s > bestScore) { bestScore = s; winner = p; }
            }

            if (winner != null) {
                callback.accept(label + ": " + winner.getName() + " → score " + bestScore);
            }
        }
    }
}
```

**Why `@FunctionalInterface`?**  
The annotation is not mandatory for the interface to work as a functional interface, but it makes the compiler enforce the single-abstract-method contract and serves as documentation. If someone accidentally adds a second abstract method, compilation fails immediately.

**Why `Consumer<String>` instead of a custom interface?**  
`Consumer<String>` is already in the standard library (`java.util.function`). Using it means the caller can pass `System.out::println`, a logger, a test assertion, or anything else — maximum flexibility without boilerplate.

---

### Q2Main.java

```java
import java.util.*;
import java.util.stream.Collectors;

public class Q2Main {

    public static void main(String[] args) {
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

        // ── Part A: ScoreDispatcher ────────────────────────────────
        System.out.println("=== Score Dispatch ===");
        ScoreDispatcher dispatcher = new ScoreDispatcher(products);
        dispatcher.register("Cheapest",    p -> -p.getPrice());
        dispatcher.register("Best stock",  p -> (double) p.getStock());
        dispatcher.register("Value index", p -> p.getStock() / p.getPrice());
        dispatcher.dispatch(System.out::println);

        // ── Part B: Stream pipelines ───────────────────────────────

        // 1. Electronics under $200, sorted by price ASC
        System.out.println("\n--- Electronics under $200 (price ASC) ---");
        products.stream()
                .filter(p -> p.getCategory().equals("Electronics") && p.getPrice() < 200)
                .sorted(Comparator.comparingDouble(Product::getPrice))
                .forEach(p -> System.out.printf("%-15s %.2f%n", p.getName(), p.getPrice()));

        // 2. Total stock per category
        System.out.println("\n--- Total stock per category ---");
        products.stream()
                .collect(Collectors.groupingBy(
                        Product::getCategory,
                        Collectors.summingInt(Product::getStock)))
                .entrySet().stream()
                .sorted(Map.Entry.comparingByKey())
                .forEach(e -> System.out.printf("%-12s: %d%n", e.getKey(), e.getValue()));

        // 3. Top 3 most expensive products (name only)
        System.out.println("\n--- Top 3 most expensive ---");
        products.stream()
                .sorted(Comparator.comparingDouble(Product::getPrice).reversed())
                .limit(3)
                .map(Product::getName)
                .forEach(System.out::println);

        // 4. Average price per category
        System.out.println("\n--- Average price per category ---");
        products.stream()
                .collect(Collectors.groupingBy(
                        Product::getCategory,
                        Collectors.averagingDouble(Product::getPrice)))
                .entrySet().stream()
                .sorted(Map.Entry.comparingByKey())
                .forEach(e -> System.out.printf("%-12s: %.2f%n", e.getKey(), e.getValue()));

        // 5. Any product below $10?
        System.out.println("\n--- Any product below $10? ---");
        boolean hasCheap = products.stream().anyMatch(p -> p.getPrice() < 10);
        System.out.println(hasCheap);

        // ── Part C: Quick Sort ─────────────────────────────────────
        List<Product> sorted = new ArrayList<>(products);
        quickSort(sorted, 0, sorted.size() - 1, Comparator.comparingDouble(Product::getPrice));
        System.out.println("\n--- Quick Sort by price ---");
        sorted.forEach(p -> System.out.printf("%-15s %.2f%n", p.getName(), p.getPrice()));
    }

    // ── Generic Quick Sort — Lomuto partition ──────────────────────
    public static <T> void quickSort(List<T> list, int low, int high, Comparator<T> cmp) {
        if (low < high) {
            int p = partition(list, low, high, cmp);
            quickSort(list, low, p - 1, cmp);
            quickSort(list, p + 1, high, cmp);
        }
    }

    public static <T> int partition(List<T> list, int low, int high, Comparator<T> cmp) {
        T pivot = list.get(high);
        int i = low - 1;
        for (int j = low; j < high; j++) {
            if (cmp.compare(list.get(j), pivot) <= 0) {
                i++;
                Collections.swap(list, i, j);
            }
        }
        Collections.swap(list, i + 1, high);
        return i + 1;
    }
}
```

---

### Expected Output

```
=== Score Dispatch ===
Cheapest: Sticky Notes → score -2.99
Best stock: Sticky Notes → score 500.0
Value index: Sticky Notes → score 167.22408963585434

--- Electronics under $200 (price ASC) ---
USB Hub         29.99
Webcam         149.99
Headphones     199.99

--- Total stock per category ---
Electronics : 345
Furniture   : 90
Stationery  : 950

--- Top 3 most expensive ---
Laptop
Monitor
Chair

--- Average price per category ---
Electronics : 325.99
Furniture   : 174.99
Stationery  : 4.16

--- Any product below $10? ---
true

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

### Design Notes — Quick Sort

**Why `Comparator<T>` instead of `T extends Comparable<T>`?**  
`Comparable` ties sorting logic to the class itself — you can only sort one way. `Comparator` is external and lets callers specify **any** ordering (price, name, stock, reversed, chained) without touching `Product`. The Stream API uses this approach everywhere (`sorted(comparator)`).

**Why `Collections.swap` instead of temp variable?**  
`Collections.swap(list, i, j)` is cleaner and works on any `List<T>` — it abstracts away the temp variable and is less error-prone. Under the hood it does exactly `T tmp = list.get(i); list.set(i, list.get(j)); list.set(j, tmp);`.

**Worst case reminder:**  
`quickSort` is O(n²) when the pivot is always the min or max (e.g., already sorted array with last-element pivot). For this exam, average-case O(n log n) suffices — mention it in the exam if asked.

---

## Grading Breakdown

| Item | Points |
|------|--------|
| Q1 — `MalformedLogEntryException` + `InvalidLogLevelException` | 10 |
| Q1 — `LogEntry.toString()` reconstructs CSV | 5 |
| Q1 — `parseLine` all 5 rules correct | 20 |
| Q1 — `main` single TWR, loop with multi-catch, summary | 20 |
| Q2A — `@FunctionalInterface ScoreFunction`, `ScoreDispatcher.dispatch` | 15 |
| Q2B — All 5 stream sections correct | 20 |
| Q2C — `quickSort` + `partition` with Comparator | 10 |
| **Total** | **100** |

---

*CENG114 Computer Programming II — Spring 2025–2026 · AYBU · Mock Exam 2 Solution*

# CENG114 — Java Lambda Expressions (Java 8+)

## Learning Objectives

After this note, you should be able to:

- Explain what a **lambda expression** is and why it exists in Java.
- Identify and create **functional interfaces** (interfaces with exactly one abstract method).
- Write lambda expressions in multiple valid syntactic forms.
- Use lambdas with common Java library types such as `Runnable`, `Comparator`, and the `java.util.function` package.
- Understand **type inference**, **scope rules**, and **variable capture** (`effectively final`).
- Use **method references** (`ClassName::methodName`) as a compact alternative to lambdas.
- Apply lambdas in typical tasks: sorting, filtering, mapping, event handlers, and streams.

> Lambdas were introduced in **Java 8** and are widely used in modern Java code.

---

## 1) Motivation: Why Lambdas?

Before Java 8, you often wrote **anonymous inner classes** to pass behavior (a piece of code) as a parameter.

### Example (before lambdas): `Runnable`

```java
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello from a thread!");
    }
};
new Thread(r).start();
```

This works, but it is **verbose**: a lot of boilerplate for a simple behavior.

### With lambda

```java
Runnable r = () -> System.out.println("Hello from a thread!");
new Thread(r).start();
```

**Key idea:** a lambda is a compact way to represent a function (behavior) that can be passed around as a value.

---

## 2) Core Concept: Functional Interfaces

A **lambda expression can only be used where a functional interface is expected**.

### Definition

A **functional interface** is an interface with **exactly one abstract method**.

Examples from the Java standard library:

- `Runnable` → `void run()`
- `Comparator<T>` → `int compare(T a, T b)`
- `Callable<V>` → `V call() throws Exception`

Java provides an annotation to document this:

```java
@FunctionalInterface
interface MyFunc {
    int apply(int x);
}
```

If you accidentally add a second abstract method, the compiler will complain.

---

## 3) Lambda Expression Syntax

General form:

```
(parameters) -> { body }
```

### 3.1 Parameters: Parentheses rules

- **No parameters**: use empty parentheses `()`

```java
() -> System.out.println("No parameters")
```

- **One parameter**: parentheses can be omitted (if type is inferred)

```java
x -> x * x
```

- **Two or more parameters**: parentheses are required

```java
(a, b) -> a + b
```

### 3.2 Body: Expression vs Block

- **Expression body** (single expression, implicit `return` if needed):

```java
x -> x + 1
```

- **Block body** (multiple statements, you must use `return` when returning a value):

```java
x -> {
    int y = x + 1;
    return y * 2;
}
```

---

## 4) Type Inference and Explicit Types

Java can often infer parameter types from context:

```java
Comparator<String> c1 = (a, b) -> a.length() - b.length();
```

You can also write types explicitly:

```java
Comparator<String> c2 = (String a, String b) -> a.length() - b.length();
```

**Rule:** If you specify a type for one parameter, you must specify for all parameters.

✅ Allowed:

```java
(String a, String b) -> a.length() - b.length()
```

❌ Not allowed:

```java
(String a, b) -> a.length() - b.length()
```

---

## 5) Common Functional Interfaces (`java.util.function`)

Java 8 introduced a standard set of functional interfaces. You should know these because they appear everywhere:

### 5.1 `Predicate<T>` — returns boolean

- Method: `boolean test(T t)`
- Typical use: filtering

```java
import java.util.function.Predicate;

Predicate<Integer> isEven = x -> x % 2 == 0;
System.out.println(isEven.test(10)); // true
```

### 5.2 `Function<T, R>` — transforms a value

- Method: `R apply(T t)`

```java
import java.util.function.Function;

Function<String, Integer> length = s -> s.length();
System.out.println(length.apply("CENG114")); // 7
```

### 5.3 `Consumer<T>` — consumes a value (no return)

- Method: `void accept(T t)`

```java
import java.util.function.Consumer;

Consumer<String> printer = s -> System.out.println(s);
printer.accept("Hello!");
```

### 5.4 `Supplier<T>` — produces a value (no parameter)

- Method: `T get()`

```java
import java.util.function.Supplier;

Supplier<Double> random = () -> Math.random();
System.out.println(random.get());
```

### 5.5 `UnaryOperator<T>` and `BinaryOperator<T>`

- `UnaryOperator<T>` extends `Function<T, T>` (same input/output type)
- `BinaryOperator<T>` extends `BiFunction<T, T, T>`

```java
import java.util.function.UnaryOperator;
import java.util.function.BinaryOperator;

UnaryOperator<Integer> square = x -> x * x;
BinaryOperator<Integer> sum = (a, b) -> a + b;
```

---

## 6) Lambdas with `Comparator` (Sorting Example)

### Sort strings by length

```java
import java.util.*;

List<String> names = Arrays.asList("Ada", "Grace", "Edsger", "Linus");
names.sort((a, b) -> a.length() - b.length());
System.out.println(names);
```

### Sort strings alphabetically (method reference)

```java
names.sort(String::compareTo);
```

---

## 7) Method References (Compact Alternative)

Method references can replace many lambdas when you are simply calling an existing method.

### 7.1 Static method reference

```java
ClassName::staticMethod
```

Example:

```java
import java.util.function.Function;

Function<String, Integer> parser = Integer::parseInt;
System.out.println(parser.apply("42"));
```

### 7.2 Instance method reference on a particular object

```java
object::instanceMethod
```

Example:

```java
Scanner sc = new Scanner(System.in);
Supplier<String> readLine = sc::nextLine;
```

### 7.3 Instance method reference on an arbitrary object of a type

```java
ClassName::instanceMethod
```

Example:

```java
Function<String, String> toUpper = String::toUpperCase;
System.out.println(toUpper.apply("ceng114"));
```

### 7.4 Constructor reference

```java
ClassName::new
```

Example:

```java
import java.util.function.Supplier;

Supplier<List<String>> listSupplier = ArrayList::new;
List<String> list = listSupplier.get();
```

---

## 8) Scope Rules and Variable Capture (`effectively final`)

A lambda can use variables from the surrounding scope, but **those variables must be final or effectively final**.

### Example

```java
int factor = 10; // effectively final if you never change it

Function<Integer, Integer> f = x -> x * factor; // OK
```

If you try to change `factor` later:

```java
factor = 20; // NOT allowed if factor is captured by the lambda
```

You will get a compile-time error.

### Why this rule exists

Because lambdas may run later (e.g., in another thread). Java restricts changes to captured local variables to prevent confusing behavior.

---

## 9) `this` in Lambdas vs Anonymous Classes

### In a lambda, `this` refers to the enclosing object

```java
public class Demo {
    private String name = "Demo";

    public void test() {
        Runnable r = () -> System.out.println(this.name);
        r.run();
    }
}
```

### In an anonymous class, `this` refers to the anonymous class instance

```java
Runnable r = new Runnable() {
    @Override
    public void run() {
        // 'this' is the anonymous Runnable instance, not Demo
    }
};
```

This difference matters when you use `this` or `super`.

---

## 10) Checked Exceptions and Lambdas

Some functional interfaces do not allow checked exceptions. For example, `Function<T,R>` does not declare `throws`.

This means code like this is NOT directly allowed:

```java
Function<String, String> f = s -> {
    // might throw IOException (checked)
    return readFile(s); // if readFile throws IOException
};
```

### Common solutions

- Handle the exception inside the lambda (try/catch)
- Wrap checked exceptions into runtime exceptions
- Define your own functional interface that allows `throws`

Example (try/catch inside lambda):

```java
Function<String, Integer> safeParse = s -> {
    try {
        return Integer.parseInt(s);
    } catch (NumberFormatException e) {
        return 0;
    }
};
```

---

## 11) Lambdas with Streams (Very Common in Modern Java)

Streams are a big reason why lambdas are used heavily.

### Example: filter + map + forEach

```java
import java.util.*;
import java.util.stream.*;

List<Integer> nums = Arrays.asList(1, 2, 3, 4, 5, 6);

nums.stream()
    .filter(x -> x % 2 == 0)      // keep evens
    .map(x -> x * x)              // square them
    .forEach(x -> System.out.println(x));
```

### Example: sum using reduce

```java
int sum = nums.stream()
              .reduce(0, (a, b) -> a + b);
System.out.println(sum);
```

### Example: count long names

```java
List<String> names = Arrays.asList("Ada", "Grace", "Edsger", "Linus");

long count = names.stream()
                  .filter(s -> s.length() >= 5)
                  .count();
System.out.println(count);
```

---

## 12) Overloading and “Target Type” Pitfalls

Sometimes lambdas can be ambiguous when there are overloaded methods.

Example idea:

```java
void doIt(Runnable r) { ... }
void doIt(Callable<Integer> c) { ... }

// doIt(() -> ???) can be ambiguous if it can match both.
```

**Tip for students:** If you get a compiler error about ambiguity, add an explicit cast:

```java
doIt((Runnable) () -> System.out.println("Hi"));
```

---

## 13) Best Practices (CENG114-Level Guidelines)

- Prefer lambdas when they make code **shorter and clearer**.
- If a lambda becomes long and complex, consider:
  - extracting it into a named method, then using a method reference.
- Always keep an eye on readability:
  - `x -> x + 1` is fine
  - 15-line lambda with nested loops is a code smell.

---

## 14) Quick Cheat Sheet

### Basic forms

```java
() -> 42
x -> x * 2
(x, y) -> x + y
(x, y) -> { int z = x + y; return z * 2; }
```

### Common interfaces

```java
Predicate<T> p = t -> ...;        // boolean
Function<T, R> f = t -> ...;      // transform
Consumer<T> c = t -> ...;         // void
Supplier<T> s = () -> ...;        // produce
```

### Method references

```java
Integer::parseInt
String::toUpperCase
System.out::println
ArrayList::new
```

---

## 15) Mini Practice Questions

1. Write a `Predicate<Integer>` that checks if a number is positive.
2. Write a `Function<String, Integer>` that returns the number of characters in a string.
3. Sort a list of strings by their last character using a lambda.
4. Convert this anonymous class to a lambda:

```java
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("Run!");
    }
};
```

---

If you want, I can also produce:
- a **CENG114-style lab sheet** (tasks + expected output),
- a **quiz** (multiple choice + coding questions),
- and a **solutions file** as a separate Markdown document.

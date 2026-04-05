# CENG114 — Java Lambda Expressions: Exercises (with Solutions)

This document provides practice tasks for Java lambda expressions and method references.

---

## Exercise 1 — Predicate (Positive Numbers)

**Task:** Create a `Predicate<Integer>` named `isPositive` that checks whether a number is greater than 0.

### Solution

```java
import java.util.function.Predicate;

Predicate<Integer> isPositive = x -> x > 0;
```

---

## Exercise 2 — Function (String Length)

**Task:** Create a `Function<String, Integer>` named `len` that returns a string’s length.

### Solution

```java
import java.util.function.Function;

Function<String, Integer> len = s -> s.length();
```

---

## Exercise 3 — Consumer (Print Uppercase)

**Task:** Create a `Consumer<String>` that prints the uppercase form of a given string.

### Solution

```java
import java.util.function.Consumer;

Consumer<String> printUpper = s -> System.out.println(s.toUpperCase());
```

---

## Exercise 4 — Supplier (Random Integer)

**Task:** Create a `Supplier<Integer>` that produces a random integer between 0 and 99.

### Solution

```java
import java.util.Random;
import java.util.function.Supplier;

Random rnd = new Random();
Supplier<Integer> rand0to99 = () -> rnd.nextInt(100);
```

---

## Exercise 5 — Sorting with Comparator

**Task:** Given a list of strings, sort them by length (ascending).

### Starter

```java
import java.util.*;

List<String> names = Arrays.asList("Ada", "Grace", "Edsger", "Linus");
```

### Solution

```java
names.sort((a, b) -> a.length() - b.length());
```

---

## Exercise 6 — Sorting by Last Character

**Task:** Sort strings by their last character.  
Assume all strings are non-empty.

### Solution

```java
names.sort((a, b) -> {
    char ca = a.charAt(a.length() - 1);
    char cb = b.charAt(b.length() - 1);
    return Character.compare(ca, cb);
});
```

---

## Exercise 7 — Convert Anonymous Class to Lambda

**Task:** Convert the following anonymous class into a lambda:

```java
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("Run!");
    }
};
```

### Solution

```java
Runnable r = () -> System.out.println("Run!");
```

---

## Exercise 8 — Method Reference Practice

**Task:** Replace the lambda with a method reference:

```java
import java.util.function.Function;

Function<String, Integer> f = s -> Integer.parseInt(s);
```

### Solution

```java
Function<String, Integer> f = Integer::parseInt;
```

---

## Exercise 9 — Streams: Filter + Map + Collect

**Task:** Given a list of integers, produce a new list that contains the squares of only the even numbers.

### Starter

```java
import java.util.*;
import java.util.stream.*;

List<Integer> nums = Arrays.asList(1, 2, 3, 4, 5, 6);
```

### Solution

```java
List<Integer> result = nums.stream()
    .filter(x -> x % 2 == 0)
    .map(x -> x * x)
    .collect(Collectors.toList());
```

---

## Exercise 10 — “Effectively Final” Rule

**Task:** What is wrong with the following code?

```java
int factor = 2;
Function<Integer, Integer> f = x -> x * factor;
factor = 3;
```

### Explanation

The lambda captures `factor`. Captured local variables must be **final or effectively final**.  
Because `factor` is reassigned (`factor = 3;`), it is not effectively final, and the code does not compile.

---

## Extra Challenge — Custom Functional Interface

**Task:** Define a functional interface `IntOp` that represents an operation on two integers and returns an integer.  
Then create a lambda for multiplication.

### Solution

```java
@FunctionalInterface
interface IntOp {
    int apply(int a, int b);
}

IntOp mul = (a, b) -> a * b;
System.out.println(mul.apply(3, 4)); // 12
```

---

If you want, I can generate a **“no solutions”** version for homework submission, and a separate instructor-only solution key.

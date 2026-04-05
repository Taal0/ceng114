# Java Wildcards and Generic Type Parameters

## 1. Generic Type Parameters (E, T, K, V, N, S, U)

### What Are Generic Type Parameters?

Generic type parameters are placeholders for actual types that will be specified when a class, interface, or method is instantiated or invoked. They allow you to write type-safe, reusable code.

### Naming Conventions

The single uppercase letters used in generics are conventions established by Java developers. Here's what each commonly means:

| Parameter | Meaning | Common Usage |
|-----------|---------|--------------|
| `E` | Element | Collections (List<E>, Set<E>) |
| `T` | Type | General purpose, any type |
| `K` | Key | Maps (Map<K, V>) |
| `V` | Value | Maps (Map<K, V>) |
| `N` | Number | Numeric types |
| `S`, `U` | Second, Third types | When multiple type parameters are needed |

**Why these names?** These are just conventions, not requirements. You could technically use any valid identifier. However, using single uppercase letters:
- Distinguishes type parameters from regular class names
- Makes code instantly recognizable as using generics
- Follows Java's established conventions (from `java.util` package)

### Example: Generic Class with Type Parameter

```java
// T is a type parameter - a placeholder for any type
public class Box<T> {
    private T content;
    
    public void set(T content) {
        this.content = content;
    }
    
    public T get() {
        return content;
    }
}

// Usage - T is replaced with String
Box<String> stringBox = new Box<>();
stringBox.set("Hello");
String value = stringBox.get();  // No casting needed

// Usage - T is replaced with Integer
Box<Integer> intBox = new Box<>();
intBox.set(42);
Integer number = intBox.get();
```

### Example: Generic Method

```java
public class Utility {
    // T is declared before return type, scoped to this method only
    public static <T> void printArray(T[] array) {
        for (T element : array) {
            System.out.println(element);
        }
    }
    
    // Multiple type parameters
    public static <K, V> void printPair(K key, V value) {
        System.out.println("Key: " + key + ", Value: " + value);
    }
}

// Usage
String[] names = {"Alice", "Bob", "Charlie"};
Utility.printArray(names);  // T inferred as String

Utility.printPair("age", 25);  // K=String, V=Integer
```

---

## 2. Wildcards (?)

### What Are Wildcards?

A wildcard (`?`) represents an **unknown type**. Unlike type parameters (T, E), wildcards are used when you don't need to refer to the type elsewhere in your code. They provide flexibility when working with generic types.

### Types of Wildcards

#### 2.1 Unbounded Wildcard (`?`)

Used when you want to accept any type and only use methods from `Object` class.

```java
public static void printList(List<?> list) {
    for (Object item : list) {
        System.out.println(item);
    }
}

// Can accept any List
printList(Arrays.asList("a", "b", "c"));      // List<String>
printList(Arrays.asList(1, 2, 3));            // List<Integer>
printList(Arrays.asList(new Dog(), new Cat())); // List<Animal>
```

#### 2.2 Upper Bounded Wildcard (`? extends Type`)

Accepts the specified type or any of its **subtypes**. Use when you want to **read** from a structure.

```java
// Accepts List of Number or any subclass (Integer, Double, Float, etc.)
public static double sumOfList(List<? extends Number> list) {
    double sum = 0.0;
    for (Number n : list) {
        sum += n.doubleValue();
    }
    return sum;
}

// Usage
List<Integer> integers = Arrays.asList(1, 2, 3);
List<Double> doubles = Arrays.asList(1.1, 2.2, 3.3);

System.out.println(sumOfList(integers));  // Works! Output: 6.0
System.out.println(sumOfList(doubles));   // Works! Output: 6.6
```

#### 2.3 Lower Bounded Wildcard (`? super Type`)

Accepts the specified type or any of its **supertypes**. Use when you want to **write** to a structure.

```java
// Accepts List of Integer or any supertype (Number, Object)
public static void addNumbers(List<? super Integer> list) {
    list.add(1);
    list.add(2);
    list.add(3);
}

// Usage
List<Integer> intList = new ArrayList<>();
List<Number> numList = new ArrayList<>();
List<Object> objList = new ArrayList<>();

addNumbers(intList);  // Works!
addNumbers(numList);  // Works!
addNumbers(objList);  // Works!
```

---

## 3. Key Differences: Type Parameters vs Wildcards

### Comparison Table

| Aspect | Type Parameter (T, E) | Wildcard (?) |
|--------|----------------------|--------------|
| **Declaration** | Must be declared (`<T>`) | Used directly (`<?>`) |
| **Reference** | Can be referenced multiple times | Cannot be referenced |
| **Purpose** | Define relationships between types | Express flexibility in argument types |
| **Scope** | Class, interface, or method level | Only in type arguments |
| **Type Safety** | Full type information available | Type is unknown |

### When to Use What?

#### Use Type Parameters When:

1. **You need to refer to the type multiple times**

```java
// T is used in parameter AND return type - must use type parameter
public static <T> T getFirst(List<T> list) {
    return list.get(0);
}
```

2. **You need to establish relationships between parameters**

```java
// Both parameters must be the same type
public static <T> void copy(List<T> source, List<T> destination) {
    destination.addAll(source);
}
```

3. **You're defining a generic class or interface**

```java
public class Pair<K, V> {
    private K key;
    private V value;
    // ...
}
```

#### Use Wildcards When:

1. **You only need to use the type once**

```java
// We just iterate - don't need to reference the type
public static void printAll(List<?> list) {
    for (Object item : list) {
        System.out.println(item);
    }
}
```

2. **You want to express "any subtype" or "any supertype"**

```java
// Any subtype of Shape can be drawn
public void drawAll(List<? extends Shape> shapes) {
    for (Shape s : shapes) {
        s.draw();
    }
}
```

3. **You're working with legacy code or mixed type scenarios**

```java
public boolean containsAll(Collection<?> c) {
    // Check if all elements exist
}
```

---

## 4. The PECS Principle

**PECS** = **P**roducer **E**xtends, **C**onsumer **S**uper

This is a guideline for choosing between `extends` and `super`:

- **Producer** (you READ from it): Use `? extends T`
- **Consumer** (you WRITE to it): Use `? super T`

### Example: PECS in Action

```java
public class Collections {
    
    // 'src' is a PRODUCER - we read from it
    // 'dest' is a CONSUMER - we write to it
    public static <T> void copy(List<? super T> dest, List<? extends T> src) {
        for (int i = 0; i < src.size(); i++) {
            dest.set(i, src.get(i));  // Read from src, write to dest
        }
    }
}

// Usage
List<Number> numbers = new ArrayList<>(Arrays.asList(0, 0, 0));
List<Integer> integers = Arrays.asList(1, 2, 3);

Collections.copy(numbers, integers);  // Works perfectly!
// numbers is now [1, 2, 3]
```

---

## 5. Common Pitfalls and Solutions

### Pitfall 1: Cannot Add to `? extends` Collections

```java
List<? extends Number> list = new ArrayList<Integer>();
// list.add(1);        // COMPILE ERROR!
// list.add(1.0);      // COMPILE ERROR!
// list.add(new Number()); // COMPILE ERROR!

// Why? Compiler doesn't know the actual type.
// The list could be List<Integer>, List<Double>, etc.
// Adding a Double to List<Integer> would break type safety.

// You can only add null
list.add(null);  // This works, but not useful
```

### Pitfall 2: Can Only Read Objects from `? super`

```java
List<? super Integer> list = new ArrayList<Number>();
list.add(1);      // Works!
list.add(2);      // Works!

// But reading gives you Object
Object obj = list.get(0);  // Only Object, not Integer or Number
// Integer i = list.get(0);  // COMPILE ERROR!
```

### Pitfall 3: Wildcard Capture

```java
// This won't compile
public static void swap(List<?> list, int i, int j) {
    // Object temp = list.get(i);
    // list.set(i, list.get(j));  // ERROR: cannot set unknown type
    // list.set(j, temp);
}

// Solution: Use a helper method with type parameter
public static void swap(List<?> list, int i, int j) {
    swapHelper(list, i, j);
}

private static <T> void swapHelper(List<T> list, int i, int j) {
    T temp = list.get(i);
    list.set(i, list.get(j));
    list.set(j, temp);
}
```

---

## 6. Quick Reference Examples

### Generic Class Definition

```java
public class Cache<K, V> {
    private Map<K, V> map = new HashMap<>();
    
    public void put(K key, V value) { map.put(key, value); }
    public V get(K key) { return map.get(key); }
}
```

### Generic Method

```java
public <T extends Comparable<T>> T findMax(List<T> list) {
    return list.stream().max(Comparable::compareTo).orElse(null);
}
```

### Unbounded Wildcard

```java
public void process(List<?> items) { /* read-only operations */ }
```

### Upper Bounded Wildcard

```java
public void draw(List<? extends Shape> shapes) { /* read shapes */ }
```

### Lower Bounded Wildcard

```java
public void fill(List<? super Integer> list) { /* add integers */ }
```

---

## 7. Summary

| Concept | Syntax | Use Case |
|---------|--------|----------|
| Type Parameter | `<T>`, `<E>`, `<K, V>` | Define generic classes/methods |
| Unbounded Wildcard | `<?>` | Accept any type, read-only |
| Upper Bounded | `<? extends T>` | Accept T or subtypes (PRODUCER) |
| Lower Bounded | `<? super T>` | Accept T or supertypes (CONSUMER) |

**Remember:**
- Use **type parameters** when you need to reference the type
- Use **wildcards** for flexibility in method parameters
- Apply **PECS** to choose between `extends` and `super`
- Wildcards make your API more flexible but less powerful than type parameters

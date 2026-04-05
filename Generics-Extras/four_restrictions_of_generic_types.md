# 4 Restrictions of Generic Types

All of these exist because of **type erasure**. Let me explain each one:

## Restriction 1: Cannot Use `new E()`

```java
public class GenericClass<E> {
    public E createInstance() {
        return new E();  // ❌ Error!
    }
}
```

### Why?

At runtime, it's unknown what `E` is. The JVM asks: "Which class's constructor should I call?"

```java
GenericClass<Integer> g1 = new GenericClass<>();
GenericClass<String> g2 = new GenericClass<>();
// At runtime, both are the same: GenericClass
// No E information!
```

### Solution: Pass the Constructor from Outside

```java
public class GenericClass<E> {
    public E createInstance(Supplier<E> supplier) {
        return supplier.get();  // ✅
    }
}

// Usage
GenericClass<Integer> g = new GenericClass<>();
Integer i = g.createInstance(() -> new Integer(0));
```

## Restriction 2: Cannot Use `new E[100]`

```java
public class GenericClass<E> {
    private E[] elements;
    
    public GenericClass() {
        elements = new E[100];  // ❌ Error!
    }
}
```

### Why?

In Java, arrays know and check their types at runtime:

```java
Object[] objArray = new String[10];
objArray[0] = 123;  // 💥 ArrayStoreException at runtime!
```

But since `E` is unknown at runtime, this check cannot be performed.

### Solution: Object Array + Cast

```java
public class GenericClass<E> {
    private E[] elements;
    
    @SuppressWarnings("unchecked")
    public GenericClass() {
        elements = (E[]) new Object[100];  // ✅ But be careful
    }
}
```

## Restriction 3: Cannot Use Generics in Static Context

```java
public class GenericClass<E> {
    private static E staticField;      // ❌ Error!
    
    public static E staticMethod() {   // ❌ Error!
        return null;
    }
    
    public static void test(E param) { // ❌ Error!
    }
}
```

### Why?

Static members belong to the class, not to instances. But `E` can be different for each instance:

```java
GenericClass<Integer> g1 = new GenericClass<>();
GenericClass<String> g2 = new GenericClass<>();

// What should staticField be? Integer or String?
// Static members are SHARED across all instances!
```

### Solution: Define Generic at Method Level

```java
public class GenericClass<E> {
    // Not the class's E, but the method's own T
    public static <T> T staticMethod(T param) {  // ✅
        return param;
    }
}
```

## Restriction 4: Exception Classes Cannot Be Generic

```java
public class MyException<E> extends Exception {  // ❌ Error!
    private E errorData;
}
```

### Why?

The catch block runs at runtime and performs type checking:

```java
try {
    // something
} catch (MyException<Integer> e1) {  // ❌ 
    // No <Integer> information at runtime!
} catch (MyException<String> e2) {   // ❌
    // Which one to catch? Both are the same class!
}
```

After type erasure, both become `MyException`, and the JVM cannot distinguish between them.

### Solution: Non-Generic Exception + Separate Class for Generic Field

```java
public class MyException extends Exception {
    private Object errorData;  // ✅ Not generic
    
    public <T> T getErrorData() {
        return (T) errorData;
    }
}
```

## Summary Table

| Restriction | Example | Reason |
|-------------|---------|--------|
| `new E()` | Instance creation | The class of E is unknown at runtime |
| `new E[100]` | Array creation | Array type checking is done at runtime |
| Static context | `static E field` | Static members are shared across all instances |
| Generic Exception | `class Ex<E> extends Exception` | Catch block cannot distinguish types at runtime |

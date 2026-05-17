<!-- Slide number: 1 -->

CENG114
Computer Programming II  ·  Spring 2025-2026
Week 12
Generics & Wildcards
Type-safe, reusable code in Java
Öğr. Gör. Yusuf Evren Aykaç
Computer Engineering Department  ·  Ankara Yıldırım Beyazıt University

### Notes:

<!-- Slide number: 2 -->
ROADMAP
Today's Agenda
Why Generics?

![preencoded.png](Image0.jpg)
Code duplication, casting pain, runtime errors before generics
Generic Classes & Methods

![preencoded.png](Image1.jpg)
Type parameters E, T, K, V — define once, use with any type
Bounded Type Parameters

![preencoded.png](Image2.jpg)
Restrict to a hierarchy with `<T extends Number>`
Wildcards: ? extends T, ? super T

![preencoded.png](Image3.jpg)
Flexibility without losing type safety — the PECS principle
Restrictions of Generics

![preencoded.png](Image4.jpg)
Type erasure and the four things you cannot do (lighter coverage)
In-Class Exercise: GenericStack<E>

![preencoded.png](Image5.jpg)
Build a type-safe stack from scratch
CENG114 - Week 12  ·  Generics & Wildcards
2 / 35

### Notes:

<!-- Slide number: 3 -->
MOTIVATION
The Problem: Life Before Generics
Imagine writing a 'Box' that holds anything. Without generics, the only general-purpose type is Object — and that opens the door to every kind of trouble.

// Pre-generics Java (≤ JDK 1.4)
public class Box {
    private Object content;

    public void set(Object c) { content = c; }

    public Object get() { return content; }
}

// Usage
Box b = new Box();
b.set("hello");
String s = (String) b.get();  // cast required

b.set(42);
String s2 = (String) b.get(); // 💥 runtime ClassCastException
Three pain points
Manual casting everywhere

![preencoded.png](Image0.jpg)
(String) b.get(), (Integer) ... — clutters every read.
Runtime errors, not compile-time

![preencoded.png](Image1.jpg)
Wrong type slips in silently; you only find out when the JVM throws ClassCastException.
No semantic intent in the API

![preencoded.png](Image2.jpg)
Box.set(Object) tells the reader nothing — what is this box supposed to hold?
CENG114 - Week 12  ·  Generics & Wildcards
3 / 35

### Notes:

<!-- Slide number: 4 -->
MOTIVATION
The Solution: Generics (since JDK 5)
A generic type parameter is a placeholder for a type. The compiler — not the JVM — checks that the right kind of object is going in and coming out. Errors move from runtime to compile time. Casts disappear from your code.

public class Box<T> {
    private T content;

    public void set(T c) { content = c; }

    public T get() { return content; }
}

// Usage
Box<String> b = new Box<>();
b.set("hello");
String s = b.get();  // no cast

b.set(42);          // ✓ compile error
Three wins
Compile-time type safety

![preencoded.png](Image0.jpg)
The compiler rejects b.set(42) for a Box<String> before you even run.
No casts at the call site

![preencoded.png](Image1.jpg)
b.get() returns String directly. Cleaner code, fewer ClassCastExceptions.
Self-documenting APIs

![preencoded.png](Image2.jpg)
Box<String>, List<Customer>, Map<Long, Order> — the type tells you the contract.
CENG114 - Week 12  ·  Generics & Wildcards
4 / 35

### Notes:

<!-- Slide number: 5 -->
GENERICS 101
Naming Conventions: E, T, K, V, N, S, U
Single capital letters are conventions, not requirements — but stick to them. They mark a name as a type parameter at a glance and make code instantly recognizable to other Java developers.
| Letter | Stands for | Typical use | Real example |
| --- | --- | --- | --- |
| E | Element | Containers / collections | List<E>, Set<E>, Queue<E> |
| T | Type | General-purpose any type | Box<T>, Optional<T> |
| K | Key | Map keys | Map<K, V>, HashMap<K, V> |
| V | Value | Map values | Map<K, V>, Function<T, R>'s R |
| N | Number | Numeric type parameter | <N extends Number> |
| S, U, R | Second, third, return | Multi-parameter classes/methods | Function<T, R>, BiFunction<T, U, R> |
Why single letters? They visually separate type parameters from regular class names — Box<Customer> reads instantly, Box<C> would not.
CENG114 - Week 12  ·  Generics & Wildcards
5 / 35

### Notes:

<!-- Slide number: 6 -->
GENERIC CLASSES
Defining a Generic Class
Declare the type parameter in angle brackets right after the class name. Inside the class, T behaves like a real type that has not yet been chosen — the user of your class will pick it.

public class Box<T> {
    private T content;

    public void set(T content) {
        this.content = content;
    }

    public T get() {
        return content;
    }
}

<T> after class name
Declares the type parameter. Once declared, T is in scope for the entire class body.

T as a field type
The compiler treats T as 'some type chosen later'. It cannot call type-specific methods on it (only Object methods).

T in method signatures
set(T) and get() : T tie the parameter and return type to the same chosen type — that is what gives type safety.
CENG114 - Week 12  ·  Generics & Wildcards
6 / 35

### Notes:

<!-- Slide number: 7 -->
GENERIC CLASSES
Box<T> in Action: Pick the Type at Use

// ─── String box ─────────────────────────────────────
Box<String> stringBox = new Box<>();
stringBox.set("Hello, generics!");
String value = stringBox.get();   // no cast — return type is String

// ─── Integer box ────────────────────────────────────
Box<Integer> intBox = new Box<>();
intBox.set(42);
Integer number = intBox.get();    // returns Integer directly

// ─── Type safety, enforced by the compiler ──────────
intBox.set("oops");                // ✗ compile error: incompatible types
Integer n = stringBox.get();      // ✗ compile error: String not assignable to Integer
The diamond operator '<>' lets the compiler infer the type — write `new Box<>()` instead of `new Box<String>()`.

![preencoded.png](Image0.jpg)
CENG114 - Week 12  ·  Generics & Wildcards
7 / 35

### Notes:

<!-- Slide number: 8 -->
GENERIC CLASSES
Multiple Type Parameters: Pair<K, V>
A generic class can declare more than one type parameter, separated by commas. This is how Map<K, V>, Function<T, R>, and other library types are built.

public class Pair<K, V> {
    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey()   { return key; }
    public V getValue() { return value; }
}

// Usage
Pair<String, Integer> age =
    new Pair<>("Ada", 5);

String name = age.getKey();
Integer  yrs = age.getValue();

Pair<Long, Order> entry =
    new Pair<>(1001L, order);

// Different K, V each time
// — same class, type-safe.
CENG114 - Week 12  ·  Generics & Wildcards
8 / 35

### Notes:

<!-- Slide number: 9 -->
GENERICS 101
Generic Methods
A method can declare its own type parameter — independent of any class-level parameter. The angle brackets go BEFORE the return type.

public class Utility {
    // <T> declared BEFORE the return type
    public static <T> void printArray(T[] array) {
        for (T element : array)
            System.out.println(element);
    }
}
Reading the signature
static <T>
<T> declares the type parameter. It's local to this method.
void
Return type. The method returns nothing here.
printArray(T[] array)
Parameter uses T — its type is bound when the method is called.

String[] names = {"Ada", "Bob", "Eve"};
Utility.printArray(names);   // T inferred = String
Integer[] nums = {1, 2, 3};
Utility.printArray(nums);    // T inferred = Integer
Inference
The compiler infers T from the argument: pass String[] → T = String.
CENG114 - Week 12  ·  Generics & Wildcards
9 / 35

### Notes:

<!-- Slide number: 10 -->
GENERICS 101
Generic Methods with Multiple Type Parameters
A method can introduce as many type parameters as it needs — list them inside the angle brackets, separated by commas.

// Two independent type parameters: K and V
public static <K, V> void printPair(K key, V value) {
    System.out.println("Key: " + key + ", Value: " + value);
}

// At each call site, K and V are inferred independently:
Utility.printPair("age", 25);           // K = String,  V = Integer
Utility.printPair(1L, customer);              // K = Long,    V = Customer
Utility.printPair(point, "origin");        // K = Point,   V = String

// You can also be explicit (rarely needed thanks to inference):
Utility.<String, Integer>printPair("x", 10);
CENG114 - Week 12  ·  Generics & Wildcards
10 / 35

### Notes:

<!-- Slide number: 11 -->
GENERICS 101
Bounded Type Parameters: <T extends X>
Sometimes you need to restrict T to a hierarchy — for example, only numeric types so you can call .doubleValue(). Use the `extends` keyword (works for classes AND interfaces).

// Without a bound: only Object methods are callable
public static <T> double sum(T[] arr) {
    double total = 0.0;
    for (T t : arr)
        total += t.doubleValue();  // ✗ no such method on Object
    return total;
}

// With a bound: T must be Number or a subtype
public static <T extends Number> double sum(T[] arr) {
    double total = 0.0;
    for (T t : arr)
        total += t.doubleValue();  // ✓ Number has it
    return total;
}
What 'extends' buys you
• Inside the method, T is treated as Number — you can call any Number method on it.
• At the call site, only Integer, Double, Long, Float, BigInteger, etc. are accepted.
• Multiple bounds: <T extends A & B & C>. At most one class, then any number of interfaces.
• Common idiom: <T extends Comparable<T>> for sortable types.
CENG114 - Week 12  ·  Generics & Wildcards
11 / 35

### Notes:

<!-- Slide number: 12 -->
WILDCARDS
Wildcards: The '?' Type
A wildcard '?' represents an unknown type. Unlike T, you cannot refer to it again elsewhere. Use it when a method only needs to accept some generic type — without naming it.

Unbounded
Upper-bounded
Lower-bounded

![preencoded.png](Image0.jpg)

![preencoded.png](Image1.jpg)

![preencoded.png](Image2.jpg)
List<?>
List<? extends T>
List<? super T>
Any type. You can read elements as Object, but you cannot add anything (except null).
T or any subtype. Producer — safe to READ as T. Cannot add (except null).
T or any supertype. Consumer — safe to WRITE T into. Reads only as Object.

void log(List<?> xs) {
  for (Object x : xs) ...
}
double sum(
  List<? extends Number> ns)
// reads ns[i] as Number
void fill(
  List<? super Integer> dst)
// dst.add(42) is allowed
CENG114 - Week 12  ·  Generics & Wildcards
12 / 35

### Notes:

<!-- Slide number: 13 -->
WILDCARDS
Unbounded Wildcard <?>
Use <?> when the method only needs Object operations — printing, counting, checking emptiness — and does not care about the element type at all.

public static void printList(List<?> list) {
    for (Object item : list)
        System.out.println(item);
}

// Accepts ANY List, regardless of element type:
printList(Arrays.asList("a", "b", "c"));
printList(Arrays.asList(1, 2, 3));
printList(Arrays.asList(dog, cat, bird));

// You CANNOT add anything (compiler doesn't know the type):
List<?> list = new ArrayList<String>();
list.add("x");                  // ✗ compile error
list.add(null);                  // ✓ only null is OK
When to reach for <?>
• Method body uses only Object methods (toString, equals, hashCode).
• You don't need to refer to the type elsewhere in the signature.
• Read-only operations: print, log, count, contains.
Why is List<?> not the same as List<Object>?
List<Object> means 'a list whose element type is exactly Object'. List<?> means 'a list of some unknown element type' — and a List<String> IS a List<?>, but it is NOT a List<Object>.
CENG114 - Week 12  ·  Generics & Wildcards
13 / 35

### Notes:

<!-- Slide number: 14 -->
WILDCARDS
Upper-Bounded Wildcard <? extends T>
Accepts T or any subtype. Inside the method, you can READ elements as T — but you CANNOT add anything (except null), because the compiler doesn't know which subtype the list actually holds.

// Sum any list of Number-or-subtype
public static double sumOfList(List<? extends Number> list) {
    double total = 0.0;
    for (Number n : list)
        total += n.doubleValue();
    return total;
}

List<Integer> ints  = List.of(1, 2, 3);
List<Double>  doubs = List.of(1.1, 2.2);

sumOfList(ints);    // ✓ 6.0
sumOfList(doubs);   // ✓ 3.3

// Without ? extends Number, only List<Number> would compile —
// List<Integer> would NOT, because List<Integer> is NOT a List<Number>.
What '? extends Number' allows

Number

Integer
✓

Double
✓

Long
✓

Float
✓
All four subtypes are accepted by the same method signature.
CENG114 - Week 12  ·  Generics & Wildcards
14 / 35

### Notes:

<!-- Slide number: 15 -->
WILDCARDS
Lower-Bounded Wildcard <? super T>
Accepts T or any supertype. Inside the method, you can WRITE T into it — but reads only give you Object, because the actual type might be Object (or any other supertype).

// Add Integers to any list that can hold them
public static void addNumbers(List<? super Integer> list) {
    list.add(1);
    list.add(2);
    list.add(3);
}

List<Integer> intList = new ArrayList<>();
List<Number>  numList = new ArrayList<>();
List<Object>  objList = new ArrayList<>();

addNumbers(intList);  // ✓
addNumbers(numList);  // ✓ Number is a supertype of Integer
addNumbers(objList);  // ✓ Object is the ultimate supertype
What '? super Integer' allows

Object
✓

Number
✓

Integer
✓
↑ direction of '? super Integer' ↑
Reading caveat
list.get(0) returns Object — the compiler can't promise Integer, since the actual list might be List<Object>.
CENG114 - Week 12  ·  Generics & Wildcards
15 / 35

### Notes:

<!-- Slide number: 16 -->
WILDCARDS
The PECS Principle

PECS
Producer Extends, Consumer Super

Producer → use ? extends T
Consumer → use ? super T

![preencoded.png](Image0.jpg)

![preencoded.png](Image1.jpg)
The structure GIVES you values.
You READ from it.
You can treat reads as type T.
You CANNOT write into it.
The structure RECEIVES values.
You WRITE into it.
You can pass T's into it safely.
Reads come back only as Object.

List<? extends Number> nums  // I read Numbers out
List<? super Integer> sink   // I push Integers in
Rule of thumb: if you only put things in, use super. If you only take things out, use extends. If you do both, use a plain T (no wildcard).
CENG114 - Week 12  ·  Generics & Wildcards
16 / 35

### Notes:

<!-- Slide number: 17 -->
WILDCARDS
PECS in Action: Collections.copy
The textbook example: copy elements from a source list to a destination list. The source PRODUCES (read), the destination CONSUMES (write).

public static <T> void copy(List<? super T> dest, List<? extends T> src) {
    for (int i = 0; i < src.size(); i++)
        dest.set(i, src.get(i));   // read T from src, write T into dest
}

List<Number>  numbers  = new ArrayList<>(List.of(0, 0, 0));
List<Integer> integers = List.of(1, 2, 3);

Collections.copy(numbers, integers);   // ✓ T inferred = Number; Integer ⊆ Number ⊆ Number

Why both wildcards? Without them, copy(List<T>, List<T>) would force src and dest to have the EXACT same T. With <? super T> on dest and <? extends T> on src, you can copy List<Integer> into List<Number> — which is the natural, useful behavior.
CENG114 - Week 12  ·  Generics & Wildcards
17 / 35

### Notes:

<!-- Slide number: 18 -->
WILDCARDS
Type Parameters vs Wildcards
| Aspect | Type Parameter <T> | Wildcard <?> |
| --- | --- | --- |
| Declaration | Must be declared, e.g. <T> | Used directly, no declaration |
| Reusable in signature? | Yes — refer to T in params, return, body | No — each ? is anonymous |
| Multiple references | List<T> a, List<T> b — must match | List<?> a, List<?> b — independent ?'s |
| Type info inside method | Full — T is a real placeholder | Unknown — only Object methods callable |
| Best for | Generic classes / methods that relate types | Method parameters that just need flexibility |
| Verbosity | Slightly heavier — declare + use | Light — single ? at the use site |
Quick test: would I name this T anywhere else? If yes → type parameter. If no → wildcard.

![preencoded.png](Image0.jpg)
CENG114 - Week 12  ·  Generics & Wildcards
18 / 35

### Notes:

<!-- Slide number: 19 -->
WILDCARDS
When to Use What?

Type parameter <T>

Wildcard ?
• Refer to the type more than once
• You only need the type once, anonymously

<T> T getFirst(List<T> list)
// T appears in param AND return type
void printAll(List<?> list)
// just iterate; no need to name the type
• Force two arguments to share a type
• Express "any subtype" or "any supertype"

<T> void copy(List<T> src, List<T> dst)
// both lists must hold the same T
void drawAll(List<? extends Shape> shapes)
// works for List<Circle>, List<Square>, …
• Define a generic class or interface
• API parameter — flexibility for callers

class Pair<K, V> { ... }
// types live in fields too
boolean containsAll(Collection<?> c)
// caller doesn't have to match exactly
CENG114 - Week 12  ·  Generics & Wildcards
19 / 35

### Notes:

<!-- Slide number: 20 -->
PITFALLS
Pitfall #1: '? extends' Lists Are Read-Only
With <? extends T>, the compiler doesn't know which subtype is actually inside. Adding any specific value could break the real list — so it forbids all writes (except null).

List<? extends Number> list = new ArrayList<Integer>();

list.add(1);                     // ✗ compile error
list.add(1.0);                   // ✗ compile error
list.add(new Number());            // ✗ compile error  (Number is abstract anyway)
list.add(null);                  // ✓  null fits any type

// Why? The actual list is ArrayList<Integer>.
// If add(1.0) were allowed, you would have a Double in a List<Integer> — type system broken.

// READING is fine — the value comes out as Number:
Number n = list.get(0);                  // ✓
CENG114 - Week 12  ·  Generics & Wildcards
20 / 35

### Notes:

<!-- Slide number: 21 -->
PITFALLS
Pitfall #2: '? super' Reads Only as Object
Mirror image of pitfall #1. With <? super T> you can ADD a T, but when you READ, the compiler can only promise Object — because the underlying list might really be List<Object>.

List<? super Integer> list = new ArrayList<Number>();

list.add(1);                      // ✓ Integer fits anywhere ≥ Integer
list.add(2);                      // ✓

Object  o = list.get(0);           // ✓ always safe
Integer i = list.get(0);           // ✗ compile error
Number  n = list.get(0);           // ✗ compile error

// Why? The list could really be List<Number> or List<Object>.
// The compiler can only guarantee Object on reads.
CENG114 - Week 12  ·  Generics & Wildcards
21 / 35

### Notes:

<!-- Slide number: 22 -->
PITFALLS
Pitfall #3: Wildcard Capture
Sometimes you genuinely need to swap two elements of a List<?>. The direct version doesn't compile — but a tiny helper method 'captures' the wildcard into a real type parameter T.

// ✗ Doesn't compile
public static void swap(List<?> list, int i, int j) {
    Object tmp = list.get(i);
    list.set(i, list.get(j));   // ✗
    list.set(j, tmp);            // ✗
}
// list.set expects the EXACT element type,
// but ? is unknown — Object is too wide.
// ✓ Capture-helper pattern
public static void swap(List<?> list, int i, int j) {
    swapHelper(list, i, j);
}

private static <T> void swapHelper(
        List<T> list, int i, int j) {
    T tmp = list.get(i);
    list.set(i, list.get(j));
    list.set(j, tmp);
}
CENG114 - Week 12  ·  Generics & Wildcards
22 / 35

### Notes:

<!-- Slide number: 23 -->
RESTRICTIONS
Type Erasure: How Generics Really Work
Generics are a COMPILE-TIME feature. After type checking, the compiler ERASES the parameters and replaces them with their bounds (or Object). At runtime, there is no T, no Box<String> vs Box<Integer> — just Box.

// What you write
public class Box<T> {
    private T content;
    public T get() { return content; }
    public void set(T v) { content = v; }
}

Box<String> b = new Box<>();
b.set("hi");
String s = b.get();
// What the JVM sees (after erasure)
public class Box {
    private Object content;
    public Object get() { return content; }
    public void set(Object v) { content = v; }
}

Box b = new Box();
b.set("hi");
String s = (String) b.get();   // cast inserted by compiler
CENG114 - Week 12  ·  Generics & Wildcards
23 / 35

### Notes:

<!-- Slide number: 24 -->
RESTRICTIONS
Four Things You Cannot Do With Generics
Each of these follows directly from type erasure: at runtime, the type parameter is gone, so anything that needs to know the runtime type fails.

1
Cannot create new E()

2
Cannot create new E[100]

return new E();
elements = new E[100];
Which class's constructor would the JVM call?
Arrays check their type at runtime — E is gone.

3
Cannot use E in static context

4
Cannot make a generic Exception

private static E field;
class MyEx<E> extends Exception
Static members are shared, but each instance can have a different E.
catch blocks can't distinguish MyEx<Integer> from MyEx<String>.
CENG114 - Week 12  ·  Generics & Wildcards
24 / 35

### Notes:

<!-- Slide number: 25 -->
RESTRICTIONS
Restrictions: The Idiomatic Workarounds

// 1) Instead of new E() — pass a Supplier (or Class<E>):
public E create(Supplier<E> factory) { return factory.get(); }
Box<Customer> b = ...; b.create(Customer::new);

// 2) Instead of new E[n] — Object[] cast (with @SuppressWarnings):
@SuppressWarnings("unchecked")
E[] data = (E[]) new Object[capacity];

// 3) Instead of static E — declare T at the METHOD level:
public class Util {
    public static <T> T identity(T x) { return x; }
}

// 4) Instead of generic Exception — non-generic + Object payload:
public class AppException extends Exception {
    private final Object payload;
    @SuppressWarnings("unchecked")
    public <T> T getPayload() { return (T) payload; }
}
CENG114 - Week 12  ·  Generics & Wildcards
25 / 35

### Notes:

<!-- Slide number: 26 -->
CASE STUDY
Case Study: GenericMatrix<E extends Number>
A real example combining bounded generics + the Template Method pattern. The abstract class defines HOW matrices are added/multiplied; subclasses fill in the per-element arithmetic.

public abstract class GenericMatrix<E extends Number> {
    // To be supplied by subclasses
    protected abstract E add(E a, E b);
    protected abstract E multiply(E a, E b);
    protected abstract E zero();

    // Concrete algorithms — same for every E
    public E[][] addMatrix(E[][] a, E[][] b) { ... }
    public E[][] multiplyMatrix(E[][] a, E[][] b) { ... }
}
What's happening here
<E extends Number>
Bounded type parameter — only numeric types allowed.
abstract methods
The 'hook' — element-wise arithmetic that subclasses define.
concrete methods
The 'template' — matrix-level loops, written once for all E.
Template Method pattern
The skeleton lives in the parent; subclasses just fill in details.
CENG114 - Week 12  ·  Generics & Wildcards
26 / 35

### Notes:

<!-- Slide number: 27 -->
CASE STUDY
GenericMatrix: Concrete Subclasses
With the bound <E extends Number>, you can build IntegerMatrix, DoubleMatrix, BigDecimalMatrix… each with three tiny method overrides.

public class IntegerMatrix extends
        GenericMatrix<Integer> {

    @Override
    protected Integer add(Integer a, Integer b) {
        return a + b;
    }

    @Override
    protected Integer multiply(Integer a, Integer b) {
        return a * b;
    }

    @Override
    protected Integer zero() { return 0; }
}
public class DoubleMatrix extends
        GenericMatrix<Double> {

    @Override
    protected Double add(Double a, Double b) {
        return a + b;
    }

    @Override
    protected Double multiply(Double a, Double b) {
        return a * b;
    }

    @Override
    protected Double zero() { return 0.0; }
}
CENG114 - Week 12  ·  Generics & Wildcards
27 / 35

### Notes:

<!-- Slide number: 28 -->
CASE STUDY
GenericMatrix.addMatrix — Step by Step

public E[][] addMatrix(E[][] m1, E[][] m2) {
    // 1) Validate dimensions
    if (m1.length != m2.length ||
        m1[0].length != m2[0].length)
        throw new RuntimeException(
            "Matrices must have same size");

    // 2) Create result of same shape
    E[][] result = (E[][])
        new Number[m1.length][m1[0].length];

    // 3) Element-wise addition
    for (int i = 0; i < result.length; i++)
        for (int j = 0; j < result[i].length; j++)
            result[i][j] = add(m1[i][j], m2[i][j]);

    return result;
}
2×3 + 2×3 → 2×3

1

2

3

7

8

9
+

4

5

6

10

11

12
=

8

10

12

14

16

18
result[i][j] = add(m1[i][j], m2[i][j])

Two nested loops. Time: O(rows × cols).
CENG114 - Week 12  ·  Generics & Wildcards
28 / 35

### Notes:

<!-- Slide number: 29 -->
CASE STUDY
GenericMatrix in Use

Console output
public class TestIntegerMatrix {
    public static void main(String[] args) {
        Integer[][] m1 =
            {{1,2,3}, {4,5,6}, {1,1,1}};
        Integer[][] m2 =
            {{1,1,1}, {2,2,2}, {0,0,0}};

        IntegerMatrix mat = new IntegerMatrix();
        Integer[][] sum  = mat.addMatrix(m1, m2);
        Integer[][] prod = mat.multiplyMatrix(
            m1, new Integer[][]{{1,2},{3,4},{5,6}});

        GenericMatrix.printResult(m1, m2,  sum,  '+');
        GenericMatrix.printResult(m1, m3, prod, '*');
    }
}
Matrix Addition:
 1 2 3       1 1 1       2 3 4
 4 5 6  +    2 2 2  =    6 7 8
 1 1 1       0 0 0       1 1 1

Matrix Multiplication:
 1 2 3       1 2       22 28
 4 5 6  *    3 4  =    49 64
 1 1 1       5 6        9 12
CENG114 - Week 12  ·  Generics & Wildcards
29 / 35

### Notes:

<!-- Slide number: 30 -->
SUMMARY
Generics Toolkit: Quick Reference
| Concept | Syntax | When to reach for it |
| --- | --- | --- |
| Generic class | class Box<T> { ... } | Container / data structure that works for many types |
| Multi-param generic | class Pair<K, V> { ... } | Map-like structures, function types |
| Generic method | static <T> T id(T x) | Utility methods independent of class-level T |
| Bounded type | <T extends Number> | You need methods of a base class/interface |
| Unbounded wildcard | List<?> | Read-only or Object-only operations |
| Upper-bounded ? | List<? extends T> | Producer — read T's out (PECS) |
| Lower-bounded ? | List<? super T> | Consumer — write T's in (PECS) |
Reach for the lightest tool that solves the problem: T when you must reuse the type, ? when you don't.
CENG114 - Week 12  ·  Generics & Wildcards
30 / 35

### Notes:

<!-- Slide number: 31 -->

IN-CLASS EXERCISE
Build a Type-Safe Stack
GenericStack<E>

![preencoded.png](Image0.jpg)
Apply everything from today: a generic class, bounded array trick, type-safe push/pop.
Time: 25 minutes  ·  Pair work encouraged

### Notes:

<!-- Slide number: 32 -->
EXERCISE
Exercise: GenericStack<E> — Specification
Implement a LIFO stack that holds elements of any type E. Use an internal array — NOT java.util.Stack or ArrayDeque — so you exercise the bounded-array trick from earlier.

public class GenericStack<E> {
    private E[] data;
    private int size;

    public GenericStack();              // default capacity 16
    public GenericStack(int capacity);

    public void push(E element);   // grow if full
    public E    pop();           // throw if empty
    public E    peek();          // throw if empty

    public int  size();
    public boolean isEmpty();

    @Override
    public String toString();    // e.g.  [1, 2, 3] (top right)
}
Requirements
• E may be any reference type — String, Integer, Customer, …
• Auto-grow the array when full (double the capacity).
• Throw EmptyStackException on pop()/peek() when size == 0.
• Stack must compile and run without raw-type warnings (one @SuppressWarnings is OK on the array creation).
• Bonus: implement Iterable<E> for use in a for-each loop.
CENG114 - Week 12  ·  Generics & Wildcards
32 / 35

### Notes:

<!-- Slide number: 33 -->
EXERCISE
Exercise: Hints & Expected Demo

Implementation Hints

Expected Demo

![preencoded.png](Image0.jpg)

![preencoded.png](Image1.jpg)

Array creation

data = (E[]) new Object[capacity];
GenericStack<Integer> s = new GenericStack<>();
s.push(1); s.push(2); s.push(3);
System.out.println(s);          // [1, 2, 3]
System.out.println(s.peek());   // 3
System.out.println(s.pop());    // 3
System.out.println(s.size());   // 2

push(E e)
if (size == data.length) grow();
data[size++] = e;

pop()
if (isEmpty()) throw new EmptyStackException();
E top = data[--size]; data[size] = null;
return top;

peek()
if (isEmpty()) throw new EmptyStackException();
return data[size - 1];
Console output

[1, 2, 3]
3
3
2

grow()
data = Arrays.copyOf(data, data.length*2);

Iterator
Return elements from top (size-1) down to 0.
CENG114 - Week 12  ·  Generics & Wildcards
33 / 35

### Notes:

<!-- Slide number: 34 -->
WRAP-UP
Key Takeaways
Generics are about COMPILE-TIME safety

![preencoded.png](Image0.jpg)
Errors caught before you run. No more ClassCastException at 3 a.m.
Type parameter <T> when you reuse the type

![preencoded.png](Image1.jpg)
Class fields, multiple parameters, return types — anywhere T appears more than once.
Wildcard <?> when the method just needs flexibility

![preencoded.png](Image2.jpg)
<?> for read-only, <? extends T> to read T's, <? super T> to write T's.
Remember PECS

![preencoded.png](Image3.jpg)
Producer-Extends, Consumer-Super. The mnemonic that decides which wildcard to use.
Type erasure shapes the restrictions

![preencoded.png](Image4.jpg)
No new E(), no new E[], no static E, no generic exceptions — all because E disappears at runtime.
CENG114 - Week 12  ·  Generics & Wildcards
34 / 35

### Notes:

<!-- Slide number: 35 -->

Questions?
Bring your GenericStack<E> attempts to the next lab — we'll review solutions together.

NEXT WEEK
Sorting & Searching Algorithms
Bubble, selection, insertion, merge & quick sort — and how generics keep them type-safe across types.
Öğr. Gör. Yusuf Evren Aykaç  ·  CENG114  ·  Ankara Yıldırım Beyazıt University

### Notes:
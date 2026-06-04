# CENG114 — Mock Final Exam 1

**Instructions:** 25 questions · 5 options each (A–E) · 90 minutes · No notes, no IDE.  
Answer key and explanations: see `mock_final_1_solution.md`.

---

## Sorting & Searching (Q1–Q2)

**Q1.** What is the worst-case time complexity of QuickSort when the Lomuto partition scheme (pivot = last element) is applied to an array that is already sorted in ascending order?

- A) O(n)
- B) O(n log n)
- C) O(n²)
- D) O(log n)
- E) O(n³)

---

**Q2.** Which of the following sorting algorithms is **both** stable **and** in-place?

- A) Quick Sort
- B) Merge Sort
- C) Selection Sort
- D) Insertion Sort
- E) Both A and D

---

## Recursion (Q3–Q4)

**Q3.** What does the following code print?

```java
static int f(int n) {
    if (n <= 1) return n;
    return f(n - 1) + f(n - 2);
}
public static void main(String[] args) {
    System.out.println(f(6));
}
```

- A) 6
- B) 7
- C) 8
- D) 13
- E) 5

---

**Q4.** In the recursive binary search below, which line(s) constitute the **base case(s)**?

```java
static int bSearch(int[] a, int lo, int hi, int key) {
    if (lo > hi) return -1;                           // Line 1
    int mid = (lo + hi) / 2;
    if (a[mid] == key) return mid;                    // Line 2
    if (a[mid] < key) return bSearch(a, mid+1, hi, key);
    return bSearch(a, lo, mid-1, key);
}
```

- A) Line 1 only
- B) Line 2 only
- C) Both Line 1 and Line 2
- D) Neither — binary search is iterative by definition
- E) The entire method body is the base case

---

## OOP / Encapsulation (Q5)

**Q5.** Which OOP principle is violated by the following class definition?

```java
public class Student {
    public int grade;
    public String name;
}
```

- A) Inheritance
- B) Polymorphism
- C) Encapsulation
- D) Abstraction
- E) No principle is violated; public fields are perfectly valid Java

---

## Inheritance & Polymorphism (Q6–Q7)

**Q6.** What does the following code print?

```java
class Animal {
    String sound() { return "..."; }
    void speak() { System.out.println(sound()); }
}
class Dog extends Animal {
    String sound() { return "Woof"; }
}
public class Main {
    public static void main(String[] args) {
        Animal a = new Dog();
        a.speak();
    }
}
```

- A) `...`
- B) `Woof`
- C) Compile error: `sound()` is not visible from `Animal`
- D) `null`
- E) Runtime error: cannot call `sound()` through an `Animal` reference

---

**Q7.** What is printed?

```java
class Shape {
    double area() { return 0; }
    void describe() { System.out.println("Area: " + area()); }
}
class Circle extends Shape {
    double r;
    Circle(double r) { this.r = r; }
    double area() { return 3.14 * r * r; }
}
public class Main {
    public static void main(String[] args) {
        Shape s = new Circle(2);
        s.describe();
    }
}
```

- A) `Area: 0.0`
- B) `Area: 12.56`
- C) `Area: 6.28`
- D) Compile error: `area()` is not accessible from `Shape`
- E) `Area: 3.14`

---

## Abstract Classes & Interfaces (Q8–Q9)

**Q8.** Which statement about abstract classes in Java is **correct**?

- A) An abstract class cannot have a constructor.
- B) An abstract class can be instantiated directly if all abstract methods are implemented by the caller.
- C) An abstract class can have both abstract methods and concrete (implemented) methods.
- D) A class can extend multiple abstract classes.
- E) An abstract method must have a method body (empty block `{}`).

---

**Q9.** What happens when you try to compile the following code?

```java
interface Drawable {
    void draw();
    void resize(double factor);
}

class Square implements Drawable {
    public void draw() { System.out.println("Drawing square"); }
    // resize() is not implemented
}
```

- A) Compiles and runs normally; `resize` defaults to a no-op.
- B) Compile error: `Square` is not declared `abstract` and does not implement `resize`.
- C) Runtime error the first time `resize` is called.
- D) Compile error: an interface cannot declare more than one method.
- E) Compiles, but `Square` becomes implicitly abstract.

---

## Exception Handling & Text I/O (Q10–Q12)

**Q10.** What does the following snippet print?

```java
public static void main(String[] args) {
    try {
        int x = Integer.parseInt("abc");
        System.out.print("X");
    } catch (NumberFormatException e) {
        System.out.print("A");
        return;
    } finally {
        System.out.print("B");
    }
    System.out.print("C");
}
```

- A) `A`
- B) `AB`
- C) `ABC`
- D) `XAB`
- E) `XB`

---

**Q11.** Which of the following is a **checked** exception?

- A) `NullPointerException`
- B) `ArrayIndexOutOfBoundsException`
- C) `ClassCastException`
- D) `IOException`
- E) `IllegalArgumentException`

---

**Q12.** Consider the following try-with-resources block:

```java
try (BufferedReader br = new BufferedReader(new FileReader("in.txt"));
     PrintWriter pw = new PrintWriter(new FileWriter("out.txt"))) {
    String line;
    while ((line = br.readLine()) != null)
        pw.println(line);
} catch (IOException e) { e.printStackTrace(); }
```

In what order are `br` and `pw` **closed** when the try block exits normally?

- A) `br` is closed first, then `pw`.
- B) `pw` is closed first, then `br`.
- C) Both are closed simultaneously.
- D) Neither is closed automatically; you must call `close()` manually.
- E) `br` only; `PrintWriter` does not implement `AutoCloseable`.

---

## Generics & Wildcards / PECS (Q13–Q15)

**Q13.** Which of the following lines inside the generic class `Container<T>` causes a **compile error**?

```java
class Container<T> {
    T item = new T();                         // Line A
    T[] arr = new T[5];                       // Line B
    static T sharedItem;                      // Line C
    boolean check(Object o) {
        return o instanceof T;                // Line D
    }
}
```

- A) Line A only
- B) Lines A and B only
- C) Lines A, B, and C only
- D) All of Lines A, B, C, and D
- E) None — all lines are valid Java

---

**Q14.** Which wildcard should replace `???` so that the method compiles and correctly accepts `List<Integer>`, `List<Double>`, and `List<Float>` as arguments?

```java
static double sumList(List<???> numbers) {
    double total = 0;
    for (Number n : numbers) total += n.doubleValue();
    return total;
}
```

- A) `<Number>`
- B) `<? super Number>`
- C) `<? extends Number>`
- D) `<?>`
- E) `<T extends Number>` as a raw wildcard inside `<>`

---

**Q15.** What happens when the following code is compiled?

```java
List<Integer> ints = new ArrayList<>();
List<Number>  nums = ints;    // Line X
nums.add(3.14);
System.out.println(ints.get(0));
```

- A) Compiles and runs; prints `3.14`.
- B) Compiles; `ClassCastException` at `nums.add(3.14)` at runtime.
- C) Compile error at Line X: `List<Integer>` is not assignable to `List<Number>`.
- D) Compile error at `nums.add(3.14)` only.
- E) Compiles; `ClassCastException` at `ints.get(0)` at runtime.

---

## Linked Lists (Q16–Q19)

**Q16.** What is the time complexity of `get(i)` on a **Singly Linked List** (no tail pointer, no index cache)?

- A) O(1)
- B) O(log n)
- C) O(n)
- D) O(n log n)
- E) O(n²)

---

**Q17.** A `SinglyLinkedList<Integer>` starts empty. The following operations are performed in order:

```
addFirst(3)   →   addFirst(2)   →   addFirst(1)   →   addLast(4)
```

The list is then printed as `[head → ... → tail]`. After one call to `removeFirst()`, what does the list contain?

- A) `[1, 2, 3]`
- B) `[2, 3, 4]`
- C) `[1, 2, 4]`
- D) `[3, 4]`
- E) `[1, 3, 4]`

---

**Q18.** The following `addAt` method has a bug. What is it?

```java
public void addAt(int index, T data) {
    Node<T> curr = head;
    for (int i = 0; i < index - 1; i++) curr = curr.next;
    Node<T> newNode = new Node<>(data);
    curr.next = newNode;          // Line A
    newNode.next = curr.next;     // Line B
}
```

- A) `newNode.next` must be assigned **before** `curr.next` is updated (Lines A and B are swapped).
- B) The loop bound should be `i < index` instead of `i < index - 1`.
- C) `newNode` should be created before the loop.
- D) `size++` is missing, and its absence changes the pointer logic.
- E) There is no bug; the code is correct.

---

**Q19.** In a **Doubly Linked List** that maintains both `head` and `tail` pointers, which of the following operations runs in **O(1)** time?

- A) `get(i)` for an arbitrary index `i` in the middle.
- B) `addLast(data)`.
- C) `addAt(n/2, data)` (insertion at the exact midpoint).
- D) `contains(value)` for an arbitrary value.
- E) `reverse()`.

---

## Stacks & Queues (Q20–Q21)

**Q20.** What is printed by the following code?

```java
Deque<Integer> stack = new ArrayDeque<>();
stack.push(1);
stack.push(2);
stack.push(3);
System.out.print(stack.pop() + " ");
stack.push(4);
System.out.print(stack.pop() + " ");
System.out.print(stack.pop() + " ");
```

- A) `1 2 3`
- B) `3 2 1`
- C) `3 4 2`
- D) `3 4 1`
- E) `1 4 2`

---

**Q21.** Which data structure is **most appropriate** for implementing a web browser's "Back" button, where pressing Back navigates to the previously visited page?

- A) Queue (FIFO)
- B) Stack (LIFO)
- C) Singly Linked List with only a head pointer
- D) ArrayList where every visit appends to the front (index 0)
- E) Priority Queue ordered by visit timestamp

---

## Lambda & Streams (Q22–Q24)

**Q22.** What does the following code print?

```java
List<Integer> nums = List.of(1, 2, 3, 4, 5);
int result = nums.stream()
                 .filter(n -> n % 2 == 0)
                 .mapToInt(n -> n * n)
                 .sum();
System.out.println(result);
```

- A) 9
- B) 16
- C) 20
- D) 25
- E) 30

---

**Q23.** Which built-in functional interface has the method signature `R apply(T t)` — takes one argument and returns a result?

- A) `Predicate<T>`
- B) `Consumer<T>`
- C) `Supplier<T>`
- D) `Function<T, R>`
- E) `Runnable`

---

**Q24.** What does the following stream pipeline print?

```java
List<String> words = List.of("banana", "apple", "cherry", "ant");
List<String> result = words.stream()
    .filter(s -> s.startsWith("a"))
    .sorted()
    .collect(Collectors.toList());
System.out.println(result);
```

- A) `[apple, ant]`
- B) `[ant, apple]`
- C) `[ant, apple, banana]`
- D) `[apple]`
- E) `[banana, apple, cherry, ant]`

---

## Event-Driven Programming (Q25)

**Q25.** In a Java event-driven system, what is the role of the **Event Handler**?

- A) It is the UI component (e.g., `Button`) that detects user input and generates the event.
- B) It is the object that carries metadata about the event, such as source and timestamp.
- C) It registers all components so they can receive events from the JVM.
- D) It is the callback code (method / lambda) that is invoked when the event fires.
- E) It prevents events from propagating to other handlers once it has processed the event.

---

*End of exam. Good luck!*

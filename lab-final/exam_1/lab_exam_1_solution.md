---
Ankara Yıldırım Beyazıt University · Department of Computer Engineering

# CENG114 — Lab Exam Mock #1 — SOLUTION
### Spring 2025–2026

> **Do not open this file before attempting the exam!**

---

## Question 1 Solution — GenericOrderQueue

### Node.java (inner class — shown separately for clarity)

### GenericOrderQueue.java

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import java.util.NoSuchElementException;

public class GenericOrderQueue<E> implements Iterable<E> {

    // ── Static nested Node ─────────────────────────────────────────
    private static class Node<E> {
        E data;
        Node<E> next;
        Node(E data) { this.data = data; this.next = null; }
    }

    // ── Fields ─────────────────────────────────────────────────────
    private Node<E> top;   // newest (most recently pushed) element
    private int size;

    // ── Core operations ────────────────────────────────────────────
    public void push(E item) {
        Node<E> newNode = new Node<>(item);
        newNode.next = top;   // prepend: new node points to old top
        top = newNode;
        size++;
    }

    public E pop() {
        if (isEmpty()) throw new NoSuchElementException("Queue is empty");
        E data = top.data;
        top = top.next;
        size--;
        return data;
    }

    public E peek() {
        if (isEmpty()) throw new NoSuchElementException("Queue is empty");
        return top.data;
    }

    public boolean isEmpty() { return top == null; }
    public int size()        { return size; }

    // ── toList ─────────────────────────────────────────────────────
    public List<E> toList() {
        List<E> result = new ArrayList<>();
        Node<E> curr = top;
        while (curr != null) {
            result.add(curr.data);
            curr = curr.next;
        }
        return result;
    }

    // ── Iterator ───────────────────────────────────────────────────
    @Override
    public Iterator<E> iterator() {
        return new Iterator<E>() {
            private Node<E> curr = top;   // own pointer — does not touch queue state

            @Override
            public boolean hasNext() { return curr != null; }

            @Override
            public E next() {
                if (!hasNext()) throw new NoSuchElementException();
                E data = curr.data;
                curr = curr.next;
                return data;
            }
        };
    }

    // ── Generic bounded static method ──────────────────────────────
    public static <E extends Comparable<E>> E findMin(GenericOrderQueue<? extends E> queue) {
        if (queue.isEmpty()) return null;
        E min = null;
        for (E element : queue) {         // uses iterator() — queue unmodified
            if (min == null || element.compareTo(min) < 0) {
                min = element;
            }
        }
        return min;
    }

    // ── toString ───────────────────────────────────────────────────
    @Override
    public String toString() { return toList().toString(); }
}
```

**Why static nested Node?**  
A non-static inner class holds an implicit reference to its outer class instance. Every `Node` would then carry a pointer to `GenericOrderQueue` — memory overhead and confusing semantics. Static nested class is self-contained.

**Why `? extends E` in findMin?**  
`GenericOrderQueue<String>` is not a subtype of `GenericOrderQueue<E>` (generics are invariant), but `GenericOrderQueue<? extends E>` accepts `GenericOrderQueue<String>`, `GenericOrderQueue<Integer>`, etc. We only *read* from the queue, so `extends` is correct (PECS: Producer Extends).

---

### Q1Main.java

```java
public class Q1Main {
    public static void main(String[] args) {
        GenericOrderQueue<String> queue = new GenericOrderQueue<>();

        System.out.println("=== GenericOrderQueue Demo ===");
        System.out.println("push: CS101, MATH201, PHYS101, CS201");
        queue.push("CS101");
        queue.push("MATH201");
        queue.push("PHYS101");
        queue.push("CS201");

        System.out.println();
        System.out.println("peek()     → " + queue.peek());
        System.out.println("size()     → " + queue.size());
        System.out.println("toList()   → " + queue.toList());
        System.out.println("findMin()  → " + GenericOrderQueue.findMin(queue));

        System.out.println();
        System.out.println("pop()      → " + queue.pop());
        System.out.println("pop()      → " + queue.pop());
        System.out.println("toList()   → " + queue.toList());
        System.out.println("size()     → " + queue.size());
    }
}
```

**Expected output:**
```
=== GenericOrderQueue Demo ===
push: CS101, MATH201, PHYS101, CS201

peek()     → CS201
size()     → 4
toList()   → [CS201, PHYS101, MATH201, CS101]
findMin()  → CS101

pop()      → CS201
pop()      → PHYS101
toList()   → [MATH201, CS101]
size()     → 2
```

---

## Question 2 Solution — ListUtils & Enrollment Report

### Student.java

```java
public class Student {
    private final String courseCode;
    private final String name;
    private final String letterGrade;
    private final int score;

    public Student(String courseCode, String name, String letterGrade, int score) {
        this.courseCode  = courseCode;
        this.name        = name;
        this.letterGrade = letterGrade;
        this.score       = score;
    }

    public String getCourseCode()  { return courseCode; }
    public String getName()        { return name; }
    public String getLetterGrade() { return letterGrade; }
    public int    getScore()       { return score; }

    @Override
    public String toString() {
        return String.format("%-8s %-8s %s  %d", name, courseCode, letterGrade, score);
    }
}
```

---

### ListUtils.java

```java
import java.util.List;

public class ListUtils {

    // (a) Producer: reads from list — ? extends Number
    public static double average(List<? extends Number> scores) {
        if (scores.isEmpty()) return 0.0;
        double sum = 0;
        for (Number n : scores) sum += n.doubleValue();
        return sum / scores.size();
    }

    // (b) Consumer: writes into destination — ? super String
    public static void collectNames(List<? extends String> source,
                                    List<? super String> destination) {
        for (String s : source) destination.add(s);
    }

    // (c) Unbounded: structure only
    public static void printAll(List<?> items) {
        for (Object item : items) System.out.println(item);
    }

    // (d) PECS combined
    public static void collectAbove(List<? extends Number> source,
                                    List<? super Number> destination,
                                    double threshold) {
        for (Number n : source) {
            if (n.doubleValue() > threshold) destination.add(n);
        }
    }
}
```

**Why (a) uses `? extends Number`:**  
We only *read* values (call `doubleValue()`). The method works for `List<Integer>`, `List<Double>`, `List<Float>` etc. — producer side, so `extends`.

**Why (b) uses `? super String` for destination:**  
We *write* `String` into destination. The destination could be `List<String>`, `List<Object>`, etc. — consumer side, so `super`.

---

### Q2Main.java

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.util.*;
import java.util.stream.Collectors;

public class Q2Main {

    public static void main(String[] args) {
        List<Student> students = loadStudents("data/courses.txt");

        System.out.println("=== Enrollment Report ===");

        // ── Section 1: All students sorted by score DESC, then name ASC ──
        System.out.println("\n--- All Students (sorted by score DESC, then name ASC) ---");
        students.stream()
                .sorted(Comparator.comparingInt(Student::getScore).reversed()
                                  .thenComparing(Student::getName))
                .forEach(s -> System.out.printf("%-8s %-8s %s  %d%n",
                        s.getName(), s.getCourseCode(), s.getLetterGrade(), s.getScore()));

        // ── Section 2: Students with score >= 85 ──
        System.out.println("\n--- Students with score >= 85 ---");

        // Using ListUtils.collectAbove for demonstration of PECS
        List<Integer> allScores = students.stream()
                .map(Student::getScore)
                .collect(Collectors.toList());
        List<Number> highScoreNumbers = new ArrayList<>();
        ListUtils.collectAbove(allScores, highScoreNumbers, 84);

        // Stream filter approach for display
        students.stream()
                .filter(s -> s.getScore() >= 85)
                .sorted(Comparator.comparingInt(Student::getScore).reversed())
                .forEach(s -> System.out.printf("%-8s %d%n", s.getName(), s.getScore()));

        // ── Section 3: Average score per course ──
        System.out.println("\n--- Average score per course ---");
        Map<String, Double> avgByCode = students.stream()
                .collect(Collectors.groupingBy(
                        Student::getCourseCode,
                        Collectors.averagingInt(Student::getScore)));

        avgByCode.entrySet().stream()
                .sorted(Map.Entry.comparingByKey())
                .forEach(e -> System.out.printf("%-8s: %.2f%n", e.getKey(), e.getValue()));

        // ── Section 4: Top scorer per course ──
        System.out.println("\n--- Top scorer per course ---");
        Map<String, Optional<Student>> topByCode = students.stream()
                .collect(Collectors.groupingBy(
                        Student::getCourseCode,
                        Collectors.maxBy(Comparator.comparingInt(Student::getScore))));

        topByCode.entrySet().stream()
                .sorted(Map.Entry.comparingByKey())
                .forEach(e -> e.getValue().ifPresent(s ->
                        System.out.printf("%-8s: %s (%d)%n",
                                e.getKey(), s.getName(), s.getScore())));
    }

    private static List<Student> loadStudents(String path) {
        List<Student> list = new ArrayList<>();
        try (BufferedReader br = new BufferedReader(new FileReader(path))) {
            String line;
            while ((line = br.readLine()) != null) {
                String[] parts = line.split(",");
                if (parts.length < 4) continue;
                String courseCode  = parts[0].trim();
                String name        = parts[1].trim();
                String letterGrade = parts[2].trim();
                int    score       = Integer.parseInt(parts[3].trim());
                list.add(new Student(courseCode, name, letterGrade, score));
            }
        } catch (IOException | NumberFormatException e) {
            System.err.println("Error loading students: " + e.getMessage());
        }
        return list;
    }
}
```

---

### data/courses.txt (copy verbatim)

```
CS101,Alice,A,90
CS101,Bob,B,78
MATH201,Charlie,A,92
MATH201,Alice,C,65
CS201,Bob,A,88
CS201,Diana,B,81
PHYS101,Charlie,C,60
PHYS101,Diana,A,95
```

---

### Expected Output

```
=== Enrollment Report ===

--- All Students (sorted by score DESC, then name ASC) ---
Diana    PHYS101  A  95
Charlie  MATH201  A  92
Alice    CS101    A  90
Bob      CS201    A  88
Diana    CS201    B  81
Bob      CS101    B  78
Alice    MATH201  C  65
Charlie  PHYS101  C  60

--- Students with score >= 85 ---
Diana    95
Charlie  92
Alice    90
Bob      88

--- Average score per course ---
CS101   : 84.00
CS201   : 84.50
MATH201 : 78.50
PHYS101 : 77.50

--- Top scorer per course ---
CS101   : Alice (90)
CS201   : Bob (88)
MATH201 : Charlie (92)
PHYS101 : Diana (95)
```

---

## Grading Breakdown

| Item | Points |
|------|--------|
| Q1 — Node + push/pop/peek/isEmpty/size | 15 |
| Q1 — toList() correct traversal (no pop) | 10 |
| Q1 — iterator() with own pointer | 10 |
| Q1 — findMin with `? extends E` signature | 10 |
| Q1 — Q1Main correct output | 10 |
| Q2 — ListUtils (a)(b)(c)(d) all correct | 20 |
| Q2 — Student class, file loading with TWR | 10 |
| Q2 — Stream sections 1–4 correct output | 15 |
| **Total** | **100** |

---

*CENG114 Computer Programming II — Spring 2025–2026 · AYBU · Mock Exam 1 Solution*

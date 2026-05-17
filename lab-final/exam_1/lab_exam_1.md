---
Ankara Yıldırım Beyazıt University · Department of Computer Engineering

# CENG114 — Computer Programming II
## Lab Exam (Mock #1)
### Spring 2025–2026

**Instructor:** Yusuf Evren AYKAÇ  
**Assistants:** Çağın ÖZKAYA, Hatice UYSAL, Yusuf Ekrem KEÇİLİOĞLU

**Estimated Duration:** 120 minutes  
**Total Points:** 100 · Q1: 55 pts · Q2: 45 pts

---

## Learning Objectives

By the end of this exam, you will have demonstrated the ability to:

1. Design and implement a **generic singly linked list** with a proper inner `Node<E>` class.
2. Implement `push`, `pop`, `peek`, `size`, `isEmpty`, and a **custom iterator** on the linked structure.
3. Write a **generic bounded method** (`? extends E`) that reads from the list without modifying it.
4. Build a **wildcard-aware utility class** (`ListUtils`) using all three wildcard forms (PECS).
5. Use `Comparator.comparing` / `thenComparing` together with **lambda expressions** to sort and filter data.
6. Produce a formatted **summary report** using the Stream API (`filter`, `map`, `sorted`, `collect`, `groupingBy`).

---

## Setup

1. Create a project folder named `mock_exam_1` and place all `.java` files inside it.
2. Create a sub-folder `mock_exam_1/data/` — you will create two input text files there (described in Q2).
3. Compile and run from the `mock_exam_1` folder so that relative paths like `data/courses.txt` resolve correctly.
4. Each question has **its own driver class** with a `main` method (`Q1Main.java`, `Q2Main.java`).
5. Do **not** use any IDE auto-generated code for the linked structure — write every pointer manipulation yourself.

---

## Allowed Imports

```
java.util.Iterator
java.util.NoSuchElementException
java.util.List
java.util.ArrayList
java.util.Comparator
java.util.stream.Collectors
java.io.BufferedReader
java.io.FileReader
java.io.IOException
```

**Forbidden:** `java.util.LinkedList`, `java.util.Stack`, `java.util.ArrayDeque`, `java.util.Queue`, any `java.util.stream.Stream` construction other than `.stream()` on a `List`.

---

## Question 1 — GenericOrderQueue (55 points)

### Scenario

The university's course enrollment system needs a **generic ordered queue** that works like a stack (LIFO) for draft processing but also supports reading all drafts in order. You are to build this structure from scratch using a linked list internally.

### Class Structure

Implement the following in `mock_exam_1/`:

```
GenericOrderQueue<E>
├── private static class Node<E>
│       E data
│       Node<E> next
│       Node(E data)
├── private Node<E> top          // newest item
├── private int size
├── + push(E item) : void
├── + pop() : E                  // throws NoSuchElementException
├── + peek() : E                 // throws NoSuchElementException
├── + isEmpty() : boolean
├── + size() : int
├── + toList() : List<E>         // top → bottom order, does NOT modify structure
└── + iterator() : Iterator<E>   // iterates top → bottom

(implements Iterable<E>)
```

#### Required Behaviour

**`push(E item)`**
- Creates a new `Node<E>` and prepends it as the new `top`.

**`pop()`**
- Removes and returns `top.data`. Throws `NoSuchElementException` with the message `"Queue is empty"` if empty.

**`peek()`**
- Returns `top.data` without removing. Same exception as `pop()`.

**`toList()`**
- Traverses from `top` to the last node and returns a `List<E>` — top element at index 0. Does **not** call `pop()`.

**`iterator()`**
- Returns an `Iterator<E>` that traverses top → bottom.
- Must implement `hasNext()` and `next()` (throwing `NoSuchElementException` when exhausted).
- `remove()` is optional — you may leave it as `throw new UnsupportedOperationException()`.

#### Generic Bounded Static Method (part of this class)

```java
public static <E extends Comparable<E>> E findMin(GenericOrderQueue<? extends E> queue);
```

- Iterates through all elements using `iterator()` and returns the **minimum** value (using `compareTo`).
- Returns `null` if the queue is empty.
- Must **not** modify the queue.

#### Example Run

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

#### Pitfalls

- `top` is `null` initially — always check `isEmpty()` before accessing `top.data`.
- `toList()` must **traverse** without popping — use a temporary `curr` pointer.
- Your `Node<E>` should be a **static nested class** (not inner class) to avoid holding a reference to the outer instance for each node.

---

## Question 2 — ListUtils & Enrollment Report (45 points)

### Scenario

DataLab Analytics has a CSV file `data/courses.txt` listing enrolled students:

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

Each line: `courseCode, studentName, letterGrade, numericScore`

### Part A — ListUtils Wildcard Utility (20 pts)

Create `ListUtils.java` with **only static methods**:

```java
// (a) ? extends Number — reads scores, computes average
static double average(List<? extends Number> scores);

// (b) ? super String — writes into destination
static void collectNames(List<? extends String> source, List<? super String> destination);

// (c) ? (unbounded) — prints any list, one element per line
static void printAll(List<?> items);

// (d) PECS combined — copy elements above threshold from source to destination
static void collectAbove(List<? extends Number> source,
                         List<? super Number> destination,
                         double threshold);
```

Requirements:
- `average`: return `0.0` for empty list.
- `collectNames`: append all source elements to destination (do not clear destination first).
- `collectAbove`: add only elements with `n.doubleValue() > threshold`.

### Part B — Enrollment Report with Streams (25 pts)

Create `Q2Main.java` with a `main` method that:

1. **Reads** `data/courses.txt` using `BufferedReader` + `try-with-resources`. Parse each line into a `Student` record (see below).
2. **Builds** a `List<Student>` in memory.
3. **Prints** the following report sections using the Stream API (one terminal operation per section):

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
Diana   95
Charlie 92
Alice   90
Bob     88

--- Average score per course ---
CS101   : 84.00
CS201   : 84.50
MATH201 : 78.50
PHYS101 : 77.50

--- Top scorer per course ---
CS101   : Alice (90)
CS201   : Diana (95... wait — Diana is PHYS101. Bob CS201=88, Diana CS201=81)
CS201   : Bob (88)
MATH201 : Charlie (92)
PHYS101 : Diana (95)
```

> **Note:** Implement `Student` as a plain class with fields `courseCode`, `name`, `letterGrade`, `score` and appropriate getters.

#### Required Stream Operations

| Section | Required operations |
|---------|-------------------|
| All students sorted | `.sorted(Comparator…)` + `.collect(toList())` |
| Score ≥ 85 | `.filter(…)` + `.sorted(…)` + `.forEach(…)` |
| Average per course | `.collect(Collectors.groupingBy(…, Collectors.averagingInt(…)))` |
| Top scorer per course | `.collect(Collectors.groupingBy(…))` then per-group max |

#### ListUtils Usage

- Use `ListUtils.average(scores)` to compute the average for Part B §3 (collect scores per course into `List<Integer>` then call `average`).
- Use `ListUtils.collectAbove(allScores, highScores, 84)` to fill the "score ≥ 85" list before printing.

---

## Hints & Common Pitfalls

1. **Iterator discipline:** In `GenericOrderQueue.iterator()`, the iterator must hold its own `curr` pointer — do not share state with the queue's `top` field.
2. **Generic method vs wildcard:** `<E extends Comparable<E>> E findMin(GenericOrderQueue<? extends E> queue)` — the wildcard means you can pass `GenericOrderQueue<String>` or `GenericOrderQueue<Integer>`; inside, elements are read as `E`.
3. **Collectors.groupingBy + downstream:** `Collectors.groupingBy(Student::getCourseCode, Collectors.averagingInt(Student::getScore))` returns `Map<String, Double>`.
4. **Comparator.comparing chain:** `.sorted(Comparator.comparingInt(Student::getScore).reversed().thenComparing(Student::getName))`
5. **try-with-resources:** Do not close the `BufferedReader` manually — the TWR block handles it.
6. **Null pointer in findMin:** If the queue is empty, `iterator().hasNext()` returns false — initialise `min = null` and return it.

---

*Ankara Yıldırım Beyazıt University · Department of Computer Engineering · Page 1 of 1*

# CENG114 Final Exam — Study Guide

> **Exam scope:** Cumulative (whole term), weighted toward post-midterm topics.  
> **Format:** 25 multiple-choice questions, 5 options (A–E), paper-pencil.

---

## 1. One-Day Timeline

| Time Block | What to Do |
|------------|------------|
| **Morning (2 h)** | Read §3 cheat-sheet below top to bottom. Re-read any row that surprises you. |
| **Mid-day (1.5 h)** | Take `mock_final_1.md` under 90-minute exam conditions (no notes, no IDE). |
| **Afternoon (1 h)** | Grade with `mock_final_1_solution.md`. For every wrong answer read the matching topic file. |
| **Evening (45 min)** | Re-read §4 (Instructor Traps) and §5 (must-memorize facts) once more. |

---

## 2. Topic Priority Table

| Priority | Topic | Expected Question Count | Key File |
|----------|-------|------------------------|----------|
| **HIGH** | Exception Handling & Text I/O | 3 | [`topics/02_exception_io_event.md`](topics/02_exception_io_event.md) |
| **HIGH** | Generics & Wildcards / PECS | 3 | [`topics/03_generics_collections.md`](topics/03_generics_collections.md) |
| **HIGH** | Linked Lists | 4 | [`topics/04_linked_lists.md`](topics/04_linked_lists.md) |
| **HIGH** | Lambda & Streams | 3 | [`topics/06_lambda_streams.md`](topics/06_lambda_streams.md) |
| **MED** | Sorting & Searching | 2 | [`topics/01_sorting_quicksort.md`](topics/01_sorting_quicksort.md) |
| **MED** | Stacks & Queues | 2 | [`topics/05_stacks_queues.md`](topics/05_stacks_queues.md) |
| **MED** | Inheritance & Polymorphism | 2 | [`topics/07_inheritance_polymorphism.md`](topics/07_inheritance_polymorphism.md) |
| **MED** | Abstract Classes & Interfaces | 2 | [`topics/08_abstract_interfaces.md`](topics/08_abstract_interfaces.md) |
| **LOW** | Recursion | 2 | [`topics/09_recursion.md`](topics/09_recursion.md) |
| **LOW** | OOP / Encapsulation | 1 | [`topics/10_oop_encapsulation.md`](topics/10_oop_encapsulation.md) |
| **LOW** | Event-Driven Programming | 1 | `02_exception_io_event.md §6` |

---

## 3. One-Glance Cheat Facts

### 3.1 Sorting Complexity & Properties

| Algorithm | Best | Average | Worst | In-Place? | Stable? |
|-----------|------|---------|-------|-----------|---------|
| Bubble Sort | O(n) | O(n²) | O(n²) | ✓ | ✓ |
| Selection Sort | O(n²) | O(n²) | O(n²) | ✓ | **✗** |
| Insertion Sort | O(n) | O(n²) | O(n²) | ✓ | ✓ |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | **✗** | ✓ |
| **Quick Sort** | O(n log n) | O(n log n) | **O(n²)** | ✓ | **✗** |

> Quick Sort worst case = already sorted array with Lomuto (last-element) pivot.  
> Only Merge Sort is stable AND O(n log n) worst-case, but uses O(n) extra space.

---

### 3.2 ArrayList vs LinkedList Complexity

| Operation | ArrayList | SLL (no tail) | DLL (with tail) |
|-----------|-----------|---------------|-----------------|
| `get(i)` | **O(1)** | O(n) | O(n) |
| `add` — front | O(n) | **O(1)** | **O(1)** |
| `add` — back | O(1) amortized | O(n) | **O(1)** |
| `remove` — front | O(n) | **O(1)** | **O(1)** |
| `remove` — back | O(1) | O(n) | **O(1)** |
| `contains` | O(n) | O(n) | O(n) |

> `get(i)` in a loop over a LinkedList = **O(n²) trap** — always prefer iterator or stream.

---

### 3.3 Exception Hierarchy (must memorise the split)

```
Throwable
├── Error              ← never catch (JVM-level: OutOfMemoryError, StackOverflowError)
└── Exception
    ├── IOException            ← CHECKED (FileNotFoundException ⊂ IOException)
    ├── SQLException           ← CHECKED
    └── RuntimeException       ← UNCHECKED
        ├── NullPointerException
        ├── ArrayIndexOutOfBoundsException
        ├── ClassCastException
        ├── NumberFormatException
        └── IllegalArgumentException
```

**Rule:** `RuntimeException` and all its subclasses → **unchecked**. Everything else under `Exception` → **checked**.

**`finally` always runs** — except `System.exit()` or JVM crash.  
**TWR close order** = reverse of open order (last opened, first closed).

---

### 3.4 PECS — Producer Extends, Consumer Super

```
Reading  from a list  →  Producer  →  <? extends T>
Writing  to   a list  →  Consumer  →  <? super T>
```

| Wildcard | Can read as | Can write? |
|----------|-------------|------------|
| `<? extends Number>` | `Number` | ✗ (compiler blocks add) |
| `<? super Integer>` | `Object` only | ✓ (Integer and subtypes) |
| `<?>` | `Object` only | ✗ (except null) |

**Type erasure — 4 things you cannot do with `T`:**

1. `new T()` — constructor unknown at runtime  
2. `new T[n]` — generic array creation  
3. `o instanceof T` — type info gone at runtime  
4. `static T field` — shared across all parameterisations  

---

### 3.5 Built-in Functional Interfaces

| Interface | Method Signature | Use as |
|-----------|-----------------|--------|
| `Runnable` | `void run()` | no-arg, no-return action |
| `Supplier<T>` | `T get()` | factory / lazy getter |
| `Consumer<T>` | `void accept(T t)` | `forEach` action |
| `Function<T,R>` | `R apply(T t)` | `map` transformation |
| `Predicate<T>` | `boolean test(T t)` | `filter` condition |
| `BiFunction<T,U,R>` | `R apply(T t, U u)` | two-input transform |
| `Comparator<T>` | `int compare(T a, T b)` | `sorted` / `sort` |

> A **functional interface** has exactly **one** abstract method. `default` and `static` methods don't count.

---

### 3.6 Stream Pipeline Skeleton

```
source.stream()
  → filter(Predicate)        // intermediate (lazy)
  → map(Function)            // intermediate (lazy)
  → sorted(Comparator)       // intermediate (lazy)
  → collect / forEach / ...  // terminal (triggers execution)
```

A stream can only be consumed **once** — second terminal op throws `IllegalStateException`.

---

### 3.7 Stack vs Queue One-Liner

| | Stack | Queue |
|-|-------|-------|
| Principle | LIFO | FIFO |
| Add | `push` (top) | `enqueue` (rear) |
| Remove | `pop` (top) | `dequeue` (front) |
| Applications | undo, DFS, call stack | BFS, printer queue, scheduling |
| Java class | `ArrayDeque` (`push`/`pop`) | `ArrayDeque` (`offer`/`poll`) |

---

## 4. Top Instructor Traps (seen in past exams & quizzes)

| # | Trap | What to watch for |
|---|------|-------------------|
| 1 | **`finally` + `return`** | `return` inside `catch` still triggers `finally`; output is always `AB`, not just `A` |
| 2 | **Most-general `catch` first** | `catch (Exception e)` before `catch (IOException e)` → compile error (unreachable) |
| 3 | **`throw` vs `throws`** | `throw` executes; `throws` declares. Using one where the other is expected = compile error |
| 4 | **`FileWriter` without `true`** | `new FileWriter("log.txt")` truncates the file; append needs `new FileWriter("log.txt", true)` |
| 5 | **Pointer order in linked list insert** | Must do `newNode.next = curr.next` **before** `curr.next = newNode`; reversed order silently breaks the chain |
| 6 | **Stale `tail` after `dequeue`** | When the last node is removed from a Queue, set `tail = null` too, or you have a dangling pointer |
| 7 | **`get(i)` in a for-i loop on LinkedList** | Each `get(i)` is O(n) → the loop is O(n²). Use iterator or stream. |
| 8 | **`List<Integer>` ≠ `List<Number>`** | Generics are invariant. Assignment fails at compile time. Use `<? extends Number>` for read-only. |
| 9 | **PECS direction** | "I'm reading numbers → use `extends`." Flipping to `super` blocks reads. |
| 10 | **Lambda captures non-final variable** | Variable used in a lambda must be `effectively final`. Mutating it after → compile error. |
| 11 | **Dynamic dispatch vs static type** | Method called is determined by the **runtime** type, not the reference type (`Animal a = new Dog(); a.sound()` → Dog's version). |
| 12 | **`==` vs `.equals()` on Strings** | `==` checks reference; `.equals()` checks content. Exam snippets often swap them. |
| 13 | **TWR close order** | Declared top-to-bottom, closed **bottom-to-top** (LIFO). |
| 14 | **`sc.nextInt()` + `sc.nextLine()`** | `nextInt()` leaves `\n` in buffer; the immediately following `nextLine()` returns `""`. Fix: add extra `sc.nextLine()`. |
| 15 | **Quick Sort worst case** | Sorted array with Lomuto (last-element) pivot → O(n²), not O(n log n). |

---

## 5. Must-Memorise Mini-Facts

- `Merge Sort` is the only O(n log n) **worst-case** sort that is also **stable**.
- `Selection Sort` is **never** stable (swaps non-adjacent elements over long distances).
- `insertionSort` beats Quick Sort on nearly-sorted arrays.
- Binary Search requires a **sorted** array; complexity O(log n).
- A recursive method **must** have a base case or it causes `StackOverflowError`.
- `abstract class` can have a constructor (used by subclass via `super()`).
- An interface method is implicitly `public abstract`; fields are `public static final`.
- A class can `implement` multiple interfaces but `extend` only one class.
- `NumberFormatException` ← `RuntimeException` → **unchecked**.
- `IOException` ← `Exception` (not RuntimeException) → **checked**.
- `AutoCloseable` is the interface required for try-with-resources.
- Stream `.sorted()` with no args uses natural order (elements must implement `Comparable`).
- `Collectors.joining(", ")` produces a single concatenated `String`.
- `Optional<T>` is returned by `findFirst()`, `min()`, `max()` — call `.get()` or `.orElse()`.

---

## 6. How to Use the Mock Exam

1. Print or open `mock_final_1.md` — **no scrolling back** to notes.
2. Set a 90-minute timer.
3. Answer all 25 questions in order; mark uncertain ones with `?`.
4. After time is up, open `mock_final_1_solution.md`.
5. Score: 1 point each. Target ≥ 20/25.
6. For each wrong answer, read the matching topic file section listed in the solution.
7. Re-do the questions you got wrong from memory the next morning.

---

*Good luck — you've got this.*

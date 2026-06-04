# CENG114 — Mock Final Exam 1 · Answer Key

**Scoring:** 1 point per question · 25 points total · Target ≥ 20/25  
**Grading legend:** ✅ correct option · ❌ plausible distractor

---

## Quick Answer Grid

| Q | Answer | Topic |
|---|--------|-------|
| 1 | **C** | Sorting |
| 2 | **D** | Sorting |
| 3 | **C** | Recursion |
| 4 | **C** | Recursion |
| 5 | **C** | OOP / Encapsulation |
| 6 | **B** | Polymorphism |
| 7 | **B** | Polymorphism |
| 8 | **C** | Abstract Classes |
| 9 | **B** | Interfaces |
| 10 | **B** | Exception Handling |
| 11 | **D** | Exception Handling |
| 12 | **B** | Text I/O / TWR |
| 13 | **D** | Generics / Type Erasure |
| 14 | **C** | Generics / PECS |
| 15 | **C** | Generics / Invariance |
| 16 | **C** | Linked Lists |
| 17 | **B** | Linked Lists |
| 18 | **A** | Linked Lists |
| 19 | **B** | Linked Lists |
| 20 | **C** | Stacks |
| 21 | **B** | Stacks |
| 22 | **C** | Streams |
| 23 | **D** | Lambda |
| 24 | **B** | Streams |
| 25 | **D** | Event-Driven |

---

## Detailed Explanations

---

### Q1 — Answer: C · O(n²)

**Topic:** Sorting — `01_sorting_quicksort.md §3.5`

Lomuto partition always picks the **last element** as pivot. On an already-sorted array `[1, 2, 3, … n]`, the pivot is always the largest element, so every partition produces a sub-array of size 0 on the left and n−1 on the right. The recurrence is T(n) = T(n−1) + O(n), which solves to **O(n²)**.

> **Trap:** Many students know QuickSort is "O(n log n) on average" and pick B. The average case is O(n log n), but the worst case is O(n²) and this question asks about worst case.

---

### Q2 — Answer: D · Insertion Sort

**Topic:** Sorting — `01_sorting_quicksort.md §2.1`

- **Stable:** equal elements keep their relative order. Insertion Sort is stable (only shifts elements that are strictly greater).
- **In-place:** needs only O(1) extra space. Insertion Sort sorts within the original array.
- Quick Sort: in-place ✓, stable ✗ (pivot swaps can reorder equals).
- Merge Sort: stable ✓, in-place ✗ (needs O(n) auxiliary array).
- Selection Sort: in-place ✓, stable ✗.

---

### Q3 — Answer: C · 8

**Topic:** Recursion — `slides/18slide_accessible.md`

`f` computes the Fibonacci sequence: f(0)=0, f(1)=1, f(n)=f(n−1)+f(n−2).

| n | f(n) |
|---|------|
| 0 | 0 |
| 1 | 1 |
| 2 | 1 |
| 3 | 2 |
| 4 | 3 |
| 5 | 5 |
| **6** | **8** |

> **Trap:** Option A (6) tempts students who forget that Fibonacci is 0-indexed — f(6) is the 7th Fibonacci number, which is 8, not 6.

---

### Q4 — Answer: C · Both Line 1 and Line 2

**Topic:** Recursion — `slides/18slide_accessible.md`

A **base case** is any condition that stops recursion without making a recursive call.

- **Line 1** (`if (lo > hi) return -1`) — terminates when the search range is empty (key not found).
- **Line 2** (`if (a[mid] == key) return mid`) — terminates when the key is found.

Both stop the recursion, so both are base cases.

---

### Q5 — Answer: C · Encapsulation

**Topic:** OOP — slides Ch. 9–10

**Encapsulation** means bundling data (fields) with the methods that operate on it and restricting direct access to fields via access modifiers. Declaring `grade` and `name` as `public` exposes internal state directly — any caller can modify them without any validation (`student.grade = -999` is legal). Encapsulation demands `private` fields + public getters/setters.

---

### Q6 — Answer: B · `Woof`

**Topic:** Polymorphism — slides Ch. 11

`a` has static type `Animal` but runtime type `Dog`. In Java, **non-static methods are dispatched dynamically** (at runtime) based on the actual object. When `a.speak()` calls `sound()`, the JVM finds `Dog.sound()` and returns `"Woof"`. This is the core of polymorphism / dynamic binding.

> **Trap:** Option A (`...`) is wrong because the `sound()` call inside `speak()` dispatches to `Dog`'s version, not `Animal`'s.

---

### Q7 — Answer: B · `Area: 12.56`

**Topic:** Polymorphism — slides Ch. 11

`s` has runtime type `Circle` with `r = 2`. `describe()` is defined in `Shape` and calls `area()` — which is overridden in `Circle`. Dynamic dispatch resolves to `Circle.area()` = 3.14 × 2² = **12.56**.

> **Trap:** Option A (`Area: 0.0`) is the result if dynamic dispatch were **not** used — but it is, so `Shape.area()` is never called.

---

### Q8 — Answer: C · Can have both abstract and concrete methods

**Topic:** Abstract Classes — slides Ch. 13

- Abstract classes **do** have constructors (called via `super()` in subclass constructors).
- They cannot be instantiated directly (`new AbstractClass()` is a compile error).
- A class can extend **only one** abstract class (Java has single inheritance).
- Abstract methods have **no body** — they end with `;`.

---

### Q9 — Answer: B · Compile error

**Topic:** Interfaces — slides Ch. 13

`Square` claims to `implement Drawable` but does not provide a body for `resize(double factor)`. The compiler requires all abstract interface methods to be implemented unless `Square` is declared `abstract`. Since `Square` is concrete, this is a **compile error**.

> **Trap:** Option E says `Square` becomes implicitly abstract — Java does NOT do this. You get a hard compile error.

---

### Q10 — Answer: B · `AB`

**Topic:** Exception Handling — `02_exception_io_event.md §3.3`

Execution trace:
1. `Integer.parseInt("abc")` throws `NumberFormatException`.
2. `catch` block prints `"A"`, then executes `return`.
3. **Before the return completes**, the `finally` block runs and prints `"B"`.
4. The `return` then executes — control leaves the method.
5. `"C"` is never reached.

> **Key rule:** `finally` runs even when `catch` contains a `return`. Only `System.exit()` or a JVM crash skips `finally`.

---

### Q11 — Answer: D · `IOException`

**Topic:** Exception Handling — `02_exception_io_event.md §3.1–3.2`

`IOException` extends `Exception` (not `RuntimeException`), so it is **checked** — the compiler forces you to either `catch` it or declare `throws IOException`.

All other options are subclasses of `RuntimeException` → unchecked:
- `NullPointerException`, `ArrayIndexOutOfBoundsException`, `ClassCastException`, `IllegalArgumentException`.

---

### Q12 — Answer: B · `pw` is closed first, then `br`

**Topic:** Text I/O / try-with-resources — `02_exception_io_event.md §3.5 / §4.3`

In a try-with-resources, resources are closed in **reverse declaration order** (LIFO). `br` is declared first, `pw` second. Therefore `pw` is closed first, then `br`. This mirrors stack semantics and prevents resource leaks.

> **Why it matters:** `pw` (a `PrintWriter` wrapping a `FileWriter`) must be closed to flush its buffer to disk. Closing `br` first would leave output unsaved if an error occurred.

---

### Q13 — Answer: D · All four lines cause compile errors

**Topic:** Generics / Type Erasure — `03_generics_collections.md §2.4`

All four are prohibited by **type erasure** — generic type information is erased at runtime:

| Line | Operation | Reason it fails |
|------|-----------|-----------------|
| A | `new T()` | Which constructor? Unknown at runtime. |
| B | `new T[5]` | Generic array creation not allowed. |
| C | `static T sharedItem` | Static context is shared; `T` is per-instance. |
| D | `o instanceof T` | `T` is erased; can't check type at runtime. |

---

### Q14 — Answer: C · `<? extends Number>`

**Topic:** Generics / PECS — `03_generics_collections.md §3.1–3.2`

The method **reads** from the list (producer) → PECS says use **extends**.  
`List<? extends Number>` accepts `List<Integer>`, `List<Double>`, `List<Float>` because all are subtypes of `Number`.

- Option A (`<Number>`) would reject `List<Integer>` — generics are **invariant**.
- Option B (`<? super Number>`) is for writing, not reading.
- Option D (`<?>`) would work structurally but the for-each loop `for (Number n : ...)` would fail to compile because `<?>` gives only `Object`.

---

### Q15 — Answer: C · Compile error at Line X

**Topic:** Generics / Invariance — `03_generics_collections.md §3.3`

Java generics are **invariant**: `List<Integer>` is **not** a subtype of `List<Number>`, even though `Integer` is a subtype of `Number`. The assignment `List<Number> nums = ints` fails at compile time (Line X).

> **Why invariance?** If it were allowed, you could then do `nums.add(3.14)` — a `Double` would be stored in `ints` (which guarantees only `Integer`), causing silent heap corruption.

---

### Q16 — Answer: C · O(n)

**Topic:** Linked Lists — `04_linked_lists.md §2.4`

A Singly Linked List has no index; `get(i)` must traverse from `head` one node at a time. In the worst case (i = n−1), this visits all n nodes → **O(n)**.

Compare: `ArrayList.get(i)` uses direct array indexing → **O(1)**.

> **Common trap:** Using `get(i)` inside a `for (int i=0; i<n; i++)` loop on a LinkedList is **O(n²)**. Always use an iterator or enhanced for-loop instead.

---

### Q17 — Answer: B · `[2, 3, 4]`

**Topic:** Linked Lists — `04_linked_lists.md §3.1–3.4`

After the four operations:
```
addFirst(3) → [3]
addFirst(2) → [2, 3]
addFirst(1) → [1, 2, 3]
addLast(4)  → [1, 2, 3, 4]
```
`removeFirst()` removes the head (1). The list becomes `[2, 3, 4]`.

---

### Q18 — Answer: A · Lines A and B must be swapped

**Topic:** Linked Lists — `04_linked_lists.md §7 (Traps)`

The bug is a classic pointer order mistake:

```java
// BUGGY (as written):
curr.next = newNode;          // Line A — curr.next is now newNode
newNode.next = curr.next;     // Line B — curr.next IS newNode → cycle!

// CORRECT:
newNode.next = curr.next;     // save old curr.next into newNode.next
curr.next = newNode;          // only then update curr.next
```

After the buggy version, `newNode.next` points to itself, creating an infinite loop.

---

### Q19 — Answer: B · `addLast(data)`

**Topic:** Linked Lists — `04_linked_lists.md §2.4 / §3.8`

With a `tail` pointer, `addLast` simply:
1. Creates a new node.
2. Sets `tail.next = newNode` and `newNode.prev = tail`.
3. Updates `tail = newNode`.

All constant-time steps → **O(1)**.

Other options:
- A: `get(i)` is O(n) even in a DLL.
- C: Midpoint insertion still requires O(n) traversal to find the midpoint.
- D: `contains` scans the list → O(n).
- E: `reverse` visits every node → O(n).

---

### Q20 — Answer: C · `3 4 2`

**Topic:** Stacks — `05_stacks_queues.md §2.1`

`ArrayDeque.push` adds to the **front** (top), `pop` removes from the **front**.

Step-by-step:
```
push(1) → stack: [1]
push(2) → stack: [2, 1]
push(3) → stack: [3, 2, 1]
pop()   → prints "3 ", stack: [2, 1]
push(4) → stack: [4, 2, 1]
pop()   → prints "4 ", stack: [2, 1]
pop()   → prints "2 ", stack: [1]
```

Output: `3 4 2`

---

### Q21 — Answer: B · Stack (LIFO)

**Topic:** Stacks — `05_stacks_queues.md §5`

The Back button always navigates to the **most recently visited** page — LIFO behaviour. Each page visit is pushed; pressing Back pops.

- Queue (FIFO) would return the oldest visited page — wrong.
- Priority Queue ordered by time is equivalent to a queue here, but adds unnecessary complexity.

---

### Q22 — Answer: C · 20

**Topic:** Streams — `06_lambda_streams.md §7`

Pipeline trace on `[1, 2, 3, 4, 5]`:

1. `filter(n -> n % 2 == 0)` → `[2, 4]`
2. `mapToInt(n -> n * n)` → `[4, 16]`
3. `.sum()` → 4 + 16 = **20**

> **Trap:** Option B (16) only includes 4² — students sometimes apply filter after map.

---

### Q23 — Answer: D · `Function<T, R>`

**Topic:** Lambda / Functional Interfaces — `06_lambda_streams.md §2`

| Interface | Signature |
|-----------|-----------|
| `Predicate<T>` | `boolean test(T t)` |
| `Consumer<T>` | `void accept(T t)` |
| `Supplier<T>` | `T get()` |
| **`Function<T,R>`** | **`R apply(T t)`** |
| `Runnable` | `void run()` |

`Function` takes one argument and returns a value — used in `stream().map(...)`.

---

### Q24 — Answer: B · `[ant, apple]`

**Topic:** Streams — `06_lambda_streams.md §7.4`

Pipeline trace on `["banana", "apple", "cherry", "ant"]`:

1. `filter(s -> s.startsWith("a"))` → `["apple", "ant"]`
2. `.sorted()` — natural (lexicographic) order → `["ant", "apple"]`
3. `.collect(toList())` → `[ant, apple]`

> **Trap:** Option A (`[apple, ant]`) preserves insertion order — but `sorted()` reorders alphabetically, so `ant` < `apple`.

---

### Q25 — Answer: D · The callback code invoked when the event fires

**Topic:** Event-Driven Programming — `02_exception_io_event.md §6.1`

The three roles in event-driven programming:

| Role | What it is |
|------|------------|
| **Event Source** | The component that generates the event (Button, Timer) |
| **Event Object** | The object carrying event metadata (ActionEvent, MouseEvent) |
| **Event Handler** | The callback (lambda / method) that **processes** the event |

Option A describes the Event Source; Option B describes the Event Object.

---

## Score Interpretation

| Score | Meaning |
|-------|---------|
| 23–25 | Excellent — ready for the final |
| 20–22 | Good — review the topics you missed |
| 16–19 | Fair — re-read the topic files for all wrong answers tonight |
| ≤ 15 | Needs work — focus on HIGH priority topics in the Study Guide |

---

*Review topic files in `lab-final/topics/` for any question you got wrong.*

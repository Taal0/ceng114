# Singly Linked Lists in Java — Insertion (Add) Operations  

---

## Table of Contents

1. [From `malloc`/`free` to `new`/GC](#from-mallocfree-to-newgc)
2. [Core insertion idea: 4 steps](#core-insertion-idea-4-steps)
3. [Data structures we will use](#data-structures-we-will-use)
4. [Add at the beginning](#add-at-the-beginning)
5. [Add after a given node (`addAfter`)](#add-after-a-given-node-addafter)
6. [Add at the end (`addEnd`)](#add-at-the-end-addend)
7. [Creating a list from an array](#creating-a-list-from-an-array)
8. [Creating a list from user input (sentinel 0)](#creating-a-list-from-user-input-sentinel-0)
9. [Creating a sorted list from user input](#creating-a-sorted-list-from-user-input)
10. [Common insertion bugs and how to avoid them](#common-insertion-bugs-and-how-to-avoid-them)
11. [Complete reference implementation (educational)](#complete-reference-implementation-educational)

---

## From `malloc`/`free` to `new`/GC

In the C notes, node creation uses a `Getnode()` function that calls `malloc(...)`, and node deletion uses `free(...)`.

In Java:

- **Create a node** with `new Node(item)`.
- **Delete a node** by removing references to it. The garbage collector (GC) will reclaim memory when appropriate.

> You cannot (and should not) manually free memory in Java.

---

## Core insertion idea: 4 steps

Insertion as these steps:

1. Create a new node
2. Assign data to the node
3. Link the new node to the list
4. Link the list to the new node

This is still exactly the right mental model in Java.

---

## Data structures we will use

We keep the node structure identical to the lecture concept:

```java
static class Node {
    int data;
    Node next;

    Node(int data) {
        this.data = data;
        this.next = null;
    }
}
```

We will present insertion in two styles:

1. **Procedural style** (closer to C): methods take `Node head` and return a new head when necessary.
2. **OOP style**: a `SinglyLinkedList` class stores `head` as a field (more idiomatic Java).

Both are useful for learning.

---

## Add at the beginning

### Concept

To add a new node at the **beginning**:

```
newNode.next = head
head = newNode
```

This works for:

- empty list (`head == null`)
- non-empty list

### Procedural style (returns new head)

```java
static Node addBeginning(Node head, int item) {
    Node newNode = new Node(item); // steps 1 + 2
    newNode.next = head;           // step 3
    return newNode;                // step 4: new node becomes the head
}
```

### OOP style (head is a field)

```java
public void addFirst(int item) {
    Node newNode = new Node(item);
    newNode.next = head;
    head = newNode;
}
```

### Why the head “must be returned” in procedural style

If a function does `head = newNode;` inside itself, it changes only the local variable unless you return it.

That’s why the original C code often returns the updated `headp`, and we mimic that in Java procedural style.

---

## Add after a given node (`addAfter`)

### When to use

You have a reference `p` to a node already in the list, and you want to insert after it.

Before:

```
... -> p -> oldNext -> ...
```

After:

```
... -> p -> newNode -> oldNext -> ...
```

### The correct two assignments

```java
newNode.next = p.next;
p.next = newNode;
```

**Order matters.**

If you do `p.next = newNode` first, you lose the original `p.next` link unless you saved it.

### Java implementation

```java
static void addAfter(Node p, int item) {
    if (p == null) {
        throw new IllegalArgumentException("Cannot insert after a null node reference.");
    }
    Node newNode = new Node(item);
    newNode.next = p.next;
    p.next = newNode;
}
```

### Why it works at the end of the list

If `p` is currently the last node, then `p.next` is `null`.

Then:

- `newNode.next = null`
- `p.next = newNode`

So the new node becomes the new last node.

---

## Add at the end (`addEnd`)

### Concept (singly list without a tail pointer)

To insert at the end, you must first find the last node.

- If the list is empty, inserting at end is same as inserting at beginning.
- Otherwise traverse until `p.next == null`.

### Implementation (procedural style)

```java
static Node addEnd(Node head, int item) {
    if (head == null) {
        return addBeginning(null, item);
    }
    Node p = head;
    while (p.next != null) {
        p = p.next;
    }
    addAfter(p, item);
    return head;
}
```

### Complexity

- `addBeginning` is O(1)
- `addAfter` is O(1)
- `addEnd` is O(n) because traversal is required

> This is one major motivation for doubly linked lists or singly linked lists with a `tail` pointer.

---

## Creating a list from an array

The lecture provides two creation strategies:

1. Add first element to beginning, then add the rest to end.
2. Add all elements to beginning (which reverses order).

### 1) Preserve array order

If array is `[5, 8, 4, 3]`, list becomes:

```
5 -> 8 -> 4 -> 3
```

```java
static Node createListFromArray(int[] arr) {
    if (arr == null || arr.length == 0) return null;

    Node head = null;
    head = addBeginning(head, arr[0]);

    Node p = head;
    for (int i = 1; i < arr.length; i++) {
        addAfter(p, arr[i]);
        p = p.next;
    }
    return head;
}
```

### 2) Add everything to the beginning (reverses order)

If array is `[5, 8, 4, 3]`, list becomes:

```
3 -> 4 -> 8 -> 5
```

```java
static Node createListFromArrayReversed(int[] arr) {
    Node head = null;
    if (arr == null) return null;

    for (int x : arr) {
        head = addBeginning(head, x);
    }
    return head;
}
```

---

## Creating a list from user input (sentinel 0)

The lecture uses **0 as a sentinel** value:
- you keep reading integers until the user enters `0`
- `0` is not inserted into the list

### Important note

Using 0 as sentinel means the list cannot contain a legitimate 0 value (unless you change the sentinel rule).

### Java version using `Scanner`

```java
import java.util.Scanner;

static Node readListUntilZero(Scanner sc) {
    Node head = null;
    Node tail = null; // keep tail to make insertion O(1)

    System.out.println("Enter the elements of the list (0 to stop):");
    while (true) {
        int num = sc.nextInt();
        if (num == 0) break;

        Node newNode = new Node(num);
        if (head == null) {
            head = newNode;
            tail = newNode;
        } else {
            tail.next = newNode;
            tail = newNode;
        }
    }
    return head;
}
```

### Why we used `tail` in this input function

That is effectively maintaining a “tail pointer” while reading input.  
This makes each insertion at end O(1), so the whole input build is O(n).

---

## Creating a sorted list from user input

The lecture includes a “sorted insert while reading” exercise:

Rule: keep the list sorted increasingly.

### Strategy (insertion sort on a list)

For each new number `num`:

1. If list is empty → insert as head
2. Else if `num <= head.data` → insert at beginning
3. Else:
   - traverse until you find insertion point:
     - stop when `p.next == null` (end), or
     - `p.next.data >= num` (first node that is not smaller)
   - insert after `p`

### Java implementation

```java
static Node insertSorted(Node head, int num) {
    // Case 1: empty list or insert before head
    if (head == null || num <= head.data) {
        return addBeginning(head, num);
    }

    // Find insertion point (node before where num should go)
    Node p = head;
    while (p.next != null && p.next.data < num) {
        p = p.next;
    }

    // Insert after p
    addAfter(p, num);
    return head;
}

static Node readSortedListUntilZero(Scanner sc) {
    Node head = null;

    System.out.println("Enter the elements of the list (0 to stop):");
    while (true) {
        int num = sc.nextInt();
        if (num == 0) break;

        head = insertSorted(head, num);
    }
    return head;
}
```

### Complexity

- Each insertion may traverse O(n) in worst case.
- For n inputs, total is O(n²) worst case.

This is a great exercise to understand linked list traversal.

---

## Common insertion bugs and how to avoid them

### Bug 1 — Wrong link update order (losing part of the list)

Incorrect:

```java
p.next = newNode;
newNode.next = p.next; // now p.next is already newNode → self-loop risk
```

Correct:

```java
newNode.next = p.next;
p.next = newNode;
```

### Bug 2 — Forgetting to handle empty list

Always check `head == null` when inserting at end, or maintain a tail reference.

### Bug 3 — Using a node reference that is not in the list

If `p` does not belong to the list, insertion corrupts structure logically.  
In educational code, we often assume `p` is valid; in real code you should encapsulate nodes.

---

## Complete reference implementation

Below is a compact but complete educational class (not production-ready, but great for learning).  
It exposes `Node` so you can replicate the lecture’s “pointer p” exercises.

```java
import java.util.Scanner;

public class SinglyLinkedListInsertionDemo {

    public static class Node {
        public int data;
        public Node next;

        public Node(int data) {
            this.data = data;
            this.next = null;
        }
    }

    public static Node addBeginning(Node head, int item) {
        Node newNode = new Node(item);
        newNode.next = head;
        return newNode;
    }

    public static void addAfter(Node p, int item) {
        if (p == null) throw new IllegalArgumentException("p is null");
        Node newNode = new Node(item);
        newNode.next = p.next;
        p.next = newNode;
    }

    public static Node addEnd(Node head, int item) {
        if (head == null) return addBeginning(null, item);
        Node p = head;
        while (p.next != null) p = p.next;
        addAfter(p, item);
        return head;
    }

    public static Node createListFromArray(int[] arr) {
        if (arr == null || arr.length == 0) return null;
        Node head = addBeginning(null, arr[0]);
        Node p = head;
        for (int i = 1; i < arr.length; i++) {
            addAfter(p, arr[i]);
            p = p.next;
        }
        return head;
    }

    public static Node createListFromArrayReversed(int[] arr) {
        Node head = null;
        if (arr == null) return null;
        for (int x : arr) head = addBeginning(head, x);
        return head;
    }

    public static Node insertSorted(Node head, int num) {
        if (head == null || num <= head.data) {
            return addBeginning(head, num);
        }
        Node p = head;
        while (p.next != null && p.next.data < num) p = p.next;
        addAfter(p, num);
        return head;
    }

    public static Node readSortedListUntilZero(Scanner sc) {
        Node head = null;
        while (true) {
            int num = sc.nextInt();
            if (num == 0) break;
            head = insertSorted(head, num);
        }
        return head;
    }

    public static void display(Node head) {
        if (head == null) {
            System.out.println("The List is EMPTY !!!");
            return;
        }
        Node p = head;
        while (p != null) {
            System.out.print(" " + p.data + " ");
            if (p.next != null) System.out.print("-->");
            p = p.next;
        }
        System.out.println();
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.println("Enter integers for a sorted linked list (0 to stop):");
        Node head = readSortedListUntilZero(sc);
        display(head);

        sc.close();
    }
}
```

---

### Next file

Continue with deletion:

- `03_Singly_Linked_Lists_Deletion_in_Java.md`

---

# Singly Linked Lists in Java — Deletion (Remove) Operations  
*A detailed Java translation of the C “delete node” operations.*

---

## Table of Contents

1. [Deletion mindset in Java](#deletion-mindset-in-java)
2. [The 4 deletion steps](#the-4-deletion-steps)
3. [How to return the deleted item in Java](#how-to-return-the-deleted-item-in-java)
4. [Delete after a node (`deleteAfter`)](#delete-after-a-node-deleteafter)
5. [Delete the first node (`deleteFirst`)](#delete-the-first-node-deletefirst)
6. [Delete the last node (`deleteLast`)](#delete-the-last-node-deletelast)
7. [Delete the n-th node (`deleteNth`)](#delete-the-n-th-node-deletenth)
8. [Destroy the entire list (`destroyList` / `clear`)](#destroy-the-entire-list-destroylist--clear)
9. [Edge cases checklist](#edge-cases-checklist)
10. [Complete reference implementation (educational)](#complete-reference-implementation-educational)

---

## Deletion mindset in Java

In C, deletion has two parts:

1. remove the node from the chain of pointers
2. free the memory (`free(del)`)

In Java, deletion still needs part (1), but part (2) becomes:

- remove references so the node is unreachable,
- let the garbage collector reclaim it later.

You may optionally set `del.next = null;` to help prevent accidental misuse and to make the node isolated.

---

## The 4 deletion steps

The lecture lists deletion steps as:

1. Save the address of the node you want to delete
2. Save the data in the node you want to delete
3. Form the new links
4. Delete the node from memory

Java translation:

1. Keep a reference to the node you will remove (`del`)
2. Store its `data` somewhere (return it, or put into a holder)
3. Rewire links (`prev.next = del.next`)
4. Remove references (`del.next = null`) and let GC handle memory

---

## How to return the deleted item in Java

The C code frequently uses `int *item` to return the deleted item to the caller.

Java has no out parameters, so typical Java alternatives are:

1. Return the deleted value (and maybe throw if deletion is impossible)
2. Return `OptionalInt`
3. Return a small result object (value + success flag)
4. Use a mutable “holder” object (closest to C out parameter)

### The “holder” approach (closest to the lecture)

```java
static class IntHolder {
    int value;
}
```

Then the delete function can do:

```java
out.value = del.data;
```

We will use this pattern in the educational translations below, because it maps directly to the lecture.

---

## Supporting Node class

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

---

## Delete after a node (`deleteAfter`)

### When to use

You want to delete the node **after** a known node `p`:

Before:

```
p -> del -> del.next -> ...
```

After:

```
p -> del.next -> ...
```

### Requirements

- `p` must not be `null`
- `p.next` must not be `null` (there must be a node to delete)

### Java implementation

```java
static void deleteAfter(Node p, IntHolder out) {
    if (p == null || p.next == null) {
        throw new IllegalArgumentException("Nothing to delete after the given node.");
    }

    Node del = p.next;      // step 1
    out.value = del.data;   // step 2

    p.next = del.next;      // step 3 (rewire)

    del.next = null;        // optional isolation step for safety
    // step 4: GC will collect del later
}
```

### Note about deleting the last node

Deleting the last node can be done with `deleteAfter(p)` if `p` is the node **before** the last node.  
So a “delete last” operation can traverse to “node before last” and call `deleteAfter`.

---

## Delete the first node (`deleteFirst`)

### Why we need a separate function

`deleteAfter(p)` requires a node **before** the node being deleted.  
But the first node has no previous node.

So we use:

- save head node
- move head to second node
- isolate old head

### Procedural style (returns new head)

```java
static Node deleteFirst(Node head, IntHolder out) {
    if (head == null) {
        throw new IllegalArgumentException("List is empty; nothing to delete.");
    }

    Node del = head;       // step 1
    out.value = del.data;  // step 2

    head = del.next;       // step 3: head moves forward

    del.next = null;       // isolate
    return head;
}
```

---

## Delete the last node (`deleteLast`)

### Cases to handle

1. **Empty list** → cannot delete
2. **Single-node list** → deleting last == deleting first
3. **Multi-node list** → traverse to node before last, then deleteAfter

### Implementation

We will mimic the lecture’s approach (with a dummy value approach also shown).

```java
static final int DUMMY = -987654321;

static Node deleteLast(Node head, IntHolder out) {
    if (head == null) {
        out.value = DUMMY;
        return null;
    }

    // If there is only one node, delete the first
    if (head.next == null) {
        return deleteFirst(head, out);
    }

    // Find the node before the last node
    Node p = head;
    while (p.next.next != null) {
        p = p.next;
    }

    // Delete the last node
    deleteAfter(p, out);
    return head;
}
```

### Why `p.next.next != null`?

If:

- `p.next` is the last node, then `p.next.next` is `null`.
- So the loop stops at the node **before** last.

---

## Delete the n-th node (`deleteNth`)

We define “n-th” using 1-based indexing:

- `n = 1` means first node
- `n = 2` means second node
- etc.

### Strategy

1. If list empty → fail
2. If `n == 1` → deleteFirst
3. Otherwise, traverse to the `(n-1)`-th node, then deleteAfter
4. If list ends before reaching `(n-1)` → fail

### Implementation (lecture-style with DUMMY)

```java
static Node deleteNth(Node head, int n, IntHolder out) {
    if (head == null || n <= 0) {
        out.value = DUMMY;
        return head;
    }

    if (n == 1) {
        return deleteFirst(head, out);
    }

    Node p = head;
    int k = 1;

    // Move p to the (n-1)-th node, if possible
    while (p.next != null && k < n - 1) {
        p = p.next;
        k++;
    }

    // If p.next is null, we never reached a valid (n-1)-th node
    if (p.next == null) {
        out.value = DUMMY;
        return head;
    }

    deleteAfter(p, out);
    return head;
}
```

### Better Java alternative (OptionalInt)

Instead of DUMMY, you can do:

- return `OptionalInt.empty()` if deletion fails
- return `OptionalInt.of(value)` if deletion succeeds

This is more idiomatic.  
However, the holder + DUMMY approach is a direct map of the lecture.

---

## Destroy the entire list (`destroyList` / `clear`)

### C concept

The C version repeatedly frees nodes while advancing the head pointer.

### Java concept

The simplest Java version is:

```java
head = null;
```

Because once nothing references the chain, GC can collect it.

However, for educational clarity (and to prevent accidental “still referenced” nodes), you can explicitly walk and break links:

```java
static Node destroyList(Node head) {
    Node p = head;
    while (p != null) {
        Node next = p.next;
        p.next = null; // isolate
        p = next;
    }
    return null; // new head is null
}
```

---

## Edge cases checklist

For every delete function, ensure you handle:

- **Empty list** (`head == null`)
- **Single-node list** (`head.next == null`)
- **Deleting first** (`n == 1`)
- **Deleting beyond length** (n too large)
- **Deleting after a node when there is no next node**

This is exactly the same discipline used in the original C lecture.

---

## Complete reference implementation (educational)

```java
public class SinglyLinkedListDeletionDemo {

    public static class Node {
        public int data;
        public Node next;
        public Node(int data) { this.data = data; }
    }

    public static class IntHolder {
        public int value;
    }

    public static final int DUMMY = -987654321;

    public static Node addBeginning(Node head, int item) {
        Node n = new Node(item);
        n.next = head;
        return n;
    }

    public static void addAfter(Node p, int item) {
        if (p == null) throw new IllegalArgumentException("p is null");
        Node n = new Node(item);
        n.next = p.next;
        p.next = n;
    }

    public static void deleteAfter(Node p, IntHolder out) {
        if (p == null || p.next == null)
            throw new IllegalArgumentException("Nothing to delete after p");
        Node del = p.next;
        out.value = del.data;
        p.next = del.next;
        del.next = null;
    }

    public static Node deleteFirst(Node head, IntHolder out) {
        if (head == null) throw new IllegalArgumentException("Empty list");
        Node del = head;
        out.value = del.data;
        head = del.next;
        del.next = null;
        return head;
    }

    public static Node deleteLast(Node head, IntHolder out) {
        if (head == null) { out.value = DUMMY; return null; }
        if (head.next == null) return deleteFirst(head, out);

        Node p = head;
        while (p.next.next != null) p = p.next;
        deleteAfter(p, out);
        return head;
    }

    public static Node deleteNth(Node head, int n, IntHolder out) {
        if (head == null || n <= 0) { out.value = DUMMY; return head; }
        if (n == 1) return deleteFirst(head, out);

        Node p = head;
        int k = 1;
        while (p.next != null && k < n - 1) {
            p = p.next;
            k++;
        }
        if (p.next == null) { out.value = DUMMY; return head; }

        deleteAfter(p, out);
        return head;
    }

    public static Node destroyList(Node head) {
        Node p = head;
        while (p != null) {
            Node next = p.next;
            p.next = null;
            p = next;
        }
        return null;
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
        Node head = null;
        head = addBeginning(head, 5);
        addAfter(head, 8);
        addAfter(head.next, 4);

        display(head);

        IntHolder out = new IntHolder();
        head = deleteLast(head, out);
        System.out.println("Deleted: " + out.value);

        display(head);

        head = destroyList(head);
        display(head);
    }
}
```

---

### Next file

Continue with worked examples and additional exercises:

- `04_Singly_Linked_Lists_Examples_and_Exercises_in_Java.md`

---

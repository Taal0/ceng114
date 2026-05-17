# Doubly Linked Lists in Java — Concepts & Core Operations  
*A very detailed Java version of the C doubly linked list lecture material.*

---

## Table of Contents

1. [What is a doubly linked list?](#what-is-a-doubly-linked-list)
2. [Node anatomy: `prev` + `data` + `next`](#node-anatomy-prev--data--next)
3. [Head and tail references (invariants)](#head-and-tail-references-invariants)
4. [C vs Java translation notes](#c-vs-java-translation-notes)
5. [Java node + list class design](#java-node--list-class-design)
6. [Initialization](#initialization)
7. [Insertion operations](#insertion-operations)
   - [Add first node to an empty list](#add-first-node-to-an-empty-list)
   - [Add at the beginning](#add-at-the-beginning)
   - [Add at the end](#add-at-the-end)
   - [Add after a given node](#add-after-a-given-node)
   - [Add before a given node](#add-before-a-given-node)
8. [Creating a list from user input (sentinel 0)](#creating-a-list-from-user-input-sentinel-0)
9. [Deletion operations](#deletion-operations)
   - [Delete first node](#delete-first-node)
   - [Delete last node](#delete-last-node)
   - [Delete the only node (single-node list)](#delete-the-only-node-single-node-list)
   - [Delete a certain node between two nodes](#delete-a-certain-node-between-two-nodes)
10. [Complexity summary](#complexity-summary)
11. [Complete reference implementation (educational)](#complete-reference-implementation-educational)

---

## What is a doubly linked list?

A **doubly linked list (DLL)** is a linked list where each node has:

- a reference to the **next** node
- a reference to the **previous** node

This enables traversal:

- forward from head to tail using `next`
- backward from tail to head using `prev`

Conceptual picture:

```
null <- [prev|data|next] <-> [prev|data|next] <-> [prev|data|next] -> null
        ^                                                  ^
       head                                                tail
```

---

## Node anatomy: `prev` + `data` + `next`

### Java Node class

```java
static class Node {
    int data;
    Node prev;
    Node next;

    Node(int data) {
        this.data = data;
        this.prev = null;
        this.next = null;
    }
}
```

---

## Head and tail references (invariants)

A DLL typically stores two external references:

- `head`: first node
- `tail`: last node

Invariants you must maintain (always true if the list is correct):

1. Empty list:
   - `head == null`
   - `tail == null`

2. Non-empty list:
   - `head.prev == null`
   - `tail.next == null`

3. For any adjacent nodes `A` and `B`:
   - if `A.next == B` then `B.prev == A`

Whenever you insert or delete, you must preserve these invariants.

---

## C vs Java translation notes

### `NULL` vs `null`

- C uses `NULL`
- Java uses `null`

### Pointers vs references

- C: a pointer is an address (can be manipulated)
- Java: a reference points to an object, but you cannot do pointer arithmetic.

### Memory allocation

- C uses `malloc` and `free`
- Java uses `new` and garbage collection.

### Updating head/tail

C lecture functions sometimes use double pointers (`node_t **headp`) because the function must update the caller’s head/tail variables.

In Java, the clean equivalent is:

- store `head` and `tail` as fields of a list object,
- and update them inside instance methods.

---

## Java node + list class design

For clarity and safety, we encapsulate:

- `Node` (internal structure)
- `head` and `tail` fields
- operations as methods

This avoids the “pass head and tail as parameters” complexity.

---

## Initialization

Empty list:

```java
head = null;
tail = null;
```

In a class:

```java
public DoublyLinkedList() {
    head = null;
    tail = null;
}
```

---

## Insertion operations

### General insertion steps (still the same)

1. Create new node
2. Put data into it
3. Link new node to neighbors
4. Link neighbors to new node  
   (More links than singly list!)

---

### Add first node to an empty list

When the list is empty, inserting a node should set **both** head and tail.

```java
public void addFirst(int item) {
    Node n = new Node(item);
    head = n;
    tail = n;
}
```

---

### Add at the beginning

This is for a non-empty list. If you want a unified method, handle empty case inside it.

#### Non-empty case logic

Before:

```
null <- head <-> ...
```

After inserting `n` at beginning:

```
null <- n <-> oldHead <-> ...
```

#### Code

```java
public void addBeginning(int item) {
    Node n = new Node(item);

    if (head == null) {
        // empty list
        head = tail = n;
        return;
    }

    n.next = head;
    head.prev = n;
    head = n;
}
```

---

### Add at the end

Again, handle empty list.

Before:

```
... <-> tail -> null
```

After inserting `n` at end:

```
... <-> oldTail <-> n -> null
```

```java
public void addEnd(int item) {
    Node n = new Node(item);

    if (tail == null) {
        // empty list
        head = tail = n;
        return;
    }

    n.prev = tail;
    tail.next = n;
    tail = n;
}
```

Because we have `tail`, this is O(1) (no traversal required).

---

### Add after a given node

#### Goal

Insert a new node `n` after node `p`.

Cases:

- `p == tail` → adding after tail means “add end”
- otherwise:
  - `n.prev = p`
  - `n.next = p.next`
  - `p.next.prev = n`
  - `p.next = n`

```java
public void addAfter(Node p, int item) {
    if (p == null) throw new IllegalArgumentException("p is null");

    if (p == tail) {
        addEnd(item);
        return;
    }

    Node n = new Node(item);
    n.prev = p;
    n.next = p.next;

    p.next.prev = n;
    p.next = n;
}
```

---

### Add before a given node

Symmetric to add-after.

Cases:

- `p == head` → adding before head means “add beginning”
- otherwise:
  - `n.next = p`
  - `n.prev = p.prev`
  - `p.prev.next = n`
  - `p.prev = n`

```java
public void addBefore(Node p, int item) {
    if (p == null) throw new IllegalArgumentException("p is null");

    if (p == head) {
        addBeginning(item);
        return;
    }

    Node n = new Node(item);
    n.next = p;
    n.prev = p.prev;

    p.prev.next = n;
    p.prev = n;
}
```

---

## Creating a list from user input (sentinel 0)

The lecture shows two ways:

1. Add each new node at the beginning (after the first node is added)
2. Add each new node at the end (after the first node is added)

In Java, we can do both easily.

### Add to beginning each time

```java
public static DoublyLinkedList readByAddingToBeginning(java.util.Scanner sc) {
    DoublyLinkedList list = new DoublyLinkedList();

    System.out.print("Enter a list of integers ending with 0: ");
    int num = sc.nextInt();
    if (num == 0) return list;

    list.addFirst(num);

    while (true) {
        num = sc.nextInt();
        if (num == 0) break;
        list.addBeginning(num);
    }
    return list;
}
```

### Add to end each time

```java
public static DoublyLinkedList readByAddingToEnd(java.util.Scanner sc) {
    DoublyLinkedList list = new DoublyLinkedList();

    System.out.print("Enter a list of integers ending with 0: ");
    int num = sc.nextInt();
    if (num == 0) return list;

    list.addFirst(num);

    while (true) {
        num = sc.nextInt();
        if (num == 0) break;
        list.addEnd(num);
    }
    return list;
}
```

---

## Deletion operations

Deletion requires careful head/tail updates.

The lecture emphasizes that in a single-node DLL, deleting first or deleting last must update **both** head and tail (special case).

---

### Delete first node

Cases:

- empty list → nothing to delete
- single-node list → use deleteOne logic
- otherwise:
  - move head to `head.next`
  - set new head’s `prev` to null
  - isolate old head

```java
public int deleteFirst() {
    if (head == null) throw new IllegalStateException("List is empty.");

    if (head == tail) {
        return deleteOne();
    }

    Node del = head;
    int item = del.data;

    head = head.next;
    head.prev = null;

    del.next = null; // isolate
    return item;
}
```

---

### Delete last node

Cases:

- empty list → nothing
- single-node list → deleteOne
- otherwise:
  - move tail to `tail.prev`
  - set new tail’s `next` to null
  - isolate old tail

```java
public int deleteLast() {
    if (tail == null) throw new IllegalStateException("List is empty.");

    if (head == tail) {
        return deleteOne();
    }

    Node del = tail;
    int item = del.data;

    tail = tail.prev;
    tail.next = null;

    del.prev = null; // isolate
    return item;
}
```

---

### Delete the only node (single-node list)

When `head == tail`, deleting must set both to null.

```java
private int deleteOne() {
    int item = head.data;
    head = null;
    tail = null;
    return item;
}
```

---

### Delete a certain node between two nodes

The C lecture shows “delete a certain node p between two nodes” (i.e., p is not head and not tail).

In Java, we can generalize:

- if p == head → deleteFirst
- if p == tail → deleteLast
- otherwise:
  - `p.prev.next = p.next`
  - `p.next.prev = p.prev`

```java
public int deleteNode(Node p) {
    if (p == null) throw new IllegalArgumentException("p is null");
    if (p == head) return deleteFirst();
    if (p == tail) return deleteLast();

    int item = p.data;

    p.prev.next = p.next;
    p.next.prev = p.prev;

    // isolate
    p.prev = null;
    p.next = null;

    return item;
}
```

---

## Complexity summary

For a DLL with `n` nodes:

| Operation | Time |
|---|---:|
| Insert at beginning | O(1) |
| Insert at end | O(1) |
| Insert after/before a known node | O(1) |
| Delete first/last | O(1) |
| Delete a known node | O(1) |
| Search by value | O(n) |
| Traverse forward/backward | O(n) |

DLL is powerful when you already have node references and want constant-time insert/delete around them.

---

## Complete reference implementation (educational)

```java
import java.util.Scanner;

public class DoublyLinkedList {

    public static class Node {
        public int data;
        public Node prev;
        public Node next;

        public Node(int data) {
            this.data = data;
        }
    }

    private Node head;
    private Node tail;

    public DoublyLinkedList() {
        head = null;
        tail = null;
    }

    public Node getHead() { return head; }
    public Node getTail() { return tail; }

    public boolean isEmpty() { return head == null; }

    public void addFirst(int item) {
        Node n = new Node(item);
        head = tail = n;
    }

    public void addBeginning(int item) {
        Node n = new Node(item);
        if (head == null) {
            head = tail = n;
            return;
        }
        n.next = head;
        head.prev = n;
        head = n;
    }

    public void addEnd(int item) {
        Node n = new Node(item);
        if (tail == null) {
            head = tail = n;
            return;
        }
        n.prev = tail;
        tail.next = n;
        tail = n;
    }

    public void addAfter(Node p, int item) {
        if (p == null) throw new IllegalArgumentException("p is null");
        if (p == tail) {
            addEnd(item);
            return;
        }
        Node n = new Node(item);
        n.prev = p;
        n.next = p.next;
        p.next.prev = n;
        p.next = n;
    }

    public void addBefore(Node p, int item) {
        if (p == null) throw new IllegalArgumentException("p is null");
        if (p == head) {
            addBeginning(item);
            return;
        }
        Node n = new Node(item);
        n.next = p;
        n.prev = p.prev;
        p.prev.next = n;
        p.prev = n;
    }

    private int deleteOne() {
        int item = head.data;
        head = null;
        tail = null;
        return item;
    }

    public int deleteFirst() {
        if (head == null) throw new IllegalStateException("List is empty.");
        if (head == tail) return deleteOne();

        Node del = head;
        int item = del.data;

        head = head.next;
        head.prev = null;

        del.next = null;
        return item;
    }

    public int deleteLast() {
        if (tail == null) throw new IllegalStateException("List is empty.");
        if (head == tail) return deleteOne();

        Node del = tail;
        int item = del.data;

        tail = tail.prev;
        tail.next = null;

        del.prev = null;
        return item;
    }

    public int deleteNode(Node p) {
        if (p == null) throw new IllegalArgumentException("p is null");
        if (p == head) return deleteFirst();
        if (p == tail) return deleteLast();

        int item = p.data;
        p.prev.next = p.next;
        p.next.prev = p.prev;
        p.prev = null;
        p.next = null;
        return item;
    }

    public void displayForward() {
        if (head == null) {
            System.out.println("The List is EMPTY !!!");
            return;
        }
        Node p = head;
        while (p != null) {
            System.out.print(" " + p.data + " ");
            if (p.next != null) System.out.print("<->");
            p = p.next;
        }
        System.out.println();
    }

    public void displayBackward() {
        if (tail == null) {
            System.out.println("The List is EMPTY !!!");
            return;
        }
        Node p = tail;
        while (p != null) {
            System.out.print(" " + p.data + " ");
            if (p.prev != null) System.out.print("<->");
            p = p.prev;
        }
        System.out.println();
    }

    public static DoublyLinkedList readByAddingToEnd(Scanner sc) {
        DoublyLinkedList list = new DoublyLinkedList();

        System.out.print("Enter a list of integers ending with 0: ");
        int num = sc.nextInt();
        if (num == 0) return list;

        list.addFirst(num);

        while (true) {
            num = sc.nextInt();
            if (num == 0) break;
            list.addEnd(num);
        }
        return list;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        DoublyLinkedList list = readByAddingToEnd(sc);
        list.displayForward();
        list.displayBackward();
        sc.close();
    }
}
```

---

### Next file

For additional exercises and problem solutions:

- `06_Doubly_Linked_Lists_Examples_and_Exercises_in_Java.md`

---

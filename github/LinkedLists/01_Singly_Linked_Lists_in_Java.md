# Singly (Forward) Linked Lists in Java  

---

## Table of Contents

1. [What a singly linked list is](#what-a-singly-linked-list-is)
2. [Arrays vs linked lists (memory + performance)](#arrays-vs-linked-lists-memory--performance)
3. [Node anatomy: data + next reference](#node-anatomy-data--next-reference)
4. [C pointers vs Java references: the key translation](#c-pointers-vs-java-references-the-key-translation)
5. [Declaring and initializing an empty list in Java](#declaring-and-initializing-an-empty-list-in-java)
6. [Referring to nodes (equivalent of `p->next->data`)](#referring-to-nodes-equivalent-of-p-next-data)
7. [Traversing a list (display)](#traversing-a-list-display)
8. [Counting nodes (size)](#counting-nodes-size)
9. [Searching for a node (iterative)](#searching-for-a-node-iterative)
10. [Searching recursively](#searching-recursively)
11. [Testing strategy: empty, one-node, multi-node](#testing-strategy-empty-one-node-multi-node)
12. [Complexity summary](#complexity-summary)
13. [Where we go next](#where-we-go-next)

---

## What a singly linked list is

A **singly linked list** (also called a **forward linked list**) is a sequence of nodes where:

- each node stores:
  1. a **data value**
  2. a reference to the **next node**
- the last node’s `next` reference is `null`
- a separate reference called `head` stores the address/reference of the **first node**

Visually (conceptually):

```
head --> [data|next] --> [data|next] --> [data|null]
```

---

## Arrays vs linked lists (memory + performance)

### Arrays (typical Java arrays)

- **Contiguous memory** conceptually (in low-level terms): elements are stored next to each other.
- **Fixed size**: once created, a plain array cannot grow.
- **Fast random access**: `arr[i]` is O(1).

### Linked lists

- **Dynamic growth/shrink**: you allocate a node each time you insert; you remove nodes when you delete.
- Nodes do **not** need to be adjacent in memory (this matters in C; in Java you still get the “not contiguous” behavior conceptually because objects are separate allocations).
- **Slow random access**: to reach the *k-th* element you must traverse from the head through `next` links → O(k).

### Trade-off you should remember

- Use an array when:
  - you know the number of elements in advance (or the array rarely changes),
  - you need fast random access.
- Use a linked list when:
  - the list size changes a lot,
  - frequent inserts/deletes in the middle/front matter,
  - random access speed is not the main requirement.

> In Java, there is also an important *overhead* factor: each node is a separate object with object header + references, so memory usage per element can be significantly higher than arrays.

---

## Node anatomy: data + next reference

### C lecture version (conceptual)

In C, a node is typically:

- `data` field (e.g., `int data`)
- `next` pointer (`struct node_s *next`)

### Java equivalent

In Java, we use a class instead of a `struct`:

```java
static class Node {
    int data;     // payload
    Node next;    // reference to the next node (null means end)

    Node(int data) {
        this.data = data;
        this.next = null;
    }
}
```

Important observations:

- `Node next;` is the Java equivalent of a “pointer to the next node”.
- `null` is the Java equivalent of `NULL`.

---

## C pointers vs Java references: the key translation

This is the most important mental conversion:

| Concept | C (lecture notes) | Java |
|---|---|---|
| Address/pointer to first node | `node_t *headp;` | `Node head;` |
| Empty list | `headp = NULL;` | `head = null;` |
| Move to next node | `p = p->next;` | `p = p.next;` |
| Access node’s data | `p->data` | `p.data` |
| Node creation | `malloc(sizeof(node_t))` | `new Node(item)` |
| Node deletion | `free(node)` | remove references; GC collects later |

### One subtle but critical Java difference: parameter passing

Java is **pass-by-value**. When you pass `head` into a method, the method receives a *copy of the reference*.

- If the method changes `p.next`, that changes the list (because it changes an object).
- If the method assigns `head = head.next`, that changes only the local copy unless:
  - you return the new head, or
  - head is stored as a field of a list object, or
  - you use a wrapper object.

So, in Java you typically design a `LinkedList` class where `head` is a field:

```java
public class SinglyLinkedList {
    private Node head;

    public void addFirst(int x) {
        Node n = new Node(x);
        n.next = head;
        head = n;
    }
}
```

This is the cleanest mapping for “head pointer updates”.

---

## Declaring and initializing an empty list in Java

### Minimal approach

```java
Node head = null; // empty list
```

That’s exactly the same idea as `headp = NULL;` in C.

---

## Referring to nodes (equivalent of `p->next->data`)

In C, you would write:

- `headp->data` for first element’s data
- `headp->next->data` for second element’s data

In Java, the equivalents are:

- `head.data`
- `head.next.data`

### Warning: NullPointerException

If `head` is `null`, then `head.data` will crash with a `NullPointerException`.  
Therefore, always check:

```java
if (head != null) {
    System.out.println(head.data);
}
```

---

## Traversing a list (display)

Traversal means: start at the head and keep following `next` until `null`.

### Java display method (procedural style)

```java
static void displayList(Node head) {
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
```

### Why this works

- `p` is like the “loop index”.
- condition `p != null` means “stop at the end”.

### Common bug: forgetting to advance

If you forget `p = p.next;`, you create an infinite loop.

---

## Counting nodes (size)

To count nodes:

1. start at head
2. increment a counter for each node
3. follow next until `null`

```java
static int sizeList(Node head) {
    int count = 0;
    Node p = head;
    while (p != null) {
        count++;
        p = p.next;
    }
    return count;
}
```

---

## Searching for a node (iterative)

Goal: find the **first node** whose `data == item`. Return the node reference, or `null` if not found.

```java
static Node searchNode(Node head, int item) {
    Node p = head;
    while (p != null && p.data != item) {
        p = p.next;
    }
    return p; // either null (not found) or a node whose data == item
}
```

### Why the order of conditions matters

This is a direct translation of a classic C pitfall discussed in the lecture notes:

- ✅ Safe: `while (p != null && p.data != item)`
- ❌ Unsafe: `while (p.data != item && p != null)`

Because if `p` becomes `null`, then evaluating `p.data` will crash.

> In Java, this is even more visible: the unsafe version throws a `NullPointerException`.

---

## Searching recursively

Recursive search logic:

- Base case 1: empty list (`head == null`) → not found → return `null`
- Base case 2: head matches (`head.data == item`) → return head
- Recursive step: search in the remaining list (`head.next`)

```java
static Node recSearchNode(Node head, int item) {
    if (head == null || head.data == item) {
        return head;
    }
    return recSearchNode(head.next, item);
}
```

### Recursion vs iteration

- recursion is elegant and mirrors the mathematical definition
- but recursion uses stack frames, so it may overflow for very large lists
- iterative version is usually preferred in production Java

---

## Testing strategy: empty, one-node, multi-node

For each operation, explicitly test these cases:

1. **Empty list**
   - `head = null`
   - display, size, search should behave safely.
2. **One-node list**
   - head exists, `head.next = null`
   - insertions and deletions at ends are special here.
3. **Multi-node list**
   - normal behavior and typical traversal.

This matches the lecture advice: “test your linked list functions for empty, one-node, and multi-node lists.”

---

## Complexity summary

For a singly linked list of `n` nodes:

| Operation | Time | Why |
|---|---:|---|
| Traverse / display | O(n) | visit each node |
| Size | O(n) | count nodes |
| Search | O(n) | may scan all nodes |
| Insert at beginning | O(1) | no traversal |
| Insert after a known node | O(1) | pointer changes only |
| Insert at end (without tail pointer) | O(n) | must traverse to last |

---

## Where we go next

This file focused on core structure + traversal + searching.

Next files:

- `02_Singly_Linked_Lists_Insertion_in_Java.md`  
  Insertion cases: beginning, after a node, end, creating lists from arrays and user input, sorted insertion.

- `03_Singly_Linked_Lists_Deletion_in_Java.md`  
  Deleting nodes: delete after a node, delete first, delete last, delete nth, destroy list.

---

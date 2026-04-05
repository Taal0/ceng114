# Doubly Linked Lists in Java — Worked Examples & Exercises  
*A Java adaptation of the DLL examples + extended solutions.*

---

## Table of Contents

1. [Display list forward](#display-list-forward)
2. [Display list in reverse](#display-list-in-reverse)
3. [Count nodes (size) from tail](#count-nodes-size-from-tail)
4. [Count nodes recursively](#count-nodes-recursively)
5. [Search first occurrence](#search-first-occurrence)
6. [Search last occurrence](#search-last-occurrence)
7. [Search in a sorted doubly linked list](#search-in-a-sorted-doubly-linked-list)
8. [Insert before the last node containing `item1`](#insert-before-the-last-node-containing-item1)
9. [Delete the n-th node (full case analysis)](#delete-the-n-th-node-full-case-analysis)
10. [Swap a two-node DLL](#swap-a-two-node-dll)
11. [Extra exercises](#extra-exercises)

---

## Assumed base DLL class

These examples assume you have the `DoublyLinkedList` class (or similar) from the previous file, with:

- `head` and `tail`
- `Node { int data; Node prev; Node next; }`

If your class names differ, only signatures need minor adjustments.

---

## Display list forward

Traverse from head using `next`.

```java
public void displayForward() {
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

---

## Display list in reverse

Traverse from tail using `prev`.

```java
public void displayBackward() {
    if (tail == null) {
        System.out.println("The List is EMPTY !!!");
        return;
    }
    Node p = tail;
    while (p != null) {
        System.out.print(" " + p.data + " ");
        if (p.prev != null) System.out.print("-->");
        p = p.prev;
    }
    System.out.println();
}
```

---

## Count nodes (size) from tail

The lecture mentions you can traverse from head or tail; in DLL you can choose.

```java
public int sizeFromTail() {
    int cnt = 0;
    Node p = tail;
    while (p != null) {
        cnt++;
        p = p.prev;
    }
    return cnt;
}
```

---

## Count nodes recursively

Recursive approach from tail:

- base: tail is null → 0
- else: 1 + recSize(tail.prev)

```java
public int recSizeFromTail(Node tailNode) {
    if (tailNode == null) return 0;
    return 1 + recSizeFromTail(tailNode.prev);
}
```

---

## Search first occurrence

Traverse forward from head.

```java
public Node searchFirst(int item) {
    Node p = head;
    while (p != null && p.data != item) {
        p = p.next;
    }
    return p; // null if not found
}
```

---

## Search last occurrence

Traverse backward from tail.

```java
public Node searchLast(int item) {
    Node p = tail;
    while (p != null && p.data != item) {
        p = p.prev;
    }
    return p;
}
```

### Why the order of conditions matters

Always check `p != null` before `p.data != item`.  
Otherwise Java throws `NullPointerException`.

---

## Search in a sorted doubly linked list

If the list is sorted ascending, we can use boundary checks:

- if list is empty → not found
- if item < head.data → not found
- if item > tail.data → not found

Then traverse from head until `p.data >= item`.

```java
public Node searchSorted(int item) {
    if (head == null) return null;
    if (item < head.data || item > tail.data) return null;

    Node p = head;
    while (p.data < item) {
        p = p.next;
    }

    if (p.data == item) return p;
    return null;
}
```

---

## Insert before the last node containing `item1`

### Problem

Find the **last** node that contains `item1` and insert `item2` **before** it.

Example:

```
8 <-> 5 <-> 8 <-> 2
```

Insert `4` before last `8`:

```
8 <-> 5 <-> 4 <-> 8 <-> 2
```

### Strategy

1. Use `searchLast(item1)` starting from tail.
2. If not found → print message / do nothing.
3. If found node is head → insertion changes head → use `addBeginning(item2)`
4. Otherwise → `addBefore(foundNode, item2)`

### Java method

```java
public void addBeforeLastOccurrence(int item1, int item2) {
    Node p = searchLast(item1);

    if (p == null) {
        System.out.println("There are no " + item1 + "s in the list!");
        return;
    }

    if (p == head) {
        addBeginning(item2);
    } else {
        addBefore(p, item2);
    }
}
```

---

## Delete the n-th node (full case analysis)

This is the most “case-heavy” DLL operation.

### Definition

Use 1-based indexing:

- n = 1 → first node
- n = size → last node

### Cases to handle

1. List empty → nothing
2. n == 1:
   - if single node: deleteOne
   - else deleteFirst
3. n > 1:
   - traverse from head to find n-th node
   - if reach tail before n: not enough nodes
   - if n-th node is tail: deleteLast
   - else deleteNode(p) (middle node)

### Java method (returns deleted value or DUMMY)

```java
public static final int DUMMY = -987654321;

public int deleteNth(int n) {
    if (head == null) {
        System.out.println("The list is empty!");
        return DUMMY;
    }
    if (n <= 0) {
        System.out.println("n must be >= 1");
        return DUMMY;
    }

    // Case: delete first
    if (n == 1) {
        return deleteFirst(); // internally handles single-node case if you wrote it that way
    }

    // Find the n-th node
    Node p = head;
    int k = 1;
    while (p != tail && k < n) {
        p = p.next;
        k++;
    }

    // If we ended at tail
    if (p == tail) {
        if (k == n) {
            return deleteLast();
        } else {
            System.out.println("There are only " + k + " nodes in the list!");
            return DUMMY;
        }
    }

    // Otherwise p is a middle node and k == n
    return deleteNode(p);
}
```

### Optional improvement

Since a DLL supports reverse traversal, you can optimize deletion by starting from head when `n` is small and from tail when `n` is near the end.

---

## Swap a two-node DLL

Assume exactly two nodes: head and tail.

Before:

```
head <-> tail
```

After swap:

```
tail <-> head
```

### Swap nodes physically (rewire prev/next)

```java
public void swapTwoNodes() {
    if (head == null || head.next == null) return; // 0 or 1 node
    if (head.next != tail) {
        throw new IllegalStateException("This swap is defined for exactly 2 nodes.");
    }

    head.next = null;
    head.prev = tail;

    tail.next = head;
    tail.prev = null;

    // swap head/tail references
    Node temp = head;
    head = tail;
    tail = temp;
}
```

### Swap only the data

```java
public void swapTwoNodeData() {
    if (head == null || tail == null) return;
    if (head == tail) return;

    int temp = head.data;
    head.data = tail.data;
    tail.data = temp;
}
```

---

## Extra exercises

1. **Reverse a doubly linked list**
   - Hint: swap each node’s `next` and `prev`, then swap head/tail.
2. **Remove duplicates from a sorted DLL**
   - Hint: compare `p.data` and `p.next.data`, delete `p.next` if equal.
3. **Insert into a sorted DLL**
   - Hint: find first node with data >= item, then addBefore it.
4. **Split a DLL into two halves**
   - Hint: slow/fast pointer, then cut the list by rewiring pointers.
5. **Check DLL integrity**
   - Verify:
     - head.prev == null, tail.next == null
     - for each node: if node.next != null then node.next.prev == node

---

If you want, I can also produce a *fully generic* `DoublyLinkedList<T>` version (using `Comparator<T>` or `T extends Comparable<T>`) for a more advanced Java course.

---

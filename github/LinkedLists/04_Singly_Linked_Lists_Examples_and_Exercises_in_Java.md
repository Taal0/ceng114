# Singly Linked Lists in Java — Worked Examples & Exercises  
*A Java adaptation of the lecture’s linked list examples + additional solutions.*

---

## Table of Contents

1. [Sum of all items (iterative)](#sum-of-all-items-iterative)
2. [Sum of all items (recursive)](#sum-of-all-items-recursive)
3. [Concatenate two lists](#concatenate-two-lists)
4. [Insert `item2` after the first node containing `item1`](#insert-item2-after-the-first-node-containing-item1)
5. [Search in a sorted list (early termination)](#search-in-a-sorted-list-early-termination)
6. [Swap a 2-node list: swap nodes vs swap data](#swap-a-2-node-list-swap-nodes-vs-swap-data)
7. [Reverse a linked list (iterative solution)](#reverse-a-linked-list-iterative-solution)
8. [Reverse a linked list (recursive solution)](#reverse-a-linked-list-recursive-solution)
9. [Extra practice problems (with guidance)](#extra-practice-problems-with-guidance)

---

## Supporting Node definition

We keep the same educational node:

```java
static class Node {
    int data;
    Node next;
    Node(int data) { this.data = data; }
}
```

---

## Sum of all items (iterative)

### Problem

Compute the sum of values in an integer singly linked list.

### Idea

Traverse the list and accumulate into `sum`.

### Java solution

```java
static int sumList(Node head) {
    int sum = 0;
    Node p = head;
    while (p != null) {
        sum += p.data;
        p = p.next;
    }
    return sum;
}
```

### Complexity

- Time: O(n)
- Space: O(1)

---

## Sum of all items (recursive)

### Idea

- Base case: empty list → sum is 0
- Recursive case: sum = head.data + sum(head.next)

### Java solution

```java
static int recSumList(Node head) {
    if (head == null) return 0;
    return head.data + recSumList(head.next);
}
```

### Complexity

- Time: O(n)
- Space: O(n) due to recursion stack

---

## Concatenate two lists

### Problem

Given two lists `head1` and `head2`, attach the second to the end of the first.

### Cases

1. If `head1` is empty → result is `head2`
2. If `head2` is empty → result is `head1`
3. Otherwise:
   - find last node of list1
   - set `last.next = head2`

### Java solution

```java
static Node concatLists(Node head1, Node head2) {
    if (head1 == null) return head2;
    if (head2 == null) return head1;

    Node p = head1;
    while (p.next != null) p = p.next;
    p.next = head2;
    return head1;
}
```

### Important note

This does not copy nodes. After concatenation, both lists share nodes.

---

## Insert `item2` after the first node containing `item1`

### Problem

Find the first node with value `item1`. Insert a new node with value `item2` after it.

### Why we need safety checks

- If the list is empty → search returns `null`
- If `item1` not found → search returns `null`
- If we call `addAfter(null, item2)` we crash

### Java solution

```java
static Node searchNode(Node head, int item) {
    Node p = head;
    while (p != null && p.data != item) p = p.next;
    return p;
}

static void addAfter(Node p, int item) {
    if (p == null) return; // or throw
    Node n = new Node(item);
    n.next = p.next;
    p.next = n;
}

static void addAfterFirstOccurrence(Node head, int item1, int item2) {
    Node p = searchNode(head, item1);
    if (p != null) {
        addAfter(p, item2);
    }
}
```

---

## Search in a sorted list (early termination)

### Problem

If a list is sorted ascending, searching can stop early:

- If you reach a value greater than the target, the target cannot exist after that.

### Algorithm

1. Traverse while `p != null` and `p.data < item`
2. When loop stops:
   - if `p == null` → not found
   - if `p.data > item` → not found
   - else `p.data == item` → found

### Java solution

```java
static Node searchSorted(Node head, int item) {
    Node p = head;
    while (p != null && p.data < item) {
        p = p.next;
    }
    if (p == null || p.data > item) return null;
    return p;
}
```

### Complexity

Worst case still O(n), but faster on average when target is small or absent.

---

## Swap a 2-node list: swap nodes vs swap data

Assume list has exactly two nodes:

```
head -> A -> B -> null
```

After swap:

```
head -> B -> A -> null
```

### Approach 1: Swap the nodes (change pointers)

Steps:

1. `temp` points to second node
2. first node points to null
3. second node points to first node
4. head points to second node

```java
static Node swapTwoNodes(Node head) {
    if (head == null || head.next == null) return head; // not enough nodes
    Node second = head.next;
    head.next = null;
    second.next = head;
    return second;
}
```

### Approach 2: Swap only the data

This is simpler if you do not want to rewire nodes:

```java
static void swapTwoNodeData(Node head) {
    if (head == null || head.next == null) return;
    int temp = head.data;
    head.data = head.next.data;
    head.next.data = temp;
}
```

### Trade-off

- Swapping nodes preserves node identity but rewires links.
- Swapping data is simpler, but if nodes contain complex objects with identity, it may not be appropriate.

---

## Reverse a linked list (iterative solution)

This was listed as a home exercise in the lecture. Here is a complete Java solution.

### Goal

Transform:

```
head -> 5 -> 8 -> 9 -> 4 -> 2 -> null
```

Into:

```
head -> 2 -> 4 -> 9 -> 8 -> 5 -> null
```

### Key idea: three references

- `prev` : previous node (initially null)
- `curr` : current node (starts at head)
- `next` : saved next node so we don’t lose the list

At each step:

1. save next: `next = curr.next`
2. reverse link: `curr.next = prev`
3. advance: `prev = curr; curr = next`

At the end, `prev` becomes the new head.

### Java solution

```java
static Node reverseIterative(Node head) {
    Node prev = null;
    Node curr = head;

    while (curr != null) {
        Node next = curr.next; // save
        curr.next = prev;      // reverse
        prev = curr;           // advance prev
        curr = next;           // advance curr
    }

    return prev; // new head
}
```

### Complexity

- Time: O(n)
- Space: O(1)

---

## Reverse a linked list (recursive solution)

Recursive strategy:

- Reverse the rest of the list
- Attach head at the end

Base cases:

- empty list → return null
- single node → return it (already reversed)

### Java solution

```java
static Node reverseRecursive(Node head) {
    if (head == null || head.next == null) return head;

    Node newHead = reverseRecursive(head.next);

    // head.next is the last node of the reversed sublist
    head.next.next = head;
    head.next = null;

    return newHead;
}
```

### Complexity

- Time: O(n)
- Space: O(n) recursion stack

---

## Extra practice problems (with guidance)

These are additional exercises you can solve using the same building blocks.

### 1) Remove the first occurrence of a value

Hint:

- If head matches → deleteFirst
- else traverse to find the node before the match and deleteAfter

### 2) Remove all occurrences of a value

Hint:

- Use a loop and a “dummy head” technique, or
- handle head separately, then deleteAfter repeatedly

### 3) Find the middle node

Hint:

- Use slow/fast pointers:
  - slow moves 1 step
  - fast moves 2 steps

### 4) Detect a cycle

Hint:

- Floyd’s tortoise and hare algorithm (slow/fast pointers)

### 5) Merge two sorted linked lists

Hint:

- Similar to merge step of merge sort
- Build result list by repeated “take smaller head”

---

## If you want a full assignment pack

If you share your instructor’s constraints (e.g., “no recursion”, “use generics”, “must use `Scanner`”, etc.), I can rewrite these exercises to match.

---

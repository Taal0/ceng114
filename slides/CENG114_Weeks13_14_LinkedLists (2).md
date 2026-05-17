<!-- Slide number: 1 -->

12

47

8

null

5

9

3

WEEKS 13 - 14  •  LECTURE

Linked Lists
in Java
Singly  •  Doubly  •  ArrayList vs LinkedList

CENG114 — Computer Programming II
Öğr. Gör. Yusuf Evren Aykaç  •  Ankara Yıldırım Beyazıt University

### Notes:

<!-- Slide number: 2 -->
TODAY'S LECTURES
Learning Objectives

01
02
Understand linked structures
Implement core operations
Know what singly and doubly linked lists are, and how Java references replace explicit pointers.
Write Java code for insertion, deletion, traversal, and search — and reason about edge cases.

03
04
Compare list implementations
Master DLL-specific power
Choose between ArrayList and LinkedList based on the access pattern of your problem.
Use prev pointers for O(1) backward traversal, delete-before, and reverse iteration.
CENG114 • Linked Lists in Java
2

### Notes:

<!-- Slide number: 3 -->
MOTIVATION
Why Do We Need Linked Lists?

The Array Problem
The Linked List Solution

10

next

20

next

30

next

40

—

10

20

30

40

?

?
null

Fixed capacity — what if we need more?
Grows on demand — link, don't shift
Size must be known in advance
Dynamic size — grows at runtime

Inserting at the front shifts every element
O(1) insert at the front

Wastes memory if over-allocated
Allocates only what's needed

CENG114 • Linked Lists in Java
3

### Notes:

<!-- Slide number: 4 -->
PART 1 — SINGLY LINKED LISTS
What Is a Singly Linked List?

A sequence of nodes where each node stores a value and a reference to the next node. A separate reference called head points to the first node. The last node's next reference is null.
head

12

next

47

next

8

next

25

next
null

first node
last node
head reference
node
null terminator
Stores the address of the first node
Contains data + reference to next
Marks the end of the list
CENG114 • Linked Lists in Java
4

### Notes:

<!-- Slide number: 5 -->
CONCEPT
Anatomy of a Node
Two fields per node:

data
next
data — the value stored in the node
42
next — a reference to the next node, or null if last

Java

class Node {
    int data;
    Node next;

    Node(int data) {
        this.data = data;
    }
}
CENG114 • Linked Lists in Java
5

### Notes:

<!-- Slide number: 6 -->
OPERATION
Traversing the List
Start at the head and follow next references until you reach null.
p

12

next

47

next

8

next

25

next

6

next
null
head

START
VISIT
ADVANCE

1

2

3
Set p = head
Read p.data, do something
Set p = p.next, repeat until null
⚠ Common bug: forgetting p = p.next inside the loop creates an infinite loop.
CENG114 • Linked Lists in Java
6

### Notes:

<!-- Slide number: 7 -->
JAVA CODE
Traversing & Searching

Display — walk head → null
Search — first match or null
void display() {
    Node p = head;
    while (p != null) {
        System.out.print(
            p.data + " ");
        p = p.next;
    }
    System.out.println();
}
Node search(int item) {
    Node p = head;
    while (p != null
           && p.data != item) {
        p = p.next;
    }

    return p;
}

Order matters:  check p != null before p.data != item — otherwise you'll get NullPointerException at the end of the list.
CENG114 • Linked Lists in Java
7

### Notes:

<!-- Slide number: 8 -->
PART  02

Insertion
Adding new nodes to a singly linked list

At the beginning
After a known node
At the end
O(1)
O(1)
O(n)

### Notes:

<!-- Slide number: 9 -->
INSERTION
The 4 Steps of Every Insertion

STEP 1
STEP 2
STEP 3
STEP 4

+

≡

→

←
Create
Fill
Link out
Link in
Create a new node object
Assign the data value to it
Point newNode.next to a list node
Make some list link point to it
The order of steps 3 and 4 matters — link the new node OUT first, then link it IN.
CENG114 • Linked Lists in Java
9

### Notes:

<!-- Slide number: 10 -->
INSERTION
Add at the Beginning  —  O(1)
BEFORE

The Idea

12

next

47

next

8

next

25

next
null
newNode.next = head;
head = newNode;
head

AFTER  —  insert 99 at the front
Why it works:

99

next

12

next

47

next

8

next

25

next
1. newNode.next points to old head
2. head now points to newNode
3. Works even on an empty list (head = null)
null
head

NEW
CENG114 • Linked Lists in Java
10

### Notes:

<!-- Slide number: 11 -->
JAVA CODE
addBeginning(item)

Time Complexity
Method • runs in constant time
O(1)
// Returns the new head after inserting `item` at the front.
Node addBeginning(Node head, int item) {
    Node newNode = new Node(item);

    newNode.next = head;   // link OUT first
    head         = newNode; // then link IN

    return head;
}

Why return head?
Java is pass-by-value. Reassigning head inside the method only changes the LOCAL copy.

The caller must capture the new head:
head = addBeginning(head, 99);

Trick:  this version handles the empty list automatically — when head is null, newNode.next becomes null too. ✓
CENG114 • Linked Lists in Java
11

### Notes:

<!-- Slide number: 12 -->
INSERTION
Add After a Known Node  —  O(1)
BEFORE  —  pointer p points to node 47, insert 99 after it

12

next

47

next

8

next

25

next
null

p
AFTER
NEW

12

next

47

next

99

next

8

next

25

next
null

p

✘  WRONG ORDER
✓  CORRECT ORDER
p.next = newNode;
newNode.next = p.next;
newNode.next = p.next;
p.next = newNode;
Loses the rest of the list — newNode points to itself.
Link OUT before linking IN — chain stays intact.
CENG114 • Linked Lists in Java
12

### Notes:

<!-- Slide number: 13 -->
JAVA CODE
addAfter(p, item)

Time Complexity
Method • O(1) — no traversal
O(1)
// Inserts `item` immediately after node `p`.
// Caller must ensure p is non-null.
void addAfter(Node p, int item) {
    Node newNode = new Node(item);

    newNode.next = p.next;  // link OUT first
    p.next       = newNode; // then link IN
}

Key insight
Once we use p.next to read the OLD next link, we are free to overwrite p.next.

Reversing the two lines loses the rest of the list.

Usage
Node p = search(47);
if (p != null)  addAfter(p, 99); // inserts 99 right after 47
CENG114 • Linked Lists in Java
13

### Notes:

<!-- Slide number: 14 -->
INSERTION
Add at the End  —  O(n)
Without a tail pointer, we must first walk to the last node — then insertAfter.
p

12

next

47

next

8

next

25

next
null
head

walk until  p.next == null

1
2
3
Find tail
Insert after p
Done
Walk from head until p.next is null
Use addAfter(p, item) — O(1)
New node becomes the new tail
💡  Tip:  keeping a tail reference makes addEnd O(1) — exactly what a DLL does.
CENG114 • Linked Lists in Java
14

### Notes:

<!-- Slide number: 15 -->
JAVA CODE
addEnd(head, item)

Time Complexity
Method • O(n) — must traverse to tail
O(n)
// Walks to the last node, then links a new node after it.
Node addEnd(Node head, int item) {
    Node newNode = new Node(item);

    if (head == null) {       // empty list
        return newNode;
    }

    Node p = head;            // find the tail
    while (p.next != null) {
        p = p.next;
    }

    p.next = newNode;         // link in
    return head;
}

Two edge cases
①  Empty list (head == null)
    new node becomes the head.

②  Loop condition p.next != null
    stops AT the last node, not past it — so we can write to p.next.
CENG114 • Linked Lists in Java
15

### Notes:

<!-- Slide number: 16 -->
PART  03

Deletion
Removing nodes — and letting the garbage collector do the cleanup

After a node
First node
Last node
n-th node
O(1)
O(1)
O(n)
O(n)

### Notes:

<!-- Slide number: 17 -->
DELETION
Delete the Node After p  —  O(1)
BEFORE  —  p points to 47, delete the node after it (= 8)

12

next

47

next

8

next

25

next
null

p
DEL
AFTER  —  rewire p.next to skip the deleted node

12

next

47

next

25

next
null

8

—
abandoned →
GC reclaims it

1. del = p.next

2. save del.data

3. p.next = del.next

4. let GC collect del
CENG114 • Linked Lists in Java
17

### Notes:

<!-- Slide number: 18 -->
JAVA CODE
deleteAfter(p)

Time Complexity
Method • O(1) — pure pointer surgery
O(1)
// Removes the node that follows `p` and returns its data.
// Throws if p is the tail (nothing to delete).
int deleteAfter(Node p) {
    if (p == null || p.next == null) {
        throw new IllegalStateException(
            "Nothing to delete after p.");
    }

    Node del   = p.next;       // node to remove
    int  item  = del.data;     // save its value
    p.next     = del.next;     // unlink
    del.next   = null;         // isolate for GC

    return item;
}

Why isolate del?
Setting del.next = null isn't strictly required — once nothing references del, the GC reclaims it.

But isolating prevents accidental access through del afterward — a defensive habit.
CENG114 • Linked Lists in Java
18

### Notes:

<!-- Slide number: 19 -->
DELETION
Delete the First & Last Node

Delete First   —   O(1)
Move head to the second node — the old head becomes unreachable.
head

12

—

47

next

8

next

25

next
1.  del = head      2.  head = head.next      3.  del.next = null
null
head

Delete Last   —   O(n)
Walk to the node before the last (p.next.next == null), then deleteAfter(p).
null

12

next

47

next

8

next

25

—
1.  walk until p.next.next == null
2.  deleteAfter(p)
head

p
CENG114 • Linked Lists in Java
19

### Notes:

<!-- Slide number: 20 -->
JAVA CODE
deleteFirst() & deleteLast()

deleteFirst  •  O(1)
deleteLast  •  O(n)
// Returns the new head after removing the first node.
Node deleteFirst(Node head) {
    if (head == null) {
        throw new IllegalStateException(
            "List is empty.");
    }

    Node del = head;
    head     = head.next;
    del.next = null;        // isolate

    return head;
}
// Walks to the node before the tail, then unlinks the tail.
Node deleteLast(Node head) {
    if (head == null) return null;
    if (head.next == null) return null; // single

    Node p = head;          // walk
    while (p.next.next != null) {
        p = p.next;
    }
    p.next = null;          // drop the tail

    return head;
}

Single-node edge case:  deleteLast on a one-node list returns null — the list becomes empty.
CENG114 • Linked Lists in Java
20

### Notes:

<!-- Slide number: 21 -->
DELETION
Edge Cases You Must Handle
Every deletion routine must defend against three list shapes.

Empty list
Single node
Multi-node

head == null

head.next == null

general case

42

next

12

next

47

next
null
null
head
null
head
head

Action:
Action:
Action:
Refuse — throw an exception, return a sentinel, or do nothing safely.
Deleting last == deleting first. After delete, head becomes null.
Rewire links and (optionally) isolate the removed node.
CENG114 • Linked Lists in Java
21

### Notes:

<!-- Slide number: 22 -->
PART  04

ArrayList vs
LinkedList
How Java's two list implementations actually differ

### Notes:

<!-- Slide number: 23 -->
COMPARISON
Two Different Memory Layouts

ArrayList
dynamic array
LinkedList
doubly linked nodes
Contiguous memory
Scattered memory, linked by references

A

B

C

D

E

A

next

B

next

C

next

D

next
null

Elements stored next to each other
Each node = data + reference to next

Direct index access — O(1) get/set
Random access requires traversal

Insert at front shifts everything
Insert at front is O(1) — no shifting

CENG114 • Linked Lists in Java
23

### Notes:

<!-- Slide number: 24 -->
COMPARISON
Performance — Big-O Side by Side

Operation

ArrayList

LinkedList

Winner

get(index)

O(1)

O(n)

ArrayList

add(element)  at end

O(1) amortized

O(1)

tie

add(0, element)  at front

O(n)

O(1)

LinkedList

add(index, element)  middle

O(n)

O(n)

tie

remove(0)  at front

O(n)

O(1)

LinkedList

contains(element)

O(n)

O(n)

tie

Memory per element

low

high (refs)

ArrayList

Key takeaway:  ArrayList wins on random access. LinkedList wins on front-end insert/remove. Almost everything else is a tie.
CENG114 • Linked Lists in Java
24

### Notes:

<!-- Slide number: 25 -->
COMPARISON
Which One Should You Use?

Need frequent random access?

YES
NO

→  Use ArrayList

Frequent insert/remove at front?

YES
NO

→  LinkedList

→  ArrayList

Rule of thumb
When in doubt, use ArrayList.  Reach for LinkedList only when you specifically need O(1) queue/deque behavior at the ends.
CENG114 • Linked Lists in Java
25

### Notes:

<!-- Slide number: 26 -->
PITFALL
The O(n²) Trap with LinkedList
Iterating a LinkedList with an index loop is silently catastrophic.

✘  DON'T  —  O(n²)
✓  DO  —  O(n)

for (int i = 0; i < list.size(); i++) {
    System.out.println(list.get(i));
}
for (String item : list) {
    System.out.println(item);
}
Why it's slow
Why it's fast
Each list.get(i) walks from head to index i — n calls × O(n) traversal = O(n²) total. 100,000 elements ≈ 10,000,000,000 hops.
The enhanced for-loop uses an internal iterator that holds a reference to the current node — each step is O(1). Total = O(n).
CENG114 • Linked Lists in Java
26

### Notes:

<!-- Slide number: 27 -->
PART  05

Doubly Linked
Lists
Two-way links — every operation gets faster, every node gets larger

### Notes:

<!-- Slide number: 28 -->
PART 5 — DOUBLY LINKED LISTS
What Is a Doubly Linked List?

Each node carries TWO links: one to the previous node (prev) and one to the next node (next). The list keeps both a head (first) and a tail (last). You can traverse in either direction.
head
tail

prev

12

next

prev

47

next

prev

8

next

prev

25

next
null
null

head / tail
prev pointer
Symmetric structure
Two external references — O(1) access to BOTH ends
Walk backward from any node — even from the tail
Most operations have a mirror image (after ↔ before)
CENG114 • Linked Lists in Java
28

### Notes:

<!-- Slide number: 29 -->
CONCEPT
Anatomy of a Doubly-Linked Node
Three fields per node:

prev — reference to previous node (or null)
prev
data
next
data — the value
42
next — reference to next node (or null)

Java

class Node {
    int  data;
    Node prev;
    Node next;

    Node(int data) {
        this.data = data;
    }
}
CENG114 • Linked Lists in Java
29

### Notes:

<!-- Slide number: 30 -->
CONCEPT
DLL Invariants — What Must Always Be True
Every insertion and deletion must preserve these three rules. Break one → the list is corrupt.

RULE  1
RULE  2
RULE  3
Empty list
Boundary nodes
Adjacent nodes

head == null
tail == null

head.prev == null
tail.next == null

if  A.next == B
then  B.prev == A
Both references are null when the list contains no nodes.
The first node has no predecessor; the last node has no successor.
Every forward link is mirrored by a backward link — and vice versa.
CENG114 • Linked Lists in Java
30

### Notes:

<!-- Slide number: 31 -->
DLL INSERTION
Add at the Beginning & at the End  —  O(1)
With both head and tail at hand, both end-insertions are constant time. No traversal needed.

addBeginning(item)
addEnd(item)
BEFORE
BEFORE

prev

5

next

prev

9

next

prev

5

next

prev

9

next

AFTER  —  insert 99
AFTER  —  append 99

prev

99

next

prev

5

next

prev

9

next

prev

5

next

prev

9

next

prev

99

next

n.next = head;
n.prev = tail;

1

1
head.prev = n;
tail.next = n;

2

2
head = n;
tail = n;

3

3
Notice the symmetry:  swap head↔tail, prev↔next, and you get one method from the other.
CENG114 • Linked Lists in Java
31

### Notes:

<!-- Slide number: 32 -->
JAVA CODE
addBeginning(item) & addEnd(item)

addBeginning  •  O(1)
addEnd  •  O(1)
void addBeginning(int item) {
    Node n = new Node(item);

    if (head == null) {     // empty list
        head = tail = n;
        return;
    }

    n.next     = head;      // link OUT
    head.prev  = n;         // back-link
    head       = n;         // promote
}
void addEnd(int item) {
    Node n = new Node(item);

    if (tail == null) {     // empty list
        head = tail = n;
        return;
    }

    n.prev     = tail;      // link OUT
    tail.next  = n;         // forward-link
    tail       = n;         // promote
}

Empty-list trick:  when head and tail are both null, we set BOTH to the new node — every later operation just works.
CENG114 • Linked Lists in Java
32

### Notes:

<!-- Slide number: 33 -->
DLL INSERTION
Add After & Add Before a Known Node  —  O(1)
Both operations rewire 4 links. They are perfect mirrors of one another.

addAfter(p, item)
addBefore(p, item)

prev

5

next

prev

9

next

prev

7

next

prev

5

next

prev

9

next

prev

7

next

p
p

prev

99

next

prev

99

next

NEW
NEW
n.prev = p;
n.next = p.next;
n.next = p;
n.prev = p.prev;

1

2

1

2
p.next.prev = n;
p.next = n;
p.prev.next = n;
p.prev = n;

3

4

3

4
Edge case: p == tail → delegate to addEnd.
Edge case: p == head → delegate to addBeginning.
CENG114 • Linked Lists in Java
33

### Notes:

<!-- Slide number: 34 -->
JAVA CODE
addAfter(p, item) & addBefore(p, item)

addAfter  •  O(1)
addBefore  •  O(1)
void addAfter(Node p, int item) {
    if (p == tail) {
        addEnd(item);
        return;
    }

    Node n = new Node(item);
    n.prev      = p;
    n.next      = p.next;

    p.next.prev = n;   // back-link
    p.next      = n;   // forward-link
}
void addBefore(Node p, int item) {
    if (p == head) {
        addBeginning(item);
        return;
    }

    Node n = new Node(item);
    n.next      = p;
    n.prev      = p.prev;

    p.prev.next = n;   // forward-link
    p.prev      = n;   // back-link
}

Link out before linking in:  set n.prev and n.next FIRST, then update the neighbors. Reverse this order → broken list.
CENG114 • Linked Lists in Java
34

### Notes:

<!-- Slide number: 35 -->
DLL DELETION
Delete First & Delete Last  —  O(1)
Both run in constant time — the tail reference makes deleteLast as cheap as deleteFirst.
deleteFirst  —  drop old head, promote head.next
deleteLast  —  drop old tail, promote tail.prev

prev

5

next

prev

9

next

prev

7

next

prev

5

next

prev

9

next

prev

7

next

new head
new tail

deleteFirst  •  O(1)
deleteLast  •  O(1)
int deleteFirst() {
    if (head == null) throw new
        IllegalStateException("empty");

    if (head == tail) {     // single
        int item = head.data;
        head = tail = null;
        return item;
    }

    Node del   = head;
    int  item  = del.data;
    head       = head.next;
    head.prev  = null;
    return item;
}
int deleteLast() {
    if (tail == null) throw new
        IllegalStateException("empty");

    if (head == tail) {     // single
        int item = head.data;
        head = tail = null;
        return item;
    }

    Node del   = tail;
    int  item  = del.data;
    tail       = tail.prev;
    tail.next  = null;
    return item;
}
CENG114 • Linked Lists in Java
35

### Notes:

<!-- Slide number: 36 -->
DLL DELETION
Delete a Known Node  —  O(1)
Given a reference to any node, removing it takes constant time — no traversal.
new bypass link

prev

12

next

prev

47

next

prev

8

next

prev

25

next

prev

6

next

p

deleteNode  •  O(1)
DLL Superpower
int deleteNode(Node p) {
    if (p == head) return deleteFirst();
    if (p == tail) return deleteLast();

    int item = p.data;
    p.prev.next = p.next;   // bypass forward
    p.next.prev = p.prev;   // bypass backward
    p.prev = p.next = null; // isolate for GC
    return item;
}
Deleting a known node in a SLL is O(n) — you must walk to find the previous node.
In a DLL, p.prev gives that previous node instantly, so the whole operation is O(1).
CENG114 • Linked Lists in Java
36

### Notes:

<!-- Slide number: 37 -->
SUMMARY
Singly vs Doubly Linked Lists

Aspect

Singly Linked

Doubly Linked

Memory per node

data + next

data + prev + next

Traversal direction

forward only

forward AND backward

addBeginning / addEnd

O(1) / O(n)*

O(1) / O(1)

Delete given a node ref p

O(n) — walk to prev

O(1) — use p.prev

Reverse iteration

O(n²) naive

O(n) via tail.prev

Code complexity

simpler

more links to maintain
* without a tail pointer

Pick SLL when
Pick DLL when
Memory is tight, traversal is one-directional, and you mostly grow at the front.
You need backward traversal, O(1) operations at both ends, or O(1) delete given a node.
CENG114 • Linked Lists in Java
37

### Notes:

<!-- Slide number: 38 -->
PRACTICE
Try These — Part 1

EXERCISE 01
EXERCISE 02
Trace a SLL insertion
Find the bug
Given head → 5 → 10 → 15 → null and the call addAfter(p, 7) where p points to node 10, draw the list after each of the four steps.
A student writes:

newNode.next = head;
head.next = newNode;
Why does this corrupt the list? Fix it in two lines.

🎯  Tip:  Draw on paper. Mark each reference with an arrow, then redraw after every line of code.
CENG114 • Linked Lists in Java
38

### Notes:

<!-- Slide number: 39 -->
PRACTICE
Try These — Part 2 (Doubly Linked)

EXERCISE 03
EXERCISE 04
Reverse a DLL
Insert before last occurrence
Implement reverse() that flips a doubly linked list. Hint: for each node swap its prev and next, then swap the head and tail references. Why is this O(n)?
Given a DLL and two values item1 and item2, insert item2 before the LAST node whose data equals item1. What's the most efficient direction to search? Why?

🎯  Tip:  When walking a DLL, tail is just as good a starting point as head. Use the closer one.
CENG114 • Linked Lists in Java
39

### Notes:

<!-- Slide number: 40 -->
WRAP-UP
Key Takeaways

01
A node = data + reference(s)
SLL nodes hold a next; DLL nodes hold prev and next. The list is just a head (and maybe a tail) pointing into a chain.

02
Link OUT before linking IN
Always set the new node's outgoing references first, then update the existing list to point to it. Reversing this order loses the rest of the list.

03
Edge cases are where bugs live
Empty list, single-node list, head, tail — every insertion and deletion must defend against all of them.

04
DLL invariants must be preserved
head.prev == null, tail.next == null, and every forward link mirrored by a backward link. Break one → undefined behavior.

05
Choose ArrayList by default
LinkedList wins only when you need O(1) operations at the ends. For random access, ArrayList beats it by orders of magnitude.
CENG114 • Linked Lists in Java
40

### Notes:

<!-- Slide number: 41 -->

END OF LECTURE

8

16

32

null

Questions?
Next week — Stacks & Queues built on top of linked lists.

CENG114  —  Computer Programming II
Öğr. Gör. Yusuf Evren Aykaç
Computer Engineering Dept.  •  Ankara Yıldırım Beyazıt University

### Notes:
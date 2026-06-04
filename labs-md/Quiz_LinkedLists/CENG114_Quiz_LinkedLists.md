# CENG114 — Computer Programming II

**Ankara Yıldırım Beyazıt University · Department of Computer Engineering**

**Quiz: Linked Lists in Practice — TrackBeat Studio**

|                |                                                          |
| -------------- | -------------------------------------------------------- |
| Instructor     | Yusuf Evren AYKAÇ                                        |
| Assistants     | Çağın ÖZKAYA · Hatice UYSAL · Yusuf Ekrem KEÇİLİOĞLU     |
| Week           | 14 (Spring 2025–2026)                                    |
| Topic          | Singly & Doubly Linked Lists, with a Generic twist       |
| Time budget    | ~1 hour, single-scenario quiz                            |

---

## Hint

A linked list is just *nodes that point at other nodes*. There is no array
under the hood — only references and `null`. The list keeps external handles
to the first node (`head`) and, when useful, also to the last node (`tail`).

Two flavours show up in this quiz:

- **Singly Linked List (SLL)** — every node carries one outgoing reference
  (`next`). Front insert / front delete are cheap; appending requires a `tail`
  pointer or a walk to the end.
- **Doubly Linked List (DLL)** — every node carries two references (`prev`
  and `next`). Both ends become O(1) *and* you can delete any node you
  already hold a reference to in O(1) — no walk required.

You will also reuse the **generics** ideas from last week: both list classes
take a single type parameter `T`, so the same code can hold `Track`s today
and `String` shout-outs tomorrow. Reminder: a `private static class Node<E>`
**does not** see the outer class' `T`; it needs its own type variable.

> **"Link OUT before linking IN."** When inserting, first wire the new
> node's outgoing references to the rest of the list, *then* update the
> existing list to point at the new node. Reverse the order and you lose
> the tail of the chain.

---

## The Scenario

You are interning at **AYBU Radio**, the campus' student-run radio station.
The senior engineers are rebuilding the live-broadcast console — codename
**TrackBeat Studio** — and the next sprint is on your desk. During a typical
show two things happen at once:

1. **Listeners send song requests** via SMS. They pile up in a *first-in,
   first-out* queue. Each request must be enqueued in O(1); the DJ peeks
   at the head to see what is "up next" and pops one off when she accepts
   it onto the playlist.
2. **The DJ curates the actual playlist.** It is a hand-ordered chain of
   tracks she navigates *during the show*: forward to the next song, back
   to the previous one if a listener phones in to replay it, insert an
   interlude right after the current song, or skip a problematic track
   instantly when a phone-in caller starts complaining.

Because the DJ walks both directions through the playlist and inserts/removes
at arbitrary positions during the live show, the playlist must be a
**doubly linked list**. The request queue, on the other hand, only ever
grows at the tail and shrinks from the head — a **singly linked list with
a `tail` pointer** is plenty (and cheaper per node).

Your job is to deliver four classes:

| # | Class               | Role                                                                       |
| - | ------------------- | -------------------------------------------------------------------------- |
| 1 | `Track`             | Domain value object: title, artist, duration in seconds.                   |
| 2 | `RequestQueue<T>`   | Generic SLL-backed FIFO queue with O(1) `enqueue` *and* `dequeue`.         |
| 3 | `DJPlaylist<T>`     | Generic DLL with O(1) ends + O(1) insert/remove given a node reference.    |
| 4 | `TrackBeatStudio`   | Driver: simulates one broadcast and prints the output in §6 byte-for-byte. |

Then implement `TrackBeatStudio.main` to reproduce §6 exactly.

---

## Class 1 — `Track`

A simple value object — no logic beyond formatting.

```
Track
- title:        String
- artist:       String
- durationSec:  int

+ Track(title: String, artist: String, durationSec: int)
+ getTitle():        String
+ getArtist():       String
+ getDurationSec():  int
+ toString():        String
```

### Requirements

- All three fields are `private final`, assigned by the constructor.
- `toString()` returns `"<title> — <artist> (m:ss)"`, e.g.
  `"Imagine — John Lennon (3:03)"`. Use the em-dash `—` (U+2014). Seconds
  are always two digits (zero-padded).

> Hint — convert total seconds with integer division and modulo:
> `String.format("%d:%02d", durationSec / 60, durationSec % 60)`.

---

## Class 2 — `RequestQueue<T>` (SLL with head + tail)

A generic FIFO queue backed by a singly linked list with **both** a head
and a tail reference. Both `enqueue` and `dequeue` must run in **O(1)** —
no traversal allowed.

```
«generic»  RequestQueue<T>
- head:  Node<T>
- tail:  Node<T>
- size:  int

+ enqueue(item: T):  void
+ dequeue():         T              // throws NoSuchElementException
+ peek():            T              // throws NoSuchElementException
+ size():            int
+ isEmpty():         boolean
+ toString():        String
```

The nested node type is a `private static class Node<E>` (note the
separate type parameter `E`, because static nested classes cannot see the
enclosing class' `T`):

```
Node<E>
- data:  E
- next:  Node<E>
```

### Algorithms

**`enqueue(item)` — O(1)**

1. Create `Node<T> n = new Node<>(item)`.
2. If `tail == null` (queue is empty), set `head = tail = n`.
3. Otherwise `tail.next = n; tail = n;`.
4. Increment `size`.

**`dequeue()` — O(1)**

1. If `head == null`, throw `NoSuchElementException("queue is empty")`.
2. Save `data = head.data`.
3. Advance `head = head.next`.
4. If `head == null` afterwards, **set `tail = null` too** — the queue is
   now empty and a stale tail would silently break the next `enqueue`.
5. Decrement `size`, return `data`.

**`peek()`**

If empty, throw `NoSuchElementException`. Otherwise return `head.data`.

**`toString()`**

Return `"[e1, e2, e3]"`, joined by `", "`. An empty queue prints `"[]"`.

> The tail pointer is the *only* reason `enqueue` is O(1); remove it and
> you are back to walking the whole list every time you push.

---

## Class 3 — `DJPlaylist<T>` (DLL)

A generic doubly linked list with both `head` and `tail`. The DJ holds
**direct references** to specific nodes (e.g. "the node that holds
*Hotel California*") and uses them to insert/remove in O(1).

```
«generic»  DJPlaylist<T>
- head:  DNode<T>
- tail:  DNode<T>
- size:  int

+ addFirst(item: T):                    DNode<T>
+ addLast(item: T):                     DNode<T>
+ addAfter(ref: DNode<T>, item: T):     DNode<T>
+ addBefore(ref: DNode<T>, item: T):    DNode<T>

+ removeFirst():                        T
+ removeLast():                         T
+ remove(node: DNode<T>):               T

+ findFirst(value: T):                  DNode<T>    // null if not found
+ size():                               int
+ isEmpty():                            boolean
+ forwardString():                      String
+ backwardString():                     String
+ toString():                           String      // same as forwardString
+ iterator():                           Iterator<T> // forward (Iterable<T>)
+ descendingIterator():                 Iterator<T> // tail → head
```

Because the driver must hold references to specific nodes, `DNode<E>` is a
**public** static nested class with at least the getter shown:

```
public static class DNode<E>
- data:  E
- prev:  DNode<E>
- next:  DNode<E>
+ getData():  E
```

The class must implement `Iterable<T>` so the driver can use enhanced
`for` loops. Implementing `descendingIterator()` is what makes O(n)
reverse traversal cheap (see slide 26 — index-based `get(i)` on a linked
list is the O(n²) trap; an iterator avoids it).

### Algorithms

Use the **four-step rule** for every insertion: create, fill, link OUT,
link IN. In a DLL each insertion rewires *exactly four* references.

**`addFirst(item)` — O(1)**

1. Create node `n`.
2. If list empty (`head == null`), set `head = tail = n`.
3. Else: `n.next = head; head.prev = n; head = n`.
4. `size++`; return `n`.

**`addLast(item)` — O(1)**

Mirror image of `addFirst`. Single-node edge case: `head = tail = n`.

**`addAfter(ref, item)` — O(1)**

1. If `ref == null`, throw `IllegalArgumentException`.
2. If `ref == tail`, delegate to `addLast(item)` (returns the new node).
3. Else:

```
n.prev      = ref;
n.next      = ref.next;
ref.next.prev = n;
ref.next      = n;
```

4. `size++`; return `n`.

**`addBefore(ref, item)` — O(1)** — mirror of `addAfter`. If `ref == head`,
delegate to `addFirst`.

**`removeFirst()` / `removeLast()` / `remove(node)` — all O(1)**

- Empty list → throw `NoSuchElementException`.
- `remove(null)` → throw `IllegalArgumentException`.
- `removeFirst`: single-node case sets `head = tail = null`; otherwise
  `head = head.next; head.prev = null`.
- `removeLast` is the symmetric mirror.
- `remove(node)`: if `node == head` or `node == tail`, delegate. Otherwise
  bypass with

```
node.prev.next = node.next;
node.next.prev = node.prev;
node.prev = node.next = null;   // isolate for GC
```

**`findFirst(value)` — O(n)**

Walk from `head`; return the first `DNode<T>` whose `data` equals `value`
(use `Objects.equals` so a `null` value does not crash). Return `null`
when no node matches.

**`forwardString()` / `backwardString()` / `toString()`**

`"[e1, e2, …]"` walking head→tail (forward) or tail→head (backward).
Empty list prints `"[]"`. `toString()` delegates to `forwardString()`.

**`iterator()` / `descendingIterator()`**

Two `Iterator<T>` implementations — one walking `head → … → null` via
`next`, one walking `tail → … → null` via `prev`. Each `hasNext()` is a
single null check; each `next()` advances by one pointer hop, so both
iterators are O(1) per step.

---

## Class 4 — `TrackBeatStudio` (driver)

A single `main` method that scripts one broadcast. Print the banners
exactly as in §6 — use a small `pad(label)` helper to align the dotted
labels (target width is 18 characters before the value-separating space;
see §6 for examples).

### The script

```text
// --- Setup ---------------------------------------------
playlist  = new DJPlaylist<Track>()
queue     = new RequestQueue<Track>()

stairway      = new Track("Stairway to Heaven",      "Led Zeppelin", 482)
wonderwall    = new Track("Wonderwall",              "Oasis",        258)
imagine       = new Track("Imagine",                 "John Lennon",  183)
hotelCal      = new Track("Hotel California",        "Eagles",       391)
smells        = new Track("Smells Like Teen Spirit", "Nirvana",      301)
bohemian      = new Track("Bohemian Rhapsody",       "Queen",        354)

// --- Phase 1: listener requests -------------------------
queue.enqueue(wonderwall)        // print "Enqueued ......"
queue.enqueue(imagine)
queue.enqueue(hotelCal)
queue.enqueue(bohemian)
print queue.size(), queue.peek(), queue, queue.dequeue(), queue.size()

// --- Phase 2: build the show playlist -------------------
drained = 0
while (!queue.isEmpty()) {
    playlist.addLast(queue.dequeue())
    drained++
}
print "Drained queue into playlist (3 tracks moved)."
print playlist

playlist.addFirst(stairway)              // print "addFirst ......"
hcNode  = playlist.findFirst(hotelCal)
imgNode = playlist.findFirst(imagine)
playlist.addAfter(hcNode, smells)        // print "addAfter(HC) ..."
playlist.addBefore(imgNode, wonderwall)  // print "addBefore(Img) ."
print playlist.size(), playlist

// --- Phase 3: live show ---------------------------------
print numbered forward order  (use playlist.iterator())
print numbered backward order (use playlist.descendingIterator())

playlist.remove(imgNode)         // print "remove(Imagine) .."
playlist.removeFirst()           // print "removeFirst() ...."
playlist.removeLast()            // print "removeLast() ....."
print playlist, total airtime in m:ss
```

---

## 6. Expected Output

Your `main` must produce this output exactly (the dotted labels are
left-padded to a 18-character prefix; one space then the value).

```text
============================================================
   CENG114 · TrackBeat Studio · Linked Lists Lab
============================================================

--- Phase 1 · Request Queue (SLL) ---
Enqueued ......... Wonderwall — Oasis (4:18)
Enqueued ......... Imagine — John Lennon (3:03)
Enqueued ......... Hotel California — Eagles (6:31)
Enqueued ......... Bohemian Rhapsody — Queen (5:54)
Queue size ....... 4
Peek (head) ...... Wonderwall — Oasis (4:18)
Queue ............ [Wonderwall — Oasis (4:18), Imagine — John Lennon (3:03), Hotel California — Eagles (6:31), Bohemian Rhapsody — Queen (5:54)]
Dequeued ......... Wonderwall — Oasis (4:18)
Queue size ....... 3

--- Phase 2 · Build Show Playlist (DLL) ---
Drained queue into playlist (3 tracks moved).
Playlist ......... [Imagine — John Lennon (3:03), Hotel California — Eagles (6:31), Bohemian Rhapsody — Queen (5:54)]
addFirst ......... Stairway to Heaven — Led Zeppelin (8:02)
addAfter(HC) ..... Smells Like Teen Spirit — Nirvana (5:01)
addBefore(Img) ... Wonderwall — Oasis (4:18)
Playlist size .... 6
Playlist ......... [Stairway to Heaven — Led Zeppelin (8:02), Wonderwall — Oasis (4:18), Imagine — John Lennon (3:03), Hotel California — Eagles (6:31), Smells Like Teen Spirit — Nirvana (5:01), Bohemian Rhapsody — Queen (5:54)]

--- Phase 3 · Live Show ---
Forward order:
  1. Stairway to Heaven — Led Zeppelin (8:02)
  2. Wonderwall — Oasis (4:18)
  3. Imagine — John Lennon (3:03)
  4. Hotel California — Eagles (6:31)
  5. Smells Like Teen Spirit — Nirvana (5:01)
  6. Bohemian Rhapsody — Queen (5:54)
Backward order:
  1. Bohemian Rhapsody — Queen (5:54)
  2. Smells Like Teen Spirit — Nirvana (5:01)
  3. Hotel California — Eagles (6:31)
  4. Imagine — John Lennon (3:03)
  5. Wonderwall — Oasis (4:18)
  6. Stairway to Heaven — Led Zeppelin (8:02)
remove(Imagine) .. Imagine — John Lennon (3:03)
removeFirst() .... Stairway to Heaven — Led Zeppelin (8:02)
removeLast() ..... Bohemian Rhapsody — Queen (5:54)
Final playlist ... [Wonderwall — Oasis (4:18), Hotel California — Eagles (6:31), Smells Like Teen Spirit — Nirvana (5:01)]
Total airtime .... 15:50

============================================================
   Broadcast Ready
============================================================
```

---

## Why This Shape? — Design Rationale

A few decisions are worth thinking about before you start typing:

- **Why a SLL with a tail pointer for the queue?** A FIFO queue only ever
  touches the two ends. Adding `prev` references would double the per-node
  memory cost and buy you nothing. Keep the cheaper structure.
- **Why expose `DNode<T>` publicly?** Because the DJ holds *references*
  to specific tracks ("the *Hotel California* node") and uses them to
  insert/remove in O(1). In a SLL she would have to walk the list every
  time — `remove(node)` would degrade to O(n).
- **Why iterators instead of `get(i)` for printing?** Slide 26 — the
  index-based loop on a linked list is the O(n²) trap. An iterator that
  holds a node reference advances in O(1) per step, so the whole forward
  walk is O(n).

---

## Pitfalls to Avoid

- **Never** write `tail.next = n` before checking that the queue is
  non-empty. The empty-queue branch of `enqueue` must set BOTH `head`
  and `tail` to the new node.
- After `dequeue()` empties the queue, **null out `tail` too**. A
  dangling `tail` pointing at the just-removed node will silently
  corrupt the next `enqueue`.
- In the DLL, "link OUT before linking IN" is not optional: set
  `n.prev` and `n.next` *before* you overwrite the neighbours'
  pointers — otherwise you lose the rest of the chain.
- `remove(node)` must defend against the head and tail cases. Do not
  blindly dereference `node.prev.next` or `node.next.prev`.
- `findFirst` must use `Objects.equals` (or guard against `null`).
  Comparing with `==` only works for the exact same object reference;
  if the caller hands you a freshly constructed `Track` it will silently
  miss.
- For `toString` / `forwardString` / `backwardString`: an empty
  list/queue prints `"[]"`, not `"[ ]"` and not `""`.
- Do **not** compute the duration with `(int)(durationSec / 60.0)` —
  use integer division `durationSec / 60` for the minutes component
  and `% 60` for the seconds.
- Do **not** iterate the playlist with an index-and-`get(i)` loop in
  your driver. There is no `get(i)` on `DJPlaylist` for a reason —
  the slide 26 trap is in this lab on purpose.

---

> **Deliverables.** Four `.java` files in one folder: `Track.java`,
> `RequestQueue.java`, `DJPlaylist.java`, `TrackBeatStudio.java`.
> Compile with `javac *.java`; the run `java TrackBeatStudio` must
> match §6 byte-for-byte (modulo trailing newlines).

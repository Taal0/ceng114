## How to Use These Notes

Recommended reading order:

1. `01_Singly_Linked_Lists_in_Java.md`
2. `02_Singly_Linked_Lists_Insertion_in_Java.md`
3. `03_Singly_Linked_Lists_Deletion_in_Java.md`
4. `04_Singly_Linked_Lists_Examples_and_Exercises_in_Java.md`
5. `05_Doubly_Linked_Lists_in_Java.md`
6. `06_Doubly_Linked_Lists_Examples_and_Exercises_in_Java.md`

Each file is written to:
- mirror the **structure** and **logic** of the original C code (head pointer, node pointers, NULL termination, add/delete cases),
- but also explain the **Java differences** (references, `null`, garbage collection, OOP design patterns).

## What You Will Find Inside

- Carefully explained Java equivalents for:
  - `display_list`, `size_list`, `search_node`, `rec_search_node`
  - `add_beginning`, `add_after`, `add_end`, list creation from arrays and user input
  - `delete_first`, `delete_after`, `delete_last`, `delete_nth`, `destroy_list`
  - Doubly-linked list operations (head/tail management, `prev` links)
- Edge cases called out explicitly:
  - empty list, single-node list, multi-node list
- Time complexity discussions
- Common pitfalls (especially `null` handling and link update order)
- Extra idiomatic Java alternatives (Optional, exceptions, encapsulating `head`/`tail` in classes)

## Notes About Java Built-in Collections

Java already provides `java.util.LinkedList` (a doubly linked list implementation).  
These notes intentionally focus on *implementing linked lists yourself* to match the educational goals.

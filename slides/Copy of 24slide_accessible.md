<!-- Slide number: 1 -->
# Introduction to Java Programming and Data Structures
Thirteenth Edition

![Front Cover: Introduction to Java Programming and Data Structures Thirteenth Edition by Liang.](Picture8.jpg)
Chapter 24
Implementing Lists, Stacks, Queues, and Priority Queues
Copyright © 2024 Pearson Education, Inc. All Rights Reserved

![Pearson Logo](PicturePlaceholder21.jpg)

### Notes:
If this PowerPoint presentation contains mathematical equations, you may need to check that your computer has the following installed:
1) MathType Plugin
2) Math Player (free versions available)
3) NVDA Reader (free versions available)

Slides in this presentation contain hyperlinks. JAWS users should be able to get a list of links by using INSERT+F7

<!-- Slide number: 2 -->
# Objectives (1 of 2)
24.1 To design common operations of lists in an interface and make the interface a subtype of Collection (§24.2).
24.2 To design and implement an array list using an array (§24.3).
24.3 To create a linked list using a linked structure (§24.4).
24.4 To design MyLinkedList class (§24.5).
24.5 To implement MyLinkedList class (§24.6).
24.6 To compare the performance between MyArrayList and MyLinkedList (§24.7).

<!-- Slide number: 3 -->
# Objectives (2 of 2)
24.7 To explore variations of linked lists (§24.8).
24.8 To design and implement a stack class using an array list and a queue class using a linked list (§24.9).
24.9 To design and implement a priority queue using a heap (§24.10).

<!-- Slide number: 4 -->
# Lists
A list is a popular data structure to store data in sequential order. For example, a list of students, a list of available rooms, a list of cities, and a list of books, etc. can be stored using lists. The common operations on a list are usually the following:
Retrieve an element from this list.
Insert a new element to this list.
Delete an element from this list.
Find how many elements are in this list.
Find if an element is in this list.
Find if this list is empty.

<!-- Slide number: 5 -->
# Two Ways to Implement Lists
There are two ways to implement a list.
Using arrays. One is to use an array to store the elements. The array is dynamically created. If the capacity of the array is exceeded, create a new larger array and copy all the elements from the current array to the new array.
Using linked list. The other approach is to use a linked structure. A linked structure consists of nodes. Each node is dynamically created to hold an element. All the nodes are linked together to form a list.

<!-- Slide number: 6 -->
# Design of ArrayList and LinkedList
For convenience, let’s name these two classes: MyArrayList and MyLinkedList. These two classes have common operations, but different data fields. The common operations can be generalized in an interface or an abstract class. Prior to Java 8, a popular design strategy is to define common operations in an interface and provide an abstract class for partially implementing the interface. So, the concrete class can simply extend the abstract class without implementing the full interface. Java 8 enables you to define default methods. You can provide default implementation for some of the methods in the interface rather than in an abstract class.

![A diagram shows MyArrayList and MyLinkedList leading to MyList which leads to java.until.Collection which finally leads to java.until.Iterable.](Picture7.jpg)

<!-- Slide number: 7 -->
# MyList Interface

![A diagram snows the interface MyList less than symbol E greater than symbol leading to java.util.Collection less than symbol E greater than symbol. For long description in Notes pane, press F6.](Picture5.jpg)
MyList

### Notes:
The codes and descriptions for the interface MyList less than symbol E greater than symbol are as follows.
+add left parenthesis index colon int, e colon E right parenthesis colon void. Inserts a new element at the specified index in this list.
+get left parenthesis index colon int right parenthesis colon E. Returns the element from this list at the specified index.
+indexOf left parenthesis e colon Object right parenthesis colon int. Returns the index of the first matching element in this list.
+lastIndexOf left parenthesis e colon E right parenthesis colon int. Returns the index of the last matching element in this list.
+remove left parenthesis index colon int right parenthesis colon E. Removes the element at the specified index and returns the removed element.
+set left parenthesis index colon int, e colon E right parenthesis colon E. Sets the element at the specified index and returns the element being replaced.
Override the add, isEmpty, remove, containsAll, addAll, removeAll, retainAll, toArray left parenthesis right parenthesis, and toArray left parenthesis T left bracket right bracket right parenthesis methods defined in Collection using default methods.

MyList: https://liveexample.pearsoncmg.com/html/MyList.html

<!-- Slide number: 8 -->
# Array Lists
Array is a fixed-size data structure. Once an array is created, its size cannot be changed. Nevertheless, you can still use array to implement dynamic data structures. The trick is to create a new larger array to replace the current array if the current array cannot hold new elements in the list.
Initially, an array, say data of Object[] type, is created with a default size. When inserting a new element into the array, first ensure there is enough room in the array. If not, create a new array with the size as twice as the current one. Copy the elements from the current array to the new array. The new array now becomes the current array.

<!-- Slide number: 9 -->
# Array List Animation
https://liveexample.pearsoncmg.com/dsanimation/ArrayListeBook.html

### Notes:
https://liveexample.pearsoncmg.com/dsanimation/ArrayListeBook.html

<!-- Slide number: 10 -->
# Insertion
Before inserting a new element at a specified index, shift all the elements after the index to the right and increase the list size by 1.

![A diagram shows inserting a new element at a specified index. For long description in Notes pane, press F6.](Picture4.jpg)
Add Method Animation

### Notes:
Before inserting e at insertion point i. A horizontal rectangle with segments from left to right as 0, 1, ellipsis i, i + 1, ellipsis, k, k + 1. The elements in these segments are e0, e1, ellipsis e sub i minus 1, e sub I, e sub i + 1, ellipsis e sub k, e sub k + 1. The segment after k + 1 are divided into two parts by a upward sloping diagonal. The rightmost segment is labeled, data.length minus 1. The segment i is labeled, insertion point.
After inserting e at insertion point i, list size is incremented by 1. Element e is inserted at segment i such that segment i before insertion becomes segment i + 1 after insertion, i + 1 becomes i + 2 and so on.

Add Method Animation: https://liveexample.pearsoncmg.com/codeanimation/ArrayListAdd.html

<!-- Slide number: 11 -->
# Deletion
To remove an element at a specified index, shift all the elements after the index to the left by one position and decrease the list size by 1.

![A diagram shows removing an element at a specified index. For long description in Notes pane, press F6.](Picture5.jpg)
Remove Method Animation

### Notes:
Before deleting the element at index i. A horizontal rectangle with segments from left to right as 0, 1, ellipsis i, i + 1, ellipsis, k, k + 1. The elements in these segments are e0, e1, ellipsis e sub i minus 1, e sub I, e sub i + 1, ellipsis e sub k, e sub k + 1. The segment after k + 1 are divided into two parts by a upward sloping diagonal. The rightmost segment is labeled, data.length minus 1. The segment i is labeled, delete this element.
After deleting the element, list size is decremented by 1. Element e sub i is deleted at segment i such that segment i before insertion becomes segment i minus 1 after insertion, i + 1 becomes i and so on.

Remove Method Animation: https://liveexample.pearsoncmg.com/codeanimation/ArrayListRemove.html

<!-- Slide number: 12 -->
# Implementing MyArrayList

![A diagram shows MyArrayList less than symbol E greater than symbol leading to the interface MyList less than symbol E greater than symbol. For long description in Notes pane, press F6.](Picture5.jpg)
TestMyArrayList
MyArrayList

### Notes:
The codes and descriptions for MyArrayList less than symbol E greater than symbol are as follows.
Hyphen data colon E left bracket right bracket. Array for storing elements in this array list.
Hyphen size colon int. The number of elements in the array list.
+MyArrayList left parenthesis right parenthesis. Creates a default array list.
+MyArrayList left parenthesis objects colon E left bracket right bracket right parenthesis. Creates an array list from an array of objects.
+trimToSize left parenthesis right parenthesis colon void. Trims the capacity of this array list to the list’s current size.
Hyphen ensureCapacity left parenthesis right parenthesis colon void. Doubles the current array size if needed. Doubles the current array size if needed.
Hyphen checkIndex left parenthesis index colon int right parenthesis colon void. Throws an exception if the index is out of bounds in the list.

MyArrayList: https://liveexample.pearsoncmg.com/html/MyArrayList.html
TestMyArrayList: https://liveexample.pearsoncmg.com/html/TestMyArrayList.html

<!-- Slide number: 13 -->
# Linked Lists
Since MyArrayList is implemented using an array, the methods get(int index) and set(int index, Object o) for accessing and modifying an element through an index and the add(Object o) for adding an element at the end of the list are efficient. However, the methods add(int index, Object o) and remove(int index) are inefficient because it requires shifting potentially a large number of elements. You can use a linked structure to implement a list to improve efficiency for adding and removing an element anywhere in a list.

<!-- Slide number: 14 -->
# Linked List Animation
https://liveexample.pearsoncmg.com/dsanimation/LinkedListeBook.html

![A screenshot of a web page shows the LinkedList Animation by Y. Daniel Liang. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
https://liveexample.pearsoncmg.com/dsanimation/LinkedListeBook.html

Enter a value and click the Search, Insert, or Delete button to search, insert, or delete the value from the list. Enter a value and an index and then click the Insert button to insert the value in the specified index. Enter an index and then click the Delete button to delete the value in the specified index.
A series of four rectangles from left to right. Each rectangle is divided into two parts, a longer part on the left and shorter part on the right. The value in the longer part from left to right is 5, 65, 7, and 17. The longer part of the first rectangle is labeled, head and that of the fourth rectangle is labeled, tail. There is a right arrow from shorter part of one rectangle to the longer part of the next rectangle.

<!-- Slide number: 15 -->
# Nodes in Linked Lists
A linked list consists of nodes. Each node contains an element, and each node is linked to its next neighbor. Thus a node can be defined as a class, as follows:

![A series of nodes labeled, node 1 to node n. For long description in Notes pane, press F6.](Picture7.jpg)
class Node<E> {
E element;
Node<E> next;
public Node(E o) {
element = o;
}
}

### Notes:
Each node has two horizontal segment with top segment labeled, element and bottom labeled, next. An arrow points from the next of one node to the element of next node. The top segment of the first node is labeled, head, the bottom segment of the last node is labeled, null, and the top segment is labeled, tail.

<!-- Slide number: 16 -->
# Adding Three Nodes (1 of 4)
The variable head refers to the first node in the list, and the variable tail refers to the last node in the list. If the list is empty, both are null. For example, you can create three nodes to store three strings in a list, as follows:
Step 1: Declare head and tail:

![Node less than symbol String greater than symbol head = null semi colon Node less than symbol String greater than symbol tail = null semicolon The list is empty now.](Picture7.jpg)

<!-- Slide number: 17 -->
# Adding Three Nodes (2 of 4)
Step 2: Create the first node and insert it to the list:

![head = new Node less than symbol greater than symbol left parenthesis start double quotation marks Chicago end double quotation marks semi colon. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
tail = head semi colon
After the first node is inserted.
A rectangle with two horizontal segments. Top segment has text start double quotation marks Chicago end double quotation marks in it. The segment is labeled, head on the left and tail on the right. The bottom segment has text, next colon null.

<!-- Slide number: 18 -->
# Adding Three Nodes (3 of 4)
Step 3: Create the second node and insert it to the list:

![tail.next = new Node less than symbol greater than symbol left parenthesis start double quotation marks Denver end double quotation marks right parenthesis semi colon. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
A rectangle with two horizontal segments. Top segment has text start double quotation marks Chicago end double quotation marks in it. The segment is labeled, head on the left and tail on the right. The bottom segment has text, next. An arrow from this rectangle points to another rectangle to the right. The rectangle has two horizontal segments. Top segment has text start double quotation marks Denver end double quotation marks in it. The bottom segment has text, next colon null.
tail = tail.next semi colon
A rectangle with two horizontal segments. Top segment has text start double quotation marks Chicago end double quotation marks in it. The segment is labeled, head on the left. The bottom segment has text, next. An arrow from this rectangle points to another rectangle to the right. The rectangle has two horizontal segments. Top segment has text start double quotation marks Denver end double quotation marks in it. The segment is labeled, tail on the right. The bottom segment has text, next colon null.

<!-- Slide number: 19 -->
# Adding Three Nodes (4 of 4)
Step 4: Create the third node and insert it to the list:

![tail.next = new Node less than symbol greater than symbol left parenthesis start double quotation marks Dallas end double quotation marks right parenthesis semi colon. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
A rectangle with two horizontal segments. Top segment has text start double quotation marks Chicago end double quotation marks in it. The segment is labeled, head on the top. The bottom segment has text, next. An arrow from this rectangle points to another rectangle to the right. The rectangle has two horizontal segments. Top segment has text start double quotation marks Denver end double quotation marks in it. The segment is labeled, tail on the right. The bottom segment has text, next. An arrow from this rectangle points to another rectangle to the right. The rectangle has two horizontal segments. Top segment has text start double quotation marks Dallas end double quotation marks in it. The bottom segment has text, next colon null.
tail = tail.next semi colon
A rectangle with two horizontal segments. Top segment has text start double quotation marks Chicago end double quotation marks in it. The segment is labeled, head on the top. The bottom segment has text, next. An arrow from this rectangle points to another rectangle to the right. The rectangle has two horizontal segments. Top segment has text start double quotation marks Denver end double quotation marks in it. The bottom segment has text, next. An arrow from this rectangle points to another rectangle to the right. The rectangle has two horizontal segments. Top segment has text start double quotation marks Dallas end double quotation marks in it. The segment is labeled, tail on the right. The bottom segment has text, next colon null.

<!-- Slide number: 20 -->
# Traversing All Elements in the List
Each node contains the element and a data field named next that points to the next node. If the node is the last in the list, its pointer data field next contains the value null. You can use this property to detect the last node. For example, you may write the following loop to traverse all the nodes in the list.
Node<E> current = head;
while (current != null) {
System.out.println(current.element);
current = current.next;
}

<!-- Slide number: 21 -->
# MyLinkedList

![A diagram shows link node leading to Node less than symbol E greater than symbol and MyLinkedList less than symbol E greater than symbol leading to the interface MyList less than symbol E greater than symbol. For long description in Notes pane, press F6.](Picture5.jpg)
MyLinkedList
TestMyLinkedList

### Notes:
A horizontal line connects Node less than symbol E greater than symbol and MyLinkedList less than symbol E greater than symbol with m near the former and 1 near the later.

The codes and descriptions for Node less than symbol E greater than symbol are as follows.
element colon E
next colon Node less than symbol E greater than symbol
The codes and descriptions for MyLinkedList less than symbol E greater than symbol are as follows.
Hyphen head colon Node less than symbol E greater than symbol. The head of the list.
Hyphen tail colon Node less than symbol E greater than symbol. The tail of the list.
Hyphen size colon int. The number of elements in the list.
+MyLinkedList left parenthesis right parenthesis. Creates a default linked list.
+MyLinkedList left parenthesis elements colon E left bracket right bracket right parenthesis. Creates a linked list from an array of elements.
+addFirst left parenthesis e colon E right parenthesis colon void. Adds an element to the head of the list.
+addLast left parenthesis e colon E right parenthesis colon void. Adds an element to the tail of the list.
+getFirst left parenthesis right parenthesis colon E. Returns the first element in the list.
+getLast left parenthesis right parenthesis colon E. Returns the last element in the list.
+removeFirst left parenthesis right parenthesis colon E. Removes the first element from the list.
+removeLast left parenthesis right parenthesis colon E. Removes the last element from the list.

MyLinkedList: https://liveexample.pearsoncmg.com/html/MyLinkedList.html
TestMyLinkedList: https://liveexample.pearsoncmg.com/html/TestMyLinkedList.html

<!-- Slide number: 22 -->
# Implementing addFirst(E e)
public void addFirst(E e) {
Node<E> newNode = new Node<>(e);
newNode.next = head;
head = newNode;
size++;
if (tail == null)
tail = head;
}
addFirst Animation

![A diagram shows inserting a new node. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
(a) Before a new node is inserted.
A series of rectangles, each with two horizontal segments. The top segment of the first rectangle has elements e0 in it. The segment is labeled, head on the top. The bottom segment has text, next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub i in it. The bottom segment has text, next. An arrow from this rectangle points to another rectangle to the right. The top segment has element e sub i + 1 in it. The bottom segment has text, next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub k in it. The bottom segment has text, next colon null. The top segment is labeled, tail. A rectangle with two horizontal segments left to the first rectangle is labeled, a new node to be inserted here. The top segment has element e in it and the bottom segment has text, next.
(b) After a new node is inserted.
A series of rectangles, each with two horizontal segments. The top segment of the first rectangle has element e in it and the bottom segment has text, next. An arrow from this rectangle points to the next rectangle to the right. The top segment of the rectangle has elements e0 in it. The segment is labeled, head on the top. The bottom segment has text, next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub i in it. The bottom segment has text, next. An arrow from this rectangle points to another rectangle to the right. The top segment has element e sub i + 1 in it. The bottom segment has text, next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub k in it. The bottom segment has text, next colon null. The top segment is labeled, tail.

addFirst Animation: https://liveexample.pearsoncmg.com/codeanimation/LinkedListAddFirst.html

<!-- Slide number: 23 -->
# Implementing addLast(E e)
public void addLast(E e) {
if (tail == null) {
head = tail = new Node<>(e);
}
else {
tail.next = new Node<>(e);
tail = tail.next;
}
size++;
}
addLast Animation

![A diagram shows inserting a new node. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
(a) Before a new node is inserted.
A series of rectangles, each with two horizontal segments. The top segment of the first rectangle has elements e0 in it. The segment is labeled, head on the top. The bottom segment has text, next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub i in it. The bottom segment has text, next. An arrow from this rectangle points to another rectangle to the right. The top segment has element e sub i + 1 in it. The bottom segment has text, next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub k in it. The bottom segment has text, next colon null. The top segment is labeled, tail. A rectangle with two horizontal segments right to the last rectangle is labeled, a new node to be inserted here. The top segment has element e in it and the bottom segment has text, null.
(b) After a new node is inserted.
A series of rectangles, each with two horizontal segments. The top segment of the first rectangle has elements e0 in it. The segment is labeled, head on the top. The bottom segment has text, next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub i in it. The bottom segment has text, next. An arrow from this rectangle points to another rectangle to the right. The top segment has element e sub i + 1 in it. The bottom segment has text, next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub k in it. The bottom segment has text, next. An arrow from this rectangle points to the next rectangle. The top segment has element e in it and the bottom segment has text, next colon null. The top segment is labeled, tail.

addLast Animation: https://liveexample.pearsoncmg.com/codeanimation/LinkedListAddLast.html

<!-- Slide number: 24 -->
# Implementing add(int index, E e)
public void add(int index, E e) {
if (index == 0) addFirst(e);
else if (index >= size) addLast(e);
else {
Node<E> current = head;
for (int i = 1; i < index; i++)
current = current.next;
Node<E> temp = current.next;
current.next = new Node<>(e);
(current.next).next = temp;
size++;
}
}
Add(index, e) Animation

![A diagram shows inserting a new node. For long description in Notes pane, press F6.](Picture4.jpg)

![A diagram shows inserting a new node. For long description in Notes pane, press F6.](Picture8.jpg)

### Notes:
(a) Before a new node is inserted.
A series of rectangles, each with two horizontal segments. The top segment of the first rectangle has elements e0 in it. The segment is labeled, head on the top. The bottom segment has text, next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub i in it. The bottom segment has text, next. The rectangle is labeled, current. An arrow from this rectangle points to another rectangle to the right. The top segment has element e sub i + 1 in it. The bottom segment has text, next. The rectangle is labeled, temp. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub k in it. The bottom segment has text, next colon null. The top segment is labeled, tail. A rectangle with two horizontal segments between current and temp rectangles is labeled, a new node to be inserted here. The top segment has element e in it and the bottom segment has text, null.

(b) After a new node is inserted.
A series of rectangles, each with two horizontal segments. The top segment of the first rectangle has elements e0 in it. The segment is labeled, head on the top. The bottom segment has text, next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub i in it. The bottom segment has text, next. The rectangle is labeled, current. An arrow from this rectangle points to the new rectangle. The top segment has element e in it. The bottom segment has text, next. An arrow from this rectangle points to the next rectangle to the right. The top segment has element e sub i + 1 in it. The bottom segment has text, next. The rectangle is labeled, temp. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub k in it. The bottom segment has text, next colon null. The top segment is labeled, tail. A rectangle with two horizontal segments between current and temp rectangles is labeled, a new node to be inserted here. The top segment has element e in it and the bottom segment has text, null.

Add(index, e) Animation: https://liveexample.pearsoncmg.com/codeanimation/LinkedListAdd.html

<!-- Slide number: 25 -->
# Implementing removeFirst()
public E removeFirst() {
if (size == 0) return null;
else {
Node<E> temp = head;
head = head.next;
size--;
if (head == null) tail = null;
return temp.element;
}
}
removeFirst Animation

![A diagram shows deleting a new node. For long description in Notes pane, press F6.](Picture4.jpg)

### Notes:
(a) Before the node is deleted.
A series of rectangles, each with two horizontal segments. The top segment of the first rectangle has elements e0 in it. The segment is labeled, head on the top. The bottom segment has text, next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub i in it. The bottom segment has text, next. An arrow from this rectangle points to the next rectangle to the right. The top segment has element e sub i + 1 in it. The bottom segment has text, next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub k in it. The bottom segment has text, next colon null. The top segment is labeled, tail. The first rectangle is labeled, delete this node.
(b) After the first node is deleted.
A series of rectangles, each with two horizontal segments. The top segment of the first rectangle has element e1 in it. The segment is labeled, head on the top. The bottom segment has text, next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub i in it. The bottom segment has text, next. An arrow from this rectangle points to another rectangle to the right. The top segment has element e sub i + 1 in it. The bottom segment has text, next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub k in it. The bottom segment has text, null. The top segment is labeled, tail.

removeFirst Animation: https://liveexample.pearsoncmg.com/codeanimation/LinkedListRemoveFirst.html

<!-- Slide number: 26 -->
# Implementing removeLast() (1 of 2)
public E removeLast() {
if (size == 0) return null;
else if (size == 1)
{
Node<E> temp = head;
head = tail = null;
size = 0;
return temp.element;
}
addLast Animation

![A diagram shows deleting a new node. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
(a) Before the node is deleted.
A series of rectangles, each with two horizontal segments. The top segment of the first rectangle has elements e0 in it. The segment is labeled, head on the top. The bottom segment has text, next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub i in it. The bottom segment has text, next. An arrow from this rectangle points to the next rectangle to the right. The top segment has element e sub i + 1 in it. The bottom segment has text, next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub k in it. The bottom segment has text, next colon null. The top segment is labeled, tail. The last rectangle is labeled, delete this node.
(b) After the last node is deleted.
A series of rectangles, each with two horizontal segments. The top segment of the first rectangle has element e1 in it. The segment is labeled, head on the top. The bottom segment has text, next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub i in it. The bottom segment has text, next. An arrow from this rectangle points to another rectangle to the right. The top segment has element e sub i + 1 in it. The bottom segment has text, next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the next rectangle. The top segment has element e sub k minus 1 in it. The bottom segment has text, null. The top segment is labeled, tail.

addLast Animation: https://liveexample.pearsoncmg.com/codeanimation/LinkedListRemoveLast.html

<!-- Slide number: 27 -->
# Implementing removeLast() (2 of 2)
else
{
Node<E> current = head;
for (int i = 0; i < size - 2; i++)
current = current.next;
Node temp = tail;
tail = current;
tail.next = null;
size--;
return temp.element;
}
}

### Notes:

<!-- Slide number: 28 -->
# Implementing remove(int index) (1 of 2)
public E remove(int index) {
if (index < 0 || index >= size) return null;
else if (index == 0) return removeFirst();
else if (index == size - 1) return removeLast();
else {
Node<E> previous = head;
for (int i = 1; i < index; i++) {
previous = previous.next;
}

### Notes:

<!-- Slide number: 29 -->
# Implementing remove(int index) (2 of 2)
Node<E> current = previous.next;
previous.next = current.next;
size--;
return current.element;
}
}
remove Animation

![A diagram shows deleting a new node. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
(a) Before the node is deleted.
A series of rectangles, each with two horizontal segments. The top segment has text element and bottom segment has text, next. The bottom segment of the last rectangle has text, null. The top segment of the first rectangle is labeled, head and the top segment of the last rectangle is labeled, tail. An arrow from the first rectangle points to ellipsis and an arrow from ellipsis point to the next rectangle labeled, previous. An arrow from this rectangle points to the next rectangle labeled, current. An arrow from this rectangle points to the next rectangle labeled, current.next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the last rectangle. The current rectangle is labeled, node to be deleted.
(b) After the node is deleted.
A series of rectangles, each with two horizontal segments. The top segment has text element and bottom segment has text, next. The bottom segment of the last rectangle has text, null. The top segment of the first rectangle is labeled, head and the top segment of the last rectangle is labeled, tail. An arrow from the first rectangle points to ellipsis and an arrow from ellipsis point to the next rectangle labeled, previous. An arrow from this rectangle points to the next rectangle labeled, current.next. An arrow from this rectangle points to ellipsis and an arrow from ellipsis points to the last rectangle.

remove Animation: https://liveexample.pearsoncmg.com/codeanimation/LinkedListRemove.html

<!-- Slide number: 30 -->
# Time Complexity for ArrayList and LinkedList
| Methods | MyArrayList/ArrayList | MyLinkedList/LinkedList |
| --- | --- | --- |
| add(e: E) | O of 1 | O of 1 |
| add(index: int, e: E) | O of n | O of n |
| clear() | O of 1 | O of 1 |
| contains(e: E) | O of n | O of n |
| get(index: int) | O of 1 | O of n |
| indexOf(e: E) | O of n | O of n |
| isEmpty() | O of 1 | O of 1 |
| lastIndexOf(e: E) | O of n | O of n |
| remove(e: E) | O of n | O of n |
| size() | O of 1 | O of 1 |
| remove(index: int) | O of n | O of n |
| set(index: int, e: E) | O of n | O of n |
| addFirst(e: E) | O of n | O of 1 |
| removeFirst() | O of n | O of 1 |

### Notes:

<!-- Slide number: 31 -->
# Circular Linked Lists
A circular, singly linked list is like a singly linked list, except that the pointer of the last node points back to the first node.

![A series of nodes, each with two horizontal segments. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
The top segment has text element and bottom segment has text, next. The nodes are numbered 1 through n. The first node is labeled, head and the last rectangle is labeled, tail. An arrow from node 1 points to node 2. An arrow from node 2 points to ellipsis and an arrow from ellipsis points to node n. An arrow from node n points back to node 1.

<!-- Slide number: 32 -->
# Doubly Linked Lists
A doubly linked list contains the nodes with two pointers. One points to the next node and the other points to the previous node. These two pointers are conveniently called a forward pointer and a backward pointer. So, a doubly linked list can be traversed forward and backward.

![A series of nodes, each with three horizontal segments. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
The top segment has text element, middle segment has text, next, and the bottom segment has text, previous. The nodes are numbered 1 through n. The first node is labeled, head and the last rectangle is labeled, tail. An arrow from next of node 1 points to node 2. An arrow from next of node 2 points to ellipsis and an arrow from ellipsis points to node n. An arrow from previous of one node points to the previous node.

<!-- Slide number: 33 -->
# Circular Doubly Linked Lists
A circular, doubly linked list is doubly linked list, except that the forward pointer of the last node points to the first node and the backward pointer of the first pointer points to the last node.

![A series of nodes, each with three horizontal segments. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
The top segment has text element, middle segment has text, next, and the bottom segment has text, previous. The nodes are numbered 1 through n. The first node is labeled, head and the last rectangle is labeled, tail. An arrow from next of node 1 points to node 2. An arrow from next of node 2 points to ellipsis and an arrow from ellipsis points to node n. An arrow from previous of one node points to the previous node. An arrow from previous of the first node points to the last node. An arrow from next of the last node points back to the first node.

<!-- Slide number: 34 -->
# Stacks
A stack can be viewed as a special type of list, where the elements are accessed, inserted, and deleted only from the end, called the top, of the stack.

![A diagram shows data 1 stored first, data 2 stored above data 1, and data 3 stored above data 2, together making a stack. While retrieving, data 3 is retrieved first, then data 2 is retrieved, and finally data 1 is retrieved.](Picture7.jpg)

### Notes:

<!-- Slide number: 35 -->
# Queues
A queue represents a waiting list. A queue can be viewed as a special type of list, where the elements are inserted into the end (tail) of the queue, and are accessed and deleted from the beginning (head) of the queue.

![A diagram shows data 1 stored first, data 2 stored above data 1, and data 3 stored above data 2, together making a stack. While retrieving, data 1 is retrieved first, then data 2 is retrieved, and finally data 3 is retrieved.](Picture7.jpg)

<!-- Slide number: 36 -->
# Stack Animation
https://liveexample.pearsoncmg.com/dsanimation/StackeBook.html

![A screenshot of a web page shows Stack Animation by Y. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
https://liveexample.pearsoncmg.com/dsanimation/StackeBook.html
Daniel Liang.
Enter a value and click the Push button to push the value into the stack. Click the Pop button to remove the top element from the stack.
A stack of number from bottom to top as 4, 3, 3, 5. 5 is labeled, top. The text box for Enter a value has 5 entered in it. The buttons Push and Pop are right to the text box.

<!-- Slide number: 37 -->
# Queue Animation
https://liveexample.pearsoncmg.com/dsanimation/QueueeBook.html

![A screenshot of a web page shows Queue Animation by Y. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
https://liveexample.pearsoncmg.com/dsanimation/QueueeBook.html
Daniel Liang.
Enter a value and click the Enqueue button to append the value into the tail of the queue. Click the Dequeue button to remove the element from the head of the queue.
A rectangle divided into 5 segment. The first segment on the left is labeled, head and last segment on the right is labeled, tail. The numbers in the segments from left to right are 5, 45, 2, 4, and 21. The text box for Enter a value has 21 entered in it. The buttons Enqueue and Dequeue are right to the text box.

<!-- Slide number: 38 -->
# Implementing Stacks and Queues
Using an array list to implement Stack
Use a linked list to implement Queue
Since the insertion and deletion operations on a stack are made only at the end of the stack, using an array list to implement a stack is more efficient than a linked list. Since deletions are made at the beginning of the list, it is more efficient to implement a queue using a linked list than an array list. This section implements a stack class using an array list and a queue using a linked list.

<!-- Slide number: 39 -->
# Design of the Stack and Queue Classes
There are two ways to design the stack and queue classes:
Using inheritance: You can define the stack class by extending the array list class, and the queue class by extending the linked list class.

![(a) Using inheritance. A diagram shows GenericStack leading to ArrayList and GenericQueue leading to LinkedList.](Picture4.jpg)
Using composition: You can define an array list as a data field in the stack class, and a linked list as a data field in the queue class.

![(b) Using composition. A diagram shows ArrayList leading to GenericStack and LinkedList leading to GenericQueue.](Picture10.jpg)

<!-- Slide number: 40 -->
# Composition is Better
Both designs are fine, but using composition is better because it enables you to define a complete new stack class and queue class without inheriting the unnecessary and inappropriate methods from the array list and linked list.

<!-- Slide number: 41 -->
# MyStack and MyQueue
GenericStack

![Codes and descriptions for GenericStack less than symbol E greater than symbol. For long description in Notes pane, press F6.](Picture4.jpg)
GenericQueue

![Codes and descriptions for GenericQueue less than symbol E greater than symbol as follows. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
Hyphen list colon java.util.ArrayList less than symbol E greater than symbol. An array list to store elements.
+GenericStack left parenthesis right parenthesis. Creates an empty stack.
+getSize left parenthesis right parenthesis colon int. Returns the number of elements in this stack.
+peek left parenthesis right parenthesis colon E. Returns the top element in this stack.
+pop left parenthesis right parenthesis colon E. Returns and removes the top element in this stack.
+push left parenthesis o colon E right parenthesis colon void. Adds a new element to the top of this stack.
+isEmpty left parenthesis right parenthesis colon boolean. Returns true if the stack is empty.

Hyphen list colon LinkedList less than symbol E greater than symbol
+enqueue left parenthesis e colon E right parenthesis colon void. Adds an element to this queue.
+dequeue left parenthesis right parenthesis colon E. Removes an element from this queue.
+getSize left parenthesis right parenthesis colon int. Returns the number of elements from this queue.

GenericStack: https://liveexample.pearsoncmg.com/html/GenericStack.html
GenericQueue: https://liveexample.pearsoncmg.com/html/GenericQueue.html

<!-- Slide number: 42 -->
# Example: Using Stacks and Queues
Write a program that creates a stack using MyStack and a queue using MyQueue. It then uses the push (enqueu) method to add strings to the stack (queue) and the pop (dequeue) method to remove strings from the stack (queue).
TestStackQueue

### Notes:
TestStackQueue: https://liveexample.pearsoncmg.com/html/TestStackQueue.html

<!-- Slide number: 43 -->
# Priority Queue
A regular queue is a first-in and first-out data structure. Elements are appended to the end of the queue and are removed from the beginning of the queue. In a priority queue, elements are assigned with priorities. When accessing elements, the element with the highest priority is removed first. A priority queue has a largest-in, first-out behavior. For example, the emergency room in a hospital assigns patients with priority numbers; the patient with the highest priority is treated first.

![Codes and descriptions for MyPriorityQueue less than symbol E extends Comparable less than symbol E greater than symbol, greater than symbol as follows. For long description in Notes pane, press F6.](Picture6.jpg)
MyPriorityQueue
TestPriorityQueue

### Notes:
Hyphen heap colon Heap less than symbol E greater than symbol
+enqueue left parenthesis element colon E right parenthesis colon void. Adds an element to this queue.
+dequeue left parenthesis right parenthesis colon E. Removes an element from this queue.
+getSize left parenthesis right parenthesis colon int. Returns the number of elements in this queue.

MyPriorityQueue: https://liveexample.pearsoncmg.com/html/MyPriorityQueue.html
TestPriorityQueue: https://liveexample.pearsoncmg.com/html/TestPriorityQueue.html

<!-- Slide number: 44 -->
# Copyright
This work is protected by United States copyright laws and is provided solely for the use of instructors in teaching their courses and assessing student learning. Dissemination or sale of any part of this work (including on the World Wide Web) will destroy the integrity of the work and is not permitted. The work and materials from it should never be made available to students except by instructors using the accompanying text in their classes. All recipients of this work are expected to abide by these restrictions and to honor the intended pedagogical purposes and the needs of other instructors who rely on these materials.

![Warning](Graphic6.jpg)

### Notes:
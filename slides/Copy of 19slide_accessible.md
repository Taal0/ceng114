<!-- Slide number: 1 -->
# Introduction to Java Programming and Data Structures
Thirteenth Edition

![Front Cover: Introduction to Java Programming and Data Structures Thirteenth Edition by Liang.](Picture3.jpg)
Chapter 19
Generics
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
19.1 To know the benefits of generics (§19.1).
19.2 To use generic classes and interfaces (§19.2).
19.3 To define generic classes and interfaces (§19.3).
19.4 To explain why generic types can improve reliability and readability (§19.3).
19.5 To define and use generic methods and bounded generic types (§19.4).
19.6 To develop a generic sort method to sort an array of Comparable objects (§19.5).
19.7 To use raw types for backward compatibility (§19.6).

<!-- Slide number: 3 -->
# Objectives (2 of 2)
19.8 To explain why wildcard generic types are necessary (§19.7).
19.9 To describe generic-type erasure and list certain restrictions and limitations on generic types caused by type erasure (§19.8).
19.10 To design and implement generic matrix classes (§19.9).

<!-- Slide number: 4 -->
# Why Do You Get a Warning?

![public class ShowUncheckedWarning left brace. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
public static void main left parenthesis String left bracket right bracket args right parenthesis left brace
java.util.ArrayList list =
new java.util.ArrayList left parenthesis right parenthesis semicolon
list.add left parenthesis start quotation marks Java Programming end quotation marks right parenthesis semi colon
right brace
right brace
The code line, list.add left parenthesis start quotation marks Java Programming end quotation marks right parenthesis semi colon, is labeled, to understand the compile warning on this line, you need to learn J D K 1.6 generics.

<!-- Slide number: 5 -->
# Fix the Warning

![A code block as follows. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
public class ShowUncheckedWarning left brace
  public static void main left parenthesis String left bracket right bracket args right parenthesis left brace
    java.util.ArrayList less than symbol String greater than symbol list = (String is highlighted)
      new java.util.ArrayList less than symbol String greater than sy left parenthesis right parenthesis semi colon (String is highlighted)
    list.add left parenthesis start double quotation marks Java Programming end double quotation marks right parenthesis semi colon
right brace
right brace
The last row of code block is labeled, No compile warning on this line.

<!-- Slide number: 6 -->
# What Is Generics?
Generics is the capability to parameterize types. With this capability, you can define a class or a method with generic types that can be substituted using concrete types by the compiler. For example, you may define a generic stack class that stores the elements of a generic type. From this generic class, you may create a stack object for holding strings and a stack object for holding numbers. Here, strings and numbers are concrete types that replace the generic type.

<!-- Slide number: 7 -->
# Why Generics?
The key benefit of generics is to enable errors to be detected at compile time rather than at runtime. A generic class or method permits you to specify allowable types of objects that the class or method may work with. If you attempt to use the class or method with an incompatible object, a compile error occurs.

<!-- Slide number: 8 -->
# Generic Type

![Two code blocks as follows. For long description in Notes pane, press F6.](Picture7.jpg)
Generic Instantiation

![Two code blocks as follows. For long description in Notes pane, press F6.](Picture11.jpg)
Improves reliability

### Notes:
(a) Prior to J D K 1.5.
package java.lang semi colon
public interface comparable left brace
public int compareTo left parenthesis Object o right parenthesis
right brace
(b) J D K 1.5.
package java.lang semi colon
public interface comparable less than symbol T greater than symbol left brace
public int compareTo left parenthesis T o right parenthesis
right brace

(a) Prior to J D K 1.5.
Comparable c = new date left parenthesis right parenthesis semi colon
System.out.println left parenthesis c.compareTo left parenthesis start double quotation marks red end double quotation marks right parenthesis right parenthesis semi colon
The codes c.compareTo left parenthesis start double quotation marks red end double quotation marks right parenthesis are highlighted and labeled, runtime error.
(b) J D K 1.5.
Comparable less than symbol c greater than symbol = new Date left parenthesis right parenthesis semi colon
System.out.println left parenthesis c.compareTo left parenthesis start double quotation marks red end double quotation marks right parenthesis, right parenthesis semi colon
c.compareTo left parenthesis start double quotation marks red end double quotation marks right parenthesis is highlighted and labeled, compile error.

<!-- Slide number: 9 -->
# Generic ArrayList in J D K 1.5

![Two charts shows ArrayList before and since J D K 1.5 as follows. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
(a) ArrayList before J D K 1.5. java.util.ArrayList
+ArrayList left parenthesis right parenthesis
+add left parenthesis o colon Object right parenthesis colon void
+add left parenthesis index colon int, o colon Object right parenthesis colon void
+clear left parenthesis right parenthesis colon void
+contains left parenthesis o colon Object right parenthesis colon Boolean
+get left parenthesis index colon int right parenthesis colon Object
+indexOf left parenthesis o colon Object right parenthesis colon int
+isEmpty left parenthesis right parenthesis colon Boolean
+lastIndexOf left parenthesis o colon Object right parenthesis colon int
+remove left parenthesis o colon Object right parenthesis colon Boolean
+size left parenthesis right parenthesis colon int
+remove left parenthesis index colon int right parenthesis colon Boolean
+set left parenthesis index colon int, o colon Object right parenthesis colon Object
(b) ArrayList since J D K 1.5. java.util.ArrayList less than symbol E greater than symbol.
+ArrayList left parenthesis  right parenthesis
+add left parenthesis o colon E right parenthesis  colon void
+add left parenthesis index colon int, o colon E right parenthesis  colon void
+clear left parenthesis  right parenthesis  colon void
+contains left parenthesis o colon Object right parenthesis  colon Boolean
+get left parenthesis index colonint right parenthesis  colon E
+indexOf left parenthesis o colon Object right parenthesis  colon int
+isEmpty left parenthesis  right parenthesis  colon boolean
+lastIndexOf left parenthesis o colon Object right parenthesis  colon int
+remove left parenthesis o colon Object right parenthesis  colon boolean
+size left parenthesis  right parenthesis  colon int
+remove left parenthesis index colon int right parenthesis  colon boolean
+set left parenthesis index colon int, o colon E right parenthesis  colon E.

<!-- Slide number: 10 -->
# No Casting Needed
ArrayList<Double> list = new ArrayList<>();
list.add(5.5); // 5.5 is automatically converted to new Double(5.5)
list.add(3.0); // 3.0 is automatically converted to new Double(3.0)
Double doubleObject = list.get(0); // No casting is needed
double d = list.get(1); // Automatically converted to double

<!-- Slide number: 11 -->
# Declaring Generic Classes and Interfaces

![Seven rows of codes for GenericStack less than symbol E greater than symbol as follows. For long description in Notes pane, press F6.](Picture5.jpg)
GenericStack

### Notes:
Row 1. List colon java.util.ArrayList less than symbol E greater than symbol. An array list to store elements.
Row 2. +GenericStack left parenthesis right parenthesis. Creates an empty stack.
Row 3. +getSize left parenthesis right parenthesis colon int. Returns the number of elements in this stack.
Row 4. +peek left parenthesis right parenthesis colon E. Returns the top element in this stack.
Row 5. +pop left parenthesis right parenthesis colon E. Returns and removes the top element in this stack.
Row 6. +push left parenthesis o colon E right parenthesis colon void. Adds a new element to the top of this stack.
Row 7. +isEmpty left parenthesis right parenthesis colon Boolean. Returns true if the stack is empty.

GenericStack: https://liveexample.pearsoncmg.com/html/GenericStack.html

<!-- Slide number: 12 -->
# Generic Static Methods
public static <E> void print(E[] list) {
for (int i = 0; i < list.length; i++)
System.out.print(list[i] + " ");
System.out.println();
}
public static void print(Object[] list) {
for (int i = 0; i < list.length; i++)
System.out.print(list[i] + " ");
System.out.println();
}

<!-- Slide number: 13 -->
# Bounded Generic Type
public static void main(String[] args ) {
Rectangle rectangle = new Rectangle(2, 2);
Circle circle = new Circle (2);
System.out.println("Same area? " +
equalArea(rectangle, circle));
}

public static <E extends GeometricObject> boolean
equalArea(E object1, E object2) {
return object1.getArea() == object2.getArea();
}

<!-- Slide number: 14 -->
# Raw Type and Backward Compatibility
// raw type
ArrayList list = new ArrayList();

This is roughly equivalent to
ArrayList<Object> list = new ArrayList<Object>();

<!-- Slide number: 15 -->
# Raw Type Is Unsafe
// Max.java: Find a maximum object
public class Max {
/** Return the maximum between two objects */
public static Comparable max(Comparable o1, Comparable o2) {
if (o1.compareTo(o2) > 0)
return o1;
else
return o2;
}
}
Runtime Error:
Max.max("Welcome", 23); // No compile error

<!-- Slide number: 16 -->
# Avoiding Unsafe Raw Types
Use
new ArrayList<ConcreteType>()
Instead of
new ArrayList();
TestArrayListNew

### Notes:
TestArrayListNew: https://liveexample.pearsoncmg.com/html/TestArrayListNew.html

<!-- Slide number: 17 -->
# Make It Safe
// Max1.java: Find a maximum object
public class Max1 {
/** Return the maximum between two objects */
public static <E extends Comparable<E>> E max(E o1, E o2) {
if (o1.compareTo(o2) > 0)
return o1;
else
return o2;
}
}
Max.max("Welcome", 23);

<!-- Slide number: 18 -->
# Wildcards
Why wildcards are necessary? See this example.
WildCardNeedDemo
unbounded wildcard
?
bounded wildcard
? extends T
? super T
lower bound wildcard
AnyWildCardDemo
SuperWildCardDemo

### Notes:
WildCardNeedDemo: https://liveexample.pearsoncmg.com/html/WildCardNeedDemo.html
AnyWildCardDemo: https://liveexample.pearsoncmg.com/html/AnyWildCardDemo.html
SuperWildCardDemo: https://liveexample.pearsoncmg.com/html/SuperWildCardDemo.html

<!-- Slide number: 19 -->
# Generic Types and Wildcard Types

![An illustration shows the generic types and wildcard types objects. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
An arrow each from a question mark, question mark super E, and E's superclass points to Object. Two arrows from E point to question mark super E and E's superclass. An arrow each from E's subclass and question mark extends E points to E.
An arrow from A less than symbol question mark greater than symbol leads to Object. An arrow each from A less than symbol question mark extends B greater than symbol leads to A less than symbol question mark greater than symbol. An arrow each from A less than symbol B's subclass greater than symbol and A less than symbol B greater than symbol leads to  A less than symbol question mark extends B greater than symbol. An arrow each from A less than symbol B greater than symbol and A less than symbol B's subclass greater than symbol leads to  A less than symbol question mark super B greater than symbol.

<!-- Slide number: 20 -->
# Erasure and Restrictions on Generics
Generics are implemented using an approach called type erasure. The compiler uses the generic type information to compile the code, but erases it afterwards. So the generic information is not available at runtime. This approach enables the generic code to be backward-compatible with the legacy code that uses raw types.

<!-- Slide number: 21 -->
# Compile Time Checking
For example, the compiler checks whether generics is used correctly for the following code in (a) and translates it into the equivalent code in (b) for runtime use. The code in (b) uses the raw type.

![Two code blocks as follows. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
(a) ArrayList less than symbol String greater than symbol list = new ArrayList less than symbol greater than symbol left parenthesis right parenthesis semi colon
list.add left parenthesis start double quotation marks Oklahoma end double quotation marks right parenthesis semi colon
String state = list.get left parenthesis 0 right parenthesis semi colon
(b) ArrayList less = new ArrayList left parenthesis right parenthesis semi colon
list.add left parenthesis Oklahoma right parenthesis semi colon
String state = left parenthesis String right parenthesis left parenthesis list.get left parenthesis 0 right parenthesis semi right parenthesis colon

<!-- Slide number: 22 -->
# Important Facts
It is important to note that a generic class is shared by all its instances regardless of its actual generic type.
GenericStack<String> stack1 = new GenericStack<>();
GenericStack<Integer> stack2 = new GenericStack<>();
Although GenericStack<String> and GenericStack<Integer> are two types, but there is only one class GenericStack loaded into the J V M.

<!-- Slide number: 23 -->
# Restrictions on Generics
Restriction 1: Cannot Create an Instance of a Generic Type. (i.e., new E()).
Restriction 2: Generic Array Creation is Not Allowed. (i.e., new E[100]).
Restriction 3: A Generic Type Parameter of a Class Is Not Allowed in a Static Context.
Restriction 4: Exception Classes Cannot be Generic.

<!-- Slide number: 24 -->
# Designing Generic Matrix Classes
Objective: This example gives a generic class for matrix arithmetic. This class implements matrix addition and multiplication common for all types of matrices.
GenericMatrix

### Notes:
GenericMatrix: https://liveexample.pearsoncmg.com/html/GenericMatrix.html

<!-- Slide number: 25 -->
# U M L Diagram

![A code block for GenericMatrix less than symbol E extends Number greater than symbol as follows. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
Hash symbol add left parenthesis element1 colon E, element2 colon E right parenthesis colon E
Hash symbol multiply left parenthesis element1 colon E, element2 colon E right parenthesis colon E
#zero left parenthesis right parenthesis colon E
+addMatrix left parenthesis matrix1 colon E left bracket right bracket left bracket right bracket, matrix2 colon E left bracket right bracket left bracket right bracket right parenthesis colon E left bracket right bracket left bracket right bracket
+multiplyMatrix left parenthesis matrix1 E left bracket right bracket left bracket right bracket, matrix2 E left bracket right bracket left bracket right bracket right parenthesis colon E left bracket right bracket left bracket right bracket
+printResult left parenthesis m1 colon Number left bracket right bracket left bracket right bracket, m2 colon Number left bracket right bracket left bracket right bracket, m3 colon Number left bracket right bracket left bracket right bracket, op colon char right parenthesis colon void
 Arrows from IntergMatrix and RationalMatrix lead to this code block.

<!-- Slide number: 26 -->
# Source Code
Objective: This example gives two programs that utilize the GenericMatrix class for integer matrix arithmetic and rational matrix arithmetic.
IntegerMatrix
TestIntegerMatrix
RationalMatrix
TestRationalMatrix

### Notes:
IntegerMatrix: https://liveexample.pearsoncmg.com/html/IntegerMatrix.html
RationalMatrix: https://liveexample.pearsoncmg.com/html/RationalMatrix.html
TestIntegerMatrix: https://liveexample.pearsoncmg.com/html/TestIntegerMatrix.html
TestRationalMatrix: https://liveexample.pearsoncmg.com/html/TestRationalMatrix.html

<!-- Slide number: 27 -->
# Copyright
This work is protected by United States copyright laws and is provided solely for the use of instructors in teaching their courses and assessing student learning. Dissemination or sale of any part of this work (including on the World Wide Web) will destroy the integrity of the work and is not permitted. The work and materials from it should never be made available to students except by instructors using the accompanying text in their classes. All recipients of this work are expected to abide by these restrictions and to honor the intended pedagogical purposes and the needs of other instructors who rely on these materials.

![Warning](Graphic6.jpg)

### Notes:
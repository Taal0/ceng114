<!-- Slide number: 1 -->
# Introduction to Java Programming and Data Structures
Thirteenth Edition

![Front Cover: Introduction to Java Programming and Data Structures Thirteenth Edition by Liang.](Picture8.jpg)
Chapter 11
Inheritance and Polymorphism
Copyright © 2024 Pearson Education, Inc. All Rights Reserved

![Pearson Logo](PicturePlaceholder21.jpg)

### Notes:
If this PowerPoint presentation contains mathematical equations, you may need to check that your computer has the following installed:
1) MathType Plugin
2) Math Player (free versions available)
3) NVDA Reader (free versions available)

Slides in this presentation contain hyperlinks. JAWS users should be able to get a list of links by using INSERT+F7

<!-- Slide number: 2 -->
# Motivations
Suppose you will define classes to model circles, rectangles, and triangles. These classes have many common features. What is the best way to design these classes so to avoid redundancy? The answer is to use inheritance.

<!-- Slide number: 3 -->
# Objectives (1 of 2)
11.1 To define a subclass from a superclass through inheritance (§11.2).
11.2 To invoke the superclass’s constructors and methods using the super keyword (§11.3).
11.3 To override instance methods in the subclass (§11.4).
11.4 To distinguish differences between overriding and overloading (§11.5).
11.5 To explore the toString()method in the Object class (§11.6).
11.6 To discover polymorphism and dynamic binding (§§11.7–11.8).
11.7 To describe casting and explain why explicit downcasting is necessary (§11.9).

<!-- Slide number: 4 -->
# Objectives (2 of 2)
11.8 To explore the equals method in the Object class (§11.10).
11.9 To store, retrieve, and manipulate objects in an ArrayList (§11.11).
11.10 To construct an array list from an array, to sort and shuffle a list, and to obtain max and min element from a list (§11.12).
11.11 To implement a Stack class using ArrayList (§11.13).
11.12 To enable data and methods in a superclass accessible from subclasses using the protected visibility modifier (§11.14).
11.13 To prevent class extending and method overriding using the final modifier (§11.14).
11.14 To automatically generate boilerplate code using Lombok (§11.16).

<!-- Slide number: 5 -->
# Superclasses and Subclasses

![A left upward side computer code shows the Super classes and Subclasses. For long description in Notes pane, press F6.](Picture5.jpg)
GeometricObject
Circle
Rectangle
TestCircleRectangle

### Notes:
It has 3 rows. Row 1, shows the Geometric Object. Row 2, shows the coding and it has 3 lines. Line 1, minus color colon String. Line 2, minus filled colon Boolean. Line 3, minus data C related colon java period util period Date. Row 3, shows the coding and it has 9 lines. Line 1, plus Geometric Object open parenthesis close parenthesis. Line 2, plus Geometric Object open parenthesis color colon String comma. Line 3, filled colon Boolean close parenthesis. Line 4, plus get Color open parenthesis close parenthesis colon String. Line 5, plus set Color open parenthesis color colon String close parenthesis colon void. Line 6, plus is Filled open parenthesis close parenthesis colon Boolean. Line 7, plus set Filled open parenthesis filled colon Boolean close parenthesis colon void. Line 8, plus get Date C related open parenthesis close parenthesis colon java period util period Date. Line 9, plus to String open parenthesis close parenthesis colon String. A left downward side computer code shows the coding and it is divided in 3 rows. Row 1, shows the Circle. Row 2, shows the minus radius colon double. Row 3, has 10 lines. Line 1, plus Circle open parenthesis close parenthesis. Line 2, plus Circle open parenthesis radius colon double close parenthesis. Line 3, plus Circle open parenthesis radius colon double comma color colon String comma. Line 4, filled colon Boolean close parenthesis. Line 5, plus get Radius open parenthesis close parenthesis colon double. Line 6, plus set Radius open parenthesis radius colon double close parenthesis colon void. Line 7, get Area open parenthesis close parenthesis colon double. Line 8, plus get Perimeter open parenthesis close parenthesis colon double. Line 9, plus get Diameter open parenthesis close parenthesis colon double. Line 10, plus print Circle open parenthesis close parenthesis colon void. A right downward side computer code shows the coding and it has 3 rows. Row 1, shows the Rectangle. Row 2, shows the coding for 2 lines. Line 1, minus width colon double. Line 2, minus height colon double. Row 3, also shows the coding for 10 lines. Line 1, plus Rectangle open parenthesis close parenthesis. Line 2, plus Rectangle open parenthesis width colon double comma height colon double close parenthesis. Line 4, color colon String comma filled colon Boolean close parenthesis. Line 5, plus get Width open parenthesis close parenthesis colon double. Line 6, plus set Width open parenthesis width colon double close parenthesis colon void. Line 7, get Height open parenthesis close parenthesis colon double. Line 8, plus set Height open parenthesis height colon double close parenthesis colon void. Line 9, plus get Area open parenthesis close parenthesis colon double. Line 10, plus get Perimeter open parenthesis close parenthesis colon double.

GeometricObject: https://liveexample.pearsoncmg.com/html/SimpleGeometricObject.html
Circle: https://liveexample.pearsoncmg.com/html/CircleFromSimpleGeometricObject.html
Rectangle: https://liveexample.pearsoncmg.com/html/RectangleFromSimpleGeometricObject.html
TestCircleRectangle: https://liveexample.pearsoncmg.com/html/TestCircleRectangle.html

<!-- Slide number: 6 -->
# Are Superclass’s Constructor Inherited?
No. They are not inherited.
They are invoked explicitly or implicitly.
Explicitly using the super keyword.
A constructor is used to construct an instance of a class. Unlike properties and methods, a superclass's constructors are not inherited in the subclass. They can only be invoked from the subclasses' constructors, using the keyword super. If the keyword super is not explicitly used, the superclass's no-arg constructor is automatically invoked.

<!-- Slide number: 7 -->
# Superclass’s Constructor Is Always Invoked
A constructor may invoke an overloaded constructor or its superclass’s constructor. If none of them is invoked explicitly, the compiler puts super() as the first statement in the constructor. For example,

![A left upward side computer code shows the coding for Superclass's Constructor Is Always Invoked. For long description in Notes pane, press F6.](Picture8.jpg)

![A left downward side computer code shows the coding for 3 lines. For long description in Notes pane, press F6.](Picture12.jpg)

### Notes:
It has 2 lines. Line 1, public A open parenthesis close parenthesis open braces. Line 2, close braces. And this is equivalent to right upward side box. A right upward side box shows the coding for 3 lines. Line 1, public A open parenthesis close parenthesis open braces. Line 2, super open parenthesis close parenthesis semicolon. Line 3, close braces.

Line 1, public A open parenthesis double d close parenthesis open braces. Line 2, slash forward slash forward some statements. Line 3, close braces. And this is equivalent to right downward side computer code. A right downward side computer code shows the coding for 4 lines. Line 1, public A open parenthesis double d close parenthesis open braces. Line 2, super open parenthesis close parenthesis semicolon. Line 3, slash forward slash forward some statements. Line 4, close braces.

<!-- Slide number: 8 -->
# Using the Keyword super
The keyword super refers to the superclass of the class in which super appears. This keyword can be used in two ways:
To call a superclass constructor
To call a superclass method

<!-- Slide number: 9 -->
# Caution
You must use the keyword super to call the superclass constructor. Invoking a superclass constructor’s name in a subclass causes a syntax error. Java requires that the statement that uses the keyword super appear first in the constructor.

<!-- Slide number: 10 -->
# Constructor Chaining (1 of 2)
Constructing an instance of a class invokes all the superclasses’ constructors along the inheritance chain. This is known as constructor chaining.
public class Faculty extends Employee {
public static void main(String[] args) {
 new Faculty();
}
  public Faculty() {
 System.out.println("(4) Faculty's no-arg constructor is  invoked");
}
}
class Employee extends Person {
public Employee() {

<!-- Slide number: 11 -->
# Constructor Chaining (2 of 2)
this("(2) Invoke Employee’s overloaded constructor");
System.out.println("(3) Employee's no-arg constructor is invoked");
}
public Employee(String s) {
System.out.println(s);
}
}
class Person {
public Person() {
System.out.println("(1) Person's no-arg constructor is invoked");
}
}

<!-- Slide number: 12 -->
# Trace Execution (1 of 9)

![A computer code shows the coding for Trace Execution. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
It has 26 lines. Line 1, public class Faculty extends Employee open braces. Line 2, public static void main open parenthesis String open parenthesis close parenthesis args close parenthesis open braces and this line shows the Start from the main method. Line 3, new Faculty open parenthesis close parenthesis semicolon. Line 4, close braces. Line 5, Blank. Line 6, public Faculty open parenthesis close parenthesis open braces. Line 7, System period out period print ln open parenthesis double quote open parenthesis 4 close parenthesis Faculty's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 8, close braces. Line 9, close braces. Line 10, blank. Line 11, class Employee extends Person open braces. Line 12, public employee open parenthesis close parenthesis open braces. Line 13, this open parenthesis double quote open parenthesis 2 close parenthesis Invoke Employee's overloaded constructor double quote close parenthesis semicolon. Line 14, System period out period print ln open parenthesis double quote open parenthesis 3 close parenthesis Employee's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 15, close braces. Line 16, blank. Line 17, public Employee open parenthesis String s close parenthesis open braces. Line 18, System period out period print ln open parenthesis s close parenthesis semicolon. Line 19, close braces. Line 20, close braces. Line 21, blank. Line 22, class Person open braces. Line 23, public Person open parenthesis close parenthesis open braces. Line 24, System period out period print ln open parenthesis double quote open parenthesis 1 close parenthesis Person's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 25, close braces. Line 26, close braces.

<!-- Slide number: 13 -->
# Trace Execution (2 of 9)

![A computer code shows the coding for Trace Execution. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
It has 26 lines. Line 1, public class Faculty extends Employee open braces. Line 2, public static void main open parenthesis String open parenthesis close parenthesis args close parenthesis open braces. Line 3, new Faculty open parenthesis close parenthesis semicolon and this line shows the Invoke Faculty constructor. Line 4, close braces. Line 5, Blank. Line 6, public Faculty open parenthesis close parenthesis open braces. Line 7, System period out period print ln open parenthesis double quote open parenthesis 4 close parenthesis Faculty's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 8, close braces. Line 9, close braces. Line 10, blank. Line 11, class Employee extends Person open braces. Line 12, public employee open parenthesis close parenthesis open braces. Line 13, this open parenthesis double quote open parenthesis 2 close parenthesis Invoke Employee's overloaded constructor double quote close parenthesis semicolon. Line 14, System period out period print ln open parenthesis double quote open parenthesis 3 close parenthesis Employee's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 15, close braces. Line 16, blank. Line 17, public Employee open parenthesis String s close parenthesis open braces. Line 18, System period out period print ln open parenthesis s close parenthesis semicolon. Line 19, close braces. Line 20, close braces. Line 21, blank. Line 22, class Person open braces. Line 23, public Person open parenthesis close parenthesis open braces. Line 24, System period out period print ln open parenthesis double quote open parenthesis 1 close parenthesis Person's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 25, close braces. Line 26, close braces.

<!-- Slide number: 14 -->
# Trace Execution (3 of 9)

![A computer code shows the coding for Trace Execution. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
It has 26 lines. Line 1, public class Faculty extends Employee open braces. Line 2, public static void main open parenthesis String open parenthesis close parenthesis args close parenthesis open braces. Line 3, new Faculty open parenthesis close parenthesis semicolon. Line 4, close braces. Line 5, Blank. Line 6, public Faculty open parenthesis close parenthesis open braces. Line 7, System period out period print ln open parenthesis double quote open parenthesis 4 close parenthesis Faculty's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 8, close braces. Line 9, close braces. Line 10, blank. Line 11, class Employee extends Person open braces. Line 12, public employee open parenthesis close parenthesis open braces and this line shows the Invoke Employee's no hyphen arg constructor. Line 13, this open parenthesis double quote open parenthesis 2 close parenthesis Invoke Employee's overloaded constructor double quote close parenthesis semicolon. Line 14, System period out period print ln open parenthesis double quote open parenthesis 3 close parenthesis Employee's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 15, close braces. Line 16, blank. Line 17, public Employee open parenthesis String s close parenthesis open braces. Line 18, System period out period print ln open parenthesis s close parenthesis semicolon. Line 19, close braces. Line 20, close braces. Line 21, blank. Line 22, class Person open braces. Line 23, public Person open parenthesis close parenthesis open braces. Line 24, System period out period print ln open parenthesis double quote open parenthesis 1 close parenthesis Person's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 25, close braces. Line 26, close braces.

<!-- Slide number: 15 -->
# Trace Execution (4 of 9)

![A computer code shows the coding for Trace Execution. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
It has 26 lines. Line 1, public class Faculty extends Employee open braces. Line 2, public static void main open parenthesis String open parenthesis close parenthesis args close parenthesis open braces. Line 3, new Faculty open parenthesis close parenthesis semicolon. Line 4, close braces. Line 5, Blank. Line 6, public Faculty open parenthesis close parenthesis open braces. Line 7, System period out period print ln open parenthesis double quote open parenthesis 4 close parenthesis Faculty's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 8, close braces. Line 9, close braces. Line 10, blank. Line 11, class Employee extends Person open braces. Line 12, public employee open parenthesis close parenthesis open braces. Line 13, this open parenthesis double quote open parenthesis 2 close parenthesis Invoke Employee's overloaded constructor double quote close parenthesis semicolon and this line shows the Invoke Employee open parenthesis String close parenthesis constructor. Line 14, System period out period print ln open parenthesis double quote open parenthesis 3 close parenthesis Employee's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 15, close braces. Line 16, blank. Line 17, public Employee open parenthesis String s close parenthesis open braces. Line 18, System period out period print ln open parenthesis s close parenthesis semicolon. Line 19, close braces. Line 20, close braces. Line 21, blank. Line 22, class Person open braces. Line 23, public Person open parenthesis close parenthesis open braces. Line 24, System period out period print ln open parenthesis double quote open parenthesis 1 close parenthesis Person's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 25, close braces. Line 26, close braces.

<!-- Slide number: 16 -->
# Trace Execution (5 of 9)

![A computer code shows the coding for Trace Execution. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
It has 26 lines. Line 1, public class Faculty extends Employee open braces. Line 2, public static void main open parenthesis String open parenthesis close parenthesis args close parenthesis open braces. Line 3, new Faculty open parenthesis close parenthesis semicolon. Line 4, close braces. Line 5, Blank. Line 6, public Faculty open parenthesis close parenthesis open braces. Line 7, System period out period print ln open parenthesis double quote open parenthesis 4 close parenthesis Faculty's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 8, close braces. Line 9, close braces. Line 10, blank. Line 11, class Employee extends Person open braces. Line 12, public employee open parenthesis close parenthesis open braces. Line 13, this open parenthesis double quote open parenthesis 2 close parenthesis Invoke Employee's overloaded constructor double quote close parenthesis semicolon. Line 14, System period out period print ln open parenthesis double quote open parenthesis 3 close parenthesis Employee's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 15, close braces. Line 16, blank. Line 17, public Employee open parenthesis String s close parenthesis open braces. Line 18, System period out period print ln open parenthesis s close parenthesis semicolon. Line 19, close braces. Line 20, close braces. Line 21, blank. Line 22, class Person open braces. Line 23, public Person open parenthesis close parenthesis open braces and this line shows the Invoke Person open parenthesis close parenthesis constructor. Line 24, System period out period print ln open parenthesis double quote open parenthesis 1 close parenthesis Person's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 25, close braces. Line 26, close braces.

<!-- Slide number: 17 -->
# Trace Execution (6 of 9)

![A computer code shows the coding for Trace Execution. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
It has 26 lines. Line 1, public class Faculty extends Employee open braces. Line 2, public static void main open parenthesis String open parenthesis close parenthesis args close parenthesis open braces. Line 3, new Faculty open parenthesis close parenthesis semicolon. Line 4, close braces. Line 5, Blank. Line 6, public Faculty open parenthesis close parenthesis open braces. Line 7, System period out period print ln open parenthesis double quote open parenthesis 4 close parenthesis Faculty's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 8, close braces. Line 9, close braces. Line 10, blank. Line 11, class Employee extends Person open braces. Line 12, public employee open parenthesis close parenthesis open braces. Line 13, this open parenthesis double quote open parenthesis 2 close parenthesis Invoke Employee's overloaded constructor double quote close parenthesis semicolon. Line 14, System period out period print ln open parenthesis double quote open parenthesis 3 close parenthesis Employee's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 15, close braces. Line 16, blank. Line 17, public Employee open parenthesis String s close parenthesis open braces. Line 18, System period out period print ln open parenthesis s close parenthesis semicolon. Line 19, close braces. Line 20, close braces. Line 21, blank. Line 22, class Person open braces. Line 23, public Person open parenthesis close parenthesis open braces. Line 24, System period out period print ln open parenthesis double quote open parenthesis 1 close parenthesis Person's no hyphen arg constructor is invoked double quote close parenthesis semicolon and this line shows the Execute print ln. Line 25, close braces. Line 26, close braces.

<!-- Slide number: 18 -->
# Trace Execution (7 of 9)

![A computer code shows the coding for Trace Execution. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
It has 26 lines. Line 1, public class Faculty extends Employee open braces. Line 2, public static void main open parenthesis String open parenthesis close parenthesis args close parenthesis open braces. Line 3, new Faculty open parenthesis close parenthesis semicolon. Line 4, close braces. Line 5, Blank. Line 6, public Faculty open parenthesis close parenthesis open braces. Line 7, System period out period print ln open parenthesis double quote open parenthesis 4 close parenthesis Faculty's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 8, close braces. Line 9, close braces. Line 10, blank. Line 11, class Employee extends Person open braces. Line 12, public employee open parenthesis close parenthesis open braces. Line 13, this open parenthesis double quote open parenthesis 2 close parenthesis Invoke Employee's overloaded constructor double quote close parenthesis semicolon. Line 14, System period out period print ln open parenthesis double quote open parenthesis 3 close parenthesis Employee's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 15, close braces. Line 16, blank. Line 17, public Employee open parenthesis String s close parenthesis open braces. Line 18, System period out period print ln open parenthesis s close parenthesis semicolon and this line shows the Execute print ln. Line 19, close braces. Line 20, close braces. Line 21, blank. Line 22, class Person open braces. Line 23, public Person open parenthesis close parenthesis open braces. Line 24, System period out period print ln open parenthesis double quote open parenthesis 1 close parenthesis Person's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 25, close braces. Line 26, close braces.

<!-- Slide number: 19 -->
# Trace Execution (8 of 9)

![The computer code shows the coding for Trace Execution. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
The computer code has 26 lines. Line 1, public class Faculty extends Employee open braces. Line 2, public static void main open parenthesis String open parenthesis close parenthesis args close parenthesis open braces. Line 3, new Faculty open parenthesis close parenthesis semicolon. Line 4, close braces. Line 5, Blank. Line 6, public Faculty open parenthesis close parenthesis open braces. Line 7, System period out period print ln open parenthesis double quote open parenthesis 4 close parenthesis Faculty's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 8, close braces. Line 9, close braces. Line 10, blank. Line 11, class Employee extends Person open braces. Line 12, public employee open parenthesis close parenthesis open braces. Line 13, this open parenthesis double quote open parenthesis 2 close parenthesis Invoke Employee's overloaded constructor double quote close parenthesis semicolon. Line 14, System period out period print ln open parenthesis double quote open parenthesis 3 close parenthesis Employee's no hyphen arg constructor is invoked double quote close parenthesis semicolon and this line shows the Execute print ln. Line 15, close braces. Line 16, blank. Line 17, public Employee open parenthesis String s close parenthesis open braces. Line 18, System period out period print ln open parenthesis s close parenthesis semicolon. Line 19, close braces. Line 20, close braces. Line 21, blank. Line 22, class Person open braces. Line 23, public Person open parenthesis close parenthesis open braces. Line 24, System period out period print ln open parenthesis double quote open parenthesis 1 close parenthesis Person's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 25, close braces. Line 26, close braces.

<!-- Slide number: 20 -->
# Trace Execution (9 of 9)

![The computer code shows the coding for Trace Execution. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
The computer code has 26 lines. Line 1, public class Faculty extends Employee open braces. Line 2, public static void main open parenthesis String open parenthesis close parenthesis args close parenthesis open braces. Line 3, new Faculty open parenthesis close parenthesis semicolon. Line 4, close braces. Line 5, Blank. Line 6, public Faculty open parenthesis close parenthesis open braces. Line 7, System period out period print ln open parenthesis double quote open parenthesis 4 close parenthesis Faculty's no hyphen arg constructor is invoked double quote close parenthesis semicolon and this line shows the Execute print ln. Line 8, close braces. Line 9, close braces. Line 10, blank. Line 11, class Employee extends Person open braces. Line 12, public employee open parenthesis close parenthesis open braces. Line 13, this open parenthesis double quote open parenthesis 2 close parenthesis Invoke Employee's overloaded constructor double quote close parenthesis semicolon. Line 14, System period out period print ln open parenthesis double quote open parenthesis 3 close parenthesis Employee's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 15, close braces. Line 16, blank. Line 17, public Employee open parenthesis String s close parenthesis open braces. Line 18, System period out period print ln open parenthesis s close parenthesis semicolon. Line 19, close braces. Line 20, close braces. Line 21, blank. Line 22, class Person open braces. Line 23, public Person open parenthesis close parenthesis open braces. Line 24, System period out period print ln open parenthesis double quote open parenthesis 1 close parenthesis Person's no hyphen arg constructor is invoked double quote close parenthesis semicolon. Line 25, close braces. Line 26, close braces.

<!-- Slide number: 21 -->
# Example on the Impact of a Superclass Without no-arg Constructor
Find out the errors in the program:
public class Apple extends Fruit {
}
class Fruit {
public Fruit(String name) {
 System.out.println("Fruit's constructor is
   invoked");
}
}

<!-- Slide number: 22 -->
# Defining a Subclass
A subclass inherits from a superclass. You can also:
Add new properties
Add new methods
Override the methods of the superclass

<!-- Slide number: 23 -->
# Calling Superclass Methods
You could rewrite the printCircle() method in the Circle class as follows:
public void printCircle() {
System.out.println("The circle is created " +
super.getDateCreated() + " and the radius is " + radius);
}

<!-- Slide number: 24 -->
# Overriding Methods in the Superclass
A subclass inherits methods from a superclass. Sometimes it is necessary for the subclass to modify the implementation of a method defined in the superclass. This is referred to as method overriding.
public class Circle extends GeometricObject {
// Other methods are omitted

/** Override the toString method defined in
    GeometricObject */
public String toString() {
  return super.toString() + "\nradius is " + radius;
}
}

<!-- Slide number: 25 -->
# Note (1 of 4)
An instance method can be overridden only if it is accessible. Thus a private method cannot be overridden, because it is not accessible outside its own class. If a method defined in a subclass is private in its superclass, the two methods are completely unrelated.

<!-- Slide number: 26 -->
# Note (2 of 4)
Like an instance method, a static method can be inherited. However, a static method cannot be overridden. If a static method defined in the superclass is redefined in a subclass, the method defined in the superclass is hidden.

<!-- Slide number: 27 -->
# Overriding versus Overloading

![A left side computer code shows the Overriding vs. Overloading. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
The computer code has 18 lines. Line 1, public class Test open braces. Line 2, public static void main open parenthesis String open parenthesis close parenthesis args close parenthesis open braces. Line 3, A a equal to new A open parenthesis close parenthesis semicolon. Line 4, a period p open parenthesis 10 close parenthesis semicolon. Line 5, a period p open parenthesis 10.0 close parenthesis semicolon. Line 6 close braces. Line 7, close braces. Line 8, class B open braces. Line 9, public void p open parenthesis double i close parenthesis open braces. Line 10, System period out period print ln open parenthesis i address 2 close parenthesis semicolon. Line 11, close braces. Line 12, close braces. Line 13, class A extends B open braces. Line 13, slash forward slash forward This method overrides the method in B. Line 14, public void p open parenthesis double i close parenthesis open braces. Line 15, System period out period print ln open parenthesis i close parenthesis semicolon. Line 16, close braces. Line 17, close braces. A right side computer code shows the Overriding vs. Overloading. The computer code has 18 lines. Line 1, public class Test open braces. Line 2, public static void main open parenthesis String open parenthesis close parenthesis args close parenthesis open braces. Line 3, A a equal to new A open parenthesis close parenthesis semicolon. Line 4, a period p open parenthesis 10 close parenthesis semicolon. Line 5, a period p open parenthesis 10.0 close parenthesis semicolon. Line 6 close braces. Line 7, close braces. Line 8, class B open braces. Line 9, public void p open parenthesis double i close parenthesis open braces. Line 10, System period out period print ln open parenthesis i address 2 close parenthesis semicolon. Line 11, close braces. Line 12, close braces. Line 13, class A extends B open braces. Line 13, slash forward slash forward This method overloads the method in B. Line 14, public void p open parenthesis print i close parenthesis open braces. Line 15, System period out period print ln open parenthesis i close parenthesis semicolon. Line 16, close braces. Line 17, close braces.

<!-- Slide number: 28 -->
# The Object Class and Its Methods
Every class in Java is descended from the java.lang.Object class. If no inheritance is specified when a class is defined, the superclass of the class is Object.

![A left side computer code shows the The Object Class. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
The computer code has 3 lines. Line 1, public class Circle open braces. Line 2, period period period. Line 3, close braces and this box is equivalent to right hand side text box. A right side computer code shows the Object Class. The computer code has 3 lines. Line 1, public class Circle extends Object open braces. Line 2, period period period. Line 3, close braces.

<!-- Slide number: 29 -->
# The toString() Method in Object
The toString() method returns a string representation of the object. The default implementation returns a string consisting of a class name of which the object is an instance, the at sign (@), and a number representing this object.
Loan loan = new Loan();
System.out.println(loan.toString());
The code displays something like Loan@15037e5 . This message is not very helpful or informative. Usually you should override the toString method so that it returns a digestible string representation of the object.

<!-- Slide number: 30 -->
# Polymorphism
Polymorphism means that a variable of a supertype can refer to a subtype object.
A class defines a type. A type defined by a subclass is called a subtype, and a type defined by its superclass is called a supertype. Therefore, you can say that Circle is a subtype of GeometricObject and GeometricObject is a supertype for Circle.
PolymorphismDemo

### Notes:
PolymorphismDemo: https://liveexample.pearsoncmg.com/html/PolymorphismDemo.html

<!-- Slide number: 31 -->
# Polymorphism, Dynamic Binding and Generic Programming (1 of 2)

![A left side computer code shows the Polymorphism, Dynamic Binding and Generic Programming. For long description in Notes pane, press F6.](Picture5.jpg)
DynamicBindingDemo

### Notes:
The computer code has 23 lines. Line 1, public class Polymorphism Demo open braces. Line 2, public static void main open parenthesis String open parenthesis close parenthesis args close parenthesis open braces. Line 3, m open parenthesis new Graduate open parenthesis close parenthesis close parenthesis semicolon. Line 4, m open parenthesis new Student open parenthesis close parenthesis close parenthesis semicolon. Line 5, m open parenthesis new Person open parenthesis close parenthesis close parenthesis semicolon. Line 6, m open parenthesis new Object open parenthesis close parenthesis open parenthesis semicolon. Line 7, close braces. Line 8, public static void m open parenthesis Object x close parenthesis open braces. Line 9, System period out period print ln open parenthesis x period to String open parenthesis close parenthesis close parenthesis semicolon. Line 10, close braces. Line 11, close braces. Line 12 class Graduate Student extends Student open braces. Line 13, close braces. Line 14, class Student extends Person open braces. Line 15, public String to String open parenthesis close parenthesis open braces. Line 16, return double quote Student double quote semicolon. Line 17, close braces. Line 18, close braces. Line 19, class Person extends Object open braces. Line 19, public String to String open parenthesis close parenthesis open braces. Lime 20, return double quote Person double quote semicolon. Line 21, close braces. Line 22, close braces.

DynamicBindingDemo: https://liveexample.pearsoncmg.com/html/DynamicBindingDemo.html

<!-- Slide number: 32 -->
# Polymorphism, Dynamic Binding and Generic Programming (2 of 2)
An object of a subtype can be used wherever its supertype value is required. This feature is known as polymorphism.
When the method m(Object x) is executed, the argument x’s toString method is invoked. x may be an instance of GraduateStudent, Student, Person, or Object. Classes GraduateStudent, Student, Person, and Object have their own implementation of the toString method. Which implementation is used will be determined dynamically by the Java Virtual Machine at runtime. This capability is known as dynamic binding.

<!-- Slide number: 33 -->
# Dynamic Binding
Dynamic binding works as follows: Suppose an object o is an instance of
is a subclass of
classes
where
is the most
is a subclass of
is a subclass of
That is,
is the Object
general class, and
is the most specific class. In Java,
class. If o invokes a method p, the J V M searches the implementation for the
method p in
in this order, until it is found. Once an
implementation is found, the search stops and the first-found implementation is invoked.

![An object shows the Dynamic Binding and makes an series of instance of classes that is C(1), C(2), upto , C(n-1), C(n). In an object there are 4 boxes. For long description in Notes pane, press F6.](Picture42.jpg)

### Notes:
1st box is for C(n) and it signifies Object. 2nd box is for C(n-1) and it makes an arrow forward to represent the 1st box that is for C(n). 3rd box is for C(2) and it's also makes an arrow forward to represent the forward series. 4th box is for C(1) and it's also makes an arrow forward to represent the Box 3rd which is for C(1).

<!-- Slide number: 34 -->
# Method Matching versus Binding
Matching a method signature and binding a method implementation are two issues. The compiler finds a matching method according to parameter type, number of parameters, and order of the parameters at compilation time. A method may be implemented in several subclasses. The Java Virtual Machine dynamically binds the implementation of the method at runtime.

<!-- Slide number: 35 -->
# Generic Programming (1 of 2)
public class PolymorphismDemo {
public static void main(String[] args) {
m(new GraduateStudent());
m(new Student());
m(new Person());
m(new Object());
}
public static void m(Object x) {
System.out.println(x.toString());
}
}
class GraduateStudent extends Student {
}
class Student extends Person {
public String toString() {
return "Student";
}
}
class Person extends Object {
public String toString() {
return "Person";
}
}

<!-- Slide number: 36 -->
# Generic Programming (2 of 2)
Polymorphism allows methods to be used generically for a wide range of object arguments. This is known as generic programming. If a method’s parameter type is a superclass (e.g., Object), you may pass an object to this method of any of the parameter’s subclasses (e.g., Student or String). When an object (e.g., a Student object or a String object) is used in the method, the particular implementation of the method of the object that is invoked (e.g., toString) is determined dynamically.

<!-- Slide number: 37 -->
# Casting Objects
You have already used the casting operator to convert variables of one primitive type to another. Casting can also be used to convert an object of one class type to another within an inheritance hierarchy. In the preceding section, the statement
m(new Student());
assigns the object new Student() to a parameter of the Object type. This statement is equivalent to:

![Object o = new Student left parenthesis left parenthesis semi colon forward slash forward slash Implicit casting m of o semi colon. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
Object o = new Student left parenthesis left parenthesis is labeled, the statement Object o = new Student left parenthesis left parenthesis, known as implicit casting, is legal because an instance of Student is automatically an instance of Object.

<!-- Slide number: 38 -->
# Why Casting Is Necessary?
Suppose you want to assign the object reference o to a variable of the Student type using the following statement:
Student b = o;
A compile error would occur. Why does the statement Object o = new Student() work and the statement Student b = o doesn’t? This is because a Student object is always an instance of Object, but an Object is not necessarily an instance of Student. Even though you can see that o is really a Student object, the compiler is not so clever to know it. To tell the compiler that o is a Student object, use an explicit casting. The syntax is similar to the one used for casting among primitive data types. Enclose the target object type in parentheses and place it before the object to be cast, as follows:
Student b = (Student)o; // Explicit casting

<!-- Slide number: 39 -->
# Casting From Superclass to Subclass
Explicit casting must be used when casting an object from a superclass to a subclass. This type of casting may not always succeed.
Apple x = (Apple)fruit;
Orange x = (Orange)fruit;

<!-- Slide number: 40 -->
# The instanceof Operator
Use the instanceof operator to test whether an object is an instance of a class:
Object myObject = new Circle();
... // Some lines of code
/** Perform casting if myObject is an instance of Circle */
if (myObject instanceof Circle) {
System.out.println("The circle diameter is " +
((Circle)myObject).getDiameter());
...
}

<!-- Slide number: 41 -->
# Java 16 instanceof Pattern Matching
When using the if statement to test if an object is an instance of a class, you can specify a binding variable. If the result of the instanceof operator is true, then the object being tested is assigned to the binding variable. This new syntax, known as instanceof pattern matching, became a standard feature since Java 16. You can simplify the code in lines 15-24 using this new feature as follows:

<!-- Slide number: 42 -->
# Java 16 New Features on instanceof
if (myObject instanceof Circle circle) {
  System.out.println("The circle area is " +
    circle.getArea());
  System.out.println("The circle diameter is " +
    circle.getDiameter());
}
else if (myObject instanceof Rectangle rectangle) {
  System.out.println("The rectangle area is " +
    rectangle.getArea());
}

<!-- Slide number: 43 -->
# Tip
To help understand casting, you may also consider the analogy of fruit, apple, and orange with the Fruit class as the superclass for Apple and Orange. An apple is a fruit, so you can always safely assign an instance of Apple to a variable for Fruit. However, a fruit is not necessarily an apple, so you have to use explicit casting to assign an instance of Fruit to a variable of Apple.

<!-- Slide number: 44 -->
# Example: Demonstrating Polymorphism and Casting
This example creates two geometric objects: a circle, and a rectangle, invokes the displayGeometricObject method to display the objects. The displayGeometricObject displays the area and diameter if the object is a circle, and displays area if the object is a rectangle.
CastingDemo

### Notes:
CastingDemo: https://liveexample.pearsoncmg.com/html/CastingDemo.html

<!-- Slide number: 45 -->
# The equals Method
The equals() method compares the contents of two objects. The default implementation of the equals method in the Object class is as follows:
public boolean equals(Object obj){
return this == obj;
}
public boolean equals(Object o) {
if (o instanceof Circle) {
  return radius ==((Circle)o).radius;
}
else
 return false;
}
For example, the equals method is overridden in the Circle class.

<!-- Slide number: 46 -->
# Note (3 of 4)
The == comparison operator is used for comparing two primitive data type values or for determining whether two objects have the same references. The equals method is intended to test whether two objects have the same contents, provided that the method is modified in the defining class of the objects. The == operator is stronger than the equals method, in that the == operator checks whether the two reference variables refer to the same object.

<!-- Slide number: 47 -->
# The ArrayList Class
You can create an array to store objects. But the array’s size is fixed once the array is created. Java provides the ArrayList class that can be used to store an unlimited number of objects.

![The computer code shows the Array List Class. For long description in Notes pane, press F6.](Picture9.jpg)

### Notes:
The computer code has 2 rows. Row 1, shows the java period util period Array List less than E greater than. Row 2, has 13 lines. Line 1, plus Array List open parenthesis close parenthesis. Line 2, plus add open parenthesis o colon E close parenthesis colon void. Line 3, plus add open parenthesis index colon int comma o colon E close parenthesis colon void. Line 4, plus clear open parenthesis close parenthesis colon void. Line 5, plus contains open parenthesis o colon Object close parenthesis colon Boolean. Line 6, plus get open parenthesis index colon int close parenthesis int. Line 7, plus index Of open parenthesis o colon Object close parenthesis colon int. Line 8, plus is Empty open parenthesis close parenthesis colon Boolean. Line 9, plus last Index Of open parenthesis o colon Object close parenthesis colon int. Line 10, plus remove open parenthesis o colon Object close parenthesis colon Boolean. Line 11, plus size open parenthesis close parenthesis colon int. Line 12, plus remove open parenthesis index colon int close parenthesis colon Boolean. Line 13, plus set open parenthesis index colon int comma o colon E close parenthesis colon E.

<!-- Slide number: 48 -->
# Generic Type
ArrayList is known as a generic class with a generic type E. You can specify a concrete type to replace E when creating an ArrayList. For example, the following statement creates an ArrayList and assigns its reference to variable cities. This ArrayList object can be used to store strings.
ArrayList<String> cities = new ArrayList<String>();
ArrayList<String> cities = new ArrayList<>();
TestArrayList

### Notes:
TestArrayList: https://liveexample.pearsoncmg.com/html/TestArrayList.html

<!-- Slide number: 49 -->
# Differences and Similarities Between Arrays and ArrayList
| Operation | Array | ArrayList |
| --- | --- | --- |
| Creating an array/ArrayList | String[] a = new String[10] | ArrayList<String> list = new ArrayList<>(); |
| Accessing an element | a[index] | list.get(index); |
| Updating an element | a[index] = "London"; | list.set(index, "London"); |
| Returning size | a.length | list.size(); |
| Adding a new element | Blank | list.add("London"); |
| Inserting a new element | Blank | list.add(index, "London"); |
| Removing an element | Blank | list.remove(index); |
| Removing an element | Blank | list.remove(Object); |
| Removing all elements | Blank | list.clear(); |
DistinctNumbers

### Notes:
DistinctNumbers: https://liveexample.pearsoncmg.com/html/DistinctNumbers.html

<!-- Slide number: 50 -->
# Array Lists From/to Arrays
Creating an ArrayList from an array of objects:
String[] array = {"red", "green", "blue"};
ArrayList<String> list = new ArrayList<>(Arrays.asList(array));
Creating an array of objects from an ArrayList:
String[] array1 = new String[list.size()];
list.toArray(array1);

<!-- Slide number: 51 -->
# max and min in an Array List
String[] array = {"red", "green", "blue"};
System.out.pritnln(java.util.Collections.max(
new ArrayList<String>(Arrays.asList(array)));
String[] array = {"red", "green", "blue"};
System.out.pritnln(java.util.Collections.min(
new ArrayList<String>(Arrays.asList(array)));

<!-- Slide number: 52 -->
# Shuffling an Array List
Integer[] array = {3, 5, 95, 4, 15, 34, 3, 6, 5};
ArrayList<Integer> list = new
ArrayList<>(Arrays.asList(array));
java.util.Collections.shuffle(list);
System.out.println(list);

<!-- Slide number: 53 -->
# Stack Animation
https://liveexample.pearsoncmg.com/dsanimation/StackeBook.html

![An object shows the Stack Animation and it has a column for 4 digits. The digits are 5, 3, 3 and 4 but the digit 5 represents the Top.](Picture5.jpg)

### Notes:
https://liveexample.pearsoncmg.com/dsanimation/StackeBook.html

<!-- Slide number: 54 -->
# The MyStack Classes
A stack to hold objects.
MyStack

![The computer code shows the My Stack Classes. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
MyStack: https://liveexample.pearsoncmg.com/html/MyStack.html

It has 3 rows. Row 1, shows the My Stack. Row 2, shows the one line coding that is minus list colon Array List. Row 3, shows the coding for 6 lines. Line 1, plus is Empty open parenthesis close parenthesis colon Boolean. Line 2, plus get Size open parenthesis close parenthesis colon int. Line 3, plus peek open parenthesis close parenthesis colon Object. Line 4, plus pop open parenthesis close parenthesis colon Object. Line 5, plus push open parenthesis o colon Object close parenthesis colon void. Line 6, plus search open parenthesis o colon Object close parenthesis colon int.

<!-- Slide number: 55 -->
# The protected Modifier
The protected modifier can be applied on data and methods in a class. A protected data or a protected method in a public class can be accessed by any class in the same package or its subclasses, even if the subclasses are in a different package.
private, default, protected, public

![A right arrow labeled, visibility increases. The text below the arrow reads, private, none (if no modifier is used), protected public.](Picture7.jpg)

### Notes:

<!-- Slide number: 56 -->
# Accessibility Summary
| Modifier on members in a class | Accessed from the same class | Accessed from the same package | Accessed from a subclass | Accessed from a different package |
| --- | --- | --- | --- | --- |
| public | sign | sign | sign | sign |
| protected | sign | sign | sign | Blank |
| default | sign | sign | Blank | Blank |
| private | sign | Blank | Blank | Blank |

<!-- Slide number: 57 -->
# Visibility Modifiers

![The computer code shows the Visibility Modifiers. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
It has divided into 2 parts that is package p1 and package p2. In the package p1 there are 3 boxes. Box 1,shows the coding for 8 lines. Line 1, public class C1 open braces. Line 2, public int x semicolon. Line 3, protected int y semicolon. Line 4, int z semicolon. Line 5, private int u semicolon. Line 6, protected void m open parenthesis close parenthesis open braces. Line 7, close braces. Line 8, close braces. Box 2, also shows the coding for 8 lines. Line 1, public class C2 open braces. Line 2, C1 o equal to new C1 open parenthesis close parenthesis semicolon. Line 3, can access o period x semicolon. Line 4, can access o period y semicolon. Line 5, can access o period z semicolon. Line 6, cannot access o period u semicolon. Line 7, can invoke o period m open parenthesis close parenthesis semicolon. Line 8, close braces. Box 3, also shows the coding for 8 lines. Line 1, public class C3. Line 2, extends C1 open braces. Line 3, can access x semicolon. Line 4, can access y semicolon. Lime 5, can access z semicolon. Line 6, cannot access u semicolon. Line 7, can invoke m open parenthesis close parenthesis semicolon. Line 8, close braces and this box makes an arrow which represent the Box 1 of package p1 and Box 1 of package p2. Now, in the package p2 has 2 boxes. Box 1, shows the coding for 8 lines. Line 1, public class C4. Line 2, extends C1 open braces. Line 3, can access x semicolon. Line 4, can access y semicolon. Line 5, cannot access z semicolon. Line 6, cannot access u semicolon. Line 7, can invoke m open parenthesis close parenthesis semicolon. Line 8, close braces. Box 2, also shows the coding for 8 lines. Line 1, public class C5 open braces. Line 2, C1 o equal to new C1 open parenthesis close parenthesis semicolon. Line 3, can access o period x semicolon. Line 4, cannot access o period y semicolon. Line 5, cannot access o period z semicolon. Line 6, cannot access o period u semicolon. Line 7, cannot invoke o period m open parenthesis close parenthesis semicolon. Line 8, close braces.

<!-- Slide number: 58 -->
# A Subclass Cannot Weaken the Accessibility
A subclass may override a protected method in its superclass and change its visibility to public. However, a subclass cannot weaken the accessibility of a method defined in the superclass. For example, if a method is defined as public in the superclass, it must be defined as public in the subclass.

<!-- Slide number: 59 -->
# Note (4 of 4)
The modifiers are used on classes and class members (data and methods), except that the final modifier can also be used on local variables in a method. A final local variable is a constant inside a method.

<!-- Slide number: 60 -->
# The final Modifier
The final class cannot be extended:
final class Math {
...
}
The final variable is a constant:
final static double PI = 3.14159;
The final method cannot be overridden by its subclasses.

<!-- Slide number: 61 -->
# Lombok: Generating Boilerplate Code Using Annotations
When you write Java code, there are lot of getter and setter methods. It is tedious to write all these boilerplate code. Project Lombok comes to rescue. Project Lombok is a Java library that provides annotations to tell the Java compiler to automatically generate boilerplate code such as getter and setter methods for data fields. To use Lombok, you need download a jar file named lombok.jar from
https://projectlombok.org/download
.

### Notes:
https://projectlombok.org/download

<!-- Slide number: 62 -->
# Copyright
This work is protected by United States copyright laws and is provided solely for the use of instructors in teaching their courses and assessing student learning. Dissemination or sale of any part of this work (including on the World Wide Web) will destroy the integrity of the work and is not permitted. The work and materials from it should never be made available to students except by instructors using the accompanying text in their classes. All recipients of this work are expected to abide by these restrictions and to honor the intended pedagogical purposes and the needs of other instructors who rely on these materials.

![Warning](Graphic6.jpg)

### Notes:
<!-- Slide number: 1 -->
# Introduction to Java Programming and Data Structures
Thirteenth Edition

![Front Cover: Introduction to Java Programming and Data Structures Thirteenth Edition by Liang.](GoogleShape162p1.jpg)
Chapter 9
Objects and Classes
Copyright © 2024 Pearson Education, Inc. All Rights Reserved

![Pearson Logo](GoogleShape165p1.jpg)

### Notes:
If this PowerPoint presentation contains mathematical equations, you may need to check that your computer has the following installed:
1) MathType Plugin
2) Math Player (free versions available)
3) NVDA Reader (free versions available)

Slides in this presentation contain hyperlinks. JAWS users should be able to get a list of links by using INSERT+F7

<!-- Slide number: 2 -->
# Motivations
After learning the preceding chapters, you are capable of solving many programming problems using selections, loops, methods, and arrays. However, these Java features are not sufficient for developing graphical user interfaces and large scale software systems. Suppose you want to develop a graphical user interface as shown below. How do you program it?

![A screenshot of Show GUI window shows button OK and Cancel on the left, followed by text box for enter you name, check boxes for bold and italic, radio buttons as red and yellow with red selected, and a drop down button with freshman selected.](GoogleShape173p2.jpg)

### Notes:

<!-- Slide number: 3 -->
# Objectives (1 of 3)
9.1 To describe objects and classes, and use classes to model objects (§9.2).
9.2 To use U M L graphical notation to describe classes and objects (§9.2).
9.3 To demonstrate how to define classes and create objects (§9.3).
9.4 To create objects using constructors (§9.4).
9.5 To define a reference variable using a reference type and access objects via object reference variables (§9.5).
9.6 To access an object’s data and methods using the object member access operator (.) (§9.5.1).
9.7 To define data fields of reference types and assign default values for an object’s data fields (§9.5.2).

### Notes:

<!-- Slide number: 4 -->
# Objectives (2 of 3)
9.8 To distinguish between object reference variables and primitive data type variables (§9.5.3).
9.9 To use the Java library classes Date, Random, and Point2D (§9.6).
9.10 To distinguish between instance and static variables and methods (§9.7).
9.11 To define private data fields with appropriate get and set methods (§9.8).
9.12 To encapsulate data fields to make classes easy to maintain (§9.9).
9.13 To develop methods with object arguments and differentiate between primitive-type arguments and object-type arguments (§9.10).
9.14 To store and process objects in arrays (§9.11).

### Notes:

<!-- Slide number: 5 -->
# Objectives (3 of 3)
9.15 To create immutable objects from immutable classes to protect the contents of objects (§9.12).
9.16 To determine the scope of variables in the context of a class (§9.13).
9.17 To use the keyword this to refer to the calling object itself (§9.14).

### Notes:

<!-- Slide number: 6 -->
# O O Programming Concepts
Object-oriented programming (O O P) involves programming using objects. An object represents an entity in the real world that can be distinctly identified. For example, a student, a desk, a circle, a button, and even a loan can all be viewed as objects. An object has a unique identity, state, and behaviors. The state of an object consists of a set of data fields (also known as properties) with their current values. The behavior of an object is defined by a set of methods.

### Notes:

<!-- Slide number: 7 -->
# Objects

![A class template with detail as follows. For long description in Notes pane, press F6.](GoogleShape204p7.jpg)
An object has both a state and behavior. The state defines the object, and the behavior defines what the object does.

### Notes:
Class name, Circle.
Data fields. Radius is, blank space. Methods, getArea.
Three objects of the Circle class are as follows.
Circle object 1. Data fields, radius is 10.
Circle object 2. Data fields, radius is 25.
Circle object 3. Data fields, radius is 125.

<!-- Slide number: 8 -->
# Classes (1 of 2)
Classes are constructs that define objects of the same type. A Java class uses variables to define data fields and methods to define behaviors. Additionally, a class provides a special type of methods, known as constructors, which are invoked to construct objects from the class.

### Notes:

<!-- Slide number: 9 -->
# Classes (2 of 2)

![A text box shows the Classes. The computer code consists 18 lines. For long description in Notes pane, press F6.](GoogleShape218p9.jpg)

### Notes:
Line 1, indicates class Circle open braces. Line 2, slash forward address address The radius of this circle address slash forward. Line 3, double radius equal to 1.0 semicolon. The line 3 is Data field. Line 4, Blank. Line 5, slash forward address address Construct a circle object address slash forward. Line 6, Circle open parenthesis close parenthesis open braces. Line 7, close braces. Line 8, Blank. Line 9, slash forward address address Construct a circle object address slash forward. Line 10, Circle open parenthesis double new Radius close parenthesis open braces. Line 11, radius equal to new Radius semicolon. Line 12, close braces. The lines 5, 6, 7, 8, 9, 10, 11 and 12 are Constructors. Line 13, Blank. Line 14, slash forward address address Return the area of this circle address slash forward. Line 15, double get Area open parenthesis close parenthesis open braces. The line 15 is Method. Line 16, return radius address radius address 3 period 14159 semicolon. Line 17, close braces. Line 18, close braces.

<!-- Slide number: 10 -->
# U M L Class Diagram

![The top box shows the UML Class Diagram. For long description in Notes pane, press F6.](GoogleShape225p10.jpg)

### Notes:
Row 1 shows the Class name (circle). Row 2 shows the Data fields (radius: double). Row 3 shows the computer coding of 6 lines for Constructors and methods. Line 1, indicates Circle open parenthesis close parenthesis. Line 2, Circle open parenthesis new Radius colon double close parenthesis. Line 3, g e t Area open parenthesis close parenthesis colon double. Line 4, g e t Perimeter open parenthesis close parenthesis colon double. Line 5, s e t Radius open parenthesis new Radius colon. Line 6, double close parenthesis colon void. A left side text box shows the 2 rows of UML notation for objects. Row 1 shows the Circle 1 colon Circle. Row 2 shows the radius equal to 1.0. A middle text box shows the 2 rows of UML notation for objects. Row 1 shows the Circle 2 colon Circle. Row 2 shows the radius equal to 25. A right side text box shows the 2 rows of UML notation for objects. Row 1 shows the Circle 3 colon Circle. Row 2 shows the radius equal to 125.

<!-- Slide number: 11 -->
# Example: Defining Classes and Creating Objects (1 of 2)
Objective: Demonstrate creating objects, accessing data, and using methods.
TestSimpleCircle

### Notes:
TestSimpleCircle: https://liveexample.pearsoncmg.com/html/TestSimpleCircle.html

<!-- Slide number: 12 -->
# Example: Defining Classes and Creating Objects (2 of 2)

![A text box shows the 3 rows for Defining Classes and Creating Objects. For long description in Notes pane, press F6.](GoogleShape240p12.jpg)
TV
TestTV

### Notes:
Row 1 shows the TV. Row 2 shows the computer coding. Line 1, indicates channel colon i n t. Line 2, shows the volume Level colon i n t. Line 3, shows the on colon boolean. Row 3 also shows the computer coding for 9 lines and in this coding + sign indicates a public modifier. Line 1, indicates plus TV open parenthesis close parenthesis. Line 2, plus t u r n On open parenthesis close parenthesis colon void. Line 3, plus tu r n Off open parenthesis close parenthesis colon void. Line 4, plus s e t Channel open parenthesis new Channel colon i n t close parenthesis colon void. Line 5, plus s e t Volume open parenthesis new Volume Level colon i n t close parenthesis colon void. Line 6, plus channel Up open parenthesis close parenthesis colon void. Line 7, plus channel Down open parenthesis close parenthesis colon void. Line 8, plus volume Up open parenthesis close parenthesis colon void. Line 9, plus volume Down open parenthesis close parenthesis colon void.

TV: https://liveexample.pearsoncmg.com/html/TV.html

TestTV: https://liveexample.pearsoncmg.com/html/TestTV.html

<!-- Slide number: 13 -->
# Constructors (1 of 2)
Circle() {
}
Circle(double newRadius) {
radius = newRadius;
}
Constructors are a special kind of methods that are invoked to construct objects.

### Notes:

<!-- Slide number: 14 -->
# Constructors (2 of 2)
A constructor with no parameters is referred to as a no-arg constructor.
Constructors must have the same name as the class itself.
Constructors do not have a return type—not even void.
Constructors are invoked using the new operator when an object is created. Constructors play the role of initializing objects.

### Notes:

<!-- Slide number: 15 -->
# Creating Objects Using Constructors
new ClassName();
Example:
new Circle();
new Circle(5.0);

### Notes:

<!-- Slide number: 16 -->
# Default Constructor
A class may be defined without constructors. In this case, a no-arg constructor with an empty body is implicitly defined in the class. This constructor, called a default constructor, is provided automatically only if no constructors are explicitly defined in the class.

### Notes:

<!-- Slide number: 17 -->
# Declaring Object Reference Variables
To reference an object, assign the object to a reference variable.
To declare a reference variable, use the syntax:
ClassName objectRefVar;
Example:
Circle myCircle;

### Notes:

<!-- Slide number: 18 -->
# Declaring/Creating Objects in a Single Step
ClassName objectRefVar = new ClassName();
Example:

![Circle myCircle = new circle left parenthesis right parenthesis semi colon. For long description in Notes pane, press F6.](GoogleShape284p18.jpg)

### Notes:
An arrow from right side to the left side of = is labeled, assign object reference. The object new circle left parenthesis right parenthesis semi colon is labeled, create an object.

<!-- Slide number: 19 -->
# Accessing Object’s Members
Referencing the object’s data:
objectRefVar.data
e.g., myCircle.radius
Invoking the object’s method:
objectRefVar.methodName(arguments)
e.g., myCircle.getArea()

### Notes:

<!-- Slide number: 20 -->
# Trace Code (1 of 7)

![A text box shows the Trace Code. The computer code consists 5 lines. For long description in Notes pane, press F6.](GoogleShape300p20.jpg)

### Notes:
Line 1, indicates Circle my Circle equal to new Circle open parenthesis 5.0 close parenthesis semicolon. Line 2, blank. Line 3, Circle my Circle equal to new Circle open parenthesis close parenthesis semicolon.ine 4, blank. Line 5, your Circle period radius equal to hundred semicolon. A right side text box shows the 1 Row for Declare my Circle i.e. no value.

<!-- Slide number: 21 -->
# Trace Code (2 of 7)

![A text box shows the Trace Code. For long description in Notes pane, press F6.](GoogleShape307p21.jpg)

### Notes:
The computer code consists 5 lines. Line 1, indicates Circle my Circle equal to new Circle open parenthesis 5.0 close parenthesis semicolon. Line 2, blank. Line 3, Circle my Circle equal to new Circle open parenthesis close parenthesis semicolon. Line 4, blank. Line 5, your Circle period radius equal to hundred semicolon.
A right upward side text box shows 1 Row for my Circle i.e. no value. A right downward side text box shows the 2 rows. Row 1 shows the Create a circle by coding i.e. colon Circle. Row 2 shows the radius colon 5.0.

<!-- Slide number: 22 -->
# Trace Code (3 of 7)

![A text box shows the Trace Code. For long description in Notes pane, press F6.](GoogleShape314p22.jpg)

### Notes:
The computer code consists 5 lines. Line 1, indicates Circle my Circle equal to new Circle open parenthesis 5.0 close parenthesis semicolon. Line 2, blank. Line 3, Circle my Circle equal to new Circle open parenthesis close parenthesis semicolon. Line 4, blank. Line 5, your Circle period radius equal to hundred semicolon.
A right upward side text box shows 1 Row for my Circle i.e. reference value and it also shows an arrow of red colour which is for Assign object reference to my Circle. A right downward side text box shows the 2 rows. Row 1 shows the Create a circle by coding i.e. colon Circle. Row 2 shows the radius colon 5.0.

<!-- Slide number: 23 -->
# Trace Code (4 of 7)

![A text box shows the Trace Code. For long description in Notes pane, press F6.](GoogleShape321p23.jpg)

### Notes:
The computer code consists 5 lines. Line 1, indicates Circle my Circle equal to new Circle open parenthesis 5.0 close parenthesis semicolon. Line 2, blank. Line 3, Circle my Circle equal to new Circle open parenthesis close parenthesis semicolon.ine 4, blank. Line 5, your Circle period radius equal to hundred semicolon. A right upward side text box shows 1 Row for my Circle i.e. reference value and it also shows an arrow of red colour. A rightward middle text box shows the 2 rows. Row 1 shows the Create a circle by coding i.e. colon Circle. Row 2 shows the radius colon 5.0. A right downward side text box shows the 1 row for Declare your Circle i.e. no value.

<!-- Slide number: 24 -->
# Trace Code (5 of 7)

![A text box shows the Trace Code. For long description in Notes pane, press F6.](GoogleShape328p24.jpg)

### Notes:
The computer code consists 5 lines. Line 1, indicates Circle my Circle equal to new Circle open parenthesis 5.0 close parenthesis semicolon. Line 2, blank. Line 3, Circle my Circle equal to new Circle open parenthesis close parenthesis semicolon line 4, blank. Line 5, your Circle period radius equal to hundred semicolon. A right upward side text box shows 1 Row for my Circle i.e. reference value and it also shows the red color arrow. A rightward middle text box shows the 2 rows. Row 1 shows the Create a circle by coding i.e. colon Circle. Row 2 shows the radius colon 5.0.A rightward middle side text box shows the 1 row for Declare your Circle i.e. no value. A right downward text box shows 2 rows for Create a new Circle object. Row 1 shows the colon Circle. Row 2 shows the radius colon 1.0.

<!-- Slide number: 25 -->
# Trace Code (6 of 7)

![A text box shows the Trace Code. For long description in Notes pane, press F6.](GoogleShape335p25.jpg)

### Notes:
The computer code consists 5 lines. Line 1, indicates Circle my Circle equal to new Circle open parenthesis 5.0 close parenthesis semicolon. Line 2, blank. Line 3, Circle my Circle equal to new Circle open parenthesis close parenthesis semicolon.ine 4, blank. Line 5, your Circle period radius equal to hundred semicolon. A right upward side text box shows 1 Row for my Circle i.e. reference value and it also shows the red color arrow. A rightward middle text box shows the 2 rows. Row 1 shows the Create a circle by coding i.e. colon Circle. Row 2 shows the radius colon 5.0. A rightward middle side text box shows the 1 row i.e. reference value for assign object reference to your circle and it also shows the red colour arrow. A right downward text box shows 2 rows for Create a new Circle object. Row 1 shows the colon Circle. Row 2 shows the radius colon 1.0.

<!-- Slide number: 26 -->
# Trace Code (7 of 7)

![A text box shows the Trace Code. For long description in Notes pane, press F6.](GoogleShape342p26.jpg)

### Notes:
The computer code consists 5 lines. Line 1, indicates Circle my Circle equal to new Circle open parenthesis 5.0 close parenthesis semicolon. Line 2, blank. Line 3, Circle my Circle equal to new Circle open parenthesis close parenthesis semicolon.ine 4, blank. Line 5, your Circle period radius equal to hundred semicolon. A right upward side text box shows 1 Row for my Circle i.e. reference value and it also shows the red colour arrow. A rightward middle text box shows the 2 rows. Row 1 shows the Create a circle by coding i.e. colon Circle. Row 2 shows the radius colon 5.0.A rightward middle side text box shows the 1 row for Declare your Circle i.e. reference value and it also shows the red color arrow. A right downward text box shows 2 rows for Create a new Circle object. Row 1 shows the colon Circle. Row 2 shows the Change radius in your circle by coding i.e. radius colon 100.0.

<!-- Slide number: 27 -->
# Caution
Recall that you use
Math.methodName(arguments) (e.g., Math.pow(3, 2.5))
to invoke a method in the Math class. Can you invoke getArea() using SimpleCircle.getArea()? The answer is no. All the methods used before this chapter are static methods, which are defined using the static keyword. However, getArea() is non-static. It must be invoked from an object using
objectRefVar.methodName(arguments) (e.g., myCircle.getArea()).
More explanations will be given in the section on “Static Variables, Constants, and Methods.”

### Notes:

<!-- Slide number: 28 -->
# Reference Data Fields
The data fields can be of reference types. For example, the following Student class contains a data field name of the String type.
public class Student {
String name; // name has default value null
int age; // age has default value 0
boolean isScienceMajor; // isScienceMajor has default value false
char gender; // c has default value '\u0000’
}

### Notes:

<!-- Slide number: 29 -->
# The Null Value
If a data field of a reference type does not reference any object, the data field holds a special literal value, null.

### Notes:

<!-- Slide number: 30 -->
# Default Value for a Data Field
The default value of a data field is null for a reference type, 0 for a numeric type, false for a boolean type, and '\u0000' for a char type. However, Java assigns no default value to a local variable inside a method.
public class Test {
public static void main(String[] args) {
Student student = new Student();
System.out.println("name? " + student.name);
System.out.println("age? " + student.age);
System.out.println("isScienceMajor? " + student.isScienceMajor);
System.out.println("gender? " + student.gender);
}
}

### Notes:

<!-- Slide number: 31 -->
# Example (1 of 3)
Java assigns no default value to a local variable inside a method.

![A text box shows the example of no default value. For long description in Notes pane, press F6.](GoogleShape376p31.jpg)

### Notes:
The computer code consists 8 lines. Line 1, public class Test open braces. Line 2, public static void main open parenthesis String open braces close braces args close parenthesis open braces. Line 3, i n t x semicolon slash forward slash forward x has no default value. Line 4, String y semicolon slash forward slash forward y has no default value. Line 5, System period out period print ln open parenthesis double quote x is double quote plus x close parenthesis semicolon. Line 6, System period out period print ln open parenthesis double quote y is double quote plus y close parenthesis semicolon. Line 7, close braces. Line 8, close braces.

<!-- Slide number: 32 -->
# Differences Between Variables of Primitive Data Types and Object Types

![Primitive type, int i = 1, i, 1. Object type, Circle c, c, reference. An arrow from reference points to c, circle, radius = 1. c, circle, radius = 1 is labeled, created using new Circle left parenthesis right parenthesis.](GoogleShape382p32.jpg)

### Notes:

<!-- Slide number: 33 -->
# Copying Variables of Primitive Data Types and Object Types

![Primitive type assignment i = j. Before, i = 1, j = 2. After i = 2, j = 2.](GoogleShape388p33.jpg)

![Object type assignment c1 = c2. Before, C1, C1. Circle, radius = 5. C2, C2. Circle, radius = 9. After, C1, C1. Circle, radius = 5. C2, C2. Circle, radius = 9. The c1 is crossed out.](GoogleShape389p33.jpg)

### Notes:

<!-- Slide number: 34 -->
# Garbage Collection (1 of 2)
As shown in the previous figure, after the assignment statement c1 = c2, c1 points to the same object referenced by c2. The object previously referenced by c1 is no longer referenced. This object is known as garbage. Garbage is automatically collected by J V M.

### Notes:

<!-- Slide number: 35 -->
# Garbage Collection (2 of 2)
Tip: If you know that an object is no longer needed, you can explicitly assign null to a reference variable for the object. The J V M will automatically collect the space if the object is not referenced by any variable.

### Notes:

<!-- Slide number: 36 -->
# The Date Class
Java provides a system-independent encapsulation of date and time in the java.util.Date class. You can use the Date class to create an instance for the current date and time and use its toString method to return the date and time as a string.

![A text box shows The Date Class and box is divided in 2 rows. For long description in Notes pane, press F6.](GoogleShape409p36.jpg)

### Notes:
Row 1, shows the coding i.e. java period util period Date. Row 2, shows the coding for 5 lines and the plus sign shows the public modifier. Line 1, plus Date open parenthesis close parenthesis. Line 2, plus Date open parenthesis elapse Time colon long close parenthesis. Line 3, plus to String open parenthesis close parenthesis colon String. Line 4, plus g e t Time open parenthesis close parenthesis colon long. Line 5, plus s e t Time open parenthesis elapse Time colon long close parenthesis colon void.

<!-- Slide number: 37 -->
# The Date Class Example
For example, the following code
java.util.Date date = new java.util.Date();
System.out.println(date.toString());
displays a string like Sun Mar 09 13:50:19 EST 2003.

### Notes:

<!-- Slide number: 38 -->
# The Random Class
You have used Math.random() to obtain a random double value between 0.0 and 1.0 (excluding 1.0). A more useful random number generator is provided in the java.util.Random class.

![java dot util dot Random + Random left parenthesis right parenthesis. For long description in Notes pane, press F6.](GoogleShape424p38.jpg)

### Notes:
Constructs a Random object with the current time as its seed.
+Random left parenthesis seed, long right parenthesis. Constructs a Random object with a specified seed.
+nextint left parenthesis right parenthesis colon int. Returns a random int value.
+nextInt left parenthesis n, int right parenthesis colon int. Returns a random int value between 0 and n (exclusive).
+nextLong left parenthesis right parenthesis, colon long. Returns a random long value.
+nextDouble left parenthesis right parenthesis colon double. Returns a random double value between 0.0 and 1.0 (exclusive).
+nextFloat left parenthesis right parenthesis colon float. Returns a random float value between 0.0F and 1.0F (exclusive).
+nextBoolean left parenthesis right parenthesis colon Boolean. Returns a random Boolean value.

<!-- Slide number: 39 -->
# The Random Class Example
If two Random objects have the same seed, they will generate identical sequences of numbers. For example, the following code creates two Random objects with the same seed 3.
Random random1 = new Random(3);
System.out.print("From random1: ");
for (int i = 0; i < 10; i++)
System.out.print(random1.nextInt(1000) + " ");
Random random2 = new Random(3);
System.out.print("\nFrom random2: ");
for (int i = 0; i < 10; i++)
System.out.print(random2.nextInt(1000) + " ");
From random1: 734 660 210 581 128 202 549 564 459 961
From random2: 734 660 210 581 128 202 549 564 459 961

### Notes:

<!-- Slide number: 40 -->
# The Point2D Class
Java A P I has a conveninent Point2D class in the javafx.geometry package for representing a point in a two-dimensional plane.

![A left side text box shows the Point 2D Class. For long description in Notes pane, press F6.](GoogleShape440p40.jpg)
TestPoint2D

### Notes:
The box is divided in 2 rows. Row 1, shows the coding i.e. java fx period geometry period Point 2D. Row 2, consists 6 lines for computer coding. Line 1, indicates plus Point 2D open parenthesis x colon double comma y colon double close parenthesis. Line 2, plus distance open parenthesis x colon double comma y colon double close parenthesis colon double. Line 3, plus distance open parenthesis p colon Point 2D close parenthesis colon double. Line 4, plus g e t X open parenthesis close parenthesis colon double. Line 5, plus g e t Y open parenthesis close parenthesis colon double. Line 6, plus to String open parenthesis close parenthesis colon String.

TestPoint2D: https://liveexample.pearsoncmg.com/html/TestPoint2D.html

<!-- Slide number: 41 -->
# Instance Variables, and Methods
Instance variables belong to a specific instance.
Instance methods are invoked by an instance of the class.

### Notes:

<!-- Slide number: 42 -->
# Static Variables, Constants, and Methods (1 of 3)
Static variables are shared by all the instances of the class.
Static methods are not tied to a specific object.
Static constants are final variables shared by all the instances of the class.

### Notes:

<!-- Slide number: 43 -->
# Static Variables, Constants, and Methods (2 of 3)
To declare static variables, constants, and methods, use the static modifier.

### Notes:

<!-- Slide number: 44 -->
# Static Variables, Constants, and Methods (3 of 3)

![An illustration titled, Static Variables, Constants, and Methods. For long description in Notes pane, press F6.](GoogleShape469p44.jpg)

### Notes:
A box titled, Circle contains 2 lines. Line 1, radius colon double number of objects colon i n t, underline. Line 2, get number of objects left parenthesis right parenthesis colon i n t, underline, get area left parenthesis right parenthesis colon double. The box is labeled, U M L Notation, underline colon static variables or methods. An arrow branches from the Circle box into two arrows each labeled, instantiate and points to two boxes. The box titled, Circle 1 colon circle has the text, radius = 1, number of objects colon = 2, underline. An arrow extends from radius = 1 to memory 1, labeled, radius. The box titled, Circle 2 colon circle has the text, radius = 5, number of objects colon = 2, underline. An arrow extends from radius = 5 to memory 4, labeled, radius. Arrows from both boxes lead to memory 2, number of objects and is labeled, after two circle, objects were created, number of objects is 2.

<!-- Slide number: 45 -->
# Example of Using Instance and Class Variables and Method
Objective: Demonstrate the roles of instance and class variables and their uses. This example adds a class variable numberOfObjects to track the number of Circle objects created.
CircleWithStaticMembers
TestCircleWithStaticMembers

### Notes:
CircleWithStaticMembers: https://liveexample.pearsoncmg.com/html/CircleWithStaticMembers.html

TestCircleWithStaticMembers: https://liveexample.pearsoncmg.com/html/TestCircleWithStaticMembers.html

<!-- Slide number: 46 -->
# Visibility Modifiers and Accessor/Mutator Methods (1 of 3)
By default, the class, variable, or method can be accessed by any class in the same package.
Public
The class, data, or method is visible to any class in any package.
Private
The data or methods can be accessed only by the declaring class.
The get and set methods are used to read and modify private properties.

### Notes:

<!-- Slide number: 47 -->
# Visibility Modifiers and Accessor/Mutator Methods (2 of 3)

![A left side text box shows the private modifier restricts access to within a class. For long description in Notes pane, press F6.](GoogleShape494p47.jpg)
The private modifier restricts access to within a class, the default modifier restricts access to within a package, and the public modifier enables unrestricted access.

### Notes:
The computer code consists 14 lines. Line 1, indicates package p1 semicolon. Line 2, blank. Line 3, public class C1 open braces. Line 4, public i n t x semicolon. Line 5, i n t y semicolon. Line 6, private i n t z semicolon. Line 7, blank. Line 8, public void m1 open parenthesis close parenthesis open braces. Line 9, close braces. Line 10, void m2 open parenthesis close parenthesis open braces. Line 11, close braces. Line 12, private void m3 open parenthesis close parenthesis open braces. Line 13, close braces. Line 14, close braces. A middle text box shows the coding for the default modifier restricts access to within a package. The computer code consists 14 lines. Line 1, indicates package p1 semicolon. Line 2, blank. Line 3, public class C2 open braces. Line 4, void a Method open parenthesis close parenthesis open braces. Line 5, C1 o equal to new C1 open parenthesis close parenthesis semicolon. Line 6, can access o period x semicolon. Line 7, can access o period y semicolon. Line 8, cannot access o period z semicolon. Line 9, blank. Line 10, can invoke o period m1 open parenthesis close parenthesis semicolon. Line 11, can invoke o period m2 open parenthesis close parenthesis semicolon. Line 12, cannot invoke o period m3 open parenthesis close parenthesis semicolon. Line 13, close braces. Line 14, close braces. A right side text box shows the coding for the public modifier enables unrestricted access. It consists 14 lines. Line 1, indicates package p2 semicolon. Line 2, blank. Line 3, public class C3 open braces. Line 4, void a Method open parenthesis close parenthesis open braces. Line 5, C1 o equal to new C1 open parenthesis close parenthesis semicolon. Line 6, can access 0 period x semicolon. Line 7, cannot access 0 period y semicolon. Line 8, cannot access 0 period z semicolon. Line 9, blank. Line 10, can invoke o period m1 open parenthesis close parenthesis semicolon. Line 11, cannot invoke 0 period m2 open parenthesis close parenthesis semicolon. Line 12, cannot invoke 0 period m3 open parenthesis close parenthesis semicolon. Line 13, close braces. Line 14, close braces.

<!-- Slide number: 48 -->
# Visibility Modifiers and Accessor/Mutator Methods (3 of 3)

![A left side text box shows the default modifier on a class restricts access to within a package. For long description in Notes pane, press F6.](GoogleShape502p48.jpg)
The default modifier on a class restricts access to within a package, and the public modifier enables unrestricted access.

### Notes:
The box contains 5 lines. Line 1, indicates package p1 semicolon. Line 2, blank. Line 3, class C1 open braces. Line 4, dot dot dot. Line 5, close braces. A middle text box also shows the default modifier on a class restricts access to within a package. The box contains 5 lines. Line 1, indicates package p1 semicolon. Line 2, blank. Line 3, public class C2 open braces. Line 4, can access C1. Line 5, close braces. A left side text box shows the public modifier enables unrestricted access. The box contains 6 lines. Line 1, package p2 semicolon. Line 2, blank. Line 3, public class C3 open braces. Line 4, cannot access C1 semicolon. Line 5, can access C2 semicolon. Line 6, close braces.

<!-- Slide number: 49 -->
# Note
An object cannot access its private members, as shown in (b). It is Ok, however, if the object is declared in its own class, as shown in (a).

![A text box (a) shows the object is declared in its own class. For long description in Notes pane, press F6.](GoogleShape511p49.jpg)

### Notes:
The box contains 13 lines. Line 1, indicates public class C open braces. Line 2, private Boolean x semicolon. Line 3, blank. Line 4, public static void main open parenthesis String open braces close braces args close parenthesis open braces. Line 5, C c equal to new C open parenthesis close parenthesis semicolon. Line 6, System period out period print ln open parenthesis c period x close parenthesis semicolon. Line 7, System period out period print ln open parenthesis c period convert open parenthesis close parenthesis close parenthesis semicolon. Line 8, close braces. Line 9, blank. Line 10, private i n t convert open parenthesis close parenthesis open braces. Line 11, return x question mark 1 colon minus 1 semicolon. Line 12, close braces. Line 13, close braces. A text box (b) shows an object cannot access its private members. This box contains 7 lines. Line 1, indicates public class Test open braces. Line 2, public static void main open parenthesis String open parenthesis close parenthesis args close parenthesis open braces. Line 3, C c equal to new C open parenthesis close parenthesis semicolon. Line 4, System period out period print ln open parenthesis c period x close parenthesis semicolon. Line 5, System period out period print ln open parenthesis c period convert open parenthesis close parenthesis close parenthesis semicolon. Line 6, close braces. Line 7, close braces.

<!-- Slide number: 50 -->
# Why Data Fields Should Be Private?
To protect data.
To make code easy to maintain.

### Notes:

<!-- Slide number: 51 -->
# Example of Data Field Encapsulation

![A text box shows the Data Field Encapsulation. For long description in Notes pane, press F6.](GoogleShape524p51.jpg)
CircleWithPrivateDataFields
TestCircleWithPrivateDataFields

### Notes:
This box has divided in 3 rows. Row 1 indicates Circle. Row 2 shows the coding for 2 lines and minus sign indicates private modifier. Line 1, minus radius colon double. Line 2, minus number of Objects colon i n t. Row 3, also shows the coding and it consists 6 lines. Line 1, plus Circle open parenthesis close parenthesis. Line 2, plus Circle open parenthesis radius colon double close parenthesis. Line 3, plus g e t Radius open parenthesis close parenthesis colon double. Line 4, plus s e t Radius open parenthesis radius colon double close parenthesis colon void. Line 5, plus g e t Number Of Objects open parenthesis close parenthesis colon i n t. Line 6, plus g e t Area open parenthesis close parenthesis colon double.

CircleWithPrivateDataFields: https://liveexample.pearsoncmg.com/html/CircleWithPrivateDataFields.html

TestCircleWithPrivateDataFields: https://liveexample.pearsoncmg.com/html/TestCircleWithPrivateDataFields.html

<!-- Slide number: 52 -->

![](GoogleShape532g381b73b2b03_0_0.jpg)

### Notes:

<!-- Slide number: 53 -->
# Passing Objects to Methods (1 of 2)
Passing by value for primitive type value (the value is passed to the parameter)
Passing by value for reference type value (the value is the reference to the object)
TestPassObject

### Notes:
TestPassObject: https://liveexample.pearsoncmg.com/html/TestPassObject.html

<!-- Slide number: 54 -->
# Passing Objects to Methods (2 of 2)

![Stack. Activation for the printArea method. int times, 5. Circle c, reference. For long description in Notes pane, press F6.](GoogleShape547p53.jpg)

### Notes:
Activation record for the main method. int n, 5. myCircle, reference.
An arrow from n, 5 to int times 5 is labeled, pass by value (here the value is 5). An arrow from reference in the activation record to reference in the stack is labeled, pass by value (here the value is the reference for the object).
The reference in both the sections lead to heap, a Circle object.

<!-- Slide number: 55 -->
# Array of Objects (1 of 3)
Circle[] circleArray = new Circle[10];
An array of objects is actually an array of reference variables. So invoking circleArray[1].getArea() involves two levels of referencing as shown in the next figure. circleArray references to the entire array. circleArray[1] references to a Circle object.

### Notes:

<!-- Slide number: 56 -->
# Array of Objects (2 of 3)
Circle[] circleArray = new Circle[10];

![CircleArray, reference leads to three arrays as follows. circleArray 0 which leads to circle object 0. circleArray 1 which leads to circle object 1. Ellipsis circleArray 9 which leads to circle object 9.](GoogleShape562p55.jpg)

### Notes:

<!-- Slide number: 57 -->
# Array of Objects (3 of 3)
Summarizing the areas of the circles
TotalArea

### Notes:
TotalArea: https://liveexample.pearsoncmg.com/html/TotalArea.html

<!-- Slide number: 58 -->
# Immutable Objects and Classes
If the contents of an object cannot be changed once the object is created, the object is called an immutable object and its class is called an immutable class. If you delete the set method in the Circle class in Listing 8.10, the class would be immutable because radius is private and cannot be changed without a set method.
A class with all private data fields and without mutators is not necessarily immutable. For example, the following class Student has all private data fields and no mutators, but it is mutable.

### Notes:

<!-- Slide number: 59 -->
# Example (2 of 3)
public class BirthDate {
private int year;
private int month;
private int day;

public BirthDate(int newYear,
int newMonth, int newDay) {
year = newYear;
month = newMonth;
day = newDay;
}

public void setYear(int newYear) {
year = newYear;
}
}
public class Student {
private int id;
private BirthDate birthDate;
public Student(int ssn,
int year, int month, int
day) {
id = ssn;
birthDate = new
BirthDate(year, month, day);
}
public int getId() {
return id;
}
public BirthDate getBirthDate() {
return birthDate;
}
}

### Notes:

<!-- Slide number: 60 -->
# Example (3 of 3)
public class Test {
public static void main(String[] args) {
Student student = new Student(111223333, 1970, 5, 3);
BirthDate date = student.getBirthDate(); date.setYear(2010); // Now the student birth year is changed!
}
}

### Notes:

<!-- Slide number: 61 -->
# What Class Is Immutable?
For a class to be immutable, it must mark all data fields private and provide no mutator methods and no accessor methods that would return a reference to a mutable data field object.

### Notes:

<!-- Slide number: 62 -->
# Scope of Variables
The scope of instance and static variables is the entire class. They can be declared anywhere inside a class.
The scope of a local variable starts from its declaration and continues to the end of the block that contains the variable. A local variable must be initialized explicitly before it can be used.

### Notes:

<!-- Slide number: 63 -->
# The this Keyword
The this keyword is the name of a reference that refers to an object itself. One common use of the this keyword is reference a class’s hidden data fields.
Another common use of the this keyword to enable a constructor to invoke another constructor of the same class.

### Notes:

<!-- Slide number: 64 -->
# Reference the Hidden Data Fields

![A Left side text box shows reference the Hidden Data Fields. For long description in Notes pane, press F6.](GoogleShape620p63.jpg)

### Notes:
It consists 12 lines. Line 1, indicates public class F open braces. Line 2, private i n t i equal to 5 semicolon. Line 3, private static double k equal to zero semicolon. Line 4, blank. Line 5, void s e t I open parenthesis i n t i close parenthesis open braces. Line 6, t h i s period i equal to i semicolon. Line 7, close braces. Line 8, blank. Line 9, static void s e t K open parenthesis double k close parenthesis open braces. Line 10, F period k equal to k semicolon. Line 11, close braces. Line 12, close braces. A right hand side text box shows the coding of F1 and F2 are the objects of F. It consists 7 lines. Line 1, F f1 equal to new F open parenthesis close parenthesis semicolon F f2 equal to new F open parenthesis close parenthesis semicolon. Line 2, blank. Line 3, Invoking f1 period s e t I open parenthesis ten close parenthesis is to execute. Line 4, t h i s i equal to 10 comma where t h i s refers f1. Line 5, blank. Line 6, Invoking f2 period s e t I open parenthesis 45 close parenthesis is to execute. Line 7, t h i s i equal to 45 comma where t h i s refers f2.

<!-- Slide number: 65 -->
# Calling Overloaded Constructor

![A text box shows the Calling Overloaded Constructor. For long description in Notes pane, press F6.](GoogleShape627p64.jpg)

### Notes:
It consists 15 lines. Line 1, indicates public class Circle open braces. Line 2, private double radius semicolon. Line 3, blank. Line 4, public Circle open parenthesis double radius close parenthesis open braces. Line 5, t h i s period radius equal to radius semicolon where this must be explicitly used to reference the data field radius of the object being constructed. Line 6, close braces. Line 7, blank. Line 8, public Circle open parenthesis close parenthesis open braces. Line 9, t h i s open parenthesis 1 period 0 close parenthesis semicolon where this is used to invoke another constructor. Line 10, close braces. Line 11, public double g e t Area open parenthesis close parenthesis open braces. Line 12, return t h i s period radius address t h i s period radius address Math period PI semicolon where Every instance variable belongs to an instance represented by this, which is normally omitted. Line 13, close braces. Line 14, close braces.

<!-- Slide number: 66 -->
# Copyright
This work is protected by United States copyright laws and is provided solely for the use of instructors in teaching their courses and assessing student learning. Dissemination or sale of any part of this work (including on the World Wide Web) will destroy the integrity of the work and is not permitted. The work and materials from it should never be made available to students except by instructors using the accompanying text in their classes. All recipients of this work are expected to abide by these restrictions and to honor the intended pedagogical purposes and the needs of other instructors who rely on these materials.

![Warning](GoogleShape634p65.jpg)

### Notes:
<!-- Slide number: 1 -->
# Introduction to Java Programming and Data Structures
Thirteenth Edition

![Front Cover: Introduction to Java Programming and Data Structures Thirteenth Edition by Liang.](Picture8.jpg)
Chapter 13
Abstract Classes and Interfaces
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
You have learned how to write simple programs to create and display G U I components. Can you write the code to respond to user actions, such as clicking a button to perform an action?
In order to write such code, you have to know about interfaces. An interface is for defining common behavior for classes (including unrelated classes). Before discussing interfaces, we introduce a closely related subject: abstract classes.

<!-- Slide number: 3 -->
# Objectives (1 of 2)
13.1 To design and use abstract classes (§13.2).
13.2 To generalize numeric wrapper classes, BigInteger, and BigDecimal using the abstract Number class (§13.3).
13.3 To process a calendar using the Calendar and GregorianCalendar classes (§13.4).
13.4 To specify common behavior for objects using interfaces (§13.5).
13.5 To define interfaces and define classes that implement interfaces (§13.5).
13.6 To define a natural order using the Comparable interface (§13.6).
13.7 To make objects cloneable using the Cloneable interface (§13.7).

<!-- Slide number: 4 -->
# Objectives (2 of 2)
13.8 To explore the similarities and differences among concrete classes, abstract classes, and interfaces (§13.8).
13.9 To design the Rational class for processing rational numbers (§13.9).
13.10 To design classes that follow the class-design guidelines (§13.10).
13.11 To use a simple one-line code to define a record (§13.11).

<!-- Slide number: 5 -->
# Abstract Classes and Abstract Methods
GeometricObject

![The computer code shows the Abstract Classes and Abstract Methods. For long description in Notes pane, press F6.](Picture5.jpg)
Circle
Rectangle
TestGeometricObject

### Notes:
The code is divided into 3 boxes that are for Geometric Object, Circle, and Rectangle. A Geometric Object box shows the coding, and it's divided into 2 rows. Row 1 shows the coding for 3 lines. Line 1, minus color colon String. Line 2, minus filled colon boolean. Line 3, minus Date Create colon java period util period Date. Row 2 also shows the coding but for 11 lines. Line 1, protected modifier Geometric Object open parenthesis close parenthesis. Line 2, protected modifier Geometric Object open parenthesis color colon String comma. Line 3, filled colon boolean close parenthesis. Line 4, plus get Color open parenthesis close parenthesis colon String. Line 5, plus set Color open parenthesis color colon String close parenthesis colon void. Line 6, plus is Filled open parenthesis close parenthesis colon boolean. Line 7, plus set Filled open parenthesis filled colon boolean close parenthesis colon void. Line 8, plus get Date Created open parenthesis close parenthesis colon java period util period Date. Line 9, plus to String open parenthesis close parenthesis colon String. Line 10, plus get Area open parenthesis close parenthesis colon double. Line 11, plus get Perimeter open parenthesis close parenthesis colon double. A Circle box shows the coding, but it's divided into two rows. Row 1, shows the coding for 1 line. Line 1, minus radius colon double. Row 2, also shows the coding but for 7 lines. Line 1, plus Circle open parenthesis close parenthesis. Line 2, plus Circle open parenthesis radius colon double close parenthesis. Line 3, plus Circle open parenthesis radius colon double comma color colon string comma. Line 4, filled colon boolean close parenthesis. Line 5, plus get Radius open parenthesis close parenthesis colon double. Line 6, plus set Radius open parenthesis radius colon double close parenthesis colon void. Line 7, plus get Diameter open parenthesis close parenthesis colon double and this box makes an arrow to represent the Geometric Object box. A Rectangle box also shows the coding and divided into 2 rows. Row 1, shows the coding for 2 lines. Line 1, minus width colon double. Line 2, minus height colon double. Row 2, also shows coding but for 8 lines. Line 1, plus Rectangle open parenthesis close parenthesis. Line 2, plus Rectangle open parenthesis width colon double comma height colon double close parenthesis. Line 3, plus Rectangle open parenthesis width colon double comma height colon double comma. Line 4, color colon string comma filled colon boolean close parenthesis. Line 5, plus get Width open parenthesis close parenthesis colon double. Line 6, plus set Width open parenthesis width colon double close parenthesis colon void. Line 7, plus get Height open parenthesis close parenthesis colon double. Line 8, plus set Height open parenthesis height colon double close parenthesis colon void.

GeometricObject: https://liveexample.pearsoncmg.com/html/GeometricObject.html
Circle: https://liveexample.pearsoncmg.com/html/Circle.html
Rectangle: https://liveexample.pearsoncmg.com/html/Rectangle.html
TestGeometricObject: https://liveexample.pearsoncmg.com/html/TestGeometricObject.html

<!-- Slide number: 6 -->
# Abstract Method in Abstract Class
An abstract method cannot be contained in a nonabstract class. If a subclass of an abstract superclass does not implement all the abstract methods, the subclass must be defined abstract. In other words, in a nonabstract subclass extended from an abstract class, all the abstract methods must be implemented, even if they are not used in the subclass.

<!-- Slide number: 7 -->
# Object Cannot Be Created From Abstract Class
An abstract class cannot be instantiated using the new operator, but you can still define its constructors, which are invoked in the constructors of its subclasses. For instance, the constructors of GeometricObject are invoked in the Circle class and the Rectangle class.

<!-- Slide number: 8 -->
# Abstract Class Without Abstract Method
A class that contains abstract methods must be abstract. However, it is possible to define an abstract class that contains no abstract methods. In this case, you cannot create instances of the class using the new operator. This class is used as a base class for defining a new subclass.

<!-- Slide number: 9 -->
# Superclass of Abstract Class May Be Concrete
A subclass can be abstract even if its superclass is concrete. For example, the Object class is concrete, but its subclasses, such as GeometricObject,may be abstract.

<!-- Slide number: 10 -->
# Concrete Method Overridden to Be Abstract
A subclass can override a method from its superclass to define it abstract. This is rare, but useful when the implementation of the method in the superclass becomes invalid in the subclass. In this case, the subclass must be defined abstract.

<!-- Slide number: 11 -->
# Abstract Class as Type
You cannot create an instance from an abstract class using the new operator, but an abstract class can be used as a data type. Therefore, the following statement, which creates an array whose elements are of GeometricObject type, is correct.
GeometricObject[] geo = new GeometricObject[10];

<!-- Slide number: 12 -->
# Case Study: The Abstract Number Class

![The computer code shows the Case Study for the Abstract Number Class. For long description in Notes pane, press F6.](Picture6.jpg)
LargestNumbers

### Notes:
It has 1 box, which is divided into 2 rows and 8 small boxes. Box 1 shows the coding now in Row 1, java period lang period Number, but in Row 2, it has 6 lines coding. Line 1, plus byte Value open parenthesis close parenthesis colon byte. Line 2, plus short Value open parenthesis close parenthesis colon short. Line 3, plus int Value open parenthesis close parenthesis colon int. Line 4, plus long Value open parenthesis close parenthesis colon long. Line 5, plus float Value open parenthesis close parenthesis colon float. Line 6, plus double Value open parenthesis close parenthesis colon double. Now, 8 small boxes that are Double, Float, Long, Integer, Short, Byte, BigInteger, BigDecimal and it's makes an arrow upward to represent the Box 1.

LargestNumbers: https://liveexample.pearsoncmg.com/html/LargestNumbers.html

<!-- Slide number: 13 -->
# The Abstract Calendar Class and Its GregorianCalendar Subclass (1 of 2)

![The computer code shows the Abstract Calendar Class and Its Gregorian Calendar Subclass. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
It has 2 boxes and each box divided into 2 rows. Box 1, shows the coding now in Row 1, java period util period Calendar but in Row 2, it shows the coding for 9 lines.
Line 1, hashtag Calendar open parenthesis close parenthesis. Constructs a default calendar.
Line 2, plus get open parenthesis field colon int close parenthesis colon int. Returns the value of the given calendar field.
Line 3, plus set open parenthesis field colon int comma month colon int comma. Sets the given calendar to the specified value.
Line 4, day Of Month colon int close parenthesis colon void. Sets the calendar with the specified year, month, and date. The month parameter is 0 based, that is, 0 is for January.
Line 5, plus get Actual Maximum open parenthesis field colon int close parenthesis colon int. Returns the maximum value that the specified calendar field could have. Returns the maximum value that the specified calendar field could have.
Line 6, plus add open parenthesis field colon int comma amount colon int close parenthesis colon void. Adds or subtracts the specified amount of time to the given calendar field.
Line 7, plus get Time open parenthesis close parenthesis colon java period util period Date. Returns a Date object representing this calendar's time value (million second offset from the UNIX epoch).
Line 8, plus set Time open parenthesis date colon java period util period Date close parenthesis colon void. Sets this calendar's time with the given Date object.
Box 2 shows the coding now in Row 1, java period util period Gregorian Calendar but in Row 2, it shows the coding for three lines.
Line 1, plus Gregorian Calendar open parenthesis close parenthesis. Constructs a GregorianCalendar for the current time.
Line 2, plus Gregorian Calendar open parenthesis year colon int comma month colon int comma day Of Month colon int close parenthesis. Constructs a GregorianCalendar for the specified year, month, and date.
Line 3, plus Gregorian Calendar open parenthesis year colon int, month colon int comma day Of Month colon int, hour colon int comma minute colon int comma second colon int close parenthesis. Constructs a GregorianCalendar for the specified year, month, date, hour, minute, and second. The month parameter is 0 based, that is, 0 is for January.

<!-- Slide number: 14 -->
# The Abstract Calendar Class and Its GregorianCalendar Subclass (2 of 2)
An instance of java.util.Date represents a specific instant in time with millisecond precision. java.util.Calendar is an abstract base class for extracting detailed information such as year, month, date, hour, minute and second from a Date object. Subclasses of Calendar can implement specific calendar systems such as Gregorian calendar, Lunar Calendar and Jewish calendar. Currently, java.util.GregorianCalendar for the Gregorian calendar is supported in the Java A P I.

<!-- Slide number: 15 -->
# The GregorianCalendar Class
You can use new GregorianCalendar() to construct a default GregorianCalendar with the current time and use new GregorianCalendar(year, month, date) to construct a GregorianCalendar with the specified year, month, and date. The month parameter is 0-based, i.e., 0 is for January.

<!-- Slide number: 16 -->
# The Get Method in Calendar Class
The get(int field) method defined in the Calendar class is useful to extract the date and time information from a Calendar object. The fields are defined as constants, as shown in the following.
| Constant | Description |
| --- | --- |
| Year | The year of the calendar. |
| Month | The month of the calendar, with 0 for January. |
| Date | The day of the calendar. |
| Hour | The hour of the calendar (12-hour notation). |
| Hour\_of\_day | The hour of the calendar (24-hour notation). |
| Minute | The minute of the calendar. |
| Second | The second of the calendar. |
| Day\_of\_week | The day number within the week, with 1 for Sunday. |
| Day\_of\_month | Same as Date. |
| Day\_of\_year | The day number in the year, with I for the first day of the year. |
| Week\_of\_month | The week number within the month, with I for the first week. |
| Week\_of\_year | The week number within the year, with 1 for the first week. |
| AM\_PM | Indicator for AM or PM (0 for AM and I for PM). |

<!-- Slide number: 17 -->
# Getting Date/Time Information From Calendar
TestCalendar

### Notes:
TestCalendar: https://liveexample.pearsoncmg.com/html/TestCalendar.html

<!-- Slide number: 18 -->
# Interfaces
What is an interface?
Why is an interface useful?
How do you define an interface?
How do you use an interface?

<!-- Slide number: 19 -->
# What Is an Interface? Why Is an Interface Useful?
An interface is a classlike construct that contains only constants and abstract methods. In many ways, an interface is similar to an abstract class, but the intent of an interface is to specify common behavior for objects. For example, you can specify that the objects are comparable, edible, cloneable using appropriate interfaces.

<!-- Slide number: 20 -->
# Define an Interface
To distinguish an interface from a class, Java uses the following syntax to define an interface:
public interface InterfaceName {
constant declarations;
abstract method signatures;
}
Example:
public interface Edible {
/** Describe how to eat */
public abstract String howToEat();
}

<!-- Slide number: 21 -->
# Interface Is a Special Class
An interface is treated like a special class in Java. Each interface is compiled into a separate bytecode file, just like a regular class. Like an abstract class, you cannot create an instance from an interface using the new operator, but in most cases you can use an interface more or less the same way you use an abstract class. For example, you can use an interface as a data type for a variable, as the result of casting, and so on.

<!-- Slide number: 22 -->
# Example (1 of 2)
You can now use the Edible interface to specify whether an object is edible. This is accomplished by letting the class for the object implement this interface using the implements keyword. For example, the classes Chicken and Fruit implement the Edible interface (See TestEdible).

![An object shows the Example for Edible and Test Edible. It has 2 boxes which are divided into two parts. For long description in Notes pane, press F6.](Picture6.jpg)
TestEdible
Edible

### Notes:
Box 1, Row 1, open braces open braces interface close braces close braces. Row 2, plus how To Eat open parenthesis close parenthesis colon String and this row divides into 2 parts or interface, Fruit and Chicken but Fruit is also divided into 2 interfaces, Orange and Apple. And, Chicken shows an arrow to another box 2. Box 2, Row 1, Animal. Row 2, plus sound open parenthesis close parenthesis colon String. Its divide into 1 interface is Tiger.
A notation to the left reads as follows.
The interface name and the method names are italicized. The dashed lines and hollow triangles are used to point to the interface.

Edible: https://liveexample.pearsoncmg.com/html/Edible.html
TestEdible: https://liveexample.pearsoncmg.com/html/TestEdible.html

<!-- Slide number: 23 -->
# Omitting Modifiers in Interfaces
All data fields are public final static and all methods are public abstract in an interface. For this reason, these modifiers can be omitted, as shown below:

![The computer code shows the Omitting Modifiers in Interfaces. For long description in Notes pane, press F6.](Picture7.jpg)
A constant defined in an interface can be accessed using syntax InterfaceName.CONSTANT_NAME (e.g., T1.K).

### Notes:
It has divided into 2 boxes. Box 1, has 4 lines. Line 1, public interface T1 open braces. Line 2, public static final int K equal to 1 semicolon. Line 3, public abstract void p open parenthesis close parenthesis semicolon. Line 4, close braces, and this box are equivalent to another box 2. Box 2 also has 4 lines. Line 1, public interface T1 open braces. Line 2, int K equal to 1 semicolon. Line 3, void p open parenthesis close parenthesis semicolon. Line 4, close braces, and this also equivalent to box 1.

<!-- Slide number: 24 -->
# Example: The Comparable Interface
// This interface is defined in
// java.lang package
package java.lang;

public interface Comparable<E> {
public int compareTo(E o);
}

<!-- Slide number: 25 -->
# The toString, equals, and hashCode Methods
Each wrapper class overrides the toString, equals, and hashCode methods defined in the Object class. Since all the numeric wrapper classes and the Character class implement the Comparable interface, the compareTo method is implemented in these classes.

<!-- Slide number: 26 -->
# Integer and BigInteger Classes

![The computer code shows the Integer and BigInteger Classes on the top the page. For long description in Notes pane, press F6.](Picture8.jpg)
String and Date Classes

![The computer code shows the String and Date Classes on the bottom of the page. For long description in Notes pane, press F6.](Picture10.jpg)

### Notes:
It has divided into 2 boxes. Box 1, has 8 lines. Line 1, public class Integer extends Number. Line 2, implements Comparable less than Integer greater than open braces. Line 3, forward slash forward slash class body omitted. Line 4, at the rate Override. Line 5, public int compare To open parenthesis Integer o close parenthesis open braces. Line 6, forward slash forward slash Implementation omitted. Line 7, close braces. Line 8, close braces. Box 2, also has 8 lines. Line 1, public class BigInteger extends Number. Line 2, implements Comparable less than BigInteger greater than open braces. Line 3, forward slash forward slash class body omitted. Line 4, at the rate Override. Line 5, public int compare To open parenthesis BigInteger o close parenthesis open braces. Line 6, forward slash forward slash Implementation omitted. Line 7, close braces. Line 8, close braces.

Box 1, has 8 lines. Line 1, public class String extends Object. Line 2, implements Comparable less than String greater than open braces. Line 3, forward slash forward slash class body omitted. Line 4, at the rate Override. Line 5, public int compare To open parenthesis String o close parenthesis open braces. Line 6, forward slash forward slash Implementation omitted. Line 7, close braces. Line 8, close braces. Box 2, has 8 lines. Line 1, public class Date extends Object. Line 2, implements Comparable less than Date greater than open braces. Line 3, forward slash forward slash class body omitted. Line 4, at the rate Override. Line 5, public int compare To open parenthesis Date o close parenthesis open braces. Line 6, forward slash forward slash Implementation omitted. Line 7, close braces. Line 8, close braces.

<!-- Slide number: 27 -->
# Example (2 of 2)
System.out.println(new Integer(3).compareTo(new Integer(5)));
System.out.println("ABC".compareTo("ABE"));
java.util.Date date1 = new java.util.Date(2013, 1, 1);
java.util.Date date2 = new java.util.Date(2012, 1, 1);
System.out.println(date1.compareTo(date2));

<!-- Slide number: 28 -->
# Generic sort Method
Let n be an Integer object, s be a String object, and d be a Date object. All the following expressions are true.

![The computer code shows the Generic sort Method. For long description in Notes pane, press F6.](Picture7.jpg)
The java.util.Arrays.sort(array) method requires that the elements in an array are instances of Comparable<E>.
SortComparableObjects

### Notes:
It has 3 boxes. Box 1, has 3 lines. Line 1, n instance of Integer. Line 2, n instance of Object. Line 3, n instance of Comparable. Box 2 also has 3 lines. Line 1, s instance of String. Line 2, s instance of Object. Line 3, s instance of Comparable. Box 3 also has 3 lines. Line 1, d instance of java period util period Date. Line 2, d instance of Object. Line 3, d instance of Comparable.

SortComparableObjects: https://liveexample.pearsoncmg.com/html/SortComparableObjects.html

<!-- Slide number: 29 -->
# Defining Classes to Implement Comparable

![The Object shows the Defining Classes to Implement Comparable. For long description in Notes pane, press F6.](Picture5.jpg)
SortRectangles
ComparableRectangle

### Notes:
It has 4 boxes. Box 1 shows the Geometric Object. Box 2 shows the Rectangle, and it makes an arrow upward to show box 1. Box 3 shows the Comparable Rectangle, and it makes 2 arrows, one for upward to show box 2 and another for the right hand side to show box 4. Box 4, has 2 rows. Row 1, has 2 lines. Line 1, open braces open braces interface close braces close braces. Line 2, java period lang period Comparable less than Comparable Rectangle greater than. Row 2, plus compare To open parenthesis o colon Comparable Rectangle close parenthesis colon int.

ComparableRectangle: https://liveexample.pearsoncmg.com/html/ComparableRectangle.html
SortRectangles: https://liveexample.pearsoncmg.com/html/SortRectangles.html

<!-- Slide number: 30 -->
# The Cloneable Interfaces
Marker Interface: An empty interface.
A marker interface does not contain constants or methods. It is used to denote that a class possesses certain desirable properties. A class that implements the Cloneable interface is marked cloneable, and its objects can be cloned using the clone() method defined in the Object class.
package java.lang;
public interface Cloneable {
}

<!-- Slide number: 31 -->
# Examples
Many classes (e.g., Date and Calendar) in the Java library implement Cloneable. Thus, the instances of these classes can be cloned. For example, the following code
Calendar calendar = new GregorianCalendar(2003, 2, 1);
Calendar calendarCopy = (Calendar)calendar.clone();
System.out.println("calendar == calendarCopy is " +
(calendar == calendarCopy));
System.out.println("calendar.equals(calendarCopy) is " +
calendar.equals(calendarCopy));
displays
calendar == calendarCopy is false
calendar.equals(calendarCopy) is true

<!-- Slide number: 32 -->
# Implementing Cloneable Interface
To define a custom class that implements the Cloneable interface, the class must override the clone() method in the Object class. The following code defines a class named House that implements Cloneable and Comparable.
House

### Notes:
House: https://liveexample.pearsoncmg.com/html/House.html

<!-- Slide number: 33 -->
# Shallow versus Deep Copy (1 of 2)
House house1 = new House(1, 1750.50);
House house2 = (House)house1.clone();

![The computer code shows the Shallow versus Deep Copy. For long description in Notes pane, press F6.](Picture7.jpg)
Shallow Copy

### Notes:
 It has 2 lines. Line 1, House house 1 equal to new House open parenthesis 1,1750 period 50 close parenthesis semicolon. Line 2, House house equal to open parenthesis House close parenthesis house 1 period clone open parenthesis close parenthesis semicolon. An object shows the Shallow Copy. It has 2 boxes, and their boxes are divided into many parts. Box 1, has 2 rows, and it makes an arrow to represent Box 2. Row 1, house 1 colon House. Row 2 has 3 lines. Line 1, id equal to 1, and it makes an arrow forward to represent the 1 box. Line 2, area equal to 1750 period 50, and it makes an arrow forward to represent the 1750.50 box. Line 3, when Built and it makes an arrow forward to represent the reference box. Then, the computer code has 2 lines. Line 1, house 2 equal to. Line 2, house 1 period clone open parenthesis close parenthesis. Box 2, has 2 rows. Row 1, house 2 colon House. Row 2, has 3 lines. Line 1, id equal to 1 and it makes an arrow forward to represent the 1 box. Line 2, area equal to 1750 period 50, and it makes an arrow forward to represent the 1750.50 box. Line 3, when Built and it makes an arrow forward to represent the reference box. Reference boxes have represented another box that has 2 rows. Row 1, when Built colon Date. Row 2, date object contents.

<!-- Slide number: 34 -->
# Shallow versus Deep Copy (2 of 2)
House house1 = new House(1, 1750.50);
House house2 = (House)house1.clone();

![The computer code shows the Shallow versus Deep Copy. For long description in Notes pane, press F6.](Picture8.jpg)
Deep Copy

### Notes:
It has 2 lines. Line 1, House house 1 equal to new House open parenthesis 1,1750 period 50 close parenthesis semicolon. Line 2, House house equal to open parenthesis House close parenthesis house 1 period clone open parenthesis close parenthesis semicolon. An object shows the Deep Copy. It has 2 boxes, and their boxes are divided into many parts. Box 1, has 2 rows, and it makes an arrow to represent Box 2. Row 1, house 1 colon House. Row 2 has 3 lines. Line 1, id equal to 1, and it makes an arrow forward to represent the 1 box. Line 2, area equal to 1750 period 50, and it makes an arrow forward to represent the 1750.50 box. Line 3, when Built and it makes an arrow forward to represent the reference box. Then, the computer code has 2 lines. Line 1, house 2 equal to. Line 2, house 1 period clone open parenthesis close parenthesis. Box 2 has 2 rows. Row 1, house 2 colon House. Row 2, has 3 lines. Line 1, id equal to 1 and it makes an arrow forward to represent the 1 box. Line 2, area equal to 1750 period 50, makes an arrow forward to represent the 1750.50 box. Line 3, when Built and it makes an arrow forward to represent the reference box. Reference boxes are represented by other boxes, which have 2 rows. Row 1, when Built colon Date. Row 2, date object contents.

<!-- Slide number: 35 -->
# Interfaces versus Abstract Classes (1 of 2)
In an interface, the data must be constants; an abstract class can have all types of data.
Each method in an interface has only a signature without implementation; an abstract class can have concrete methods.
| Blank | Variables | Constructors | Methods |
| --- | --- | --- | --- |
| Abstract class | No restrictions. | Constructors are invoked by subclasses through constructor chaining. An abstract class cannot be instantiated using the new operator. | No restrictions. |
| Interface | All variables must be public static final | No constructors. An interface cannot be instantiated using the new operator. | All methods must be public abstract instance methods |

<!-- Slide number: 36 -->
# Interfaces versus Abstract Classes (2 of 2)
All classes share a single root, the Object class, but there is no single root for interfaces. Like a class, an interface also defines a type. A variable of an interface type can reference any instance of the class that implements the interface. If a class extends an interface, this interface plays the same role as a superclass. You can use an interface as a data type and cast a variable of an interface type to its subclass, and vice versa.

![An object shows the Interfaces versus Abstract Classes, cont. It has 8 boxes, and they are interrelated with each other. Boxes are Interface underscore 2, Interface_1, Interface1, Interface 2_2, Interface 2_1, Class2, Class1 and Object.](Picture7.jpg)
Suppose that c is an instance of Class2. c is also an instance of Object, Class1, Interface1, Interface1_1, Interface1_2, Interface2_1, and Interface2_2.

<!-- Slide number: 37 -->
# Caution: Conflict Interfaces
In rare occasions, a class may implement two interfaces with conflict information (e.g., two same constants with different values or two methods with same signature but different return type). This type of errors will be detected by the compiler.

<!-- Slide number: 38 -->
# Whether to Use an Interface or a Class?
Abstract classes and interfaces can both be used to model common features. How do you decide whether to use an interface or a class? In general, a strong is-a relationship that clearly describes a parent-child relationship should be modeled using classes. For example, a staff member is a person. A weak is-a relationship, also known as an is-kind-of relationship, indicates that an object possesses a certain property. A weak is-a relationship can be modeled using interfaces. For example, all strings are comparable, so the String class implements the Comparable interface. You can also use interfaces to circumvent single inheritance restriction if multiple inheritance is desired. In the case of multiple inheritance, you have to design one as a superclass, and others as interface.

<!-- Slide number: 39 -->
# The Rational Class

![The computer code shows the Rational Class. For long description in Notes pane, press F6.](Picture6.jpg)
Rational
TestRationalClass

### Notes:
 It has 3 boxes, and these are interrelated with each other, but there is one Rational box that has 3 rows and shows the coding. Box 1, java period lang period Number. Box 2, java period lang period Comparable less than Rational greater than. Box 3 shows the Rational, it is interrelated with forwarded two boxes, and it indicates Add, Subtract, Multiply, Divide. Now, the Rational box has 3 rows. Row 1 shows the Rational. Row 2, has 2 lines. Line 1, minus numerator colon long. Line 2, minus denominator colon long. Row 3 has 15 lines. Line 1, plus Rational, open parenthesis close parenthesis. Line 2, plus Rational, open parenthesis numerator colon long comma. Line 3, denominator colon long close parenthesis. Line 4, plus get Numerator open parenthesis close parenthesis colon long. Line 5, plus get Denominator open parenthesis close parenthesis colon long. Line 6, plus add open parenthesis second Rational colon Rational close parenthesis colon. Line 7, Rational. Line 8, plus subtract open parenthesis second Rational colon. Line 9, Rational close parenthesis colon Rational. Line 10, plus multiply open parenthesis second Rational 1 colon. Line 11, Rational close parenthesis colon Rational. Line 12, plus divide open parenthesis second Rational colon. Line 13, Rational close parenthesis colon Rational. Line 14, plus to String open parenthesis close parenthesis colon String. Line 15, minus gcd open parenthesis n colon long comma d colon long close parenthesis colon long.

Rational: https://liveexample.pearsoncmg.com/html/Rational.html
TestRationalClass: https://liveexample.pearsoncmg.com/html/TestRationalClass.html

<!-- Slide number: 40 -->
# Designing a Class (1 of 5)
(Coherence) A class should describe a single entity, and all the class operations should logically fit together to support a coherent purpose. You can use a class for students, for example, but you should not combine students and staff in the same class, because students and staff have different entities.

<!-- Slide number: 41 -->
# Designing a Class (2 of 5)
(Separating responsibilities) A single entity with too many responsibilities can be broken into several classes to separate responsibilities. The classes String, StringBuilder, and StringBuffer all deal with strings, for example, but have different responsibilities. The String class deals with immutable strings, the StringBuilder class is for creating mutable strings, and the StringBuffer class is similar to StringBuilder except that StringBuffer contains synchronized methods for updating strings.

<!-- Slide number: 42 -->
# Designing a Class (3 of 5)
Classes are designed for reuse. Users can incorporate classes in many different combinations, orders, and environments. Therefore, you should design a class that imposes no restrictions on what or when the user can do with it, design the properties to ensure that the user can set properties in any order, with any combination of values, and design methods to function independently of their order of occurrence.

<!-- Slide number: 43 -->
# Designing a Class (4 of 5)
Provide a public no-arg constructor and override the equals method and the toString method defined in the Object class whenever possible.

<!-- Slide number: 44 -->
# Designing a Class (5 of 5)
Follow standard Java programming style and naming conventions. Choose informative names for classes, data fields, and methods. Always place the data declaration before the constructor, and place constructors before methods. Always provide a constructor and initialize variables to avoid programming errors.

<!-- Slide number: 45 -->
# Using Visibility Modifiers (1 of 2)
Each class can present two contracts – one for the users of the class and one for the extenders of the class. Make the fields private and accessor methods public if they are intended for the users of the class. Make the fields or method protected if they are intended for extenders of the class. The contract for the extenders encompasses the contract for the users. The extended class may increase the visibility of an instance method from protected to public, or change its implementation, but you should never change the implementation in a way that violates that contract.

<!-- Slide number: 46 -->
# Using Visibility Modifiers (2 of 2)
A class should use the private modifier to hide its data from direct access by clients. You can use get methods and set methods to provide users with access to the private data, but only to private data you want the user to see or to modify. A class should also hide methods not intended for client use. The gcd method in the Rational class is private, for example, because it is only for internal use within the class.

<!-- Slide number: 47 -->
# Using the Static Modifier
A property that is shared by all the instances of the class should be declared as a static property.

<!-- Slide number: 48 -->
# Records
Often you need to create a simple object to hold data. Before Java 16, you have to define a class and write a lot of boilerplate code. This is tedious, however. Since Java 14, you can define a record to simplify coding. For example, the following code defines a record for Teachers.
public record Teacher(String name, java.util.Date date) {}
This simple one-line code is roughly equivalent to the following class definition in LiveExample 13.14.
Teacher

### Notes:
Teacher: https://liveexample.pearsoncmg.com/html/Teacher.html

<!-- Slide number: 49 -->
# Copyright
This work is protected by United States copyright laws and is provided solely for the use of instructors in teaching their courses and assessing student learning. Dissemination or sale of any part of this work (including on the World Wide Web) will destroy the integrity of the work and is not permitted. The work and materials from it should never be made available to students except by instructors using the accompanying text in their classes. All recipients of this work are expected to abide by these restrictions and to honor the intended pedagogical purposes and the needs of other instructors who rely on these materials.

![Warning](Graphic6.jpg)

### Notes:
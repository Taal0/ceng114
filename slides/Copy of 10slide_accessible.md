<!-- Slide number: 1 -->
# Introduction to Java Programming and Data Structures
Thirteenth Edition

![Front Cover: Introduction to Java Programming and Data Structures Thirteenth Edition by Liang.](Picture8.jpg)
Chapter 10
Thinking in Objects
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
You see the advantages of object-oriented programming from the preceding chapter. This chapter will demonstrate how to solve problems using the object-oriented paradigm.

<!-- Slide number: 3 -->
# Objectives (1 of 2)
10.1 To apply class abstraction to develop software (§10.2).
10.2 To explore the differences between the procedural paradigm and object-oriented paradigm (§10.3).
10.3 To discover the relationships between classes (§10.4).
10.4 To design programs using the object-oriented paradigm (§§10.5–10.6).
10.5 To create objects for primitive values using the wrapper classes (Byte, Short, Integer, Long, Float, Double, Character, and Boolean) (§10.7).
10.6 To simplify programming using automatic conversion between primitive types and wrapper class types (§10.8).
10.7 To use the BigInteger and BigDecimal classes for computing very large numbers with arbitrary precisions (§10.9).

<!-- Slide number: 4 -->
# Objectives (2 of 2)
10.8 To use the String class to process immutable strings (§10.10).
10.9 To use the StringBuilder and StringBuffer classes to process mutable strings (§10.11).

<!-- Slide number: 5 -->
# Class Abstraction and Encapsulation
Class abstraction means to separate class implementation from the use of the class. The creator of the class provides a description of the class and let the user know how the class can be used. The user of the class does not need to know how the class is implemented. The detail of implementation is encapsulated and hidden from the user.

![A box for class is shaded and labeled, class implementation is like a black box hidden from the clients. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
 Next to this box is a box with text, class contacts (Signatures of public methods and public constants). There is a two way arrow between this box and a box to the right. The text in the box on the right reads, clients use the class through the contact of the class.

<!-- Slide number: 6 -->
# Designing the Loan Class

![Fifteen rows of codes and descriptions for loan as follows. For long description in Notes pane, press F6.](Picture5.jpg)
Loan
TestLoanClass

### Notes:
Row 1. annuallnterestRate colon double. The annual interest rate of the loan (default, 2.5).
Row 2. numberOfYears colon int. The number of years for the loan (default, 1).
Row 3. loanAmount colon double. The loan amount (default, 1000).
Row 4. loanDate colon Date. The date this loan was created.
Row 5. +Loan left parenthesis right parenthesis. Constructs a default Loan object.
Row 6. +Loan left parenthesis annualInterestRate colon double, numberOfYears colon int, loanAmount colon double right parenthesis. Constructs a loan with specified interest rate, years, and loan amount.
Row 7. +getAnnuallnterestRate left parenthesis right parenthesis colon double. Returns the annual interest rate of this loan.
Row 8. +getNumberOfYears left parenthesis right parenthesis colon int. Returns the number of the years of this loan.
Row 9. +getLoanAmount left parenthesis right parenthesis colon double. Returns the amount of this loan.
Row 10. +getLoanDate left parenthesis right parenthesis colon Date. Returns the date of the creation of this loan.
Row 11. +setAnnualInterestRate left parenthesis annuallnterestRatecolon double right parenthesis colon void. Sets a new annual interest rate to this loan.
Row 12. +setNumberOfYears left parenthesis numberOfYearscolon int right parenthesis colon void. Sets a new number of years to this loan.
Row 13. +setLoanAmount left parenthesis loanAmountcolon double right parenthesis colon void. Sets a new amount to this loan.
Row 14. +getMonthlyPayment left parenthesis right parenthesis colon double. Returns the monthly payment of this loan.
Row 15. +getTotalPayrnent left parenthesis right parenthesis colon double. Returns the total payment of this loan.

Loan: https://liveexample.pearsoncmg.com/html/Loan.html

TestLoanClass: https://liveexample.pearsoncmg.com/html/TestLoanClass.html

<!-- Slide number: 7 -->
# Object-Oriented Thinking
Chapters 1-8 introduced fundamental programming techniques for problem solving using loops, methods, and arrays. The studies of these techniques lay a solid foundation for object-oriented programming. Classes provide more flexibility and modularity for building reusable software. This section improves the solution for a problem introduced in Chapter 3 using the object-oriented approach. From the improvements, you will gain the insight on the differences between the procedural programming and object-oriented programming and see the benefits of developing reusable code using objects and classes.

<!-- Slide number: 8 -->
# The BMI Class

![Eight rows of codes and descriptions for BMI as follows. For long description in Notes pane, press F6.](Picture5.jpg)
BMI
UseBMIClass

### Notes:
Row 1. name colon String. The name of the person.
Row 2. age colon int. The age of the person.
Row 3. weight colon double. The weight of the person in pounds.
Row 4. height colon double. The height of the person in inches.
Row 5. +BMI left parenthesis name colon String, age colon int, weight colon double, height colon double right parenthesis. Creates a BMI object with the specified name, age, weight, and height.
Row 6. +BMI left parenthesis name colon String, weight colon double, height colon double right parenthesis. Creates a BMI object with the specified name, weight, height, and a default age 20.
Row 7. +getBMI left parenthesis right parenthesis colon double. Returns the BMI.
Row 8. +getStatus left parenthesis right parenthesis colon String. Returns the BMI status (for example, normal, overweight, etc.)
The text reads, the get methods for these data fields are /provided in the class, but omitted in the UML diagram for brevity.

BMI: https://liveexample.pearsoncmg.com/html/BMI.html

UseBMIClass: https://liveexample.pearsoncmg.com/html/UseBMIClass.html

<!-- Slide number: 9 -->
# Class Relationships
Association
Aggregation
Composition
Inheritance (Chapter 13)
Association: is a general binary relationship that describes an activity between two classes.

![A diagram shows a line connecting box for student on the left to box for course on the right. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
The line is labeled, 5.60 near the student end and asterisk symbol near the course end. The text reads, take followed by a right arrow. Another line connects box for course to the box for faculty. The line is labeled, 0.3 near course end and 1 near faculty end. The text reads, teach followed by a left arrow. The text teacher is written below faculty.

<!-- Slide number: 10 -->
# Object Composition
Composition is actually a special case of the aggregation relationship. Aggregation models has-a relationships and represents an ownership relationship between two objects. The owner object is called an aggregating object and its class an aggregating class. The subject object is called an aggregated object and its class an aggregated class.

![A diagram shows a line connecting box for name on the left to box for student on the right. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
The line is labeled, 1 near each end. A shaded rhombus attached to the box for student on the left side is labeled, composition. Another line connects box for student to fox for address. The line is labeled, 1.3 near student end and 1 near address end. A rhombus attached to the box for student on the right side is labeled, aggregation.

<!-- Slide number: 11 -->
# Class Representation
An aggregation relationship is usually represented as a data field in the aggregating class. For example, the relationship in Figure 10.6 can be represented as follows:

![The left side Computer code shows the Aggregated class. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
The computer code has 3 lines. Line 1, public class Name open braces. Line 2, period period period. Line 3, close braces.
A middle computer code shows the Aggregating class. The computer code has 5 lines. Line 1, public class Student open braces. Line 2, private Name name semicolon. Line 3, private Address address semicolon. Line 4, period period period. Line 5, close braces.
The right side computer code shows the Aggregated class. The computer code has 3 lines. Line 1, public class Address open braces. Line 2, period period period. Line 3, close braces.

<!-- Slide number: 12 -->
# Aggregation or Composition
Since aggregation and composition relationships are represented using classes in similar ways, many texts don’t differentiate them and call both compositions.

### Notes:

<!-- Slide number: 13 -->
# Aggregation Between Same Class (1 of 2)
Aggregation may exist between objects of the same class. For example, a person may have a supervisor.

![A diagram shows box for person attached to a larger box for supervisor near top left corner. The sides of the box for supervisor are labeled, 1.](Picture6.jpg)
public class Person {
// The type for the data is the class itself
private Person supervisor;
...
}

### Notes:

<!-- Slide number: 14 -->
# Aggregation Between Same Class (2 of 2)
What happens if a person has several supervisors?

![A diagram shows box for person attached to a larger box for supervisor near top left corner. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
A rhombus is attached to the box for person on the right side. The top side of the box for supervisor is labeled, 1 and the side on the left is labeled, m. The code reads as follows.
Public class Person left brace
Ellipsis
private Person left bracket right bracket supervisor semi colon
right brace.

<!-- Slide number: 15 -->
# Example: The Course Class

![Nine rows of codes and descriptions for course as follows. For long description in Notes pane, press F6.](Picture5.jpg)
Course
TestCourse

### Notes:
Row 1. courseName colon String. The name of the course.
Row 2. students colon String left bracket right bracket. An array to store the students for the course.
Row 3. numberOfStudents colon int. The number of students (default colon 0).
Row 4. +Course left parenthesis courseName colon String right parenthesis. Creates a course with the specified name.
Row 5. +getCourseName left parenthesis right parenthesis colon String. Returns the course name.
Row 6. +addStudent left parenthesis student colon String right parenthesis colon void. Adds a new student to the course.
Row 7. +dropStudent left parenthesis student colon String right parenthesis colon void. Drops a student from the course.
Row 8. +getStudents left parenthesis right parenthesis colon String left bracket right bracket. Returns the students in the course.
Row 9. +getNumberOfStudents left parenthesis right parenthesis colon int. Returns the number of students in the course.

Course: https://liveexample.pearsoncmg.com/html/Course.html

TestCourse: https://liveexample.pearsoncmg.com/html/TestCourse.html

<!-- Slide number: 16 -->
# Example: The Stackofintegers Class

![Nine rows of codes and description for StackOfIntegers as follows. For long description in Notes pane, press F6.](Picture6.jpg)
TestStackOfIntegers

### Notes:
Row 1. elements colon int left bracket right bracket. An array to store integers in the stack.
Row 2. size colon int. The number of integers in the stack.
Row 3. +StackOfIntegers left parenthesis right parenthesis. Constructs an empty stack with a default capacity of 16.
Row 4. +StackOflntegers left parenthesis capacity colon int right parenthesis. Constructs an empty stack with a specified capacity.
Row 5. +empty left parenthesis right parenthesis colon Boolean. Returns true if the stack is empty.
Row 6. +peek left parenthesis right parenthesis colon int. Returns the integer at the top of the stack without removing it from the stack.
Row 7. +push left parenthesis value colon int right parenthesis colon int. Stores an integer into the top of the stack.
Row 8. +pop left parenthesis right parenthesis colon int. Removes the integer at the top of the stack and returns it.
Row 9. +getSize left parenthesis right parenthesis colon int. Returns the number of elements in the stack.

TestStackOfIntegers: https://liveexample.pearsoncmg.com/html/TestStackOfIntegers.html

<!-- Slide number: 17 -->
# Designing the Stackofintegers Class

![An illustraton shows a stake of integers in an array. Data 1 is stored first, data 2 Is stored above data 2, and data 3 is stored above data 2. While retrieving the data, data 3 is retrieved first, then data 2 is retrieved, and finally data 1 is retrived.](Picture6.jpg)

### Notes:

<!-- Slide number: 18 -->
# Implementing Stackofintegers Class

![An illustration shows a vertical column with six boxes labeled from top to bottom as elements array for capacity minus 1, vertical ellipsis elements array for size minus 1, vertical ellipsis, element array 1, element array 0. For long description in Notes pane, press F6.](Picture6.jpg)
StackOfIntegers

### Notes:
The box elements array for size minus 1 is labeled, top and last box is labeled, bottom. The boxes from the box labeled, top to box labeled, bottom are labeled, size. The whole vertical column is labeled, capacity.

StackOfIntegers: https://liveexample.pearsoncmg.com/html/StackOfIntegers.html

<!-- Slide number: 19 -->
# Wrapper Classes
Boolean
Character
Short
Byte
Integer
Long
Float
Double
Note: (1) The wrapper classes do not have no-arg constructors. (2) The instances of all wrapper classes are immutable, i.e., their internal values cannot be changed once the objects are created.

<!-- Slide number: 20 -->
# The Integer and Double Classes

![The computer code shows The Integer and Double Classes. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
The computer code is divided into 2 boxes. The left box shows the java period lang period Integer. The computer code has of  14 lines.
Line1, minus value colon int
Line 2, plus MAX_VALUE colon int
Line 3, plus MIN_VALUE colon int
Line 4, plus Integer open parenthesis value colon int close parenthesis
Line 5, plus Integer open parenthesiss colon String close parenthesis
Line 6, plus byte Value open parenthesis close parenthesis colon byte
Line 7, plus short Value open parenthesis close parenthesis colon short
Line 8, plus intValue open parenthesis close parenthesis colon int
Line 9, plus longValue open parenthesis close parenthesis colon long
Line 10, plus float Value open parenthesis close parenthesis colon float
Line 11, plus double Value open parenthesis close parenthesis colon double
Line 12, plus compare To open parenthesis colon Integer close parenthesis colon int
Line 13, plus to String open parenthesis close parenthesis colon String
Line 14, plus value Of open parenthesis colon String close parenthesis colon Integer
Line 15, plus value Of open parenthesis colon String, radix colon int close parenthesis colon Integer
Line 16, plus parseInt open parenthesis colon String close parenthesis colon int
Line 17, plus parseInt open parenthesis colon String, radix colon int close parenthesis colon int

The Right box shows java period lang period Double.
The computer code has of  17 lines.
Line1, minus value colon double
Line 2, plus MAX_VALUE colon double
Line 3, plus MIN_VALUE colon double
Line 4, plus Double open parenthesis value colon double close parenthesis
Line 5, plus Double open parenthesis colon String close parenthesis
Line 6, plus byte Value open parenthesis close parenthesis colon byte
Line 7, plus short Value open parenthesis close parenthesis colon short
Line 8, plus intValue open parenthesis close parenthesis colon int
Line 9, plus longValue open parenthesis close parenthesis colon long
Line 10, plus float Value open parenthesis close parenthesis colon float
Line 11, plus double Value open parenthesis close parenthesis colon double
Line 12, plus compare To open parenthesis colon Double close parenthesis colon into
Line 13, plus to String open parenthesis close parenthesis colon String
Line 14, plus value Of open parenthesis colon String close parenthesis colon Double
Line 15, plus value Of open parenthesis colon String, radix colon int close parenthesis colon Double
Line 16, plus parse Double open parenthesis colon String close parenthesis colon double
Line 17, plus parse Double open parenthesis colon String, radix colon int close parenthesis colon double

<!-- Slide number: 21 -->
# The Integer Class and the Double Class
Constructors
Class Constants MAX_VALUE, MIN_VALUE
Conversion Methods

<!-- Slide number: 22 -->
# Numeric Wrapper Class Constructors
You can construct a wrapper object either from a primitive data type value or from a string representing the numeric value. The constructors for Integer and Double are:
public Integer(int value)
public Integer(String s)
public Double(double value)
public Double(String s)

<!-- Slide number: 23 -->
# Numeric Wrapper Class Constants
Each numerical wrapper class has the constants MAX_VALUE and MIN_VALUE. MAX_VALUE represents the maximum value of the corresponding primitive data type. For Byte, Short, Integer, and Long, MIN_VALUE represents the minimum byte, short, int, and long values. For Float and Double, MIN_VALUE represents the minimum positive float and double values. The following statements display the maximum integer (2,147,483,647), the minimum positive float (1.4E-45), and the maximum double floating-point number (1.79769313486231570e+308d).

<!-- Slide number: 24 -->
# Conversion Methods
Each numeric wrapper class implements the abstract methods doubleValue, floatValue, intValue, longValue, and shortValue, which are defined in the Number class. These methods “convert” objects into primitive type values.

<!-- Slide number: 25 -->
# The Static valueof Methods
The numeric wrapper classes have a useful class method, valueOf(String s). This method creates a new object initialized to the value represented by the specified string. For example:
Double doubleObject = Double.valueOf("12.4");
Integer integerObject = Integer.valueOf("12");

<!-- Slide number: 26 -->
# The Methods for Parsing Strings Into Numbers
You have used the parseInt method in the Integer class to parse a numeric string into an int value and the parseDouble method in the Double class to parse a numeric string into a double value. Each numeric wrapper class has two overloaded parsing methods to parse a numeric string into an appropriate numeric value.

<!-- Slide number: 27 -->
# Automatic Conversion Between Primitive Types and Wrapper Class Types
J D K 1.5 allows primitive type and wrapper classes to be converted automatically. For example, the following statement in (a) can be simplified as in (b):

![The computer code shows the Automatic Conversion Between Primitive Types and Wrapper Class Types. For long description in Notes pane, press F6.](Picture10.jpg)

![Integer left bracket right bracket Array = left brace 1, 2, 3 right brace semi colon. For long description in Notes pane, press F6.](Picture12.jpg)

### Notes:
The left computer code shows the coding of Integer open bracket close bracket int Array equals to open braces new Integer open parenthesis 2 close parenthesis comma  new Integer open parenthesis 4 close parenthesis, new Integer open parenthesis3 close parenthesis close braces semicolon which is equivalent to right computer code that shows the Integer open bracket close bracket int Array equals to open braces 2, 4, 3 close braces semicolon are New JDK 1.5 boxing.

System.out.prinln left parenthesis intArray left bracket 0 right bracket + intArray left bracket 1 right bracket + intArray left bracket 2 right bracket. The arrays are together labeled, unboxing.

<!-- Slide number: 28 -->
# Biginteger and Bigdecimal (1 of 2)
If you need to compute with very large integers or high precision floating-point values, you can use the BigInteger and BigDecimal classes in the java.math package. Both are immutable. Both extend the Number class and implement the Comparable interface.

<!-- Slide number: 29 -->
# Biginteger and Bigdecimal (2 of 2)
BigInteger a = new BigInteger("9223372036854775807");
BigInteger b = new BigInteger("2");
BigInteger c = a.multiply(b); // 9223372036854775807 * 2
System.out.println(c);
BigDecimal a = new BigDecimal(1.0);
BigDecimal b = new BigDecimal(3);
BigDecimal c = a.divide(b, 20, BigDecimal.ROUND_UP);
System.out.println(c);
LargeFactorial

### Notes:
LargeFactorial: https://liveexample.pearsoncmg.com/html/LargeFactorial.html

<!-- Slide number: 30 -->
# The String Class
Constructing a String:
String message = "Welcome to Java“;
String message = new String("Welcome to Java“);
String s = new String();
Obtaining String length and Retrieving Individual Characters in a string
String Concatenation (concat)
Substrings (substring(index), substring(start, end))
Comparisons (equals, compareTo)
String Conversions
Finding a Character or a Substring in a String
Conversions between Strings and Arrays
Converting Characters and Numeric Values to Strings

<!-- Slide number: 31 -->
# Constructing Strings
String newString = new String(stringLiteral);
String message = new String("Welcome to Java");
Since strings are used frequently, Java provides a shorthand initializer for creating a string:
String message = "Welcome to Java";

<!-- Slide number: 32 -->
# Strings Are Immutable
A String object is immutable; its contents cannot be changed. Does the following code change the contents of the string?
String s = "Java";
s = "HTML";

<!-- Slide number: 33 -->
# Trace Code (1 of 5)

![The computer code shows the Strings Are Immutable. The computer code has 2 lines. Line 1, String s equals to double quotes Java double quotes semi colon. Line 2, s equals to double quotes HTML double quotes semi colon .](Picture7.jpg)

![The upward computer code shows the Trace code. It has two lines. For long description in Notes pane, press F6.](Picture9.jpg)

### Notes:
 Line 1, String s equal to double quote Java double quote semicolon. Line 2, s equal to double quote HTML double quote semicolon.
The left downward side computer code shows the Trace code. It has 3 lines. Line 1, After executing String s equal to double quote Java double quote semicolon. Line 2, shows the one box which is for s and it's represent an arrow which shows the another box which has 2 rows. Row 1, colon String. Row 2, String object for double quote Java double quote and this row shows the Contents cannot be changed.
The left rightward side computer code shows the Trace code. It has 5 lines. Line 1, After executing s equal to double quote HTML double quote semicolon. Line 2, shows the one box which is for s and it's represent two arrows which shows the two boxes that is one box has 2 rows and another box also has 2 rows. So, the one box shows the 2 rows coding. Row 1, colon String. Row 2, String object for double quote Java double quote. Now, the another box also has two rows which shows the coding. Row 1, colon String. Row 2, String object for double quote HTML double quote.

<!-- Slide number: 34 -->
# Trace Code (2 of 5)

![String s = start double quotation marks Java end double quotation marks semi colon S = start double quotation marks HTML end double quotation marks semi colon.](Picture9.jpg)

![The upward computer code shows the Trace code. It has two lines. For long description in Notes pane, press F6.](Picture11.jpg)

### Notes:
 Line 1, String s equal to double quote Java double quote semicolon. Line 2, s equal to double quote HTML double quote semicolon.
The left downward side computer code shows the Trace code. It has 3 lines. Line 1, After executing String s equal to double quote Java double quote semicolon. Line 2, shows the one box which is for s and it's represent an arrow which shows the another box which has 2 rows. Row 1, colon String. Row 2, String object for double quote Java double quote and this row shows the Contents cannot be changed.
The left rightward side computer code shows the Trace code. It has 5 lines. Line 1, After executing s equal to double quote HTML double quote semicolon. Line 2, shows the one box which is for s and it's represent two arrows which shows the two boxes that is one box has 2 rows and another box also has 2 rows. So, the one box shows the 2 rows coding. Row 1, colon String. Row 2, String object for double quote Java double quote. Now, the another box also has two rows which shows the coding. Row 1, colon String. Row 2, String object for double quote HTML double quote."

<!-- Slide number: 35 -->
# Interned Strings
Since strings are immutable and are frequently used, to improve efficiency and save memory, the J V M uses a unique instance for string literals with the same character sequence. Such an instance is called interned. For example, the following statements:

<!-- Slide number: 36 -->
# Examples (1 of 4)

![String s1 = Start double quotation marks Welcome to Java end double quotation marks semi colon. For long description in Notes pane, press F6.](Picture8.jpg)
A new object is created if you use the new operator.
If you use the string initializer, no new object is created if the interned object is already created.
display
s1 == s is false
s1 == s3 is true

### Notes:
String s2 = new String left parenthesis start double quotation marks Welcome to Java end double quotation marks right parenthesis semi colon
String s3 = Start double quotation marks Welcome to Java end double quotation marks semi colon
System.out.println left parenthesis start double quotation marks s1 == s2 is end double quotation marks + left parenthesis s1 == s2 right parenthesis, right parenthesis semi colon
System.out.println left parenthesis start double quotation marks s1 == s3 is end double quotation marks + left parenthesis s1 == s3 right parenthesis, right parenthesis semi colon
An illustration shows two boxes each labaled, String. The text in the top box reads, Interned string object for start double quotation marks Welcome to Java end double quotation marks. The box is labeled, s1 and s3.
The text in the bottom box reads, a string object for start double quotation marks Welcome to Java end double quotation marks. The box is labeled, s2.

<!-- Slide number: 37 -->
# Trace Code (3 of 5)

![String s1 = Start double quotation marks Welcome to Java end double quotation marks semi colon. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
String s2 = new String left parenthesis start double quotation marks Welcome to Java end double quotation marks right parenthesis semi colon
String s3 = Start double quotation marks Welcome to Java end double quotation marks semi colon
An illustration shows boxes each labaled, String. The text in the box reads, Interned string object for start double quotation marks Welcome to Java end double quotation marks. The box is labeled, s1.
The code for string s1 and the box are highlighted.

<!-- Slide number: 38 -->
# Trace Code (4 of 5)

![String s1 = Start double quotation marks Welcome to Java end double quotation marks semi colon. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
String s2 = new String left parenthesis start double quotation marks Welcome to Java end double quotation marks right parenthesis semi colon
String s3 = Start double quotation marks Welcome to Java end double quotation marks semi colon
An illustration shows two boxes each labaled, String. The text in the top box reads, Interned string object for start double quotation marks Welcome to Java end double quotation marks. The box is labeled, s1.
The text in the bottom box reads, a string object for start double quotation marks Welcome to Java end double quotation marks. The box is labeled, s2.
The code and box for s2 are highlighted.

<!-- Slide number: 39 -->
# Trace Code (5 of 5)

![String s1 = Start double quotation marks Welcome to Java end double quotation marks semi colon. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
String s2 = new String left parenthesis start double quotation marks Welcome to Java end double quotation marks right parenthesis semi colon
String s3 = Start double quotation marks Welcome to Java end double quotation marks semi colon
System.out.println left parenthesis start double quotation marks s1 == s2 is end double quotation marks + left parenthesis s1 == s2 right parenthesis, right parenthesis semi colon
System.out.println left parenthesis start double quotation marks s1 == s3 is end double quotation marks + left parenthesis s1 == s3 right parenthesis, right parenthesis semi colon
An illustration shows two boxes each labaled, String. The text in the top box reads, Interned string object for start double quotation marks Welcome to Java end double quotation marks. The box is labeled, s1 and s3.
The text in the bottom box reads, a string object for start double quotation marks Welcome to Java end double quotation marks. The box is labeled, s2.
The code and box for s3 are highlighted.

<!-- Slide number: 40 -->
# Replacing and Splitting Strings (1 of 2)

![Four rows of codes and descriptions for java.lang.String as follows. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
Row 1. +replace left parenthesis oldChar colon char, newChar colon char right parenthesis colon String. Returns a new string that replaces all matching character in this string with the new character.
Row 2. +replaceFirst left parenthesis oldString colon String, newString colon String right parenthesis colon String. Returns a new string that replaces the first matching substring in this string with the new substring.
Row 3. +replaceAll left parenthesis oldString colon String, newString colon String right parenthesis colon String. Returns a new string that replace all matching substrings in this string with the new substring.
Row 4. +split left parenthesis delimiter colon String right parenthesis colon String left bracket right bracket. Returns an array of strings consisting of the substrings split by the delimiter.

<!-- Slide number: 41 -->
# Examples (2 of 4)
"Welcome".replace('e', 'A') returns a new string, WAlcomA.
"Welcome".replaceFirst("e", "AB") returns a new string, WABlcome.
"Welcome".replace("e", "AB") returns a new string, WABlcomAB.
"Welcome".replace("el", "AB") returns a new string, WABcome.

<!-- Slide number: 42 -->
# Splitting a String
String[] tokens = "Java#HTML#Perl".split("#", 0);
for (int i = 0; i < tokens.length; i++)
System.out.print(tokens[i] + " ");
displays
Java H T M L Perl

<!-- Slide number: 43 -->
# Matching, Replacing and Splitting by Patterns (1 of 3)
You can match, replace, or split a string by specifying a pattern. This is an extremely useful and powerful feature, commonly known as regular expression. Regular expression is complex to beginning students. For this reason, two simple patterns are used in this section. Please
refer to Supplement
“Regular Expressions,” for
further studies.
"Java".matches("Java");
"Java".equals("Java");
"Java is fun".matches("Java.*");
"Java is cool".matches("Java.*");

<!-- Slide number: 44 -->
# Matching, Replacing and Splitting by Patterns (2 of 3)
The replaceAll, replaceFirst, and split methods can be used with a regular expression. For example, the following statement returns a new string that replaces $, +, or # in "a+b$#c" by the string NNN.
String s = "a+b$#c".replaceAll("[$+#]", "NNN");
System.out.println(s);
Here the regular expression [$+#] specifies a pattern that matches $, +, or #. So, the output is aNNNbNNNNNNc.

<!-- Slide number: 45 -->
# Matching, Replacing and Splitting by Patterns (3 of 3)
The following statement splits the string into an array of strings delimited by some punctuation marks.
String[] tokens = "Java,C?C#,C++".split("[.,:;?]");
for (int i = 0; i < tokens.length; i++)
System.out.println(tokens[i]);

<!-- Slide number: 46 -->
# Convert Character and Numbers to Strings
The String class provides several static valueOf methods for converting a character, an array of characters, and numeric values to strings. These methods have the same name valueOf with different argument types char, char[], double, long, int, and float. For example, to convert a double value to a string, use String.valueOf(5.44). The return value is string consists of characters ‘5’, ‘.’, ‘4’, and ‘4’.

<!-- Slide number: 47 -->
# StringBuilder and StringBuffer
The StringBuilder/StringBuffer class is an alternative to the String class. In general, a StringBuilder/StringBuffer can be used wherever a string is used. StringBuilder/StringBuffer is more flexible than String. You can add, insert, or append new contents into a string buffer, whereas the value of a String object is fixed once the string is created.

<!-- Slide number: 48 -->
# StringBuilder Constructors

![Three rows of codes and descriptions for java.lang.StringBuilder as follows. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
Row 1. +StringBuilder left parenthesis right parenthesis. Constructs an empty string builder with capacity 16.
Row 2. +StringBuilder left parenthesis capacity colon int right parenthesis. Constructs a string builder with the specified capacity.
Row 3. +StringBuilder left parenthesis S colon String right parenthesis. Construcyts a string builder with the specified string.

<!-- Slide number: 49 -->
# Modifying Strings in the Builder

![Thirteen rows of codes and descriptions for java.lang.StringBuilder as follows. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
Row 1. +append left parenthesis data colon char left bracket right bracket right parenthesis colon StringBuilder. Appends a char array into this string builder.
Row 2. +append left parenthesis data colon char left bracket right bracket, offset colon int, len colon int right parenthesis colon StringBuilder. Appends a subarray in data into this string builder.
Row 3. +append left parenthesis v colon aPrimitiveType right parenthesis colon String Builder. Appends a primitive type value as a string to this builder.
Row 4. +append left parenthesis s colon String right parenthesis colon StringBuilder. Appends a string to this string builder.
Row 5. +delete left parenthesis startIndex colon int, endlndex colon int right parenthesis colon StringBuilder. Deletes characters from startlndex to endlndex.
Row 6. +deleteCharAt left parenthesis index colon int right parenthesis colon StringBuilder. Deletes a character at the specified index.
Row 7. +insert left parenthesis index colon int, data colon char left bracket right bracket, offset colon int, len colon int right parenthesis colon StringBuilder. Inserts a subarray of the data in the array to the builder at the specified index.
Row 8. +insert left parenthesis offset colon int, data colon char left bracket right bracket right parenthesis colon StringBuilder. Inserts data into this builder at the position offset.
Row 9. +insert left parenthesis offset colon int, b colon aPrimitiveType right parenthesis colon StringBuilder. Inserts a value converted to a string into this builder.
Row 10. +insert left parenthesis offset colon int, s colon String right parenthesis colon StringBuilder. Inserts a string into this builder at the position offset.
Row 11. +replace left parenthesis startlndex colon int, endlndex colon int, s colon String right parenthesis colon StringBuilder. Replaces the characters in this builder from startlndex to endlndex with the specified string.
Row 12. +reverse left parenthesis right parenthesis colon StringBuilder. Reverses the characters in the builder.
Row 13. +setCharAt left parenthesis index colon int, ch colon char right parenthesis colon void. Sets a new character at the specified index in this builder.

<!-- Slide number: 50 -->
# Examples (3 of 4)
stringBuilder.append("Java");
stringBuilder.insert(11, "HTML and ");
stringBuilder.delete(8, 11) changes the builder to Welcome Java.
stringBuilder.deleteCharAt(8) changes the builder to
Welcome o Java.
stringBuilder.reverse() changes the builder to avaJ ot emocleW.
stringBuilder.replace(11, 15, "HTML")
changes the builder to Welcome to HTML.
stringBuilder.setCharAt(0, 'w') sets the builder to welcome to Java.

<!-- Slide number: 51 -->
# The toString, capacity, length, setLength, and charAt Methods

![Eight rows of codes and descriptions for java.lang.StringBuilder as follows. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
Row 1. +toString left parenthesis right parenthesis colon String. Returns a string object from the string builder.
Row 2. +capacity left parenthesis right parenthesis colon int. Returns the capacity of this string builder.
Row 3. +charAt left parenthesis index colon int right parenthesis colon char. Returns the character at the specified index.
Row 4. +length left parenthesis right parenthesis colon int. Returns the number of characters in this builder.
Row 5. +setLength left parenthesis newLength colon int right parenthesis colon void. Sets a new length in this builder.
Row 6. +substring left parenthesis startlndex colon int right parenthesis colon String. Returns a substring starting at startlndex.
Row 7. +substring left parenthesis startlndex colon int, endlndex colon int right parenthesis colon String. Returns a substring from startlndex to endlndex 1.
Row 8. +trimToSize left parenthesis right parenthesis colon void. Reduces the storage size used for the string builder.

<!-- Slide number: 52 -->
# Problem: Checking Palindromes Ignoring Non-alphanumeric Characters
This example gives a program that counts the number of occurrence of each letter in a string. Assume the letters are not case-sensitive.
PalindromeIgnoreNonAlphanumeric

### Notes:
PalindromeIgnoreNonAlphanumeric: https://liveexample.pearsoncmg.com/html/PalindromeIgnoreNonAlphanumeric.html

<!-- Slide number: 53 -->
# Regular Expressions
A regular expression (abbreviated regex) is a string that describes a pattern for matching a set of strings. Regular expression is a powerful tool for string manipulations. You can use regular expressions for matching, replacing, and splitting strings.

<!-- Slide number: 54 -->
# Matching Strings
"Java".matches("Java");
"Java".equals("Java");
"Java is fun".matches("Java.*")
"Java is cool".matches("Java.*")
"Java is powerful".matches("Java.*")

<!-- Slide number: 55 -->
# Regular Expression Syntax (1 of 2)
| Regular Expression | Matches | Example |
| --- | --- | --- |
| x | a specified character x | Java matches Java |
| . | any single character | Java matches J..a |
| (ab|cd) | ab or cd | ten matches t(en|im) |
| [abc] | a, b, or c | Java matches Ja[uvwx]a |
| [^abc] | any character except a, b, or c | Java matches Ja[^ars]a |
| [a-z] | a through z | Java matches [A-M]av[a-d] |
| [^a-z] | any character except a through z | Java matches Jav[^b-d] |
| [a-e[m-p]] | a through e or m through p | Java matches [A-G[I-M]]av[a-d] |
| [a-e&& [c-p]] | intersection of a-e with c-p | Java matches [A-P&& [I-M]]av[a-d] |
| \d | a digit, same as [0-9] | Java2 matches "Java[\\d]" |
| \D | a non-digit | $Java matches "[\\D][\\D]ava" |
| \w | a word character | Java1 matches "[\\w]ava[\\w]" |
| \W | a non-word character | $Java matches "[\\W][\\w]ava" |

<!-- Slide number: 56 -->
# Regular Expression Syntax (2 of 2)
| Regular Expression | Matches | Example |
| --- | --- | --- |
| \s | a whitespace character | "Java 2" matches "Java\\s2" |
| \S | a non-whitespace char | Java matches "[\\S]ava" |
| p\* | zero or more occurrences of pattern p | aaaabb matches "a\*\*b" ababab matches "(ab)\*" |
| p+ | one or more occurrences of pattern p | a matches "a+b\*" able matches "(ab)+.\*" |
| p? | zero or one occurrence of pattern p | Java matches "J?Java" Java matches "J?ava" |
| p{n} | exactly n occurrences of pattern p | Java matches "Ja{1}.\*" Java does not match ".{2}" |
| p{n,} | at least n occurrences of pattern p | aaaa matches "a{1,}" a does not match "a{2,}" |
| p{n,m} | between n and m occurrences (inclusive) | aaaa matches "a{1,9}" abb does not match "a{2,9}bb" |

<!-- Slide number: 57 -->
# Replacing and Splitting Strings (2 of 2)

![Four rows of codes and descriptions for java.lang.String as follows. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
Row 1. +matches left parenthesis regex colon String right parenthesis colon Boolean. Returns true if this string matches the pattern.
Row 2. +replaceAll left parenthesis regex colon String, replacement colon String right parenthesis colon String. Returns a new string that replaces all matching substrings with the replacement.
Row 3. +replaceFirst left parenthesis regex colon String, replacement colon String right parenthesis colon String. Returns a new string that replaces the first matching substring with the replacement.
Row 4. +split left parenthesis regex colon String right parenthesis colon String. Returns an array of strings consisting of the substrings split by the matches.

<!-- Slide number: 58 -->
# Examples (4 of 4)
String s = "Java Java Java".replaceAll("v\\w", "wi") ;
String s = "Java Java Java".replaceFirst("v\\w", "wi") ;
String[] s = "Java1HTML2Perl".split("\\d");

<!-- Slide number: 59 -->
# Copyright
This work is protected by United States copyright laws and is provided solely for the use of instructors in teaching their courses and assessing student learning. Dissemination or sale of any part of this work (including on the World Wide Web) will destroy the integrity of the work and is not permitted. The work and materials from it should never be made available to students except by instructors using the accompanying text in their classes. All recipients of this work are expected to abide by these restrictions and to honor the intended pedagogical purposes and the needs of other instructors who rely on these materials.

![Warning](Graphic6.jpg)

### Notes:
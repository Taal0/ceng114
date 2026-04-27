<!-- Slide number: 1 -->
# Introduction to Java Programming and Data Structures
Thirteenth Edition

![Front Cover: Introduction to Java Programming and Data Structures Thirteenth Edition by Liang.](Picture8.jpg)
Chapter 12
Exception Handling and Text I O
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
When a program runs into a runtime error, the program terminates abnormally. How can you handle the runtime error so that the program can continue to run or terminate gracefully? This is the subject we will introduce in this chapter.

<!-- Slide number: 3 -->
# Objectives (1 of 4)
12.1 To get an overview of exceptions and exception handling (§12.2).
12.2 To explore the advantages of using exception handling (§12.2).
12.3 To distinguish exception types: Error (fatal) versus Exception (nonfatal) and checked versus unchecked (§12.3).
12.4 To declare exceptions in a method header (§12.4.1).
12.5 To throw exceptions in a method (§12.4.2).
12.6 To write a try-catch block to handle exceptions (§12.4.3).
12.7 To explain how an exception is propagated (§12.4.3).

<!-- Slide number: 4 -->
# Objectives (2 of 4)
12.8 To obtain information from an exception object (§12.4.4).
12.9 To develop applications with exception handling (§12.4.5).
12.10 To use the finally clause in a try-catch block (§12.5).
12.11 To use exceptions only for unexpected errors (§12.6).
12.12 To rethrow exceptions in a catch block (§12.7).
12.13 To create chained exceptions (§12.8).
12.14 To define custom exception classes (§12.9).

<!-- Slide number: 5 -->
# Objectives (3 of 4)
12.15 To discover file/directory properties, to delete and rename files/directories, and to create directories using the File class (§12.10).
12.16 To write data to a file using the PrintWriter class (§12.11.1).
12.17 To use try-with-resources to ensure that the resources are closed automatically (§12.11.2).
12.18 To read data from a file using the Scanner class (§12.11.3).
12.19 To understand how data is read using a Scanner (§12.11.4).
12.20 To develop a program that replaces text in a file (§12.11.5).
12.21 To read data from the Web (§12.12).

<!-- Slide number: 6 -->
# Objectives (4 of 4)
12.22 To develop a Web crawler (§12.13).

<!-- Slide number: 7 -->
# Exception-Handling Overview
Show runtime error
Quotient
Fix it using an if statement
QuotientWithIf
With a method
QuotientWithMethod

### Notes:
Quotient: https://liveexample.pearsoncmg.com/html/Quotient.html
QuotientWithIf: https://liveexample.pearsoncmg.com/html/QuotientWithIf.html
QuotientWithMethod: https://liveexample.pearsoncmg.com/html/QuotientWithMethod.html

<!-- Slide number: 8 -->
# Exception Advantages
QuotientWithException
Now you see the advantages of using exception handling. It enables a method to throw an exception to its caller. Without this capability, a method must handle the exception or terminate the program.

### Notes:
QuotientWithException: https://liveexample.pearsoncmg.com/html/QuotientWithException.html

<!-- Slide number: 9 -->
# Handling InputMismatchException
InputMismatchExceptionDemo
By handling InputMismatchException, your program will continuously read an input until it is correct.

### Notes:
InputMismatchExceptionDemo: https://liveexample.pearsoncmg.com/html/InputMismatchExceptionDemo.html

<!-- Slide number: 10 -->
# Exception Types

![An object shows the Exception Types. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
In this, we can see different types of boxes. Box 1 shows the Object. Box 2 shows the Throwable, and it makes an arrow to represent the forward Box 1 that is Object. Now Box 2, divided into two parts that are Exception and Error. Box 3 shows the Exception and this box is divided into four parts: Class Not Found Exception, 10 Exception, Runtime Exception, and Many more Classes. Box 4 shows the Error and is divided into three parts that are Linkage Error, Virtual Machine Error, Many more Classes. Now, box Runtime Exception also divides into five parts as follows, and they are Arithmetic Exception, Null Pointer Exception, Index Out Of Bound Exception, IIlegal Argument Exception, Many more Classes.

<!-- Slide number: 11 -->
# System Errors

![An object shows the System Errors. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
In this, we can see different types of boxes. Box 1 shows the Object. Box 2 shows the Throwable, and it makes an arrow to represent the forward Box 1 that is Object. Now Box 2, divided into two parts that are Exception and Error. Box 3 shows the Exception, and this box is divided into four parts, Class Not Found Exception, 10 Exception, Runtime Exception, and Many more Classes. Box 4 shows the Error and is divided into three parts that are Linkage Error, Virtual Machine Error, Many more Classes. Now, box Runtime Exception is also divided into five parts, Arithmetic Exception, Null Pointer Exception, Index Out Of Bound Exception, IIlegal Argument Exception, and Many more Classes.

<!-- Slide number: 12 -->
# Exceptions

![An object shows the Exceptions. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
In this, we can see different types of boxes. Box 1 shows the Object. Box 2 shows the Throwable, and it makes an arrow to represent the forward Box 1 that is Object. Now Box 2, divided into two parts that are Exception and Error. Box 3 shows the Exception, and this box is divided into four parts that are Class Not Found Exception, 10 Exception, Runtime Exception, Many more Classes. Box 4 shows the Error and is divided into three parts that are Linkage Error, Virtual Machine Error, Many more Classes. Now, box Runtime Exception is also divided into five parts, Arithmetic Exception, Null Pointer Exception, Index Out Of Bound Exception, IIlegal Argument Exception, and Many more Classes.

<!-- Slide number: 13 -->
# Runtime Exceptions

![An object shows the Runtime Exceptions. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
In this, we can see different types of boxes. Box 1 shows the Object. Box 2 shows the Throwable, and it makes an arrow to represent the forward Box 1 that is Object. Now Box 2, divided into two parts that are Exception and Error. Box 3 shows the Exception, and this box is divided into four parts: Class Not Found Exception, 10 Exception, Runtime Exception, and Many more Classes. Box 4 shows the Error and is divided into three parts that are Linkage Error, Virtual Machine Error, Many more Classes. Now, box Runtime Exception is also divided into five parts, Arithmetic Exception, Null Pointer Exception, Index Out Of Bound Exception, IIlegal Argument Exception, and Many more Classes.

<!-- Slide number: 14 -->
# Checked Exceptions verersusus Unchecked Exceptions
RuntimeException, Error and their subclasses are known as unchecked exceptions. All other exceptions are known as checked exceptions, meaning that the compiler forces the programmer to check and deal with the exceptions.

<!-- Slide number: 15 -->
# Unchecked Exceptions (1 of 2)
In most cases, unchecked exceptions reflect programming logic errors that are not recoverable. For example, a NullPointerException is thrown if you access an object through a reference variable before an object is assigned to it; an IndexOutOfBoundsException is thrown if you access an element in an array outside the bounds of the array. These are the logic errors that should be corrected in the program. Unchecked exceptions can occur anywhere in the program. To avoid cumbersome overuse of try-catch blocks, Java does not mandate you to write code to catch unchecked exceptions.

<!-- Slide number: 16 -->
# Unchecked Exceptions (2 of 2)

![An object shows the Unchecked Exceptions. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
In this, we can see different types of boxes. Box 1 shows the Object. Box 2 shows the Throwable, and it makes an arrow to represent the forward Box 1 that is Object. Now Box 2, divided into two parts that are Exception and Error. Box 3 shows the Exception, and this box is divided into four parts: Class Not Found Exception, 10 Exception, Runtime Exception, and Many more Classes. Box 4 shows the Error and is divided into three parts that are Linkage Error, Virtual Machine Error, Many more Classes. Now, box Runtime Exception is also divided into five parts, Arithemtic Exception, Null Pointer Exception, Index Out Of Bound Exception, IIlegal Argument Exception, and many more Classes.

<!-- Slide number: 17 -->
# Declaring, Throwing, and Catching Exceptions

![The computer code shows the Declaring, Throwing, and Catching Exceptions. For long description in Notes pane, press F6.](Picture8.jpg)

### Notes:
It has divided into two boxes that is Method 1 and Method 2. Box 1 shows the method one open parenthesis close parenthesis open braces, but the Method 1 box has its own box and has 6 lines. Line 1, try open braces. Line 2 invokes method two semicolons and makes an arrow representing Box 2 for method 2. Line 3, close braces. Line 4, catch open parenthesis Exception ex close parenthesis open braces, and it indicates it catch Exception. Line 5, Process Exception semicolon. Line 6, close braces. Line 7, close braces. Box 2 shows method two, and it has 5 lines. Line 1, method 1 open parenthesis close parenthesis throws Exception open braces, and it indicates, declare Exception. Line 2, if open parenthesis an error occurs close parenthesis open braces. Line 3, throw new Exception open parenthesis close parenthesis semicolon, and it indicates throw Exception. Line 4, close braces. Line 5, close braces.

<!-- Slide number: 18 -->
# Declaring Exceptions
Every method must state the types of checked exceptions it might throw. This is known as declaring exceptions.
public void myMethod()
throws IOException
public void myMethod()
throws IOException, OtherException

<!-- Slide number: 19 -->
# Throwing Exceptions
When the program detects an error, the program can create an instance of an appropriate exception type and throw it. This is known as throwing an exception. Here is an example,
throw new TheException();
TheException ex = new TheException();throw ex;

<!-- Slide number: 20 -->
# Throwing Exceptions Example
/** Set a new radius */
public void setRadius(double newRadius)
throws IllegalArgumentException {
if (newRadius >= 0)
radius = newRadius;
else
throw new IllegalArgumentException(
"Radius cannot be negative");
}

<!-- Slide number: 21 -->
# Catching Exceptions (1 of 2)
try {
statements; // Statements that may throw exceptions
}
catch (Exception1 exVar1) {
handler for exception1;
}
catch (Exception2 exVar2) {
handler for exception2;
}
...
catch (ExceptionN exVar3) {
handler for exceptionN;
}

<!-- Slide number: 22 -->
# Catching Exceptions (2 of 2)

![An example of catching exceptions. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
main method left brace
ellipsis
try left brace
ellipsis
invoke method1 semi colon
statement1 semi colon
right brace
catch (Exception1 ex1) left brace
Process ex1 semi colon
right brace
statement2 semi colon
right brace
An arrow from the invoke method1 points to method1 below.
method1 left brace
ellipsis
try left brace
ellipsis
invoke method2 semi colon
statement3 semi colon
right brace
catch (Exception2 ex2) left brace
Process ex2 semi colon
right brace
statement4 semi colon
right brace
An arrow from the invoke method2 points to method2 below.
method2 left brace
ellipsis
try left brace
ellipsis
invoke method3 semi colon
statement5 semi colon
right brace
catch (Exception3 ex3) left brace
Process ex3 semi colon
right brace
statement6 semi colon
right brace
An arrow from the invoke method3 points to method3 below.
An exception is thrown in method3.
Call Stack
main method
method1 stacked above main method
method2 stacked above stack of method1 and main method.
method3 stacked above stack of method2, method1, and main method.

<!-- Slide number: 23 -->
# Catch or Declare Checked Exceptions (1 of 2)
Suppose p2 is defined as follows:

![The computer code shows the Catch or Declare Checked Exceptions. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
The computer code is divided into two parts. Part A contains 8 lines. Line 1, void p1 open parenthesis close parenthesis open braces Line 2, try open braces Line 3, p 2 open parenthesis close parenthesis semicolon Line 4, close braces Line 5, catch open parenthesis IOException ex close parenthesis open braces Line 6, period period period Line 7, close braces Line 8, close braces Part B contains 3 lines. Line 1, void p1 open parenthesis close parenthesis throws IOException open braces Line 2, p2 open parenthesis close parenthesis semicolon Line 3, close braces

<!-- Slide number: 24 -->
# Catch or Declare Checked Exceptions (2 of 2)

![Java forces you to deal with checked exceptions. For long description in Notes pane, press F6.](Picture8.jpg)

### Notes:
If a method declares a checked exception (i.e., an exception other than Error or RuntimeException), you must invoke it in a try catch block or declare to throw the exception in the calling method. For example, suppose that method p1 invokes method p2 and p2 may throw a checked exception (e.g., IOException), you have to write the code as shown in (a) or (b).
The computer code shows the Catch or Declare Checked Exceptions. The computer code is divided into two parts. Part A contains 8 lines. Line 1, void p1 open parenthesis close parenthesis open braces Line 2, try open braces Line 3, p 2 open parenthesis close parenthesis semicolon Line 4, close braces Line 5, catch open parenthesis IOException ex close parenthesis open braces Line 6, ellipsis Line 7, close braces Line 8, close braces Part B contains 3 lines. Line 1, void p1 open parenthesis close parenthesis throws IOException open braces Line 2, p2 open parenthesis close parenthesis semicolon Line 3, close braces.
An arrow from the try catch block in the paragraph above points to first code block. An arrow from declare in the paragraph above points to the second code block.

<!-- Slide number: 25 -->
# Example: Declaring, Throwing, and Catching Exceptions
Objective: This example demonstrates declaring, throwing, and catching exceptions by modifying the setRadius method in the Circle class defined in Chapter 9. The new setRadius method throws an exception if radius is negative.
CircleWithException
TestCircleWithException

### Notes:
CircleWithException: https://liveexample.pearsoncmg.com/html/CircleWithException.html
TestCircleWithException: https://liveexample.pearsoncmg.com/html/TestCircleWithException.html

<!-- Slide number: 26 -->
# Rethrowing Exceptions
try {
statements;
}
catch(TheException ex) {
perform operations before exits;
throw ex;
}

<!-- Slide number: 27 -->
# The finally Clause
try {
statements;
}
catch(TheException ex){
handling ex;
}
finally {
finalStatements;
}

<!-- Slide number: 28 -->
# Trace a Program Execution (1 of 11)

![The computer code shows Trace a Program Execution. For long description in Notes pane, press F6.](Picture4.jpg)

### Notes:
The computer code contains 10 lines. Line 1, try open braces Line 2, statements semicolon It is Suppose no exceptions in the statements Line 3, close braces Line 4, catch open parenthesis TheException ex close parenthesis open braces Line 5, handling ex semicolon Line 6, close braces Line 7, finally open braces Line 8, final statements semicolon Line 9, close braces Line 10, Next statement semicolon

<!-- Slide number: 29 -->
# Trace a Program Execution (2 of 11)

![The computer code shows the Trace a Program Execution. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
The computer code contains 10 lines. Line 1, try open braces Line 2, statements semicolon Line 3, close braces Line 4, catch open parenthesis TheException ex close parenthesis open braces Line 5, handling ex semicolon Line 6, close braces Line 7, finally open braces Line 8, finalStatements semicolon This line shows The final block is always executed Line 9, close braces Line 10, Next statement semicolon

<!-- Slide number: 30 -->
# Trace a Program Execution (3 of 11)

![The computer code shows the Trace a Program Execution. For long description in Notes pane, press F6.](Picture4.jpg)

### Notes:
The computer code contains 10 lines. Line 1, try open braces Line 2, statements semicolon Line 3, close braces Line 4, catch open parenthesis TheException ex close parenthesis open braces Line 5, handling ex semicolon Line 6, close braces Line 7, finally, open braces Line 8, final statements semicolon Line 9, close braces Line 10, Next statement semicolon This line shows the Next statement in the method is executed

<!-- Slide number: 31 -->
# Trace a Program Execution (4 of 11)

![The computer code shows the Trace a Program Execution. For long description in Notes pane, press F6.](Picture4.jpg)

### Notes:
The computer code contains 12 lines. Line 1, try open brace Line 2, statement1 semicolon Line 3, statement2 semicolon This line shows that Suppose an exception of type Exception1 is thrown in statement2 Line 4, statement3 semicolon Line 5, close braces Line 6, catch open parenthesis Exception1 ex close parenthesis open braces Line 7, handling ex semicolon Line 8, close braces Line 9, finally open braces Line 10, final statements semicolon Line 11, close braces line 12, Next statement semicolon

<!-- Slide number: 32 -->
# Trace a Program Execution (5 of 11)

![The computer code shows the Trace a Program Execution. For long description in Notes pane, press F6.](Picture4.jpg)

### Notes:
The computer code contains 12 lines. Line 1, try open braces Line 2, statement1 semicolon Line 3, statement2 semicolon Line 4, statement3 semicolon Line 5, close braces Line 6, catch open parenthesis Exception1 ex close parenthesis open braces Line 7, handling ex semicolon This line shows that the Exception is handled period Line 8, close braces Line 9, finally open braces Line 10, final statements semicolon Line 11, close braces Line 12, Next statement semicolon

<!-- Slide number: 33 -->
# Trace a Program Execution (6 of 11)

![The computer code shows the Trace a Program Execution. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
The computer code contains 13 lines. Line 1, try open braces Line 2, statement1 semicolon Line 3, statement 2 semicolon Line 5, statement3 semicolon Line 6, close braces Line 7, catch open parenthesis Exception1 ex close parenthesis open braces Line 8, handling ex semicolon Line 9, close braces Line 10, finally open braces Line 11, final statements semicolon This line shows that The final block is always executed period Line 12, close braces Line 13, Next statement semicolon

<!-- Slide number: 34 -->
# Trace a Program Execution (7 of 11)

![The computer code shows the Trace a Program Execution. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
The computer code contains 11 lines. Line 1, try open braces Line 2, statement1 semicolon Line 3, statement2 semicolon Line 3, statement3 semicolon Line 4, close braces Line 5, catch open parenthesis Exception1 ex close parenthesis open braces Line 6, handling ex semicolon Line 7, close braces Line 8, finally, open braces Line 9, final statements semicolon Line 10, close braces Line 11, Next statement semicolon This line shows that The next statement in the method is now executed period

<!-- Slide number: 35 -->
# Trace a Program Execution (8 of 11)

![The computer code shows the Trace a Program Execution. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
The computer code contains 16 lines. Line 1, try open braces Line 2, statement1 semicolon Line 3, statement2 semicolon This line shows that statement2 throws an exception of type Exception2 period Line 4, statement3 semicolon Line 5, close braces Line 6, catch open parenthesis Exception1 ex close parenthesis open braces Line 7, handling ex semicolon Line 8, close braces Line 9, catch open parenthesis Exception2 ex close parenthesis open braces Line 10, handling ex semicolon Line 11, throw ex semicolon Line 12, close braces Line 13, finally open braces Line 14, final statements semicolon Line 15, close braces Line 16, Next statement semicolon

<!-- Slide number: 36 -->
# Trace a Program Execution (9 of 11)

![The computer code shows the Trace a Program Execution. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
The computer code contains 16 lines. Line 1, try open braces Line 2, statement1 semicolon Line 3, statement2 semicolon Line 4, statement3 semicolon Line 5, close braces Line 6, catch open parenthesis Exception1 ex close parenthesis open braces Line 7, handling ex semicolon Line 8, close braces Line 9, catch open parenthesis Exception2 ex close parenthesis open braces Line 10, handling ex semicolon This line shows the Handling exception Line 11, throw ex semicolon Line 12, close braces Line 13, finally open braces Line 14, final statements semicolon Line 15, close braces Line 16, Next statement semicolon

<!-- Slide number: 37 -->
# Trace a Program Execution (10 of 11)

![The computer code shows the Trace a Program Execution. For long description in Notes pane, press F6.](Picture4.jpg)

### Notes:
The computer code contains 16 lines. Line 1, try open braces Line 2, statement1 semicolon Line 3, statement2 semicolon Line 4, statement3 semicolon Line 5, close braces Line 6, catch open parenthesis Exception1 ex close parenthesis open braces Line 7, handling ex semicolon Line 8, close braces Line 9, catch open parenthesis Exception2 ex close parenthesis open braces Line 10, handling ex semicolon Line 11, throw ex semicolon Line 12, close braces Line 13, finally open braces Line 14, final statements semicolon This line shows Execute the final block Line 15, close braces Line 16, Next statement semicolon

<!-- Slide number: 38 -->
# Trace a Program Execution (11 of 11)

![The computer code shows the Trace a Program Execution. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
The computer code contains 16 lines. Line 1, try open braces Line 2, statement1 semicolon Line 3, statement2 semicolon Line 4, statement3 semicolon Line 5, close braces Line 6, catch open parenthesis Exception1 ex close parenthesis open braces Line 7, handling ex semicolon Line 8, close braces Line 9, catch open parenthesis Exception2 ex close parenthesis open braces Line 10, handling ex semicolon Line 11, throw ex semicolon This line shows Rethrow the Exception and control is transferred to the caller Line 12, close braces Line 13, finally open braces Line 14, final Statements semicolon Line 15, close braces Line 16, Next statement semicolon

<!-- Slide number: 39 -->
# Cautions When Using Exceptions
Exception handling separates error-handling code from normal programming tasks, thus making programs easier to read and to modify. Be aware, however, that exception handling usually requires more time and resources because it requires instantiating a new exception object, rolling back the call stack, and propagating the errors to the calling methods.

<!-- Slide number: 40 -->
# When to Throw Exceptions
An exception occurs in a method. If you want the exception to be processed by its caller, you should create an exception object and throw it. If you can handle the exception in the method where it occurs, there is no need to throw it.

<!-- Slide number: 41 -->
# When to Use Exceptions (1 of 2)
When should you use the try-catch block in the code? You should use it to deal with unexpected error conditions. Do not use it to deal with simple, expected situations. For example, the following code
try {
System.out.println(refVar.toString());
}
catch (NullPointerException ex) {
System.out.println("refVar is null");
}

<!-- Slide number: 42 -->
# When to Use Exceptions (2 of 2)
is better to be replaced by
if (refVar != null)
System.out.println(refVar.toString());
else
System.out.println("refVar is null");

<!-- Slide number: 43 -->
# Defining Custom Exception Classes
Use the exception classes in the A  P I whenever possible.
Define custom exception classes if the predefined classes are not sufficient.
Define custom exception classes by extending Exception or a subclass of Exception.

<!-- Slide number: 44 -->
# Custom Exception Class Example
In Listing 13.8, the setRadius method throws an exception if the radius is negative. Suppose you wish to pass the radius to the handler, you have to create a custom exception class.
InvalidRadiusException
CircleWithRadiusException
TestCircleWithRadiusException

### Notes:
InvalidRadiusException: https://liveexample.pearsoncmg.com/html/InvalidRadiusException.html
CircleWithRadiusException: https://liveexample.pearsoncmg.com/html/InvalidRadiusException.htmlhttps:/liveexample.pearsoncmg.com/html/InvalidRadiusException.html
TestCircleWithRadiusException: https://liveexample.pearsoncmg.com/html/TestCircleWithRadiusException.html

<!-- Slide number: 45 -->
# Assertions
An assertion is a Java statement that enables you to assert an assumption about your program. An assertion contains a Boolean expression that should be true during program execution. Assertions can be used to assure program correctness and avoid logic errors.

<!-- Slide number: 46 -->
# Declaring Assertions
An assertion is declared using the new Java keyword assert in J D K 1.4 as follows:
assert assertion; or
assert assertion : detailMessage;
where assertion is a Boolean expression and detailMessage is a primitive-type or an Object value.

<!-- Slide number: 47 -->
# Executing Assertions
When an assertion statement is executed, Java evaluates the assertion. If it is false, an AssertionError will be thrown. The AssertionError class has a no-arg constructor and seven overloaded single-argument constructors of type int, long, float, double, boolean, char, and Object.
For the first assert statement with no detail message, the no-arg constructor of AssertionError is used. For the second assert statement with a detail message, an appropriate AssertionError constructor is used to match the data type of the message. Since AssertionError is a subclass of Error, when an assertion becomes false, the program displays a message on the console and exits.

<!-- Slide number: 48 -->
# Executing Assertions Example
public class AssertionDemo {
public static void main(String[] args) {
int i; int sum = 0;
for (i = 0; i < 10; i++) {
sum += i;
}
assert i == 10;
assert sum > 10 && sum < 5 * 10 : "sum is " + sum;
}
}

<!-- Slide number: 49 -->
# Compiling Programs With Assertions
Since assert is a new Java keyword introduced in J D K 1.4, you have to compile the program using a J D K 1.4 compiler. Furthermore, you need to include the switch –source 1.4 in the compiler command as follows:
javac –source 1.4 AssertionDemo.java
Note: If you use J D K 1.5, there is no need to use the –source 1.4 option in the command.

<!-- Slide number: 50 -->
# Running Programs With Assertions
By default, the assertions are disabled at runtime. To enable it, use the switch –enableassertions, or –ea for short, as follows:
java –ea AssertionDemo
Assertions can be selectively enabled or disabled at class level or package level. The disable switch is –disableassertions or –da for short. For example, the following command enables assertions in package package1 and disables assertions in class Class1.
java –ea:package1 –da:Class1 AssertionDemo

<!-- Slide number: 51 -->
# Using Exception Handling or Assertions (1 of 4)
Assertion should not be used to replace exception handling. Exception handling deals with unusual circumstances during program execution. Assertions are to assure the correctness of the program. Exception handling addresses robustness and assertion addresses correctness. Like exception handling, assertions are not used for normal tests, but for internal consistency and validity checks. Assertions are checked at runtime and can be turned on or off at startup time.

<!-- Slide number: 52 -->
# Using Exception Handling or Assertions (2 of 4)
Do not use assertions for argument checking in public methods. Valid arguments that may be passed to a public method are considered to be part of the method’s contract. The contract must always be obeyed whether assertions are enabled or disabled. For example, the following code in the Circle class should be rewritten using exception handling.
public void setRadius(double newRadius) {
assert newRadius >= 0;
radius = newRadius;
}

<!-- Slide number: 53 -->
# Using Exception Handling or Assertions (3 of 4)
Use assertions to reaffirm assumptions. This gives you more confidence to assure correctness of the program. A common use of assertions is to replace assumptions with assertions in the code.

<!-- Slide number: 54 -->
# Using Exception Handling or Assertions (4 of 4)
Another good use of assertions is place assertions in a switch statement without a default case. For example,
switch (month) {
case 1: ... ; break;
case 2: ... ; break;
...
case 12: ... ; break;
default: assert false : "Invalid month: " + month
}

<!-- Slide number: 55 -->
# The File Class
The File class is intended to provide an abstraction that deals with most of the machine-dependent complexities of files and path names in a machine-independent fashion. The filename is a string. The File class is a wrapper class for the file name and its directory path.

<!-- Slide number: 56 -->
# Obtaining File Properties and Manipulating File

![The computer code shows the Obtaining file properties and manipulating the file. It has divided into 2 rows. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
 Row 1, java period io period File. Row 2 has 22 lines.
Line 1, plus File open parenthesis pathname colon String close parenthesis. Creates a File object for the specified path name. The path name may be a directory or a file.
Line 2, plus File open parenthesis parent colon String comma child colon String close parenthesis. Creates a File object for the child under the directory parent. The child may be a file name or a subdirectory.
Line 3, plus File open parenthesis parent colon File comma chold colon String close parenthesis. Creates a File object for the child under the directory parent. The parent is a File object. In the preceding constructor, the parent is a string.
Line 4, plus exists open parenthesis close parenthesis colon boolean. Returns true if the file or the directory represented by the File object exists.
Line 5, plus can Read open parenthesis close parenthesis colon boolean. Returns true if the file represented by the File object exists and can be read.
Line 6, plus can Write open parenthesis close parenthesis colon boolean. Returns true if the file represented by the File object exists and can be written.
Line 7, plus is Directory open parenthesis close parenthesis colon boolean. Returns true if the File object represents a directory.
Line 8, plus is File open parenthesis close parenthesis colon boolean. Returns true if the Fi1e object represents a file.
Line 9, plus is Absolute open parenthesis close parenthesis colon boolean. Returns true if the Fi1e object is created using an absolute path name.
Line 10, plus is Hidden open parenthesis close parenthesis colon boolean. Returns true if the file represented in the File object is hidden. The exact definition of hidden is system dependent. On Windows, you can mark a file hidden in the File Properties dialog box. On Unix systems, a file is hidden if its name begins with a period left parenthesis dot right parenthesis character.
Line 11, plus get Absolute Path open parenthesis close parenthesis colon String. Returns the complete absolute file or directory name represented by the File object.
Line 12, plus get Canonical Path open parenthesis close parenthesis colon String. Returns the same as getAbsolutePath left parenthesis right parenthesis except that it removes redundant names, such as start double quotation marks dot end double quotation marks and start double quotation marks dot dot end double quotation marks, from the path name, resolves symbolic links (on Unix), and converts drive letters to standard uppercase (on Windows).
Line 13, plus get Name open parenthesis close parenthesis colon String. Returns the last name of the complete directory and file name represented by the File object. For example. new File left parenthesis start double quotation marks c colon backslash backslash book backslash backslash test dot dat end double quotation marks right parenthesis.getName left parenthesis right parenthesis returns test dot dat.
Line 14, plus get Path open parenthesis close parenthesis colon String. Returns the complete directory and file name represented by the Fi1e object. For example, new Fi1e left parenthesis start double quotation marks c colon backslash backslash book backslash backslash test dot dat end double quotation marks right parenthesis.getPath left parenthesis right parenthesis returns test dot dat.
Line 15, plus get Parent open parenthesis close parenthesis colon String. Returns the complete parent directory of the current directory or the file represented by the File object. For example, new File left parenthesis start double quotation marks c colon backslash backslash book backslash backslash test dot dat end double quotation marks right parenthesis.getParent left parenthesis right parenthesis returns c colon book.
Line 16, plus last Modified open parenthesis close parenthesis colon long. Returns the time that the file was last modified. Returns the time that the file was last modified.
Line 17, plus length open parenthesis close parenthesis colon long. Returns the size of the file, or 0 if it does not exist or if it is a directory.
Line 18, plus list File open parenthesis close parenthesis colon File open braces close braces. Returns the files under the directory for a directory File object.
Line 19, plus delete open parenthesis close parenthesis colon boolean. Deletes the file or directory represented by this File object. The method returns true if the deletion succeeds.
Line 20, plus rename To open parenthesis dest colon File close parenthesis colon boolean. Renames the file or directory represented by this File object to the specified name represented in dest. The method returns true if the operation succeeds.
Line 21, plus mkdir open parenthesis close parenthesis colon boolean. Creates a directory represented in this File object. Returns true if the the directory is created successfully.
Line 22, plus mkdirs open parenthesis close parenthesis colon boolean. Same as mkdir left parenthesis right parenthesis except that it creates directory along with its parent directories if the parent directories do not exist.

<!-- Slide number: 57 -->
# Problem: Explore File Properties
Objective: Write a program that demonstrates how to create files in a platform-independent way and use the methods in the File class to obtain their properties. The following figures show a sample run of the program on Windows and on Unix.

![Box 2, shows the Command Prompt slash telnet panda. For long description in Notes pane, press F6.](Picture15.jpg)

![The computer code shows the Explore File Properties. For long description in Notes pane, press F6.](Picture4.jpg)
TestFileClass

### Notes:
It has divided into 2 boxes. Box 1, shows the Command Prompt and it has 16 lines. Line 1, C colon slash downward book greater than java Test File Class. Line 2, Does it exist question mark true. Line 3, Can it be read question mark true. Line 4, Can it be written question mark true. Line 5, Is it a directory question mark true. Line 6, Is it a file question mark true. Line 7, Is it a absolute question mark false. Line 8, Is it hidden question mark false. Line 9, What is its absolute path question mark C colon slash downward book slash downward period slash downward image slash downward us period gif. Line 10, What is its canonical path question mark C colon slash downward book slash downward period slash downward image slash downward us period gif. Line 11, What is its name question mark us period gif. Line 12, What is its path question mark period slash downward image slash downward us period gif. Line 13, When was it last modified question mark Sat May 08 14 colon 00 colon 34 EDT 1999. Line 14, What is the path seperator question mark semicolon. Line 15, What is the name seperator question mark slash downward. Line 16, C colon slash downward book greater than.

It has 18 lines. Line 1, dollar pwd. Line 2, slash forward home slash forward liang slash forward book. Line 3, dollar java Test File Class. Line 4, Does it exist question mark true. Line 5, Can it be read question mark true. Line 6, Can it be written question mark true. Line 7, Is it a directory question mark true. Line 8, Is it a file question mark true. Line 9, Is it a absolute question mark false. Line 10, Is it hidden question mark false. Line 11, What is its absolute path question mark C colon slash downward book slash downward period slash downward image slash downward us period gif. Line 12, What is its canonical path question mark C colon slash downward book slash downward period slash downward image slash downward us period gif. Line 13, What is its name question mark us period gif. Line 14, What is its path question mark period slash downward image slash downward us period gif. Line 15, When was it last modified question mark Wed Jan 23 11 colon 00 colon 14 EDT 2002. Line 16, What is the path seperator question mark semicolon. Line 17, What is the name seperator question mark slash downward. Line 18, dollar.

TestFileClass: https://liveexample.pearsoncmg.com/html/TestFileClass.html

<!-- Slide number: 58 -->
# Text I/O
A File object encapsulates the properties of a file or a path, but does not contain the methods for reading/writing data from/to a file. In order to perform I/O, you need to create objects using appropriate Java I/O classes. The objects contain the methods for reading/writing data from/to a file. This section introduces how to read/write strings and numeric values from/to a text file using the Scanner and PrintWriter classes.

<!-- Slide number: 59 -->
# Writing Data Using PrintWriter

![The computer code shows Writing Data Using PrintWriter. The computer code contains 12 lines. For long description in Notes pane, press F6.](Picture5.jpg)
WriteData

### Notes:
Line 1, java period io period PrintWriter.
Line 2, plus PrintWriter open parenthesis filename colon String close parenthesis. Creates a PrintWriter for the specified file.
Line 3, plus print open parenthesis s colon String close parenthesis colon void. Writes a string.
Line 4, plus print open parenthesis c colon char close parenthesis colon void. Writes a character.
Line 5, plus print open parenthesis cArray colon char left bracket right bracket close parenthesis colon void. Writes an array of character.
Line 6, plus print open parenthesis i colon int close parenthesis colon void. Writes an int value.
Line 7, plus print open parenthesis l colon long close parenthesis colon void. Writes a long value.
Line 8, plus print open parenthesis f colon float close parenthesis colon void. Writes a float value.
Line 9, plus print open parenthesis d colon double close parenthesis colon void. Writes a double value.
Line 10, plus print open parenthesis b colon boolean close parenthesis colon void. Writes a boolean value.
Line 11, Also contains the overloaded println methods period. A println method acts like a print method; additionally it prints a line separator. The line separator string is defined by the system. It is backslash r backslash n on Windows and backslash n on Unix.
Line 12, Also contains the overloaded printf methods period. The printf method was introduced in 4.6, Formatting Console Output and Strings.

WriteData: https://liveexample.pearsoncmg.com/html/WriteData.html

<!-- Slide number: 60 -->
# Try-With-Resources
Programmers often forget to close the file. J D K 7 provides the followings new try-with-resources syntax that automatically closes the files.
try (declare and create resources) {
Use the resource to process the file;
}
WriteDataWithAutoClose

### Notes:
WriteDataWithAutoClose: https://liveexample.pearsoncmg.com/html/WriteDataWithAutoClose.html

<!-- Slide number: 61 -->
# Reading Data Using Scanner

![The computer code shows Reading Data Using Scanner. The computer code contains 14 lines. For long description in Notes pane, press F6.](Picture6.jpg)
ReadData

### Notes:
Line 1, java period util period Scanner.
Line 2, plus Scanner open parenthesis source colon File close parenthesis. Creates a Scanner object to read data from the specified file.
Line 3, plus Scanner open parenthesis source colon String close parenthesis. Creates a Scanner object to read data from the specified string.
Line 4, plus close open parenthesis close parenthesis. Closes this scanner.
Line 5, plus hasNext open parenthesis close parenthesis colon boolean. Returns true if this scanner has another token in its input.
Line 6, plus next open parenthesis close parenthesis colon String. Returns next token as a string.
Line 7, plus nextByte open parenthesis close parenthesis colon byte. Returns next token as a byte.
Line 8, plus nextShort open parenthesis close parenthesis colon short. Returns next token as a short.
Line 9, plus nextInt open parenthesis close parenthesis colon int. Returns next token as an int. Returns next token as an int.
Line 10, plus nextLong open parenthesis close parenthesis colon long. Returns next token as a long.
Line 11, plus nextFloat open parenthesis close parenthesis colon float. Returns next token as a float.
Line 12, plus nextDouble open parenthesis close parenthesis colon double. Returns next token as a double.
Line 13, plus useDelimiter open parenthesis pattern colon String close parenthesis colon Scanner. Sets this scanner’s delimiting pattern.

ReadData: https://liveexample.pearsoncmg.com/html/ReadData.html

<!-- Slide number: 62 -->
# Problem: Replacing Text
Write a class named ReplaceText that replaces a string in a text file with a new string. The filename and strings are passed as command-line arguments as follows:
java ReplaceText sourceFile targetFile oldString newString
For example, invoking
java ReplaceText FormatString.java t.txt StringBuilder StringBuffer
replaces all the occurrences of StringBuilder by StringBuffer in FormatString.java and saves the new file in t.txt.
ReplaceText

### Notes:
ReplaceText: https://liveexample.pearsoncmg.com/html/ReplaceText.html

<!-- Slide number: 63 -->
# Reading Data From the Web (1 of 2)
Just like you can read data from a file on your computer, you can read data from a file on the Web.

![An object shows the Reading Data from the Web. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
In the Object there is one box for Client and it's divided into two boxes that is Web Browser, Application Program. Then there is an oval shape which is for Internet, and this represents the interrelationship between the box Client and box Server. Now, Box Server represents the one box which is for Web Server, one cylindrical shape which is for Local files and these are interrelated with each other.

<!-- Slide number: 64 -->
# Reading Data From the Web (2 of 2)
URL url = new URL("www.google.com/index.html");
After a URL object is created, you can use the openStream() method defined in the URL class to open an input stream and use this stream to create a Scanner object as follows:
Scanner input = new Scanner(url.openStream());
ReadFileFromURL

### Notes:
ReadFileFromURL: https://liveexample.pearsoncmg.com/html/ReadFileFromURL.html

<!-- Slide number: 65 -->
# Case Study: Web Crawler (1 of 3)
This case study develops a program that travels the Web by following hyperlinks.

![A diagram shows a program that travels the Web by following hyperlinks. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
The starting URL contains URL1, URL2, and URL3. URL1 contains URL11, URL12, and URL13 and so on. URL2 contains URL21 and URL2, and so on. URL3 contains URL31, URL32, URL33, and URL4 and so on.

<!-- Slide number: 66 -->
# Case Study: Web Crawler (2 of 3)
The program follows the U R L s to traverse the Web. To avoid that each U R L is traversed only once, the program maintains two lists of U R L s. One list stores the U R L s pending for traversing and the other stores the U R L s that have already been traversed. The algorithm for this program can be described as follows:

<!-- Slide number: 67 -->
# Case Study: Web Crawler (3 of 3)
Add the starting U R L to a list named listOfPendingURLs;
while listOfPendingURLs is not empty {
Remove a URL from listOfPendingURLs;
if this URL is not in listOfTraversedURLs {
Add it to listOfTraversedURLs;
Display this URL;
Exit the while loop when the size of S is equal to 100.
Read the page from this URL and for each URL contained in the page {
Add it to listOfPendingURLs if it is not is listOfTraversedURLs;
}
}
}
WebCrawler

### Notes:
WebCrawler: https://liveexample.pearsoncmg.com/html/WebCrawler.html

<!-- Slide number: 68 -->
# Copyright
This work is protected by United States copyright laws and is provided solely for the use of instructors in teaching their courses and assessing student learning. Dissemination or sale of any part of this work (including on the World Wide Web) will destroy the integrity of the work and is not permitted. The work and materials from it should never be made available to students except by instructors using the accompanying text in their classes. All recipients of this work are expected to abide by these restrictions and to honor the intended pedagogical purposes and the needs of other instructors who rely on these materials.

![Warning](Graphic6.jpg)

### Notes:
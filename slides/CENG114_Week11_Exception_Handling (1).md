<!-- Slide number: 1 -->

CENG114 — COMPUTER PROGRAMMING II
Week 11
Exception Handling
in Java
Keep the program flowing — even when things go wrong.

Instructor
Lect. Yusuf Evren AYKAC
Department of Computer Engineering  ·  Ankara Yildirim Beyazit University

### Notes:

<!-- Slide number: 2 -->
What we will cover today

Agenda

01

02
Introduction
Exception Hierarchy
What is an exception · why we handle it
Throwable, Exception, Error tree

03

04
Types of Exceptions
Built-in Exceptions
Checked vs Unchecked vs Error
Common runtime exceptions with examples

05

06
try / catch / finally
Multi-catch & Nested try
Syntax, behavior, internal flow
Multiple exception paths

07

08
throw & throws
Exception Propagation
Raising and declaring exceptions
How exceptions travel up the call stack

09

10
Custom Exceptions
Bonus: Text File I/O
User-defined exception classes
Reading & writing .txt files
CENG114 — Week 11
Exception Handling in Java  ·  2

### Notes:

<!-- Slide number: 3 -->
Learning Objectives

By the end of this lecture you will be able to…
Define what an exception is and why exception handling matters.
Identify the structure of Java's exception class hierarchy.

✓

✓
Distinguish between checked exceptions, unchecked exceptions, and errors.
Use try / catch / finally to handle runtime errors safely.

✓

✓
Apply multi-catch and nested try blocks where appropriate.
Use throw to raise exceptions and throws to declare them.

✓

✓
Trace exception propagation through the call stack.
Design and use custom (user-defined) exception classes.

✓

✓
Read from and write to text files using Java I/O classes.

✓
CENG114 — Week 11
Exception Handling in Java  ·  3

### Notes:

<!-- Slide number: 4 -->
PART 01

Introduction
What an exception is, and why we care.

### Notes:

<!-- Slide number: 5 -->
What is an Exception?

DICTIONARY
IN JAVA
exception
An exception is an event that disrupts the normal flow of a program.
(noun) An abnormal condition; a deviation from what is expected or normal.
It is an object that is thrown at runtime by the JVM (or by your code).

Key idea
An exception is not just an error message — it is a real Java object. When something goes wrong (dividing by zero, opening a missing file, accessing a null reference…), the JVM creates an instance of an exception class and throws it. We can catch it and decide what should happen next, instead of letting the program crash.
CENG114 — Week 11
Exception Handling in Java  ·  5

### Notes:

<!-- Slide number: 6 -->
Where do exceptions come from?

A few everyday situations in Java

Math gone wrong
Missing files
Bad indices

int x = 10 / 0;

new FileReader("data.txt");

arr[100]; // arr.length = 5
→ ArithmeticException
→ FileNotFoundException
→ ArrayIndexOutOfBoundsException

Null references
Bad parsing
Network / I/O

String s = null; s.length();

Integer.parseInt("hello");

socket.read();
→ NullPointerException
→ NumberFormatException
→ IOException
All of these are normal compile-time-valid code. The damage only shows up at runtime.
CENG114 — Week 11
Exception Handling in Java  ·  6

### Notes:

<!-- Slide number: 7 -->
The Problem: A disrupted program flow

Suppose statement 5 throws an exception — what happens to statements 6–10?

statement 1;

Without exception handling

statement 2;
If an exception is thrown at line 5 and nobody catches it, the JVM:

statement 3;

statement 4;
Prints the exception message
Prints the stack trace
Terminates the program

Statements 6 through 10 are silently skipped — even critical cleanup code (closing files, releasing locks).

statement 5;   ←  exception occurs

statement 6;   ✗ NOT executed

statement 7;   ✗ NOT executed

statement 8;   ✗ NOT executed

statement 9;   ✗ NOT executed

statement 10;   ✗ NOT executed
CENG114 — Week 11
Exception Handling in Java  ·  7

### Notes:

<!-- Slide number: 8 -->
What is Exception Handling?

Exception handling is a mechanism to respond to runtime errors — such as ClassNotFoundException, IOException, SQLException, RemoteException — so the program can continue running instead of crashing.

01
02
03
Detect
Throw
Handle
The JVM (or your code) detects that something went wrong and creates an exception object.
The exception object is thrown — execution of the current block is halted.
A matching catch block (somewhere up the call stack) handles it; normal flow resumes.
CENG114 — Week 11
Exception Handling in Java  ·  8

### Notes:

<!-- Slide number: 9 -->
Why exception handling matters

Advantages

Maintain normal flow
Separate error code
The rest of the program continues to run after a recoverable error.
Error-handling code is decoupled from the main business logic — easier to read and maintain.

Group related errors
Propagate when needed
Different error types can be caught individually or as a family using a common superclass.
Errors can be passed up the call stack to the layer that knows what to do.

Clean up reliably
Robust applications
finally and try-with-resources guarantee cleanup (closing files, sockets, connections).
Software keeps serving its users even when unexpected events occur.
CENG114 — Week 11
Exception Handling in Java  ·  9

### Notes:

<!-- Slide number: 10 -->
PART 02

Exception Hierarchy
The Throwable family tree.

### Notes:

<!-- Slide number: 11 -->
Java Exception Class Hierarchy

java.lang.Throwable is the root of all things that can be thrown.

Throwable

Exception

Error

IOException

ArithmeticException

StackOverflowError

SQLException

NullPointerException

VirtualMachineError

ClassNotFoundException

NumberFormatException

OutOfMemoryError

RuntimeException

IndexOutOfBoundsException

ArrayIndexOutOfBoundsException

StringIndexOutOfBoundsException
Solid lines = inheritance · Dashed lines (amber) = RuntimeException family / unchecked
CENG114 — Week 11
Exception Handling in Java  ·  11

### Notes:

<!-- Slide number: 12 -->
Exception vs Error

Both extend Throwable, but they mean very different things.

Exception

Error
Recoverable (in principle)
Irrecoverable (almost always)
Caused by application-level conditions.
The program can usually continue if handled properly.
Includes IOException, SQLException, RuntimeException…
You should design your program to deal with these.
Caused by serious problems in the JVM environment.
Catching them is generally not useful.
Includes OutOfMemoryError, StackOverflowError, VirtualMachineError…
Best response is usually to let the program crash.
CENG114 — Week 11
Exception Handling in Java  ·  12

### Notes:

<!-- Slide number: 13 -->
The RuntimeException family

Exceptions you will meet most often

RuntimeException and all of its descendants are unchecked exceptions. The compiler does not force you to catch or declare them.

ArithmeticException
Math operation failed (e.g., divide by zero).
NullPointerException
Calling a method on a null reference.

ArrayIndexOutOfBoundsException
Array index < 0 or ≥ length.
StringIndexOutOfBoundsException
Bad index used inside a String method.

NumberFormatException
String could not be parsed into a number.
ClassCastException
Invalid cast between incompatible types.

IllegalArgumentException
Method received an inappropriate argument.
IllegalStateException
Object is in an invalid state for the call.
CENG114 — Week 11
Exception Handling in Java  ·  13

### Notes:

<!-- Slide number: 14 -->
PART 03

Types of Exceptions
Checked, Unchecked, and Errors.

### Notes:

<!-- Slide number: 15 -->
Three categories

According to Oracle, Java exceptions fall into three groups

1

2

3
Checked Exception
Unchecked Exception
Error
Direct subclasses of Throwable (excluding RuntimeException and Error). Verified at compile time.
Subclasses of RuntimeException. Not verified at compile time — only detected at runtime.
Subclasses of Error. Represent serious problems usually outside the programmer's control.

Examples
Examples
Examples
IOException, SQLException, ClassNotFoundException
ArithmeticException, NullPointerException, ArrayIndexOutOfBoundsException
OutOfMemoryError, VirtualMachineError, AssertionError
CENG114 — Week 11
Exception Handling in Java  ·  15

### Notes:

<!-- Slide number: 16 -->
Checked vs Unchecked

A side-by-side comparison

Aspect

Checked

Unchecked

Parent class

Direct child of Exception (not RuntimeException)

Subclass of RuntimeException

Verified by compiler?

Yes — must be caught or declared

No — compiler does not force handling

When detected?

At compile time

At runtime

Typical cause

External / environmental conditions

Programming bugs

Example

IOException, SQLException

NullPointerException, ArithmeticException

Handling required?

Mandatory

Optional (but often a good idea)
CENG114 — Week 11
Exception Handling in Java  ·  16

### Notes:

<!-- Slide number: 17 -->
PART 04

Built-in Exceptions
Common runtime exceptions you should know.

### Notes:

<!-- Slide number: 18 -->
Common built-in exceptions — a quick catalog

#

Exception

When it is thrown

1

ArithmeticException

Illegal arithmetic operation, e.g. dividing an integer by zero

2

ArrayIndexOutOfBoundsException

Array index < 0 or ≥ array length

3

ClassNotFoundException

A class with the given name cannot be located

4

FileNotFoundException

Trying to read/open a file that doesn't exist

5

IOException

An input/output operation fails or is interrupted

6

InterruptedException

A thread that is sleeping or waiting is interrupted

7

NoSuchFieldException

A class doesn't contain the requested field

8

NoSuchMethodException

A class doesn't contain the requested method

9

NullPointerException

Accessing a member of a null object reference

10

NumberFormatException

A string can't be parsed into a numeric type

11

RuntimeException

Generic supertype for any runtime exception

12

StringIndexOutOfBoundsException

Index used by a String method is invalid
CENG114 — Week 11
Exception Handling in Java  ·  18

### Notes:

<!-- Slide number: 19 -->
ArithmeticException

Integer division by zero

Java

Output
Can't divide a number by 0
class ArithmeticDemo {
    public static void main(String[] args) {
        try {
            int a = 30, b = 0;
            int c = a / b;     // cannot divide by zero
            System.out.println("Result = " + c);
        } catch (ArithmeticException e) {
            System.out.println("Can't divide a number by 0");
        }
    }
}

What happened
Dividing an integer by 0 throws ArithmeticException at runtime. The catch block intercepts it and prints a friendly message instead of letting the program crash.
CENG114 — Week 11
Exception Handling in Java  ·  19

### Notes:

<!-- Slide number: 20 -->
NullPointerException

Calling a method on a null reference

Java

Output
NullPointerException..
class NullPointerDemo {
    public static void main(String[] args) {
        try {
            String a = null;   // no object
            System.out.println(a.charAt(0));
        } catch (NullPointerException e) {
            System.out.println("NullPointerException..");
        }
    }
}

What happened
The variable 'a' does not point to any String object — it is null. Calling charAt(0) on null throws NullPointerException, which we catch.
CENG114 — Week 11
Exception Handling in Java  ·  20

### Notes:

<!-- Slide number: 21 -->
ArrayIndexOutOfBoundsException

Index outside the array's range

Java

Output
Array Index is Out Of Bounds
class ArrayIndexDemo {
    public static void main(String[] args) {
        try {
            int[] a = new int[5];
            a[6] = 9;   // valid indices: 0..4
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Array Index is Out Of Bounds");
        }
    }
}

What happened
An int[5] has valid indices 0–4. Writing to index 6 violates the bounds and throws ArrayIndexOutOfBoundsException.
CENG114 — Week 11
Exception Handling in Java  ·  21

### Notes:

<!-- Slide number: 22 -->
StringIndexOutOfBoundsException

Index outside a String's range

Java

Output
StringIndexOutOfBoundsException
class StringIndexDemo {
    public static void main(String[] args) {
        try {
            String a = "This is like chipping ";  // length 22
            char c = a.charAt(24);  // out of range
            System.out.println(c);
        } catch (StringIndexOutOfBoundsException e) {
            System.out.println("StringIndexOutOfBoundsException");
        }
    }
}

What happened
String methods such as charAt(i) verify that 0 ≤ i < length(). Index 24 is invalid for a 22-character string.
CENG114 — Week 11
Exception Handling in Java  ·  22

### Notes:

<!-- Slide number: 23 -->
NumberFormatException

String could not be parsed as a number

Java

Output
Number format exception
class NumberFormatDemo {
    public static void main(String[] args) {
        try {
            int num = Integer.parseInt("akki");
            System.out.println(num);
        } catch (NumberFormatException e) {
            System.out.println("Number format exception");
        }
    }
}

What happened
Integer.parseInt expects a numeric string. Passing "akki" cannot be converted, so a NumberFormatException is thrown.
CENG114 — Week 11
Exception Handling in Java  ·  23

### Notes:

<!-- Slide number: 24 -->
FileNotFoundException

Opening a file that doesn't exist

Java

Output
File does not exist
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;

class FileNotFoundDemo {
    public static void main(String[] args) {
        try {
            File file = new File("E://file.txt");
            FileReader fr = new FileReader(file);
        } catch (FileNotFoundException e) {
            System.out.println("File does not exist");
        }
    }
}

What happened
FileNotFoundException is a checked exception — the compiler forces you to either catch it or declare it with throws.
CENG114 — Week 11
Exception Handling in Java  ·  24

### Notes:

<!-- Slide number: 25 -->
PART 05

Exception Keywords
The five tools at your disposal.

### Notes:

<!-- Slide number: 26 -->
Five keywords for handling exceptions

Encloses code that may throw an exception. Must be followed by catch and/or finally.

try

Handles a specific kind of exception. Must follow a try block.

catch

Executes important cleanup code. Runs whether or not an exception occurred.

finally

Raises (throws) an exception explicitly from inside a method.

throw

Declares in the method signature that the method may pass an exception up.

throws
CENG114 — Week 11
Exception Handling in Java  ·  26

### Notes:

<!-- Slide number: 27 -->
PART 06

try / catch / finally
Java's main exception-handling structure.

### Notes:

<!-- Slide number: 28 -->
try / catch syntax

try-catch

try-finally
try {
    // code that may throw an exception
} catch (ExceptionType ref) {
    // handle the exception
}
try {
    // code that may throw an exception
} finally {
    // always executed (e.g., cleanup)
}

try-catch-finally
try {
    // code that may throw an exception
} catch (ExceptionType ref) {
    // handle the exception
} finally {
    // always runs
}
Rule: try cannot stand alone — it must be followed by catch and/or finally.
CENG114 — Week 11
Exception Handling in Java  ·  28

### Notes:

<!-- Slide number: 29 -->
Without exception handling

What happens if we don't use try/catch?

Java

Output
Exception in thread "main"
java.lang.ArithmeticException: / by zero
public class TestTryCatch1 {
    public static void main(String[] args) {
        int data = 50 / 0;  // throws ArithmeticException
        System.out.println("rest of the code...");
    }
}

✗ Problem
The line printing "rest of the code..." is never executed. Anything after the failing statement is skipped — even if it was 100 lines of important logic.
CENG114 — Week 11
Exception Handling in Java  ·  29

### Notes:

<!-- Slide number: 30 -->
With exception handling

Wrap the risky code in try/catch and the program survives.

Java

Output
java.lang.ArithmeticException: / by zero
rest of the code...
public class TestTryCatch2 {
    public static void main(String[] args) {
        try {
            int data = 50 / 0;
        } catch (ArithmeticException e) {
            System.out.println(e);
        }
        System.out.println("rest of the code...");
    }
}

✓ Solution
The exception is intercepted, a message is printed, and the program continues — the line after the try/catch runs normally.
CENG114 — Week 11
Exception Handling in Java  ·  30

### Notes:

<!-- Slide number: 31 -->
Internal working of try/catch

What the JVM does when an exception occurs

int data = 10/0;

Exception object

an exception object is thrown

Is it handled?
NO
YES

Default JVM handler
Application handler
Prints exception description
Prints the stack trace
Terminates the program
Your catch block runs. Cleanup in finally executes. Normal program flow continues with the statements after the try/catch/finally structure.
CENG114 — Week 11
Exception Handling in Java  ·  31

### Notes:

<!-- Slide number: 32 -->
PART 07

Multi-catch & Nested try
Multiple paths, controlled scope.

### Notes:

<!-- Slide number: 33 -->
Why multi-catch?

Different exceptions usually require different responses.

Multi-catch
Use multiple catch blocks when you want to take a different action based on which exception was thrown.
try {
    int[] a = new int[5];
    a[5] = 30 / 0;
} catch (ArithmeticException e) {
    System.out.println("task1 completed");
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("task2 completed");
} catch (Exception e) {
    System.out.println("common task completed");
}
System.out.println("rest of the code...");
Each catch handles one exception type
Only one catch block runs per try
Order from MOST SPECIFIC to MOST GENERAL
Optional fallback: catch (Exception e) at the end
CENG114 — Week 11
Exception Handling in Java  ·  33

### Notes:

<!-- Slide number: 34 -->
Most specific to most general

Compile-time error if catch ordering is wrong

✓ Correct order

✗ Wrong order — compile-time error

try { ... }
catch (ArithmeticException e)             { ... }
catch (ArrayIndexOutOfBoundsException e) { ... }
catch (Exception e)                     { ... }

// specific subclasses are caught first;
// the generic Exception sits at the bottom
// as a safety net.
try { ... }
catch (Exception e)                     { ... }
catch (ArithmeticException e)             { ... }
catch (ArrayIndexOutOfBoundsException e) { ... }

// Exception is the parent — once it is
// listed first, no specific subtype can
// ever be reached. Compiler rejects this.
CENG114 — Week 11
Exception Handling in Java  ·  34

### Notes:

<!-- Slide number: 35 -->
Nested try blocks

When part of a block needs its own special handling.

Nested try
Why nest?
try {
    try {
        System.out.println("going to divide");
        int b = 39 / 0;
    } catch (ArithmeticException e) {
        System.out.println(e);
    }
    try {
        int[] a = new int[5];
        a[5] = 4;
    } catch (ArrayIndexOutOfBoundsException e) {
        System.out.println(e);
    }
} catch (Exception e) {
    System.out.println("handled");
}
Sometimes a small section of a block can fail in one specific way, while the surrounding block can fail in a different way. Wrapping the inner section in its own try lets each layer handle what it knows about, and forward anything else.
Pattern
Outer try — broad operation
Inner try — specific risky step
Each catch can handle or rethrow
CENG114 — Week 11
Exception Handling in Java  ·  35

### Notes:

<!-- Slide number: 36 -->
PART 08

The finally block
Cleanup that always runs.

### Notes:

<!-- Slide number: 37 -->
The finally block

A guarantee for cleanup

finally is a block that always executes — whether or not an exception was thrown, and whether or not it was caught.

Case 1
Case 2
Case 3
No exception
Exception not handled
Exception caught
try runs to the end. finally runs after.
try is interrupted. finally still runs. Then JVM propagates the exception.
catch handles the exception. finally runs after catch.
Note: finally does NOT run if the JVM exits abruptly (System.exit() or a fatal native error).
CENG114 — Week 11
Exception Handling in Java  ·  37

### Notes:

<!-- Slide number: 38 -->
finally — Case 1

No exception thrown

Java

Output
5
finally block is always executed
rest of the code...
class FinallyCase1 {
    public static void main(String[] args) {
        try {
            int data = 25 / 5;
            System.out.println(data);
        } catch (NullPointerException e) {
            System.out.println(e);
        } finally {
            System.out.println("finally block is always executed");
        }
        System.out.println("rest of the code...");
    }
}

What happened
No exception happens. The try block completes, the finally block runs anyway, and the rest of main continues.
CENG114 — Week 11
Exception Handling in Java  ·  38

### Notes:

<!-- Slide number: 39 -->
finally — Case 2

Exception thrown but NOT caught

Java

Output
finally block is always executed
Exception in thread "main"
java.lang.ArithmeticException: / by zero
class FinallyCase2 {
    public static void main(String[] args) {
        try {
            int data = 25 / 0;
            System.out.println(data);
        } catch (NullPointerException e) {
            // does NOT match ArithmeticException
            System.out.println(e);
        } finally {
            System.out.println("finally block is always executed");
        }
        System.out.println("rest of the code...");
    }
}

What happened
The catch handles only NullPointerException, so the ArithmeticException is unhandled. Even so, finally runs first, and only then does the JVM propagate the exception and terminate.
CENG114 — Week 11
Exception Handling in Java  ·  39

### Notes:

<!-- Slide number: 40 -->
finally — Case 3

Exception thrown AND caught

Java

Output
java.lang.ArithmeticException: / by zero
finally block is always executed
rest of the code...
class FinallyCase3 {
    public static void main(String[] args) {
        try {
            int data = 25 / 0;
            System.out.println(data);
        } catch (ArithmeticException e) {
            System.out.println(e);
        } finally {
            System.out.println("finally block is always executed");
        }
        System.out.println("rest of the code...");
    }
}

What happened
catch handles the exception, finally still runs (cleanup), and the program continues normally.
CENG114 — Week 11
Exception Handling in Java  ·  40

### Notes:

<!-- Slide number: 41 -->
PART 09

throw & throws
Raising and declaring exceptions.

### Notes:

<!-- Slide number: 42 -->
The throw keyword

Explicitly raise an exception from your code

Syntax

throw is followed by an instance of an exception class. It works for both checked and unchecked exceptions, and is most often used with custom exceptions.
throw exceptionInstance;

throw new IOException("sorry, device error");

Example: validate(int age)

Output
Exception in thread "main"
java.lang.ArithmeticException:
not valid
public class TestThrow1 {
    static void validate(int age) {
        if (age < 18)
            throw new ArithmeticException("not valid");
        else
            System.out.println("welcome to vote");
    }
    public static void main(String[] args) {
        validate(13);
        System.out.println("rest of the code...");
    }
}
CENG114 — Week 11
Exception Handling in Java  ·  42

### Notes:

<!-- Slide number: 43 -->
Exception Propagation

Unhandled exceptions travel up the call stack

m()

Rules

If an exception is not caught in the method where it occurs, it is passed back to the caller.
It keeps moving up until a matching catch is found, or until it reaches main.
If main also doesn't catch it, the JVM terminates the program.
Unchecked exceptions propagate automatically.
Checked exceptions do NOT propagate by default — they must be declared with throws.
propagates up

n()

p()

main()
Call Stack
CENG114 — Week 11
Exception Handling in Java  ·  43

### Notes:

<!-- Slide number: 44 -->
Unchecked exceptions propagate automatically

Example

Java

Output
exception handled
normal flow...
class TestExceptionPropagation1 {
    void m() { int data = 50 / 0; }
    void n() { m(); }
    void p() {
        try { n(); }
        catch (Exception e) {
            System.out.println("exception handled");
        }
    }
    public static void main(String[] args) {
        TestExceptionPropagation1 obj = new TestExceptionPropagation1();
        obj.p();
        System.out.println("normal flow...");
    }
}

What happened
ArithmeticException is thrown in m(), passes through n(), and is finally caught in p(). All without any throws declarations — that's how unchecked exceptions work.
CENG114 — Week 11
Exception Handling in Java  ·  44

### Notes:

<!-- Slide number: 45 -->
The throws keyword

Declare that a method may pass an exception to its caller

Syntax

throws does not throw an exception itself — it announces that the method may throw one. Used mainly for checked exceptions.
returnType methodName() throws ExceptionType {
    // method body
}

Advantage
Allows checked exceptions to propagate up the call stack, and informs callers that they must handle (or further declare) the exception.

Example
import java.io.IOException;
class Testthrows1 {
    void m() throws IOException { throw new IOException("device error"); }
    void n() throws IOException { m(); }
    void p() { try { n(); } catch (Exception e) { System.out.println("handled"); } }
}
CENG114 — Week 11
Exception Handling in Java  ·  45

### Notes:

<!-- Slide number: 46 -->
Calling a method that declares an exception

You must either CATCH it or DECLARE it again

Case 1 — Catch it

Case 2 — Declare it
Wrap the call in try/catch. The exception is consumed inside the calling method.
Add throws to the calling method. The exception is forwarded to the next caller.

public static void main(String[] args) {
    try {
        m.method();
    } catch (Exception e) {
        System.out.println("handled");
    }
}
public static void main(String[] args)
        throws IOException {
    m.method();
    System.out.println("normal flow...");
}
// runs fine if no exception;
// crashes at runtime if it occurs.
CENG114 — Week 11
Exception Handling in Java  ·  46

### Notes:

<!-- Slide number: 47 -->
throw vs throws

Two similar-looking keywords with very different jobs

#

throw

throws

Purpose

Explicitly raises an exception

Declares that a method may raise exceptions

Followed by

An instance (object) of an exception class

One or more exception class names

Used inside

The body of a method

The method signature

Checked exceptions

Cannot be propagated by throw alone

Can be propagated to the caller

Multiple at once?

Only one exception per throw

Can declare multiple, comma-separated

Example

throw new ArithmeticException("...");

void m() throws IOException, SQLException
CENG114 — Week 11
Exception Handling in Java  ·  47

### Notes:

<!-- Slide number: 48 -->
final  ·  finally  ·  finalize

Three similar names, three completely different things

final
finally
finalize
keyword
block
method
Applies a restriction. final class can't be inherited; final method can't be overridden; final variable can't be reassigned.
Runs important code (cleanup, resource release) whether an exception happened or not. Always paired with try/catch.
Called by the garbage collector before reclaiming an object's memory. Now considered legacy; rarely used.

final int x = 100;

try { ... } finally { ... }

public void finalize() { ... }
CENG114 — Week 11
Exception Handling in Java  ·  48

### Notes:

<!-- Slide number: 49 -->
PART 10

Custom Exceptions
Make exceptions speak your domain.

### Notes:

<!-- Slide number: 50 -->
Why create your own exceptions?

Make error reporting meaningful for your application

Domain-specific naming
Custom messages
InvalidAgeException is more descriptive than generic Exception when validating user data.
Pass your own message that fits the business rule.

Distinct catch logic
Code organization
Callers can catch your exception specifically, separately from built-in ones.
Keep validation rules and error semantics close to your domain model.
Just extend Exception (for checked) or RuntimeException (for unchecked) and add a constructor.

Pattern
class MyException extends Exception {
    MyException(String message) { super(message); }
}
CENG114 — Week 11
Exception Handling in Java  ·  50

### Notes:

<!-- Slide number: 51 -->
Custom exception — full example

InvalidAgeException for a vote-eligibility check

Java

Output
Exception occurred:
InvalidAgeException: not valid
rest of the code...
class InvalidAgeException extends Exception {
    InvalidAgeException(String s) { super(s); }
}

class TestCustomException1 {
    static void validate(int age) throws InvalidAgeException {
        if (age < 18)
            throw new InvalidAgeException("not valid");
        else System.out.println("welcome to vote");
    }
    public static void main(String[] args) {
        try { validate(13); }
        catch (Exception m) {
            System.out.println("Exception occurred: " + m);
        }
        System.out.println("rest of the code...");
    }
}

How to read it
InvalidAgeException extends Exception (so it's a checked exception).
validate uses throws to declare it.
main catches it and prints the message.
CENG114 — Week 11
Exception Handling in Java  ·  51

### Notes:

<!-- Slide number: 52 -->
PART 11

Bonus: Text File I/O
Reading from and writing to .txt files.

### Notes:

<!-- Slide number: 53 -->
Working with text files in Java

All major file operations can throw IOException — exception handling is essential here.

Reading

Writing
FileReader
Reads characters from a file.
BufferedReader
Wraps FileReader for efficient line-by-line reading.
Scanner
Convenient for tokenized input (words, numbers).
FileWriter
Writes characters to a file.
BufferedWriter
Buffered, efficient line-oriented writing.
PrintWriter
Adds println / printf-style writing.
CENG114 — Week 11
Exception Handling in Java  ·  53

### Notes:

<!-- Slide number: 54 -->
Reading from a text file

BufferedReader + try-with-resources (auto-closes the file)

ReadTextFile.java

Notes
import java.io.*;

public class ReadTextFile {
    public static void main(String[] args) {
        try (BufferedReader br = new BufferedReader(
                new FileReader("input.txt"))) {
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }
        } catch (FileNotFoundException e) {
            System.out.println("File not found: " + e.getMessage());
        } catch (IOException e) {
            System.out.println("I/O error: " + e.getMessage());
        }
    }
}
try-with-resources auto-closes the reader, even if an exception is thrown.
FileNotFoundException is a subclass of IOException — catch the more specific one first.
readLine() returns null when the file ends.
CENG114 — Week 11
Exception Handling in Java  ·  54

### Notes:

<!-- Slide number: 55 -->
Writing to a text file

BufferedWriter / PrintWriter, again with try-with-resources

WriteTextFile.java

output.txt
import java.io.*;

public class WriteTextFile {
    public static void main(String[] args) {
        String[] lines = { "Hello", "World", "CENG114" };
        try (PrintWriter pw = new PrintWriter(
                new BufferedWriter(
                    new FileWriter("output.txt")))) {
            for (String line : lines) {
                pw.println(line);
            }
        } catch (IOException e) {
            System.out.println("Could not write file: " + e.getMessage());
        }
    }
}
Hello
World
CENG114

Tips
new FileWriter(file, true) appends instead of overwriting.
PrintWriter has println, print, printf.
Always handle IOException — disk full, no permissions, etc.
CENG114 — Week 11
Exception Handling in Java  ·  55

### Notes:

<!-- Slide number: 56 -->
Read → process → write

A tiny end-to-end example: copy a file, prefixing each line with its number

NumberLines.java
import java.io.*;

public class NumberLines {
    public static void main(String[] args) {
        try (
            BufferedReader br = new BufferedReader(new FileReader("input.txt"));
            PrintWriter    pw = new PrintWriter(new FileWriter("output.txt"))
        ) {
            String line; int n = 1;
            while ((line = br.readLine()) != null) {
                pw.println(n + ": " + line);
                n++;
            }
            System.out.println("Done.");
        } catch (IOException e) {
            System.out.println("I/O error: " + e.getMessage());
        }
    }
}
CENG114 — Week 11
Exception Handling in Java  ·  56

### Notes:

<!-- Slide number: 57 -->
Summary

What to remember from this lecture

Exception is an object
Throwable is the root

1

2
Created at runtime when something goes wrong; disrupts normal flow.
Splits into Exception (recoverable) and Error (irrecoverable).

Checked vs Unchecked
try / catch / finally

3

4
Checked = compiler-verified; Unchecked = subclasses of RuntimeException.
Wrap risky code; catch handles; finally guarantees cleanup.

throw vs throws
Propagation

5

6
throw raises; throws declares — completely different roles.
Unhandled unchecked exceptions travel up the call stack automatically.

Custom exceptions
I/O = exception territory

7

8
Extend Exception or RuntimeException for domain-specific errors.
File operations almost always require try/catch and try-with-resources.
CENG114 — Week 11
Exception Handling in Java  ·  57

### Notes:

<!-- Slide number: 58 -->

Thank you!

Questions?
CENG114 — Computer Programming II  ·  Week 11  ·  Exception Handling in Java
Lect. Yusuf Evren AYKAC  ·  Department of Computer Engineering  ·  AYBU

### Notes:
<!-- Slide number: 1 -->
# Introduction to Java Programming and Data Structures
Thirteenth Edition

![Front Cover: Introduction to Java Programming and Data Structures Thirteenth Edition by Liang.](Picture3.jpg)
Chapter 18
Recursion
Copyright © 2024 Pearson Education, Inc. All Rights Reserved

![Pearson Logo](PicturePlaceholder21.jpg)

### Notes:
If this PowerPoint presentation contains mathematical equations, you may need to check that your computer has the following installed:
1) MathType Plugin
2) Math Player (free versions available)
3) NVDA Reader (free versions available)

Slides in this presentation contain hyperlinks. JAWS users should be able to get a list of links by using INSERT+F7

<!-- Slide number: 2 -->
# Motivations (1 of 2)
Suppose you want to find all the files under a directory that contains a particular word. How do you solve this problem? There are several ways to solve this problem. An intuitive solution is to use recursion by searching the files in the subdirectories recursively.

<!-- Slide number: 3 -->
# Motivations (2 of 2)
H-trees, depicted in Figure 18.1, are used in a very large-scale integration (V L S I) design as a clock distribution network for routing timing signals to all parts of a chip with equal propagation delays. How do you write a program to display H-trees? A good approach is to use recursion.

![Box 1 shows the H-tree, in the bottom there is a button for Enter an order and there is a box next to the button which has 0 written.](Picture12.jpg)

![Box 2 shows the H-trees, in the bottom there is a button for Enter an order and there is a box next to the button which has 1 written.](Picture14.jpg)

![Box 3, shows the H-trees, in the bottom there is a button for Enter an order, and there is a box next to the button which has 2 written.](Picture16.jpg)

![Box 4, shows the H-trees, in the bottom there is a button for Enter an order and there is a box next to the button which has 3 written.](Picture18.jpg)

<!-- Slide number: 4 -->
# Objectives (1 of 2)
18.1 To describe what a recursive method is and the benefits of using recursion (§18.1).
18.2 To develop recursive methods for recursive mathematical functions (§§18.2–18.3).
18.3 To explain how recursive method calls are handled in a call stack (§§18.2–18.3).
18.4 To solve problems using recursion (§18.4).
18.5 To use an overloaded helper method to derive a recursive method (§18.5).
18.6 To implement a selection sort using recursion (§18.5.1).

<!-- Slide number: 5 -->
# Objectives (2 of 2)
18.7 To implement a binary search using recursion (§18.5.2).
18.8 To get the directory size using recursion (§18.6).
18.9 To solve the Tower of Hanoi problem using recursion (§18.7).
18.10 To draw fractals using recursion (§18.8).
18.11 To discover the relationship and difference between recursion and iteration (§18.9).
18.12 To know tail-recursive methods and why they are desirable (§18.10).

<!-- Slide number: 6 -->
# Computing Factorial (1 of 11)
Mathematic notation:
Function:
ComputeFactorial

### Notes:
ComputeFactorial: https://liveexample.pearsoncmg.com/html/ComputeFactorial.html

<!-- Slide number: 7 -->
# Computing Factorial (2 of 11)

<!-- Slide number: 8 -->
# Computing Factorial (3 of 11)

<!-- Slide number: 9 -->
# Computing Factorial (4 of 11)

<!-- Slide number: 10 -->
# Computing Factorial (5 of 11)

<!-- Slide number: 11 -->
# Computing Factorial (6 of 11)

<!-- Slide number: 12 -->
# Computing Factorial (7 of 11)

<!-- Slide number: 13 -->
# Computing Factorial (8 of 11)

<!-- Slide number: 14 -->
# Computing Factorial (9 of 11)

<!-- Slide number: 15 -->
# Computing Factorial (10 of 11)

<!-- Slide number: 16 -->
# Computing Factorial (11 of 11)

<!-- Slide number: 17 -->
# Trace Recursive Factorial (1 of 11)

![The computer code shows the Trace Recursive factorial. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
It has 1 line. Line 1, factorial open parenthesis 4 close parenthesis and which indicates the Executes factorial open parenthesis 4 close parenthesis. An object shows the Stack. It's shape as like as a test tube and divided into 2 lines. Line 1, Space Required for factorial open parenthesis 4 close parenthesis. Line 2, Main method.

<!-- Slide number: 18 -->
# Trace Recursive Factorial (2 of 11)

![The computer code shows the Trace Recursive factorial. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
It has 2 lines. Line 1, factorial open parenthesis 4 close parenthesis and it's make a downward arrow to represent the Step 0 colon executes factorial open parenthesis 4 close parenthesis. Line 2, return 4 address factorial open parenthesis 3 close parenthesis and which indicates the Executes factorial open parenthesis 3 close parenthesis. An object shows the Stack. It's shape as like as a test tube and divided into 3 lines. Line 1, Space Required for factorial open parenthesis 3 close parenthesis. Line 2, Space Required for factorial open parenthesis 4 close parenthesis. Line 3, Main method.

<!-- Slide number: 19 -->
# Trace Recursive Factorial (3 of 11)

![The computer code shows the Trace Recursive factorial. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
It has 3 lines. Line 1, factorial open parenthesis 4 close parenthesis and it's make a downward arrow to represent the Step 0 colon executes factorial open parenthesis 4 close parenthesis. Line 2, return 4 address factorial open parenthesis 3 close parenthesis and make a downward arrow to represent the Step 1 colon executes factorial open parenthesis 3 close parenthesis. Line 3, return 3 address factorial open parenthesis 2 close parenthesis which indicates the Executes factorial open parenthesis 2 close parenthesis. An object shows the Stack. It's shape as like as a test tube and divided into 4 lines. Line 1, Space Required for factorial open parenthesis 2 close parenthesis. Line 2, Space Required for factorial open parenthesis 3 close parenthesis. Line 3, Space Required for factorial open parenthesis 4 close parenthesis. Line 4, Main method.

<!-- Slide number: 20 -->
# Trace Recursive Factorial (4 of 11)

![The computer code shows the Trace Recursive factorial. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
It has 4 lines. Line 1, factorial open parenthesis 4 close parenthesis and it's make a downward arrow to represent the Step 0 colon executes factorial open parenthesis 4 close parenthesis. Line 2, return 4 address factorial open parenthesis 3 close parenthesis and make a downward arrow to represent the Step 1 colon executes factorial open parenthesis 3 close parenthesis. Line 3, return 3 address factorial open parenthesis 2 close parenthesis and make a downward arrow to represent the Step 2 colon executes factorial open parenthesis 2 close parenthesis. Line 4, return 2 address factorial open parenthesis 1 close parenthesis which indicates the Executes factorial open parenthesis 1 close parenthesis. An object shows the Stack. It's shape as like as a test tube and divided into 5 lines. Line 1, Space Required for factorial open parenthesis 1 close parenthesis. Line 2, Space Required for factorial open parenthesis 2 close parenthesis. Line 3, Space Required for factorial open parenthesis 3 close parenthesis. Line 4, Space Required for factorial open parenthesis 4 close parenthesis. Line 5, Main method.

<!-- Slide number: 21 -->
# Trace Recursive Factorial (5 of 11)

![The computer code shows the Trace Recursive factorial. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
It has 5 lines. Line 1, factorial open parenthesis 4 close parenthesis and it's make a downward arrow to represent the Step 0 colon executes factorial open parenthesis 4 close parenthesis. Line 2, return 4 address factorial open parenthesis 3 close parenthesis and make a downward arrow to represent the Step 1 colon executes factorial open parenthesis 3 close parenthesis. Line 3, return 3 address factorial open parenthesis 2 close parenthesis and make a downward arrow to represent the Step 2 colon executes factorial open parenthesis 2 close parenthesis. Line 4, return 2 address factorial open parenthesis 1 close parenthesis and make a downward arrow to represent the Step 3 colon executes factorial open parenthesis 1 close parenthesis. Line 5, return 1 address factorial open parenthesis 0 close parenthesis which indicates the Executes factorial open parenthesis 0 close parenthesis. An object shows the Stack. It's shape as like as a test tube and divided into 6 lines. Line 1, Space Required for factorial open parenthesis 0 close parenthesis. Line 2, Space Required for factorial open parenthesis 1 close parenthesis. Line 3, Space Required for factorial open parenthesis 2 close parenthesis. Line 4, Space Required for factorial open parenthesis 3 close parenthesis. Line 5, Space Required for factorial open parenthesis 4 close parenthesis. Line 6, Main method.

<!-- Slide number: 22 -->
# Trace Recursive Factorial (6 of 11)

![The computer code shows the Trace Recursive factorial. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
It has 6 lines. Line 1, factorial open parenthesis 4 close parenthesis and it's make a downward arrow to represent the Step 0 colon executes factorial open parenthesis 4 close parenthesis. Line 2, return 4 address factorial open parenthesis 3 close parenthesis and make a downward arrow to represent the Step 1 colon executes factorial open parenthesis 3 close parenthesis. Line 3, return 3 address factorial open parenthesis 2 close parenthesis and make a downward arrow to represent the Step 2 colon executes factorial open parenthesis 2 close parenthesis. Line 4, return 2 address factorial open parenthesis 1 close parenthesis and make a downward arrow to represent the Step 3 colon executes factorial open parenthesis 1 close parenthesis. Line 5, return 1 address factorial open parenthesis 0 close parenthesis and make a downward arrow to represent the Step 4 colon executes factorial open parenthesis 0 close parenthesis. Line 6, return 1 which indicates the returns 1. An object shows the Stack. It's shape as like as a test tube and divided into 6 lines. Line 1, Space Required for factorial open parenthesis 0 close parenthesis. Line 2, Space Required for factorial open parenthesis 1 close parenthesis. Line 3, Space Required for factorial open parenthesis 2 close parenthesis. Line 4, Space Required for factorial open parenthesis 3 close parenthesis. Line 5, Space Required for factorial open parenthesis 4 close parenthesis. Line 6, Main method.

<!-- Slide number: 23 -->
# Trace Recursive Factorial (7 of 11)

![The computer code shows the Trace Recursive factorial. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
It has 6 lines. Line 1, factorial open parenthesis 4 close parenthesis and it's make a downward arrow to represent the Step 0 colon executes factorial open parenthesis 4 close parenthesis. Line 2, return 4 address factorial open parenthesis 3 close parenthesis and make a downward arrow to represent the Step 1 colon executes factorial open parenthesis 3 close parenthesis. Line 3, return 3 address factorial open parenthesis 2 close parenthesis and make a downward arrow to represent the Step 2 colon executes factorial open parenthesis 2 close parenthesis. Line 4, return 2 address factorial open parenthesis 1 close parenthesis and make a downward arrow to represent the Step 3 colon executes factorial open parenthesis 1 close parenthesis. Line 5, return 1 address factorial open parenthesis 0 close parenthesis and which indicates the returns factorial open parenthesis 1 close parenthesis and make a downward arrow to represent the Step 4 colon executes factorial open parenthesis 0 close parenthesis. Line 6, return 1, it's make a return arrow to represent the Step 5 colon return 1 which indicates the returns factorial open parenthesis 0 close parenthesis. An object shows the Stack. It's shape as like as a test tube and divided into 6 lines. Line 1, Space Required for factorial open parenthesis 0 close parenthesis. Line 2, Space Required for factorial open parenthesis 1 close parenthesis. Line 3, Space Required for factorial open parenthesis 2 close parenthesis. Line 4, Space Required for factorial open parenthesis 3 close parenthesis. Line 5, Space Required for factorial open parenthesis 4 close parenthesis. Line 6, Main method.

<!-- Slide number: 24 -->
# Trace Recursive Factorial (8 of 11)

![The computer code shows the Trace Recursive factorial. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
It has 6 lines. Line 1, factorial open parenthesis 4 close parenthesis and it's make a downward arrow to represent the Step 0 colon executes factorial open parenthesis 4 close parenthesis. Line 2, return 4 address factorial open parenthesis 3 close parenthesis and make a downward arrow to represent the Step 1 colon executes factorial open parenthesis 3 close parenthesis. Line 3, return 3 address factorial open parenthesis 2 close parenthesis and make a downward arrow to represent the Step 2 colon executes factorial open parenthesis 2 close parenthesis. Line 4, return 2 address factorial open parenthesis 1 close parenthesis and make a downward arrow to represent the Step 3 colon executes factorial open parenthesis 1 close parenthesis. Line 5, return 1 address factorial open parenthesis 0 close parenthesis, it's make a return arrow to represent the Step 6 colon return 1 which indicates the returns factorial open parenthesis 1 close parenthesis and make a downward arrow to represent the Step 4 colon executes factorial open parenthesis 0 close parenthesis. Line 6, return 1, it's make a return arrow which indicates the Step 5 colon return 1. An object shows the Stack. It's shape as like as a test tube and divided into 5 lines. Line 1, Space Required for factorial open parenthesis 1 close parenthesis. Line 2, Space Required for factorial open parenthesis 2 close parenthesis. Line 3, Space Required for factorial open parenthesis 3 close parenthesis. Line 4, Space Required for factorial open parenthesis 4 close parenthesis. Line 5, Main method.

<!-- Slide number: 25 -->
# Trace Recursive Factorial (9 of 11)

![The computer code shows the Trace Recursive factorial. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
It has 6 lines. Line 1, factorial open parenthesis 4 close parenthesis and it's make a downward arrow to represent the Step 0 colon executes factorial open parenthesis 4 close parenthesis. Line 2, return 4 address factorial open parenthesis 3 close parenthesis and make a downward arrow to represent the Step 1 colon executes factorial open parenthesis 3 close parenthesis. Line 3, return 3 address factorial open parenthesis 2 close parenthesis and make a downward arrow to represent the Step 2 colon executes factorial open parenthesis 2 close parenthesis. Line 4, return 2 address factorial open parenthesis 1 close parenthesis, it's make a return arrow to represent the Step 7 colon return 2 which indicates the returns factorial open parenthesis 2 close parenthesis and make a downward arrow to represent the Step 3 colon executes factorial open parenthesis 1 close parenthesis. Line 5, return 1 address factorial open parenthesis 0 close parenthesis, it's make a return arrow which indicates the Step 6 colon return 1 and make a downward arrow to represent the Step 4 colon executes factorial open parenthesis 0 close parenthesis. Line 6, return 1, it's make a return arrow which indicates the Step 5 colon return 1. An object shows the Stack. It's shape as like as a test tube and divided into 4 lines. Line 1, Space Required for factorial open parenthesis 2 close parenthesis. Line 2, Space Required for factorial open parenthesis 3 close parenthesis. Line 3, Space Required for factorial open parenthesis 4 close parenthesis. Line 4, Main method.

<!-- Slide number: 26 -->
# Trace Recursive Factorial (10 of 11)

![The computer code shows the Trace Recursive factorial. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
It has 6 lines. Line 1, factorial open parenthesis 4 close parenthesis and it's make a downward arrow to represent the Step 0 colon executes factorial open parenthesis 4 close parenthesis. Line 2, return 4 address factorial open parenthesis 3 close parenthesis and make a downward arrow to represent the Step 1 colon executes factorial open parenthesis 3 close parenthesis. Line 3, return 3 address factorial open parenthesis 2 close parenthesis, it's make a return arrow to represent the Step 8 colon return 6 which indicates the returns factorial open parenthesis 3 close parenthesis and make a downward arrow to represent the Step 2 colon executes factorial open parenthesis 2 close parenthesis. Line 4, return 2 address factorial open parenthesis 1 close parenthesis, it's make a return arrow which indicates the Step 7 colon return 2 and make a downward arrow to represent the Step 3 colon executes factorial open parenthesis 1 close parenthesis. Line 5, return 1 address factorial open parenthesis 0 close parenthesis, it's make a return arrow which indicates the Step 6 colon return 1 and make a downward arrow to represent the Step 4 colon executes factorial open parenthesis 0 close parenthesis. Line 6, return 1, it's make a return arrow which indicates the Step 5 colon return 1. An object shows the Stack. It's shape as like as a test tube and divided into 3 lines. Line 1, Space Required for factorial open parenthesis 3 close parenthesis. Line 2, Space Required for factorial open parenthesis 4 close parenthesis. Line 3, Main method.

<!-- Slide number: 27 -->
# Trace Recursive Factorial (11 of 11)

![The computer code shows the Trace Recursive factorial. It has 6 lines. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
Line 1, factorial open parenthesis 4 close parenthesis and it's make a downward arrow to represent the Step 0 colon executes factorial open parenthesis 4 close parenthesis. Line 2, return 4 address factorial open parenthesis 3 close parenthesis, it's make a return arrow to represent the Step 9 colon return 24 which indicates the returns factorial open parenthesis 4 close parenthesis and make a downward arrow to represent the Step 1 colon executes factorial open parenthesis 3 close parenthesis. Line 3, return 3 address factorial open parenthesis 2 close parenthesis, it's make a return arrow which indicates the Step 8 colon return 6 and make a downward arrow to represent the Step 2 colon executes factorial open parenthesis 2 close parenthesis. Line 4, return 2 address factorial open parenthesis 1 close parenthesis, it's make a return arrow which indicates the Step 7 colon return 2 and make a downward arrow to represent the Step 3 colon executes factorial open parenthesis 1 close parenthesis. Line 5, return 1 address factorial open parenthesis 0 close parenthesis, it's make a return arrow which indicates the Step 6 colon return 1 and make a downward arrow to represent the Step 4 colon executes factorial open parenthesis 0 close parenthesis. Line 6, return 1, it's make a return arrow which indicates the Step 5 colon return 1. An object shows the Stack. It's shape as like as a test tube and divided into 2 lines. Line 1, Space Required for factorial open parenthesis 4 close parenthesis. Line 2, Main method.

<!-- Slide number: 28 -->
# factorial left parenthesis 4 right parenthesis                  Stack Trace

![An object shows the factorial (4) Stack Trace. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
It's has 9 shapes which is like as a test tube. Shape 1, has 1 line. Line 1, Space Required for factorial open parenthesis 4 close parenthesis. Shape 2, has 2 lines. Line 1, Space Required factorial open parenthesis 3 close parenthesis. Line 2, Space Required for factorial open parenthesis 4 close parenthesis. Shape 3, has 3 lines. Line 1, Space Required for factorial open parenthesis 2 close parenthesis. Line 2, Space Required for factorial open parenthesis 3 close parenthesis Line 3, Space Required for factorial open parenthesis 4 close parenthesis. Shape 4, Line 1, Space Required for factorial open parenthesis 1 close parenthesis. Line 2, Space Required for factorial open parenthesis 2 close parenthesis. Line 3, Space Required for factorial open parenthesis 3 close parenthesis. Line 4, Space Required for factorial open parenthesis 4 close parenthesis. Shape 5, has 5 lines. Line 1, Space Required for factorial open parenthesis 0 close parenthesis. Line 2, Space Required for factorial open parenthesis 1 close parenthesis. Line 3, Space Required for factorial open parenthesis 2 close parenthesis. Line 4, Space Required for factorial open parenthesis 3 close parenthesis. Line 5, Space Required for factorial open parenthesis 4 close parenthesis. Shape 6, has 4 lines. Line 1, Space Required for factorial open parenthesis 1 close parenthesis. Line 2, Space Required for factorial open parenthesis 2 close parenthesis. Line 3, Space Required for factorial open parenthesis 3 close parenthesis. Line 4, Space Required for factorial open parenthesis 4 close parenthesis. Shape 7, has 3 lines. Line 1, Space Required for factorial open parenthesis 2 close parenthesis. Line 2, Space Required for factorial open parenthesis 3 close parenthesis. Line 3, Space Required for factorial open parenthesis 4 close parenthesis. Shape 8, has 2 lines. Line 1, Space Required for factorial open parenthesis 3 close parenthesis. Line 2, Space Required for factorial open parenthesis 4 close parenthesis. Shape 9, has 1 line. Line 1, Space Required for factorial open parenthesis 4 close parenthesis.

<!-- Slide number: 29 -->
# Other Examples

<!-- Slide number: 30 -->
# Fibonacci Numbers (1 of 2)
Fibonacci series: 0 1 1 2 3 5 8 13 21 34 55 89…
indices: 0 1 2 3 4 5 6 7 8 9 10 11
ComputeFibonacci

### Notes:
ComputeFibonacci: https://liveexample.pearsoncmg.com/html/ComputeFibonacci.html

<!-- Slide number: 31 -->
# Fibonacci Numbers (2 of 2)

![A diagram shows an arrow labeled, 0: call fib(4) leading from fib(4) to return fib(3) + fib(2). For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
An arrow labeled, 2: call fib(2) leads from return fib(3) + fib(2) to return fib(1) + fib(0). An arrow labeled, 3: call fib(1) leads from return fib(1) + fib(0) to return 1. An arrow labeled, 4: return fib(1) leads from return 1 to return fib(1) + fib(0). An arrow labeled, 5: call fib(0) leads from return fib(1) + fib(0) to return 0. An arrow labeled, 6: return fib(0) leads from return 0 to return fib(1) + fib(0). An arrow labeled, 7: return fib(2) leads from return fib(1) + fib(0) to return fib(2) + fib(1). An arrow labeled, 8: call fib(1) leads from return fib(2) + fib(1) to return 1. An arrow labeled, 9: return fib(1) leads from return 1 to return fib(2) + fib(1). An arrow labeled, 10: call fib(3) leads from return fib(2) + fib(1) to return fib(3) + fib(2). An arrow labeled, 11: call fib(2) leads from return fib(3) + fib(2) to return fib(1) + fib(0). An arrow labeled, 12: call fib(1) leads from return fib(1) + fib(0) to return 1. An arrow labeled, 13: call fib(1) leads from return 1 to return fib(1) + fib(0). An arrow labeled, 14: call fib(0) leads from return fib(1) + fib(0) to return 0. An arrow labeled, 15: call fib(0) leads from return 0 to return fib(1) + fib(0). An arrow labeled, 1: call fib(2) leads from return fib(1) + fib(0) to return fib(3) + fib(2). An arrow labeled, 17: call fib(4) leads from return fib(3) + fib(2) to fib(4).

<!-- Slide number: 32 -->
# Problem Solving Using Recursion (1 of 2)
In general, to solve a problem using recursion, you break it into subproblems. If a subproblem resembles the original problem, you can apply the same approach to solve the subproblems recursively. A subproblem is almost the same as the original problem in nature with a smaller size.

<!-- Slide number: 33 -->
# Characteristics of Recursion
All recursive methods have the following characteristics:
The method is implemented using a conditional statement that leads to different cases.
One or more base cases (the simplest case) are used to stop recursion.
Every recursive call reduces the original problem, bringing it increasingly closer to a base case until it becomes that case.

<!-- Slide number: 34 -->
# Problem Solving Using Recursion (2 of 2)
nPrintln(“Welcome”, n);
one is to print the message one time and the other is to print the message for n-1 times.
The second problem is the same as the original problem with a smaller size.
The base case for the problem is n==0. You can solve this problem using recursion as follows:
public static void nPrintln(String message, int n) {
if (n >= 1) {
System.out.println(message);
nPrintln(message, n - 1);
} // The base case is n < 1
}

<!-- Slide number: 35 -->
# Think Recursively
Many of the problems presented in the early chapters can be solved using recursion if you think recursively. For example, the palindrome problem can be solved recursively as follows:
public static boolean isPalindrome(String s) {
if (s.length() <= 1) // Base case
return true;
else if (s.charAt(0) != s.charAt(s.length() - 1)) // Base case
return false;
else
return isPalindrome(s.substring(1, s.length() - 1));
}
RecursivePalindromeUsingSubstring

### Notes:
RecursivePalindromeUsingSubstring: https://liveexample.pearsoncmg.com/html/RecursivePalindromeUsingSubstring.html

<!-- Slide number: 36 -->
# Recursive Helper Methods (1 of 2)
Sometimes you can find a solution by defining a recursive method to a problem similar to the original problem. This new method is called a recursive helper method. The original method can be solved by invoking the recursive helper method.

<!-- Slide number: 37 -->
# Recursive Helper Methods (2 of 2)
The preceding recursive isPalindrome method is not efficient, because it creates a new string for every recursive call. To avoid creating new strings, use a helper method:
public static boolean isPalindrome(String s) {
return isPalindrome(s, 0, s.length() - 1);
}
public static boolean isPalindrome(String s, int low, int high) {
if (high <= low) // Base case
return true;
else if (s.charAt(low) != s.charAt(high)) // Base case
return false;
else
return isPalindrome(s, low + 1, high - 1);
}
RecursivePalindrome

### Notes:
RecursivePalindrome: https://liveexample.pearsoncmg.com/html/RecursivePalindromeUsingSubstring.html

<!-- Slide number: 38 -->
# Recursive Selection Sort
Find the smallest number in the list and swaps it with the first number.
Ignore the first number and sort the remaining smaller list recursively.
RecursiveSelectionSort

### Notes:
RecursiveSelectionSort: https://liveexample.pearsoncmg.com/html/RecursiveSelectionSort.html

<!-- Slide number: 39 -->
# Recursive Binary Search
Case 1: If the key is less than the middle element, recursively search the key in the first half of the array.
Case 2: If the key is equal to the middle element, the search ends with a match.
Case 3: If the key is greater than the middle element, recursively search the key in the second half of the array.
RecursiveBinarySearch

### Notes:
RecursiveBinarySearch: https://liveexample.pearsoncmg.com/html/RecursiveBinarySearch.html

<!-- Slide number: 40 -->
# Recursive Implementation (1 of 2)
/** Use binary search to find the key in the list */
public static int recursiveBinarySearch(int[] list, int key) {
int low = 0;
int high = list.length - 1;
return recursiveBinarySearch(list, key, low, high);
}

<!-- Slide number: 41 -->
# Recursive Implementation (2 of 2)
/** Use binary search to find the key in the list between
list[low] list[high] */
public static int recursiveBinarySearch(int[] list, int key,
int low, int high) {
if (low > high) // The list has been exhausted without a match
return -low - 1;
int mid = (low + high) / 2;
if (key < list[mid])
return recursiveBinarySearch(list, key, low, mid - 1);
else if (key == list[mid])
return mid;
else
return recursiveBinarySearch(list, key, mid + 1, high);
}

<!-- Slide number: 42 -->
# Directory Size (1 of 2)
The preceding examples can easily be solved without using recursion. This section presents a problem that is difficult to solve without using recursion. The problem is to find the size of a directory. The size of a directory is the sum of the sizes of all files in the directory. A directory may contain subdirectories. Suppose a directory contains files , , …, , and subdirectories , , …, , as shown below.

![A diagram shows a directory with many files labeled, f1, f2, ellipsis f sub n and subdirectories labeled, d1, d2, ellipsis, d sub n.](Picture7.jpg)

<!-- Slide number: 43 -->
# Directory Size (2 of 2)
The size of the directory can be defined recursively as follows:

![A diagram shows a directory with many files labeled, f1, f2, ellipsis f sub n and subdirectories labeled, d1, d2, ellipsis, d sub n.](Picture5.jpg)
DirectorySize

### Notes:
DirectorySize: https://liveexample.pearsoncmg.com/html/DirectorySize.html

<!-- Slide number: 44 -->
# Tower of Hanoi (1 of 2)
There are n disks labeled 1, 2, 3, …, n, and three towers labeled A, B, and C.
No disk can be on top of a smaller disk at any time.
All the disks are initially placed on tower A.
Only one disk can be moved at a time, and it must be the top disk on the tower.

<!-- Slide number: 45 -->
# Tower of Hanoi Animation
https://liveexample.pearsoncmg.com/dsanimation/TowerOfHanoi.html

### Notes:
https://liveexample.pearsoncmg.com/dsanimation/TowerOfHanoi.html

<!-- Slide number: 46 -->
# Tower of Hanoi (2 of 2)

![A diagram shows the tower of Hanoi as follows. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
Original position. A stack of three disks at A with B and C blank. Disks are labeled, 1, 2, 3 from top to bottom.
Step 1. Move disk 1 from A to B.
Step 2. Move disk 2 from A to C.
Step 3. Move disk 1 from B to C.
Step 4. Move disk 3 from A to B.
Step 5. Move disk 1 from C to A.
Step 6. Move disk 2 from C to B.
Step 7. Move disk 1 from A to B.

<!-- Slide number: 47 -->
# Solution to Tower of Hanoi (1 of 2)
The Tower of Hanoi problem can be decomposed into three subproblems.

![A diagram shows solution to tower of Hanoi. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
Original position. A stack of n disks at A, B and C are blank.
Step 1. Move the first n minus 1 disks from A to C recursively.
Step 2. Move disk n, the last disk, from A to B.
Step 3. Move n minus 1 disks from C to B recursively.

<!-- Slide number: 48 -->
# Solution to Tower of Hanoi (2 of 2)
Move the first
disks from A to C with the assistance
of tower B.
Move disk n from A to B.
Move
disks from C to B with the assistance
of tower A.
TowerOfHanoi

### Notes:
TowerOfHanoi: https://liveexample.pearsoncmg.com/html/TowerOfHanoi.html

<!-- Slide number: 49 -->
# Exercise 18.3 G C D
gcd(2, 3) = 1
gcd(2, 10) = 2
gcd(25, 35) = 5
gcd(205, 301) = 5
gcd(m, n)
Approach 1: Brute-force, start from min(n, m) down to 1, to check if a number is common divisor for both m and n, if so, it is the greatest common divisor.
Approach 2: Euclid’s algorithm
Approach 3: Recursive method

<!-- Slide number: 50 -->
# Approach 2: Euclid’s Algorithm
// Get absolute value of m and n;
t1 = Math.abs(m); t2 = Math.abs(n);
// r is the remainder of t1 divided by t2;
r = t1 % t2;
while (r != 0) {
t1 = t2;
t2 = r;
r = t1 % t2;
}

// When r is 0, t2 is the greatest common
// divisor between t1 and t2
return t2;

<!-- Slide number: 51 -->
# Approach 3: Recursive Method
gcd(m, n) = n if m % n = 0;
gcd(m, n) = gcd(n, m % n); otherwise;

<!-- Slide number: 52 -->
# Fractals
A fractal is a geometrical figure just like triangles, circles, and rectangles, but fractals can be divided into parts, each of which is a reduced-size copy of the whole. There are many interesting examples of fractals. This section introduces a simple fractal, called Sierpinski triangle, named after a famous Polish mathematician.

<!-- Slide number: 53 -->
# Sierpinski Triangle
It begins with an equilateral triangle, which is considered to be the Sierpinski fractal of order (or level) 0, as shown in Figure (a).
Connect the midpoints of the sides of the triangle of order 0 to create a Sierpinski triangle of order 1, as shown in Figure (b).
Leave the center triangle intact. Connect the midpoints of the sides of the three other triangles to create a Sierpinski of order 2, as shown in Figure (c).
You can repeat the same process recursively to create a Sierpinski triangle of order 3, 4, …, and so on, as shown in Figure (d).

![A screenshot shows a window titled, Sierpinski Triangle. For long description in Notes pane, press F6.](Picture22.jpg)

![A screenshot shows a window titled, Sierpinski Triangle. For long description in Notes pane, press F6.](Picture14.jpg)

![A screenshot shows a window titled, Sierpinski Triangle. The window shows a triangle with a text box for Enter an order. The text box has 0 entered in in.](Picture12.jpg)

![A screenshot shows a window titled, Sierpinski Triangle. For long description in Notes pane, press F6.](Picture20.jpg)

### Notes:
The window shows a triangle with a text box for Enter an order. The text box has 1 entered in in. There is an inverted triangle inside this triangle formed by joining mid points of the sides of the triangle. The triangle is divided into four triangles.

The window shows a triangle with a text box for Enter an order. The text box has 2 entered in in. There is an inverted triangle inside this triangle formed by joining mid points of the sides of the triangle. The triangle is divided into four triangles. Each triangle is further divided into four traingle in similar way, except the middle triangle.

The window shows a triangle with a text box for Enter an order. The text box has 3 entered in in. There is an inverted triangle inside this triangle formed by joining mid points of the sides of the triangle. The triangle is divided into four triangles. Each triangle is further divided into four traingle in similar way, except the middle triangle. Similarly, each triangle is again divided into four triangles.

<!-- Slide number: 54 -->
# Sierpinski Triangle Solution

![A diagram shows a triangle with vertices labeled, P1, P2, and P3. For long description in Notes pane, press F6.](Picture5.jpg)
SierpinskiTriangle

### Notes:
The triangle is labeled, draw the Sierpinski triangle, displayTriangles (order, p1, p2, p3). The triangle is then divided into four triangles by drawing dashed lines from the point p12, p23, and p31. The triangle at the top is labeled, recursively draw the small Sierpinski triangle, displayTriangles (order, 1, p1, p12, p31). The bottom left triangle is labeled, recursively draw the small Sierpinski triangle, displayTriangles (order, 1, p12, p1, p23). The bottom right triangle is labeled, recursively draw the small Sierpinski triangle, displayTriangles (order, 1, p31, p23, p3).

SierpinskiTriangle: https://liveexample.pearsoncmg.com/html/SierpinskiTriangle.html

<!-- Slide number: 55 -->
# Recursion versus Iteration
Recursion is an alternative form of program control. It is essentially repetition without a loop.
Recursion bears substantial overhead. Each time the program calls a method, the system must assign space for all of the method’s local variables and parameters. This can consume considerable memory and requires extra time to manage the additional space.

<!-- Slide number: 56 -->
# Advantages of Using Recursion
Recursion is good for solving the problems that are inherently recursive.

<!-- Slide number: 57 -->
# Tail Recursion
A recursive method is said to be tail recursive if there are no pending operations to be performed on return from a recursive call.
ComputeFactorial
Non-tail recursive
ComputeFactorialTailRecursion
Tail recursive

### Notes:
ComputeFactorial: https://liveexample.pearsoncmg.com/html/ComputeFactorial.html
ComputeFactorialTailRecursion: https://liveexample.pearsoncmg.com/html/ComputeFactorialTailRecursion.html

<!-- Slide number: 58 -->
# Copyright
This work is protected by United States copyright laws and is provided solely for the use of instructors in teaching their courses and assessing student learning. Dissemination or sale of any part of this work (including on the World Wide Web) will destroy the integrity of the work and is not permitted. The work and materials from it should never be made available to students except by instructors using the accompanying text in their classes. All recipients of this work are expected to abide by these restrictions and to honor the intended pedagogical purposes and the needs of other instructors who rely on these materials.

![Warning](Graphic6.jpg)

### Notes:
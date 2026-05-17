<!-- Slide number: 1 -->
# Introduction to Java Programming and Data Structures
Thirteenth Edition

![Front Cover: Introduction to Java Programming and Data Structures Thirteenth Edition by Liang.](GoogleShape293p1.jpg)
Selections from: Chapter 7 and 23
Sorting / Searching
Copyright © 2024 Pearson Education, Inc. All Rights Reserved

![Pearson Logo](GoogleShape296p1.jpg)

### Notes:
If this PowerPoint presentation contains mathematical equations, you may need to check that your computer has the following installed:
1) MathType Plugin
2) Math Player (free versions available)
3) NVDA Reader (free versions available)

Slides in this presentation contain hyperlinks. JAWS users should be able to get a list of links by using INSERT+F7

<!-- Slide number: 2 -->
# Why Study Sorting?
Sorting is a classic subject in computer science. There are three reasons for studying sorting algorithms.
First, sorting algorithms illustrate many creative approaches to problem solving and these approaches can be applied to solve other problems.
Second, sorting algorithms are good for practicing fundamental programming techniques using selection statements, loops, methods, and arrays.
Third, sorting algorithms are excellent examples to demonstrate algorithm performance.

### Notes:

<!-- Slide number: 3 -->
# What Data to Sort?
The data to be sorted might be integers, doubles, characters, or objects. §7.8, “Sorting Arrays,” presented selection sort and insertion sort for numeric values. The selection sort algorithm was extended to sort an array of objects in §11.5.7, “Example: Sorting an Array of Objects.” The Java A P I contains several overloaded sort methods for sorting primitive type values and objects in the java.util.Arrays and java.util.Collections class. For simplicity, this section assumes:
data to be sorted are integers,
data are sorted in ascending order, and
data are stored in an array. The programs can be easily modified to sort other types of data, to sort in descending order, or to sort data in an ArrayList or a LinkedList.

### Notes:

<!-- Slide number: 4 -->
# Sorting Arrays
Sorting, like searching, is also a common task in computer programming. Many different algorithms have been developed for sorting. This section introduces a simple, intuitive sorting algorithms: selection sort.

### Notes:

<!-- Slide number: 5 -->
# Selection Sort
Selection sort finds the smallest number in the list and places it first. It then finds the smallest number remaining and places it second, and so on until the list contains only a single number.

![An illustration shows Selection sort. For long description in Notes pane, press F6.](GoogleShape323g3c793739c4b_0_418.jpg)

### Notes:
Row 1. Select 1 (the smallest) and swap it with 2 (the first) in the list. The numbers in the second column are 2, 9, 5, 4, 8, 1, 6. All the numbers are in same color. A double sided arrow points between 2 and 1. The arrow is labeled swap. Third column is blank in this row.
Row 2. The number 1 is now in the correct position and thus no longer needs to be considered. The numbers in the second column are 1, 9, 5, 4, 8, 2, 6. All the numbers except 1 are of the same color. A double sided arrow points between 9 and 2. The arrow is labeled swap. Text in third column reads, Select 2 (the smallest) and swap it with 9 (the first) in the remaining list.
Row 3. The text reads, The number 2 is now in the correct position and thus has no longer needs to be considered. The numbers in the second column are 1, 2, 5, 4, 8, 9, 6. All the numbers except 1 and 2 are of same color. A double-sided arrow labeled swap connects 5 and 4. Text in the third column reads, Select 4 (the smallest) and swap it with 5 (the first) in the remaining list.
Row 4. Text reads, The number 4 is now in the correct position and thus no longer needs to be considered. The numbers in the second column are 1, 2, 4, 5, 8, 9, 6. All the numbers except 1, 2, and 4 are of the same color. Text in the third column reads, 5 is the smallest and in the right position. No swap is necessary.
Row 5. Text reads, The number 5 is now in the correct position and thus no longer needs to be considered. The numbers in the second column are 1, 2, 4, 5, 8, 9, 6. All numbers except 1, 2, 4, and 5 are of the same color. A double sided arrow labeled swap points to 8 and 6. Text in the third column reads, Select 6 (the smallest) and swap it with 8 (the first) in the remaining list.
Row 6. Text reads, The number 6 is now in the correct position and thus no longer needs to be considered. The numbers in the second column are 1, 2, 4, 5, 6, 9, 8. All the numbers except 1, 2, 4, 5, 6 are of the same color. A double side arrow labeled swap points to 9 and 8. Text in the third column reads, Select 8 (the smallest) and swap it with 9 (the first) in the remaining list.
Row 7. Text reads, The number 8 is now in the correct position and thus no longer needs to be considered. The numbers in the second column are 3, 2, 4, 5, 6, 8, 9. All the numbers except 9 are of the same color. Text in the third column reads, Since there is only one element remaining in the list, the sort is completed.

<!-- Slide number: 6 -->
# Selection Sort Animation
https://liveexample.pearsoncmg.com/dsanimation/SelectionSortNew.html

### Notes:
https://liveexample.pearsoncmg.com/dsanimation/SelectionSortNew.html

<!-- Slide number: 7 -->
# From Idea to Solution (3 of 6)
for (int i = 0; i < list.length; i++) {
select the smallest element in list[i..listSize-1];
swap the smallest with list[i], if necessary;
// list[i] is in its correct position.
// The next iteration apply on list[i+1..listSize-1]
}
list [0] list [1] list [2] list [3] ... list [10]
list [0] list [1] list [2] list [3] ... list [10]
list [0] list [1] list [2] list [3] ... list [10]
list [0] list [1] list [2] list [3] ... list [10]
list [0] list [1] list [2] list [3] ... list [10]
...
list [0] list [1] list [2] list [3] ... list [10]

### Notes:

<!-- Slide number: 8 -->
# From Idea to Solution (4 of 6)

![An arrow from the text, select the smallest element in list open bracket I ellipses list Size-1 close bracket, points to a blank text box, under the title, Expand, and the selected text, int j = i + 1; j less than list.](GoogleShape349g3c793739c4b_0_443.jpg)

### Notes:

<!-- Slide number: 9 -->
# From Idea to Solution (5 of 6)

![An arrow from the selected text, select the smallest element in list open bracket I ellipses list Size minus 1 close bracket, to the text under the head, Expand. The text reads, double current Min = list open bracket i close bracket.](GoogleShape355g3c793739c4b_0_448.jpg)

### Notes:

<!-- Slide number: 10 -->
# From Idea to Solution (6 of 6)

![An arrow from the text, swap the smallest with list open bracket i close bracket, if necessary; points to the text under the head Expand. The text reads, if open parenthesis current Min Index exclamation mark = i close parenthesis open brace.](GoogleShape361g3c793739c4b_0_453.jpg)

### Notes:

<!-- Slide number: 11 -->
# Wrap It in a Method

![forward slash asterisk symbol asterisk symbol The method for sorting the numbers asterisk symbol forward slash. For long description in Notes pane, press F6.](GoogleShape368g3c793739c4b_0_458.jpg)

### Notes:
public static void selectionSort left parenthesis double left bracket right bracket list right parenthesis {
for left parenthesis int i = 0 semi colon i is less than list.length semi colon i++ right parenthesis left brace
forward slash forward slash Find the minimum in the list left bracket I list.length minus 1 right brace
double currentMin = list left bracket i right bracket semi colon
int currentMinIndex = i semi colon
for left parenthesis int j = i + 1 semi colon j is less list.length semi colon j++ right parenthesis right brace
if left parenthesis currentMin is greater than list left bracket j right bracket right parenthesis left brace
currentMin = list left bracket j right bracket semi colon
currentMinIndex = j semi colon
right brace
right brace
forward slash forward slash Swap list left bracket i right bracket with list left bracket currentMinIndex right bracket if necessary semi colon
if left parenthesis currentMinIndex mark of exclamation = i right parenthesis left brace
list left bracket currentMinIndex right bracket = list left bracket i right bracket semi colon
list left bracket i right bracket = currentMin semi colon
right brace
right brace
right brace
Invoke it selectionSort left parenthesis yourList right parenthesis

<!-- Slide number: 12 -->
# The Arrays.sort Method
Since sorting is frequently used in programming, Java provides several overloaded sort methods for sorting an array of int, double, char, short, long, and float in the java.util.Arrays class. For example, the following code sorts an array of numbers and an array of characters.
double[] numbers = {6.0, 4.4, 1.9, 2.9, 3.4, 3.5};
java.util.Arrays.sort(numbers);
char[] chars = {'a', 'A', '4', 'F', 'D', 'P'};
java.util.Arrays.sort(chars);
Java 8 now provides Arrays.parallelSort(list) that utilizes the multicore for fast sorting.

### Notes:

<!-- Slide number: 13 -->
# The Arrays.toString(list) Method
The Arrays.toString(list) method can be used to return a string representation for the list.

### Notes:

<!-- Slide number: 14 -->
# Insertion Sort (1 of 2)
The insertion sort algorithm sorts a list of values by repeatedly inserting an unsorted element into a sorted sublist until the whole list is sorted.
int[ ] myList = {2, 9, 5, 4, 8, 1, 6}; // Unsorted

![An example shows the steps in the insertion sort algorithm as follows. For long description in Notes pane, press F6.](GoogleShape390p6.jpg)

### Notes:
Step 1. Initially, the sorted sublist contains the first element in the list. Insert 9 into the sublist. 2 9 5 4 8 1 6. An arrow point to the number 9.
Step 2. The sorted sublist is left brace 2, 9 right brace. Insert 5 into the sublist. 2 9 5 4 8 1 6. A right arrow point from 9 to 5 and a left arrow points back from 5 to 9.
Step 3. The sorted sublist is left brace 2, 5, 9 right brace. Insert 4 into the sublist. 2 5 9 4 8 1 6. A right arrow point from 5 to 9 and another right arrow points from 9 to 4. A left arrow points back from 4 to 5.
Step 4. The sorted sublist is left brace 2, 4, 5, 9 right brace. Insert 8 into the sublist. 2 4 5 9 8 1 6. A right arrow points from 9 to 8 and a left arrow points back from 8 to 9.
Step 5. The sorted sublist is left brace 2, 4, 5, 8, 9 right brace. Insert 1 into the sublist. 2 4 5 8 9 1 6. There is a right arrow between the numbers except between 1 and 6. A left arrow points back from 1 to 2.
Step 6. The sorted sublist is left brace 1, 2, 4, 5, 8, 9 right brace. Insert 6 into the sublist. 1 2 4 5 8 9 6. There is a right arrow between 8 and 9 and between 9 and 6. A left arrow points back from 6 to 8.
Step 7. The entire list is now sorted. 1 2 4 5 6 8 9.

<!-- Slide number: 15 -->
# Insertion Sort Animation
https://liveexample.pearsoncmg.com/dsanimation/InsertionSortNeweBook.html

### Notes:
https://liveexample.pearsoncmg.com/dsanimation/InsertionSortNeweBook.html

<!-- Slide number: 16 -->
# Insertion Sort (2 of 2)
int[ ] myList = {2, 9, 5, 4, 8, 1, 6}; // Unsorted

![A rectangle with 7 segments. The number in these segments from left to right are 2, 9, 5, 4, 8, 1, 6. The first segment is shaded in gray and second segment is shaded in orange.](GoogleShape404p8.jpg)

![A rectangle with 7 segments. The number in these segments from left to right are 2, 9, 5, 4, 8, 1, 6. The first two segments are shaded in gray and third segment is shaded in orange.](GoogleShape408p8.jpg)

![A rectangle with 7 segments. The number in these segments from left to right are 2, 5, 9, 4, 8, 1, 6. The first three segments are shaded in gray and fourth segment is shaded in orange.](GoogleShape405p8.jpg)

![A rectangle with 7 segments. The number in these segments from left to right are 2, 4, 5, 9, 8, 1, 6. The first four segments are shaded in gray and fifth segment is shaded in orange.](GoogleShape409p8.jpg)

![A rectangle with 7 segments. The number in these segments from left to right are 2, 4, 5, 8, 9, 1, 6. The first five segments are shaded in gray and sixth segment is shaded in orange.](GoogleShape406p8.jpg)

![A rectangle with 7 segments. The number in these segments from left to right are 1, 2, 4, 5, 8, 9, 6. The first six segments are shaded in gray and last sement is shaded in orange.](GoogleShape410p8.jpg)

![A rectangle with 7 segments. The number in these segments from left to right are 1, 2, 4, 5, 6, 8, 9. The seven segments are shaded in gray.](GoogleShape407p8.jpg)

### Notes:

<!-- Slide number: 17 -->
# How to Insert?
The insertion sort algorithm sorts a list of values by repeatedly inserting an unsorted element into a sorted sublist until the whole list is sorted.

![An example shows the steps in the insertion sort algorithm as follows. For long description in Notes pane, press F6.](GoogleShape418p9.jpg)

### Notes:
List. 0, 2. 1, 5. 2, 9, 3, 4. 4, blank. 5, blank. 6, blank. Step 1. Save 4 to a temporary variable CurrentElement.
List. 0, 2. 1, 5. 2, blank. 3, 9. 4, blank. 5, blank. 6, blank. Step 2. Move list 2 to list 3.
List. 0, 2. 1, blank. 2, 5, 3, 9. 4, blank. 5, blank. 6, blank. Step 3. Move list 1 to list 2.
List. 0, 2. 1, 4. 2, 5 3, 9. 4, blank. 5, blank. 6, blank. Step 4. Assign CurrentElement to list 1.

<!-- Slide number: 18 -->
# From Idea to Solution (1 of 2)
for (int i = 1; i < list.length; i++) {
insert list[i] into a sorted sublist list[0..i-1] so that
list[0..i] is sorted
}
list[0]
list[0] list[1]
list[0] list[1] list[2]
list[0] list[1] list[2] list[3]
list[0] list[1] list[2] list[3] …

### Notes:

<!-- Slide number: 19 -->
# From Idea to Solution (2 of 2)

![for left parenthesis int i = 1 semi colon i less than list.length semi colon i++ right parenthesis left brace insert list left bracket i right bracket into a sorted sublist list left bracket 0. For long description in Notes pane, press F6.](GoogleShape436p11.jpg)
InsertionSort

### Notes:
i minus 1 right bracket so that
 list left bracket 0. .i right bracket is sorted
right brace
Expand
double currentElement = list left bracket i right bracket semi colon
 int k semi colon
for left parenthesis k = i minus 1 semi colon k is greater than or equal to 0 and and list left bracket k right bracket greater than symbol currentElement semi colon k minus minus right parenthesis left brace
 list left bracket k + 1 right bracket = list left bracket k right bracket semi colon
 right brace
 forward slash forward slash Insert the current element into list left bracket k + 1 right bracket
 list left bracket k + 1 right bracket = currentElement semi colon
An arrow from the code block before expand points to code block after expand.

InsertionSort: https://liveexample.pearsoncmg.com/html/InsertionSort.html

<!-- Slide number: 20 -->
# Bubble Sort

![Example of bubble sort as follows. For long description in Notes pane, press F6.](GoogleShape444p12.jpg)

![O of n squared](GoogleShape446p12.jpg)
Bubble sort time:

![Left parenthesis n minus 1 right parenthesis + left parenthesis n minus 2 right parenthesis + ellipsis + 2 + 1 = n squared over 2 minus n over 2](GoogleShape447p12.jpg)
BubbleSort

### Notes:
(a) First pass.
Row 1. 2 9 5 4 8 1 (2 and 9 highlighted).
Row 2. 2 5 9 4 8 1. (5 and 9 highlighted).
Row 3. 2 5 4 9 8 1. (4 and 9 highlighted).
Row 4. 2 5 4 8 9 1. (8 and 9 highlighted).
Row 5. 2 5 4 8 1 9. (1 and 9 highlighted).
(b) Second pass.
Row 1. 2 5 4 8 1 9. (2 and 5 highlighted).
Row 2. 2 4 5 8 1 9. (4 and 5 highlighted).
Row 3. 2 4 5 8 1 9. (5 and 8 highlighted).
Row 4. 2 4 5 1 8 9. (1 and 8 highlighted).
(c) Third pass.
Row 1. 2 4 5 1 8 9. (2 and 4 highlighted).
Row 2. 2 4 5 1 8 9. (4 and 5 highlighted).
Row 3. 2 4 1 5 8 9. (1 and 5 highlighted).
(d) Fourth pass.
Row 1. 2 4 1 5 8 9. (2 and 4 highlighted).
Row 2. 2 1 4 5 8 9. (1 and 4 highlighted).
(e) Fifth pass.
Row 1. 1 2 4 5 8 9. (1 and 2 highlighted).

BubbleSort: https://liveexample.pearsoncmg.com/html/BubbleSort.html

<!-- Slide number: 21 -->
# Bubble Sort Animation
https://liveexample.pearsoncmg.com/dsanimation/BubbleSortNeweBook.html

### Notes:
https://liveexample.pearsoncmg.com/dsanimation/BubbleSortNeweBook.html

<!-- Slide number: 22 -->
# Searching Arrays
Searching is the process of looking for a specific element in an array; for example, discovering whether a certain score is included in a list of scores. Searching is a common task in computer programming. There are many algorithms and data structures devoted to searching. In this section, two commonly used approaches are discussed, linear search and binary search.

![An array reads, For long description in Notes pane, press F6.](GoogleShape463g3c793739c4b_0_0.jpg)

### Notes:
public class Linear Search open brace
backslash double asterisk The method for finding a key in the list asterisk backslash
public static int linear Search open parenthesis int open and close brackets list, int key close parenthesis open brace
for open parenthesis int i = 0; i less than list. Length; i plus plus close parenthesis
if open parenthesis key equal to list open bracket i close bracket close parenthesis return i; return negative 1; close brace, close brace.
for open parenthesis int i = 0; i less than list. Length; i plus plus close parenthesis is highlighted
A row labeled list is divided into multiple columns. The first three cells are labeled as open bracket 0 close bracket, open bracket 1 close bracket, open bracket 2 close bracket, ellipses.
A key below the row reads, Compare key with list open bracket i close bracket for i = 0, 1, ellipses.

<!-- Slide number: 23 -->
# Linear Search
The linear search approach compares the key element, key, sequentially with each element in the array list. The method continues to do so until the key matches an element in the list or the list is exhausted without a match being found. If a match is made, the linear search returns the index of the element in the array that matches the key.
If no match is found, the search returns

![negative 1.](GoogleShape471g3c793739c4b_0_7.jpg)

### Notes:

<!-- Slide number: 24 -->
# Linear Search Animation (1 of 2)

![An illustration shows Linea search animation. There are two columns labeled, Key and List. For long description in Notes pane, press F6.](GoogleShape478g3c793739c4b_0_14.jpg)

### Notes:
Row 1. Key, 3 highlighted in red. List, 6, 4, 1, 9, 7, 3, 2, 8. 6 is highlighted in red.
Row 2. Key, 3 highlighted in red. List, 6, 4, 1, 9, 7, 3, 2, 8. 4 is highlighted in red.
Row 3. Key, 3 highlighted in red. List, 6, 4, 1, 9, 7, 3, 2, 8. 1 is highlighted in red.
Row 4. Key, 3 highlighted in red. List, 6, 4, 1, 9, 7, 3, 2, 8. 9 is highlighted in red.
Row 5. Key, 3 highlighted in red. List, 6, 4, 1, 9, 7, 3, 2, 8. 7 is highlighted in red.
Row 6. Key, 3 highlighted in green. List, 6, 4, 1, 9, 7, 3, 2, 8. 3 is highlighted in green.

<!-- Slide number: 25 -->
# Linear Search Animation (2 of 2)
https://liveexample.pearsoncmg.com/dsanimation/LinearSearcheBook.html

### Notes:
https://liveexample.pearsoncmg.com/dsanimation/LinearSearcheBook.html

<!-- Slide number: 26 -->
# From Idea to Solution (1 of 6)

![Forward slash asterisk symbol asterisk symbol The method for finding a key in the list asterisk symbol forward slash. For long description in Notes pane, press F6.](GoogleShape492g3c793739c4b_0_26.jpg)
Trace the method

![int left bracket right bracket list = left brace 1, 4, 4, 2, 5, negative 3, 6, 2 right brace semi colon. For long description in Notes pane, press F6.](GoogleShape494g3c793739c4b_0_26.jpg)

### Notes:
public static int linearSearch left parenthesis int left bracket right bracket list, int key right parenthesis left brace
for left parenthesis int t = 0 semi colon i is less than list.length semi colon i ++ right parenthesis
if left parenthesis key = = list left bracket I right bracket right parenthesis
return i semi colon;
right brace

int i = linearSearch left parenthesis list, 4 right parenthesis semi colon forward slash forward slash return 1
int j = linearSearch left parenthesis list, negative 4 right parenthesis semi colon forward slash forward slash return negative 1
int k = linearSearch left parenthesis list, negative 3 right parenthesis semi colon forward slash forward slash return 5

<!-- Slide number: 27 -->
# Binary Search (1 of 6)
For binary search to work, the elements in the array must already be ordered. Without loss of generality, assume that the array is in ascending order.
e.g., 2 4 7 10 11 45 50 59 60 66 69 70 79
The binary search first compares the key with the element in the middle of the array.

### Notes:

<!-- Slide number: 28 -->
# Binary Search (2 of 6)
Consider the following three cases:
If the key is less than the middle element, you only need to search the key in the first half of the array.
If the key is equal to the middle element, the search ends with a match.
If the key is greater than the middle element, you only need to search the key in the second half of the array.

### Notes:

<!-- Slide number: 29 -->
# Binary Search (3 of 6)

![An illustration shows Binary search, consisting of two columns labeled, Key and List. For long description in Notes pane, press F6.](GoogleShape513g3c793739c4b_0_44.jpg)

### Notes:
Row 1. Key, 8 highlighted in red. List, 1, 2, 3, 4, 6, 7, 8, 9. 4 is highlighted in red. 1 and 9 are highlighted in pink.
Row 2. Key, 8 highlighted in red. List, 1, 2, 3, 4, 6, 7, 8, 9. 7 is highlighted in red. 6 and 9 are highlighted in pink.
Row 3. Key, 8 highlighted in green. List, 1, 2, 3, 4, 6, 7, 8, 9. 8 is highlighted in green. 9 is highlighted in pink.

<!-- Slide number: 30 -->
# Binary Search Animation
https://liveexample.pearsoncmg.com/dsanimation/BinarySearcheBook.html

### Notes:
https://liveexample.pearsoncmg.com/dsanimation/BinarySearcheBook.html

<!-- Slide number: 31 -->
# Binary Search (4 of 6)

![An illustration shows Binary search. It shows key as 11. For long description in Notes pane, press F6.](GoogleShape527g3c793739c4b_0_56.jpg)

### Notes:
key less than 50. A row titled list shows the numbers as 2, 4, 7, 10, 11, 45, 50, 59, 60, 66, 69, 70, and 79. Each of these numbers are labeled in chronological order, in the range of 0 to 12. Open bracket 0 close bracket is labeled as low, open bracket 6 close bracket is labeled mid, and open bracket 12 close bracket is labeled high. 50 is written in bold.
The second row shows key greater than 7. It has a row labeled, list that shows the numbers 2, 4, 7, 10, 11, 45. These numbers are labeled in chronological order in the range of 0 to 5. open bracket 0 close bracket is labeled low, open bracket 2 close bracket is labeled mid, and open bracket 5 close bracket is labeled high.
The third row shows key equal to 11. It shows a row labeled list that has the numbers 10, 11, and 45. These numbers are labeled as 3, 4, 5 respectively. open bracket 3 close bracket is labeled low, open bracket 4 close bracket is labeled mid, and open bracket 5 close bracket is labeled high.

<!-- Slide number: 32 -->
# Binary Search (5 of 6)

![An illustration shows Binary search. It shows key as 54. For long description in Notes pane, press F6.](GoogleShape534g3c793739c4b_0_62.jpg)

### Notes:
key greater than 50. A row titled list shows the numbers as 2, 4, 7, 10, 11, 45, 50, 59, 60, 66, 69, 70, and 79. Each of these numbers are labeled in chronological order, in the range of 0 to 12. Open bracket 0 close bracket is labeled as low, open bracket 6 close bracket is labeled mid, and open bracket 12 close bracket is labeled high. 50 is written in bold.
The second row shows key less than 66. It has a row labeled list that shows the numbers 59, 60, 66, 69, 70, 79 placed in the range of 7 to 12, while the row is blank in the range of 0 to 6. Open bracket 7 close bracket is labeled low, open bracket 9 close bracket is labeled mid, and open bracket 12 close bracket is labeled high.
The third row shows key less than 59. It shows a row labeled list that has the numbers 59 and 60. These numbers are labeled as 7 and 8 respectively. Open bracket 7 close bracket is labeled low and mid, and open bracket 8 close bracket is labeled high.
Another row shows the numbers 59 and 60, labeled as 7 and 8. The row is blank against 6. Open bracket 7 close bracket is labeled low, and open bracket 6 close bracket is labeled high.

<!-- Slide number: 33 -->
# Binary Search (6 of 6)
The binarySearch method returns the index of the element in the list that matches the search key if it is contained in the list. Otherwise, it returns
-insertion point

![negative 1.](GoogleShape542g3c793739c4b_0_68.jpg)
The insertion point is the point at which the key would be inserted into the list.

### Notes:

<!-- Slide number: 34 -->
# From Idea to Solution (2 of 6)
/** Use binary search to find the key in the list */
public static int binarySearch(int[] list, int key) {
int low = 0;
int high = list.length - 1;

while (high >= low) {
int mid = (low + high) / 2;
if (key < list[mid])
high = mid - 1;
else if (key == list[mid])
return mid;
else
low = mid + 1;
}

return -1 - low;
}

### Notes:

<!-- Slide number: 35 -->
# The Arrays.binarySearch Method
Since binary search is frequently used in programming, Java provides several overloaded binarySearch methods for searching a key in an array of int, double, char, short, long, and float in the java.util.Arrays class. For example, the following code searches the keys in an array of numbers and an array of characters.

![int list open and close bracket list = open brace 2, 4, 7, 10, 11, 45, 50, 59, 60, 66, 69, 70, and 79 close brace. For long description in Notes pane, press F6.](GoogleShape557g3c793739c4b_0_81.jpg)
For the binarySearch method to work, the array must be pre-sorted in increasing order.

### Notes:
Another line shows the text, java.util.Arrays.binary Search open parenthesis list, 11 close parenthesis, close parenthesis. 11 in the int list and 11 here are connected to each other.
Another set shows char open and close brackets chars = open brace a, c, g, x, y, z close brace. All the alphabets are in single quotes. The last line of this set reads java.util.Arrays.binarySearch open parenthesis chars, t with single quotes close parenthesis close parenthesis. x in char chars is connected to t here.

<!-- Slide number: 36 -->
# END

### Notes:
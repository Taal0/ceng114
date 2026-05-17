Department of Computer Engineering
CENG114 – Computer Programming II

Spring 2025 - 2026
Lab Guide #2 – Week 3

OBJECTIVE: Recursion

Instructor : Yusuf Evren AYKAÇ
Assistants : Çağın ÖZKAYA, Hatice UYSAL, Yusuf Ekrem KEÇİLİOĞLU

Q1. Write a recursive Java method that replaces all occurrences of a given character with another character in a String.
The program should take a string, a character to find, and a character to replace from the user as input.

public static String replaceChar(String str, char search, char rep)

Parameter

Description

 `str`

The string to search through

`search`

The character to find

`rep`

The character to replace it with

>  Important:  You  are  not  allowed  to  use  built-in  methods  such  as  `String.replace()`  or  `String.replaceAll()`.  Your
solution must be purely recursive — no loops (`for`, `while`) are permitted.
---
Hint — How to Think Recursively:
1. Base case: What should the method return when the string is empty (`""`)?
2. Recursive case: Check the first character — if it matches `search`, use `rep`; otherwise, keep it as is. Then make a
recursive call for the rest of the string (from index 1 onward).
---
**Example Run 1:**
Enter a string: Replace a character with another one
Enter a character to find: a
Enter a character to replace: x
Result: Replxce x chxrxcter with xnother one

**Example Run 2:**
Enter a string: Merhaba Dunya
Enter a character to find: a
Enter a character to replace: @
Result: Merh@b@ Duny@

**Example Run 3:**
Enter a string: Hello World
Enter a character to find: z
Enter a character to replace: x
Result: Hello World

**Example Run 4:**
Enter a string:
Enter a character to find: a
Enter a character to replace: b
Result:

Q2. Write a recursive Java method that calculates the sum of the elements at even indices (0, 2, 4, 6, 8) of an integer
array.  Your  program  should  declare  an  integer  array  of  ten  elements,  where  the values are taken from the user as
input.

public static int sumEvenIndexed(int[] arr, int index)

Parameter

Description

`arr`

The integer array to process

`index`

The current index being examined

> Important: Your solution must be purely recursive — no loops (`for`, `while`) are permitted for calculating the sum.
You may use a loop only for reading input from the user.
---
Hint — How to Think Recursively:
1. Base case: What should the method return when `index` exceeds the array length?
2.  Recursive  case:  If  the  current  `index`  is  even,  add  `arr[index]`  to  the  result;  otherwise,  add  `0`.  Then  make  a
recursive call with `index + 1`.
—

**Example Run 1:**
Enter an integer number: 15
Enter an integer number: 18
Enter an integer number: 25
Enter an integer number: 3
Enter an integer number: 21
Enter an integer number: 52
Enter an integer number: 45
Enter an integer number: 5
Enter an integer number: 9
Enter an integer number: 1

**Example Run 3:**
Enter an integer number: -5
Enter an integer number: 3
Enter an integer number: 10
Enter an integer number: -2
Enter an integer number: 7
Enter an integer number: 1
Enter an integer number: -8
Enter an integer number: 4
Enter an integer number: 6
Enter an integer number: 0

Array:    [15, 18, 25, 3, 21, 52, 45, 5, 9, 1]

Array:    [-5, 3, 10, -2, 7, 1, -8, 4, 6, 0]

The sum of the even indexed elements is 115
(15 + 25 + 21 + 45 + 9 = 115)

The sum of the even indexed elements is 10
(-5 + 10 + 7 + (-8) + 6 = 10)

**Example Run 2:**
Enter an integer number: 10
Enter an integer number: 20
Enter an integer number: 30
Enter an integer number: 40
Enter an integer number: 50
Enter an integer number: 60
Enter an integer number: 70
Enter an integer number: 80
Enter an integer number: 90
Enter an integer number: 100

Array:    [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]

The sum of the even indexed elements is 250
(10 + 30 + 50 + 70 + 90 = 250)

Q3.
Part A:
Write a recursive Java method that counts how many times a specified letter appears in a given word.
Part B:
Write  a  main method that reads a sentence and a letter from the user, then finds and prints the word in which the
given letter occurs the most by using the method from Part A.

Part A: Recursively counts how many times 'letter' appears in 'word'
public static int countChar(String word, char letter)

Parameter

Description

`word`

A single word to search through

`letter`

The character to count

> Important: The `countChar` method must be purely recursive — no loops (`for`, `while`) are permitted inside it. You
**may** use loops in `main` for splitting or iterating over words.
---
Hint — How to Think Recursively for `countChar`:

1. Base case: What should the method return when the word is empty (`""`)?
2. Recursive case: Check the first character — if it matches `letter`, return `1 + countChar(rest)`. Otherwise, return `0 +
countChar(rest)`.
---
**Example Run 1:**
Enter a sentence: Universe may weigh less than thought
Enter a letter: e

Word breakdown:
  "Universe"  → 'e' appears 2 times  ← MAX
  "may"       → 'e' appears 0 times
  "weigh"     → 'e' appears 1 time
  "less"      → 'e' appears 1 time
  "than"      → 'e' appears 0 times
  "thought"   → 'e' appears 0 times

'e' occurs at most in the word "Universe"

**Example Run 2:**
Enter a sentence: Researchers believe universe contains less matter
Enter a letter: e

Word breakdown:
  "Researchers" → 'e' appears 3 times  ← MAX
  "believe"     → 'e' appears 3 times
  "universe"    → 'e' appears 2 times
  "contains"    → 'e' appears 0 times
  "less"        → 'e' appears 1 time
  "matter"      → 'e' appears 1 time

'e' occurs at most in the word "Researchers"

**Example Run 3:**
Enter a sentence: Researchers believe universe contains less matter
Enter a letter: t

Word breakdown:
  "Researchers" → 't' appears 0 times
  "believe"     → 't' appears 0 times
  "universe"    → 't' appears 0 times
  "contains"    → 't' appears 1 time
  "less"        → 't' appears 0 times
  "matter"      → 't' appears 2 times  ← MAX

't' occurs at most in the word "matter"

**Example Run 4:**
Enter a sentence: Hello World
Enter a letter: z

Word breakdown:
  "Hello" → 'z' appears 0 times
  "World" → 'z' appears 0 times

'z' was not found in any word.

**Example Run 5:**
Enter a sentence: Mississippi
Enter a letter: s

Word breakdown:
  "Mississippi" → 's' appears 4 times  ← MAX

's' occurs at most in the word "Mississippi"


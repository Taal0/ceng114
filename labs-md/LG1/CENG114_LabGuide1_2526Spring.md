Department of Computer Engineering
CENG114 – Computer Programming II

Spring 2025 - 2026
Lab Guide #1 – Week 2

OBJECTIVE: Arrays, Search and, Sort Algorithms

Instructor : Yusuf Evren AYKAÇ
Assistants : Çağın ÖZKAYA, Hatice UYSAL, Yusuf Ekrem KEÇİLİOĞLU

1.  A  small library needs to alphabetically sort their new book titles. Write a Java program that implements bubble sort to

arrange an array of book titles in alphabetical order.

Create a method bubbleSort that:

●  Takes a String array as input
●  Sorts the strings in alphabetical order
●  Uses the bubble sort algorithm
●
Is case-sensitive in comparisons

The program should:

●

Initialize an array with these 6 book titles:

○
○
○
○
○
○

"Tutunamayanlar"
"Animal Farm"
"Brave New World"
"Jane Eyre"
"Dune"
"Moby Dick"

●  Display the array before and after sorting
●  Print the number of swaps performed

Example Run:

Original Order:

Tutunamayanlar, Animal Farm, Brave New World, Jane Eyre, Dune, Moby Dick

Final Order:

Animal Farm, Brave New World, Dune, Jane Eyre, Moby Dick, Tutunamayanlar

Number of swaps performed: 8

2. Develop a Java program that manages a list of renowned painters. The program should provide the following functionalities:

★

Initialization: Create a String array named painters and initialize it with the following values: "Da Vinci", "Monet", "Van
Gogh", "Rembrandt", "Picasso", "Raphael", and "Michelangelo". Ensure the array has a capacity of 7.

★  Display: Implement a method display(String[] painters) that receives a String array of painter names and displays

them on the console. Utilize a for-each loop to iterate through the array and print each name.

★  Binary Search: Implement a method binarySearch(String[] painters, String name) that performs a binary
search on the provided painters array to locate the index of a specified name. The method should employ a while loop and
adhere to the binary search algorithm. If the name is found, return its index; otherwise, return -1.

★  Add Name: Implement a method addNameToArray(String[] painters, String name) that adds a new name to
the painters array. Create a new array with an increased size, copy the existing elements from the original array, and append
the new name. Return the updated array.

★  Remove Name: Implement a method removeNameFromArray(String[] painters, String name) that removes a
specified name from the painters array. Construct a new array with a reduced size, copy the elements from the original array

(excluding the name to be removed), and return the modified array.

★  Bubble Sort: Implement a method bubbleSort(String[] painters) that sorts the painters array in ascending

alphabetical order using the bubble sort algorithm. Utilize nested for loops to compare and swap adjacent elements until the
array is sorted. Return the sorted array.

★  User Interaction: Implement a user interface that allows the user to interact with the program. The program should:

○  Display the current contents of the painters array.
○
○

Prompt the user to choose an action: "add", "delete", or "*" to exit.
If the user chooses "delete":

Prompt for a painter's name.

■
■  Use the binarySearch method to check if the name exists.
■
■

If the name is not found, display a warning message.
If the name is found, remove it using the removeNameFromArray method and display the updated array.

○

Continue prompting the user for actions until they enter the sentinel value "*".

Constraints:

●  Do not utilize any methods from the Arrays class for binary search or sorting.
●  Do not employ the System.arraycopy() method.
●  Maintain case-sensitivity when comparing names.
●  Assume valid user input and that the painters array will not become full.

EXAMPLE RUN:

Current array content:
Da Vinci, Monet, Van Gogh, Rembrandt, Picasso, Raphael, Michelangelo,
Enter "add" to add painter, "delete" to delete an existing painter, or "*" to EXIT: add

Enter painter's name or enter
(Populated Array: Da Vinci, Monet, Van Gogh, Rembrandt, Picasso, Raphael, Michelangelo) Hale Asaf
Current array content:
Da Vinci, Hale Asaf, Michelangelo, Monet, Picasso, Raphael, Rembrandt, Van Gogh, Enter "add" to add painter,
"delete" to delete an existing painter, or "*" to EXIT: delete
Enter painter's name:
Monet

Current array content:
Da Vinci, Hale Asaf, Michelangelo, Picasso, Raphael, Rembrandt, Van Gogh,
Enter "add" to add painter, "delete" to delete an existing painter, or "*" to EXIT: add

Enter painter's name or enter
(Populated Array: Da Vinci, Hale Asaf, Michelangelo, Picasso, Raphael, Rembrandt, Van Gogh) Abidin Dino

Current array content:
Abidin Dino, Da Vinci, Hale Asaf, Michelangelo, Picasso, Raphael, Rembrandt, Van Gogh, Enter "add" to add
painter, "delete" to delete an existing painter, or "*" to EXIT: delete
Enter painter's name:
Mihri Musfik
There is no such painter in the list.
Enter "add" to add painter, "delete" to delete an existing painter, or "*" to EXIT: add

Enter painter's name or enter
(Populated Array: Abidin Dino, Da Vinci, Hale Asaf, Michelangelo, Picasso, Raphael, Rembrandt, Van Gogh) Abidin
Dino
Painter is already in the list.

Current array content:
Abidin Dino, Da Vinci, Hale Asaf, Michelangelo, Picasso, Raphael, Rembrandt, Van Gogh, Enter "add" to add
painter, "delete" to delete an existing painter, or "*" to EXIT:*


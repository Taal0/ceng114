CENG114 – Computer Programming II

Spring 2025 – 2026

Lab Guide #3 – Week 5

OBJECTIVE: Objects and Classes, Encapsulation, Array of
Objects, Sorting (Selection Sort), Searching, Recursion, String
Manipulation

Instructor: Yusuf Evren AYKAÇ
Assistants: Çağın ÖZKAYA,
Hatice UYSAL, Yusuf Ekrem KEÇİLİOĞLU

Problem Statement

Write a Java program that manages the patient records of a veterinary clinic. The system should
allow the receptionist to register new patients, search for existing patients, sort records, remove
discharged patients, and calculate daily feeding costs. Create Pet and VetClinic classes as shown
in the following Class Diagram.

VetClinic

+ MAX_CAPACITY: final int = 6
+ DAILY_FEED_COST_PER_KG: final double = 2.5
- patients: Pet[]
- patientCount: int

0..*
◄───◆
has-a

+ VetClinic()
+ addPet(Pet): boolean
+ removePet(int): boolean
+ findPetByOwner(String): Pet
+ sortByAgeSelectionSort(): void
+ calculateTotalFeedCost(int): double
+ getHeaviestPet(): Pet
+ display(): void

Pet

- petID: int
- name: String
- species: String
- ownerName: String
- age: int
- weight: double
- lastAssignedID: int = 1000

+ Pet(String, String, String, int,
double)
+ getPetID(): int
+ getName(): String
+ getSpecies(): String
+ getOwnerName(): String
+ getAge(): int
+ getWeight(): double
+ toString(): String

Class Specifications

Class Pet

Data Members

•
•
•
•
•
•
•

int petID – unique identifier for each pet (automatically assigned starting from 1001).
String name – name of the pet (e.g., "Boncuk", "Pamuk").
String species – species of the pet (e.g., "Dog", "Cat", "Rabbit").
String ownerName – name of the owner.
int age – age of the pet in months.
double weight – weight of the pet in kilograms.
static int lastAssignedID = 1000 – tracks the most recently assigned pet ID.

Constructor

•

Pet(String name, String species, String ownerName, int age, double weight)

Initializes all data members and automatically increments lastAssignedID to assign a unique petID.

Methods

•  Getter methods for all fields (getPetID, getName, getSpecies, getOwnerName, getAge,

getWeight).
String toString() – returns a formatted string with all pet details.

•

Class VetClinic

Constants

•
•

static final int MAX_CAPACITY = 6 – maximum number of pets the clinic can hold.
static final double DAILY_FEED_COST_PER_KG = 2.5 – daily feeding cost per kilogram of
the pet (in TL).

Data Members

•
•

Pet[] patients – array of Pet objects (size MAX_CAPACITY).
int patientCount – current number of registered patients.

Methods

•

boolean addPet(Pet pet)

Adds a new pet to the patients array if the clinic is not full. Returns true if successful, false
otherwise.

•

boolean removePet(int petID)

Removes the pet with the given ID. If found, replaces it with the last pet in the array, decrements
the counter, and returns true. Returns false if not found.

•

Pet findPetByOwner(String ownerName)

Performs a linear search and returns the first pet belonging to the given owner. Returns null if not
found.

•

void sortByAgeSelectionSort()

Sorts the patients array in ascending order of age using the Selection Sort algorithm. Only the first
patientCount elements should be considered.

•

double calculateTotalFeedCost(int index)

Recursively calculates the total daily feeding cost for all registered patients starting from the given
index. The feeding cost for a single pet is: weight * DAILY_FEED_COST_PER_KG. The base case
is when the index reaches patientCount.
Hint: totalCost(index) = patients[index].getWeight() * DAILY_FEED_COST_PER_KG +
totalCost(index + 1)

•

Pet getHeaviestPet()

Iterates through the patients array and returns the pet with the highest weight.

•

void display()

Prints the details of all registered patients to the console using their toString() method.

Class VetManager (Main Class)

Main Method Requirements

1.  Prompt the user to input data for 6 pets (name, species, owner, age in months, and weight

in kg).

2.  The weight must be entered in the format kg:gr (e.g., 12:500 means 12.5 kg). Use

String.split() to parse and convert to a double.

3.  Store all pets in a VetClinic object and display all patients.

4.  Sort the patients by age (ascending) and display the sorted list.

5.  Ask the user which pet to discharge (by ID) and update the list.

6.  Ask the user to search for a pet by owner name and display the result.

7.  Display the heaviest pet in the clinic.

8.  Calculate and display the total daily feeding cost using the recursive method (starting from

index 0).

  EXAMPLE RUN 1:

=== Veterinary Clinic Registration ===

Pet no. 1:
  Name: Boncuk
  Species: Dog
  Owner: Ahmet Yilmaz
  Age (months): 24
  Weight (kg:gr): 12:500
  >> Boncuk (Dog) has been registered. [ID: 1001]

Pet no. 2:
  Name: Pamuk
  Species: Cat
  Owner: Elif Demir
  Age (months): 8
  Weight (kg:gr): 4:200
  >> Pamuk (Cat) has been registered. [ID: 1002]

Pet no. 3:
  Name: Karamel
  Species: Dog
  Owner: Mehmet Kaya
  Age (months): 36
  Weight (kg:gr): 25:0
  >> Karamel (Dog) has been registered. [ID: 1003]

Pet no. 4:
  Name: Minnak
  Species: Rabbit
  Owner: Zeynep Arslan
  Age (months): 6
  Weight (kg:gr): 1:800
  >> Minnak (Rabbit) has been registered. [ID: 1004]

Pet no. 5:
  Name: Fistik
  Species: Cat

  Owner: Cagin Ozkaya
  Age (months): 14
  Weight (kg:gr): 5:100
  >> Fistik (Cat) has been registered. [ID: 1005]

Pet no. 6:
  Name: Simba
  Species: Dog
  Owner: Hatice Uysal
  Age (months): 48
  Weight (kg:gr): 30:750
  >> Simba (Dog) has been registered. [ID: 1006]

============================================
     Current Patients in the Clinic
============================================
ID: 1001 | Name: Boncuk | Species: Dog
  Owner: Ahmet Yilmaz | Age: 24 months | Weight: 12.5 kg

ID: 1002 | Name: Pamuk | Species: Cat
  Owner: Elif Demir | Age: 8 months | Weight: 4.2 kg

ID: 1003 | Name: Karamel | Species: Dog
  Owner: Mehmet Kaya | Age: 36 months | Weight: 25.0 kg

ID: 1004 | Name: Minnak | Species: Rabbit
  Owner: Zeynep Arslan | Age: 6 months | Weight: 1.8 kg

ID: 1005 | Name: Fistik | Species: Cat
  Owner: Cagin Ozkaya | Age: 14 months | Weight: 5.1 kg

ID: 1006 | Name: Simba | Species: Dog
  Owner: Hatice Uysal | Age: 48 months | Weight: 30.75 kg

============================================
     Sorted by Age (Ascending)
============================================
ID: 1004 | Name: Minnak | Species: Rabbit
  Owner: Zeynep Arslan | Age: 6 months | Weight: 1.8 kg

ID: 1002 | Name: Pamuk | Species: Cat
  Owner: Elif Demir | Age: 8 months | Weight: 4.2 kg

ID: 1005 | Name: Fistik | Species: Cat
  Owner: Cagin Ozkaya | Age: 14 months | Weight: 5.1 kg

ID: 1001 | Name: Boncuk | Species: Dog
  Owner: Ahmet Yilmaz | Age: 24 months | Weight: 12.5 kg

ID: 1003 | Name: Karamel | Species: Dog
  Owner: Mehmet Kaya | Age: 36 months | Weight: 25.0 kg

ID: 1006 | Name: Simba | Species: Dog
  Owner: Hatice Uysal | Age: 48 months | Weight: 30.75 kg

============================================
Which pet would you like to discharge? Enter ID: 1003
>> Karamel (ID: 1003) has been discharged.

============================================
     Updated Patient List
============================================
ID: 1004 | Name: Minnak | Species: Rabbit
  Owner: Zeynep Arslan | Age: 6 months | Weight: 1.8 kg

ID: 1002 | Name: Pamuk | Species: Cat
  Owner: Elif Demir | Age: 8 months | Weight: 4.2 kg

ID: 1005 | Name: Fistik | Species: Cat
  Owner: Cagin Ozkaya | Age: 14 months | Weight: 5.1 kg

ID: 1001 | Name: Boncuk | Species: Dog
  Owner: Ahmet Yilmaz | Age: 24 months | Weight: 12.5 kg

ID: 1006 | Name: Simba | Species: Dog
  Owner: Hatice Uysal | Age: 48 months | Weight: 30.75 kg

============================================
Whose pet would you like to search? Cagin Ozkaya
>> Found: Fistik (Cat), ID: 1005, Age: 14 months, Weight: 5.1 kg

============================================
>> Heaviest patient: Simba (Dog) belonging to Hatice Uysal
   Weight: 30.75 kg

============================================
>> Total daily feeding cost: 135.875 TL
   (Calculated recursively for 5 patients)

  EXAMPLE RUN 2:

=== Veterinary Clinic Registration ===

Pet no. 1:
  Name: Tarcin
  Species: Cat
  Owner: Selin Yildiz
  Age (months): 18
  Weight (kg:gr): 3:750
  >> Tarcin (Cat) has been registered. [ID: 1001]

Pet no. 2:
  Name: Aslan
  Species: Dog
  Owner: Burak Celik
  Age (months): 60
  Weight (kg:gr): 35:200
  >> Aslan (Dog) has been registered. [ID: 1002]

Pet no. 3:
  Name: Zeytin
  Species: Cat
  Owner: Derya Aksoy

  Age (months): 10
  Weight (kg:gr): 4:600
  >> Zeytin (Cat) has been registered. [ID: 1003]

Pet no. 4:
  Name: Poncik
  Species: Dog
  Owner: Emre Sahin
  Age (months): 30
  Weight (kg:gr): 18:0
  >> Poncik (Dog) has been registered. [ID: 1004]

Pet no. 5:
  Name: Bulut
  Species: Hamster
  Owner: Ayse Kara
  Age (months): 4
  Weight (kg:gr): 0:350
  >> Bulut (Hamster) has been registered. [ID: 1005]

Pet no. 6:
  Name: Duman
  Species: Cat
  Owner: Yusuf Ekrem Kecilioglu
  Age (months): 22
  Weight (kg:gr): 6:100
  >> Duman (Cat) has been registered. [ID: 1006]

============================================
     Current Patients in the Clinic
============================================
ID: 1001 | Name: Tarcin | Species: Cat
  Owner: Selin Yildiz | Age: 18 months | Weight: 3.75 kg

ID: 1002 | Name: Aslan | Species: Dog
  Owner: Burak Celik | Age: 60 months | Weight: 35.2 kg

ID: 1003 | Name: Zeytin | Species: Cat
  Owner: Derya Aksoy | Age: 10 months | Weight: 4.6 kg

ID: 1004 | Name: Poncik | Species: Dog
  Owner: Emre Sahin | Age: 30 months | Weight: 18.0 kg

ID: 1005 | Name: Bulut | Species: Hamster
  Owner: Ayse Kara | Age: 4 months | Weight: 0.35 kg

ID: 1006 | Name: Duman | Species: Cat
  Owner: Yusuf Ekrem Kecilioglu | Age: 22 months | Weight: 6.1 kg

============================================
     Sorted by Age (Ascending)
============================================
ID: 1005 | Name: Bulut | Species: Hamster
  Owner: Ayse Kara | Age: 4 months | Weight: 0.35 kg

ID: 1003 | Name: Zeytin | Species: Cat
  Owner: Derya Aksoy | Age: 10 months | Weight: 4.6 kg

ID: 1001 | Name: Tarcin | Species: Cat
  Owner: Selin Yildiz | Age: 18 months | Weight: 3.75 kg

ID: 1006 | Name: Duman | Species: Cat
  Owner: Yusuf Ekrem Kecilioglu | Age: 22 months | Weight: 6.1 kg

ID: 1004 | Name: Poncik | Species: Dog
  Owner: Emre Sahin | Age: 30 months | Weight: 18.0 kg

ID: 1002 | Name: Aslan | Species: Dog
  Owner: Burak Celik | Age: 60 months | Weight: 35.2 kg

============================================
Which pet would you like to discharge? Enter ID: 1008
>> Error: No pet found with ID 1008.

============================================
     Updated Patient List
============================================
ID: 1005 | Name: Bulut | Species: Hamster
  Owner: Ayse Kara | Age: 4 months | Weight: 0.35 kg

ID: 1003 | Name: Zeytin | Species: Cat
  Owner: Derya Aksoy | Age: 10 months | Weight: 4.6 kg

ID: 1001 | Name: Tarcin | Species: Cat
  Owner: Selin Yildiz | Age: 18 months | Weight: 3.75 kg

ID: 1006 | Name: Duman | Species: Cat
  Owner: Yusuf Ekrem Kecilioglu | Age: 22 months | Weight: 6.1 kg

ID: 1004 | Name: Poncik | Species: Dog
  Owner: Emre Sahin | Age: 30 months | Weight: 18.0 kg

ID: 1002 | Name: Aslan | Species: Dog
  Owner: Burak Celik | Age: 60 months | Weight: 35.2 kg

============================================
Whose pet would you like to search? Ayse Kara
>> Found: Bulut (Hamster), ID: 1005, Age: 4 months, Weight: 0.35 kg

============================================
>> Heaviest patient: Aslan (Dog) belonging to Burak Celik
   Weight: 35.2 kg

============================================
>> Total daily feeding cost: 170.0 TL
   (Calculated recursively for 6 patients)

  EXAMPLE RUN 3:

=== Veterinary Clinic Registration ===

Pet no. 1:
  Name: Macaron
  Species: Parrot
  Owner: Ali Veli
  Age (months): 12
  Weight (kg:gr): 0:900
  >> Macaron (Parrot) has been registered. [ID: 1001]

Pet no. 2:
  Name: Findik
  Species: Dog
  Owner: Fatma Ozturk
  Age (months): 42
  Weight (kg:gr): 22:300
  >> Findik (Dog) has been registered. [ID: 1002]

Pet no. 3:
  Name: Latte
  Species: Cat
  Owner: Kemal Ates
  Age (months): 5
  Weight (kg:gr): 2:100
  >> Latte (Cat) has been registered. [ID: 1003]

Pet no. 4:
  Name: Pamir
  Species: Dog
  Owner: Sevgi Dogan
  Age (months): 28
  Weight (kg:gr): 15:500
  >> Pamir (Dog) has been registered. [ID: 1004]

Pet no. 5:
  Name: Ceviz
  Species: Rabbit
  Owner: Nihan Polat
  Age (months): 16
  Weight (kg:gr): 2:750
  >> Ceviz (Rabbit) has been registered. [ID: 1005]

Pet no. 6:
  Name: Kaplan
  Species: Dog
  Owner: Omer Aydin
  Age (months): 54
  Weight (kg:gr): 40:0
  >> Kaplan (Dog) has been registered. [ID: 1006]

============================================
     Current Patients in the Clinic
============================================
ID: 1001 | Name: Macaron | Species: Parrot
  Owner: Ali Veli | Age: 12 months | Weight: 0.9 kg

ID: 1002 | Name: Findik | Species: Dog
  Owner: Fatma Ozturk | Age: 42 months | Weight: 22.3 kg

ID: 1003 | Name: Latte | Species: Cat
  Owner: Kemal Ates | Age: 5 months | Weight: 2.1 kg

ID: 1004 | Name: Pamir | Species: Dog
  Owner: Sevgi Dogan | Age: 28 months | Weight: 15.5 kg

ID: 1005 | Name: Ceviz | Species: Rabbit
  Owner: Nihan Polat | Age: 16 months | Weight: 2.75 kg

ID: 1006 | Name: Kaplan | Species: Dog
  Owner: Omer Aydin | Age: 54 months | Weight: 40.0 kg

============================================
     Sorted by Age (Ascending)
============================================
ID: 1003 | Name: Latte | Species: Cat
  Owner: Kemal Ates | Age: 5 months | Weight: 2.1 kg

ID: 1001 | Name: Macaron | Species: Parrot
  Owner: Ali Veli | Age: 12 months | Weight: 0.9 kg

ID: 1005 | Name: Ceviz | Species: Rabbit
  Owner: Nihan Polat | Age: 16 months | Weight: 2.75 kg

ID: 1004 | Name: Pamir | Species: Dog
  Owner: Sevgi Dogan | Age: 28 months | Weight: 15.5 kg

ID: 1002 | Name: Findik | Species: Dog
  Owner: Fatma Ozturk | Age: 42 months | Weight: 22.3 kg

ID: 1006 | Name: Kaplan | Species: Dog
  Owner: Omer Aydin | Age: 54 months | Weight: 40.0 kg

============================================
Which pet would you like to discharge? Enter ID: 1002
>> Findik (ID: 1002) has been discharged.

============================================
     Updated Patient List
============================================
ID: 1003 | Name: Latte | Species: Cat
  Owner: Kemal Ates | Age: 5 months | Weight: 2.1 kg

ID: 1001 | Name: Macaron | Species: Parrot
  Owner: Ali Veli | Age: 12 months | Weight: 0.9 kg

ID: 1005 | Name: Ceviz | Species: Rabbit
  Owner: Nihan Polat | Age: 16 months | Weight: 2.75 kg

ID: 1004 | Name: Pamir | Species: Dog
  Owner: Sevgi Dogan | Age: 28 months | Weight: 15.5 kg

ID: 1006 | Name: Kaplan | Species: Dog
  Owner: Omer Aydin | Age: 54 months | Weight: 40.0 kg

============================================
Whose pet would you like to search? Can Yildirim
>> No pet found belonging to Can Yildirim.

============================================
>> Heaviest patient: Kaplan (Dog) belonging to Omer Aydin
   Weight: 40.0 kg

============================================
>> Total daily feeding cost: 153.125 TL
   (Calculated recursively for 5 patients)

Example Run

=== Veterinary Clinic Registration ===

Pet no. 1:
  Name: Boncuk
  Species: Dog
  Owner: Ahmet Yilmaz
  Age (months): 24
  Weight (kg:gr): 12:500
  >> Boncuk (Dog) has been registered. [ID: 1001]

Pet no. 2:
  Name: Pamuk
  Species: Cat
  Owner: Elif Demir
  Age (months): 8
  Weight (kg:gr): 4:200
  >> Pamuk (Cat) has been registered. [ID: 1002]

Pet no. 3:
  Name: Karamel
  Species: Dog
  Owner: Mehmet Kaya
  Age (months): 36
  Weight (kg:gr): 25:0
  >> Karamel (Dog) has been registered. [ID: 1003]

Pet no. 4:
  Name: Minnak
  Species: Rabbit
  Owner: Zeynep Arslan
  Age (months): 6
  Weight (kg:gr): 1:800
  >> Minnak (Rabbit) has been registered. [ID: 1004]

Pet no. 5:
  Name: Fistik
  Species: Cat
  Owner: Cagin Ozkaya
  Age (months): 14
  Weight (kg:gr): 5:100
  >> Fistik (Cat) has been registered. [ID: 1005]

Pet no. 6:
  Name: Simba
  Species: Dog
  Owner: Hatice Uysal
  Age (months): 48
  Weight (kg:gr): 30:750
  >> Simba (Dog) has been registered. [ID: 1006]

============================================
     Current Patients in the Clinic
============================================
ID: 1001 | Name: Boncuk | Species: Dog
  Owner: Ahmet Yilmaz | Age: 24 months | Weight: 12.5 kg

ID: 1002 | Name: Pamuk | Species: Cat
  Owner: Elif Demir | Age: 8 months | Weight: 4.2 kg

ID: 1003 | Name: Karamel | Species: Dog
  Owner: Mehmet Kaya | Age: 36 months | Weight: 25.0 kg

ID: 1004 | Name: Minnak | Species: Rabbit
  Owner: Zeynep Arslan | Age: 6 months | Weight: 1.8 kg

ID: 1005 | Name: Fistik | Species: Cat
  Owner: Cagin Ozkaya | Age: 14 months | Weight: 5.1 kg

ID: 1006 | Name: Simba | Species: Dog
  Owner: Hatice Uysal | Age: 48 months | Weight: 30.75 kg

============================================
     Sorted by Age (Ascending)
============================================
ID: 1004 | Name: Minnak | Species: Rabbit
  Owner: Zeynep Arslan | Age: 6 months | Weight: 1.8 kg

ID: 1002 | Name: Pamuk | Species: Cat
  Owner: Elif Demir | Age: 8 months | Weight: 4.2 kg

ID: 1005 | Name: Fistik | Species: Cat
  Owner: Cagin Ozkaya | Age: 14 months | Weight: 5.1 kg

ID: 1001 | Name: Boncuk | Species: Dog
  Owner: Ahmet Yilmaz | Age: 24 months | Weight: 12.5 kg

ID: 1003 | Name: Karamel | Species: Dog
  Owner: Mehmet Kaya | Age: 36 months | Weight: 25.0 kg

ID: 1006 | Name: Simba | Species: Dog
  Owner: Hatice Uysal | Age: 48 months | Weight: 30.75 kg

============================================
Which pet would you like to discharge? Enter ID: 1003
>> Karamel (ID: 1003) has been discharged.

============================================
     Updated Patient List
============================================
ID: 1004 | Name: Minnak | Species: Rabbit
  Owner: Zeynep Arslan | Age: 6 months | Weight: 1.8 kg

ID: 1002 | Name: Pamuk | Species: Cat
  Owner: Elif Demir | Age: 8 months | Weight: 4.2 kg

ID: 1005 | Name: Fistik | Species: Cat
  Owner: Cagin Ozkaya | Age: 14 months | Weight: 5.1 kg

ID: 1001 | Name: Boncuk | Species: Dog
  Owner: Ahmet Yilmaz | Age: 24 months | Weight: 12.5 kg

ID: 1006 | Name: Simba | Species: Dog
  Owner: Hatice Uysal | Age: 48 months | Weight: 30.75 kg

============================================
Whose pet would you like to search? Cagin Ozkaya
>> Found: Fistik (Cat), ID: 1005, Age: 14 months, Weight: 5.1 kg

============================================

>> Heaviest patient: Simba (Dog) belonging to Hatice Uysal
   Weight: 30.75 kg

============================================
>> Total daily feeding cost: 136.375 TL
   (Calculated recursively for 5 patients)

Hints

•  For the weight parsing: use String.split(":") to separate kilograms and grams, then

combine them. For example, "12:500" becomes 12 + 500 / 1000.0 = 12.5 kg.

•  For Selection Sort, in each pass find the minimum-age pet in the unsorted portion and swap

it with the current position.

•  For the recursive method, think about the base case first: when index == patientCount,

return 0.0. Otherwise, compute the current pet's cost and add the recursive call for the next
index.

•  When removing a pet, do not leave gaps in the array. Copy the last element to the removed

position and decrement the counter.

•  Use Scanner for user input. Remember to handle nextLine() after nextInt() to consume

the remaining newline character.


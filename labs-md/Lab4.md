Department of Computer Engineering

Ankara Yıldırım Beyazıt University
CENG114 – Computer Programming II
Spring 2025 – 2026

Lab Guide 4

OBJECTIVE: Visibility Modifiers, Accessors/Mutators, Array of Objects, String
Manipulation (split, StringBuilder), Inheritance (Is-A Relation), ArrayList

Instructor : Yusuf Evren AYKAÇ
Assistants : Çağın ÖZKAYA, Hatice UYSAL, Yusuf
Ekrem KEÇİLİOĞLU

Week : 6

Question 1 – Campus Shuttle Luggage System
Write a Java program that stores and displays the luggage information during a check-in
process of the Ankara Yıldırım Beyazıt University Campus Shuttle operations. Create
Luggage and LuggageList classes as shown in the following Class Diagrams.

Class: Luggage

Luggage

- luggage_ID: int
- belongsTo: String
- weight_kilo: int
- capacity_lt: double
- lastUsedId: int = 500
+ Luggage(String, int, double)
+ getLuggageId(): int
+ getBelongsTo(): String
+ getWeight(): int
+ getCapacity(): double
+ toString(): String

Class: LuggageList

LuggageList

+ MAX_COUNT: final int = 5
+ MAX_KILOS: final int = 50
- myLuggages[]: Luggage
- total_Kilo: int
- total_LuggageCount: int
+ addLuggage(Luggage): boolean
+ removeLuggage(String): boolean
+ getLuggage(String): Luggage
+ getAll(): Luggage[]
+ getHighestCapacityLuggage(): Luggage
+ display(): void

Implementation Steps

Create a Luggage class:

•  Write the data members as shown in the class diagram.

CENG114 – Lab Guide 4 Ankara Yıldırım Beyazıt University

Department of Computer Engineering

•  Write a non-default constructor that takes belongsTo, weight_kilo and capacity_lt as

parameters and assigns the values. Also, set the lastUsedId as the object’s
luggage_ID and increment it.

•  Write getter methods for luggage_ID, belongsTo, weight_kilo and capacity_lt.
•  Write a toString() method that returns luggage_ID, belongsTo, weight_kilo and

capacity_lt.

Create a LuggageList class:

•  Write static final members MAX_COUNT with value of 5 (determines the size of the

object array) and MAX_KILOS with value of 50. The campus shuttle’s cargo capacity
allows up to 5 pieces of luggage with a maximum total of 50 kilos.

•  Write an object array myLuggages[] created from the Luggage class, with

MAX_COUNT as the size.

•  Write a data member named total_LuggageCount to store the number of luggage

items in the system.

•  Write an addLuggage() method that takes a Luggage object as parameter and

checks if the array is full or the weight limit is exceeded. If not full, add the Luggage
object to the array and return true. If the array is full or weight limit is exceeded,
return false.

•  Write a removeLuggage() method that takes the luggage_ID of the Luggage to be

removed. Search the array to find the given id (the parameter type is String, so you
will need to parse it to int). If found, move the last element to the index of the found
item and decrement the count. Return true if successful, false otherwise.

•  Write a getLuggage() method that takes the ‘belongsTo’ information as parameter,
searches for that luggage in the object array. If found, return the object; otherwise,
return null.

•  Write a getAll() method that returns the myLuggages object array.
•  Write a getHighestCapacityLuggage() method that searches for the luggage with the

highest capacity and returns that Luggage object.

•  Write a display() method that prints all luggage objects’ information to the console.

Create a Main Class called ShuttleCargo:

•

In the main method, input 5 pieces of luggage and store them into a LuggageList
object.

•  Scan capacity information as width:height:length in cm (e.g., 10:10:10) and convert it

to liters using the split() method of String. (HINT: 1000 cm³ = 1 liter)

•  Display the contents of the LuggageList object using the toString method.
•  Ask the user which luggage to remove and then remove it.
•  Ask the user which luggage to search and then display it.
•  Display the luggage with the highest capacity.

Example Run

Luggage no. 1:
Belongs to: Evren Aykac
Enter weight in kilos:
20
Enter capacity like Width:Height:Length
10:10:10
The luggage belonging to: Evren Aykac is added to the list.

Luggage no. 2:

CENG114 – Lab Guide 4 Ankara Yıldırım Beyazıt University

Department of Computer Engineering

Belongs to: Cagin Ozkaya
Enter weight in kilos:
10
Enter capacity like Width:Height:Length
15:20:30
The luggage belonging to: Cagin Ozkaya is added to the list.

Luggage no. 3:
Belongs to: Hatice Uysal
Enter weight in kilos:
5
Enter capacity like Width:Height:Length
11:12:13
The luggage belonging to: Hatice Uysal is added to the list.

Luggage no. 4:
Belongs to: Yusuf Ekrem Kecilioglu
Enter weight in kilos:
6
Enter capacity like Width:Height:Length
5:6:7
The luggage belonging to: Yusuf Ekrem Kecilioglu is added to the list.

Luggage no. 5:
Belongs to: Kemal Yilmaz
Enter weight in kilos:
8
Enter capacity like Width:Height:Length
5:11:9
Error: Max size of (kilo or/and count) is reached! Cannot add!

-----------------
Here is a list of the luggages...
ID: 500
Belongs to: Evren Aykac
Weight of the luggage: 20
Capacity of the luggage: 1.0 Liters

ID: 501
Belongs to: Cagin Ozkaya
Weight of the luggage: 10
Capacity of the luggage: 9.0 Liters

ID: 502
Belongs to: Hatice Uysal
Weight of the luggage: 5
Capacity of the luggage: 1.716 Liters

ID: 503
Belongs to: Yusuf Ekrem Kecilioglu
Weight of the luggage: 6
Capacity of the luggage: 0.21 Liters

------------------
Which luggage would you like to delete?
Enter an ID:
501
The Luggage belonging to: Cagin Ozkaya is removed.

ID: 500
Belongs to: Evren Aykac
Weight of the luggage: 20
Capacity of the luggage: 1.0 Liters

CENG114 – Lab Guide 4 Ankara Yıldırım Beyazıt University

Department of Computer Engineering

ID: 503
Belongs to: Yusuf Ekrem Kecilioglu
Weight of the luggage: 6
Capacity of the luggage: 0.21 Liters

ID: 502
Belongs to: Hatice Uysal
Weight of the luggage: 5
Capacity of the luggage: 1.716 Liters

------------------
Whose luggage would you like to search?
Evren Aykac
Here is the luggage you were looking for...
ID: 500
Belongs to: Evren Aykac
Weight of the luggage: 20
Capacity of the luggage: 1.0 Liters

-----------------
The luggage belonging to: Hatice Uysal has the highest
capacity of 1.716 liters.
-----------------

CENG114 – Lab Guide 4 Ankara Yıldırım Beyazıt University

Department of Computer Engineering

Question 2 – Staff Information System with String Manipulation
Create the following class Staff, by using the class structure given below.

Staff

- name: String
- surname: String
- email: String
- phone: int
+ Staff()
+ Staff(String, String, String, int)
+ getSurname(): String
+ toString(): String

Tasks

a)  Write a Java program that gets 3 strings from the user, where each string has its fields
separated by the * character. The program should separate the strings using the split()
method and convert them into a Staff objects array. Finally, display the content of the array.

Before displaying output with toString(), do the following:

b)  The phone numbers must be reversed. Use the reverse() method of StringBuilder.

c)  Append the CENG department extension, which is 14, at the end of the phone numbers
using the append() method of StringBuilder.

While displaying output with toString(), do the following:

d)  While displaying your Staff objects, find the surname’s index in the output of the
toString() method using indexOf(). Print the index as: "Surname's position is at #"

e)  Lastly, compare the first object’s surname with the second object’s surname using the
compareTo() method. If they are the same, print an appropriate message; otherwise, print
"Surnames are not equal".

Example Run

Enter 1. string:
Hatice*Uysal*huysal@aybu.edu.tr*7045092

Enter 2. string:
Evren*Aykac*eaykac@aybu.edu.tr*1705092

Enter 3. string:
Cagin*Ozkaya*cozkaya@aybu.edu.tr*4705092

NAME: Hatice
SURNAME: Uysal
EMAIL: huysal@aybu.edu.tr
PHONE: 290540714
Surname's position is at 21

NAME: Evren

CENG114 – Lab Guide 4 Ankara Yıldırım Beyazıt University

Department of Computer Engineering

SURNAME: Aykac
EMAIL: eaykac@aybu.edu.tr
PHONE: 290507114
Surname's position is at 21

NAME: Cagin
SURNAME: Ozkaya
EMAIL: cozkaya@aybu.edu.tr
PHONE: 290507414
Surname's position is at 21

Surnames are not equal..

Note: In this question, you should use StringBuilder instead of the older StringBuffer class.
StringBuilder is preferred in single-threaded contexts as it provides better performance.

CENG114 – Lab Guide 4 Ankara Yıldırım Beyazıt University

Department of Computer Engineering

Question 3 – Campus Tech Shop with Inheritance
Create the class diagrams shown below exactly. Pay attention to the Is-A (inheritance)
relations among them.

CENG114 – Lab Guide 4 Ankara Yıldırım Beyazıt University

Department of Computer Engineering

System Class:

Shop_Sys

# electronicsArrayList: ArrayList<Electronics>
+ addElectronics(Electronics): void
+ removeElectronics(int): Electronics
+ getAllElectronics(): String
+ getSmartphone(): String
+ showTotalElectronics(): String

Parent Class:

Electronics

# ID: int
# brand: String
# type: String
# price: double
# total: int
+ Electronics(int, String, String, double)
+ checkID(int): boolean
+ toString(): String

Child Classes (inherit from Electronics):

Smartphone

Wearable

PowerBank

- operator: String

- bodypart: String

Constructor w/ params
+findPromotion():String
+toString(): String

Constructor w/ params
+toString(): String

- capacity: int
- cableLength: int

Constructor w/ params
+toString(): String

Implementation Steps

Implement Electronics.java (parent class):

•  Data members are ID, brand, type and price.
•  Write a non-default constructor and toString() method.
•  Write a checkID() method that takes an id number, compares it with its own id, and

returns a boolean value.

•  Write any necessary getter and/or setter methods for all the classes.

Implement Smartphone.java (inherits from Electronics):

•  Data member is operator.
•  Write a non-default constructor and a toString() method.
•  Write a findPromotion() method that checks the following conditions and returns the

promotion information:
◦
◦
◦

If the operator is Turkcell, customer can speak free in the mornings.
If the operator is Vodafone, customer can speak free at lunch times.
If the operator is Turk Telekom, customer can speak free in the evenings.

Implement Wearable.java (inherits from Electronics):

•  Data member is bodypart.
•  Write a non-default constructor and a toString() method.

CENG114 – Lab Guide 4 Ankara Yıldırım Beyazıt University

Department of Computer Engineering

Implement PowerBank.java (inherits from Electronics):
•  Data members are capacity and cableLength.
•  Write a non-default constructor and a toString() method.

Implement Shop_Sys.java (system class):

•  Data member electronicsArrayList stores Electronics objects in an ArrayList.
•  Write an addElectronics() method to add an Electronics object to the ArrayList.
•  Write a removeElectronics() method that takes an id as parameter, finds and

removes the matching item from the ArrayList, decreases the total in the Electronics
class, and returns the removed object.

•  Write a getAllElectronics() method that concatenates and returns all ArrayList items’

toString() output.

•  Write a getSmartphone() method that returns all Smartphone objects’ toString()

output along with their promotion information.

•  Write a showTotalElectronics() method that returns the total electronics count as a

String.

Write a Main Class:

•  Ask the user how many smartphone objects to create and get input for each (id,

brand, type, price, operator).

•  Create the following pre-defined objects:

new Wearable(510, "Apple", "Series 9", 12000, "Wrist");
new Wearable(503, "Meta", "Quest 3", 18000, "Head");
new PowerBank(505, "Anker", "PowerCore", 800, 20000, 120);

•  Store all objects into the ArrayList of Shop_Sys and display only the Smartphone

objects’ information first, along with the total electronics count.

•  Ask for an id number to remove an object from the list.
•  Finally, print all remaining electronics’ information and the updated total count.

Example Run

Enter number of smartphones:
3

Enter smartphone id:
100
Enter smartphone brand:
Apple
Enter smartphone type:
iPhone 16
Enter smartphone price:
75000
Enter smartphone operator information:
Turk Telekom

Enter smartphone id:
101
Enter smartphone brand:
Samsung
Enter smartphone type:
Galaxy S25 Ultra
Enter smartphone price:
72000

CENG114 – Lab Guide 4 Ankara Yıldırım Beyazıt University

Department of Computer Engineering

Enter smartphone operator information:
Turkcell

Enter smartphone id:
102
Enter smartphone brand:
Google
Enter smartphone type:
Pixel 9 Pro
Enter smartphone price:
55000
Enter smartphone operator information:
Vodafone

Show only Smartphone objects:
Smartphone Information:
ID: 100
Brand: Apple
Price: 75000.0 TL
Type: iPhone 16
Operator: Turk Telekom
it is free in the evenings

Smartphone Information:
ID: 101
Brand: Samsung
Price: 72000.0 TL
Type: Galaxy S25 Ultra
Operator: Turkcell
it is free in the mornings

Smartphone Information:
ID: 102
Brand: Google
Price: 55000.0 TL
Type: Pixel 9 Pro
Operator: Vodafone
it is free in the lunch times

Total number of devices: 6

Enter the id of the device you want to remove:
102
The device with id 102 is removed from the list.

All Electronics:
Smartphone Information:
ID: 100
Brand: Apple
Price: 75000.0 TL
Type: iPhone 16
Operator: Turk Telekom

Smartphone Information:
ID: 101
Brand: Samsung
Price: 72000.0 TL
Type: Galaxy S25 Ultra
Operator: Turkcell

Wearable Information:
ID: 510
Brand: Apple

CENG114 – Lab Guide 4 Ankara Yıldırım Beyazıt University

Department of Computer Engineering

Price: 12000.0 TL
Type: Series 9
Bodypart: Wrist

Wearable Information:
ID: 503
Brand: Meta
Price: 18000.0 TL
Type: Quest 3
Bodypart: Head

PowerBank Information:
ID: 505
Brand: Anker
Price: 800.0 TL
Type: PowerCore
Capacity: 20000 mAh
Cable Length: 120 cm

Total number of devices: 5

CENG114 – Lab Guide 4 Ankara Yıldırım Beyazıt University


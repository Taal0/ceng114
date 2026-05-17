Ankara Yıldırım Beyazıt University
Department of Computer Engineering

CENG114 – Computer Programming II
Lab Guide #9: Generics, Bounded Wildcards

Instructor: Yusuf Evren AYKAÇ

Week: 13 (Spring 2025–2026)

Assistants: Çağın ÖZKAYA, Hatice UYSAL, Yusuf Ekrem KEÇİLİOĞLU

Lab: 9

HINT:

Java's wildcards exist because List<Integer> is not a subtype of List<Number>. To work safely across
parameterised types, we use bounded wildcards. The mnemonic is PECS — Producer Extends, Consumer
Super.

PECS at a Glance

? extends T    the collection produces Ts (you READ from it).

? super T      the collection consumes Ts (you WRITE to it).

?              you don't care about the element type — only structural ops (size, isEmpty,
iteration to print).

Question:

You are joining the engineering team of DataLab, a small analytics service used by research groups on
campus. Each research group sends DataLab a different shape of numeric data:

•  A psychology group uploads Likert-scale survey ratings as List<Integer> (1–7).

•  A physics group uploads sensor readings as List<Double> (temperature, voltage).

•  An image-processing group uploads pixel intensities as List<Float>.

•  A finance group uploads transaction amounts as List<Number>.

•  A logging module stores arbitrary objects as List<Object> for audit trails.

The platform also keeps labeled data containers like ("GPA", 3.75) or ("Subject", "Alice") for
human-readable reports.

Your job is to design five components so that:

•  Each numeric utility is written once and works for every Number subtype.

•  Writing utilities accept the most permissive destination type (e.g., addIntegers must also accept

List<Number> or List<Object>).

•  Generic-blind utilities (size, print, format) work over any collection.

Page 1 of 8    ·    Ankara Yıldırım Beyazıt University · Department of Computer Engineering

#

1

2

3

4

5

Class

Role

DataContainer<T>

NumberFilter

DataProcessor

Generic label + payload (instance state, single type
parameter)

Static numeric filters using ? extends Number

Static writer utilities using ? super T

CollectionUtils

Static type-agnostic utilities using ?

StatisticsCalculator

Static utilities combining all wildcard kinds

Then write TestWildcards.java to exercise everything (§9).

Class 1 — DataContainer<T>

A simple generic value object: a label paired with a payload of any type.

DataContainer<T>

- data: T

- label: String

+ DataContainer(data: T, label: String)

+ getData(): T

+ getLabel(): String

+ toString(): String

Requirements

•  Both fields are private and final.

•  Constructor sets both fields.

•

toString() returns "<label>: <data>" — e.g., "Age: 42" or "GPA: 3.75".

Why generic? A single class supports DataContainer<Integer>, DataContainer<Double>, and
DataContainer<String> without duplication.

Class 2 — NumberFilter (Upper-Bounded Wildcards)

Reading-only utilities over numeric lists. Each method must work for every Number subtype with a
single signature.

«utility»  NumberFilter   <<static>>

+ filterGreaterThan(numbers: List<? extends Number>, threshold: double): List<Number>

Page 2 of 8    ·    Ankara Yıldırım Beyazıt University · Department of Computer Engineering

+ filterInRange(numbers: List<? extends Number>, min: double, max: double):
List<Number>

+ findMax(numbers: List<? extends Number>): Double

+ findMin(numbers: List<? extends Number>): Double

Why ? extends Number? We only read from the input list and never insert into it (Producer →
extends).

1  filterGreaterThan

Returns a new list containing every element strictly greater than threshold.

Algorithm

1.  Create an empty List<Number> named result.

2.  For each n in numbers, if n.doubleValue() > threshold, add n to result.

3.  Return result.

Edge case: an empty input returns an empty list, not null.

2  filterInRange

Returns numbers within the closed range [min, max].

Algorithm

1.  Create an empty List<Number> result.

2.  For each n, let v = n.doubleValue().

3.

If min ≤ v ≤ max, add n to result.

4.  Return result.

3  findMax  /  findMin

Both return a Double, or null if the list is empty.

Algorithm — findMax (findMin is symmetric)

1.

2.

If the list is empty, return null.

Initialise max = numbers.get(0).doubleValue().

3.  For each remaining n, if n.doubleValue() > max, set max = n.doubleValue().

4.  Return max.

Class 3 — DataProcessor (Lower-Bounded Wildcards)

Writer utilities. Each method inserts elements into a destination list, where the destination may be the
most permissive supertype.

«utility»  DataProcessor   <<static>>

+ addIntegers(destination: List<? super Integer>, values: Integer...): void

+ <T> copyAll(source: List<? extends T>, destination: List<? super T>): void

Page 3 of 8    ·    Ankara Yıldırım Beyazıt University · Department of Computer Engineering

+ <T> fill(collection: List<? super T>, value: T, count: int): void

+ collectGreaterThan(source: List<? extends Number>,
                    destination: List<? super Number>, threshold: double): void

Why ? super T? We write into the destination (Consumer → super). One signature accepts
List<Integer>, List<Number>, and List<Object> as targets.

1  addIntegers

Algorithm

1.  For each v in values, call destination.add(v).

2  copyAll<T>

The textbook PECS demo: read with extends, write with super.

Algorithm

1.  For each e in source, call destination.add(e).

3  fill<T>

Algorithm

1.

If count ≤ 0, return immediately.

2.  Repeat count times: collection.add(value).

4  collectGreaterThan

Combines read (extends) with write (super).

Algorithm

1.  For each n in source, if n.doubleValue() > threshold, call destination.add(n).

Class 4 — CollectionUtils (Unbounded Wildcards)

Type-agnostic utilities. We never look at the element's actual type — only its presence, identity (null), or
string form.

«utility»  CollectionUtils   <<static>>

+ printAll(c: Collection<?>): void

+ getSize(c: Collection<?>): int

+ isEmpty(c: Collection<?>): boolean

+ haveSameSize(c1: Collection<?>, c2: Collection<?>): boolean

+ countNulls(c: Collection<?>): int

+ format(c: Collection<?>): String

Why use ? alone? No write happens; no element-type method is needed. Maximum flexibility.

Page 4 of 8    ·    Ankara Yıldırım Beyazıt University · Department of Computer Engineering

1  printAll

For each element, print it on its own line via System.out.println(e). The default toString() is invoked
automatically.

2  getSize  /  isEmpty

Delegate to c.size() and c.isEmpty().

3  haveSameSize

Return c1.size() == c2.size().

4  countNulls

Algorithm

1.

Initialise count = 0.

2.  For each e in c, if e == null, increment count.

3.  Return count.

5  format

Build the string [e1, e2, e3] using StringBuilder, separator ", ". For an empty collection, return "[]".

Class 5 — StatisticsCalculator (All Three Wildcards)

The capstone class — uses every wildcard kind in one cohesive utility.

«utility»  StatisticsCalculator   <<static>>

+ calculateSum(numbers: List<? extends Number>): double

+ calculateAverage(numbers: List<? extends Number>): double

+ collectAboveAverage(source: List<? extends Number>,
                     destination: List<? super Number>): void

+ createReport(data: Collection<?>): String

+ normalizeAndCollect(source: List<? extends Number>,
                     destination: List<? super Double>): void

1  calculateSum

Sum n.doubleValue() over the list and return as double.

2  calculateAverage

If empty, return 0.0; else return calculateSum(numbers) / numbers.size().

3  collectAboveAverage

Algorithm

1.  Compute avg = calculateAverage(source).

2.  For each n in source, if n.doubleValue() > avg, add n to destination.

Page 5 of 8    ·    Ankara Yıldırım Beyazıt University · Department of Computer Engineering

4  createReport

Return a multi-line report:

Elements: <count>
Empty:    <true|false>
Contents: [e1, e2, ...]

5  normalizeAndCollect

Min-max normalisation to the range [0, 100], written into a List<? super Double>.

Algorithm

1.

If source is empty, return immediately.

2.  Compute min and max via NumberFilter.findMin / findMax.

3.  Edge case: if min == max, push 50.0 for every element and return.

4.  Otherwise, for each n, compute normalised = ((n.doubleValue() − min) / (max − min)) × 100 and

add to destination.

TestWildcards.java

Implement a single main that runs five demo parts. Print a section banner before each part exactly as
shown in §10.

Part 1 — DataContainer

DataContainer<Integer> age  = new DataContainer<>(25,    "Age");
DataContainer<Double>  gpa  = new DataContainer<>(3.75,  "GPA");
DataContainer<String>  name = new DataContainer<>("Alice", "Name");

Part 2 — Upper-Bounded Wildcards (NumberFilter)

List<Integer> integers = Arrays.asList(5, 10, 15, 20, 25, 30);
List<Double>  doubles  = Arrays.asList(5.5, 10.5, 15.5, 20.5, 25.5);
List<Float>   floats   = Arrays.asList(7.5f, 12.5f, 17.5f, 22.5f);

Part 3 — Lower-Bounded Wildcards (DataProcessor)

List<Integer> intList = new ArrayList<>();
List<Number>  numList = new ArrayList<>();
List<Object>  objList = new ArrayList<>();

DataProcessor.addIntegers(intList, 1, 2, 3);
DataProcessor.addIntegers(numList, 4, 5, 6);
DataProcessor.addIntegers(objList, 7, 8, 9);

List<Integer> source     = Arrays.asList(100, 200, 300);
List<Number>  numTarget  = new ArrayList<>();
List<Number>  fillList   = new ArrayList<>();
List<Integer> sourceData = Arrays.asList(5, 15, 25, 35, 45);
List<Number>  collected  = new ArrayList<>();

Page 6 of 8    ·    Ankara Yıldırım Beyazıt University · Department of Computer Engineering

Part 4 — Unbounded Wildcards (CollectionUtils)

List<String>  strings    = Arrays.asList("Java", "Generics", "Wildcards");
List<Integer> numbers4   = Arrays.asList(10, 20, 30, 40);
List<Double>  doublesNul = Arrays.asList(1.1, 2.2, null, 3.3, null);

Part 5 — Combined (StatisticsCalculator)

List<Integer> data     = Arrays.asList(10, 20, 30, 40, 50, 60, 70);
List<Integer> original = Arrays.asList(10, 30, 50, 70, 90);
List<Number>  normOut  = new ArrayList<>();

Expected Output

Your TestWildcards.java should produce output that matches the following (whitespace and decoration
may differ slightly):

============================================================
   Java Generics & Wildcards Lab — DataLab Analytics
============================================================

--- Part 1 · DataContainer ---
Age: 25
GPA: 3.75
Name: Alice

--- Part 2 · Upper-Bounded Wildcards (? extends T) ---
Integers > 12 ............ [15, 20, 25, 30]
Doubles  > 15 ............ [15.5, 20.5, 25.5]
Floats   > 10 ............ [12.5, 17.5, 22.5]
Integers in range [10,25]  [10, 15, 20, 25]
Max of integers .......... 30.0
Min of doubles ........... 5.5

--- Part 3 · Lower-Bounded Wildcards (? super T) ---
Integer list ............. [1, 2, 3]
Number  list ............. [4, 5, 6]
Object  list ............. [7, 8, 9]
Copied Int -> Number ..... [100, 200, 300]
Filled with 42 (x5) ...... [42, 42, 42, 42, 42]
Collected n > 20 ......... [25, 35, 45]

--- Part 4 · Unbounded Wildcards (?) ---
printAll(strings):
Java
Generics
Wildcards
size(numbers) ............ 4
isEmpty(strings) ......... false
sameSize(strings, nums) .. false
nullCount(doubles) ....... 2
format(numbers) .......... [10, 20, 30, 40]

--- Part 5 · Combined (All Wildcards) ---

Page 7 of 8    ·    Ankara Yıldırım Beyazıt University · Department of Computer Engineering

Data ..................... [10, 20, 30, 40, 50, 60, 70]
Sum ...................... 280.0
Average .................. 40.0
Above average ............ [50, 60, 70]
Report:
  Elements: 7
  Empty:    false
  Contents: [10, 20, 30, 40, 50, 60, 70]
Original ................. [10, 30, 50, 70, 90]
Normalized [0,100] ....... [0.0, 25.0, 50.0, 75.0, 100.0]

============================================================
   All Tests Complete
============================================================

Pitfalls to Avoid
✗  Don't try destination.add(...) on a List<? extends Number> — it won't compile (and shouldn't).
✗  Don't read a value typed as T from List<? super T> — you only get Object back.
✗  For findMax / findMin, an empty list returns null — not 0.0.
✗  In normalizeAndCollect, handle the min == max edge case before division.
✓  When in doubt, ask: "Am I reading from this list, writing to it, or just iterating?" PECS will tell
you the answer.

Page 8 of 8    ·    Ankara Yıldırım Beyazıt University · Department of Computer Engineering


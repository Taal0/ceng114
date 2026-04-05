# Java Core Concepts: Comparable, hashCode/equals, and Cloneable

This document provides comprehensive examples and explanations of key Java interfaces and methods used for object comparison, hashing, and cloning.

---

## 1. Comparable Interface

### Purpose
The `Comparable` interface allows objects to be sorted naturally. Classes implementing this interface can define their own sorting logic.

### Student Class Implementation

```java
public class Student implements Comparable<Student> {
    private String name;
    private int grade;
    
    public Student(String name, int grade) {
        this.name = name;
        this.grade = grade;
    }
    
    @Override
    public int compareTo(Student other) {
        return this.grade - other.grade; // Ascending order by grade
    }
    
    @Override
    public String toString() {
        return name + " (" + grade + ")";
    }
}
```

### Usage Example

```java
public class Main {
    public static void main(String[] args) {
        Student s1 = new Student("Ayşe", 90);
        Student s2 = new Student("Ali", 75);
        Student s3 = new Student("Zeynep", 85);
        
        List<Student> list = Arrays.asList(s1, s2, s3);
        Collections.sort(list); // Works thanks to Comparable
        
        System.out.println(list);
    }
}
```

**Output:**
```
[Ali (75), Zeynep (85), Ayşe (90)]
```

### Where It's Used
* `Collections.sort()` or `Arrays.sort()` methods
* Sorted collections like `TreeSet` and `TreeMap`
* Scenarios requiring custom objects to be stored in sorted order

---

## 2. hashCode() and equals() Methods

### Purpose
The `hashCode()` method is primarily used in hash-based collections (`HashMap`, `HashSet`, `Hashtable`). These collections use hash table logic to quickly search or compare objects.

### Basic Implementation

```java
import java.util.Objects;

public class Student {
    private int id;
    private String name;
    
    public Student(int id, String name) {
        this.id = id;
        this.name = name;
    }
    
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Student)) return false;
        Student other = (Student) obj;
        return id == other.id && name.equals(other.name);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(id, name); // Modern approach
    }
    
    @Override
    public String toString() {
        return "Student{id=" + id + ", name='" + name + "'}";
    }
}
```

### Testing equals() and hashCode()

```java
public class Main {
    public static void main(String[] args) {
        Student s1 = new Student(1, "Ayşe");
        Student s2 = new Student(1, "Ayşe");
        
        System.out.println(s1.equals(s2));      // true
        System.out.println(s1.hashCode() == s2.hashCode()); // true
        
        HashSet<Student> set = new HashSet<>();
        set.add(s1);
        set.add(s2);
        System.out.println(set.size()); // 1 (considered same object)
    }
}
```

---

## 3. HashCode Collision Demonstration

### Example 1: Normal Hash Distribution

This example demonstrates how `hashCode()` and `equals()` work together in a `HashSet`.

```java
import java.util.HashSet;
import java.util.Objects;

class Student {
    private int id;
    private String name;
    
    public Student(int id, String name) {
        this.id = id;
        this.name = name;
    }
    
    @Override
    public int hashCode() {
        int hash = Objects.hash(id, name);
        System.out.println("hashCode() called for: " + name + " → " + hash);
        return hash;
    }
    
    @Override
    public boolean equals(Object obj) {
        System.out.println("equals() called between " + this.name + " and " + ((Student)obj).name);
        if (this == obj) return true;
        if (!(obj instanceof Student)) return false;
        Student other = (Student) obj;
        return id == other.id && Objects.equals(name, other.name);
    }
    
    @Override
    public String toString() {
        return "Student{id=" + id + ", name='" + name + "'}";
    }
}

public class Main {
    public static void main(String[] args) {
        HashSet<Student> set = new HashSet<>();
        Student s1 = new Student(1, "Ayşe");
        Student s2 = new Student(1, "Ayşe");
        Student s3 = new Student(2, "Ali");
        
        System.out.println("Adding s1...");
        set.add(s1);
        
        System.out.println("\nAdding s2...");
        set.add(s2);
        
        System.out.println("\nAdding s3...");
        set.add(s3);
        
        System.out.println("\nSet contents: " + set);
    }
}
```

**Output:**
```
Adding s1...
hashCode() called for: Ayşe → 1952837195

Adding s2...
hashCode() called for: Ayşe → 1952837195
equals() called between Ayşe and Ayşe

Adding s3...
hashCode() called for: Ali → 92347384

Set contents: [Student{id=1, name='Ayşe'}, Student{id=2, name='Ali'}]
```

**Explanation:** When `s2` is added, it has the same hash code as `s1`, so `equals()` is called to verify they're actually the same object.

---

### Example 2: Forced Hash Collision

This example shows what happens when all objects return the same hash code.

```java
import java.util.HashSet;
import java.util.Objects;

class Student {
    private int id;
    private String name;
    
    public Student(int id, String name) {
        this.id = id;
        this.name = name;
    }
    
    // Forcing all objects to return the same hash value:
    @Override
    public int hashCode() {
        System.out.println("hashCode() called for: " + name + " → returns 100");
        return 100; // All go to the same bucket
    }
    
    @Override
    public boolean equals(Object obj) {
        System.out.println("equals() called between " + this.name + " and " + ((Student)obj).name);
        if (this == obj) return true;
        if (!(obj instanceof Student)) return false;
        Student other = (Student) obj;
        return id == other.id && Objects.equals(name, other.name);
    }
    
    @Override
    public String toString() {
        return "Student{id=" + id + ", name='" + name + "'}";
    }
}

public class Main {
    public static void main(String[] args) {
        HashSet<Student> set = new HashSet<>();
        Student s1 = new Student(1, "Ayşe");
        Student s2 = new Student(2, "Ali");
        Student s3 = new Student(3, "Zeynep");
        
        System.out.println("Adding s1...");
        set.add(s1);
        
        System.out.println("\nAdding s2...");
        set.add(s2);
        
        System.out.println("\nAdding s3...");
        set.add(s3);
        
        System.out.println("\nSet contents: " + set);
    }
}
```

**Output:**
```
Adding s1...
hashCode() called for: Ayşe → returns 100

Adding s2...
hashCode() called for: Ali → returns 100
equals() called between Ayşe and Ali

Adding s3...
hashCode() called for: Zeynep → returns 100
equals() called between Ayşe and Zeynep
equals() called between Ali and Zeynep

Set contents: [Student{id=1, name='Ayşe'}, Student{id=2, name='Ali'}, Student{id=3, name='Zeynep'}]
```

---

## 4. Performance Impact of Hash Collisions

### The Downside of Same Bucket

When multiple objects share the same hash code, performance degrades because:

* Normally, hash-based collections have O(1) lookup time (very fast)
* With many collisions, multiple objects end up in the same bucket
* Java must check each object in that bucket using `equals()`, one by one
* This can slow down search/insert operations to O(n)

### How Java Handles Collisions

| Java Version | Bucket Structure | Description |
|--------------|------------------|-------------|
| Java 7 and earlier | Linked List | Colliding objects were added to a sequential list |
| Java 8 and later | Balanced Tree (Red-Black Tree) | If a bucket has more than 8 collisions, a tree is used instead → improves search speed |

This ensures that even with many collisions, modern Java maintains reasonable performance.

---

## 5. Retrieving Specific Objects from Same Bucket

### HashMap Example

```java
import java.util.HashMap;

class Student {
    int id;
    String name;
    
    public Student(int id, String name) {
        this.id = id;
        this.name = name;
    }
    
    @Override
    public int hashCode() {
        return 100; // Force same bucket
    }
    
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Student)) return false;
        Student other = (Student) o;
        return id == other.id && name.equals(other.name);
    }
    
    @Override
    public String toString() {
        return name + " (" + id + ")";
    }
}

public class Main {
    public static void main(String[] args) {
        HashMap<Student, String> map = new HashMap<>();
        
        Student s1 = new Student(1, "Ayşe");
        Student s2 = new Student(2, "Ali");
        
        map.put(s1, "Data1");
        map.put(s2, "Data2");
        
        // Even though they're in the same bucket, equals() helps find the correct data:
        Student key = new Student(2, "Ali");
        System.out.println(map.get(key)); // Data2
    }
}
```

**Explanation:**
* `s1` and `s2` are in the same bucket (hashCode = 100)
* When `map.get()` is called, Java first finds the bucket, then applies `equals()` to elements in that bucket to select the correct object

### Summary Table

| Situation | What Happens | Result |
|-----------|--------------|--------|
| Different `hashCode` | Different buckets | Fast access ✅ |
| Same `hashCode`, `equals == false` | Same bucket, different cells | Performance degrades ⚠️ |
| Same `hashCode`, `equals == true` | Same object | Single copy stored ✅ |

---

## 6. Cloneable Interface

### What is Cloneable?

`Cloneable` is a **marker interface** (empty interface) in the `java.lang` package with no methods.

```java
public interface Cloneable { }
```

It works with the `clone()` method from `Object` class:

```java
protected Object clone() throws CloneNotSupportedException
```

### Basic Implementation

```java
class Student implements Cloneable {
    int id;
    String name;
    
    public Student(int id, String name) {
        this.id = id;
        this.name = name;
    }
    
    @Override
    public Object clone() throws CloneNotSupportedException {
        return super.clone(); // Calls Object.clone() method
    }
    
    @Override
    public String toString() {
        return "Student{id=" + id + ", name='" + name + "'}";
    }
}

public class Main {
    public static void main(String[] args) {
        try {
            Student s1 = new Student(1, "Ayşe");
            Student s2 = (Student) s1.clone(); // Create a copy
            
            System.out.println("Original: " + s1);
            System.out.println("Clone:    " + s2);
            System.out.println("Same object? " + (s1 == s2)); // false → different objects
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
    }
}
```

**Output:**
```
Original: Student{id=1, name='Ayşe'}
Clone:    Student{id=1, name='Ayşe'}
Same object? false
```

**Note:**
* `s2` is a completely separate object but its content is the same as `s1`
* The `==` operator returns false because they're at different memory addresses

---

## 7. Shallow vs Deep Copy

| Type | Description | Example |
|------|-------------|---------|
| 🩶 Shallow Copy | Only copies first-level fields. If the object contains other objects, it copies their references. | `super.clone()` (default) |
| 🩵 Deep Copy | Also copies all nested objects (completely independent copy). | Written manually or with custom `clone()` code |

### Deep Copy Example

```java
class Address implements Cloneable {
    String city;
    
    public Address(String city) { 
        this.city = city; 
    }
    
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

class Student implements Cloneable {
    int id;
    String name;
    Address address;
    
    public Student(int id, String name, Address address) {
        this.id = id; 
        this.name = name; 
        this.address = address;
    }
    
    @Override
    protected Object clone() throws CloneNotSupportedException {
        Student cloned = (Student) super.clone();
        cloned.address = (Address) address.clone(); // Deep copy
        return cloned;
    }
}

public class Main {
    public static void main(String[] args) throws CloneNotSupportedException {
        Address addr = new Address("Ankara");
        Student s1 = new Student(1, "Ayşe", addr);
        Student s2 = (Student) s1.clone();
        
        s2.address.city = "İzmir"; // Only changing s2's address
        
        System.out.println(s1.address.city); // Ankara
        System.out.println(s2.address.city); // İzmir
    }
}
```

**Explanation:** Now `s1` and `s2` are completely independent copies. Changing `s2`'s address doesn't affect `s1`.

---

## Summary

* **Comparable**: Enables natural ordering of custom objects
* **hashCode/equals**: Essential for hash-based collections; proper implementation ensures correctness and performance
* **Cloneable**: Allows object duplication; be mindful of shallow vs deep copy requirements
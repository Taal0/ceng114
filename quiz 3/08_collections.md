# 08 — Collections Framework

> **Mantık:** Java'da hazır veri yapıları. Hangisini kullanacağını **task'ın gereksinimine göre** seç: indexli mi? sırası önemli mi? tekrar olmamalı mı? key-value mı?

---

## Hızlı Karar Ağacı

```
İhtiyacın ne?
├── Sıralı liste (indexle erişim)
│    ├── Sık erişim, az ekleme/silme  → ArrayList
│    └── Sık ekleme/silme (özellikle ortadan/baştan) → LinkedList
├── Tekrarsız küme
│    ├── Sıra umurunda değil → HashSet
│    └── Otomatik sıralı   → TreeSet
├── Key → Value
│    ├── Sıra umurunda değil → HashMap
│    └── Key'e göre sıralı  → TreeMap
├── LIFO yapısı  → Stack
└── FIFO yapısı  → LinkedList (Queue olarak) veya ArrayDeque
```

---

## 1) List Interface

### ArrayList (en yaygın)

```java
import java.util.ArrayList;
import java.util.List;

List<String> list = new ArrayList<>();
list.add("Ali");
list.add("Veli");
list.add(0, "Ahmet");          // başa ekle

list.get(1);                   // "Ali"
list.set(1, "Mehmet");         // değiştir
list.remove(0);                // index ile sil
list.remove("Veli");           // değer ile sil
list.contains("Mehmet");       // true
list.size();                   // 1
list.isEmpty();
list.clear();

for (String s : list) {        // foreach
    System.out.println(s);
}
```

### LinkedList

```java
import java.util.LinkedList;

LinkedList<Integer> ll = new LinkedList<>();
ll.add(1);
ll.addFirst(0);
ll.addLast(2);
ll.removeFirst();
ll.removeLast();
ll.peek();         // ilk eleman
```

> **ArrayList vs LinkedList:**
> - `ArrayList.get(i)` → O(1), `LinkedList.get(i)` → O(n)
> - `LinkedList.addFirst()` → O(1), `ArrayList.add(0, x)` → O(n)

---

## 2) Set Interface

### HashSet

```java
import java.util.HashSet;
import java.util.Set;

Set<String> set = new HashSet<>();
set.add("Java");
set.add("Python");
set.add("Java");           // tekrar eklenmez

set.contains("Java");      // true
set.remove("Python");
set.size();                // 1

for (String s : set) System.out.println(s);  // sıra GARANTİ DEĞİL
```

### TreeSet (otomatik sıralı)

```java
import java.util.TreeSet;

TreeSet<Integer> ts = new TreeSet<>();
ts.add(5); ts.add(1); ts.add(3);

for (int x : ts) System.out.println(x);  // 1, 3, 5
ts.first();    // 1
ts.last();     // 5
```

### LinkedHashSet (eklenme sırası korunur)

```java
LinkedHashSet<String> lhs = new LinkedHashSet<>();
lhs.add("c"); lhs.add("a"); lhs.add("b");
// iteration: c, a, b (eklenme sırası)
```

---

## 3) Map Interface

### HashMap

```java
import java.util.HashMap;
import java.util.Map;

Map<String, Integer> ages = new HashMap<>();
ages.put("Ali", 25);
ages.put("Veli", 30);
ages.put("Ali", 26);             // override

ages.get("Ali");                 // 26
ages.containsKey("Veli");        // true
ages.containsValue(30);          // true
ages.remove("Veli");
ages.size();

// Iteration
for (Map.Entry<String, Integer> e : ages.entrySet()) {
    System.out.println(e.getKey() + " → " + e.getValue());
}

for (String k : ages.keySet())   { ... }
for (int v : ages.values())      { ... }
```

### TreeMap (key'e göre sıralı)

```java
TreeMap<String, Integer> tm = new TreeMap<>();
tm.put("banana", 2);
tm.put("apple", 1);
// iteration: apple, banana (alfabetik)
tm.firstKey();
tm.lastKey();
```

---

## 4) Stack (LIFO)

```java
import java.util.Stack;

Stack<Integer> s = new Stack<>();
s.push(1);
s.push(2);
s.push(3);

s.peek();   // 3 (en üst)
s.pop();    // 3 (çıkar ve döndür)
s.peek();   // 2
s.empty();  // false
s.search(1); // pozisyon (1-indexli, üstten)
```

> **Modern alternatif:** `ArrayDeque` (daha hızlı). Quiz'de `Stack` yeter.

---

## 5) Queue (FIFO)

```java
import java.util.Queue;
import java.util.LinkedList;

Queue<String> q = new LinkedList<>();
q.offer("a");   // ekle (sona)
q.offer("b");
q.offer("c");

q.peek();       // "a" (ön, çıkarmadan bak)
q.poll();       // "a" (ön, çıkar ve döndür)
q.size();       // 2
```

> `add` vs `offer`: dolduğunda `add` exception, `offer` `false` döner.
> `remove` vs `poll`: boşken `remove` exception, `poll` `null` döner.

---

## 6) Iterator

```java
List<String> list = new ArrayList<>(List.of("a", "b", "c"));
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    String s = it.next();
    if (s.equals("b")) it.remove();   // güvenli silme
}
```

> Foreach içinde `list.remove()` ÇALIŞMAZ → `ConcurrentModificationException`. `Iterator.remove()` kullan.

---

## 7) Collections Utility Class

Static metodlar:

```java
import java.util.Collections;

List<Integer> list = new ArrayList<>(List.of(3, 1, 4, 1, 5));

Collections.sort(list);                     // [1, 1, 3, 4, 5]
Collections.sort(list, Collections.reverseOrder()); // azalan
Collections.reverse(list);
Collections.shuffle(list);
Collections.max(list);
Collections.min(list);
Collections.frequency(list, 1);             // 1 kaç kere geçiyor
```

---

## 8) Comparable vs Comparator

### Comparable — sınıfın **doğal sıralaması**

```java
public class Student implements Comparable<Student> {
    String name;
    int gpa;

    @Override
    public int compareTo(Student other) {
        return Integer.compare(this.gpa, other.gpa);
    }
}

Collections.sort(students); // gpa'ya göre artan
```

### Comparator — **dışarıdan** kural ver

```java
List<Student> students = ...;

Collections.sort(students, (a, b) -> a.name.compareTo(b.name));
// veya
Collections.sort(students, Comparator.comparing(s -> s.name));
Collections.sort(students, Comparator.comparingInt(s -> -s.gpa)); // azalan
```

---

## Tam Örnek: Kelime Sayma (klasik quiz sorusu)

```java
import java.util.*;
import java.io.*;

public class WordCount {
    public static void main(String[] args) throws Exception {
        Map<String, Integer> count = new TreeMap<>();

        try (Scanner in = new Scanner(new File("text.txt"))) {
            while (in.hasNext()) {
                String w = in.next().toLowerCase();
                count.put(w, count.getOrDefault(w, 0) + 1);
            }
        }

        for (Map.Entry<String, Integer> e : count.entrySet()) {
            System.out.println(e.getKey() + ": " + e.getValue());
        }
    }
}
```

---

## Sık Yapılan Hatalar

1. **`==` ile karşılaştırma** — String/Object için `equals` kullan.
2. **`HashMap`'te sıra beklemek** — yok. `LinkedHashMap` veya `TreeMap` kullan.
3. **`Set`'te aynı obj'i eklemek**, `equals`/`hashCode` override etmemişsin → tekrar eder.
4. **Foreach içinde `remove`** → `ConcurrentModificationException`.
5. **`List.of(...)` immutable** — `add` çağırırsan `UnsupportedOperationException`. `new ArrayList<>(List.of(...))` ile sarmala.
6. **`int` yerine `Integer`** — generic'lerde primitive yok.

---

## Quiz'de Sorulabilecek

1. "Bir `ArrayList<Integer>` oluştur, kullanıcıdan 10 sayı al, **çift olanları** yeni listeye al, yazdır."
2. "`HashMap<String, Integer>` ile bir metindeki kelimelerin **frekansını** bul."
3. "`TreeSet` kullanarak kullanıcının girdiği kelimeleri **sıralı tekrarsız** yazdır."
4. "Generic `Stack` kullanarak verilen string'i **ters çevir**."
5. "Bir `Queue` ile **kuyruk simülasyonu** yap (gelen müşteri, işlenen müşteri)."

### Quiz şablonu — frekans:

```java
import java.util.*;

public class Frequency {
    public static void main(String[] args) {
        String[] words = {"java", "python", "java", "c++", "java", "python"};

        Map<String, Integer> freq = new HashMap<>();
        for (String w : words) {
            freq.put(w, freq.getOrDefault(w, 0) + 1);
        }

        for (Map.Entry<String, Integer> e : freq.entrySet()) {
            System.out.println(e.getKey() + ": " + e.getValue());
        }
    }
}
```

# 03 — Generics, Wildcards & Collections

> **Kaynaklar:** `slides/CENG114_Week12_Generics_Wildcards (1).md` · `github/Generics-Extras/four_restrictions_of_generic_types.md` · `labs-md/Lab9_Solutions (1)/LabGuide9_Week13.md`

---

## 1. Neden Bu Konu Var?

Generic'ler olmadan `ArrayList` sadece `Object` tutardı, her okumada cast gerekirdi ve `ClassCastException` yalnızca çalışma zamanında ortaya çıkardı. Generic'ler bu hataları **derleme zamanına** çeker.

---

## 2. Anahtar Kavramlar

### 2.1 Generic Sınıf

```java
public class Box<T> {
    private T value;
    public Box(T value)  { this.value = value; }
    public T getValue()  { return value; }
    @Override
    public String toString() { return "Box[" + value + "]"; }
}

Box<Integer> intBox = new Box<>(42);
Box<String>  strBox = new Box<>("hello");
```

### 2.2 Generic Metot

```java
public static <T extends Comparable<T>> T max(T a, T b) {
    return a.compareTo(b) >= 0 ? a : b;
}

// Çağrı — tip çıkarımı otomatik
System.out.println(max(3, 7));          // 7
System.out.println(max("apple", "mango")); // mango
```

### 2.3 Generic Arayüz

```java
public interface Pair<A, B> {
    A first();
    B second();
}

public class OrderedPair<A, B> implements Pair<A, B> {
    private final A a;
    private final B b;
    public OrderedPair(A a, B b) { this.a = a; this.b = b; }
    public A first()  { return a; }
    public B second() { return b; }
}
```

### 2.4 Type Erasure — 4 Kısıtlama

Generic tip bilgisi **çalışma zamanında silinir** (type erasure). Bunun 4 sonucu:

| # | Ne Yapılamaz | Neden |
|---|-------------|-------|
| 1 | `new T()` | Hangi `T`'nin constructor'u çağrılacak bilinmez |
| 2 | `new T[10]` | Generic dizi oluşturulamaz |
| 3 | `instanceof T` | Çalışma zamanında T bilgisi yok |
| 4 | `static T field` | Statik alanlar sınıfın tüm örnekleriyle paylaşılır, T belirsiz |

```java
// HEPSI DERLEME HATASI:
class Bag<T> {
    T item = new T();           // ✗ 1
    T[] arr = new T[5];         // ✗ 2
    boolean test(Object o) { return o instanceof T; } // ✗ 3
    static T sharedItem;        // ✗ 4
}
```

---

## 3. Wildcards & PECS

### 3.1 Üç Wildcard

| Syntax | Anlam | Ne Zaman |
|--------|-------|----------|
| `<?>` | Herhangi bir tip | Sadece yapısal işlem (size, print) |
| `<? extends T>` | T veya T'nin alt tipi | **Üretici** (okuyorsun) |
| `<? super T>` | T veya T'nin üst tipi | **Tüketici** (yazıyorsun) |

### 3.2 PECS — Producer Extends, Consumer Super

```
Producer → Extends  (listeden okuyorsun)
Consumer → Super    (listeye yazıyorsun)
```

```java
// PRODUCER — listeden okuma, ? extends Number
static double sumList(List<? extends Number> numbers) {
    double total = 0;
    for (Number n : numbers) total += n.doubleValue();
    return total;
}
// List<Integer>, List<Double>, List<Float> hepsini kabul eder!
sumList(List.of(1, 2, 3));
sumList(List.of(1.5, 2.5));

// CONSUMER — listeye yazma, ? super Integer
static void addIntegers(List<? super Integer> dest, int... vals) {
    for (int v : vals) dest.add(v);
}
List<Number> numList = new ArrayList<>();
List<Object> objList = new ArrayList<>();
addIntegers(numList, 1, 2, 3);  // List<Number> → Integer'ın süperi
addIntegers(objList, 4, 5, 6);  // List<Object> → Integer'ın süperi
```

### 3.3 Neden `List<Integer>` → `List<Number>` değil?

```java
List<Integer> ints = new ArrayList<>();
// List<Number> nums = ints;  // DERLEME HATASI!
// Çünkü: List<Number>'a Double eklenebilir ama ints sadece Integer tutabilir

// Çözüm: wildcard kullan
List<? extends Number> nums = ints;  // OK ama nums üzerine add() yapılamaz
```

---

## 4. Collections API Özeti

### 4.1 ArrayList

```java
List<String> list = new ArrayList<>();
list.add("a");           // sona ekle
list.add(0, "z");        // başa ekle
list.get(0);             // "z"
list.set(0, "x");        // 0. elemanı değiştir
list.remove(0);          // indeksle sil
list.remove("a");        // değerle sil (ilk bulduğu)
list.size();             // eleman sayısı
list.contains("a");      // boolean
Collections.sort(list);  // sırala
```

### 4.2 HashMap

```java
Map<String, Integer> map = new HashMap<>();
map.put("alice", 90);
map.get("alice");          // 90
map.getOrDefault("bob", 0); // 0 (bulunamazsa default)
map.containsKey("alice");  // true
map.remove("alice");
for (Map.Entry<String,Integer> e : map.entrySet()) {
    System.out.println(e.getKey() + " → " + e.getValue());
}
```

### 4.3 HashSet

```java
Set<String> set = new HashSet<>();
set.add("a");
set.add("a");     // tekrar ekleme — sette yalnızca bir "a" olur
set.contains("a"); // true
set.size();        // 1
```

---

## 5. Boilerplate — Generic Stack

```java
import java.util.ArrayList;
import java.util.EmptyStackException;

public class GenericStack<T> {
    private ArrayList<T> data = new ArrayList<>();

    public void push(T item) { data.add(item); }

    public T pop() {
        if (isEmpty()) throw new EmptyStackException();
        return data.remove(data.size() - 1);
    }

    public T peek() {
        if (isEmpty()) throw new EmptyStackException();
        return data.get(data.size() - 1);
    }

    public boolean isEmpty() { return data.isEmpty(); }
    public int size() { return data.size(); }

    @Override
    public String toString() { return data.toString(); }
}
```

---

## 6. Sık Tuzaklar

| Tuzak | Açıklama |
|-------|----------|
| `List<? extends Number>`'a `add()` | Derlenmiyor — tip güvenliği |
| `List<? super Integer>`'dan okuma | `Object` döner — type-safe değil |
| `ArrayList<int>` | Primitive type kullanılamaz — `ArrayList<Integer>` |
| Raw type: `List list = ...` | Unchecked warning + ClassCastException riski |
| `new T()` yazmak | Type erasure nedeniyle derlenmez |

---

## 7. Sınavda Nasıl Sorulur?

- "Generic LinkedList / Stack / Queue sınıfı yaz" → Mock Exam 1 – Q1
- "Bu yardımcı metot hangi wildcard almalı?" → PECS sorusu
- "Bu kod neden derlenmez?" → type erasure kısıtlamaları
- Lab 9'un yapısı: `NumberFilter (? extends)`, `DataProcessor (? super)`, `CollectionUtils (?)`

---

## 8. Mini Self-Check

1. `static <T> void copy(List<? extends T> src, List<? super T> dst)` metoduna `copy(intList, numList)` çağrısı geçerli mi?
2. `List<?>` üzerine `add(null)` yapılabilir mi?
3. Neden `ArrayList<Integer>` → `ArrayList<Number>` atama derlenmez?

<details>
<summary>Cevaplar</summary>

1. Evet — `intList` Integer üretir (extends T=Integer OK), `numList` Integer tüketir (super T=Integer → Number ⊇ Integer, OK).
2. Evet — `null` her tipe atanabildiğinden tek istisnadır.
3. `ArrayList<Number>`'a `Double` eklenebilir ama `ArrayList<Integer>` yalnızca `Integer` tutabilir. Bu çelişki derleme hatası olarak engellenir.

</details>

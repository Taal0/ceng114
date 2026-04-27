# 07 — Generics

> **Mantık:** Class veya method'a **tip parametresi** vererek aynı kodu farklı tiplerle çalıştırma. **Compile-time** tip kontrolü sağlar (cast'a gerek kalmaz, runtime hatası önlenir).

---

## Neden Generics?

```java
// JDK 1.5 ÖNCESİ (kötü)
ArrayList list = new ArrayList();
list.add("Java");
String s = (String) list.get(0);   // cast gerekli, hata riski

// JDK 1.5 SONRASI (iyi)
ArrayList<String> list = new ArrayList<>();
list.add("Java");
String s = list.get(0);            // cast yok, tip güvenli
```

**Faydalar:**
1. **Compile-time tip kontrolü** (runtime hatası yerine compile error).
2. **Cast gerekmez** → daha okunaklı.
3. **Tek kodla** birden fazla tip.

---

## 1) Generic Class

```java
public class Box<T> {
    private T value;

    public void set(T value) { this.value = value; }
    public T get() { return value; }
}

// Kullanım
Box<String> sb = new Box<>();
sb.set("Hello");
String s = sb.get();

Box<Integer> ib = new Box<>();
ib.set(42);
int n = ib.get();
```

`<T>` herhangi bir tip placeholder'ıdır. Konvansiyon:
- `T` — type
- `E` — element
- `K`, `V` — key, value
- `N` — number

---

## 2) Generic Class — Birden Fazla Parametre

```java
public class Pair<K, V> {
    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey()   { return key; }
    public V getValue() { return value; }
}

// Kullanım
Pair<String, Integer> p = new Pair<>("Ali", 25);
```

---

## 3) Generic Method

```java
public class Util {
    // <E> dönüş tipinden ÖNCE gelir
    public static <E> void print(E[] array) {
        for (E item : array) {
            System.out.println(item);
        }
    }

    public static <E> E firstElement(E[] array) {
        return array[0];
    }
}

// Kullanım
Integer[] ints = {1, 2, 3};
Util.<Integer>print(ints);   // tip açıkça
Util.print(ints);            // veya çıkarım yapsın
```

---

## 4) Bounded Generic Type (`extends`)

Tip parametresine **kısıt** koymak:

```java
// E sadece Number ve alt sınıfları olabilir
public static <E extends Number> double sum(E[] array) {
    double total = 0;
    for (E n : array) {
        total += n.doubleValue();
    }
    return total;
}

// Comparable olmak ŞART
public static <E extends Comparable<E>> E max(E[] a) {
    E m = a[0];
    for (int i = 1; i < a.length; i++) {
        if (a[i].compareTo(m) > 0) m = a[i];
    }
    return m;
}
```

**Sözdizimi notları:**
- Class için de aynı: `class SortedList<E extends Comparable<E>> { ... }`
- `extends` hem class hem interface için kullanılır generics'te.

---

## 5) Wildcards (`?`)

Wildcard = "**bilmediğim/önemsiz tip**" anlamına gelir.

### Üç türü var:

```java
List<?>              // unbounded: herhangi bir tip
List<? extends Number> // upper bound: Number veya alt sınıfı
List<? super Integer>  // lower bound: Integer veya üst sınıfı
```

### Örnek:

```java
public static void print(List<?> list) {
    for (Object o : list) System.out.println(o);
}

public static double sumNumbers(List<? extends Number> list) {
    double sum = 0;
    for (Number n : list) sum += n.doubleValue();
    return sum;
}

// Kullanım
List<Integer> ints = List.of(1, 2, 3);
List<Double>  dbls = List.of(1.1, 2.2);
sumNumbers(ints);   // OK
sumNumbers(dbls);   // OK
```

### PECS kuralı (kolay hatırlamak için)
- **P**roducer **E**xtends — sadece **okuyacaksan**: `? extends T`
- **C**onsumer **S**uper — sadece **yazacaksan**: `? super T`

```java
List<? extends Number> nums = ...; // okunabilir, eklenmez
nums.add(5);  // ❌ derleme hatası
Number n = nums.get(0);  // ✓

List<? super Integer> ints = ...;  // Integer eklenebilir
ints.add(5);   // ✓
Object o = ints.get(0);  // sadece Object olarak okunur
```

---

## 6) Tipik Generic Sort Method (Liang stili)

```java
public static <E extends Comparable<E>> void sort(E[] list) {
    for (int i = 0; i < list.length - 1; i++) {
        E currentMin = list[i];
        int currentMinIndex = i;

        for (int j = i + 1; j < list.length; j++) {
            if (currentMin.compareTo(list[j]) > 0) {
                currentMin = list[j];
                currentMinIndex = j;
            }
        }

        if (currentMinIndex != i) {
            list[currentMinIndex] = list[i];
            list[i] = currentMin;
        }
    }
}

// Kullanım
Integer[] nums = {5, 2, 4, 1};
sort(nums);

String[] words = {"banana", "apple", "cherry"};
sort(words);
```

---

## 7) Generic Stack Örneği (klasik soru)

```java
public class GenericStack<E> {
    private java.util.ArrayList<E> list = new java.util.ArrayList<>();

    public void push(E o)    { list.add(o); }
    public E pop()           { return list.remove(list.size() - 1); }
    public E peek()          { return list.get(list.size() - 1); }
    public int getSize()     { return list.size(); }
    public boolean isEmpty() { return list.isEmpty(); }
}

// Kullanım
GenericStack<String> s = new GenericStack<>();
s.push("a"); s.push("b");
System.out.println(s.pop()); // b
```

---

## Type Erasure (kısaca)

Generics **runtime'da silinir**. Compile sonrası `List<String>` aslında `List` olur.
- `new T()` ❌ yapılmaz
- `T.class` ❌ kullanılmaz
- `T[] arr = new T[10];` ❌ yapılmaz (workaround: `Object[]` cast)

---

## Sık Yapılan Hatalar

1. **Diamond `<>` unutmak (eski stil):** `new ArrayList<String>()` modern, `new ArrayList()` warning verir.
2. **Generic method'da `<E>`'yi return tipinden önce koymamak.**
3. **`<? extends T>` listeye ekleme yapmaya çalışmak** (PECS!).
4. **Primitive tip kullanmak** — `List<int>` ❌, `List<Integer>` ✓.
5. **`E.class` yazmak** — type erasure'dan dolayı imkansız.

---

## Quiz'de Sorulabilecek

1. "Generic `Box<T>` sınıfı yaz: `set`, `get` metodları olsun."
2. "Generic `<E extends Comparable<E>> E max(E a, E b)` method'u yaz."
3. "Generic `<E> void swap(E[] a, int i, int j)` yaz."
4. "Generic `Pair<K, V>` sınıfı yaz."
5. "`<E extends Comparable<E>>` ile generic insertion sort yaz."

### Quiz şablonu — generic max:

```java
public class Util {
    public static <E extends Comparable<E>> E max(E[] a) {
        E maxVal = a[0];
        for (int i = 1; i < a.length; i++) {
            if (a[i].compareTo(maxVal) > 0) {
                maxVal = a[i];
            }
        }
        return maxVal;
    }

    public static void main(String[] args) {
        Integer[] nums = {3, 7, 1, 9, 4};
        System.out.println(max(nums)); // 9

        String[] words = {"banana", "apple", "cherry"};
        System.out.println(max(words)); // cherry
    }
}
```

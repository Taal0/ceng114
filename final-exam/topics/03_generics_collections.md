# 03 — Generics, Wildcards & Collections

> **Öncelik:** HIGH · **Tahmini soru sayısı:** ~3  
> **Kaynak:** `slides/CENG114_Week12_Generics_Wildcards (1).md`

---

## 1. Generic Sınıf, Metot, Arayüz

### 1.1 Generic Sınıf

```java
public class Box<T> {
    private T value;
    public Box(T value)  { this.value = value; }
    public T getValue()  { return value; }
}

Box<Integer> intBox = new Box<>(42);
Box<String>  strBox = new Box<>("hello");
// Box<int> strBox = new Box<>(5);  // HATA: primitive type kullanılamaz
```

### 1.2 Generic Metot

```java
public static <T extends Comparable<T>> T max(T a, T b) {
    return a.compareTo(b) >= 0 ? a : b;
}

System.out.println(max(3, 7));            // 7
System.out.println(max("apple", "mango")); // mango
```

### 1.3 Generic Arayüz

```java
public interface Pair<A, B> {
    A first();
    B second();
}
```

---

## 2. Type Erasure — 4 Kısıtlama

Generic tip bilgisi **derleme sonrası silinir** (`type erasure`). Bu yüzden 4 şey yapılamaz:

| # | Ne yapılamaz | Neden |
|---|--------------|-------|
| 1 | `new T()` | Hangi constructor'ın çağrılacağı bilinemez |
| 2 | `new T[10]` | Generic dizi oluşturulamaz |
| 3 | `o instanceof T` | Çalışma zamanında T bilgisi yok |
| 4 | `static T field` | Statik alan tüm örneklerle paylaşılır, T belirsiz |

```java
// BUNLARIN HEPSI DERLEME HATASI:
class Bag<T> {
    T item = new T();                       // ✗ kural 1
    T[] arr = new T[5];                     // ✗ kural 2
    boolean test(Object o) { return o instanceof T; } // ✗ kural 3
    static T shared;                        // ✗ kural 4
}
```

---

## 3. Wildcards & PECS

### 3.1 Üç Wildcard

| Syntax | Anlam | Kullanım |
|--------|-------|----------|
| `<?>` | Herhangi bir tip | Sadece yapısal işlem (size, print) |
| `<? extends T>` | T veya T'nin alt tipi | **Producer (Üretici)** — listeden okuma |
| `<? super T>` | T veya T'nin üst tipi | **Consumer (Tüketici)** — listeye yazma |

### 3.2 PECS Kuralı

```
Producer → Extends  (listeden okuyorsun)
Consumer → Super    (listeye yazıyorsun)
```

```java
// PRODUCER — okuma için ? extends Number
static double sum(List<? extends Number> list) {
    double total = 0;
    for (Number n : list) total += n.doubleValue();
    return total;
    // list.add(1.5);  // DERLEME HATASI — extends listesine yazılamaz
}
sum(List.of(1, 2, 3));      // List<Integer> geçerli
sum(List.of(1.5, 2.5));     // List<Double> geçerli

// CONSUMER — yazma için ? super Integer
static void fill(List<? super Integer> dest, int... vals) {
    for (int v : vals) dest.add(v);
    // Number n = dest.get(0);  // DERLEME HATASI — Object dışında okuma yapılamaz
}
List<Number> numList = new ArrayList<>();
fill(numList, 1, 2, 3);   // Number ⊇ Integer — geçerli
```

### 3.3 Neden `List<Integer>` → `List<Number>` DEĞİL?

```java
List<Integer> ints = new ArrayList<>();
// List<Number> nums = ints;   // DERLEME HATASI — Generics invariant!
// Neden? List<Number>'a Double eklenebilir ama ints sadece Integer tutabilir.

// Çözüm: wildcard
List<? extends Number> nums = ints;   // OK — ama add() yapılamaz
```

---

## 4. Wildcard Okuma / Yazma Özeti

| Wildcard | Okuma | Yazma |
|----------|-------|-------|
| `<? extends Number>` | `Number` olarak okuyabilir | `add()` yapılamaz (compile error) |
| `<? super Integer>` | Yalnızca `Object` olarak okuyabilir | `Integer` ve alt tipleri yazılabilir |
| `<?>` | Yalnızca `Object` | Yalnızca `null` |

---

## 5. Collections API Özeti

### ArrayList

```java
List<String> list = new ArrayList<>();
list.add("b");            // sona ekle
list.add(0, "a");         // başa ekle
list.get(0);              // "a" — O(1)
list.set(1, "z");         // değiştir
list.remove(0);           // indeksle sil
list.remove("z");         // değerle sil
list.size();              // eleman sayısı
Collections.sort(list);   // doğal sıralama
```

### HashMap

```java
Map<String, Integer> map = new HashMap<>();
map.put("ali", 90);
map.get("ali");               // 90
map.getOrDefault("veli", 0);  // 0 (bulunamazsa default)
map.containsKey("ali");       // true
for (Map.Entry<String, Integer> e : map.entrySet())
    System.out.println(e.getKey() + "=" + e.getValue());
```

---

## 6. Exceptions & Hatalar

| Durum | Sonuç |
|-------|-------|
| `List<? extends T>`'ye `add()` | Compile error |
| `List<? super T>`'den `T` olarak okuma | Compile error (Object olarak okunabilir) |
| `ArrayList<int>` | Compile error — primitive type yasak; `ArrayList<Integer>` kullan |
| `new T()` veya `new T[n]` | Compile error — type erasure |
| Raw type: `List list = ...` | Unchecked warning + `ClassCastException` riski çalışma zamanında |

---

## 7. Sınav Tuzakları (MC)

| # | Tuzak | Açıklama |
|---|-------|----------|
| 1 | **`List<Integer>` ≠ `List<Number>`** | Generics invariant; atama compile error. `<? extends Number>` ile çöz |
| 2 | **PECS yönü** | "Okuyorum → `extends`"; "Yazıyorum → `super`". Ters yazılırsa compile error |
| 3 | **`extends` listesine `add()`** | `List<? extends Number>`'a herhangi bir `add()` → compile error |
| 4 | **`super` listesinden okuma** | `List<? super Integer>`'dan `Integer` döndürme → compile error; sadece `Object` geliyor |
| 5 | **`new T()` yazmak** | Type erasure nedeniyle compile error — sınavda "neden derlenmez?" sorusu |
| 6 | **`ArrayList<int>`** | Primitive yasak; `ArrayList<Integer>` olmalı |

---

## 8. Self-Check

1. `static <T> void copy(List<? extends T> src, List<? super T> dst)` — `copy(intList, numList)` geçerli mi?
2. `List<?>`'ye `add(null)` yapılabilir mi?
3. Neden `ArrayList<Integer>` → `ArrayList<Number>` ataması derlenmez?
4. `new T()` neden compile error verir?

<details>
<summary>Cevaplar</summary>

1. Evet — `intList` Integer üretir (extends T=Integer OK), `numList` Integer tüketir (super T=Integer, Number ⊇ Integer OK).
2. Evet — `null` her tipe atanabilir; tek istisnadır.
3. `ArrayList<Number>`'a `Double` eklenebilir ama `ArrayList<Integer>` yalnızca `Integer` tutabilir — bu çelişki compile error'a yol açar.
4. Type erasure — runtime'da `T`'nin hangi sınıf olduğu bilinmez, dolayısıyla constructor çağrılamaz.

</details>

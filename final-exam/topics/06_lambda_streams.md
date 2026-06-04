# 06 — Lambda Expressions & Streams

> **Öncelik:** HIGH · **Tahmini soru sayısı:** ~3  
> **Kaynak:** `github/Lambda/CENG114_Java_Lambda_Expressions.md` · `slides/Copy of 15slide_accessible.md`

---

## 1. Functional Interface

Tam olarak **bir abstract metodu** olan interface. `default` ve `static` metotlar sayılmaz.

```java
@FunctionalInterface
interface Transformer<T, R> {
    R transform(T input);   // tek abstract metot — lambda ile kullanılabilir
}

Transformer<String, Integer> len = s -> s.length();
System.out.println(len.transform("hello"));   // 5
```

---

## 2. Standart Functional Interface'ler

| Interface | Metot | Kullanım |
|-----------|-------|----------|
| `Runnable` | `void run()` | Parametre yok, dönüş yok |
| `Supplier<T>` | `T get()` | Fabrika / lazy getter |
| `Consumer<T>` | `void accept(T t)` | `forEach` eylemi |
| `Function<T,R>` | `R apply(T t)` | `map` dönüşümü |
| `Predicate<T>` | `boolean test(T t)` | `filter` koşulu |
| `BiFunction<T,U,R>` | `R apply(T t, U u)` | İki girişli dönüşüm |
| `Comparator<T>` | `int compare(T a, T b)` | Sıralama |

```java
Predicate<String>       isLong  = s -> s.length() > 5;
Function<String, Integer> len   = String::length;
Consumer<String>        printer  = System.out::println;
Supplier<String>        greeting = () -> "Merhaba!";

isLong.test("lambda");    // true
len.apply("hello");       // 5
printer.accept("world");  // world
greeting.get();           // Merhaba!
```

---

## 3. Lambda Sözdizimi

```java
// Çok parametre, blok gövde
(String name, int age) -> { return name + age; }

// Tip çıkarımı, tek ifade (return gizli)
(name, age) -> name.length() + age

// Tek parametre (parantez opsiyonel)
name -> name.toUpperCase()

// Parametre yok
() -> System.out.println("Merhaba!")
```

---

## 4. Effectively Final — Variable Capture

```java
int multiplier = 3;   // effectively final — sonradan değiştirilmiyor

Function<Integer, Integer> f = x -> x * multiplier;   // OK

// multiplier = 5;  // bu satır eklenirse lambda satırı DERLEME HATASI verir
```

> Lambda, **instance alanlarına** (`this.field`) erişebilir — buradaki `this` lambda'yı çevreleyen sınıftır.

---

## 5. Method Reference — 4 Tür

| Tür | Sözdizimi | Lambda karşılığı |
|-----|-----------|-----------------|
| Statik metot | `ClassName::staticMethod` | `x -> ClassName.staticMethod(x)` |
| Belirli nesne | `obj::method` | `x -> obj.method(x)` |
| Keyfi nesne (aynı tip) | `ClassName::instanceMethod` | `x -> x.method()` |
| Constructor | `ClassName::new` | `x -> new ClassName(x)` |

```java
List<String> words = List.of("banana", "apple", "cherry");
words.stream()
     .map(String::toUpperCase)       // keyfi nesne
     .forEach(System.out::println);  // belirli nesne
```

---

## 6. Stream Pipeline

```
kaynak.stream()
  → intermediate op (lazy — sonuç üretmez)
  → ...
  → terminal op  (eager — stream'i tüketir, sonucu üretir)
```

### 6.1 Intermediate Operations (lazy)

```java
.filter(n -> n % 2 == 0)          // Predicate ile filtrele
.map(n -> n * n)                   // Function ile dönüştür
.sorted()                          // doğal sıralama (Comparable zorunlu)
.sorted(Comparator.reverseOrder()) // ters sıralama
.distinct()                        // tekrarları kaldır
.limit(5)                          // en fazla 5 eleman
.skip(2)                           // ilk 2'yi atla
```

### 6.2 Terminal Operations (eager)

```java
.collect(Collectors.toList())       // List<T>
.collect(Collectors.toSet())        // Set<T>
.collect(Collectors.joining(", "))  // String birleştir
.collect(Collectors.groupingBy(f))  // Map<K, List<T>>
.forEach(System.out::println)       // her elemana eylem
.count()                            // long
.findFirst()                        // Optional<T>
.anyMatch(p)  / .allMatch(p) / .noneMatch(p)  // boolean
.min(cmp)    / .max(cmp)           // Optional<T>
.reduce(0, Integer::sum)           // katlama (fold)
```

### 6.3 Tam Örnek

```java
List<String> names = List.of("Alice", "Bob", "Charlie", "Ana", "Amy");

// 'A' ile başlayanları uzunluğa göre sırala, büyük harfe çevir
List<String> result = names.stream()
    .filter(n -> n.startsWith("A"))
    .sorted(Comparator.comparingInt(String::length))
    .map(String::toUpperCase)
    .collect(Collectors.toList());
// [AMY, ANA, ALICE]

// Gruplama
Map<Integer, List<String>> byLen = names.stream()
    .collect(Collectors.groupingBy(String::length));
// {3=[Bob, Ana, Amy], 5=[Alice], 7=[Charlie]}
```

---

## 7. Optional

`findFirst()`, `min()`, `max()` gibi operasyonlar `Optional<T>` döndürür:

```java
Optional<String> first = names.stream()
    .filter(n -> n.startsWith("Z"))
    .findFirst();

first.isPresent();        // false
first.orElse("Yok");      // "Yok"
first.get();              // NoSuchElementException — boşsa fırlar!
```

---

## 8. Comparator Zincirleme

```java
students.sort(
    Comparator.comparing(Student::getGrade)  // önce nota göre
              .reversed()                     // büyükten küçüğe
              .thenComparing(Student::getName) // eşit notlarda ada göre
);
```

---

## 9. Exceptions & Hatalar

| Exception | Ne Zaman |
|-----------|----------|
| `IllegalStateException` | Bir stream ikinci kez terminal op ile kullanılmaya çalışıldığında |
| `NoSuchElementException` | Boş `Optional`'da `.get()` çağrısı |
| Compile error | Lambda içinde non-effectively-final değişken kullanımı |
| Compile error | `forEach` / `map` içine checked exception fırlatan lambda yazmak |

---

## 10. Sınav Tuzakları (MC)

| # | Tuzak | Açıklama |
|---|-------|----------|
| 1 | **Lambda + non-final değişken** | Lambda içinde kullanılan değişken mutlaka effectively final olmalı; mutasyon → compile error |
| 2 | **Stream tek tüketim** | İki terminal op aynı stream üzerinde → `IllegalStateException` |
| 3 | **`sorted()` → `Comparable` zorunlu** | Argümansız `sorted()` natural order kullanır; öğeler `Comparable` implemente etmeli |
| 4 | **`Optional.get()` boşsa** | `NoSuchElementException` fırlar; `.orElse()` güvenli alternatif |
| 5 | **Lazy intermediate ops** | `filter`, `map` vb. tek başına hiçbir şey yapmaz; terminal op olmadan çalışmaz |
| 6 | **`Collectors.joining()`** | `Stream<String>` üzerinde çalışır; `Stream<Integer>` üzerinde doğrudan kullanılamaz |

---

## 11. Self-Check

1. `[1,2,3,4,5]` üzerinde `.filter(x -> x > 3).map(x -> x * 2).collect(toList())` sonucu?
2. Lambda içinde yakalanan değişken neden effectively final olmak zorunda?
3. `Predicate<T> p1 = x -> x > 0; Predicate<T> p2 = x -> x < 10; p1.and(p2).test(5)` ne döndürür?
4. Aynı stream'i iki kez kullanmaya çalışırsak ne olur?

<details>
<summary>Cevaplar</summary>

1. `[8, 10]` — filter: {4,5}, map: {8,10}.
2. Lambda değişkenin anlık değerini yakalar; sonradan değiştirilirse yakalanan değer tutarsız hale gelir. Java bunu compile error ile engeller.
3. `true` — hem 5 > 0 hem de 5 < 10 sağlandı.
4. `IllegalStateException: stream has already been operated upon or closed`.

</details>

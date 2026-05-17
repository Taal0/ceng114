# 06 — Lambda Expressions & Streams

> **Kaynaklar:** `github/Lambda/CENG114_Java_Lambda_Expressions.md` · `slides/Copy of 15slide_accessible.md` (Ch. 15)

---

## 1. Neden Bu Konu Var?

Lambda ifadeleri, anonim iç sınıfların yerine geçen kompakt bir sözdizimi sunar. Stream API ise koleksiyonlar üzerinde `filter → map → collect` zincirleri kurarak fonksiyonel veri işleme sağlar. Modern Java'nın en sık kullanılan özelliğidir.

---

## 2. Functional Interface

Yalnızca **bir tane abstract metodu** olan interface:

```java
@FunctionalInterface
public interface Greeting {
    String greet(String name);     // tek abstract metot
    // default ve static metotlar olabilir
}
```

Standart kütüphanedeki önemli functional interface'ler:

| Interface | Metot | Açıklama |
|-----------|-------|----------|
| `Runnable` | `void run()` | Parametre yok, dönüş yok |
| `Supplier<T>` | `T get()` | Parametre yok, T döndürür |
| `Consumer<T>` | `void accept(T t)` | T alır, dönüş yok |
| `Function<T,R>` | `R apply(T t)` | T alır, R döndürür |
| `Predicate<T>` | `boolean test(T t)` | T alır, boolean döndürür |
| `BiFunction<T,U,R>` | `R apply(T t, U u)` | 2 parametre, R döndürür |
| `Comparator<T>` | `int compare(T a, T b)` | Karşılaştırma |

---

## 3. Lambda Sözdizimi — 4 Form

```java
// Form 1: çok satır, tipler açık
(String name, int age) -> {
    System.out.println(name + " is " + age);
    return name.length();
}

// Form 2: tek ifade, tip çıkarımı
(name, age) -> name.length() + age

// Form 3: tek parametre, parantez isteğe bağlı
name -> name.toUpperCase()

// Form 4: parametre yok
() -> System.out.println("Hello!")
```

---

## 4. Method Reference — 4 Tür

| Tür | Sözdizimi | Lambda karşılığı |
|-----|-----------|-----------------|
| Statik metot | `ClassName::staticMethod` | `x -> ClassName.staticMethod(x)` |
| Instance metot (belirli nesne) | `obj::method` | `x -> obj.method(x)` |
| Instance metot (keyfi nesne) | `ClassName::instanceMethod` | `x -> x.method()` |
| Constructor | `ClassName::new` | `x -> new ClassName(x)` |

```java
List<String> words = List.of("banana", "apple", "cherry");

words.stream()
     .map(String::toUpperCase)           // tip 3: keyfi nesne
     .forEach(System.out::println);      // tip 2: belirli nesne (System.out)
```

---

## 5. Effectively Final & Variable Capture

```java
int multiplier = 3;  // effectively final — sonradan değiştirilmez

Function<Integer, Integer> tripler = x -> x * multiplier;  // OK

// multiplier = 5;  // bunu eklersen → DERLEME HATASI
```

> Lambda içinde **instance alanlarına** `this.field` ile erişilebilir — lambda içindeki `this` lambda'yı çevreleyen sınıfa atıfta bulunur (anonim sınıftan farklı!).

---

## 6. Predicate, Function, Consumer Kullanımı

```java
import java.util.function.*;

Predicate<String>  isLong    = s -> s.length() > 5;
Function<String, Integer> len = String::length;
Consumer<String>   printer   = System.out::println;
Supplier<String>   greeting  = () -> "Hello!";

System.out.println(isLong.test("lambda"));   // true
System.out.println(len.apply("hello"));      // 5
printer.accept("world");                      // world
System.out.println(greeting.get());           // Hello!

// Predicate kombinasyonları
Predicate<String> startsWithA = s -> s.startsWith("A");
Predicate<String> longAndA = isLong.and(startsWithA);
Predicate<String> shortOrA  = isLong.negate().or(startsWithA);
```

---

## 7. Stream Pipeline

```
source.stream() → [intermediate ops] → terminal op
```

### 7.1 Kaynak

```java
List<Integer> nums = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
```

### 7.2 Intermediate Operations (lazy — sonuç üretmez)

```java
.filter(n -> n % 2 == 0)       // çift sayılar
.map(n -> n * n)                // karesini al
.sorted()                       // doğal sıralama
.sorted(Comparator.reverseOrder()) // ters sıralama
.distinct()                     // tekrarları kaldır
.limit(5)                       // en fazla 5 eleman
.skip(2)                        // ilk 2'yi atla
.peek(System.out::println)      // debug için — tüketmez
```

### 7.3 Terminal Operations (eager — stream'i tüketir)

```java
.collect(Collectors.toList())           // List<T>
.collect(Collectors.toSet())            // Set<T>
.collect(Collectors.joining(", "))      // String birleştir
.collect(Collectors.groupingBy(f))      // Map<K, List<T>>
.forEach(System.out::println)           // her elemana eylem
.count()                                // long
.findFirst()                            // Optional<T>
.anyMatch(p)                            // boolean
.allMatch(p)                            // boolean
.noneMatch(p)                           // boolean
.min(comparator)                        // Optional<T>
.max(comparator)                        // Optional<T>
.reduce(0, Integer::sum)                // toplam
.toArray()                              // Object[]
```

### 7.4 Tam Örnek

```java
import java.util.*;
import java.util.stream.*;

List<String> names = List.of("Alice", "Bob", "Charlie", "Ana", "Dave", "Amy");

// 'A' ile başlayanları uzunluklarına göre sırala, büyük harf yap, topla
List<String> result = names.stream()
    .filter(n -> n.startsWith("A"))
    .sorted(Comparator.comparingInt(String::length))
    .map(String::toUpperCase)
    .collect(Collectors.toList());

System.out.println(result);  // [AMY, ANA, ALICE]

// Toplam uzunluk
int totalLen = names.stream()
    .mapToInt(String::length)
    .sum();
System.out.println(totalLen);  // 24

// Gruplama
Map<Integer, List<String>> byLen = names.stream()
    .collect(Collectors.groupingBy(String::length));
System.out.println(byLen);  // {3=[Bob, Ana, Amy], 5=[Alice, Dave], 7=[Charlie]}
```

---

## 8. Comparator Zincirleme

```java
List<Student> students = ...;

students.sort(
    Comparator.comparing(Student::getGrade)        // önce nota göre
              .reversed()                           // büyükten küçüğe
              .thenComparing(Student::getName)      // eşit notlarda ada göre
);
```

---

## 9. Boilerplate — Functional Interface + Callback

```java
@FunctionalInterface
interface DataProcessor<T, R> {
    R process(T input);
}

public class Pipeline {
    static <T, R> List<R> transform(List<T> items, DataProcessor<T, R> processor) {
        List<R> result = new ArrayList<>();
        for (T item : items) result.add(processor.process(item));
        return result;
    }

    public static void main(String[] args) {
        List<String> words = List.of("hello", "world", "java");

        // Lambda ile
        List<Integer> lengths = transform(words, s -> s.length());
        System.out.println(lengths);  // [5, 5, 4]

        // Method reference ile
        List<String> upper = transform(words, String::toUpperCase);
        System.out.println(upper);   // [HELLO, WORLD, JAVA]
    }
}
```

---

## 10. Sık Tuzaklar

| Tuzak | Açıklama |
|-------|----------|
| Lambda içinde non-final değişken kullanma | `effectively final` olmak zorunda |
| Stream'i iki kez terminal op ile kullanma | İkinci kullanımda `IllegalStateException` |
| `forEach` içinde exception fırlatma | Checked exception lambda içinde doğrudan fırlatılamaz |
| `map` vs `flatMap` karıştırma | `map` 1→1, `flatMap` 1→N (stream of streams düzleştirir) |
| `collect(toList())` Java 16 öncesi | `Collectors.toList()` gerekir; Java 16+ `toList()` doğrudan |

---

## 11. Sınavda Nasıl Sorulur?

- "Bu listeyi stream ile filtrele, sırala, topla" → Mock Exam 1 – Q2, Mock Exam 2 – Q2
- "Kendi @FunctionalInterface'ini yaz ve lambda ile kullan"
- "Şu anonim sınıfı lambda'ya dönüştür"
- "groupingBy ile rapor üret" → `Collectors.groupingBy`

---

## 12. Mini Self-Check

1. `stream().filter(x -> x > 3).map(x -> x * 2).collect(toList())` — `[1,2,3,4,5]` için sonuç?
2. Lambda içinde yakalanan (captured) değişken neden `effectively final` olmak zorunda?
3. `Predicate<T> p1 = x -> x > 0; Predicate<T> p2 = x -> x < 10; p1.and(p2).test(5)` ne döndürür?

<details>
<summary>Cevaplar</summary>

1. `[8, 10]` — filter: {4,5}, map: {8,10}
2. Lambda, değişkenin anlık değerini yakalar; değişken sonradan değişirse yakalanan değer geçersiz hale gelir. Java bunu derleyici hatası olarak engeller.
3. `true` — her iki predicate de sağlandı (5 > 0 ve 5 < 10).

</details>

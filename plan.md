# 🚀 CENG114 Midterm — Bu Gece Çalışma Planı

> **Sınav:** Yarın sabah (06.04.2026) | **Kapsam:** Syllabus Hafta 1-8 → Slayt Ch9, Ch10, Ch11  
> **Kitap:** Liang, 13th Ed. | **Midterm ağırlık:** %20

---

## Çalışma Sırası (Öncelik → Düşüğe)

### 🔴 1. Chapter 11 — Inheritance & Polymorphism ⏱️ ~3 saat

**En çok soru buradan gelir. Dynamic binding ve casting tuzaklarına özellikle dikkat.**

**Sırasıyla çalış:**

1. **`extends` ile subclass oluşturma** — superclass-subclass ilişkisi, `is-a` mantığı
2. **`super()` ve constructor chaining** ⚠️
   - `super()` her zaman constructor'ın **ilk satırı** olmalı
   - Yazmazsan Java otomatik `super()` (no-arg) çağırır → superclass'ta no-arg yoksa **COMPILE ERROR**
   - Constructor zincirleme sırası: en üstteki superclass'tan başlar, alta iner
3. **Method overriding vs overloading** ⚠️
   - Override: aynı imza, subclass'ta yeniden tanım → `@Override`
   - Overload: aynı isim, **farklı parametre listesi**
4. **Dynamic binding (çalışma zamanı method seçimi)** ⚠️⚠️⚠️
   - Derleyici → **referans tipine** bakar (compile-time)
   - JVM → **gerçek nesne tipine** bakar (runtime)
   - `GeometricObject g = new Circle();` → `g.toString()` → **Circle'ınki** çalışır
5. **Upcasting / Downcasting + `instanceof`** ⚠️⚠️
   - Upcasting otomatik: `Object o = new Circle();` ✅
   - Downcasting açık: `Circle c = (Circle) o;` → `instanceof` ile kontrol et, yoksa `ClassCastException`
6. **`equals()` ve `toString()` override** — Object sınıfından
7. **`ArrayList<E>`** — add, get, remove, size, nesne dizisi yerine kullanım
8. **`protected` modifier** — subclass'tan erişilebilir, dışarıdan erişilemez
9. **`final`** — `final class` = extend edilemez, `final method` = override edilemez

---

### 🟠 2. Chapter 9 — Objects & Classes ⏱️ ~2 saat

**Temeli biliyorsun ama tuzak sorular reference ve static'ten gelir.**

1. **Class tanımlama, constructor** — no-arg vs parametreli, `this()` ile zincirleme
2. **Reference vs Primitive farkı** ⚠️⚠️
   - Primitive: `int x = 5;` → değerin kendisi
   - Reference: `Circle c = new Circle();` → heap'teki nesnenin adresi
   - `null` atanabilir, `==` adres karşılaştırır
3. **`this` keyword** — mevcut nesneye referans, field-parametre isim çakışmasını çözer
4. **`static` vs instance** ⚠️⚠️
   - `static` → sınıfa ait, `ClassName.method()` ile çağrılır, `this` kullanılamaz
   - `static` method instance field'a **erişemez**
   - `static` field tüm nesneler tarafından **paylaşılır**
5. **Pass by value (of reference)** ⚠️⚠️
   - Java'da **her şey** pass-by-value
   - Nesne geçince referansın **kopyası** geçer → field değişikliği **kalıcı**, referans değişikliği **kalıcı değil**
6. **Encapsulation** — `private` field + `get/set` method
7. **Immutable class** — tüm field'lar `private final`, setter yok, mutable nesnelerin kopyasını döndür
8. **Default değerler** — `int=0`, `double=0.0`, `boolean=false`, `String/Object=null`

---

### 🟡 3. Chapter 10 — Thinking in Objects ⏱️ ~1.5 saat

1. **Wrapper Classes** (Integer, Double, Character vb.)
   - **Autoboxing:** `Integer x = 5;` (otomatik `int` → `Integer`)
   - **Unboxing:** `int y = x;` (otomatik `Integer` → `int`)
2. **String tuzakları** ⚠️⚠️⚠️
   - `"hello" == "hello"` → **true** (String pool)
   - `new String("hello") == "hello"` → **false** (heap vs pool)
   - **Her zaman `.equals()` kullan**
   - String **immutable** — `concat()`, `substring()` yeni nesne döndürür
3. **`StringBuilder`** — mutable string, `append()`, `insert()`, `delete()` → döngüde string birleştirmede kullan
4. **`BigInteger` / `BigDecimal`** — `add()`, `multiply()` method'ları, `+` operatörü **çalışmaz**
5. **Composition / Aggregation** — "has-a" ilişkisi, `Course` içinde `Student[]` gibi

---

### 🟢 4. Abstract Classes & Interfaces (Ch13 konuları) ⏱️ ~1 saat

> Syllabus Hafta 7. Slayt 11'den sonra ama syllabus kapsamında. **Bilmenin zararı olmaz.**

Repo'daki `Chapter-13-Extension` klasörüne bak:

1. **`abstract class` vs `interface` farkı** ⚠️ (kesin sorulur)
   - Abstract: `extends`, tek kalıtım, constructor **olabilir**, field **olabilir**
   - Interface: `implements`, çoklu kalıtım, constructor **yok**, sadece `public abstract` method (Java 8+ default method var)
2. **`Comparable<T>`** — `compareTo()` method'u, `Collections.sort()` için
3. **`hashCode()` ve `equals()`** — HashSet/HashMap için şart, ikisi birlikte override edilmeli
4. **`Cloneable`** — marker interface, `clone()`, **shallow vs deep copy** farkı

---

### 🔵 5. Design Principles & Metrics (Hafta 8) ⏱️ ~30 dk

Repo'daki `OOP-Metrics-Examples` klasörüne bak:

1. **LCOM** (Lack of Cohesion of Methods) — düşük = iyi, yüksek = sınıf çok fazla iş yapıyor
2. **WMC** (Weighted Methods per Class) — toplam method karmaşıklığı
3. **RFC** (Response for a Class) — sınıfın çağırdığı toplam method sayısı
4. **Composition vs Inheritance** — "Prefer composition over inheritance" prensibi

---

## 🎯 Hocanın Detay Sorma Stili İçin Hazırlık

| Soru Tipi | Nereden Gelir | Hazırlık |
|---|---|---|
| "Çıktı ne olur?" | Dynamic binding, constructor chaining, String pool | Kodu kafanda çalıştır |
| "Compile error mı, runtime error mı?" | Downcasting, `super()` eksikliği, static→this | Kuralları ezberle |
| "Fark nedir?" | Override vs Overload, `==` vs `equals()`, abstract vs interface | Karşılaştırma tablosunu bilir |
| UML diyagramı çiz/oku | Inheritance ok yönü (alt→üst), composition (♦) | Basit şekilleri bil |

> [!TIP]
> **Son 1 saat:** Sadece ⚠️ işaretli konuları tekrar et. Bunlar hocanın en sevdiği tuzak noktaları.

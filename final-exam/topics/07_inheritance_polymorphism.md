# 07 — Inheritance & Polymorphism

> **Öncelik:** MED · **Tahmini soru sayısı:** ~2  
> **Kaynak:** `slides/Copy of 11slide_accessible.md` (Liang Ch. 11)

---

## 1. Temel Kavramlar

| Terim | Açıklama |
|-------|----------|
| **Superclass** | Kalıtım veren üst sınıf (parent class) |
| **Subclass** | Kalıtım alan alt sınıf (child class) |
| **`extends`** | Bir sınıftan miras almak — bir sınıf yalnızca **bir** sınıfı extends edebilir |
| **`super()`** | Üst sınıfın constructor'ını çağır — subclass constructor'ının **ilk satırı** olmalı |
| **Override** | Alt sınıfta üst sınıfın metodunu yeniden tanımlamak — aynı imza |
| **Overload** | Aynı sınıfta farklı parametre listesiyle aynı adda metot yazmak |
| **Dynamic dispatch** | Çağrılan metodun **runtime** tipine göre belirlenmesi (polymorphism) |
| **Upcast** | Alt sınıf nesnesini üst sınıf referansıyla tutmak — otomatik |
| **Downcast** | Üst sınıf referansını alt sınıfa cast etmek — açıkça yazılmalı |

---

## 2. Inheritance Sözdizimi

```java
public class Animal {
    private String name;

    public Animal(String name) { this.name = name; }

    public String getName() { return name; }

    public String sound() { return "..."; }

    @Override
    public String toString() { return "Animal[" + name + "]"; }
}

public class Dog extends Animal {
    private String breed;

    public Dog(String name, String breed) {
        super(name);          // üst sınıf constructor'ı ÖNCE çağrılmalı
        this.breed = breed;
    }

    @Override
    public String sound() { return "Hav!"; }  // override

    public String getBreed() { return breed; }
}
```

---

## 3. Polymorphism & Dynamic Dispatch

```java
Animal a1 = new Animal("Genel");
Animal a2 = new Dog("Karabaş", "Kangal");   // upcast — otomatik

System.out.println(a1.sound());   // "..."    — Animal.sound()
System.out.println(a2.sound());   // "Hav!"   — Dog.sound() çağrıldı!
// Derleme zamanı tipi Animal, ama runtime tipi Dog → Dog.sound() çalışır
```

> **Dynamic dispatch:** Metot hangi sınıfa ait olduğu **runtime'da** belirlenir, referans tipine göre değil.  
> **Static type (compile-time):** referansın tipi → derleyici bunu bilir.  
> **Dynamic type (runtime):** nesnenin gerçek tipi → JVM bunu kullanır.

---

## 4. Override vs Overload

| | Override | Overload |
|-|----------|----------|
| Nerede | Alt sınıfta | Aynı sınıfta |
| İmza | **Aynı** (aynı parametre listesi) | **Farklı** parametre listesi |
| Dönüş tipi | Aynı veya covariant | Farklı olabilir |
| Annotation | `@Override` (önerilir) | Yok |
| Hangi seçilir | Runtime tipine göre (dynamic) | Compile-time'da belirlenir |

```java
class Printer {
    void print(String s) { System.out.println(s); }           // overload 1
    void print(int n) { System.out.println(n); }              // overload 2
    void print(String s, int n) { System.out.println(s+n); }  // overload 3
}
```

---

## 5. Constructor Chaining

```java
// new Faculty() çağrısında çalışma sırası:
// 1. Person() → 2. Employee(String) → 3. Employee() → 4. Faculty()

class Person {
    Person() { System.out.println("1. Person"); }
}
class Employee extends Person {
    Employee() {
        this("overloaded");           // önce Employee(String) çağrılır
        System.out.println("3. Employee no-arg");
    }
    Employee(String s) { System.out.println("2. " + s); }
}
class Faculty extends Employee {
    Faculty() { System.out.println("4. Faculty"); }
}
// new Faculty() çıktısı: 1. Person  2. overloaded  3. Employee no-arg  4. Faculty
```

> Constructor'ın ilk satırında `super()` veya `this()` yoksa derleyici otomatik `super()` ekler.

---

## 6. Casting & instanceof

```java
Animal a = new Dog("Rex", "Labrador");   // upcast — otomatik

// Downcast — Dog metodlarına erişmek için
if (a instanceof Dog) {
    Dog d = (Dog) a;          // güvenli downcast
    System.out.println(d.getBreed());  // Labrador
}

// Güvensiz downcast
Animal cat = new Animal("Kedi");
Dog d2 = (Dog) cat;     // ClassCastException! (runtime'da)
```

---

## 7. `==` vs `.equals()` — Strings

```java
String s1 = new String("hello");
String s2 = new String("hello");

s1 == s2;          // false — farklı referanslar (heap'te iki nesne)
s1.equals(s2);     // true  — içerik aynı
```

> String karşılaştırmalarında **her zaman `.equals()`** kullan. `==` referansı karşılaştırır.

---

## 8. `final` Modifier

```java
final class Immutable { }          // extend edilemez
// class Sub extends Immutable {}  // COMPILE ERROR

class Base {
    final void locked() { }        // override edilemez
}
class Child extends Base {
    // void locked() { }           // COMPILE ERROR
}
```

---

## 9. `protected` Erişim

```java
class Parent {
    protected int x = 10;   // alt sınıflar ve aynı paket erişebilir
}
class Child extends Parent {
    void show() { System.out.println(x); }  // OK — protected miras alındı
}
```

---

## 10. Exceptions & Hatalar

| Exception | Ne Zaman |
|-----------|----------|
| `ClassCastException` | Uyumsuz downcast: `(Dog) new Animal(...)` |
| Compile error | Override metodunda daha kısıtlayıcı erişim belirleyici (`public` → `private`) |
| Compile error | `super()` constructor çağrısı ilk satırda değilse |

---

## 11. Sınav Tuzakları (MC)

| # | Tuzak | Açıklama |
|---|-------|----------|
| 1 | **Dynamic dispatch** | `Animal a = new Dog(...)` → `a.sound()` çağrısı Dog.sound()'u çalıştırır |
| 2 | **`==` vs `.equals()`** | `==` referansı karşılaştırır; `"hello" == "hello"` farklı nesneler için `false` |
| 3 | **ClassCastException** | Uyumsuz downcast compile'da değil runtime'da patlar |
| 4 | **Constructor sırası** | `new Faculty()` → önce en üst superclass, sonra aşağı iner |
| 5 | **`super()` zorunluluğu** | Üst sınıfta no-arg constructor yoksa alt sınıf açıkça `super(args)` çağırmalı; yoksa compile error |
| 6 | **Override ≠ Overload** | Overload compile-time'da seçilir; override runtime'da |

---

## 12. Self-Check

1. `Animal a = new Dog("Rex","Lab"); a.sound()` ne yazdırır — Animal.sound() mu Dog.sound() mu?
2. `s1 == s2` ile `s1.equals(s2)` arasındaki fark nedir?
3. Downcast neden runtime'da `ClassCastException` fırlayabilir?
4. `new Faculty()` çağrısında constructor'lar hangi sırayla çalışır?

<details>
<summary>Cevaplar</summary>

1. **Dog.sound()** — runtime tipi Dog, dynamic dispatch Dog'un metodunu çağırır.
2. `==` heap'teki referans adreslerini karşılaştırır; `equals()` içerik değerini. String'ler için `equals()` kullan.
3. Referans tipi üst sınıf olsa bile nesne gerçekte başka bir alt sınıf olabilir; runtime bu tür uyumsuzluğu tespit eder.
4. En üstten en alta: Person → Employee(String) → Employee() → Faculty.

</details>

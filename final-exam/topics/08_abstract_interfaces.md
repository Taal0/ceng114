# 08 — Abstract Classes & Interfaces

> **Öncelik:** MED · **Tahmini soru sayısı:** ~2  
> **Kaynak:** `slides/Copy of 13slide_accessible.md` (Liang Ch. 13)

---

## 1. Abstract Class

### 1.1 Temel Kurallar

```java
public abstract class Shape {
    private String color;

    // Constructor olabilir — new ile oluşturulamaz ama super() ile çağrılabilir
    public Shape(String color) { this.color = color; }

    // Abstract metot — gövde yok, alt sınıf implemente ETMEK ZORUNDA
    public abstract double getArea();
    public abstract double getPerimeter();

    // Concrete metot — alt sınıflar aynen kullanabilir veya override edebilir
    public String getColor() { return color; }

    @Override
    public String toString() {
        return getClass().getSimpleName() + "[color=" + color +
               ", area=" + String.format("%.2f", getArea()) + "]";
    }
}
```

### 1.2 Alt Sınıf

```java
public class Circle extends Shape {
    private double radius;

    public Circle(double radius, String color) {
        super(color);          // abstract sınıfın constructor'ı super() ile çağrılır
        this.radius = radius;
    }

    @Override
    public double getArea()      { return Math.PI * radius * radius; }

    @Override
    public double getPerimeter() { return 2 * Math.PI * radius; }
}
```

### 1.3 Özet: Abstract Class Kuralları

| Kural | Açıklama |
|-------|----------|
| `new AbstractClass()` | **YASAK** — compile error |
| Constructor | **OLABİLİR** — `super()` ile çağrılır |
| Abstract metot olmak zorunda değil | Abstract class sıfır abstract metot içerebilir |
| Alt sınıf tüm abstract metotları implemente etmeli | Etmezse alt sınıf da `abstract` olmalı |
| Tip olarak kullanılabilir | `Shape s = new Circle(...)` geçerli |
| `abstract` + `static` veya `private` | **YASAK** — anlamlı değil |

---

## 2. Interface

### 2.1 Tanım

```java
public interface Drawable {
    // Alanlar: implicitly public static final
    double PI = 3.14159;              // → public static final double PI = 3.14159

    // Metotlar: implicitly public abstract
    void draw();                      // → public abstract void draw()
    double area();

    // Java 8+: default metot — implementasyon var, override edilebilir
    default String describe() {
        return "Drawable object, area=" + area();
    }

    // Java 8+: static metot — interface üzerinden çağrılır
    static void printInfo() {
        System.out.println("Drawable interface");
    }
}
```

### 2.2 Implements

```java
public class Rectangle extends Shape implements Drawable, Comparable<Rectangle> {
    private double width, height;

    public Rectangle(double w, double h) {
        super("black");
        this.width = w; this.height = h;
    }

    @Override public double getArea()      { return width * height; }
    @Override public double getPerimeter() { return 2 * (width + height); }
    @Override public void draw()           { System.out.println("Rectangle çiziliyor"); }

    @Override
    public int compareTo(Rectangle other) {
        return Double.compare(this.getArea(), other.getArea());
    }
}
```

### 2.3 Interface Kuralları

| Kural | Açıklama |
|-------|----------|
| `implements` ile çoklu | Bir sınıf **birden fazla** interface implement edebilir |
| `extends` tek | Bir sınıf yalnızca **bir** sınıfı extends edebilir |
| `new Interface()` | **YASAK** — compile error |
| Tip olarak kullanılabilir | `Drawable d = new Rectangle(...)` geçerli |
| Tüm abstract metotlar implemente edilmeli | Edilmezse sınıf `abstract` olmalı |
| Field gizli modifiers | `public static final` (ister yaz ister yazma) |
| Metot gizli modifiers | `public abstract` (ister yaz ister yazma) |

---

## 3. Abstract Class vs Interface Karşılaştırması

| | Abstract Class | Interface |
|-|----------------|-----------|
| `new` ile nesne | Hayır | Hayır |
| Constructor | **Evet** | Hayır |
| Miras | `extends` — tek | `implements` — çoklu |
| Field durumu | Normal instance alanlar | `public static final` |
| Metot durumu | Abstract + concrete karışık | Abstract + default + static |
| Ne zaman kullan | "is-a" ilişkisi + shared state | "can-do" yeteneği, çoklu miras |

---

## 4. `Comparable` Interface

```java
public class Student implements Comparable<Student> {
    private String name;
    private double gpa;

    public Student(String name, double gpa) { this.name = name; this.gpa = gpa; }

    @Override
    public int compareTo(Student other) {
        return Double.compare(this.gpa, other.gpa);  // artan GPA
        // negatif → this < other
        // 0       → this == other
        // pozitif → this > other
    }
}

List<Student> list = new ArrayList<>();
list.add(new Student("Ali", 3.2));
list.add(new Student("Ayşe", 3.8));
Collections.sort(list);   // Comparable kullanır
```

---

## 5. `Comparator` Interface (lambda ile)

```java
// Lambda ile Comparator — ada göre sırala
Comparator<Student> byName = (a, b) -> a.name.compareTo(b.name);
list.sort(byName);

// Method reference
list.sort(Comparator.comparing(s -> s.name));
```

---

## 6. Exceptions & Hatalar

| Durum | Sonuç |
|-------|-------|
| `new AbstractClass()` | Compile error |
| `new SomeInterface()` | Compile error |
| Abstract metot implemente edilmezse | Alt sınıf `abstract` olmak zorunda; değilse compile error |
| Interface alanını `= 5` dışında atama | Compile error — `public static final`, sadece tanımda değer verilebilir |
| `abstract static void method()` | Compile error — abstract static anlamsız |

---

## 7. Sınav Tuzakları (MC)

| # | Tuzak | Açıklama |
|---|-------|----------|
| 1 | **Abstract class constructor** | `new AbstractClass()` yasak, ama constructor tanımlanabilir ve `super()` ile çağrılır |
| 2 | **Interface field implicit modifier** | `int X = 5` → `public static final int X = 5`; değiştirilemez |
| 3 | **Interface method implicit modifier** | `void method()` → `public abstract void method()` |
| 4 | **Çoklu extends** | Bir sınıf birden fazla sınıfı `extends` edemez; `implements` çoklu olabilir |
| 5 | **Partial implementation** | Abstract class'ın tüm abstract metotlarını implement etmeyince alt sınıf da `abstract` olmalı |
| 6 | **`default` metot** | Interface'de gövdeli metot (Java 8+); `abstract` değil, override edilebilir |

---

## 8. Self-Check

1. Abstract class'ın constructor'ı var mı? Nasıl çağrılır?
2. Bir sınıf aynı anda kaç sınıfı `extends` edebilir, kaç interface'i `implements` edebilir?
3. Interface'deki `double PI = 3.14` satırının tam açılımı nedir?
4. Abstract sınıfın tüm abstract metotlarını implemente etmeyen bir alt sınıf ne olur?

<details>
<summary>Cevaplar</summary>

1. Evet, constructor olabilir. `new` ile çağrılamaz; yalnızca alt sınıfın constructor'ından `super(...)` ile çağrılır.
2. **extends:** yalnızca 1; **implements:** istediği kadar (çoklu interface).
3. `public static final double PI = 3.14` — implicit modifier'lar bunlar.
4. Alt sınıf da `abstract` olmak zorundadır; değilse compile error.

</details>

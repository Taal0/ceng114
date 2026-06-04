# 10 — OOP / Encapsulation

> **Öncelik:** LOW · **Tahmini soru sayısı:** ~1  
> **Kaynak:** `slides/Copy of 09slide_accessible.md` + `Copy of 10slide_accessible.md` (Liang Ch. 9–10)

---

## 1. OOP Temel Kavramları

| Terim | Açıklama |
|-------|----------|
| **Class** | Nesnelerin şablonu — data fields + methods |
| **Object** | Sınıfın örneği (instance); kendi state'ine sahip |
| **Encapsulation** | Data fields'i `private` yaparak dış erişimi getter/setter ile kontrol etmek |
| **`this`** | Metot veya constructor içinde mevcut nesneyi (instance) temsil eder |
| **Constructor** | `new` ile çağrılan, nesneyi başlatan özel metot; sınıf adıyla aynı, dönüş tipi yok |
| **Getter** | `private` alana okuma erişimi — `getX()` |
| **Setter** | `private` alana yazma erişimi — `setX(val)` |

---

## 2. Erişim Belirleyiciler (Access Modifiers)

| Modifier | Aynı Sınıf | Aynı Paket | Alt Sınıf | Her Yer |
|----------|-----------|-----------|----------|---------|
| `private` | ✓ | ✗ | ✗ | ✗ |
| (default) | ✓ | ✓ | ✗ | ✗ |
| `protected` | ✓ | ✓ | ✓ | ✗ |
| `public` | ✓ | ✓ | ✓ | ✓ |

---

## 3. Temel OOP Sınıfı

```java
public class BankAccount {
    private String owner;    // private — dışarıdan doğrudan erişilemez
    private double balance;

    // No-arg constructor
    public BankAccount() {
        this("Anonim", 0.0);   // this() — aynı sınıfın başka constructor'ını çağır
    }

    // Parametreli constructor
    public BankAccount(String owner, double balance) {
        this.owner   = owner;    // this.owner: alan; owner: parametre
        this.balance = balance;
    }

    // Getter
    public String getOwner()   { return owner; }
    public double getBalance() { return balance; }

    // Setter — doğrulama eklenebilir
    public void setOwner(String owner) {
        if (owner == null || owner.isBlank())
            throw new IllegalArgumentException("İsim boş olamaz");
        this.owner = owner;
    }

    // Business method
    public void deposit(double amount) {
        if (amount <= 0) throw new IllegalArgumentException("Pozitif miktar girin");
        balance += amount;
    }

    @Override
    public String toString() {
        return String.format("BankAccount[%s, %.2f TL]", owner, balance);
    }
}
```

---

## 4. Static vs Instance

```java
public class Counter {
    private static int total = 0;   // static — tüm nesneler paylaşır
    private int id;                 // instance — her nesneye özgü

    public Counter() {
        total++;          // static alan — sınıf adıyla erişim daha açık: Counter.total
        this.id = total;
    }

    public static int getTotal() { return total; }   // static metot
    public int getId()           { return id; }      // instance metot
}

Counter c1 = new Counter();   // total=1, id=1
Counter c2 = new Counter();   // total=2, id=2
System.out.println(Counter.getTotal());  // 2
```

> **Static metot** içinde `this` kullanılamaz — instance yok.  
> **Static field** tüm örnekler tarafından paylaşılır.

---

## 5. `this` Keyword Kullanımları

```java
public class Point {
    private int x, y;

    public Point(int x, int y) {
        this.x = x;   // 1. alan-parametre isim çakışmasını çöz
        this.y = y;
    }

    public Point() {
        this(0, 0);   // 2. başka constructor'ı çağır (ilk satır olmalı)
    }

    public Point translate(int dx, int dy) {
        this.x += dx;
        this.y += dy;
        return this;  // 3. method chaining — mevcut nesneyi döndür
    }
}
```

---

## 6. Immutable Class

```java
public final class ImmutablePoint {   // final — extend edilemez
    private final int x;              // final — değer sonradan değiştirilemez
    private final int y;

    public ImmutablePoint(int x, int y) { this.x = x; this.y = y; }

    public int getX() { return x; }   // getter var, setter yok
    public int getY() { return y; }
}
```

---

## 7. Exceptions & Hatalar

| Durum | Sonuç |
|-------|-------|
| `private` alana doğrudan dış erişim | Compile error |
| `static` metodundan `this` kullanımı | Compile error |
| `this()` ilk satır değilse | Compile error |
| Constructor'da döngüsel `this()` çağrısı | Compile error |
| `final` alanı constructor dışında atama | Compile error |

---

## 8. Sınav Tuzakları (MC)

| # | Tuzak | Açıklama |
|---|-------|----------|
| 1 | **`this` vs `this()`** | `this` → mevcut nesne; `this(...)` → aynı sınıfın başka constructor'ı |
| 2 | **Static metodda `this`** | `static` metodun instance'ı yok — `this` compile error verir |
| 3 | **Default değerler** | `int` → 0, `double` → 0.0, `boolean` → `false`, referans → `null` |
| 4 | **Constructor dönüş tipi yok** | `void BankAccount()` yazılırsa constructor değil normal metot sayılır |
| 5 | **`private` + alt sınıf** | `private` alan alt sınıfta **doğrudan** kullanılamaz; `protected` ya da getter gerekir |

---

## 9. Self-Check

1. `this.owner = owner` satırında `this.owner` ile `owner` ne fark?
2. Static metodun içinde `this.field = 5` yazılırsa ne olur?
3. `private final int x` olan bir alana constructor dışında değer atanabilir mi?
4. Default constructor ne zaman otomatik oluşturulur?

<details>
<summary>Cevaplar</summary>

1. `this.owner` → sınıfın instance alanı; `owner` → parametrenin yerel değişkeni. `this` isim çakışmasını çözer.
2. Compile error — static metot bağlı bir instance olmadığından `this` anlamsız.
3. Hayır — `final` alan yalnızca tanımlandığı yerde veya constructor içinde atanabilir.
4. Sınıfta **hiçbir** constructor yazılmamışsa derleyici otomatik no-arg constructor ekler; herhangi bir constructor yazılırsa otomatik eklenmez.

</details>

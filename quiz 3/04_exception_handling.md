# 04 — Exception Handling (İstisna Yönetimi)

> **Mantık:** Program hata verdiğinde **çökmesin**, sen kontrolü ele al. Java'da bu mekanizmaya **exception handling** denir.

---

## Exception Hiyerarşisi (Liang slide 10-13'ten)

```
Object
  └─ Throwable
       ├─ Error           (kritik, JVM seviyesi → genelde yakalanmaz)
       │   ├─ VirtualMachineError
       │   └─ LinkageError
       └─ Exception
            ├─ IOException                ← CHECKED
            ├─ ClassNotFoundException     ← CHECKED
            └─ RuntimeException           ← UNCHECKED
                 ├─ ArithmeticException        (5/0)
                 ├─ NullPointerException       (null.method())
                 ├─ IndexOutOfBoundsException  (arr[100])
                 ├─ IllegalArgumentException
                 └─ NumberFormatException      (Integer.parseInt("abc"))
```

### Checked vs Unchecked

| | Checked | Unchecked |
|---|---|---|
| Compiler zorlar mı? | **EVET** | hayır |
| Tipik | `IOException`, `FileNotFoundException` | `RuntimeException` ve alt sınıfları |
| Çözüm | `try-catch` veya `throws` |  zorunlu değil ama yakalayabilirsin |

---

## try-catch-finally

```java
try {
    // riskli kod
    int x = Integer.parseInt(s);
}
catch (NumberFormatException e) {
    // sadece bu exception için
    System.out.println("Sayı değil: " + e.getMessage());
}
catch (Exception e) {
    // diğerleri için (ama önce özel sonra genel!)
    e.printStackTrace();
}
finally {
    // her durumda çalışır (exception olsa da olmasa da)
    System.out.println("Bitti");
}
```

**Önemli kurallar:**
1. `catch` blokları **özelden genele** sıralı olmalı. Yoksa compile hatası.
2. `finally` her zaman çalışır, **`return` olsa bile**.
3. Tek `try` zorunlu, `catch` veya `finally`'den **biri** olmalı.

---

## throws — Exception'ı yukarı fırlat

Method **kendi yakalamak istemiyorsa** çağıran method'a fırlatır:

```java
public static void readFile(String path) throws IOException {
    Scanner in = new Scanner(new File(path)); // IOException atabilir
    // catch yok, çağıran ilgilensin
}

public static void main(String[] args) {
    try {
        readFile("data.txt");
    } catch (IOException e) {
        System.out.println("Dosya okunamadı");
    }
}
```

---

## throw — Exception'ı kendin fırlat

```java
public void setRadius(double r) throws IllegalArgumentException {
    if (r < 0) {
        throw new IllegalArgumentException("Radius negatif olamaz");
    }
    this.radius = r;
}
```

> `throws` (s ile): method imzasında, "fırlatabilirim" demek.
> `throw` (s yok): method gövdesinde, "şimdi fırlatıyorum" demek.

---

## Custom Exception (Kendi Exception'ın)

```java
public class InvalidAgeException extends Exception {
    public InvalidAgeException(String message) {
        super(message);
    }
}

// Kullanım
public void setAge(int age) throws InvalidAgeException {
    if (age < 0 || age > 150) {
        throw new InvalidAgeException("Yaş geçersiz: " + age);
    }
    this.age = age;
}
```

> `extends Exception` → **checked**, `extends RuntimeException` → **unchecked**.

---

## Tam Çalışan Örnek (Liang Quotient stili)

```java
import java.util.Scanner;

public class QuotientWithException {
    public static int quotient(int n1, int n2) {
        if (n2 == 0) {
            throw new ArithmeticException("Bölen sıfır olamaz");
        }
        return n1 / n2;
    }

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        System.out.print("İki sayı gir: ");
        int n1 = in.nextInt();
        int n2 = in.nextInt();

        try {
            int result = quotient(n1, n2);
            System.out.println(n1 + " / " + n2 + " = " + result);
        } catch (ArithmeticException ex) {
            System.out.println("Hata: " + ex.getMessage());
        }
        System.out.println("Program devam ediyor.");
    }
}
```

---

## Exception Object'ten Bilgi Al

```java
catch (Exception e) {
    e.getMessage();       // mesaj
    e.toString();         // tam isim + mesaj
    e.printStackTrace();  // tüm stack trace'i yazdır
}
```

---

## Sık Yapılan Hatalar

1. **Catch sıralaması ters** — önce `Exception`, sonra `ArithmeticException` → compile hatası.
2. **`throw` yerine `throws` veya tersi.**
3. **Checked exception için `throws` koymamak** → compile hatası.
4. **`finally`'de `return`** → diğer return'leri ezer (kötü pratik).
5. **Sessiz catch** — `catch (Exception e) {}` → hatayı yutar, debug imkansız hale gelir.

---

## Quiz'de Sorulabilecek

1. "Kullanıcıdan **iki sayı al**, böl, sıfıra bölünme hatasını yakala."
2. "Custom `NegativeBalanceException` yaz, bakiye negatife düşerse fırlat."
3. "`Integer.parseInt` sırasında oluşacak `NumberFormatException`'ı yakala, kullanıcıdan tekrar iste." → `while` + try-catch.
4. "`finally` bloğunun ne zaman çalıştığını gösteren bir program yaz."

### Tipik "tekrar iste" şablonu:

```java
Scanner in = new Scanner(System.in);
int n = 0;
boolean ok = false;
while (!ok) {
    try {
        System.out.print("Sayı gir: ");
        n = in.nextInt();
        ok = true;
    } catch (java.util.InputMismatchException e) {
        System.out.println("Geçersiz, tekrar dene.");
        in.next(); // hatalı input'u tüket
    }
}
```

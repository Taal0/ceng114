# 02 — Exception Handling · Text I/O · Event-Driven Programming

> **Kaynaklar:** `slides/CENG114_Week11_Exception_Handling (1).md` · `slides/Copy of 15slide_accessible.md` (Ch. 15) · `labs-md/Lab8Solutions (1)/CENG114_Lab8.md`

---

## 1. Neden Bu Konu Var?

Gerçek programlar her zaman hata üretir: kullanıcı yanlış girdi verir, dosya bulunamaz, ağ kesilir. Exception handling bu hataları **çökmeden** ele almayı sağlar. I/O ise verileri kalıcı kılmanın temel yoludur. Event-Driven programming ise GUI ve reaktif sistemlerin temelidir.

---

## 2. Anahtar Kavramlar

### 2.1 Throwable Hiyerarşisi

```
Throwable
├── Error          (JVM hataları, yakalamazsın — OutOfMemoryError vb.)
└── Exception
    ├── IOException             ← checked
    ├── SQLException            ← checked
    ├── RuntimeException        ← unchecked
    │   ├── NullPointerException
    │   ├── ArrayIndexOutOfBoundsException
    │   ├── ClassCastException
    │   └── NumberFormatException
    └── (kendi custom Exception'ların)
```

### 2.2 Checked vs. Unchecked

| | Checked | Unchecked |
|--|---------|-----------|
| Miras alır | `Exception` | `RuntimeException` |
| Derleme zorunluluğu | `catch` veya `throws` gerekir | Gerekli değil |
| Ne zaman kullanılır | Beklenen hatalar (dosya yok, parse hatası) | Programcı hatası (null erişim, index aşımı) |
| Örnek | `IOException`, `InvalidGradeException` | `NullPointerException`, `NumberFormatException` |

### 2.3 Try-Catch-Finally Yürütme Sırası

```java
try {
    // 1. kod çalışır
    riskyOperation(); // burası exception fırlatırsa...
    // 2. burası ATLENIR
} catch (IOException e) {
    // 3. uygun catch bloğu çalışır
    System.err.println("IO hatası: " + e.getMessage());
} catch (Exception e) {
    // 4. daha genel catch — SIRALAMA ÖNEMLİ (özelden genele)
} finally {
    // 5. HER DURUMDA çalışır (return olsa bile!)
    cleanup();
}
```

> **Kritik kural:** `finally` bloku, `catch` bloğu içinde `return` olsa bile çalışır.

### 2.4 Multi-Catch (Java 7+)

```java
catch (IOException | NumberFormatException e) {
    // iki tipte aynı işlem
}
```

### 2.5 throw vs throws

```java
// throws: metot imzasında, "bu metot bu hatayı fırlatabilir" bildirimi
public void readFile(String path) throws IOException {
    // throw: gerçek fırlatma
    if (path == null) throw new IllegalArgumentException("path null olamaz");
    // ...
}
```

### 2.6 Custom Exception

```java
// Checked custom exception
public class InvalidGradeException extends Exception {
    public InvalidGradeException(String message) {
        super(message);
    }
}

// Unchecked custom exception
public class InsufficientFundsException extends RuntimeException {
    private final double amount;
    public InsufficientFundsException(double amount) {
        super("Yetersiz bakiye: " + amount);
        this.amount = amount;
    }
    public double getAmount() { return amount; }
}
```

---

## 3. Text I/O — Dosya Okuma/Yazma

### 3.1 Temel Sınıflar

| Amaç | Tercih Edilen Sınıf | Alternatif |
|------|-------------------|------------|
| Dosya okuma | `BufferedReader(FileReader(...))` | `Scanner(new File(...))` |
| Dosya yazma | `PrintWriter(FileWriter(...))` | `BufferedWriter` |
| Append modu | `new FileWriter("log.txt", true)` | — |

### 3.2 Try-With-Resources (TWR)

```java
// Kaynak otomatik kapatılır — finally { reader.close(); } yazmana gerek yok
try (BufferedReader br = new BufferedReader(new FileReader("data/input.txt"))) {
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException e) {
    System.err.println("Dosya okunamadı: " + e.getMessage());
}
```

Birden fazla kaynak:
```java
try (BufferedReader br = new BufferedReader(new FileReader("input.txt"));
     PrintWriter  pw = new PrintWriter(new FileWriter("output.txt"))) {
    // br ve pw her ikisi de otomatik kapatılır
}
```

> **Neden kritik?** `FileWriter` kapatılmazsa buffer flush olmaz → dosyaya yazılan son satırlar kaybolabilir.

### 3.3 Tam I/O Boilerplate (Lab 8 tarzı)

```java
import java.io.*;

public class FileProcessorDemo {
    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(new FileReader("data/students.txt"));
             PrintWriter    valid  = new PrintWriter(new FileWriter("data/valid.txt"));
             PrintWriter    errors = new PrintWriter(new FileWriter("data/errors.log"))) {

            String line;
            int lineNum = 0, okCount = 0, errCount = 0;

            while ((line = reader.readLine()) != null) {
                lineNum++;
                String[] parts = line.split(",");
                try {
                    int grade = Integer.parseInt(parts[1].trim());
                    validateGrade(grade);          // throws InvalidGradeException
                    valid.println(line);
                    okCount++;
                } catch (NumberFormatException e) {
                    errors.printf("Line %d: Cannot parse '%s'%n", lineNum, parts[1].trim());
                    errCount++;
                } catch (InvalidGradeException e) {
                    errors.printf("Line %d: %s [%s]%n", lineNum, e.getMessage(), parts[0].trim());
                    errCount++;
                }
            }
            System.out.printf("Done. %d valid, %d errors.%n", okCount, errCount);

        } catch (IOException e) {
            System.err.println("Dosya hatası: " + e.getMessage());
        }
    }

    static void validateGrade(int g) throws InvalidGradeException {
        if (g < 0 || g > 100)
            throw new InvalidGradeException("Grade " + g + " is out of valid range (0-100).");
    }
}
```

---

## 4. Event-Driven Programming

### 4.1 Üç Temel Kavram

```
Event Source  ──(olay oluştu)──►  Event Object  ──►  Event Handler
  (Button)                       (ActionEvent)      (handler.handle(e))
```

- **Event Source:** Olayı tetikleyen bileşen (Button, TextField, ...)
- **Event Object:** Olayın detaylarını taşıyan nesne (`ActionEvent`, `MouseEvent`, ...)
- **Event Handler:** Olayı işleyen kod (`EventHandler<ActionEvent>` implementasyonu)

### 4.2 JavaFX Olmadan — Functional Interface ile Event Dispatch

```java
@FunctionalInterface
interface EventHandler<T> {
    void handle(T event);
}

class Button {
    private EventHandler<String> handler;
    void setOnClick(EventHandler<String> h) { this.handler = h; }
    void click(String msg) { if (handler != null) handler.handle(msg); }
}

public class EventDemo {
    public static void main(String[] args) {
        Button btn = new Button();

        // Lambda ile handler kaydetme
        btn.setOnClick(msg -> System.out.println("Tıklandı: " + msg));

        btn.click("OK");      // → Tıklandı: OK
        btn.click("Cancel");  // → Tıklandı: Cancel
    }
}
```

### 4.3 JavaFX Gerçek Örnek (slides/Copy of 15slide_accessible.md)

```java
Button btOK = new Button("OK");
btOK.setOnAction(e -> System.out.println("OK clicked"));
```

Anonim inner class versiyonu (eski stil):
```java
btOK.setOnAction(new EventHandler<ActionEvent>() {
    @Override
    public void handle(ActionEvent e) {
        System.out.println("OK clicked");
    }
});
```

> Lambda ile EventHandler aynı şey — `EventHandler<ActionEvent>` de `@FunctionalInterface`.

---

## 5. Sık Tuzaklar

| Tuzak | Açıklama |
|-------|----------|
| `catch (Exception e)` en başa koyma | Daha spesifik exception'lar erişilemez hale gelir → **compile error** |
| TWR'da `close()` manuel çağrısı | TWR zaten kapatır, ikinci kez çağırmak gerek yok |
| `throws` yerine `throw` | `throw` fırlatır; `throws` metot imzasında bildirim |
| Checked exception'ı catch etmeden çağırma | Derlenmiyor — `catch` bloku veya `throws` zorunlu |
| `FileWriter` append argümanı | `new FileWriter("f", true)` append; `new FileWriter("f")` üstüne yazar |
| `finally`'de return | `try`'daki return değerini ezer — kaçın |

---

## 6. Sınavda Nasıl Sorulur?

Lab 8 birebir tekrar edilebilir, farklı senaryo ile:
- "Log dosyası işleme: bozuk satırları errors.log'a yaz" → TWR + custom checked exception
- "Banka işlemleri: AccountNotFoundException, InsufficientFundsException" → Lab 8 Q3 tarzı
- "Functional Interface ile callback yazın" → Lambda + Event-Driven (Mock Exam 2 – Q2)

---

## 7. Mini Self-Check

1. `finally` bloğu ne zaman **çalışmaz**?
2. `NumberFormatException` checked mi unchecked mi?
3. Aşağıdaki kod ne yazdırır?

```java
try {
    int x = Integer.parseInt("abc");
} catch (NumberFormatException e) {
    System.out.print("A");
    return;
} finally {
    System.out.print("B");
}
System.out.print("C");
```

<details>
<summary>Cevaplar</summary>

1. `System.exit()` çağrısı veya JVM çökmesi durumunda. Normal return/exception akışında her zaman çalışır.
2. Unchecked — `RuntimeException`'dan türer.
3. `AB` — `catch` "A" yazdırır, `return` öncesi `finally` çalışır ve "B" yazdırır; "C" hiç ulaşılmaz.

</details>

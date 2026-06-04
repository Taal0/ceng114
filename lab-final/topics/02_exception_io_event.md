# 02 — Exception Handling · Text I/O · Event-Driven Programming

> **Kaynaklar:** `slides/CENG114_Week11_Exception_Handling (1).md` · `slides/Copy of 15slide_accessible.md` (Ch. 15) · `labs-md/Lab8Solutions (1)/CENG114_Lab8.md`

---

## 1. Neden Bu Konu Var?

Gerçek programlar her zaman hata üretir: kullanıcı yanlış girdi verir, dosya bulunamaz, ağ kesilir. Exception handling bu hataları **çökmeden** ele almayı sağlar. I/O ise verileri kalıcı kılmanın temel yoludur. Event-Driven programming ise GUI ve reaktif sistemlerin temelidir.

---

## 2. Terim Sözlüğü

| Terim | Ne anlama gelir |
|-------|-----------------|
| **Exception** | Program çalışırken oluşan olağandışı durum; bir nesne olarak fırlatılır |
| **Throwable** | Java'da fırlatılabilen her şeyin kök sınıfı |
| **Error** | JVM seviyesinde kritik hatalar — `OutOfMemoryError`, `StackOverflowError`; yakalamaya çalışma |
| **Checked exception** | Derleyici tarafından zorunlu tutulan; `catch` veya `throws` olmadan kod derlenmez |
| **Unchecked exception** | Derleyici zorlamaz; `RuntimeException` altındaki tüm hatalar |
| **Stack trace** | Hatanın hangi sırayla hangi metodlardan geçerek ulaştığını gösteren iz — `e.printStackTrace()` |
| **Propagation** | Exception'ın yakalanmadığında çağrı yığıtında yukarı doğru "kabarcık gibi" yükselmesi |
| **Re-throw** | Bir exception'ı yakaladıktan sonra tekrar fırlatmak — genellikle logging için |
| **try-with-resources (TWR)** | `AutoCloseable` nesneleri otomatik kapatan `try` yapısı |
| **AutoCloseable** | `close()` metoduna sahip interface; TWR sadece bunu implemente edenleri yönetir |
| **Event Source** | Olayı üretecek bileşen (buton, text field, timer) |
| **Event Object** | Olayın bilgilerini (kaynak, zaman damgası) taşıyan nesne |
| **Event Handler** | Olayı alıp işleyen callback kodu |
| **Functional Interface** | Tam olarak **bir** soyut metodu olan interface; lambda ile doğrudan kullanılabilir |
| **Lambda** | Anonim fonksiyon kısayolu; `(parametre) -> gövde` |
| **Callback** | Dışarıdan iletilen ve belirli bir an çağrılan fonksiyon referansı |

---

## 3. Anahtar Kavramlar

### 3.1 Throwable Hiyerarşisi

```
Throwable
├── Error          (JVM hataları, yakalamazsın — OutOfMemoryError, StackOverflowError vb.)
└── Exception
    ├── IOException             ← checked
    │   └── FileNotFoundException ← checked
    ├── SQLException            ← checked
    ├── RuntimeException        ← unchecked
    │   ├── NullPointerException
    │   ├── ArrayIndexOutOfBoundsException
    │   ├── ClassCastException
    │   ├── NumberFormatException
    │   └── IllegalArgumentException
    └── (kendi custom Exception'ların)
```

> **Kural:** `RuntimeException` ve onun alt sınıfları → **unchecked**. Diğer her `Exception` alt sınıfı → **checked**.

---

### 3.2 Checked vs. Unchecked

|  | Checked | Unchecked |
|--|---------|-----------|
| Miras alır | `Exception` | `RuntimeException` |
| Derleme zorunluluğu | `catch` veya `throws` gerekir | Gerekli değil |
| Ne zaman kullanılır | Beklenen, kurtarılabilir hatalar (dosya yok, parse hatası) | Programcı hatası (null erişim, index aşımı) |
| Örnek | `IOException`, `InvalidGradeException` | `NullPointerException`, `NumberFormatException` |

---

### 3.3 Try-Catch-Finally — Tam Syntax ve Yürütme Sırası

```java
try {
    // 1. Bu blok çalışır.
    riskyOperation();
    // Exception fırlatılırsa buradan sonrası ATLANIR.
} catch (FileNotFoundException e) {   // 2a. En spesifik önce!
    System.err.println("Dosya bulunamadı: " + e.getMessage());
} catch (IOException e) {             // 2b. Daha genel
    System.err.println("IO hatası: " + e.getMessage());
} catch (Exception e) {               // 2c. En genel en sona
    e.printStackTrace();              // tüm yığıt izini yazdır
} finally {
    // 3. HER DURUMDA çalışır: exception olsa da olmasa da, return olsa bile!
    cleanup();
}
```

**Yürütme senaryoları:**

| Durum | try | catch | finally |
|-------|-----|-------|---------|
| Exception yok | Tümü çalışır | Atlanır | Çalışır |
| Eşleşen exception | Exception noktasına kadar | Çalışır | Çalışır |
| Eşleşmeyen exception | Exception noktasına kadar | Atlanır | Çalışır, sonra yukarı fırlatılır |
| `System.exit()` | Durur | Durur | **ÇALIŞMAZ** |

> **Kritik kural:** `catch` bloğu içindeki `return` bile `finally`'yi tetikler.

---

### 3.4 Multi-Catch (Java 7+)

```java
// Sözdizimi: catch (TipA | TipB değişkenAdı)
try {
    process();
} catch (IOException | NumberFormatException e) {
    // İki exception aynı işlemi gerektiriyorsa tek blokta
    System.err.println("Hata: " + e.getMessage());
}
```

> **Uyarı:** Multi-catch'teki tipler birbirinin alt sınıfı olamaz — `catch (Exception | IOException e)` compile hatası verir.

---

### 3.5 throw vs throws — Fark Ne?

```java
// throws → metot imzasında bildirim: "çağıran bunu handle etmeli"
public void readFile(String path) throws IOException, InvalidGradeException {

    // throw → exception nesnesini gerçekten fırlat
    if (path == null) {
        throw new IllegalArgumentException("path null olamaz");
    }

    if (path.isEmpty()) {
        throw new IOException("Boş dosya yolu");
    }
}
```

**Özet:** `throws` söz verir, `throw` yapar.

---

### 3.6 Custom Exception — İki Kalıp

```java
// ── Checked custom exception ──────────────────────────────────────
// Exception'dan türetildi → derleme zorunluluğu var
public class InvalidGradeException extends Exception {
    private final int grade;

    public InvalidGradeException(int grade) {
        super("Geçersiz not: " + grade + ". Beklenen: 0-100 arası.");
        this.grade = grade;
    }

    public int getGrade() { return grade; }
}

// ── Unchecked custom exception ────────────────────────────────────
// RuntimeException'dan türetildi → throws bildirimi zorunlu değil
public class InsufficientFundsException extends RuntimeException {
    private final double amount;

    public InsufficientFundsException(double amount) {
        super(String.format("Yetersiz bakiye. Gereken: %.2f TL", amount));
        this.amount = amount;
    }

    public double getAmount() { return amount; }
}
```

**Kullanım:**

```java
void validateGrade(int g) throws InvalidGradeException {
    if (g < 0 || g > 100)
        throw new InvalidGradeException(g);
}

void withdraw(double amount) {
    if (amount > balance)
        throw new InsufficientFundsException(amount - balance);
    balance -= amount;
}
```

---

### 3.7 Exception Propagation (Kabarcık Davranışı)

```java
public class PropagationDemo {
    static void level3() throws IOException {
        throw new IOException("Disk dolu");      // burada fırlatıldı
    }
    static void level2() throws IOException {
        level3();                                 // yakalamazsan yukarı geçer
    }
    static void level1() {
        try {
            level2();
        } catch (IOException e) {
            System.out.println("level1 yakaladı: " + e.getMessage());
        }
    }
    public static void main(String[] args) {
        level1();  // → level1 yakaladı: Disk dolu
    }
}
```

Stack trace şunu gösterir: `main → level1 → level2 → level3` — exception `level3`'te oluştu.

---

### 3.8 Re-throw (Yeniden Fırlatma)

```java
void process() throws IOException {
    try {
        riskyOp();
    } catch (IOException e) {
        System.err.println("Log: " + e.getMessage());  // önce logla
        throw e;                                        // sonra tekrar fırlat
    }
}
```

---

## 4. Text I/O — Dosya Okuma / Yazma

### 4.1 Temel Sınıflar ve Amaçları

| Sınıf | Amaç | Notlar |
|-------|------|--------|
| `FileReader` | Karakter bazlı dosya okuma | Tamponlama yok → yavaş |
| `BufferedReader` | `FileReader`'ı sarmalar, satır satır okuma | `readLine()` metodu sağlar |
| `FileWriter` | Karakter bazlı dosya yazma | İkinci argüman `true` → append |
| `PrintWriter` | Biçimli yazma — `print`, `println`, `printf` | Yaygın tercih |
| `Scanner(File)` | Token bazlı dosya okuma | `nextInt()`, `nextLine()` |

### 4.2 Sınıf Sarmalama (Wrapping) Mantığı

```
FileReader ──sarmalanır──► BufferedReader ──satır okur──► String line
FileWriter ──sarmalanır──► PrintWriter   ──yazar───────► dosya
```

```java
// Sarmalama sözdizimi:
BufferedReader br = new BufferedReader(new FileReader("dosya.txt"));
PrintWriter    pw = new PrintWriter(new FileWriter("çıktı.txt"));
```

### 4.3 Try-With-Resources (TWR) — Tam Syntax

```java
// AutoCloseable'ı implemente eden her nesne TWR ile kullanılabilir
try (ResourceType res = new ResourceType(...)) {
    // res burada kullanılır
} catch (ExceptionType e) {
    // hata yönetimi
}
// res.close() otomatik çağrılır — finally yazmana gerek yok
```

**Çoklu kaynak:**

```java
try (BufferedReader br = new BufferedReader(new FileReader("input.txt"));
     PrintWriter    pw = new PrintWriter(new FileWriter("output.txt"))) {
    // Kapanış sırası: pw önce kapatılır, sonra br — açılış sırasının tersi
}
```

> **Neden önemli?** `FileWriter` kapatılmazsa bellek içindeki tampon (buffer) diske yazılmaz → son satırlar kaybolur.

### 4.4 `readLine()` Döngüsü Kalıbı

```java
// Klasik null-kontrollü döngü — her zaman bu kalıbı kullan
String line;
while ((line = br.readLine()) != null) {
    // line ile çalış
}
```

`readLine()` → dosya sonu geldiğinde `null` döner, satır sonunu (`\n`) stringe dahil **etmez**.

### 4.5 `PrintWriter` Yazma Metodları

```java
pw.print("Satır sonu yok");
pw.println("Satır sonu var");
pw.printf("Adı: %-10s  Notu: %3d%n", name, grade);  // biçimli
pw.flush();  // TWR kullanmıyorsan buffer'ı elle boşalt
```

### 4.6 Append Modu

```java
// İkinci argüman true → mevcut dosyanın sonuna ekle
PrintWriter pw = new PrintWriter(new FileWriter("log.txt", true));
```

---

## 5. Senaryo Kodları

### Senaryo A — Öğrenci Notu İşleme (Lab 8 tarzı)

**Giriş dosyası (`students.txt`):**
```
Alice,95
Bob,abc
Carol,110
Dave,78
```

**Hedef:** Geçerli satırları `valid.txt`'e, hatalıları `errors.log`'a yaz.

```java
import java.io.*;

public class GradeProcessor {

    public static void main(String[] args) {
        String inputPath  = "data/students.txt";
        String validPath  = "data/valid.txt";
        String errorPath  = "data/errors.log";

        try (BufferedReader reader = new BufferedReader(new FileReader(inputPath));
             PrintWriter    valid  = new PrintWriter(new FileWriter(validPath));
             PrintWriter    errors = new PrintWriter(new FileWriter(errorPath))) {

            String line;
            int lineNum = 0, okCount = 0, errCount = 0;

            while ((line = reader.readLine()) != null) {
                lineNum++;
                String[] parts = line.split(",");

                if (parts.length != 2) {
                    errors.printf("Satır %d: Beklenen format 'Ad,Not' → '%s'%n", lineNum, line);
                    errCount++;
                    continue;
                }

                String name     = parts[0].trim();
                String gradeStr = parts[1].trim();

                try {
                    int grade = Integer.parseInt(gradeStr);     // NumberFormatException?
                    validateGrade(name, grade);                 // InvalidGradeException?
                    valid.printf("%s,%d%n", name, grade);
                    okCount++;
                } catch (NumberFormatException e) {
                    errors.printf("Satır %d: '%s' sayıya dönüştürülemedi%n", lineNum, gradeStr);
                    errCount++;
                } catch (InvalidGradeException e) {
                    errors.printf("Satır %d: %s%n", lineNum, e.getMessage());
                    errCount++;
                }
            }

            System.out.printf("Bitti. %d geçerli, %d hatalı.%n", okCount, errCount);

        } catch (IOException e) {
            System.err.println("Dosya hatası: " + e.getMessage());
        }
    }

    static void validateGrade(String name, int grade) throws InvalidGradeException {
        if (grade < 0 || grade > 100)
            throw new InvalidGradeException(
                String.format("'%s' için not %d aralık dışı (0-100)", name, grade)
            );
    }
}
```

**Beklenen çıktı:**
- `valid.txt` → `Alice,95` ve `Dave,78`
- `errors.log` → Bob (parse hatası) + Carol (110 aralık dışı)

---

### Senaryo B — Banka Hesabı (Checked + Unchecked bir arada)

```java
public class BankAccount {
    private String owner;
    private double balance;

    public BankAccount(String owner, double initialBalance) {
        if (initialBalance < 0)
            throw new IllegalArgumentException("Başlangıç bakiyesi negatif olamaz");
        this.owner   = owner;
        this.balance = initialBalance;
    }

    public void deposit(double amount) {
        if (amount <= 0)
            throw new IllegalArgumentException("Yatırılacak miktar pozitif olmalı");
        balance += amount;
        System.out.printf("[%s] %.2f TL yatırıldı. Bakiye: %.2f TL%n", owner, amount, balance);
    }

    public void withdraw(double amount) {
        if (amount <= 0)
            throw new IllegalArgumentException("Çekilecek miktar pozitif olmalı");
        if (amount > balance)
            throw new InsufficientFundsException(amount - balance);
        balance -= amount;
        System.out.printf("[%s] %.2f TL çekildi. Bakiye: %.2f TL%n", owner, amount, balance);
    }

    public double getBalance() { return balance; }
}

// --- Kullanım ---
public class BankDemo {
    public static void main(String[] args) {
        BankAccount acc = new BankAccount("Alice", 500.0);

        try {
            acc.deposit(200.0);
            acc.withdraw(800.0);      // → InsufficientFundsException
            acc.withdraw(100.0);      // bu satır çalışmaz
        } catch (InsufficientFundsException e) {
            System.err.println("İşlem başarısız: " + e.getMessage());
            System.err.printf("Eksik: %.2f TL%n", e.getAmount());
        } catch (IllegalArgumentException e) {
            System.err.println("Geçersiz işlem: " + e.getMessage());
        }
    }
}
```

---

### Senaryo C — Log Dosyasına Append Etme

```java
import java.io.*;
import java.time.LocalDateTime;

public class Logger {

    private final String logFile;

    public Logger(String logFile) {
        this.logFile = logFile;
    }

    // throws bildirimi → çağıran handle etmeli
    public void log(String level, String message) throws IOException {
        // true → append modu
        try (PrintWriter pw = new PrintWriter(new FileWriter(logFile, true))) {
            pw.printf("[%s] [%s] %s%n", LocalDateTime.now(), level, message);
        }
    }

    public static void main(String[] args) {
        Logger logger = new Logger("app.log");
        try {
            logger.log("INFO",  "Uygulama başlatıldı");
            logger.log("WARN",  "Düşük bellek uyarısı");
            logger.log("ERROR", "Veritabanına bağlanılamadı");
        } catch (IOException e) {
            System.err.println("Log yazılamadı: " + e.getMessage());
        }
    }
}
```

---

### Senaryo D — Scanner ile Konsol Girişi + Exception Handling

```java
import java.util.InputMismatchException;
import java.util.Scanner;

public class SafeInputDemo {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int number = -1;

        while (number < 0) {
            System.out.print("Pozitif bir sayı girin: ");
            try {
                number = sc.nextInt();
                if (number < 0)
                    System.out.println("Negatif sayı kabul edilmiyor, tekrar deneyin.");
            } catch (InputMismatchException e) {
                System.out.println("Sayısal değer giriniz!");
                sc.nextLine(); // hatalı token'ı temizle
            }
        }

        System.out.println("Girilen sayı: " + number);
        sc.close();
    }
}
```

---

## 6. Event-Driven Programming

### 6.1 Üç Temel Kavram

```
Event Source  ──(olay oluştu)──►  Event Object  ──►  Event Handler
  (Button)                       (ActionEvent)      (handler.handle(e))
```

| Kavram | Tanım | JavaFX Karşılığı |
|--------|-------|-----------------|
| **Event Source** | Olayı tetikleyen bileşen | `Button`, `TextField`, `Timer` |
| **Event Object** | Olayın detaylarını (kaynak, zaman) taşıyan nesne | `ActionEvent`, `MouseEvent`, `KeyEvent` |
| **Event Handler** | Olayı işleyen callback | `EventHandler<ActionEvent>` implementasyonu |

---

### 6.2 Functional Interface ve Lambda

```java
// @FunctionalInterface → tam olarak 1 soyut metod
@FunctionalInterface
interface EventHandler<T> {
    void handle(T event);   // tek soyut metod
}
```

Lambda sözdizimi:

```
(parametre listesi) -> ifade
(parametre listesi) -> { blok }
```

Örnekler:

```java
EventHandler<String> h1 = msg -> System.out.println(msg);           // tek satır
EventHandler<String> h2 = msg -> { System.out.println(msg); };      // blok
EventHandler<String> h3 = System.out::println;                      // method reference
```

---

### 6.3 Özel Event Sistemi — Adım Adım Yapı

```java
// 1. Functional interface tanımla
@FunctionalInterface
interface EventHandler<T> {
    void handle(T event);
}

// 2. Event source: handler listesi tut, tetiklenince çağır
class Button {
    private String label;
    private EventHandler<String> clickHandler;

    public Button(String label) { this.label = label; }

    public void setOnClick(EventHandler<String> handler) {
        this.clickHandler = handler;
    }

    // Simüle: butona tıklandı
    public void simulateClick() {
        if (clickHandler != null)
            clickHandler.handle(label + " tıklandı");
    }
}

// 3. Kullan
public class EventDemo {
    public static void main(String[] args) {
        Button save   = new Button("Kaydet");
        Button cancel = new Button("İptal");

        // Lambda ile kayıt
        save.setOnClick(msg -> System.out.println("İşlem: " + msg));
        cancel.setOnClick(msg -> {
            System.out.println("İptal edildi: " + msg);
            System.out.println("Değişiklikler sıfırlandı.");
        });

        save.simulateClick();    // → İşlem: Kaydet tıklandı
        cancel.simulateClick();  // → İptal edildi: İptal tıklandı
    }
}
```

---

### 6.4 Çoklu Handler — Observer Kalıbına Giriş

```java
import java.util.ArrayList;
import java.util.List;

class EventSource {
    private List<EventHandler<String>> handlers = new ArrayList<>();

    public void addHandler(EventHandler<String> h) {
        handlers.add(h);
    }

    public void fire(String event) {
        for (EventHandler<String> h : handlers)
            h.handle(event);
    }
}

public class MultiHandlerDemo {
    public static void main(String[] args) {
        EventSource src = new EventSource();

        src.addHandler(e -> System.out.println("Handler 1: " + e));
        src.addHandler(e -> System.out.println("Handler 2: " + e));

        src.fire("veri değişti");
        // → Handler 1: veri değişti
        // → Handler 2: veri değişti
    }
}
```

---

### 6.5 JavaFX Gerçek Örnek

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class FXDemo extends Application {
    @Override
    public void start(Stage primaryStage) {
        Button    btOK     = new Button("OK");
        Button    btCancel = new Button("İptal");
        Label     label    = new Label("Bekleniyor...");

        // Lambda ile handler — en kısa yol
        btOK.setOnAction(e -> label.setText("OK tıklandı!"));

        // Anonim iç sınıf — eski stil, aynı anlama gelir
        btCancel.setOnAction(new javafx.event.EventHandler<javafx.event.ActionEvent>() {
            @Override
            public void handle(javafx.event.ActionEvent e) {
                label.setText("İptal edildi.");
            }
        });

        VBox root = new VBox(10, btOK, btCancel, label);
        primaryStage.setScene(new Scene(root, 300, 150));
        primaryStage.setTitle("Event Demo");
        primaryStage.show();
    }
}
```

> Lambda ile `EventHandler<ActionEvent>` aynı şeydir — `EventHandler` zaten `@FunctionalInterface`.

---

## 7. Sık Tuzaklar

| Tuzak | Açıklama |
|-------|----------|
| `catch (Exception e)` en başa koyma | Daha spesifik exception'lar **erişilemez** hale gelir → compile error |
| `FileNotFoundException` yerine sadece `IOException` | `FileNotFoundException`, `IOException`'ın alt sınıfıdır — genel catch yeterliyse sorun yok ama izleme güçleşir |
| TWR'da `close()` manuel çağrısı | TWR zaten kapatır; tekrar çağırmak gerek yok |
| `throws` yerine `throw` | `throw` fırlatır, `throws` metot imzasında bildirim |
| Checked exception'ı catch/throws olmadan çağırma | Derlenmiyor |
| `FileWriter` append argümanı unutmak | `new FileWriter("f")` mevcut dosyayı **sıfırlar**; `true` eklemeyi unutma |
| `finally`'de `return` | `try` bloğundaki `return` değerini ezer — kaçın |
| `sc.nextInt()` sonrası `sc.nextLine()` | `nextInt()` satır sonunu tüketmez; ardından `nextLine()` boş string döner → `sc.nextLine()` ekle |
| Multi-catch'te hiyerarşik tipler | `catch (Exception | IOException e)` → compile error; alt sınıf zaten kapsamda |

---

## 8. Sınavda Nasıl Sorulur?

Lab 8 birebir tekrar edilebilir, farklı senaryo ile:

- **"Log dosyası işleme: bozuk satırları errors.log'a yaz"** → TWR + custom checked exception + append modu
- **"Banka işlemleri: AccountNotFoundException, InsufficientFundsException"** → Lab 8 Q3 tarzı
- **"Functional Interface ile callback yazın"** → Lambda + Event-Driven (Mock Exam 2 – Q2)
- **"Aşağıdaki try-catch-finally bloğu ne yazdırır?"** → Yürütme sırası soruları (A/B/AB/ABC hangisi çıkar?)
- **"Custom exception sınıfı yazın, checked/unchecked seçin ve gerekçelendirin"** → Miras açıklaması önemli

---

## 9. Mini Self-Check

1. `finally` bloğu ne zaman **çalışmaz**?
2. `NumberFormatException` checked mi, unchecked mi?
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

4. `FileWriter("log.txt", true)` ile `FileWriter("log.txt")` arasındaki fark nedir?
5. Aşağıdaki iki catch bloğunun sırası neden önemlidir?

```java
catch (FileNotFoundException e) { ... }
catch (IOException e) { ... }
```

6. Lambda `msg -> System.out.println(msg)` ve anonim sınıf versiyonu işlevsel olarak özdeş midir?
7. TWR'da birden fazla kaynak tanımlandığında kapanış sırası ne olur?

<details>
<summary>Cevaplar</summary>

1. `System.exit()` çağrısı veya JVM çökmesi (örn. `kill -9`). Normal return/exception akışında **her zaman** çalışır.
2. **Unchecked** — `RuntimeException`'dan türer.
3. `AB` — `catch` "A" yazdırır, `return` öncesi `finally` çalışır "B" yazdırır; "C"'ye hiç ulaşılmaz.
4. `true` argümanı → dosyanın sonuna **ekler** (append). Argümansız → dosyayı **sıfırdan** yazar.
5. `FileNotFoundException`, `IOException`'ın alt sınıfıdır. Eğer `IOException` önce gelseydi, `FileNotFoundException`'ı da yakalardı ve alttaki blok hiç erişilemez olurdu → compile error.
6. **Evet** — her ikisi de `EventHandler<String>`'i implemente eder. Lambda, `@FunctionalInterface`'lerin kısayoludur.
7. **Açılışın tersi** — en son açılan kaynak ilk kapatılır (LIFO).

</details>

# 02 — Exception Handling · Text I/O · Event-Driven Programming

> **Öncelik:** HIGH · **Tahmini soru sayısı:** ~3  
> **Kaynak:** `slides/CENG114_Week11_Exception_Handling (1).md`

---

## 1. Throwable Hiyerarşisi

```
Throwable
├── Error            ← JVM seviyesi, yakalamazsın (OutOfMemoryError, StackOverflowError)
└── Exception
    ├── IOException              ← CHECKED
    │   └── FileNotFoundException ← CHECKED
    ├── SQLException             ← CHECKED
    └── RuntimeException         ← UNCHECKED
        ├── NullPointerException
        ├── ArrayIndexOutOfBoundsException
        ├── ClassCastException
        ├── NumberFormatException
        └── IllegalArgumentException
```

**Kural:** `RuntimeException` ve tüm alt sınıfları → **unchecked**. Diğer her `Exception` alt sınıfı → **checked**.

---

## 2. Checked vs. Unchecked

| | Checked | Unchecked |
|--|---------|-----------|
| Miras alır | `Exception` (RuntimeException değil) | `RuntimeException` |
| Derleme zorunluluğu | `catch` veya `throws` olmadan **derlenmez** | Gerek yok |
| Örnek | `IOException`, `FileNotFoundException` | `NullPointerException`, `NumberFormatException` |

---

## 3. try / catch / finally

```java
try {
    // 1. Çalışır
    int x = Integer.parseInt("abc");   // NumberFormatException fırlar
    System.out.println("buraya ulaşılamaz");
} catch (NumberFormatException e) {   // 2a. en spesifik ÖNCE
    System.out.print("A");
    return;                            // return bile finally'yi durduramaz!
} catch (Exception e) {               // 2b. daha genel SONRA
    System.out.print("B");
} finally {
    System.out.print("C");            // 3. HER DURUMDA çalışır
}
// Çıktı: AC   (return'e rağmen C yazdırıldı)
```

### Yürütme Senaryoları

| Durum | try | catch | finally |
|-------|-----|-------|---------|
| Exception yok | Tümü | Atlanır | Çalışır |
| Eşleşen exception | Kısmen | Çalışır | Çalışır |
| Eşleşmeyen exception | Kısmen | Atlanır | Çalışır, sonra yukarı fırlar |
| `System.exit()` | Durur | Durur | **ÇALIŞMAZ** |

> `finally` yalnızca `System.exit()` veya JVM çökmesinde çalışmaz.

---

## 4. throw vs throws

```java
// throws → metot imzasında bildirim; çağıran handle etmek zorunda
public void readFile(String path) throws IOException {

    // throw → exception nesnesini gerçekten fırlat
    if (path == null)
        throw new IllegalArgumentException("path null olamaz");

    // ...dosya işlemleri...
}
```

**Özet:** `throws` söz verir, `throw` yapar.

---

## 5. Custom Exception

```java
// Checked custom exception — Exception'dan türetildi
public class InvalidGradeException extends Exception {
    public InvalidGradeException(int grade) {
        super("Geçersiz not: " + grade + " (beklenen 0-100)");
    }
}

// Kullanım
void validate(int grade) throws InvalidGradeException {
    if (grade < 0 || grade > 100)
        throw new InvalidGradeException(grade);
}
```

---

## 6. Text I/O — Temel Sınıflar

| Sınıf | Amaç |
|-------|------|
| `FileReader` | Karakter bazlı okuma |
| `BufferedReader` | `readLine()` — satır okuma, `FileReader`'ı sarmalar |
| `FileWriter` | Karakter bazlı yazma; `true` → append modu |
| `PrintWriter` | `print/println/printf` ile biçimli yazma |
| `Scanner(File)` | Token bazlı okuma |

```java
// Sarmalama kalıbı
BufferedReader br = new BufferedReader(new FileReader("girdi.txt"));
PrintWriter    pw = new PrintWriter(new FileWriter("cikti.txt"));
```

---

## 7. Try-With-Resources (TWR)

```java
// AutoCloseable implementasyonu olan her nesne TWR içinde tanımlanabilir
try (BufferedReader br = new BufferedReader(new FileReader("input.txt"));
     PrintWriter    pw = new PrintWriter(new FileWriter("output.txt"))) {

    String line;
    while ((line = br.readLine()) != null) {
        pw.println(line.toUpperCase());
    }
}
// Kapanış sırası: pw önce kapatılır, sonra br → açılış sırasının TERSİ (LIFO)
```

> TWR kullanılmazsa `finally` içinde `close()` yazmak gerekir. `FileWriter` kapatılmazsa buffer diske yazılmaz → son satırlar kaybolur.

---

## 8. Append Modu

```java
// İkinci argüman true → dosyanın sonuna ekler
PrintWriter pw = new PrintWriter(new FileWriter("log.txt", true));

// İkinci argüman yok (veya false) → dosyayı sıfırdan yazar!
PrintWriter pw2 = new PrintWriter(new FileWriter("log.txt"));
```

---

## 9. Scanner Tuzağı — nextInt + nextLine

```java
Scanner sc = new Scanner(System.in);
int age = sc.nextInt();         // "25\n" → 25 okunur, \n tamponda kalır
String name = sc.nextLine();    // "\n" okunur → name = "" (BOŞ!)

// Düzeltme: nextInt() sonrasına ekstra nextLine() ekle
int age2  = sc.nextInt();
sc.nextLine();                  // tampandaki \n'yi temizle
String name2 = sc.nextLine();   // artık gerçek isim okunur
```

---

## 10. Event-Driven Programming

```java
// Functional interface — tam olarak 1 soyut metot
@FunctionalInterface
interface EventHandler<T> {
    void handle(T event);
}

// Lambda ile kayıt
Button save = new Button("Kaydet");
save.setOnClick(msg -> System.out.println("İşlem: " + msg));

// JavaFX karşılığı
btOK.setOnAction(e -> label.setText("Tıklandı!"));
```

| Kavram | Tanım |
|--------|-------|
| **Event Source** | Olayı tetikleyen bileşen (`Button`, `TextField`) |
| **Event Object** | Olayın bilgisini taşıyan nesne (`ActionEvent`) |
| **Event Handler** | Callback kodu — `EventHandler<ActionEvent>` |

---

## 11. Exceptions & Hatalar Tablosu

| Exception | Checked? | Ne Zaman |
|-----------|----------|----------|
| `FileNotFoundException` | Checked | Dosya bulunamazsa |
| `IOException` | Checked | Genel I/O hatası |
| `NumberFormatException` | Unchecked | `Integer.parseInt("abc")` |
| `NullPointerException` | Unchecked | null referansına erişim |
| `IllegalArgumentException` | Unchecked | Geçersiz argüman |
| Compile error | — | Checked exception'a `catch`/`throws` olmadan erişim |

---

## 12. Sınav Tuzakları (MC)

| # | Tuzak | Açıklama |
|---|-------|----------|
| 1 | **`finally` + `return`** | `catch` içindeki `return` bile `finally`'yi tetikler; çıktı "AB" değil "ABC" olur |
| 2 | **En genel `catch` önce** | `catch (Exception e)` → `catch (IOException e)` sırası → compile error (unreachable) |
| 3 | **`throw` vs `throws`** | `throw` fırlatır, `throws` bildirir; yeri karıştırılırsa compile error |
| 4 | **`FileWriter` append** | `new FileWriter("f")` dosyayı sıfırlar; append için `new FileWriter("f", true)` |
| 5 | **TWR kapanış sırası** | Açılış LIFO tersi ile kapanır — son açılan ilk kapanır |
| 6 | **nextInt + nextLine** | `nextInt()` `\n`'yi tamponda bırakır; hemen ardından `nextLine()` boş string döner |
| 7 | **Multi-catch hiyerarşi** | `catch (Exception \| IOException e)` compile error; alt sınıf zaten kapsamda |
| 8 | **`NumberFormatException`** | Unchecked — `catch` zorunlu değil ama sınavda "checked mi?" sorusu var |

---

## 13. Self-Check

1. `finally` bloğu ne zaman çalışmaz?
2. `NumberFormatException` checked mi, unchecked mi? Neden?
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

4. İki kaynakla TWR: hangi kaynak önce kapanır?

<details>
<summary>Cevaplar</summary>

1. `System.exit()` çağrısı veya JVM çökmesi. Normal akışta (return/exception dahil) **her zaman** çalışır.
2. **Unchecked** — `RuntimeException`'dan türer. Derleyici zorlamaz.
3. **AB** — catch "A" yazar, `return` öncesinde `finally` çalışır "B" yazar; "C"'ye ulaşılmaz.
4. **En son açılan önce** kapanır — LIFO sırası.

</details>

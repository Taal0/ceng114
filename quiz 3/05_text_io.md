# 05 — Text I/O (Dosya İşlemleri)

> **Amaç:** Dosyaya yazmak (`PrintWriter`) ve dosyadan okumak (`Scanner`). Hepsi `IOException` (checked) atabilir, **`throws` veya `try-catch` ZORUNLU**.

---

## File Sınıfı (java.io.File)

`File` aslında **dosyanın içeriği değil**, dosya/klasör hakkındaki **metadata**'dır (path, var mı, boyutu vs.).

```java
import java.io.File;

File file = new File("data.txt");

file.exists();         // var mı?
file.isFile();         // dosya mı?
file.isDirectory();    // klasör mü?
file.length();         // byte cinsinden boyut
file.getName();        // "data.txt"
file.getAbsolutePath();
file.delete();         // sil
file.canRead();        // okuyabilir miyim?
file.canWrite();
file.lastModified();
```

---

## PrintWriter — Dosyaya Yazmak

```java
import java.io.PrintWriter;

public static void main(String[] args) throws IOException {
    PrintWriter out = new PrintWriter("data.txt");

    out.println("İlk satır");
    out.println(42);
    out.printf("Ad: %s, Yaş: %d%n", "Ali", 25);

    out.close(); // ZORUNLU - yoksa içerik dosyaya yazılmaz!
}
```

**Method'lar:** `print`, `println`, `printf`, `write`.

> `close()` çağrılmazsa buffer flush olmaz, dosya **boş** kalabilir. **Mutlaka kapat.**

---

## Scanner — Dosyadan Okumak

```java
import java.util.Scanner;
import java.io.File;

Scanner in = new Scanner(new File("data.txt"));

while (in.hasNext()) {
    String word = in.next();   // boşlukla ayrılmış token
    System.out.println(word);
}
in.close();
```

**Method'lar:**

| Method | Ne döner |
|---|---|
| `next()` | sonraki token (String) |
| `nextLine()` | sonraki tüm satır |
| `nextInt()` | int |
| `nextDouble()` | double |
| `hasNext()` | başka token var mı? |
| `hasNextInt()` | sonraki int mi? |
| `hasNextLine()` | başka satır var mı? |

---

## try-with-resources (Liang ÇOK SEVER)

`close()`'u **otomatik** çağırır. Java 7+:

```java
try (PrintWriter out = new PrintWriter("data.txt")) {
    out.println("merhaba");
}   // out.close() otomatik

try (Scanner in = new Scanner(new File("data.txt"))) {
    while (in.hasNext()) {
        System.out.println(in.next());
    }
}
```

**Birden fazla resource:**

```java
try (Scanner in  = new Scanner(new File("input.txt"));
     PrintWriter out = new PrintWriter("output.txt")) {

    while (in.hasNext()) {
        String word = in.next();
        out.println(word.toUpperCase());
    }
}
```

> Hoca quiz'de "resource'u doğru kapat" derse **bunu kullan**.

---

## Tam Örnek 1: Dosyaya yaz, sonra oku

```java
import java.io.*;
import java.util.Scanner;

public class WriteAndRead {
    public static void main(String[] args) throws IOException {

        // Yaz
        try (PrintWriter out = new PrintWriter("scores.txt")) {
            out.println("Ali 85");
            out.println("Veli 92");
            out.println("Ayşe 78");
        }

        // Oku
        try (Scanner in = new Scanner(new File("scores.txt"))) {
            int total = 0, count = 0;
            while (in.hasNext()) {
                String name = in.next();
                int score = in.nextInt();
                System.out.println(name + ": " + score);
                total += score;
                count++;
            }
            System.out.println("Ortalama: " + (double) total / count);
        }
    }
}
```

---

## Tam Örnek 2: Dosyadaki sayıları topla

```java
import java.io.*;
import java.util.Scanner;

public class SumNumbers {
    public static void main(String[] args) {
        try (Scanner in = new Scanner(new File("numbers.txt"))) {
            int sum = 0;
            while (in.hasNextInt()) {
                sum += in.nextInt();
            }
            System.out.println("Toplam: " + sum);
        } catch (FileNotFoundException e) {
            System.out.println("Dosya bulunamadı: " + e.getMessage());
        }
    }
}
```

---

## Tam Örnek 3: Satır satır oku, kelime say

```java
try (Scanner in = new Scanner(new File("text.txt"))) {
    int lineCount = 0, wordCount = 0;
    while (in.hasNextLine()) {
        String line = in.nextLine();
        lineCount++;
        wordCount += line.split("\\s+").length;
    }
    System.out.println("Satır: " + lineCount + ", Kelime: " + wordCount);
}
```

---

## Sık Yapılan Hatalar

1. **`close()` çağırmamak** → dosya boş kalır.
   - Çözüm: `try-with-resources`.
2. **`throws IOException` koymamak** → compile hatası.
3. **Yanlış path** → `FileNotFoundException`.
   - Path **çalışma dizinine göre** veya tam yol.
4. **`nextInt` sonrası `nextLine`** → `nextInt` newline'ı tüketmez, `nextLine` boş döner.
   - Çözüm: araya `in.nextLine()` koy.
5. **`hasNext` ile `hasNextInt` karıştırmak** → tip uyumsuzluğu.

---

## Quiz'de Sorulabilecek

1. "`numbers.txt` dosyasındaki tam sayıları oku, **toplamlarını** ekrana yazdır."
2. "Bir öğrenci listesini (isim, not) `students.txt`'ye yaz, sonra geri oku, ortalamayı bastır."
3. "Bir dosyadaki **satır sayısını** bul."
4. "Bir dosyadaki kelimelerin **büyük harfli halini** başka dosyaya yaz."

### Quiz şablonu (kopyala-uyarla):

```java
import java.io.*;
import java.util.Scanner;

public class QuizSolution {
    public static void main(String[] args) {
        try (Scanner in  = new Scanner(new File("input.txt"));
             PrintWriter out = new PrintWriter("output.txt")) {

            while (in.hasNext()) {
                // ... task'a göre işle ...
                String token = in.next();
                out.println(token);
            }

        } catch (FileNotFoundException e) {
            System.out.println("Dosya yok: " + e.getMessage());
        }
    }
}
```

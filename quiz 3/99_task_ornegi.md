# 99 — Quiz Task Örneği (Hocam Tarzı)

> Bu, hocanın quiz'de **tipik olarak vereceği task PDF'inin** öykündüğü bir örnektir. Aşağıdaki yönergelere harfiyen uyma alıştırması yap; her detay önemli.

---

## TASK PDF — ÖRNEK

**CENG114 — Lab Quiz 3**
**Süre:** 60 dakika
**Dosya adı:** `StudentManager.java`

### Gereksinimler

`Student` adında bir sınıf yazın. Aşağıdaki **birebir** isimlere uyun:

1. Field'lar (private):
   - `String name`
   - `int id`
   - `double gpa`

2. Constructor: `Student(String name, int id, double gpa)`

3. Getter'lar: `getName()`, `getId()`, `getGpa()`

4. `toString()` metodu — tam olarak şu formatta dönmeli:
   ```
   [id] name (gpa)
   ```
   Örnek: `[101] Ali (3.45)`

5. `Student` sınıfı `Comparable<Student>` interface'ini **implement etmeli**. `compareTo`, **GPA'ya göre azalan** sıralayacak şekilde yazılmalı.

---

`StudentManager` adında ayrı bir sınıf yazın. `main` metodu olmalı.

1. Programınız `students.txt` dosyasından öğrenci verilerini okumalı. Her satır şu formatta:
   ```
   id name gpa
   ```
   Örnek:
   ```
   101 Ali 3.45
   102 Veli 3.80
   103 Ayse 3.20
   ```

2. Okuduğunuz öğrencileri bir `ArrayList<Student>`'a koyun.

3. Listeyi **GPA'ya göre AZALAN** şekilde **insertion sort algoritması ile** sıralayın. (Hazır `Collections.sort` veya `Arrays.sort` **KULLANMAYIN**, kendi insertion sort'unuzu yazın.)

4. Sıralı listeyi `output.txt` dosyasına yazın, her öğrenci ayrı satırda `toString()` formatında.

5. Dosya bulunamazsa **kullanıcıya anlamlı bir mesaj** verin, program çökmesin.

---

### Notlandırma

- Class/method isimleri yanlışsa: **0**
- Bubble veya selection sort kullanırsa: **0** (insertion istendi!)
- `Comparable` implement edilmemişse: **-30**
- Try-catch yoksa (FileNotFoundException): **-15**
- `try-with-resources` kullanılmamışsa: **-10**
- `toString` formatı yanlışsa: **-15**

---

## ÇÖZÜM (Tam Çalışan Kod)

### `Student.java` (aynı dosyada başka class olabilir)

```java
public class Student implements Comparable<Student> {
    private String name;
    private int id;
    private double gpa;

    public Student(String name, int id, double gpa) {
        this.name = name;
        this.id = id;
        this.gpa = gpa;
    }

    public String getName() { return name; }
    public int getId()      { return id; }
    public double getGpa()  { return gpa; }

    @Override
    public String toString() {
        return "[" + id + "] " + name + " (" + gpa + ")";
    }

    @Override
    public int compareTo(Student other) {
        // AZALAN: büyük gpa önce → other - this
        return Double.compare(other.gpa, this.gpa);
    }
}
```

### `StudentManager.java`

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.Scanner;

public class StudentManager {

    // INSERTION SORT — generic Comparable
    public static <E extends Comparable<E>> void insertionSort(ArrayList<E> list) {
        for (int i = 1; i < list.size(); i++) {
            E key = list.get(i);
            int j = i - 1;
            // azalan için compareTo'nun zaten ters olduğunu unutma
            // Comparable'da compareTo > 0 → key, list[j]'den "büyük"
            while (j >= 0 && list.get(j).compareTo(key) > 0) {
                list.set(j + 1, list.get(j));
                j--;
            }
            list.set(j + 1, key);
        }
    }

    public static void main(String[] args) {
        ArrayList<Student> students = new ArrayList<>();

        // OKU
        try (Scanner in = new Scanner(new File("students.txt"))) {
            while (in.hasNext()) {
                int id      = in.nextInt();
                String name = in.next();
                double gpa  = in.nextDouble();
                students.add(new Student(name, id, gpa));
            }
        } catch (FileNotFoundException e) {
            System.out.println("Hata: students.txt bulunamadı.");
            return;
        }

        // SIRALA — insertion sort, AZALAN
        insertionSort(students);

        // YAZ
        try (PrintWriter out = new PrintWriter("output.txt")) {
            for (Student s : students) {
                out.println(s.toString());
            }
        } catch (FileNotFoundException e) {
            System.out.println("Hata: output.txt yazılamadı.");
        }

        System.out.println("Tamamlandı. " + students.size() + " öğrenci işlendi.");
    }
}
```

### `students.txt` (test girdisi)

```
101 Ali 3.45
102 Veli 3.80
103 Ayse 3.20
104 Mehmet 3.95
105 Fatma 3.10
```

### `output.txt` (beklenen çıktı)

```
[104] Mehmet (3.95)
[102] Veli (3.8)
[101] Ali (3.45)
[103] Ayse (3.2)
[105] Fatma (3.1)
```

---

## Quiz'e Girerken Kontrol Listesi

1. ✅ **Class/method isimleri** task'tekiyle birebir aynı mı?
2. ✅ **İstenen algoritma** mı (insertion vs bubble)?
3. ✅ **Field'lar private** mı, getter'lar public mı?
4. ✅ **`Comparable` implement** edildi mi?
5. ✅ **`compareTo`** doğru yön (artan/azalan)?
6. ✅ **`toString` formatı** birebir mi?
7. ✅ **Dosya path** çalışma dizinine göre doğru mu?
8. ✅ **`try-with-resources`** kullanıldı mı?
9. ✅ **`FileNotFoundException`** yakalandı mı?
10. ✅ **`return` veya `System.exit`** ile graceful sonlandırma var mı?

---

## Bonus: Hocanın Sevdiği Detaylar

1. **`Double.compare(a, b)`** kullan, `a - b` yapma (precision hatası).
2. **Resource'ları `try-with-resources` ile kapat**.
3. **Method'ları küçük tut** (sort ayrı, IO ayrı).
4. **Komentleri Türkçe ya İngilizce, biri seçilsin** (karışmasın).
5. **`@Override`** annotation'ını **unutma**.
6. **Magic number kullanma** — `final int MAX = 100;` gibi sabit yap.

---

## Eğer Quiz JavaFX Tabanlıysa

Hoca event-driven sorduysa, task şu tarzda olur:

> "Bir pencerede iki TextField (sayı), bir Button ('Topla'), bir Label olsun. Butona basınca toplamı label'a yazsın. Sayı dışı girdi için exception handling olsun."

→ Çözüm için **`06_event_driven.md`** dosyasındaki **AdderApp** şablonunu uyarlayabilirsin.

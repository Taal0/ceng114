# CENG114 — Quiz 3 Cheat Sheet

> **Altın kural:** Hoca **ne istediyse** onu yaz. Insertion istenmişse Bubble yazma. Method ismi, parametre tipi ve dönüş tipi task PDF'iyle birebir aynı olsun.

---

## 1) Sorting — hangi algoritma neye benzer?

| Algoritma | Tek cümlelik mantık | Loop yapısı | Big-O |
|---|---|---|---|
| **Bubble** | Komşuları kıyasla, büyüğü sona "yuvarla" | `for i ; for j=0..n-i-1` | O(n²) |
| **Selection** | Kalan kısımdaki **min**'i bul, başa koy | `for i ; for j=i+1..n` | O(n²) |
| **Insertion** | Eldeki kartı **doğru yere sok** (sola kaydır) | `for i=1..n ; while j>=0 && a[j]>key` | O(n²) |
| **Merge** | İkiye böl, sırala, **birleştir** | recursive + merge | O(n log n) |
| **Quick** | Pivot seç, **partition** et, recursive | recursive + partition | O(n log n) ort. |

**Ayırt etme ipuçları:**
- "İçteki döngü `n - i - 1`'e kadar gidiyor" → **Bubble**
- "Min indexi tutuyoruz, sonun**da** swap" → **Selection**
- "Sağdan sola kaydırma var, `while` kullanılıyor" → **Insertion**

---

## 2) Searching

```java
// Linear — sıralı olmak zorunda değil
for (int i = 0; i < a.length; i++)
    if (a[i] == key) return i;
return -1;

// Binary — DİZİ SIRALI OLMALI
int low = 0, high = a.length - 1;
while (low <= high) {
    int mid = (low + high) / 2;
    if (key < a[mid])      high = mid - 1;
    else if (key > a[mid]) low  = mid + 1;
    else return mid;
}
return -low - 1; // Liang konvansiyonu: bulunamazsa
```

---

## 3) Exception — şablon

```java
try {
    // riskli kod
} catch (SpecificException e) {
    // ÖNCE özel exception
} catch (Exception e) {
    // SONRA genel
} finally {
    // her durumda çalışır (resource kapatma)
}
```

- **Checked** (IOException, FileNotFoundException) → ya `try-catch` ya `throws`
- **Unchecked** (NullPointerException, ArithmeticException, IndexOutOfBoundsException) → zorunlu değil
- Custom: `class MyException extends Exception { ... }`

---

## 4) Text I/O — şablon

```java
// YAZMA
try (PrintWriter out = new PrintWriter("data.txt")) {
    out.println("merhaba");
}

// OKUMA
try (Scanner in = new Scanner(new File("data.txt"))) {
    while (in.hasNext()) {
        String s = in.next();
    }
}
```

`try-with-resources` → `close()` otomatik. **Liang bunu sever.**

---

## 5) Event-Driven (JavaFX) — şablon

```java
Button btn = new Button("Tıkla");
btn.setOnAction(e -> System.out.println("Basıldı"));
// veya:
btn.setOnAction(new EventHandler<ActionEvent>() {
    @Override public void handle(ActionEvent e) { ... }
});
```

- **Source** = Button, **Handler** = `EventHandler<T>`, **Event** = `ActionEvent`
- Lambda: `e -> { ... }` (parametre tek ise parantez gereksiz)

---

## 6) Generics

```java
public class Box<T> {              // generic class
    private T value;
    public void set(T v) { value = v; }
    public T get() { return value; }
}

public static <E> void print(E[] a) { ... }       // generic method
public static <E extends Comparable<E>> E max(E[] a) { ... } // bounded
List<? extends Number> nums;       // wildcard: okunur, eklenmez
List<? super Integer> ints;        // wildcard: Integer eklenebilir
```

---

## 7) Collections — hangisi ne için?

| Amaç | Sınıf | Özellik |
|---|---|---|
| Sıralı, dizi gibi | `ArrayList<E>` | indexli erişim O(1) |
| Sık ekleme/silme | `LinkedList<E>` | başa/sona O(1) |
| Tekrarsız | `HashSet<E>` | sıra yok |
| Sıralı tekrarsız | `TreeSet<E>` | otomatik sıralı |
| Anahtar-değer | `HashMap<K,V>` | sıra yok |
| Sıralı map | `TreeMap<K,V>` | key'e göre sıralı |
| LIFO | `Stack<E>` | `push/pop/peek` |
| FIFO | `LinkedList` (Queue) | `offer/poll/peek` |

```java
List<Integer> list = new ArrayList<>();
list.add(5);
Collections.sort(list);
for (int x : list) System.out.println(x);
```

---

## 8) Quiz'de SIK YAPILAN HATALAR

1. **Yanlış algoritma:** "Insertion" denmişse Bubble yazma.
2. **Method imzası uyuşmuyor:** `static void sort(int[] a)` istemiş, sen `int[] sort(int[] a)` yazıyorsun.
3. **0-indexed vs 1-indexed:** Java her zaman `0..n-1`.
4. **Off-by-one:** `j < n - i - 1` mi `j <= n - i - 1` mi? Bubble'da `<`.
5. **Generic kullanmadın:** `Comparable` istenmişse `compareTo` çağır.
6. **Resource kapatmadın:** `Scanner`/`PrintWriter` için `try-with-resources`.
7. **`throws` koymadın:** `FileNotFoundException` checked'tir.
8. **`equals` yerine `==`:** String için `==` ASLA, `equals` kullan.
9. **Class adı dosya adı ile uyuşmuyor:** `public class Foo` → `Foo.java`.
10. **`main` yok / yanlış:** `public static void main(String[] args)` birebir.

---

## 9) Hızlı checklist (yazdıktan sonra)

- [ ] Method imzası task'tekiyle aynı mı?
- [ ] Algoritma **doğru** mu (istenen mi)?
- [ ] Hangi koleksiyon istendi, onu mu kullandım?
- [ ] Exception istenen yerde var mı?
- [ ] Test girdisinde sınır durumlar (boş dizi, tek eleman) çalışıyor mu?
- [ ] Çıktının formatı task'tekiyle aynı mı (boşluk, yeni satır)?

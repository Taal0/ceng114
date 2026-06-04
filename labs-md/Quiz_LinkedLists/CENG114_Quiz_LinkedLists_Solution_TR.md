# CENG114 — Computer Programming II
## Quiz Çözümü: Linked Lists in Practice — TrackBeat Studio

**Ankara Yıldırım Beyazıt Üniversitesi · Bilgisayar Mühendisliği Bölümü**

> Bu dosya `CENG114_Quiz_LinkedLists.md` sorusunun referans çözümünü içerir.
> Dört Java dosyası aynı klasörde olmalı; sonra:
>
> ```bash
> javac -encoding UTF-8 *.java
> java TrackBeatStudio
> ```
>
> komutu sorudaki `§6 Expected Output` bloğunu birebir basar.

---

## 1. Tasarım Özeti — "Hangi yapıyı neden seçtik?"

| Sınıf              | Veri Yapısı                | Niye?                                                                          |
| ------------------ | -------------------------- | ------------------------------------------------------------------------------ |
| `Track`            | Plain Old Java Object      | Sadece veri tutuyor; mantığı yok.                                              |
| `RequestQueue<T>`  | SLL + `head` *ve* `tail`   | FIFO için yeter; `tail` sayesinde `enqueue` O(1) — `prev` alanına ihtiyaç yok. |
| `DJPlaylist<T>`    | DLL + `head` ve `tail`     | Çift yönlü gezme; elimizdeki bir düğümü O(1) silmek için `prev` şart.          |
| `TrackBeatStudio`  | Driver                     | Tek bir yayını simüle eder, çıktıyı üretir.                                    |

### Üç Anahtar Fikir

1. **Tail pointer'ı SLL'i "uca ekle" için kurtarır.** `enqueue` her seferinde
   listeyi baştan sona dolaşmak yerine doğrudan `tail.next = n; tail = n;`
   yapar — O(n) yerine O(1). `dequeue` boş kuyruğa düştüğünde **`tail`'i de
   `null`'a çekmeyi unutma**, yoksa bir sonraki `enqueue` ölü bir düğümü
   yeniden zincire takar.
2. **DLL'in tek süper-gücü: O(1) `remove(node)`.** SLL'de "şu düğümü sil"
   demek `head`'ten yürüyüp önceki düğümü bulmaktır — O(n). DLL'de
   `node.prev` zaten elinin altında, dolayısıyla iki satırda bypass yapılır.
   Bu yüzden DJ playlist'inde DLL kullanıyoruz.
3. **Generic tip parametresi `T` static nested sınıflarda görülmez.**
   `Node<E>` ve `DNode<E>` ayrı bir tip parametresi alır. Bu Java'nın
   syntax kuralı, hata değil.

---

## 2. `Track.java`

Tek başına dolaşır, sadece veri ve format işi:

```java
public class Track {
    private final String title;
    private final String artist;
    private final int durationSec;

    public Track(String title, String artist, int durationSec) {
        this.title = title;
        this.artist = artist;
        this.durationSec = durationSec;
    }

    public String getTitle()       { return title; }
    public String getArtist()      { return artist; }
    public int    getDurationSec() { return durationSec; }

    @Override
    public String toString() {
        int min = durationSec / 60;
        int sec = durationSec % 60;
        return String.format("%s — %s (%d:%02d)", title, artist, min, sec);
    }
}
```

**Dikkat edilecek noktalar**

- `private final` alanlar = immutability. Bir kez kurulan track değişmez —
  iki playlist arasında paylaşmak güvenli.
- `%02d` saniyeyi sıfırla doldurur: 183 saniye → `"3:03"`, asla `"3:3"`.
- Em-dash (`—`) U+2014 karakteridir. Kaynak dosyayı UTF-8 olarak kaydet.

---

## 3. `RequestQueue.java`

```java
import java.util.NoSuchElementException;

public class RequestQueue<T> {

    private static class Node<E> {
        E data;
        Node<E> next;
        Node(E data) { this.data = data; }
    }

    private Node<T> head;
    private Node<T> tail;
    private int size;

    public void enqueue(T item) {
        Node<T> n = new Node<>(item);
        if (tail == null) {           // boş kuyruk: head VE tail birlikte
            head = tail = n;
        } else {                      // sona ekle, tail'i ilerlet
            tail.next = n;
            tail = n;
        }
        size++;
    }

    public T dequeue() {
        if (head == null) {
            throw new NoSuchElementException("queue is empty");
        }
        T data = head.data;
        head = head.next;
        if (head == null) {           // KRİTİK: tail de null olmalı
            tail = null;
        }
        size--;
        return data;
    }

    public T peek() {
        if (head == null) {
            throw new NoSuchElementException("queue is empty");
        }
        return head.data;
    }

    public int     size()    { return size; }
    public boolean isEmpty() { return size == 0; }

    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder("[");
        Node<T> p = head;
        while (p != null) {
            sb.append(p.data);
            if (p.next != null) sb.append(", ");
            p = p.next;
        }
        return sb.append("]").toString();
    }
}
```

**Tartışma**

- `enqueue` ve `dequeue` her ikisi de **sabit zamanda** çalışır: hiçbir
  döngü, hiçbir yürüyüş yok. SLL'de tail pointer'ı tutmasaydık `enqueue`
  her çağrıda head'ten sona kadar yürür — O(n).
- `dequeue` boşaltma anında `tail = null` ataması olmazsa, bir sonraki
  `enqueue` çağrısı ölü düğümün `next`'ine yazar — head ise hâlâ `null`
  görüldüğü için "boş kuyruk" branch'ine girer ve sessizce zincir bozulur.
  Bu, sınavda çok rastladığımız bir buggy implementasyon.
- `toString` boş kuyrukta `"[]"` döner — `while` döngüsüne hiç girmez,
  doğrudan `sb` baş ve son köşeli parantezleri birleştirir.

---

## 4. `DJPlaylist.java`

```java
import java.util.Iterator;
import java.util.NoSuchElementException;
import java.util.Objects;

public class DJPlaylist<T> implements Iterable<T> {

    public static class DNode<E> {
        private E data;
        private DNode<E> prev;
        private DNode<E> next;

        DNode(E data) { this.data = data; }
        public E getData() { return data; }
    }

    private DNode<T> head;
    private DNode<T> tail;
    private int size;

    // ------------------------------------------------------------------
    //  INSERTIONS
    // ------------------------------------------------------------------

    public DNode<T> addFirst(T item) {
        DNode<T> n = new DNode<>(item);
        if (head == null) {                 // boş liste
            head = tail = n;
        } else {
            n.next = head;                  // önce DIŞA bağla
            head.prev = n;                  // sonra İÇERİ bağla
            head = n;
        }
        size++;
        return n;
    }

    public DNode<T> addLast(T item) {
        DNode<T> n = new DNode<>(item);
        if (tail == null) {
            head = tail = n;
        } else {
            n.prev = tail;                  // önce DIŞA
            tail.next = n;                  // sonra İÇERİ
            tail = n;
        }
        size++;
        return n;
    }

    public DNode<T> addAfter(DNode<T> ref, T item) {
        if (ref == null) throw new IllegalArgumentException("ref is null");
        if (ref == tail) return addLast(item);

        DNode<T> n = new DNode<>(item);
        n.prev = ref;                       // 4 referansı sırasıyla yaz
        n.next = ref.next;
        ref.next.prev = n;
        ref.next = n;
        size++;
        return n;
    }

    public DNode<T> addBefore(DNode<T> ref, T item) {
        if (ref == null) throw new IllegalArgumentException("ref is null");
        if (ref == head) return addFirst(item);

        DNode<T> n = new DNode<>(item);
        n.next = ref;
        n.prev = ref.prev;
        ref.prev.next = n;
        ref.prev = n;
        size++;
        return n;
    }

    // ------------------------------------------------------------------
    //  REMOVALS
    // ------------------------------------------------------------------

    public T removeFirst() {
        if (head == null) throw new NoSuchElementException("playlist is empty");
        T data = head.data;
        if (head == tail) {                 // tek elemanlı liste
            head = tail = null;
        } else {
            head = head.next;
            head.prev = null;
        }
        size--;
        return data;
    }

    public T removeLast() {
        if (tail == null) throw new NoSuchElementException("playlist is empty");
        T data = tail.data;
        if (head == tail) {
            head = tail = null;
        } else {
            tail = tail.prev;
            tail.next = null;
        }
        size--;
        return data;
    }

    public T remove(DNode<T> node) {
        if (node == null) throw new IllegalArgumentException("node is null");
        if (node == head) return removeFirst();
        if (node == tail) return removeLast();

        T data = node.data;
        node.prev.next = node.next;          // ileri bypass
        node.next.prev = node.prev;          // geri bypass
        node.prev = node.next = null;        // GC için izole et
        size--;
        return data;
    }

    // ------------------------------------------------------------------
    //  QUERIES
    // ------------------------------------------------------------------

    public DNode<T> findFirst(T value) {
        DNode<T> p = head;
        while (p != null) {
            if (Objects.equals(p.data, value)) return p;
            p = p.next;
        }
        return null;
    }

    public int     size()    { return size; }
    public boolean isEmpty() { return size == 0; }

    public String forwardString() {
        StringBuilder sb = new StringBuilder("[");
        DNode<T> p = head;
        while (p != null) {
            sb.append(p.data);
            if (p.next != null) sb.append(", ");
            p = p.next;
        }
        return sb.append("]").toString();
    }

    public String backwardString() {
        StringBuilder sb = new StringBuilder("[");
        DNode<T> p = tail;
        while (p != null) {
            sb.append(p.data);
            if (p.prev != null) sb.append(", ");
            p = p.prev;
        }
        return sb.append("]").toString();
    }

    @Override
    public String toString() { return forwardString(); }

    // ------------------------------------------------------------------
    //  ITERATORS  —  her step O(1), toplam yürüyüş O(n).
    //  Slayt 26'daki get(i) tuzağına düşmemenin yolu budur.
    // ------------------------------------------------------------------

    @Override
    public Iterator<T> iterator() {
        return new Iterator<T>() {
            private DNode<T> p = head;

            @Override public boolean hasNext() { return p != null; }
            @Override public T next() {
                if (p == null) throw new NoSuchElementException();
                T data = p.data;
                p = p.next;
                return data;
            }
        };
    }

    public Iterator<T> descendingIterator() {
        return new Iterator<T>() {
            private DNode<T> p = tail;

            @Override public boolean hasNext() { return p != null; }
            @Override public T next() {
                if (p == null) throw new NoSuchElementException();
                T data = p.data;
                p = p.prev;
                return data;
            }
        };
    }
}
```

**Tartışma — DLL'in en kritik kısımları**

- **`addAfter` / `addBefore`'da 4 referans yazımı.** Sıra önemli ama
  *belirli koşullarla*. `n.prev` ve `n.next`'i (yeni düğümün dış
  bağlantıları) doldurmadan komşunun pointer'ını ezersen rest of the list
  uçar. Algoritmanın 4 satırını ezbere okumak değil, hangi pointer'ın
  henüz okunmamış olduğunu görmek lazım: `ref.next.prev = n` satırını
  yazdığımız anda hâlâ `ref.next` aslında ESKİ next'i gösteriyor olmalı.
  Bu yüzden `n.next = ref.next` SATIRINDAN ÖNCE `ref.next`'i
  değiştirmedik.
- **`remove(node)`'da head/tail delegasyonu.** Eğer ortadaki düğüm değilse
  `node.prev` veya `node.next` `null` olabilir; o satırda
  `NullPointerException` patlar. Önce kontrol et, sonra cesurca
  dereference et.
- **`Objects.equals`** — null-safe karşılaştırma. `p.data.equals(value)`
  yazsaydık ve liste içinde null bir track varsa NPE düşerdi.
- **İki ayrı iterator.** Java'nın `LinkedList`'i de tam böyle yapar
  (`iterator()` + `descendingIterator()`). Slayt 26'daki uyarıyı somut
  bir yere oturtuyor: `for (Track t : playlist)` enhanced-for'u bu
  iterator'u kullandığı için O(n); aynı şeyi `playlist.get(i)`'yle
  yapmaya kalksak (ki sınıfta öyle bir metot yok — bilerek) O(n²)
  olurdu.

---

## 5. `TrackBeatStudio.java`

```java
import java.util.Iterator;

public class TrackBeatStudio {

    public static void main(String[] args) {
        // --- Setup ----------------------------------------------------
        Track stairway   = new Track("Stairway to Heaven",      "Led Zeppelin", 482);
        Track wonderwall = new Track("Wonderwall",              "Oasis",        258);
        Track imagine    = new Track("Imagine",                 "John Lennon",  183);
        Track hotelCal   = new Track("Hotel California",        "Eagles",       391);
        Track smells     = new Track("Smells Like Teen Spirit", "Nirvana",      301);
        Track bohemian   = new Track("Bohemian Rhapsody",       "Queen",        354);

        banner("CENG114 · TrackBeat Studio · Linked Lists Lab");
        System.out.println();

        // --- Phase 1 : listener requests (SLL) ------------------------
        System.out.println("--- Phase 1 · Request Queue (SLL) ---");

        RequestQueue<Track> queue = new RequestQueue<>();
        for (Track t : new Track[] { wonderwall, imagine, hotelCal, bohemian }) {
            queue.enqueue(t);
            System.out.println(pad("Enqueued") + t);
        }
        System.out.println(pad("Queue size")  + queue.size());
        System.out.println(pad("Peek (head)") + queue.peek());
        System.out.println(pad("Queue")       + queue);

        Track firstOut = queue.dequeue();
        System.out.println(pad("Dequeued")    + firstOut);
        System.out.println(pad("Queue size")  + queue.size());
        System.out.println();

        // --- Phase 2 : build the show playlist (DLL) ------------------
        System.out.println("--- Phase 2 · Build Show Playlist (DLL) ---");

        DJPlaylist<Track> playlist = new DJPlaylist<>();
        int drained = 0;
        while (!queue.isEmpty()) {
            playlist.addLast(queue.dequeue());
            drained++;
        }
        System.out.println("Drained queue into playlist (" + drained + " tracks moved).");
        System.out.println(pad("Playlist") + playlist);

        playlist.addFirst(stairway);
        System.out.println(pad("addFirst") + stairway);

        DJPlaylist.DNode<Track> hcNode  = playlist.findFirst(hotelCal);
        DJPlaylist.DNode<Track> imgNode = playlist.findFirst(imagine);

        playlist.addAfter(hcNode, smells);
        System.out.println(pad("addAfter(HC)") + smells);

        playlist.addBefore(imgNode, wonderwall);
        System.out.println(pad("addBefore(Img)") + wonderwall);

        System.out.println(pad("Playlist size") + playlist.size());
        System.out.println(pad("Playlist")      + playlist);
        System.out.println();

        // --- Phase 3 : live show --------------------------------------
        System.out.println("--- Phase 3 · Live Show ---");

        printNumbered("Forward order:",  playlist.iterator());
        printNumbered("Backward order:", playlist.descendingIterator());

        Track removed = playlist.remove(imgNode);
        System.out.println(pad("remove(Imagine)") + removed);

        Track wasFirst = playlist.removeFirst();
        System.out.println(pad("removeFirst()")  + wasFirst);

        Track wasLast = playlist.removeLast();
        System.out.println(pad("removeLast()")   + wasLast);

        System.out.println(pad("Final playlist") + playlist);

        int total = 0;
        for (Track t : playlist) total += t.getDurationSec();
        System.out.println(pad("Total airtime") + formatDuration(total));

        System.out.println();
        banner("Broadcast Ready");
    }

    // ------------------------------------------------------------------
    //  KÜÇÜK YARDIMCILAR
    // ------------------------------------------------------------------

    /** "Label" + boşluk + nokta dolgusu (toplam 18 karakter) + tek boşluk. */
    private static String pad(String label) {
        StringBuilder sb = new StringBuilder(label).append(' ');
        while (sb.length() < 18) sb.append('.');
        return sb.append(' ').toString();
    }

    private static void banner(String text) {
        System.out.println("============================================================");
        System.out.println("   " + text);
        System.out.println("============================================================");
    }

    private static void printNumbered(String label, Iterator<Track> iter) {
        System.out.println(label);
        int i = 1;
        while (iter.hasNext()) {
            System.out.printf("  %d. %s%n", i++, iter.next());
        }
    }

    private static String formatDuration(int sec) {
        return String.format("%d:%02d", sec / 60, sec % 60);
    }
}
```

**Driver hakkında**

- **`pad` neden 18 karakter?** Soru §6'da gösterilen tüm satırlar
  noktalı prefix dahil 18 karakter uzunluğunda, sonra tek bir boşluk
  ve değer geliyor. Bu hizalama olmadan çıktı bayt-bayt eşleşmez. `pad`
  her label için doğru sayıda nokta üretir; manuel sayıp yazma derdine
  girmek yok.
- **Total airtime hesabı.** `playlist` final state'inde sadece 3 track
  kaldı: Wonderwall (258), Hotel California (391), Smells Like Teen
  Spirit (301). 258 + 391 + 301 = 950 saniye = 15:50. Driver içinde
  enhanced-for ile gezilebilmesinin sebebi DJPlaylist'in `Iterable<T>`
  implementasyonu — slayt 26'nın "doğru" yolu.
- **Print ettiğin track'in toString'i Object'in default'una bağlı.**
  Her `System.out.println(t)` veya `pad(...) + t` çağrısında otomatik
  olarak `Track.toString()` çağrılır — bu yüzden track'lerin tek
  satırlık format'ı her yerde tutarlı.

---

## 6. Beklenen Çıktı — Doğrulama

`java TrackBeatStudio` komutu birebir şu çıktıyı üretmeli (sorunun
§6'sıyla aynı):

```text
============================================================
   CENG114 · TrackBeat Studio · Linked Lists Lab
============================================================

--- Phase 1 · Request Queue (SLL) ---
Enqueued ......... Wonderwall — Oasis (4:18)
Enqueued ......... Imagine — John Lennon (3:03)
Enqueued ......... Hotel California — Eagles (6:31)
Enqueued ......... Bohemian Rhapsody — Queen (5:54)
Queue size ....... 4
Peek (head) ...... Wonderwall — Oasis (4:18)
Queue ............ [Wonderwall — Oasis (4:18), Imagine — John Lennon (3:03), Hotel California — Eagles (6:31), Bohemian Rhapsody — Queen (5:54)]
Dequeued ......... Wonderwall — Oasis (4:18)
Queue size ....... 3

--- Phase 2 · Build Show Playlist (DLL) ---
Drained queue into playlist (3 tracks moved).
Playlist ......... [Imagine — John Lennon (3:03), Hotel California — Eagles (6:31), Bohemian Rhapsody — Queen (5:54)]
addFirst ......... Stairway to Heaven — Led Zeppelin (8:02)
addAfter(HC) ..... Smells Like Teen Spirit — Nirvana (5:01)
addBefore(Img) ... Wonderwall — Oasis (4:18)
Playlist size .... 6
Playlist ......... [Stairway to Heaven — Led Zeppelin (8:02), Wonderwall — Oasis (4:18), Imagine — John Lennon (3:03), Hotel California — Eagles (6:31), Smells Like Teen Spirit — Nirvana (5:01), Bohemian Rhapsody — Queen (5:54)]

--- Phase 3 · Live Show ---
Forward order:
  1. Stairway to Heaven — Led Zeppelin (8:02)
  2. Wonderwall — Oasis (4:18)
  3. Imagine — John Lennon (3:03)
  4. Hotel California — Eagles (6:31)
  5. Smells Like Teen Spirit — Nirvana (5:01)
  6. Bohemian Rhapsody — Queen (5:54)
Backward order:
  1. Bohemian Rhapsody — Queen (5:54)
  2. Smells Like Teen Spirit — Nirvana (5:01)
  3. Hotel California — Eagles (6:31)
  4. Imagine — John Lennon (3:03)
  5. Wonderwall — Oasis (4:18)
  6. Stairway to Heaven — Led Zeppelin (8:02)
remove(Imagine) .. Imagine — John Lennon (3:03)
removeFirst() .... Stairway to Heaven — Led Zeppelin (8:02)
removeLast() ..... Bohemian Rhapsody — Queen (5:54)
Final playlist ... [Wonderwall — Oasis (4:18), Hotel California — Eagles (6:31), Smells Like Teen Spirit — Nirvana (5:01)]
Total airtime .... 15:50

============================================================
   Broadcast Ready
============================================================
```

---

## 7. Karmaşıklık Analizi

| Operasyon                              | Bu Çözümde   | Naif SLL'de  |
| -------------------------------------- | ------------ | ------------ |
| `RequestQueue.enqueue`                 | **O(1)**     | O(n)*        |
| `RequestQueue.dequeue` / `peek`        | **O(1)**     | O(1)         |
| `DJPlaylist.addFirst` / `addLast`      | **O(1)**     | O(1) / O(n)* |
| `DJPlaylist.addAfter(node, …)`         | **O(1)**     | O(1)         |
| `DJPlaylist.addBefore(node, …)`        | **O(1)**     | O(n) (önceyi bul) |
| `DJPlaylist.remove(node)`              | **O(1)**     | O(n) (önceyi bul) |
| `DJPlaylist.findFirst(value)`          | O(n)         | O(n)         |
| Forward / Backward traversal (iterator)| O(n)         | O(n) / O(n²) **(reverse)** |

\* tail pointer yokken. Tail pointer eklemek SLL'i bu satırlarda DLL ile
eşitler — ama yine de "elimdeki node'u sil" senaryosunu hızlandırmaz.

---

## 8. Sık Yapılan Hatalar (Grader Defterinden)

1. **Boş kuyrukta `tail` null'a çekilmemiş.** İlk `enqueue`'dan sonra
   patlamaz — ama "boş yapana kadar dequeue, sonra enqueue" patterni
   sessizce zinciri bozar. Görmek zor, debug etmek dert.
2. **DLL'de tek elemanlı listede `removeFirst` ya da `removeLast`.**
   `head.prev = null` veya `tail.next = null` derken `head == tail`
   olduğunu unuturlarsa stale pointer'lar kalır. Çözümde `if (head == tail)`
   kontrolü tam bu yüzden ilk satır.
3. **`addAfter`'da `n.next = ref.next` yerine önce `ref.next = n` yazmak.**
   ESKİ `ref.next`'i kaybedersin — listenin geri kalanı uçar. Sıra:
   önce DIŞA bağla (`n.prev`, `n.next`), sonra İÇERİ (komşuların
   pointer'ları).
4. **`findFirst`'te `==` ile karşılaştırmak.** Aynı içerikteki farklı
   `Track` instance'ları için `==` `false` döner; çağıran tarafında
   `playlist.findFirst(new Track(...))` yazsanız listede olmasına rağmen
   `null` döner. `Objects.equals` ve `Track.equals` override etmek
   daha doğru olur — ama bu quizde aynı referansı dolaştırıyoruz
   (driver `hcNode = playlist.findFirst(hotelCal)` diyor, `hotelCal`
   tanımlı bir değişken), o yüzden iki yöntem de çalışıyor.
5. **`removeFirst` çağrısı boş listede `NoSuchElementException` değil
   `NullPointerException` fırlatıyor.** Java'nın `LinkedList`'i de
   `NoSuchElementException` fırlatır — sözleşme uyumu önemli; sınavda
   da puan kırılır.
6. **Enhanced-for yerine `for (int i = 0; i < n; i++) get(i)` yazmak.**
   Bu quizde `get(i)` yok (bilerek) ama sınavın kendi ödevinde
   `LinkedList<Track>` ile karşılaşırsan **enhanced-for kullan** (slayt 26).

---

## 9. Genişletme Önerileri (Lab dışı, kendi kendine)

- **`DJPlaylist.removeAll(value)`** ekle: aynı değere sahip tüm node'ları
  tek geçişte sil. Dikkat — döngü içinde silerken `node.next`'i silmeden
  ÖNCE okumalısın.
- **`reverse()`** metodu yaz: `head`/`tail`'i swap'la, sonra her node'un
  `prev`/`next`'ini swap'la. Tek geçiş O(n).
- **`RequestQueue<String>`** kullanıp DJ'in dinleyicilerden gelen
  shout-out mesajlarını işlemeye sok — generic yapının ne kadar yeniden
  kullanılabilir olduğunu göstermek için güzel.
- **`processNextHour(int seconds)`** — playlist'in başından itibaren
  toplam süresi `seconds`'i geçmeyen ilk N track'i dön ve oynat (yani
  sıradan çıkar). Iterator'u tüketmek ile `removeFirst()` arasındaki
  farkı düşün.

---

> **Özet:** SLL + tail = ucuz kuyruk; DLL = O(1) navigation + O(1)
> ortadan silme. "Önce DIŞA, sonra İÇERİ" + "boş listeyi/tek düğümü
> ayrı düşün" iki kuralı bilirsen, bu lab'deki bütün kodu bir oturuşta
> doğru yazabilirsin.

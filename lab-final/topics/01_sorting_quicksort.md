# 01 — Sıralama Algoritmaları & Quick Sort

> **Kaynak:** `slides/Week1_.md` (Liang Ch. 7 & 23) · tahtada anlatılan Quick Sort

---

## 1. Neden Bu Konu Var?

Sıralama, veri işlemin temelidir. Hoca Hafta 9'da tüm klasik algoritmalardan geçti; Quick Sort tahtada anlatıldı ama slaytlarda yok — bu yüzden ayrıca öğrenmen gerekiyor.

---

## 2. Anahtar Kavramlar

### 2.1 Karmaşıklık Özeti

| Algoritma | En İyi | Ortalama | En Kötü | Yerinde? | Kararlı? |
|-----------|--------|----------|---------|----------|----------|
| Bubble Sort | O(n) | O(n²) | O(n²) | ✓ | ✓ |
| Selection Sort | O(n²) | O(n²) | O(n²) | ✓ | ✗ |
| Insertion Sort | O(n) | O(n²) | O(n²) | ✓ | ✓ |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | ✗ | ✓ |
| **Quick Sort** | **O(n log n)** | **O(n log n)** | **O(n²)** | **✓** | **✗** |

> **Kararlı (stable):** Eşit elemanların göreli sırası korunur.  
> **Yerinde (in-place):** Ekstra O(n) bellek gerektirmez.

### 2.2 Bubble Sort

```java
static void bubbleSort(int[] a) {
    for (int i = 0; i < a.length - 1; i++) {
        for (int j = 0; j < a.length - 1 - i; j++) {
            if (a[j] > a[j + 1]) {
                int tmp = a[j]; a[j] = a[j + 1]; a[j + 1] = tmp;
            }
        }
    }
}
```

### 2.3 Selection Sort

```java
static void selectionSort(int[] a) {
    for (int i = 0; i < a.length - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < a.length; j++)
            if (a[j] < a[minIdx]) minIdx = j;
        int tmp = a[minIdx]; a[minIdx] = a[i]; a[i] = tmp;
    }
}
```

### 2.4 Insertion Sort

```java
static void insertionSort(int[] a) {
    for (int i = 1; i < a.length; i++) {
        int key = a[i], j = i - 1;
        while (j >= 0 && a[j] > key) { a[j + 1] = a[j]; j--; }
        a[j + 1] = key;
    }
}
```

---

## 3. Quick Sort — Detaylı Anlatım

### 3.1 Fikir

1. Bir **pivot** seç (Lomuto: son eleman).
2. Diziyi böl: pivot'tan küçükler solda, büyükler sağda.
3. Her iki tarafa **özyinelemeli** uygula.

### 3.2 Lomuto Partition — Pseudocode

```
partition(a, low, high):
    pivot = a[high]
    i = low - 1
    for j = low to high - 1:
        if a[j] <= pivot:
            i++
            swap(a[i], a[j])
    swap(a[i+1], a[high])
    return i + 1          // pivot'un son konumu

quickSort(a, low, high):
    if low < high:
        p = partition(a, low, high)
        quickSort(a, low, p - 1)
        quickSort(a, p + 1, high)
```

### 3.3 Java Kodu

```java
static void quickSort(int[] a, int low, int high) {
    if (low < high) {
        int p = partition(a, low, high);
        quickSort(a, low, p - 1);
        quickSort(a, p + 1, high);
    }
}

static int partition(int[] a, int low, int high) {
    int pivot = a[high];
    int i = low - 1;
    for (int j = low; j < high; j++) {
        if (a[j] <= pivot) {
            i++;
            int tmp = a[i]; a[i] = a[j]; a[j] = tmp;
        }
    }
    int tmp = a[i + 1]; a[i + 1] = a[high]; a[high] = tmp;
    return i + 1;
}
```

Kullanım: `quickSort(arr, 0, arr.length - 1);`

### 3.4 Tam Trace — 7 Elemanlı Dizi

Dizi: `[3, 6, 8, 10, 1, 2, 1]`  Pivot = `a[6] = 1`

```
Başlangıç:  [3, 6, 8, 10, 1, 2, 1]   i=-1, pivot=1
j=0: a[0]=3 > 1  → geç
j=1: a[1]=6 > 1  → geç
j=2: a[2]=8 > 1  → geç
j=3: a[3]=10 > 1 → geç
j=4: a[4]=1 ≤ 1  → i=0, swap(a[0],a[4]) → [1, 6, 8, 10, 3, 2, 1]
j=5: a[5]=2 > 1  → geç
Son swap: swap(a[1], a[6])            → [1, 1, 8, 10, 3, 2, 6]
partition döner: p=1

Sol alt dizi:  [1]           → zaten sıralı
Sağ alt dizi:  [8,10,3,2,6]  → tekrar quickSort(...)

... (özyineleme devam eder) ...

Sonuç: [1, 1, 2, 3, 6, 8, 10]
```

### 3.5 En Kötü Durum (Worst Case)

Dizi zaten sıralı: `[1, 2, 3, 4, 5, 6, 7]`

- Lomuto ile pivot her seferinde **en büyük eleman** olur.
- Partition her seferinde 0 elemanlı sol, n−1 elemanlı sağ üretir.
- **Özyineleme derinliği:** n → T(n) = T(n−1) + O(n) = **O(n²)**

**Çözüm:** Pivot seçimini rastgeleleştir (`random pivot`) ya da "median of three" kullan.

---

## 4. Boilerplate — Tam Derlenebilir Sınıf

```java
import java.util.Arrays;

public class SortDemo {
    public static void main(String[] args) {
        int[] a = {5, 3, 8, 1, 9, 2, 7, 4, 6};
        quickSort(a, 0, a.length - 1);
        System.out.println(Arrays.toString(a)); // [1, 2, 3, 4, 5, 6, 7, 8, 9]
    }

    static void quickSort(int[] a, int lo, int hi) {
        if (lo < hi) {
            int p = partition(a, lo, hi);
            quickSort(a, lo, p - 1);
            quickSort(a, p + 1, hi);
        }
    }

    static int partition(int[] a, int lo, int hi) {
        int pivot = a[hi], i = lo - 1;
        for (int j = lo; j < hi; j++)
            if (a[j] <= pivot) { i++; int t = a[i]; a[i] = a[j]; a[j] = t; }
        int t = a[i+1]; a[i+1] = a[hi]; a[hi] = t;
        return i + 1;
    }
}
```

---

## 5. Sık Tuzaklar

| Hata | Açıklama |
|------|----------|
| `if (low < high)` yerine `if (low <= high)` | Sonsuz özyineleme — tek elemanlı dizide dur |
| Pivot'u swap'lamayı unutmak | Dizinin son konumundaki pivot hiç yerine gelmez |
| `i = low` başlatmak | `i = low - 1` olmalı; ilk swap öncesi konumlanma |
| Merge sort için O(1) alan iddiası | Merge sort O(n) ek alan gerektirir |

---

## 6. Sınavda Nasıl Sorulur?

Lab 7 tarzı değil; ama geçmiş yıllarda ve quiz 3'te:

- "Şu diziyi Quick Sort ile sırala, adımları göster" → tam trace beklenir
- Bir `Comparator` ile generic quick sort yazması istenebilir (Mock Exam 2 – Q2)
- "Neden worst case O(n²)?" → pivot seçim stratejisiyle açıkla

---

## 7. Mini Self-Check

1. `[7, 6, 5, 4, 3, 2, 1]` dizisini Lomuto pivot (son eleman) ile Quick Sort yap. Kaç partition çağrısı olur?
2. Quick Sort neden **kararsız (unstable)**?
3. Insertion Sort hangi durumda Quick Sort'tan daha hızlıdır?

<details>
<summary>Cevaplar</summary>

1. 6 partition çağrısı — her seferinde pivot en küçük, n−1 elemanlı sağ oluşur → worst case ağaç.
2. Partition sırasında eşit elemanlar pivot'un ilerisine taşınabilir; göreli sıra bozulur.
3. Neredeyse sıralı (nearly sorted) dizilerde Insertion Sort O(n)'e yaklaşır; Quick Sort hâlâ O(n log n) ortalaması ama swap maliyeti var.

</details>

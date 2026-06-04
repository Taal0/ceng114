# 01 — Sorting & Searching

> **Öncelik:** MED · **Tahmini soru sayısı:** ~2  
> **Kaynak:** `slides/Week1_.md` (Liang Ch. 7 & 23)

---

## 1. Karmaşıklık & Özellik Tablosu

| Algoritma | En İyi | Ortalama | En Kötü | Yerinde? | Kararlı? |
|-----------|--------|----------|---------|----------|----------|
| Bubble Sort | O(n) | O(n²) | O(n²) | ✓ | ✓ |
| Selection Sort | O(n²) | O(n²) | O(n²) | ✓ | **✗** |
| Insertion Sort | O(n) | O(n²) | O(n²) | ✓ | ✓ |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | **✗** | ✓ |
| **Quick Sort** | O(n log n) | O(n log n) | **O(n²)** | ✓ | **✗** |

**Yerinde (in-place):** Ekstra O(n) bellek gerektirmez.  
**Kararlı (stable):** Eşit elemanların göreli sırası değişmez.

> - **Merge Sort** tek O(n log n) worst-case + stable algoritmadır — ama O(n) ekstra bellek kullanır.  
> - **Selection Sort** asla kararlı değildir (uzun mesafe swap).  
> - **Insertion Sort** neredeyse sıralı dizilerde O(n)'e yaklaşır; bu durumda Quick Sort'tan hızlıdır.

---

## 2. Klasik Sort Kodları (MC izleme)

### Bubble Sort

```java
static void bubbleSort(int[] a) {
    for (int i = 0; i < a.length - 1; i++)
        for (int j = 0; j < a.length - 1 - i; j++)
            if (a[j] > a[j + 1]) {
                int t = a[j]; a[j] = a[j+1]; a[j+1] = t;
            }
}
```

### Insertion Sort

```java
static void insertionSort(int[] a) {
    for (int i = 1; i < a.length; i++) {
        int key = a[i], j = i - 1;
        while (j >= 0 && a[j] > key) { a[j+1] = a[j]; j--; }
        a[j+1] = key;
    }
}
```

---

## 3. Quick Sort — Detaylı

### 3.1 Lomuto Partition — Pseudocode

```
partition(a, low, high):
    pivot = a[high]        // Lomuto: son eleman pivot
    i = low - 1
    for j = low to high-1:
        if a[j] <= pivot:
            i++
            swap(a[i], a[j])
    swap(a[i+1], a[high])  // pivot yerine oturdu
    return i + 1

quickSort(a, low, high):
    if low < high:
        p = partition(a, low, high)
        quickSort(a, low, p - 1)
        quickSort(a, p + 1, high)
```

### 3.2 Java Kodu

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
    for (int j = low; j < high; j++)
        if (a[j] <= pivot) { i++; int t = a[i]; a[i] = a[j]; a[j] = t; }
    int t = a[i+1]; a[i+1] = a[high]; a[high] = t;
    return i + 1;
}
// Çağrı: quickSort(arr, 0, arr.length - 1);
```

### 3.3 Worst Case — Neden O(n²)?

```
Dizi: [1, 2, 3, 4, 5]   (zaten sıralı, Lomuto pivot = son eleman)

partition(0,4): pivot=5 → sol={1,2,3,4}, sağ={}  → bölünme: 4 ve 0
partition(0,3): pivot=4 → sol={1,2,3},   sağ={}  → bölünme: 3 ve 0
...

T(n) = T(n-1) + O(n)  →  O(n²)
```

Çözüm: **random pivot** veya "median of three" seçimi.

---

## 4. Binary Search

```java
static int binarySearch(int[] a, int target) {
    int lo = 0, hi = a.length - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;   // overflow-safe
        if      (a[mid] == target) return mid;
        else if (a[mid] < target)  lo = mid + 1;
        else                       hi = mid - 1;
    }
    return -1;   // bulunamadı
}
```

> Binary search **sıralı dizi** gerektirir. Karmaşıklık: O(log n).

---

## 5. Exceptions & Hatalar

| Durum | Sonuç |
|-------|-------|
| `if (low <= high)` koşulunu `low < high` yerine kullanmak | Tek elemanlı dizide sonsuz özyineleme |
| Sırasız dizi üzerinde binary search | Yanlış sonuç — exception yok ama davranış tanımsız |
| Pivot swap unutulursa | Partition bozuk — dizi yanlış sıralanır |

---

## 6. Sınav Tuzakları (MC)

| # | Tuzak | Açıklama |
|---|-------|----------|
| 1 | **Quick Sort worst case** | Sıralı dizi + Lomuto pivot → O(n²), O(n log n) değil |
| 2 | **Merge Sort bellek** | Stable ve O(n log n) worst-case, ama O(n) ekstra bellek gerektirir |
| 3 | **Selection Sort unstable** | Uzun mesafe swap yüzünden eşit elemanlar yer değiştirir |
| 4 | **Insertion Sort nearly-sorted** | En iyi O(n) — neredeyse sıralı dizilerde Quick Sort'u geçer |
| 5 | **Binary Search ön koşulu** | Dizi **sıralı** olmalı; değilse yanlış sonuç verir |

---

## 7. Self-Check

1. Tek stable VE O(n log n) worst-case algoritma hangisi?
2. Quick Sort neden unstable?
3. `[7,6,5,4,3,2,1]` Lomuto pivot ile kaç partition çağrısı olur?
4. Binary search O(log n) — 1024 elemanlı dizide en fazla kaç karşılaştırma?

<details>
<summary>Cevaplar</summary>

1. Merge Sort.
2. Partition sırasında eşit elemanlar pivot'un ilerisine taşınabilir; göreli sıra bozulur.
3. 6 partition — her seferinde pivot en küçük, n−1 elemanlı sağ alt dizi oluşur (worst case ağaç).
4. log₂(1024) = 10 → en fazla 10 karşılaştırma.

</details>

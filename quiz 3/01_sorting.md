# 01 — Sorting (Sıralama Algoritmaları)

> **Quiz uyarısı:** Hoca **Insertion** dediyse Bubble yazma. Algoritmaları **mantığından** ezberle, kodun karakteristik kısmını gör.

---

## Genel Şablon (hepsi için)

```java
public class SortDemo {
    public static void main(String[] args) {
        int[] a = {5, 2, 4, 6, 1, 3};
        sort(a);
        for (int x : a) System.out.print(x + " ");
    }

    public static void sort(int[] a) { /* algoritma buraya */ }
}
```

---

## 1) Bubble Sort

**Mantık:** Her geçişte komşu iki elemanı kıyasla, soldaki sağdan büyükse swap. Böylece **en büyük eleman sona "yuvarlanır"**. Her geçişte 1 eleman daha "kesin yerleşmiş" olur.

```java
public static void bubbleSort(int[] a) {
    int n = a.length;
    for (int i = 0; i < n - 1; i++) {
        boolean swapped = false;
        for (int j = 0; j < n - i - 1; j++) {
            if (a[j] > a[j + 1]) {
                int tmp = a[j];
                a[j] = a[j + 1];
                a[j + 1] = tmp;
                swapped = true;
            }
        }
        if (!swapped) break; // optimizasyon: zaten sıralıysa çık
    }
}
```

**Trace `[5, 2, 4, 1]`:**
```
Pass 1: [5,2,4,1] → [2,5,4,1] → [2,4,5,1] → [2,4,1,5]   (5 sağa yuvarlandı)
Pass 2: [2,4,1,5] → [2,4,1,5] → [2,1,4,5]                (4 yerine geçti)
Pass 3: [2,1,4,5] → [1,2,4,5]
```

**Karakteristik (kodu görünce tanı):**
- İç döngü `for (int j = 0; j < n - i - 1; j++)` → **bu Bubble!**
- Hep **komşu** karşılaştırma: `a[j]` vs `a[j+1]`

---

## 2) Selection Sort

**Mantık:** Sıralanmamış kısımdaki **en küçük elemanın indexini bul**, sıralanmamış kısmın **başıyla swap** yap. Tek bir swap'le yer değiştirir, Bubble gibi sürekli değil.

```java
public static void selectionSort(int[] a) {
    int n = a.length;
    for (int i = 0; i < n - 1; i++) {
        int minIndex = i;
        for (int j = i + 1; j < n; j++) {
            if (a[j] < a[minIndex]) {
                minIndex = j;
            }
        }
        // turun sonunda tek swap
        if (minIndex != i) {
            int tmp = a[i];
            a[i] = a[minIndex];
            a[minIndex] = tmp;
        }
    }
}
```

**Trace `[5, 2, 4, 1]`:**
```
i=0: min(2,4,1)=1, swap → [1,2,4,5]   (Wait: a=[5,2,4,1] → minIndex=3, swap a[0]↔a[3] → [1,2,4,5])
i=1: min(2,4,5)=2, swap a[1]↔a[1] → [1,2,4,5]
i=2: min(4,5)=4, swap a[2]↔a[2] → [1,2,4,5]
```

**Karakteristik:**
- `int minIndex = i;` → **bu Selection!**
- Swap iç döngünün **dışında** (her turda 1 swap)

---

## 3) Insertion Sort

**Mantık:** Eline yeni bir kart aldın; **elindeki sıralı kartların arasında doğru yere sokuyorsun**. Sağdaki büyük olanları **bir kaydırıyorsun**, açılan deliğe yerleştiriyorsun. İskambil oyunundaki gibi.

```java
public static void insertionSort(int[] a) {
    for (int i = 1; i < a.length; i++) {
        int key = a[i];
        int j = i - 1;
        // key'den büyük olanları sağa kaydır
        while (j >= 0 && a[j] > key) {
            a[j + 1] = a[j];
            j--;
        }
        a[j + 1] = key; // doğru yere yerleştir
    }
}
```

**Trace `[5, 2, 4, 1]`:**
```
i=1, key=2: 5>2 kaydır → [5,5,4,1] → yerleştir → [2,5,4,1]
i=2, key=4: 5>4 kaydır → [2,5,5,1] → 2<4 dur → [2,4,5,1]
i=3, key=1: hepsi >1, hepsini kaydır → [1,2,4,5]
```

**Karakteristik:**
- `int key = a[i];` ve `while (j >= 0 && a[j] > key)` → **bu Insertion!**
- **Kaydırma** var, swap yok
- Dış döngü `i = 1`'den başlar (i=0 zaten sıralı sayılır)

---

## 4) Merge Sort

**Mantık:** Diziyi ortadan **ikiye böl**, her parçayı **recursive sırala**, sonra **sıralı iki parçayı birleştir** (merge).

```java
public static void mergeSort(int[] a) {
    if (a.length > 1) {
        // 1) BÖL
        int[] left = new int[a.length / 2];
        System.arraycopy(a, 0, left, 0, left.length);
        mergeSort(left);

        int[] right = new int[a.length - left.length];
        System.arraycopy(a, left.length, right, 0, right.length);
        mergeSort(right);

        // 2) BİRLEŞTİR
        merge(left, right, a);
    }
}

private static void merge(int[] left, int[] right, int[] result) {
    int i = 0, j = 0, k = 0;
    while (i < left.length && j < right.length) {
        if (left[i] <= right[j]) result[k++] = left[i++];
        else                     result[k++] = right[j++];
    }
    while (i < left.length)  result[k++] = left[i++];
    while (j < right.length) result[k++] = right[j++];
}
```

**Karakteristik:**
- Recursive + iki alt dizi (`left`, `right`)
- `merge` fonksiyonu **iki sıralı diziyi birleştirir**

---

## 5) Quick Sort

**Mantık:** Bir **pivot** seç (ilk eleman). Pivot'tan **küçükleri sola, büyükleri sağa** ata (partition). Sol parçayı recursive sırala, sağı recursive sırala.

```java
public static void quickSort(int[] a) {
    quickSort(a, 0, a.length - 1);
}

private static void quickSort(int[] a, int first, int last) {
    if (last > first) {
        int pivotIndex = partition(a, first, last);
        quickSort(a, first, pivotIndex - 1);
        quickSort(a, pivotIndex + 1, last);
    }
}

private static int partition(int[] a, int first, int last) {
    int pivot = a[first];
    int low = first + 1;
    int high = last;

    while (high > low) {
        while (low <= high && a[low] <= pivot) low++;
        while (low <= high && a[high] > pivot) high--;
        if (high > low) { // swap
            int tmp = a[high]; a[high] = a[low]; a[low] = tmp;
        }
    }
    while (high > first && a[high] >= pivot) high--;
    if (pivot > a[high]) {
        a[first] = a[high];
        a[high] = pivot;
        return high;
    }
    return first;
}
```

**Karakteristik:**
- "pivot" kelimesi ve "partition" fonksiyonu
- Recursive + tek dizi üzerinde **in-place**

---

## Karşılaştırma Tablosu

| | En iyi | Ortalama | En kötü | Stable? | In-place? |
|---|---|---|---|---|---|
| Bubble | O(n) | O(n²) | O(n²) | ✓ | ✓ |
| Selection | O(n²) | O(n²) | O(n²) | ✗ | ✓ |
| Insertion | O(n) | O(n²) | O(n²) | ✓ | ✓ |
| Merge | O(n log n) | O(n log n) | O(n log n) | ✓ | ✗ |
| Quick | O(n log n) | O(n log n) | O(n²) | ✗ | ✓ |

---

## Comparable ile Generic Sort (slayttaki tarz)

Hoca `int[]` yerine **`E[] extends Comparable<E>`** istiyorsa:

```java
public static <E extends Comparable<E>> void selectionSort(E[] a) {
    for (int i = 0; i < a.length - 1; i++) {
        E currentMin = a[i];
        int currentMinIndex = i;
        for (int j = i + 1; j < a.length; j++) {
            if (currentMin.compareTo(a[j]) > 0) {
                currentMin = a[j];
                currentMinIndex = j;
            }
        }
        if (currentMinIndex != i) {
            a[currentMinIndex] = a[i];
            a[i] = currentMin;
        }
    }
}
```

> `compareTo` döndürdüğü değer: **negatif** = küçük, **0** = eşit, **pozitif** = büyük.

---

## Quiz'de Sorulabilecek Tipik Görevler

1. "Insertion sort kullanarak `int[]` diziyi **azalan** sırala." → koşulu `a[j] < key` yap.
2. "Selection sort'la `String[]` sırala." → `s1.compareTo(s2)` kullan.
3. "Bubble sort'u **early-exit optimizasyonuyla** yaz." → `swapped` bayrağı.
4. "Verilen `[7,3,9,1]` için **her geçişten sonraki** durumu yazdır." → her dış iter sonu `print`.

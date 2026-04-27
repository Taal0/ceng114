# 02 — Searching (Arama Algoritmaları)

> **Kritik:** Binary Search **DİZİ SIRALI olmadan çalışmaz**. Linear ise her dizide çalışır.

---

## 1) Linear Search (Sıralı Arama / Doğrusal Arama)

**Mantık:** Diziye baştan sona bak. Eşleşen elemanı bulursan **indexini** döndür, bulamazsan **-1**.

```java
public static int linearSearch(int[] list, int key) {
    for (int i = 0; i < list.length; i++) {
        if (list[i] == key) {
            return i;
        }
    }
    return -1;
}
```

**Trace `linearSearch([4, 5, 3, 7, 9], 7)`:**
```
i=0: 4 == 7? hayır
i=1: 5 == 7? hayır
i=2: 3 == 7? hayır
i=3: 7 == 7? EVET → return 3
```

**Karakteristik:**
- Tek `for` döngüsü
- Karşılaştırma `==` (int için), `equals` (object için)

**Complexity:** O(n)

---

## 2) Binary Search (İkili Arama)

**Mantık:** Dizi **sıralı** olmalı. Ortadaki elemana bak:
- Aradığın **eşitse** → buldun, indexini döndür.
- Aradığın **küçükse** → sol yarıda ara (`high = mid - 1`).
- Aradığın **büyükse** → sağ yarıda ara (`low = mid + 1`).

```java
public static int binarySearch(int[] list, int key) {
    int low = 0;
    int high = list.length - 1;

    while (high >= low) {
        int mid = (low + high) / 2;
        if (key < list[mid]) {
            high = mid - 1;
        } else if (key == list[mid]) {
            return mid;
        } else {
            low = mid + 1;
        }
    }
    return -low - 1; // Liang konvansiyonu: insertion point - 1
}
```

**Trace `binarySearch([1, 3, 5, 7, 9, 11], 7)`:**
```
low=0, high=5, mid=2: list[2]=5 < 7 → low=3
low=3, high=5, mid=4: list[4]=9 > 7 → high=3
low=3, high=3, mid=3: list[3]=7 == 7 → return 3
```

**Karakteristik:**
- `low`, `high`, `mid` değişkenleri
- `mid = (low + high) / 2`
- `while (high >= low)` veya `while (low <= high)`
- Sıralı dizi gerekli

**Complexity:** O(log n)

---

## Recursive Binary Search

```java
public static int binarySearch(int[] list, int key, int low, int high) {
    if (low > high) return -low - 1; // bulunamadı
    int mid = (low + high) / 2;
    if (key < list[mid])
        return binarySearch(list, key, low, mid - 1);
    else if (key == list[mid])
        return mid;
    else
        return binarySearch(list, key, mid + 1, high);
}
```

---

## Sık Yapılan Hatalar

| Hata | Çözüm |
|---|---|
| Binary'yi sıralanmamış diziye uygulamak | Önce `Arrays.sort(a)` |
| `mid = (low + high) / 2` overflow (büyük n için) | `mid = low + (high - low) / 2` |
| `while (low < high)` yazmak | `while (low <= high)` (eşitlik şart) |
| Bulunamadığında `0` döndürmek | `-1` döndür (veya Liang: `-low - 1`) |
| String aramada `==` kullanmak | `key.equals(list[i])` |

---

## Quiz'de Sorulabilecek

1. "Verilen `int[]` ve `key` için **binary search** yaz." → mid mantığı.
2. "Linear search'ü `String[]` için yaz." → `equals` kullan.
3. "Recursive binary search yaz."
4. "`Arrays.binarySearch(a, k)` kullanmadan sıralı dizide ara." → kendin yaz.

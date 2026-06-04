# 09 — Recursion

> **Öncelik:** LOW · **Tahmini soru sayısı:** ~2  
> **Kaynak:** `slides/18slide_accessible.md` (Liang Ch. 18)

---

## 1. Temel Kavramlar

| Terim | Açıklama |
|-------|----------|
| **Recursive method** | Kendini çağıran metot |
| **Base case** | Özyinelemeyi durduran koşul — **zorunlu** |
| **Recursive case** | Sorunu küçülterek tekrar kendini çağıran kısım |
| **Call stack** | Her recursive çağrı yeni bir stack frame açar |
| **Stack overflow** | Base case yoksa veya hiç ulaşılamazsa call stack dolar |

---

## 2. Factorial

```java
// Recursive
static long factorial(int n) {
    if (n == 0) return 1;          // base case
    return n * factorial(n - 1);   // recursive case
}

// Iterative karşılığı
static long factorialIter(int n) {
    long result = 1;
    for (int i = 2; i <= n; i++) result *= i;
    return result;
}
```

**Trace:** `factorial(4)`

```
factorial(4)
  → 4 * factorial(3)
         → 3 * factorial(2)
                → 2 * factorial(1)
                       → 1 * factorial(0)
                              → 1  (base case)
                       → 1 * 1 = 1
                → 2 * 1 = 2
         → 3 * 2 = 6
  → 4 * 6 = 24
```

---

## 3. Fibonacci

```java
static int fib(int n) {
    if (n <= 1) return n;             // base case: fib(0)=0, fib(1)=1
    return fib(n - 1) + fib(n - 2);  // recursive case
}
// DİKKAT: Naive recursive fib O(2^n) — büyük n'ler için çok yavaş
```

---

## 4. Binary Search — Recursive

```java
static int binarySearch(int[] a, int target, int lo, int hi) {
    if (lo > hi) return -1;                    // base case: bulunamadı
    int mid = lo + (hi - lo) / 2;
    if (a[mid] == target) return mid;           // base case: bulundu
    if (a[mid] < target)  return binarySearch(a, target, mid + 1, hi);
    else                  return binarySearch(a, target, lo, mid - 1);
}
// Kullanım: binarySearch(arr, key, 0, arr.length - 1)
```

---

## 5. Recursion vs Iteration

| | Recursion | Iteration |
|-|-----------|-----------|
| Kod okunabilirliği | Genellikle daha kısa/açık | Daha uzun ama belleği verimli |
| Bellek | Her çağrı stack frame kullanır | Sabit bellek |
| Hız | Stack frame overhead var | Genellikle daha hızlı |
| Risk | Base case unutulursa `StackOverflowError` | Sonsuz döngü |
| Kullanım | Ağaç/graf gezme, divide-and-conquer | Basit döngüler, sayaç |

---

## 6. Exceptions & Hatalar

| Exception | Ne Zaman |
|-----------|----------|
| `StackOverflowError` | Base case yok veya ulaşılamaz; özyineleme sonsuz devam eder |

```java
// StackOverflowError örneği — base case YOK
static int badFactorial(int n) {
    return n * badFactorial(n - 1);  // n=0'a ulaşınca dur olmalıydı!
}
```

> `StackOverflowError` bir `Error`'dur — `Exception` değil. Yakalamaya çalışma.

---

## 7. Sınav Tuzakları (MC)

| # | Tuzak | Açıklama |
|---|-------|----------|
| 1 | **Base case yoksa** | `StackOverflowError` fırlar — "hangi hata oluşur?" sorusu |
| 2 | **Fibonacci O(2^n)** | Naive recursive fib üstel zaman — büyük n için impractical |
| 3 | **Call stack çağrı sırası** | Her recursive çağrı yeni stack frame açar; dönüş geriye doğru işler |
| 4 | **`StackOverflowError` tip** | `Error` — `RuntimeException` değil; yakalamazsın |

---

## 8. Self-Check

1. `factorial(5)` kaç kez çağrı yapar (main hariç)?
2. Base case olmayan bir recursive metodda ne olur?
3. Recursive binary search kaç kez çağrı yapar? (16 elemanlı dizi, hedef yok)
4. `StackOverflowError` checked mi, unchecked mi?

<details>
<summary>Cevaplar</summary>

1. 5 çağrı: `factorial(5) → factorial(4) → ... → factorial(0)`.
2. `StackOverflowError` — call stack taşar.
3. log₂(16) + 1 = 5 çağrı (her adımda dizi yarıya iner, base case `lo > hi`'de 1 çağrı daha).
4. **Unchecked** — ama aslında `Error` alt sınıfı; `RuntimeException` değil. Her halükarda yakalamazsın.

</details>

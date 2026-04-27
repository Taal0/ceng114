# 03 — Recursion (Özyineleme)

> **Mantık:** Bir method **kendini çağırır**. Her recursive method'da iki şey olmak zorundadır:
> 1. **Base case** (taban durum): recursion'ı durduran koşul.
> 2. **Recursive case**: problemi daha küçük bir versiyonuna indirgeyen kendine-çağrı.

---

## Recursion İskelet

```java
public static T solve(...) {
    if (BASE_CASE) {
        return TRIVIAL_ANSWER;
    }
    // küçük probleme indirgeme
    return COMBINE( solve(SMALLER_INPUT), ... );
}
```

---

## 1) Factorial (n!)

**Tanım:** `0! = 1`, `n! = n * (n-1)!`

```java
public static long factorial(int n) {
    if (n == 0)              // base case
        return 1;
    return n * factorial(n - 1);  // recursive case
}
```

**Trace `factorial(4)`:**
```
factorial(4) = 4 * factorial(3)
             = 4 * (3 * factorial(2))
             = 4 * (3 * (2 * factorial(1)))
             = 4 * (3 * (2 * (1 * factorial(0))))
             = 4 * 3 * 2 * 1 * 1 = 24
```

---

## 2) Fibonacci

**Tanım:** `fib(0)=0`, `fib(1)=1`, `fib(n) = fib(n-1) + fib(n-2)`

```java
public static long fib(int n) {
    if (n <= 1) return n;          // 2 base case birden
    return fib(n - 1) + fib(n - 2);
}
```

**Dikkat:** Bu naive versiyon **çok yavaştır** (`O(2^n)`). Quiz'de bunu yazmak yeterli ama hoca "verimli yaz" derse iteratif yap:

```java
public static long fibIter(int n) {
    if (n <= 1) return n;
    long a = 0, b = 1;
    for (int i = 2; i <= n; i++) {
        long c = a + b;
        a = b;
        b = c;
    }
    return b;
}
```

---

## 3) Sum of Array

```java
public static int sum(int[] a, int index) {
    if (index >= a.length) return 0;          // base
    return a[index] + sum(a, index + 1);      // recursive
}
// Çağrı: sum(arr, 0)
```

---

## 4) Power (x^n)

```java
public static double power(double x, int n) {
    if (n == 0) return 1;
    if (n < 0)  return 1.0 / power(x, -n);
    return x * power(x, n - 1);
}
```

---

## 5) Reverse a String

```java
public static String reverse(String s) {
    if (s.length() <= 1) return s;
    return reverse(s.substring(1)) + s.charAt(0);
}
```

**Trace `reverse("abc")`:**
```
reverse("abc") = reverse("bc") + 'a'
              = (reverse("c") + 'b') + 'a'
              = ("c" + "b") + "a" = "cba"
```

---

## 6) Palindrome Check

```java
public static boolean isPalindrome(String s) {
    if (s.length() <= 1) return true;
    if (s.charAt(0) != s.charAt(s.length() - 1)) return false;
    return isPalindrome(s.substring(1, s.length() - 1));
}
```

---

## 7) GCD (Euclid)

```java
public static int gcd(int a, int b) {
    if (b == 0) return a;
    return gcd(b, a % b);
}
```

---

## 8) Tower of Hanoi (klasik)

```java
public static void hanoi(int n, char from, char to, char via) {
    if (n == 1) {
        System.out.println("Move disk 1 from " + from + " to " + to);
        return;
    }
    hanoi(n - 1, from, via, to);
    System.out.println("Move disk " + n + " from " + from + " to " + to);
    hanoi(n - 1, via, to, from);
}
// Çağrı: hanoi(3, 'A', 'C', 'B')
```

---

## Recursion vs Iteration

| | Recursion | Iteration |
|---|---|---|
| Yazım | Kısa, doğal | Daha uzun olabilir |
| Bellek | Stack frame harcar | Sabit bellek |
| Risk | StackOverflowError | yok |
| Tipik kullanım | Ağaç, böl-ve-yönet | Basit döngüler |

---

## Sık Yapılan Hatalar

1. **Base case yok / yanlış** → sonsuz recursion → `StackOverflowError`.
2. **Recursive çağrı küçük input'a inmiyor** → yine sonsuz.
   - `factorial(n)` içinde `factorial(n)` çağırmak (tipik hata) → `factorial(n-1)` olmalı.
3. **Return etmeyi unutmak** → `solve(n-1);` yerine `return solve(n-1);`.
4. **Yanlış birleştirme** → `factorial(n-1) * n` ile `n * factorial(n-1)` aynı ama `n - factorial(n-1)` yanlış.

---

## Quiz'de Sorulabilecek

1. "Recursive olarak bir sayının **basamak toplamını** bul." → `n%10 + sumDigits(n/10)`
2. "Recursive olarak bir String içinde **bir karakteri say**."
3. "Recursive `power(x, n)` yaz."
4. "Recursive olarak bir dizinin **maksimumunu** bul."

```java
// Bonus: Maksimum (recursive)
public static int max(int[] a, int index) {
    if (index == a.length - 1) return a[index];
    return Math.max(a[index], max(a, index + 1));
}
```

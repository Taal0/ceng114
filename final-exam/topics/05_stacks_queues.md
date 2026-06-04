# 05 — Stacks & Queues

> **Öncelik:** MED · **Tahmini soru sayısı:** ~2  
> **Kaynak:** `slides/Copy of 24slide_accessible.md` (Liang Ch. 24)

---

## 1. Stack vs Queue

| | Stack | Queue |
|-|-------|-------|
| Prensip | **LIFO** (Last In, First Out) | **FIFO** (First In, First Out) |
| Ekleme yeri | Tepe (top) | Arka (rear) |
| Çıkarma yeri | Tepe (top) | Ön (front) |
| Uygulama | Geri alma, DFS, çağrı yığını, parantez eşleştirme | BFS, yazıcı kuyruğu, işlem sırası |
| Java stdlib | `ArrayDeque` — `push`/`pop` | `ArrayDeque` — `offer`/`poll` |

---

## 2. Stack Operasyonları

```
push(A) → push(B) → push(C)

  top → [C]
        [B]
        [A]

pop()  → C döner, tepede B kalır
peek() → B döner, kaldırmaz
```

| Operasyon | Açıklama | Karmaşıklık |
|-----------|----------|-------------|
| `push(e)` | Tepeye ekle | O(1) |
| `pop()` | Tepedekini çıkar & döndür | O(1) |
| `peek()` | Tepedekini döndür (kaldırmaz) | O(1) |
| `isEmpty()` | Boş mu? | O(1) |

---

## 3. Queue Operasyonları

```
enqueue(A) → enqueue(B) → enqueue(C)

front → [A] [B] [C] ← rear

dequeue() → A (öndeki çıkar)
peek()    → A (kaldırmaz)
```

| Operasyon | Açıklama | Karmaşıklık |
|-----------|----------|-------------|
| `enqueue(e)` / `offer(e)` | Kuyruğa sona ekle | O(1) |
| `dequeue()` / `poll()` | Önden çıkar & döndür | O(1) |
| `peek()` | Ön elemanı döndür | O(1) |

---

## 4. Linked-List Tabanlı Stack

```java
import java.util.EmptyStackException;

public class LinkedStack<T> {
    private static class Node<T> {
        T data; Node<T> next;
        Node(T data, Node<T> next) { this.data = data; this.next = next; }
    }

    private Node<T> top;
    private int size;

    public void push(T item) {
        top = new Node<>(item, top);   // yeni node'un next'i eski top
        size++;
    }

    public T pop() {
        if (isEmpty()) throw new EmptyStackException();
        T item = top.data;
        top = top.next;
        size--;
        return item;
    }

    public T peek() {
        if (isEmpty()) throw new EmptyStackException();
        return top.data;
    }

    public boolean isEmpty() { return top == null; }
    public int size() { return size; }
}
```

---

## 5. Linked-List Tabanlı Queue

```java
import java.util.NoSuchElementException;

public class LinkedQueue<T> {
    private static class Node<T> {
        T data; Node<T> next;
        Node(T data) { this.data = data; }
    }

    private Node<T> head, tail;
    private int size;

    public void enqueue(T item) {
        Node<T> n = new Node<>(item);
        if (tail == null) { head = tail = n; }
        else { tail.next = n; tail = n; }
        size++;
    }

    public T dequeue() {
        if (isEmpty()) throw new NoSuchElementException();
        T item = head.data;
        head = head.next;
        if (head == null) tail = null;   // liste boşaldı — tail'i temizle!
        size--;
        return item;
    }

    public T peek() {
        if (isEmpty()) throw new NoSuchElementException();
        return head.data;
    }

    public boolean isEmpty() { return head == null; }
    public int size() { return size; }
}
```

---

## 6. Java Standart Kütüphane

```java
import java.util.Deque;
import java.util.ArrayDeque;

// Stack olarak kullan
Deque<Integer> stack = new ArrayDeque<>();
stack.push(1);    // addFirst
stack.push(2);
stack.pop();      // 2 — removeFirst
stack.peek();     // 1

// Queue olarak kullan
Deque<Integer> queue = new ArrayDeque<>();
queue.offer(1);   // addLast
queue.offer(2);
queue.poll();     // 1 — removeFirst
queue.peek();     // 2
```

> `java.util.Stack` eski (legacy) sınıf — tercih et `ArrayDeque`.

---

## 7. Yaygın Uygulama — Parantez Eşleştirme

```java
static boolean isBalanced(String expr) {
    Deque<Character> stack = new ArrayDeque<>();
    for (char c : expr.toCharArray()) {
        if (c == '(' || c == '[' || c == '{') stack.push(c);
        else if (c == ')' || c == ']' || c == '}') {
            if (stack.isEmpty()) return false;
            char top = stack.pop();
            if ((c == ')' && top != '(') ||
                (c == ']' && top != '[') ||
                (c == '}' && top != '{')) return false;
        }
    }
    return stack.isEmpty();   // açılmamış parantez kalmadıysa true
}
```

---

## 8. Exceptions & Hatalar

| Exception | Ne Zaman |
|-----------|----------|
| `EmptyStackException` | Boş stack'te `pop()` veya `peek()` |
| `NoSuchElementException` | Boş queue'da `dequeue()` veya `peek()` |
| Dangling pointer (logic error) | Queue'da son eleman çıkarılınca `tail = null` yapılmazsa |

---

## 9. Sınav Tuzakları (MC)

| # | Tuzak | Açıklama |
|---|-------|----------|
| 1 | **Boş stack'te pop/peek** | `EmptyStackException` — her zaman `isEmpty()` kontrol et |
| 2 | **Queue'da stale tail** | Son `dequeue` sonrası `if (head == null) tail = null` unutulursa dangling pointer |
| 3 | **LIFO vs FIFO** | Stack → LIFO; Queue → FIFO; karıştırılırsa pop/dequeue sırası yanlış |
| 4 | **`java.util.Stack` kullanımı** | Thread-safe ama yavaş; sınavda genellikle `ArrayDeque` tercih edilir |

---

## 10. Self-Check

1. `push(1), push(2), push(3), pop(), push(4), pop(), pop()` — çıkan değerler sırasıyla nedir?
2. Queue: `enqueue(A), enqueue(B), dequeue(), enqueue(C), dequeue()` — çıkan değerler?
3. Queue'da son eleman dequeue edilince neden `tail = null` yapılması gerekir?

<details>
<summary>Cevaplar</summary>

1. `3, 4, 2` — stack'te `[1]` kalır.
2. `A, B` — queue'da `[C]` kalır.
3. `head` `null` olur; ama `tail` hâlâ o son düğümü işaret eder. Sonraki `enqueue`'da `tail.next = newNode` yapılacak, ama `tail` zaten geçersiz bir düğüm → dangling pointer + yanlış davranış.

</details>

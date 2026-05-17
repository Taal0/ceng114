# 05 — Stacks & Queues

> **Kaynaklar:** `slides/Copy of 24slide_accessible.md` (Ch. 24) · Liang Ch. 24

---

## 1. Neden Bu Konu Var?

Stack ve Queue, Linked List'in iki özel kullanım şeklidir. Gerçek dünyada parantez kontrolü (derleyiciler), geri alma (undo), BFS (graf arama) gibi onlarca yerde kullanılır.

---

## 2. Anahtar Kavramlar

### 2.1 Stack — LIFO (Last In, First Out)

```
push(A) → push(B) → push(C)

  top → [C]
        [B]
        [A]

pop() → C,  peek() → B (kaldırmaz)
```

| Operasyon | Açıklama | Karmaşıklık |
|-----------|----------|-------------|
| `push(e)` | Tepeye ekle | O(1) |
| `pop()` | Tepedekini çıkar & döndür | O(1) |
| `peek()` | Tepedekini döndür (kaldırma) | O(1) |
| `isEmpty()` | Boş mu? | O(1) |
| `size()` | Eleman sayısı | O(1) |

### 2.2 Queue — FIFO (First In, First Out)

```
enqueue(A) → enqueue(B) → enqueue(C)

front → [A] [B] [C] ← rear

dequeue() → A (öndeki çıkar)
peek()    → A (kaldırma)
```

| Operasyon | Açıklama | Karmaşıklık |
|-----------|----------|-------------|
| `enqueue(e)` | Kuyruğa sona ekle | O(1) |
| `dequeue()` | Önden çıkar & döndür | O(1) |
| `peek()` | Ön elemanı döndür | O(1) |
| `isEmpty()` | Boş mu? | O(1) |

---

## 3. Implementasyonlar

### 3.1 Linked-List Tabanlı Stack

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

### 3.2 Array Tabanlı Stack

```java
import java.util.EmptyStackException;

public class ArrayStack<T> {
    private Object[] data;
    private int top = -1;

    @SuppressWarnings("unchecked")
    public ArrayStack(int capacity) { data = new Object[capacity]; }

    public void push(T item) {
        if (top == data.length - 1) throw new RuntimeException("Stack dolu");
        data[++top] = item;
    }

    @SuppressWarnings("unchecked")
    public T pop() {
        if (isEmpty()) throw new EmptyStackException();
        T item = (T) data[top];
        data[top--] = null;   // GC için referansı temizle
        return item;
    }

    @SuppressWarnings("unchecked")
    public T peek() {
        if (isEmpty()) throw new EmptyStackException();
        return (T) data[top];
    }

    public boolean isEmpty() { return top == -1; }
    public int size() { return top + 1; }
}
```

### 3.3 Linked-List Tabanlı Queue (head = front, tail = rear)

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
        Node<T> newNode = new Node<>(item);
        if (tail == null) { head = tail = newNode; }
        else { tail.next = newNode; tail = newNode; }
        size++;
    }

    public T dequeue() {
        if (isEmpty()) throw new NoSuchElementException();
        T item = head.data;
        head = head.next;
        if (head == null) tail = null;   // liste boşaldı
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

## 4. Java Standart Kütüphane Eşdeğerleri

```java
import java.util.Deque;
import java.util.ArrayDeque;

// Stack olarak kullan
Deque<Integer> stack = new ArrayDeque<>();
stack.push(1);       // addFirst
stack.push(2);
System.out.println(stack.pop());    // 2 (removeFirst)
System.out.println(stack.peek());   // 1 (peekFirst)

// Queue olarak kullan
Deque<Integer> queue = new ArrayDeque<>();
queue.offer(1);      // addLast
queue.offer(2);
System.out.println(queue.poll());   // 1 (removeFirst)
System.out.println(queue.peek());   // 2 (peekFirst)
```

> `java.util.Stack` sınıfı eski (legacy) — sınavda `ArrayDeque` veya kendi implementasyonun tercih edilir.

---

## 5. Yaygın Uygulamalar

### 5.1 Parantez Eşleştirme (Stack)

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
    return stack.isEmpty();
}
```

### 5.2 BFS — Genişlik Öncelikli Arama (Queue)

```java
// Graf düğümleri için — Queue ile katman katman gezme
Queue<Integer> queue = new LinkedList<>();
boolean[] visited = new boolean[n];
queue.offer(start);
visited[start] = true;
while (!queue.isEmpty()) {
    int node = queue.poll();
    for (int neighbor : graph[node]) {
        if (!visited[neighbor]) {
            visited[neighbor] = true;
            queue.offer(neighbor);
        }
    }
}
```

---

## 6. Stack vs. Queue Karşılaştırması

| Özellik | Stack | Queue |
|---------|-------|-------|
| Prensip | LIFO | FIFO |
| Ekleme yeri | Tepe | Arka (rear) |
| Çıkarma yeri | Tepe | Ön (front) |
| Kullanım | Geri alma, DFS, çağrı yığını | BFS, yazdırma kuyruğu, işlem sırası |
| Java stdlib | `ArrayDeque` (push/pop) | `ArrayDeque` (offer/poll) |

---

## 7. Sık Tuzaklar

| Tuzak | Açıklama |
|-------|----------|
| `pop()` boş stack'te | `EmptyStackException` — her zaman `isEmpty()` kontrol et |
| Queue'da `tail = null` unutmak | Son dequeue'da tail dangling kalır |
| Array stack: `data[top--] = null` yazmamak | GC leak — null ile temizle |
| `java.util.Stack` kullanımı | Thread-safe ama yavaş — tercih et `ArrayDeque` |

---

## 8. Sınavda Nasıl Sorulur?

- "Linked list tabanlı generic Stack/Queue yaz" → Mock Exam 1 – Q1
- "Parantez eşleştirme algoritması" → Stack uygulaması
- "Stack pop sırası ne?" → LIFO trace sorusu

---

## 9. Mini Self-Check

1. `push(1), push(2), push(3), pop(), push(4), pop(), pop()` sonucu ne?
2. Queue'da `enqueue(A), enqueue(B), dequeue(), enqueue(C), dequeue()` sonucu ne?
3. Tek bir stack kullanarak queue implementasyonu mümkün mü?

<details>
<summary>Cevaplar</summary>

1. pop sırası: 3, 4, 2 — stack'te kalan: [1]
2. dequeue sırası: A, B — queue'da kalan: [C]
3. Evet — iki stack ile (push stack + pop stack) Queue simüle edilebilir; amortize O(1).

</details>

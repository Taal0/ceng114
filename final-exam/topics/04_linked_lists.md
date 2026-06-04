# 04 — Linked Lists

> **Öncelik:** HIGH · **Tahmini soru sayısı:** ~4  
> **Kaynak:** `slides/CENG114_Weeks13_14_LinkedLists (2).md`

---

## 1. Temel Kavramlar

### 1.1 Node Yapısı

```java
// Singly Linked List (SLL) node
class Node<T> {
    T data;
    Node<T> next;
    Node(T data) { this.data = data; this.next = null; }
}

// Doubly Linked List (DLL) node
class DNode<T> {
    T data;
    DNode<T> prev;
    DNode<T> next;
    DNode(T data) { this.data = data; }
}
```

### 1.2 SLL vs DLL Alan Farkı

```java
// SLL — sadece head
private Node<T> head;
private int size;

// DLL — head VE tail
private DNode<T> head;
private DNode<T> tail;
private int size;
```

---

## 2. Karmaşıklık Tablosu

| Operasyon | ArrayList | SLL (tail yok) | DLL (tail var) |
|-----------|-----------|----------------|----------------|
| `get(i)` | **O(1)** | O(n) | O(n) |
| `add` — başa | O(n) | **O(1)** | **O(1)** |
| `add` — sona | O(1) amortize | O(n) | **O(1)** |
| `remove` — baştan | O(n) | **O(1)** | **O(1)** |
| `remove` — sondan | O(1) | O(n) | **O(1)** |
| `contains` | O(n) | O(n) | O(n) |

> **O(n²) tuzağı:** `get(i)` çağrısını bir `for-i` döngüsü içinde LinkedList üzerinde kullanmak → her `get(i)` O(n) → döngü O(n²). Her zaman iterator veya stream tercih et.

---

## 3. Kritik Operasyonlar

### 3.1 SLL — Başa Ekleme (addFirst)

```java
public void addFirst(T data) {
    Node<T> newNode = new Node<>(data);
    newNode.next = head;   // 1. yeni düğümün next'i eski head
    head = newNode;        // 2. head güncellendi
    size++;
}
```

### 3.2 SLL — Ortaya Ekleme (addAt)

```java
public void addAt(int index, T data) {
    if (index < 0 || index > size) throw new IndexOutOfBoundsException();
    if (index == 0) { addFirst(data); return; }
    Node<T> curr = head;
    for (int i = 0; i < index - 1; i++) curr = curr.next;
    Node<T> newNode = new Node<>(data);
    newNode.next = curr.next;  // ÖNCE: yeni düğümü sonrakine bağla
    curr.next = newNode;       // SONRA: önceki düğümü yeni düğüme bağla
    size++;
}
```

> **Pointer sırası kritik!** `curr.next = newNode` ÖNCE yazılırsa zincir kopar — kaybedilen düğümler geri alınamaz.

### 3.3 SLL — Baştan Silme (removeFirst)

```java
public T removeFirst() {
    if (head == null) throw new NoSuchElementException();
    T removed = head.data;
    head = head.next;
    size--;
    return removed;
}
```

### 3.4 SLL — Sondan Silme (removeLast)

```java
public T removeLast() {
    if (head == null) throw new NoSuchElementException();
    if (head.next == null) {           // tek elemanlı liste
        T removed = head.data;
        head = null;
        size--;
        return removed;
    }
    Node<T> curr = head;
    while (curr.next.next != null) curr = curr.next;  // sondan önceki düğüm
    T removed = curr.next.data;
    curr.next = null;
    size--;
    return removed;
}
```

### 3.5 DLL — Sona Ekleme (addLast, O(1))

```java
public void addLast(T data) {
    DNode<T> newNode = new DNode<>(data);
    if (tail == null) { head = tail = newNode; size++; return; }
    newNode.prev = tail;
    tail.next = newNode;
    tail = newNode;
    size++;
}
```

### 3.6 DLL — Sondan Silme (removeLast, O(1))

```java
public T removeLast() {
    if (tail == null) throw new NoSuchElementException();
    T removed = tail.data;
    if (head == tail) {     // tek eleman
        head = tail = null;
    } else {
        tail = tail.prev;
        tail.next = null;   // dangling pointer'ı temizle!
    }
    size--;
    return removed;
}
```

### 3.7 SLL — Ters Çevirme (reverse, in-place)

```java
public void reverse() {
    Node<T> prev = null, curr = head, next;
    while (curr != null) {
        next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    head = prev;
}
```

---

## 4. Edge Case'ler

| Durum | Kontrol Edilmesi Gereken |
|-------|--------------------------|
| **Boş liste** | `head == null` → tüm remove/get işlemlerinde |
| **Tek elemanlı** | `head.next == null` → removeLast'ta `head = null` |
| **DLL son eleman silinince** | `tail = tail.prev; tail.next = null` — yoksa dangling pointer |
| **İndeks sınır dışı** | `index < 0 \|\| index > size` (add) / `index >= size` (get/remove) |

---

## 5. Exceptions & Hatalar

| Exception | Ne Zaman Fırlar |
|-----------|-----------------|
| `NoSuchElementException` | Boş listede `removeFirst()`, `removeLast()` |
| `IndexOutOfBoundsException` | `addAt(-1, ...)` veya `addAt(size+1, ...)` |
| `NullPointerException` | `head.next.next` kontrolü olmadan tek elemanlı listede ilerlemek |

---

## 6. Sınav Tuzakları (MC)

| # | Tuzak | Açıklama |
|---|-------|----------|
| 1 | **Pointer sırası** | `curr.next = newNode` ÖNCE yazılırsa zincirin geri kalanı kaybolur. Doğru: `newNode.next = curr.next` → `curr.next = newNode` |
| 2 | **Stale tail** | DLL'de son düğüm `dequeue` edilince `tail = null` da set edilmeli; yapılmazsa dangling pointer kalır |
| 3 | **get(i) döngüsü** | `for(int i=0; i<list.size(); i++) list.get(i)` bir LinkedList üzerinde O(n²) |
| 4 | **Tek elemanlı silme** | `head.next == null` özel durumunu unutmak → `removeLast`'ta `curr.next.next` NPE verir |
| 5 | **size sayacı** | Her ekleme/silme sonrası `size++`/`size--` unutulursa `size()` yanlış döner |

---

## 7. Self-Check

1. SLL'de `addLast` O(1) yapabilmek için ne gerekir?
2. `addAt` içinde `newNode.next = curr.next` ile `curr.next = newNode` satırlarının yerleri değiştirilirse ne olur?
3. DLL'de `removeLast` O(1) iken SLL'de neden O(n)?
4. Boş bir `SinglyLinkedList`'e `removeLast()` çağrısı yapılırsa hangi exception fırlar?

<details>
<summary>Cevaplar</summary>

1. `tail` pointer tutmak — son düğüme O(1)'de erişim sağlar.
2. `curr.next = newNode` önce yapılırsa `curr.next`'in eski değeri (zincirin devamı) kaybolur; yeni düğümün `next`'i `null` olur → liste o noktadan kesilir.
3. SLL'de sondan önceki düğüme ulaşmak için head'den tarama gerekir (O(n)). DLL'de `tail.prev` pointer'ı sayesinde O(1).
4. `NoSuchElementException` (veya boş kontrol yoksa `NullPointerException`).

</details>

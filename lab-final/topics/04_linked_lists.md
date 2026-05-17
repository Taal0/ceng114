# 04 — Linked Lists

> **Kaynaklar:** `slides/CENG114_Weeks13_14_LinkedLists (2).md` · `github/LinkedList/linkedlistVSarraylist.md`

---

## 1. Neden Bu Konu Var?

Array bellekte ardışık bloklar gerektirir — başa eleman ekleme O(n)'dir (her şeyi kaydır). Linked List ise düğümleri pointer ile bağlar: başa ekleme O(1), rastgele erişim ise O(n).

---

## 2. Anahtar Kavramlar

### 2.1 Node Yapısı

```java
// Singly Linked List node
class Node<T> {
    T data;
    Node<T> next;
    Node(T data) { this.data = data; this.next = null; }
}

// Doubly Linked List node
class DNode<T> {
    T data;
    DNode<T> prev;
    DNode<T> next;
    DNode(T data) { this.data = data; }
}
```

### 2.2 SLL — Singly Linked List Alanları

```java
public class SinglyLinkedList<T> {
    private Node<T> head;  // ilk düğüm
    private int size;
}
```

### 2.3 DLL — Doubly Linked List Alanları

```java
public class DoublyLinkedList<T> {
    private DNode<T> head;
    private DNode<T> tail;
    private int size;
}
```

### 2.4 Karmaşıklık Tablosu — ArrayList vs LinkedList

| Operasyon | ArrayList | LinkedList (SLL) |
|-----------|-----------|-----------------|
| `get(i)` | O(1) | O(n) |
| `add` — sona | O(1) amortize | O(n) SLL / O(1) DLL (tail) |
| `add` — başa | O(n) | O(1) |
| `add` — ortaya | O(n) | O(n) (arama) + O(1) (bağlama) |
| `remove` — baştan | O(n) | O(1) |
| `remove` — sona | O(1) | O(n) SLL / O(1) DLL |
| `remove` — ortadan | O(n) | O(n) |
| `contains` | O(n) | O(n) |
| Bellek | Daha az (no pointers) | Fazla (her node'da pointer) |

---

## 3. Temel Operasyonlar — Kod

### 3.1 SLL — Başa Ekleme

```java
public void addFirst(T data) {
    Node<T> newNode = new Node<>(data);
    newNode.next = head;
    head = newNode;
    size++;
}
```

### 3.2 SLL — Sona Ekleme

```java
public void addLast(T data) {
    Node<T> newNode = new Node<>(data);
    if (head == null) { head = newNode; size++; return; }
    Node<T> curr = head;
    while (curr.next != null) curr = curr.next;
    curr.next = newNode;
    size++;
}
```

### 3.3 SLL — Belirli İndekse Ekleme

```java
public void addAt(int index, T data) {
    if (index < 0 || index > size) throw new IndexOutOfBoundsException();
    if (index == 0) { addFirst(data); return; }
    Node<T> curr = head;
    for (int i = 0; i < index - 1; i++) curr = curr.next;
    Node<T> newNode = new Node<>(data);
    newNode.next = curr.next;
    curr.next = newNode;
    size++;
}
```

### 3.4 SLL — Baştan Silme

```java
public T removeFirst() {
    if (head == null) throw new NoSuchElementException();
    T removed = head.data;
    head = head.next;
    size--;
    return removed;
}
```

### 3.5 SLL — Sona Silme

```java
public T removeLast() {
    if (head == null) throw new NoSuchElementException();
    if (head.next == null) {               // tek elemanlı liste
        T removed = head.data; head = null; size--; return removed;
    }
    Node<T> curr = head;
    while (curr.next.next != null) curr = curr.next;  // sondan önceki
    T removed = curr.next.data;
    curr.next = null;
    size--;
    return removed;
}
```

### 3.6 SLL — Değere Göre Silme

```java
public boolean removeByValue(T data) {
    if (head == null) return false;
    if (head.data.equals(data)) { head = head.next; size--; return true; }
    Node<T> curr = head;
    while (curr.next != null) {
        if (curr.next.data.equals(data)) {
            curr.next = curr.next.next;
            size--;
            return true;
        }
        curr = curr.next;
    }
    return false;
}
```

### 3.7 SLL — Ters Çevirme (in-place)

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

### 3.8 DLL — Sona Ekleme (tail pointer ile)

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

### 3.9 DLL — Sona Silme (tail pointer ile — O(1)!)

```java
public T removeLast() {
    if (tail == null) throw new NoSuchElementException();
    T removed = tail.data;
    if (head == tail) { head = tail = null; }  // tek eleman
    else {
        tail = tail.prev;
        tail.next = null;
    }
    size--;
    return removed;
}
```

---

## 4. Traversal (Geçiş) ve Arama

```java
public void printAll() {
    Node<T> curr = head;
    while (curr != null) {
        System.out.print(curr.data + " → ");
        curr = curr.next;
    }
    System.out.println("null");
}

public boolean contains(T data) {
    Node<T> curr = head;
    while (curr != null) {
        if (curr.data.equals(data)) return true;
        curr = curr.next;
    }
    return false;
}
```

---

## 5. En Tehlikeli Edge Case'ler

| Durum | Ne Kontrol Etmeli |
|-------|------------------|
| **Boş liste** | `head == null` → `removeFirst`, `removeLast`, `contains` hepsinde |
| **Tek elemanlı liste** | `head.next == null` → sona silmede `tail = null`, başa silmede `head = null` |
| **Son node silme (DLL)** | `tail.prev`'i yeni tail yap, `tail.next = null` — yoksa dangling pointer |
| **İndeks sınır dışı** | `index < 0 || index > size` (add) veya `index >= size` (get/remove) |
| **null data** | `equals()` yerine `Objects.equals()` ya da null guard |

---

## 6. Boilerplate — Tam Singly Linked List

```java
import java.util.NoSuchElementException;

public class SinglyLinkedList<T> {
    private static class Node<T> {
        T data; Node<T> next;
        Node(T data) { this.data = data; }
    }

    private Node<T> head;
    private int size;

    public void addFirst(T data) {
        Node<T> n = new Node<>(data); n.next = head; head = n; size++;
    }

    public void addLast(T data) {
        Node<T> n = new Node<>(data);
        if (head == null) { head = n; size++; return; }
        Node<T> c = head; while (c.next != null) c = c.next; c.next = n; size++;
    }

    public T removeFirst() {
        if (head == null) throw new NoSuchElementException();
        T d = head.data; head = head.next; size--; return d;
    }

    public int size() { return size; }
    public boolean isEmpty() { return size == 0; }

    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder("[");
        Node<T> c = head;
        while (c != null) { sb.append(c.data); if (c.next != null) sb.append(", "); c = c.next; }
        return sb.append("]").toString();
    }
}
```

---

## 7. Sık Tuzaklar

| Tuzak | Açıklama |
|-------|----------|
| `curr.next = newNode` sonra `newNode.next = curr.next` | Bağlantı zinciri koptu — sıra önemli! |
| Tek elemanlı listede `curr.next.next != null` | `NullPointerException` — önce `curr.next != null` kontrol et |
| DLL'de `prev` güncellemesini unutmak | Çift yönlü bağlantı bozulur |
| `size` sayacını güncellememek | `size()` yanlış döner |

---

## 8. Sınavda Nasıl Sorulur?

- "Generic linked list sınıfı yaz: node, insert front/end, remove, toString" → Mock Exam 1 – Q1
- "Şu LL'yi reverse et" → in-place reverse (3 pointer tekniği)
- "ArrayList vs LinkedList karmaşıklık karşılaştırması" → tablo ezber

---

## 9. Mini Self-Check

1. SLL'de `addLast` O(1) yapmak için ne gerekir?
2. DLL'de `removeLast` O(1) mi O(n) mi? Neden?
3. `head = null` olan bir `SinglyLinkedList`'e `removeFirst()` çağrısı ne olur?

<details>
<summary>Cevaplar</summary>

1. `tail` pointer tutmak — son düğüme direkt erişim.
2. O(1) — `tail.prev` pointer'ı sayesinde önceki düğüme O(1)'de ulaşılır. SLL'de O(n) olurdu.
3. `NoSuchElementException` fırlatılır (veya `NullPointerException` — boş liste kontrolü yapmak zorunlu).

</details>

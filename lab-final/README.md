# Lab Final Hazırlık Paketi — CENG114 Spring 2025–2026

Bu klasör, **Lab Final sınavı** için oluşturulmuş tüm çalışma materyallerini içerir.

---

## Önerilen Çalışma Sırası

### 1. Adım — Konu taraması (2–3 saat)

Zayıf hissettiğin konudan başla; hepsini sırayla bitirmeye çalışma.

| Dosya | Konu | Öncelik |
|-------|------|---------|
| [topics/01_sorting_quicksort.md](./topics/01_sorting_quicksort.md) | Bubble/Selection/Insertion özet + Quick Sort detay | Orta |
| [topics/02_exception_io_event.md](./topics/02_exception_io_event.md) | Exception, Text I/O, Event-Driven | **Yüksek** |
| [topics/03_generics_collections.md](./topics/03_generics_collections.md) | Generic class/method, wildcards, PECS | **Yüksek** |
| [topics/04_linked_lists.md](./topics/04_linked_lists.md) | Singly/Doubly LL, insert/delete, edge cases | **Yüksek** |
| [topics/05_stacks_queues.md](./topics/05_stacks_queues.md) | Stack LIFO, Queue FIFO, uygulamalar | Orta |
| [topics/06_lambda_streams.md](./topics/06_lambda_streams.md) | Lambda, functional interface, Stream pipeline | **Yüksek** |

### 2. Adım — Mock Exam 1 (2 saat — kağıtsız!)

1. Zamanlayıcını **120 dakikaya** kur.
2. [exam_1/lab_exam_1.md](./exam_1/lab_exam_1.md) dosyasını aç — **çözüme bakma**.
3. Süre dolunca [exam_1/lab_exam_1_solution.md](./exam_1/lab_exam_1_solution.md) ile karşılaştır.

**Exam 1 odağı:** Generics, Linked List tabanlı koleksiyon, Wildcards (PECS), Lambda/Stream

### 3. Adım — Mock Exam 2 (2 saat — kağıtsız!)

1. Aynı prosedür.
2. Soru: [exam_2/lab_exam_2.md](./exam_2/lab_exam_2.md)
3. Çözüm: [exam_2/lab_exam_2_solution.md](./exam_2/lab_exam_2_solution.md)

**Exam 2 odağı:** Exception handling, Text File I/O, try-with-resources, Lambda + Stream, Quick Sort

### 4. Adım — Eksik kalan konu anlatımlarını tekrar oku

Hatalı bölümleri not al, o konunun `topics/` dosyasına dön.

---

## Mock Exam Nasıl Kullanılır?

```
[ Timer: 120 dak ]  →  Soru dosyasını çöz  →  Çözümle karşılaştır  →  Hataları not al
```

- Süreyi **geçme** — sınav koşullarını simüle et.
- IDE kullan, ama ChatGPT/Copilot kapatılsın.
- Çıktıyı `javac` + `java` ile gerçekten derlemeyi dene.

---

## Kapsanan Konular (Müfredatla eşleşme)

| Hafta | Konu | Konu Dosyası | Mock Exam |
|-------|------|--------------|-----------|
| 9 | Sorting / Searching | `01_sorting_quicksort.md` | Exam 2 – Q2 |
| 10 | Abstract Classes & Interfaces | *(Lab 7 zaten mevcut)* | — |
| 11 | Exception Handling | `02_exception_io_event.md` | Exam 2 – Q1 |
| 12 | Generics & Wildcards | `03_generics_collections.md` | Exam 1 – Q1, Q2 |
| 13–14 | Linked Lists | `04_linked_lists.md` | Exam 1 – Q1 |
| 14 | Stacks & Queues | `05_stacks_queues.md` | Exam 1 – Q1 |
| 15 | Lambda & Streams + Event-Driven | `06_lambda_streams.md` | Exam 1 – Q2, Exam 2 – Q2 |

---

*CENG114 Computer Programming II — Spring 2025–2026 · AYBU*

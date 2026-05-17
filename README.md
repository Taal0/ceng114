# CENG114: Çalışma ve Notlar Deposu

Bu repository, **CENG114 — Computer Programming II** (AYBU, Spring 2025/2026) dersi için hocanın slaytları, müfredat ve öğrenci notlarından derlenmiş çalışma materyallerini içerir.

---

## Hızlı başlangıç

Sınava veya lab quize kısa süre kaldıysa özet notlarla başlayın:

- **[quiz 3/00_cheatsheet.md](./quiz%203/00_cheatsheet.md)** — konu başlıkları ve kısa hatırlatmalar
- Konuya göre: `quiz 3/` altındaki `01_sorting.md`, `04_exception_handling.md`, `07_generics.md` vb.

---

## Slaytlar (`slides/`)

Hocanın PowerPoint dosyaları [markitdown](https://github.com/microsoft/markitdown) ile markdown’a çevrilmiştir. Arama ve okuma için doğrudan bu klasörü kullanın.

### Ders haftaları (CENG114)

| Dosya | Konu |
|-------|------|
| [CENG114_Syllabus_ (1).md](./slides/CENG114_Syllabus_%20(1).md) | Müfredat, notlandırma, haftalık program |
| [Week1_.md](./slides/Week1_.md) | Hafta 1 — Sorting & Searching (Liang Ch. 7, 23) |
| [CENG114_Week11_Exception_Handling (1).md](./slides/CENG114_Week11_Exception_Handling%20(1).md) | Exception handling |
| [CENG114_Week12_Generics_Wildcards (1).md](./slides/CENG114_Week12_Generics_Wildcards%20(1).md) | Generics & wildcards |
| [CENG114_Weeks13_14_LinkedLists (2).md](./slides/CENG114_Weeks13_14_LinkedLists%20(2).md) | Linked lists (Hafta 13–14) |

### Pearson / Liang bölümleri

| Dosya | Bölüm |
|-------|--------|
| [Copy of 09slide_accessible.md](./slides/Copy%20of%2009slide_accessible.md) | Ch. 9 — Objects and Classes |
| [Copy of 10slide_accessible.md](./slides/Copy%20of%2010slide_accessible.md) | Ch. 10 — Thinking in Objects |
| [Copy of 11slide_accessible.md](./slides/Copy%20of%2011slide_accessible.md) | Ch. 11 — Inheritance |
| [Copy of 12slide_accessible.md](./slides/Copy%20of%2012slide_accessible.md) | Ch. 12 — Polymorphism |
| [Copy of 13slide_accessible.md](./slides/Copy%20of%2013slide_accessible.md) | Ch. 13 — Abstract Classes & Interfaces |
| [Copy of 14slide_accessible.md](./slides/Copy%20of%2014slide_accessible.md) | Ch. 14 — JavaFX Basics |
| [Copy of 15slide_accessible.md](./slides/Copy%20of%2015slide_accessible.md) | Ch. 15 — Event-Driven Programming |
| [Copy of 16slide_accessible.md](./slides/Copy%20of%2016slide_accessible.md) | Ch. 16 — Exception Handling (Liang) |
| [18slide_accessible.md](./slides/18slide_accessible.md) | Ch. 18 — Recursion |
| [Copy of 19slide_accessible.md](./slides/Copy%20of%2019slide_accessible.md) | Ch. 19 — Generics |
| [Copy of 24slide_accessible.md](./slides/Copy%20of%2024slide_accessible.md) | Ch. 24 — Lists, Stacks, Queues, Priority Queues |

Kaynak `.pptx` dosyaları repo kökünde durabilir; güncel metin için her zaman `slides/*.md` sürümünü tercih edin.

---

## Konu klasörleri ve önerilen çalışma sırası

1. **[LinkedLists/](./LinkedLists/)** — Tek/çift yönlü bağlı listeler, insertion/deletion, örnekler
2. **[Chapter-13-Extension/](./Chapter-13-Extension/)** — Abstract sınıflar, interface’ler, `Comparable`, `equals`/`hashCode`, `Cloneable`
3. **[Lambda/](./Lambda/)** & **[Generics-Extras/](./Generics-Extras/)** — Lambda ifadeleri, generics kısıtları, wildcards
4. **[OOP-Metrics-Examples/](./OOP-Metrics-Examples/)** — LCOM, RFC, WMC, cyclomatic complexity (Hafta 8)
5. **[quiz 3/](./quiz%203/)** — Lab Quiz 3 ve final dönemi için özet + örnek sorular

`github/` altında aynı içeriklerin yedek/kopya sürümleri bulunabilir; günlük çalışma için kök dizindeki klasörleri kullanın.

---

## Çalışma tavsiyeleri

Hoca sıkça “kodun çıktısı ne olur?” ve ince detay soruları sorduğu için sadece okumak yetmez:

- **Bellek diyagramı çizin** — referansların stack/heap üzerindeki yönünü görselleştirin
- **Uç durumları arayın** — dynamic binding, constructor chaining, string pool, casting
- **Çıktıyı tahmin edin** — polymorphism örneklerinde önce tahmin, sonra doğrulama

---

*CENG114 Computer Programming II — Educational Materials*

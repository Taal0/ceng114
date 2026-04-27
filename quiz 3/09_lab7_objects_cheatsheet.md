# 09 — Lab7 Objects Cheat Sheet (Interface + Abstract + Pipeline)

> Kaynak: `slides/CENG114_LabGuide7.md` + `quiz 3/100_task_ornegi_lab7.md`  
> Amaç: **objelerle çalışma akışını** sınavda hızlı yazabilmek.

---

## 1) Büyük Resim (Kim, ne yapıyor?)

- `Normalizable` → `normalize(String)` sözleşmesi
- `Tokenizable` → `tokenize(String)` sözleşmesi
- `PatternSearchable` → pattern sayma/arama sözleşmesi
- `TextProcessor` (abstract) → ortak helper + **final template method**
- `LowercaseStage`, `PunctuationCleaner`, `SpaceNormalizer` → `TextProcessor`'ı **extends**
- `LetterDigitTokenizer` → hem `TextProcessor`'ı extends, hem `Tokenizable` implement
- `ModerationPipeline` → `List<Normalizable>` + `Tokenizable` tutar, akışı yönetir
- `Main` → objeleri üretir, zinciri kurar, çalıştırır

---

## 2) Object İlişkileri (sınavda en kritik)

### A) Interface referansı ile çalışma (polymorphism)

```java
Normalizable n1 = new LowercaseStage();
Normalizable n2 = new PunctuationCleaner();
Normalizable n3 = new SpaceNormalizer();
```

- Solda interface tip, sağda gerçek nesne
- Bu sayede pipeline aynı listede farklı stage'leri tutabilir

### B) Abstract class instantiate edilmez

```java
// TextProcessor tp = new TextProcessor(); // HATALI
TextProcessor tp = new LowercaseStage();   // DOĞRU
```

### C) Aynı nesneyi farklı tipte referansla tutabilirsin

```java
LetterDigitTokenizer t = new LetterDigitTokenizer();
Tokenizable tk = t;        // tokenize metodu için
Normalizable nm = t;       // normalize metodu için
TextProcessor tp = t;      // helper/getName erişimi için
```

---

## 3) Pipeline Akışı (objeler nasıl konuşuyor?)

```java
ModerationPipeline p = new ModerationPipeline()
    .addStage(new LowercaseStage())
    .addStage(new PunctuationCleaner())
    .addStage(new SpaceNormalizer())
    .setTokenizer(new LetterDigitTokenizer());

String[] out = p.run("  OMG!! This Movie was SOOO good,,,  ");
```

`run` içindeki gerçek akış:
1. `current = rawText`
2. listedeki her `Normalizable` için: `current = stage.normalize(current)`
3. en sonda: `tokenizer.tokenize(current)`
4. çıktı: `String[]`

---

## 4) Sınavda Yazım Sırası (karışmamak için)

1. `Normalizable`
2. `Tokenizable`
3. `PatternSearchable` (istenirse)
4. `TextProcessor` (abstract + `final normalize`)
5. `LowercaseStage`
6. `PunctuationCleaner`
7. `SpaceNormalizer`
8. `LetterDigitTokenizer`
9. `SimplePatternCounter` (istenirse)
10. `ModerationPipeline`
11. `Main` (**en son**)

---

## 5) Template Method Mantığı (çok sorulur)

`TextProcessor.normalize(...)` neden `final`?
- Alt sınıf ortak akışı bozamasın diye
- Her stage'de aynı iskelet zorunlu:
  1. null kontrol
  2. `String` -> `StringBuffer`
  3. `processImpl(buf)` çağrısı (hook)
  4. `buf.toString()`

Alt sınıf sadece şurayı yazar:

```java
@Override
protected void processImpl(StringBuffer buf) {
    // stage'e özel mutasyon
}
```

---

## 6) Object Tarafındaki En Sık Hatalar

1. `extends` / `implements` karışıyor
   - class -> class: `extends`
   - class -> interface: `implements`
2. `TextProcessor`'ı `new`lemeye çalışmak
3. `addStage` / `setTokenizer` içinde `return this` unutmak
4. `run` içinde `current`'ı güncellememek
5. tokenizer set etmeden `run` çağırmak (`null` riski)
6. `normalize`'ı alt sınıfta override etmeye çalışmak (`final`)

---

## 7) Yasak/Serbest API (objelerden bağımsız ama hayati)

### String'den sadece:
- `charAt`
- `length`

### Yasaklardan kritik olanlar:
- `split`, `substring`, `indexOf`, `trim`, `toLowerCase`, `equals`
- `Character.*`
- regex

### Serbest:
- `StringBuffer` metodları (`append`, `setCharAt`, `deleteCharAt`, `setLength`, ...)

---

## 8) 30 Saniyelik Ezber Kartı

- **Interface** = ne yapar? (sözleşme)
- **Abstract class** = ortak nasıl yapar? (iskelet + helper)
- **Concrete stage** = bir işi yapar (single responsibility)
- **Pipeline** = stage'leri sırayla çalıştırır
- **Main** = nesneleri kurar, zinciri bağlar, çalıştırır

---

## 9) Mini Sınav Şablonu (Object Kurulumu)

```java
LowercaseStage s1 = new LowercaseStage();
PunctuationCleaner s2 = new PunctuationCleaner();
SpaceNormalizer s3 = new SpaceNormalizer();
LetterDigitTokenizer tokenizer = new LetterDigitTokenizer();

ModerationPipeline pipeline = new ModerationPipeline()
    .addStage(s1)
    .addStage(s2)
    .addStage(s3)
    .setTokenizer(tokenizer);

String[] tokens = pipeline.run(input);
```

> Kural: `Main`de sadece **kurulum + orchestrasyon**, asıl iş logic class'larda.


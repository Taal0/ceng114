# 100 — LabGuide7 Bazlı Task Örneği (Hocam Tarzı)

> Bu örnek, `CENG114_LabGuide7.md` kurallarına göre hazırlanmış ikinci bir quiz/lab task simülasyonudur. Özellikle **String kısıtları** ve **template method** disiplini hedeflenir.

---

## TASK PDF — ÖRNEK 2

**CENG114 — Lab Quiz**
**Süre:** 70 dakika
**Dosya adı:** `TextModerationDemo.java` (tek dosyada birden fazla class olabilir)

### Senaryo

Bir sosyal platformun yorumlarını temizleyen bir mini preprocessing modülü geliştiriyorsun.
Hedef çıktı:

`"  SPAM!!! Buy NOW 100% FREE!!!  "`  
→ `["spam", "buy", "now", "100", "free"]`

---

## Zorunlu Kısıtlar (Birebir Uygula)

1. `String` sınıfından sadece:
   - `charAt(int index)`
   - `length()`

2. Yasak:
   - `split`, `substring`, `indexOf`, `replace`, `replaceAll`, `trim`, `toLowerCase`, `toUpperCase`, `contains`, `equals`, `equalsIgnoreCase`
   - `Character.*`
   - `regex`

3. `StringBuffer` serbest.
4. Döngü içinde `String +` yapma (benchmark harici).
5. İzinli importlar:
   - `java.util.ArrayList`, `java.util.List`, `java.util.HashSet`, `java.util.Set`, `java.util.Arrays` (`Arrays.asList` için)

---

## Gereksinimler

### Task 1 — Arayüzler

Aşağıdakileri aynen tanımla:

```java
public interface Normalizable {
    String normalize(String input);
}

public interface Tokenizable {
    String[] tokenize(String input);
}
```

### Task 2 — Soyut sınıf

`TextProcessor` adında abstract class yaz:

- `Normalizable` implement etsin.
- Aşağıdaki helper'ları içersin:
  - `isWhitespace`
  - `isPunctuation`
  - `isAsciiLetter`
  - `isDigit`
  - `toLower`
- `normalize` metodu `final` olsun, `null` input için `""` dönsün.
- Hook method:
  - `protected abstract void processImpl(StringBuffer buf);`
  - `public abstract String getName();`

### Task 3 — LowercaseStage

`TextProcessor`'dan türesin, tüm harfleri küçültsün (`toLower` + `setCharAt`).

### Task 4 — PunctuationCleaner

Noktalama karakterlerini silsin.
**Geriye doğru** dolaşarak `deleteCharAt(i)` kullan.
Kod içinde 1 satır yorumla “neden geri dolaştığını” açıkla.

### Task 5 — SpaceNormalizer

- Tüm whitespace dizilerini tek `' '` boşluğa indir.
- Baştaki/sondaki boşluğu kaldır.
- Tek geçiş O(n).

### Task 6 — LetterDigitTokenizer

`TextProcessor`'ı extends + `Tokenizable` implement et.

- `processImpl` no-op.
- `tokenize`:
  - whitespace gördüğünde token bitir,
  - boş olmayan token'ları `ArrayList<String>`'e ekle,
  - `String[]` döndür.

### Task 7 — Optional capability interface

Yeni bir interface ekle:

```java
public interface PatternSearchable {
    int countOccurrences(String text, String pattern);
}
```

`SimplePatternCounter` sınıfı bu interface'i implement etsin.

- `indexOf` yasak olduğu için manuel tarama yap.
- `text="ha ha haha"` ve `pattern="ha"` için sonuç `4` olmalı.

### Task 8 — Pipeline

`ModerationPipeline` sınıfı:

- `List<Normalizable> stages`
- `Tokenizable tokenizer`
- `addStage(...)` ve `setTokenizer(...)` fluent (`return this`)
- `run(String rawText)`:
  1. normalizer'ları sırayla uygula
  2. tokenize et
  3. `String[]` döndür

### Task 9 — Main

En az 3 kirli input çalıştır:

1. tab içeren,
2. çok satırlı whitespace içeren,
3. punctuation + rakam + mixed-case içeren.

Her biri için:

```text
=== Raw input ===
"..."
[LowercaseStage]    -> "..."
[PunctuationCleaner] -> "..."
[SpaceNormalizer]   -> "..."
[LetterDigitTokenizer] -> [...]
```

`PatternSearchable` demosunu da yazdır:

```text
=== Pattern demo ===
text = "ha ha haha"
pattern = "ha"
count = 4
```

---

## Notlandırma (Örnek Rubrik)

- Yasak API kullanımı: **-40**
- `normalize` final değilse: **-20**
- `StringBuffer` yerine loop içinde `String +`: **-15**
- `PunctuationCleaner` ileri yönde silme (indeks kayması bug): **-15**
- Fluent API bozuk (`return this` yok): **-10**
- Çıktı formatı tutmuyor: **-10**

---

## Çözüm İskeleti (Kodlamaya başlamadan checklist)

1. Önce interface'leri yaz.
2. `TextProcessor` + helper'lar + `final normalize`.
3. 3 stage class (`LowercaseStage`, `PunctuationCleaner`, `SpaceNormalizer`).
4. `LetterDigitTokenizer`.
5. `ModerationPipeline`.
6. `PatternSearchable` + `SimplePatternCounter`.
7. `Main` testleri ve exact output.

---

## Mini İpucu Kartı

- `toLower` ASCII:
  - `'A' <= c && c <= 'Z'` ise `(char)(c + 32)`
- `isDigit`:
  - `'0' <= c && c <= '9'`
- token flush:
  - whitespace görünce `if (token.length() > 0) list.add(token.toString())`

---

## Quizde Çakılmamak için Son Kontrol

- [ ] `split`/`substring`/`equals` hiç yok mu?
- [ ] `Character.*` hiç yok mu?
- [ ] `normalize(null)` gerçekten `""` dönüyor mu?
- [ ] `PunctuationCleaner` geriye doğru mu siliyor?
- [ ] `run` sırası doğru mu: normalize -> tokenize?
- [ ] Çıktı satır formatı task ile birebir mi?

---

## Çözüm Kodu (Sınavda Yazım Sırası)

> Aşağıdaki sıra, sınavda en az karışıklıkla yazabileceğin sıradır.  
> Her blok ayrı dosya gibi gösterildi; istersen tek `.java` dosyada da birleştirebilirsin.

### 1) `Normalizable.java`

```java
public interface Normalizable {
    String normalize(String input);
}
```

### 2) `Tokenizable.java`

```java
public interface Tokenizable {
    String[] tokenize(String input);
}
```

### 3) `PatternSearchable.java`

```java
public interface PatternSearchable {
    int countOccurrences(String text, String pattern);
}
```

### 4) `TextProcessor.java`

```java
public abstract class TextProcessor implements Normalizable {
    protected boolean isWhitespace(char c) {
        return c == ' ' || c == '\t' || c == '\n' || c == '\r';
    }

    protected boolean isPunctuation(char c) {
        return c == '.' || c == ',' || c == '!' || c == '?' || c == ';' || c == ':'
            || c == '"' || c == '\'' || c == '(' || c == ')' || c == '[' || c == ']'
            || c == '{' || c == '}' || c == '-' || c == '_' || c == '/' || c == '\\';
    }

    protected boolean isAsciiLetter(char c) {
        return (c >= 'A' && c <= 'Z') || (c >= 'a' && c <= 'z');
    }

    protected boolean isDigit(char c) {
        return c >= '0' && c <= '9';
    }

    protected char toLower(char c) {
        // ASCII: 'A'..'Z' aralığını 32 ekleyerek 'a'..'z' yapıyoruz.
        if (c >= 'A' && c <= 'Z') return (char) (c + 32);
        return c;
    }

    public final String normalize(String input) {
        // Lab kuralı: null güvenliği, exception atmadan empty string dön.
        if (input == null) return "";
        // Template Method: ortak akış burada; alt sınıf sadece processImpl sağlar.
        StringBuffer buf = new StringBuffer();
        for (int i = 0; i < input.length(); i++) {
            buf.append(input.charAt(i));
        }
        processImpl(buf);
        return buf.toString();
    }

    protected abstract void processImpl(StringBuffer buf);
    public abstract String getName();
}
```

### 5) `LowercaseStage.java`

```java
public class LowercaseStage extends TextProcessor {
    @Override
    protected void processImpl(StringBuffer buf) {
        for (int i = 0; i < buf.length(); i++) {
            buf.setCharAt(i, toLower(buf.charAt(i)));
        }
    }

    @Override
    public String getName() {
        return "LowercaseStage";
    }
}
```

### 6) `PunctuationCleaner.java`

```java
public class PunctuationCleaner extends TextProcessor {
    @Override
    protected void processImpl(StringBuffer buf) {
        // Backward walk avoids index shift after deleteCharAt(i).
        for (int i = buf.length() - 1; i >= 0; i--) {
            if (isPunctuation(buf.charAt(i))) {
                buf.deleteCharAt(i);
            }
        }
    }

    @Override
    public String getName() {
        return "PunctuationCleaner";
    }
}
```

### 7) `SpaceNormalizer.java`

```java
public class SpaceNormalizer extends TextProcessor {
    @Override
    protected void processImpl(StringBuffer buf) {
        // out: normalize edilmiş yeni içerik; sonra tekrar buf içine kopyalanacak.
        StringBuffer out = new StringBuffer();
        // true başlatıyoruz ki baştaki whitespace'ler direkt atılsın.
        boolean prevWasSpace = true;

        for (int i = 0; i < buf.length(); i++) {
            char c = buf.charAt(i);
            if (isWhitespace(c)) {
                if (!prevWasSpace) {
                    // Arka arkaya kaç whitespace olursa olsun sadece 1 boşluk bırak.
                    out.append(' ');
                    prevWasSpace = true;
                }
            } else {
                out.append(c);
                prevWasSpace = false;
            }
        }

        if (out.length() > 0 && out.charAt(out.length() - 1) == ' ') {
            // Sondaki tek artakalan boşluğu da sil (trim etkisi).
            out.deleteCharAt(out.length() - 1);
        }

        // Kural: processImpl yeni referans döndürmez, mevcut buf'i in-place günceller.
        buf.setLength(0);
        buf.append(out);
    }

    @Override
    public String getName() {
        return "SpaceNormalizer";
    }
}
```

### 8) `LetterDigitTokenizer.java`

**Akış (çok kısa):**
1. Girdiyi soldan sağa karakter karakter gez.
2. Harf/rakam/diğer boşluk olmayan karakterleri geçici `token` buffer'ına biriktir.
3. Whitespace görünce token bitir (`list`e ekle, `token`ı sıfırla).
4. Döngü bitince elde kalan son token'ı unutma.
5. `ArrayList<String>` -> `String[]` dön.

```java
import java.util.ArrayList;

public class LetterDigitTokenizer extends TextProcessor implements Tokenizable {
    @Override
    protected void processImpl(StringBuffer buf) {
        // no-op
    }

    @Override
    public String getName() {
        return "LetterDigitTokenizer";
    }

    @Override
    public String[] tokenize(String input) {
        // 0) Güvenlik: null input gelirse boş dizi dön.
        if (input == null) return new String[0];
        // 1) Çıkış tokenlarını burada tutacağız.
        ArrayList<String> list = new ArrayList<>();
        // O an biriken karakterler (tek bir token).
        StringBuffer token = new StringBuffer();

        // 2) Metni soldan sağa gez.
        for (int i = 0; i < input.length(); i++) {
            char c = input.charAt(i);
            if (isWhitespace(c)) {
                // 3) Boşluk gördük: token varsa finalize et.
                if (token.length() > 0) {
                    // Token bitti: listeye ekle ve buffer'ı sıfırla.
                    list.add(token.toString());
                    token.setLength(0);
                }
            } else {
                // 4) Boşluk değilse mevcut token'a ekle.
                token.append(c);
            }
        }

        // 5) Input boşlukla bitmemişse son token hala bufferdadır.
        if (token.length() > 0) {
            // Metin whitespace ile bitmediyse son token burada eklenir.
            list.add(token.toString());
        }

        // 6) Task çıktısı String[] istediği için array'e çeviriyoruz.
        return list.toArray(new String[0]);
    }
}
```

### 9) `SimplePatternCounter.java`

**Akış (çok kısa):**
1. `text` içinde pattern başlayabilecek her `i` pozisyonunu dene.
2. İç döngüde `pattern`i karakter karakter karşılaştır.
3. Tam eşleşme varsa `count++`.
4. Overlap eşleşmeler de sayılır (örn: `haha` içinde `ha` 2 kez).

```java
public class SimplePatternCounter implements PatternSearchable {
    @Override
    public int countOccurrences(String text, String pattern) {
        // 0) Geçersiz durumlar
        if (text == null || pattern == null) return 0;
        if (pattern.length() == 0) return 0;
        if (pattern.length() > text.length()) return 0;

        int count = 0;
        // Her i pozisyonunu "pattern buradan başlıyor mu?" diye kontrol et.
        for (int i = 0; i <= text.length() - pattern.length(); i++) {
            // i pozisyonunda şimdilik eşleşiyor varsay.
            boolean ok = true;
            for (int j = 0; j < pattern.length(); j++) {
                // text[i + j] ile pattern[j] eşleşmezse bu başlangıç geçersiz.
                if (text.charAt(i + j) != pattern.charAt(j)) {
                    ok = false;
                    break;
                }
            }
            // İç döngü hiç bozulmadıysa full match var.
            if (ok) count++;
        }
        return count;
    }
}
```

### 10) `ModerationPipeline.java`

**Akış (çok kısa):**
1. Stage listesi bir işlem zinciri (normalize adımları).
2. `run` çağrıldığında tüm stage'ler sırayla uygulanır.
3. Son normalize edilmiş metin tokenizer'a verilir.
4. Tokenizer set edilmediyse boş dizi döner.

```java
import java.util.ArrayList;
import java.util.List;

public class ModerationPipeline {
    // normalize stage'leri burada tutulur (sıra önemlidir).
    private List<Normalizable> stages = new ArrayList<>();
    // son adım: parçalama işi.
    private Tokenizable tokenizer;

    public ModerationPipeline addStage(Normalizable stage) {
        // Fluent API: stage ekle ve aynı nesneyi geri dön.
        stages.add(stage);
        return this;
    }

    public ModerationPipeline setTokenizer(Tokenizable t) {
        // Fluent API: tokenizer ata ve aynı nesneyi geri dön.
        tokenizer = t;
        return this;
    }

    public String[] run(String rawText) {
        // 1) current değişkeni her stage sonrası güncellenen ara sonuçtur.
        String current = rawText;
        // Stage'ler tanımlandığı sırada çalışır (chain order çok kritik).
        for (int i = 0; i < stages.size(); i++) {
            current = stages.get(i).normalize(current);
        }
        // 2) Tokenizer verilmemişse NPE yerine güvenli boş dönüş.
        if (tokenizer == null) return new String[0];
        // Normalized text en son tokenizer'a gider.
        return tokenizer.tokenize(current);
    }
}
```

### 11) `TextModerationDemo.java` (Main en son)

**Akış (çok kısa):**
1. Stage nesnelerini oluştur.
2. Pipeline'ı fluent zincirle kur.
3. Test inputlarını hem adım adım (`printStages`) hem tek çağrıyla (`pipeline.run`) çalıştır.
4. En sonda pattern counter demosu yap.

```java
import java.util.Arrays;

public class TextModerationDemo {
    private static void printStages(String raw, TextProcessor[] stages, Tokenizable tokenizer) {
        // Bu helper, her stage'den sonra çıkan ara metni görmeyi sağlar.
        System.out.println("=== Raw input ===");
        System.out.println("\"" + raw + "\"");
        System.out.println();

        // raw -> s1 -> s2 -> s3 şeklinde ilerliyoruz.
        String current = raw;
        for (int i = 0; i < stages.length; i++) {
            current = stages[i].normalize(current);
            System.out.println("[" + stages[i].getName() + "] -> \"" + current + "\"");
        }
        // Son adımda tokenize edilmiş final hali.
        System.out.println("[LetterDigitTokenizer] -> " + Arrays.toString(tokenizer.tokenize(current)));
        System.out.println();
    }

    public static void main(String[] args) {
        // Her stage tek sorumluluk prensibiyle ayrı class.
        LowercaseStage s1 = new LowercaseStage();
        PunctuationCleaner s2 = new PunctuationCleaner();
        SpaceNormalizer s3 = new SpaceNormalizer();
        LetterDigitTokenizer tokenizer = new LetterDigitTokenizer();

        // Fluent API: addStage(...).addStage(...).setTokenizer(...)
        ModerationPipeline pipeline = new ModerationPipeline()
            .addStage(s1)
            .addStage(s2)
            .addStage(s3)
            .setTokenizer(tokenizer);

        String in1 = "  SPAM!!! Buy NOW 100% FREE!!!  ";
        String in2 = "\tHello,\t\tWORLD!!\n\nJava 101...\r";
        String in3 = "  OMG!! This Movie was SOOO good,,,  LOVED it!!!  ";

        // Aşama aşama debug çıktısı.
        printStages(in1, new TextProcessor[]{s1, s2, s3}, tokenizer);
        printStages(in2, new TextProcessor[]{s1, s2, s3}, tokenizer);
        printStages(in3, new TextProcessor[]{s1, s2, s3}, tokenizer);

        // Tek çağrıda final output almak istediğinde run() kullanırsın.
        System.out.println("Pipeline run(in1) -> " + Arrays.toString(pipeline.run(in1)));
        System.out.println();

        // Bonus kabiliyet: pattern arama/sayma.
        PatternSearchable counter = new SimplePatternCounter();
        String text = "ha ha haha";
        String pattern = "ha";
        int count = counter.countOccurrences(text, pattern);

        System.out.println("=== Pattern demo ===");
        System.out.println("text = \"" + text + "\"");
        System.out.println("pattern = \"" + pattern + "\"");
        System.out.println("count = " + count);
    }
}
```


---
name: uyap-udf-sablonu
description: UYAP Doküman Editörü (.udf) dilekçe şablonu. Dilekçe UDF dosyasına ya da PDF'e çevrilirken UYAP UDF MCP'nin create_udf_from_markdown aracıyla kullan. Antet, mahkeme başlıkları ve "Gönderilmek Üzere" sırası, TAB ile hizalanan künye, bölüm başlıkları, otomatik numaralandırma, içtihat bloğu, tarih ve sağa yaslı imza, boşluk ve kenar kuralları.
---

# UYAP UDF şablonu

Dilekçe önce bu kurallara göre markdown olarak yazılır, sonra UYAP UDF MCP'nin `create_udf_from_markdown` aracıyla `.udf` dosyasına çevrilir. PDF gerekiyorsa `convert_udf_to_pdf` ya da `convert_markdown_to_pdf` kullanılır.

İçeriğe ilişkin kurallar (alıntı, künye, vurgu, sonuç ve istem) `dilekce-yazimi` becerisindedir. Antet ve imza bilgileri `dilekce-yazimi/profil.md` dosyasındadır; o beceri kurulu değilse bilgileri kullanıcıya sor, uydurma.

## 1. Antet

Dilekçenin en üstündeki avukat bilgileri UYAP'ın üst bilgi alanına yazılır. Satır başına `@|` konur (ortalı, her sayfada tekrarlanır). Antetin bittiği yere mutlaka bir boş antet satırı (`@|<br>`) eklenir ki antet metne yapışmasın.

```
@|AVUKAT AD SOYAD
@|Adres satırı
@|GSM: 0 5xx xxx xx xx
@|<br>
```

Satırlar `profil.md`'deki "Antet" satırlarından alınır. Büroda birden çok avukat varsa dilekçeyi yazan avukatın anteti kullanılır.

## 2. Mahkeme başlıkları

- Mahkeme adları tamamen **BÜYÜK HARF**, kalın, ortalı ve 14 punto yazılır: `#|**İZMİR NÖBETÇİ ASLİYE HUKUK MAHKEMESİNE**`
- "Gönderilmek Üzere" ibaresi ortalı, kalın ve italik yazılır: `|***Gönderilmek Üzere***`
- **Sıra her dilekçe türünde aynıdır:** en üstte dilekçeyi inceleyecek merci (istinafta BAM ilgili dairesi, temyizde Yargıtay ilgili dairesi, itirazda itiraz mercii), ortada "Gönderilmek Üzere", en altta kararı veren mahkeme. Kararı veren mahkeme üste yazılmaz.
- Başlık bloğunun içine `<br>` konmaz. `<br>` yalnızca son mahkeme başlığından sonra, künyeye geçmeden önce bir kez konur.

```
#|**İZMİR BÖLGE ADLİYE MAHKEMESİ İLGİLİ HUKUK DAİRESİNE**
|***Gönderilmek Üzere***
#|**İZMİR 3. ASLİYE HUKUK MAHKEMESİNE**
<br>
```

## 3. Künye

- Yalnızca etiket (iki noktaya kadar olan kısım) kalındır; değer normal yazılır.
- İki noktaları hizalamak için boşluk değil **TAB** kullanılır. Bu belgedeki örneklerde `\t` bir TAB karakterini gösterir; metne gerçek TAB karakteri yazılır. UYAP'ta TAB durakları 72 pt arayla dizilir; iki nokta sütunu 144 pt'dedir.
  - **Kısa etiketler 2 TAB alır:** DOSYA NO, SORGU NO, DAVACI, DAVALI, VEKİLİ, VEKİLLERİ, SANIK, ŞÜPHELİ, MÜDAFİİ, MÜŞTEKİ, KATILAN, MAĞDUR, MÜDAHİL, ŞİKAYETÇİ, HÜKÜMLÜ, ALACAKLI, BORÇLU, KONU, SUÇ, SUÇ TARİHİ.
  - **Uzun etiketler 1 TAB alır:** İTİRAZ EDEN, TALEP EDEN, BAŞVURUCU, KARŞI TARAF, KARŞI DAVALI, İSTİNAF EDEN, TEMYİZ EDEN, İHBAR OLUNAN, DAVA KONUSU, DAVA DEĞERİ, KARAR TARİHİ, TEBLİĞ TARİHİ, DEĞİŞİK İŞ NO.
  - **Hiçbir etikete 3 TAB konmaz;** üç TAB alan satır sağa kayar. Künye eski bir dilekçeden kopyalanmaz, TAB'lar her seferinde bu kurala göre yazılır.
  - Listede olmayan bir etiket için: kalın Times New Roman 12 punto genişliği 72 pt'den kısaysa 2 TAB, uzunsa 1 TAB.
  - 144 pt'yi aşan etiketler (ör. SUÇA SÜRÜKLENEN ÇOCUK) kısaltılır (ör. SSÇ) ya da iki satıra bölünür.
- Taşan değer (uzun şirket unvanı gibi) iki nokta sütununun hizasından devam eder. UYAP UDF MCP bunu ilk bölüm başlığından önceki `ETİKET\t\t: değer` satırlarına kendiliğinden uygular; satırı elle bölmeye gerek yoktur.
- **Taraf sırası:** Dilekçeyi biz veriyorsak `DOSYA NO` satırından hemen sonra önce müvekkil ve onun vekili, sonra karşı taraf ve vekili yazılır. Müvekkilin sıfatı (davacı, davalı, borçlu vb.) bu sırayı değiştirmez.

```
**DOSYA NO\t\t:** 2026/123 Esas
**DAVALI\t\t:** Örnek Yapı Ticaret A.Ş.
**VEKİLİ\t\t:** Av. Zeynep DENEME
**DAVACI\t\t:** Ayşe ÖRNEK
**VEKİLİ\t\t:** Av. Ali VELİ
**KONU\t\t:** Cevap dilekçemizin sunulmasıdır.
```

## 4. Bölüm başlıkları ve numaralandırma

- "AÇIKLAMALAR :", "İTİRAZ NEDENLERİ :", "HUKUKİ NEDENLER :", "DELİLLER :", "SONUÇ VE İSTEM :" gibi ana başlıklar büyük harf ve kalın yazılır.
- Madde başlıkları büyük harf ve kalın, altındaki açıklama düz metindir.
- Bölüm başlığından sonra `1. ...` ile başlayan satırlar UYAP'ın otomatik listesine çevrilir; her bölümde numara 1'den başlar. Yeni dilekçede normal rakam (`1., 2., 3.`) kullanılır, Roma rakamı kullanılmaz. Roma rakamları yalnızca dışarıdan gelen metinlerdeki hiyerarşiyi bozmamak içindir (`I.` ana liste, altındaki `1.` alt liste). Yıldızlar rakamın dışına konur: `**XVIII.**`, `X**VIII.**` değil.

```
**AÇIKLAMALAR :**

**1. TARAFLAR ARASINDA YAZILI BİR HİZMET SÖZLEŞMESİ KURULMUŞTUR.**

Müvekkil ile davalı şirket arasında...
```

## 5. İçtihat bloğu

```
**Yargıtay 3. Hukuk Dairesi, 2022/8423 Esas, 2023/291 Karar, 23.02.2023 tarihli** kararında şöyledir:

>>> *"...karar metninden aynen alınan kısım **dosyayla ilgili tek vurgu** devamı..."*

(Kaynak: https://mevzuat.adalet.gov.tr/ictihat/893377000)
```

Künye kalın, "kararında şöyledir:" normal; alıntı `>>>` ile girintili ve italik; vurgu italiğin içinde `**...**` ile yazılır ve kalın-italik çıkar; kaynak satırı italik değildir.

## 6. Tarih ve imza

- Tarih, SONUÇ VE İSTEM'in son talebinin sonuna bitişik yazılır: `...saygıyla arz ve talep ederim. 24.11.2026`
- İmza bloğu sayfanın sağına `||` ile yaslanır:

```
||**DAVALI VEKİLİ**
||**Av. Zeynep DENEME**
||(E-imzalıdır)
```

## 7. Boşluklar

- UDF'te boş paragraflar dev boşluklar yaratır; dönüştürücü boş satırları yok sayar.
- Manuel boşluk (`<br>`) yalnızca iki yerde kullanılır: antet bittiğinde (`@|<br>`) ve mahkeme başlığı bloğu bittiğinde (`<br>`). Başka hiçbir yere konmaz.
- Markdown kaynağında okunurluk için her paragraftan sonra bir boş satır bırakılır. Antet, künye ve imza blokları kendi içinde bitişik yazılır. Bu boş satırlar UDF'i etkilemez.
- Satır aralığı: antet, mahkeme başlıkları ve imza bloğu sıkı (0,1 cm), geri kalan her yer UYAP'ın standart 0,3 cm boşluğuyla üretilir. UYAP UDF MCP bunu kendiliğinden yapar.
- Sayfa kenar boşlukları daima 1,5 cm'dir.

## 8. Yazım ayrıntıları

- Yıldızlar kelimeye bitişik yazılır (`**vurgu**`). Metinde tek başına `*` kullanılmaz.
- Kalın içinde italik yazılabilir: `**"Çelişkili Davranma Yasağı" (*Venire Contra Factum Proprium*)**`
- UYAP Doküman Editörü emoji açamaz; emoji kullanılmaz.

## 9. PDF ve dosyalar

- UDF, PDF'e yalnızca bilgisayarda kurulu resmi Uyap Doküman Editörü'nün motoruyla çevrilir (UYAP UDF MCP'nin `convert_udf_to_pdf` aracı). Resmi motor çalışmazsa başka bir yönteme sessizce geçilmez; işlem durdurulur ve kullanıcıya bildirilir.
- Belgenin doğrulama kodu varsa PDF'e UYAP'ın karekodu basılır; mizanpaj UYAP'takiyle aynıdır.
- Kullanıcı "eski hâllerini sil" dese de dosyalar kalıcı olarak silinmez; Çöp Kutusu'na taşınır.

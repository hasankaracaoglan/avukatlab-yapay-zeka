---
name: ictihat-arastirmasi
description: Türk yargı kararlarında emsal araştırması. Dilekçe için emsal karar aranırken, bir kararın künyesi teyit edilirken ya da alıntı karar metniyle denetlenirken kullan. Yargı MCP (Bedesten) arama operatörleri, web'den terim bulma, 100 künye listeleyip en fazla 20 kararı tam metin okuma, içtihat hiyerarşisi, verify_bedesten_quote ile alıntı doğrulama ve dilekçeye hazır alıntı bloğu.
---

# İçtihat araştırması

Bu beceri Yargı MCP ve Mevzuat MCP sunucularıyla çalışır. Sunucular kurulu değilse karar araştırması yapılamaz; kullanıcıya bildir ve karar uydurma.

Bulunan kararlar dilekçeye `dilekce-yazimi` becerisindeki kurallarla konur: alıntı harfi harfine, künye `kunye` alanından, kaynak tam bağlantı.

## Bağlayıcı beş kural

1. **Karar uydurulmaz.** Bulunamayan şey "bulunamadı" diye yazılır.
2. **Tırnak içi harfi harfine aynıdır.** Alıntı yalnızca `get_bedesten_document_markdown` çıktısından kopyalanır; hafızadan ya da kendi notundan alıntı yazılmaz.
3. **Künye `kunye` alanından aynen alınır.** Tarih karar metninden çıkarılmaz; metinde alt mahkeme ve BAM tarihleri de geçer.
4. **Kaynak satırı tam bağlantıdır:** `(Kaynak: https://mevzuat.adalet.gov.tr/ictihat/<documentId>)`. `emsal.uyap.gov.tr` kullanılmaz.
5. **Dışarıya yalnızca hukuki terim gider.** Web aramasına ve karar sorgularına müvekkil adı, dosya numarası ya da kişileri ve olayı tanıtacak ayrıntı yazılmaz.

## Araçlar

- **Ana arama:** `search_bedesten_unified` (Yargıtay, Danıştay, yerel mahkeme, istinaf hukuk, KYB). Sonuçlar alaka sırasıyla, sayfa başına 10 karar döner.
- **Konu araştırması:** `search_smart_decisions` (100 künye) → `download_bedesten_documents` (seçilenleri indirir) → `get_bedesten_document_markdown` (tam metin, önbellekten anında gelir).
- **Alıntı denetimi:** `verify_bedesten_quote`. AYM kararlarında çalışmaz.
- **Anayasa Mahkemesi:** `search_anayasa_unified`, `get_anayasa_document_unified`.
- **Mevzuat:** Mevzuat MCP'nin `search_mevzuat` ve `search_within_mevzuat` araçları.
- `search` adlı araç kullanılmaz; o yalnızca başka bir istemcinin derin araştırma özelliği içindir.
- Araç adları istemciye göre önekli görünebilir (ör. `mcp__yargi-mcp__search_bedesten_unified`). Araçlar ertelenmiş geliyorsa önce yükle.
- Bedesten'in istek sınırı vardır: çağrıları paralel gönderme. 429 hatası gelirse sunucu bekleyip yeniden dener.

## Arama operatörleri

- **Tam ifade aramasından kaçın.** Türkçe sondan eklemeli olduğu için `"kilometresi düşürülmüş"` gibi tırnaklı aramalar "kilometresinin düşürüldüğü" geçen kararları kaçırır.
- **Terimleri AND ile bağla:** `kilometre AND gizli AND ayıp`. Operatörsüz çok kelimeli sorgu OR gibi çalışır ve binlerce gevşek sonuç getirir.
- **`kök*` ek genişletmesi:** Kelimenin sonuna `*` konursa (`taahhütname*`) hal, iyelik ve çoğul ekleri kendiliğinden eklenir. Kelime yalın hâlde yazılır; eş anlamlılar OR ile eklenir: `tahliye* AND (taahhütname* OR taahhüt*)`.
- **Fiil türevleri** `*` ile üretilmez; OR ile yazılır: `düşürülmüş OR düşürüldüğü OR düşürülmesi`.
- `AND`, `OR`, `NOT`, parantez, `+terim`, `-terim` ve `"tam ifade"` çalışır. `?`, bulanık (`~`) ve yakınlık araması desteklenmez.
- **Sıralama ve tarih:** En yeni kararlar için `sort_by="date"`; güncel içtihat için `kararTarihiStart` (ör. `"2020-01-01"`).
- **Şapkalı harf kullanma:** `velâyet*` boş döner; `velayet*` yaz.
- **Daire filtresi:** Konuya bakan daire belliyse `birimAdi` ile daralt (ör. `H3`, `C15`, `HGK`, `D10`).
- **Mevzuat aramasında** (`search_mevzuat`) AND/OR/NOT çalışmaz: `+terim1 +terim2` (ikisi de geçsin), `terim1 terim2` (biri geçsin), `-terim` (geçmesin). Joker (`yatırım*`) ve bulanık (`yatırımcı~`) desteklenir. Kanun numarasıyla arama için `mevzuat_no` (ör. `6098`).

## Bilinen bir kararı teyit etmek

Künyesi bilinen bir kararı doğrulamak için `search_smart_decisions` kullanılmaz. Esas ve karar numarası birlikte, gerekiyorsa daire filtresiyle aranır:

`search_bedesten_unified(phrase="2022/8423 AND 2023/291", court_types=["YARGITAYKARARI"], birimAdi="H3")`

## Konu araştırması: akış

1. **Sorunu ayrıştır.** Olayı ve savunulan tezi bir ile üç alt soruya böl. Her alt soru için aradığın kararı bir cümleyle tarif et. Konuya hangi dairenin baktığını belirle; emin değilsen iş bölümünü web'de kontrol et.
2. **Terimleri bul.** Doğrudan genel kelimelerle aramak yerine önce web'de hukuki makale ve yazılarda iki ila dört arama yap. Yargıtay'ın o konuda kullandığı kalıp ifadeleri topla (ör. "fiilen hareket", "temsil yetkisi").
3. **100 künye listele.** `search_smart_decisions` ile sorguyu `kök*`, AND/OR ve parantezle kur. `court_types`'ı yargı koluna göre seç (`["YARGITAYKARARI"]` ya da `["DANISTAYKARAR"]`). Güncel içtihat için `kararTarihiStart` ver, genellikle son 5-8 yıl. Çıktıda `incomplete` alanı varsa liste eksiktir.
4. **Pes etme.** Sorgu sonuç vermezse ya da sonuçlar alakasızsa farklı terimler, eş anlamlılar ve daha geniş kavramlarla yeniden ara; gerekirse web adımına dön. En az üç farklı sorgu denemeden "bulunamadı" deme.
5. **En fazla 20 karar seç.** Ölçütler: alaka sırası, konuya bakan daire, hiyerarşi ve güncellik. Birden çok sorgu yaptıysan listeleri birleştirerek seç.
6. **İndir ve oku.** Seçilenleri tek `download_bedesten_documents` çağrısıyla indir. Her birini `get_bedesten_document_markdown` ile baştan sona oku. Her kararı okuduktan sonra kısa bir not çıkar: documentId, künye, sonuç (bozma, onama; kimin lehine) ve alıntılanacak paragrafın ilk kelimeleri. Alıntıyı yazacağın zaman kararı yeniden çağır.
7. **Alıntıyı doğru yerden al.** Alıntı, kararı veren dairenin ya da kurulun kendi değerlendirmesinden alınır. Aktarılan ilk derece veya BAM gerekçesi, tebliğname, itiraz, taraf iddiası ve karşı oy, dairenin görüşüymüş gibi sunulmaz. Alıntı kısa ve hedefli olur, genellikle 40-150 kelime; gereksiz ara kısımlar `...` ile atlanır. Karar ilkeyi lehe koyup somut olayda aleyhe uygulamışsa bunu da söyle.
8. **Doğrula.** Her alıntıyı dilekçeye gireceği biçimiyle (`>>> *"..."*`, vurgu dahil) `verify_bedesten_quote` ile kontrol et. Sonuç `TAM_ESLESME` değilse aracın gösterdiği kaynak metniyle düzelt ve yeniden kontrol et. `TAM_ESLESME` almayan alıntı tırnak içinde kullanılmaz. AYM kararlarında alıntıyı `get_anayasa_document_unified` çıktısıyla kelime kelime karşılaştır.
9. **Aleyhe içtihadı da bildir.** Tezimize aykırı kararları ve karşı tarafın kullanabileceklerini kısaca yaz. Avukatın duruşmada sürprizle karşılaşmaması, lehe karar bulmak kadar önemlidir.

## Seçim ölçütleri

- **Hiyerarşi:** Önce İçtihadı Birleştirme Kararları, Hukuk Genel Kurulu ve Ceza Genel Kurulu, sonra daire kararları. Yargıtay kararı yoksa bölge adliye mahkemesi, o da yoksa ilk derece kararı.
- En güçlü üç ila beş karar önerilir. Aynı şeyi söyleyen kararlardan en üst kurulunki ve en yenisi yeter.
- Bedesten'de istinaf kararı olarak yalnızca hukuk daireleri (`ISTINAFHUKUK`) vardır; ceza istinaf kararları yoktur.
- İdari yargıda bölge idare mahkemesi kararları açık veritabanlarında yoktur. Danıştay temyiz kararlarındaki BİM özetleri ve İDDK/VDDK kararları üzerinden aranır; künye Danıştay kararının künyesidir.
- Konuyla ilgili kanun değiştiyse değişiklikten önceki karar kullanılmaz ya da değişiklik not edilir.

## Sonuç biçimi

Her önerilen karar için tezi tam bir cümleyle yaz, documentId, sonuç (bozma/onama, kimin lehine), neden uygun olduğu ve varsa zayıf yönü belirt. Altına dilekçeye hazır bloğu ver:

```markdown
**<kunye alanı aynen>** kararında şöyledir:

>>> *"...karar metninden aynen alınan kısım **dosyayla ilgili tek vurgu** devamı..."*

(Kaynak: https://mevzuat.adalet.gov.tr/ictihat/<documentId>)
```

Ardından aleyhe kararları, okunan ama seçilmeyen kararları (neden seçilmediğiyle) ve bulunamayanları (denenen sorgularla) listele.

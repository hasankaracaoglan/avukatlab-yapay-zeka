---
name: dilekce-yazimi
description: Türk hukukunda dilekçe yazım kuralları. Dava, cevap, itiraz, istinaf, temyiz, savunma, mütalaaya karşı beyan ve usuli dilekçeler yazılırken, düzenlenirken ya da denetlenirken kullan. Emsal karar alıntısının harfi harfine olması, künye ve kaynak biçimi, cümle hâlinde madde başlıkları, ölçülü kalın vurgu, italik alıntı, sonuç ve istem numaralandırması.
---

# Dilekçe yazımı

Bu beceri bir avukatın dilekçe yazım alışkanlıklarını toplar. Kendi tarzınıza uymayan maddeyi değiştirebilir ya da silebilirsiniz.

## İşe başlamadan

- **Avukat bilgileri** (antet, vekil adı, imza) bu klasördeki `profil.md` dosyasındadır. Dosya yoksa ya da ilgili satırda `[doldurun: ...]` yer tutucusu kalmışsa bilgiyi kullanıcıya sor. Adres, telefon ya da baro bilgisi uydurma.
- **UDF çıktısı:** Dilekçe UYAP Doküman Editörü dosyası (.udf) ya da PDF olarak üretilecekse `uyap-udf-sablonu` becerisini de uygula.
- **Emsal karar ve mevzuat:** Esasa ilişkin her dilekçede, kullanıcının istemesini beklemeden emsal karar ve mevzuat kullanılır. Araştırma için `ictihat-arastirmasi` becerisini uygula. Yargı MCP ve Mevzuat MCP kurulu değilse karar alıntısı yapma; bunu kullanıcıya bildir ve dilekçeyi alıntısız yaz.
- **Gizlilik:** Web aramasına ve karar arama sorgularına müvekkil adı, dosya numarası ya da kişileri ve olayı tanıtacak ayrıntı yazılmaz. Yalnızca hukuki terimler aranır.

## Kurallar

### 1. Karar uydurulmaz

Var olmayan, sahte ya da doğrulanmamış bir Yargıtay, Danıştay, bölge adliye mahkemesi, Anayasa Mahkemesi veya kurul kararı yazılmaz. Yalnızca veritabanından alınmış, gerçek kararlar kullanılır. Uygun karar bulunamazsa uydurmak yerine "Bu konuda uygun bir emsal karar bulunamadı." diye açıkça bildirilir.

### 2. Tırnak içi harfi harfine aynıdır

Tırnak içine (`"..."` ya da `“...”`) alınan her ifade, kaynak metindeki cümlelerle kelimesi kelimesine aynı olmak zorundadır. Tırnak içine özet, başka sözlerle anlatım ya da yapay cümle yazılmaz.

- Atlama yapılacaksa yalnızca aradaki kısım üç nokta (`...`) ile atlanır. Tırnak içinde kalan bütün kelimeler kaynakla birebir örtüşür.
- Tırnaksız olarak kendi cümlenle özet aktarabilirsin. Tırnak açıldığı anda bu kural bağlayıcıdır.
- Bedesten'den alınan kararlardaki (Yargıtay, Danıştay, BAM) alıntılar, dilekçe tamamlanmadan önce dilekçedeki hâliyle Yargı MCP'nin `verify_bedesten_quote` aracıyla kontrol edilir. Sonuç `TAM_ESLESME` değilse alıntı, aracın gösterdiği kaynak metne göre düzeltilir ya da tırnaksız anlatıma çevrilir.
- Anayasa Mahkemesi kararlarında bu araç çalışmaz. Alıntı karar metniyle elle, kelime kelime karşılaştırılır.

### 3. Emsal karar özetlenmez, aynen alıntılanır

Emsal karar kullanılırken kararın metni özetlenmez. Dosyayla ilgili kısım doğrudan, tırnak içinde ve blok hâlinde aynen alıntılanır. Kararın dairesi, esas numarası, karar numarası ve tarihi açıkça yazılır.

Alıntı, kararı veren dairenin ya da kurulun kendi değerlendirmesinden alınır. Karar metninde aktarılan ilk derece veya BAM gerekçesi, tebliğname, itiraz, taraf iddiası ve karşı oy, dairenin görüşüymüş gibi sunulmaz.

### 4. Künye ve kaynak biçimi sabittir

Alıntının yerleşimi şöyledir:

```markdown
**Yargıtay 3. Hukuk Dairesi, 2022/8423 Esas, 2023/291 Karar, 23.02.2023 tarihli** kararında şöyledir:

>>> *"Kira ilişkisinden doğan ve üç aylık kira tutarı kesinlik sınırını geçmeyen diğer davalara ilişkin nihai kararlar, 6100 sayılı Hukuk Muhakemeleri Kanunu'nun 362 nci maddesi uyarınca **temyiz edilemez.**"*

(Kaynak: https://mevzuat.adalet.gov.tr/ictihat/893377000)
```

- **Künye** giriş cümlesinin içinde, alıntının en başında, kalın ve italik olmadan yazılır. Giriş cümlesinin geri kalanı ("kararında şöyledir:" gibi) serbest kurulabilir ve normal yazılır.
- Künye, Yargı MCP araçlarının döndürdüğü `kunye` alanından aynen alınır. Karar tarihi karar metninden çıkarılmaz; metinde alt mahkeme ve BAM kararlarının tarihleri de geçer.
- **Alıntı** `>>>` ile girintili, tırnak içinde ve italik (`*"..."*`) yazılır.
- **Kaynak** alıntının hemen altında, parantez içinde ve italik olmadan yazılır.
  - Kararın resmi tam bağlantısı varsa kaynak satırına yalnızca bu bağlantı yazılır: `(Kaynak: https://mevzuat.adalet.gov.tr/ictihat/<belge-no>)`. Bu durumda ana adres ayrıca yazılmaz.
  - Tam bağlantı yoksa resmi sitenin ana adresi yazılır: `(Kaynak: mevzuat.adalet.gov.tr)`.
  - Yargıtay kararlarında kaynak daima mevzuat.adalet.gov.tr üzerinden gösterilir; `emsal.uyap.gov.tr` yazılmaz.
  - AYM ve Danıştay kararlarında resmi tam bağlantı bulunabiliyorsa mutlaka eklenir. Resmi olmayan (özel, üçüncü taraf) sitelerin bağlantısı konmaz. Avukat dilekçeyi UYAP'a yüklemeden önce bağlantıları isterse kendisi siler.

### 5. Madde başlıkları tam cümledir

Madde başlıkları salt kavram tamlaması (ör. "MÜVEKKİLİN AİLEVİ DURUMU VE AĞIR MAĞDURİYETİ") olarak değil, o maddedeki iddiayı özetleyen tam ve kurallı bir cümle olarak kurulur (ör. "MÜVEKKİL AİLESİNE BAKMAKLA YÜKÜMLÜDÜR VE TUTUKLULUĞUN DEVAMI AĞIR MAĞDURİYET YARATACAKTIR."). Başlık büyük harfle ve kalın yazılır.

### 6. Usuli dilekçeler kısadır

Esasa ilişkin savunma içermeyen, yalnızca belge istenmesi, baro görevlendirmesi, adres bildirimi gibi usuli ya da idari dilekçelerde gereksiz ayrıntıya ve uzun maddelere girilmez. Talep tek, kısa ve doğrudan bir paragrafta yazılır. Bu dilekçelerde emsal karar araştırması ve kalın vurgu gerekmez.

### 7. Sonuç ve istem numaralıdır

"SONUÇ VE İSTEM :" başlığından sonraki giriş cümlesi alt satıra indirilmez, başlıkla aynı satıra yazılır. Talepler bunun altında **1., 2., 3.** diye numaralandırılarak ayrı ayrı yazılır. Son talebin sonuna, bitişik olarak dilekçenin tarihi eklenir.

```markdown
**SONUÇ VE İSTEM :** Yukarıda arz ve izah edilen ve Sayın Mahkemenizce re'sen gözetilecek nedenlerle;

1. Davanın kabulüne,

2. Yargılama giderleri ile vekâlet ücretinin davalıya yükletilmesine karar verilmesini saygıyla arz ve talep ederim. 24.11.2026
```

### 8. Vurgu ölçülüdür

Esasa ilişkin dilekçelerin gövdesinde, hâkimin gözünden kaçmaması gereken **birkaç yer** kendiliğinden kalın yazılır. Vurgu az olduğu için etkilidir; her yer kalın olursa hiçbir yer öne çıkmaz.

- **Her maddede en fazla iki yer** kalın yazılır: o maddeyi kazandıracak tek beyan ya da olgu ve gerekirse maddenin sonuç cümlesi (ör. **davanın reddine karar verilmesi gerekmektedir.**).
- **Bir paragrafta en fazla bir kalın yer** bulunur. Paragrafın tamamı kalın yapılmaz.
- **Her emsal karar alıntısında yalnızca bir yer**, yani kararın dosyayla doğrudan ilgili kısmı, kalın-italik yapılır. Alıntının geri kalanı italik kalır.
- Giriş paragrafı, kişisel durum bilgileri ve ikinci derecedeki tespitler kalın yazılmaz.
- Madde başlıkları, künye ve SONUÇ VE İSTEM zaten biçimli olduğu için ayrıca vurgulanmaz.
- Tırnak içindeki bir ifadeyi kalın yapmak 2. kuralı etkilemez: yalnızca biçim eklenir, tek kelime değiştirilmez.

### 9. Alıntılar italiktir

Bir belgeden ya da beyandan kelimesi kelimesine aktarılan her alıntı tırnak içinde ve italik yazılır: tanık, sanık, müşteki ve bilirkişi beyanları; duruşma ve kolluk tutanakları; rapor, müzekkere cevabı ve resmi yazılar; mütalaa, iddianame ve karar metinleri; kanun maddesinin metni.

- Vurgulanan alıntı kalın-italik olur. Alıntının yalnızca bir kısmı vurgulanacaksa alıntı italik kalır, o kısım kalın-italik yapılır.
- Bir belgeden aktarılmayan, kavram olarak tırnağa alınan kelimeler (ör. "satış" iddiası) ve madde başlıkları italik yapılmaz.
- Alıntı italik yapılırken kaynağıyla bir kez daha karşılaştırılır. Tırnak içindeki ifade kaynakta aynen geçmiyorsa kaynaktaki hâliyle düzeltilir ya da tırnaksız anlatıma çevrilir.

Yazım biçimi:

- Düz alıntı: `*"Olay günü işyerinde değildim."*`
- Bütünüyle vurgulanan alıntı: `***"Sözleşmeyi imzalamadım."***`
- Bir kısmı vurgulanan alıntı: `*"Teslim tarihinde **eser eksiksiz olarak** davalıya verildi."*`
- Yıldızlar kelimeye bitişik yazılır (`**vurgu**`); yıldız ile kelime arasına boşluk konmaz. Metinde tek başına bir `*` karakteri kullanılmaz, biçim işareti sanılır.

### 10. Soyadları büyük harfle yazılır

Dilekçede geçen bütün şahısların (taraf, vekil, sanık, tanık vb.) soyadı büyük harfle yazılır: `Ayşe ÖRNEK`, `Av. Zeynep DENEME`. Tırnak içindeki doğrudan alıntılarda adın kaynaktaki yazımı korunur.

## Bitirmeden önce

1. Tırnak içindeki her karar alıntısı `verify_bedesten_quote` ile `TAM_ESLESME` aldı mı?
2. Her künye `kunye` alanından mı alındı, kaynak satırı doğru mu?
3. Vurgu sınırları aşıldı mı?
4. Antet, vekil adı ve imza `profil.md`'deki bilgilerle mi yazıldı?
5. Tarih, sonuç ve istemin sonunda mı?

Yapay zekâ yardımıyla yazılan dilekçenin içeriğinden ve doğruluğundan dilekçeyi imzalayan avukat sorumludur. Dilekçeyi göndermeden önce baştan sona okuyun.

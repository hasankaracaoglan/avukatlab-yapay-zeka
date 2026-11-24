# AvukatLab Yapay Zekâ Paketi

Claude, Antigravity gibi yapay zekâ asistanlarıyla dilekçe yazan avukatlar için kural ve araç paketi:

- **Üç beceri (skill):** Asistan dilekçe yazarken bu kuralları uygular.
  - `dilekce-yazimi`: Emsal karar alıntısının harfi harfine olması, künye ve kaynak biçimi, cümle hâlinde madde başlıkları, ölçülü vurgu, sonuç ve istem.
  - `uyap-udf-sablonu`: UYAP Doküman Editörü şablonu. Antet, mahkeme başlıkları ve "Gönderilmek Üzere" sırası, TAB'la hizalı künye, numaralandırma, imza.
  - `ictihat-arastirmasi`: Bedesten arama operatörleri, 100 künyeden 20 tam metne inen araştırma yöntemi, alıntının karar metniyle doğrulanması.
- **Üç MCP sunucusu** (ayrı depolarda): [Yargı MCP](https://github.com/hasankaracaoglan/yargi-mcp), [Mevzuat MCP](https://github.com/hasankaracaoglan/mevzuat-mcp), [UYAP UDF MCP](https://github.com/hasankaracaoglan/uyap-udf-mcp).

Beceriler bir avukatın kendi dilekçe alışkanlıklarından derlendi. Hepsi düz metin dosyalarıdır: istediğinizi kurar, istediğiniz maddeyi kendi tarzınıza göre değiştirirsiniz.

## Kurulum

### En kolay yol: asistanınıza kurdurun

Claude Code ya da Antigravity gibi bilgisayarınızda komut çalıştırabilen bir asistan kullanıyorsanız, sohbete şunu yapıştırın:

```
AvukatLab paketini benim için kur:
https://github.com/hasankaracaoglan/avukatlab-yapay-zeka
Adımlar depodaki KURULUM.md dosyasında.
```

Asistan neleri kurmak istediğinizi sorar. Sonra antetiniz için adınızı, adresinizi ve telefonunuzu sorar. MCP sunucularını ve becerileri kurar, her komuttan önce izninizi ister. Bitince asistanı kapatıp açmanız yeterlidir.

### Elle kurulum

1. Bu depoyu indirin (yeşil **Code** düğmesi › **Download ZIP**) ve açın.
2. `skills/dilekce-yazimi/profil.md` dosyasını kendi bilgilerinizle doldurun.
3. İstediğiniz beceri klasörlerini kopyalayın:
   - **Claude Code:** `~/.claude/skills/`
   - **Antigravity:** genel beceri klasörü (yeni sürümlerde `~/.gemini/config/skills/`; bkz. [Antigravity belgeleri](https://antigravity.google/docs/skills))
   - **Claude Desktop:** her beceri klasörünü ayrı bir zip yapıp ayarlardaki beceriler (Skills) bölümünden yükleyin.
4. MCP sunucularını kendi sayfalarındaki adımlarla kurun.

### Beceriler her zaman devrede olsun

Asistan beceriyi işe göre kendisi seçer; dilekçe yazarken kaçırmasın isterseniz genel kural dosyanıza (Claude Code'da `~/.claude/CLAUDE.md`) şu paragrafı ekleyin:

```
Dilekçe, itiraz, istinaf, temyiz veya savunma yazarken `dilekce-yazimi` becerisini; dilekçe UDF'e ya da PDF'e çevrilirken `uyap-udf-sablonu` becerisini; emsal karar aranırken `ictihat-arastirmasi` becerisini kullan.
```

Beceriyi elle de çağırabilirsiniz: `/dilekce-yazimi`.

## Güncelleme ve kaldırma

- **MCP sunucuları** her açılışta GitHub'daki son sürümü kullanır.
- **Beceriler** sizin kopyanızdır ve kendiliğinden güncellenmez; böylece değişiklikleriniz korunur. Yeni sürüm için asistana "AvukatLab becerilerini güncelle, benim değişikliklerimi koru" deyin.
- **Kaldırmak** için beceri klasörlerini silin ve varsa "her zaman" paragrafını kural dosyanızdan çıkarın.

## Bilmeniz gerekenler

- **Sorumluluk:** Yapay zekâ yardımıyla yazılan dilekçenin içeriğinden ve doğruluğundan dilekçeyi imzalayan avukat sorumludur. Beceriler alıntının karar metniyle denetlenmesini zorunlu tutar, ama asistan yine de hata yapabilir. Göndermeden önce dilekçeyi baştan sona okuyun.
- **Gizlilik:** Sohbette yazdıklarınızı kullandığınız yapay zekâ hizmeti işler; bunun için o hizmetin koşulları geçerlidir. Müvekkil bilgilerini yazmadan önce bunu göz önünde bulundurun. Beceriler, web ve karar aramalarına müvekkil adı ya da dosya numarası yazılmamasını şart koşar.
- **Bağımsızlık:** Bu paket Adalet Bakanlığı, UYAP, HAVELSAN, Anthropic ya da Google ile bağlantılı değildir.

## Lisans

Beceriler ve belgeler [Creative Commons Atıf 4.0 Uluslararası (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/deed.tr) lisansıyla paylaşılır: kaynağı belirterek kopyalayabilir, değiştirebilir ve paylaşabilirsiniz. MCP sunucularının lisansı kendi depolarındadır (MIT).

Hazırlayan: Hasan Karacaoğlan · [avukatlab.tr](https://avukatlab.tr)

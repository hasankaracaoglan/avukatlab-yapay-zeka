# Kurulum talimatı (yapay zekâ asistanı için)

Bu dosya, kullanıcının "AvukatLab paketini kur" dediği yapay zekâ asistanı içindir. Adımları sırayla izle. Komut çalıştırmadan ve dosya değiştirmeden önce kullanıcıdan izin iste. Kullanıcı avukattır ve teknik ayrıntı beklemez: soruları kısa, açık ve Türkçe sor.

## 0. Önce sor

Kullanıcıya iki soru sor:

1. **Neleri kuralım?** Varsayılan: hepsi.
   - MCP sunucuları: Yargı MCP (karar arama), Mevzuat MCP (mevzuat), UYAP UDF MCP (UDF dilekçe ve PDF).
   - Beceriler: `dilekce-yazimi`, `uyap-udf-sablonu`, `ictihat-arastirmasi`.
2. **Hangi asistanda kullanacaksınız?** Claude Code, Antigravity, Claude Desktop ya da birkaçı. Sen hangi uygulamada çalışıyorsan onu varsayılan öner.

Beceriler MCP sunucuları olmadan da kurulabilir, ama `ictihat-arastirmasi` Yargı MCP'siz, `uyap-udf-sablonu` da UYAP UDF MCP'siz işe yaramaz. Bunu kullanıcıya bir cümleyle söyle.

## 1. MCP sunucuları

Her sunucunun kendi README'sindeki "Yapay zekâ asistanıyla kurulum" bölümünü izle:

- https://github.com/hasankaracaoglan/yargi-mcp
- https://github.com/hasankaracaoglan/mevzuat-mcp
- https://github.com/hasankaracaoglan/uyap-udf-mcp

Özetle: `uv` kurulu değilse kur; `uvx`'in tam yolunu bul ve ayarlarda daima tam yolu kullan; sunucuyu asistanın MCP ayarına `<uvx tam yolu> --from git+https://github.com/hasankaracaoglan/<ad> <ad>` komutuyla ekle; JSON ayar dosyasını değiştirmeden önce yedeğini al ve var olan sunuculara dokunma. Sunucunun kendisini terminalde çalıştırma, girdi bekleyip açık kalır.

## 2. Becerileri indir

Bu deponun son hâlini geçici bir klasöre indir:

```bash
TMP=$(mktemp -d)
curl -L https://github.com/hasankaracaoglan/avukatlab-yapay-zeka/archive/refs/heads/main.tar.gz | tar -xz -C "$TMP"
# beceriler: $TMP/avukatlab-yapay-zeka-main/skills/<beceri-adı>/
```

`git` kuruluysa `git clone --depth 1` de kullanılabilir.

## 3. Profil

`dilekce-yazimi/profil.md` antet, vekil adı ve imza bilgilerini tutar. Kopyalamadan önce kullanıcıya sor ve indirilen kopyadaki `profil.md`'yi doldur:

1. Adınız ve soyadınız (dilekçede "Av. Ad SOYAD" biçiminde yazılır)?
2. Büro adresiniz (antetin ikinci satırı)?
3. Antette hangi telefon yazsın?
4. Antete e-posta, KEP adresi ya da baro sicil numarası da eklensin mi?
5. Büronuzda sizin adınıza dilekçe yazacak başka avukat var mı? Varsa onun bilgileri de ayrı bölüm olarak eklenir.
6. Nöbetçi mahkeme başlıklarında hangi şehir yazsın?

Kullanıcı bir bilgiyi vermek istemezse o satırı `[doldurun]` olarak bırak; dilekçe yazılırken beceri o bilgiyi sorar. Bilgileri dosya dışında hiçbir yere yazma ya da gönderme.

## 4. Becerileri kopyala

Kopyalanacak klasör `skills/<beceri-adı>/` klasörünün tamamıdır (içindeki `SKILL.md` ve diğer dosyalarla).

| Asistan | Hedef klasör |
|---|---|
| Claude Code | `~/.claude/skills/<beceri-adı>/` |
| Antigravity | Genel beceri klasörü. Yeni sürümlerde `~/.gemini/config/skills/<beceri-adı>/`, eski sürümlerde `~/.gemini/antigravity/skills/<beceri-adı>/`. Hangisinin geçerli olduğundan emin değilsen Antigravity'nin beceri belgelerine bak (https://antigravity.google/docs/skills). |
| Claude Desktop | Klasör kopyalanmaz. Her beceri klasörünü ayrı bir `.zip` dosyası yap (zip'in içinde en üstte `<beceri-adı>/SKILL.md` olsun) ve kullanıcının masaüstüne koy. Kullanıcıya, Claude Desktop'ın ayarlarındaki beceriler (Skills) bölümünden bu zip'leri yüklemesini söyle. Arayüz adları sürüme göre değişebilir; becerilerin çalışması için ayarlarda kod çalıştırma özelliğinin açık olması gerekebilir. |

- Hedefte aynı adlı bir klasör zaten varsa **üzerine yazma.** Kullanıcı onu kendine göre değiştirmiş olabilir. Önce farkları göster, ne yapılacağını sor (güncelleme için 7. adıma bak).
- Kopyaladıktan sonra her hedefte `SKILL.md` dosyasının bulunduğunu kontrol et.

## 5. "Her zaman kullan" notu (isteğe bağlı)

Asistan beceriyi kendisi devreye sokar, ama bazen kaçırabilir. Kullanıcıya sor: "Dilekçe yazarken bu beceriler her zaman kullanılsın mı?" Evet derse asistanın genel kural dosyasına şu tek paragrafı ekle (önce dosyanın yedeğini al, dosyadaki başka hiçbir şeye dokunma):

```
Dilekçe, itiraz, istinaf, temyiz veya savunma yazarken `dilekce-yazimi` becerisini; dilekçe UDF'e ya da PDF'e çevrilirken `uyap-udf-sablonu` becerisini; emsal karar aranırken `ictihat-arastirmasi` becerisini kullan.
```

| Asistan | Genel kural dosyası |
|---|---|
| Claude Code | `~/.claude/CLAUDE.md` (yoksa oluştur) |
| Antigravity | Genel kural dosyası (genellikle `~/.gemini/GEMINI.md`; senin sürümünde başka bir yer kullanılıyorsa onu kullan) |
| Claude Desktop | Dosya yoktur. Kullanıcıya bu paragrafı Claude Desktop'ın ayarlarındaki kişisel talimat alanına ya da kullandığı projenin talimatlarına yapıştırmasını söyle. |

## 6. Bitir

1. Kullanıcıya asistan uygulamasını tamamen kapatıp yeniden açmasını söyle.
2. Nasıl deneyeceğini söyle: "Bir kira alacağı davası için kısa bir dava dilekçesi taslağı yaz" ya da doğrudan `/dilekce-yazimi`.
3. Neyin nereye kurulduğunu kısa bir listeyle bildir. Yedeklerin yerini de söyle.
4. Kuralların kullanıcının kendi kopyası olduğunu, istediği maddeyi `SKILL.md` dosyalarında değiştirebileceğini söyle.

## 7. Güncelleme

Kullanıcı "AvukatLab becerilerini güncelle" derse:

1. Deponun son hâlini 2. adımdaki gibi geçici klasöre indir.
2. Her beceri için kullanıcının kurulu kopyasıyla yeni sürümü karşılaştır. Kullanıcının kendi yaptığı değişiklikleri ve `profil.md`'yi koru.
3. Yeni sürümdeki değişiklikleri kullanıcıya kısaca anlat, onay alınca uygula. Güncellemeden önce eski klasörün yedeğini al.

MCP sunucuları her açılışta GitHub'daki son sürümü kendiliğinden kullanır; onlar için bir şey yapmak gerekmez.

## 8. Kaldırma

Kullanıcı kaldırmak isterse: beceri klasörlerini sil (Mac'te Çöp Kutusu'na taşı), 5. adımdaki paragrafı kural dosyasından çıkar ve MCP sunucularını asistanın MCP ayarından kaldır. Her dosyayı değiştirmeden önce yedeğini al.

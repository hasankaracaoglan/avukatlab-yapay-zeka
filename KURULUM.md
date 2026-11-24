# Kurulum talimatı (yapay zekâ asistanı için)

Bu dosya, kullanıcının "AvukatLab paketini kur" dediği yapay zekâ asistanı içindir. Adımları sırayla izle. Kullanıcı avukattır ve teknik ayrıntı beklemez: soruları kısa, açık ve Türkçe sor.

Bu talimat, MCP sunucularının README'lerindeki özgün projeye ait kurulum bölümlerinin (Antigravity betikleri, uzak sunucu adresleri, PyPI'den `uvx yargi-mcp`, `saidsurucu/...` depoları) **yerine geçer**. O bölümleri uygulama.

## 1. Soruları tek seferde sor

Hepsini tek mesajda sor:

1. **Neleri kuralım?** Varsayılan: hepsi.
   - MCP sunucuları: Yargı MCP (karar arama), Mevzuat MCP (mevzuat), UYAP UDF MCP (UDF dilekçe ve PDF).
   - Beceriler: `dilekce-yazimi`, `uyap-udf-sablonu`, `ictihat-arastirmasi`.
   - Not olarak söyle: `ictihat-arastirmasi` Yargı MCP'siz, `uyap-udf-sablonu` UYAP UDF MCP'siz işe yaramaz.
2. **Hangi asistanda kullanacaksınız?** Claude Code, Antigravity, Claude Desktop ya da birkaçı. Sen hangi uygulamada çalışıyorsan onu varsayılan öner.
3. **Antet ve imza bilgileri** (`dilekce-yazimi` kurulacaksa):
   - Adınız ve soyadınız (dilekçede "Av. Ad SOYAD" biçiminde yazılır)?
   - Büro adresiniz?
   - Antette hangi telefon yazsın?
   - Antete e-posta, KEP adresi ya da baro sicil numarası da eklensin mi?
   - Büroda sizin adınıza dilekçe yazacak başka avukat var mı? Varsa onun bilgileri.
   - Nöbetçi mahkeme başlıklarında hangi şehir yazsın?
4. **Dilekçe yazarken bu beceriler her zaman kullanılsın mı?** Asistan beceriyi işe göre kendisi seçer, ama bazen kaçırabilir. Evet denirse 7. adımdaki tek paragraf kural dosyasına eklenir.

Cevaplar gelince 2. adımdaki kontrolleri yap ve kullanıcıya **tek bir plan** göster: ne nereye kurulacak, hangi ayar dosyaları değişecek (yedekleriyle), ilk indirmenin yaklaşık süresi ve boyutu. Plan onaylanınca uygula. Kullandığın uygulama komut ve dosya değişikliği için ayrıca izin soruyorsa onu ayrıca tekrar sorma.

## 2. Ön kontroller

- **uvx'in tam yolu:** `command -v uvx`. Boş dönerse şu yollara bak: `~/.local/bin/uvx`, `/opt/homebrew/bin/uvx`, `/usr/local/bin/uvx` (Windows'ta `%USERPROFILE%\.local\bin\uvx.exe`). Hiçbirinde yoksa uv'yi kur:
  - macOS/Linux: `curl -LsSf https://astral.sh/uv/install.sh | sh`
  - Windows: `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`
  - Kurulumdan sonra yolu yeniden bul. Ayarlarda **daima tam yolu** kullan; masaüstü uygulamaları `~/.local/bin`'i PATH'te görmez.
- **Antigravity'nin yerleri:** `~/.gemini/config/` klasörü varsa yeni sürümdür: MCP ayarı `~/.gemini/config/mcp_config.json`, beceriler `~/.gemini/config/skills/`, kurallar `~/.gemini/config/rules/`. Yoksa eski yerleri kullan: `~/.gemini/antigravity/mcp_config.json` ve `~/.gemini/antigravity/skills/`. Belge: https://antigravity.google/docs/skills
- **Claude Desktop:** ayar dosyası macOS'ta `~/Library/Application Support/Claude/claude_desktop_config.json`, Windows'ta `%APPDATA%\Claude\claude_desktop_config.json`. Düzenlemeden önce kullanıcıdan Claude Desktop'ı kapatmasını iste.
- **Var olan kurulumlar:** Her hedefte aynı adlı MCP girdisi (`yargi-mcp`, `mevzuat-mcp`, `uyap-udf-mcp`), aynı adlı beceri klasörü ya da 7. adımdaki paragraf zaten var mı, bak. Claude Code için `claude mcp list` ya da `claude mcp get <ad>`. Eski Yargı MCP kullanıcılarında `uvx yargi-mcp` gibi artık çalışmayan bir girdi olabilir. Var olanı planda göster; değiştirmek için ayrıca izin al.
- **Yedek klasörü:** Bütün yedekler tek bir yere alınır: `~/AvukatLab-yedekler/<YYYYAAGG-SSDD>/`. Yedekleri **asla** beceri ya da kural klasörlerinin içine koyma; oradaki kopya ikinci bir beceri ya da kural olarak yüklenir.

## 3. MCP sunucuları

Her sunucu için komut `<uvx tam yolu> --from git+https://github.com/hasankaracaoglan/<ad> <ad>` biçimindedir. `<ad>`: `yargi-mcp`, `mevzuat-mcp`, `uyap-udf-mcp`.

- **Claude Code:**
  ```
  claude mcp add yargi-mcp -s user -- <uvx tam yolu> --from git+https://github.com/hasankaracaoglan/yargi-mcp yargi-mcp
  claude mcp add mevzuat-mcp -s user -- <uvx tam yolu> --from git+https://github.com/hasankaracaoglan/mevzuat-mcp mevzuat-mcp
  claude mcp add uyap-udf-mcp -s user -- <uvx tam yolu> --from git+https://github.com/hasankaracaoglan/uyap-udf-mcp uyap-udf-mcp
  ```
  Aynı adlı eski girdi varsa ve kullanıcı değiştirmeyi onayladıysa önce `claude mcp remove <ad> -s user`.
- **Claude Desktop ve Antigravity:** JSON ayar dosyasının yedeğini al. Dosya yoksa `{"mcpServers": {}}` ile oluştur. `mcpServers` nesnesine şu girdileri ekle, var olan diğer sunuculara dokunma, sonra dosyanın geçerli JSON olduğunu doğrula:
  ```json
  "yargi-mcp": {"command": "<uvx tam yolu>", "args": ["--from", "git+https://github.com/hasankaracaoglan/yargi-mcp", "yargi-mcp"]},
  "mevzuat-mcp": {"command": "<uvx tam yolu>", "args": ["--from", "git+https://github.com/hasankaracaoglan/mevzuat-mcp", "mevzuat-mcp"]},
  "uyap-udf-mcp": {"command": "<uvx tam yolu>", "args": ["--from", "git+https://github.com/hasankaracaoglan/uyap-udf-mcp", "uyap-udf-mcp"]}
  ```
- **Ön ısıtma:** İlk açılışta sunucunun bağımlılıkları indirilir. Bu sürede uygulama sunucuyu bozuk sanabilir, o yüzden önceden yap. Her sunucu için şunu çalıştır:

  `<uvx tam yolu> --from git+https://github.com/hasankaracaoglan/<ad> python -c "print('ok')"`

  Çıktı `ok` olmalı. Yargı MCP boş bilgisayarda 2-3 dakika sürer ve 600 MB civarı indirir. Kullanıcıya önceden söyle; komutun zaman aşımı en az 5 dakika olsun ya da arka planda çalıştır.
- **Sunucunun kendisini** (`... yargi-mcp` komutunu) terminalde çalıştırma; girdi bekleyip açık kalır.
- PDF dönüştürme için Mac'te Uyap Doküman Editörü'nün `/Applications` altında kurulu olması gerekir; ayrıca Java gerekmez.

## 4. Depoyu indir ve sürümü kaydet

```bash
curl -fL https://github.com/hasankaracaoglan/avukatlab-yapay-zeka/archive/refs/heads/main.tar.gz -o /tmp/avukatlab.tar.gz
mkdir -p /tmp/avukatlab && tar -xzf /tmp/avukatlab.tar.gz -C /tmp/avukatlab
curl -fsS https://api.github.com/repos/hasankaracaoglan/avukatlab-yapay-zeka/commits/main | grep -m1 '"sha"'
```

- Beceriler `/tmp/avukatlab/avukatlab-yapay-zeka-main/skills/<beceri-adı>/` altındadır. Kabuk değişkenleri bir komuttan ötekine taşınmayabilir; yolları her komutta açıkça yaz.
- Son komutun verdiği sürüm kimliğini (sha) not et; 6. adımda kullanılacak. `git` kuruluysa `git clone --depth 1` ve `git rev-parse HEAD` de olur.

## 5. Profili doldur

İndirilen kopyadaki `skills/dilekce-yazimi/profil.md`'yi 1. adımdaki cevaplarla doldur:

- Antet 1. satırı Türkçe büyük harfle yazılır: i → İ, ı → I (`Deniz` → `DENİZ`, `Işık` → `IŞIK`). Programlama dillerinin büyük harf işlevlerine güvenme.
- Soyad dilekçenin her yerinde büyük harfle yazılır: `Av. Deniz TEST`.
- Kullanıcının istemediği isteğe bağlı satırı (e-posta, KEP, sicil) sil.
- `[doldurun]` yalnızca kullanıcının vermediği zorunlu bilgide kalır; dilekçe yazılırken beceri onu sorar.
- Bu bilgileri profil dosyası dışında hiçbir yere yazma ya da gönderme.

## 6. Becerileri kopyala

Kopyalanacak olan, `skills/<beceri-adı>/` klasörünün tamamıdır.

| Asistan | Hedef |
|---|---|
| Claude Code | `~/.claude/skills/<beceri-adı>/` |
| Antigravity | 2. adımda bulunan beceri klasörü: `<...>/skills/<beceri-adı>/` |
| Claude Desktop | Klasör kopyalanmaz, zip yapılır (aşağıda). |

- Hedefte aynı adlı klasör varsa üzerine yazma. Kullanıcı onu kendine göre değiştirmiş olabilir; 9. adımdaki güncelleme yolunu izle.
- Her kopyanın içine `.avukatlab-surum` adlı bir dosya yaz, içinde 4. adımdaki sürüm kimliği (sha) olsun. Güncellemede kullanıcının değişikliklerini ayırmak için gerekir.
- Kopyaladıktan sonra her hedefte `SKILL.md` bulunduğunu kontrol et.
- **Claude Desktop için zip:** Her beceri için:

  ```bash
  cd /tmp/avukatlab/avukatlab-yapay-zeka-main/skills && zip -r -X ~/Desktop/<beceri-adı>.zip <beceri-adı> -x '*.DS_Store'
  ```

  `unzip -l` ile zip'in en üstünde `<beceri-adı>/SKILL.md` olduğunu kontrol et. Kullanıcıya şunları söyle:
  - Zip'leri Claude Desktop'ın ayarlarındaki beceriler (Skills) bölümünden yüklesin. Menü adları sürüme göre değişebilir. Becerilerin çalışması için ayarlarda kod çalıştırma özelliğinin açık olması gerekebilir.
  - `dilekce-yazimi.zip` antet bilgilerini içerir ve Claude'un hizmetine yüklenir.
  - Profil değişirse zip'in yeniden yapılıp yüklenmesi gerekir.
  - Yükledikten sonra masaüstündeki zip'leri silebilir.

## 7. "Her zaman kullan" paragrafı (1. adımda evet dendiyse)

Eklenecek metin:

```
Dilekçe, itiraz, istinaf, temyiz veya savunma yazarken `dilekce-yazimi` becerisini; dilekçe UDF'e ya da PDF'e çevrilirken `uyap-udf-sablonu` becerisini; emsal karar aranırken `ictihat-arastirmasi` becerisini kullan.
```

- Paragraf hedefte zaten varsa yeniden ekleme. Dosyayı değiştirmeden önce yedeğini al; dosyadaki başka hiçbir şeye dokunma.

| Asistan | Nereye |
|---|---|
| Claude Code | `~/.claude/CLAUDE.md` dosyasının sonuna (yoksa oluştur). |
| Antigravity (yeni) | Yeni bir dosya: `~/.gemini/config/rules/avukatlab.md`. Dosya **mutlaka** şu baş bilgiyle başlar; başlamazsa Antigravity kuralı uyarı vermeden yok sayar: `---` / `trigger: always_on` / `description: AvukatLab becerilerini dilekçe işlerinde kullan` / `---`, ardından paragraf. |
| Antigravity (eski) | `~/.gemini/GEMINI.md` dosyasının sonuna (baş bilgi gerekmez). |
| Claude Desktop | Dosya yoktur. Kullanıcıya paragrafı Claude Desktop ayarlarındaki kişisel tercihler/talimatlar alanına ya da kullandığı projenin talimatlarına yapıştırmasını söyle. |

## 8. Bitir

1. Geçici klasörleri temizle: `/tmp/avukatlab` ve `/tmp/avukatlab.tar.gz`. İçinde doldurulmuş profil var. Mac'te Çöp Kutusu'na taşı.
2. Kullanıcıya asistan uygulamalarını tamamen kapatıp yeniden açmasını söyle.
3. Nasıl deneyeceğini söyle: "Bir kira alacağı davası için kısa bir dava dilekçesi taslağı yaz" ya da doğrudan `/dilekce-yazimi`.
4. Neyin nereye kurulduğunu ve yedeklerin yerini (`~/AvukatLab-yedekler/...`) kısa bir listeyle bildir.
5. Kuralların kullanıcının kendi kopyası olduğunu, istediği maddeyi `SKILL.md` dosyalarında değiştirebileceğini söyle.

## 9. Güncelleme

Kullanıcı "AvukatLab becerilerini güncelle" derse:

1. Kurulu her becerinin `.avukatlab-surum` dosyasından kurulduğu sürümü oku.
2. O sürümü (`https://github.com/hasankaracaoglan/avukatlab-yapay-zeka/archive/<sha>.tar.gz`) ve son sürümü (4. adım) ayrı geçici klasörlere indir.
3. Her dosya için üç hâli karşılaştır: kurulan sürüm, kullanıcının şimdiki kopyası, son sürüm.
   - Kullanıcının kopyası kurulan sürümden farklıysa, o fark kullanıcının değişikliğidir; korunur.
   - Son sürüm kurulan sürümden farklıysa, o fark güncellemedir.
   - `profil.md` hiçbir zaman değiştirilmez.
4. Güncellemeleri kullanıcıya kısaca anlat. Kullanıcının değişikliğiyle çakışan yeri ayrıca göster ve ne yapılacağını sor. Onay alınca, yedek aldıktan sonra uygula ve `.avukatlab-surum`'u yeni sürümle değiştir.

`.avukatlab-surum` yoksa kurulan sürüm bilinmez. Farkları dosya dosya gösterip kullanıcıya sor.

MCP sunucuları her açılışta GitHub'daki son sürümü kendiliğinden kullanır; onlar için bir şey yapmak gerekmez.

## 10. Kaldırma

Kullanıcı kaldırmak isterse, her adımdan önce yedek al:

- **Beceriler:** Beceri klasörlerini kaldır (Mac'te Çöp Kutusu'na taşı). Claude Desktop'a yüklenenleri kullanıcı ayarlardan siler.
- **"Her zaman" paragrafı:** Kural dosyasından çıkar ya da `~/.gemini/config/rules/avukatlab.md` dosyasını kaldır.
- **MCP sunucuları:**
  - Claude Code'da `claude mcp remove <ad> -s user`.
  - Claude Desktop ve Antigravity'de JSON'daki girdileri sil.
- **İsteğe bağlı:** Masaüstündeki zip'leri ve uv önbelleğini (`uv cache clean`, birkaç yüz MB) temizle.

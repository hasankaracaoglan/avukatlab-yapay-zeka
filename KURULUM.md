# Kurulum talimatı (yapay zekâ asistanı için)

Bu dosya, kullanıcının "AvukatLab paketini kur" dediği yapay zekâ asistanı içindir. Adımları sırayla izle. Kullanıcı avukattır ve teknik ayrıntı beklemez: soruları kısa, açık ve Türkçe sor.

Bu talimat, MCP sunucularının README'lerindeki özgün projeye ait kurulum bölümlerinin (Antigravity betikleri, uzak sunucu adresleri, PyPI'den `uvx yargi-mcp`, `saidsurucu/...` depoları) **yerine geçer**. O bölümleri uygulama.

Talimat macOS'ta denendi. Windows'ta eşdeğer komutları kullan ve kullanıcıya Windows'ta denenmediğini söyle.

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
   - Antete e-posta, KEP adresi ya da baro sicil numarası eklensin mi? Hangileri?
   - Büroda sizin adınıza dilekçe yazacak başka avukat var mı? Varsa onun bilgileri.
   - Nöbetçi mahkeme başlıklarında hangi şehir yazsın?
4. **Dilekçe yazarken bu beceriler her zaman kullanılsın mı?** Asistan beceriyi işe göre kendisi seçer, ama bazen kaçırabilir. Evet denirse 8. adımdaki tek paragraf kural dosyasına eklenir.

## 2. Ön kontroller (yalnızca bak, bir şey kurma)

- **uv:** `command -v uv`. Boş dönerse şu yollara bak: `~/.local/bin/uv`, `/opt/homebrew/bin/uv`, `/usr/local/bin/uv` (Windows'ta `%USERPROFILE%\.local\bin\uv.exe`). Bulursan tam yolunu not et. Bulamazsan uv'nin kurulumu plana bir madde olarak girer (3. adım).
- **Antigravity'nin yerleri:**
  - `~/.gemini/config/` klasörü varsa yeni sürümdür: MCP ayarı `~/.gemini/config/mcp_config.json`, beceriler `~/.gemini/config/skills/`, kurallar `~/.gemini/config/rules/`.
  - Yoksa eski yerleri kullan: `~/.gemini/antigravity/mcp_config.json`, `~/.gemini/antigravity/skills/`, kural dosyası `~/.gemini/GEMINI.md`.
  - Belge: https://antigravity.google/docs/skills
  - Klasör yoksa kurulumda oluştur.
- **Claude Desktop:** ayar dosyası macOS'ta `~/Library/Application Support/Claude/claude_desktop_config.json`, Windows'ta `%APPDATA%\Claude\claude_desktop_config.json`.
- **Uyap Doküman Editörü:** PDF dönüştürme için gerekir. Mac'te `/Applications` altında adında "Uyap" ve "Doküman" geçen bir uygulama var mı, bak; sonucu plana yaz. UDF üretmek için gerekmez.
- **Var olan kurulumlar:** Her hedefte aynı adlı MCP girdisi (`yargi-mcp`, `mevzuat-mcp`, `uyap-udf-mcp`), aynı adlı beceri klasörü ve 8. adımdaki paragraf var mı, bak.
  - Claude Code için her ad için `claude mcp get <ad>` çalıştır; çıktı girdinin kapsamını da gösterir (user, project, local). `claude mcp list` kullanma, her sunucuyu çalıştırıp yavaşlar.
  - Eski Yargı MCP kullanıcılarında `uvx yargi-mcp` gibi artık çalışmayan bir girdi olabilir.

## 3. Planı göster, onay al

Kullanıcıya tek bir plan göster:

- Ne nereye kurulacak.
- Hangi ayar dosyaları değişecek ve yedekleri nereye alınacak.
- uv kurulu değilse kurulacağı.
- Var olan aynı adlı girdiler ve becerilerin değiştirilmesi için soru. Hayır denirse o parçayı atla ve sonunda bildir.
- Mevzuat MCP'nin bazı araçları (mevzuat.gov.tr sorguları) Chromium ister: yaklaşık 480 MB, 3 dakika. Asıl kullanılan Bedesten araçları istemez. "Şimdi indirelim mi?" diye sor; önerin "evet" olsun. Hayır denirse o araçlar ilk kullanımda kendisi indirir, o arama uzun sürer.
- Süre ve yer: MCP sunucularının ilk kurulumu toplam 5-10 dakika sürer (Yargı MCP yaklaşık 4 dk, Mevzuat MCP 1 dk, UYAP UDF MCP 30 sn; Chromium ayrıca 3 dk). Yaklaşık 1,5 GB yer kaplar (Chromium dahil).
- Claude Desktop ayarı değişecekse Claude Desktop'ın kapatılması. Sen Claude Desktop'ın içinde çalışıyorsan bunu isteme; bitince yeniden başlatmasını söyle.

Plan onaylanınca uygula. Kullandığın uygulama komut ve dosya değişikliği için ayrıca izin soruyorsa onu ayrıca tekrar sorma.

**Yedekler:** Bütün yedekler tek bir klasöre alınır: `~/AvukatLab-yedekler/<tarih-saat>/`, örneğin `~/AvukatLab-yedekler/20261124-1408/`. Dosya adı `<uygulama>-<asıl dosya adı>` olur, örneğin `claude-desktop-claude_desktop_config.json`. Yedekleri **asla** beceri ya da kural klasörlerinin içine koyma; oradaki kopya ikinci bir beceri ya da kural olarak yüklenir.

## 4. MCP sunucuları

Sunucular bilgisayara **kalıcı olarak** kurulur. Açılırken internete, GitHub'a ya da git'e ihtiyaç duymazlar.

1. **uv** kurulu değilse kur, sonra tam yolunu yeniden bul:
   - macOS/Linux: `curl -LsSf https://astral.sh/uv/install.sh | sh`
   - Windows: `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`
2. **Sunucuları kur.** Her biri için (`<uv>` = uv'nin tam yolu):

   ```bash
   UV_HTTP_TIMEOUT=180 <uv> tool install https://github.com/hasankaracaoglan/yargi-mcp/archive/refs/heads/yerel.zip
   UV_HTTP_TIMEOUT=180 <uv> tool install https://github.com/hasankaracaoglan/mevzuat-mcp/archive/refs/heads/yerel.zip
   UV_HTTP_TIMEOUT=180 <uv> tool install https://github.com/hasankaracaoglan/uyap-udf-mcp/archive/refs/heads/main.zip
   ```

   - Yargı MCP birkaç dakika sürer: komutun zaman aşımı en az 10 dakika olsun ya da arka planda çalıştır.
   - "operation timed out" ile yarıda kalırsa aynı komutu bir kez daha çalıştır; inen paketler önbellekte kalır.
3. **Çalıştırılabilir dosyaların yeri:** `<uv> tool dir --bin` (çoğunlukla `~/.local/bin`). Her sunucunun komutu bu klasördeki aynı adlı dosyadır, örneğin `~/.local/bin/yargi-mcp`. Ayarlarda **daima bu tam yolu** yaz; `~` yazma, açık yolu yaz. Masaüstü uygulamaları `~/.local/bin`'i PATH'te görmez.
4. **Chromium** (planda evet dendiyse):

   ```bash
   "$(<uv> tool dir)/mevzuat-mcp/bin/python" -m playwright install chromium
   ```

   Windows'ta Python yolu `...\mevzuat-mcp\Scripts\python.exe` olur.
5. **Asistanlara ekle:**
   - **Claude Code:**

     ```
     claude mcp add yargi-mcp -s user -- <bin>/yargi-mcp
     claude mcp add mevzuat-mcp -s user -- <bin>/mevzuat-mcp
     claude mcp add uyap-udf-mcp -s user -- <bin>/uyap-udf-mcp
     ```

     Aynı adlı eski girdi varsa ve kullanıcı değiştirmeyi onayladıysa:
     - Önce `claude mcp get <ad>` çıktısını yedek klasörüne `claude-code-<ad>.txt` olarak yaz.
     - Sonra girdiyi bulunduğu kapsamdan kaldır: `claude mcp remove <ad> -s <user|project|local>`.
     - Başka bir kapsamdaki aynı adlı girdi, yeni kullanıcı girdisinin önüne geçer.
   - **Claude Desktop ve Antigravity:**
     - JSON ayar dosyasının yedeğini al. Dosya yoksa `{"mcpServers": {}}` ile oluştur.
     - `mcpServers` nesnesine şu girdileri ekle:

       ```json
       "yargi-mcp": {"command": "<bin>/yargi-mcp", "args": []},
       "mevzuat-mcp": {"command": "<bin>/mevzuat-mcp", "args": []},
       "uyap-udf-mcp": {"command": "<bin>/uyap-udf-mcp", "args": []}
       ```

     - Var olan diğer sunuculara ve dosyadaki başka anahtarlara dokunma. Aynı adlı eski girdiyi, onay alındıysa yerinde değiştir.
     - Sonra dosyanın geçerli JSON olduğunu doğrula.
6. **Sunucuları terminalde çalıştırma;** girdi bekleyip açık kalırlar. Kurulduklarını `<uv> tool list` ile kontrol et.

## 5. Depoyu indir ve sürümü kaydet

Önce sürüm kimliğini al, sonra tam o sürümü indir:

```bash
curl -fsS https://api.github.com/repos/hasankaracaoglan/avukatlab-yapay-zeka/commits/main | grep -m1 '"sha"'
D=$(mktemp -d) && echo "$D"
curl -fL https://github.com/hasankaracaoglan/avukatlab-yapay-zeka/archive/<sha>.tar.gz -o "<D>/paket.tar.gz"
tar -xzf "<D>/paket.tar.gz" -C "<D>"
```

- Beceriler `<D>/avukatlab-yapay-zeka-<sha>/skills/<beceri-adı>/` altındadır.
- Kabuk değişkenleri bir komuttan ötekine taşınmayabilir. `echo` ile çıkan yolu ve sha'yı sonraki her komutta açıkça yaz.
- sha'yı not et; 7. adımda kullanılır.

## 6. Profili doldur

İndirilen kopyadaki `skills/dilekce-yazimi/profil.md`'yi 1. adımdaki cevaplarla doldur:

- Antet 1. satırı Türkçe büyük harfle yazılır: i → İ, ı → I (`Işıl Çiçek` → `IŞIL ÇİÇEK`). Programlama dillerinin büyük harf işlevlerine güvenme.
- Soyad dilekçenin her yerinde büyük harfle yazılır: `Av. Işıl ÇİÇEK`.
- E-posta, KEP ve baro sicil numarası ayrı satırlardır. Kullanıcının istemediği satırı sil, istediğini örnekteki önekle yaz.
- `[doldurun]` yalnızca kullanıcının vermediği zorunlu bilgide kalır; dilekçe yazılırken beceri onu sorar.
- Bu bilgileri profil dosyası dışında hiçbir yere yazma ya da gönderme.

## 7. Becerileri kopyala

Kopyalanacak olan, `skills/<beceri-adı>/` klasörünün tamamıdır.

| Asistan | Hedef |
|---|---|
| Claude Code | `~/.claude/skills/<beceri-adı>/` |
| Antigravity | 2. adımda bulunan beceri klasörü: `<...>/skills/<beceri-adı>/` |
| Claude Desktop | Klasör kopyalanmaz, zip yapılır (aşağıda). |

- Hedefte aynı adlı klasör varsa üzerine yazma. Kullanıcı onu kendine göre değiştirmiş olabilir; 10. adımdaki güncelleme yolunu izle.
- Her kopyanın içine `.avukatlab-surum` adlı bir dosya yaz, içinde 5. adımdaki sha olsun. Güncellemede kullanıcının değişikliklerini ayırmak için gerekir.
- Kopyaladıktan sonra her hedefte `SKILL.md` bulunduğunu kontrol et.

**Claude Desktop için zip:**

- Zip'i önce geçici klasörde üret, sonra masaüstüne taşı:

  ```bash
  cd "<D>/avukatlab-yapay-zeka-<sha>/skills" && zip -r -X "<D>/<beceri-adı>.zip" <beceri-adı> -x '*.DS_Store'
  ```

- Masaüstünde aynı adlı bir zip varsa önce Çöp Kutusu'na taşı (9. adım). `zip` var olan arşive ekleme yapar ve eski dosyalar içinde kalır.
- `unzip -l` ile zip'in en üstünde `<beceri-adı>/SKILL.md` olduğunu kontrol et.
- `.avukatlab-surum` zip'e konmaz.

## 8. "Her zaman kullan" paragrafı (1. adımda evet dendiyse)

Eklenecek paragraf:

```
Dilekçe, itiraz, istinaf, temyiz veya savunma yazarken `dilekce-yazimi` becerisini; dilekçe UDF'e ya da PDF'e çevrilirken `uyap-udf-sablonu` becerisini; emsal karar aranırken `ictihat-arastirmasi` becerisini kullan.
```

Paragraf hedefte zaten varsa yeniden ekleme. Dosyayı değiştirmeden önce yedeğini al; dosyadaki başka hiçbir şeye dokunma.

- **Claude Code:** `~/.claude/CLAUDE.md` dosyasının sonuna ekle; dosya yoksa oluştur.
- **Antigravity (yeni sürüm):** Yeni bir dosya oluştur: `~/.gemini/config/rules/avukatlab.md`. Dosya **mutlaka** aşağıdaki baş bilgiyle başlar; başlamazsa Antigravity kuralı uyarı vermeden yok sayar:

  ```
  ---
  trigger: always_on
  description: AvukatLab becerilerini dilekçe işlerinde kullan
  ---

  Dilekçe, itiraz, istinaf, temyiz veya savunma yazarken `dilekce-yazimi` becerisini; dilekçe UDF'e ya da PDF'e çevrilirken `uyap-udf-sablonu` becerisini; emsal karar aranırken `ictihat-arastirmasi` becerisini kullan.
  ```

- **Antigravity (eski sürüm):** `~/.gemini/GEMINI.md` dosyasının sonuna ekle; baş bilgi gerekmez.
- **Claude Desktop:** Dosya yoktur. Paragrafı kullanıcıya ver. Claude Desktop ayarlarındaki kişisel tercihler ya da talimatlar alanına, veya kullandığı projenin talimatlarına kendisi yapıştırır.

## 9. Bitir

1. **Geçici klasörü sil:** `rm -rf "<D>"`. İçinde doldurulmuş profil var.
2. **Çöp Kutusu'na taşıma** (eski zip, kaldırma vb.): Mac'te `/usr/bin/trash <yol>` varsa onu kullan. Yoksa `mv <yol> ~/.Trash/`.
3. **Son mesajda** kısa bir liste hâlinde şunları söyle:
   - Neyin nereye kurulduğu ve yedeklerin yeri (`~/AvukatLab-yedekler/...`).
   - Asistan uygulamalarının tamamen kapatılıp yeniden açılması gerektiği.
   - Nasıl deneneceği:
     - Herhangi bir uygulamada: "Bir kira alacağı davası için kısa bir dava dilekçesi taslağı yaz".
     - Claude Code ve Antigravity'de beceri `/dilekce-yazimi` ile doğrudan da çağrılabilir.
   - Claude Desktop kurulduysa:
     - Masaüstündeki zip'lerin ayarlardaki beceriler (Skills) bölümünden nasıl yükleneceği. Menü adları sürüme göre değişebilir. Becerilerin çalışması için kod çalıştırma özelliğinin açık olması gerekebilir.
     - `dilekce-yazimi.zip` antet bilgilerini içerir ve Claude'un hizmetine yüklenir. Masaüstü bir bulut hizmetine eşitleniyorsa zip oraya da gider; yükledikten sonra zip'ler silinmeli.
     - Profil değişirse zip'in yeniden yapılıp yüklenmesi gerekir.
     - 8. adımdaki paragrafı nereye yapıştıracağı.
   - Uyap Doküman Editörü bulunamadıysa: PDF dönüştürme editör kurulana kadar çalışmaz, UDF üretimi çalışır.
   - Kuralların kullanıcının kendi kopyası olduğu, istediği maddeyi `SKILL.md` dosyalarında değiştirebileceği.
   - Güncelleme için "AvukatLab'i güncelle" demesinin yeterli olduğu.
   - Atlanan bir parça varsa neden atlandığı.

## 10. Güncelleme

Kullanıcı "AvukatLab'i güncelle" derse hem sunucuları hem becerileri yenile.

**MCP sunucuları:** Kurulu her sunucu için:

```bash
UV_HTTP_TIMEOUT=180 <uv> tool install --reinstall --refresh <4. adımdaki adres>
```

Komut, sunucuyu son sürümle yeniden kurar. Yargı MCP'nin karar önbelleği bu sırada temizlenir; kararlar gerektikçe yeniden iner. Bitince asistan uygulamalarının yeniden başlatılması gerekir.

**Beceriler:**

1. Kurulu her becerinin `.avukatlab-surum` dosyasından kurulduğu sürümü oku.
2. O sürümü (`archive/<eski-sha>.tar.gz`) ve son sürümü (5. adım) ayrı geçici klasörlere indir.
3. Her dosya için üç hâli karşılaştır: kurulan sürüm, kullanıcının şimdiki kopyası, son sürüm.
   - Kullanıcının kopyası kurulan sürümden farklıysa bu fark kullanıcının değişikliğidir, korunur.
   - Son sürüm kurulan sürümden farklıysa bu fark güncellemedir.
   - `profil.md` hiçbir zaman değiştirilmez.
4. Güncellemeleri kullanıcıya kısaca anlat. Kullanıcının değişikliğiyle çakışan yeri ayrıca göster ve ne yapılacağını sor.
5. Onay alınca, yedek aldıktan sonra uygula ve `.avukatlab-surum`'u yeni sha ile değiştir.
6. `.avukatlab-surum` yoksa kurulan sürüm bilinmez. Farkları dosya dosya gösterip sor.
7. Claude Desktop'taki beceriler, güncel klasörden yeniden zip yapılıp kullanıcı tarafından yeniden yüklenir.

## 11. Kaldırma

Her adımdan önce yedek al.

- **Beceriler:** Beceri klasörlerini Çöp Kutusu'na taşı. Claude Desktop'a yüklenenleri kullanıcı ayarlardan siler.
- **"Her zaman" paragrafı:** Kural dosyasından çıkar ya da `~/.gemini/config/rules/avukatlab.md`'yi Çöp Kutusu'na taşı.
- **MCP sunucularının ayarları:**
  - Claude Code'da `claude mcp remove <ad> -s user`.
  - Claude Desktop ve Antigravity'de JSON'daki girdileri sil.
- **MCP sunucularının kendileri:** `<uv> tool uninstall yargi-mcp mevzuat-mcp uyap-udf-mcp`.
- **İsteğe bağlı temizlik:** uv önbelleği (`<uv> cache clean`) ve Chromium (`~/Library/Caches/ms-playwright`). Chromium'u yalnızca başka bir program kullanmıyorsa sil.

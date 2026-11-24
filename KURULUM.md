# Kurulum talimatı (yapay zekâ asistanı için)

Bu dosya, kullanıcının "AvukatLab paketini kur" dediği yapay zekâ asistanı içindir. Adımları sırayla izle. Kullanıcı avukattır ve teknik ayrıntı beklemez: soruları kısa, açık ve Türkçe sor.

Bu talimat, MCP sunucularının README'lerindeki özgün projeye ait kurulum bölümlerinin (Antigravity betikleri, uzak sunucu adresleri, PyPI'den `uvx yargi-mcp`, `saidsurucu/...` depoları) **yerine geçer**. O bölümleri uygulama.

Paket **Claude Code**, **Codex** ve **Antigravity** içindir. Talimat macOS'ta denendi; Windows'ta eşdeğer komutları kullan ve kullanıcıya Windows'ta denenmediğini söyle.

Kullanıcı Claude Desktop'ın sohbet bölümüne ya da başka bir sohbet uygulamasına kurmak isterse, bu paketin onu desteklemediğini söyle ve Claude Code'u öner. Claude Desktop'ın ayar dosyasına dokunma.

## 1. Soruları tek seferde sor

Hepsini tek mesajda sor:

1. **Neleri kuralım?** Varsayılan: hepsi.
   - MCP sunucuları: Yargı MCP (karar arama), Mevzuat MCP (mevzuat), UYAP UDF MCP (UDF dilekçe ve PDF).
   - Beceriler: `dilekce-yazimi`, `uyap-udf-sablonu`, `ictihat-arastirmasi`.
   - Not olarak söyle: `ictihat-arastirmasi` Yargı MCP'siz, `uyap-udf-sablonu` UYAP UDF MCP'siz işe yaramaz.
2. **Hangi asistanda kullanacaksınız?** Claude Code, Codex, Antigravity ya da birkaçı. Sen hangi uygulamada çalışıyorsan onu varsayılan öner.
3. **Antet ve imza bilgileri** (`dilekce-yazimi` kurulacaksa):
   - Adınız ve soyadınız (dilekçede "Av. Ad SOYAD" biçiminde yazılır)?
   - Büro adresiniz?
   - Antette hangi telefon yazsın?
   - Antete e-posta, KEP adresi ya da baro sicil numarası eklensin mi? Eklenecekse adresini ya da numarasını da yazın.
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
- **Codex:** Kullanıcının Codex klasörü `~/.codex/` (ortam değişkeni `CODEX_HOME` verilmişse o klasör). MCP ayarı `~/.codex/config.toml`, "her zaman" notu `~/.codex/AGENTS.md` (aynı yerde `AGENTS.override.md` varsa Codex yalnızca onu okur; o durumda nota oraya ekle). Beceriler Codex klasörünün dışında, `~/.agents/skills/` altındadır. Belge: https://learn.chatgpt.com/docs/build-skills
- **Uyap Doküman Editörü:** PDF dönüştürme için gerekir, UDF üretmek için gerekmez. `ls /Applications ~/Applications 2>/dev/null | grep -i uyap` ile bak ve sonucu plana yaz. Aramada Türkçe harf kullanma ("Doküman" gibi): macOS dosya adlarındaki Türkçe harfleri ayrıştırılmış biçimde saklar, Türkçe harfli arama uygulama kurulu olsa da boş döner.
- **Var olan kurulumlar:** Her hedefte aynı adlı MCP girdisi (`yargi-mcp`, `mevzuat-mcp`, `uyap-udf-mcp`), aynı adlı beceri klasörü ve 8. adımdaki paragraf var mı, bak.
  - Claude Code için her ad için `claude mcp get <ad>` çalıştır; çıktı girdinin kapsamını da gösterir (user, project, local). `claude mcp list` kullanma, her sunucuyu çalıştırıp yavaşlar.
  - Eski Yargı MCP kullanıcılarında `uvx yargi-mcp` gibi artık çalışmayan bir girdi olabilir.
  - `<uv> tool list` ile aynı adlı kurulu araç, `<uv> tool dir --bin` klasöründe aynı adlı dosya var mı bak. Varsa planda göster; onay alınırsa kurulumda `--force` ekle.
  - Codex için `codex mcp get <ad>` ile aynı adlı girdi var mı bak.
  - `claude` komutu yoksa (ör. kurulumu Antigravity'den yapıyorsan) Claude Code adımları için komutları kullanıcıya ver; Claude Code'u açtığında kendisi çalıştırsın. `~/.claude.json`'ı elle düzenleme.

## 3. Planı göster, onay al

Kullanıcıya tek bir plan göster:

- Ne nereye kurulacak.
- Hangi ayar dosyaları değişecek ve yedekleri nereye alınacak.
- uv kurulu değilse kurulacağı.
- Var olan aynı adlı girdiler ve becerilerin değiştirilmesi için soru. Hayır denirse o parçayı atla ve sonunda bildir.
- Mevzuat MCP'nin bazı araçları (mevzuat.gov.tr sorguları) Chromium ister: yaklaşık 480 MB, 3 dakika. Asıl kullanılan Bedesten araçları istemez. "Şimdi indirelim mi?" diye sor; önerin "evet" olsun. Hayır denirse o araçlar ilk kullanımda kendisi indirir, o arama uzun sürer.
- Süre ve yer: MCP sunucularının ilk kurulumu internet hızına göre 3-10 dakika sürer (en uzunu Yargı MCP; Chromium ayrıca 3 dk). Sunucular, uv'nin önbelleği ve Chromium birlikte yaklaşık 1,8 GB yer kaplar.

Plan onaylanınca uygula. Uygulamanın kendi izin pencereleri dışında kullanıcıya ayrıca onay sorma.

**Yedekler:** Bütün yedekler tek bir klasöre alınır: `~/AvukatLab-yedekler/<tarih-saat>/`, örneğin `~/AvukatLab-yedekler/20261124-1408/`. Dosya adı `<uygulama>-<asıl dosya adı>` olur; önekler `claude-code-`, `codex-` ve `antigravity-`. Örnek: `antigravity-mcp_config.json`. Yedekleri **asla** beceri ya da kural klasörlerinin içine koyma; oradaki kopya ikinci bir beceri ya da kural olarak yüklenir.

## 4. MCP sunucuları

Sunucular bilgisayara **kalıcı olarak** kurulur. Açılırken internete, GitHub'a ya da git'e ihtiyaç duymazlar.

1. **uv** kurulu değilse kur, sonra tam yolunu yeniden bul:
   - macOS/Linux: `curl -LsSf https://astral.sh/uv/install.sh | sh`
   - Windows: `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`
2. **Sunucuları kur.** Her biri için (`<uv>` = uv'nin tam yolu):

   ```bash
   UV_HTTP_TIMEOUT=180 <uv> tool install --managed-python --python 3.12 https://github.com/hasankaracaoglan/yargi-mcp/archive/refs/heads/yerel.zip
   UV_HTTP_TIMEOUT=180 <uv> tool install --managed-python --python 3.12 https://github.com/hasankaracaoglan/mevzuat-mcp/archive/refs/heads/yerel.zip
   UV_HTTP_TIMEOUT=180 <uv> tool install --managed-python --python 3.12 https://github.com/hasankaracaoglan/uyap-udf-mcp/archive/refs/heads/main.zip
   ```

   - Yargı MCP birkaç dakika sürer: komutun zaman aşımı en az 10 dakika olsun ya da arka planda çalıştır.
   - "operation timed out" ile yarıda kalırsa aynı komutu bir kez daha çalıştır; inen paketler önbellekte kalır.
   - `--managed-python --python 3.12`, sunucuların uv'nin kendi Python'unu kullanmasını sağlar. Böylece Homebrew gibi başka bir Python'un güncellenmesi ya da kaldırılması sunucuları bozmaz.
   - uv "... is not on your PATH" diye uyarır. Bu uyarıyı yok say; `uv tool update-shell` çalıştırma ve kabuk dosyalarına dokunma. Ayarlarda tam yol kullanıldığı için gerekmez.
3. **Çalıştırılabilir dosyaların yeri:** `<uv> tool dir --bin` (çoğunlukla `~/.local/bin`). Her sunucunun komutu bu klasördeki aynı adlı dosyadır, örneğin `~/.local/bin/yargi-mcp`. Ayarlarda **daima bu tam yolu** yaz; `~` yazma, açık yolu yaz. Uygulamalar `~/.local/bin`'i PATH'te görmeyebilir.
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
   - **Codex:** `~/.codex/config.toml` dosyasının yedeğini al (yoksa oluştur) ve sonuna her sunucu için şu bölümü ekle. Codex sunucunun açılması için varsayılan olarak 10 saniye, bir araç için 60 saniye bekler; Yargı MCP'nin ilk açılışı ve 100 kararlık arama bu sınırlara yakın olduğu için süreler uzatılır:

     ```toml
     [mcp_servers.yargi-mcp]
     command = "<bin>/yargi-mcp"
     startup_timeout_sec = 60
     tool_timeout_sec = 300
     ```

     `mevzuat-mcp` ve `uyap-udf-mcp` için de aynısı. Dosyadaki başka bölümlere dokunma. Aynı adlı eski bölüm varsa onay alınca yerinde değiştir. Sonra `codex mcp list` ile üç sunucunun `enabled` göründüğünü kontrol et.
   - **Antigravity:**
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
- `[doldurun: ...]` yer tutucusu yalnızca kullanıcının vermediği zorunlu bilgide kalır; dilekçe yazılırken beceri onu sorar.
- Bu bilgileri profil dosyası dışında hiçbir yere yazma ya da gönderme.

## 7. Becerileri kopyala

Kopyalanacak olan, `skills/<beceri-adı>/` klasörünün tamamıdır.

| Asistan | Hedef |
|---|---|
| Claude Code | `~/.claude/skills/<beceri-adı>/` |
| Codex | `~/.agents/skills/<beceri-adı>/` |
| Antigravity | 2. adımda bulunan beceri klasörü: `<...>/skills/<beceri-adı>/` |

- Hedefte aynı adlı klasör varsa üzerine yazma. Kullanıcı onu kendine göre değiştirmiş olabilir; 10. adımdaki güncelleme yolunu izle.
- Her kopyanın içine `.avukatlab-surum` adlı bir dosya yaz, içinde 5. adımdaki sha olsun. Güncellemede kullanıcının değişikliklerini ayırmak için gerekir.
- Kopyaladıktan sonra her hedefte `SKILL.md` bulunduğunu kontrol et.

## 8. "Her zaman kullan" paragrafı (1. adımda evet dendiyse)

Eklenecek paragraf:

```
Dilekçe, itiraz, istinaf, temyiz veya savunma yazarken `dilekce-yazimi` becerisini; dilekçe UDF'e ya da PDF'e çevrilirken `uyap-udf-sablonu` becerisini; emsal karar aranırken `ictihat-arastirmasi` becerisini kullan.
```

Paragraf hedefte zaten varsa yeniden ekleme. Dosyayı değiştirmeden önce yedeğini al; dosyadaki başka hiçbir şeye dokunma.

- **Claude Code:** `~/.claude/CLAUDE.md` dosyasının sonuna ekle; dosya yoksa oluştur.
- **Codex:** `~/.codex/AGENTS.md` dosyasının sonuna ekle; dosya yoksa oluştur (aynı yerde `AGENTS.override.md` varsa ona ekle).
- **Antigravity (yeni sürüm):** Yeni bir dosya oluştur: `~/.gemini/config/rules/avukatlab.md`. Dosya **mutlaka** aşağıdaki baş bilgiyle başlar; başlamazsa Antigravity kuralı uyarı vermeden yok sayar:

  ```
  ---
  trigger: always_on
  description: AvukatLab becerilerini dilekçe işlerinde kullan
  ---

  Dilekçe, itiraz, istinaf, temyiz veya savunma yazarken `dilekce-yazimi` becerisini; dilekçe UDF'e ya da PDF'e çevrilirken `uyap-udf-sablonu` becerisini; emsal karar aranırken `ictihat-arastirmasi` becerisini kullan.
  ```

- **Antigravity (eski sürüm):** `~/.gemini/GEMINI.md` dosyasının sonuna ekle; baş bilgi gerekmez.

## 9. Bitir

1. **Geçici klasörü sil:** `rm -rf "<D>"`. İçinde doldurulmuş profil var.
2. **Çöp Kutusu'na taşıma** (kaldırma vb.): Mac'te `/usr/bin/trash <yol>` varsa onu kullan. Yoksa `mv -n <yol> ~/.Trash/`; Çöp Kutusu'nda aynı adlı dosya varsa ada tarih-saat ekle, var olanın üzerine yazma.
3. **Son mesajda** kısa bir liste hâlinde şunları söyle:
   - Neyin nereye kurulduğu ve yedeklerin yeri (`~/AvukatLab-yedekler/...`).
   - Asistan uygulamalarının tamamen kapatılıp yeniden açılması gerektiği.
   - Nasıl deneneceği:
     - "Bir kira alacağı davası için kısa bir dava dilekçesi taslağı yaz" demek.
     - Beceriyi doğrudan çağırmak için Claude Code ve Antigravity'de `/dilekce-yazimi`, Codex'te `$dilekce-yazimi` yazmak.
   - Uyap Doküman Editörü bulunamadıysa: PDF dönüştürme editör kurulana kadar çalışmaz, UDF üretimi çalışır.
   - Kuralların kullanıcının kendi kopyası olduğu, istediği maddeyi `SKILL.md` dosyalarında değiştirebileceği.
   - Güncelleme için "AvukatLab'i güncelle" demesinin yeterli olduğu.
   - Atlanan bir parça varsa neden atlandığı.

## 10. Güncelleme

Kullanıcı "AvukatLab'i güncelle" derse hem sunucuları hem becerileri yenile.

**MCP sunucuları:** Kurulu her sunucu için:

```bash
UV_HTTP_TIMEOUT=180 <uv> tool install --reinstall --refresh --managed-python --python 3.12 <4. adımdaki adres>
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

## 11. Kaldırma

Her adımdan önce yedek al.

- **Beceriler:** Beceri klasörlerini Çöp Kutusu'na taşı.
- **"Her zaman" paragrafı:** Kural dosyasından çıkar ya da `~/.gemini/config/rules/avukatlab.md`'yi Çöp Kutusu'na taşı.
- **MCP sunucularının ayarları:**
  - Claude Code'da `claude mcp remove <ad> -s user`.
  - Codex'te `config.toml`'daki `[mcp_servers.<ad>]` bölümlerini sil (ya da `codex mcp remove <ad>`).
  - Antigravity'de JSON'daki girdileri sil.
- **MCP sunucularının kendileri:** `<uv> tool uninstall yargi-mcp mevzuat-mcp uyap-udf-mcp`.
- **İsteğe bağlı temizlik:** uv önbelleği (`<uv> cache clean`) ve Chromium (`~/Library/Caches/ms-playwright`). Chromium'u yalnızca başka bir program kullanmıyorsa sil.

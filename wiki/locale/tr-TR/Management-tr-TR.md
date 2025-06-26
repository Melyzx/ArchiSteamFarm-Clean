# Yönetim

Bu bölüm, ASF sürecini en iyi şekilde yönetme ile ilgili konuları kapsar. Kullanımı zorunlu olmasa da, özellikle sistem yöneticileri, ASF'yi üçüncü taraf depolarında kullanılmak üzere paketleyen kişiler ve ileri düzey kullanıcılar için paylaşmak istediğimiz ipuçları, püf noktaları ve iyi uygulamaları içerir.

---

## Linux için `systemd` servisi

`generic` ve `linux` varyantlarında, ASF, **[`systemd`](https://systemd.io)** için bir yapılandırma dosyası olan `ArchiSteamFarm@.service` dosyası ile birlikte gelir. ASF'yi bir hizmet olarak çalıştırmak, örneğin makineniz başlatıldığında otomatik olarak başlatmak istiyorsanız, uygun bir `systemd` servisi bu işi yapmanın en iyi yolu olarak kabul edilir; bu nedenle, `nohup`, `screen` veya benzeri yöntemlerle yönetmek yerine bunu yüksek derecede öneriyoruz.

Öncelikle, ASF'yi çalıştırmak istediğiniz kullanıcı için bir hesap oluşturun; eğer henüz mevcut değilse. Bu örnek için `asf` kullanıcısını kullanacağız; farklı bir kullanıcı tercih ettiyseniz, aşağıdaki örneklerde `asf` kullanıcısını seçtiğiniz kullanıcı ile değiştirmelisiniz. Hizmetimiz, ASF'yi `root` olarak çalıştırmanıza izin vermez, çünkü bu bir **[kötü uygulama](#never-run-asf-as-administrator)** olarak kabul edilir.

```sh
su # Veya root shell'ine geçmek için sudo -i
useradd -m asf # ASF'yi çalıştırmak istediğiniz kullanıcı hesabını oluşturun
```

Sonraki adımda, ASF'yi `/home/asf/ArchiSteamFarm` klasörüne çıkarın. Klasör yapısı, hizmet birimimiz için önemlidir; `$HOME`'unuzda `ArchiSteamFarm` klasörü olmalıdır, yani `/home/<user>`. Her şeyi doğru yaptıysanız, `/home/asf/ArchiSteamFarm/ArchiSteamFarm@.service` dosyasının mevcut olması gerekir. `linux` varyantını kullanıyorsanız ve dosyayı Linux'ta çıkarmadıysanız, örneğin Windows'tan dosya transferi yaptıysanız, ayrıca `chmod +x /home/asf/ArchiSteamFarm/ArchiSteamFarm` komutunu da çalıştırmanız gerekebilir.

Aşağıdaki tüm işlemleri `root` olarak yapacağız, bu yüzden `su` veya `sudo -i` ile root shell'ine geçin.

Öncelikle, klasörümüzün hala `asf` kullanıcısına ait olduğundan emin olmak iyi bir fikirdir; bir kez `chown -hR asf:asf /home/asf/ArchiSteamFarm` komutunu çalıştırmak yeterlidir. İzinler yanlış olabilir, örneğin zip dosyasını `root` olarak indirip veya çıkarırsanız.

İkinci olarak, ASF'nin `generic` varyantını kullanıyorsanız, `dotnet` komutunun tanındığından ve standart konumlardan birinde olduğundan emin olmalısınız: `/usr/local/bin`, `/usr/bin` veya `/bin`. Bu, `dotnet /path/to/ArchiSteamFarm.dll` komutunu çalıştıran `systemd` servisi için gereklidir. `dotnet --info` komutunun çalışıp çalışmadığını kontrol edin; çalışıyorsa, `command -v dotnet` komutunu girerek nerede olduğunu öğrenebilirsiniz. Resmi yükleyiciyi kullandıysanız, `/usr/bin/dotnet` veya diğer iki konumdan birinde olmalıdır, bu sorun değil. OS-spesifik bir varyant kullanıyorsanız, bu konuda bir şey yapmanıza gerek yoktur. Eğer özel bir konumda, örneğin `/usr/share/dotnet/dotnet` içinde ise, bunu `ln -s "$(command -v dotnet)" /usr/bin/dotnet` komutuyla bir **[symlink](https://wikipedia.org/wiki/Symbolic_link)** ile bağlayın. Şimdi `command -v dotnet` komutu `/usr/bin/dotnet` rapor etmelidir, bu da `systemd` biriminizin çalışmasını sağlar.

Sonraki adımda, `ln -s /home/asf/ArchiSteamFarm/ArchiSteamFarm\@.service /etc/systemd/system/ArchiSteamFarm\@.service` komutunu çalıştırın; bu, hizmet bildirimize sembolik bir bağlantı oluşturacak ve `systemd`'ye kaydedecektir. Sembolik bağlantı, ASF'nin `systemd` biriminizi ASF güncellemeleri sırasında otomatik olarak güncellemesine olanak sağlar - duruma bağlı olarak, bu yaklaşımı kullanmak isteyebilirsiniz ya da sadece dosyayı `cp` ederek kendi başınıza yönetebilirsiniz.

Sonrasında, `systemd`'nin hizmetimizi tanıyıp tanımadığını kontrol edin:

```
systemctl status ArchiSteamFarm@asf

○ ArchiSteamFarm@asf.service - ArchiSteamFarm Hizmeti (asf üzerinde)
     Loaded: loaded (/etc/systemd/system/ArchiSteamFarm@.service; disabled; vendor preset: enabled)
     Active: inactive (dead)
       Docs: https://github.com/JustArchiNET/ArchiSteamFarm/wiki
```

işaretinden sonra belirttiğimiz kullanıcıya özel dikkat edin; bizim örneğimizde `asf`. `systemd` hizmet birimimiz sizden kullanıcıyı belirtmenizi bekler, çünkü bu, ikili dosyanın `/home/<user>/ArchiSteamFarm` gibi tam konumunu ve `systemd`'nin süreci başlatacağı kullanıcıyı etkiler.

Eğer `systemd` yukarıdaki gibi bir çıktı verdiyse, her şey yolunda demektir ve neredeyse işimizi bitirdik. Şimdi tek yapmanız gereken, seçtiğiniz kullanıcı olarak hizmetimizi başlatmak: `systemctl start ArchiSteamFarm@asf`. Birkaç saniye bekleyin ve durumu tekrar kontrol edin:

```
systemctl status ArchiSteamFarm@asf

● ArchiSteamFarm@asf.service - ArchiSteamFarm Hizmeti (asf üzerinde)
     Loaded: loaded (/etc/systemd/system/ArchiSteamFarm@.service; disabled; vendor preset: enabled)
     Active: active (running) since (...)
       Docs: https://github.com/JustArchiNET/ArchiSteamFarm/wiki
   Main PID: (...)
(...)
```

Eğer `systemd` `active (running)` diyorsa, her şey yolunda demektir ve ASF sürecinin çalışıyor olması gerekir, örneğin `journalctl -r` komutuyla doğrulayabilirsiniz; çünkü ASF varsayılan olarak konsol çıktısını `systemd`'ye yazar. Şu anda kurulumunuzdan memnun kaldıysanız, `systemd`'ye hizmetinizi otomatik olarak başlatması için boot sırasında `systemctl enable ArchiSteamFarm@asf` komutunu çalıştırabilirsiniz. Hepsi bu.

Herhangi bir nedenle süreci durdurmak isterseniz, `systemctl stop ArchiSteamFarm@asf` komutunu çalıştırabilirsiniz. Benzer şekilde, ASF'nin otomatik olarak başlatılmasını devre dışı bırakmak isterseniz, `systemctl disable ArchiSteamFarm@asf` komutunu kullanabilirsiniz, bu oldukça basittir.

Lütfen, `systemd` hizmetimiz için standart girişin etkinleştirilmediğini unutmayın; bu yüzden genellikle konsol üzerinden ayrıntılarınızı giremeyeceksiniz. `systemd` üzerinden çalıştırmak, **[`Headless: true`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#headless)** ayarını belirlemekle eşdeğerdir ve tüm sonuçları ile birlikte gelir. Neyse ki, giriş sırasında ek bilgiler sağlamak veya ASF sürecinizi daha fazla yönetmek için **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)** kullanarak ASF'nizi yönetmek oldukça kolaydır.

### Ortam değişkenleri

Örneğin özel bir `--cryptkey` **[komut satırı argümanı](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments#arguments)** kullanmak istiyorsanız, `ASF_CRYPTKEY` ortam değişkenini belirterek `systemd` hizmetimize ek ortam değişkenleri sağlayabilirsiniz.

Özel ortam değişkenleri sağlamak için `/etc/asf` klasörünü oluşturun (varsa yoksa), `mkdir -p /etc/asf`. Daha sonra, `/etc/asf/<user>` dosyasına yazın, burada `<user>` hizmeti çalıştırdığınız kullanıcıdır (`yukarıdaki örnekte`asf`, yani`/etc/asf/asf`). Dosyaların yalnızca `root` kullanıcısı tarafından okunabilmesini sağlamak için `chown -hR root:root /etc/asf && chmod 700 /etc/asf` komutunu çalıştırmanızı öneririz, çünkü bu dosyalar `ASF_CRYPTKEY` gibi hassas özellikler içerebilir.

değişkenlerini içermelidir. Özel bir ortam değişkeni bulunmayanları `ASF_ARGS` içinde belirtebilirsiniz:

```sh
# Gerçekten ihtiyaç duyduğunuzları sadece belirtin
ASF_ARGS="--no-config-migrate --no-config-watch"
ASF_CRYPTKEY="my_super_important_secret_cryptkey"
ASF_NETWORK_GROUP="my_network_group"

# Ve ilgilendiğiniz diğer tüm değişkenler
```

### Hizmet biriminin bir kısmını geçersiz kılmak

`systemd`'nin esnekliği sayesinde, ASF biriminin bir kısmını orijinal birim dosyasını koruyarak ve ASF'nin örneğin otomatik güncellemeler gibi bir kısmını güncellemesine izin vererek geçersiz kılmak mümkündür.

Bu örnekte, varsayılan ASF `systemd` birim davranışını yalnızca başarı durumunda yeniden başlatmadan, aynı zamanda kritik çökme durumlarında da yeniden başlatmak istiyoruz. Bunu yapmak için, `[Service]` altında `Restart` özelliğini varsayılan `on-success`'tan `always`'e geçersiz kılacağız. Basitçe `systemctl edit ArchiSteamFarm@asf` komutunu çalıştırın, elbette `asf`'yi hizmetinizin hedef kullanıcısı ile değiştirin. Sonra `systemd`'nin uygun bölümde belirttiği değişikliklerinizi ekleyin:

```sh
### /etc/systemd/system/ArchiSteamFarm@asf.service.d/override.conf dosyasını düzenleme
### Burada ve alttaki yorum arasındaki her şey dosyanın yeni içeriği olacaktır

[Service]
Restart=always

### Bu yorumun altındaki satırlar atılacaktır

### /etc/systemd/system/ArchiSteamFarm@asf.service
# [Install]
# WantedBy=multi-user.target
#
# [Service]
# EnvironmentFile=-/etc/asf/%i
# ExecStart=dotnet /home/%i/ArchiSteamFarm/ArchiSteamFarm.dll --no-restart --service --system-required
# Restart=on-success
# RestartSec=1s
# SyslogIdentifier=asf-%i
# User=%i
# (...)
```

Ve bu kadar, şimdi biriminiz `[Service]` altında sadece `Restart=always` varmış gibi davranıyor.

Tabii ki, alternatif olarak dosyayı `cp` ederek kendiniz yönetebilirsiniz, ancak bu, ASF birimini orijinal haliyle koruyarak esnek değişiklikler yapmanıza olanak tanır, örneğin bir **[symlink](https://wikipedia.org/wiki/Symbolic_link)** ile.

---

## ASF'yi asla yönetici olarak çalıştırmayın!

ASF, sürecin yönetici (`root`) olarak çalışıp çalışmadığını doğrulayan kendi doğrulamasını içerir. `root` olarak çalıştırmak, ASF süreci tarafından yapılan herhangi bir işlem için **gerekli değildir**, uygun şekilde yapılandırılmış bir ortamda çalışıyorsa ve bu nedenle bir **kötü uygulama** olarak değerlendirilmelidir. Bu, Windows'ta ASF'nin "yönetici olarak çalıştır" ayarı ile **asla** çalıştırılmaması ve Unix'te ASF'nin kendisi için **ayrı bir kullanıcı hesabı** olması veya bir masaüstü sisteminde kendi hesabınızı yeniden kullanması gerektiği anlamına gelir.

`ASF`'yi `root` olarak çalıştırmayı neden teşvik etmediğimize dair daha ayrıntılı bilgi için **[superuser](https://superuser.com/questions/218379/why-is-it-bad-to-run-as-root)** ve diğer kaynaklara başvurabilirsiniz. Hala ikna olmadıysanız, ASF sürecinin hemen başında `rm -rf /*` komutunu çalıştırsa makinenize ne olacağını kendinize sorun.

### `root` olarak çalışıyorum çünkü ASF dosyalarına yazamıyor

Bu, ASF'nin erişmeye çalıştığı dosyaların izinlerinin yanlış yapılandırıldığı anlamına gelir. `root` hesabına giriş yapmalısınız (ya `su` ya da `sudo -i` ile) ve ardından izinleri düzeltmek için `chown -hR asf:asf /path/to/ASF` komutunu çalıştırmalısınız; burada `asf:asf` kullanacağınız kullanıcı ve `/path/to/ASF` da ilgili dizindir. Eğer özel bir `--path` kullanıyorsanız, ASF'nin farklı bir dizini kullanmasını belirtiyorsanız, aynı komutu o yol için de çalıştırmalısınız.

Bunu yaptıktan sonra, ASF'nin kendi dosyalarına yazma ile ilgili herhangi bir sorun yaşamanız gerekmemelidir; çünkü ASF'nin ilgilendiği her şeyin sahibi, ASF sürecinin çalışacağı kullanıcıya değiştirilmiştir.

### `root` olarak çalışıyorum çünkü başka nasıl yapılacağını bilmiyorum

```sh
su # Veya root shell'ine geçmek için sudo -i
useradd -m asf # ASF'yi çalıştırmak istediğiniz kullanıcı hesabını oluşturun
chown -hR asf:asf /path/to/ASF # Yeni kullanıcınızın ASF dizinine erişimi olduğundan emin olun
su asf -c /path/to/ASF/ArchiSteamFarm # Veya sudo -u asf /path/to/ASF/ArchiSteamFarm, programı kendi kullanıcı hesabınız altında başlatmak için
```

Bu manuel olarak yapmaktır; yukarıda açıklandığı gibi **[`systemd` servisini](#systemd-service-for-linux)** kullanmak çok daha kolaydır.

### Daha iyi bildiğimi düşünüyorum ve yine de `root` olarak çalışmak istiyorum

ASF sizi zorla durdurmaz, sadece kısa bir uyarı gösterir. Bir gün programda bir hata nedeniyle tüm işletim sisteminizi ve verilerinizi kaybederseniz, şaşırmayın - uyarıldınız.

---

## Birden fazla örnek

ASF, aynı makinede birden fazla sürecin çalıştırılması ile uyumludur. Bu örnekler tamamen bağımsız olabilir veya aynı ikili dosya konumundan türemiş olabilir (bu durumda, farklı `--path` **[komut satırı argümanı](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)** ile çalıştırmak istersiniz).

Aynı ikili dosyadan birden fazla örnek çalıştırırken, genellikle tüm yapılandırmalarda otomatik güncellemeleri devre dışı bırakmanız gerektiğini unutmayın; çünkü bunlar arasında otomatik güncellemeler konusunda senkronizasyon yoktur. Otomatik güncellemeleri etkin tutmak istiyorsanız, bağımsız örnekleri tercih etmenizi öneririz, ancak tüm diğer ASF örneklerinin kapalı olduğundan emin olabildiğiniz sürece güncellemeleri çalıştırmak hala mümkündür.

ASF, diğer ASF örnekleri ile minimum bir OS çapında, süreçler arası iletişimi sürdürmek için elinden geleni yapacaktır. Bu, ASF'nin yapılandırma dizinini diğer örneklerle kontrol etmesini ve `*LimiterDelay` **[global yapılandırma özellikleri](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)** ile yapılandırılmış çekirdek süreç limitörlerini paylaşmasını içerir. Bu, birden fazla ASF örneğinin oran sınırlama sorunlarıyla karşılaşmasını önler. Teknik açıdan, tüm platformlar, Windows'ta `C:\Users\<YourUser>\AppData\Local\Temp\ASF` ve Unix'te `/tmp/ASF` gibi geçici dizinlerde oluşturulmuş ASF dosya tabanlı kilit mekanizmasını kullanır.

ASF örneklerinin aynı `*LimiterDelay` özelliklerini paylaşmaları gerekmez; farklı değerler kullanabilirler, çünkü her ASF kendi yapılandırılmış gecikmesini kilidi aldıktan sonra salma zamanına ekler. Yapılandırılmış `*LimiterDelay` sıfır olarak ayarlandığında, ASF örneği, diğer örneklerle hala paylaşılan bir kilidi korusa bile, belirli bir kaynağın kilidini beklemeyi tamamen atlar. Başka bir değere ayarlandığında, ASF diğer ASF örnekleriyle düzgün şekilde senkronize olacak ve sırası geldiğinde kilidi serbest bırakacak, ardından yapılandırılmış gecikme süresi kadar bekleyerek diğer örneklerin devam etmesine izin verecektir.

ASF, paylaşılmış kapsam konusunda karar verirken `WebProxy` ayarını dikkate alır; bu, farklı `WebProxy` yapılandırmaları kullanan iki ASF örneğinin limitörlerini birbirleriyle paylaşmayacağı anlamına gelir. Bu, farklı ağ arayüzlerinden beklenildiği gibi `WebProxy` yapılandırmalarının aşırı gecikmeler olmadan çalışmasını sağlamak için uygulanmıştır. Bu, çoğu kullanım durumu için yeterli olmalıdır; ancak, eğer örneğin isteklerinizi farklı bir şekilde yönlendiriyorsanız, `--network-group` **[komut satırı argümanı](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)** aracılığıyla özel bir ağ grubu belirleyebilirsiniz. Bu, ASF grubunu bu örnekle senkronize etmenizi sağlar. özgü olarak kullanılır; bu, ASF'nin doğru grubu belirlemek için `WebProxy`'yi kullanmayı bırakacağı anlamına gelir, çünkü bu durumda gruplamadan siz sorumlusunuz.

Yukarıda açıklandığı gibi **[`systemd` servisini](#systemd-service-for-linux)** birden fazla ASF örneği için kullanmak çok basittir; sadece başka bir kullanıcı kullanın ve geri kalan adımları izleyin. Bu, ayrı ikili dosyalarla birden fazla ASF örneği çalıştırmaya eşdeğer olacaktır; bu nedenle bunlar da otomatik olarak güncellenebilir ve birbirlerinden bağımsız olarak çalışabilir.
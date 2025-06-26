# Düşük Bellek Kurulum

Bu, **[yüksek performans kurulumu](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/High-performance-setup)** ile tam zıttır ve genellikle ASF'nin bellek kullanımını azaltmak istiyorsanız bu ipuçlarını takip etmelisiniz; bunun bedeli, genel performansın düşmesidir.

---

ASF, tanım gereği kaynakları son derece hafif kullanır. Kullanımınıza bağlı olarak, Linux üzerinde 128 MB VPS bile çalıştırabilir, ancak bu kadar düşük bir yapılandırma genellikle önerilmez ve çeşitli sorunlara yol açabilir. Hafif olmasına rağmen, ASF optimal hızda çalışmak için gerektiğinde işletim sisteminden daha fazla bellek talep etmekten çekinmez.

ASF, uygulama olarak mümkün olduğunca optimize ve verimli olmaya çalışır, bu da çalıştırma sırasında kullanılan kaynakları da dikkate alır. Bellek söz konusu olduğunda, ASF performansı bellek tüketiminin önüne geçer, bu da geçici bellek "dalgaları"na yol açabilir. Örneğin, 3+ rozet sayfasına sahip hesaplarla, ASF önce ilk sayfayı alır ve analiz eder, ardından toplam sayfa sayısını okur ve her ek sayfa için alma görevini başlatır. Bu, kalan sayfaların eşzamanlı olarak alınmasına ve analiz edilmesine yol açar. Bu "ek" bellek kullanımı (minimum operasyon için gereken bellekten daha fazla) büyük ölçüde hızlanabilir ve genel performansı artırabilir. Benzer şeyler, tüm genel ASF görevleri için geçerlidir, örneğin aktif ticaret tekliflerinin analizinde, ASF tüm teklifleri aynı anda analiz edebilir. Ayrıca, ASF (C# çalışma zamanı) kullanılmayan belleği işletim sistemine hemen geri vermez, bu da ASF sürecinin giderek daha fazla bellek tükettiğini ancak neredeyse hiç geri vermediğini görebilirsiniz. Bazı insanlar bunu şüpheli bulabilir ve hatta bellek sızıntısı olarak değerlendirebilir, ancak endişelenmeyin, bunlar beklenen davranışlardır.

ASF, mevcut kaynakları mümkün olduğunca iyi kullanacak şekilde son derece optimize edilmiştir. ASF'nin yüksek bellek kullanımı, ASF'nin o belleği **aktif olarak kullanması** ve **gereksinim duyması** anlamına gelmez. Genellikle ASF, bellek tahsis ederken gelecekteki işlemler için "yer" olarak saklar, çünkü işletim sisteminden her bellek parçası için talepte bulunmaktansa, bellek tahsisatını hızlandırmak büyük ölçüde performansı artırabilir. Çalışma zamanı, kullanılmayan ASF belleğini işletim sistemine **gerçekten** ihtiyaç duyulduğunda geri vermelidir. **[Kullanılmayan bellek israf edilen bellektir](https://www.howtogeek.com/128130/htg-explains-why-its-good-that-your-computers-ram-is-full)**. Gerekli bellek ihtiyacı, mevcut bellekten daha yüksek olduğunda sorun yaşarsınız, ASF'nin bellek tahsislerini saklamak yerine performansını artırmak amacıyla bazı bellek tahsisatlarını tutması sorun oluşturmaz. Sorunlar genellikle Linux çekirdeğiniz ASF sürecini OOM (out of memory) nedeniyle öldürdüğünde ortaya çıkar, `htop`'ta ASF sürecini en yüksek bellek tüketicisi olarak gördüğünüzde değil.

**[Çöp toplama](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science))** işlemi ASF'de çok karmaşık bir mekanizmadır ve ASF'yi, işletim sisteminizi ve diğer süreçleri dikkate alacak şekilde akıllıdır. Çok fazla boş bellek olduğunda, ASF performansı artırmak için gereken her şeyi talep eder. Bu, sunucu GC ile 1 GB kadar olabilir. İşletim sistemi belleği neredeyse dolu olduğunda, ASF otomatik olarak bazı bellekleri işletim sistemine geri verebilir, bu da ASF'nin toplam bellek kullanımını 50 MB kadar düşük tutabilir. 50 MB ile 1 GB arasındaki fark büyüktür, ancak 512 MB VPS ile 32 GB'lık büyük bir sunucu arasındaki fark da büyüktür. ASF bu belleğin faydalı olacağına garanti verebiliyorsa ve aynı zamanda başka hiçbir şey bu bellek kullanımını gerektirmiyorsa, bunu saklamayı tercih eder ve geçmişte gerçekleştirilen rutinlere dayalı olarak kendini otomatik olarak optimize eder. ASF'de kullanılan GC, kendini ayarlayabilir ve süreç daha uzun süre çalıştıkça daha iyi sonuçlar elde edebilir.

Bu nedenle, ASF sürecinin belleği kurulumdan kuruluşa değişiklik gösterir, çünkü ASF mevcut kaynakları **en verimli şekilde kullanmaya** çalışır ve Windows XP zamanlarında olduğu gibi sabit bir şekilde değil. ASF'nin kullandığı gerçek (gerçek) bellek `stats` **[komutu](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** ile doğrulanabilir ve genellikle sadece birkaç bot için yaklaşık 4 MB ile 30 MB arasında değişir; IPC gibi özellikler ve diğer gelişmiş özellikler kullanıldığında bu değer artar. `stats` komutuyla elde edilen bellek, ayrıca çöp toplayıcı tarafından henüz geri alınmamış serbest belleği de içerir. Diğer her şey, paylaşılan çalışma zamanı belleği (yaklaşık 40-50 MB) ve yürütme için ayrılmış alanı içerir (değişken). Bu nedenle, aynı ASF düşük bellekli VPS ortamında 50 MB kadar az bellek kullanabilirken, masaüstünüzde 1 GB'a kadar çıkabilir. ASF, ortamınıza aktif olarak uyum sağlar ve işletim sisteminizi baskı altında bırakmadan, aynı zamanda bol miktarda kullanılmayan belleği kullanarak optimal dengeyi bulmaya çalışır.

---

Bellek kullanımını tahmin etmek için ASF'yi doğru yönde yönlendirmeye yardımcı olabilecek birçok yöntem vardır. Genel olarak, bunu yapmanız gerekmedikçe çöp toplayıcının huzur içinde çalışmasına izin vermek en iyisidir. Ancak bu her zaman mümkün olmayabilir, örneğin Linux sunucunuz ayrıca birkaç web sitesi, MySQL veritabanı ve PHP işçileri barındırıyorsa, ASF'nin bellek kullanımını küçültmek için onun küçülmesini göze alamazsınız, çünkü bu genellikle çok geç olur ve performans bozulması daha erken gelir. Bu genellikle daha fazla ayar yapmakla ilgilidir ve bu nedenle bu sayfayı okumanız yararlı olabilir.

Aşağıdaki öneriler birkaç kategoriye ayrılmıştır, çeşitli zorluk seviyeleriyle.

---

## ASF Kurulumu (Kolay)

Aşağıdaki ipuçları **performansı olumsuz etkilemez** ve tüm kurulumlara güvenle uygulanabilir.

- Mümkünse **[generic versiyon](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up#generic-setup)** kullanın. Generic versiyon, içinde çalışma zamanı bulunmadığı için, tek bir dosya olarak gelmediği için, çalıştırıldığında kendini çıkarmadığı için daha az bellek tüketir ve daha küçüktür. OS'e özgü paketler pratik ve kullanışlıdır, ancak ASF'yi başlatmak için gereken her şeyi içerir; bunu kendiniz halledebilir ve generic ASF varyantını kullanabilirsiniz.
- Birden fazla ASF örneği çalıştırmayın. ASF, aynı anda sınırsız sayıda botu işlemek için tasarlanmıştır ve her ASF örneğini farklı bir arayüze/IP adresine bağlamadıkça, **bir** ASF süreci ile birden fazla bot olmalıdır (gerekirse).
- Botların `FarmingPreferences` içinde `ShutdownOnFarmingFinished` özelliğini kullanın. Aktif bot, devre dışı bırakılan birinden daha fazla kaynak kullanır. It's not a significant save, as the state of bot still needs to be kept, but you're saving some amount of resources, especially all resources related to networking, such as TCP sockets. Gerekirse diğer botları her zaman açabilirsiniz.
- Bot sayınızı düşük tutun. `Enabled` olmayan bot örneği daha az kaynak kullanır, çünkü ASF başlatmakla uğraşmaz. Ayrıca, ASF'nin her yapılandırma için bir bot oluşturması gerektiğini de unutmayın, bu nedenle belirli bir botu `başlatmanız` gerekmediğinde ve biraz daha fazla bellek tasarrufu yapmak istiyorsanız, `Bot.json`'u geçici olarak `Bot.json.bak` olarak yeniden adlandırabilirsiniz, böylece ASF devre dışı bırakılmış bot örneğinin durumunu bellekte tutmaz ve diğer şeyler için yer bırakır (çok küçük bir tasarruf, %99,9 durumlarda bunu umursamamalısınız, sadece botlarınızı `Enabled` olarak `false` bırakın). This way you won't be able to `start` it without renaming it back, but ASF also won't bother keeping state of this bot in memory, leaving room for other things (very small save, in 99.9% cases you shouldn't bother with it, just keep your bots with `Enabled` of `false`).
- Yapılandırmalarınızı ince ayar yapın. Özellikle genel ASF yapılandırması birçok ayar içerir; örneğin, `LoginLimiterDelay`'i artırarak botlarınızı daha yavaş getirebilir, bu da mevcut başlatılmış örneğin rozetleri almasını sağlar, botlarınızı daha hızlı başlatmak yerine, bu daha fazla kaynak kullanır çünkü daha fazla bot aynı anda büyük iş The less work that has to be done at the same time - the less memory used.

Bellek kullanımıyla başa çıkarken aklınızda bulundurmanız gereken birkaç şey bunlardır. Ancak, bu şeyler bellek kullanımı üzerinde herhangi bir "kritik" etkide bulunmaz, çünkü bellek kullanımı genellikle ASF'nin başa çıkması gereken şeylerden gelir ve kart çiftleştirmesi için kullanılan iç yapılar değil.

En fazla kaynak tüketen işlevler:
- Badge page parsing
- Inventory parsing

Which means that memory will spike the most when ASF is dealing with reading badge pages, and when it's dealing with its inventory (e.g. sending trade or working with STM). Bu, bellek kullanımının en çok artacağı anlamına gelir, çünkü ASF çok büyük miktarda veriyle başa çıkmak zorundadır - favori tarayıcınızın bu iki sayfayı açması da bu kadar düşük olmayacaktır. Üzgünüm, işler böyle çalışıyor - rozet sayfalarınızın sayısını azaltın ve envanter öğelerinizin sayısını düşük tutun, bu kesinlikle yardımcı olabilir.

---

## Çalışma Zamanı Ayarları (Gelişmiş)

Aşağıdaki ipuçları **performans düşüşüne yol açar** ve dikkatli kullanılmalıdır.

Bu ayarları uygulamanın önerilen yolu `DOTNET_` ortam değişkenlerini kullanmaktır. Elbette, diğer yöntemleri de kullanabilirsiniz, örneğin `runtimeconfig.json`, ancak bazı ayarlar bu şekilde ayarlanamaz ve bunun yanı sıra ASF, bir sonraki güncellemede özel `runtimeconfig.json`'unuzu kendi dosyasıyla değiştirecektir. Bu nedenle, süreci başlatmadan önce kolayca ayarlayabileceğiniz ortam değişkenlerini kullanmanızı öneririz.

.NET çalışma zamanı, **[çöp toplayıcıyı](https://docs.microsoft.com/dotnet/core/run-time-config/garbage-collector)** birçok şekilde ayarlamanıza olanak tanır ve bu, ihtiyaçlarınıza göre GC sürecini ince ayar yapabilir. Aşağıda özellikle önemli olduğunu düşündüğümüz özellikler belgelenmiştir.

### [`GCHeapHardLimitPercent`](https://docs.microsoft.com/dotnet/core/run-time-config/garbage-collector#heap-limit-percent)

> GC yığın kullanımının toplam fiziksel belleğin yüzdesi olarak belirlenen izin verilen sınırı belirtir.

ASF süreci için "sert" bellek sınırı, bu ayar GC'yi toplam belleğin sadece bir alt kümesini kullanacak şekilde ayarlar. Bu, çeşitli sunucu benzeri durumlarda, ASF'ye belirli bir bellek yüzdesi tahsis etmek ancak asla daha fazlasını tahsis etmemek için özellikle yararlı olabilir. ASF'nin bellek kullanımını sınırlandırmak, tüm bu bellek tahsisatlarının kaybolmasını sihirli bir şekilde ortadan kaldırmaz; bu nedenle bu değeri çok düşük ayarlamak, ASF sürecinin zorunlu olarak sona ermesine neden olabilir.

Öte yandan, bu değeri yeterince yüksek ayarlamak, ASF'nin asla gerçekçi olarak karşılayabileceğinizden daha fazla bellek kullanmamasını sağlar ve makinenize ağır yük altında bile biraz nefes alma alanı bırakır, aynı zamanda programın işini en verimli şekilde yapmasına izin verir.

### [`GCConserveMemory`](https://learn.microsoft.com/dotnet/core/runtime-config/garbage-collector#conserve-memory)

> Çöp toplayıcının daha sık çöp toplama yapmasını ve olası olarak daha uzun duraklama sürelerini göze alarak belleği korumasını yapılandırır.

0-9 arasında bir değer kullanılabilir. Daha büyük bir değer, daha fazla GC'nin bellek üzerinde optimize edilmesini sağlar.

### [`GCHighMemPercent`](https://docs.microsoft.com/dotnet/core/run-time-config/garbage-collector#high-memory-percent)

> GC'nin daha agresif hale gelmeye başladığı, kullanılan bellek miktarını belirtir.

Bu ayar, işletim sisteminizin tamamının bellek eşik değerini yapılandırır; bu eşiği geçtikten sonra GC daha agresif hale gelir ve daha fazla bellek serbest bırakmak için daha yoğun GC işlemi gerçekleştirir ve sonuçta daha fazla bellek serbest bırakılır. Bu özelliği, tüm işletim sisteminizin performansı için "kritik" olarak düşündüğünüz maksimum bellek miktarını (yüzde olarak) ayarlamak iyi bir fikirdir. Varsayılan değer 90%'dır ve genellikle bunu 80-97% aralığında tutmak istersiniz, çünkü çok düşük bir değer GC'den gereksiz bir agresyon ve performans düşüşüne neden olabilir, çok yüksek bir değer ise işletim sisteminize gereksiz bir yük bindirir, çünkü ASF bellek tahsisatlarından bazılarını serbest bırakabilir.

### **[`GCLatencyLevel`](https://github.com/dotnet/runtime/blob/a1d48d6c00b5aecc063d1a58b0d9281c611ada91/src/coreclr/gc/gcpriv.h#L445-L468)**

> Optimize etmek istediğiniz GC gecikme seviyesini belirtir.

Bu, ASF için çok iyi çalıştığı doğrulanan belgelenmemiş bir özelliktir, GC nesil boyutlarını sınırlandırarak ve sonuç olarak GC'nin bunları daha sık ve agresif şekilde temizlemesini sağlar. Varsayılan (dengeli) gecikme seviyesi `1`'dir, ancak `0` kullanarak bellek kullanımına yönelik ayarlamalar yapabilirsiniz.

### [`gcTrimCommitOnLowMemory`](https://docs.microsoft.com/dotnet/standard/garbage-collection/optimization-for-shared-web-hosting)

> Ayarlandığında, geçici segment için taahhüt edilmiş alanı daha agresif şekilde küçültür. Bu, çok sayıda sunucu süreci örneği çalıştıran ve mümkün olduğunca az bellek taahhüdü yapmak isteyen ortamlar için kullanılır.

Bu çok fazla iyileştirme sağlamaz, ancak özellikle ASF için sistem düşük bellekli olduğunda GC'yi daha agresif hale getirebilir.

---

Seçilen özellikleri uygun ortam değişkenlerini ayarlayarak etkinleştirebilirsiniz. Örneğin, Linux (shell):

```shell
# Bunları yapılandırmayı unutmayın eğer bunları kullanmayı planlıyorsanız
export DOTNET_GCHeapHardLimitPercent=0x4B # 75% olarak hex
export DOTNET_GCHighMemPercent=0x50 # 80% olarak hex

export DOTNET_GCConserveMemory=9
export DOTNET_GCLatencyLevel=0
export DOTNET_gcTrimCommitOnLowMemory=1

./ArchiSteamFarm # OS'e özgü yapı için
./ArchiSteamFarm.sh # Generic yapı için
```

Veya Windows (powershell):

```powershell
# Bunları yapılandırmayı unutmayın eğer bunları kullanmayı planlıyorsanız
$Env:DOTNET_GCHeapHardLimitPercent=0x4B # 75% olarak hex
$Env:DOTNET_GCHighMemPercent=0x50 # 80% olarak hex

$Env:DOTNET_GCConserveMemory=9
$Env:DOTNET_GCLatencyLevel=0
$Env:DOTNET_gcTrimCommitOnLowMemory=1

.\ArchiSteamFarm.exe # OS'e özgü yapı için
.\ArchiSteamFarm.cmd # Generic yapı için
```

Özellikle `GCLatencyLevel`, performansı çok fazla azaltmadan ASF'nin bellek kullanımını önemli ölçüde düşürerek büyük faydalar sağlayabilir. Bu, özellikle `OptimizationMode` ile performansı çok fazla bozmadan ASF'nin bellek kullanımını büyük ölçüde azaltmak için uygulayabileceğiniz en iyi ipuçlarından biridir.

---

## ASF Ayarı (Orta Düzey)

Aşağıdaki ipuçları **ciddi performans düşüşüne yol açar** ve dikkatle kullanılmalıdır.

- Son çare olarak, `MinMemoryUsage` için `OptimizationMode` **[global yapılandırma özelliği](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)** kullanabilirsiniz. Amacını dikkatlice okuyun, çünkü bu, neredeyse hiçbir bellek yararı sağlamadan performansta ciddi düşüşlere yol açar. Genellikle **[çalışma zamanı ayarları](#runtime-tuning-advanced)**'na geçtikten sonra ve bu yöntemleri tükettikten sonra kullanılması gereken son seçenektir. Kesinlikle kritik değilse, bellek kısıtlı ortamlarda bile `MinMemoryUsage` kullanmanızı önermiyoruz.

---

## Önerilen Optimizasyon

- Basit ASF kurulum ipuçlarıyla başlayın, **[generic ASF varyantını](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up#generic-setup)** kullanın ve belki de ASF'nizi yanlış şekilde kullanıyor olabilirsiniz; örneğin, tüm botlarınız için süreci birkaç kez başlatıyor veya sadece bir veya iki tanesini otomatik olarak başlatmak istiyorsanız, bunları aktif tutuyorsunuz.
- Eğer bu hala yeterli değilse, yukarıda belirtilen yapılandırma özelliklerini etkinleştirin ve tüm `DOTNET_` ortam değişkenlerini kullanın. Özellikle `GCLatencyLevel` düşük mali
- Eğer bu bile yardımcı olmadıysa, son çare olarak `MinMemoryUsage` `OptimizationMode`'u etkinleştirin. Bu, ASF'yi neredeyse her şeyi senkronize şekilde çalışacak şekilde zorlar, bu da çok daha yavaş çalışmasını sağlar ancak iş parçacığı havuzuna dayanmadan paralel yürütme işlemlerini dengelemesine izin verir.

Belleği daha da azaltmak fiziksel olarak imkansızdır, ASF zaten performansta ciddi şekilde azalmıştır ve tüm olanaklarınızı tüketmişsinizdir, hem kod hem de çalışma zamanı açısından. Kullanım için biraz ekstra bellek eklemeyi düşünün, hatta 128 MB büyük bir fark yaratabilir.
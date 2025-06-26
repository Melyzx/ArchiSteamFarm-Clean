# Yapılandırma

Bu sayfa ASF yapılandırmasına ayrılmıştır. `config` dizininin eksiksiz bir dokümantasyonu olarak hizmet vererek ASF'yi ihtiyaçlarınıza göre ayarlamanıza olanak tanır.

* **[Giriş](#introduction)**
* **[Web tabanlı ConfigGenerator](#web-based-configgenerator)**
* **[ASF-ui configuration](#asf-ui-configuration)**
* **[Manuel yapılandırma](#manual-configuration)**
* **[Global config](#global-config)**
* **[Bot config](#bot-config)**
* **[Dosya yapısı](#file-structure)**
* **[JSON mapping](#json-mapping)**
* **[Compatibility mapping](#compatibility-mapping)**
* **[Configs compatibility](#configs-compatibility)**
* **[Otomatik Yeniden Yükleme](#auto-reload)**

---

## Giriş

ASF yapılandırması iki ana bölüme ayrılır - genel (süreç) yapılandırması ve her botun yapılandırması. Her botun `BotName.json` (burada `BotName` botun adıdır) adlı kendi bot yapılandırma dosyası vardır, genel ASF (süreç) yapılandırması ise `ASF.json` adlı tek bir dosyadır.

Bir bot, ASF sürecinde yer alan tek bir buhar hesabıdır. Düzgün çalışması için ASF'nin en az **bir** tanımlanmış bot örneğine ihtiyacı vardır. Bot örneklerinin süreçle zorlanan bir sınırı yoktur, bu nedenle istediğiniz kadar bot (buhar hesabı) kullanabilirsiniz.

ASF, yapılandırma dosyalarını depolamak için **[JSON](https://en.wikipedia.org/wiki/JSON)** biçimini kullanır. Programı yapılandırabileceğiniz insan dostu, okunabilir ve çok evrensel bir formattır. Yine de endişelenmeyin, ASF'yi yapılandırmak için JSON'u bilmenize gerek yok. Bir çeşit bash komut dosyasıyla ASF konfigürasyonlarını toplu olarak oluşturmak isterseniz diye bundan bahsettim.

Yapılandırma çeşitli şekillerde yapılabilir. ASF'den bağımsız yerel bir uygulama olan **[Web tabanlı ConfigGenerator](https://justarchinet.github.io/ASF-WebConfigGenerator)** aracımızı kullanabilirsiniz. Doğrudan ASF'de yapılan yapılandırma için **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)** IPC ön ucumuzu kullanabilirsiniz. Son olarak, aşağıda belirtilen sabit JSON yapısını takip ettikleri için her zaman yapılandırma dosyalarını manuel olarak oluşturabilirsiniz. Kullanılabilir seçenekleri kısaca açıklayacağız.

---

## Web tabanlı ConfigGenerator

**[Web tabanlı ConfigGenerator](https://justarchinet.github.io/ASF-WebConfigGenerator)** aracımızın amacı, ASF yapılandırma dosyalarını oluşturmak için kullanılan kullanıcı dostu bir ön uç sağlamaktır. Web tabanlı ConfigGenerator %100 istemci tabanlıdır, yani girdiğiniz ayrıntıların hiçbir yere gönderilmediği, yalnızca yerel olarak işlendiği anlamına gelir. Bu, güvenlik ve güvenilirliği garanti eder, çünkü tüm dosyaları indirip `index.html` dosyasını en sevdiğiniz tarayıcıda çalıştırırsanız **[çevrimdışı](https://github.com/JustArchiNET/ASF-WebConfigGenerator/tree/main/docs)** bile çalışabilir.

Web tabanlı ConfigGenerator'ın Chrome ve Firefox'ta düzgün çalıştığı doğrulanmıştır, ancak en popüler javascript özellikli tarayıcılarda düzgün çalışması gerekir.

Kullanımı oldukça basittir - uygun sekmeye geçerek `ASF` veya `Bot` yapılandırması oluşturmak isteyip istemediğinizi seçin, seçilen yapılandırma dosyası sürümünün ASF sürümünüzle eşleştiğinden emin olun, ardından tüm ayrıntıları girin ve "indir" düğmesine basın. Bu dosyayı ASF `config` dizinine taşıyın, gerekirse mevcut dosyaların üzerine yazın. Muhtemel diğer tüm değişiklikler için tekrarlayın ve yapılandırılacak tüm mevcut seçeneklerin açıklaması için bu bölümün geri kalanına bakın.

---

## ASF-ui configuration

**[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)** IPC arayüzümüz, ASF'yi de yapılandırmanıza olanak tanır ve ilk yapılandırmaları oluşturduktan sonra ASF'yi yeniden yapılandırmak için üstün bir çözümdür çünkü bunları statik olarak oluşturan Web tabanlı ConfigGenerator'ın aksine, konfigürasyonları yerinde düzenleyebilir.

ASF-ui'yi kullanmak için **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** arayüzümüzün kendisinin etkinleştirilmiş olması gerekir. `IPC`, varsayılan olarak etkindir, bu nedenle kendiniz devre dışı bırakmadığınız sürece hemen erişebilirsiniz.

Programı başlattıktan sonra, ASF'nin **[IPC adresine](http://localhost:1242)** gidin. Her şey düzgün çalıştıysa, ASF yapılandırmasını da oradan değiştirebilirsiniz.

---

## Manuel yapılandırma

Genel olarak ConfigGenerator'ımızı veya ASF-ui'mizi kullanmanızı şiddetle tavsiye ederiz, çünkü bu çok daha kolaydır ve JSON yapısında hata yapmamanızı sağlar, ancak herhangi bir nedenle istemiyorsanız, uygun konfigürasyonları manuel olarak da oluşturabilirsiniz. Uygun yapıda iyi bir başlangıç için aşağıdaki JSON örneklerini kontrol edin, içeriği bir dosyaya kopyalayabilir ve konfigürasyonunuz için temel olarak kullanabilirsiniz. Ön uçlarımızdan herhangi birini kullanmadığınız için, config dosyanızın **[geçerli](https://jsonlint.com)** olduğundan emin olun, çünkü ayrıştırılamazsa ASF onu yüklemeyi reddeder. Geçerli bir JSON olsa bile, ASF tarafından istendiği gibi tüm özelliklerin doğru türe sahip olduğundan da emin olmanız gerekir. Kullanılabilir tüm alanların doğru JSON yapısı için **[JSON eşleme](#json-mapping)** bölümüne ve aşağıdaki belgelerimize bakın.

---

## Global config

Global config `ASF.json` dosyasında bulunur ve aşağıdaki yapıya sahiptir:

```json
{
    "AutoRestart": true,
    "Blacklist": [],
    "CommandPrefix": "!",
    "ConfirmationsLimiterDelay": 10,
    "ConnectionTimeout": 90,
    "CurrentCulture": null,
    "Debug": false,
    "DefaultBot": null,
    "FarmingDelay": 15,
    "FilterBadBots": true,
    "GiftsLimiterDelay": 1,
    "Headless": false,
    "IdleFarmingPeriod": 8,
    "InventoryLimiterDelay": 4,
    "IPC": true,
    "IPCPassword": null,
    "IPCPasswordFormat": 0,
    "LicenseID": null,
    "LoginLimiterDelay": 10,
    "MaxFarmingTime": 10,
    "MaxTradeHoldDuration": 15,
    "MinFarmingDelayAfterBlock": 60,
    "OptimizationMode": 0,
    "PluginsUpdateList": [],
    "PluginsUpdateMode": 0,
    "ShutdownIfPossible": false,
    "SteamMessagePrefix": "/me ",
    "SteamOwnerID": 0,
    "SteamProtocols": 7,
    "UpdateChannel": 1,
    "UpdatePeriod": 24,
    "WebLimiterDelay": 300,
    "WebProxy": null,
    "WebProxyPassword": null,
    "WebProxyUsername": null
}
```

---

Tüm seçenekler aşağıda açıklanmıştır:

### `AutoRestart`

``true`\` varsayılan değeri olan ``bool`\` türü. Bu özellik, ASF'nin gerektiğinde kendi kendine yeniden başlatma işlemi yapıp yapamayacağını tanımlar. ASF güncellemesi ( `UpdatePeriod` veya `update` komutu ile yapılır), `ASF.json` config düzenlemesi, `restart` komutu ve benzeri gibi ASF'den kendi kendine yeniden başlatma gerektiren birkaç olay vardır. Yeniden başlatma genellikle iki bölümden oluşur - yeni bir süreç oluşturmak ve mevcut olanı bitirmek. Çoğu kullanıcı bundan memnun olmalı ve bu özelliği varsayılan değeri olan `true` olarak tutmalıdır, ancak - ASF'yi kendi komut dosyanız aracılığıyla ve/veya `dotnet` ile çalıştırıyorsanız, sürecin başlatılması üzerinde tam kontrole sahip olmak isteyebilirsiniz ve arka planda sessizce çalışan yeni (yeniden başlatılan) bir ASF süreci ve komut dosyasının ön planında olmayan, eski ASF süreciyle birlikte çıkan bir durumdan kaçının. Bu, özellikle yeni sürecin artık doğrudan çocuğunuz olmayacağı ve bu da örneğin onun için standart konsol girişini kullanamamanıza neden olacağı gerçeği göz önüne alındığında önemlidir.

Böyle bir durumda, bu özellik özel olarak sizin içindir ve bunu `false` olarak ayarlayabilirsiniz. Ancak, bu durumda süreci yeniden başlatmaktan **sizin** sorumlu olduğunuzu unutmayın. Bu bir şekilde önemlidir çünkü ASF, yeni bir süreç oluşturmak yerine (örneğin güncellemeden sonra) yalnızca çıkış yapar, bu nedenle tarafınızdan herhangi bir mantık eklenmezse, siz tekrar başlatana kadar çalışmayı durdurur. ASF her zaman başarıyı (sıfır) veya başarıyı (sıfır olmayan) gösteren uygun hata koduyla çıkar, bu şekilde komut dosyanıza, başarısızlık durumunda ASF'nin otomatik olarak yeniden başlatılmasını önlemesi veya en azından yerel bir kopya oluşturması gereken uygun mantığı ekleyebilirsiniz. Ayrıca, `restart` komutunun bu özelliğin nasıl ayarlandığına bakılmaksızın her zaman ASF'yi yeniden başlatacağını unutmayın, çünkü bu özellik varsayılan davranışı tanımlarken `restart` komutu her zaman işlemi yeniden başlatır. Bu özelliği devre dışı bırakmak için bir nedeniniz yoksa, etkin tutmalısınız.

---

### `Blacklist`

Varsayılan değeri boş olan `ImmutableHashSet<uint>` türü. ASF kara listesi, bu rozetleri çiftçilik için uygun değil olarak işaretleme amacına hizmet eder, böylece neyin yetiştirileceğine karar verirken bunları sessizce görmezden gelebilir ve tuzağa düşmeyebiliriz. Adından da anlaşılacağı gibi, bu genel yapılandırma özelliği, otomatik ASF farm süreci tarafından tamamen yok sayılacak uygulama kimliklerini (oyunları) tanımlar. Ne yazık ki Steam, yaz/kış indirimi rozetlerini "kart düşüşü için uygun" olarak işaretlemeyi sever, bu da ASF sürecini aslında bir oyun olmayan geçerli bir oyun olduğunu düşündürerek ASF sürecini karıştırır. Herhangi bir kara liste olmasaydı, ASF sonunda aslında bir oyun olmayan bir oyunu çiftçilik yaparken "takılır" ve olmayacak kart düşüşünü sonsuza kadar beklerdi.

ASF varsayılan olarak iki kara liste içerir - ASF koduna kodlanmış ve düzenlenmesi mümkün olmayan `SalesBlacklist` ve burada tanımlanan normal `Blacklist`. `SalesBlacklist`, ASF sürümüyle birlikte güncellenir ve genellikle yayınlandığı tarihteki tüm "kötü" uygulama kimliklerini içerir, bu nedenle güncel ASF kullanıyorsanız, burada tanımlanan kendi `Blacklist` listenizi tutmanız gerekmez. Bu özelliğin temel amacı, yeni, ASF sürümü yayınlandığı sırada bilinmeyen ve yetiştirilmemesi gereken uygulama kimliklerini kara listeye almanıza izin vermektir. Sabit kodlanmış `SalesBlacklist` mümkün olduğunca hızlı bir şekilde güncellenmektedir, bu nedenle en son ASF sürümünü kullanıyorsanız kendi `Blacklist` listenizi güncellemeniz gerekmez, ancak `Blacklist` olmadan yeni bir satış rozeti yayınladığında ASF'yi güncellemek zorunda kalırsınız - en son ASF kodunu kullanmaya zorlamak istemiyorum, bu nedenle bu özellik, bir nedenden dolayı istemiyorsanız veya yeni hardcoded'a güncelleyemiyorsanız ASF'yi "düzeltmenize" izin vermek için burada. yeni ASF sürümünde `SalesBlacklist`, yine de eski ASF'nizin çalışmaya devam etmesini istiyorsunuz. Bu özelliği düzenlemek için **güçlü** bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

Bunun yerine bot tabanlı bir kara liste arıyorsanız, `fb`, `fbadd` ve `fbrm` **[komutlarına](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** bir göz atın.

---

### `CommandPrefix`

Varsayılan değeri `!` olan `string` türü. Bu özellik, ASF **[komutları](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** için kullanılan **büyük/küçük harfe duyarlı** öneki belirtir. Başka bir deyişle, ASF'nin sizi dinlemesi için her ASF komutunun önüne eklemeniz gereken şey budur. ASF'nin komut öneki kullanmamasını sağlamak için bu değeri `null` veya boş olarak ayarlamak mümkündür, bu durumda komutları düz tanımlayıcılarıyla girersiniz. Ancak, bunu yapmak potansiyel olarak ASF'nin performansını düşürecektir çünkü ASF, `CommandPrefix` ile başlamazsa mesajı daha fazla ayrıştırmamak için optimize edilmiştir - eğer kasıtlı olarak kullanmamaya karar verirseniz, ASF tüm mesajları okumaya ve yanıt vermeye zorlanacaktır. , ASF komutu olmasalar bile. Bu nedenle, varsayılan `!` değerini beğenmiyorsanız, `/` gibi bazı `CommandPrefix` kullanmaya devam etmeniz önerilir. Tutarlılık için, `CommandPrefix` tüm ASF sürecini etkiler. Bu özelliği düzenlemek için bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `ConfirmationsLimiterDelay`

Varsayılan değeri `10` olan `byte` türü. ASF, hız sınırını tetiklemekten kaçınmak için iki ardışık 2FA onayı getirme isteği arasında en az `ConfirmationsLimiterDelay` saniye olmasını sağlayacaktır - bunlar örneğin `2faok` komutu sırasında **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** tarafından ve ayrıca çeşitli ticaretle ilgili işlemler gerçekleştirilirken gerektiği gibi kullanılır. Varsayılan değer, testlerimize göre ayarlandı ve sorunlarla karşılaşmak istemiyorsanız düşürülmemelidir. Bu özelliği düzenlemek için **güçlü** bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `ConnectionTimeout`

Varsayılan değeri `90` olan `byte` türü. Bu özellik, ASF tarafından yapılan çeşitli ağ eylemleri için zaman aşımlarını saniye cinsinden tanımlar. Özellikle, `ConnectionTimeout`, HTTP ve IPC istekleri için zaman aşımını saniye cinsinden tanımlar, `ConnectionTimeout / 10` başarısız kalp atışı sayısının maksimum sayısını tanımlarken, `ConnectionTimeout / 30` ilk Steam ağı bağlantı isteği için izin verilen dakika sayısını tanımlar. `90` varsayılan değeri, insanların çoğu için iyi olmalıdır, ancak oldukça yavaş bir ağ bağlantınız veya bilgisayarınız varsa, bu sayıyı daha yüksek bir değere ( `120` gibi) çıkarmak isteyebilirsiniz. Daha büyük değerlerin yavaş veya erişilemeyen Steam sunucularını sihirli bir şekilde düzeltmeyeceğini unutmayın, bu nedenle olmayacak bir şey için sonsuza kadar beklememeli ve daha sonra tekrar denemeliyiz. Bu değerin çok yüksek ayarlanması, ağ sorunlarının yakalanmasında aşırı gecikmeye ve genel performansta düşüşe neden olur. Bu değeri çok düşük ayarlamak, genel kararlılığı ve performansı da düşürecektir, çünkü ASF hala ayrıştırılmakta olan geçerli isteği iptal edecektir. Bu nedenle, bu değeri varsayılan değerden daha düşük ayarlamak genellikle avantaj sağlamaz, çünkü Steam sunucuları zaman zaman çok yavaş olma eğilimindedir ve ASF isteklerini ayrıştırmak için daha fazla zaman gerektirebilir. Sorunları daha erken tespit etmek ve ASF'nin daha hızlı yeniden bağlanmasını/yanıt vermesini sağlamak istiyorsanız, varsayılan değer (veya biraz daha düşük, örneğin `60` gibi, ASF'yi daha az sabırlı hale getirmek) işe yaramalıdır. Bunun yerine ASF'nin ağ sorunlarıyla (başarısız istekler, kaybolan kalp atışları veya Steam bağlantısı kesildi gibi) karşılaştığını fark ederseniz, bunun **sizin ağınızdan değil**, Steam'in kendisinden kaynaklandığından eminseniz, bu değeri artırmak mantıklı olabilir. zaman aşımlarını artırmak ASF'yi daha "sabırlı" hale getirir ve hemen yeniden bağlanmaya karar vermez.

Bu özelliğin artırılmasını gerektirebilecek bir örnek durum, ASF'nin Steam tarafından tamamen kabul edilip işlenmesi 2 dakikadan fazla sürebilen çok büyük ticaret teklifleriyle ilgilenmesine izin vermektir. Varsayılan zaman aşımını artırarak, ASF daha sabırlı olacak ve ticaretin gerçekleşmediğine karar verip ilk talebi terk etmeden önce daha uzun süre bekleyecektir.

Başka bir durum, iletilen verileri işlemek için daha fazla zamana ihtiyaç duyan çok yavaş makine veya internet bağlantısından kaynaklanabilir. CPU/ağ bant genişliği neredeyse hiçbir zaman darboğaz olmadığından bu oldukça nadir bir durumdur, ancak yine de bahsetmeye değer bir olasılıktır.

Kısacası, varsayılan değer çoğu durum için uygun olmalıdır, ancak gerekirse artırmak isteyebilirsiniz. Yine de, varsayılan değerin çok üzerine çıkmanın da pek bir anlamı yok, çünkü daha büyük zaman aşımları erişilemeyen Steam sunucularını sihirli bir şekilde düzeltmeyecek. Bu özelliği düzenlemek için bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `CurrentCulture`

Varsayılan değeri `null` olan `string` türü. Varsayılan olarak ASF, işletim sistemi dilinizi kullanmaya çalışır ve mümkünse bu dildeki çevrilmiş dizeleri kullanmayı tercih eder. Bu, ASF'yi en popüler dillerde **[yerelleştirmeye](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization)** çalışan topluluğumuz sayesinde mümkün oldu. Herhangi bir nedenle işletim sisteminizin yerel dilini kullanmak istemiyorsanız, bunun yerine kullanmak istediğiniz herhangi bir geçerli dili seçmek için bu yapılandırma özelliğini kullanabilirsiniz. Kullanılabilir tüm kültürlerin bir listesi için lütfen **[MSDN](https://msdn.microsoft.com/en-us/library/cc233982.aspx)** adresini ziyaret edin ve `Language tag` araması yapın. Genellikle, ana diliniz yerine İngilizce olarak ASF'yi tercih ediyorsanız, bu yapılandırma özelliğini kullanmak istersiniz. ASF'nin hem belirli kültürleri (örneğin `"en-GB"`) hem de nötr kültürleri (örneğin `"en"`) kabul ettiğini belirtmek güzeldir. Geçerli kültürü belirtmek, para birimi/tarih biçimi ve benzerleri gibi diğer kültüre özgü davranışları da etkileyecektir. Yerel olmayan bir kültürü seçtiyseniz, dile özgü karakterleri düzgün bir şekilde görüntülemek için ek yazı tipi/dil paketlerine ihtiyacınız olabileceğini lütfen unutmayın.

---

### `Hata ayıklama`

Varsayılan değeri `false` olan `bool` türü. Bu özellik, işlemin hata ayıklama modunda çalışıp çalışmayacağını tanımlar. Hata ayıklama modundayken ASF, `config` ile birlikte tüm ASF ve Steam sunucuları arasındaki iletişimi takip eden özel bir `debug` dizini oluşturur. Hata ayıklama bilgileri, ağ iletişimi ve genel ASF iş akışıyla ilgili kötü sorunların tespit edilmesine yardımcı olabilir. Buna ek olarak, bazı program rutinleri çok daha ayrıntılı olacaktır, örneğin `WebBrowser`, bazı isteklerin neden başarısız olduğunun kesin nedenini belirtir - bu girişler normal ASF günlüğüne yazılır. **Geliştirici tarafından istenmedikçe ASF'yi Hata Ayıklama modunda çalıştırmamalısınız**. ASF'yi hata ayıklama modunda çalıştırmak **performansı düşürür**, **kararlılığı olumsuz etkiler** ve **çeşitli yerlerde çok daha ayrıntılıdır**, bu nedenle **yalnızca** kasıtlı olarak, kısa vadede, belirli bir sorunu hata ayıklamak, sorunu yeniden oluşturmak veya başarısız bir istek hakkında daha fazla bilgi almak ve benzeri amaçlar için kullanılmalıdır, ancak normal program yürütme için **kullanılmamalıdır**. **Birçok** yeni hata, sorun ve istisna göreceksiniz - bunların tümünü kendiniz analiz etmeye karar verirseniz, ASF, Steam ve onun tuhaflıkları hakkında iyi bir bilgiye sahip olduğunuzdan emin olun, çünkü her şey alakalı değildir.

**UYARI:** Bu modu etkinleştirmek, Steam'e giriş yapmak için kullandığınız girişler ve parolalar gibi **potansiyel olarak hassas** bilgilerin günlüğe kaydedilmesini içerir (ağ günlüğü nedeniyle). Bu veriler hem `debug` dizinine hem de standart `log.txt` dosyasına yazılır (bu, artık bu bilgileri günlüğe kaydetmek için kasıtlı olarak çok daha ayrıntılıdır). ASF tarafından oluşturulan hata ayıklama içeriğini herhangi bir genel yerde yayınlamamalısınız, ASF geliştiricisi size her zaman e-postasına veya başka bir güvenli konuma göndermeyi hatırlatmalıdır. Bu hassas ayrıntıları saklamıyoruz veya kullanmıyoruz, hata ayıklama rutinlerinin bir parçası olarak yazılıyorlar çünkü varlıkları sizi etkileyen sorunla alakalı olabilir. ASF günlüğünü hiçbir şekilde değiştirmemenizi tercih ederiz, ancak endişeleniyorsanız, bu hassas ayrıntıları uygun şekilde düzenlemekte özgürsünüz.

> Redaksiyon, hassas bilgilerin örneğin yıldızlarla değiştirilmesini içerir. Hassas satırları tamamen kaldırmaktan kaçınmalısınız, çünkü bunların salt varlığı alakalı olabilir ve korunmalıdır.

---

### `DefaultBot`

Varsayılan değeri `null` olan `string` türü. Bazı senaryolarda ASF, bir şeyin işlenmesinden sorumlu varsayılan bir bot kavramıyla çalışır - örneğin IPC komutları veya hedef botu belirtmediğinizde etkileşimli konsol. Bu özellik, bu tür senaryoları işlemekten sorumlu varsayılan botu, buraya `BotName` adını koyarak seçmenize olanak tanır. Belirtilen bot yoksa veya `null` varsayılan değerini kullanırsanız, ASF bunun yerine alfabetik olarak sıralanan ilk kayıtlı botu seçer. Genellikle, IPC ve etkileşimli konsol komutlarındaki `[Bots]` argümanını atlamak ve bu tür çağrılar için her zaman aynı botu varsayılan bot olarak seçmek istiyorsanız, bu yapılandırma özelliğini kullanmak istersiniz.

---

### `FarmingDelay`

Varsayılan değeri `15` olan `byte` türü. ASF'nin çalışması için, şu anda yetiştirilen oyunu her `FarmingDelay` dakikada bir kontrol ederek, belki de tüm kartları çoktan düşürmüş olup olmadığını kontrol eder. Bu özelliğin çok düşük ayarlanması, aşırı miktarda buhar isteğinin gönderilmesine neden olabilirken, çok yüksek ayarlanması, ASF'nin tamamen yetiştirildikten sonra belirli bir unvanı `FarmingDelay` dakikaya kadar "yetiştirmesine" neden olabilir. Varsayılan değer çoğu kullanıcı için mükemmel olmalıdır, ancak çok sayıda bot çalıştırıyorsanız, gönderilen buhar isteklerini sınırlamak için bunu `30` dakika gibi bir süreye artırmayı düşünebilirsiniz. ASF'nin olay tabanlı bir mekanizma kullandığı ve her Steam öğesi düştüğünde oyun rozeti sayfasını kontrol ettiği, bu nedenle genel olarak **onu sabit zaman aralıklarında kontrol etmemize bile gerek olmadığı** belirtmek güzeldir, ancak Steam ağına tam olarak güvenmediğimiz için - Yine de oyun rozeti sayfasını kontrol ediyoruz, eğer son `FarmingDelay` dakikalarında kart bırakma olayı ile kontrol etmediysek (Steam ağı bize öğenin bırakılması veya bunun gibi şeyler hakkında bilgi vermediyse). Steam ağının düzgün çalıştığını varsayarsak, bu değeri azaltmak **çiftçilik verimliliğini hiçbir şekilde artırmaz**, **ağ yükünü önemli ölçüde artırırken** - yalnızca varsayılan `15` dakikadan itibaren artırılması (gerekirse) önerilir. Bu özelliği düzenlemek için **güçlü** bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `FilterBadBots`

Varsayılan değeri `true` olan `bool` türü. Bu özellik, ASF'nin bilinen ve işaretlenmiş kötü aktörlerden alınan ticaret tekliflerini otomatik olarak reddedip reddetmeyeceğini tanımlar. Bunu yapmak için ASF, kara listeye alınmış Steam tanımlayıcılarının bir listesini almak için gerektiği gibi sunucumuzla iletişim kuracaktır. Listelenen botlar, bizim tarafımızdan ASF girişimine zararlı olarak sınıflandırılan, **[davranış kurallarımızı](https://github.com/JustArchiNET/ArchiSteamFarm/blob/main/.github/CODE_OF_CONDUCT.md)** ihlal eden, sağlanan işlevselliği ve kaynakları kullanan kişiler tarafından işletilmektedir. **[`PublicListing`]([https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin#publiclisting](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin#publiclisting))** gibi diğer insanları kötüye kullanmak ve istismar etmek için veya sunucuya DDoS saldırıları başlatmak gibi düpedüz suç faaliyetleri yürütüyorlar. ASF, tüm kullanıcıları arasında genel adalet, dürüstlük ve işbirliği konusunda güçlü bir duruş sergilediğinden, bu özellik varsayılan olarak etkinleştirilir ve bu nedenle ASF, sunduğu hizmetlerden zararlı olarak sınıflandırdığımız botları filtreler. Bu özelliği düzenlemek için **güçlü** bir nedeniniz yoksa (örneğin ifademize katılmamak ve bu botların çalışmasına kasıtlı olarak izin vermek (hesaplarınızı istismar etmek dahil)), bunu varsayılan değerde tutmalısınız.

---

### `GiftsLimiterDelay`

Varsayılan değeri `1` olan `byte` türü. ASF, oran sınırını tetiklemekten kaçınmak için iki ardışık hediye/anahtar/lisans işleme (kullanma) isteği arasında en az `GiftsLimiterDelay` saniye olmasını sağlayacaktır. Buna ek olarak, `owns` **[komutu](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** tarafından verilenler gibi oyun listesi istekleri için genel sınırlayıcı olarak da kullanılacaktır. Bu özelliği düzenlemek için **güçlü** bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `Headless`

Varsayılan değeri `false` olan `bool` türü. Bu özellik, işlemin başsız modda çalışıp çalışmayacağını tanımlar. Başsız moddayken ASF, bir sunucuda veya başka bir etkileşimli olmayan ortamda çalıştığını varsayar, bu nedenle konsol girişi aracılığıyla herhangi bir bilgi okumaya çalışmaz. Bu, isteğe bağlı ayrıntıları (2FA kodu, SteamGuard kodu, parola veya ASF'nin çalışması için gereken diğer değişkenler gibi hesap kimlik bilgileri) ve diğer tüm konsol girişlerini (etkileşimli komut konsolu gibi) içerir. Başka bir deyişle, `Headless` modu, ASF konsolunu salt okunur yapmakla eşdeğerdir. Bu ayar, esas olarak ASF'yi sunucularında çalıştıran kullanıcılar için yararlıdır, çünkü örneğin 2FA kodu sormak yerine, ASF bir hesabı durdurarak işlemi sessizce iptal eder. ASF'yi bir sunucuda çalıştırmıyorsanız ve ASF'nin başsız olmayan modda çalışabildiğini daha önce onayladıysanız, bu özelliği devre dışı bırakmalısınız. Başsız moddayken herhangi bir kullanıcı etkileşimi reddedilecek ve hesaplarınız başlatılırken **herhangi bir** konsol girişi gerektiriyorsa çalışmayacaktır. Bu, sunucular için yararlıdır, çünkü ASF, kullanıcıların bunları sağlamasını (sonsuza kadar) beklemek yerine, kimlik bilgileri istendiğinde hesaba giriş yapmayı deneyebilir.

Enabling this mode will allow you to supply required console input through other means, i.e. ASF-ui (ASF API), or through **[`input`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands#input-command)** command. Bu özelliği nasıl ayarlayacağınızdan emin değilseniz, varsayılan `false` değeriyle bırakın.

---

### `IdleFarmingPeriod`

Varsayılan değeri `8` olan `byte` türü. ASF'nin yetiştirecek hiçbir şeyi olmadığında, hesabın çiftçilik yapacak yeni oyunlar alıp almadığını periyodik olarak her `IdleFarmingPeriod` saatte bir kontrol edecektir. Yeni aldığımız oyunlar söz konusu olduğunda bu özelliğe gerek yoktur, çünkü ASF bu durumda rozet sayfalarını otomatik olarak kontrol edecek kadar akıllıdır. `IdleFarmingPeriod` esas olarak, sahip olduğumuz eski oyunlara takas kartlarının eklenmesi gibi durumlar içindir. Bu durumda olay olmadığı için ASF, bunu kapsamak istiyorsak rozet sayfalarını periyodik olarak kontrol etmelidir. `0` değeri bu özelliği devre dışı bırakır. Ayrıca bkz: `FarmingPreferences` içindeki `ShutdownOnFarmingFinished` tercihi.

---

### `InventoryLimiterDelay`

Varsayılan değeri `4` olan `byte` türü. ASF, oran sınırını tetiklemekten kaçınmak için iki ardışık web envanter isteği arasında en az `InventoryLimiterDelay` saniye olmasını sağlayacaktır - bunlar, örneğin envanter bildirimlerini okundu olarak işaretlerken kullanılır, ayrıca üçüncü taraf eklentileri tarafından diğer kullanıcıların envanterini getirmek için de kullanılabilir. Bu özellik, kendi envanterimizi almak için kullanılmaz, çünkü ASF bunun için çok daha verimli dahili ağ çağrısı kullanır, bu nedenle `loot` veya `transfer` gibi komutları hiçbir şekilde etkilemez. `4` varsayılan değeri, 100'den fazla ardışık bot örneğinin envanterlerini işaretlemeye göre ayarlanmıştır ve kullanıcıların çoğunu (hepsini değilse de) tatmin etmelidir. Ancak, çok az sayıda botunuz varsa bunu azaltmak, hatta `0` olarak değiştirmek isteyebilirsiniz, böylece ASF gecikmeyi yok sayar ve Steam envanterlerini çok daha hızlı işaretler. Yine de dikkatli olun, çünkü çok düşük ayarlamak, Steam'in IP'nizi geçici olarak yasaklamasına neden olur ve bu, herhangi bir arama yapmanızı tamamen engeller. Çok fazla envanter isteği olan çok sayıda bot çalıştırıyorsanız, bu değeri artırmanız da gerekebilir, ancak bu durumda muhtemelen bu isteklerin sayısını sınırlamaya çalışmalısınız. Bu özelliği düzenlemek için **güçlü** bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `IPC`

Varsayılan değeri `true` olan `bool` türü. Bu özellik, ASF'nin **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** sunucusunun işlemle birlikte başlayıp başlamamasını tanımlar. IPC, yerel bir HTTP sunucusu başlatarak **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)** kullanımı da dahil olmak üzere süreçler arası iletişime izin verir. ASF-ui dahil olmak üzere ASF ile herhangi bir üçüncü taraf IPC entegrasyonu kullanmayı düşünmüyorsanız, bu seçeneği güvenle devre dışı bırakabilirsiniz. Aksi takdirde, etkin tutmak (varsayılan seçenek) iyi bir fikirdir.

---

### `IPCPassword`

Varsayılan değeri `null` olan `string` türü. Bu özellik, IPC aracılığıyla yapılan her API çağrısı için zorunlu parolayı tanımlar ve ekstra bir güvenlik önlemi görevi görür. Boş olmayan bir değere ayarlandığında, tüm IPC istekleri, burada belirtilen parolaya ayarlanmış ekstra bir `password` özelliği gerektirir. `null` varsayılan değeri, parolanın gerekliliğini atlayarak ASF'nin tüm sorguları dikkate almasını sağlar. Buna ek olarak, bu seçeneğin etkinleştirilmesi, çok kısa bir süre içinde çok fazla yetkisiz istek gönderdikten sonra belirli bir `IPAddress` adresini geçici olarak yasaklayacak olan yerleşik IPC kaba kuvvet önleme mekanizmasını da etkinleştirir. Bu özelliği düzenlemek için bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `IPCPasswordFormat`

Varsayılan değeri `0` olan `byte` türü. Bu özellik, `IPCPassword` özelliğinin biçimini tanımlar ve temeldeki tür olarak `EHashingMethod` kullanır. Daha fazla bilgi edinmek istiyorsanız lütfen **[Güvenlik](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** bölümüne bakın, çünkü `IPCPassword` özelliğinin gerçekten eşleşen `IPCPasswordFormat` içinde parola içerdiğinden emin olmanız gerekir. Başka bir deyişle, `IPCPasswordFormat` öğesini değiştirdiğinizde, `IPCPassword` öğeniz yalnızca olmak istemekle kalmayıp **zaten** bu biçimde olmalıdır. Ne yaptığınızı bilmiyorsanız, varsayılan `0` değeriyle tutmalısınız.

---

### `LicenseID`

Varsayılan değeri `null` olan `Guid?` türü. Bu özellik, **[sponsorlarımızın](https://github.com/sponsors/JustArchi)**, çalışması için ücretli kaynaklar gerektiren isteğe bağlı özelliklerle ASF'yi geliştirmesine olanak tanır. Şimdilik bu, `ItemsMatcher` eklentisindeki **[`MatchActively`]([https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin#matchactively](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin#matchactively))** özelliğini kullanmanıza izin veriyor.

GitHub'ı aylık ve bir kerelik katmanlar sunduğu, tam otomasyona izin verdiği ve size anında erişim sağladığı için kullanmanızı tavsiye etsek de, şu anda mevcut olan diğer tüm **[bağış seçeneklerini](https://github.com/JustArchiNET/ArchiSteamFarm#archisteamfarm)** da **destekliyoruz**. Belirli bir süre için geçerli manuel bir lisans almak için diğer yöntemleri kullanarak nasıl bağış yapılacağına ilişkin talimatlar için **[bu gönderiye](https://github.com/JustArchiNET/ArchiSteamFarm/discussions/2780#discussioncomment-4486091)** bakın.

Kullanılan yöntemden bağımsız olarak, ASF sponsoruysanız lisansınızı **[buradan](https://asf.justarchi.net/User/Status)** alabilirsiniz. Kimliğinizi doğrulamak için GitHub ile oturum açmanız gerekecek, yalnızca herkese açık bilgi olan kullanıcı adınızı istiyoruz. `LicenseID`, `f6a0529813f74d119982eb4fe43a9a24` gibi 32 onaltılık karakterden oluşur.

**`LicenseID` kimliğinizi başkalarıyla paylaşmadığınızdan emin olun**. Kişisel olarak verildiği için sızdırılırsa iptal edilebilir. Herhangi bir şans eseri bu sizin başınıza yanlışlıkla geldiyse, aynı yerden yeni bir tane oluşturabilirsiniz.

Ekstra ASF işlevlerini etkinleştirmek istemiyorsanız, lisansı sağlamanıza gerek yoktur.

---

### `LoginLimiterDelay`

Varsayılan değeri `10` olan `byte` türü. ASF, oran sınırını tetiklemekten kaçınmak için iki ardışık bağlantı girişimi arasında en az `LoginLimiterDelay` saniye olmasını sağlayacaktır. `10` varsayılan değeri, 100'den fazla bot örneğine bağlanmaya göre ayarlanmıştır ve kullanıcıların çoğunu (hepsini değilse de) tatmin etmelidir. Ancak, çok az sayıda botunuz varsa bunu artırmak/azaltmak, hatta `0` olarak değiştirmek isteyebilirsiniz, böylece ASF gecikmeyi yok sayar ve Steam'e çok daha hızlı bağlanır. Çok fazla botunuz varken bunu çok düşük ayarlarsanız, özellikle aynı IP adresini kullanan diğer Steam istemcileri/araçlarıyla birlikte, Steam'in IP'nizi geçici olarak yasaklayacağı ve bunun da oturum açmanızı **tamamen** engelleyeceği konusunda uyarın. `InvalidPassword/RateLimitExceeded` hatasıyla - ve bu yalnızca ASF'yi değil, normal Steam istemcinizi de içerir. Aynı şekilde, aşırı sayıda bot çalıştırıyorsanız, özellikle de aynı IP adresini kullanan diğer Steam istemcileri/araçlarıyla birlikte, büyük olasılıkla oturum açmaları daha uzun bir süre boyunca yaymak için bu değeri artırmanız gerekecektir.

Yan not olarak, bu değer aynı zamanda ASF tarafından zamanlanmış tüm eylemlerde, örneğin `SendTradePeriod` içindeki işlemlerde yük dengeleme arabelleği olarak da kullanılır. Bu özelliği düzenlemek için **güçlü** bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `MaxFarmingTime`

Varsayılan değeri `10` olan `byte` türü. Bildiğiniz gibi Steam her zaman düzgün çalışmıyor, bazen oyun oynamamıza rağmen oyun süremizin kaydedilmemesi gibi garip durumlar olabiliyor. ASF, tek bir oyunun solo modda en fazla `MaxFarmingTime` saat boyunca oynanmasına izin verecek ve bu süreden sonra tamamen oynandığını düşünecektir. Bu, garip durumların olması durumunda değil, aynı zamanda Steam'in ASF'nin daha fazla ilerlemesini durduracak yeni bir rozet yayınlaması durumunda da çiftçilik sürecinin donmaması için gereklidir (bkz: `Blacklist`). `10` saatlik varsayılan değer, bir oyundaki tüm buhar kartlarını düşürmek için yeterli olmalıdır. Bu özelliğin çok düşük ayarlanması, geçerli oyunların atlanmasına neden olabilir (ve evet, yetiştirilmesi 9 saat süren geçerli oyunlar var), çok yüksek ayarlanması ise çiftçilik sürecinin donmasına neden olabilir. Lütfen bu özelliğin tek bir çiftçilik oturumunda yalnızca tek bir oyunu etkilediğini (bu nedenle tüm kuyruğu geçtikten sonra ASF o unvana geri dönecektir) ve ayrıca toplam oyun süresine değil dahili ASF çiftçilik süresine dayalı olmadığını unutmayın, bu nedenle ASF ayrıca bir yeniden başlatmanın ardından o başlığa geri dönün. Bu özelliği düzenlemek için **güçlü** bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `MaxTradeHoldDuration`

Varsayılan değeri `15` olan `byte` türü. Bu özellik, **[ticaret](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** bölümünde tanımlandığı gibi, kabul etmeye istekli olduğumuz ticaret bekleme süresinin maksimum süresini gün olarak tanımlar - ASF, daha uzun süre bekletilen işlemleri reddedecektir. `MaxTradeHoldDuration` günden fazla. Bu seçenek yalnızca `TradingPreferences` değeri `SteamTradeMatcher` olan botlar için anlamlıdır, çünkü `Master`/`SteamOwnerID` işlemlerini ve bağışları etkilemez. Trade holds are annoying for everyone, and nobody really wants to deal with them. Ticaret beklemeleri herkes için can sıkıcıdır ve kimse gerçekten onlarla uğraşmak istemez. ASF'nin liberal kurallar üzerinde çalışması ve takasta tutulsa da tutulmasa da herkese yardım etmesi gerekiyor - bu yüzden bu seçenek varsayılan olarak `15` olarak ayarlandı. Ancak, bunun yerine ticaret beklemelerinden etkilenen tüm işlemleri reddetmeyi tercih ederseniz, buraya `0` belirtebilirsiniz. Lütfen kısa ömürlü kartların bu seçenekten etkilenmediğini ve **[ticaret](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** bölümünde açıklandığı gibi, ticaret beklemeleri olan kişiler için otomatik olarak reddedildiğini unutmayın, bu nedenle genel olarak herkesi yalnızca bu nedenle reddetmeye gerek yoktur. Bu özelliği düzenlemek için bir nedeniniz yoksa, varsayılan değerde tutmalısınız.


---

### `MinFarmingDelayAfterBlock`

Varsayılan değeri `60` olan `byte` türü. Bu özellik, ASF'nin daha önce bir oyun başlatarak şu anda çiftçilik yapan ASF'yi zorla bağlantısını kesmeye karar vermeniz durumunda gerçekleşen `LoggedInElsewhere` ile bağlantısı kesilirse, kart çiftçiliğine devam etmeden önce bekleyeceği minimum süreyi saniye cinsinden tanımlar. Bu gecikme, esas olarak kolaylık ve genel gider nedenleriyle mevcuttur, örneğin, oyun kilidinin yalnızca bir saniyeliğine mevcut olması nedeniyle ASF ile hesabınızı tekrar işgal etmekle uğraşmak zorunda kalmadan oyunu yeniden başlatmanıza olanak tanır. Oturumu geri alma işleminin `LoggedInElsewhere` bağlantısının kesilmesine neden olması nedeniyle, ASF tüm yeniden bağlantı prosedüründen geçmek zorundadır, bu da makine ve Steam ağına ek baskı uygular, bu nedenle mümkünse ek bağlantı kesintilerinden kaçınmak tercih edilir. Varsayılan olarak bu, `60` saniye olarak yapılandırılmıştır, bu da oyunu çok fazla güçlük çekmeden yeniden başlatmanıza izin vermek için yeterli olmalıdır. Ancak, örneğin ağınızın sık sık bağlantısının kesilmesi ve ASF'nin çok erken devralması gibi, bu durumda yeniden bağlanma sürecinden kendiniz geçmek zorunda kalmanız gibi, bunu artırmakla ilgilenebileceğiniz senaryolar vardır. Bu özellik için maksimum `255` değerine izin veriyoruz, bu da tüm yaygın senaryolar için yeterli olmalıdır. Yukarıdakilere ek olarak, gecikmeyi azaltmak, hatta `0` değeriyle tamamen kaldırmak da mümkündür, ancak yukarıda belirtilen nedenlerden dolayı bu genellikle önerilmez. Bu özelliği düzenlemek için bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `OptimizationMode`

`byte` type with default value of `0`. Bu özellik, ASF'nin çalışma zamanı sırasında tercih edeceği optimizasyon modunu tanımlar. Şu anda ASF iki modu desteklemektedir - `MaxPerformance` olarak adlandırılan `0` ve `MinMemoryUsage` olarak adlandırılan `1`. Varsayılan olarak ASF, mümkün olduğu kadar çok şeyi paralel (eşzamanlı) olarak çalıştırmayı tercih eder, bu da tüm CPU çekirdekleri, birden çok CPU iş parçacığı, birden çok soket ve birden çok iş parçacığı havuzu görevi arasında yük dengelemesi yaparak performansı artırır. Örneğin, ASF, çiftçilik yapılacak oyunları kontrol ederken ilk rozet sayfanızı soracak ve ardından istek geldiğinde, ASF bundan kaç tane rozet sayfasına sahip olduğunuzu okuyacak ve ardından diğerlerini aynı anda isteyecektir. Bu, **neredeyse her zaman** istemeniz gereken şeydir, çünkü çoğu durumda ek yük minimumdur ve asenkron ASF kodunun faydaları, tek bir CPU çekirdeği ve yoğun şekilde sınırlı güce sahip en eski donanımda bile görülebilir. Ancak, paralel olarak işlenen birçok görevle, ASF çalışma zamanı bunların bakımından, örneğin soketleri açık tutmaktan, iş parçacıklarını canlı tutmaktan ve görevlerin işlenmesinden sorumludur, bu da zaman zaman artan bellek kullanımına neden olabilir ve eğer aşırı derecede kısıtlanmışsanız kullanılabilir bellek miktarına göre, ASF'yi mümkün olduğunca az görev kullanmaya zorlamak ve tipik olarak paralel olabilen zaman uyumsuz kodu eşzamanlı bir şekilde çalıştırmak için bu özelliği `1` (`MinMemoryUsage`) olarak değiştirmek isteyebilirsiniz. . Bu özelliği yalnızca **[düşük bellekli kurulumu](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)** daha önce okuduysanız ve dev gibi performans artışını çok küçük bir bellek ek yükü azalması için feda etmek istiyorsanız düşünmelisiniz. Genellikle bu seçenek, **[düşük bellekli kurulumda](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)** açıklandığı gibi ASF kullanımınızı sınırlamak veya çalışma zamanının çöp toplayıcısını ayarlamak gibi diğer olası yollarla elde edebileceğinizden çok daha kötüdür, örneğin ASF kullanımınızı sınırlayarak veya çalışma zamanının çöp toplayıcısını ayarlayarak, **[düşük bellekli kurulumda](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)** açıklandığı gibi. Bu nedenle, diğer (çok daha iyi) seçeneklerle tatmin edici sonuçlar elde edemediyseniz, çalışma zamanı yeniden derlemesinden hemen önce, `MinMemoryUsage` öğesini **son çare** olarak kullanmalısınız. Bu özelliği düzenlemek için **güçlü** bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `PluginsUpdateList`

Varsayılan değeri boş olan `ImmutableHashSet<string>` türü. Bu özellik, aşağıda tanımlanan `PluginsUpdateMode` uyarınca otomatik güncellemeler için dikkate alınması kara listeye alınmış veya beyaz listeye alınmış eklenti derleme adlarının listesini tanımlar.

Bu özelliği düzenlemek için bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `PluginsUpdateMode`

Varsayılan değeri `0` olan `byte` türü. Bu özellik, yukarıda tanımlanan `PluginsUpdateList` öğesine anlam veren eklenti güncelleme modunu tanımlar. Bu özelliği belirterek, beyan edilenler dışındaki tüm eklentiler için otomatik güncellemeleri kolayca etkinleştirebilir/devre dışı bırakabilirsiniz.

- `Whitelist` olarak adlandırılan `0` değeri, `PluginsUpdateList` içinde tanımlananlar dışındaki tüm eklentilerin otomatik güncellemesini **devre dışı bırakır**.
- `Blacklist` olarak adlandırılan `1` değeri, `PluginsUpdateList` içinde tanımlananlar dışındaki tüm eklentiler için otomatik güncellemeyi **etkinleştirir**.

**ASF ekibi, kendi güvenliğiniz için yalnızca güvenilir taraflardan otomatik güncellemeleri etkinleştirmeniz gerektiğini hatırlatmak ister**. Kötü amaçlı eklentilerin, bu ayardan **bağımsız olarak** kendilerini güncellemeye veya uzaktan komutları çalıştırmaya karar verebileceğini unutmayın, bu nedenle bu ayar yalnızca ASF tarafından sağlanan eklenti güncelleme işlevselliği için geçerlidir ve yine de kullandığınız her eklentiyi uygun şekilde doğruladığınızdan emin olmalısınız.

Eklentilerin güncellemeleri, varsayılan olarak ASF güncelleme rutiniyle birlikte gerçekleştirilir - **[`UpdateChannel`](#updatechannel)** ve **[`UpdatePeriod`](#updateperiod)**. `update` komutu gibi standart ASF güncelleme mekanizmaları, isteğe bağlı eklenti güncellemesini de tetikleyecektir. Bunun yerine eklentileri aynı anda ASF sürümünü güncellemeden manuel olarak güncellemek isterseniz, `updateplugins` komutu böyle bir olasılık sunar.

Bu özelliği düzenlemek için bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `ShutdownIfPossible`

Varsayılan değeri `false` olan `bool` türü. Etkinleştirildiğinde, ASF mümkünse, yani kayıtlı tüm botlarınız durdurulduğunda, işlemi kapatmaya çalışır. Bu, tüm bot örneklerinizde `ShutdownOnFarmingFinished` ile birleştirildiğinde özellikle yararlı olabilir, çünkü bu şekilde ASF, botlarınızdan sonuncusu çiftçiliği bitirdiğinde otomatik olarak kapanabilir.

Kullanıcıların çoğunluğunun beklentisi, örneğin `IPC` kullanımı için sürecin her zaman çalışır durumda olması olduğundan, bu seçenek varsayılan olarak devre dışıdır.

---

### `SteamMessagePrefix`

Varsayılan değeri `"/me "` olan `string` türü. Bu özellik, ASF tarafından gönderilen tüm Steam mesajlarının önüne eklenecek bir ön eki tanımlar. Varsayılan olarak ASF, bot mesajlarını Steam sohbetinde farklı renkte göstererek daha kolay ayırt etmek için `"/me "` öneki kullanır. Bahsetmeye değer bir diğer önek, benzer bir sonuca ulaşan ancak farklı bir biçimlendirme kullanan `"/pre "` öneki. Ön ek kullanımını tamamen devre dışı bırakmak ve tüm ASF mesajlarını geleneksel bir şekilde çıkarmak için bu özelliği boş bir dizeye veya `null` olarak da ayarlayabilirsiniz. Bu özelliğin yalnızca Steam mesajlarını etkilediğini belirtmek güzeldir - diğer kanallar (IPC gibi) aracılığıyla döndürülen yanıtlar etkilenmez. Standart ASF davranışını özelleştirmek istemiyorsanız, varsayılan olarak bırakmanız iyi bir fikirdir.

---

### `SteamOwnerID`

Varsayılan değeri `0` olan `ulong` türü. Bu özellik, ASF süreç sahibinin 64 bit biçimindeki Steam Kimliğini tanımlar ve belirli bir bot örneğinin `Master` iznine çok benzer (ancak bunun yerine geneldir). Bu özelliği neredeyse her zaman kendi ana Steam hesabınızın kimliğine ayarlamak istersiniz. `Master` izni, bot örneği üzerinde tam kontrol içerir, ancak `exit`, `restart` veya `update` gibi genel komutlar yalnızca `SteamOwnerID` için ayrılmıştır. Bu kullanışlıdır, çünkü arkadaşlarınızın botlarını çalıştırmak isteyebilirsiniz, ancak onların ASF sürecini kontrol etmelerine, örneğin `exit` komutuyla çıkış yapmalarına izin vermeyebilirsiniz. `0` varsayılan değeri, ASF sürecinin sahibi olmadığını belirtir, bu da kimsenin genel ASF komutlarını veremeyeceği anlamına gelir. Bu özelliğin yalnızca Steam sohbeti için geçerli olduğunu unutmayın. **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** ve etkileşimli konsol, bu özellik ayarlanmamış olsa bile `Owner` komutlarını yürütmenize izin verecektir.

---

### `SteamProtocols`

Varsayılan değeri `7` olan bayt işaretleri türü. Bu özellik, ASF'nin Steam sunucularına bağlanırken kullanacağı Steam protokollerini tanımlar ve aşağıdaki gibi tanımlanır:

| Değer | İsim      | Açıklama                                                                                    |
| ----- | --------- | ------------------------------------------------------------------------------------------- |
| 0     | Hiçbiri   | Protokol yok                                                                                |
| 1     | TCP       | **[İletim Kontrol Protokolü](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)** |
| 2     | UDP       | **[Kullanıcı Datagram Protokolü](https://en.wikipedia.org/wiki/User_Datagram_Protocol)**    |
| 4     | WebSocket | **[WebSocket](https://en.wikipedia.org/wiki/WebSocket)**                                    |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[json mapping](#json-mapping)** if you'd like to learn more. Bayraklardan hiçbirinin etkinleştirilmemesi `None` seçeneğiyle sonuçlanır ve bu seçenek kendi başına geçersizdir.

Varsayılan olarak ASF, kesintiler ve diğer benzer Steam sorunlarıyla mücadele etmek için mevcut tüm Steam protokollerini kullanacaktır. Genellikle, ASF'yi yalnızca bir veya iki belirli protokol kullanacak şekilde sınırlamak istiyorsanız, bu özelliği değiştirmek istersiniz. Such measures could be needed in various situations, for example if you've blocked UDP traffic on your firewall and you want to ensure only TCP traffic goes through, or if you're using `WebProxy` and want to use it also for Steam client connection, as only `WebSocket` protocol supports that.

Bu özelliği düzenlemek için bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `UpdateChannel`

Varsayılan değeri `1` olan `byte` türü. Bu özellik, otomatik güncellemeler (eğer `UpdatePeriod` `0`'dan büyükse) veya güncelleme bildirimleri (aksi takdirde) için kullanılan güncelleme kanalını tanımlar. Currently ASF supports three update channels - `0` which is called `None`, `1`, which is called `Stable`, and `2`, which is called `PreRelease`. `Stable` kanalı, kullanıcıların çoğunluğu tarafından kullanılması gereken varsayılan yayın kanalıdır. `PreRelease` channel in addition to `Stable` releases, also includes **pre-releases** dedicated for advanced users and other developers in order to test new features, confirm bugfixes or give feedback about planned enhancements. **PreRelease versions often contain unpatched bugs, work-in-progress features or rewritten implementations**. If you don't consider yourself advanced user, please stay with default `1` (`Stable`) update channel. `PreRelease` channel is dedicated to users who know how to report bugs, deal with issues and give feedback - no technical support will be given. Daha fazla bilgi edinmek isterseniz ASF **[sürüm döngüsüne](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)** göz atın. Tüm sürüm kontrollerini tamamen kaldırmak istiyorsanız, `UpdateChannel` öğesini `0` (`Yok`) olarak da ayarlayabilirsiniz. `UpdateChannel` öğesini `0` olarak ayarlamak, `update` komutu da dahil olmak üzere güncellemelerle ilgili tüm işlevleri tamamen devre dışı bırakacaktır. `Yok` kanalının kullanılması, kendinizi her türlü soruna maruz bırakmanız nedeniyle **kesinlikle önerilmez** (aşağıdaki `UpdatePeriod` açıklamasında belirtilmiştir).

**Ne yaptığınızı bilmiyorsanız**, **şiddetle** tavsiye ederiz. varsayılan olarak tutmak için.

---

### `UpdatePeriod`

Varsayılan değeri `24` olan `byte` türü. Bu özellik, ASF'nin otomatik güncellemeleri ne sıklıkta kontrol etmesi gerektiğini tanımlar. Güncellemeler yalnızca yeni özellikler ve kritik güvenlik yamaları almak için değil, aynı zamanda hata düzeltmeleri, performans geliştirmeleri, kararlılık iyileştirmeleri ve daha fazlasını almak için de çok önemlidir. `0`'dan büyük bir değer ayarlandığında, yeni bir güncelleme mevcut olduğunda ASF otomatik olarak kendini indirecek, değiştirecek ve yeniden başlatacaktır ( `AutoRestart` izin veriyorsa). Bunu başarmak için ASF, her `UpdatePeriod` saatte bir GitHub depomuzda yeni bir güncelleme olup olmadığını kontrol edecektir. `0` değeri otomatik güncellemeleri devre dışı bırakır, ancak yine de `update` komutunu manuel olarak çalıştırmanıza izin verir. `UpdatePeriod`'in takip etmesi gereken uygun `UpdateChannel`'ı ayarlamakla da ilgilenebilirsiniz.

ASF'nin güncelleme işlemi, ASF'nin kullandığı tüm klasör yapısının güncellenmesini içerir, ancak `config` dizininde bulunan kendi yapılandırma dosyalarınıza veya veritabanlarınıza dokunmadan - bu, dizindeki ASF ile ilgili olmayan tüm ekstra dosyaların güncelleme sırasında kaybolabileceği anlamına gelir. `24` varsayılan değeri, gereksiz kontroller ve yeterince yeni olan ASF arasında iyi bir denge sağlar.

Bu özelliği devre dışı bırakmak için **güçlü** bir nedeniniz olmadıkça, makul bir `UpdatePeriod` içinde otomatik güncellemeleri **kendi iyiliğiniz için** etkin tutmalısınız. Bunun nedeni yalnızca en son kararlı ASF sürümünden başka bir şeyi desteklemememiz değil, aynı zamanda **güvenlik garantimizi yalnızca en son sürüm için vermemizdir**. Eski bir ASF sürümü kullanıyorsanız, kendinizi küçük hatalardan, bozuk işlevlerden, **[kalıcı Steam hesabı askıya almalarıyla](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#did-somebody-get-banned-for-it)** biten her türlü soruna maruz bırakıyorsunuz, bu nedenle **şiddetle tavsiye ediyoruz** Kendi iyiliğiniz için ASF sürümünüzün her zaman güncel olduğundan emin olun. Otomatik güncellemeler, sorunlu kodu daha da kötüleşmeden önce devre dışı bırakarak veya yamalayarak her türlü soruna hızlı bir şekilde yanıt vermemizi sağlar - bundan vazgeçerseniz, tüm güvenlik garantilerimizi kaybedersiniz ve çalıştırmanın sonuçlarına katlanma riskini alırsınız. yalnızca Steam ağına değil, aynı zamanda (tanım gereği) kendi Steam hesabınıza da zarar verebilecek potansiyel.

---

### `WebLimiterDelay`

Varsayılan değeri `300` olan `ushort` türü. Bu özellik, milisaniye cinsinden, aynı Steam web hizmetine iki ardışık istek gönderme arasındaki minimum gecikme miktarını tanımlar. Bu tür bir gecikme, Steam'in dahili olarak kullandığı **[AkamaiGhost](https://www.akamai.com)** hizmetinin, belirli bir zaman dilimi içinde gönderilen küresel istek sayısına dayalı olarak hız sınırlaması içermesi nedeniyle gereklidir. Normal koşullarda akamai bloğuna ulaşmak oldukça zordur, ancak çok büyük bir iş yükü ve devam eden büyük bir istek kuyruğu altında, çok kısa bir süre içinde çok fazla istek göndermeye devam edersek tetiklemek mümkündür.

Varsayılan değer, ASF'nin özellikle `steamcommunity.com`, `api.steampowered.com` ve `store.steampowered.com` gibi hizmetlere Steam web hizmetlerine erişen tek araç olduğu varsayımına göre ayarlanmıştır. Aynı web hizmetlerine istek gönderen başka araçlar kullanıyorsanız, aracınızın `WebLimiterDelay` benzeri bir işlevsellik içerdiğinden ve her ikisini de varsayılan değerin iki katına, yani `600` olarak ayarladığınızdan emin olmalısınız. Bu, hiçbir koşulda 300 ms'de 1'den fazla istek göndermeyi aşmayacağınızı garanti eder.

Genel olarak, `WebLimiterDelay` değerinin varsayılan değerin altına düşürülmesi, bazıları kalıcı olabilen çeşitli IP ile ilgili engellemelere yol açabileceğinden **kesinlikle önerilmez**. Varsayılan değer, sunucuda tek bir ASF örneğini çalıştırmak ve orijinal Steam istemcisiyle birlikte normal senaryoda ASF kullanmak için yeterince iyidir. Kısacası, tek bir IP'den tek bir Steam alan adına gönderilen tüm isteklerin genel sayısı, 300 ms'de 1 isteği asla geçmemelidir. Çoğu kullanım için doğru olmalı ve yalnızca (asla azaltmamalısınız) artırmalısınız.

Bu özelliği düzenlemek için bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `WebProxy`

Varsayılan değeri `null` olan `string` türü. This property defines a web proxy address that will be used for internal http-related communication, especially to services such as `github.com`, `api.steampowered.com`, `steamcommunity.com` and `store.steampowered.com`. It applies to general (non-bot specific) communication, as well as bot-specific communication if their equivalent `WebProxy` configuration property is not set. Proxying ASF requests could be exceptionally useful for bypassing various kinds of firewalls, especially the great firewall in China.

Bu özellik uri dizesi olarak tanımlanır:

> Bir URI dizesi, bir şemadan (desteklenir: http/https/socks4/socks4a/socks5), bir ana bilgisayardan ve isteğe bağlı bir bağlantı noktasından oluşur. Tam bir uri dizesine örnek olarak `"http://contoso.com:8080"` verilebilir.

Proxy'niz kullanıcı kimlik doğrulaması gerektiriyorsa, `WebProxyUsername` ve/veya `WebProxyPassword` öğelerini de ayarlamanız gerekir. Böyle bir ihtiyaç yoksa, yalnızca bu özelliğin ayarlanması yeterlidir.

If you require proxying internal Steam network communication (CMs) as well, then you should ensure to configure **[`SteamProtocols`](#steamprotocols)** bot's property to a value that allows only websocket transport, i.e. a value of `4`, as only websockets are supported for proxying.

Bu özelliği düzenlemek için bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `WebProxyPassword`

Varsayılan değeri `null` olan `string` türü. Bu özellik, proxy işlevselliği sağlayan hedef bir `WebProxy` makinesi tarafından desteklenen temel, sindirim, NTLM ve Kerberos kimlik doğrulamasında kullanılan parola alanını tanımlar. Proxy'niz kullanıcı kimlik bilgileri gerektirmiyorsa, buraya herhangi bir şey girmenize gerek yoktur. Bu seçeneğin kullanılması, yalnızca `WebProxy` de kullanılıyorsa anlamlıdır, aksi takdirde hiçbir etkisi yoktur.

Bu özelliği düzenlemek için bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `WebProxyUsername`

Varsayılan değeri `null` olan `string` türü. Bu özellik, proxy işlevselliği sağlayan hedef bir `WebProxy` makinesi tarafından desteklenen temel, sindirim, NTLM ve Kerberos kimlik doğrulamasında kullanılan kullanıcı adı alanını tanımlar. Proxy'niz kullanıcı kimlik bilgileri gerektirmiyorsa, buraya herhangi bir şey girmenize gerek yoktur. Bu seçeneğin kullanılması, yalnızca `WebProxy` de kullanılıyorsa anlamlıdır, aksi takdirde hiçbir etkisi yoktur.

Bu özelliği düzenlemek için bir nedeniniz yoksa, varsayılan ayarda bırakmalısınız.

---

## Bot config

Zaten bilmeniz gerektiği gibi, her botun aşağıdaki örnek JSON yapısına dayalı kendi yapılandırması olmalıdır. Botunuza nasıl isim vermek istediğinize karar vererek başlayın (örneğin `1.json`, `main.json`, `primary.json` veya `AnythingElse.json`) ve yapılandırmaya geçin.

**Uyarı:** Bot `ASF` olarak adlandırılamaz (bu anahtar kelime genel yapılandırma için ayrılmıştır), ASF ayrıca nokta ile başlayan tüm yapılandırma dosyalarını yok sayacaktır.

Bot yapılandırması aşağıdaki yapıya sahiptir:

```json
{
    "AcceptGifts": false,
    "BotBehaviour": 0,
    "CompleteTypesToSend": [],
    "CustomGamePlayedWhileFarming": null,
    "CustomGamePlayedWhileIdle": null,
    "Enabled": false,
    "FarmingOrders": [],
    "FarmingPreferences": 0,
    "GamesPlayedWhileIdle": [],
    "HoursUntilCardDrops": 3,
    "LootableTypes": [1, 3, 5],
    "MatchableTypes": [5],
    "OnlineFlags": 0,
    "OnlinePreferences": 0,
    "OnlineStatus": 1,
    "PasswordFormat": 0,
    "RedeemingPreferences": 0,
    "RemoteCommunication": 3,
    "SendTradePeriod": 0,
    "SteamLogin": null,
    "SteamMasterClanID": 0,
    "SteamParentalCode": null,
    "SteamPassword": null,
    "SteamTradeToken": null,
    "SteamUserPermissions": {},
    "TradeCheckPeriod": 60,
    "TradingPreferences": 0,
    "TransferableTypes": [1, 3, 5],
    "UseLoginKeys": true,
    "UserInterfaceMode": 0,
    "WebProxy": null,
    "WebProxyPassword": null,
    "WebProxyUsername": null
}
```

---

Tüm seçenekler aşağıda açıklanmıştır:

### `AcceptGifts`

`bool` type with default value of `false`. Etkinleştirildiğinde, ASF bota gönderilen tüm buhar hediyelerini (cüzdan hediye kartları dahil) otomatik olarak kabul edecek ve kullanacaktır. Bu, `SteamUserPermissions` içinde tanımlananlar dışındaki kullanıcılardan gönderilen hediyeleri de içerir. E-posta adresine gönderilen hediyelerin doğrudan istemciye iletilmediğini unutmayın, bu nedenle ASF bunları sizin yardımınız olmadan kabul etmeyecektir.

Bu seçenek yalnızca alt hesaplar için önerilir, çünkü büyük olasılıkla birincil hesabınıza gönderilen tüm hediyeleri otomatik olarak kullanmak istemezsiniz. Bu özelliğin etkinleştirilip etkinleştirilmemesi konusunda emin değilseniz, varsayılan `false` değeriyle bırakın.

---

### `BotBehaviour`

Varsayılan değeri `0` olan `byte flags` türü. Bu özellik, çeşitli olaylar sırasında ASF'ye benzer bot davranışını tanımlar ve aşağıdaki gibi tanımlanır:

| Değer | İsim                          | Açıklama                                                                                                                     |
| ----- | ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| 0     | Hiçbiri                       | No special bot behaviour, sane default settings                                                                              |
| 1     | RejectInvalidFriendInvites    | ASF'nin geçersiz arkadaş davetlerini yoksaymak yerine reddetmesine neden olur                                                |
| 2     | RejectInvalidTrades           | ASF'nin geçersiz ticaret tekliflerini yoksaymak yerine reddetmesine neden olur                                               |
| 4     | RejectInvalidGroupInvites     | ASF'nin geçersiz grup davetlerini yoksaymak yerine reddetmesine neden olur                                                   |
| 8     | DismissInventoryNotifications | ASF'nin tüm envanter bildirimlerini otomatik olarak kapatmasına neden olur                                                   |
| 16    | MarkReceivedMessagesAsRead    | ASF'nin alınan tüm mesajları otomatik olarak okundu olarak işaretlemesine neden olur                                         |
| 32    | MarkBotMessagesAsRead         | ASF'nin diğer ASF botlarından gelen mesajları (aynı örnekte çalışan) otomatik olarak okundu olarak işaretlemesine neden olur |
| 64    | DisableIncomingTradesParsing  | Will cause ASF to never process incoming trade offers                                                                        |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[json mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

In general you want to modify this property if you expect to change various automation settings related to bot's activity. The default settings involve ASF running in non-invasive mode, which enables only beneficial automation that does not go against user's will.

Geçersiz arkadaş daveti, `FamilySharing` iznine ( `SteamUserPermissions` içinde tanımlanmıştır) veya daha üst düzey bir kullanıcıdan gelmeyen bir davettir. Normal moddaki ASF, beklediğiniz gibi bunları yok sayarak size bunları kabul edip etmemekte serbest seçim bırakır. `RejectInvalidFriendInvites`, bu davetlerin otomatik olarak reddedilmesine neden olur ve bu da diğer kişilerin sizi arkadaş listelerine ekleme seçeneğini pratik olarak devre dışı bırakır (çünkü ASF, `SteamUserPermissions` içinde tanımlanan kişiler dışında tüm bu istekleri reddeder). Tüm arkadaş davetlerini açıkça reddetmek istemiyorsanız, bu seçeneği etkinleştirmemelisiniz.

Geçersiz ticaret teklifi, yerleşik ASF modülü aracılığıyla kabul edilmeyen bir tekliftir. Bu konuyla ilgili daha fazla bilgi, ASF'nin hangi tür ticareti otomatik olarak kabul etmeye istekli olduğunu açıkça tanımlayan **[ticaret](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** bölümünde bulunabilir. Geçerli işlemler ayrıca diğer ayarlar, özellikle `TradingPreferences` tarafından da tanımlanır. `RejectInvalidTrades`, geçersiz tüm ticaret tekliflerinin yoksayılması yerine reddedilmesine neden olur. ASF tarafından otomatik olarak kabul edilmeyen tüm ticaret tekliflerini açıkça reddetmek istemiyorsanız, bu seçeneği etkinleştirmemelisiniz.

Geçersiz grup daveti, `SteamMasterClanID` grubundan gelmeyen bir davettir. Normal moddaki ASF, beklediğiniz gibi bu grup davetlerini yok sayarak belirli bir Steam grubuna katılıp katılmamaya kendiniz karar vermenizi sağlar. `RejectInvalidGroupInvites`, tüm bu grup davetlerinin otomatik olarak reddedilmesine neden olarak sizi `SteamMasterClanID` dışındaki herhangi bir gruba davet etmeyi imkansız hale getirir. Tüm grup davetlerini açıkça reddetmek istemiyorsanız, bu seçeneği etkinleştirmemelisiniz.

Yeni öğeler almakla ilgili sürekli Steam bildirimlerinden rahatsız olmaya başladığınızda `DismissInventoryNotifications` son derece yararlıdır. ASF, bildirimin kendisinden kurtulamaz, çünkü bu, Steam istemcinize yerleşiktir, ancak bildirim aldıktan sonra otomatik olarak temizleyebilir, bu da artık "envanterdeki yeni öğeler" bildirimini ortadan kaldırmayacaktır. Alınan tüm öğeleri (özellikle ASF ile yetiştirilen kartları) kendiniz değerlendirmeyi düşünüyorsanız, doğal olarak bu seçeneği etkinleştirmemelisiniz. Çıldırmaya başladığınızda, bunun bir seçenek olarak sunulduğunu unutmayın.

`MarkReceivedMessagesAsRead`, hem özel hem de grup olmak üzere, ASF'nin çalıştığı hesap tarafından alınan **tüm** mesajları otomatik olarak okundu olarak işaretler. Bu genellikle yalnızca alt hesaplar tarafından, örneğin ASF komutlarını yürütürken sizden gelen "yeni mesaj" bildirimini temizlemek için kullanılmalıdır. Bu seçeneği birincil hesaplar için önermiyoruz, **ASF açık bırakılarak kapatıldığı varsayılarak çevrimdışı olduğunuz sırada gerçekleşenler de dahil olmak üzere** her türlü yeni mesaj bildiriminden kendinizi kesmek istemediğiniz sürece.

`MarkBotMessagesAsRead`, yalnızca bot mesajlarını okundu olarak işaretler. However, keep in mind that when using that option on group chats with your bots and other people, Steam implementation of acknowledging chat message **also** acknowledges all messages that happened **before** the one you decided to mark, so if by any chance you don't want to miss unrelated message that happened in-between, you typically want to avoid using this feature. Obviously, it's also risky when you have multiple primary accounts (e.g. from different users) running in the same ASF instance, as you can also miss their normal out-of-ASF messages.

`DisableIncomingTradesParsing` will stop ASF from parsing incoming trade offers, this means that whole trading functionality related to that will be non-operative. Since ASF works in the least invasive mode by default, accepting only trade offers from `Master` users and above, never touching other trade offers - incoming trades parsing is enabled by default. This setting makes the most sense for people that would like to ensure no additional requests/overhead related to trades parsing, disabling whole logic in process, for example because they know that their bots will never receive master trade requests, and therefore do not require ASF participating in their trading activity at all. Keep in mind that having this option specified will disable all other options that depend on incoming trades as well, such as `AcceptDonations` or `SteamTradeMatcher` - custom plugins will also be unable to process incoming trade offers in usual way.

If you're unsure how to configure this option, it's best to leave it at default.

---

### `CompleteTypesToSend`

Boş olması varsayılan değeri olan bir `ImmutableHashSet<byte>` türüdür. ASF, burada belirtilen bir öğe türü kümesini tamamladığında, otomatik olarak bitmiş tüm setlerle birlikte `Master` iznine sahip kullanıcıya bir buhar ticareti gönderebilir, bu da örneğin STM eşleştirmesi için belirli bir bot hesabını kullanırken, bitmiş setleri başka bir hesaba taşımak istediğinizde çok kullanışlıdır. Bu seçenek `loot` komutuyla aynı şekilde çalışır, bu nedenle `Master` iznine sahip bir kullanıcıyı gerektirdiğini unutmayın, ayrıca geçerli bir `SteamTradeToken`'a ihtiyacınız olabilir ve ilk etapta ticaret yapmaya uygun bir hesap kullanmanız gerekebilir.

Bugün itibariyle, bu ayarda aşağıdaki öğe türleri desteklenmektedir:

| Değer | İsim            | Açıklama                                                 |
| ----- | --------------- | -------------------------------------------------------- |
| 3     | FoilTradingCard | `TradingCard`'ın folyo çeşidi                            |
| 5     | TradingCard     | Rozet yapmak için kullanılan Steam kartı (folyo olmayan) |

`byte flags` türünde ve varsayılan değeri `0` olan bu özellik, ASF'nin ticaret yaparkenki davranışını tanımlar ve aşağıda belirtilmiştir:

Bu seçeneği kullanmanın ek yükü nedeniyle, yalnızca kendi başlarına setleri bitirme şansı gerçek olan bot hesaplarında kullanılması önerilir - örneğin, zaten `FarmingPreferences` içinde `SendOnFarmingFinished` tercihi, `SendTradePeriod` veya `loot` komutunu normal olarak kullanıyorsanız etkinleştirmenin bir anlamı yoktur.

Bu seçeneği nasıl yapılandıracağınızdan emin değilseniz, en iyisi varsayılan olarak bırakmaktır.

---

### `CustomGamePlayedWhileFarming`

`string` türünde ve varsayılan değeri `null` olan bir özellik. ASF, farm yaparken kendisini şu an farm yapılan oyun yerine "Steam dışı oyun oynanıyor: `CustomGamePlayedWhileFarming`" olarak gösterebilir. Bu, arkadaşlarınıza farm yaptığınızı bildirmenin bir yolu olabilir ancak `Çevrimdışı` `OnlineStatus` kullanmak istemiyorsanız. Lütfen ASF'nin Steam ağı tarafından görüntüleme sırasını garanti edemeyeceğini unutmayın, bu nedenle bu yalnızca düzgün veya düzgün olmayacak bir öneridir. Özellikle, ASF `32` slotun tamamını saatlerin artırılması gereken oyunlarla doldurursa, özel isim `Complex` farm algoritmasında görüntülenmeyecektir. Varsayılan değeri `null` bu özelliği devre dışı bırakır.

ASF, metninizde isteğe bağlı olarak kullanabileceğiniz birkaç özel değişken sağlar. `{0}`, ASF tarafından şu anda farm yapılan oyun(lar)ın `AppID`si ile, `{1}` ise şu anda farm yapılan oyun(lar)ın `GameName`i ile değiştirilecektir.

---

### `CustomGamePlayedWhileIdle`

Varsayılan değeri `null` olan `string` türüdür. `CustomGamePlayedWhileFarming`'e benzer, ancak ASF'nin yapacak hiçbir şeyi olmadığı (hesap tamamen farm yapıldığı için) durum içindir. Lütfen ASF'nin Steam ağının gerçek görüntülenme sırasını garanti edemeyeceğini unutmayın, bu nedenle bu yalnızca düzgün görüntülenebilecek veya görüntülenmeyebilecek bir öneridir. `GamesPlayedWhileIdle`'ı bu seçenekle birlikte kullanıyorsanız, `GamesPlayedWhileIdle`'ın öncelikli olduğunu unutmayın, bu nedenle `31`'den fazlasını beyan edemezsiniz, aksi takdirde `CustomGamePlayedWhileIdle`, özel ad için yuva kaplayamaz. `Null` varsayılan değeri bu özelliği devre dışı bırakır.

---

### `Etkin`

Varsayılan değeri `false` olan `bool` türü. Bu özellik, botun etkin olup olmadığını tanımlar. Etkinleştirilmiş bot örneği (`true`) ASF çalıştırıldığında otomatik olarak başlayacak, devre dışı bırakılmış bot örneği (`false`) ise manuel olarak başlatılması gerekecektir. Varsayılan olarak her bot devre dışıdır, bu nedenle muhtemelen otomatik olarak başlatılması gereken tüm botlarınız için bu özelliği `true` olarak değiştirmek istersiniz.

---

### `FarmingOrders`

Boş olması varsayılan değeri olan bir `ImmutableList<byte>` türüdür. Bu özellik, ASF tarafından belirli bir bot hesabı için kullanılan **tercih edilen** farm sırasını tanımlar. Şu anda aşağıdaki farm siparişleri mevcuttur:

| Değer | İsim                      | Açıklama                                                                               |
| ----- | ------------------------- | -------------------------------------------------------------------------------------- |
| 0     | Unordered                 | Sıralama yok, CPU performansını biraz artırıyor                                        |
| 1     | AppIDsAscending           | Önce en düşük `appIDs`'ye sahip oyunları farm yapmayı dene                             |
| 2     | AppIDsDescending          | Önce en yüksek `appIDs`'ye sahip oyunları farm yapmayı dene                            |
| 3     | CardDropsAscending        | Önce en düşük kart düşüş sayısı kalan oyunları farm yapmayı dene                       |
| 4     | CardDropsDescending       | Önce en yüksek kart düşüş sayısı kalan oyunları farm yapmayı dene                      |
| 5     | HoursAscending            | Önce en düşük oyun saati olan oyunları farm yapmayı dene                               |
| 6     | HoursDescending           | Önce en yüksek oyun saati olan oyunları farm yapmayı dene                              |
| 7     | NamesAscending            | A'dan başlayarak oyunları alfabetik sıraya göre farm yapmayı dene                      |
| 8     | NamesDescending           | Z'den başlayarak oyunları ters alfabetik sıraya göre farm yapmayı dene                 |
| 9     | Random                    | Oyunları tamamen rastgele sırada farm yapmayı dene (programın her çalışmasında farklı) |
| 10    | BadgeLevelsAscending      | Önce en düşük rozet seviyesine sahip oyunları farm yapmayı dene                        |
| 11    | BadgeLevelsDescending     | Önce en yüksek rozet seviyesine sahip oyunları farm yapmayı dene                       |
| 12    | RedeemDateTimesAscending  | Hesabımızdaki en eski oyunları önce farm yapmayı dene                                  |
| 13    | RedeemDateTimesDescending | Hesabımızdaki en yeni oyunları önce farm yapmayı dene                                  |
| 14    | MarketableAscending       | Önce pazarlanamaz kart düşüşleri olan oyunları farm yapmayı dene                       |
| 15    | MarketableDescending      | Önce pazarlanabilir kart düşüşleri olan oyunları farm yapmayı dene                     |

Bu özellik bir dizi olduğundan, sabit sıranızda birkaç farklı ayar kullanmanıza olanak tanır. Örneğin, önce pazarlanabilir oyunlara, ardından en yüksek rozet seviyesine sahip olanlara ve son olarak alfabetik olarak sıralamak için `15`, `11` ve `7` değerlerini ekleyebilirsiniz. Tahmin edebileceğiniz gibi, sıra gerçekten önemlidir, çünkü ters sıra (`7`, `11` ve `15`) tamamen farklı bir şey başarır (oyunları önce alfabetik olarak sıralar ve oyun adları benzersiz olduğundan diğer ikisi etkili bir şekilde işe yaramaz). Çoğu insan muhtemelen hepsinden sadece bir sipariş kullanacaktır, ancak isterseniz ekstra parametrelerle daha fazla sıralama yapabilirsiniz.

Ayrıca yukarıdaki tüm açıklamalarda "dene" kelimesine de dikkat edin - gerçek ASF sırası, seçilen **[kart farm algoritmasından](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** büyük ölçüde etkilenir ve sıralama yalnızca ASF'nin performans açısından aynı olduğunu düşündüğü sonuçları etkiler. Örneğin, `Basit` algoritmada, seçilen `FarmingOrders` mevcut farm oturumunda tamamen dikkate alınmalıdır (her oyun aynı performans değerine sahip olduğundan), `Karmaşık` algoritmada ise gerçek sıra önce saatlerden etkilenir ve ardından seçilen `FarmingOrders`'a göre sıralanır. Bu, mevcut oyun süresine sahip oyunların diğerlerine göre önceliği olacağından farklı sonuçlara yol açacaktır, bu nedenle ASF, öncelikle zaten gerekli `HoursUntilCardDrops`ı geçen oyunları tercih edecek ve ancak o zaman bu oyunları seçtiğiniz `FarmingOrders`'a göre daha fazla sıralayacaktır. Aynı şekilde, ASF zaten yükseltilmiş oyunları tükettiğinde, kalan kuyruğu önce saatlere göre sıralayacaktır (bu, kalan oyunlardan herhangi birini `HoursUntilCardDrops`'a yükseltmek için gereken süreyi azaltacaktır). Bu nedenle, bu yapılandırma özelliği, ASF'nin performansı olumsuz etkilemediği sürece (bu durumda, ASF her zaman `FarmingOrders` yerine farm performansını tercih edecektir) saygı göstermeye çalışacağı bir **öneridir**.

ASF'de, `fq` **[komutları](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** aracılığıyla erişilebilen bir farming öncelik kuyruğu bulunur. Bu özellik kullanıldığında, gerçek farming sırası öncelikle performansa, sonra farming kuyruğuna ve son olarak `FarmingOrders`'a göre sıralanır.

---

### `FarmingPreferences`

Varsayılan değeri `0` olan `byte flags` türündedir. Bu özellik, ASF'nin tarım (farming) ile ilgili davranışını tanımlar ve aşağıdaki gibi tanımlanmıştır:

| Değer | İsim                      |
| ----- | ------------------------- |
| 0     | Hiçbiri                   |
| 1     | FarmingPausedByDefault    |
| 2     | ShutdownOnFarmingFinished |
| 4     | SendOnFarmingFinished     |
| 8     | FarmPriorityQueueOnly     |
| 16    | SkipRefundableGames       |
| 32    | SkipUnplayedGames         |
| 64    | EnableRiskyCardsDiscovery |
| 256   | AutoUnpackBoosterPacks    |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[json mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

Tüm seçenekler aşağıda açıklanmıştır.

`FarmingPausedByDefault` `CardsFarmer` modülünün başlangıç durumunu tanımlar. Normalde bot, `Enabled` veya `start` komutu nedeniyle başlatıldığında otomatik olarak tarıma başlar. `FarmingPausedByDefault` kullanarak otomatik tarım sürecini manuel olarak `resume` etmek isterseniz, örneğin sürekli `play` kullanmak ve otomatik `CardsFarmer` modülünü hiç kullanmamak istiyorsanız bu seçeneği kullanabilirsiniz - bu, **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** ile aynı şekilde çalışır.

`ShutdownOnFarmingFinished` tarım tamamlandığında botun kapanmasını sağlar. Normalde ASF, işlem aktif olduğu sürece bir hesabı "işgal" eder. Hesap tarım işlemi tamamlandığında, ASF belirli aralıklarla (her `IdleFarmingPeriod` saatte bir) bu hesabı kontrol eder ve yeni kart oyunları eklenmişse tarıma devam eder. This is useful for majority of people, as ASF can automatically resume farming when needed. Ancak, tarım işlemi tamamlandığında işlemi durdurmak isteyebilirsiniz. Bu bayrağı kullanarak bunu gerçekleştirebilirsiniz. Etkinleştirildiğinde, ASF hesap tam olarak tarandıktan sonra oturumu kapatır ve bu hesap artık periyodik olarak kontrol edilmez veya işgal edilmez. ASF'nin bot örneğinde sürekli çalışmasını mı yoksa tarım işlemi tamamlandığında durmasını mı istediğinize kendiniz karar vermelisiniz.

Bu seçenek, `ShutdownIfPossible` ile birleştirildiğinde en anlamlı olur, böylece tüm hesaplar durdurulduğunda ASF de kapanır ve bilgisayarınızın dinlenmesini sağlar, bu da uyku veya kapanma gibi diğer eylemleri planlamanıza olanak tanır.

`SendOnFarmingFinished` tamamlanan tarım sürecinden sonra elde edilen her şeyi `Master` yetkisine sahip kullanıcıya otomatik olarak göndermenizi sağlar, bu da ticaretle uğraşmak istemiyorsanız oldukça kullanışlıdır. Bu seçenek, `loot` komutuyla aynı şekilde çalışır, bu nedenle `Master` yetkisine sahip bir kullanıcı, geçerli bir `SteamTradeToken` ve ticaret yapmaya uygun bir hesap gerektirir. Bu özellik etkin olduğunda, tarım tamamlandıktan sonra `loot` işlemi başlatılacağı gibi, yeni öğeler bildirimi alındığında ve her ticaretin ardından da `loot` işlemi başlatılacaktır. Bu özellik özellikle başkalarından alınan öğeleri hesabınıza yönlendirmek için yararlıdır. Typically you'll want to use **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** together with this feature, although it's not a requirement if you intend to handle 2FA confirmations manually in timely fashion.

`FarmPriorityQueueOnly`, ASF'nin sadece `fq` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** ile öncelikli tarım sırasına eklediğiniz uygulamaları otomatik olarak taraması gerektiğini tanımlar. Bu seçenek etkinleştirildiğinde, ASF listedeki olmayan tüm `appID`leri atlar ve böylece otomatik ASF tarımı için oyunları seçmenizi sağlar. Eğer sıraya hiç oyun eklemediyseniz, ASF hesabınızda taranacak hiçbir şey yokmuş gibi davranır.

`SkipRefundableGames`, ASF'nin geri ödenebilir oyunları otomatik taramadan atlayıp atlamaması gerektiğini tanımlar. Geri ödenebilir oyun, son 2 hafta içinde Steam Mağazası'ndan satın aldığınız ve henüz 2 saatten fazla oynamadığınız bir oyundur. Varsayılan olarak, ASF Steam iade politikasını tamamen görmezden gelir ve her şeyi tarar, bu da çoğu kişinin beklediği gibi olur. Ancak, geri ödenebilir oyunlarınızı kendiniz değerlendirip gerekirse iade etmek istiyorsanız ve ASF'nin oyun süresini olumsuz etkilemesini istemiyorsanız bu bayrağı kullanabilirsiniz. Bu seçeneği etkinleştirirseniz, Steam Mağazası'ndan satın aldığınız oyunlar, satın alma tarihinden itibaren 14 güne kadar ASF tarafından taranmaz ve hesabınızda başka oyun yoksa taranacak hiçbir şey yokmuş gibi görünür.

`SkipUnplayedGames`, ASF'nin henüz başlatmadığınız oyunları atlayıp atlamaması gerektiğini tanımlar. Bu bağlamda oynanmamış bir oyun, Steam'de kaydedilmiş tam olarak hiç oyun süresine sahip olmayan bir oyundur. Bu bayrağı kullanırsanız, Steam bu oyunlar için herhangi bir oyun süresi kaydedene kadar bu oyunlar atlanır. Bu, ASF'nin hangi oyunları tarayabileceğini daha iyi kontrol etmenizi sağlar ve denemediğiniz oyunları taramadan atlar, böylece Steam'in seçilen bazı özelliklerini daha kullanışlı hale getirir.

`EnableRiskyCardsDiscovery`, ASF'nin rozet sayfalarını yükleyemediği ve bu nedenle tarama için kullanılabilir oyunları bulamadığı durumlarda devreye giren ek bir geri dönüş mekanizmasını etkinleştirir. Özellikle, büyük miktarda kart düşüşüne sahip bazı hesaplar rozet sayfalarını yükleyemediği için tarama işlemi mümkün olmaz. For those handful cases, this option allows alternative algorithm to be used, which uses a combination of boosters possible to craft and booster packs the account is eligible for, in order to find potentially available games to idle, then spending excessive amount of resources for verifying and fetching required information, and attempting to start the process of farming on limited amount of data and information in order to eventually reach a situation when badge page loads and we'll be able to use normal approach. Bu geri dönüş mekanizması kullanıldığında, ASF sadece sınırlı veriyle çalışır, bu nedenle ASF'nin gerçekte olduğundan çok daha az düşüş bulması tamamen normaldir - diğer düşüşler tarama sürecinin ilerleyen aşamalarında bulunur.

Bu seçenek "riskli" olarak adlandırılmasının çok iyi bir nedeni vardır - çok yavaş ve operasyon için önemli miktarda kaynak (ağ istekleri dahil) gerektirir, bu nedenle uzun vadede etkinleştirilmesi **önerilmez**. Bu seçeneği yalnızca hesabınızın rozet sayfalarını yükleyemediğini ve ASF'nin sürekli olarak gerekli bilgileri yükleyemediğini belirlediğiniz durumlarda kullanmalısınız. Bu süreci mümkün olduğunca optimize etmek için elimizden geleni yapsak da, bu seçeneğin ters tepebileceği ve çok sayıda istek göndermek ve Steam sunucularında yük oluşturmak gibi istenmeyen sonuçlara neden olabileceği, hatta geçici veya kalıcı yasaklamalara neden olabileceği mümkündür. Bu nedenle, önceden uyarıyoruz ve bu seçeneği hiçbir garanti vermeden sunuyoruz, kullanımı tamamen sizin sorumluluğunuzdadır.

`AutoUnpackBoosterPacks` will automatically unpack all booster packs upon receiving new items notification. This will allow you to immediately acquire additional card drops, which might be wanted scenario especially in combination with other options (e.g. `SteamTradeMatcher` or `CompleteTypesToSend`).

---

### `GamesPlayedWhileIdle`

Varsayılan değeri boş olan `ImmutableHashSet<uint>` türündedir. ASF'nin tarayacak hiçbir şeyi yoksa, belirttiğiniz Steam oyunlarını (`appID`ler) oynayabilir. Bu şekilde oyunları oynamak, bu oyunların "oynanma saatlerini" artırır, ancak bunun dışında başka bir etkisi yoktur. appID`lere sahip olması gerekir. Bu özelliği`CustomGamePlayedWhileIdle`ile birleştirebilirsiniz,`CustomGamePlayedWhileIdle`'ı tek başına kullanarak herhangi bir`appID`yi oynamadan da kullanabilirsiniz. Her iki özellik de`GamesPlayedWhileIdle`'a benzersiz öncelik vererek kullanılabilir. ASF, bir bot tarama yaptığında bu özelliği geçersiz kılar ve yalnızca tarama yapmadığında etkili olur. Örneğin, herhangi bir öğe düşüşü olmadığında saatlerinizin sayılmasını sağlamak için "AFK saati" olarak bu özelliği kullanabilirsiniz. Bu, Steam profilinizde "oyun oynuyor" görünmenize ve Steam'deki oyun saatinizi artırmanıza neden olur. Ancak, yalnızca bu özelliği kullanmak için bir oyun satın almak, Valve'in Hizmet Şartları'nı ihlal edebilir, bu nedenle dikkatli olun. This feature can be enabled at the same time with `CustomGamePlayedWhileIdle` in order to play your selected games while showing custom status in Steam network, but in this case, like in `CustomGamePlayedWhileFarming` case, the actual display order is not guaranteed. Please note that Steam allows ASF to play only up to `32` `appIDs` total, therefore you can put only as many games in this property.

---

### `HoursUntilCardDrops`

Varsayılan değeri `3` olan `byte` türündedir. This property defines if account has card drops restricted, and if yes, for how many initial hours. Restricted card drops means that account is not receiving any card drops from given game until the game is played for at least `HoursUntilCardDrops` hours. Unfortunately there is no magical way to detect that, so ASF relies on you. This property affects **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** that will be used. Setting this property properly will maximize profits and minimize time required for cards to be farmed. Remember that there is no obvious answer whether you should use one or another value, since it fully depends on your account. It seems that older accounts which never asked for refund have unrestricted card drops, so they should use a value of `0`, while new accounts and those who did ask for refund have restricted card drops with a value of `3`. This is however only theory, and should not be taken as a rule. The default value for this property was set based on "lesser evil" and majority of use cases.

---

### `LootableTypes`

`ImmutableHashSet<byte>` türünde, varsayılan değeri `1, 3, 5` olan Steam öğe türleridir. Bu özellik, ASF'nin yağmalama davranışını tanımlar - hem manuel olarak bir **[komut](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** kullanarak, hem de bir veya daha fazla yapılandırma özelliği aracılığıyla otomatik olarak. ASF, yalnızca `LootableTypes` içindeki öğelerin bir takas teklifine dahil edilmesini sağlar, bu nedenle bu özellik, size gönderilen bir takas teklifinde almak istediğiniz öğeleri seçmenizi sağlar.

| Değer | İsim                  | Açıklama                                                             |
| ----- | --------------------- | -------------------------------------------------------------------- |
| 0     | Unknown               | Aşağıdakilerden herhangi birine uymayan her tür                      |
| 1     | BoosterPack           | Bir oyundan 3 rastgele kart içeren güçlendirici paket                |
| 2     | Emoticon              | Emoticon to use in Steam Chat                                        |
| 3     | FoilTradingCard       | `TradingCard`'ın folyo varyantı                                      |
| 4     | ProfileBackground     | Steam profilinizde kullanmak için profil arka planı                  |
| 5     | TradingCard           | Rozet yapmak için kullanılan Steam takas kartı (folyo olmayan)       |
| 6     | SteamGems             | Güçlendiriciler yapmak için kullanılan Steam taşları, çuvallar dahil |
| 7     | SaleItem              | Steam satışları sırasında verilen özel öğeler                        |
| 8     | Consumable            | Kullanıldıktan sonra kaybolan özel sarf malzemeleri                  |
| 9     | ProfileModifier       | Steam profil görünümünü değiştirebilen özel öğeler                   |
| 10    | Sticker               | Steam sohbetinde kullanılabilecek özel öğeler                        |
| 11    | ChatEffect            | Steam sohbetinde kullanılabilecek özel öğeler                        |
| 12    | MiniProfileBackground | Steam profili için özel arka plan                                    |
| 13    | AvatarProfileFrame    | Steam profili için özel avatar çerçevesi                             |
| 14    | AnimatedAvatar        | Steam profili için özel animasyonlu avatar                           |
| 15    | KeyboardSkin          | Steam deck için özel klavye görünümü                                 |
| 16    | StartupVideo          | Steam deck için özel başlangıç videosu                               |

Yukarıdaki ayarlara bakılmaksızın, ASF yalnızca **[Steam topluluk öğeleri](https://steamcommunity.com/my/inventory/#753_6)** (`appID` 753, `contextID` 6) için istek yapar, dolayısıyla tüm oyun öğeleri, hediyeler ve benzeri şeyler ticaret tekliflerinden hariç tutulur.

Varsayılan ASF ayarı, botun en yaygın kullanımına dayanmaktadır ve yalnızca booster paketleri ve ticaret kartları (foil dahil) transfer edilir. Burada tanımlanan özellik, bu davranışı ihtiyaçlarınıza göre değiştirme imkanı sunar. Ayrıca, Valve yeni Steam öğeleri yayınladığında, bu öğeler ASF tarafından `Unknown` olarak işaretlenebilir ve buraya eklenene kadar bu şekilde kalır. Bu nedenle, genellikle `Unknown` türünü `TransferableTypes` içinde dahil etmeniz önerilmez, çünkü Steam Ağı bozulursa ve tüm öğelerinizi `Unknown` olarak raporlarsa, ASF tüm envanterinizi bir ticaret teklifinde gönderebilir. Bu yüzden, her şeyi transfer etmeyi bekleseniz bile `Unknown` türünü `TransferableTypes` içinde dahil etmemek en iyi uygulamadır.

---

### `MatchableTypes`

`ImmutableHashSet<byte>` varsayılan değeri `5` olan Steam öğe türlerini içeren bir türdür. Bu özellik, `TradingPreferences` içindeki `SteamTradeMatcher` seçeneği etkinleştirildiğinde eşleştirilebilecek Steam öğe türlerini tanımlar. Türler aşağıda tanımlanmıştır:

| Değer | İsim                  | Açıklama                                                              |
| ----- | --------------------- | --------------------------------------------------------------------- |
| 0     | Unknown               | Aşağıdakilere uymayan her tür                                         |
| 1     | BoosterPack           | Bir oyundan 3 rastgele kart içeren destek paketi                      |
| 2     | Emoticon              | Emoticon to use in Steam Chat                                         |
| 3     | FoilTradingCard       | `Takas Kartı`nın folyo varyantı                                       |
| 4     | ProfileBackground     | Steam profilinde kullanılacak profil arka planı                       |
| 5     | TradingCard           | Rozet işlemek için kullanılan Steam takas kartı (folyo olmayan)       |
| 6     | SteamGems             | Destekler oluşturmak için kullanılan Steam cevherleri, çuvallar dahil |
| 7     | SaleItem              | Steam satışları sırasında verilen özel öğeler                         |
| 8     | Consumable            | Kullanıldıktan sonra kaybolan özel tüketilebilir öğeler               |
| 9     | ProfileModifier       | Steam profil görünümünü değiştirebilen özel öğeler                    |
| 10    | Sticker               | Steam sohbetinde kullanılabilecek özel öğeler                         |
| 11    | ChatEffect            | Steam sohbetinde kullanılabilecek özel öğeler                         |
| 12    | MiniProfileBackground | Steam profili için özel arka plan                                     |
| 13    | AvatarProfileFrame    | Steam profili için özel avatar çerçevesi                              |
| 14    | AnimatedAvatar        | Steam profili için özel animasyonlu avatar                            |
| 15    | KeyboardSkin          | Steam deck için özel klavye kaplaması                                 |
| 16    | StartupVideo          | Steam deck için özel başlangıç videosu                                |

Elbette, bu özellik için kullanmanız gereken türler genellikle yalnızca `2`, `3`, `4` ve `5`'i içerir, çünkü sadece bu türler STM tarafından desteklenmektedir. ASF, öğelerin nadirliğini keşfetmek için uygun bir mantık içerir, bu nedenle ASF aynı oyun ve türden, aynı nadirliği paylaşan öğeleri adil olarak kabul edecektir.

Lütfen **ASF'nin bir takas botu olmadığını** ve **piyasa fiyatıyla ilgilenmeyeceğini** unutmayın. Aynı nadirlikteki öğeleri aynı fiyatta kabul etmiyorsanız, bu seçenek sizin için DEĞİLDİR. Bu ayarı değiştirmeye karar vermeden önce bu ifadeyi anladığınızdan ve kabul ettiğinizden emin olun.

Ne yaptığınızı bilmiyorsanız, varsayılan değeri olan `5` ile bırakmalısınız.

---

### `OnlineFlags`

Varsayılan değeri `0` olan `ushort flags` türü. Bu özellik, **[`OnlineStatus`](#onlinestatus)** için ek bir çevrimiçi varlık özelliği olarak çalışır ve Steam ağına duyurulan ek çevrimiçi varlık özelliklerini belirtir. `Offline` dışındaki **[`OnlineStatus`](#onlinestatus)** gerektirir ve aşağıda tanımlanmıştır:

| Değer | İsim              | Açıklama                                      |
| ----- | ----------------- | --------------------------------------------- |
| 0     | Hiçbiri           | Özel çevrimiçi varlık bayrağı yok, varsayılan |
| 256   | ClientTypeWeb     | Client is using web interface                 |
| 512   | ClientTypeMobile  | İstemci mobil uygulamayı kullanıyor           |
| 1024  | ClientTypeTenfoot | Client is using big picture                   |
| 2048  | ClientTypeVR      | İstemci VR başlığını kullanıyor               |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[json mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

Bu özelliğin temel aldığı `EPersonaStateFlag` türü, daha fazla kullanılabilir bayrak içerir, ancak bildiğimiz kadarıyla bunların hiçbir etkisi yoktur, bu nedenle görünürlük için kesilmişlerdir.

Bu özelliği nasıl ayarlayacağınızdan emin değilseniz, varsayılan değeri `0` olarak bırakabilirsiniz.

---

### `OnlinePreferences`

`byte flags` type with default value of `0`. This property can enable some additional online features on the Steam platform, and is defined as below:

| Değer | İsim        | Açıklama                               |
| ----- | ----------- | -------------------------------------- |
| 0     | Hiçbiri     | No special online preferences, default |
| 1     | IsSteamDeck | Present itself as Steam Deck           |

Lütfen bu özelliğin `flags` alanı olduğuna dikkat edin, bu nedenle mevcut değerlerin herhangi bir kombinasyonunu seçmek mümkündür. Check out **[json mapping](#json-mapping)** if you'd like to learn more. Bayraklardan hiçbirinin etkinleştirilmemesi `None` seçeneğiyle sonuçlanır.

If you're not sure how to set this property, leave it with default value of `0`.

---

### `OnlineStatus`

Varsayılan değeri `1` olan `byte` türü. Bu özellik, botun Steam ağına giriş yaptıktan sonra duyurulacağı Steam topluluk durumunu belirtir. Şu anda aşağıdaki durumlardan birini seçebilirsiniz:

| Değer | İsim            |
| ----- | --------------- |
| 0     | Çevrimdışı      |
| 1     | Çevrimiçi       |
| 2     | Busy            |
| 3     | Away            |
| 4     | Snooze          |
| 5     | LookingToTrade  |
| 6     | Oynamak İstiyor |
| 7     | Görünmez        |

`Çevrimdışı` durumu birincil hesaplar için son derece kullanışlıdır. Bildiğiniz gibi, bir oyunu farmlamak aslında Steam durumunuzu "Oyun oynuyor: XXX" olarak gösterir, bu da arkadaşlarınızı yanıltabilir, oyunu oynadığınızı sanmalarına neden olabilir, oysa aslında sadece kart farmlıyorsunuzdur. `Çevrimdışı` durumu bu sorunu çözer - hesabınız ASF ile Steam kartlarını farmlarken asla "oyunda" olarak gösterilmeyecektir. Bu, ASF'nin düzgün çalışması için Steam Topluluğu'na giriş yapmasına gerek olmadığı için mümkündür, yani aslında bu oyunları oynuyoruz, Steam ağına bağlıyız, ancak çevrimiçi varlığımızı hiç duyurmadan. Çevrimdışı durumdayken oynanan oyunların oyun sürenize katkıda bulunacağını ve profilinizde "son zamanlarda oynandı" olarak görüneceğini unutmayın.

Ek olarak, ASF çalışırken bildirimleri ve okunmamış mesajları almak istiyorsanız, ancak aynı zamanda Steam istemcisini açık tutmak istemiyorsanız, bu özellik de önemlidir. Bu, ASF'nin aslında bir Steam istemcisi gibi davrandığı için, ve istemese de, Steam tüm bu mesajları ve diğer olayları ona yayınlar. Hem ASF hem de kendi Steam istemciniz çalışıyorsa bu sorun değildir, çünkü her iki istemci de tam olarak aynı olayları alır. Ancak, sadece ASF çalışıyorsa, Steam ağı belirli olayları ve mesajları "teslim edildi" olarak işaretleyebilir, bu da geleneksel Steam istemcinizin mevcut olmadığından dolayı alamayacağı anlamına gelir. Çevrimdışı durumu bu sorunu da çözer, çünkü ASF bu durumda topluluk olayları için asla dikkate alınmaz, bu nedenle geri döndüğünüzde tüm okunmamış mesajlar ve diğer olaylar düzgün bir şekilde okunmamış olarak işaretlenir.

ASF'nin `Çevrimdışı` modda çalışmasının, genellikle Steam sohbet yoluyla komut alamayacağına dikkat etmek önemlidir, çünkü sohbet ve tüm topluluk varlığı aslında tamamen çevrimdışıdır. Bu soruna bir çözüm, benzer şekilde çalışırken (durumu gizlemek), ancak mesajları alıp yanıtlamaya devam eden (dolayısıyla yukarıda belirtilen bildirimleri ve okunmamış mesajları reddetme potansiyeline sahip) `Görünmez` modunu kullanmaktır. `Görünmez` modu, durumu göstermek istemediğiniz alt hesaplarda, ancak yine de komut gönderebilmek istediğinizde en mantıklısıdır.

Ancak, `Görünmez` modun bir sorunu vardır - birincil hesaplarda iyi çalışmaz. Bunun nedeni, **çevrimiçi olan** herhangi bir Steam oturumunun, ASF'nin kendisi yapmasa bile, durumu **açığa çıkarmasıdır**. Bu, ASF tarafında çözülemeyen mevcut bir Steam ağı sınırlaması/hatası nedeniyle oluşur, bu nedenle `Görünmez` modunu kullanmak istiyorsanız, aynı hesaba yapılan **tüm** diğer oturumların da `Görünmez` modunu kullanmasını sağlamanız gerekecektir. Bu, ASF'nin umarım tek aktif oturum olduğu alt hesaplarda geçerli olacaktır, ancak birincil hesaplarda neredeyse her zaman arkadaşlarınıza `Çevrimiçi` olarak görünmeyi tercih edeceksiniz, yalnızca ASF etkinliğini gizleyeceksiniz ve bu durumda `Görünmez` modunu kullanmak sizin için tamamen işe yaramaz (bu durumda `Çevrimdışı` modunu kullanmanızı öneririz). Valve'in bu sınırlamayı/hatasını gelecekte çözeceğini umuyoruz, ancak bunun yakın zamanda gerçekleşmesini beklemem.

Bu özelliği nasıl yapılandıracağınızdan emin değilseniz, birincil hesaplar için `0` (`Çevrimdışı`) ve diğer durumlarda varsayılan `1` (`Çevrimiçi`) değerini kullanmanız önerilir.

---

### `PasswordFormat`

`byte` type with default value of `0` (`PlainText`). Varsayılan değeri `0` (`Düz Metin`) olan `byte` türündeki bu özellik, `SteamPassword` özelliğinin formatını tanımlar ve şu anda **[güvenlik](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** bölümünde belirtilen değerleri destekler. Bu yönergeleri takip etmelisiniz, çünkü `SteamPassword` özelliğinin gerçekten belirtilen `PasswordFormat` formatında olduğundan emin olmanız gerekecektir. Başka bir deyişle, `PasswordFormat`'ı değiştirdiğinizde `SteamPassword`'ınız **zaten** bu formatta olmalıdır, sadece bu formatta olmaya çalışmak yeterli değildir. Ne yaptığınızı bilmiyorsanız, varsayılan değeri olan `0` ile bırakmalısınız.

Eğer bir botun `PasswordFormat`'ını Steam ağına en az bir kez giriş yapmış bir botta değiştirirseniz, bir sonraki bot başlatmasında bir kezlik bir şifre çözme hatası alabilirsiniz - bu, `PasswordFormat`'ın `Bot.db` veritabanı dosyasında hassas özelliklerin otomatik şifreleme/şifre çözme ile ilgili olarak da kullanılması nedeniyle oluşur. Bu hatayı güvenle göz ardı edebilirsiniz, çünkü ASF bu durumdan kendi başına kurtulabilir. Ancak, eğer bu sürekli bir durumsa, örneğin her yeniden başlatmada meydana geliyorsa, araştırılması gerekir.

---

### `RedeemingPreferences`

`byte flags` type with default value of `0`. This property defines ASF behaviour when redeeming cd-keys, and is defined as below:

| Değer | İsim                               | Açıklama                                                                                                                         |
| ----- | ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| 0     | Hiçbiri                            | Özel çevirme tercihleri yok, varsayılan                                                                                          |
| 1     | Forwarding                         | Çevrilmesi mümkün olmayan anahtarları diğer botlara iletme                                                                       |
| 2     | Dağıtma                            | Tüm anahtarları kendisi ve diğer botlar arasında dağıtma                                                                         |
| 4     | Eksik Oyunları Sakla               | Keep keys for (potentially) missing games when forwarding, leaving them unused                                                   |
| 8     | AssumeWalletKeyOnBadActivationCode | `KötüEtkinlikKodu` anahtarlarını `Müşteriden ÇevrilemezKod` olarak varsay ve bu nedenle onları cüzdan anahtarları olarak çevirme |

Lütfen bu özelliğin `flags` alanı olduğuna dikkat edin, bu nedenle mevcut değerlerin herhangi bir kombinasyonunu seçmek mümkündür. Daha fazla bilgi edinmek istiyorsanız **[json mapping](#json-mapping)** bölümüne göz atın. Bayraklardan hiçbirinin etkinleştirilmemesi `None` seçeneğiyle sonuçlanır.

`İletme` seçeneği, çevrilmesi mümkün olmayan bir anahtarı, eksik olan oyunu bulunan başka bir bağlı ve oturum açmış bota iletecektir (kontrol edilebiliyorsa). En yaygın durum, `ZatenSatınAlınmış` oyununu eksik olan başka bir bota iletmektir, ancak bu seçenek ayrıca `GerekliUygulamaSahibiDeğil`, `OranSınırlı` veya `KısıtlıÜlke` gibi diğer senaryoları da kapsar.

`Dağıtma` seçeneği, botun aldığı tüm anahtarları kendisi ve diğer botlar arasında dağıtmasına neden olur. Bu, her botun bir anahtar alacağı anlamına gelir. Genellikle bu, aynı oyun için birçok anahtar çevirdiğinizde ve bunları botlarınız arasında eşit şekilde dağıtmak istediğinizde kullanılır, çeşitli farklı oyunlar için anahtar çevirmektense. Bu özellik, tek bir `redeem` eyleminde sadece bir anahtar çeviriyorsanız (dağıtılacak ekstra anahtarlar olmadığı için) mantıklı değildir.

`Eksik Oyunları Sakla`, anahtarın gerçekten botumuza ait olup olmadığından emin olamadığımızda `İletme`'yi atlamasına neden olur. Bu, `İletme`'nin yalnızca `ZatenSatınAlınmış` anahtarlarına uygulanacağı anlamına gelir, ayrıca `GerekliUygulamaSahibiDeğil`, `OranSınırlı` veya `KısıtlıÜlke` gibi diğer durumları kapsamaz. Genellikle bu seçeneği birincil hesaplarda kullanmak istersiniz, böylece anahtarların çevirildiği hesabın daha sonra geçici olarak `OranSınırlı` hale gelmesi durumunda daha fazla iletilmeyecektir. Açıklamadan da tahmin edebileceğiniz gibi, `İletme` etkinleştirilmemişse bu alanın hiçbir etkisi yoktur.

`Kötü Etkinlik Kodu Üzerine Cüzdan Anahtarı Say` seçeneği, `KötüEtkinlikKodu` anahtarlarını `Müşteriden ÇevrilemezKod` olarak değerlendirilmesini sağlar ve bu nedenle ASF'nin bunları cüzdan anahtarları olarak çevirmeye çalışmasını sağlar. Steam, cüzdan anahtarlarını `KötüEtkinlikKodu` olarak duyurabilir (ve daha önce olduğu gibi `Müşteriden ÇevrilemezKod` olarak değil), bu da ASF'nin bunları çevirmeye asla teşebbüs etmemesine neden olur. Ancak, bu tercihi **karşısında** öneriyoruz, çünkü bu, ASF'nin her geçersiz anahtarı cüzdan kodu olarak çevirmeye çalışmasına neden olur ve bu da Steam hizmetine aşırı miktarda (potansiyel olarak geçersiz) istek gönderir, tüm potansiyel sonuçlarıyla birlikte. Bunun yerine, cüzdan anahtarlarını bilinçli olarak çevirirken `ForceAssumeWalletKey` **[`redeem^`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands#redeem-modes)** modunu kullanmanızı öneririz, bu, gerekli olduğunda sadece gerektiği şekilde çalıştırılacak bir geçici çözümü sağlar.

Hem `İletme` hem de `Dağıtma` seçeneğini etkinleştirmek, dağıtım özelliğini iletme özelliğinin üzerine ekler, bu da ASF'nin önce tüm botlarda bir anahtarı çevirmeye çalışacağı (iletme) ve ardından bir sonraki anahtara geçeceği (dağıtma) anlamına gelir. Bu seçeneği, `İletme`'yi kullanmak istediğinizde, ancak her anahtar ile sırayla gitmek yerine anahtarın kullanılması sırasında botu değiştirme davranışını istediğinizde kullanmak istersiniz (bu, yalnızca `İletme` olurdu). Bu davranış, anahtarlarınızın çoğunun hatta tümünün aynı oyuna bağlı olduğunu bildiğinizde faydalı olabilir, çünkü bu durumda `İletme` tek başına her şeyi önce bir botta çevirmeye çalışacaktır (anahtarlarınız benzersiz oyunlar içindir). Ancak `İletme` + `Dağıtma`, yeni anahtar çevirme görevini ilk bot yerine başka bir bota "dağıtarak" bir anahtarın "anlamsız" bir denemesini atlatarak, bu durumda mantıklıdır (anahtarlar aynı oyun içinse).

Tüm çevirme senaryoları için gerçek bot sırası alfabetiktir, kullanılabilir olmayan botlar hariç (bağlı değil, durdurulmuş veya benzeri). Lütfen, her IP ve hesap başına saatlik bir çevirme deneme sınırı olduğunu ve `OK` ile bitmeyen her çevirme denemesinin başarısız denemelere katkıda bulunduğunu unutmayın. ASF, `ZatenSatınAlınmış` hatalarını minimize etmek için elinden geleni yapacaktır, örneğin, anahtarın belirli bir oyuna sahip olduğu bir botta bir anahtarın iletilmesini önlemeye çalışarak, ancak Steam'in lisansları nasıl yönettiği nedeniyle her zaman çalışacağının garantisi yoktur. `İletme` veya `Dağıtma` gibi çevirme bayraklarını kullanmak, `OranSınırlı` olma olasılığınızı her zaman artıracaktır.

Ayrıca, anahtarları erişiminizin olmadığı botlara iletemez veya dağıtamazsınız. Bu açık olmalı, ancak çevirme sürecinize dahil etmek istediğiniz tüm botların en azından `Operatör` olduğundan emin olun, örneğin `status ASF` **[komutu](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** ile.

---

### `RemoteCommunication`

Varsayılan değeri `0` olan `byte flags` türü. Bu özellik, cd-anahtarlarını çevirirken ASF'nin davranışını tanımlar ve aşağıda belirtilmiştir:

| Değer | İsim           | Açıklama                                                                                                                                                                                                                                                            |
| ----- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0     | Hiçbiri        | Üçüncü taraf iletişimine izin verilmez, bu da seçilen ASF özelliklerini kullanılamaz hale getirir                                                                                                                                                                   |
| 1     | SteamGrubu     | **[ASF'nin Steam grubuyla](https://steamcommunity.com/groups/archiasf)** iletişime izin verir                                                                                                                                                                       |
| 2     | GenelListeleme | **[ASF'nin STM listelemesiyle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin#publiclisting)** iletişime izin verir, eğer kullanıcı ayrıca **[`TradingPreferences`](#tradingpreferences)** bölümünde `SteamTradeMatcher`'ı etkinleştirmişse |

Lütfen bu özelliğin `flags` alanı olduğunu unutmayın, bu nedenle mevcut değerlerin herhangi bir kombinasyonunu seçmek mümkündür. Daha fazla bilgi edinmek isterseniz **[json mapping](#json-mapping)** kısmına göz atın. Hiçbir bayrağı etkinleştirmemek `Hiçbiri` seçeneğiyle sonuçlanır.

Bu seçenek, ASF tarafından sunulan her üçüncü taraf iletişimini değil, yalnızca diğer ayarlar tarafından ima edilmeyenleri içerir. Örneğin, ASF'nin otomatik güncellemelerini etkinleştirdiyseniz, ASF, yapılandırmanıza göre hem GitHub (indirmeler için) hem de sunucumuzla (sağlama toplamı doğrulaması için) iletişim kuracaktır. Aynı şekilde, **[`TradingPreferences`](#tradingpreferences)** içinde `MatchActively` öğesini etkinleştirmek, bu işlevsellik için gerekli olan listelenen botları getirmek için sunucumuzla iletişim kurmayı gerektirir.

Bu konu hakkında daha fazla açıklama **[uzak iletişim](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication)** bölümünde mevcuttur. Bu özelliği düzenlemek için bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `SendTradePeriod`

Varsayılan değeri `0` olan `byte` türü. Bu özellik, `FarmingPreferences` içindeki `SendOnFarmingFinished` tercihine çok benzer şekilde çalışır, tek bir farkla - çiftçilik bittiğinde ticaret göndermek yerine, ne kadar çiftçilik yapmamız gerektiğine bakılmaksızın, her `SendTradePeriod` saatte bir gönderebiliriz. sol. Çiftçiliği bitirmesini beklemek yerine alt hesaplarınızı normal olarak `loot` yapmak istiyorsanız bu kullanışlıdır. `0` varsayılan değeri bu özelliği devre dışı bırakır, botunuzun size örneğin her gün ticaret göndermesini istiyorsanız buraya `24` koymalısınız.

Genellikle bu özellikle birlikte **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** kullanmak isteyeceksiniz, ancak 2FA onaylarını zamanında manuel olarak işlemek istiyorsanız bu bir gereklilik değildir. Bu özelliği nasıl ayarlayacağınızdan emin değilseniz, varsayılan `0` değeriyle bırakın. Bu özelliği nasıl ayarlayacağınızdan emin değilseniz, varsayılan değeri olan `0` ile bırakmalısınız.

---

### `SteamLogin`

Varsayılan değeri `null` olan `string` türü. Bu özellik, Steam girişinizi tanımlar - buhara giriş yapmak için kullandığınız özellik. Steam girişinizi buraya tanımlamanın yanı sıra, her ASF başlangıcında yapılandırmaya koymak yerine Steam girişinizi girmek istiyorsanız varsayılan `null` değerini de koruyabilirsiniz. Hassas verileri yapılandırma dosyasına kaydetmek istemiyorsanız bu sizin için yararlı olabilir.

---

### `SteamMasterClanID`

Varsayılan değeri `0` olan `ulong` türü. Bu özellik, botun grup sohbeti de dahil olmak üzere otomatik olarak katılması gereken buhar grubunun steamID'sini tanımlar. Grubunuzun steamID'sini **[sayfasına](https://steamcommunity.com/groups/archiasf)** gidip ardından bağlantının sonuna `/memberslistxml?xml=1` ekleyerek kontrol edebilirsiniz, böylece bağlantı **[şuna](https://steamcommunity.com/groups/archiasf/memberslistxml?xml=1)** benzeyecektir. Ardından, grubunuzun steamID'sini sonuçtan alabilirsiniz, `<groupID64>` etiketinde bulunur. Yukarıdaki örnekte bu `103582791440160998` olur. Başlangıçta belirli bir gruba katılmaya çalışmanın yanı sıra, bot bu gruba gelen grup davetlerini de otomatik olarak kabul edecek ve bu da grubunuzun özel üyeliği varsa botunuzu manuel olarak davet etmenizi mümkün kılacaktır. Botlarınız için özel bir grubunuz yoksa, bu özelliği varsayılan `0` değeriyle tutmalısınız.

---

### `SteamParentalCode`

Varsayılan değeri `null` olan `string` türü. Bu özellik, Steam ebeveyn PIN'inizi tanımlar. ASF, buhar ebeveyninin koruduğu kaynaklara erişim gerektirir, bu nedenle bu özelliği kullanırsanız, normal çalışabilmesi için ASF'ye ebeveyn kilidi açma PIN'i sağlamalısınız. `null` varsayılan değeri, bu hesabın kilidini açmak için gereken bir buhar ebeveyn PIN'i olmadığı anlamına gelir ve buhar ebeveyn işlevini kullanmıyorsanız muhtemelen istediğiniz şey budur.

Sınırlı durumlarda, ASF geçerli bir Steam ebeveyn kodu da oluşturabilir, ancak bu, aşırı miktarda işletim sistemi kaynağı ve tamamlanması için ek süre gerektirir, başarılı olması garanti edilmez, bu nedenle buna güvenmemeyi öneririz. ASF'nin kullanması için konfigürasyonda geçerli bir `SteamParentalCode` yerine koyun. ASF, PIN'in gerekli olduğunu belirlerse ve kendi başına bir PIN oluşturamazsa, sizden girdi ister.

---

### `SteamPassword`

Varsayılan değeri `null` olan `string` türü. Bu özellik, Steam parolanızı tanımlar - buhara giriş yapmak için kullandığınız parola. Steam parolanızı buraya tanımlamanın yanı sıra, her ASF başlangıcında yapılandırmaya koymak yerine Steam parolanızı girmek istiyorsanız varsayılan `null` değerini de koruyabilirsiniz. Hassas verileri yapılandırma dosyasına kaydetmek istemiyorsanız bu sizin için yararlı olabilir.

---

### `SteamTradeToken`

`string` türünde ve varsayılan değeri `null` olan bir özellik. Botunuz arkadaş listenizde olduğunda, bot ticaret jetonuyla uğraşmadan size hemen bir ticaret gönderebilir, bu nedenle bu özelliği varsayılan `null` değeriyle bırakabilirsiniz. Ancak botunuzu arkadaş listenizde BULUNDURMAMAYA karar verirseniz, bu botun işlem göndermeyi beklediği kullanıcı olarak bir ticaret jetonu oluşturmanız ve doldurmanız gerekecektir. Başka bir deyişle, bu özellik, **bu** bot örneğinin `SteamUserPermissions` içinde `Master` izniyle tanımlanan hesabın ticaret jetonuyla doldurulmalıdır.

Jetonunuzu, `Master` iznine sahip oturum açmış kullanıcı olarak bulmak için **[buraya](https://steamcommunity.com/my/tradeoffers/privacy)** gidin ve ticaret URL'nize bir göz atın. Aradığımız jeton, ticaret URL'nizdeki `&token=` kısmından sonraki 8 karakterden oluşur. Bu 8 karakteri `SteamTradeToken` olarak kopyalayıp buraya koymalısınız. Tüm ticaret URL'sini ve `&token=` kısmını değil, yalnızca jetonun kendisini (8 karakter) ekleyin.

---

### `SteamUserPermissions`

Varsayılan değeri boş olan `ImmutableDictionary<ulong, byte>` türü. Bu özellik, 64 bit buhar kimliğiyle tanımlanan belirli bir Steam kullanıcısını, ASF örneğindeki iznini belirten `byte` numarasına eşleyen bir sözlük özelliğidir. Şu anda ASF'de mevcut olan bot izinleri şu şekilde tanımlanır:

| Değer | İsim           | Açıklama                                                                                                                                                                                                               |
| ----- | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0     | Hiçbiri        | Özel izin yok, bu esas olarak bu sözlükte eksik olan buhar kimliklerine atanan bir referans değeridir - bu izinle kimseyi tanımlamaya gerek yoktur                                                                     |
| 1     | Aile Paylaşımı | Aile paylaşımı kullanıcıları için minimum erişim sağlar. Bir kez daha, bu esas olarak bir referans değeridir, çünkü ASF, kitaplığımızı kullanmak için izin verdiğimiz buhar kimliklerini otomatik olarak keşfedebilir. |
| 2     | Operatör       | Verilen bot örneklerine temel erişim sağlar, esas olarak lisans ekleme ve anahtarları kullanma                                                                                                                         |
| 3     | Master         | Belirli bir bot örneğine tam erişim sağlar                                                                                                                                                                             |

Kısacası, bu özellik belirli kullanıcılar için izinleri yönetmenizi sağlar. İzinler, esas olarak ASF **[komutlarına](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** erişim için önemlidir, aynı zamanda ticaretleri kabul etmek gibi birçok ASF özelliğini etkinleştirmek için de önemlidir. Örneğin, kendi hesabınızı `Master` olarak ayarlamak ve 2-3 arkadaşınıza `Operator` erişimi vermek isteyebilirsiniz, böylece ASF ile botunuz için kolayca anahtarları kullanabilirler, ancak örneğin durdurmaya uygun **olmazlar** . Bu sayede belirli kullanıcılara kolayca izin atayabilir ve belirlediğiniz dereceye kadar botunuzu kullanmalarına izin vereler.

We recommend to set exactly one user as `Master`, and any amount you wish as `Operators` and below. While it's technically possible to set multiple `Masters` and ASF will work correctly with them, for example by accepting all of their trades sent to the bot, ASF will use only one of them (with lowest steam ID) for every action that requires a single target, for example a `loot` request, so also properties like `SendOnFarmingFinished` preference in `FarmingPreferences` or `SendTradePeriod`. If you perfectly understand those limitations, especially the fact that `loot` request will always send items to the `Master` with lowest steam ID, regardless of the `Master` that actually executed the command, then you can define multiple users with `Master` permission here, but we still recommend a single master scheme.

It's nice to note that there is one more extra `Owner` permission, which is declared as `SteamOwnerID` global config property. You can't assign `Owner` permission to anybody here, as `SteamUserPermissions` property defines only permissions that are related to the bot instance, and not ASF as a process. For bot-related tasks, `SteamOwnerID` is treated the same as `Master`, so defining your `SteamOwnerID` here is not necessary.

---

### `TradeCheckPeriod`

`byte` type with default value of `60`. Normally ASF handles incoming trade offers right after receiving notification about one, but sometimes because of Steam glitches it can't do it at that time, and such trade offers remain ignored until next trade notification or bot restart occurs, which may lead to trades being cancelled or items not available at that later time. Bu parametre sıfırdan farklı bir değere ayarlanırsa, ASF belirli aralıklarla (`TradeCheckPeriod` dakikada bir) bu bekleyen ticaretleri kontrol eder. Varsayılan değer, Steam sunucularına yapılan ek isteklerle gelen ticaretlerin kaybolma olasılığı arasında bir denge gözetilerek seçilmiştir. Ancak, ASF'yi sadece kartları toplamak için kullanıyorsanız ve gelen ticaretleri otomatik olarak işlemek istemiyorsanız, bu özelliği tamamen devre dışı bırakmak için `0` olarak ayarlayabilirsiniz. Öte yandan, botunuz [ASF'nin STM listelemesi](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin#publiclisting) gibi halka açık listelemelerde yer alıyorsa veya başka otomatik hizmetler sağlıyorsa, bu parametreyi 15 dakika gibi daha kısa bir süreye düşürmek isteyebilirsiniz.

---

### `TradingPreferences`

Varsayılan değeri `0` olan `byte flags` türündedir. This property defines ASF behaviour when in trading, and is defined as below:

| Değer | İsim                | Açıklama                                                                                                                                                                                                                                           |
| ----- | ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0     | Hiçbiri             | Özel ticaret tercihi yok, varsayılan                                                                                                                                                                                                               |
| 1     | AcceptDonations     | Kaybedeceğimiz hiçbir şeyin olmadığı ticaretleri kabul eder                                                                                                                                                                                        |
| 2     | SteamTradeMatcher   | **[STM](https://www.steamtradematcher.com)** benzeri ticaretlere pasif olarak katılır. Daha fazla bilgi için **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#steamtradematcher)** sayfasını ziyaret edin                   |
| 4     | MatchEverything     | `SteamTradeMatcher`'ın ayarlanmasını gerektirir ve bununla birlikte iyi ve nötr ticaretlerin yanı sıra kötü ticaretleri de kabul eder                                                                                                              |
| 8     | DontAcceptBotTrades | Diğer bot örneklerinden gelen `loot` ticaret tekliflerini otomatik olarak kabul etmez                                                                                                                                                              |
| 16    | MatchActively       | **[STM](https://www.steamtradematcher.com)** benzeri ticaretlere aktif olarak katılır. Daha fazla bilgi için **[ItemsMatcherPlugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin#matchactively)** sayfasını ziyaret edin |

Lütfen bu özelliğin `flags` alanı olduğuna dikkat edin, bu nedenle mevcut değerlerin herhangi bir kombinasyonunu seçmek mümkündür. Daha fazla bilgi edinmek istiyorsanız **[json mapping](#json-mapping)** bölümüne göz atın. Bayraklardan hiçbirinin etkinleştirilmemesi `None` seçeneğiyle sonuçlanır.

ASF'nin ticaret mantığı hakkında daha fazla açıklama ve mevcut her bayrağın tanımı için **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** bölümüne göz atabilirsiniz.

---

### `TransferableTypes`

`ImmutableHashSet<byte>` type with default value of `1, 3, 5` steam item types. `ImmutableHashSet<byte>` türünde ve varsayılan değeri `1, 3, 5` olan bu özellik, `transfer` **[komutu](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** sırasında botlar arasında hangi Steam öğe türlerinin transfer edileceğini tanımlar. ASF, yalnızca `TransferableTypes` içindeki öğeleri ticaret tekliflerine dahil eder, bu nedenle bu özellik size bir ticaret teklifinde ne alacağınızı seçme imkanı sağlar.

| Değer | İsim                  | Açıklama                                                                             |
| ----- | --------------------- | ------------------------------------------------------------------------------------ |
| 0     | Unknown               | Aşağıdakilerden hiçbiriyle uyumlu olmayan her tür                                    |
| 1     | BoosterPack           | Bir oyundan rastgele 3 kart içeren booster paketi                                    |
| 2     | Emoticon              | Steam Sohbetinde kullanılacak emotikon                                               |
| 3     | FoilTradingCard       | `TradingCard`'ın foil varyantı                                                       |
| 4     | ProfileBackground     | Steam profilinizde kullanılacak profil arka planı                                    |
| 5     | TradingCard           | Steam ticaret kartı, rozetler için kullanılır (foil olmayan)                         |
| 6     | SteamGems             | Boosterlar ve torbalar dahil olmak üzere crafting için kullanılan Steam mücevherleri |
| 7     | SaleItem              | Steam satışları sırasında verilen özel öğeler                                        |
| 8     | Consumable            | Kullanıldıktan sonra kaybolan özel tüketilebilir öğeler                              |
| 9     | ProfileModifier       | Steam profil görünümünü değiştiren özel öğeler                                       |
| 10    | Sticker               | Steam sohbetinde kullanılabilecek özel öğeler                                        |
| 11    | ChatEffect            | Steam sohbetinde kullanılabilecek özel öğeler                                        |
| 12    | MiniProfileBackground | Steam profili için özel arka plan                                                    |
| 13    | AvatarProfileFrame    | Steam profili için özel avatar çerçevesi                                             |
| 14    | AnimatedAvatar        | Steam profili için özel animasyonlu avatar                                           |
| 15    | KeyboardSkin          | Steam deck için özel klavye kaplaması                                                |
| 16    | StartupVideo          | Steam deck için özel başlangıç videosu                                               |

Lütfen yukarıdaki ayarlardan bağımsız olarak, ASF'nin yalnızca **[Steam topluluk öğelerini](https://steamcommunity.com/my/inventory/#753_6)** (`appID` 753, `contextID` 6) talep edeceğini unutmayın, bu nedenle tüm oyun öğeleri, hediyeler ve benzerleri tanım gereği takas teklifinden hariç tutulur.

Varsayılan ASF ayarı, botun en yaygın kullanımına dayanır ve yalnızca güçlendirici paketler ve takas kartları (folyolar dahil) yağmalanır. Burada tanımlanan özellik, bu davranışı sizi memnun edecek şekilde değiştirmenize olanak tanır. Valve'in yeni bir Steam öğesi yayınladığında, yukarıda tanımlanmayan tüm türlerin `Unknown` türü olarak gösterileceğini unutmayın. Bu, gelecekteki bir sürümde burada eklenene kadar ASF tarafından `Unknown` olarak işaretlenecektir. That's why in general it's not recommended to include `Unknown` type in your `TransferableTypes`, unless you know what you're doing, and you also understand that ASF will send your entire inventory in a trade offer if Steam Network gets broken again and reports all your items as `Unknown`. Benim güçlü önerim, her şeyi yağmalamayı bekleseniz bile `Unknown` türünü `LootableTypes` içine dahil etmemeniz yönündedir.

---

### `UseLoginKeys`

Varsayılan değeri `true` olan `bool` türü. `bool` türünde ve varsayılan değeri `true` olan bu özellik, ASF'nin bu Steam hesabı için oturum açma anahtarları mekanizmasını kullanıp kullanmayacağını tanımlar. Oturum açma anahtarları, resmi Steam istemcisindeki "beni hatırla" seçeneğiyle oldukça benzer şekilde çalışır ve ASF'nin geçici bir kez kullanım anahtarını saklayıp kullanmasını sağlar, böylece şifre, Steam Guard veya 2FA kodu sağlamaya gerek kalmadan oturum açabilirsiniz. Oturum açma anahtarı `BotName.db` dosyasında saklanır ve otomatik olarak güncellenir. Bu nedenle, ASF ile başarılı bir şekilde bir kez oturum açtıktan sonra şifre/SteamGuard/2FA kodu sağlamanıza gerek kalmaz.

Oturum açma anahtarları varsayılan olarak kullanılır, böylece her oturum açmada `SteamPassword`, SteamGuard veya 2FA kodu (eğer **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** kullanmıyorsanız) girmenize gerek kalmaz. Ayrıca, oturum açma anahtarı yalnızca bir kez kullanılabilir ve şifrenizi ifşa etmez. Aynı yöntem, orijinal Steam istemcinizde oturum açma anahtarını sakladığınızda ve bir sonraki oturum açma girişiminizde kullanıldığında kullanılan yöntemle aynıdır.

Ancak, bazı kişiler bu küçük ayrıntılardan bile endişe duyabilir, bu yüzden bu seçenek burada mevcut, böylece ASF'nin kapatıldıktan sonra önceki oturumu yeniden başlatmanıza izin verecek herhangi bir token saklamasını sağlayabilirsiniz. Bu seçeneği devre dışı bırakmak, resmi Steam istemcisinde "beni hatırla"yı işaretlememekle aynı şekilde çalışır. Ne yaptığınızı bilmiyorsanız, varsayılan değeri `true` olarak bırakmalısınız.

---

### `UserInterfaceMode`

Varsayılan değeri `0` olan `byte` türü. This property specifies user interface mode that the bot will be announced with after logging in to the Steam network. It might influence how the account is visible e.g. on the Steam chat, if your presence allows that through `OnlineStatus`. Şu anda aşağıdaki modlardan birini seçebilirsiniz:

| Değer | İsim           | Açıklama                  |
| ----- | -------------- | ------------------------- |
| `0`   | VGUI           | Default Steam client mode |
| `1`   | Tenfoot        | Big picture mode          |
| `2`   | Mobil          | Steam mobile app          |
| `3`   | Web            | Web browser session       |
| `4`   | ClientUI       |                           |
| `5`   | MobileChat     | Steam mobile chat app     |
| `6`   | EmbeddedClient |                           |

Not all modes might have an effect. Also, you might be interested in checking `OnlinePreferences`, since some additional features are enabled there. Bu özelliği nasıl ayarlayacağınızdan emin değilseniz, varsayılan değeri olan `0` ile bırakmalısınız.

---

### `WebProxy`

`string` türünde ve varsayılan değeri `null` olan bir özellik. This property defines a web proxy address that will be used for bot-specific internal http-related communication, especially to services such as `api.steampowered.com`, `steamcommunity.com` and `store.steampowered.com`. If not set, ASF will use global `WebProxy` setting specified above instead. Proxying ASF requests could be exceptionally useful for bypassing various kinds of firewalls, especially the great firewall in China.

Bu özellik uri dizesi olarak tanımlanır:

> Bir URI dizesi, bir şemadan (desteklenir: http/https/socks4/socks4a/socks5), bir ana bilgisayardan ve isteğe bağlı bir bağlantı noktasından oluşur. Tam bir uri dizesine örnek olarak `"http://contoso.com:8080"` verilebilir.

Proxy'niz kullanıcı kimlik doğrulaması gerektiriyorsa, `WebProxyUsername` ve/veya `WebProxyPassword` öğelerini de ayarlamanız gerekir. Böyle bir ihtiyaç yoksa, yalnızca bu özelliğin ayarlanması yeterlidir.

If you require proxying internal Steam network communication (CMs) as well, then you should ensure to configure **[`SteamProtocols`](#steamprotocols)** bot's property to a value that allows only websocket transport, i.e. a value of `4`, as only websockets are supported for proxying.

Bu özelliği düzenlemek için bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `WebProxyPassword`

`string` türünde ve varsayılan değeri `null` olan bir özellik. Bu özellik, proxy işlevselliği sağlayan hedef bir `WebProxy` makinesi tarafından desteklenen temel, sindirim, NTLM ve Kerberos kimlik doğrulamasında kullanılan parola alanını tanımlar. Proxy'niz kullanıcı kimlik bilgileri gerektirmiyorsa, buraya herhangi bir şey girmenize gerek yoktur. Bu seçeneğin kullanılması, yalnızca `WebProxy` de kullanılıyorsa anlamlıdır, aksi takdirde hiçbir etkisi yoktur.

Bu özelliği düzenlemek için bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

### `WebProxyUsername`

Varsayılan değeri `null` olan `string` türü. Bu özellik, proxy işlevselliği sağlayan hedef bir `WebProxy` makinesi tarafından desteklenen temel, sindirim, NTLM ve Kerberos kimlik doğrulamasında kullanılan kullanıcı adı alanını tanımlar. Proxy'niz kullanıcı kimlik bilgileri gerektirmiyorsa, buraya herhangi bir şey girmenize gerek yoktur. Bu seçeneğin kullanılması, yalnızca `WebProxy` de kullanılıyorsa anlamlıdır, aksi takdirde hiçbir etkisi yoktur.

Bu özelliği düzenlemek için bir nedeniniz yoksa, varsayılan değerde tutmalısınız.

---

## Dosya Yapısı

ASF oldukça basit bir dosya yapısı kullanır.

```text
├── 📁 config
│     ├── ASF.json
│     ├── ASF.db
│     ├── Bot1.json
│     ├── Bot1.db
│     ├── Bot2.json
│     ├── Bot2.db
│     └── ...
├── ArchiSteamFarm.dll
├── log.txt
└── ...
```

ASF'yi yeni bir konuma taşımak, örneğin başka bir bilgisayara, `config` dizinini taşımak/yapıştırmak yeterlidir ve bu, herhangi bir "ASF yedeği" işlemi için önerilen yöntemdir, çünkü programın geri kalan kısmını GitHub'dan indirme şansınız her zaman vardır ve ASF'nin iç dosyalarını bozmadan (örneğin, hatalı bir yedekleme yoluyla) risk almazsınız.

`log.txt` dosyası, son ASF çalıştırmanız sırasında üretilen günlükleri içerir. Bu dosya hassas bilgi içermez ve sorunlar, çökme durumları veya basitçe son ASF çalıştırmanızda neler olduğunu anlamak için son derece yararlıdır. Sorunlar veya hatalarla karşılaşırsanız, bu dosyayı çok sık soracağız. ASF bu dosyayı otomatik olarak yönetir, ancak gelişmiş bir kullanıcıysanız ASF **[logging](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Logging)** modülünü daha fazla özelleştirebilirsiniz.

`config` directory is the place that holds configuration for ASF, including all of its bots.

`ASF.json`, global ASF yapılandırma dosyasıdır. Bu yapılandırma, ASF'nin bir işlem olarak nasıl davranacağını belirler ve tüm botlar ile programı etkiler. Burada, ASF işlem sahibi, otomatik güncellemeler veya hata ayıklama gibi global özellikleri bulabilirsiniz.

`BotName.json`, belirli bir bot örneğinin yapılandırma dosyasıdır. Bu yapılandırma, verilen bot örneğinin nasıl davrandığını belirler, bu nedenle bu ayarlar yalnızca o bota özgüdür ve diğer botlarla paylaşılmaz. Bu, botları çeşitli farklı ayarlarla yapılandırmanıza ve tüm botların tam olarak aynı şekilde çalışmasını gerektirmeden yapılandırmanıza olanak tanır. Her bot, `BotName` yerine sizin tarafınızdan seçilen benzersiz bir tanımlayıcı ile adlandırılır.

`config` dizini, ASF'nin yapılandırmasını ve tüm botlarının yapılandırmasını içerir.

`ASF.db`, global ASF veritabanı dosyasıdır. Global kalıcı bir depolama olarak işlev görür ve ASF işlemiyle ilgili çeşitli bilgileri saklamak için kullanılır, örneğin yerel Steam sunucularının IP'leri. **Bu dosyayı düzenlememelisiniz**.

`BotName.db`, belirli bir bot örneğinin veritabanı dosyasıdır. Bu dosya, belirli bir bot örneği hakkında kalıcı depolamada kritik verileri saklamak için kullanılır, örneğin oturum açma anahtarları veya ASF 2FA. **Bu dosyayı düzenlememelisiniz**.

`BotName.keys`, **[background games redeemer](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)**'a anahtarları aktarmak için kullanılabilecek özel bir dosyadır. Bu dosya zorunlu değildir ve otomatik olarak oluşturulmaz, ancak ASF tarafından tanınır. Anahtarlar başarıyla içe aktarıldıktan sonra bu dosya otomatik olarak silinir.

`BotName.maFile`, **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**'yı içe aktarmak için kullanılabilecek özel bir dosyadır. Bu dosya zorunlu değildir ve otomatik olarak oluşturulmaz, ancak `BotName`'iniz ASF 2FA kullanmıyorsa ASF tarafından tanınır. ASF 2FA başarıyla içe aktarıldıktan sonra bu dosya otomatik olarak silinir.

---

## JSON mapping

Bu bölümde, ASF yapılandırma dosyalarında kullanılan JSON türleri ve nasıl kullanıldıkları açıklanır. Her özelliğin belirli bir türü vardır ve bu tür, kabul edilebilecek değerleri belirler. Yanlış türde bir değer kullanırsanız, ASF yapılandırmanızı doğru şekilde okuyamaz.

**ConfigGenerator'ı kullanmanızı şiddetle tavsiye ederiz** - bu araç, tür doğrulama gibi çoğu düşük seviyeli işlemi sizin yerinize halleder, böylece sadece geçerli değerler girmeniz yeterlidir. Bu bölüm, yapılandırmaları manuel olarak oluşturup düzenleyenler içindir, böylece hangi değerlerin kullanılabileceğini bilirsiniz.

ASF'de kullanılan türler, aşağıda belirtilen yerleşik C# türleridir:

---

Boolean türü, yalnızca `true` ve `false` değerlerini kabul eder.

Örnek: `"Enabled": true`

---

Unsigned integer türü, yalnızca `0` ile `4294967295` (dahil) arasındaki tam sayıları kabul eder.

Örnek: `"WebLimiterDelay": 300`

---

Unsigned byte türü, yalnızca `0` ile `255` (dahil) arasındaki tam sayıları kabul eder.

Örnek: `"ConnectionTimeout": 90`

---

Unsigned short türü, yalnızca `0` ile `65535` (dahil) arasındaki tam sayıları kabul eder.

---

Unsigned long integer türü, yalnızca `0` ile `18446744073709551615` (dahil) arasındaki tam sayıları kabul eder.

Örnekler: `"LicenseID": null`, `"LicenseID": "f6a0529813f74d119982eb4fe43a9a24"`

---

String türü, herhangi bir karakter dizisini kabul eder, boş diziler `""` ve `null` dahil. Boş dizi ve `null` değeri ASF tarafından aynı şekilde işlenir, bu yüzden hangisini kullanacağınız tercih meselesidir (biz genellikle `null` kullanıyoruz).

Örnek: `"SteamMasterClanID": 103582791440160998`

---

Nullable UUID türü, JSON'da string olarak kodlanır. UUID, `0` ile `9` ve `a` ile `f` arasında 32 onaltılı karakterden oluşur. ASF, geçerli UUID biçimlerinin bir çeşidini kabul eder - küçük harfli, büyük harfli, tireli veya tiresiz. Bu özellik nullable olduğundan, UUID'nin sağlanmadığını belirtmek için özel bir değer olan `null` da kabul edilir.

Örnekler: `"SteamLogin": null`, `"SteamLogin": ""`, `"SteamLogin": "MyAccountName"`

---

Verilen `valueType` türündeki değerlerin değiştirilemez koleksiyonu (liste). JSON'da, verilen `valueType` türündeki öğelerin dizisi olarak tanımlanır. ASF, listenin sıralamasının önemli olabileceğini ve birden fazla değeri desteklediğini belirtmek için `List` kullanır.

): `"FarmingOrders": [15, 11, 7]`

---

Verilen `valueType` türündeki benzersiz değerlerin değiştirilemez koleksiyonu (set). JSON'da, verilen `valueType` türündeki öğelerin dizisi olarak tanımlanır. ASF, setin sıralamasının önemli olmadığını ve olası tekrarları göz ardı ettiğini belirtmek için `HashSet` kullanır.

): `"Blacklist": [267420, 303700, 335590]`

---

Verilen `keyType` türündeki benzersiz anahtarları, `valueType` türündeki değerlere eşleyen değiştirilemez bir sözlük (map). JSON'da, anahtar-değer çiftleri içeren bir nesne olarak tanımlanır. `keyType` her zaman tırnak içinde olmalıdır, hatta bu bir `ulong` gibi değer türü olsa bile. Anahtarların sözlük içinde benzersiz olması gerektiğini unutmayın, bu kısıtlama JSON tarafından da uygulanır.

Örnek (ImmutableDictionary<ulong, byte>): `"SteamUserPermissions": { "76561198174813138": 3, "76561198174813137": 1 }`

---

Flags özelliği, birden fazla farklı özelliği bir araya getirir ve bit düzeyinde işlemler uygulayarak tek bir nihai değer oluşturur. Bu, aynı anda birden fazla farklı izin verilen değeri seçmenize olanak tanır. Nihai değer, etkinleştirilen tüm seçeneklerin değerlerinin toplamı olarak oluşturulur.

Örneğin, aşağıdaki değerler verilmiştir:

| Değer | İsim    |
| ----- | ------- |
| 0     | Hiçbiri |
| 1     | A       |
| 2     | B       |
| 4     | C       |

`B + C` kullanmak `6` değerini verir, `A + C` kullanmak `5` değerini verir, `C` kullanmak `4` değerini verir vb. Bu, tüm mümkün kombinasyonları oluşturmanıza olanak tanır - tümünü etkinleştirirseniz, `None + A + B + C` yaparak `7` elde edersiniz. Ayrıca, `0` değerine sahip bayrağın tanım gereği diğer tüm kombinasyonlarda etkin olduğunu unutmayın, bu yüzden genellikle hiçbir şeyi etkinleştirmeyen bir bayrak olabilir (`None` gibi).

So as you can see, in above example we have 3 available flags to switch on/off (`A`, `B`, `C`), and `8` possible values overall:
- `None -> 0`
- `A -> 1`
- `B -> 2`
- `A + B -> 3`
- `C -> 4`
- `A + C -> 5`
- `B + C -> 6`
- `A + B + C -> 7`

Örnek: `"SteamProtocols": 7`

---

## Compatibility mapping

JavaScript'in web tabanlı ConfigGenerator kullanırken basit `ulong` alanlarını JSON'da doğru şekilde serileştirememesi nedeniyle, `ulong` alanları sonuçta `s_` öneki ile birlikte string olarak render edilir. Bu, örneğin `"SteamOwnerID": 76561198006963719` değerinin ConfigGenerator tarafından `"s_SteamOwnerID": "76561198006963719"` olarak yazılmasını içerir. ASF, bu string mapping'ini otomatik olarak işlemek için doğru mantığı içerir, bu yüzden `s_` girişleri yapılandırmalarınızda geçerli ve doğru şekilde üretilmiştir. Yapılandırmaları kendiniz oluşturuyorsanız, mümkünse orijinal `ulong` alanlarını kullanmanızı öneririz, ancak bunu yapamazsanız, bu şemayı izleyerek `s_` öneki eklenmiş stringler olarak kodlayabilirsiniz. JavaScript kısıtlamasını nihayet çözmeyi umuyoruz.

---

## Configs compatibility

ASF'nin eski yapılandırmalarla uyumlu kalması önceliklidir. Yeni bir yapılandırma özelliği tanıtıldığında, eksik yapılandırma özellikleri varsayılan değerleri ile tanımlanmış gibi ele alınır. Bu nedenle, yeni bir ASF sürümünde yeni bir yapılandırma özelliği tanıtıldığında, tüm yapılandırmalarınız **uyumlu** kalır ve ASF, bu yeni yapılandırma özelliğini varsayılan değeri ile tanımlar. İhtiyacınıza göre yapılandırma özelliklerini ekleyebilir, kaldırabilir veya düzenleyebilirsiniz.

Belirlenen yapılandırma özelliklerini sadece değiştirmek istediğiniz özelliklerle sınırlamanızı öneririz, bu sayede tüm diğerleri için varsayılan değerleri otomatik olarak alırsınız. Bu, yapılandırmanızı temiz tutmanın yanı sıra varsayılan değeri değiştirmeyi istemediğiniz bir özellik varsa uyumluluğu artırır (örneğin, `WebLimiterDelay`).

ASF, yapılandırmalarınızı otomatik olarak yeniden biçimlendirip varsayılan değeri tutan alanları kaldırarak taşır/optimize eder. Bu davranışı `--no-config-migrate` **[komut satırı argümanı](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments#arguments)** ile devre dışı bırakabilirsiniz, örneğin yapılandırma dosyalarını salt okunur olarak sağlıyorsanız ve ASF'nin dosya değişikliklerine tepki vermesini istemiyorsanız.

---

## Otomatik Yenile

ASF, yapılandırmaların "anında" değiştirilmesinin farkındadır - bu nedenle ASF otomatik olarak şunları yapar:
- Yeni bir bot yapılandırması oluşturduğunuzda yeni bir bot örneği oluşturur (ve gerekiyorsa başlatır)
- Eski bir bot yapılandırması sildiğinizde eski bot örneğini durdurur (ve gerekiyorsa kaldırır)
- Bir yapılandırmayı düzenlediğinizde herhangi bir bot örneğini durdurur (ve gerekiyorsa başlatır)
- Yapılandırma adını değiştirdiğinizde botu yeniden başlatır (ve gerekiyorsa)

Bunların tümü şeffaf bir şekilde yapılır ve programı yeniden başlatmanıza veya diğer (etkilenmeyen) bot örneklerini öldürmenize gerek kalmaz.

Ayrıca, ASF, çekirdek ASF `ASF.json` yapılandırmasını değiştirir Likewise, program will quit if you delete or rename it.

You can disable this behaviour with `--no-config-watch` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments#arguments)** if you have a specific reason, for example you don't want from ASF to react to file changes in `config` folder.
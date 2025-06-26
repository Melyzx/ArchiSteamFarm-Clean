# SSS

Temel SSS bölümümüz, aklınıza gelebilecek standart soruları ve cevapları kapsar. Daha az yaygın olan konular için lütfen bunun yerine [**genişletilmiş SSS**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Extended-FAQ) bölümümüzü ziyaret edin.

# İçindekiler

* [Genel](#general)
* [Benzer araçlarla karşılaştırma](#comparison-with-similar-tools)
* [Güvenlik / Gizlilik / VAC / Banlanmalar / Kullanım Koşulları](#security--privacy--vac--bans--tos)
* [Çeşitli](#misc)
* [Sorunlar](#issues)

---

## Genel

### ASF nedir?
### Program neden hesabımda çiftçilik yapacak hiçbir şey olmadığını iddia ediyor?
### ASF neden tüm oyunlarımı algılamıyor?
### Hesabım neden sınırlı?

ASF'nin ne olduğunu anlamaya çalışmadan önce, Steam kartlarının ne olduğunu ve bunların nasıl elde edileceğini anladığınızdan emin olmalısınız; [**buradan**](https://steamcommunity.com/tradingcards/faq) resmi SSS sayfasına bakabilirsiniz güzel bir şekilde açıklanmıştır.

Kısacası Steam kartları, belirli bir oyuna sahip olduğunuzda almaya hak kazandığınız koleksiyon öğeleridir ve rozetler oluşturmak, Steam pazarında satış yapmak veya seçtiğiniz başka herhangi bir amaç için kullanılabilir.

Temel noktalar burada bir kez daha belirtilmiştir, çünkü insanlar genel olarak onlarla aynı fikirde olmak istemezler ve bunların sanki yokmuş gibi davranmayı tercih ederler:

- **Herhangi bir kart düşüşüne hak kazanabilmek için Steam hesabınızda oyuna sahip olmanız gerekir. Aile paylaşımı kart hakkı vermez.**
- **Oyununuz şu şekilde işaretlenemiyor: [özel](https://help.steampowered.com/faqs/view/1150-C06F-4D62-4966)ASF, çiftçilik sırasında bu tür oyunları otomatik olarak atlayacaktır.**
- **Oyuna sonsuz bir çiftçilik yapamazsınız, her oyunda sabit sayıda kart düşüşü vardır. Bütün kartları düşürdüğünüzde (tam setin yaklaşık yarısı), oyuna artık çiftçilik yapamazsınız. Aldığınız kartlara ne olduğunu satıp satmadığınız, takas edip etmediğiniz, üretip üretmediğiniz veya unuttuğunuz önemli değil, kart düşüşleriniz bittiğinde çiftçilik biter.**
- **F2P oyunlarından hiç para harcamadan kart düşüremezsiniz. Bu, Team Fortress 2 veya Dota 2 gibi kalıcı olarak F2P oyunları anlamına gelir. F2P oyunlarına sahip olmak size kart düşürme hakkı vermez.**
- **Sahip olunan oyunlardan bağımsız olarak [sınırlı hesaba](https://help.steampowered.com/faqs/view/71D3-35C2-AD96-AA3A) sahipseniz kart düşüremezsiniz. Geçmişte mümkündü, ancak artık mümkün değil.**
- **Bir promosyon sırasında ücretsiz olarak edindiğiniz ücretli oyunlar, mağaza sayfasında ne görüntülendiğine bakılmaksızın kart düşmeleri için uygun değildir. Geçmişte mümkündü, ancak artık mümkün değil.**

Gördüğünüz gibi, satın aldığınız bir oyunu veya para yatırdığınız F2P oyununu oynadığınız için Steam kartları size verilir. Böyle bir oyunu yeterince uzun süre oynarsanız, o oyun için tüm kartlar sonunda envanterinize düşecek ve bir rozeti tamamlamanızı (setin kalan yarısını aldıktan sonra), satmanızı veya başka ne isterseniz yapmanızı mümkün kılacaktır.

Artık Steam'in temellerini açıkladığımıza göre ASF'yi açıklayabiliriz. Programın kendisini tam olarak anlamak oldukça karmaşıktır, bu nedenle tüm teknik ayrıntıları araştırmak yerine aşağıda çok basitleştirilmiş bir açıklama sunacağız.

ASF, sağladığınız Steam hesap bilgilerinizi kullanarak yerleşik, özel Steam istemcisi uygulamamız aracılığıyla Steam hesabınıza giriş yapar. Bütün sayfaları inceleyip kart düşürmeye uygun oyunlar listesini oluşturduktan sonra, ASF en verimli kart düşürme algoritmasını seçer ve işlemi başlatır. Başarılı bir şekilde giriş yaptıktan sonra, çiftçilik yapmaya uygun oyunları bulmak için [**rozet**](https://steamcommunity.com/my/badges) sayfanızı araştırır (`X` düşecek kart sayısı). Süreç, seçilen [**çiftçilik algoritmasına**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance) bağlıdır, ancak genellikle uygun oyunu oynamaktan ve oyunun düşecek kartı olup olmadığını periyodik olarak (ayrıyeten her kart düşüşünde) kontrol etmekten oluşur \- eğer hayır ise ASF, tüm oyunlar tamamen çiftçilik yapılana kadar aynı prosedürü kullanarak bir sonraki oyuna devam edebilir.

Unutmayın, yukarıdaki açıklama basitleştirilmiştir ve ASF'nin sunduğu düzinelerce ekstra özelliği ve işlevi açıklamamaktadır. Eğer ASF ile alakalı her detayı bilmek istiyorsanız [**Wiki sayfamızı**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki) ziyaret ediniz. Teknik detaylara girmeden, herkesin anlayabileceği şekilde açıklamaya çalıştım \- uzman kullanıcıların daha derine gitmesi desteklenmektedir.

Şimdi, ASF bir program olarak bazı büyülü özellikler sunar. İlk olarak, hiçbir oyunu indirmenizi gerektirmez, oyunları direk oynayabilir. İkinci olarak, normal Steam istemcinizden tamamen bağımsızdır \- Steam istemcisini çalıştırmanız gerekmez ve hatta Steam istemcisini kurmanızı bile gerektirmez. Üçüncü olarak, otomatikleştirilmiş bir çözümdür \- yani haberiniz bile olmadan ASF otomatik olarak herşeyi halleder, ne yapması gerektiğini söylemenize ihtiyaç duymaz \- ki zamanınızı ve çabanızı boşa harcamaz. Son olarak, Steam ağını benzetim işlemi ile kandırmak zorunda değildir (mesela Idle Master programı bunu yapar), çünkü Steam ağı ile direk iletişim kurabilmektedir. Ayrıca çok hızlı ve hafif olup, çok uğraşmadan kolayca kart düşürmek isteyen herkes için harika bir çözümdür \- özellikle başka birşeyler yaparken arkaplanda çalışması veya çevrimdışı oynarken bile çalışması kullanışlıdır.

Yukarıdaki herşey çok güzel, ancak ASF'nin ayrıca Steam tarafından uygulanan bazı teknik sınırlamaları da var \- sahip olmadığınız oyunlardan kart düşüremeyiz, sonsuza dek programı çalıştırıp oyunların kart düşürme limitiden daha fazla kart düşüremeyiz, ayrıca siz oyun oynarken kart düşüremeyiz. ASF'nin çalışma şekli göz önüne alındığında bunların hepsi "mantıklı" olmalıdır, ancak ASF'nin süper güçlere sahip olmadığını ve fiziksel olarak imkansız olan bir şeyi yapmayacağını belirtmek güzel, bu yüzden bunu aklınızda bulundurun \- temelde bu Sanki birisine başka bir bilgisayardan hesabınıza giriş yapmasını ve bu oyunları sizin için oynamasını söylemişsiniz gibi.

Özetlemek gerekirse \- ASF, hak kazandığınız kartları çok fazla güçlük çekmeden düşürmenize yardımcı olan bir programdır. Ayrıca başka işlevler de sunar, ancak şimdilik buna bağlı kalalım.

---

### Hesap bilgilerimi yazmam gerekiyor mu?

**Evet**. ASF, resmi Steam istemcisinin yaptığı gibi hesap kimlik bilgilerinizi gerektirir çünkü Steam Ağı etkileşimi için aynı yöntemi kullanmaktadır. Ancak bu, hesap bilgilerinizi ASF yapılandırmalarına koymanız gerektiği anlamına gelmez; ASF'yi `geçersiz`/boş `SteamOturum aç` ve/veya `SteamŞifresi` ile kullanmaya devam edebilir ve gerektiğinde her ASF çalıştırmasında bu verileri girin (diğer bazı oturum açma kimlik bilgilerinin yanı sıra, yapılandırmaya bakın). Bu şekilde bilgileriniz hiçbir yere kaydedilmez, ancak elbette ASF sizin yardımınız olmadan otomatik olarak başlatılamaz. ASF ayrıca [**güvenliğinizi**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security) artırmanın birkaç başka yolunu da sunar, eğer ileri düzey kullanıcıysanız wiki'nin bu bölümünü okumaktan çekinmeyin. Değilseniz ve hesap bilgilerinizi ASF yapılandırmalarına koymak istemiyorsanız, bunu yapmayın ve bunun yerine ASF istediğinde bunları gerektiği gibi girin.

ASF aracının kişisel kullanımınız için olduğunu ve kimlik bilgilerinizin bilgisayarınızı asla terk etmediğini unutmayın. Ayrıca bunları kimseyle paylaşmıyorsunuz, bu da [**Steam Hizmet Şartları**](https://store.steampowered.com/subscriber\_agreement)'nı karşılıyor. \- birçok insanın unuttuğu çok önemli bir şey. Bilgilerinizi sunucularımıza veya üçüncü taraflara göndermiyorsunuz, yalnızca doğrudan Valve tarafından işletilen Steam sunucularına gönderiyorsunuz. Kimlik bilgilerinizi bilmiyoruz ve bunları yapılandırmalarınıza koysanız da koymasanız da sizin için kurtaramıyoruz.

---

### Kartların düşmesi için ne kadar beklemem gerekiyor?

**Gerektiği sürece** \- ciddi anlamda. Her oyunun geliştiricisi/yayıncısı tarafından belirlenen farklı kart düşürme zorluğu seviyeleri vardır ve kartların ne kadar hızlı düşürüldüğü tamamen onlara bağlıdır. Oyunların çoğunda her 30 dakikada bir 1 düşme gerçekleşir, ancak bir kartı düşürmeden önce birkaç saat bile oynamanızı gerektiren oyunlar da vardır. Buna ek olarak, [**performans**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance) bölümünde belirtildiği gibi, henüz yeterli süre oynamadığınız oyunlardan hesabınızın kart düşüşü alması da kısıtlanabilir. Lütfen ASF'nin bir oyunu tamamlamasının ne kadar süreceğini tahmin etmeye çalışmayın; karar vermek ne size ne de ASF'ye bağlı değildir. Bunu daha hızlı hale getirmek için yapabileceğiniz hiçbir şey yoktur ve kartların zamanında düşürülmemesiyle ilgili bir "hata" yoktur \- kartların düşme sürecini siz kontrol etmezsiniz, ASF de bunu yapmaz. En iyi durumda ortalama her 30 dakikada bir 1 kart alabilirsiniz. En kötü senaryoda, ASF'yi başlattıktan sonra 4 saat boyunca kart alamayabilirsiniz. Bu durumların her ikisi de normaldir ve [**Performans**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance) bölümümüzde ele alınmaktadır.

---

### Kart düşürme çok uzun sürüyor, bir şekilde hızlandırabilir miyim?

Çiftçilik hızını büyük ölçüde etkileyen tek şey, bot örneğiniz için seçilen \*\*[kart çiftçiliği algoritmasıdır](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance). Diğer her şeyin etkisi ihmal edilebilir düzeydedir ve çiftçiliği daha hızlı hale getirmezken, ASF sürecini birkaç kez başlatmak gibi bazı eylemler bile çiftçiliği hızlandıracaktır. **kötüleştir**. Eğer çiftçilik sürecinin her bir saniyesini gerçekten kazanma isteğiniz varsa, o zaman ASF bazı temel çiftçilik değişkenlerine ince ayar yapmanızı sağlar: `ÇiftçilikGecikme` \- hepsi şurada açıklanmıştır [**yapılandırma**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration). Bununla birlikte, söylediğim gibi, etki göz ardı edilebilir ve belirli bir hesap için uygun kart çiftçiliği algoritmasını seçmek, çiftçilik hızını büyük ölçüde etkileyebilecek tek önemli seçimdir, geri kalan her şey tamamen kozmetiktir. Çiftçilik hızı konusunda endişelenmek yerine, ASF'yi başlatın ve işini yapmasına izin verin; bunu aklıma gelen en etkili şekilde yaptığına sizi temin ederim. Ne kadar az önemserseniz o kadar tatmin olursunuz.

---

### Ancak ASF, çiftçiliğin yaklaşık X zaman alacağını söyledi\!

ASF, düşürmeniz gereken kart sayısına ve seçtiğiniz algoritmaya bağlı olarak size kaba bir tahmin sunar; bu, çiftçilik için harcayacağınız gerçek süreye hiçbir şekilde yakın değildir; ASF yalnızca en iyi durumu varsaydığından, bu genellikle bundan daha uzundur ve tüm Steam Ağı tuhaflıklarını, internet bağlantılarını, Steam sunucularının aşırı yüklenmesini ve benzerlerini göz ardı eder. Bu, ASF'nin ne kadar süreyle tarım yapmasını bekleyebileceğinizin yalnızca genel bir göstergesi olarak görülmelidir; çoğu zaman en iyi durumda, gerçek zaman bazı durumlarda önemli ölçüde farklılık gösterse de farklılık gösterecektir. Yukarıda belirtildiği gibi, belirli bir oyunun ne kadar süreyle çiftçilik yapacağını tahmin etmeye çalışmayın, bu size bağlı değil, ne de ASF'nin kararı.

---

### ASF android veya akıllı telefonumda çalışabilir mi?

ASF, .NET'in çalışma uygulamasını gerektiren bir C\# programıdır. Android, .NET 6.0'dan başlayarak geçerli bir platform haline geldi, ancak şu anda ASF'nin Android'de gerçekleşmesinde büyük bir engel var. [**üzerinde ASP.NET çalışma zamanının bulunmaması**](https://github.com/dotnet/aspnetcore/issues/35077). Yerel bir seçenek mevcut olmasa da, ARM mimarisinde GNU/Linux için uygun ve çalışan yapılar mevcuttur, bu nedenle şunun gibi bir şey kullanmak tamamen mümkündür: [**Linux Dağıtımı**](https://play.google.com/store/apps/details?id=ru.meefik.linuxdeploy) Linux'u kurmak için, ardından her zamanki gibi Linux chroot'unda ASF'yi kullanmak için.

Tüm ASF gereksinimleri karşılandığında/eğer karşılanırsa, resmi bir Android yapısı yayınlamayı değerlendireceğiz.

---

### ASF, CS:GO veya Unturned gibi Steam oyunlarındaki öğeleri toplayabilir mi?

**HAYIR**, bu karşı [**Steam Hizmet Şartları**](https://store.steampowered.com/subscriber\_agreement) ve Valve, TF2 öğelerini çiftçiliğe yönelik son topluluk yasaklama dalgasıyla birlikte bunu açıkça belirtti. ASF, oyun öğeleri çiftçisi değil, bir Steam kartları yetiştirme programıdır \- oyun öğeleri yetiştirme konusunda herhangi bir yeteneği yoktur ve esas olarak Steam kullanım koşullarının ihlali nedeniyle bu tür bir özelliğin gelecekte eklenmesi planlanmamaktadır. Lütfen bu konuda soru sormayın \- alabileceğiniz en iyi şey, sorun yaşayan bazı tuzlu kullanıcılardan gelen bir rapordur. ASF yalnızca Steam takas kartlarına odaklanıyor. Aynı şey, CS:GO yayınlarındaki çiftçilik damlaları gibi diğer tüm çiftçilik türleri için de geçerlidir.

---

### Hangi oyunların farmlanması gerektiğini seçebilir miyim?

**Evet**, birkaç farklı yolla. Farming kuyruğunun varsayılan sırasını değiştirmek istiyorsanız, işte yapmanız gereken budur `TarımSiparişleri` [**bot yapılandırma özelliği**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration\#bot-config) için kullanılabilir. Verilen oyunların otomatik olarak çiftçilik yapılmasını engellemek için manuel olarak kara listeye almak istiyorsanız, aşağıdakilerle birlikte sunulan boşta kara listeyi kullanabilirsiniz: `Facebook` [**emretmek**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands). Her şeyi çiftçilik yapmak ancak bazı uygulamalara her şeyden önce öncelik vermek istiyorsanız, boşta öncelik sırasının mevcut olduğu şey budur. `fq` [**emretmek**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands) için kullanılabilir. Ve son olarak, yalnızca seçtiğiniz belirli oyunları farmlamak istiyorsanız, bunu beyan edebilirsiniz. `FarmPriorityQueueYalnızca` botta [**`ÇiftçilikTercihler`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration\#farmingpreferences) Bunu başarmak için, seçtiğiniz uygulamalarınızı boşta öncelik sırasına eklemekle birlikte.

Yukarıda açıklanan otomatik kart yetiştirme modülünü yönetmenin yanı sıra, ASF'yi aşağıdaki düğmelerle manuel tarım moduna da geçirebilirsiniz: `oynamak` [**emretmek**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)veya aşağıdakiler gibi başka çeşitli harici ayarları kullanın: `Boştayken Oynanan Oyunlar` [**bot yapılandırma özelliği**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration\#bot-config).

---

### Kart düşüşleriyle ilgilenmiyorum, bunun yerine oynanan saatleri farmlamak istiyorum, bu mümkün mü?

Evet, ASF bunu en azından birkaç yolla yapmanıza olanak tanır.

Bunu başarmanın en iyi yolu kullanmaktır [**`Boştayken Oynanan Oyunlar`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration\#gamesplayedwhileidle) ASF'nin gruplayacak kartı olmadığında seçtiğiniz uygulama kimliklerini oynatacak yapılandırma özelliği. If you'd like to play your games all the time, even if you do have card drops from other games, then you can combine it with **[`FarmPriorityQueueOnly`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#farmingpreferences)**, so ASF will farm only those games for card drops that you explicitly set, or, alternatively, **[`FarmingPausedByDefault`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#farmingpreferences)**, which will cause cards farming module to be paused until you unpause it yourself.

You can also make use of the **[`play`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands#commands-1)** command, which will cause ASF to play your selected games. Ancak şunu unutmayın `oynamak` ASF'nin varsayılan duruma geri dönmesine neden olan kalıcı bir durum olmadığından yalnızca geçici olarak oynamak istediğiniz oyunlar için kullanılmalıdır; Steam ağıyla bağlantı kesildiğinde. Bu nedenle kullanmanızı öneririz. `Boştayken Oynanan Oyunlar`Seçtiğiniz oyunları gerçekten kısa bir süreliğine başlatmak ve ardından genel akışa geri dönmek istemiyorsanız.

---

### Linux/macOS kullanıcısıyım, işletim sistemim için mevcut olmayan ASF oyunları farmlayacak mı? ASF, 32 bit işletim sistemi üzerinde çalıştırdığımda 64 bit oyunları çalıştıracak mı?

Evet, ASF gerçek oyun dosyalarını indirme zahmetine bile girmiyor, dolayısıyla herhangi bir platform veya teknik gereksinimden bağımsız olarak Steam hesabınıza bağlı tüm lisanslarınızla çalışacaktır. Ayrıca, eşleşen bölgede olmasanız bile belirli bir bölgeye bağlı oyunlar (bölge kilitli oyunlar) için de çalışmalıdır, ancak bunu garanti etmiyoruz (en son denediğimizde işe yaradı).

---

## Benzer araçlar ile kıyaslama

---

### ASF Idle Master'a benzer mi?

Tek benzerlik, her iki programın da genel amacıdır; kart düşüşü almak için Steam oyunlarını yetiştirmektir. Gerçek çiftçilik yöntemi, program yapısı, işlevsellik, uyumluluk, kullanılan algoritmalar, özellikle de kaynak kodunun kendisi de dahil olmak üzere diğer her şey tamamen farklıdır ve bu iki programın birbiriyle hiçbir ortak yanı yoktur, hatta çekirdek temeli bile \- IM .NET üzerinde çalışıyor Çerçeve, .NET'te ASF (Çekirdek). ASF, basit bir kod düzenlemesiyle çözülmesi mümkün olmayan IM sorunlarını çözmek için oluşturuldu \- bu nedenle ASF, tek bir kod satırı veya hatta IM'den gelen genel bir fikir kullanılmadan sıfırdan yazıldı, çünkü o kod ve bu fikirler tamamen kusurluydu. başlamak için. IM ve ASF, Windows ve Linux gibidir; her ikisi de işletim sistemleridir ve her ikisi de bilgisayarınıza yüklenebilir, ancak benzer amaçlara hizmet etmek dışında birbirleriyle neredeyse hiçbir şeyi paylaşmazlar.

Bu aynı zamanda IM beklentilerine dayalı olarak ASF'yi IM ile karşılaştırmamanızın nedenidir. ASF ve IM'yi kendilerine özgü özelliklere sahip tamamen bağımsız programlar olarak değerlendirmelisiniz. Bazıları gerçekten örtüşüyor ve her ikisinde de belirli bir özellik bulabilirsiniz, ancak çok nadiren, çünkü ASF, IM'ye kıyasla tamamen farklı bir yaklaşımla amacına hizmet ediyor.

---

### Şu anda Idle Master kullanıyorsanız ve benim için iyi çalışıyorsa, ASF kullanmaya değer mi?

**Evet**, içinde açıklanmıştır [**komutlar**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands\#notes) bölüm. Bunu Steam grup sohbeti aracılığıyla yapabilirsiniz, ancak [**ASF**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC\#asf-ui) senin için daha kolay olabilir.

ASF'nin uygun mantığı var **yayınlanmamış oyunlar** \- IM, henüz piyasaya sürülmemiş olsalar bile, halihazırda eklenmiş kartlara sahip oyunları toplamaya çalışacaktır. Tabii ki, bu oyunları çıkış tarihine kadar farmlamanız mümkün olmadığından, farmlama süreciniz sıkışacaktır. Bu, onu kara listeye eklemenizi, yayınlanmasını beklemenizi veya manuel olarak atlamanızı gerektirir. Bu çözümlerin hiçbiri iyi değil ve hepsi sizin dikkatinizi gerektiriyor \- ASF, yayınlanmamış oyunların (geçici olarak) çiftçiliğini otomatik olarak atlıyor ve daha sonra oyun çıktıklarında onlara geri dönerek sorunu tamamen ortadan kaldırır ve sorunla verimli bir şekilde ilgilenir.

ASF'nin ayrıca uygun bir mantığı vardır. **seri** videolar. Steam'de kartları olan ancak yanlış duyurulan birçok video var `uygulama kimliği` Rozetler sayfasında, örneğin [**Çift Güzel Macera**](https://store.steampowered.com/app/402590) \- IM yanlışlıkla yanlış farm yapacak `uygulama kimliği`, bu hiçbir düşüşe neden olmaz ve süreç takılıp kalır. Bir kez daha, ya kara listeye almanız ya da manuel olarak atlamanız gerekecek; her ikisi de dikkatinizi gerektiriyor. ASF otomatik olarak uygun olanı keşfeder `uygulama kimliği` çiftçilik için kart düşmesine neden olur.

Buna ek olarak ASF **çok daha istikrarlı ve güvenilir** ağ sorunları ve Steam tuhaflıkları söz konusu olduğunda çoğu zaman çalışır ve yapılandırıldıktan sonra hiçbir şekilde ilgilenmenizi gerektirmez, oysa IM çoğu zaman birçok kişi için bozulur, ekstra düzeltmeler gerektirir veya ne olursa olsun çalışmaz. Aynı zamanda tamamen Steam istemcinize bağlıdır; bu, Steam istemcinizde herhangi bir ciddi sorun yaşandığında çalışamayacağı anlamına gelir. ASF, Steam ağına bağlanabildiği sürece düzgün çalışıyor ve Steam istemcisinin çalıştırılmasını ve hatta kurulmasını gerektirmiyor.

Bunlar 3 **çok önemli** ASF'yi neden kullanmayı düşünmeniz gerektiğine dair bazı noktalar var çünkü bunlar Steam kartlarını yetiştiren herkesi doğrudan etkiliyor ve Steam bakımları ve tuhaflıklar herkesin başına geldiğinden "bu beni dikkate almıyor" demenin bir yolu yok. SSS'nin geri kalanında öğrenebileceğiniz daha az ve daha önemli düzinelerce neden var. Yani kısaca konuşursak, **Evet**, IM ile karşılaştırıldığında mevcut olan herhangi bir ekstra ASF özelliğine ihtiyacınız olmadığında bile ASF'yi kullanmalısınız.

Buna ek olarak, IM resmi olarak durdurulmuştur ve çok daha güçlü çözümlerin (yalnızca ASF değil) varlığı göz önüne alındığında, gelecekte kimsenin düzeltme zahmetine girmesine gerek kalmadan tamamen bozulabilir. IM zaten pek çok insan için çalışmıyor ve bu sayı yalnızca artıyor, azalmıyor. İlk etapta eski yazılımları, yalnızca IM'yi değil, aynı zamanda kullanımdan kaldırılmış diğer tüm programları da kullanmaktan kaçınmalısınız. Aktif bakımcının olmaması, hiç kimsenin çalışıp çalışmadığını umursamadığı anlamına gelir ve **işlevselliğinden hiç kimse sorumlu değildir**Bu da güvenlik açısından çok önemli bir konu. Steam altyapısında gerçek sorunlara neden olan kritik bir hatanın olması yeterlidir \- kimse bunu düzeltmezse, Steam, bunun bir sorun olduğunun farkında bile olmadan, kullanan insanlara zaten olduğu gibi, başka bir yasaklama dalgası yayınlayabilir. , tahmin edin ne oldu, ASF'nin eski versiyonu.

---

### ASF, Idle Master'in sunmadığı hangi özellikleri sunuyor?

Sizin için neyin "ilginç" olduğunu düşündüğünüze bağlı. Birden fazla hesap oluşturmayı planlıyorsanız, cevap zaten açıktır çünkü ASF, tüm hesapları tek bir üstün çözümle toplamanıza, kaynaklardan, güçlüklerden ve uyumluluk sorunlarından tasarruf etmenize olanak tanır. Ancak bu soruyu soruyorsanız büyük olasılıkla bu özel ihtiyacınıza sahip değilsiniz, o halde ASF'de kullanılan tek bir hesap için geçerli olan diğer faydaları değerlendirelim.

Her şeyden önce, bahsettiğiniz bazı yerleşik özelliklere sahipsiniz [**üstünde**](\#is-it-worth-it-to-use-asf-if-im-currently-using-idle-master-and-it-works-fine-for-me) Bunlar, nihai hedefiniz ne olursa olsun çiftçiliğin temelidir ve çoğu zaman tek başına bu, ASF'yi kullanmayı düşünmek için yeterlidir. Ancak bunu zaten biliyorsunuz, o halde daha ilginç özelliklere geçelim:

- **Çevrimdışı çiftçilik yapabilirsiniz** (`Çevrimiçi durum` içinde `Çevrimdışı` ayar). Çevrimdışı çiftçilik yapmak, Steam oyun içi durumunuzu tamamen atlamanızı mümkün kılar; bu, arkadaşlarınız ASF'nin sizin adınıza bir oyun oynadığını bile fark etmeden, aynı anda Steam'de "Çevrimiçi" gösterirken ASF ile çiftçilik yapmanıza olanak tanır. Bu üstün bir özelliktir, çünkü Steam istemcinizde çevrimiçi kalmanızı sağlarken, sürekli oyun değişiklikleriyle arkadaşlarınızı rahatsız etmez veya gerçekte oyun oynamadığınız halde oyun oynadığınızı düşünmeleri için onları yanıltmaz. Tek başına bu nokta, eğer kendi arkadaşlarınıza saygı duyuyorsanız, ASF'yi kullanmayı değerli kılar, ancak bu yalnızca başlangıçtır. Bu özelliğin Steam'in gizlilik ayarlarıyla hiçbir ilgisi olmadığını da belirtmekte fayda var; oyunu kendiniz başlatırsanız, arkadaşlarınıza oyun içi olarak düzgün bir şekilde görünürsünüz, yalnızca ASF kısmını görünmez yaparsınız ve hesabınızı hiçbir şekilde etkilemezsiniz. .

- **İade edilebilir oyunları atlayabilirsiniz** (`İade EdilebilirOyunları Atla` botta `ÇiftçilikTercihler` özellik). ASF, iade edilebilir oyunlar için uygun yerleşik mantığa sahiptir ve ASF'yi, iade edilebilir oyunları otomatik olarak farmlamayacak şekilde yapılandırabilirsiniz. Bu, ASF'nin mümkün olan en kısa sürede kartları düşürmeye çalışmasına gerek kalmadan, Steam mağazasından yeni satın aldığınız oyununuzun paranıza değip değmediğini kendinizi değerlendirmenize olanak tanır. Oyunu 2 saatten fazla oynarsanız veya satın alma işleminizin üzerinden 2 hafta geçerse, artık iade yapılamayacağı için ASF o oyuna devam edecektir. O zamana kadar, beğenip beğenmediğiniz konusunda tam kontrole sahip olursunuz ve gerekirse oyunu manuel olarak kara listeye almanıza veya tüm süre boyunca ASF'yi kullanmamanıza gerek kalmadan kolayca iade edebilirsiniz.

- **Oynanmamış oyunları atlayabilirsiniz** (`OynanmamışOyunları Atla` botta `ÇiftçilikTercihler` özellik). ASF, saatlerce oyun oynamak için uygun bir yerleşik mantığa sahiptir ve ASF'yi, oynanmamış oyunları otomatik olarak düzenlemeyecek şekilde yapılandırabilirsiniz. Bu, oynadığınız ve farm yaptığınız oyunları, hepsini manuel olarak kara listeye almanıza veya ASF'yi tamamen kullanmayı atlamanıza gerek kalmadan kontrol etmenize olanak tanır.

- **Yeni öğeleri otomatik olarak alındı ​​olarak işaretleyebilirsiniz** (`Bot Davranışı` ile ilgili `Envanter Bildirimlerini Kapat` özellik). ASF ile çiftçilik yapmak, hesabınızın yeni kart düşmesiyle sonuçlanacaktır. Bunun olacağını zaten biliyorsunuz, bu yüzden ASF'nin bu işe yaramaz bildirimi sizin için temizlemesine izin verin ve yalnızca önemli şeylerin dikkatinizi çekmesini sağlayın. Tabii ki, sadece istersen.

- **Tercih edilen çiftçilik sırasını daha fazla seçenekle özelleştirebilirsiniz** (`TarımSiparişleri` özellik). Belki de ilk önce yeni satın aldığınız oyunları farmlamak istersiniz? Yoksa en yaşlılarınız mı? Kart düşme sayısına göre mi? Zaten oluşturduğunuz rozet seviyeleri? Saatlerce oynandı mı? Alfabetik olarak mı? AppID'lere göre mi? Ya da belki tamamen rastgele? Buna tamamen karar vermek size kalmış.

- **ASF setlerinizi tamamlamanıza yardımcı olabilir** (`TicaretTercihleri` ile `SteamTradeMatcher` özellik). Biraz daha gelişmiş müdahalelerle ASF'nizi otomatik olarak kabul edecek tam özellikli kullanıcı robotuna dönüştürebilirsiniz. [**STM**](https://www.steamtradematcher.com) teklifler, her gün herhangi bir kullanıcı etkileşimi olmadan setlerinizi eşleştirmenize yardımcı olur. ASF, Steam mobil kimlik doğrulayıcınızı içe aktarmanıza ve onayları kabul ederek tüm süreci tamamen otomatikleştirmenize olanak tanıyan kendi ASF 2FA modülünü bile içerir. Ya da belki manuel olarak kabul edip ASF'nin bu işlemleri yalnızca sizin için hazırlamasına izin vermek istersiniz? Bu bir kez daha karar vermek tamamen size kalmış.

- **ASF sizin için arka planda anahtarları kullanabilir** ([**arka plan oyunları kurtarıcı**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer) özellik). Belki de çeşitli paketlerden, kendinizi kullanamayacak kadar tembel olduğunuz, birçok pencereden geçerek Steam hüküm ve koşullarını tekrar tekrar kabul ettiğiniz yüzlerce anahtarınız vardır. Neden anahtar listenizi ASF'ye kopyalayıp yapıştırıp işini yapmasına izin vermiyorsunuz? Why not copy-paste your list of keys into ASF and let it do its job? ASF, arka planda tüm bu anahtarları otomatik olarak kullanacak ve her bir kullanma girişiminin nasıl sonuçlandığını size bildirecek uygun çıktıyı sağlayacaktır. Üstelik yüzlerce anahtarınız varsa, er ya da geç Steam tarafından hız sınırlamasına tabi tutulacağınız garantidir ve ASF de bunu biliyor, sabırla hız sınırının kalkmasını bekleyecek ve kaldığı yerden devam edecektir.

Artık tamamıyla devam edebiliriz [**ASF wiki'si**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home), programın her bir özelliğine işaret ediyor, ancak bir yere çizgi çekmemiz gerekiyor. İşte bu, ASF kullanıcısı olarak keyfini çıkarabileceğiniz özelliklerin bir listesi; bunlardan sadece bir tanesi kolaylıkla geriye bakmamak için önemli bir neden olarak değerlendirilebilir ve biz aslında listeledik **çok fazla** Bunlardan daha fazlasını wiki'mizin geri kalanında öğrenebilirsiniz. Ah evet ve kendi komutlarınızı yazmanıza izin veren ASF API'si veya harika bot yönetimi gibi şeylerin ayrıntısına bile girmedik çünkü işi basit tutmak istedik.

---

### ASF Idle Master'dan daha hızlı mıdır?

**Evet**, açıklama oldukça karmaşık olsa da.

Sisteminizde oluşturulan ve sonlandırılan her yeni işlemde, Steam istemcisi otomatik olarak o anda oynamakta olduğunuz tüm oyunlarınızı içeren bir istek gönderir \- bu şekilde buhar ağı saatleri hesaplayabilir ve kartların düşmesini sağlayabilir. Ancak Steam ağı, oynadığınız süreyi 1 saniyelik aralıklarla sayar ve yeni istek gönderildiğinde mevcut durum sıfırlanır. Başka bir deyişle, her 0,5 saniyede bir yeni işlem oluşturup/öldürürseniz, hiçbir zaman kart düşürmezsiniz çünkü her 0,5 saniyede bir buhar istemcisi yeni bir istek gönderir ve buhar ağı asla 1 saniyelik oyun süresini bile saymaz. Üstelik, işletim sisteminin çalışma şekli nedeniyle, aslında siz hiçbir şey yapmadan yeni süreçlerin oluşturulduğunu/sonlandırıldığını görmek oldukça yaygındır; yani bilgisayarınızda hiçbir şey yapmasanız bile, arka planda çalışmaya devam eden birçok süreç vardır. /diğer işlemleri her zaman sonlandırmak. Idle master, Steam istemcisini temel alır, dolayısıyla bu mekanizma, onu kullanıyorsanız sizi etkiler.

ASF, buhar istemcisine dayalı değildir, kendi buhar istemci uygulamasına sahiptir. Bu sayede ASF'nin yaptığı şey herhangi bir süreç başlatmak değil, aslında oyun oynamaya başladığımız Steam ağına gerçek bir istek göndermek. Bu, Steam istemcisi tarafından gönderilen isteğin aynısıdır, ancak ASF buhar istemcisi üzerinde gerçek kontrole sahip olduğumuz için, yeni süreçler oluşturmamıza gerek yok ve her süreç değişikliğinde istek gönderme konusunda Steam istemcisini taklit etmiyoruz. Yani yukarıda açıklanan mekanizma bizi etkilemiyor. Bu sayede steam web tarafında o 1 saniyelik aralığı asla kesmiyoruz ve bu da çiftçilik hızımızı arttırıyor.

---

### Ancak fark gerçekten fark edilir mi?

Hayır. Normal Steam istemcisi ve boşta kalan master ile meydana gelen kesintilerin kart düşmeleri üzerinde ihmal edilebilir bir etkisi vardır, dolayısıyla ASF'yi üstün kılacak gözle görülür bir fark yoktur.

Ancak orada **dır-dir** bir fark vardır ve işletim sisteminizin yoğunluğuna bağlı olarak kartların **irade** Çok şanssızsanız birkaç saniyeden birkaç dakikaya kadar daha hızlı düşer. Her ne kadar ASF'yi yalnızca kartları daha hızlı düşürdüğü için kullanmayı düşünmesem de, hem ASF hem de Idle Master, steam web'in çalışma şeklinden etkilendiğinden, ASF, steam web ile daha etkili bir şekilde etkileşime girerken, Idle Master, steam istemcisinin gerçekte ne olduğunu kontrol edemez. yapıyor (yani bu Idle Master'ın hatası değil, buhar istemcisinin kendisi).

---

### ASF aynı anda birçok oyunu çalıştırabilir mi?

**Evet**, fakat seçilen \*\*[kart çalıştırma algoritması](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)\*\*na göre ASF'nin bu özelliği ne zaman kullanılacağını daha iyi bilir. Birden fazla oyunda kart düşürme oranı sıfıra yakın olduğundan ASF, bu zorlukların üstesinden gelmek için saatlerce birden fazla oyun çiftçiliği kullanıyor. `Kartın Düşmesine Kalan Saatler` kadar daha hızlı `32` oyunları bir kerede Bu aynı zamanda ASF'nin yapılandırma kısmına odaklanmanızın ve hedefe ulaşmanın en iyi yolunun ne olduğuna algoritmaların karar vermesine izin vermenizin nedenidir \- doğru olduğunu düşündüğünüz şey gerçekte doğru olmayabilir, birden fazla oyunu aynı anda çalıştırmak size bir şey kazandırmayacaktır. herhangi bir kart düşüşüyle ​​birlikte.

---

### ASF oyunları hızlıca atlayabilir mi?

**Hayır**, ASF [**Steam açıklarını**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance\#steam-glitches) desteklemez ve kullanmayı önermez.

---

### ASF, kartlar eklenmeden önce her oyunu X saat boyunca otomatik olarak oynayabilir mi?

**HAYIR**Steam kartları sistem değişikliğinin asıl amacı yanlış istatistikler ve hayalet oyuncularla mücadele etmekti. ASF buna gereğinden fazla katkıda bulunmayacak, böyle bir özelliğin eklenmesi planlanmadı ve gerçekleşmeyecek. Oyununuz her zamanki gibi kart düşüşü alıyorsa, ASF bunları mümkün olan en kısa sürede toplayacaktır.

---

### ASF çalışırken oyun oynayabilir miyim?

**Hayır**. ASF, IM'den farklı olarak bağımsız Steam istemcisine sahiptir ve Steam ağı yalnızca **aynı anda bir Steam istemcisi** oyun oynamak. Bununla birlikte, istediğiniz zaman bir oyunu başlatarak (ve Steam ağının diğer istemciyle bağlantısının kesilmesi gerekip gerekmediği sorulduğunda "Tamam"ı tıklayarak) ASF bağlantısını kesebilirsiniz \- ASF, oyun bitene kadar sabırla bekleyecek ve daha sonra işleme devam edecektir. Alternatif olarak, eğer sizin için tatmin ediciyse, istediğiniz zaman çevrimdışı modda oynamaya devam edebilirsiniz.

Birden fazla oyun oynarken kartların düşme oranının zaten 0'a yakın olduğunu unutmayın, bu nedenle bunu IM ile yapmanın doğrudan bir faydası yoktur, ancak ASF ile başlatılan diğer oyunlara müdahale edilmemesinin güçlü faydaları vardır ki bu çok önemlidir. Örneğin. VAC açısından.

---

## Güvenlik / Gizlilik / VAC / Banlanmalar / Kullanım Koşulları

---

### Bunu kullandığım için VAC ban yiyebilir miyim?

Hayır, bu mümkün değildir çünkü ASF (Idle Master veya SAM'den farklı olarak) steam istemcisine veya onun işlemlerine hiçbir şekilde müdahale etmez. ASF çalışırken güvenli sunucularda oynarken bile ASF kullanımı nedeniyle VAC yasağı almak fiziksel olarak imkansızdır; bunun nedeni **ASF, Steam İstemcisinin kurulmasını bile gerektirmez** düzgün çalışması için. ASF is one of the only few farming programs that can currently guarantee being VAC-free.

---

### ASF kullanmak, belirtildiği gibi VAC korumalı sunucularda oynamamı engelleyebilir mi? [**Burada**](https://help.steampowered.com/faqs/view/22C0-03D0-AE4B-04E8)?

ASF, Steam istemcisinin çalışmasını ve hatta kurulu olmasını gerektirmez. Bu kavrama göre olması gereken **Olumsuz** ASF, Steam istemcisine ve tüm süreçlerine müdahale edilmeyeceğini garanti ettiğinden, VAC ile ilgili herhangi bir soruna neden olabilir; ASF'nin sunduğu VAC'siz garantiden bahsederken ana nokta budur.

Kullanıcılara ve bildiğim kadarıyla şu anda durum böyle; ASF kullanırken hiç kimse yukarıdaki bağlantıda belirtildiği gibi herhangi bir sorun bildirmedi. Yukarıdaki sorunu Idle Master'da net bir şekilde oluştururken ASF'de de oluşturamadık.

Bununla birlikte, Valve'ın bir noktada ASF'yi yine de kara listeye ekleyebileceğini unutmayın, ancak bunu yapsalar bile bu tamamen saçmalıktır, yine de bilgisayarınızdan VAC korumalı oyunları oynayabilir ve aynı anda ASF'yi kullanabilirsiniz; Bu yüzden ASF'nin VAC açısından şüpheli olmaması gerektiğini çok iyi bildiklerinden eminim ve ASF'yi sebepsiz yere kara listeye alarak hayatımızı zorlaştırmayacaklar. Yine de en kötü durumda, yukarıda belirtildiği gibi oynayamayacaksınız çünkü Steam'in ASF ikili dosyasını kara listeye alıp almamasına bakılmaksızın ASF'nin VAC'siz garantisi hala burada (ve ASF'yi Steam olmadan başka herhangi bir makinede de başlatabilirsiniz) istemcinin kurulu olması). Şu anda bunların hiçbirini yapmaya gerek yok, umarız böyle devam eder.

---

### Güvenli mi?

ASF'nin bir yazılım olarak güvenli olup olmadığını sorarsanız, yani bilgisayarınıza herhangi bir zarar vermeyeceği, özel verilerinizi çalmayacağı, virüs veya buna benzer şeyler yüklemeyeceği anlamına gelir; güvenlidir. ASF, kötü amaçlı yazılım, veri çalma, kripto para madencileri ve kullanıcı tarafından kötü niyetli veya istenmeyen olarak kabul edilebilecek diğer (ve tüm) şüpheli davranışlardan arındırılmıştır. Buna ek olarak özel bir alanımız var. [**uzaktan iletişim**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication) Programı kendi başınıza yapacak şekilde yapılandırdığınızın ötesine geçen gizlilik politikamızı ve ASF davranışımızı kapsayan bölüm.

Kodumuz açık kaynaktır ve dağıtılmış ikili dosyalar her zaman şuradan derlenir: [**halka açık kaynaklar**](https://en.wikipedia.org/wiki/Open-source\_software) ile [**otomatik ve güvenilir sürekli entegrasyon sistemleri**](https://en.wikipedia.org/wiki/Build\_automation)ve geliştiricilerin kendileri bile değil. Her yapı, derleme komut dosyamız izlenerek tekrarlanabilir ve tamamen aynı sonuçla sonuçlanır, [**deterministik**](https://en.wikipedia.org/wiki/Deterministic\_system) IL (ikili) kod. Herhangi bir nedenle yapılarımıza güvenmiyorsanız, ASF'nin kullandığı tüm kitaplıklar (SteamKit2 gibi) dahil olmak üzere, ASF'yi her zaman kaynaktan derleyebilir ve kullanabilirsiniz; bunlar da açık kaynaklıdır.

Ancak sonuçta bu her zaman uygulamanızın geliştiricisi/geliştiricileri için bir güven meselesidir, bu nedenle ASF'nin güvenli olup olmadığına kendiniz karar vermelisiniz; kararınızı yukarıda belirtilen teknik argümanlarla destekleyebilirsiniz. Bir şeye sırf ben öyle dedim diye körü körüne inanmayın; kendinizi kontrol edin, çünkü emin olmanın tek yolu budur.

---

### Banlanabilirmiyim?

Bu soruyu cevaplamak için, [**Steam Hizmet Şartları**](https://store.steampowered.com/subscriber\_agreement)'a kesintisiz bakmalıyız. Aslında Steam birden fazla hesabın kullanılmasını yasaklamıyor. [**buna izin veriyor**](https://help.steampowered.com/faqs/view/7EFD-3CAE-64D3-1C31\#share) bu, aynı mobil kimlik doğrulayıcıyı birden fazla hesapta kullanabileceğinizi ima eder. Ancak hesapların başkalarıyla paylaşılmasına izin vermiyor, ancak bunu burada yapmıyoruz.

ASF'yi dikkate alan tek gerçek nokta şudur:
> Herhangi bir Subscription Marketplace sürecini değiştirmek veya otomatikleştirmek için Hileleri, otomasyon yazılımlarını (botları), modları, hackleri veya başka herhangi bir yetkisiz üçüncü taraf yazılımını kullanamazsınız.

Soru aslında Abonelik Pazaryeri sürecinin ne olduğudur. Okuyabildiğimiz gibi:

> Abonelik Pazarına bir örnek Steam Topluluk Pazarıdır

Abonelik pazaryerinden Steam topluluk pazarını veya buhar mağazasını anlıyorsak, abonelik pazarı sürecini değiştirmiyor veya otomatikleştirmiyoruz. Fakat...

> Valve, (a) Valve'in bu tür Abonelikleri genel olarak benzer konumdaki Abonelere sağlamayı bırakması veya (b) bu ​​Sözleşmenin herhangi bir şartını (herhangi bir Abonelik Koşulları veya Kullanım Kuralları).

Bu nedenle, her Steam yazılımında olduğu gibi ASF, Valve tarafından yetkilendirilmemektedir ve Valve'in aniden ASF kullanan hesapları yasaklamaya karar vermesi durumunda askıya alınmayacağınızı garanti edemem. This is extremely unlikely considering the fact that ASF is being used on more than a few million of Steam accounts, but still a possibility, regardless of actual probability.

Özellikle Çünkü:
> Valve tarafından yazılmayan tüm Abonelikler, İçerikler ve Hizmetler ile ilgili olarak Valve, Steam'de veya diğer kaynaklarda mevcut olan bu tür üçüncü taraf içeriklerini taramaz. Valve bu tür üçüncü taraf içeriklerine ilişkin hiçbir sorumluluk veya yükümlülük kabul etmez. Bazı üçüncü taraf uygulama yazılımları işletmeler tarafından ticari amaçlarla kullanılabilir; ancak bu tür yazılımları yalnızca özel kişisel kullanım için Steam aracılığıyla edinebilirsiniz.

Ancak Valve, belirtildiği gibi "Buhar avaralarının" mevcut olduğunu açıkça kabul eder [**Burada**](https://help.steampowered.com/faqs/view/22C0-03D0-AE4B-04E8), yani bana sorarsanız, eminim ki eğer onlarla araları iyi olmasa, VAC açısından sorun yaratabileceklerini belirtmek yerine zaten bir şeyler yaparlardı. Buradaki anahtar kelime **Buhar** avaralar, örneğin ASF ve değil **oyun** aylaklar.

InternalRequest() Yasak \<- HEAD https://steamcommunity.com/my/edit/settings

```
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
```

Bu yazılımı kullanırken risk size aittir. It's extremely unlikely that you can get banned for that, but if you do, you can blame only yourself for that.

---

### Did anybody get banned for it?

**Evet**Şu ana kadar Steam'in askıya alınmasıyla sonuçlanan en az birkaç olay yaşadık. Temel nedenin ASF olup olmadığı tamamen farklı bir hikaye ve muhtemelen hiçbir zaman bilemeyeceğiz.

İlk vaka, 1000'den fazla botu olan bir adamın ticaretinin (tüm botlarla birlikte) yasaklanmasıyla ilgiliydi; büyük ihtimalle aşırı kullanım nedeniyle. `ASF'yi yağmala` tüm botlarda aynı anda gerçekleştirilir veya çok kısa sürede tek taraflı şüpheli miktarda işlem yapılır.

> Merhaba XXX, Steam Destek ile iletişime geçtiğiniz için teşekkür ederiz. Bu hesabın bot hesaplarından oluşan bir ağı yönetmek için kullanıldığı anlaşılıyor. Bot yapmak Steam Abonelik Sözleşmesinin ihlalidir.

Lütfen biraz sağduyunuzu kullanın ve sırf ASF izin verdiği için böyle çılgınca şeyler yapabileceğinizi varsaymayın. Yapmak `ASF'yi yağmala` 1.000'den fazla bot kolayca bir [**DDoS**](https://en.wikipedia.org/wiki/DDoS) saldırı ve kişisel olarak birisinin böyle bir şey nedeniyle yasaklanmasına şaşırmadım. Steam hizmetiyle ilgili olarak minimum düzeyde adil kullanım sağlayın ve **büyük ihtimalle** iyi olacaksın.

İkinci vaka, Steam'in 2017 Kış İndirimi sırasında 170'den fazla botu olan bir adamın yasaklanmasıyla ilgiliydi.

> Hesabınız Steam abonesinin sözleşmesinin ihlali nedeniyle engellendi. Takaslara ve diğer faktörlere bakılırsa, bu hesap Steam'de tahsil edilebilir kartların yanı sıra yalnızca ticari değil ilgili faaliyetlerde de yasa dışı olarak toplamak için kullanıldı. Hesap kalıcı olarak engellendi ve Steam Destek bu konuda ek destek sağlayamıyor.

Bu vakanın analiz edilmesi bir kez daha çok zor çünkü Steam desteğinden gelen ve neredeyse hiç ayrıntı vermeyen belirsiz yanıtlar var. Kişisel düşüncelerime göre, bu kullanıcı muhtemelen Steam kartlarını bir tür para karşılığında değiştirdi (seviye yükseltme botu?) veya başka bir şekilde Steam'den para çekmeye çalıştı. Farkında değilseniz, bu da kanuna göre yasa dışıdır. [**Steam Hizmet Şartları**](https://store.steampowered.com/subscriber\_agreement). We're not in position to tell you what you can do with the trading cards obtained through ASF - but the user in question definitely didn't just craft badges with them.

Üçüncü vaka, 120'den fazla bota sahip kullanıcının, kuralları ihlal ettiği için yasaklanmasıyla ilgiliydi [**Steam'in çevrimiçi davranışı**](https://store.steampowered.com/online\_conduct?l=english).

> Merhaba XXX, Steam Destek ile iletişime geçtiğiniz için teşekkür ederiz. Bu ve diğer hesaplar, Steam'in çevrimiçi davranışını ihlal eden ağ altyapımızı sular altında bırakmak için kullanıldı. Hesap kalıcı olarak engellendi ve Steam Destek bu konuda ek destek sağlayamıyor.

Kullanıcı tarafından sağlanan ekstra ayrıntılar nedeniyle bu durumun analiz edilmesi biraz daha kolaydır. Görünüşe göre kullanıcı kullanıyordu **çok eski bir ASF sürümü** Bu, ASF'nin Steam sunucularına aşırı sayıda istek göndermesine neden olan bir hatayı içeriyordu. Hatanın kendisi ilk başta mevcut değildi ancak gelecek sürümde düzeltilen Steam'deki değişiklik nedeniyle etkinleştirildi. **ASF yalnızca şu durumlarda desteklenir: [en son kararlı sürüm](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest) GitHub'da yayınlandı**. Yazılım insanlar tarafından yazılır ve insanlar hata yapma eğilimindedir. Hatanın küresel bir kapsamı varsa, hızla düzeltiliyor ve hata düzeltmesi olarak tüm kullanıcılara yayınlanıyor. Valve, bariz sebeplerden ötürü benim hatam yüzünden bir milyondan fazla ASF kullanıcısını aniden yasaklamayacaktır. Ancak, güncel ASF'yi kullanmaktan kasıtlı olarak vazgeçerseniz, o zaman tanım gereği, çok küçük bir kullanıcı azınlığı içindesiniz demektir. **bu gibi olaylara maruz** dolayı **destek yok**, ASF'nin eski sürümünü denetleyen, düzelten kimse olmadığı ve onu başlatarak doğrudan yasaklanmayacağınızı garanti eden kimse olmadığı için. **Lütfen güncel yazılımı kullanın**, yalnızca ASF'yi değil, diğer tüm uygulamaları da destekler.

Kullanıcıya göre en son vaka Haziran 2021 civarında meydana geldi:

> Programınızı kullanarak 3 yıldır 28 hesapla, son 6 aydır ise 128 hesapla booster paketler yapıyorum. Destek paketleri oluşturmak ve bunları ana hesaba göndermek için aynı anda maksimum 15 hesapla çevrimiçiydim. Geçtiğimiz ay online hesap sayımı aynı anda 20'ye çıkardım ve bundan 1 hafta sonra tüm hesaplarım yasaklandı. Bu e-posta sizi suçlamak için değil, aksine sonuçlarının her zaman farkındaydım. Hangi tür davranışların kalıcı yasaklamayla sonuçlandığını bilmenizi istedim.

Çevrimiçi eşzamanlı hesaplardaki artışın yasağın doğrudan nedeni olup olmadığını söylemek zor, buna güvenmezdim, bunun yerine tek başına hesap sayısının ana suçlu olduğuna inanıyorum, çevrimiçi hesapların artan eşzamanlılığı muhtemelen sadece dikkat çekti Söz konusu kullanıcının bizim önerimizden çok daha fazla botu olduğu açıkça görülüyor.

---

Yukarıdaki olayların hepsinin ortak bir yanı var: ASF yalnızca bir araçtır ve **senin** onu nasıl kullanacağınıza karar verin. ASF'yi doğrudan kullandığınız için yasaklanmazsınız, ancak **Nasıl** onu kullanıyorsun. Bu, yalnızca tek bir hesabın çiftçiliğine yardımcı bir araç olabileceği gibi binlerce bottan oluşan devasa bir çiftçilik ağı da olabilir. Her durumda, hukuki tavsiye vermiyorum ve ilk etapta ASF kullanımınıza kendiniz karar vermelisiniz. I'm not hiding any information that could help you, e.g. the fact that ASF got some people banned (and in what context), as I have no reason to - it's your choice what you want to do with that information.

Bana sorarsanız \- biraz sağduyunuzu kullanın, tavsiye ettiğimizden daha fazla bota sahip olmaktan kaçının, aynı anda yüzlerce işlem göndermeyin, her zaman güncel ASF sürümünü kullanın ve siz de *meli* iyi ol. Bu nitelikteki her bir olay **bir sebep** Bu her zaman önerimizi dikkate almayan ve kaç tane bot çalıştırabileceklerini bizden daha iyi bildiklerine karar veren insanların başına gelmiştir. Bunun sadece bir tesadüf mü yoksa gerçek bir faktör mü olduğuna karar vermek size kalmış. Herhangi bir hukuki tavsiye sunmuyorum, yalnızca faydalı bulabileceğiniz düşüncelerimi veriyorum veya bunları tamamen göz ardı edip yalnızca yukarıda bağlantılı gerçeklere dayanarak hareket ediyorum.

---

### ASF hangi gizlilik bilgilerini ifşa ediyor?

Detaylı açıklamayı şurada bulabilirsiniz [**uzaktan iletişim**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication) bölüm. Gizliliğinize önem veriyorsanız bunu gözden geçirmelisiniz; ASF'de kullanılan hesapların neden Steam grubumuza katıldığını merak ediyorsanız. ASF hiçbir hassas bilgiyi toplamaz ve bunları üçüncü taraflarla paylaşmaz.

---

## Çeşitli

---

### 32 bit Windows gibi desteklenmeyen bir işletim sistemi kullanıyorum, yine de ASF'nin en son sürümünü kullanabilir miyim?

Evet ve bu sürüm hiçbir şekilde desteklenmiyor değil, yalnızca resmi olarak oluşturulmadı. Çıkış yapmak [**uyumluluk**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility) genel varyant bölümü. ASF'nin işletim sistemine güçlü bir bağımlılığı yoktur ve 32 bit Windows içeren, çalışan bir .NET çalışma zamanı alabileceğiniz her yerde çalışabilir. `win-x86` Bizden işletim sistemine özel paket.

---

### ASF harika\! Bağış yapabilir miyim?

Evet, projemizden memnun kaldığınızı duyduğumuza çok sevindik\! Her başlığın altında çeşitli bağış olanakları bulabilirsiniz. [**serbest bırakmak**](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest) ve ayrıca [**ana sayfada**](https://github.com/JustArchiNET/ArchiSteamFarm). Genel para bağışlarına ek olarak Steam öğelerini de kabul ettiğimizi belirtmek güzel, yani eğer isterseniz ASF ile yetiştirdiğiniz kaplamaları, anahtarları veya kartların küçük bir kısmını bağışlamaktan hiçbir şey sizi alıkoyamaz. Cömertliğiniz için şimdiden teşekkür ederiz\! Thank you in advance for your generosity!

---

### Hesabımı korumak için Steam ebeveyn PIN'ini kullanıyorum, bunu bir yere girmem gerekiyor mu?

Evet ayarlamanız gerekiyor `SteamEbeveyn Kodu` bot yapılandırma özelliği. Bunun temel nedeni, ASF'nin Steam hesabınızın birçok korunan bölümüne erişmesi ve ASF'nin onsuz çalışmasının imkansız olmasıdır.

---

### ASF'nin varsayılan olarak herhangi bir oyunu toplamasını istemiyorum, ancak ekstra ASF özelliklerini kullanmak istiyorum. Mümkün mü?

Yes, if you just want to start ASF with paused cards farming module, you can set `FarmingPausedByDefault` in `FarmingPreferences` bot config property in order to achieve that. Bu şunları yapmanıza olanak sağlayacaktır: `sürdürmek` çalışma zamanı sırasında.

Kart yetiştirme modülünü tamamen devre dışı bırakmak ve siz aksini açıkça söylemeden asla çalışmayacağından emin olmak istiyorsanız, o zaman ayarlamanızı öneririz. `FarmPriorityQueueYalnızca` botta `ÇiftçilikTercihler`Bu, yalnızca duraklatmak yerine, siz oyunları boşta öncelik sırasına kendiniz ekleyene kadar çiftçiliği tamamen devre dışı bırakacaktır.

Kart yetiştirme modülü duraklatıldığında/devre dışı bırakıldığında, aşağıdakiler gibi ekstra ASF özelliklerinden yararlanabilirsiniz: `Boştayken Oynanan Oyunlar`.

---

### ASF tepsiye küçültülebilir mi?

ASF bir konsol uygulamasıdır, simge durumuna küçültülecek bir pencere yoktur çünkü pencere sizin için işletim sisteminiz tarafından yaratılmıştır. Bununla birlikte, bunu yapabilen herhangi bir üçüncü taraf aracını kullanabilirsiniz; [**RBTray**](https://github.com/benbuck/rbtray) Windows için veya [**ekran**](https://linux.die.net/man/1/screen) Linux/macOS için. Bunlar sadece örnektir, benzer işlevlere sahip başka birçok uygulama vardır.

---

### ASF'nin kullanılması destek paketleri almaya uygunluğu korur mu?

**Hayır**. ASF'nin herhangi bir hizmetle bağlantısı yoktur ve bu tür iddiaların tümü yanlıştır. Steam hesabınız sizin mülkünüzdür ve hesabınızı dilediğiniz şekilde kullanabilirsiniz ancak Valve, [**resmi Hizmet Şartları**](https://store.steampowered.com/subscriber\_agreement) O:

---

### ASF ile iletişim kurmanın bir yolu var mı?

Evet, birkaç farklı yolla. Çıkış yapmak [**komutlar**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands) Daha fazla bilgi için bölüm.

---

### ASF çevirisine yardımcı olmak istiyorum, ne yapmam gerekiyor?

İlginiz için teşekkürler\! Tüm detayları haberimizde bulabilirsiniz [**yerelleştirme**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization) bölüm.

---

### ASF'ye eklenmiş yalnızca bir (ana) hesabım var, yine de Steam sohbeti aracılığıyla komutlar verebilir miyim?

**Yes**, it's explained in **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands#notes)** section. You can do so through Steam group chat, although using **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)** could be easier for you.

---

### ASF çalışıyor gibi görünüyor ancak herhangi bir kart düşüşü almıyorum\!

Kart çiftçiliği oranı oyundan oyuna farklılık gösterir, bunu şurada okuyabilirsiniz: [**verim**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance). Genellikle biraz zaman alır **oyun başına birkaç saat**ve programı başlattıktan sonraki birkaç dakika içinde kartların düşmesini beklememelisiniz. ASF'nin kartların durumunu aktif olarak kontrol ettiğini ve mevcut oyun tamamen çiftçilik yapıldıktan sonra oyunu değiştirdiğini görebiliyorsanız, her şey yolunda gidiyor demektir. Gibi bir seçeneği etkinleştirmiş olmanız mümkündür. `Envanter Bildirimlerini Kapat` ile ilgili `Bot Davranışı` Envanter bildirimlerini otomatik olarak reddeder. Çıkış yapmak [**yapılandırma**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration) detaylar için.

---

### Hesabım için ASF işlemini nasıl tamamen durdurabilirim?

Örneğin Windows'ta \[X\] öğesine tıklayarak ASF işlemini kapatmanız yeterlidir. Bunun yerine seçtiğiniz belirli bir botu durdurup diğerlerini çalışır durumda tutmak istiyorsanız şuraya bir göz atın: `Etkinleştirilmiş` [**bot yapılandırma özelliği**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration\#bot-config), veya `durmak` [**emretmek**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands). If you instead want to stop automatic farming process, yet keep ASF running for your account, then that's what `FarmingPausedByDefault` option of `FarmingPreferences` in **[bot config property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)** and `pause` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** is for.

---

### ASF ile kaç tane bot çalıştırabilirim?

Bir program olarak ASF'nin bot örnekleri için herhangi bir üst sınırı yoktur, dolayısıyla makinenizdeki hafıza kadar çalıştırabilirsiniz, ancak yine de Steam ağı ve diğer Steam hizmetleri tarafından sınırlısınız. Şu anda tek bir IP ve tek bir ASF örneği ile 100-200'e kadar bot çalıştırabilirsiniz. IP sınırlamalarını aşarak daha fazla IP ve daha fazla ASF örneğiyle daha fazla bot çalıştırmak mümkündür. Bu kadar büyük miktarda bot kullanıyorsanız, bunların sayısını kendiniz kontrol etmeniz gerektiğini, örneğin hepsinin aynı anda oturum açtığından ve çalıştığından emin olmanız gerektiğini unutmayın. ASF, bu kadar çok sayıda bot için ayarlanmamıştır ve genel kural şu ​​şekildedir: **Ne kadar çok bota sahipseniz o kadar çok sorunla karşılaşırsınız**. Ayrıca yukarıdaki limitin genel olarak birçok dahili faktöre bağlı olduğuna dikkat edin; bu, katı bir limitten ziyade yaklaşık bir limittir; büyük olasılıkla yukarıda belirtilenden daha fazla/daha az bot çalıştırabileceksiniz.

ASF takımı en fazla **10 adet Steam hesabına** **sahip olmanızı** ve bu nedenle de aynı anda **en fazla 10 bot** çalıştırmanızı tavsiye eder. Önerilen sayının üzerindekiler desteklenmemektedir ve risk büyüklüğüne aittir, bizim burada yaptığımız tavsiyeye uymamaktadır. Bu öneri, kendi önerilerimizin yanı sıra dahili Valve yönergelerine dayanmaktadır. Bu kurala uyup uymayacağınız sizin seçiminizdir, bir araç olarak ASF, Steam hesaplarınızın askıya alınmasıyla sonuçlansa bile, kendi isteğinize aykırı hareket etmeyecektir. Bu nedenle, önerdiğimiz değerlerin üzerine çıkarsanız ASF size bir uyarı görüntüleyecektir, ancak yine de istediğiniz herhangi bir şeyi, risk size ait olmak üzere ve desteğimiz olmadan çalıştırmanıza izin verecektir.

---

### O zaman daha fazla ASF örneği çalıştırabilir miyim?

Her örneğin kendi dizinine ve kendi yapılandırmalarına sahip olduğunu ve bir örnekte kullanılan hesabın diğerinde kullanılmadığını varsayarak, bir makinede istediğiniz kadar ASF örneğini çalıştırabilirsiniz. Ancak kendinize bunu neden yapmak istediğinizi sorun. ASF is optimized to handle more than a hundred of accounts at the same time, and launching that hundred of bots in their own ASF instances degrades performance, takes more OS resources (such as CPU and memory), and causes a potential synchronization issues between standalone ASF instances, as ASF is forced to share its limiters with other instances.

Bu nedenle benim **güçlü öneri** yani, her zaman bir IP/arabirim başına maksimum bir ASF örneğini çalıştırın. Daha fazla IP'niz/arayüzünüz varsa, her durumda kendi IP'sini/arayüzünü veya benzersizini kullanan daha fazla ASF örneği çalıştırabilirsiniz. `WebProxy` ayar. Bunu yapmazsanız, daha fazla ASF örneği başlatmak tamamen anlamsızdır çünkü tek bir IP/arabirim başına 1'den fazla örnek başlatmaktan hiçbir şey elde edemezsiniz. Steam, başka bir ASF örneğinde başlattığınız için daha fazla bot çalıştırmanıza sihirli bir şekilde izin vermeyecektir ve ASF, başlangıçta sizi sınırlamaz.

Elbette, aynı ağ arayüzünde birden fazla ASF örneği için hala geçerli kullanım durumları vardır; örneğin, botlar ve hatta ASF işlemlerinin kendisi arasındaki yalıtımı garanti altına almak amacıyla her arkadaşınızın kendi benzersiz ASF örneğine sahip olduğu arkadaşlarınız için ASF hizmeti barındırmak gibi. ancak bu şekilde herhangi bir Steam sınırlamasını aşamazsınız, bu tamamen farklı bir amaçtır.

---

### Bir anahtarı kullanırken durumun anlamı nedir?

Durum, verilen kullanma girişiminin nasıl sonuçlandığını gösterir. Mümkün olan pek çok farklı durum vardır; en yaygın olanları şunlardır:

| Status                  | Açıklama                                                                                                                                                                                                                          |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Detay Yok               | Başarıyı gösteren "Tamam" durumu \- anahtar başarıyla kullanıldı.                                                                                                                                                                |
| Zaman aşımı             | Steam ağı belirtilen sürede yanıt vermedi, anahtarın kullanılıp kullanılmadığını bilmiyoruz (büyük olasılıkla öyleydi, ancak tekrar deneyebilirsiniz).                                                                            |
| BadActivationCode       | Sağlanan anahtar geçersiz (Steam ağı tarafından geçerli bir anahtar olarak tanınmıyor).                                                                                                                                           |
| DuplicateActivationCode | Sağlanan anahtar zaten başka bir hesap tarafından kullanılmış veya geliştirici/yayıncı tarafından iptal edilmiş.                                                                                                                  |
| Zaten Satın Alınmış     | Hesabınız zaten sahibi `paket kimliği` bu anahtarla bağlantılıdır. Bunun anahtarın olup olmadığını göstermediğini unutmayın. `Etkinleştirme Kodunu Çoğalt` ya da değil \- yalnızca geçerli olduğu ve bu denemede kullanılmadığı. |
| Kısıtlanmış Ülke        | Bu bölge kilitli anahtardır ve hesabınız, onu kullanmasına izin verilen geçerli bölgede değildir.                                                                                                                                 |
| DoesNotOwnRequiredApp   | Başka bir uygulamayı kaçırdığınız için bu anahtarı kullanamazsınız \- özellikle DLC paketini kullanmaya çalışırken temel oyun.                                                                                                   |
| RateLimited             | Çok fazla para kullanma girişiminde bulundunuz ve hesabınız geçici olarak engellendi. Bir saat sonra tekrar deneyin.                                                                                                              |

---

### Herhangi bir kart yetiştirme/boşta kalma hizmetine bağlı mısınız?

**Evet**. ASF is not affiliated with any service and all such claims are false. Your Steam account is your property and you can use your account in whatever way you wish, but Valve clearly stated in **[official ToS](https://store.steampowered.com/subscriber_agreement)** that:

> Kullanıcı adınızın ve şifrenizin gizliliğinden ve bilgisayar sisteminizin güvenliğinden siz sorumlusunuz. Valve, şifrenizin ve Hesabınızın kullanımından veya oturum açma adınızın ve şifrenizin sizin tarafınızdan veya kasten veya ihmal sonucu bilgilerinizi ifşa ettiğiniz herhangi bir kişi tarafından kullanılmasından kaynaklanan Steam'deki tüm iletişim ve faaliyetlerden sorumlu değildir. Bu gizlilik hükmünü ihlal eden kullanıcı adı ve/veya şifre.

ASF, diğer geliştiricilerin ASF'yi kendi projeleri ve hizmetleriyle yasal olarak daha fazla entegre etmelerine olanak tanıyan liberal Apache 2.0 Lisansı ile lisanslanmıştır. Ancak ASF'yi kullanan bu tür üçüncü taraf projelerin güvenli, incelenmiş, uygun veya yasal olduğu garanti edilmez. [**Steam Hizmet Şartları**](https://store.steampowered.com/subscriber\_agreement). Fikrimizi öğrenmek istiyorsanız **HİÇBİR hesap ayrıntınızı üçüncü taraf hizmetlerle paylaşmamanızı önemle tavsiye ederiz.**. Eğer böyle bir hizmet ortaya çıkarsa **tipik dolandırıcılık**büyük olasılıkla Steam hesabınız olmadan sorunla baş başa kalacaksınız ve ASF ekibi bunların hiçbirinin incelenmesine izin vermediği için ASF güvenli olduğunu iddia eden üçüncü taraf hizmetler için herhangi bir sorumluluk kabul etmeyecektir. Başka bir deyişle, **yukarıda yaptığımız öneriye aykırı olarak, riski size ait olmak üzere bunları kullanıyorsunuz**.

Bunun yanı sıra resmi [**Steam Hizmet Şartları**](https://store.steampowered.com/subscriber\_agreement) açıkça şunu belirtmektedir:

> Valve tarafından özel olarak izin verilmediği sürece şifrenizi veya Hesabınızı ifşa edemez, paylaşamaz veya başkalarının kullanmasına izin veremezsiniz.

Bu sizin hesabınız ve sizin seçiminiz. Just don't say that nobody warned you. Kimsenin seni uyarmadığını söyleme. Bir program olarak ASF, hesap ayrıntılarınızı kimseyle paylaşmadığınız ve programı kendi kişisel kullanımınız için kullandığınız için yukarıda belirtilen tüm kuralları karşılar, ancak diğer herhangi bir "kart yetiştirme hizmeti" sizden hesap kimlik bilgilerinizi gerektirir. , dolayısıyla yukarıdaki kuralı da ihlal ediyor (aslında bunlardan birkaçı). ile beğen [**Steam Hizmet Şartları**](https://store.steampowered.com/subscriber\_agreement) değerlendirme, herhangi bir hukuki tavsiye sunmuyoruz ve bize göre bu hizmetleri kullanmak isteyip istemediğinize kendiniz karar vermelisiniz. **doğrudan ihlal ediyor [Steam Hizmet Şartları](https://store.steampowered.com/subscriber\_agreement)** ve Valve bunu öğrenirse uzaklaştırmayla sonuçlanabilir. Yukarıda işaret edildiği gibi, **bu tür hizmetlerin hiçbirini KULLANMAMANIZI şiddetle tavsiye ederiz**.

---

## Sorunlar

---

### Oyunlarımdan biri 10 saatten fazla süredir farmlanıyor ama hâlâ kart alamadım\!

Bunun nedeni, aynı oyun için iki lisansınız olduğunda meydana gelen ve birinde kart düşüşü sınırlı olan Steam'in bilinen sorunuyla ilgili olabilir. Bu genellikle Steam'deki toplu bir hediye sırasında oyunu ücretsiz olarak etkinleştirdiğinizde ve ardından aynı oyun için bir anahtarı etkinleştirdiğinizde (ancak sınırlama olmaksızın), ör. ücretli bir paketten. Böyle bir durum meydana gelirse, Steam rozet sayfasında oyunda hala düşecek kartların olduğunu ancak oyunu ne kadar oynarsanız oynayın, hesabınızdaki ücretsiz lisans nedeniyle kartların asla düşmeyeceğini bildirir. Bu bir ASF sorunu değil, Steam sorunu olduğundan, bunu ASF açısından bir şekilde atlatamayız ve bunu sizin kendiniz çözmeniz gerekir.

Sorunu çözmenin iki yolu var. Öncelikle bu oyunu ASF'de kara listeye alabilirsiniz. `fbadd` [**emretmek**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands) veya ile `Kara liste` [**konfigürasyon özelliği**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration). Bu, ASF'nin bu oyundan kart toplamaya çalışmasını engelleyecektir ancak etkilenen oyundan kart düşürmenizi engelleyen temel sorunu çözmeyecektir. İkinci olarak, hesabınızdan ücretsiz lisansı kaldırmak için Steam destek self-servis aracını kullanabilirsiniz; geriye yalnızca kart düşüşlerini içeren tam lisans kalır. Bunun için öncelikle adresinizi ziyaret edin. [**lisanslar ve ürün anahtarı etkinleştirmeleri**](https://store.steampowered.com/account/licenses) sayfasına gidin ve etkilenen oyunun hem ücretsiz hem de ücretli lisansını bulun. Genellikle oldukça kolaydır \- her ikisi de benzer ada sahiptir, ancak ücretsiz olanın lisans adında "sınırlı ücretsiz promosyon paketi" veya başka bir "promosyon" ve ayrıca "satın alma yöntemi" alanında "ücretsiz" bulunur. Bazen, örneğin ücretsiz paketin bir pakette olması ve farklı bir ada sahip olması durumunda bu daha zor olabilir. Sometimes it might be more tricky, for example if free package was in some bundle and has a different name. Bunun gibi iki lisans bulduysanız, o zaman bu gerçekten burada açıklanan sorundur ve oyunu kaybetmeden ücretsiz lisansı güvenli bir şekilde kaldırabilirsiniz.

Ücretsiz lisansı hesabınızdan kaldırmak için şu adresi ziyaret edin: [**Steam destek sayfası**](https://help.steampowered.com/wizard/HelpWithGame) ve etkilenen oyunun adını arama alanına yazın, oyun "ürünler" bölümünde mevcut olmalıdır, üzerine tıklayın. Alternatif olarak, yalnızca kullanabilirsiniz `https://help.steampowered.com/wizard/HelpWithGame?appid=<appID>` bağla ve değiştir `<uygulama kimliği>` Sorunlara neden olan oyunun appID'si ile. Daha sonra, "Bu oyunu hesabımdan kalıcı olarak kaldırmak istiyorum" seçeneğine tıklayın ve ardından yukarıda bulduğunuz hatalı ücretsiz lisansı seçin; genellikle adında "sınırlı ücretsiz promosyon paketi" (veya benzeri) bulunan lisansı seçin. Ücretsiz lisansın kaldırılmasının ardından ASF, etkilenen oyundan kartları sorunsuz bir şekilde çıkarabilmelidir; Steam'in bu sefer doğru lisansı aldığından emin olmak için kaldırma işleminden sonra çiftçilik işlemini yeniden başlatmalısınız.

---

### ASF oyunu algılamıyor `X` çiftçilik için uygun, ancak bunun Steam takas kartlarını da içerdiğini biliyorum\!

Burada iki ana sebep var. İlk ve en bariz sebep, bahsettiğiniz gerçektir. **Steam mağazası** Verilen oyunun, kart düşmelerinin etkin olduğu oyun olarak duyurulduğu yer. Bu **yanlış** varsayım, çünkü basitçe oyunun **sahip olmak** kart düşmeleri dahildir, ancak bu işlevin o oyun için olması şart değildir **etkinleştirilmiş** derhal. Bununla ilgili daha fazlasını şurada okuyabilirsiniz [**resmi duyuru**](https://steamcommunity.com/games/593110/announcements/detail/1954971077935370845).

Kısacası Steam mağazasındaki kart düşürme simgesinin hiçbir anlamı yok, kontrol edin [**rozet sayfaları**](https://steamcommunity.com/my/badges) bir oyunda kart düşürmenin etkin olup olmadığının onaylanması için \- ASF'nin yaptığı da budur. Eğer oyununuz listede kartların düşmesi muhtemel bir oyun olarak görünmüyorsa bu oyun **Olumsuz** nedeni ne olursa olsun çiftçilik yapmak mümkündür.

İkinci sorun ise daha az belirgindir ve rozet sayfanızda oyununuzun gerçekten de kart düşüşleriyle mevcut olduğunu ancak ASF tarafından hemen çiftçilik yapılmadığını görebildiğiniz durumdur. ASF'nin rozet sayfalarını kontrol edememesi (aşağıda açıklanmıştır) gibi başka bir hatayla karşılaşmadığınız sürece, bu sadece bir önbellek etkisidir ve ASF tarafında Steam hala güncel olmayan rozet sayfasını rapor etmektedir. Bu sorun, önbellek geçersiz kılındığında er ya da geç kendi kendine çözülecektir. Bunu bizim açımızdan düzeltmenin de bir yolu yok.

Elbette tüm bunlar, ASF'yi varsayılan, dokunulmamış ayarlarla çalıştırdığınızı varsayar, çünkü bu oyunu çiftçilik kara listesine de ekleyebilirsiniz, seçilenleri kullanın `ÇiftçilikTercihler` örneğin `FarmPriorityQueueYalnızca` veya `İade EdilebilirOyunları Atla`, ve benzeri.

---

### ASF aracılığıyla çiftçilik yapılan oyunların oyun süresi neden artmıyor?

Öyle ama **gerçek zamanlı değil**. Steam, oyun sürenizi sabit aralıklarla kaydeder ve bunun için güncelleme planlar, ancak bırakın oturumu kapattığınız anda, hemen güncelleneceğinin garantisi yoktur. Oynatma süresinin gerçek zamanlı olarak güncellenmemesi, kaydedilmediği anlamına gelmez; genellikle yaklaşık her 30 dakikada bir güncellenir.

---

### Bir uyarı ile günlükteki bir hata arasındaki fark nedir?

ASF, günlüğüne çeşitli kayıt düzeylerine ilişkin bir sürü bilgi yazar. Amacımız açıklamak **açık olarak** ASF'nin ne yaptığı, Steam'in çözmesi gereken sorunlar veya üstesinden gelmesi gereken diğer sorunlar da dahil. Çoğu zaman her şey alakalı değildir, bu nedenle ASF'de sorunlar açısından kullanılan iki ana seviyemiz vardır: bir uyarı seviyesi ve hata seviyesi.

Genel ASF kuralı, uyarıların **Olumsuz** hatalar, bu nedenle **Olumsuz** rapor edilmelidir. Bir uyarı, potansiyel olarak istenmeyen bir şeyin meydana geldiğinin göstergesidir. İster Steam'in tepki vermemesi, ister API atma hataları ya da ağ bağlantınızın kesilmesi olsun, bu bir uyarıdır ve bunun olmasını beklediğimiz anlamına gelir, bu nedenle ASF gelişimini bununla meşgul etmeyin. Elbette bunlar hakkında soru sormakta veya desteğimizi kullanarak yardım almakta özgürsünüz, ancak bunların rapor edilmeye değer ASF hataları olduğunu düşünmemelisiniz (aksini onaylamadığımız sürece).

Öte yandan hatalar, olmaması gereken bir duruma işaret eder; bu nedenle, bunlara neden olanın siz olmadığınızdan emin olduğunuz sürece rapor edilmeye değerdirler. Olmasını beklediğimiz yaygın bir durumsa bu durum bir uyarıya dönüştürülür. Aksi takdirde, muhtemelen sizin kendi teknik sorununuzun bir sonucu olmadığı varsayılarak, sessizce göz ardı edilmemesi gereken düzeltilmesi gereken bir hatadır. Örneğin, geçersiz içerik koymak `ASF.json` ASF onu ayrıştıramayacağından dosya bir hata verecektir, ancak onu oraya koyan sizsiniz, bu nedenle bu hatayı bize bildirmemelisiniz (ASF'nin yanlış olduğunu ve yapınızın aslında kesinlikle doğru olduğunu onaylamadığınız sürece) doğru).

Tek bir TL;DR cümlesiyle \- hataları bildirin, uyarıları bildirmeyin. Yine de destek bölümlerimizden uyarıları sorabilir ve yardım alabilirsiniz.

---

### ASF başlamıyor, program penceresi hemen kapanıyor\!

Normal şartlarda herhangi bir ASF çökmesi veya çıkışı, `log.txt` programın dizininde görüntüleyebilirsiniz; bu, bunun nedenini bulmak için kullanılabilir. Buna ek olarak, son birkaç günlük dosyası da şu adreste arşivlenir: `kütükler` dizin, ana dizin olduğundan `log.txt` her ASF çalıştırmasında dosyanın üzerine yazılır.

Ancak, .NET çalışma zamanı bile makinenizde önyükleme yapamıyorsa, o zaman `log.txt` oluşturulmayacaktır. Eğer bu başınıza gelirse, büyük ihtimalle yukarıda belirtildiği gibi .NET önkoşullarını yüklemeyi unutmuşsunuzdur. [**kurulum**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up\#os-specific-setup) rehber. Diğer yaygın sorunlar arasında işletim sisteminiz için yanlış ASF varyantını başlatmaya çalışmak veya başka bir şekilde yerel .NET çalışma zamanı bağımlılıklarının eksik olması yer alır. Konsol penceresi mesajı okuyamayacağınız kadar erken kapanırsa bağımsız konsolu açın ve ASF ikili programını buradan başlatın. Örneğin Windows'ta ASF dizinini açın, basılı tutun `Vardiya`, klasörün içine sağ tıklayın ve "*komut penceresini burada aç*" (veya *güç kalkanı*), ardından konsola yazın `.\ArchiSteamFarm.exe` ve enter ile onaylayın. Bu şekilde ASF'nin neden düzgün şekilde başlamadığına dair kesin mesaj alırsınız.

---

### ASF, ben oynarken Steam İstemcisi oturumumu başlatıyor\! / *Bu hesap başka bir bilgisayarda oturum açmış*

Bu, siz oyun oynarken Steam arayüzünde hesabın başka bir yerde kullanıldığını belirten bir mesaj olarak görünür. Bu sorunun iki farklı nedeni olabilir.

Bunun bir nedeni, özellikle bir oyun kilidini düzgün şekilde tutmayan, ancak bu kilidin istemci tarafından ele geçirilmesini bekleyen bozuk paketlerden (oyunlardan) kaynaklanmaktadır. Böyle bir pakete örnek Skyrim SE olabilir. Steam istemciniz oyunu düzgün bir şekilde başlatıyor ancak oyun kendisini kullanılıyor olarak kaydetmiyor. Bu nedenle ASF, işlemi sürdürmenin ücretsiz olduğunu görüyor ve bunu yapıyor ve Steam aniden hesabın başka bir yerde kullanıldığını tespit ettiğinden bu sizi Steam ağından atıyor.

ASF beklerken bilgisayarınızda (özellikle başka bir makinede) oyun oynuyorsanız ve ağ bağlantınızı kaybederseniz ikinci neden ortaya çıkabilir. Bu durumda, Steam ağı sizi çevrimdışı olarak işaretler ve oynama kilidini serbest bırakır (yukarıdaki gibi), bu da ASF'yi (örneğin başka bir makinede) çiftçiliğe devam etmesi için tetikler. Bilgisayarınız tekrar çevrimiçi olduğunda, Steam artık oynatma kilidini alamıyor (bu artık ASF tarafından tutuluyor, yine yukarıdakine benzer) ve aynı mesajı gösteriyor.

ASF tarafındaki her iki nedenin de çözümü aslında çok zordur, çünkü ASF, Steam ağı hesabın tekrar kullanılmasının ücretsiz olduğunu bildirdikten sonra çiftçiliğe devam eder. Oyunu kapattığınızda normalde olan budur, ancak bozuk paketlerde oyununuz hala çalışıyor olsa bile bu hemen gerçekleşebilir. ASF'nin bağlantınızın kesilip kesilmediğini, oyun oynamayı bırakıp bırakmadığınızı veya hala oyun kilidini uygun şekilde tutmayan bir oyun oynadığınızı bilmenin hiçbir yolu yoktur.

Bu sorunun tek doğru çözümü botunuzu manuel olarak duraklatmak `Duraklat` oynamaya başlamadan önce ve oyuna devam etmeden önce `sürdürmek` işin bittiğinde. Alternatif olarak sorunu görmezden gelebilir ve çevrimdışı Steam istemcisiyle oynuyormuş gibi davranabilirsiniz.

---

### `Steam'le bağlantı kesildi!` \- Steam sunucuları ile bağlantı kuramıyorum.

ASF yalnızca **denemek** Steam sunucularıyla bağlantı kurmak için kullanılır ve internet bağlantısının olmaması, Steam'in kapalı olması, güvenlik duvarınızın bağlantınızı engellemesi, üçüncü taraf araçlar, yanlış yapılandırılmış rotalar veya geçici arızalar gibi birçok nedenden dolayı başarısız olabilir. Etkinleştirebilirsiniz `Hata ayıklama` kesin hata nedenlerini belirten daha ayrıntılı günlüğü kontrol etmek için mod; ancak bu genellikle kendi eylemlerinizden kaynaklanır; örneğin çok sayıda Steam IP'sini kara listeye alan ve erişmenizi çok zorlaştıran "CS:GO MM Sunucu Seçici"yi kullanmak gibi. Buhar ağı.

ASF, bağlantı kurmak için elinden gelenin en iyisini yapacaktır; bu, yalnızca güncellenen sunucu listesi hakkında soru sormakla kalmayıp aynı zamanda sonuncusu başarısız olduğunda başka bir IP denemeyi de içerir; dolayısıyla, belirli bir sunucu veya rotada gerçekten geçici bir sorun varsa, ASF er ya da geç bağlanacaktır. Ancak, eğer güvenlik duvarının arkasındaysanız veya başka bir şekilde Steam sunucularına ulaşamıyorsanız, o zaman tabii ki bunu kendiniz düzeltmeniz gerekir; `Hata ayıklama` modu.

Ayrıca makinenizin, ASF'deki varsayılan protokolü kullanarak Steam sunucularıyla bağlantı kuramaması da mümkündür. ASF'nin kullanmasına izin verilen protokolleri değiştirerek değiştirebilirsiniz. `SteamProtokolleri` genel yapılandırma özelliği. Örneğin Steam'e ulaşmada sorun yaşıyorsanız `UDP` protokol (örn. güvenlik duvarları nedeniyle), belki de daha fazla şansınız olur `TCP` veya `WebSocket`.

Örneğin ASF'nin taşınması nedeniyle yanlış sunucuların önbelleğe alınması gibi pek olası olmayan bir durumda `yapılandırma` klasörü bir makineden tamamen farklı bir ülkede bulunan başka bir makineye aktarma, silme `ASF.db` Bir sonraki başlatmada Steam sunucularını yenilemek yardımcı olabilir. Çoğu zaman buna ihtiyaç duyulmaz ve yapılması da gerekmez, çünkü bu liste ilk başlatmada ve bağlantı kurulduğunda otomatik olarak yenilenir \- bundan sadece Steam listesiyle ilgili her şeyi temizlemenin bir yolu olarak bahsediyoruz. ASF tarafından önbelleğe alınan sunucular.

---

### `Steam'e giriş yapılamıyor: TryAnotherCM/Geçersiz`, `Hizmet Kullanılamıyor/Geçersiz`

Yukarıdaki gibi, ancak bu sefer bağlandığınız sunucu açıkça kullanılamıyor. Genellikle Steam bakım penceresi sırasında olur, bu konuda yapabileceğiniz hiçbir şey yoktur, ASF, isteği kabul edene kadar otomatik olarak farklı bir sunucuyla yeniden deneyecektir. Maksimum bir saatten fazla sürmemelidir.

---

### `Rozet bilgisi alınamadı, daha sonra tekrar deneyeceğim!`

Genellikle bu, hesabınıza erişmek için Steam ebeveyn PIN'ini kullandığınız ancak bunu ASF yapılandırmasına koymayı unuttuğunuz anlamına gelir. Geçerli bir PIN girmelisiniz `SteamEbeveyn Kodu` bot config özelliğini kullanın, aksi takdirde ASF web içeriğinin çoğuna erişemeyecektir, dolayısıyla düzgün çalışamayacaktır. Başını aşmak [**yapılandırma**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration) hakkında daha fazla bilgi edinmek için `SteamEbeveyn Kodu`.

Diğer nedenler arasında geçici Steam sorunu, ağ sorunu veya benzerleri yer alır. Sorun birkaç saat sonra kendi kendine çözülmezse ve ASF'yi uygun şekilde yapılandırdığınızdan eminseniz bu durumu bize bildirmekten çekinmeyin.

---

### ASF başarısız oluyor `5 denemeden sonra istek başarısız oldu` hatalar\!

Genellikle bu, hesabınıza erişmek için Steam ebeveyn PIN'ini kullandığınız ancak bunu ASF yapılandırmasına koymayı unuttuğunuz anlamına gelir. Geçerli bir PIN girmelisiniz `SteamEbeveyn Kodu` bot config özelliğini kullanın, aksi takdirde ASF web içeriğinin çoğuna erişemeyecektir, dolayısıyla düzgün çalışamayacaktır. Başını aşmak [**yapılandırma**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration) hakkında daha fazla bilgi edinmek için `SteamEbeveyn Kodu`.

Sebep ebeveyn PIN'i değilse, o zaman bu en yaygın hatadır ve buna alışmalısınız; bu sadece ASF'nin Steam Ağına bir istek gönderdiği ve art arda 5 kez geçerli bir yanıt alamadığı anlamına gelir. . Genellikle bu, Steam'in kapalı olduğu veya bazı zorluklar ya da bakım sorunları yaşadığı anlamına gelir \- ASF bu tür sorunların farkındadır ve bunlar birkaç saatten daha uzun süre sürekli olarak meydana gelmediği ve diğer kullanıcılarda bu tür sorunlar olmadığı sürece bunlar hakkında endişelenmemelisiniz.

Steam'in kapalı olup olmadığı nasıl kontrol edilir? [**Buhar Durumu**](https://steamstat.us) Steam'in olup olmadığını kontrol etmek için mükemmel bir kaynaktır. **olmalı** Özellikle Topluluk veya Web API ile ilgili hatalar fark ederseniz Steam'de zorluk yaşanıyor demektir. ASF'yi kendi haline bırakıp kısa bir süre kesinti yaşadıktan sonra işini yapmasına izin vermek ya da bırakıp kendiniz beklemek isteyebilirsiniz.

Ancak durum her zaman böyle değildir, çünkü bazı durumlarda Steam sorunları Steam Durumu tarafından tespit edilemeyebilir; örneğin böyle bir durum Valve'ın Steam Topluluğu için HTTPS desteğini kesmesi 7 Haziran 2016'da gerçekleşti \- erişim [**SteamTopluluğu**](https://steamcommunity.com) HTTPS aracılığıyla bir hata veriyordu. Bu nedenle Steam Durumuna da körü körüne güvenmeyin; her şeyin olması gerektiği gibi çalışıp çalışmadığını kendiniz kontrol etmek en iyisidir.

Buna ek olarak Steam, aynı anda aşırı sayıda istekte bulunmanız durumunda IP'nizi geçici olarak yasaklayacak çeşitli hız sınırlayıcı önlemler içerir. ASF bunun farkındadır ve yapılandırmada kullanmanız gereken birkaç farklı sınırlayıcı sunar. Varsayılan ayarlar temel alınarak değiştirildi **aklı başında** Çok fazla sayıda bot kullanıyorsanız, Steam'in bile size gitmenizi söylediği kadar büyük miktarda kullanıyorsanız, o zaman ya size bunu söyleyene kadar onları değiştirirsiniz ya da size söyleneni yaparsınız. Sanırım ikinci yol sizin için bir seçenek değil, bu yüzden gidip bu konuyu okuyun ve konuya özellikle dikkat edin. `WebSınırlayıcıGecikme` bu, tüm web istekleri için geçerli olan genel bir sınırlayıcıdır.

Herkes için geçerli olan bir "altın kural" yoktur, çünkü bloklar üçüncü taraf faktörlerden büyük ölçüde etkilenir, bu nedenle kendinizi denemeniz ve sizin için işe yarayan bir değer bulmanız gerekir. Ayrıca söylediklerimi görmezden gelebilir ve şöyle bir şey kullanabilirsiniz: `10000` bunun doğru çalışması garantilidir, ancak o zaman ASF'nizin her şeye 10 saniyede nasıl tepki verdiğinden ve rozet ayrıştırmanın 5 dakika sürdüğünden şikayet etmeyin. Buna ek olarak, vurduğunuz çok sayıda bot olduğundan hiçbir sınırlayıcının hiçbir şey yapmaması tamamen mümkündür. [**kesin sınır**](\#how-many-bots-can-i-run-with-asf) yukarıda bahsedilmişti. Evet, Steam ağına (istemci) sorunsuz bir şekilde giriş yapabilmeniz tamamen mümkündür, ancak aynı anda 100 oturumunuz kuruluysa Steam web (web sitesi) sizi dinlemeyi reddedecektir. ASF, hem Steam ağının hem de Steam webinin işbirliği içinde olmasını gerektirir; sizi kurtaramayacağınız sorunlar haline getirmek için yalnızca bir tanesini devre dışı bırakır.

Hiçbir şey yardımcı olmazsa ve neyin bozulduğuna dair hiçbir fikriniz yoksa, her zaman etkinleştirebilirsiniz. `Hata ayıklama` moduna geçin ve ASF günlüğünde isteklerin tam olarak neden başarısız olduğunu görün. Örneğin:

```text
InternalRequest() HEAD https://steamcommunity.com/my/edit/settings
```

Şunu gör `Yasaklı` kod? Bu, ince ayar yapmadığınız için aşırı miktarda istek nedeniyle geçici olarak yasaklandığınız anlamına gelir `WebSınırlayıcıGecikme` henüz düzgün bir şekilde (diğer tüm istekler için de aynı hata kodunu aldığınızı varsayarsak). Burada listelenen başka nedenler de olabilir, örneğin `İç Sunucu Hatası`, `Hizmet kullanılamıyor` ve Steam bakımını/sorunlarını gösteren zaman aşımları. Her zaman ASF tarafından belirtilen bağlantıyı kendiniz ziyaret etmeyi deneyebilir ve çalışıp çalışmadığını kontrol edebilirsiniz; eğer çalışmıyorsa, o zaman ASF'nin neden buna da erişemediğini biliyorsunuzdur. Eğer öyleyse ve aynı hata bir veya iki gün sonra kaybolmuyorsa, araştırmaya ve raporlamaya değer olabilir.

Bunu yapmadan önce şunları yapmalısınız: **İlk etapta hatanın rapor edilmeye değer olduğundan emin olun**. Bu SSS'de ticaretle ilgili bir sorun gibi bir şeyden bahsediliyorsa, o zaman bu geçerli değildir. Özellikle ağınız kararsız olduğunda veya Steam kapalıyken, bir veya iki kez meydana gelen geçici bir sorun varsa, sorun yoktur. Ancak, sorununuzu 2 gün boyunca art arda birkaç kez yeniden oluşturabildiyseniz, bu süreçte ASF'yi ve makinenizi yeniden başlattıysanız ve burada sorunun çözülmesine yardımcı olacak bir SSS girişi olmadığından emin olduysanız, bu durumda buna değer olabilir. hakkında soruyorum.

---

### ASF donuyor gibi görünüyor ve ben bir tuşa basana kadar konsola hiçbir şey yazdırmıyor\!

Büyük ihtimalle Windows kullanıyorsunuz ve konsolunuzda QuickEdit modu etkin. Bakınız [**Bu**](https://stackoverflow.com/questions/30418886/how-and-why-does-quickedit-mode-in-command-prompt-freeze-applications) Teknik açıklama için StackOverflow ile ilgili soru. ASF konsol pencerenize sağ tıklayarak, özellikleri açarak ve uygun onay kutusunun işaretini kaldırarak QuickEdit modunu devre dışı bırakmalısınız.

---

### ASF takas kabul edemez veya gönderemez\!

İlk olarak açık olan şey; yeni hesaplar sınırlı olarak başlar. Cüzdanını yükleyerek veya mağazada 5$ harcayarak hesabınızın kilidini açana kadar ASF, bu hesabı kullanarak işlem göndermeyi de kabul edemez. Bu durumda ASF, içindeki hiçbir kartın takası mümkün olmadığından envanterin boş göründüğünü belirtecektir.

Daha sonra, eğer kullanmıyorsanız [**ASF 2FA**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)ASF'nin aslında ticareti kabul etmesi/göndermesi mümkündür, ancak bunu e-postanız aracılığıyla onaylamanız gerekir. Benzer şekilde, klasik 2FA kullanıyorsanız kimlik doğrulayıcınız aracılığıyla işlemi onaylamanız gerekir. Onaylar: **zorunlu** şimdi, bunları kendi başınıza kabul etmek istemiyorsanız kimlik doğrulayıcınızı ASF 2FA'ya aktarmayı düşünün.

Ayrıca yalnızca arkadaşlarınızla ve ticaret bağlantısı bilinen kişilerle ticaret yapabileceğinizi unutmayın. Eğer başlatmaya çalışıyorsan *Bot \-\> Usta* ticaret mesela `yağma`, o zaman ya botunuzu arkadaş listenizde bulundurmanız gerekir ya da `SteamTradeToken` Bot'un yapılandırmasında bildirildi. Belirtecin geçerli olduğundan emin olun; aksi takdirde işlem gönderemezsiniz.

Son olarak, yeni cihazların 7 günlük takas kilidine sahip olduğunu unutmayın; bu nedenle, hesabınızı ASF'ye yeni eklediyseniz en az 7 gün bekleyin; bu süreden sonra her şey çalışmalıdır. Bu sınırlama şunları içerir: **ikisi birden** kabul etmek **Ve** esnaf gönderiyorum. Her zaman tetiklenmiyor ve anında işlem gönderip kabul edebilen insanlar var. İnsanların çoğunluğu etkileniyor ve kilit **irade** aynı makinedeki buhar istemciniz aracılığıyla takas gönderip kabul edebilseniz bile bu gerçekleşir. Sabırla bekleyin, daha hızlı hale getirmek için yapabileceğiniz hiçbir şey yok. Benzer şekilde, 2FA, SteamGuard, şifre, e-posta ve benzeri gibi Steam güvenliğiyle ilgili çeşitli ayarları kaldırmak/değiştirmek için de benzer kilit alabilirsiniz. Genel olarak, bu hesaptan kendinizin bir işlem gönderip gönderemediğinizi kontrol edin; evet ise, büyük ihtimalle yeni cihazdan gelen klasik 7 günlük kilittir.

Ve son olarak, bir hesapta diğerine yalnızca 5 bekleyen işlem bulunabileceğini unutmayın; bu nedenle, o bottan halihazırda kabul edilecek 5 (veya daha fazla) bekleyen işleminiz varsa ASF, işlem göndermede başarısız olacaktır. Bu nadiren bir sorundur, ancak özellikle ASF'yi otomatik olarak işlem göndermeye ayarladıysanız, ancak ASF 2FA kullanmıyorsanız ve bunları onaylamayı unuttuysanız, şunu da belirtmekte fayda var.

Hiçbir şey yardımcı olmadıysa, her zaman etkinleştirebilirsiniz `Hata ayıklama` moduna geçin ve isteklerin neden başarısız olduğunu kendiniz kontrol edin. Steam'in çoğu zaman saçma sapan konuştuğunu ve sunulan gerekçenin mantıklı olmayabileceğini, hatta tamamen yanlış olabileceğini lütfen unutmayın. Bu nedeni yorumlamaya karar verirseniz, Steam ve tuhaflıkları hakkında yeterli bilgiye sahip olduğunuzdan emin olun. Mantıksal bir neden olmaksızın bu sorunun görülmesi de oldukça yaygındır ve bu durumda önerilen tek çözüm, hesabı ASF'ye yeniden eklemektir (ve tekrar 7 gün beklemek). Bazen bu sorun da kendi kendine düzeliyor *sihirli bir şekilde*aynı şekilde kırılıyor. Bununla birlikte, genellikle ya 7 günlük takas kilitlenmesi, geçici buhar sorunu ya da her ikisi birden söz konusudur. Gerçek nedeni ayıklama dürtüsünüz olmadığı sürece, neyin yanlış olduğunu manuel olarak kontrol etmeden önce birkaç gün vermek en iyisidir (ve genellikle yine de beklemek zorunda kalırsınız, çünkü hata mesajı hiçbir anlam ifade etmez ve size de yardımcı olmaz). en ufak bir şekilde).

Her durumda, ASF yalnızca **denemek** Takas kabul etmek/göndermek için Steam'e uygun bir istek göndermek. Steam'in bu isteği kabul edip etmemesi ASF'nin kapsamı dışındadır ve ASF sihirli bir şekilde bu isteğin çalışmasını sağlamayacaktır. Bu özellikle ilgili bir hata yok ve geliştirilecek bir şey de yok çünkü mantık ASF'nin dışında gerçekleşiyor. Bu nedenle, bozuk olmayan eşyaların onarılmasını istemeyin ve ayrıca ASF'nin neden takas kabul edemediğini veya gönderemediğini sormayın \- **Bilmiyorum ve ASF de bilmiyor**. Ya bununla ilgilenin ya da daha iyisini biliyorsanız kendinizi düzeltin.

---

### Neden her girişte 2FA/SteamGuard kodunu girmem gerekiyor? / *Süresi dolmuş giriş anahtarı kaldırıldı*

ASF oturum açma anahtarlarını kullanır (eğer sakladıysanız) `Oturum Açma Anahtarlarını Kullan` kimlik bilgilerini geçerli tutmak için Steam'in kullandığı mekanizmanın aynısını kullanır \- 2FA/SteamGuard belirteci yalnızca bir kez gereklidir. Bununla birlikte, Steam ağ sorunları ve tuhaflıkları nedeniyle, giriş anahtarının ağda kaydedilmemiş olması tamamen mümkündür; bu tür sorunları yalnızca ASF'de değil, normal Steam istemcisinde de gördük (giriş \+ şifre girilmesi gerekiyor) "beni hatırla" seçeneğinden bağımsız olarak her çalıştırmada).

Kaldırabilirsin `BotAdı.db` Ve `BotAdı.bin` (varsa) etkilenen hesabın bağlantısını kesin ve ASF'yi hesabınıza bir kez daha bağlamayı deneyin, ancak bu muhtemelen hiçbir işe yaramayacaktır. Bazı kullanıcılar bunu bildirdi [**tüm cihazların yetkisini kaldırma**](https://store.steampowered.com/twofactor/manage) Steam tarafında yardımcı olacaktır, şifreyi değiştirmek de aynısını yapacaktır. Ancak bunlar yalnızca çalışması garanti edilmeyen geçici çözümlerdir; gerçek ASF tabanlı çözüm, kimlik doğrulayıcınızı şu şekilde içe aktarmaktır: [**ASF 2FA**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication) \- bu şekilde ASF, ihtiyaç duyulduğunda otomatik olarak belirteçler oluşturabilir ve bunları manuel olarak girmenize gerek kalmaz. Genellikle sorun bir süre sonra sihirli bir şekilde kendiliğinden çözülür, böylece bunun olmasını bekleyebilirsiniz. Elbette Valve'den de çözüm isteyebilirsiniz çünkü Steam ağını giriş anahtarlarımızı kabul etmeye zorlayamam.

Bir yan not olarak, oturum açma anahtarlarını şununla da kapatabilirsiniz: `Oturum Açma Anahtarlarını Kullan` yapılandırma özelliği şuna ayarlandı: `YANLIŞ`, ancak bu sorunu çözmez, yalnızca ilk oturum açma anahtarı hatasını atlayın. ASF, burada açıklanan sorunun zaten farkındadır ve tüm oturum açma kimlik bilgilerini kendisi garanti edebiliyorsa, oturum açma anahtarlarını kullanmamak için elinden geleni yapacaktır; `Oturum Açma Anahtarlarını Kullan` ASF 2FA kullanarak tüm giriş ayrıntılarını birlikte sağlayabiliyorsanız manuel olarak.

---

### Hata alıyorum: *Steam'e giriş yapılamıyor: `Geçersiz şifre` veya `Hız limiti aşıldı`*

Bu hata pek çok anlama gelebilir; bunlardan bazıları şunlardır:

- Geçersiz Giriş/Şifre kombinasyonu (belli ki)
- ASF tarafından oturum açmak için kullanılan süresi dolmuş oturum açma anahtarı
- Kısa sürede çok fazla başarısız oturum açma girişimi (kaba kuvvet karşıtı)
- Kısa sürede çok fazla oturum açma denemesi (hız sınırlayıcı)
- Giriş yapmak için captcha zorunluluğu (yukarıdaki iki nedenden kaynaklanma olasılığı yüksektir)
- Steam Ağının oturum açmanızı engelliyor olabileceği başka herhangi bir neden

Kaba kuvvet önleme ve hız sınırlaması durumunda sorun bir süre sonra ortadan kalkacaktır, bu nedenle bekleyin ve bu arada oturum açmaya çalışmayın. Bu sorunla sık sık karşılaşıyorsanız, belki de artırmak akıllıca olacaktır. `Giriş SınırlayıcıGecikme` ASF'nin config özelliği. Programın aşırı yeniden başlatılması ve diğer kasıtlı/kasıtsız oturum açma istekleri kesinlikle bu soruna yardımcı olmayacaktır, bu nedenle mümkünse bundan kaçınmaya çalışın.

Giriş anahtarının süresinin dolması durumunda \- ASF eskisini kaldıracak ve bir sonraki girişte yenisini isteyecektir (bu, hesabınız 2FA korumalıysa 2FA jetonu koymanızı gerektirir. Hesabınız ASF 2FA kullanıyorsa, jeton oluşturulacak ve otomatik olarak kullanılır). Bu doğal olarak zamanla gerçekleşebilir, ancak her girişte bu sorunla karşılaşırsanız, Steam'in bir nedenden ötürü, sorunda belirtildiği gibi, giriş anahtarı kaydetme isteklerimizi görmezden gelmeye karar vermesi mümkündür. [**üstünde**](\#why-do-i-have-to-put-2fasteamguard-code-on-each-login--removed-expired-login-key). Elbette devre dışı bırakabilirsiniz `Oturum Açma Anahtarlarını Kullan` tamamen, ancak bu sorunu çözmez, yalnızca her seferinde süresi dolmuş oturum açma anahtarlarını kaldırma ihtiyacını ortadan kaldırır. Yukarıdaki konuya göre asıl çözüm ASF 2FA kullanmaktır.

Ve son olarak, yanlış kullanıcı adı \+ şifre kombinasyonunu kullandıysanız, elbette bunu düzeltmeniz veya bu kimlik bilgilerini kullanarak bağlanmaya çalışan botu devre dışı bırakmanız gerekir. ASF kendi başına olup olmadığını tahmin edemez. `Geçersiz şifre` geçersiz kimlik bilgileri veya yukarıda listelenen nedenlerden herhangi biri anlamına gelir; bu nedenle başarılı olana kadar denemeye devam edecektir.

ASF'nin buhardaki tuhaflıklara uygun şekilde tepki verecek kendi yerleşik sistemine sahip olduğunu, eninde sonunda bağlanıp işine devam edeceğini, dolayısıyla sorun geçiciyse herhangi bir şey yapmanıza gerek olmadığını unutmayın. Sorunları sihirli bir şekilde çözmek için ASF'yi yeniden başlatmak, işleri yalnızca daha da kötüleştirecektir (çünkü yeni ASF, önceki ASF'nin oturum açamama durumunu bilmeyecek ve beklemek yerine bağlanmayı deneyecektir), bu nedenle ne yapacağınızı bilmiyorsanız bunu yapmaktan kaçının.

Son olarak, her Steam isteğinde olduğu gibi \- ASF yalnızca **denemek** Sağladığınız kimlik bilgilerini kullanarak oturum açmak için. Bu isteğin başarılı olup olmayacağı ASF'nin kapsamı ve mantığı dışındadır; bu konuda hiçbir hata yoktur ve hiçbir şey düzeltilemez veya iyileştirilemez.

---

### I can't input login details that ASF is asking for
### `System.InvalidOperationException: Cannot read keys when either application does not have a console or when console input has been redirected`
### `System.IO.IOException: Giriş/çıkış hatası`
### `RequestInput() input is invalid!`

If this error happened during ASF input (e.g. you can see `GetUserInput()` in the stacktrace) then it's caused by your environment which prohibits ASF from reading standard input of your console. That can occur due to a lot of reasons, but the most common one is you running ASF in the wrong environment (e.g. in `systemd`, `nohup` or `&` background instead of `screen` on Linux). ASF standart girişine erişemezse bu hatanın günlüğe kaydedildiğini ve ASF'nin çalışma zamanı sırasında ayrıntılarınızı kullanamadığını görürsünüz.

Normally you should correct the above issue, i.e. allow ASF to access standard input so you can supply the details. However, if you **expect** this to happen, so you **intend** to run ASF in input-less environment, then you should explicitly tell ASF that it's the case, by setting **[`Headless`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#headless)** mode appropriately. Bu, ASF'ye hiçbir durumda kullanıcı girdisi istememesini söyleyecek ve ASF'yi girdisiz ortamlarda güvenli bir şekilde çalıştırmanıza olanak tanıyacaktır. You can answer selected input prompts through other means in this mode, e.g. in ASF-ui.

---

### `System.Net.Http.WinHttpException: Bir güvenlik hatası oluştu`

Bu hata, ASF'nin belirli bir sunucuyla güvenli bağlantı kuramaması durumunda, neredeyse tamamen SSL sertifikası güvensizliği nedeniyle meydana gelir.

Neredeyse tüm durumlarda bu hatanın nedeni **makinenizde yanlış tarih/saat**. Her SSL sertifikasının yayınlanma tarihi ve son kullanma tarihi vardır. Tarihiniz geçersizse ve bu iki sınırın dışındaysa potansiyel bir nedenden dolayı sertifikaya güvenilemez. [**MITM**](https://en.wikipedia.org/wiki/Man-in-the-middle\_attack) saldırıyor ve ASF bağlantı kurmayı reddediyor.

Açık çözüm, makinenizdeki tarihi uygun şekilde ayarlamaktır. Windows'ta bulunan yerel senkronizasyon gibi otomatik tarih senkronizasyonunun kullanılması önemle tavsiye edilir veya `ntpd` Linux'ta.

Makinenizdeki tarihin uygun olduğundan emin olduysanız ve hatanın ortadan kalkmasını istemiyorsanız sisteminizin güvendiği SSL sertifikaları güncelliğini kaybetmiş veya geçersiz olabilir. Bu durumda, örneğin erişip erişemediğinizi kontrol ederek makinenizin güvenli bağlantılar kurabildiğinden emin olmalısınız. `https://github.com` seçtiğiniz herhangi bir tarayıcıyla veya CLI aracıyla `kıvırmak`. If you confirmed that this works properly, feel free to ask us for support.

---

### `System.Threading.Tasks.TaskCanceledException: Bir görev iptal edildi`

Bu uyarı, Steam'in ASF isteğine belirtilen sürede yanıt vermediği anlamına gelir. Genellikle Steam ağındaki aksaklıklardan kaynaklanır ve ASF'yi hiçbir şekilde etkilemez. Diğer durumlarda bu, isteğin 5 denemeden sonra başarısız olmasıyla aynıdır. Steam'i taleplerimize yanıt vermeye zorlayamayacağımız için bu sorunu bildirmenin çoğu zaman bir anlamı yoktur.

---

### `'System.Security.Cryptography.CngKeyLite' için tür başlatıcısı bir istisna attı`

Bu sorun neredeyse yalnızca devre dışı bırakılmış/durdurulmuş `CNG Anahtar İzolasyonu` ASF için temel şifreleme işlevselliği sağlayan ve olmadan programın çalışamayacağı Windows hizmeti. Başlatarak bu sorunu çözebilirsiniz. `services.msc` ve bunu sağlamak `CNG Anahtar İzolasyonu` Windows hizmetinde başlatma devre dışı bırakılmamış ve şu anda çalışıyor.

---

### ASF, AntiVirus'um tarafından kötü amaçlı yazılım olarak algılanıyor\! Neler oluyor?

**ASF'yi güvenilir kaynaktan indirdiğinizden emin olun**. The only official and trusted source is **[ASF releases](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** page on GitHub (and this is also the source for ASF auto-updates) - **any other source is untrusted by definition and can contain malware added by other people** - you should not trust any other download location, ensure that your ASF always comes from us.

ASF'nin güvenilir bir kaynaktan indirildiğini doğruladıysanız, büyük olasılıkla bu yalnızca yanlış bir pozitiftir. Bu **geçmişte oldu**, **şu anda oluyor**, Ve **gelecekte olacak**. ASF kullanırken gerçek güvenlik konusunda endişeleniyorsanız, gerçek algılama oranı için ASF'yi birçok farklı AV ile taramanızı öneririm; [**VirüsToplam**](https://www.virustotal.com) (veya bunun gibi seçtiğiniz başka bir web hizmeti).

Kullandığınız AV yanlışlıkla ASF'yi kötü amaçlı yazılım olarak algılarsa, o zaman **Bu dosya örneğini AV'nizin geliştiricilerine geri göndermek iyi bir fikirdir, böylece onu analiz edebilirler ve algılama motorlarını geliştirebilirler.**, açıkça düşündüğün kadar iyi çalışmıyor. ASF kodunda bir sorun yok ve bizim için düzeltecek bir şey de yok, çünkü ilk etapta kötü amaçlı yazılım dağıtmıyoruz, bu nedenle bu yanlış pozitifleri bize bildirmenin bir anlamı yok. Yukarıda belirtildiği gibi daha fazla analiz için ASF örneğini göndermenizi önemle tavsiye ederiz, ancak bununla uğraşmak istemiyorsanız her zaman bazı AV istisnalarına ASF ekleyebilir, AV'nizi devre dışı bırakabilir veya başka bir tane kullanabilirsiniz. Ne yazık ki AV'lerin aptal olmasına alışığız, çünkü arada bir bazı AV'ler ASF'yi bir virüs olarak algılıyor, bu genellikle çok kısa sürüyor ve geliştiriciler tarafından hızlı bir şekilde düzeltiliyor, ancak yukarıda da belirttiğimiz gibi \- **oldu**, **olur** Ve **olacak** her zaman. ASF herhangi bir kötü amaçlı kod içermez, ASF kodunu inceleyebilir ve hatta kendiniz kaynaktan derleyebilirsiniz. AV buluşsal yöntemlerinden ve yanlış pozitiflerden saklanmak için ASF kodunu gizleyen bilgisayar korsanları değiliz, bu nedenle bizden bozuk olmayan şeyleri düzeltmemizi beklemeyin; bizim düzeltebileceğimiz bir "virüs" yok.
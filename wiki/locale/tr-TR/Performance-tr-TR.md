# Performans

ASF'nin birincil amacı, ASF'nin kendi başına tahmin etmesi veya kontrol etmesi imkansız olan küçük bir kullanıcı sağladığı veri seti ile otomatik olarak kontrol edilebilen daha büyük bir veri setine dayanarak mümkün olan en verimli şekilde çiftlik yapmak.

Otomatik modda, ASF, hangi oyunların çiftlik yapılacağını seçmenize veya kart çiftlik algoritmasını değiştirmenize izin vermez. **ASF, ne yapması gerektiğini ve mümkün olan en hızlı şekilde çiftlik yapmak için hangi kararları vermesi gerektiğini sizden daha iyi bilir.** Amacınız, ASF'nin kendi başına tahmin edemediği yapılandırma özelliklerini uygun şekilde ayarlamaktır; diğer her şey kapsanmıştır. Your objective is to set config properties properly, as ASF can't guess them on its own, everything else is covered.

---

Bir süre önce Valve, kart düşüşleri için algoritmayı değiştirdi. Bu noktadan itibaren, Steam hesaplarını iki kategoriye ayırabiliriz: **kart düşüşleri kısıtlı** ve **kart düşüşleri kısıtlı olmayan** hesaplar. Bu iki tür arasındaki tek fark, kart düşüşleri kısıtlı olan hesapların, verilen bir oyundan kart almak için bu oyunu en az `X` saat oynamaları gerektiğidir. Geri ödeme talep etmeyen eski hesapların **kısıtlı olmayan kart düşüşleri** olduğu görünmektedir, yeni hesaplar ve geri ödeme talebinde bulunanlar ise **kısıtlı kart düşüşleri** içerir. Ancak bu yalnızca bir teoridir ve kural olarak alınmamalıdır. Bu nedenle, **açık bir cevap yoktur** ve ASF, hangi durumun hesabınız için uygun olduğunu **sizden** öğrenir.

---

ASF'nin şu anda iki çiftlik algoritması bulunmaktadır:

**`Basit`** algoritması, kısıtlı olmayan kart düşüşleri olan hesaplar için en iyi şekilde çalışır. Bu, ASF tarafından kullanılan birincil algoritmadır. Bot, çiftlik yapılacak oyunları bulur ve her bir oyundaki tüm kartlar düşene kadar tek tek çiftlik yapar. Bu, birden fazla oyunu çiftlik yaparken kart düşüş oranının sıfıra yakın ve tamamen etkisiz olduğunu gösterir.

**`Karmaşık`** algoritma, kısıtlı hesapların da karlarını maksimize etmelerine yardımcı olmak için uygulanmıştır. ASF, önce `HoursUntilCardDrops` saatlik oynama süresini geçen tüm oyunlar üzerinde standart **`Basit`** algoritmasını kullanır, ardından `HoursUntilCardDrops` saatten daha az kalan oyunlarla (32 sınırına kadar) aynı anda çiftlik yapar, herhangi biri `HoursUntilCardDrops` saat işaretini geçtiğinde, ASF döngüyü baştan başlatır (o oyunda **`Basit`** algoritmasını kullanarak, < `HoursUntilCardDrops` olanlarda aynı anda çiftlik yaparak devam eder). Bu durumda, gereken saatlere oyunları getirmek için çoklu oyun çiftlik kullanabiliriz. Oyun saatleri çiftlik yaparken, ASF **kart çiftliği yapmaz** ve bu süre zarfında herhangi bir kart düşüşünü kontrol etmez (yukarıda belirtilen nedenlerle).

Şu anda, ASF kart çiftlik algoritmasını tamamen `HoursUntilCardDrops` yapılandırma özelliğine (ki bu **siz** tarafından ayarlanır) dayanarak seçmektedir. `HoursUntilCardDrops` `0` olarak ayarlanmışsa, **`Basit`** algoritması kullanılacaktır; aksi takdirde, **`Karmaşık`** algoritması kullanılacaktır - kart düşüşleri için çiftlik yapmadan önce tüm oyunların oynama süresini verilen saat miktarına getirmek için yapılandırılmıştır.

---

### **There is no obvious answer which algorithm is better for you**.

Bu, kart çiftlik algoritmasını seçmemenizin nedenlerinden biridir; bunun yerine, ASF'ye hesabınızın kısıtlı düşüşlere sahip olup olmadığını söylersiniz. Hesabınız kısıtlı olmayan kart düşüşlerine sahipse, **`Basit`** algoritması bu hesapta **daha iyi çalışacaktır**, çünkü tüm oyunları `X` saate getirme süresini boşa harcamayacağız - kart düşüş oranı, birden fazla oyun çiftlik yaparken %0'a yakındır. Öte yandan, hesabınızda kart düşüşleri kısıtlıysa, **`Karmaşık`** algoritması sizin için daha iyi olacaktır, çünkü bir oyunun `HoursUntilCardDrops` saatine ulaşmadan solo çiftlik yapmanın anlamı yoktur - bu nedenle önce **oyun süresini çiftlik yapacağız**, **sonra** solo modda kartları çiftlik yapacağız.

Başkasının size söylediği gibi `HoursUntilCardDrops` ayarını körü körüne yapmayın - testler yapın, sonuçları karşılaştırın ve aldığınız verilere dayanarak hangi seçeneğin sizin için daha iyi olduğunu belirleyin. Biraz çaba harcarsanız, ASF'nin hesabınız için maksimum verimlilikte çalışmasını sağlayabilirsiniz ki bu, şu anda bu wiki sayfasını okuduğunuzu göz önünde bulundurursak, muhtemelen istediğiniz şeydir. Herkes için çalışan bir çözüm olsaydı, bir seçim yapmazdınız - ASF kendisi karar verirdi.

---

### Hesabınızın kısıtlı olup olmadığını nasıl en iyi şekilde öğrenirsiniz?

Oynamama sürelerinde en az 5 oyun içeren **oyun kaydı olmayan** bazı oyunlara sahip olduğunuzdan emin olun ve `HoursUntilCardDrops` değerini `0` olarak ayarlayarak ASF'yi çalıştırın. Daha doğru sonuçlar için, çiftlik yapma dönemi boyunca hiçbir şey oynamadığınızdan emin olun (ASF'yi gece boyunca çalıştırmak en iyisidir). It would be a good idea if you didn't play anything during farming period for more accurate results (best to run ASF during the night). ASF'nin bu 5 oyunu çiftlik yapmasına izin verin ve ardından sonuçları kontrol edin.

ASF, belirli bir oyun için bir kartın düştüğünü açıkça belirtir. Hedefiniz, ASF tarafından elde edilen en hızlı kart düşüşüdür. Örneğin, hesabınız kısıtlı değilse, ilk kart düşüşü çiftlik yapmaya başladığınızdan yaklaşık 30 dakika sonra olmalıdır. Bu ilk 30 dakika içinde en az bir oyunda kart düştüğünü fark ederseniz, bu, hesabınızın **kısıtlı olmadığını** ve `HoursUntilCardDrops` değerini `0` olarak ayarlamanız gerektiğini gösterir.

Öte yandan, her oyunun ilk kartını düşürmeden en az `X` saat geçtiğini fark ederseniz, bu, `HoursUntilCardDrops`'ı ayarlamanız gereken bir gösterge olacaktır. Çoğu (belki de tüm) kısıtlı kullanıcılar, kartların düşmesi için en az `3` saat oynama süresine ihtiyaç duyar ve bu da `HoursUntilCardDrops` ayarının varsayılan değeridir.

Oyunların farklı düşüş oranlarına sahip olabileceğini unutmayın, bu yüzden teorinizin doğru olup olmadığını test etmek için **en az 3 oyun, tercihen 5+** oyun kullanmalısınız, böylece tesadüfi yanlış sonuçlarla karşılaşmadığınızdan emin olabilirsiniz. Bir oyunda bir saatten daha kısa sürede bir kart düşüşü, hesabınızın **kısıtlı olmadığını** ve `HoursUntilCardDrops`'ı `0` olarak ayarlamanız gerektiğini doğrular, ancak hesabınızın **kısıtlı** olduğunu doğrulamak için, belirli bir işarete ulaşmadan kart düşürmeyen en az birkaç oyuna ihtiyacınız vardır.

Geçmişte `HoursUntilCardDrops` yalnızca `0` veya `2` olarak ayarlanabiliyordu ve bu nedenle ASF'nin iki değer arasında geçiş yapmak için tek bir `CardDropsRestricted` özelliği vardı. Son değişikliklerle birlikte, çoğu kullanıcının artık önceki `2` yerine `3` saate ihtiyaç duyduğunu ve `HoursUntilCardDrops`'ın artık dinamik olduğunu ve hesap bazında herhangi bir değere ulaşabileceğini fark ettik.

Sonuç olarak, karar tabii ki size bağlıdır.

Ve daha da kötüleştirmek için - bazı kişilerin Steam hataları (evet, birçok böyle var) veya Valve tarafından yapılan bazı mantık ayarlamaları nedeniyle kısıtlı durumdan kısıtlı olmayan duruma geçtiğini yaşadım. Dolayısıyla, hesabınızın kısıtlı olduğunu doğruladıysanız (veya değilse), bunun böyle kalacağını düşünmeyin - kısıtlı olmayan duruma geçmek için geri ödeme talep etmek yeterlidir. Önceden belirlenmiş değerin artık uygun olmadığını düşünüyorsanız, her zaman tekrar test yapabilir ve gerektiği şekilde güncelleyebilirsiniz.

---

Varsayılan olarak, ASF, `HoursUntilCardDrops`'ı `3` olarak varsayar, çünkü bunun daha küçük olması gerektiğinde `3`'ü ayarlamanın olumsuz etkisi, diğer yoldan yapılmasına göre daha küçüktür. Bunun nedeni, en kötü durumda, her `32` oyun için `3` saat çiftlik yapmayı boşa harcayacağız; ancak `HoursUntilCardDrops` varsayılan olarak `0` olarak ayarlanmış olsaydı, her oyunun çiftlik yapma süresi boşa harcanacaktı. However, you should still tune this variable to match your account for maximum efficiency, as this is only a blind guess based on potential drawbacks and majority of users (so we're trying to choose "lesser evil" by default).

Şu anda iki algoritma, tüm mevcut hesap senaryoları için yeterlidir ve en verimli şekilde çiftlik yapmak için kullanılabilir, bu nedenle başka algoritmalar eklenmesi planlanmamaktadır.

ASF'nin ayrıca `play` komutu aracılığıyla etkinleştirilebilen manuel çiftlik yapma modu da içerdiğini belirtmek güzel. **[Komutlar](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** hakkında daha fazla bilgi edinebilirsiniz.

---

## Steam Hataları

Kart düşüşleri algoritması her zaman olması gerektiği gibi çalışmayabilir ve çeşitli Steam hatalarının meydana gelmesi tamamen mümkündür, örneğin kartların kısıtlı hesaplarda düşmesi, kartların oyun kapandığında/değiştirildiğinde düşmesi, kartların hiç düşmemesi gibi.

Bu bölüm, ASF'nin neden **X**'i yapmadığını merak edenler içindir, örneğin kartları daha hızlı çiftlik yapmak için oyunları hızlı bir şekilde değiştirmek.

Bir **Steam hatası** nedir - belirli bir eylemin **belirsiz** bir davranışı tetiklemesi, **niyet edilmemiş, belgelenmemiş ve mantık hatası olarak kabul edilen** bir durumdur. **Tanımsızdır**, bu, temiz bir test ortamında güvenilir bir şekilde yeniden üretilemeyeceği anlamına gelir ve bu nedenle, hatanın meydana gelip gelmediğini ve bunun nasıl aşılacağını tahmin etmeye çalışan hacklere başvurulmadan kodlanmıştır. Genellikle geliştiriciler mantık hatasını düzeltene kadar geçici olarak devam eder, ancak bazı küçük hatalar uzun süre fark edilmeden kalabilir.

Bir **Steam hatası** olarak kabul edilen iyi bir örnek, oyun kapatıldığında kart düşürülme durumu olup, bu durum, bir dereceye kadar Idle Master'ın oyun atlama fonksiyonu ile kullanılabilir.

- **Tanımsız Davranış** - Hatanın tetiklenmesi durumunda 0 veya 1 kart düşüp düşmeyeceğini söyleyemezsiniz.
- **Not intended** - based on past experience and behaviour of Steam network that doesn't result in same behaviour when sending a single request.
- **Belgelenmemiş** - Steam web sitesinde kartların nasıl elde edileceği açıkça belgelenmiştir ve **her yerde** kartların **oynayarak** elde edildiği belirtilmiştir.
- **Mantık Hatası Olarak Kabul Edilmiştir** - Oyunları kapatma veya değiştirme, kartların düşmesi üzerinde herhangi bir etkiye sahip olmamalıdır, çünkü kartların **oynama süresi** ile elde edileceği belirtilmiştir.
- **Tanımsızdır, Güvenilir Olarak Yeniden Üretilemez** - Herkes için çalışmaz ve bir kez işe yaramış olsa bile, ikinci kez işe yaramayabilir.

Artık Steam hatasının ne olduğunu ve kartların oyun kapatıldığında düşmesinin **bir hata** olduğunu fark ettiğimizde, ikinci noktaya geçebiliriz - **ASF tanımı gereği Steam ağını kötüye kullanmaz ve Steam ToS, protokoller ve genel olarak kabul edilenlerle uyum sağlamaya çalışır.** Steam ağını sürekli oyun açma/kapatma talepleriyle spam yapmak, **[DoS saldırısı](https://en.wikipedia.org/wiki/Denial-of-service_attack)** olarak değerlendirilebilir ve **[Steam Çevrimiçi Davranış](https://store.steampowered.com/online_conduct/?l=english)** kurallarını doğrudan ihlal eder. Spamming Steam network with constant game opening/closing requests can be considered a **[DoS attack](https://en.wikipedia.org/wiki/Denial-of-service_attack)** and **directly violates [Steam Online Conduct](https://store.steampowered.com/online_conduct/?l=english)**.

> Bir Steam abonesi olarak, aşağıdaki davranış kurallarına uymayı kabul edersiniz.
> 
> Şunları yapmayacaksınız:
> 
> Bir Steam sunucusuna saldırmak veya Steam'i başka şekilde bozmak.

Steam hatasını diğer programlarla (örneğin IM) tetikleyip tetiklemediğiniz veya bizimle aynı fikirde olup olmadığınız, böyle bir davranışın DoS saldırısı olarak değerlendirilip değerlendirilmediği, Valve'ın kararına bağlıdır, ancak bu durumu kötüye kullanmak/kötüye kullanmak için çok sayıda Steam ağ talepleri göndermek, Valve'ın da benzer bir görüşe sahip olacağına emin olabilirsiniz.

ASF **asla** Steam hatalarından, kötüye kullanımlardan, hacklerden veya Steam ağında istenmeyen olarak gördüğümüz herhangi bir illegal veya istenmeyen etkinlikten yararlanmayacaktır, bu da **[katkıda bulunma](https://github.com/JustArchiNET/ArchiSteamFarm/blob/main/.github/CONTRIBUTING.md)** bölümünde belirtilmiştir.

Birkaç kuruş kartı daha hızlı elde etmek için Steam hesabınızı riske atmak istiyorsanız, üzülerek belirtmeliyim ki ASF otomatik modda bu tür bir işlev sunmayacaktır, ancak **[komut](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** aracılığıyla istediğiniz her şeyi yapabilirsiniz. Steam hatalarını ve bunları kendi yararınıza kullanmayı tavsiye etmiyoruz - ASF ile veya başka bir araçla olsun. Sonuçta, hesabınız ve bununla ne yapmak istediğiniz size bağlıdır - sadece sizi uyardık.
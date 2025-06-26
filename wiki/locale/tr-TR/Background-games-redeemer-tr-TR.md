# Arka Plan Oyunları Kurtarıcı

Arka Plan Oyunları Kurtarıcı, belirli bir dizi Steam CD-anahtarını (isimleriyle birlikte) arka planda kullanılmak üzere içe aktarmanızı sağlayan özel bir ASF özelliğidir. Bu özellikle, çok sayıda anahtarınız varsa ve tüm toplu işleminiz tamamlanmadan önce `RateLimited` **[durumunu](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#what-is-the-meaning-of-status-when-redeeming-a-key)** ulaşmayı garantilediyseniz kullanışlıdır.

Arkaplan oyunları kurtarıcısı, tek bir bot alanına sahip olacak şekilde yapılır, bu da `RedeemingPreferences`'dan yararlanmadığı anlamına gelir. Bu özellik, gerekirse, `redeem` **[komutu](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** ile birlikte (veya bunun yerine) kullanılabilir.

---

## İçe Aktarma

İçe aktarma işlemi iki yolla yapılabilir - bir dosya kullanarak veya IPC kullanarak.

### Dosya

ASF, kendi `yapılandırma` dizininde `BotName.keys` adlı bir dosyayı, `BotName` botunuzun adı olduğunu görecektir. Bu dosya, her biri bir tab karakteriyle ayrılmış ve bir sonraki girdiyi belirtmek için yeni bir satırla biten, oyun adıyla birlikte CD anahtarının sabit bir yapısına sahiptir. Birden fazla tab kullanılıyorsa, ilk girdi oyunun adı, son girdi bir CD anahtarı olarak kabul edilir ve aradaki her şey yok sayılır. Örneğin:

```text
POSTAL 2    ABCDE-EFGHJ-IJKLM
Domino Craft VR 12345-67890-ZXCVB
A Week of Circus Terror POIUY-KJHGD-QWERT
Terraria    ThisIsIgnored   ThisIsIgnoredToo    ZXCVB-ASDFG-QWERT
```

Alternatif olarak, sadece anahtar formatını da kullanabilirsiniz (her giriş arasında yeni bir satırla). Bu durumda ASF, doğru ismi doldurmak için Steam'in yanıtını (mümkünse) kullanacaktır. Herhangi bir tür anahtar etiketlemesi için anahtarlarınıza kendiniz isim vermenizi öneririz, çünkü Steam'de kullanılan paketler etkinleştirilen oyunların mantığını takip etmek zorunda değildir, bu yüzden geliştiricinin ne koyduğuna bağlı olarak doğru oyun isimlerini, özel paket isimlerini (örneğin Humble Indie Bundle 18) veya tamamen yanlış ve potansiyel olarak kötü amaçlı isimleri (örneğin Half-Life 4) görebilirsiniz.

```text
ABCDE-EFGHJ-IJKLM
12345-67890-ZXCVB
POIUY-KJHGD-QWERT
ZXCVB-ASDFG-QWERT
```

Hangi biçime bağlı kalmaya karar vermiş olursanız olun, ASF, bot başlangıcında veya daha sonra yürütme sırasında `keys` dosyanızı içe aktarır. Dosyanızın başarılı bir şekilde ayrıştırılmasından ve geçersiz girişlerin olası olarak atılmasından sonra, düzgün tespit edilen tüm oyunlar arka plan kuyruğuna eklenecek ve `BotName.keys` dosyasının kendisi `config` dizininden kaldırılacaktır.

### IPC

Yukarıda belirtilen anahtar dosyasını kullanmanın yanı sıra, ASF ayrıca herhangi bir IPC aracı tarafından yürütülebilecek olan `GamesToRedeemInBackground` **[ASF API uç noktasını](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-api)** da açığa çıkarır, ASF-ui da dahil olmak üzere. IPC kullanmak daha güçlü olabilir, çünkü özel bir ayırıcı kullanma gibi uygun ayrıştırmayı kendiniz yapabilirsiniz, veya tamamen kendi özelleştirilmiş anahtar yapınızı tanıtabilirsiniz.

---

## Kuyruk

Oyunlar başarıyla içe aktarıldığında, sıraya eklenirler. Bot, Steam ağına bağlı olduğu sürece ve kuyruk boş değilse ASF otomatik olarak arka plan kuyruğundan geçer. `RateLimited` ile sonuçlanmayan bir anahtar, `config` dizininde bir dosyaya düzgün bir şekilde yazılmış durumu ile birlikte kuyruktan kaldırılır - işlemde kullanılan bir anahtar ise (örneğin `NoDetail`, `BadActivationCode`, `DuplicateActivationCode`) `BotName.keys.used` veya aksi takdirde `BotName.keys.unused`. ASF, anahtarın Steam ağı tarafından anlamlı bir ad alması garanti edilmediği için kasıtlı olarak sağladığınız oyunun adını kullanır - bu şekilde, gerekirse/kullanmak isterseniz anahtarlarınızı özel adlar kullanarak etiketleyebilirsiniz.

İşlem sırasında hesabımız `RateLimited` durumuna ulaşırsa, kuyruk geçici olarak soğuma süresinin kaybolmasını beklemek için bir saat boyunca askıya alınır. Ardından, süreç kaldığı yerden devam eder, ta ki tüm kuyruk boşalana veya başka bir `RateLimited` oluşana kadar.

---

## Örnek

100 anahtarlık bir listeniz olduğunu varsayalım. Öncelikle ASF `config` dizininde yeni bir `BotunuzunAdı.keys.new` dosyası oluşturmalısınız. ASF'ye bu dosyayı oluşturulduğu anda almaması gerektiğini bildirmek için `.new` uzantısını ekledik (çünkü yeni boş dosya olduğundan, içe aktarmaya henüz hazır değil).

Şimdi yeni dosyamızı açabilir ve 100 anahtar listemizi oraya kopyala-yapıştır yapabilirsiniz, gerekiyorsa formatı düzeltebilirsiniz. Düzeltilen `BotName.keys.new` dosyamız tam olarak 100 (veya 101, son yeni satır ile) satıra sahip olacak, her satır `GameName\tcd-key\n` yapısında olacak, burada `\t` tab karakteri ve `\n` yeni satırdır.

Şimdi bu dosyayı `BotName.keys.new`'den `BotName.keys`'e yeniden adlandırarak ASF'nin alması için hazır olduğunu belirtmeye hazırsınız. Bunu yaptığınız anda, ASF dosyayı otomatik olarak içe aktaracak (yeniden başlatmaya gerek kalmadan) ve ardından silerek tüm oyunlarımızın ayrıştırıldığını ve kuyruğa eklendiğini onaylayacaktır.

`BotName.keys` dosyasını kullanmak yerine, IPC API uç noktasını da kullanabilirsiniz veya her ikisini birden kombin edebilirsiniz.

Bir süre sonra, `BotName.keys.used` ve `BotName.keys.unused` dosyaları oluşturulacaktır. Bu dosyalar, kullanma sürecimizin sonuçlarını içerir. Örneğin, `BotName.keys.unused` dosyasını `BotName2.keys` dosyasına yeniden adlandırarak kullanılmayan anahtarlarımızı başka bir bot için gönderebilirsiniz, çünkü önceki bot bu anahtarları kullanmamıştır. Ya da kullanılmayan anahtarları başka bir dosyaya kopyala-yapıştır yaparak daha sonra kullanmak üzere saklayabilirsiniz, sizin tercihiniz. ASF kuyruğundan geçerken yeni girdilerin çıktı `used` ve `unused` dosyalarımıza ekleneceğini unutmayın, bu nedenle bunları kullanmadan önce kuyruğun tamamen boşalmasını beklemek önerilir. Bu dosyalara kuyruğun tamamen boşalmasından önce erişmeniz gerekiyorsa, öncelikle erişmek istediğiniz çıkış dosyasını başka bir dizine **taşımalı**, **ardından** ayrıştırmalısınız. Bunun nedeni, ASF'nin siz işinizi yaparken bazı yeni sonuçlar ekleyebilmesi ve bu durumun bazı anahtarların kaybına yol açabilmesidir; örneğin, içinde 3 anahtar olan bir dosyayı okuyup ardından silerseniz, ASF'nin bu arada kaldırdığınız dosyaya 4 anahtar daha eklemiş olmasını tamamen kaçırabilirsiniz. Bu dosyalara erişmek istiyorsanız, onları ASF `config` dizininden başka bir yere taşıyarak bunu yapmalısınız, örneğin yeniden adlandırarak.

Kuyruğumuzda bazı oyunlar varken, yukarıdaki adımları tekrarlayarak içe aktarma için ekstra oyunlar eklemek de mümkündür. ASF, ekstra girdilerimizi mevcut kuyrukla düzgün bir şekilde ekleyip sonunda işlem yapacaktır.

---

## Notlar

Arka plan anahtar kurtarıcı, altında `OrderedDictionary` kullanır, bu da CD-anahtarlarınızın dosyada belirtildiği sırayı koruyacağı anlamına gelir (veya IPC API çağrısı). Bu, belirli bir CD-anahtarının yalnızca yukarıda listelenen CD-anahtarlarına doğrudan bağımlılığı olabileceği anlamına gelir, ancak aşağıdakilere değil. Örneğin, `G` oyununu önce etkinleştirmeyi gerektiren `D` DLC'sine sahipseniz, `G` oyununun CD-anahtarı **her zaman** `D` DLC'sinden önce eklenmelidir. Aynı şekilde, `D` DLC'si `A`, `B` ve `C` üzerine bağımlılıklara sahipse, bunların tümü önce eklenmelidir (kendi bağımlılık

Yukarıdaki şemayı takip etmemek, hesabınızın tüm kuyruğunu geçtikten sonra etkinleştirilmesi uygun olsa bile, `DoesNotOwnRequiredApp` ile `DLC'nizin etkinleştirilmemesiyle` sonuçlanacaktır. Bunu önlemek istiyorsanız, kuyrukta her zaman temel oyundan sonra DLC'nin eklenmesini sağlamalısınız.
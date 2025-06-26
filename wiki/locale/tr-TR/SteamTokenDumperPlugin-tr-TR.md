# SteamTokenDumper Eklentisi

`SteamTokenDumperPlugin` is official ASF **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** developed by us, which allows you to contribute to **[SteamDB](https://steamdb.info)** project by sharing package tokens, app tokens and depot keys that your Steam account has access to. Toplanan veriler ve SteamDB'nin neden bu verilere ihtiyaç duyduğuna dair genişletilmiş bilgiye SteamDB'nin **[Token Dumper](https://steamdb.info/tokendumper)** sayfasından ulaşabilirsiniz. Gönderilen veriler herhangi bir hassas bilgi içermez ve yukarıdaki açıklamada belirtilen güvenlik/gizlilik riskini taşımaz.

---

## Enabling the plugin

ASF, `SteamTokenDumperPlugin` eklentisini sürümle birlikte paketler, ancak eklenti varsayılan olarak devre dışı bırakılmıştır. Eklentiyi etkinleştirmek için `SteamTokenDumperPluginEnabled` ASF global yapılandırma özelliğini `true` olarak ayarlamanız gerekir, JSON sözdizimiyle:

```json
{
  "SteamTokenDumperPluginEnabled": true
}
```

ASF programı başlatıldığında, eklenti başarılı bir şekilde etkinleştirilip etkinleştirilmediğini standart ASF günlükleme mekanizması aracılığıyla size bildirecektir. Ayrıca, eklentiyi web tabanlı yapılandırma oluşturucumuz aracılığıyla da etkinleştirebilirsiniz.

---

## Teknik Detaylar

Eklenti etkinleştirildiğinde, eklenti botlarınızı, paket token'ları, uygulama token'ları ve botlarınızın erişim sağladığı depo anahtarlarını toplamak için kullanacaktır. Veri toplama modülü, veri toplama işlemi sırasında ek yükü en aza indirmek için pasif ve aktif rutinler içerir.

Planlanan kullanım senaryosunu gerçekleştirmek için, yukarıda açıklanan veri toplama rutinine ek olarak, belirli aralıklarla SteamDB'ye hangi verilerin gönderilmesi gerektiğini belirlemekle sorumlu bir gönderim rutini başlatılır. Bu rutin, ASF başlatıldığından itibaren en fazla `1` saat içinde çalışacak ve her `24` saatte bir kendini tekrar edecektir. Eklenti, gönderilmesi gereken veri miktarını en aza indirmek için elinden geleni yapacaktır, bu nedenle eklentinin topladığı bazı veriler gönderilmesi gereksiz olarak değerlendirilebilir ve dolayısıyla atlanabilir (örneğin, erişim token'ını değiştirmeyen bir uygulama güncellemesi).

Eklenti, `config/SteamTokenDumper.cache` konumunda kaydedilen kalıcı bir önbellek veritabanı kullanır. Bu dosya, ASF için `config/ASF.db` dosyasına benzer bir amaç taşır. Dosya, toplanan ve gönderilen verileri kaydetmek ve farklı ASF çalıştırmaları arasında yapılması gereken iş miktarını en aza indirmek için kullanılır. Dosyanın kaldırılması, sürecin sıfırdan başlamasına neden olur ve bu, mümkünse kaçınılmalıdır.

---

## Veri

ASF, istekte bulunan katkıda bulunan `steamID`'yi içerir. Bu `SteamOwnerID` olarak belirlediğiniz veya belirlemediyseniz, en fazla lisansa sahip botun Steam ID'sidir. İlan edilen katkıda bulunan, sürekli yardım için SteamDB'den ek avantajlar alabilir (örneğin, web sitesinde bağışçı rütbesi), ancak bu tamamen SteamDB'nin takdirine bağlıdır.

Her durumda, SteamDB personeli yardımınız için şimdiden teşekkür eder. Gönderilen veriler, SteamDB'nin işlevini yerine getirmesine, özellikle paketler, uygulamalar ve depolar hakkındaki bilgileri takip etmesine olanak sağlar; bu da yardımınız olmadan mümkün olmazdı.

---

## Komut

STD eklentisi, seçili botlar için yenileme ve gönderim yapmanızı sağlayan ekstra bir ASF komutu olan `std [Bots]` ile birlikte gelir. Komutu kullanmak, etkinleştirilmiş yapılandırma gerektirmediğinden, otomatik veri toplama ve gönderimden kaçınmanıza ve süreci manuel olarak kontrol etmenize olanak tanır. Doğal olarak, komutu etkinleştirilmiş yapılandırmayla da çalıştırabilirsiniz; bu durumda yalnızca alışılmış veri toplama ve gönderim prosedürlerini beklenenden daha erken tetikler.

Tüm mevcut botlar için yenileme tetiklemek adına `!std ASF` komutunu öneririz. Ancak, yalnızca seçili botlar için de tetikleyebilirsiniz.

---

## Gelişmiş yapılandırma

Eklentimiz, iç yapılandırmaları tercihinize göre ince ayar yapmanıza yardımcı olabilecek gelişmiş yapılandırmayı destekler.

Gelişmiş yapılandırmanın `ASF.json` içinde bulunan yapısı aşağıdaki gibidir:

```json
{
  "SteamTokenDumperPlugin": {
    "Enabled": false,
    "SecretAppIDs": [],
    "SecretDepotIDs": [],
    "SecretPackageIDs": [],
    "SkipAutoGrantPackages": true
  }
}
```

Tüm seçenekler aşağıda açıklanmıştır:

### `Enabled`

`false` varsayılan değerine sahip `bool` türünde bir özellik. Bu özellik, yukarıda açıklanan `SteamTokenDumperPluginEnabled` kök seviyeli özelliğiyle aynı şekilde çalışır ve genellikle diğer gelişmiş özellikleri kullanan kişiler için (dolayısıyla çoğunlukla diğer gelişmiş özellikleri kullananlar) eklentiyle ilgili yapılandırmanın kendi yapısında olmasını tercih edenler için kullanılabilir.

---

### `SecretAppIDs`

Varsayılan değeri boş olan `ImmutableHashSet<uint>` türünde bir özellik. Bu özellik, eklentinin çözmeyeceği `appIDs`'yi belirtir ve eğer zaten çözülmüşse, token'ı göndermeyecektir. Bu özellik, özellikle geliştiriciler, yayınevleri veya kapalı beta testçileri gibi yayımlanmamış ürünler hakkında potansiyel olarak hassas bilgilere erişimi olan kişiler için yararlı olabilir.

---

### `SecretDepotIDs`

Varsayılan değeri boş olan `ImmutableHashSet<uint>` türünde bir özellik. Bu özellik, eklentinin çözmeyeceği `depotIDs`'yi belirtir ve eğer zaten çözülmüşse, anahtarı göndermeyecektir. Bu özellik, özellikle geliştiriciler, yayınevleri veya kapalı beta testçileri gibi yayımlanmamış ürünler hakkında potansiyel olarak hassas bilgilere erişimi olan kişiler için yararlı olabilir.

---

### `SecretPackageIDs`

Varsayılan değeri boş olan `ImmutableHashSet<uint>` türünde bir özellik. Bu özellik, eklentinin çözmeyeceği `packageIDs` (aynı zamanda `subIDs` olarak da bilinir) belirtir ve eğer zaten çözülmüşse, token'ı göndermeyecektir. Bu özellik, özellikle geliştiriciler, yayınevleri veya kapalı beta testçileri gibi yayımlanmamış ürünler hakkında potansiyel olarak hassas bilgilere erişimi olan kişiler için yararlı olabilir.

---

### `SkipAutoGrantPackages`

Varsayılan değeri `true` olan `bool` türünde bir özellik. Bu özellik, `SecretPackageIDs`'ye çok benzerdir ve etkinleştirildiğinde, `AutoGrant` olan `EPaymentMethod`'a sahip paketlerin aşağıda açıklanan çözüm rutininde atlanmasına neden olur. `AutoGrant` ödeme yöntemi, **[Steamworks](https://partner.steamgames.com)** tarafından, geliştirici hesaplarına paketleri otomatik olarak vermek için kullanılır. Bu, diğer `Secret` seçenekleri kadar açık olmasa da ve bu nedenle hiçbir şey garanti etmemektedir (çünkü başka `AutoGrant` paketleriniz olabilir ve bunları hala göndermek istemeyebilirsiniz), çoğu durumda gizli paketleri atlamak için yeterli olmalıdır. Bu seçenek varsayılan olarak etkinleştirilmiştir, çünkü gerçekten `AutoGrant` paketlerine erişimi olan kişilerin bu paketleri genel kamuoyuna sızdırmak istemesi neredeyse hiç gerçekleşmez, bu nedenle `false` değeri kullanımı oldukça durumsaldır.

---

## Daha Fazla Açıklama

Kök düzeyinde, her Steam hesabı bir dizi paket (lisanslar, abonelikler) içerir ve bunlar `packageID` (aynı zamanda `subID` olarak da bilinir) ile sınıflandırılır. Her paket, `appID` ile sınıflandırılan birkaç uygulama içerebilir. Her uygulama ise `depotID` ile sınıflandırılan birkaç depo içerebilir.

```text
├── sub/124923
│     ├── app/292030
│     │     ├── depot/292031
│     │     ├── depot/378648
│     │     └── ...
│     ├── app/378649
│     └── ...
└── ...
```

Eklentimiz, atlanan öğeleri dikkate alan iki rutine sahiptir - çözüm rutini ve gönder

Çözüm rutini, yukarıda görebileceğiniz ağacın çözülmesinden sorumludur. Paketleri/uygulamaları/depolarları önceden kara listeye alarak, ağacı kara listeye alınan dal/yaprak yerinde etkili bir şekilde keseceksiniz ve kalan kısımları belirtme gereksinimini ortadan kaldıracaksınız. Yukarıdaki örnekte, `124923` paketi atlanmışsa, `SecretPackageIDs` veya `SkipAutoGrantPackages` aracılığıyla ve bu tek bağlantılı olduğunuz tek paketse, `292030` uygulamaID'si çözülmeyecektir ve tanım gereği, eğer `292031` ve `378648` depolarına bağlantılı başka uygulamalar bulunmuyorsa, onlar da çözülmeyecektir. Ancak, eklenti ağacı zaten çözmüşse, bu yalnızca belirli öğenin güncellenmesini durdurur (örneğin, yeni uygulamalar eklenmesi), eklentinin zaten çözülmüş olan mevcut öğeleri "unutmasını" sağlamaz (örneğin, önceki durumda bulunan uygulamalar). Eğer yeni bazı atlama seçeneklerini etkinleştirdiyseniz ve ASF'nin zaten çözülmüş ağacı geçmediğinden emin olmak istiyorsanız, eklentinin önbelleğini tuttuğu `config/SteamTokenDumper.cache` dosyasını bir kezlik olarak kaldırmayı düşünebilirsiniz.

Gönderim rutini, zaten çözülmüş (yukarıdaki çözüm rutini tarafından) paket token'larını, uygulama token'larını ve depo anahtarlarını göndermekle sorumludur. Burada kara liste uygulamanız hemen etki gösterir, çünkü eklenti zaten çözülmüş bilgilere sahip olsa bile, kara listeye alınmışsa bunları SteamDB'ye göndermeyecektir, çözülüp çözülmediği önemli değildir. Ancak burada ağacı konuşmuyoruz, gönderim rutini, bilginin hangi paketlerden geldiğini bilmez, dolayısıyla yalnızca belirli kara listeye alınmış öğeler hakkında bilgi atlar, diğerleriyle olan ilişkisine bakılmaksızın.

Geliştiriciler ve yayınevleri için, `SkipAutoGrantPackages`'i etkinleştirmenin ve potansiyel olarak yalnızca `SecretPackageIDs` kullanmanın yeterli olması gerekir; çünkü bu, ağacı başlangıç dalında keser ve dahili paketlerin ve depoların daha sonra gönderilmemesini garanti eder, eğer başka bir paket bu uygulamaya bağlantılı değilse. Kesinlikle emin olmak isterseniz, `SecretAppIDs`'i de kullanabilirsiniz, bu da uygulamanın diğer lisanslara bağlanması durumunda bile çözülmesini engeller. `SecretDepotIDs` kullanımı genellikle gerekli değildir, yalnızca belirli bir depo atlamak gibi özel bir ihtiyacınız varsa veya üçüncü bir güvenlik katmanı eklemek istiyorsanız kullanılır.
# Steam Aile Paylaşımı

ASF supports Steam Family Sharing - the old as well as the new system. In order to understand how ASF works with that, you should firstly read how **[Steam Family Sharing works](https://store.steampowered.com/promotion/familysharing)**, which is available on Steam store. In addition to that, check **[the news](https://store.steampowered.com/news/app/593110/view/4149575031735702628)** for upcoming new Steam Family Sharing system, which ASF is also compatible with.

---

## ASF

Asf'deki Steam Aile Paylaşımı özelliği için destek şeffaftır, yani yeni Bot / işlem yapılandırma özellikleri tanıtılmaz - hemen ek bir yerleşik işlevsellik olarak çalışır.

ASF, kütüphanenin aile paylaşımı kullanıcıları tarafından kilitlendiğinin farkında olmak için uygun mantığı içerir, bu nedenle bir oyun başlatması nedeniyle onları oyun oturumundan "atmaz". ASF, kilidi tutan birincil hesapla tamamen aynı şekilde hareket edecektir, bu nedenle bu kilit steam istemciniz veya aile paylaşımı kullanıcılarınızdan biri tarafından tutuluyorsa, ASF kart düşürmeye çalışmaz, bunun yerine kilidin serbest bırakılmasını bekler. This is mostly related to the old system - new system allows your family sharing users to play games other than those that ASF is currently farming.

In addition to above, after logging in, ASF will access your family sharing systems (old and new), from which it'll extract users (Steam IDs) allowed to use your library. Those users are given `FamilySharing` permission to use **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, especially allowing them to use `pause~` command on bot account that is sharing games with them, which allows to pause automatic cards farming module in order to launch a game that can be shared - this also mostly applies to the old system, but might still be used with the new system in case ASF is currently farming game that your users want to play.

Yukarıda açıklanan her iki fonksiyonu birbirine bağlamak, arkadaşlarınızın kart toplama işleminiz için `pause~` komutunu başlatmasına, bir oyun başlatmasına, istediği kadar oynamasına izin verir, daha sonra oyunu bitirdikten sonra Kart toplama işlemi ASF tarafından otomatik olarak devam eder. Tabii ki, ASF şu anda aktif bir toplama işlemi yapmıyorsa, `pause~` göndermek gerekli değildir, çünkü arkadaşlarınız oyunu hemen başlatabilir ve yukarıda açıklanan mantık oturumdan atılmamanızı sağlar.

---

## Kısıtlamalar

Steam ağı, yanlış durum güncellemeleri yayınlayarak ASF'yi yanıltmayı sever, bu da ASF'nin sürece devam etmenin iyi olduğuna inanmasına ve sonuç olarak arkadaşınızı çok erken tekmelemesine neden olabilir. Bu, **[SSS girişinde](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#asf-is-kicking-my-steam-client-session-while-im-playing--this-account-is-logged-on-another-pc)** tarafımızdan daha önce açıklananla tamamen aynı konudur. Tamamen aynı çözümleri öneriyoruz, esas olarak arkadaşlarınızı `Operatör` iznine (veya üstüne) tanıtmak ve `pause` ve `resume` komutlarından yararlanmalarını söylemek.
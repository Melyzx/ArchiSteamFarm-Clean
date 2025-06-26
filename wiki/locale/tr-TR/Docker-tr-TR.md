# Docker

ASF, **[docker konteyneri](https://www.docker.com/what-container)** olarak mevcuttur. Docker paketlerimiz şu anda hem **[ghcr.io](https://github.com/JustArchiNET/ArchiSteamFarm/pkgs/container/archisteamfarm)** hem de **[Docker Hub](https://hub.docker.com/r/justarchi/archisteamfarm)** üzerinde bulunmaktadır.

ASF'yi Docker konteynerinde çalıştırmanın **gelişmiş bir kurulum** olduğunu ve çoğu kullanıcı için **gerekli olmadığını** ve genellikle konteynersiz kurulumlara göre **hiçbir avantaj sağlamadığını** belirtmek önemlidir. ASF'yi bir hizmet olarak çalıştırmak için Docker'ı (örneğin, işletim sisteminizle otomatik olarak başlatmak) çözüm olarak düşünüyorsanız, **[yönetim](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#systemd-service-for-linux)** bölümünü okuyup uygun bir `systemd` servisi kurmayı düşünmelisiniz; bu genellikle Docker konteynerinde ASF çalıştırmaktan **hemen hemen her zaman** daha iyi bir fikirdir.

ASF'yi Docker konteynerinde çalıştırmak genellikle **bir dizi yeni sorun ve mesele** ile karşılaşmanıza neden olur ve bunları kendiniz çözmek zorunda kalırsınız. Bu nedenle, Docker hakkında bilginiz yoksa ve iç işleyişini anlamada yardıma ihtiyacınız varsa, Docker kullanımını şiddetle öneririz. Bu bölüm, genellikle çok karmaşık kurulumların geçerli kullanım senaryoları içindir; örneğin, ileri düzey ağ kurulumları veya ASF'nin `systemd` servisiyle birlikte sağladığı standart sanal kutulama ötesindeki güvenlik için. Bu kadar az sayıda insan için, Docker uyumluluğu ile ilgili ASF kavramlarını burada daha iyi açıkladık; eğer ASF ile birlikte Docker kullanmaya karar verirseniz, Docker hakkında yeterli bilginiz olduğunu varsayıyoruz.

---

## Etiketler

ASF, 4 ana tür **[etiket](https://hub.docker.com/r/justarchi/archisteamfarm/tags)** ile mevcuttur:


### `main`

Bu etiket her zaman `main` dalındaki en son commit ile derlenen ASF'yi işaret eder ve bu, en son ürünü doğrudan **[CI](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/publish.yml?query=branch%3Amain)** pipeline'ımızdan almanıza eşdeğerdir. Genellikle bu etiketi kullanmaktan kaçınmalısınız çünkü bu etiket, geliştiriciler ve ileri düzey kullanıcılar için yazılımın en hatalı seviyesidir ve genellikle kararlı veya test edilmiş olduğuna dair bir garanti yoktur. `main` GitHub dalındaki her commit ile güncellenir, bu yüzden sık sık güncellemeler (ve bazı şeylerin bozulması) bekleyebilirsiniz. Bu etiket, ASF projesinin mevcut durumunu işaret etmek için buradadır, bu da daima kararlı veya test edilmiş olduğu anlamına gelmez. Bu etiket, herhangi bir üretim ortamında kullanılmamalıdır.


### `released`

Bu etiket yukarıdakilere çok benzer şekilde, en son **[yayınlanmış](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** ASF sürümüne işaret eder, ön sürümler de dahil. `main` etiketine kıyasla, bu imaj, yeni bir GitHub etiketi itildiğinde güncellenir. Stabil ve taze olan arasında denge kurmayı seven ileri düzey/kapsamlı kullanıcılar için tasarlanmıştır. `latest` etiketini kullanmak istemiyorsanız bu etiketi öneririz. Pratikte, bu etiket, çekme zamanı itibariyle en son `A.B.C.D` sürümüne işaret eden sürülen etiketle aynı şekilde çalışır. Bu etiketin kullanılması, **[ön sürümler](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)** ile eşdeğerdir.


### `latest`

Diğer etiketlerin aksine, bu etiket ASF'nin otomatik güncellemeler özelliğini içerir ve en son **[kararlı](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** ASF sürümüne işaret eder. Bu etiketin amacı, kendini güncelleyebilen, işletim sistemine özgü ASF sürümünü çalıştırabilen mantıklı bir varsayılan Docker konteyneri sağlamaktır. Bu nedenle, imajın mümkün olduğunca sık güncellenmesi gerekmez çünkü dahili ASF sürümü her zaman kendini güncelleyebilir. Elbette, `UpdatePeriod` güvenle kapatılabilir (0 olarak ayarlanabilir), ancak bu durumda muhtemelen dondurulmuş `A.B.C.D` sürümünü kullanmanız gerekir. Benzer şekilde, otomatik güncellemeyi `released` etiketi ile yapmak için varsayılan `UpdateChannel`'ı değiştirebilirsiniz.

`latest` imajı otomatik güncellemeler özelliği ile geldiğinden, yalnızca `linux` ASF sürümünü içeren bare OS ile gelir; diğer tüm etiketler ise OS ile birlikte .NET çalışma zamanı ve `generic` ASF sürümü içerir. Bunun nedeni, daha yeni (güncellenmiş) ASF sürümünün, imajın oluşturulduğu çalışma zamanından daha yeni bir çalışma zamanı gerektirebilmesidir; bu da imajın baştan yeniden oluşturulmasını gerektirir, bu da planlanan kullanım senaryosunu geçersiz kılar.

### `A.B.C.D`

Yukarıdaki etiketlerle karşılaştırıldığında, bu etiket tamamen dondurulmuştur, yani yayımlandıktan sonra imaj güncellenmeyecektir. Bu, GitHub'daki ilk sürümden sonra hiçbir zaman dokunulmayan sürümlerimiz gibi çalışır ve size kararlı ve dondurulmuş bir ortam garanti eder. Genellikle, belirli bir ASF sürümünü kullanmak ve herhangi bir otomatik güncelleme istememek istiyorsanız bu etiketi kullanmalısınız (örneğin `latest` etiketi ile sunulan güncellemeler gibi).

---

## Hangi etiket benim için en iyisi?

Bu, ne aradığınıza bağlıdır. Çoğu kullanıcı için, `latest` etiketi en iyi seçim olmalıdır çünkü tam olarak masaüstü ASF'nin yaptığı şeyi sağlar, sadece özel bir Docker konteyneri olarak hizmet eder. Kapsamlı kontrol ve belirli bir sürüme bağlı ASF sürümünü tercih edenler için `released` etiketi önerilir. Belirli bir dondurulmuş ASF sürümünü kullanmak ve bunun değişmesini istemiyorsanız, `A.B.C.D` sürümleri size her zaman geri dönebilmeniz için sabit ASF kilometre taşları olarak sunulmuştur.

Genel olarak `main` yapılarını denemeyi teşvik etmiyoruz, çünkü bunlar ASF projesinin mevcut durumunu işaret etmek için buradadır. Bu durumun düzgün çalışacağını garanti eden hiçbir şey yoktur, ancak ASF'nin geliştirilmesiyle ilgileniyorsanız denemenizden memnuniyet duyarız.

---

## Mimari

ASF Docker imajı şu anda `linux` platformunda, `x64`, `arm` ve `arm64` olmak üzere 3 mimari hedef alarak inşa edilmektedir. Daha fazlasını **[uyumluluk](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility)** bölümünde okuyabilirsiniz.

Etiketlerimiz çoklu platform manifesti kullanır, bu da Docker'ın makinenizde doğru imajı otomatik olarak seçeceği anlamına gelir. Eğer şu anda çalıştığınız platformla eşleşmeyen belirli bir platform imajını çekmek isterseniz, bunu uygun docker komutlarında `--platform` anahtarı ile yapabilirsiniz, örneğin `docker run`. Daha fazla bilgi için Docker dokümantasyonuna **[image manifest](https://docs.docker.com/registry/spec/manifest-v2-2)** göz atabilirsiniz.

---

## Kullanım

Tam referans için **[resmi docker dokümantasyonunu](https://docs.docker.com/engine/reference/commandline/docker)** kullanmalısınız, bu kılavuzda sadece temel kullanımı ele alacağız, daha derinlemesine araştırmaya açıksınız.

### Merhaba ASF!

Öncelikle docker'ımızın düzgün çalışıp çalışmadığını doğrulamalıyız, bu bizim ASF'nin "merhaba dünya"sı olacaktır:

```shell
docker run -it --name asf --pull always --rm justarchi/archisteamfarm
```

`docker run`, sizin için yeni bir ASF Docker konteyneri oluşturur ve bunu ön planda çalıştırır (`-it`). `--pull always`, güncel imajın önce çekilmesini sağlar ve `--rm`, konteyner durduğunda temizlenmesini garanti eder, çünkü şu anda her şeyin düzgün çalışıp çalışmadığını test ediyoruz.

Her şey başarılı bir şekilde tamamlandığında, tüm katmanları çekip konteyneri başlattıktan sonra, ASF'nin düzgün şekilde başlatıldığını ve tanımlanmış Hit `CTRL+C` to terminate the ASF process and therefore also the container.

If you take a closer look at the command then you'll notice that we didn't declare any tag, which automatically defaulted to `latest` one. If you want to use other tag than `latest`, for example `released`, then you should declare it explicitly:

```shell
docker run -it --name asf --pull always --rm justarchi/archisteamfarm:released
```

---

## Using a volume

ASF'yi bir docker konteynerinde kullanıyorsanız, programı kendisi yapılandırmanız gerekmektedir. Bunun için çeşitli farklı yöntemler kullanabilirsiniz, ancak önerilen yöntem, ASF `config` dizinini yerel makinede oluşturmak ve ardından ASF docker konteynerinde paylaşılan bir birim olarak bağlamaktır.

Örneğin, ASF yapılandırma klasörünüzün `/home/archi/ASF/config` dizininde olduğunu varsayalım. Bu dizin, çalıştırmak istediğimiz temel `ASF.json` dosyasını ve botları içerir. Şimdi yapmamız gereken tek şey, bu dizini ASF docker konteynerinde paylaşılan bir birim olarak bağlamaktır. ASF, yapılandırma dizinini (`/app/config`) beklemektedir.

```shell
docker run -it -v /home/archi/ASF/config:/app/config --name asf --pull always justarchi/archisteamfarm
```

Ve bu kadar, şimdi ASF docker konteyneriniz, yerel makinenizle paylaşılan bir dizini okuma-yazma modunda kullanacak, bu da ASF'yi yapılandırmak için ihtiyacınız olan her şeydir. Benzer şekilde, `/app/logs` veya `/app/plugins` gibi ASF ile paylaşmak istediğiniz diğer birimleri de bağlayabilirsiniz.

Tabii ki, istediğimizi elde etmek için sadece belirli bir yol, başka bir yol oluşturmanızı engelleyen hiçbir şey yoktur. Örneğin, kendi `Dockerfile`'ınızı oluşturarak, yapılandırma dosyalarınızı ASF docker konteyneri içindeki `/app/config` dizinine kopyalayan bir yol oluşturabilirsiniz. Bu kılavuzda sadece temel kullanımı ele alıyoruz.

### Volume permissions

ASF konteyneri varsayılan olarak varsayılan `root` kullanıcısıyla başlatılır, bu da dahili izin işlemlerini halletmesine ve ardından ana işlemin geri kalan kısmı için `asf` (UID `1000`) kullanıcısına geçmesine izin verir. Bu, çoğu kullanıcı için tatmin edici olmalıdır, ancak paylaşılan bir birimdeki yeni oluşturulan dosyaların normalde `asf` kullanıcısı tarafından sahiplenileceği anlamına gelir, bu da paylaşılan birim için başka bir kullanıcı istenmeyen bir durum olabilir.

ASF'nin hangi kullanıcı altında çalıştığı iki şekilde değiştirebilirsiniz. İlk önerilen yöntem, hedef UID ile `ASF_USER` ortam değişkenini bildirmektir. İkinci, alternatif yöntem ise, doğrudan docker tarafından desteklenen `--user` **[bayrağı](https://docs.docker.com/engine/reference/run/#user)** geçmektir.

Örneğin, `uid`'nizi `id -u` komutuyla kontrol edebilir ve yukarıda belirtildiği gibi bildirebilirsiniz. Örneğin, hedef kullanıcınızın `uid`'si 1001 ise:

```shell
docker run -it -e ASF_USER=1001 -v /home/archi/ASF/config:/app/config --name asf --pull always justarchi/archisteamfarm

# Alternatif olarak, aşağıdaki sınırlamaları anlıyorsanız
docker run -it -u 1001 -v /home/archi/ASF/config:/app/config --name asf --pull always justarchi/archisteamfarm
```

`ASF_USER` ve `--user` bayrağı arasındaki fark ince, ancak önemlidir. `ASF_USER`, ASF tarafından desteklenen özel bir mekanizmadır, bu senaryoda docker konteyneri hala `root` olarak başlar ve ardından ASF başlatma komut dosyası ana ikiliyi `ASF_USER` altında başlatır. `--user` bayrağını kullanırken, verilen kullanıcı olarak ASF başlatma komut dosyası dahil olmak üzere tüm süreci başlatıyorsunuz. İlk seçenek, ASF başlatma komut dosyasının izinleri ve diğer şeyleri otomatik olarak sizin için ele almasına izin verir, örneğin `/app` ve `/asf` dizinlerinizin gerçekten `ASF_USER` tarafından sahiplenildiğini sağlar. İkinci senaryoda, `root` olarak çalışmadığımız için bunu yapamayız ve bunun hepsini önceden kendiniz halletmeniz beklenir.

`--user` bayrağını kullanmaya karar verdiyseniz, tüm ASF dosyalarının varsayılan `asf` kullanıcısından yeni özel kullanıcınıza sahip olacak şekilde sahipliğini değiştirmeniz gerekmektedir. Bunun için aşağıdaki komutu çalıştırabilirsiniz:

```shell
# ASF_USER kullanmıyorsanız yalnızca çalıştırın
docker exec -u root asf chown -hR 1001 /app /asf
```

Bu, `docker run` ile konteynerinizi oluşturduktan sonra yalnızca bir kez yapılmalı ve `--user` docker bayrağı aracılığıyla özel kullanıcıyı kullanmayı tercih ettiyseniz yapılmalıdır. Ayrıca, yukarıdaki komutta `1001` argümanını gerçekten ASF'yi çalıştırmak istediğiniz `UID`'ye değiştirmeyi unutmayın.

### SELinux ile birim

If you're using SELinux in enforced state on your OS, which is the default for example on RHEL-based distros, then you should mount the volume appending `:Z` option, which will set correct SELinux context for it.

```
docker run -it -v /home/archi/ASF/config:/app/config:Z --name asf --pull always justarchi/archisteamfarm
```

Bu, ASF'nin docker konteyneri içindeyken birime hedeflenen dosyaları oluşturmasına izin verecektir.

---

## Birden fazla örneğin senkronizasyonu

ASF, **[yönetim](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#multiple-instances)** bölümünde belirtildiği gibi birden fazla örneğin senkronizasyonunu desteklemektedir. ASF'yi bir docker konteynerinde çalıştırırken, ASF'yi birden fazla konteyner ile çalıştırıyorsanız ve bunların birbirleriyle senkronize olmasını istiyorsanız, bu sürece "katılmanız" isteğe bağlıdır.

Varsayılan olarak, her bir docker konteynerinde çalışan ASF bağımsızdır, yani senkronizasyon gerçekleşmez. Bunları birbirleriyle senkronize etmek için, senkronize etmek istediğiniz her ASF konteynerinde `/tmp/ASF` yolunu, docker ana bilgisayarınızdaki bir paylaşılan yola, okuma-yazma modunda bağlamalısınız. Bu, yukarıda açıklanan bir birimi bağlama ile aynı şekilde gerçekleştirilir, sadece farklı yollarla:

```shell
mkdir -p /tmp/ASF-g1
docker run -v /tmp/ASF-g1:/tmp/ASF -v /home/archi/ASF/config:/app/config --name asf1 --pull always justarchi/archisteamfarm
docker run -v /tmp/ASF-g1:/tmp/ASF -v /home/john/ASF/config:/app/config --name asf2 --pull always justarchi/archisteamfarm
# Ve böyle devam eder, tüm ASF konteynerleri şimdi birbirleriyle senkronize edilir
```

ASF'nin `/tmp/ASF` dizinini de geçici bir `/tmp` dizinine bağlamanızı öneririz, ancak tabii ki kullanımınıza uygun başka bir dizin seçebilirsiniz. Aynı senkronizasyon sürecine katılan diğer konteynerlarla paylaşılan her ASF konteynerinin `/tmp/ASF` dizini paylaşılmalıdır.

As you've probably guessed from example above, it's also possible to create two or more "synchronization groups", by binding different docker host paths into ASF's `/tmp/ASF`.

Yukarıdaki örnekte tahmin ettiğiniz gibi, ASF'nin `/tmp/ASF`'yi bağlamak, tamamen isteğe bağlıdır ve aslında önerilmez, yalnızca iki veya daha fazla ASF konteynerini senkronize etmek istiyorsanız. Tek konteyner kullanımı için `/tmp/ASF`'yi bağlamayı önermiyoruz, çünkü yalnızca bir ASF konteyneri çalıştırmayı bekliyorsanız hiçbir fayda sağlamaz ve aksine kaçınılabilir sorunlara neden olabilir.

---

## Komut satırı değişkenleri

ASF, docker konteyneri içinden **[komut satırı argümanları](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)** geçirmenize olanak tanır. Desteklenen anahtarlar için belirli bir ortam değişkeni kullanmalı ve geri kalanı için `ASF_ARGS` kullanmalısınız. Bunun için `docker run` komutuna eklenen `-e` anahtarını kullanabilirsiniz, örneğin:

```shell
docker run -it -e "ASF_CRYPTKEY=MyPassword" -e "ASF_ARGS=--no-config-migrate" --name asf --pull always justarchi/archisteamfarm
```

Bu, `--cryptkey` argümanını docker konteyneri içinde çalışan ASF işlemine doğru bir şekilde iletecek ve diğer argümanları da iletecektir. Tabii ki, gelişmiş bir kullanıcıysanız, `ENTRYPOINT`'i değiştirebilir veya `CMD` ekleyebilir ve özel argümanları kendiniz iletebilirsiniz.

Özel şifreleme anahtarı veya diğer gelişmiş seçenekleri sağlamak istemediğiniz sürece, genellikle özel bir ortam değişkeni eklemenize gerek yoktur, çünkü docker konteynerlerimiz zaten `--no-restart` `--system-required` gibi beklenen varsayılan seçeneklerle çalışacak şekilde yapılandırılmıştır, bu nedenle bu bayraklar `ASF_ARGS` içinde açıkça belirtilmesi gerekmez.

---

## IPC

Varsayılan `IPC` **[genel yapılandırma özelliği](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)** için değeri değiştirmediyseniz, zaten etkin durumdadır. Ancak, Docker konteynerinde IPC'nin çalışması için iki ek adım yapmanız gerekmektedir. İlk olarak, dışarıdan bağlanmanıza izin vermek için `IPCPassword` kullanmalı veya özel `IPC.config` dosyasında varsayılan `KnownNetworks`'ü değiştirmelisiniz. Gerçekten ne yaptığınızı biliyorsanız, sadece `IPCPassword` kullanın. İkinci olarak, docker dış trafiği loopback arayüzüne yönlendiremediği için varsayılan dinleme adresini `localhost` olarak değiştirmeniz gerekmektedir. Tüm arabirimlere dinleyecek bir ayar örneği `http://*:1242` olabilir. Tabii ki, yerel LAN veya VPN ağı gibi daha kısıtlayıcı bağlantılar da kullanabilirsiniz, ancak bu, dışarıdan erişilebilen bir rota olmalıdır - `localhost` bunu yapmaz, çünkü rota tamamen konuk makine içindedir.

Yukarıdakileri yapmak için aşağıdaki gibi **[özel IPC yapılandırması](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#custom-configuration)** kullanmalısınız:

```json
{
    "Kestrel": {
        "Endpoints": {
            "HTTP": {
                "Url": "http://*:1242"
            }
        }
    }
}
```

Loopback dışı arabirimde IPC'yi ayarladıktan sonra, ASF'nin `1242/tcp` bağlantı noktasını `docker run` komutuna `-P` veya `-p` anahtarını kullanarak eşlemesi gerekmektedir.

Örneğin, aşağıdaki komut ASF IPC arabirimini yalnızca ana makineye açacaktır:

```shell
docker run -it -p 127.0.0.1:1242:1242 -p [::1]:1242:1242 --name asf --pull always justarchi/archisteamfarm
```

Her şeyi düzgün bir şekilde ayarladıysanız, yukarıdaki `docker run` komutu, artık doğru bir şekilde konuk makinenize yönlendirilen standart `localhost:1242` rotasından ana makineden **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** arabirimini çalıştıracaktır. Ayrıca, bu rotayı daha ileriye açmıyoruz, bu nedenle bağlantı yalnızca docker ana bilgisayarı içinde yapılabilir ve bu nedenle güvenli kalır. Tabii ki, ne yaptığınızı biliyorsanız ve uygun güvenlik önlemlerini sağladıysanız rotayı daha ileriye açabilirsiniz.

---

### Tam örnek

Yukarıdaki tüm bilgileri birleştirerek, tam bir kurulum örneği aşağıdaki gibi görünecektir:

```shell
docker run -p 127.0.0.1:1242:1242 -p [::1]:1242:1242 -v /home/archi/ASF/config:/app/config -v /home/archi/ASF/plugins:/app/plugins --name asf --pull always justarchi/archisteamfarm
```

Bu, `/home/archi/ASF/config` dizinindeki tüm ASF yapılandırma dosyalarını içeren tek bir ASF konteyneri kullanacağınızı varsayar. Makinenize uygun yapılandırma yolunu değiştirmelisiniz. Bu kurulum, isteğe bağlı IPC kullanımı için de hazırdır, eğer `IPC.config`'i yapılandırma dizininize aşağıdaki gibi bir içerikle eklemeye karar verdiyseniz: ASF için özel eklentiler de sağlayabilirsiniz, bunları `/home/archi/ASF/plugins` dizinine koyabilirsiniz.

```json
{
    "Kestrel": {
        "Endpoints": {
            "HTTP": {
                "Url": "http://*:1242"
            }
        }
    }
}
```

---

## İpuçları

ASF docker konteyneriniz hazır olduğunda her seferinde `docker run` komutunu kullanmanız gerekmez. ASF docker konteynerini `docker stop asf` ve `docker start asf` komutlarıyla kolayca durdurabilir ve başlatabilirsiniz. Unutmayın, `latest` etiketini kullanmıyorsanız, güncel ASF kullanmak hala `docker stop`, `docker rm` ve `docker run` yapmanızı gerektirir. Bunun nedeni, ASF sürümünü kullanmak için her seferinde taze ASF docker görüntüsünden konteynerinizi yeniden oluşturmanız gerekmektedir. `latest` etiketinde, ASF kendini otomatik olarak güncelleme yeteneği dahil edilmiştir, bu nedenle güncel ASF kullanmak için görüntüyü yeniden oluşturmanız gerekmez (ancak büyük ASF sürüm güncellemeleri arasında geçiş yaparken taze .NET çalışma zamanı bağımlılıklarını ve temel işletim sistemini kullanmak için zaman zaman bunu yapmanız iyi bir fikir olabilir).

Yukarıda belirtildiği gibi, `latest` dışındaki etikete sahip ASF otomatik olarak güncellenmez, bu da **siz**'in güncel `justarchi/archisteamfarm` deposunu kullanmakla sorumlu olduğunuz anlamına gelir. Bu, uygulamanın çalışırken kendi koduna dokunmaması gerektiği için birçok avantaja sahiptir, ancak docker konteynerinizde ASF sürümü hakkında endişelenmek zorunda kalmamanın getirdiği kolaylığı da anlıyoruz. İyi uygulamalara ve doğru docker kullanımına önem veriyorsanız, `latest` yerine `released` etiketini öneririz, ancak eğer bununla uğraşmak istemiyorsanız ve ASF'nin hem çalışmasını hem de kendini otomatik olarak güncellemesini istiyorsanız, o zaman `latest` kullanabilirsiniz.

ASF'yi genellikle `Headless: true` global ayarıyla docker konteynerinde çalıştırmalısınız. Bu, ASF'ye eksik ayrıntıları sağlamak için burada olmadığınızı açıkça belirtecek ve bunları sormaması gerektiğini söyleyecektir. Tabii ki, başlangıç ​​kurulumu için bu seçeneği `false` olarak bırakmayı düşünmelisiniz, böylece kolayca ayarları yapabilirsiniz, ancak uzun vadede genellikle ASF konsoluna bağlı değilsinizdir, bu nedenle ASF'ye bunu bildirmek ve gerektiğinde `input` **[komutunu](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** kullanmak mantıklı olacaktır. Böylece ASF, gerçekleşmeyecek kullanıcı girişi için sonsuz süre beklemek zorunda kalmaz (ve bunu yaparken kaynakları boşa harcamaz). Ayrıca, ASF'nin, örneğin sinyalleri yönlendirmek için önemli olan, `docker stop asf` isteğinde ASF'nin zarif bir şekilde kapanmasını mümkün kılan, konteyner içinde etkileşimli olmayan bir modda çalışmasına izin verir.
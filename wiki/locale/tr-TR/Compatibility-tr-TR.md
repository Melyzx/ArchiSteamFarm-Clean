# Uyumluluk

Bu çalışma zamanı ASF kodunu düzgün bir şekilde çalıştırdığı sürece, temel işletim sisteminin Windows, Linux, macOS, BSD, Sony Playstation 4, Nintendo Wii veya tost makineniz olup olmadığı önemli değildir - **[.NET](https://dotnet.microsoft.com/download/dotnet)** olduğu sürece, **[ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** (generic versiyonda) da vardır. Bu aynı zamanda ASF'nin **belirli bir işletim sistemi gereksinimi olmadığı** anlamına gelir, çünkü çalışması için işletim sisteminin kendisine değil, bu işletim sisteminde çalışan **çalışma zamanı** gerektirir.

Bu yaklaşımın büyük avantajları vardır; çünkü CIL platformdan bağımsızdır, bu nedenle ASF birçok mevcut işletim sisteminde, özellikle Windows, Linux ve macOS'ta yerel olarak çalışabilir. Bu, emülasyon gerektirmediği gibi, CPU SSE talimatları gibi platform ve donanım ile ilgili optimizasyonların da desteklendiği anlamına gelir. Bu sayede ASF, üstün performans ve optimizasyon sağlarken, mükemmel uyumluluk ve güvenilirlik sunar.

This also means that ASF has **no specific OS requirement**, because it requires working **runtime** on that OS and not OS itself. ASF, .NET platformunda çalışan bir C# uygulamasıdır. Bu, ASF'nin doğrudan CPU'nuzda çalışan **[makine koduna](https://en.wikipedia.org/wiki/Machine_code)** değil, onu çalıştırmak için CIL uyumlu bir çalışma zamanı gerektiren **[CIL (Common Intermediate Language)](https://en.wikipedia.org/wiki/Common_Intermediate_Language)** koduna derlendiği anlamına gelir.

Ancak, ASF'yi nerede çalıştırırsanız çalıştırın, hedef platformunuzda **[.NET önkoşullarının](https://github.com/dotnet/core/blob/main/Documentation/prereqs.md)** yüklü olduğundan emin olmalısınız. Bunlar, doğru çalışma zamanı işlevselliği için gerekli olan düşük seviyeli kütüphaneler olup, ASF'nin çalışması için kesinlikle gereklidir. Muhtemelen bu kütüphanelerin bazıları (veya tümü) zaten yüklü olabilir.

---

## ASF Paketleme

ASF, genel paket ve işletim sistemi spesifik olmak üzere iki ana çeşitte gelir. İşlevsellik açısından her iki paket de aynıdır ve her ikisi de kendilerini otomatik olarak güncelleyebilirler. Aralarındaki tek fark, ASF **genel** paketinin aynı zamanda **işletim sistemi spesifik** çalışma zamanını içerip içermediğidir.

---

### Generic

Genel paket, herhangi bir makineye özgü kod içermeyen platform bağımsız bir yapıdır. Bu kurulum, işletim sisteminizde **uygun sürümde** .NET çalışma zamanının yüklü olmasını gerektirir. Bağımlılıkları güncel tutmanın ne kadar zahmetli olduğunu hepimiz biliyoruz, bu nedenle bu paket, .NET'i zaten kullanan ve ASF için yalnızca yüklü olan çalışma zamanını kullanarak bağımlılıklarını çoğaltmak istemeyen insanlar için buradadır. Genel paket ayrıca, **çalışan bir .NET çalışma zamanının** elde edilebileceği her yerde ASF'yi çalıştırmanıza olanak tanır, işletim sistemi spesifik ASF yapısı olup olmadığına bakılmaksızın.

Eğer ASF'yi çalıştırmak isteyen sıradan veya hatta ileri düzey bir kullanıcıysanız ve .NET teknik detaylarına dalmak istemiyorsanız genel lezzeti kullanmanız önerilmez. Başka bir deyişle - eğer ne olduğunu biliyorsanız, kullanabilirsiniz, aksi takdirde aşağıda açıklanan işletim sistemi spesifik paketi kullanmak çok daha iyidir.

---

### İşletim Sistemi Spesifik

İşletim sistemi spesifik paket, genel paketteki yönetilen kodun yanı sıra, ilgili platform için yerel kodu da içerir. Başka bir deyişle, işletim sistemi spesifik paket, uygun .NET çalışma zamanını da içerir, bu da tüm kurulum karmaşasını tamamen atlamanızı ve ASF'yi doğrudan başlatmanızı sağlar. İşletim sistemi spesifik paket, adından da anlaşılacağı gibi, işletim sistemi spesifik olup her işletim sistemi kendi sürümünü gerektirir - örneğin Windows, PE32+ `ArchiSteamFarm.exe` ikili dosyasını gerektirirken, Linux, Unix ELF `ArchiSteamFarm` ikili dosyası ile çalışır. Bildiğiniz gibi, bu iki tür birbirleriyle uyumlu değildir.

ASF şu anda aşağıdaki işletim sistemi spesifik çeşitlerde mevcuttur:

- `linux-arm`, glibc 2.35/musl 1.2.2 ve daha yenisine sahip 32-bit ARM tabanlı (ARMv7+) GNU/Linux işletim sistemlerinde çalışır. Bu varyant, Raspberry Pi 2 (ve daha yenileri) gibi platformları kapsar, daha eski ARM mimarileri (Raspberry Pi 0 & 1'de bulunan ARMv6 gibi) ile çalışmaz, gerekli GNU/Linux ortamını uygulamayan işletim sistemleri ile de çalışmaz (Android gibi).
- `linux-arm64`, glibc 2.23/musl 1.2.2 ve daha yenisine sahip 64-bit ARM tabanlı (ARMv8+) GNU/Linux işletim sistemlerinde çalışır. Bu varyant, Raspberry Pi 3 (ve daha yenileri) gibi platformları kapsar, gerekli 64-bit kütüphaneleri bulunmayan 32-bit işletim sistemleri (32-bit Raspberry Pi OS gibi) ile çalışmaz, gerekli GNU/Linux ortamını uygulamayan işletim sistemleri ile de çalışmaz (Android gibi).
- `linux-x64`, glibc 2.23/musl 1.2.2 ve daha yenisine sahip 64-bit GNU/Linux işletim sistemlerinde çalışır.
- `osx-arm64`, 13 ve daha yeni sürümlerdeki 64-bit ARM tabanlı (Apple silicon) macOS işletim sistemlerinde çalışır.
- `osx-x64`, 13 ve daha yeni sürümlerdeki 64-bit macOS işletim sistemlerinde çalışır.
- `win-arm64`, 10, 11 ve daha yeni sürümlerdeki 64-bit ARM tabanlı (ARMv8+) Windows işletim sistemlerinde çalışır.
- `win-x64`, 10, 11, Server 2012+ ve daha yeni sürümlerdeki 64-bit Windows işletim sistemlerinde çalışır.

Tabii ki, işletim sistemi mimari kombinasyonunuz için mevcut bir işletim sistemi spesifik paketiniz yoksa, her zaman uygun .NET çalışma zamanını kendiniz yükleyebilir ve genel ASF lezzetini çalıştırabilirsiniz, bu da genel ASF yapısının var olmasının ana nedenidir. Genel ASF yapısı platformdan bağımsızdır ve çalışan bir .NET çalışma zamanına sahip herhangi bir platformda çalışır. Bu önemli bir nottur - ASF belirli bir işletim sistemi veya mimari değil, .NET çalışma zamanı gerektirir. Örneğin, 32-bit Windows çalıştırıyorsanız, özel bir `win-x86` ASF sürümü olmamasına rağmen, `win-x86` sürümünde .NET SDK'sını yükleyebilir ve genel ASF'yi sorunsuz çalıştırabilirsiniz. Her mevcut ve birilerinin kullandığı işletim sistemi mimari kombinasyonunu hedeflememiz mümkün olmadığından bir yerde bir çizgi çekmemiz gerekiyor. x86, en azından 2004'ten beri modası geçmiş bir mimari olduğundan, bu çizginin iyi bir örneğidir.

For a complete list of all supported platforms and OSes by .NET 9.0, visit **[release notes](https://github.com/dotnet/core/blob/main/release-notes/9.0/supported-os.md)**.

---

## Çalışma Zamanı Gereksinimleri

İşletim sistemi spesifik bir paket kullanıyorsanız, çalışma zamanı gereksinimlerini düşünmenize gerek yoktur, çünkü ASF her zaman doğru ve güncel çalışma zamanını içerir ve **[.NET önkoşulları](https://github.com/dotnet/core/blob/main/Documentation/prereqs.md)** yüklü ve güncel olduğu sürece düzgün çalışacaktır. Başka bir deyişle, **.NET çalışma zamanını veya SDK'yı yüklemeniz gerekmez**, çünkü işletim sistemi spesifik yapılar yalnızca yerel işletim sistemi bağımlılıklarını (önkoşullar) gerektirir ve başka bir şey gerektirmez.

Ancak, **genel** ASF paketini çalıştırmaya çalışıyorsanız, .NET çalışma zamanınızın ASF'nin gerektirdiği platformu desteklediğinden emin olmalısınız.

ASF as a program is targeting **.NET 9.0** (`net9.0`) right now, but it may target newer platform in the future. `net9.0` is supported since 9.0.100 SDK (9.0.0 runtime), although ASF might prefer **latest runtime at the moment of compilation**, so you should ensure that you have **[latest SDK](https://dotnet.microsoft.com/download)** (or at least runtime) available for your machine. Genel ASF varyantı, çalışma zamanınız

Şüphe duyduğunuzda, GitHub'daki ASF sürümlerini derlemek ve dağıtmak için **[sürekli entegrasyonun](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/publish.yml?query=branch%3Amain)** ne kullandığını kontrol edin. Her yapıda .NET doğrulama adımının bir parçası olarak `dotnet --info` çıktısını bulabilirsiniz.
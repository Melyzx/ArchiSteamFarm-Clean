# Komut satırı argümanları

ASF, programın çalışma süresini etkileyebilecek çeşitli komut satırı argümanlarını destekler. Bu argümanlar, programın nasıl çalışacağını belirlemek için ileri düzey kullanıcılar tarafından kullanılabilir. `ASF.json` yapılandırma dosyasının varsayılan kullanım şekliyle karşılaştırıldığında, komut satırı argümanları çekirdek başlatma (örneğin `--path`), platforma özgü ayarlar (örneğin `--system-required`) veya hassas veriler (örneğin `--cryptkey`) için kullanılır.

---

## Kullanım

Kullanım işletim sisteminize ve ASF zevkinize göre değişir.

Genel:

```shell
dotnet ArchiSteamFarm.dll --argument --otherOne
```

Windows:

```powershell
.\ArchiSteamFarm.exe --argument --otherOne
```

Linux/macOS:

```shell
./ArchiSteamFarm --argument --otherOne
```

Komut satırı argümanları, `ArchiSteamFarm.cmd` veya `ArchiSteamFarm.sh` gibi genel yardımcı betiklerde de desteklenir. Buna ek olarak, **[yönetim](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#environment-variables)** ve **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker#command-line-arguments)** bölümlerinde belirtildiği gibi `ASF_ARGS` çevre değişkeni de kullanılabilir.

Argümanınız boşluk içeriyorsa, onu tırnak içine almayı unutmayın. Şu ikisi hatalıdır:

```shell
./ArchiSteamFarm --path /home/archi/My Downloads/ASF # Hatalı!
./ArchiSteamFarm --path=/home/archi/My Downloads/ASF # Hatalı!
```

Ancak şu ikisi tamamen hatasızdır:

```shell
./ArchiSteamFarm --path "/home/archi/My Downloads/ASF" # Tamam
./ArchiSteamFarm "--path=/home/archi/My Downloads/ASF" # Tamam
```

## Argümanlar

`--cryptkey <key>` veya `--cryptkey=<key>` - ASF'yi `<key>` değeriyle özel bir kriptografik anahtarla başlatır. Bu seçenek, **[güvenliği](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** etkiler ve ASF'nin varsayılan yerine, sağladığınız `<key>` anahtarını kullanmasına neden olur. Bu özellik, varsayılan şifreleme anahtarını (şifreleme amacıyla) ve tuzu (hashleme amacıyla) etkilediğinden, bu anahtarla şifrelenen/hashlenen her şeyin her ASF çalıştırmasında geçmesi gerektiğini unutmayın.

`<key>` uzunluğu veya karakterleri için bir gereklilik yoktur, ancak güvenlik nedenleriyle rastgele 32 karakterden oluşan uzun bir parola seçmenizi öneririz, örneğin `tr -dc A-Za-z0-9 < /dev/urandom | head -c 32; echo` komutunu Linux'ta kullanarak.

Bu detayı sağlamanın iki başka yolunun daha olduğunu belirtmekte fayda var: `--cryptkey-file` ve `--input-cryptkey`.

Bu özelliğin doğası gereği, `ASF_CRYPTKEY` çevre değişkenini belirleyerek de cryptkey ayarlamak mümkündür, bu da sürecin argümanlarında hassas detaylardan kaçınmak isteyenler için daha uygun olabilir.

---

`--cryptkey-file <path>` veya `--cryptkey-file=<path>` - ASF'yi `<path>` dosyasından okunan özel bir kriptografik anahtarla başlatır. Bu, yukarıda açıklanan `--cryptkey <key>` ile aynı amaca hizmet eder, sadece mekanizma farklıdır; bu özellik, `<key>` anahtarını sağlanan `<path>` yolundan okur. Bunu `--path` ile birlikte kullanıyorsanız, göreceli yolun argümanların sırasına bağlı olarak farklı olacağını unutmayın, yani `--cryptkey-file`'dan önce veya sonra `--path`'i değiştirip değiştirmediğinize bağlı olarak.

Bu özelliğin doğası gereği, cryptkey dosyasını `ASF_CRYPTKEY_FILE` çevre değişkenini belirleyerek de ayarlamak mümkündür, bu da sürecin argümanlarında hassas detaylardan kaçınmak isteyenler için daha uygun olabilir.

---

`--ignore-unsupported-environment` - ASF'nin desteklenmeyen bir ortamda çalışmaya ilişkin sorunları göz ardı etmesine neden olur, bu normalde bir hata ile bildirilir ve zorunlu bir çıkışla sonuçlanır. Desteklenmeyen ortamlar, örneğin `linux-x64` üzerinde `win-x64` işletim sistemi özel yapısını çalıştırmayı içerir. Bu bayrak, ASF'nin bu tür senaryolarda çalışmayı denemesine izin verirken, bu senaryoların resmi olarak desteklenmediğini ve tamamen **kendi sorumluluğunuzda** ASF'yi zorladığınızı unutmayın. **Tüm** desteklenmeyen ortam senaryolarının **düzeltilebileceğini** belirtmek önemlidir. Mevcut sorunları düzeltmenizi şiddetle tavsiye ederiz.

---

`--input-cryptkey` - ASF'nin başlatma sırasında `--cryptkey` hakkında sormasına neden olur. Bu seçenek, çevre değişkenlerinde veya bir dosyada cryptkey sağlamaktansa, her ASF çalıştırmasında manuel olarak girmek isteyenler için yararlı olabilir.

---

`--minimized` - ASF konsol penceresinin başlatıldıktan kısa bir süre sonra küçültülmesine neden olur. Özellikle otomatik başlatma senaryolarında kullanışlıdır, ancak bu senaryolar dışında da kullanılabilir. Bu seçenek, uygun ortam desteği gerektirir - tüm olası senaryolarda düzgün çalışmayabilir.

---

`--network-group <group>` veya `--network-group=<group>` - ASF'nin limitlerini `<group>` değeriyle özel bir ağ grubuyla başlatmasına neden olur. Bu seçenek, **[birden fazla instance](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#multiple-instances)** çalıştırarak, belirli bir instance'ın yalnızca aynı ağ grubunu paylaşan instance'lara bağımlı olduğunu ve geri kalanından bağımsız olduğunu belirtir. Genellikle bu özelliği yalnızca ASF isteklerini özel bir mekanizma (örneğin farklı IP adresleri) aracılığıyla yönlendiriyorsanız ve ağ gruplarını kendiniz ayarlamak istiyorsanız kullanmak istersiniz, ASF'nin bunu otomatik olarak yapmasına güvenmek yerine (bu şu anda yalnızca `WebProxy`'yi dikkate almak anlamına gelir). Özel bir ağ grubu kullanırken, bu yerel makine içinde benzersiz bir tanımlayıcıdır ve ASF herhangi bir diğer detayı, örneğin `WebProxy` değerini dikkate almaz, böylece farklı `WebProxy` değerleri olan iki instance'ı başlatmanıza ve bunların yine de birbirine bağımlı olmasına izin verir.

Bu özelliğin doğası gereği, değeri `ASF_NETWORK_GROUP` çevre değişkenini belirleyerek de ayarlamak mümkündür, bu da sürecin argümanlarında hassas detaylardan kaçınmak isteyenler için daha uygun olabilir.

---

`--no-config-migrate` - varsayılan olarak ASF, yapılandırma dosyalarınızı en son sözdizimine otomatik olarak geçirecektir. Geçiş, eski özelliklerin en son özelliklere dönüştürülmesini, varsayılan değerlere sahip özelliklerin kaldırılmasını (çünkü bunların etkisi yoktur) ve dosyanın genel olarak temizlenmesini (girintiyi düzeltmek gibi) içerir. Bu neredeyse her zaman iyi bir fikirdir, ancak ASF'nin yapılandırma dosyalarını otomatik olarak yeniden yazmasını istemediğiniz belirli bir durumunuz olabilir. Örneğin, yapılandırma dosyalarınızı (yalnızca sahip için okuma izni) `chmod 400` yapmak veya `chattr +i` ile dosyaları değiştirmek isteyebilirsiniz, bu da güvenlik önlemi olarak herkes için yazma erişimini reddeder. Genellikle yapılandırma geçişinin etkin tutulmasını öneririz, ancak bunu devre dışı bırakmak ve bunun yerine ASF'nin bunu yapmamasını tercih etmek için belirli bir nedeniniz varsa, bu anahtarı kullanarak bu amacı gerçekleştirebilirsiniz. Ancak, doğru ayarları ASF'ye sağlama sorumluluğunun bundan sonra sizin yeni sorumluluğunuz olacağını, özellikle gelecekteki ASF sürümlerinde özelliklerin kullanımdan kaldırılması ve yeniden düzenlenmesi konusunda unutmayın.

---

`--no-config-watch` - varsayılan olarak ASF, dosya değişiklikleriyle ilgili olayları dinlemek için `config` dizininiz üzerinde bir `FileSystemWatcher` kurar, böylece onlara etkileşimli olarak uyum sağlayabilir. ılmasını veya **[BGR](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)**'ye anahtarlarınızı `config` dizinine bıraktığınızda anahtarların yüklenmesini içerir. Bu anahtar, ASF'nin `config` dizinindeki tüm değişiklikleri tamamen görmezden gelmesini sağlar, bu da bu tür eylemleri manuel olarak gerçekleştirmenizi gerektirir, eğer uygun görülürse (bu genellikle süreci yeniden başlatmak anlamına gelir). Yapılandırma olaylarını etkin tutmayı öneririz, ancak belirli bir nedeniniz varsa ve bunun yerine ASF'nin bunu yapmamasını tercih ediyorsanız, bu anahtarı kullanarak bu amacı gerçekleştirebilirsiniz.

---

`--no-restart` - bu anahtar, **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker)** konteynerlerimiz tarafından kullanılır ve `AutoRestart`'ı `false` olarak zorlar. Özel bir ihtiyacınız yoksa, bunun yerine `AutoRestart` özelliğini doğrudan yapılandırmanızda yapılandırmalısınız. Bu anahtar, docker betiğimizin genel yapılandırmanızı dokunmadan kendi ortamına uyarlayabilmesi için burada. Elbette, ASF'yi bir betik içinde çalıştırıyorsanız, bu anahtarı da kullanabilirsiniz (aksi takdirde genel yapılandırma özelliğiyle daha iyisiniz).

---

`--no-steam-parental-generation` - varsayılan olarak ASF, **[`SteamParentalCode`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#steamparentalcode)** yapılandırma özelliğinde açıklandığı gibi otomatik olarak Steam ebeveyn PIN'leri oluşturmaya çalışır. Ancak, bu, aşırı miktarda OS kaynakları gerektirebilir, bu anahtar, bu davranışı devre dışı bırakmanızı sağlar, bu da ASF'nin otomatik oluşturmayı atlayıp kullanıcıdan PIN istemeye doğrudan geçmesine neden olur, bu normalde yalnızca otomatik oluşturma başarısız olduğunda gerçekleşir. Genellikle oluşturmanın etkin tutulmasını öneririz, ancak belirli bir nedeniniz varsa ve bunun yerine bu davranışı devre dışı bırakmak istiyorsanız, bu anahtarı kullanarak bu amacı gerçekleştirebilirsiniz.

---

`--path <path>` veya `--path=<path>` - ASF her başlatıldığında kendi dizinine gider. Bu argümanı belirterek, ASF başlatma işleminden sonra belirtilen dizine gider, bu da çeşitli uygulama parçaları (`config`, `logs`, `plugins` ve `www` dizinleri ile `NLog.config` dosyası dahil) için özel bir yol kullanmanıza izin verir, binary'yi aynı yerde çoğaltmaya gerek kalmadan. Bu, özellikle binary'yi gerçek yapılandırmadan ayırmak istiyorsanız faydalı olabilir, Linux benzeri paketlemede yapıldığı gibi - bu şekilde, birden fazla farklı kurulumla tek bir (güncel) binary kullanabilirsiniz. Yol, ASF binary'sinin mevcut konumuna göre göreceli veya mutlak olabilir. Bu komutun yeni "ASF evi"ne işaret ettiğini unutmayın - orijinal ASF ile aynı yapıya sahip bir dizin, içinde `config` dizini olan, aşağıdaki örnekte açıklandığı gibi.

Bu özelliğin doğası gereği, `ASF_PATH` çevre değişkenini belirleyerek beklenen yolu ayarlamak da mümkündür, bu da sürecin argümanlarında hassas detaylardan kaçınmak isteyenler için daha uygun olabilir.

ASF'nin birden fazla örneğini çalıştırmak için bu komut satırı argümanını kullanmayı düşünüyorsanız, bu konuyla ilgili **[yönetim sayfamızı](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#multiple-instances)** okumanızı öneririz.

Örnekler:

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/TargetDirectory # Mutlak yol
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../TargetDirectory # Göreceli yol da çalışır
ASF_PATH=/opt/TargetDirectory dotnet /opt/ASF/ArchiSteamFarm.dll # Çevre değişkeni olarak aynı
```

```text
├── 📁 /opt
│     ├── 📁 ASF
│     │     ├── ⚙️ ArchiSteamFarm.dll
│     │     └── ...
│     └── 📁 TargetDirectory
│           ├── 📁 config
│           ├── 📁 logs (oluşturulmuş)
│           ├── 📁 plugins (isteğe bağlı)
│           ├── 📁 www (isteğe bağlı)
│           ├── 📄 log.txt (oluşturulmuş)
│           └── 📄 NLog.config (isteğe bağlı)
└── ...
```

---

`--service` - bu anahtar, `systemd` hizmetimiz tarafından kullanılır ve `Headless`'ı `true` olarak zorlar. Özel bir ihtiyacınız yoksa, bunun yerine `Headless` özelliğini doğrudan yapılandırmanızda yapılandırmalısınız. Bu anahtar, `systemd` hizmetimizin genel yapılandırmanızı dokunmadan kendi ortamına uyarlayabilmesi için burada. Benzer bir ihtiyacınız varsa, bu anahtarı da kullanabilirsiniz (aksi takdirde genel yapılandırma özelliğiyle daha iyisiniz).

---

`--system-required` - bu anahtarı belirlemek, ASF'nin işleminin ömrü boyunca sistemin açık ve çalışır durumda olmasını gerektirdiğini OS'ye sinyal vermeye çalışmasına neden olur. Bu anahtar, şu anda yalnızca Windows makinelerinde etkilidir ve bu, işlem çalıştığı sürece sisteminizin uyku moduna geçmesini engeller. Özellikle gece boyunca PC'nizde veya dizüstü bilgisayarınızda farming yaparken kullanışlı olabilir, çünkü ASF çalışırken sisteminizi uyanık tutabilecektir.
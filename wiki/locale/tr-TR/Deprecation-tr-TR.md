# Kullanımdan kaldırma

Gelişimi ve kullanımını çok daha tutarlı hale getirmek için tutarlı bir kullanımdan kaldırma politikası izlemeye çalışıyoruz.

---

## Kullanımdan Kaldırma Nedir?

Kullanımdan kaldırma, daha önce kullanılan seçenekleri, argümanları, işlevleri veya kullanım durumlarını geçersiz kılan küçük veya büyük değişikliklerin sürecidir. Kullanımdan kaldırma genellikle, söz konusu şeyin başka (benzer) bir biçime yeniden yazıldığı anlamına gelir ve uygun şekilde geçiş yapmanız gerektiğini garanti eder. Bu durumda, sadece belirli bir işlevselliğin daha uygun bir yere taşınması söz konusudur.

ASF hızla değişir ve her zaman daha iyi hale gelmeye çalışır. Maalesef, bu, mevcut işlevselliğin bazı bölümlerinin yeni özelliklerden, uyumluluktan veya kararlılıktan yararlanmak için başka bir programa taşınabileceği anlamına gelir. Bu sayede yıllar önce yaptığımız eskimiş veya acı verici yanlış geliştirme kararlarına bağlı kalmak zorunda kalmayız. Önceki işlevselliğin beklenen kullanımına uygun makul bir yedek sunmaya çalışıyoruz, bu nedenle kullanımdan kaldırma genellikle zararsızdır ve önceki kullanıma küçük düzeltmeler gerektirir.

---

## Kullanımdan Kaldırma Aşamaları

ASF, geçişi çok daha kolay ve zahmetsiz hale getirmek için 2 aşamalı bir kullanımdan kaldırma süreci izleyecektir.

### Aşama 1

Aşama 1, belirli bir özelliğin kullanımdan kaldırılmasıyla başlar ve hemen başka bir çözümün (veya yeniden tanıtım planı yoksa hiçbir çözümün) mevcut olmasıyla gerçekleşir.

Bu aşamada, ASF kullanımdan kaldırılmış bir işlev kullanıldığında uygun bir uyarı mesajı gösterecektir. Mümkün olduğunca, ASF eski davranışı taklit etmeye ve uyumluluğunu sürdürmeye çalışacaktır. ASF, bu işlevsellik açısından en azından bir sonraki kararlı sürüme kadar Aşama 1'de kalacaktır. Bu, umarım uyumluluğu bozmadan, tüm araçlarınızda ve desenlerinizde yeni davranışı karşılamak için uygun geçişi yapabileceğiniz andır. Uygun değişiklikleri yaptığınızı, kullanımdan kaldırma uyarısını artık görmemekten anlayabilirsiniz.

### Aşama 2

Aşama 2, yukarıda açıklanan Aşama 1'in ardından ve kararlı bir sürümde yayınlandıktan sonra planlanmıştır. Bu aşama, kullanımdan kaldırılmış işlevselliğin tamamen kaldırılmasını getirir, bu da ASF'nin kullanımdan kaldırılmış bir işlevi kullanmaya çalıştığınızı bile tanımayacağı anlamına gelir, çünkü bu işlev mevcut kodda bulunmaz. ASF artık herhangi bir uyarı göstermeyecek, çünkü ne yaptığınızı tanımamaktadır.

---

## Özet

Geçiş yapmak için daha az veya çok **bir ay** süreniz var, bu da hatta sıradan bir ASF kullanıcısı olsanız bile yeterli olmalıdır. Bu süreden sonra, ASF eski ayarların herhangi bir etkisi olacağını garanti etmez (Aşama 2), bu da belirli özelliklerin tamamen işlevsiz hale gelmesine neden olabilir. Bir aydan uzun sürelik bir etkinlikten sonra ASF'yi başlatıyorsanız, **[sıfırdan başlayın](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up)** veya kaçırdığınız tüm değişiklik günlüklerini okuyup kullanımınızı güncel duruma manuel olarak uyarlamanız tavsiye edilir.

Çoğu durumda, kullanımdan kaldırma uyarısını göz ardı etmek genel ASF işlevselliğini kullanılamaz hale getirmeyecektir, ancak varsayılan davranışa geri dönecektir (bu da kişisel tercihlerinize uymayabilir).

---

## Örnek

Önceki V3.1.2.2 sürümünde `--server` **[komut satırı argümanı](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)** `IPC` **[global yapılandırma özelliğine](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)** taşındı.

### Aşama 1

Aşama 1, V3.1.2.2 sürümünde `--server` kullanımı için uygun uyarıyı eklediğimizde gerçekleşti. Artık geçersiz olan `--server` argümanı, `IPC: true` global yapılandırma özelliğine otomatik olarak eşlendi ve bu geçici olarak eski `--server` anahtarıyla aynı şekilde davranıyordu. Bu, herkesin ASF eski argümanı kabul etmeyi durdurmadan önce uygun geçişi yapabilmesini sağladı.

### Aşama 2

Aşama 2, V3.1.2.9 kararlı sürümünün hemen ardından V3.1.3.0 sürümünde gerçekleşti ve yukarıda açıklanan Aşama 1'den sonra geldi. Aşama 2, ASF'nin `--server` argümanını tanımayı tamamen bırakmasını sağladı ve bunu, program üzerinde artık etkisi olmayan diğer geçersiz argümanlar gibi ele aldı. `--server` kullanımını `IPC: true` olarak değiştirmeyenler için, ASF artık uygun eşleme yapmadığından IPC tamamen işlevsiz hale geldi.
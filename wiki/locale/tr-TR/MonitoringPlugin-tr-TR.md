# MonitoringPlugin

`MonitoringPlugin`, ASF'nin resmi **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)**'idir ve **[Prometheus](https://prometheus.io)** zaman serisi veritabanı aracılığıyla ASF sürecini izlemenizi sağlar.

---

## Ekran Görüntüleri

<details>
  <summary>Göster</summary>

![screenshot](https://github.com/JustArchiNET/ArchiSteamFarm/assets/1069029/46778d0b-1ee6-4dab-8645-eb179f09468e)

</details>

---

## Gereksinimler

**[Teknik kısıtlamalar](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-development#native-dependencies)** nedeniyle, bu eklenti `generic` varyantındaki ASF'yi gerektirir.

---

## Enabling the plugin

ASF varsayılan olarak `MonitoringPlugin` ile birlikte gelmez, ancak her ASF sürümünde isteğe bağlı bir ek olarak dahil edilmiştir. Eklentiyi, ASF sürümünüzle uyumlu olan resmi **[yayın](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**'den indirin, ardından eklenti için özel bir `plugins/ArchiSteamFarm.OfficialPlugins.Monitoring` dizini oluşturun ve arşivi buraya çıkartın.

ASF'nin bir sonraki başlatılmasında, günlükler eklentinin başarılı bir şekilde yüklendiğini standart ASF günlükleme mekanizması aracılığıyla belirtecektir. Ayrıca, `/Api/metrics` URL'sine IPC arayüzünüzden erişerek bunu doğrulayabilirsiniz. IPC şifresi kullanıyorsanız, uygun yetkilendirme gerektiğinden, `/Api/metrics` URL'sine `?password=<YourIPCPassword>` eklemeniz gerekecektir. Görüntülediğiniz içerik aşağıdakine benzer olmalıdır:

```text
# TYPE asf_build_info gauge
# HELP asf_build_info Build information about ASF in form of label values
asf_build_info{variant="source",version="6.0.2.5"} 1 1713715703686

# TYPE asf_runtime_info gauge
# HELP asf_runtime_info Runtime information about ASF in form of label values
asf_runtime_info{framework=".NET 8.0.4",operating_system="Debian GNU/Linux trixie/sid",runtime="linux-x64"} 1 1713715703686
(...)
```

ASF ve botlarla ilgili metrikler, adlarında `asf_` öneki ile özel olarak belirlenmiştir. Diğer metrikler (örneğin .NET runtime veya ASF'nin `HttpClient`'ı ile ilgili olanlar), evrensel .NET süreç kurallarına dayalı olarak otomatik olarak üretilir ve bu tür bir öneki taşımaz.

---

## Prometheus Yapılandırması

Eklentinin düzgün çalıştığını doğruladıktan sonra, **[Prometheus](https://prometheus.io)** örneğinize aşağıdaki gibi bir scrape yapılandırması ekleyebilirsiniz:

```yaml
scrape_configs:
  - job_name: ArchiSteamFarm
    metrics_path: /Api/metrics
    params:
      password:
        - YourIPCPassword
    static_configs:
      - targets:
          - 127.0.0.1:1242
```

Doğal olarak, barındırılan **[Prometheus](https://prometheus.io)** örneğinizin ASF'nin IPC arayüzüne erişebildiğinden emin olmanız gerekir, `password` ve `targets`'ı kullanımınıza göre uyarlayın. IPC şifreniz ayarlı değilse (bu önerilmez), `params` bölümünü eklemeyi atlayabilirsiniz. Birden fazla ASF örneği çalıştırıyorsanız ve farklı IPC şifreleri kullanıyorsanız, ek scrape yapılandırmaları ekleyebilirsiniz; her örnek için bir tane. Aksi takdirde, aynı şifreyi paylaşan birkaç `targets`'ı belirtebilirsiniz.

---

## Grafana Panosu

Metrikleriniz Prometheus tarafından toplandıktan sonra, **[Grafana](https://grafana.com)** kullanarak görselleştirme yapabilirsiniz. Bu nedenle, ASF örneğinizi varsayılan ayarlarla çalıştırdığınızı varsayarak, dosyayı **[buradan](http://127.0.0.1:1242/grafana-dashboard.json)** indirebilirsiniz. Alternatif olarak, JSON dosyasını **[repository](https://raw.githubusercontent.com/JustArchiNET/ArchiSteamFarm/main/ArchiSteamFarm.OfficialPlugins.Monitoring/overlay/all/www/grafana-dashboard.json)**'dan da alabilirsiniz.
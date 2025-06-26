# 監控外掛程式

`MonitoringPlugin`是ASF的官方[**外掛程式**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-zh-TW)，它使您能夠透過[**Prometheus**](https://prometheus.io)時間序列資料庫監控ASF程序。

---

## 擷圖

<details>  <summary>顯示</summary>

![擷圖](https://github.com/JustArchiNET/ArchiSteamFarm/assets/1069029/46778d0b-1ee6-4dab-8645-eb179f09468e)

</details>

---

## 需求

由於[**技術限制**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-development-zh-TW#本機相依性)，這個外掛程式需要使用`generic`變體版本的ASF。

---

## 啟用外掛程式

預設情形下ASF**不**附隨`MonitoringPlugin`，但它仍作為選擇性的附加元件存在於每個ASF版本中。 從官方[**版本頁面**](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)下載符合您ASF版本的外掛程式，然後為該外掛程式建立一個專屬的`plugins/ArchiSteamFarm.OfficialPlugins.Monitoring`資料夾，並將壓縮檔解壓縮至其中。

在下次啟動ASF時，外掛程式將會透過標準ASF記錄機制來通知您成功啟用。 您也可以前往IPC介面`/Api/metrics` URL來驗證。 若您使用IPC密碼，您需要提供正確的驗證資訊，例如在`/Api/metrics` URL後面加上`?password=<您的IPC密碼>`。 您看到的內容應該類似下列：

```text
# TYPE asf_build_info gauge
# HELP asf_build_info Build information about ASF in form of label values
asf_build_info{variant="source",version="6.0.2.5"} 1 1713715703686

# TYPE asf_runtime_info gauge
# HELP asf_runtime_info Runtime information about ASF in form of label values
asf_runtime_info{framework=".NET 8.0.4",operating_system="Debian GNU/Linux trixie/sid",runtime="linux-x64"} 1 1713715703686
(...)
```

與ASF及其Bot的指標，會在它們的名稱中含有專屬前綴`asf_`。 其他的指標例如.NET執行環境或ASF的`HttpClient`，是依據通用.NET程序規則自動產生的，並不會帶有此前綴。

---

## Prometheus 設定

在您確認外掛程式正確運作後，可以像這樣在您的[**Prometheus**](https://prometheus.io)實例中新增一個收集設定：

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

很明顯您需要確保您代管的[**Prometheus**](https://prometheus.io)實例能夠存取ASF的IPC介面，並依您的實際使用方式填寫`password`及`targets`。 若您並未設定IPC密碼（並不建議這樣做），則能跳過`params`部分。 若您執行多個擁有不同IPC密碼的ASF實例，您可以新增額外的收集設定來對應每個實例，因為無法針對每個實例來設定出查詢參數。 但如果它們使用相同的密碼，您就能直接宣告多個`targets`。

---

## Grafana 儀表板

在Prometheus收集到您的各項數據後，您就能使用[**Grafana**](https://grafana.com)來視覺化這些資料。 外掛程式自帶了由標準IPC機制提供的`/grafana-dashboard.json`檔案，因此假設您以預設設定執行您的ASF實例，就可以在[**這裡**](http://127.0.0.1:1242/grafana-dashboard.json)下載。 或者，您也可以從我們的[**儲存庫**](https://raw.githubusercontent.com/JustArchiNET/ArchiSteamFarm/main/ArchiSteamFarm.OfficialPlugins.Monitoring/overlay/all/www/grafana-dashboard.json)中抓取。
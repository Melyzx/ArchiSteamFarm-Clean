# Überwachungserweiterung

Die `Überwachungserweiterung` ist eine offizielles ASF **[Erweiterung](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)**, welche Ihnen erlaubt den ASF Fortschritt mittels **[Prometheus](https://prometheus.io)** Datenbanken zu überwachen.

---

## Screenshots

<details>
<summary>Anzeigen</summary>

![screenshot](https://github.com/JustArchiNET/ArchiSteamFarm/assets/1069029/46778d0b-1ee6-4dab-8645-eb179f09468e)

</details>

---

## Voraussetzungen

Aufgrund **[technischer Einschränkungen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-development#native-dependencies)** erfordert diese Erweiterung eine `generische` Variante von ASF.

---

## Aktivierung der Erweiterung

ASF wird **nicht** standardmäßig mit der `Überwachungserweiterung` ausgeliefert, es ist jedoch als optionale Erweiterung in jeder ASF Version enthalten. Laden Sie die Erweiterung aus der offiziellen **[Version](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**, die Ihrer ASF Version entspricht, herunter, erstellen Sie dann ein eigenes Verzeichnis `plugins/ArchiSteamFarm.OfficialPlugins.Monitoring` für die Erweiterung und entpacken Sie das Archiv dorthin.

Beim nächsten Start von ASF wird in den Protokollen angezeigt, dass die Erweiterung über den Standard ASF Protokollierer erfolgreich geladen wurde. Sie können dies auch überprüfen, indem Sie in Ihrer IPC Schnittstelle zur URL `/Api/metrics` navigieren. Wenn Sie ein IPC Passwort verwenden, benötigen Sie eine entsprechende Autorisierung, z.B. durch das Anhängen von `?password=<YourIPCPassword>` an die URL `/Api/metrics`. Der Inhalte, den Sie sehen, sollte ähnlich wie unten aussehen:

```text
# TYPE asf_build_info gauge
# HELP asf_build_info Build information about ASF in form of label values
asf_build_info{variant="source",version="6.0.2.5"} 1 1713715703686

# TYPE asf_runtime_info gauge
# HELP asf_runtime_info Runtime information about ASF in form of label values
asf_runtime_info{framework=".NET 8.0.4",operating_system="Debian GNU/Linux trixie/sid",runtime="linux-x64"} 1 1713715703686
(...)
```

Messwerte, die ASF und die Bots betreffen, haben das `asf_` Präfix im Namen. Andere Messwerte, welche z.B. die .NET Laufzeit oder den `HttpClient` von ASF betreffen, werden automatisch auf der Grundlage universeller .NET Prozessregeln generiert und tragen kein solches Präfix.

---

## Prometheus Konfiguration

Sobald Sie sichergestellt haben, dass die Erweiterung korrekt funktioniert, können sie eine Scrape Konfiguration zu Ihrer **[Prometheus](https://prometheus.io)** Instanz hinzufügen, wie:

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

Natürlich müssen Sie sicherstellen, dass Ihre gehostete **[Prometheus](https://prometheus.io)** Instanz in der Lage ist, die IPC Schnittstelle von ASF zu erreichen, passen Sie `password` und `targets` dementsprechend an. Wenn Sie kein IPC Passwort vergeben haben (was nicht empfohlen wird), können Sie das Hinzufügen des Abschnitts `params` überspringen. Falls Sie mehrere ASF Instanzen mit unterschiedlichen IPC Passwörtern betreiben, können Sie zusätzliche Scrape Konfigurationen hinzufügen, pro Instanz eine, da die Abfrageparameter nicht pro Ziel festgelegt werden können. Andernfalls können Sie mehrere `Ziele` angeben, wenn sie das gleiche Kennwort haben.

---

## Grafana Dashboard

Sobald Ihre Messwerte von Prometheus erfasst wurden, ist es möglich **[Grafana](https://grafana.com)** zur Visualisierung zu verwenden. Die Erweiterung wird mit der Datei `/grafana-dashboard.json` geliefert, die über Standard IPC Mechanismen bereitgestellt wird. Wenn Sie also Ihre ASF-Instanz mit den Standardeinstellungen betreiben, können Sie es **[hier](http://127.0.0.1:1242/grafana-dashboard.json)** herunterladen. Alternativ können Sie die JSON-Datei auch aus unserem **[Repository](https://raw.githubusercontent.com/JustArchiNET/ArchiSteamFarm/main/ArchiSteamFarm.OfficialPlugins.Monitoring/overlay/all/www/grafana-dashboard.json)** herunterladen.
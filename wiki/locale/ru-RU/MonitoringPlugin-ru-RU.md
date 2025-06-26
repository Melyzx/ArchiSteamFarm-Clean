# Плагин мониторинга

`MonitoringPlugin` is an official ASF **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)**, which allows you to monitor ASF process via **[Prometheus](https://prometheus.io)** time-series database.

---

## Скриншоты

<details>
  <summary>Раскрыть</summary>

![screenshot](https://github.com/JustArchiNET/ArchiSteamFarm/assets/1069029/46778d0b-1ee6-4dab-8645-eb179f09468e)

</details>

---

## Требования

Из-за **[технических ограничений](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-development#native-dependencies)** для этого плагина требуется `generic` вариант ASF.

---

## Активация плагина

ASF **не** поставляется в комплекте `MonitoringPlugin` по умолчанию, однако он включен в качестве дополнения в каждом релизе ASF. Загрузите плагин с официального **[релиза](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**, который соответствует вашей версии ASF, а затем создайте выделеннyю папку`plugins/ArchiSteamFarm.OfficialPlugins.Monitoring` для плагина, и, наконец, распакуйте архив там.

On the next launch of ASF, the logs will indicate that the plugin has been successfully loaded through standard ASF logging mechanism. You can also verify this by navigating to `/Api/metrics` URL in your IPC interface. Если вы используете пароль IPC, вам понадобится соответствующая авторизация, например добавить `?password=<YourIPCPassword>` к адресу`/Api/metrics`. Контент, который вы видите должен выглядеть примерно так:

```text
# TYPE asf_build_info gauge
# HELP asf_build_info Информация о сборке ASF в виде значений меток
asf_build_info{variant="source",version="6.0.2. "} 1 1713715703686

# TYPE asf_runtime_info gauge
# HELP asf_runtime_info Информация о работе ASF в виде значений меток
asf_runtime_info{фреймворк=". ET 8.0.4",operating_system="Debian GNU/Linux trixie/sid",runtime="linux-x64"} 1 1713715703686
(...)
```

Metrics regarding ASF and the bots have dedicated prefix `asf_` in their name. Other metrics e.g. regarding the .NET runtime or ASF's `HttpClient` are automatically generated based on universal .NET process rules and do not carry such prefix.

---

## Prometheus config

Once you verified the plugin is working correctly, you can add a scrape configuration to your **[Prometheus](https://prometheus.io)** instance as such:

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

Naturally, you need to ensure that your hosted **[Prometheus](https://prometheus.io)** instance is able to reach ASF's IPC interface, adapt `password` and `targets` accordingly to your usage. If you do not have IPC password set (which is not recommended), you can skip the addition of the `params` section. In case you're running multiple ASF instances with different IPC passwords, you can add additional scrape configurations, one per instance, as the query parameters can not be set on a per-target basis. Otherwise, you can declare several `targets` if they share the same password.

---

## Grafana dashboard

Once your metrics are gathered by Prometheus, it's possible to use **[Grafana](https://grafana.com)** for visualization. The plugin comes with `/grafana-dashboard.json` file served by standard IPC mechanisms, so assuming you're running your ASF instance with default settings, you can download it **[here](http://127.0.0.1:1242/grafana-dashboard.json)**. Alternatively, you can also grab the JSON file from our **[repository](https://raw.githubusercontent.com/JustArchiNET/ArchiSteamFarm/main/ArchiSteamFarm.OfficialPlugins.Monitoring/overlay/all/www/grafana-dashboard.json)** as well.
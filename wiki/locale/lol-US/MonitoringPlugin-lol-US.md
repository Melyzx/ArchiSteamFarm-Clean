# MonitoringPlugin

`MonitoringPlugin` IZ AN OFFISHUL ASF **[PLUGIN](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-lol-US)**, WHICH ALLOWS U 2 MONITOR ASF PROCES VIA **[PROMETHEUS](https://prometheus.io)** TIEM-SERIEZ DATABASE.

---

## SCREENSHOTS

<details>
  <summary>SHOW</summary>

![screenshot](https://github.com/JustArchiNET/ArchiSteamFarm/assets/1069029/46778d0b-1ee6-4dab-8645-eb179f09468e)

</details>

---

## REQUIREMENTS

DUE 2 **[TECHNICAL CONSTRAINTS](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-development-lol-US#nativ-dependenciez)**, DIS PLUGIN REQUIREZ `generic` VARIANT OV ASF.

---

## ENABLIN TEH PLUGIN

ASF DOEZ **NOT** COME WIF `MonitoringPlugin` BUNDLD BY DEFAULT, HOWEVR, IZ INCLUDD AS OPSHUNAL ADDISHUN IN EVRY ASF RELEASE. DOWNLOAD TEH PLUGIN FRUM TEH OFFISHUL **[RELEASE](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** DAT MATCHEZ UR ASF VERSHUN, DEN CREATE DEDICATD `plugins/ArchiSteamFarm.OfficialPlugins.Monitoring` DIRECTORY 4 DA PLUGIN, AN FINALLY EXTRACT TEH ARCHIV THAR.

ON TEH NEXT LAUNCH OV ASF, TEH LOGS WILL INDICATE DAT TEH PLUGIN HAS BEEN SUCCESFULLY LOADD THRU STANDARD ASF LOGGIN MECHANISM. U CAN ALSO VERIFY DIS BY NAVIGATIN 2 `/Api/metrics` URL IN UR IPC INTERFACE. IF U R USIN IPC PASWORD, ULL NED PROPR AUTHORIZASHUN, E.G. APPENDIN `?password=<YourIPCPassword>` 2 TEH `/Api/metrics` URL. TEH CONTENT U C SHUD LOOK SIMILAR 2 BELOW:

```text
# TYPE asf_build_info gauge
# HELP asf_build_info Build information about ASF in form of label values
asf_build_info{variant="source",version="6.0.2.5"} 1 1713715703686

# TYPE asf_runtime_info gauge
# HELP asf_runtime_info Runtime information about ASF in form of label values
asf_runtime_info{framework=".NET 8.0.4",operating_system="Debian GNU/Linux trixie/sid",runtime="linux-x64"} 1 1713715703686
(...)
```

METRICS REGARDIN ASF AN TEH BOTS HAS DEDICATD PREFIX `asf_` IN THEIR NAYM. OTHR METRICS E.G. REGARDIN TEH .NET RUNTIME OR ASFS `HttpClient` R AUTOMATICALLY GENERATD BASD ON UNIVERSAL .NET PROCES RULEZ AN DO NOT CARRY SUCH PREFIX.

---

## PROMETHEUS CONFIG

ONCE U VERIFID TEH PLUGIN IZ WERKIN RITE, U CAN ADD SCRAPE CONFIGURASHUN 2 UR **[PROMETHEUS](https://prometheus.io)** INSTANCE AS SUCH:

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

NATURALLY, U NED 2 ENSURE DAT UR HOSTD **[PROMETHEUS](https://prometheus.io)** INSTANCE IZ ABLE 2 REACH ASFS IPC INTERFACE, ADAPT `password` AN `targets` ACCORDINGLY 2 UR USAGE. IF U DO NOT HAS IPC PASWORD SET (WHICH IZ NOT RECOMMENDD), U CAN SKIP TEH ADDISHUN OV TEH `params` SECSHUN. IN CASE URE RUNNIN MULTIPLE ASF INSTANCEZ WIF DIFFERENT IPC PASWORDZ, U CAN ADD ADDISHUNAL SCRAPE CONFIGURASHUNS, WAN PER INSTANCE, AS TEH QUERY PARAMETERS CAN NOT BE SET ON PER-TARGET BASIS. OTHERWIZE, U CAN DECLARE SEVERAL `targets` IF THEY SHARE TEH SAME PASWORD.

---

## GRAFANA DASHBORD

ONCE UR METRICS R GATHERD BY PROMETHEUS, IZ POSIBLE 2 USE **[GRAFANA](https://grafana.com)** 4 VISUALIZASHUN. TEH PLUGIN COMEZ WIF `/grafana-dashboard.json` FILE SERVD BY STANDARD IPC MECHANISMS, SO ASSUMIN URE RUNNIN UR ASF INSTANCE WIF DEFAULT SETTINGS, U CAN DOWNLOAD IT **[HER](http://127.0.0.1:1242/grafana-dashboard.json)**. ALTERNATIVELY, U CAN ALSO GRAB TEH JSON FILE FRUM R **[REPOSITORY](https://raw.githubusercontent.com/JustArchiNET/ArchiSteamFarm/main/ArchiSteamFarm.OfficialPlugins.Monitoring/overlay/all/www/grafana-dashboard.json)** AS WELL.
# MonitoringPlugin

`MonitoringPlugin` es un **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-es-ES)** oficial de ASF, que te permite monitorear el proceso de ASF a través de la base de datos de serie temporal **[Prometheus](https://prometheus.io)**.

---

## Capturas de pantalla

<details>  <summary>Mostrar</summary>

![screenshot](https://github.com/JustArchiNET/ArchiSteamFarm/assets/1069029/46778d0b-1ee6-4dab-8645-eb179f09468e)

</details>

---

## Requisitos

Debido a **[restricciones técnicas](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-development#native-dependencies)**, este plugin requiere la variante `generic` de ASF.

---

## Habilitando el plugin

ASF **no** viene con `MonitoringPlugin` empaquetado por defecto, sin embargo, está incluido como una adición opcional en cada versión de ASF. Descarga el plugin desde la **[publicación](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** oficial que coincida con tu versión de ASF, luego crea un directorio `plugins/ArchiSteamFarm.OfficialPlugins.Monitoring` dedicado para el plugin, y finalmente extrae ahí el archivo.

En la próxima ejecución de ASF, los registros indicarán que el plugin ha sido cargado exitosamente a través del mecanismo de registro estándar de ASF. También puedes verificar esto navegando a la URL `/Api/metrics` en tu interfaz IPC. Si estás usando una contraseña para IPC, necesitarás la autorización adecuada, por ejemplo, añadir `?password=<YourIPCPassword>` a la URL `/Api/metrics`. El contenido que verás debería ser similar al siguiente:

```text
# TYPE asf_build_info gauge
# HELP asf_build_info Build information about ASF in form of label values
asf_build_info{variant="source",version="6.0.2.5"} 1 1713715703686

# TYPE asf_runtime_info gauge
# HELP asf_runtime_info Runtime information about ASF in form of label values
asf_runtime_info{framework=".NET 8.0.4",operating_system="Debian GNU/Linux trixie/sid",runtime="linux-x64"} 1 1713715703686
(...)
```

Las métricas relativas a ASF y los bots tienen el prefijo dedicado `asf_` en su nombre. Otras métricas, por ejemplo, relativas a .NET runtime o el `HttpClient` de ASF son generadas automáticamente con base en las reglas universales del proceso .NET y no llevan dicho prefijo.

---

## Configuración de Prometheus

Una vez que hayas verificado que el plugin funciona correctamente, puedes añadir una configuración a tu instancia de **[Prometheus](https://prometheus.io)**, tal como:

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

Naturalmente, necesitas asegurarte de que tu instancia de **[Prometheus](https://prometheus.io)** es capaz de conectarse con la interfaz IPC de ASF, adapta `password` y `targets` de acuerdo a tu uso. Si no tienes establecida una contraseña para IPC (lo cual no se recomienda), puedes omitir la adición de la sección `params`. En caso de que ejecutes múltiples instancias de ASF con diferentes contraseñas para IPC, puedes añadir configuraciones adicionales, una por instancia, ya que los parámetros de consulta no pueden establecerse por cada objetivo. De lo contrario, puedes declarar varios `targets` si comparten la misma contraseña.

---

## Grafana dashboard

Una vez que tus métricas sean recolectadas por Prometheus, es posible usar **[Grafana](https://grafana.com)** para su visualización. El plugin viene con el archivo `/grafana-dashboard.json` servido por mecanismos estándar de IPC, asumiendo que estás ejecutando tu instancia de ASF con los ajustes predeterminados, puedes descargarlo **[aquí](http://127.0.0.1:1242/grafana-dashboard.json)**. Alternativamente, también puedes tomar el archivo JSON de nuestro **[repositorio](https://raw.githubusercontent.com/JustArchiNET/ArchiSteamFarm/main/ArchiSteamFarm.OfficialPlugins.Monitoring/overlay/all/www/grafana-dashboard.json)**.
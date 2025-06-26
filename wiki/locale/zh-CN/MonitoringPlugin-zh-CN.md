# 监控插件

`MonitoringPlugin` 是 ASF 官方[**插件**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-zh-CN)，它允许您通过 **[Prometheus](https://prometheus.io)** 时间序列数据库监控 ASF 进程。

---

## 屏幕截图

<details>
  <summary>显示</summary>

![screenshot](https://github.com/JustArchiNET/ArchiSteamFarm/assets/1069029/46778d0b-1ee6-4dab-8645-eb179f09468e)

</details>

---

## 安装要求

由于[技术限制](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-development-zh-CN#本机依赖项)，此插件需要 `generic` 版本的 ASF。

---

## 启用插件

ASF 默认**不**打包 `MonitoringPlugin`，但它会作为可选组件包含在每个 ASF 版本中。 从官方[**发行版页面**](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)下载符合您 ASF 版本的插件，然后为这个插件创建专门的 `plugins/ArchiSteamFarm.OfficialPlugins.Monitoring` 文件夹，最后将压缩文件解压到其中。

在 ASF 程序下次启动时，此插件会通过标准的 ASF 日志机制通知您插件已成功启用。 您也可以访问 IPC 接口的 `/Api/metrics` URL，来验证这一点。 如果您使用 IPC 密码，则需要提供正确的身份验证信息，例如，在 `/Api/metrics` URL 后面添加 `?password=<密码>`。 您看到的内容应该类似这样：

```text
# TYPE asf_build_info gauge
# HELP asf_build_info Build information about ASF in form of label values
asf_build_info{variant="source",version="6.0.2.5"} 1 1713715703686

# TYPE asf_runtime_info gauge
# HELP asf_runtime_info Runtime information about ASF in form of label values
asf_runtime_info{framework=".NET 8.0.4",operating_system="Debian GNU/Linux trixie/sid",runtime="linux-x64"} 1 1713715703686
(...)
```

与 ASF 及其机器人相关的指标名称包含专用前缀 `asf_`。 其他指标通常是 .NET 运行时环境或 ASF 的 `HttpClient` 根据通用 .NET 进程规则自动生成的，它们不带有这个前缀。

---

## Prometheus 配置

您确认插件正常工作之后，就可以像这样，在您的 **[Prometheus](https://prometheus.io)** 实例中添加一个抓取配置：

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

很显然，您需要确保您托管的 **[Prometheus](https://prometheus.io)** 实例能够访问到 ASF 的 IPC 接口，并根据您的实际情况填写 `password` 和 `targets` 配置。 如果您没有设置 IPC 密码（不推荐），则可以逃过 `params` 部分。 如果您正在运行多个 IPC 密码不同的 ASF 实例，则可以添加额外的抓取配置，每个实例对应一组配置，因为查询参数无法按照每个 Target 单独配置。 否则，如果他们的密码相同，您就可以直接声明多个 `targets`。

---

## Grafana 仪表盘

Prometheus 收集到您的统计指标后，您就可以使用 **[Grafana](https://grafana.com)** 来显示数据。 此插件自带 `/grafana-dashboard.json` 文件，并通过标准 IPC 机制托管，所以假设您正以默认设置运行 ASF 实例，则可以在[**这里**](http://127.0.0.1:1242/grafana-dashboard.json)下载。 或者，您也可以从我们的[**代码仓库**](https://raw.githubusercontent.com/JustArchiNET/ArchiSteamFarm/main/ArchiSteamFarm.OfficialPlugins.Monitoring/overlay/all/www/grafana-dashboard.json)获取。
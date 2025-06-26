# MonitoringPlugin

`MonitoringPlugin` é um **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-pt-BR)** oficial do ASF, que permite que você monitore o processo do ASF através do **[Prometheus](https://prometheus.io)** que é um banco de dados de cronogramas.

---

## Capturas de tela

<details>
  <summary>Mostrar</summary>

![screenshot](https://github.com/JustArchiNET/ArchiSteamFarm/assets/1069029/46778d0b-1ee6-4dab-8645-eb179f09468e)

</details>

---

## Requisitos

Devido a **[restrições técnicas](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-development-pt-BR#dependências-nativas)**, este plugin requer a variante `generic` do ASF.

---

## Ativando o Plugin

O ASF **não** vem com o `MonitoringPlugin` agrupado por padrão, no entanto, ele está incluso como adição opcional toda versão do ASF. Baixe o plugin da **[versão oficial](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** que corresponda à sua versão do ASF, depois crie um diretório dedicado `plugins/ArchiSteamFarm.OfficialPlugins.Monitoring` para o plugin e, por fim, extraia o arquivo compactado nesse local.

Na próxima inicialização do ASF, os logs indicarão que o plugin foi carregado com sucesso através do mecanismo padrão de logs do ASF. Você também pode verificar isso navegando até a URL `/Api/metrics` na sua interface IPC. Se você está usando a senha IPC, você precisará de autorização adequada, acrescentando `?password=<YourIPCPassword>` à URL `/Api/metrics`. O conteúdo que você vê deve se parecer com o mesmo conteúdo abaixo:

```text
# TYPE asf_build_info gauge
# HELP asf_build_info Build information about ASF in form of label values
asf_build_info{variant="source",version="6.0.2.5"} 1 1713715703686

# TYPE asf_runtime_info gauge
# HELP asf_runtime_info Runtime information about ASF in form of label values
asf_runtime_info{framework=".NET 8.0.4",operating_system="Debian GNU/Linux trixie/sid",runtime="linux-x64"} 1 1713715703686
(...)
```

Métricas do ASF e os bots dedicaram o prefixo `asf_` no seu nome. Outras métricas, como as relacionadas ao runtime do .NET ou ao `HttpClient` do ASF, são geradas automaticamente com base nas regras universais de processo do .NET e não possuem esse prefixo.

---

## Configuração do Prometheus

Depois de verificar que o plugin está funcionando corretamente, você pode adicionar uma configuração de scrape (coleta) à sua instância do **[Prometheus](https://prometheus.io)** da seguinte maneira:

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

Naturalmente, você precisa garantir que sua instância do **[Prometheus](https://prometheus.io)** hospedada seja capaz de acessar a interface IPC do ASF, adaptando a `password` e `targets` de acordo com o seu uso. Caso você não definiu uma senha para o IPC (o que não é recomendado), pode pular a adição da seção `params`. Caso você esteja executando várias instâncias do ASF com senhas de IPC diferentes, você pode adicionar configurações de scrape (coleta) adicionais, uma para cada instância, pois os parâmetros de consulta não podem ser definidos individualmente para cada destino. Caso contrário, você pode declarar vários `targets` se eles compartilharem a mesma senha.

---

## Painel do Grafana

Uma vez que suas métricas são coletadas pelo Prometheus, é possível usar o **[Grafana](https://grafana.com)** para visualização. O plugin vem com o arquivo `/grafana-dashboard.json` servido pelos mecanismos padrão do IPC, então, assumindo que você está executando sua instância do ASF com as configurações padrão, você pode baixá-lo **[aqui](http://127.0.0.1:1242/grafana-dashboard.json)**. Alternativamente, você também pode obter o arquivo JSON do nosso **[repositório](https://raw.githubusercontent.com/JustArchiNET/ArchiSteamFarm/main/ArchiSteamFarm.OfficialPlugins.Monitoring/overlay/all/www/grafana-dashboard.json)**.
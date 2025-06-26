# Comunicação remota

Esta seção elabora sobre a comunicação remota que o ASF inclui, incluindo uma explicação adicional sobre como alguém pode influenciá-la. Embora não consideremos nada abaixo como malicioso ou indesejado de outra forma, e tampouco somos legalmente obrigados a divulgá-lo, queremos que você entenda melhor a funcionalidade do programa, especialmente em relação à sua privacidade e aos dados compartilhados.

## Steam

O ASF se comunica com a rede Steam (**[servidores CM](https://api.steampowered.com/ISteamDirectory/GetCMList/v1?cellid=0)**), bem como a **[API do Steam](https://steamcommunity.com/dev)**, a **[Loja Steam](https://store.steampowered.com)** e a **[Comunidade Steam](https://steamcommunity.com)**.

Não é possível desabilitar nenhuma das comunicações acima, pois é a base fundamental na qual o ASF se baseia para fornecer sua funcionalidade básica. Você precisará se abster de usar o ASF caso não se sentir confortável com o que foi mencionado acima.

## Grupo Steam

O ASF se comunica com o nosso **[grupo Steam](https://steamcommunity.com/groups/archiasf)**. O grupo fornece a você anúncios, especialmente sobre novas versões, problemas críticos, problemas do Steam e outras coisas importantes para manter a comunidade atualizada. Também permite que você use nosso suporte técnico, fazendo perguntas, resolvendo problemas, relatando problemas ou sugerindo melhorias. Por padrão, as contas usadas no ASF entrarão automaticamente no grupo após iniciar a sessão.

Você pode decidir não participar do grupo desativando a opção `SteamGroup` nas configurações de **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#remotecommunication)** do bot.

## GitHub

O ASF se comunica com a **[API do GitHub](https://api.github.com)** para buscar **[atualizações do ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** para a funcionalidade de atualização. Isso é feito como parte das atualizações automáticas (caso você manteve **[`UpdatePeriod`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#updateperiod)** habilitado), assim como o **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)** `update`. Você pode influenciar a comunicação do ASF com o GitHub através da propriedade **[`UpdateChannel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#updatechannel)** - definir como `None` resultará na desativação de toda a funcionalidade de atualização, incluindo a comunicação com o GitHub a esse respeito.

## Servidor do ASF

O ASF se comunica com **[nosso próprio servidor](https://asf.justarchi.net)** para funcionalidades mais avançadas. Em particular, isto inclui:
- Verificação dos checksums das compilações do ASF baixadas do GitHub em relação ao nosso próprio banco de dados independente para garantir que todas as compilações baixadas sejam legítimas (livres de malware, ataques de MITM ou outras manipulações)
- Obtenção da lista de bots ruins para filtragem caso você manteve a configuração global `FilterBadBots` habilitada.
- Divulgação de seu bot em **[nossa lista](https://asf.justarchi.net/STM)** caso você ativou `SteamTradeMatcher` em **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#tradingpreferences)** e atender a outros critérios
- Download dos bots disponíveis para troca da **[nossa listagem](https://asf.justarchi.net/STM)** caso você tiver habilitado `MatchActively` em **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#tradingpreferences)** e que atenda aos outros critérios

Como medida de segurança, não é possível desabilitar a verificação de checksum para as compilações do ASF. No entanto, você pode desativar completamente as atualizações automáticas caso desejar evitar isso, como descrito acima na seção do GitHub.

Você pode desativar a configuração `FilterBadBots` caso quiser evitar a obtenção da lista do servidor.

Você pode optar por não ser divulgado na lista desativando a opção `PublicListing` nas configurações de **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#remotecommunication)** do bot. Isso pode ser útil caso você quiser executar um bot do `SteamTradeMatcher` sem ser divulgado ao mesmo tempo.

O download de bots de nossa lista é obrigatório para a configuração `MatchActively`, você precisará desativar essa configuração caso não estiver disposto a aceitar isso.
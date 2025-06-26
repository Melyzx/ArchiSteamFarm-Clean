# ItemsMatcherPlugin

`ItemsMatcherPlugin` é um **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-pt-BR)** oficial que estende o ASF com recursos de listagem do ASF STM. Em particular, isso inclui `PublicListing` em **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#remotecommunication)** e `MatchActively` em **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#tradingpreferences)**. O ASF vem com o `ItemsMatcherPlugin` incluído junto com o lançamento, portanto está pronto para uso imediato.

---

## `PublicListing`

A listagem pública, como o nome indica, é a listagem dos bots ASF STM atualmente disponíveis. A listagem pública está localizada em **[nosso site](https://asf.justarchi.net/STM)** ela é gerenciada automaticamente e é usada como um serviço público tanto para os usuários do ASF que usam o `MatchActively`, bem como usuários e não-usuários do ASF para troca manual.

Para que seu inventário seja listado você tem um conjunto de requisitos a cumprir. No mínimo, você deve permitir a opção `PublicListing` em `RemoteCommunication` (configuração padrão), assim como ter a opção `SteamTradeMatcher` ativa em `TradingPreferences`, um **[inventário público](https://steamcommunity.com/my/edit/settings)** uma conta **[não limitada](https://help.steampowered.com/faqs/view/71D3-35C2-AD96-AA3A)** e o **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)** ativo. Requisitos adicionais incluem a autenticação 2FA ativa há pelo menos 15 dias, a última alteração de senha há mais de 5 dias, a falta de limitações de conta, como bloqueios, proibições económicas e proibições de troca. Naturally, you must also have at least one (tradable) item in your inventory from specified `MatchableTypes`, such as trading cards. Além disso, bots com mais de `500000` não são aceitos devido à sobrecarga, nesse caso recomendamos dividir seu inventário em várias contas.

Mesmo a `PublicListing` vindo ativada por padrão, por favor, note que você **não** será exibido no site se você não cumprir todos os requisitos, especialmente a configuração `SteamTradeMatcher` que não está habilitada por padrão. Para pessoas que não atendem aos critérios, o ASF não se comunicará com o servidor mesmo que elas mantenham a `PublicListing` habilitada. Além disso, a listagem pública é compatível somente com a última versão estável do ASF e pode se recusar a mostrar bots desatualizados, especialmente se faltar neles funcionalidades cruciais que só podem ser encontradas em novas versões.

### Como exatamente isso funciona

O ASF envia os dados iniciais uma vez após o login, que contém todas as propriedades que a listagem pública utiliza. Em seguida, a cada 10 minutos o ASF envia uma solicitação muito pequena de "pulso" que notifica o nosso servidor de que o bot ainda está sendo executado. Se por algum motivo o pulso não chegar, por exemplo devido a problemas de rede, então o ASF vai tentar reenviá-lo a cada minuto, até o servidor registrá-lo. Desta forma, nosso servidor sabe precisamente quais bots ainda estão em execução e prontos para aceitar ofertas de troca. O ASF também enviará um anúncio inicial conforme necessário, por exemplo, se ele detectar que nosso inventário sofreu uma mudança.

Nós exibimos todas as contas ASF com 2FA+STM que estuiveram ativas **nos últimos 15 minutos**. Os usuários são classificados de acordo com sua utilidade relativa - bots configurados com `MatchEverything` com a tag `Any` que aceitem todas as trocas 1:1, depois pela quantidade de jogos únicos e finalmente os itens.

### API

A listagem API do STM só aceita bots ASF no momento. Não há como inserir bots de terceiros em nossa listagem no momento, já que não podemos analisar seus códigos facilmente e assegurar que eles satisfazem nossa lógica de trocas. Portanto, para participar da listagem você precisa ter a última versão estável do ASF, embora ela possa rodar com **[plugins](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-pt-BR)**.

Para os usuários da listagem nós temos um endpoint muito simples **[`/Api/Listing/Bots`](https://asf.justarchi.net/Api/Listing/Bots)** que pode ser usado. Ele inclui todos os dados que temos, além dos inventários dos usuários que fazem parte do recurso `Matchativamente`.

### Política de privacidade

Se você concordar em aparecer na nossa listagem, ativando o `SteamTradeMatcher` e não recusando a `PublicListing`, conforme especificado acima, vamos armazenar temporariamente alguns dados da sua conta Steam em nosso servidor para fornecer as funcionalidades esperadas.

Informações públicas(expostas pela Steam para interessados) inclui:
- Seu identificador Steam (na forma de 64-bit, para gerar ligações)
- Seu apelido (para fins de exibição)
- Seu avatar (hash, para fins de exibição)

As informações condicionalmente públicas (expostas pelo Steam a cada parte interessada se você atender aos requisitos da listagem) incluem:
- Seu **[inventário](https://steamcommunity.com/my/inventory/#753_6)** (para que as pessoas possam usar `MatchActively` com os seus itens).

Informações privadas (dados selecionados necessários para fornecer as funcionalidades) incluem:
- Seu **[token de trocas](https://steamcommunity.com/my/tradeoffers/privacy)** (para que pessoas fora da sua lista de amigos possam te enviar propostas de trocas)
- Sua configuração `MatchableTypes` (para fins de exibição e correspondência)
- Suca configuração `MatchEverything` (para fins de exibição e correspondência)
- Sua configuração `MaxTradeHoldDuration` (para que outras pessoas saibam se você está disposto a aceitar as trocas)

Seus dados serão armazenados por, no máximo, duas semanas se você parar de usar (anunciar na) nossa listagem, e serão excluídos automaticamente após esse período.

---

## `MatchActively`

A configuração `MatchActively` é a versão ativa de **[`SteamTradeMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-pt-BR#steamtradematcher)** que inclui correspondência interativa onde o bot enviará negociações para outras pessoas. Isso pode funcionar em espera sozinho, ou junto com a configuração `SteamTradeMatcher`. Essa função requer que você configure sua `LicenseID`, pois ela usa servidores terceirizados e recursos pagos para operar.

Para usar essa opção há uma série de requisitos para serem atendidas. Você precisa, pelo menos, uma conta **[sem restrições](https://help.steampowered.com/faqs/view/71D3-35C2-AD96-AA3A)**, o **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-pt-BR#asf-2fa)** ativo e ao menos um tipo válido em `MatchableTypes`, como cartas colecionáveis, por exemplo. Requisitos adicionais incluem a autenticação 2FA ativa há pelo menos 15 dias, última alteração de senha há mais de 5 dias, falta de limitações de conta como bloqueios, proibições do mercado e proibições de troca.

Se você cumprir todos os requisitos acima o ASF vai se comunicar periodicamente com a nossa **[listagem STM pública do ASF](#publiclisting)** para corresponder automaticamente com nossos bots que estiverem disponíveis.

Durante a correspondência o ASF vai pesquisar em seu inventário, então comunicar com nosso servidor para encontrar todos os possíveis `MatchableTypes` de outros bots que estejam disponíveis. Já que a comunicação é feita diretamente com nosso servidor o processo requer apenas uma simples requisição e já teremos informações se o bot oferece algo que nos interesse (se uma correspondencia for encontrada o ASF vai enviar e confirmar a troca automaticamente).

Esse módulo deve ser transparente. As correspondências devem começar em aproximadamente `1` hora desde a ativação do ASF, e repetirá automaticamente a cada `6` horas (caso necessário). A funcionalidade `MatchActively` foi projetada para ser usada como uma medida periódica de longo prazo, garantindo que estamos ativamente progredindo na conclusão de conjuntos de cartas. No entanto, pessoas que não executam o ASF 24/7 também podem considerar o uso do **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)** `match`. Os usuários alvos desse módulo são principalmente contas principais e contas alternativas "ocultas", embora ele possa ser usado em qualquer bot que não foi configurado para `MatchEverything`. Além disso, bots com mais de `500000` itens não são aceitos para pareamento devido ao excesso de carga. Recomendamos dividir seu inventário entre várias contas nesse caso.

O ASF faz o seu melhor para minimizar a quantidade de solicitações e a pressão gerada por usar esta opção, enquanto maximiza a eficiência das correspondências até o limite possível. O algoritmo exato de escolha dos bots para combinar e organizar todo o processo é um detalhe de implementação do ASF e pode mudar de acordo com os feedbacks, situações e possíveis ideias futuras.

A versão atual do algoritmo faz com que o ASF priorize os bots `Any`, especialmente aqueles que têm uma maior diversidade de jogos dos quais seus itens provêm. Quando não houver mais bots `Any` disponíveis, o ASF passará para os bots `Fair`, seguindo a mesma regra de diversidade. O ASF tentará parear cada bot disponível pelo menos uma vez, garantindo que não estamos perdendo progresso em um conjunto.

O `MatchActively` leva em conta os bots que você pôs na lista negra de trocas através do **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)** `tbadd` e não vai tentar trocas com eles. Pode ser usado para dizer ao ASF quais bots nunca devem ser combinados, mesmo se eles tiverem potenciais duplicatas que poderíamos usar.

O ASF também fará o possível para garantir que as ofertas de troca sejam concluídas. Na próxima execução, que normalmente ocorre em 6 horas, o ASF cancelará quaisquer ofertas de troca pendentes que ainda não tenham sido aceitas e despriorizará os SteamIDs envolvidos nelas, buscando priorizar bots mais ativos primeiro. Ainda assim, se os bots despriorizados forem os únicos que possuem a correspondência necessária, ainda tentaremos pareá-los (novamente).

---

### Por que preciso de um `LicenseID` para usar `MatchActively`? Não era de graça anteriormente?

ASF é, e continua de graça, com o código aberto, Como foi definido no começo do projeto em outubro de 2015. O código-fonte do plugin `ItemsMatcher` e, portanto, da funcionalidade `MatchActively`, está disponível em nosso repositório. O programa ASF é totalmente não comercial, não ganhamos nada com contribuições, desenvolvimento ou publicação. Nos últimos 7+ anos, o ASF recebeu uma quantidade enorme de desenvolvimento e continua sendo aprimorado a cada lançamento estável mensal, principalmente por uma única pessoa, **[JustArchi](https://github.com/JustArchi)**, sem qualquer compromisso. O único financiamento que recebemos vem de doações não obrigatórias feitas por nossos usuários.

Por um longo período, até outubro de 2022, a funcionalidade `MatchActively` fazia parte do núcleo do ASF e estava disponível para todos os usuários. Em outubro de 2022, a Valve, empresa por trás do Steam, impôs limites rigorosos na obtenção de inventários de outros bots, o que tornou a funcionalidade anterior completamente inutilizável, sem possibilidade de solução para esse problema. Portanto, devido ao fato de que a funcionalidade se tornou completamente inutilizável, sem chance de ser corrigida, ela precisou ser removida do núcleo do ASF por estar obsoleta.

`MatchActively` foi ressuscitado como parte do plugin oficial `ItemsMatcher`, que aprimora ainda mais o ASF com a funcionalidade de correspondência ativa de cartas. Ressuscitar a funcionalidade `MatchActively` exigiu de nós um **trabalho extraordinário** para criar o backend do ASF, um serviço totalmente novo hospedado em um servidor, com mais de uma centena de proxies pagos anexados para lidar com os inventários, tudo exclusivamente para permitir que os clientes do ASF utilizem o `MatchActively` como antes. Devido à quantidade de trabalho envolvida, bem como aos recursos que não são gratuitos e precisam ser pagos mensalmente por nós (domínio, servidor, proxies), decidimos oferecer essa funcionalidade aos nossos patrocinadores, ou seja, pessoas que já apoiam o projeto ASF mensalmente e graças às quais podemos disponibilizar esses recursos pagos.

Nosso objetivo não é lucrar com isso, mas sim cobrir os **custos mensais** exclusivamente relacionados à oferta dessa opção. Por isso, oferecemos essa funcionalidade praticamente de graça, mas precisamos cobrar um pequeno valor, pois não podemos arcar com centenas de dólares do nosso próprio bolso todo mês apenas para disponibilizá-la para você. Nós realmente não estamos em posição de discutir o preço, pois foi a Valve que nos impôs esses custos. A alternativa seria não ter essa funcionalidade disponível, o que, claro, se aplica caso você decida, por qualquer motivo, que não pode justificar o uso do nosso plugin nesses termos.

De qualquer forma, entendemos que `MatchActively` não é para todos e esperamos que você também compreenda por que não podemos oferecê-lo gratuitamente.

---

### Como posso conseguir acesso?

`ItemsMatcher` é oferecido como parte do nível de patrocínio mensal de $5+ no **[GitHub do JustArchi](https://github.com/sponsors/JustArchi)**. Também é possível se tornar um patrocinador único, embora, nesse caso, a licença seja válida apenas por um mês (com possibilidade de extensão da mesma forma). Após se tornar um patrocinador do nível de $5 (ou superior), leia a seção **[configuração](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#licenseid)** para obter e preencher o `LicenseID`. Depois disso, basta ativar `MatchActively` em `TradingPreferences` do bot escolhido.

A licença permite que você envie uma quantidade limitada de solicitações ao servidor. O nível $5 permite usar o `MatchActively` para uma conta bot (4 solicitações diárias), e cada $5 adicional adiciona mais duas contas bot (8 solicitações diariamente). Por exemplo, se você quiser executá-lo em três contas, isso será coberto pelo nível de $10 ou superior.
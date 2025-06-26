# Desenvolvimento de plugins

O ASF inclui suporte para plugins personalizados que podem ser carregados durante a execução. Os plugins permitem que você personalize o comportamento do ASF, adicionando comandos e lógicas de troca personalizados ou mesmo uma integração completa com serviços de terceiros e APIs.

Esta página descreve os plugins do ASF sob a perspectiva dos desenvolvedores — criação, manutenção, publicação e afins. Caso você quiser ver pela perspectiva do usuário, vá **[aqui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-pt-BR)** em vez disso.

---

## Base

Plugins são bibliotecas padrão .NET que definem uma classe que herda da interface comum `IPlugin`, declarada no ASF. Você pode desenvolver plugins de forma completamente independente da linha principal do ASF e reutilizá-los nas versões atuais e futuras do ASF, desde que a API interna do ASF permaneça compatível. O sistema de plugins utilizado no ASF é baseado no `System.Composition`, anteriormente conhecido como **[Managed Extensibility Framework](https://docs.microsoft.com/dotnet/framework/mef)**, que permite ao ASF descobrir e carregar suas bibliotecas durante a execução.

---

### Primeiros passos

Nós preparamos o **[ASF-PluginTemplate](https://github.com/JustArchiNET/ASF-PluginTemplate)** para você, que pode (e deve) usar como base para o seu projeto de plugin. Usar o modelo **não é obrigatório** (pois você pode fazer tudo do zero), mas nós altamente recomendamos que você o utilize, pois pode agilizar drasticamente seu desenvolvimento e reduzir o tempo necessário para acertar todos os detalhes. Simplesmente confira o **[README](https://github.com/JustArchiNET/ASF-PluginTemplate/blob/main/README.md)** do modelo e ele irá te guiar adiante. De qualquer forma, cobriremos os conceitos básicos abaixo caso você queira começar do zero ou entender melhor os conceitos utilizados no modelo de plugin - geralmente, você não precisa fazer nada disso se decidir usar nosso modelo de plugin.

Seu projeto deve ser uma biblioteca padrão .NET voltada para o framework apropriado da versão alvo do ASF, conforme especificado na seção de **[compilação](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compilation-pt-BR)**.

O projeto deve referenciar o assembly principal `ArchiSteamFarm`, seja pela biblioteca pré-compilada `ArchiSteamFarm.dll` que você baixou como parte do lançamento, ou pelo projeto de código-fonte (por exemplo, se você decidiu adicionar a árvore do ASF como um submódulo). Isso permitirá que você acesse e descubra as estruturas, métodos e propriedades do ASF, especialmente a interface principal `IPlugin`, da qual você precisará herdar no próximo passo. O projeto também deve referenciar, no mínimo, o `System.Composition.AttributedModel`, que permite a você usar o atributo `[Export]` para que seu `IPlugin` seja utilizado pelo ASF. Além disso, você pode querer ou precisar referenciar outras bibliotecas comuns para interpretar as estruturas de dados fornecidas em algumas interfaces. No entanto, a menos que precise delas explicitamente, isso será suficiente por enquanto.

Se você fez tudo certo, seu `csproj` será semelhante ao exemplo abaixo:

```csproj
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net9.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <!-- Como você está carregando o plugin no processo do ASF, que já inclui essa dependência, IncludeAssets="compile" permite que você a omita do resultado final. -->
    <PackageReference Include="System.Composition.AttributedModel" IncludeAssets="compile" Version="9.0.0" />
  </ItemGroup>

  <ItemGroup>
    <ProjectReference Include="C:\\Path\To\ArchiSteamFarm\ArchiSteamFarm.csproj" ExcludeAssets="all" Private="false" />

    <!-- Caso esteja compilando com o binário DLL baixado, use isso em vez de <ProjectReference> acima. -->
    <!-- <Reference Include="ArchiSteamFarm" HintPath="C:\\Path\To\Downloaded\ArchiSteamFarm.dll" /> -->
  </ItemGroup>
</Project>
```

Do lado do código, a classe do seu plugin deve herdar da interface `IPlugin` (seja explicitamente ou implicitamente, herdando de uma interface mais especializada, como `IASF`) e incluir o atributo `[Export(typeof(IPlugin))]` para ser reconhecida pelo ASF durante a execução. O exemplo mais simples que alcança isso seria o seguinte:

```csharp
using System;
using System.Composition;
using System.Threading.Tasks;
using ArchiSteamFarm;
using ArchiSteamFarm.Plugins;

namespace YourNamespace.YourPluginName;

[Export(typeof(IPlugin))]
public sealed class YourPluginName : IPlugin {
	public string Name => nameof(YourPluginName);
	public Version Version => typeof(YourPluginName).Assembly.GetName().Version;

	public Task OnLoaded() {
		ASF.ArchiLogger.LogGenericInfo("Hello World!");

		return Task.CompletedTask;
	}
}
```

Para utilizar o seu plugin, você deve primeiro compilá-lo. Você pode fazer isso tanto a partir do seu IDE quanto dentro do diretório raiz do seu projeto através do seguinte comando:

```shell
# Se o seu projeto for independente (não é preciso definir um nome já que ele será único)
dotnet publish -c "Release" -o "out"


# Se o seu projeto fizer parte da árvore de código do ASF (para evitar a compilação de partes desnecessárias)
dotnet publish YourPluginName -c "Release" -o "out"
```

Após isso seu plugin estará pronto. Cabe a você decidir exatamente como deseja distribuir e publicar seu plugin, mas recomendamos criar um arquivo zip onde você colocará seu plugin compilado junto com suas **[dependências](#dependências-de-plugin)**. Dessa forma, o usuário precisará apenas extrair seu arquivo zip para um subdiretório independente dentro do diretório `plugins` e não precisará fazer mais nada.

Esse é o cenário mais básico, apenas para começar. Temos o projeto **[`ExamplePlugin`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.CustomPlugins.ExamplePlugin)**, que mostra interfaces e ações de exemplo que você pode utilizar em seu próprio plugin, incluindo comentários úteis. Fique à vontade para dar uma olhada se quiser aprender com um código funcional ou explorar o namespace `ArchiSteamFarm.Plugins` por conta própria e consultar a documentação incluída para conhecer todas as opções disponíveis. Também iremos detalhar alguns conceitos fundamentais abaixo para explicá-los melhor.

Caso, em vez do plugin de exemplo, você queira aprender com projetos reais, existem vários plugins oficiais desenvolvidos por nós, como **[`ItemsMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.ItemsMatcher)**, **[`MobileAuthenticator`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.MobileAuthenticator)** ou **[`SteamTokenDumper`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.SteamTokenDumper)**. Além disso, há também plugins desenvolvidos por outros desenvolvedores em nossa seção de **[terceiros](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party-pt-BR#plugins-para-o-asf)**.

---

### Disponibilidade de API

Além do que você tem acesso diretamente nas interfaces, o ASF expõe muitas de suas APIs internas, que você pode utilizar para expandir a funcionalidade. Por exemplo, se você precisar enviar uma nova requisição para a web do Steam, não é necessário criar tudo do zero, evitando assim os desafios que já enfrentamos anteriormente. Basta usar nosso `Bot.ArchiWebHandler`, que já expõe vários métodos `UrlWithSession()` para você utilizar, lidando automaticamente com aspectos de baixo nível, como autenticação, atualização de sessão e limitação de requisições. Da mesma forma, para enviar requisições web fora da plataforma Steam, você pode usar a classe padrão `.NET HttpClient`, mas é uma ideia muito melhor utilizar o `Bot.ArchiWebHandler.WebBrowser`, que está disponível para você e oferece suporte adicional, como a repetição automática de requisições que falharam.

Temos uma política bastante aberta em relação à disponibilidade de nossa API. Portanto, se quiser utilizar algo que o código do ASF já inclua, basta **[abrir um problema](https://github.com/JustArchiNET/ArchiSteamFarm/issues)** e explicar nela o seu caso de uso planejado para a API interna do ASF. Muito provavelmente não teremos objeções, desde que seu caso de uso faça sentido. Isso também inclui todas as sugestões relacionadas a novas interfaces `IPlugin` que possam fazer sentido para expandir a funcionalidade existente.

Independentemente da disponibilidade da API do ASF, nada impede que você, por exemplo, inclua a biblioteca `Discord.Net` em sua aplicação e crie uma ponte entre seu bot do Discord e os comandos do ASF, já que seu plugin também pode ter suas próprias dependências. As possibilidades são infinitas, e fizemos o possível para oferecer a maior liberdade e flexibilidade dentro do seu plugin, sem impor limites artificiais. Seu plugin é carregado no processo principal do ASF e pode fazer qualquer coisa que seja viável dentro do código C#.

---

### Compatibilidade de API

É importante enfatizar que o ASF é uma aplicação de usuário final e não uma biblioteca típica com uma API fixa da qual você possa depender incondicionalmente. Isso significa que você não pode presumir que seu plugin, uma vez compilado, continuará funcionando com todas as futuras versões do ASF. Isso simplesmente não é viável se quisermos continuar desenvolvendo o programa. Além disso, abrir mão da adaptação às constantes mudanças do Steam apenas para manter a compatibilidade retroativa não seria adequado para o nosso caso. Isso deve ser algo lógico para você, mas é importante destacar esse fato.

Faremos o possível para manter as partes públicas do ASF funcionando e estáveis, mas não hesitaremos em quebrar a compatibilidade caso haja motivos suficientemente válidos, seguindo nossa política de **[descontinuação](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Deprecation-pt-BR)**. Isso é especialmente importante em relação às estruturas internas do ASF que são expostas a você como parte da infraestrutura do ASF (por exemplo, `ArchiWebHandler`), pois podem ser aprimoradas (e, consequentemente, reescritas) em futuras versões do ASF. Faremos o possível para informá-lo adequadamente nos changelogs e incluir avisos apropriados em tempo de execução sobre recursos obsoletos. Não pretendemos reescrever tudo apenas por reescrever, então você pode ter certa segurança de que a próxima versão menor do ASF não destruirá seu plugin apenas por ter um número de versão mais alto. No entanto, acompanhar os changelogs e verificar ocasionalmente se tudo está funcionando bem é uma ótima ideia.

---

### Dependências de plugin

Seu plugin incluirá pelo menos duas dependências por padrão: a referência `ArchiSteamFarm` para a API interna (no mínimo `IPlugin`) e a `PackageReference` de `System.Composition.AttributedModel`, que é necessária para que ele seja reconhecido como um plugin do ASF pela cláusula `[Export]`. Além disso, ele pode incluir mais dependências, dependendo do que você decidiu fazer em seu plugin (por exemplo, a biblioteca `Discord.Net` caso tenha decidido integrá-lo ao Discord).

A saída da sua compilação incluirá sua biblioteca principal `YourPluginName.dll`, assim como todas as dependências que você referenciou. Como você está desenvolvendo um plugin para um programa que já está em funcionamento, não é necessário e, na verdade, **não deve** incluir dependências que o ASF já inclui, como `ArchiSteamFarm`, `SteamKit2` ou `AngleSharp`. Remover as dependências compartilhadas com o ASF da sua compilação não é um requisito absoluto para o funcionamento do seu plugin, mas fazê-lo reduzirá drasticamente o uso de memória e o tamanho do seu plugin, além de melhorar o desempenho, pois o ASF compartilhará suas próprias dependências com você e carregará apenas as bibliotecas que ele não reconhecer.

De modo geral, é uma prática recomendada incluir apenas as bibliotecas que o ASF não inclui ou que estão em uma versão incorreta/incompatível. Exemplos disso seriam, obviamente, `YourPluginName.dll`, mas também `Discord.Net.dll`, caso você tenha decidido depender dela, já que o ASF não a inclui. Incluir bibliotecas que são compartilhadas com o ASF ainda pode fazer sentido se você quiser garantir a compatibilidade da API (por exemplo, ter certeza de que a versão de `AngleSharp` usada no seu plugin será sempre a versão `X`, e não a fornecida pelo ASF). No entanto, isso tem um custo em termos de maior uso de memória/tamanho e pior desempenho, portanto, deve ser avaliado com cuidado.

Se você sabe que a dependência necessária já está incluída no ASF, pode marcá-la com `IncludeAssets="compile"`, como mostrado no exemplo de `csproj` acima. Isso instruirá o compilador a não publicar a biblioteca referenciada, pois o ASF já a inclui. Da mesma forma, note que referenciamos o projeto do ASF com `ExcludeAssets="all" Private="false"`, o que funciona de maneira semelhante — instruindo o compilador a não gerar nenhum arquivo do ASF, pois o usuário já os possui. Isso se aplica apenas ao referenciar o projeto do ASF, pois, se você referenciar uma biblioteca `dll`, não estará gerando arquivos do ASF como parte do seu plugin.

Caso você tenha dúvidas sobre a declaração acima e não saiba ao certo, verifique quais bibliotecas `dll` estão incluídas no pacote `ASF-generic.zip` e certifique-se de que seu plugin inclua apenas aquelas que ainda não fazem parte dele. Para a maioria dos plugins mais simples, isso será apenas `YourPluginName.dll`. Se encontrar algum problema em tempo de execução relacionado a determinadas bibliotecas, inclua também as bibliotecas afetadas. Se nada mais funcionar, você sempre pode optar por incluir tudo.

---

### Dependências nativas

As dependências nativas são geradas como parte de compilações específicas do Sistema Operacional, pois não há tempo de execução .NET disponível no host e o ASF está sendo executado por meio de seu próprio tempo de execução .NET que é agrupado como parte da compilação específica do Sistema Operacional. Para minimizar o tamanho da compilação, o ASF corta suas dependências nativas para incluir apenas o código que pode ser alcançado dentro do programa, o que efetivamente reduz as partes não utilizadas do tempo de execução. Para minimizar o tamanho da compilação, o ASF corta suas dependências nativas para incluir apenas o código que pode ser alcançado dentro do programa, o que efetivamente reduz as partes não utilizadas do tempo de execução. Isso pode criar um problema potencial para o seu plugin caso você se depare com uma situação em que ele dependa de algum recurso do .NET que não é utilizado no ASF e, por isso, as compilações específicas de SO não consigam executá-lo corretamente, geralmente resultando em exceções como `System.MissingMethodException` ou `System.Reflection.ReflectionTypeLoadException`. À medida que seu plugin cresce em tamanho e se torna cada vez mais complexo, mais cedo ou mais tarde você acabará encontrando limitações não cobertas pela nossa compilação específica para cada sistema operacional.

Isto nunca será um problema com as compilações genéricas, pois elas nunca lidam com dependências nativas (pois elas têm um tempo de execução completo no host, executando o ASF). Por isso, é uma prática recomendada **usar seu plugin exclusivamente em compilações genéricas**, embora isso tenha a desvantagem de excluir usuários que executam compilações específicas para determinados sistemas operacionais do ASF. Se você estiver se perguntando se seu problema está relacionado a dependências nativas, você pode usar este método para verificação, carregue seu plugin em uma compilação genérica do ASF e veja se ele funciona. Caso positivo, tudo estará certo com as dependencias do pugin e os problemas estão sendo causados pelas dependencias nativas.

Infelizmente, tivemos que fazer uma escolha difícil entre publicar todo o tempo de execução como parte de nossas compilações específicas para SO, ou cortar os recursos não utilizados deixanto a compilação final com cerca de 80 MB a menos em comparação a versão completa. Nós escolhemos a segunda opção, e infelizmente é impossível para você incluir os recursos que faltam no tempo de execução juntamente com seu plugin. Se seu projeto precisar de acesso a recursos de tempo de execução que não estão incluídos, será necessário adicionar o runtime completo do .NET do qual você depende, o que significa executar seu plugin na versão `generic` do ASF. Você não pode executar seu plugin em compilações específicas para SO, já que essas compilações simplesmente não possuem um recurso do tempo de execução que você precisa, e o tempo de execução do .NET runtime é incapaz de "mesclar" a dependência nativa que você poderia ter fornecido à nossa. Talvez isso melhore um dia no futuro, mas no momento simplesmente não é possível.

As compilações específicas de SO do ASF incluem o mínimo de funcionalidades adicionais que são necessárias para executar nossos plugins oficiais. Ele também expande ligeiramente a quantidade de dependências adicionais que podem ser necessárias para os plug-ins mais básicos. Portanto, nem todos os plugins vão precisar se preocupar com dependências nativas - apenas aqueles que vão além do que o ASF e nossos plugins oficiais precisam diretamente. Isso é feito como em extra, porque se nós precisarmos incluir dependências adicionais para nossos propósitos, podemos simplesmente fornecê-las diretamente ao ASF, tornando-as disponíveis para você também. Infelizmente, isso nem sempre é suficiente, e conforme o seu plugin fica maior e mais complexo, a probabilidade de se deparar com funcionalidades problemáticas aumenta. Portanto, geralmente recomendamos que você execute seus plugins personalizados exclusivamente na versão `generic` do ASF. Você ainda pode verificar manualmente que a compilação específica do Sistema Operacional do ASF tem tudo o que o seu plugin necessita para funcionar - mas como isso muda tanto para suas atualizações como para as nossas, poderá ser um desafio mantê-lo.

Às vezes, pode ser possível "contornar" funcionalidades ausentes usando opções alternativas ou reimplementando-as no seu plugin. No entanto, isso nem sempre é possível ou viável, especialmente caso a funcionalidade ausente venha de dependências de terceiros que você inclua como parte do seu plugin. Você sempre pode tentar executar seu plugin em uma build específica do Sistema Operacional e tentar fazê-lo funcionar, mas isso pode se tornar um transtorno a longo prazo, especialmente caso você queira que seu código simplesmente funcione, em vez de ficar lutando com a superfície do ASF.

---

## Atualizações automáticas

O ASF oferece duas interfaces para a implementação de atualizações automáticas em seu plugin:

- `IGitHubPluginUpdates`, que oferece uma maneira fácil de implementar atualizações baseadas no GitHub, semelhante ao mecanismo de atualização geral do ASF.
- `IPluginUpdates`, que fornece uma funcionalidade de nível mais baixo, permitindo a implementação de um mecanismo de atualização personalizado caso você precise de algo mais complexo.

---

### **[`IGithubPluginUpdates`](https://github.com/JustArchiNET/ArchiSteamFarm/blob/main/ArchiSteamFarm/Plugins/Interfaces/IGitHubPluginUpdates.cs)**

A lista mínima de itens necessários para que as atualizações funcionem:

- Você precisa declarar o `RepositoryName`, que define de onde as atualizações serão obtidas.
- Você precisa utilizar as tags e os lançamentos fornecidos pelo GitHub. A tag deve estar em um formato que possa ser interpretado como uma versão do plugin, por exemplo, `1.0.0.0`.
- A propriedade `Version` do plugin deve corresponder à tag da qual ela se origina. Isso significa que o binário disponível sob a tag `1.2.3.4` deve se apresentar como `1.2.3.4`.
- Cada tag deve ter um lançamento correspondente disponível no GitHub, com um arquivo zip como recurso de lançamento que inclua os arquivos binários do seu plugin. Os arquivos binários que contêm suas classes `IPlugin` devem estar disponíveis no diretório raiz dentro do arquivo zip.

Isso permitirá que o mecanismo do ASF:

- Resolva a `Version` atual do seu plugin, por exemplo, `1.0.1`.
- Use a API do GitHub para buscar a última `tag` disponível no repositório definido em `RepositoryName`, por exemplo, `1.0.2`.
- Determine que `1.0.2` > `1.0.1`, verifique o lançamento da tag `1.0.2` para encontrar o arquivo `.zip` com a atualização do plugin.
- Baixe esse arquivo `.zip`, extraia-o e coloque seu conteúdo no diretório que anteriormente continha o `YourPlugin.dll`.
- Reinicie o ASF para carregar seu plugin na versão `1.0.2`.

Notas adicionais:

- As atualizações de plugin podem exigir valores de configuração apropriados no ASF, especificamente **[`PluginsUpdateMode`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#pluginsupdatemode)** e/ou **[`PluginsUpdateList`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#pluginsupdatelist)**.
- Nosso template de plugin já inclui tudo o que você precisa, basta **[renomear](https://github.com/JustArchiNET/ASF-PluginTemplate?tab=readme-ov-file#renaming-myawesomeplugin)** o plugin para os valores corretos, e ele funcionará imediatamente.
- Você pode usar uma combinação da versão mais recente de lançamento e também dos pré-lançamentos, eles serão utilizados conforme o `UpdateChannel` que o usuário definiu.
- Existe a propriedade booleana `CanUpdate` que você pode sobrescrever para desativar ou ativar as atualizações do plugin do seu lado, por exemplo, caso queira pular atualizações temporariamente ou por qualquer outro motivo.
- É possível ter vários arquivos zip no lançamento caso você queira direcionar diferentes versões do ASF. Veja as **[observações](https://github.com/JustArchiNET/ArchiSteamFarm/blob/main/ArchiSteamFarm/Plugins/Interfaces/IGitHubPluginUpdates.cs#L79-L106)** de `GetTargetReleaseAsset()`. Por exemplo, você pode ter `MyPlugin-V6-0.zip` assim como `MyPlugin.zip`, o que fará com que o ASF na versão `V6.0.x.x` escolha o primeiro, enquanto todas as outras versões do ASF usarão o segundo.
- Caso o acima não seja suficiente para você e você precise de uma lógica personalizada para escolher o arquivo `.zip` correto, é possível sobrescrever a função `GetTargetReleaseAsset()` e fornecer o artefato que o ASF deve utilizar.
- Caso seu plugin precise realizar algum trabalho extra antes ou depois da atualização, você pode sobrescrever os métodos `OnPluginUpdateProceeding()` e/ou `OnPluginUpdateFinished()`.

---

### **[`IPluginUpdates`](https://github.com/JustArchiNET/ArchiSteamFarm/blob/main/ArchiSteamFarm/Plugins/Interfaces/IPluginUpdates.cs)**

Essa interface permite que você implemente uma lógica personalizada para as atualizações caso, por qualquer motivo, o `IGithubPluginUpdates` não seja suficiente para você, por exemplo, porque você utiliza tags que não podem ser interpretadas como versões ou porque você não está utilizando a plataforma do GitHub.

Existe uma única função `GetTargetReleaseURL()` que você pode sobrescrever, a qual espera de você uma `Uri?` do local de atualização do plugin desejado. O ASF fornece algumas variáveis principais relacionadas ao contexto em que a função foi chamada, mas fora isso, você é responsável por implementar tudo o que for necessário nessa função e fornecer ao ASF a URL que deve ser utilizada ou `null` caso a atualização não esteja disponível.

Fora isso, é semelhante às atualizações via GitHub, onde a URL deve apontar para um arquivo `.zip` com os arquivos binários disponíveis no diretório raiz. Você também tem disponíveis os métodos `OnPluginUpdateProceeding()` e `OnPluginUpdateFinished()`.
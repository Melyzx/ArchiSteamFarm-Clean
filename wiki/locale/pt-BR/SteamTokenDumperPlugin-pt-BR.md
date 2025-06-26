# SteamTokenDumperPlugin

`SteamTokenDumperPlugin` é o **[plug-in](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-pt-BR)** oficial do ASF desenvolvido por nós, que permite que você contribua com o projeto **[SteamDB](https://steamdb.info)** compartilhando tokens de pacote, tokens de app e chaves de depósito que sua conta Steam tem acesso. As informações na integra sobre o porque o SteamDB precisa de tais dados podem ser encontrado na página de **[Depósito de Tokens](https://steamdb.info/tokendumper)** do SteamDB. Os dados enviados não incluem qualquer informação potencialmente sensível, e não possui qualquer risco à sua segurança ou privacidade, como especificado acima.

---

## Ativando o Plugin

O ASF vem com p `SteamTokenDumperPlugin` incluso no pacote de lançamento, mas tal plugin vem desativado por padrão. Você pode ativar o plugin setando `SteamTokenDumperPluginEnabled` nas configurações globais do ASF para `true`, na syntax JSON:

```json
{
  "SteamTokenDumperPluginEnabled": true
}
```

Ao abrir o programa do ASF, o plugin lhe dirá se ele foi ativado com sucesso por meio do mecanismo padrão de registro do ASF. Você também pode ativar o plugin através do nosso gerador de configuração baseado na web.

---

## Detalhes técnicos

Ao habilitar, o plugin usará os bots que você está executando no ASF para coletar dados sob a forma de pacotes de tokens, tokens de aplicativos e códigos de armazenamento aos quais seus bots tem acesso. O módulo de coleta de dados inclui rotinas ativas e passivas que devem minimizar a sobrecarga adicional causada pela coleta de dados.

Para atender ao caso de uso planejado, além de coleta de dados explicados acima, rotina de submissão é inicializada como responsável por determinar quais dados devem ser enviados ao SteamDB periodicamente. Essa rotina disparará em até `1` horas após o início do ASF, e se repetirá a cada `24` horas. O plugin fará o seu melhor para minimizar a quantidade de dados que precisam ser enviados, portanto é possível que alguns dados que o plugin colete sejam marcados como inúteis, e, portanto, ignorados (por exemplo, atualização do aplicativo que não altera o token de acesso).

O plugin usa um banco de dados de cache persistente salvo no local `config/SteamTokenDumper.cache`, que serve uma finalidade similar ao `config/ASF.db` para o ASF. O arquivo é usado para gravar os dados reunidos e enviados e minimizar a quantidade de trabalho que precisa ser feito através de diferentes execuções do ASF. Remoção do arquivo faz com que o processo seja reiniciado do zero, o que deve ser evitado se possível.

---

## Dados

O ASF inclui o colaborador `steamID` na solicitação, o que é determinado como `SteamOwnerID` que você definiu no ASF, ou caso não tenha, o Steam ID do bot, que possui a maioria das licenças. O contribuidor anunciado pode receber algumas vantagens adicionais do SteamDB para ajuda contínua (e. . rank do doador no site), mas isso fica inteiramente a critério do SteamDB.

De qualquer forma, a equipe do SteamDB gostaria de agradecer antecipadamente pela sua ajuda. Os dados enviados permitem que o SteamDB opere, em particular para rastrear informações sobre pacotes, aplicativos e depósitos, o que não seria mais possível sem a sua ajuda.

---

## Comando

O plug-in STD vem com um comando extra do ASF, `std [Bots]`, que permite a você acionar a atualização e a submissão dos bots selecionados sob demanda. Usar o comando não requer uma configuração ativada, o que permite que você pule a coleta automática e o envio, e controle o processo por você mesmo manualmente. Naturalmente, também pode ser executado por uma configuração habilitada, o que simplesmente acionará os procedimentos normais de coleta e envio antes do esperado.

Recomendamos `!std ASF` para acionar a atualização de todos os bots disponíveis. No entanto, você também pode acioná-lo para os selecionados, se você quiser.

---

## Configuração avançada

Nosso plug-in suporta configuração avançada, o que pode ser útil para pessoas que gostariam de ajustar os dados internos à sua preferência.

A configuração avançada tem a seguinte estrutura localizada dentro do `ASF.json`:

```json
{
  "SteamTokenDumperPlugin": {
    "Enabled": false,
    "SecretAppIDs": [],
    "SecretDepotIDs": [],
    "SecretPackageIDs": [],
    "SkipAutoGrantPackages": true
  }
}
```

Todas as opções são explicadas abaixo:

### `Enabled`

Tipo `bool` com valor padrão `false`. Essa propriedade atua como o `SteamTokenDumperPluginEnabled`, propriedade de raiz explicada acima, e pode ser usada em vez disso, dedicado a pessoas que prefeririam ter toda a configuração relacionada ao plugin em sua própria estrutura (então, provavelmente quem já usa outras propriedades avançadas explicadas abaixo).

---

### `SecretAppIDs`

Tipo `ImmutableHashSet<uint>` com valor padrão vazio. Esta propriedade específica os `appIDs` que o plug-in não resolverá e se já estiverem resolvidos, não enviará o token. Esta propriedade pode ser útil para pessoas com acesso a informações potencialmente sensíveis sobre itens não publicados, especialmente os desenvolvedores, editores ou testadores de betas fechados.

---

### `SecretDepotIDs`

Tipo `ImmutableHashSet<uint>` com valor padrão vazio. Esta propriedade específica os `depotIDs` que o plug-in não resolverá e se já estiverem resolvidos, não enviará a chave. Esta propriedade pode ser útil para pessoas com acesso a informações potencialmente sensíveis sobre itens não publicados, especialmente os desenvolvedores, editores ou testadores de betas fechados.

---

### `SecretPackageIDs`

Tipo `ImmutableHashSet<uint>` com valor padrão vazio. Essa propriedade especifica os `packageIDs` (também conhecidos como `subIDs`) que o plugin não resolverá e, caso já estejam resolvidos, não enviará o token para eles. Esta propriedade pode ser útil para pessoas com acesso a informações potencialmente sensíveis sobre itens não publicados, especialmente os desenvolvedores, editores ou testadores de betas fechados.

---

### `SkipAutoGrantPackages`

Tipo `bool` com valor padrão `true`. Essa propriedade funciona de maneira muito semelhante a `SecretPackageIDs` e, quando ativada, fará com que pacotes com `EPaymentMethod` de `AutoGrant` sejam ignorados durante a rotina de resolução explicada abaixo. O método de pagamento `AutoGrant` é usado pelo **[Steamworks](https://partner.steamgames.com)** para conceder automaticamente pacotes em contas de desenvolvedores. Embora isso não seja tão explícito quanto outras opções `Secret` e, portanto, não garanta nada (já que você pode ter outros pacotes além de `AutoGrant` que ainda não deseja enviar), deve ser suficiente para ignorar a maioria, se não todos, dos pacotes secretos. Essa opção é ativa por padrão, como as pessoas que realmente tem acesso a pacotes `AutoGrant` quase nunca vão querer vazar essas informações ao publico, então usando o valor de `falso` é bem pessoal.

---

## Explicações adicionais

No nível raiz, cada conta do Steam possui um conjunto de pacotes (licenças, assinaturas), que são classificados pelo seu `packageID` (também conhecido como `subID`). Cada pacote pode conter vários aplicativos classificados por seu `appID`. Cada aplicativo pode então incluir vários depots classificados pelo seu `depotID`.

```text
├── sub/124923
│     ├── app/292030
│     │     ├── depot/292031
│     │     ├── depot/378648
│     │     └── ...
│     ├── app/378649
│     └── ...
└── ...
```

Nosso plugin inclui duas rotinas que levam em conta os itens ignorados: a rotina de resolução e a rotina de envio.

A rotina de resolução é responsável por resolver a árvore que você pode ver acima. Ao colocar pacotes, aplicativos ou depots na lista de bloqueio antecipadamente, você cortará efetivamente a árvore no ponto do ramo ou folha bloqueada, sem a necessidade adicional de especificar as partes restantes. No nosso exemplo acima, se o pacote `124923` fosse ignorado, seja por `SecretPackageIDs` ou `SkipAutoGrantPackages`, e ele fosse o único pacote que você possui vinculado ao appID `292030`, então o appID `292030` também não seria resolvido. Por definição, se não houvesse outros aplicativos resolvidos vinculados aos depots `292031` e `378648`, então eles também não seriam resolvidos. No entanto, lembre-se de que, se o plugin já tiver resolvido a árvore, isso efetivamente apenas impedirá que determinado item seja atualizado (por exemplo, novos aplicativos adicionados). Isso não fará com que o plugin "esqueça" os itens existentes que já foram resolvidos (por exemplo, aplicativos encontrados nesse pacote antes de você decidir bloqueá-lo). Se você acabou de ativar algumas opções de ignorar e deseja garantir que o ASF não percorra a árvore já resolvida, pode considerar remover uma única vez o arquivo `config/SteamTokenDumper.cache`, onde o plugin armazena seu cache.

A rotina de envio é responsável por enviar tokens de pacotes, tokens de aplicativos e chaves de depots de itens já resolvidos (pela rotina de resolução acima). Aqui, sua lista de bloqueio tem efeito imediato, pois, mesmo que o plugin já tenha resolvido a informação, a rotina de envio não a enviará para o SteamDB se ela estiver na lista de bloqueio, independentemente de ter sido resolvida ou não. No entanto, lembre-se de que não estamos mais falando sobre a árvore neste momento, a rotina de envio não sabe se as informações sobre o aplicativo vêm deste ou daquele pacote, portanto, apenas ignore informações sobre itens específicos da lista negra, independentemente da relação em que estão com os outros.

Para a maioria dos desenvolvedores e publicadores, ativar `SkipAutoGrantPackages` deve ser suficiente, potencialmente complementado por `SecretPackageIDs`, pois isso corta efetivamente a árvore no ramo inicial e garante que os aplicativos e depots incluídos posteriormente não sejam enviados, desde que não haja outro pacote vinculado ao mesmo aplicativo. Caso queira ter certeza absoluta, além disso, você também pode usar `SecretAppIDs`, que impedirá a resolução do aplicativo mesmo que existam outras licenças não bloqueadas vinculadas a ele. O uso de `SecretDepotIDs` não deve ser necessário, a menos que você tenha uma necessidade específica (como ignorar apenas um determinado depot enquanto ainda envia informações sobre pacotes e aplicativos) ou queira adicionar mais uma camada para garantir segurança extra.
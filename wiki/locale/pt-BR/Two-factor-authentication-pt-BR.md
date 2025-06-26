# Autenticação em duas etapas

O Steam inclui um sistema de autenticação em duas etapas que exige informações extras para diversas atividades relacionadas à conta. Você pode ler mais sobre isso **[aqui](https://help.steampowered.com/faqs/view/2E6E-A02C-5581-8904)** e **[aqui](https://help.steampowered.com/faqs/view/34A1-EA3F-83ED-54AB)**. Esta página considera o sistema de autenticação em duas etapas (2FA), assim como nossa solução que se integra a ele, chamada ASF 2FA.

---

# Lógica do ASF

Independentemente de você usar o ASF 2FA ou não, o ASF inclui a lógica adequada e está totalmente ciente das contas protegidas por 2FA no Steam. Ele vai te pedir pelos dados necessários quando for preciso (durante o login, por exemplo). Embora você possa fornecer essas informações manualmente, certas funcionalidades do ASF (como `MatchActively`) exigem que o ASF 2FA esteja operante na sua conta bot, permitindo que ele responda automaticamente às solicitações de 2FA sempre que necessário pelo ASF.

---

# ASF 2FA

O ASF 2FA é um módulo integrado responsável por fornecer recursos de autenticação em duas etapas (2FA) ao processo do ASF, como a geração de tokens e aceitar confirmações. Ele pode funcionar tanto em modo autônomo quanto duplicando os detalhes do seu autenticador existente (para que você possa usar seu autenticador atual e o ASF 2FA ao mesmo tempo).

Você pode verificar se sua conta bot já usa o ASF 2FA executando o **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR) **`2fa`. Sem configurar o ASF 2FA, todos os comandos padrão `2fa` serão inoperantes, o que significa que seu bot não estará disponível para funcionalidades avançadas do ASF que exigem que o módulo esteja operante.

---

# Recomendações

Existem muitas maneiras de tornar o ASF 2FA operativo, aqui incluímos nossas recomendações com base na sua situação atual:

- Caso você já esteja usando um aplicativo não oficial de terceiros que permite extrair os detalhes do 2FA com facilidade, basta **[importar](#importar)** esses detalhes para o ASF.
- Caso você esteja está usando o aplicativo oficial e não se importa em redefinir suas credenciais de 2FA, a melhor maneira é desativar o 2FA e, em seguida, **[criar](#criação)** novas credenciais de 2FA usando o **[autenticador conjunto](#Autenticador-conjunto)**, o que permitirá que você use o aplicativo oficial e o ASF 2FA. Esse método não requer acesso root ou conhecimento avançado, basta seguir as instruções aqui descritas, e é, sem dúvida, uma opção superior para esse cenário.
- Caso você esteja usando o aplicativo oficial e não deseja recriar suas credenciais de 2FA, suas opções são muito limitadas. Geralmente, você precisará de acesso root e realizar procedimentos adicionais para **[importar](#importar)** esses detalhes, e mesmo assim, pode ser impossível.
- Caso você ainda não esteja usando 2FA e não se importe, recomendamos que use o ASF 2FA com o **[autenticador autônomo](#autenticador-autônomo)** ou o **[autenticador conjunto](#autenticador-conjunto)** com o aplicativo oficial (mesmo como mencionado acima).

Abaixo, discutimos todas as opções possíveis e os métodos que conhecemos.

---

## Criação

O ASF vem com um **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-pt-BR)** oficial chamado `MobileAuthenticator`, que estende ainda mais o ASF 2FA, permitindo que você vincule um autenticador 2FA completamente novo. Isso pode ser útil no caso de você não conseguir ou não desejar usar outras ferramentas e não se importar de que o ASF 2FA se torne o seu autenticador principal (e talvez único). O processo de criação também é usado no método do autenticador conjunto. Naturalmente, nesse cenário, seu autenticador pode coexistir em dois lugares ao mesmo tempo – ambos gerarão os mesmos códigos e ambos poderão confirmar as mesmas confirmações.

### Etapas em comum para ambos os cenários

Não importa se você planeja usar o ASF como autenticador autônomo ou conjunto, você precisará realizar os seguintes passos de inicialização:

1. Crie um bot ASF para a conta desejada, inicie-o e faça login, o que provavelmente você já fez.
2. Adicione um número de telefone funcional e operacional à conta **[aqui](https://store.steampowered.com/phone/manage)** para ser usado pelo bot. Isso permitirá que você receba o código via SMS e possibilitará a recuperação, se necessário. Este passo não é obrigatório em todos os cenários, porém recomendamos realizá-lo, a menos que você tenha certeza do que está fazendo.
3. Certifique-se de que você ainda não está usando o 2FA para sua conta, se caso estiver, desative-o primeiro. Isso **colocará** sua conta em retenção temporária de trocas, e não há como evitar isso. Somente o processo de **[importação](#importar)** pode pular essa etapa.
4. Execute o comando `2fainit [Bot]`, substituindo `[Bot]` pelo nome do seu bot.

Pressupondo que você tenha recebido uma resposta bem-sucedida, as duas seguintes coisas aconteceram:

- Um novo arquivo `<Bot>.maFile.PENDING` foi gerado pelo ASF no seu diretório `config`.
- Um SMS foi enviado do Steam para o número de telefone que você atribuiu à conta acima. Caso você ainda não definiu um número de telefone, então um e-mail será enviado em vez disso para o endereço de e-mail da conta.

Os detalhes do autenticador ainda não estão operacionais, no entanto, você pode revisar o arquivo gerado, caso desejar. Caso você queira garantir uma segurança adicional, pode, por exemplo, já anotar o código de revogação. Os próximos passos dependerão do cenário escolhido.

### Autenticador autônomo

Caso você quiser usar o ASF como seu autenticador principal (ou até mesmo único), agora você precisa realizar o passo final:

5. Execute o comando `2fafinalize [Bot] <ActivationCode>`, substituindo `[Bot]` com o seu nome do bot e `<ActivationCode>` com o código que você recebeu através do SMS ou e-mail na etapa anterior.

### Autenticador conjunto

Caso você queira ter o mesmo autenticador tanto no ASF quanto no aplicativo móvel oficial do Steam, agora você precisa realizar os próximos passos, que são mais complicados:

5. Ignore o código SMS ou e-mail que você recebeu no passo anterior.
6. Instale o aplicativo móvel do Steam caso você ainda não o tenha instalado e inicie-o. Vá até a aba do Steam Guard e adicione um novo autenticador seguindo as etapas do próprio aplicativo.
7. Após seu autenticador no aplicativo móvel ser adicionado e estiver funcionando, retorne ao ASF. Agora, em vez de finalizar, só precisamos informar ao ASF que o aplicativo móvel já ativou os detalhes previamente gerados:
 - Aguarde até que o próximo código 2FA seja exibido no aplicativo móvel do Steam e use o comando `2fafinalized [Bot] <2FACodeFromApp>`, substituindo `[Bot]` pelo nome do seu bot e `<2FACodeFromApp>` pelo código que você vê atualmente no aplicativo móvel do Steam. Caso o código gerado pelo ASF e o código que você forneceu forem iguais, o ASF assumirá que o autenticador foi adicionado corretamente e prosseguirá com a importação do seu novo autenticador.
 - Recomendamos fortemente que faça a etapa acima para garantir que suas credenciais sejam válidas. No entanto, se você não deseja (ou não pode) verificar se os códigos são os mesmos e sabe o que está fazendo, você pode usar em vez disso o comando `2fafinalizedforce [Bot]`, substituindo `[Bot]` pelo nome do seu bot. O ASF assumirá que o autenticador foi adicionado corretamente e prosseguirá com a importação do seu autenticador recém-criado. Esteja ciente de que, nesse modo, o ASF não consegue verificar se os códigos correspondem, o que significa que você pode importar credenciais inválidas (não ativadas).

### Após a finalização

Assumindo que tudo funcionou corretamente, o arquivo `<Bot>.maFile.PENDING` gerado anteriormente foi renomeado para `<Bot>.maFile.NEW`. Isso indica que suas credenciais de 2FA agora estão válidas e ativas. Recomendamos que você mova esse arquivo para **um local seguro e protegido**. Além disso, caso tenha decidido usar o autenticador autônomo, recomendamos que você abra o arquivo no editor de sua preferência e anote o `revocation_code`, que permitirá revogar o autenticador, como o nome sugere, caso você o perca. No método de autenticador conjunto, você já deve ter feito isso no aplicativo móvel do Steam, mas sinta-se à vontade para fazer o mesmo, caso precise.

Em relação aos detalhes técnicos, o `maFile` gerado inclui todas as informações que recebemos do servidor Steam durante a vinculação do autenticador, além do campo `device_id`, que pode ser necessário para outros autenticadores (de terceiros), caso você decida importar esse `maFile` para eles.

O ASF importa automaticamente o seu autenticador assim que o procedimento é concluído, e, portanto, o comando `2fa` e outros comandos relacionados agora devem estar operacionais para a conta bot à qual você vinculou o autenticador. Recomendamos que você verifique isso.

---

## Importar

O processo de importação requer um autenticador já vinculado e operacional que seja suportado pelo ASF. Temos instruções para algumas fontes oficiais e não oficiais de 2FA, além de um método manual que permite que você forneça as credenciais necessárias por conta própria. Observe que essas instruções devem ser usadas **somente** caso você já estiver utilizando a solução mencionada — como o processo envolve aplicativos e ferramentas de terceiros, **não recomendamos o uso delas**, e estamos mencionando isso exclusivamente para pessoas que já decidiram usá-las e gostariam de importar os detalhes gerados para o ASF 2FA.

Todos os guias a seguir exigem que você já tenha um autenticador **funcionando e operacional** sendo usado com dada ferramenta/aplicativo. O ASF 2FA não funcionará corretamente se você importar dados inválidos, portanto, certifique-se de que seu autenticador esteja funcionando corretamente antes de tentar importá-lo. Isso inclui testar e verificar que as seguintes funções de autenticador funcionam corretamente:
- Você pode gerar tokens e esses tokens são aceitos pela rede do Steam (é possível iniciar a sessão com eles)
- Você pode buscar confirmações e elas estão chegando no seu autenticador móvel
- Você pode reagir a essas confirmações, e elas são devidamente reconhecidas pela rede Steam como confirmadas ou rejeitadas

Certifique-se de que o seu autenticador está funcionando verificando se as ações mencionadas acima funcionam — se não funcionarem, elas também não funcionarão no ASF.

---

### Dispositivo Android

Em geral, para importar o autenticador do seu Android você vai precisar de acesso **[root](https://pt.wikipedia.org/wiki/Root_no_Android)**. As instruções abaixo exigem de você um conhecimento razoável no mundo da modificação do Android. Certamente, não vamos explicar cada passo aqui. Visite o **[XDA](https://xdaforums.com)** e outros recursos para informações/ajuda adicionais com o que está descrito abaixo.

Assumindo que você tem o aplicativo oficial do **[Steam](https://play.google.com/store/apps/details?id=com.valvesoftware.android.steam.community)** funcionando e operacional (requer root no seu dispositivo):

- Instale o **[Magisk](https://topjohnwu.github.io/Magisk/install.html)** e ative o Zygisk nas configurações.
- Instale o **[LSPosed](https://github.com/LSPosed/LSPosed?tab=readme-ov-file#install)** (para o Zygisk) e certifique-se que funciona.
- Instale o módulo do LSPosed **[SteamGuardExtractor](https://github.com/hax0r31337/SteamGuardExtractor?tab=readme-ov-file#usage)** e ative-o nas configurações do LSPosed.
- Force o encerramento do aplicativo Steam e, em seguida, abra-o. Uma **[janela com os detalhes extraídos](https://github.com/JustArchiNET/ArchiSteamFarm/assets/1069029/ab5ab71e-d664-4e49-9eb4-9f4d9ba32aa2)** deve aparecer, e você deve clicar em copiar.

Agora que você extraiu com sucesso os detalhes necessários, desative o módulo para evitar que a janela apareça toda vez. Em seguida, copie os valores de `shared_secret` e `identity_secret` da conta que você pretende adicionar ao ASF 2FA para um novo arquivo de texto com a seguinte estrutura:

```json
{
  "shared_secret": "STRING",
  "identity_secret": "STRING"
}
```

Substitua cada valor `STRING` com a chave privada apropriada dos detalhes extraídos. Após fazer isso, renomeie o arquivo para `BotName.maFile`, onde `BotName` é o nome do seu bot ao qual está adicionando o ASF 2FA, e coloque-o no diretório `config` do ASF se ainda não o fez.

Inicie o ASF, que deverá detectar seu arquivo e importá-lo. Caso você tenha importado o arquivo correto com os 'secrets' válidos, tudo deve funcionar corretamente, o que você pode verificar usando os comandos `2fa`. Caso tenha cometido um erro, você pode sempre remover `Bot.db` e começar de novo, se necessário.

---

### SteamDesktopAuthenticator

Se você estiver usando seu autenticador no SDA, você deve ter notado que há um arquivo chamado `steamID.maFile` na pasta `maFiles`. Certifique-se de que o `maFile` esteja em formato não criptografado, pois o ASF não pode descriptografar os arquivos SDA - o conteúdo do arquivo não criptografado deve começar com o caractere `{` e terminar com o caractere `}`. Se necessário, você pode remover a criptografia nas configurações do SDA primeiro, e então, ativar novamente quando você terminar. Assim que o arquivo estiver em formato não criptografado, copie-o para o diretório `config` do ASF.

Agora você pode renomear `steamID.maFile` para `BotName.maFile` no diretório de configuração do ASF, onde `BotName` é o nome do seu bot ao qual você está adicionando o ASF 2FA. Como alternativa, você pode deixá-lo como está, o ASF vai selecioná-lo automaticamente após o login. Renomear o arquivo ajuda o ASF, tornando possível usar o ASF 2FA antes de fazer login. Se você não fizer isso, o arquivo só poderá ser reconhecido após o ASF fazer login com sucesso (pois o ASF não sabe o `steamID` de sua conta antes de efetuar o login).

Inicie o ASF, que deverá detectar seu arquivo e importá-lo. Caso você tenha importado o arquivo correto com os 'secrets' válidos, tudo deve funcionar corretamente, o que você pode verificar usando os comandos `2fa`. Se você cometeu algum erro você pode remover o arquivo `Bot.db` e recomeçar.

---

### WinAuth

Em primeiro lugar, crie um arquivo novo com o nome `NomeDoBot.maFile` na pasta config do ASF, onde `NomeDoBot` é o nome do bot para o qual você está adicionando o ASF 2FA. Se você colocar o nome errado, o arquivo não será selecionado pelo ASF.

Agora inicie o Winauth como de costume. Clique com o botão direito no ícone da steam e selecione "Show SteamGuard and Recovery Code". Então marque "Allow copy". Você vai observar uma estrutura JSON familiar para você na parte inferior da janela, começando com `{`. Copie todo o texto para o arquivo `NomeDoBot.maFile` que você criou na etapa anterior.

Inicie o ASF, que deverá detectar seu arquivo e importá-lo. Caso você tenha importado o arquivo correto com os 'secrets' válidos, tudo deve funcionar corretamente, o que você pode verificar usando os comandos `2fa`. Se você cometeu algum erro você pode remover o arquivo `Bot.db` e recomeçar.

---

### Manualmente

Se você for um usuário avançado, você pode gerar o arquivo maFile manualmente. Isso pode ser usado caso você queira importar o autenticador de outras fontes que não as descritas acima. Ele deve ter uma **[estrutura JSON válida](https://jsonlint.com)** de:

```json
{
  "shared_secret": "STRING",
  "identity_secret": "STRING"
}
```

Dados padrão do autenticador tem mais campos - eles são completamente ignorados pelo ASF durante a importação, já que não são necessários. Você não precisa removê-los — o ASF requer apenas um JSON válido com os 2 campos obrigatórios descritos acima e ignorará os campos adicionais (se houver). Claro, você precisa substituir o campo `STRING` no exemplo acima com valores válidos para sua conta. Cada `STRING` deve ser uma representação codificada em base64 dos bytes dos quais a chave privada apropriada é composta.

---

## Perguntas frequentes (FAQ)

### Como o ASF faz uso do módulo 2FA?

Se o ASF 2FA estiver disponível, o ASF o usará para confirmação automática de transações que estão sendo enviadas/aceitas pelo ASF. Ele também será capaz de gerar automaticamente tokens 2FA conforme a necessidade, por exemplo para logar. Além disso, o ASF 2FA permite comandos `2fa` para você usar.

---

### Como posso obter o token 2FA?

Você vai precisar de um token 2FA para acessar uma conta protegida pelo 2FA, que também inclui todas as contas com ASF 2FA. Caso você tenha decidido usar o autenticador autônomo, deve utilizar o comando `2fa <BotNames>` para gerar um token temporário para as instâncias de bot fornecidas. Em todos os outros cenários, recomendamos usar o autenticador original que você utilizou, embora você também possa usar o comando se for mais conveniente para você.

---

### Posso usar meu autenticador original depois de importá-lo como ASF 2FA?

Sim, seu autenticador original continua funcional e você pode usá-lo juntamente com o ASF 2FA. No entanto, lembre-se de que, se você invalidá-lo por qualquer método, as credenciais do ASF 2FA vinculadas também deixarão de ser válidas.

---

### Como remover o ASF 2FA?

Simplesmente pare o ASF e remova o arquivo `NomeDoBot.db` do bot com o ASF 2FA vinculado que deseja remover. Esta opção removerá o 2FA importado associado ao ASF, mas NÃO invalidará (desvinculará) o seu autenticador. Caso você queira invalidar seu autenticador, além de removê-lo do ASF (primeiramente), deve desvinculá-lo no autenticador original de sua escolha. Caso você não possa fazer isso por algum motivo, por exemplo, porque está usando o ASF 2FA no modo autônomo, use o código de revogação que você recebeu durante a configuração no site do Steam. Não é possível invalidar seu autenticador através do ASF.

---

### Vinculei o autenticador em um aplicativo de terceiros e, em seguida, importei para o ASF. Posso vinculá-lo novamente ao meu telefone?

**Não**. Fazer isso invalidará as credenciais importadas anteriormente e o seu ASF 2FA deixará de funcionar (pois os códigos gerados não serão mais aceitos pelo Steam). Primeiramente, decida onde você quer ter seu autenticador original ou de terceiros, e então importe-o como ASF 2FA.

---

### Usar o ASF 2FA é melhor do que um autenticador de terceiros configurado para aceitar todas as confirmações?

**Sim**, por vários motivos. Primeiro e mais importante - usar o ASF 2FA **significantemente** aumenta sua segurança, uma vez que o módulo 2FA do ASF assegura que o ASF só aceitará automaticamente suas próprias confirmações, então mesmo que um atacante solicite uma troca prejudicial, o ASF 2FA **não** aceitará tal troca, já que ela não foi gerada pelo ASF. Além da parte de segurança, usar o ASF 2FA também traz benefícios de desempenho/otimização, pois o ASF 2FA busca e aceita as confirmações imediatamente após serem geradas, e não por meio de consultas ineficientes a cada X minutos, como é feito por outras soluções. Não há razão para usar um autenticador de terceiros em vez do ASF 2FA se você planeja automatizar as confirmações geradas pelo ASF — é exatamente para isso que o ASF 2FA serve, e usá-lo não entra em conflito com o fato de você confirmar outras coisas no autenticador de sua escolha. Recomendamos fortemente usar o ASF 2FA para toda a atividade do ASF.
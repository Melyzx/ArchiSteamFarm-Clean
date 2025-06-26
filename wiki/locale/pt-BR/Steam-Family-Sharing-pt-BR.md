# Compartilhamento de Biblioteca Steam

O ASF suporta o Compartilhamento de Biblioteca Steam - tanto o sistema antigo quanto o novo. Para entender como o ASF funciona com isso, você deve primeiro ler sobre como o **[Compartilhamento de Biblioteca Steam funciona](https://store.steampowered.com/promotion/familysharing)**, que está disponível na loja Steam. Além disso, confira **[as notícias](https://store.steampowered.com/news/app/593110/view/4149575031735702628)** sobre o novo sistema do Compartilhamento de Biblioteca Steam, com o qual o ASF também é compatível.

---

## ASF

O suporte para o recurso de Compartilhamento de Biblioteca Steam no ASF é transparente, o que significa que não é necessário qualquer nova propriedade de configuração do bot ou do processo e funciona imediatamente como uma funcionalidade interna extra.

O ASF inclui uma lógica apropriada para estar ciente caso a biblioteca seja bloqueada por um usuário do compartilhamento de biblioteca, portanto ele não vai "expulsar" esses jogadores da sessão quando abrirem um jogo. O ASF agirá como se a conta principal tivesse ativado o bloqueio, portanto independente desse bloqueio ser mantido pelo seu cliente Steam ou por outro membro do compartilhamento de biblioteca, o ASF não tentará coletar, em vez disso, ele vai esperar que o bloqueio seja liberado. Isso está principalmente relacionado ao sistema antigo - o novo sistema permite que seus outros membros do compartilhamento familiar joguem jogos diferentes daqueles que o ASF está atualmente ocupando.

Além do mencionado acima, após fazer login, o ASF acessará seus sistemas de compartilhamento familiar (antigo e novo), dos quais extrairá os usuários (IDs do Steam) autorizados a usar sua biblioteca. Esses usuários recebem a permissão `FamilySharing` para usar **[comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)**, especialmente permitindo que eles usem o comando `pause~` na conta do bot que está compartilhando jogos com eles. Isso permite pausar o módulo de coleta de cartas automático para iniciar um jogo que pode ser compartilhado - isso também se aplica principalmente ao sistema antigo, mas ainda pode ser usado com o novo sistema caso o ASF esteja atualmente rodando um jogo que seus usuários desejam jogar.

Conectar ambas as funcionalidades descritas acima permite que seus amigos usem `pause~` para pausarem o processo de coleta de cartas, iniciem um jogo, joguem o tempo que desejarem e, após encerrarem o jogo, o processo de coleta de cartas será automaticamente retomado pelo ASF. É claro, usar o comando `pause~` não é necessário se o ASF não estiver rodando nenhum jogo, pois seus amigos podem abrir o jogo normalmente e a lógica descrita acima garante que eles não serão expulsos da seção.

---

## Limitações

A rede Steam adora enganar o ASF compartilhando atualizações de status falsas, que pode fazer o ASF acreditar que pode retomar o processo, expulsando seu amigo do jogo. Esse problema é exatamente o mesmo já explicado **[nessa seção](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ-pt-BR#o-asf-est%C3%A1-encerrando-minha-sess%C3%A3o-no-cliente-steam-enquanto-eu-jogo--voc%C3%AA-j%C3%A1-iniciou-a-sess%C3%A3o-em-outro-computador)**. Recomendamos exatamente as mesmas soluções, principalmente promovendo seus amigos com a permissão de `Operator` (ou acima) e pedindo para que eles usem os comandos `pause` e `resume`.
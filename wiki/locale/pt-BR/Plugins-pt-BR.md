# Plugins

O ASF inclui suporte para plugins personalizados que podem ser carregadas durante a execução. Os plugins permitem personalizar o comportamento do ASF, por exemplo, adicionando comandos personalizados, lógica de troca personalizada ou integração completa com serviços e APIs de terceiros.

Esta página descreve os plugins do ASF da perspectiva dos usuários – explicação, uso e afins. Caso você queira ver a perspectiva do desenvolvedor, vá para **[aqui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-development-pt-BR)**.

---

## Uso

O ASF carrega os plugins localizados na pasta `plugins`, dentro da pasta do ASF. É uma prática recomendada (que se torna obrigatória com as atualizações automáticas de plugins) manter um diretório dedicado para cada plugin que você deseja usar, que pode ser baseado no nome do plugin, como `MeuPlugin`. A estrutura então será `plugins/MeuPlugin`. Todos os arquivos binários do plugin devem ser colocados dentro dessa pasta, o ASF vai carregar e usar seu plugin corretamente após reiniciar.

Normalmente, os desenvolvedores de plugins publicam seus plugins em formato de arquivo `zip` com binários dentro, o que significa que você deve descompactar esse arquivo zip em seu próprio subdiretório dedicado dentro do diretório `plugins`.

Se o plugin foi carregado com êxito, você verá seu nome e versão no log. Você deve consultar os desenvolvedores dos plugins em caso de dúvidas, problemas ou forma de uso relacionado com os plugins que você decidiu usar.

Você pode encontrar alguns plugins em destaque na seção **[terceiros](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party-pt-br#plugins-para-o-asf)**.

**Por favor, note que alguns plugins podem ser mal intencionados**. Você deve sempre garantir que está usando plugins de desenvolvedores em quem confia, inclusive os da seção de terceiros acima. Os desenvolvedores do ASF não podem garantir os benefícios padrões do ASF (tal como a não presença de malwares ou ser livre de VAC ban) caso você decida usar qualquer plugin personalizado. Você precisa entender que os plugins têm controle total sobre o processo do ASF uma vez carregados, e devido a isso, não podemos oferecer suporte a configurações que utilizam plugins personalizados, uma vez que você não está mais executando o código original do ASF.

---

## Compatibilidade

Dependendo da complexidade, escopo e vários outros fatores do plugin, é totalmente possível que ele exija que você use a variante **[genérica](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up-pt-BR#configuração-genérica)** do ASF, em vez da normalmente recomendada variante **[específica para o Sistema Operacional](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up-pt-BR#instalador-para-sistemas-operacionais-específicos)**. Isso acontece porque a variante específica para o Sistema Operacional vem apenas com a funcionalidade principal necessária para o próprio ASF, e seu plugin pode exigir partes que estão fora do escopo principal do ASF, resultando em incompatibilidade com as versões reduzidas específicas para o Sistema Operacional.

Em geral, ao usar plugins de terceiros, recomendamos o uso da variante genérica do ASF para garantir a máxima compatibilidade. No entanto, nem todos os plugins podem exigir isso – consulte as informações do seu plugin para descobrir se é necessário usar a variante genérica do ASF ou não.
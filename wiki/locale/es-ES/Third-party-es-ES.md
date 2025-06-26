# Aplicaciones de terceros

Esta sección incluye varias adiciones de terceros escritas exclusivamente (o principalmente) para usarse junto con ASF. Varían desde plugins de ASF, pasando por aplicaciones web simples y bibliotecas internas para integración, terminando con bots para otras plataformas. Si deseas añadir algo a la lista, háznoslo saber en Discord o en nuestro grupo de Steam.

Por favor, ten en cuenta que los siguientes programas **no** son mantenidos por los desarrolladores de ASF y por consiguiente **no damos garantía para ninguno de ellos**, especialmente en términos de seguridad, certeza o cumplimiento de los Términos de Servicio de Steam. Esta lista solo se mantiene para referencia. Siempre debes asegurarte de que las utilidades de terceros que usarás son seguras y suficientemente legítimas para ti, ya que las usas bajo tu propio riesgo.

---

## Plugins de ASF

### **[CatPoweredPlugins](https://github.com/CatPoweredPlugins)** (**[Rudokhvist](https://github.com/Rudokhvist)**)

- **[ASFAchievementManager](https://github.com/CatPoweredPlugins/ASFAchievementManager)**, plugin para ASF que permite administrar los logros de Steam.
- **[BirthdayPlugin](https://github.com/CatPoweredPlugins/BirthdayPlugin)**, plugin de ASF para recibir felicitaciones de cumpleaños.
- **[CaseInsensitiveASF](https://github.com/CatPoweredPlugins/CaseInsensitiveASF)**, plugin de ASF para hacer que en los nombres de los bots no se distinga entre mayúsculas y minúsculas.
- **[CommandAliasPlugin](https://github.com/CatPoweredPlugins/CommandAliasPlugin)**, plugin de ASF para configurar alias de comandos personalizados para los comandos de ASF.
- **[CommandlessRedeem](https://github.com/CatPoweredPlugins/CommandlessRedeem)**, plugin de ASF para reimplementar la activación de claves sin un comando.
- **[ItemDispenser](https://github.com/CatPoweredPlugins/ItemDispenser)**, plugin de ASF para aceptar automáticamente solicitudes de intercambio para ciertos tipos de artículos.
- **[SelectiveLootAndTransferPlugin](https://github.com/CatPoweredPlugins/SelectiveLootAndTransferPlugin)**, plugin para ASF que proporciona un comando `transfer` avanzado para transferir artículos de Steam.

### **[Citrinate](https://github.com/Citrinate)**

- **[BoosterManager](https://github.com/Citrinate/BoosterManager)**, plugin para ASF que proporciona una interfaz sencilla de usar para convertir gemas en packs de refuerzo, así como otras funciones para administrar inventarios y anuncios del mercado.
- **[CS2Interface](https://github.com/Citrinate/CS2Interface)**, plugin para ASF que permite interactuar con Counter-Strike 2 usando **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-es-ES)**.
- **[FreePackages](https://github.com/Citrinate/FreePackages)**, plugin para ASF que busca paquetes gratuitos en Steam y los añade a tu cuenta.

### **[Vita](https://github.com/ezhevita)**

- **[FriendAccepter](https://github.com/ezhevita/FriendAccepter)**, plugin de ASF para aceptar automáticamente todas las solicitudes de amistad.
- **[GameRemover](https://github.com/ezhevita/GameRemover)**, plugin para ASF que implementa un comando para eliminar licencias de Steam para las instancias de bot seleccionadas.
- **[GetEmail](https://github.com/ezhevita/GetEmail)**, plugin para ASF que implementa un comando para obtener directamente de Steam la dirección de correo electrónico de las instancias de bot especificadas.
- **[ResetAPIKey](https://github.com/ezhevita/ResetAPIKey)**, plugin para ASF que implementa un comando para restablecer la clave API de las instancias de bot seleccionadas.

### Otros

- **[ASFEnhance](https://github.com/chr233/ASFEnhance)**, plugin para ASF que lo mejora con diversas nuevas funciones, especialmente comandos.
- **[ASFFreeGames](https://github.com/maxisoft/ASFFreeGames)**, plugin para ASF que permite obtener automáticamente juegos de Steam gratis publicados en Reddit.

---

## Integraciones

- **[ASFBot](https://github.com/dmcallejo/ASFBot)**, bot de telegram escrito en python con integración ASF.
- **[ASF STM userscript](https://greasyfork.org/en/scripts/404754-asf-stm)**, para aquellos que quieran enviar ofertas de intercambio automatizadas a bots en nuestra **[lista ASF STM](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin-es-ES#publiclisting)** a través de un navegador web, sin usar la función `MatchActively` provista por ASF.
- **[simple-asf-bot](https://github.com/deluxghost/simple-asf-bot)**, otro bot (mínimo) de telegram escrito en python con integración de ASF.

---

## Bibliotecas

- **[ASF-IPC](https://github.com/deluxghost/ASF_IPC)**, biblioteca python para mayor integración con la interfaz IPC de ASF.

---

## Paquetes

- **[AUR repo #1](https://aur.archlinux.org/packages/asf)**, permite instalar fácilmente ASF en arch linux.
- **[AUR repo #2](https://aur.archlinux.org/packages/archisteamfarm-bin)**, permite instalar fácilmente ASF en arch linux.
- **[Homebrew](https://formulae.brew.sh/formula/archi-steam-farm)**, permite instalar fácilmente ASF en macOS.
- **[Nix](https://search.nixos.org/packages?channel=unstable&show=ArchiSteamFarm&from=0&size=50&sort=relevance&type=packages&query=ArchiSteamFarm)**, permite instalar fácilmente ASF en distros con Nix.
- **[NixOS](https://search.nixos.org/options?channel=unstable&from=0&size=50&sort=relevance&type=packages&query=ArchiSteamFarm)**, permite configurar e instalar ASF con NixOS.
- **[Winget](https://github.com/microsoft/winget-pkgs/tree/master/manifests/j/JustArchiNET/ArchiSteamFarm)**, permite instalar fácilmente ASF en Windows.

---

## Herramientas

- **[Keys extractor](https://umaim.github.io/SKE)**, permite copiar y pegar claves en varios formatos y crear comandos `redeem` para ASF. Revisa el **[repositorio de GitHub](https://github.com/PixvIO/SKE)** para más detalles.
- **[ASF Mass Config Editor](https://github.com/genesix-eu/ASF_MCE)**, permite administrar múltiples configuraciones de ASF más fácilmente.

---

## ¿Quieres encontrar más?

Recomendamos el tema **[ArchiSteamFarm](https://github.com/topics/archisteamfarm)** en GitHub para todos los proyectos que se integran con ASF.
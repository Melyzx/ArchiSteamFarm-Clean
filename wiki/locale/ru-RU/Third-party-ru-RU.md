# Сторонние разработки

Этот раздел включает в себя различные дополнения сторонних разработчиков, разработанные специально (или в основном) для использования совместно с ASF. Они покрывают весь спектр начиная с плагинов для ASF, простых веб-приложений, включая простые веб-приложения и внутренние библиотеки для удобства дальнейшей интеграции, и заканчивая полнофункциональными ботами для других платформ. Если вы хотите добавить что-то в этот список - свяжитесь с нами на нашем сервере Discord или в нашей группе Steam.

Пожалуйста, обратите внимание, что программы ниже **не** поддерживаются разработчиками ASF и поэтому **мы не даём никаких гарантий относительно них**, особенно в части безопасности, надёжности и соответствия соглашению подписчика Steam. Этот список поддерживается только для справок. Вы всегда должны быть уверены, что сторонняя утилита, которую вы собираетесь использовать, является безопасной и достаточно легальной для вас, поскольку вы используете их на свой страх и риск.

---

## Плагины для ASF

### **[CatPoweredPlugins](https://github.com/CatPoweredPlugins)** (**[Rudokhvist](https://github.com/Rudokhvist)**)

- **[ASFAchievementManager](https://github.com/CatPoweredPlugins/ASFAchievementManager)**, plugin for ASF that allows you to manage Steam achievements.
- **[BirthdayPlugin](https://github.com/CatPoweredPlugins/BirthdayPlugin)**, plugin for ASF to receive birthday congratulations.
- **[CaseInsensitiveASF](https://github.com/CatPoweredPlugins/CaseInsensitiveASF)**, plugin for ASF to make bot names case-insensitive.
- **[CommandAliasPlugin](https://github.com/CatPoweredPlugins/CommandAliasPlugin)**, plugin for ASF to setup custom command aliases for ASF commands.
- **[CommandlessRedeem](https://github.com/CatPoweredPlugins/CommandlessRedeem)**, plugin for ASF to re-implement key redeeming without a command.
- **[ItemDispenser](https://github.com/CatPoweredPlugins/ItemDispenser)**, plugin for ASF to automatically accept trade request for certain type(s) of items.
- **[SelectiveLootAndTransferPlugin](https://github.com/CatPoweredPlugins/SelectiveLootAndTransferPlugin)**, plugin for ASF providing advanced `transfer` command for transferring Steam items.

### **[Citrinate](https://github.com/Citrinate)**

- **[BoosterManager](https://github.com/Citrinate/BoosterManager)**, плагин для ASF, предоставляющий простой в использовании интерфейс для превращения самоцветов в наборы карточек, а также различные функции для управления инвентарем и лотами, размещенными на Торговой площадке.
- **[CS2Interface](https://github.com/Citrinate/CS2Interface)**, плагин для ASF, позволяющий взаимодействовать с Counter-Strike 2 с помощью **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**.
- **[FreePackages](https://github.com/Citrinate/FreePackages)**, плагин для ASF, который находит бесплатные пакеты в Steam и добавляет их на ваш аккаунт.

### **[VITA](https://github.com/ezhevita)**

- **[FriendAccepter](https://github.com/ezhevita/FriendAccepter)**, плагин для ASF, позволяющий автоматически принимать все приглашения в друзья.
- **[GameRemover](https://github.com/ezhevita/GameRemover)**, плагин для ASF, реализующий команду удаления лицензий Steam для выбранных экземпляров ботов.
- **[GetEmail](https://github.com/ezhevita/GetEmail)**, плагин для ASF, реализующий команду для получения адресов электронной почты заданных экземпляров ботов непосредственно из Steam.
- **[ResetAPIKey](https://github.com/ezhevita/ResetAPIKey)**, плагин для ASF, реализующий команду сброса ключа API для выбранных экземпляров ботов.

### Другое

- **[ASFEnhance](https://github.com/chr233/ASFEnhance)**, плагин для ASF, дополняющий его различными новыми функциями, особенно командами.
- **[ASFFreeGames](https://github.com/maxisoft/ASFFreeGames)**, плагин для ASF, позволяющий автоматически собирать бесплатные steam-игры, выложенные на reddit.

---

## Интеграции

- **[ASFBot](https://github.com/dmcallejo/ASFBot)**, бот для telegram, написанный на python и интегрированный с ASF.
- **[ASF STM](https://greasyfork.org/en/scripts/404754-asf-stm)** - userscript для тех, кто хочет отправлять автоматизированные торговые предложения ботам в нашем **[листинге ASF STM](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin#publiclisting)** через веб-браузер без использования функции `MatchActively`, предоставляемой ASF.
- **[simple-asf-bot](https://github.com/deluxghost/simple-asf-bot)**, another (minimal) telegram bot written in python featuring ASF integration.

---

## Библиотеки

- **[ASF-IPC](https://github.com/deluxghost/ASF_IPC)**, библиотека на языке python для удобной интеграции с интерфейсом ASF IPC.

---

## Пакетная установка

- **[репозиторий AUR #1](https://aur.archlinux.org/packages/asf)**, позволяющий легко установить ASF на Arch linux.
- **[репозиторий AUR #2](https://aur.archlinux.org/packages/archisteamfarm-bin)**, позволяющий легко установить ASF на Arch linux.
- **[репозиторий Homebrew](https://formulae.brew.sh/formula/archi-steam-farm)**, позволяющий легко установить ASF на macOS.
- **[репозиторий Nix](https://search.nixos.org/packages?channel=unstable&show=ArchiSteamFarm&from=0&size=50&sort=relevance&type=packages&query=ArchiSteamFarm)**, позволяющий легко установить ASF на дистрибутивы с Nix.
- **[репозиторий NixOS](https://search.nixos.org/options?channel=unstable&from=0&size=50&sort=relevance&type=packages&query=ArchiSteamFarm)**, позволяющий легко установить ASF на дистрибутивы с NixOS.
- **[Winget](https://github.com/microsoft/winget-pkgs/tree/master/manifests/j/JustArchiNET/ArchiSteamFarm)**, позволяющий легко установить ASF на Windows.

---

## Инструменты

- **[Keys extractor](https://umaim.github.io/SKE)**, позволяет вам копировать/вставлять ключи в различных форматах и формировать команду `redeem` для ASF. Вы найдёте подробности в **[репозитории на GitHub](https://github.com/PixvIO/SKE)**.
- **[ASF Mass Config Editor](https://github.com/genesix-eu/ASF_MCE)**, позволяет проще управлять большим количеством конфигурационных файлов ASF.

---

## Хотите найти больше?

Мы рекомендуем использовать на GitHub метку **[ArchiSteamFarm](https://github.com/topics/archisteamfarm)** для всех проектов, имеющих интеграцию с ASF.
# Extensions

ASF includes support for custom plugins that can be loaded during runtime. Plugins allow you to customize ASF behaviour, for example by adding custom commands, custom trading logic or whole integration with third-party services and APIs.

This page describes ASF plugins from users perspective - explanation, usage and likewise. If you want to view developer's perspective, move **[here](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-development)** instead.

---

## Utilisation

ASF charge les plugins depuis le répertoire `plugins` situé dans votre dossier ASF. It's a recommended practice (which becomes mandatory with plugin auto-updates) to maintain a dedicated directory for each plugin that you want to use, which can be based off its name, such as `MyPlugin`. Faire cela donnera comme résultat une structure type `plugins/MyPlugin`. Finally, all binary files of the plugin should be put inside that dedicated folder, and ASF will properly discover and use your plugin after restart.

Usually plugin developers will publish their plugins in form of a `zip` file with binaries inside, which means that you should unpack that zip file to its own dedicated subdirectory inside `plugins` directory.

If the plugin was loaded successfully, you'll see its name and version in your log. You should consult your plugin developers in case of questions, issues or usage related to the plugins that you've decided to use.

Vous pouvez trouver des plugins en vedette dans notre section **[contenu tiers](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party#asf-plugins)**.

**Faites attention les plugins ASF peuvent-être malveillants**. You should always ensure that you're using plugins made by developers that you can trust, even those from the third-party section above. Les développeurs d'ASF ne peuvent plus vous garantir les avantages habituels d'ASF (tels que l'absence de logiciels malveillants ou l'absence de bans VAC) si vous décidez d'utiliser des plugins personnalisés. Vous devez comprendre que les plugins ont un contrôle total sur le processus ASF une fois chargé, à cause de cela, nous sommes également incapables de prendre en charge les configurations qui utilisent des plugins personnalisés, puisque vous n'utilisez plus de code ASF d'origine.

---

## Compatibilité

Depending on plugin's complexity, scope and a lot of other factors, it's entirely possible that it'll require from you to use **[generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up#generic-setup)** ASF variant, instead of usually recommended **[OS-specific](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up#os-specific-setup)**. This is because OS-specific variant comes only with core functionality required for ASF itself, and your plugin may require parts that fall outside of main ASF scope, in result being incompatible with trimmed OS-specific builds.

In general, when using third-party plugins, we recommend using ASF generic variant for maximum compatibility. However, not all plugins may require it - please refer to your plugin's information in order to find out whether you need to use generic ASF variant or not.
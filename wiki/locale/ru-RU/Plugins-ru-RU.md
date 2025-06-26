# Плагины

ASF includes support for custom plugins that can be loaded during runtime. Плагины позволяют вам изменять поведение ASF, например добавляя дополнительные команды, новую логику принятия обменов или полную интеграцию со сторонними сервисами и API.

This page describes ASF plugins from users perspective - explanation, usage and likewise. If you want to view developer's perspective, move **[here](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-development)** instead.

---

## Использование

ASF загружает плагины из папки `plugins` расположенной в вашей папке с ASF. It's a recommended practice (which becomes mandatory with plugin auto-updates) to maintain a dedicated directory for each plugin that you want to use, which can be based off its name, such as `MyPlugin`. Это даст в результате структуру каталогов `plugins/MyPlugin`. Наконец, все двоичные файлы плагина следует положить в эту выделенную папку, и ASF обнаружит и начнёт использовать ваш плагин после перезапуска.

Usually plugin developers will publish their plugins in form of a `zip` file with binaries inside, which means that you should unpack that zip file to its own dedicated subdirectory inside `plugins` directory.

Если плагин успешно загружен - вы увидите его название и версию в журнале ASF. В случае вопросов или проблем, связанных с используемым плагином, вам следует обратиться к разработчикам плагина.

Вы можете найти некоторые плагины в нашем разделе "**[Сторонние разработки](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party-ru-RU#user-content-Плагины-для-asf)**".

**Помните о том, что плагины ASF могут быть вредоносными**. You should always ensure that you're using plugins made by developers that you can trust, even those from the third-party section above. Разработчики ASF не могут вам гарантировать обычных преимуществ ASF (как например отсутствие вирусов или гарантию отсутствия VAC) если вы решите использовать какие-либо пользовательские плагины. Нужно понимать, что, после загрузки, плагины полностью контролируют процесс ASF, из-за этого мы также не сможем поддерживать настройки, которые используют пользовательские плагины, так как вы запускаете не оригинальный ASF код.

---

## Совместимость

Depending on plugin's complexity, scope and a lot of other factors, it's entirely possible that it'll require from you to use **[generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up#generic-setup)** ASF variant, instead of usually recommended **[OS-specific](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up#os-specific-setup)**. This is because OS-specific variant comes only with core functionality required for ASF itself, and your plugin may require parts that fall outside of main ASF scope, in result being incompatible with trimmed OS-specific builds.

In general, when using third-party plugins, we recommend using ASF generic variant for maximum compatibility. However, not all plugins may require it - please refer to your plugin's information in order to find out whether you need to use generic ASF variant or not.
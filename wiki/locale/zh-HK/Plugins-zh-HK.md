# 外掛程式

ASF includes support for custom plugins that can be loaded during runtime. 外掛程式允許您通過添加自訂命令、自訂交易邏輯或與第三方工具和 API的整體集成自訂ASF行為。

This page describes ASF plugins from users perspective - explanation, usage and likewise. If you want to view developer's perspective, move **[here](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-development)** instead.

---

## 使用方法

ASF從位於ASF資料夾中的`plugins`目錄中載入外掛程式。 It's a recommended practice (which becomes mandatory with plugin auto-updates) to maintain a dedicated directory for each plugin that you want to use, which can be based off its name, such as `MyPlugin`. 這樣做將生成 `plugins/MyPlugin` 的最終樹結構。 最後，外掛程式的所有二進位檔案都應該放在那個專用資料夾裏，ASF 會在重新開機後成功偵測並使用您的外掛程式。

Usually plugin developers will publish their plugins in form of a `zip` file with binaries inside, which means that you should unpack that zip file to its own dedicated subdirectory inside `plugins` directory.

如果外掛程式已成功載入，您將在日誌中看到它的名稱和版本。 在遇到與您決定使用的外掛程式相關的錯誤或用法問題時，您應該諮詢相關外掛程式開發人員。

您可以在我們的**[第三方工具](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party#asf-plugins)**部分找到一些特色外掛程式。

**Please note that ASF plugins could be malicious**. You should always ensure that you're using plugins made by developers that you can trust, even those from the third-party section above. 如果您決定使用任何自訂外掛程式，ASF 開發人員將無法再保證您通常的 ASF 優勢（如絕無惡意軟件或避免VAC）。 You need to understand that plugins have full control over ASF process once loaded, due to that we're also unable to support setups that utilize custom plugins, since you're no longer running vanilla ASF code.

---

## 兼容性

Depending on plugin's complexity, scope and a lot of other factors, it's entirely possible that it'll require from you to use **[generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up#generic-setup)** ASF variant, instead of usually recommended **[OS-specific](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up#os-specific-setup)**. This is because OS-specific variant comes only with core functionality required for ASF itself, and your plugin may require parts that fall outside of main ASF scope, in result being incompatible with trimmed OS-specific builds.

In general, when using third-party plugins, we recommend using ASF generic variant for maximum compatibility. However, not all plugins may require it - please refer to your plugin's information in order to find out whether you need to use generic ASF variant or not.
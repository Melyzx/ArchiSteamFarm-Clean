# 第三方工具

本章節包含專門（或主要）與ASF配合使用的各種第三方工具的內容。 它們包括ASF外掛程式、簡單的網路應用程式、用於一體化的內部函式庫，以及適用於各種平台的全能Bot。 若您想在此清單中新增內容，請在Discord或我們的Steam群組上聯絡我們。

請注意，以下程式&#8203;**並非**&#8203;由ASF開發人員所維護，因此&#8203;**我們不提供任何保證**&#8203;，特別是它們的安全性及是否符合Steam服務條款。 此清單僅供參考。 您應始終保證您欲使用的第三方工具對您來說足夠安全與合規，因為您需自行承擔使用它們的風險。

---

## ASF 外掛程式

### **[CatPoweredPlugins](https://github.com/CatPoweredPlugins)** (**[Rudokhvist](https://github.com/Rudokhvist)**)

- **[ASFAchievementManager](https://github.com/CatPoweredPlugins/ASFAchievementManager)**, plugin for ASF that allows you to manage Steam achievements.
- **[BirthdayPlugin](https://github.com/CatPoweredPlugins/BirthdayPlugin)**, plugin for ASF to receive birthday congratulations.
- **[CaseInsensitiveASF](https://github.com/CatPoweredPlugins/CaseInsensitiveASF)**, plugin for ASF to make bot names case-insensitive.
- **[CommandAliasPlugin](https://github.com/CatPoweredPlugins/CommandAliasPlugin)**, plugin for ASF to setup custom command aliases for ASF commands.
- **[CommandlessRedeem](https://github.com/CatPoweredPlugins/CommandlessRedeem)**, plugin for ASF to re-implement key redeeming without a command.
- **[ItemDispenser](https://github.com/CatPoweredPlugins/ItemDispenser)**, plugin for ASF to automatically accept trade request for certain type(s) of items.
- **[SelectiveLootAndTransferPlugin](https://github.com/CatPoweredPlugins/SelectiveLootAndTransferPlugin)**, plugin for ASF providing advanced `transfer` command for transferring Steam items.

### **[Citrinate](https://github.com/Citrinate)**

- **[BoosterManager](https://github.com/Citrinate/BoosterManager)**&#8203;：提供ASF一項易用的介面，能夠將寶石合成擴充包，並擁有管理物品庫及市集的功能。
- **[CS2Interface](https://github.com/Citrinate/CS2Interface)**&#8203;：使您能夠透過&#8203;**[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-zh-TW)**&#8203;使用《絕對武力 2》。
- **[FreePackages](https://github.com/Citrinate/FreePackages)**&#8203;：使ASF能在Steam上搜尋免費遊戲，並將其新增至帳號中。

### **[Vita](https://github.com/ezhevita)**

- **[FriendAccepter](https://github.com/ezhevita/FriendAccepter)**&#8203;：ASF將會自動接受所有好友邀請。
- **[GameRemover](https://github.com/ezhevita/GameRemover)**&#8203;：實作ASF指令，以刪除指定Bot實例的Steam授權條款。
- **[GetEmail](https://github.com/ezhevita/GetEmail)**&#8203;：實作ASF指令，以直接從Steam獲得指定Bot實例的電子郵件地址。
- **[ResetAPIKey](https://github.com/ezhevita/ResetAPIKey)**&#8203;：實作ASF指令，以重置指定Bot實例的API金鑰。

### 其他

- **[ASFEnhance](https://github.com/chr233/ASFEnhance)**&#8203;：加入新功能以增強ASF，特別是指令。
- **[ASFFreeGames](https://github.com/maxisoft/ASFFreeGames)**&#8203;：使ASF能夠自動收集在Reddit上發表的免費Steam遊戲。

---

## 整合

- **[ASFBot](https://github.com/dmcallejo/ASFBot)**&#8203;：用Python編寫，整合ASF與Telegram機器人。
- **[ASF STM使用者腳本](https://greasyfork.org/zh-TW/scripts/404754-asf-stm)**&#8203;：使您可以透過瀏覽器向我們的&#8203;**[ASF STM清單](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin-zh-TW#publiclisting)**&#8203;中的Bot發送自動交易報價，而不必使用ASF提供的&#8203;`MatchActively`&#8203;功能。
- **[simple-asf-bot](https://github.com/deluxghost/simple-asf-bot)**, another (minimal) telegram bot written in python featuring ASF integration.

---

## 函式庫

- **[ASF-IPC](https://github.com/deluxghost/ASF_IPC)**&#8203;：用於進一步整合ASF的IPC介面的Python函式庫。

---

## 封裝

- **[AUR repo #1](https://aur.archlinux.org/packages/asf)**&#8203;：讓您能輕鬆地在Arch Linux上安裝ASF。
- **[AUR repo #2](https://aur.archlinux.org/packages/archisteamfarm-bin)**&#8203;：讓您能輕鬆地在Arch Linux上安裝ASF。
- **[Homebrew](https://formulae.brew.sh/formula/archi-steam-farm)**&#8203;：讓您能輕鬆地在macOS上安裝ASF。
- **[Nix](https://search.nixos.org/packages?channel=unstable&show=ArchiSteamFarm&from=0&size=50&sort=relevance&type=packages&query=ArchiSteamFarm)**&#8203;：讓您能輕鬆地在帶有Nix的發行版本上安裝ASF。
- **[NixOS](https://search.nixos.org/options?channel=unstable&from=0&size=50&sort=relevance&type=packages&query=ArchiSteamFarm)**&#8203;：讓您能輕鬆地在NixOS上設定並安裝ASF。
- **[Winget](https://github.com/microsoft/winget-pkgs/tree/master/manifests/j/JustArchiNET/ArchiSteamFarm)**&#8203;：讓您能輕鬆地在Windows上安裝ASF。

---

## 工具

- **[Keys extractor](https://umaim.github.io/SKE)**&#8203;：讓您能複製貼上含有各種格式的序號，並使ASF建立&#8203;`redeem`&#8203;指令。 前往&#8203;**[GitHub repo](https://github.com/PixvIO/SKE)**&#8203;以了解更多。
- **[ASF Mass Config Editor](https://github.com/genesix-eu/ASF_MCE)**&#8203;：讓您能更方便地管理多個ASF設定檔。

---

## 想要發現更多嗎？

我們建議您在GitHub上搜尋&#8203;**[ArchiSteamFarm](https://github.com/topics/archisteamfarm)**&#8203;主題，來尋找關於ASF的所有專案。
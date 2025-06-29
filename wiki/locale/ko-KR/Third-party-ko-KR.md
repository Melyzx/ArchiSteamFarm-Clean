# 서드 파티

이 항목에는 독점적 혹은 주로 ASF와 함께 사용하도록 작성된 여러 서드 파티 애드온이 있습니다. 통합을 위한 내부 라이브러리와 간단한 웹 어플리케이션 같은 ASF 플러그인부터 다른 플랫폼을 위한 완전한 기능을 가진 봇 까지 범위가 넓습니다. 목록에 뭔가 추가하고 싶다면 Discord나 Steam그룹으로 알려주시기 바랍니다.

아래의 프로그램들은 ASF 개발자들이 유지보수하지 **않으므로** 보안, 안전, 스팀 서비스약관 준수 측면에서 **어떠한 것도 보장하지 않음**을 유의하시기 바랍니다. 이 목록은 참조용으로만 유지됩니다. You should always ensure that the third-party utility you're about to use is safe and legit enough for you, as you're using all of them at your own risk.

---

## ASF 플러그인

### **[CatPoweredPlugins](https://github.com/CatPoweredPlugins)** (**[Rudokhvist](https://github.com/Rudokhvist)**)

- **[ASFAchievementManager](https://github.com/CatPoweredPlugins/ASFAchievementManager)**, plugin for ASF that allows you to manage Steam achievements.
- **[BirthdayPlugin](https://github.com/CatPoweredPlugins/BirthdayPlugin)**, plugin for ASF to receive birthday congratulations.
- **[CaseInsensitiveASF](https://github.com/CatPoweredPlugins/CaseInsensitiveASF)**, plugin for ASF to make bot names case-insensitive.
- **[CommandAliasPlugin](https://github.com/CatPoweredPlugins/CommandAliasPlugin)**, plugin for ASF to setup custom command aliases for ASF commands.
- **[CommandlessRedeem](https://github.com/CatPoweredPlugins/CommandlessRedeem)**, plugin for ASF to re-implement key redeeming without a command.
- **[ItemDispenser](https://github.com/CatPoweredPlugins/ItemDispenser)**, plugin for ASF to automatically accept trade request for certain type(s) of items.
- **[SelectiveLootAndTransferPlugin](https://github.com/CatPoweredPlugins/SelectiveLootAndTransferPlugin)**, plugin for ASF providing advanced `transfer` command for transferring Steam items.

### **[Citrinate](https://github.com/Citrinate)**

- **[BoosterManager](https://github.com/Citrinate/BoosterManager)**, plugin for ASF providing an easy-to-use interface for turning gems into booster packs as well as various features for managing inventories and market listings.
- **[CS2Interface](https://github.com/Citrinate/CS2Interface)**, plugin for ASF that allows you to interact with Counter-Strike 2 using **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**.
- **[FreePackages](https://github.com/Citrinate/FreePackages)**, plugin for ASF that finds free packages on Steam and adds them to your account.

### **[Vita](https://github.com/ezhevita)**

- **[FriendAccepter](https://github.com/ezhevita/FriendAccepter)**, plugin for ASF to automatically accept all friend invites.
- **[GameRemover](https://github.com/ezhevita/GameRemover)**, plugin for ASF implementing a command to remove Steam licenses for selected bot instances.
- **[GetEmail](https://github.com/ezhevita/GetEmail)**, plugin for ASF implementing a command to fetch e-mail address of given bot instances directly from Steam.
- **[ResetAPIKey](https://github.com/ezhevita/ResetAPIKey)**, plugin for ASF implementing a command to reset API key for selected bot instances.

### 기타

- **[ASFEnhance](https://github.com/chr233/ASFEnhance)**, plugin for ASF enhancing it with various new features, especially commands.
- **[ASFFreeGames](https://github.com/maxisoft/ASFFreeGames)**, plugin for ASF allowing one to automatically collect free steam games posted on reddit.

---

## 통합

- **[ASFBot](https://github.com/dmcallejo/ASFBot)**, ASF와 통합되는 python으로 작성한 텔레그램 봇입니다.
- **[ASF STM userscript](https://greasyfork.org/en/scripts/404754-asf-stm)**, for those who want to send automated trade offers to bots on our **[ASF STM listing](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin#publiclisting)** through web browser, without using `MatchActively` feature provided by ASF.
- **[simple-asf-bot](https://github.com/deluxghost/simple-asf-bot)**, another (minimal) telegram bot written in python featuring ASF integration.

---

## 라이브러리

- **[ASF-IPC](https://github.com/deluxghost/ASF_IPC)**, ASF의 IPC 인터페이스와 더욱 통합되는 python 라이브러리입니다.

---

## 패키징

- **[AUR repo #1](https://aur.archlinux.org/packages/asf)**, 아치 리눅스에서 ASF를 쉽게 설치할 수 있습니다.
- **[AUR repo #2](https://aur.archlinux.org/packages/archisteamfarm-bin)**, 아치 리눅스에서 ASF를 쉽게 설치할 수 있습니다.
- **[Homebrew](https://formulae.brew.sh/formula/archi-steam-farm)**, allowing you to easily install ASF on macOS.
- **[Nix](https://search.nixos.org/packages?channel=unstable&show=ArchiSteamFarm&from=0&size=50&sort=relevance&type=packages&query=ArchiSteamFarm)**, allowing you to easily install ASF on distros with Nix.
- **[NixOS](https://search.nixos.org/options?channel=unstable&from=0&size=50&sort=relevance&type=packages&query=ArchiSteamFarm)**, allowing you to configure and install ASF with NixOS.
- **[Winget](https://github.com/microsoft/winget-pkgs/tree/master/manifests/j/JustArchiNET/ArchiSteamFarm)**, allowing you to easily install ASF on windows.

---

## 도구

- **[키 추출기](https://umaim.github.io/SKE)**, 키를 복사-붙여넣기해서 다양한 형식으로 만들 수 있으며 ASF의 `redeem` 명령을 만들 수 있습니다. 자세한 내용은 **[GitHub repo](https://github.com/PixvIO/SKE)** 를 확인하시기 바랍니다.
- **[ASF 대량 환경설정 에디터](https://github.com/genesix-eu/ASF_MCE)**, 다량의 ASF 환경설정을 더 쉽게 관리할 수 있습니다.

---

## 더 찾아보고 싶은가요?

GitHub에서 **[ArchiSteamFarm](https://github.com/topics/archisteamfarm)** 토픽으로 ASF와 통합하는 모든 프로젝트를 찾아보기를 권합니다.
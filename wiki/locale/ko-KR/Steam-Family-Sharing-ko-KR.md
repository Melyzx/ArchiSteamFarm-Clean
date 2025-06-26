# Steam 가족 공유

ASF supports Steam Family Sharing - the old as well as the new system. In order to understand how ASF works with that, you should firstly read how **[Steam Family Sharing works](https://store.steampowered.com/promotion/familysharing)**, which is available on Steam store. In addition to that, check **[the news](https://store.steampowered.com/news/app/593110/view/4149575031735702628)** for upcoming new Steam Family Sharing system, which ASF is also compatible with.

---

## ASF

ASF의 Steam 가족 공유 기능 지원은 투명합니다. 즉, 어떠한 새로운 bot/프로세스 설정 항목이 없으며 추가적인 붙박이 기능으로서 ASF의 외부에서 작동합니다.

ASF는 가족 공유 이용자가 라이브러리를 사용하고 있는지 판단하기 위한 적절한 로직을 포함하고 있으며, 게임 실행시 플레이 중인 세션에서 "쫓아내지" 않을 것입니다. ASF는 주 계정이 플레이중인것처럼 정확히 똑같이 동작하므로, Steam 클라이언트가 플레이중이거나 가족 공유 이용자가 플레이중이라면 ASF는 농사를 시도하지 않고 플레이가 끝날때까지 기다립니다. This is mostly related to the old system - new system allows your family sharing users to play games other than those that ASF is currently farming.

In addition to above, after logging in, ASF will access your family sharing systems (old and new), from which it'll extract users (Steam IDs) allowed to use your library. Those users are given `FamilySharing` permission to use **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, especially allowing them to use `pause~` command on bot account that is sharing games with them, which allows to pause automatic cards farming module in order to launch a game that can be shared - this also mostly applies to the old system, but might still be used with the new system in case ASF is currently farming game that your users want to play.

Connecting both functionalities described above allows your friends to `pause~` your cards farming process, start a game, play as long as they wish, and then after they're done playing, cards farming process will be automatically resumed by ASF. 물론 ASF가 농사중이 아니라면 `pause~`의 사용은 필요하지 않습니다. 당신의 친구들은 게임을 즉시 실행할 수 있고 위에서 설명한 로직은 그들을 쫓아내지 않을 것입니다.

---

## 한계

Steam 네트워크는 잘못된 상태 업데이트를 보내서 ASF가 오해하는 것을 좋아합니다. ASF는 농사를 재개해도 좋다고 믿을 것이고, 그 결과 당신의 친구를 너무 빨리 쫓아낼 것입니다. 이것은 **[이 자주묻는 질문 항목](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ-ko-KR#asf-is-kicking-my-steam-client-session-while-im-playing--this-account-is-logged-on-another-pc)** 에 설명된 것과 정확하게 동일한 이슈입니다. We recommend exactly the same solutions, mainly promoting your friends to `Operator` permission (or above) and telling them to make use of `pause` and `resume` commands.
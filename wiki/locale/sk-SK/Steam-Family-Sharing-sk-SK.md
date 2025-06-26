# Steam Family Sharing

ASF supports Steam Family Sharing - the old as well as the new system. In order to understand how ASF works with that, you should firstly read how **[Steam Family Sharing works](https://store.steampowered.com/promotion/familysharing)**, which is available on Steam store. In addition to that, check **[the news](https://store.steampowered.com/news/app/593110/view/4149575031735702628)** for upcoming new Steam Family Sharing system, which ASF is also compatible with.

---

## ASF

Support for Steam Family Sharing feature in ASF is transparent, which means that it doesn't introduce any new bot/process config properties - it works out of the box as an extra built-in functionality.

ASF includes appropriate logic in order to be aware of library being locked by family sharing users, therefore it won't "kick" them out of playing session due to launching a game. ASF will act exactly the same as with primary account holding the lock, therefore if that lock is being held either by your steam client, or by one of your family sharing users, ASF will not attempt to farm, instead, it will wait for the lock to be released. This is mostly related to the old system - new system allows your family sharing users to play games other than those that ASF is currently farming.

In addition to above, after logging in, ASF will access your family sharing systems (old and new), from which it'll extract users (Steam IDs) allowed to use your library. Those users are given `FamilySharing` permission to use **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, especially allowing them to use `pause~` command on bot account that is sharing games with them, which allows to pause automatic cards farming module in order to launch a game that can be shared - this also mostly applies to the old system, but might still be used with the new system in case ASF is currently farming game that your users want to play.

Connecting both functionalities described above allows your friends to `pause~` your cards farming process, start a game, play as long as they wish, and then after they're done playing, cards farming process will be automatically resumed by ASF. Of course, issuing `pause~` is not needed if ASF is currently not farming anything actively, because your friends can launch the game right away, and logic described above ensures that they won't be kicked out of the session.

---

## Limitations

Steam network loves to mislead ASF by broadcasting false status updates, which may lead to ASF believing it's fine to resume process, and in result kick your friend too soon. This is exactly the same issue as the one already explained by us in **[this FAQ entry](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#asf-is-kicking-my-steam-client-session-while-im-playing--this-account-is-logged-on-another-pc)**. We recommend exactly the same solutions, mainly promoting your friends to `Operator` permission (or above) and telling them to make use of `pause` and `resume` commands.
# Steam Family Sharing

ASF поддерживает функцию Family Library Sharing - как по старой системе, так и по новой. In order to understand how ASF works with that, you should firstly read how **[Steam Family Sharing works](https://store.steampowered.com/promotion/familysharing)**, which is available on Steam store. In addition to that, check **[the news](https://store.steampowered.com/news/app/593110/view/4149575031735702628)** for upcoming new Steam Family Sharing system, which ASF is also compatible with.

---

## ASF

Поддержка Steam Family Sharing в ASF достаточно прозрачна, в том смысле что она не требует никаких дополнительных параметров конфигурации бота/процесса - она просто работает "из коробки" как дополнительный встроенный функционал.

ASF включает в себя соответствующую логику для того, чтобы отслеживать использование библиотеки пользователями Steam Family Sharing, и поэтому не будет "выбрасывать" их из игровой сессии из-за запуска игры. ASF будет работать так же, как в случае когда библиотека заблокирована основным аккаунтом, поэтому если библиотека используется либо с вашего клиента steam, либо одним из пользователей Steam Family Sharing, ASF не будет пытаться фармить, и будет вместо этого ожидать пока библиотека освободится. This is mostly related to the old system - new system allows your family sharing users to play games other than those that ASF is currently farming.

In addition to above, after logging in, ASF will access your family sharing systems (old and new), from which it'll extract users (Steam IDs) allowed to use your library. Those users are given `FamilySharing` permission to use **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, especially allowing them to use `pause~` command on bot account that is sharing games with them, which allows to pause automatic cards farming module in order to launch a game that can be shared - this also mostly applies to the old system, but might still be used with the new system in case ASF is currently farming game that your users want to play.

Connecting both functionalities described above allows your friends to `pause~` your cards farming process, start a game, play as long as they wish, and then after they're done playing, cards farming process will be automatically resumed by ASF. Разумеется, команда `pause~` не нужна если ASF в данный момент ничего не фармит, поскольку ваши друзья могут просто запустить игру, и логика, описанная выше, проследит чтобы они не были "выброшены" из игровой сессии.

---

## Ограничения

Сеть Steam любит дурачить ASF пересылая неверные обновления состояния, что может привести к ситуации, когда ASF считает что можно продолжать процесс фарма, и в результате "выбросит" вашего друга раньше времени. Это та же проблема, что и описанная нами в **[этом пункте ЧаВо](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ-ru-RU#user-content-asf-%D0%BE%D1%82%D0%BA%D0%BB%D1%8E%D1%87%D0%B0%D0%B5%D1%82-%D0%BC%D0%BE%D1%8E-%D1%81%D0%B5%D1%81%D1%81%D0%B8%D1%8E-%D0%BA%D0%BB%D0%B8%D0%B5%D0%BD%D1%82%D0%B0-steam-%D0%BA%D0%BE%D0%B3%D0%B4%D0%B0-%D1%8F-%D0%B8%D0%B3%D1%80%D0%B0%D1%8E--%D0%AD%D1%82%D0%BE%D1%82-%D0%B0%D0%BA%D0%BA%D0%B0%D1%83%D0%BD%D1%82-%D1%83%D0%B6%D0%B5-%D0%B3%D0%B4%D0%B5-%D1%82%D0%BE-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B5%D1%82%D1%81%D1%8F)**. Мы рекомендуем те же методы её решения, в основном - предоставить вашим друзьям права доступа `Operator` (или выше) и научить их использовать команды `pause` и `resume`.
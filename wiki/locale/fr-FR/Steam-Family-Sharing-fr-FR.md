# Partage familial Steam

ASF supports Steam Family Sharing - the old as well as the new system. In order to understand how ASF works with that, you should firstly read how **[Steam Family Sharing works](https://store.steampowered.com/promotion/familysharing)**, which is available on Steam store. In addition to that, check **[the news](https://store.steampowered.com/news/app/593110/view/4149575031735702628)** for upcoming new Steam Family Sharing system, which ASF is also compatible with.

---

## ASF

La prise en charge de la fonctionnalité du partage familial dans ASF est transparente, ce qui signifie qu’elle ne présente aucune nouvelle propriété de configuration bot / process. Elle fonctionne immédiatement en tant que fonctionnalité intégrée supplémentaire.

ASF inclut une logique appropriée afin de s’assurer que la bibliothèque est verrouillée par les utilisateurs du partage familial. Par conséquent, elle ne les "coupera" pas de la session en raison du lancement d’une partie. ASF agira exactement de la même manière que le compte principal qui détient le verrou. Par conséquent, si ce verrou est détenu par votre client Steam ou par l’un de vos utilisateurs du partage familial, ASF ne tentera pas de créer une ferme, mais attendra que le verrou soit libre. This is mostly related to the old system - new system allows your family sharing users to play games other than those that ASF is currently farming.

In addition to above, after logging in, ASF will access your family sharing systems (old and new), from which it'll extract users (Steam IDs) allowed to use your library. Those users are given `FamilySharing` permission to use **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, especially allowing them to use `pause~` command on bot account that is sharing games with them, which allows to pause automatic cards farming module in order to launch a game that can be shared - this also mostly applies to the old system, but might still be used with the new system in case ASF is currently farming game that your users want to play.

Connecting both functionalities described above allows your friends to `pause~` your cards farming process, start a game, play as long as they wish, and then after they're done playing, cards farming process will be automatically resumed by ASF. Bien entendu, l'émission de ` pause ~` n'est pas nécessaire si ASF n'effectue actuellement aucune activité de farm active, car vos amis peuvent lancer le jeu immédiatement, et la logique décrite ci-dessus garantit qu'ils ne seront pas expulsés du serveur.

---

## Restriction

Le réseau Steam aime induire en erreur ASF en diffusant de fausses mises à jour de statut, ce qui pourrait conduire ASF à croire qu'il est bon de reprendre le processus et que, par conséquent, expulse votre ami trop tôt. C'est exactement le même problème que celui que nous avons déjà expliqué dans **[cette entrée de la FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#asf-is-kicking-my-steam-client-session-while-im-playing--this-account-is-logged-on-another-pc)**. Nous recommandons exactement les mêmes solutions, principalement promouvoir vos amis à la permission `Opérateur` (ou supérieure) et leur disant de faire usage des commandes `pause` et `reprendre`.
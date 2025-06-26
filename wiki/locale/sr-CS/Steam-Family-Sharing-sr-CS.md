# Steam porodično dijeljenje

ASF supports Steam Family Sharing - the old as well as the new system. In order to understand how ASF works with that, you should firstly read how **[Steam Family Sharing works](https://store.steampowered.com/promotion/familysharing)**, which is available on Steam store. In addition to that, check **[the news](https://store.steampowered.com/news/app/593110/view/4149575031735702628)** for upcoming new Steam Family Sharing system, which ASF is also compatible with.

---

## ASF

Podrška za Steam porodično dijeljenje je transparentna u ASF-u, što znači da se ne prave novi botovi/procesi u konfiguraciju - radi odmah kao dodatna funkcionalnost.

ASF prepoznaje ako neko igra i neće ga izbaciti iz igrice ako pokrenete igricu. ASF će biti kao primarni profil koji ima ključ, pa ako je taj ključ u vašem posjedovanju, ili u posjedovanju nekog sa kim je podijeljen, ASF neće farmati, odnosno, čekaće da se ključ oslobodi. This is mostly related to the old system - new system allows your family sharing users to play games other than those that ASF is currently farming.

In addition to above, after logging in, ASF will access your family sharing systems (old and new), from which it'll extract users (Steam IDs) allowed to use your library. Those users are given `FamilySharing` permission to use **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, especially allowing them to use `pause~` command on bot account that is sharing games with them, which allows to pause automatic cards farming module in order to launch a game that can be shared - this also mostly applies to the old system, but might still be used with the new system in case ASF is currently farming game that your users want to play.

Connecting both functionalities described above allows your friends to `pause~` your cards farming process, start a game, play as long as they wish, and then after they're done playing, cards farming process will be automatically resumed by ASF. Naravno, slanje `pause~` nije potrebno ako ASF ne farma nešto trenutno, zato što vaši prijatelji mogu pokrenuti igricu odmah, i oni neće biti izbačeni iz igrice.

---

## Limitacije

Steam network često šalje netačne poruke za status korisnika ASF-u, što može prouzrokovati da ASF povjeruje da je moguće da nastavi sa radom, a za rezultat toga da vaš prijatelj bude izbačen. Ovo je isti problem kao onaj već pomenut u **[ovoj FAQ odrednici](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#asf-is-kicking-my-steam-client-session-while-im-playing--this-account-is-logged-on-another-pc)**. Mi predlažemo istu rješenje, tj. davanje vašem prijatelju `Operator` dozvolu (ili veću) i obavijestiti ga da koristi `pause` i `resume` komande.
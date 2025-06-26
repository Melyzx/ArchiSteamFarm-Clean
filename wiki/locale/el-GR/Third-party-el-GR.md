# Εφαρμογές τρίτων

Η ενότητα αυτή περιλαμβάνει διάφορες προσθήκες τρίτων που έχουν γραφτεί αποκλειστικά (ή κυρίως) για χρήση μαζί με το ASF. Κυμαίνονται από ASF plugins, μέχρι απλών διαδικτυακών εφαρμογών και εσωτερικών βιβλιοθηκών για ενσωμάτωση, που τελειώνουν με πλήρη χαρακτηριστικά bots για άλλες πλατφόρμες. Αν θέλετε να προσθέσετε κάτι στη λίστα, ενημερώστε μας στο Discord ή στην ομάδα στο Steam.

Λάβετε υπόψη ότι τα ακόλουθα προγράμματα **δεν** συντηρούνται από προγραμματιστές ASF και ως εκ τούτου **δεν δίνουμε καμία εγγύηση για κανένα**, ειδικά όσον αφορά τους όρους ασφάλειας, την ασφάλεια ή τη συμμόρφωση με το Steam ToS. Ο κατάλογος αυτός διατηρείται μόνο για αναφορά. You should always ensure that the third-party utility you're about to use is safe and legit enough for you, as you're using all of them at your own risk.

---

## ASF plugins

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

- **[FriendAccepter](https://github.com/ezhevita/FriendAccepter)**, πρόσθετο για ASF να αποδέχεται αυτόματα όλες τις προσκλήσεις φίλου.
- **[GameRemover](https://github.com/ezhevita/GameRemover)**, πρόσθετο για ASF εφαρμογή μιας εντολής για να αφαιρέσετε άδειες Steam για επιλεγμένες περιπτώσεις bot.
- **[GetEmail](https://github.com/ezhevita/GetEmail)**, πρόσθετο για ASF εφαρμογή μιας εντολής για τη λήψη της διεύθυνσης ηλεκτρονικού ταχυδρομείου των δοσμένων παρουσιών bot απευθείας από το Steam.
- **[ResetAPIKey](https://github.com/ezhevita/ResetAPIKey)**, πρόσθετο για ASF, εντολή για να επαναφέρετε το κλειδί API για επιλεγμένες παρουσίες bot.

### Άλλο

- **[ASFEnhance](https://github.com/chr233/ASFEnhance)**, πρόσθετο για ASF, για ενίσχυση με διάφορα νέα χαρακτηριστικά, ειδικά εντολές.
- **[ASFFreeGames](https://github.com/maxisoft/ASFFreeGames)**, plugin for ASF allowing one to automatically collect free steam games posted on reddit.

---

## Ενσωματώσεις

- **[ASFBot](https://github.com/dmcallejo/ASFBot)**, telegram bot γραμμένο σε python με ενσωμάτωση ASF.
- **[ASF STM userscript](https://greasyfork.org/en/scripts/404754-asf-stm)**, για όσους θέλουν να στείλουν προσφορές αυτοματοποιημένων συναλλαγών σε bots στη δικιά μας **[ASF STM λίστα](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin#publiclisting)** μέσω web browser, χωρίς να χρησιμοποιήσετε τη λειτουργία `MatchActively` που παρέχεται από ASF.
- **[simple-asf-bot](https://github.com/deluxghost/simple-asf-bot)**, another (minimal) telegram bot written in python featuring ASF integration.

---

## Βιβλιοθήκες

- **[ASF-IPC](https://github.com/deluxghost/ASF_IPC)**, βιβλιοθήκη python για περαιτέρω ενσωμάτωση στη διεπαφή IPC του ASF.

---

## Συσκευασία

- **[AUR repo #1](https://aur.archlinux.org/packages/asf)**, επιτρέποντάς σας να εγκαταστήσετε εύκολα το ASF σε arch linux.
- **[AUR repo #2](https://aur.archlinux.org/packages/archisteamfarm-bin)**, επιτρέποντάς σας να εγκαταστήσετε εύκολα το ASF σε arch linux.
- **[Homebrew](https://formulae.brew.sh/formula/archi-steam-farm)**, επιτρέποντάς σας να εγκαταστήσετε εύκολα το ASF στο macOS.
- **[Nix](https://search.nixos.org/packages?channel=unstable&show=ArchiSteamFarm&from=0&size=50&sort=relevance&type=packages&query=ArchiSteamFarm)**, επιτρέποντάς σας να εγκαταστήσετε εύκολα το ASF σε distros με Nix.
- **[NixOS](https://search.nixos.org/options?channel=unstable&from=0&size=50&sort=relevance&type=packages&query=ArchiSteamFarm)**, επιτρέποντάς σας να ρυθμίσετε και να εγκαταστήσετε το ASF με NixOS.
- **[Winget](https://github.com/microsoft/winget-pkgs/tree/master/manifests/j/JustArchiNET/ArchiSteamFarm)**, allowing you to easily install ASF on windows.

---

## Εργαλεία

- **[Keys extractor](https://umaim.github.io/SKE)**, σας επιτρέπει να αντιγράψετε τα κλειδιά σε διάφορες μορφές και να δημιουργήσετε `redeem` εντολή για το ASF. Ελέγξτε το **[GitHub repo](https://github.com/PixvIO/SKE)** για περισσότερες λεπτομέρειες.
- **[ASF Mass Config Editor](https://github.com/genesix-eu/ASF_MCE)**, το οποίο επιτρέπει την εύκολη διαχείριση πολλαπλών ASF ρυθμίσεων.

---

## Θέλετε να βρείτε περισσότερα;

Συνιστούμε τη συζήτηση **[ArchiSteamFarm](https://github.com/topics/archisteamfarm)** στο GitHub για όλα τα έργα που ενσωματώνονται με ASF.
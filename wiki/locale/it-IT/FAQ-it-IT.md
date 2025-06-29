# FAQ

La nostra FAQ di base copre le domande standard e le risposte che potresti avere. Invece per questioni meno comuni, per favore visita il nosto **[FAQ esteso](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Extended-FAQ)**.

# Tabella dei contenuti

* [Generale](#general)
* [Confronto con strumenti simili](#comparison-with-similar-tools)
* [Sicurezza / Privacy / VAC / Ban / TdS](#security--privacy--vac--bans--tos)
* [Varie](#misc)
* [Problemi](#issues)

---

## Generale

### Cos'è ASF?
### Why does the program claim that there is nothing to farm on my account?
### Why ASF doesn't detect all of my games?
### Perché il mio profilo è limitato?

Prima di provare a comprendere cosa ASF sia, dovresti assicurarti di aver capito cosa sono le schede di Steam, e come ottenerle, ben descritto nella FAQ ufficiale **[qui](https://steamcommunity.com/tradingcards/faq)**.

In breve, le carte di Steam sono oggetti raccoglibili che sei idoneo a ricevere quando possiedi un particolare gioco, e sono utilizzabili per creare distintivi, vendere sul mercato di Steam od ogni altro scopo di tua scelta.

Core points are stated once again here, because people in general don't want to agree with them and prefer to pretend that those do not exist:

- **Devi possedere il gioco sul tuo profilo di Steam per essere idoneo a ricevere ogni carta da esso. La condivisione familiare non conta.**
- **Your game can't be marked as [private](https://help.steampowered.com/faqs/view/1150-C06F-4D62-4966), ASF will automatically skip such games during farming.**
- **Non puoi farmare infinitamente il gioco, ogni gioco ha un numero fisso di carte ottenibili. Once you drop all of them (around a half of the full set), the game is not a candidate for farming anymore. Non importa che tu abbia venduto, scambiato, creato o dimenticato cosa sia successo a quelle carte che hai ottenuto, una volta finite le carte ottenibili, il gioco è finito.**
- **Non puoi ottenere carte dai giochi F2P senza spendere denaro in essi. This means permanently F2P games like Team Fortress 2 or Dota 2. Owning F2P games does not grant you card drops.**
- **Non puoi ottenere carte su [profili limitati](https://help.steampowered.com/faqs/view/71D3-35C2-AD96-AA3A), indipendentemente dai giochi posseduti. Era possibile in passato, ma non è più il caso.**
- **Paid games that you've obtained for free during a promotion can't be farmed for card drops, regardless of what is displayed on the store page. Era possibile in passato, ma non è più il caso.**

Quindi, come puoi vedere, le carte di Steam ti sono assegnate giocando un gioco che hai comprato, o un gioco F2P in cui hai speso soldi. Se giochi abbastanza a lungo a tale gioco, tutte le carte per quel gioco andranno nel tuo inventario, rendendoti possibile di completare un distintivo (dopo aver ottenuto metà del mazzo), venderle o fare qualsiasi altra cosa tu voglia.

Ora che abbiamo spiegato le basi di Steam, possiamo spiegare ASF. Il programma stesso è abbastanza complesso da comprendere a pieno, quindi invece di scavare in tutti i dettagli tecnici, offriremo una spiegazione molto semplificata sotto.

ASF accede al tuo profilo di Steam tramite la nostra implementazione del client di Steam personalizzata usando le credenziali da te fornite. After successfully logging in, it parses your **[badges](https://steamcommunity.com/my/badges)** in order to find games that are available for farming (`X` card drops remaining). Dopo aver analizzato tutte le pagine e costruito l'elenco finale di giochi disponibili, ASF sceglie l'algoritmo di farming più efficiente e avvia il processo. The process depends upon chosen **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** but usually it consists of playing eligible game and periodically (plus on each item drop) checking if game is fully farmed already - if yes, ASF can proceed with the next title, using the same procedure, until all games are fully farmed.

Tieni a mente che la spiegazione sotto è semplificata e non descrive dozzine di funzionalità extra e funzioni che ASF offre. Visita il resto della **[nostra wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki)** se vuoi sapere ogni dettaglio di ASF. Ho provato a renderlo abbastanza semplice da esser comprensibile per tutti, senza portare i dettagli tecnici; gli utenti avanzati sono incoraggiati a scavare più in profondità.

Ora come un programma, ASF offre della magia. Prima di tutto, non deve scaricare ogni file di gioco, può giocare da subito ai giochi. Secondo, è interamente indipendente dal tuo client di Steam normale, non devi averlo in esecuzione, né installato. Terzo, è una soluzione automatizzata, a significare che ASF fa automaticamente tutto per conto tuo, senza doverti dire ciò che fa, il che ti risparmia tempo e fastidi. Infine, non deve ingannare la rete di Steam per emulazione del processo (che ad es. Idle Master sta usando), poiché può comunicarvi direttamente. Inoltre è super veloce e leggero, essendo una soluzione fantastica per tutti coloro che vogliono ottenere facilmente le carte senza troppo fastidio; diventa specialmente utile lasciandolo in esecuzione in background mentre si fa altro o persino giocando in modalità offline.

All of the above is nice, but ASF also has some technical limitations that are enforced by Steam - we can't farm games that you don't own, we can't farm games forever in order to get extra drops past the enforced limit, and we can't farm games while you're playing. All of that should be "logical", considering the way how ASF works, but it's nice to note that ASF doesn't have super-powers and won't do something that is physically impossible, so keep that in mind - it's basically the same as if you told someone to log in on your account from another PC and farm those games for you.

Quindi per riassumere, ASF è un programma che ti aiuta a ottenere queste carte per cui sei idoneo, senza troppi fastidi. Offre anche diverse altre funzioni, ma per ora atteniamoci solo a questa.

---

### Devo inserire le credenziali del mio profilo?

**Sì**. ASF richiede le tue credenziali del profilo così come il client ufficiale di Steam, usando lo stesso metodo per l'interazione di rete di Steam. Questo comunque non significa che devi inserire le credenziali del tuo profilo nelle configurazioni di ASF, puoi continuare a usare ASF con `null`/empty `SteamLogin` e/0 `SteamPassword` e inserire quei dati a ogni esecuzione di ASF, quando richiesti (nonché diverse altre credenziali di accesso, riferisciti alla configurazione). Così i tuoi dettagli non sono salvati da nessuna parte, ma ovviamente ASF non può avviarsi da solo senza il tuo aiuto. ASF offre anche diversi altri modi per aumentare la tua **[sicurezza](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)**, quindi sentiti libero di leggere quella parte della wiki se sei un utente avanzato. Se non lo sei, e non vuoi inserire le credenziali del tuo profilo nelle configurazioni di ASF, allora semplicemente non farlo, inseriscile invece quando necessario, quando ASF le richiede.

Tieni a mente che lo strumento ASF è per il tuo uso personale e che le tue credenziali non abbandonano mai il tuo computer. Inoltre non le condividi con nessuno, soddisfacendo i **[TdS di Steam](https://store.steampowered.com/subscriber_agreement)**, una cosa molto importante di cui le persone si dimenticano. Non invii i tuoi dettagli ai nostri server o altre terze parti, solo direttamente ai server di Steam operati da Valve. Non conosciamo le tue credenziali e non siamo capaci di recuperarle da te, indipendentemente dal fatto che tu le metta o no nelle tue configurazioni.

---

### Quanto devo attendere per ottenere le carte?

**Quanto ci vuole**, sul serio. Ogni gioco ha differenti difficoltà di farming impostate dallo sviluppatore/ediotre, e dipende totalmente da loro quanto velocemente sono ottenibili le carte. Gran parte dei giochi rilascia 1 carta ogni 30 minuti di gioco, ma ci sono anche giochi che ti richiedono di giocare anche diverse ore prima di ottenerne una. Inoltre, il tuo profilo potrebbe esser limitato dal ricevere carte dai giochi che non hai ancora giocato abbastanza, come dichiarato nella sezione **[prestazioni](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**. Non tentare di indovinare quanto a lungo ASF dovrebbe farmare il dato titolo; la decisione non dipende da te, né da ASF. Non c'è niente che tu possa fare per renderlo più veloce, e non esiste "bug" in relazione alle carte non rilasciate in modo tempestivo; non controlli il processo di ottenimento delle carte, né ASF. Nel migliore dei casi, riceverai la media di 1 carta ogni 30 minuti. Nel peggiore dei casi, non riceverai una carta anche per 4 ore dall'avvio di ASF. Entrambe le situazioni sono normali e coperte nella nostra sezione **[prestazioni](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**.

---

### Il farming richiede troppo tempo, posso velocizzarlo in qualche modo?

L'unica cosa che influenza pesantemente la velocità di farming è l'**[algoritmo di farming delle carte](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** per l'istanza del tuo bot. Ogni altra cosa ha effetti trascurabili e non renderà più veloce il farming, mentre alcune azioni come il lancio del processo di ASF diverse volte lo **peggiorerà**. Se hai davvero urgenza di ottenere da ogni singolo secondo dal processo di farming, allora ASF ti consente di affinare alcune variabili principali di farming come `FarmingDelay`, tutte spiegate in **[configurazione](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**. Tuttavia, come detto, l'effetto è trascurabile e scegliere l'algoritmo di farming delle carte adatto per il dato profilo è l'unica scelta cruciale che può pesantemente influenzare la velocità di farming, tutto il resto è puramente cosmetico. Invece di preoccuparti della velocità di farming, lancia ASF e fagli fare il suo lavoro, posso assicurarti che lo sta facendo nel modo più efficiente che potesse venirmi in mente. Meno ti importa, più sarai soddisfatto.

---

### Ma ASF ha detto che il farming avrebbe impiegato circa X tempo!

ASF approssima in base al numero di carte che devi ottenere, e il tuo algoritmo scelto; questo non è per nulla vicino al tempo reale che passerai farmando, solitamente più lungo, poiché ASF presume solo il miglior caso, e ignora tutte le stranezze della Rete di Steam, le disconnessioni da Internet, i sovraccarichi dei server di Steam e simili. Dovrebbe esser visto solo come un indicatore generale di quanto a lungo aspettarsi che ASF farmi, molto spesso nel caso migliore, poiché il tempo reale differirà, anche significativamente in alcuni casi. Come accennato sopra, non provare a indovinare quanto a lungo il dato gioco sarà farmato, non dipende né da te né da ASF decidere.

---

### ASF può funzionare sul mio android/smartphone?

ASF is a C# program that requires working implementation of .NET. Android became a valid platform starting with .NET 6.0, however, there is currently a major blocker in making ASF happen on Android due to **[lack of ASP.NET runtime available on it](https://github.com/dotnet/aspnetcore/issues/35077)**. Even though there isn't a native option available, there are proper and working builds for GNU/Linux on ARM architecture, so it's totally possible to use something like **[Linux Deploy](https://play.google.com/store/apps/details?id=ru.meefik.linuxdeploy)** for installing Linux, then using ASF in such Linux chroot as usual.

When/If all ASF requirements are satisfied, we'll consider releasing an official Android build.

---

### Can ASF farm items from Steam games, such as CS:GO or Unturned?

**No**, è contro i **[TeC di Steam](https://store.steampowered.com/subscriber_agreement)** e Valve lo ha chiaramente dichiarato con l'ultima ondata di ban della community per il farming di oggetti di TF2. ASF è un programma di farming di carte di Steam, non di oggetti di gioco, non ha alcuna capacità di farming di oggetti di gioco e non è previsot aggiungere tale funzionalità in futuro, mai, principalmente per la violazione dei termini d'uso di Steam. Sei pregato di non chiedere a proposito, il meglio che potresti ottenere è una segnalazione da qualche utente e avere problemi. The same goes for all other types of farming, such as farming drops from CS:GO broadcasts. ASF si concentra esclusivamente sulle carte di scambio di Steam.

---

### Can I choose which games should be farmed?

**Sì**, in vari diversi modi. If you want to alter the default order of farming queue, then that's what `FarmingOrders` **[bot configuration property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)** can be used for. If you want to manually blacklist given games from being farmed automatically, you can use idle blacklist which is available with `fb` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. If you'd like to farm everything but give some apps priority over everything else, that is what idle priority queue available with `fq` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** can be used for. And finally, if you want to farm specific games of your choice only, then you can declare `FarmPriorityQueueOnly` in bot's **[`FarmingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#farmingpreferences)** in order to achieve this, together with adding your selected apps to idle priority queue.

Oltre a gestire il modulo di farming di carte automatico descritto sopra, puoi anche passare alla modalità manuale di ASF con il **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** `play`, o usare varie altre impostazioni esterne come `GamesPlayedWhileIdle`, una **[proprietà di configurazione del bnot](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)**.

---

### I'm not interested in card drops, I'd like to farm hours played instead, is that possible?

Sì, ASF ti consente di farlo in vari modi.

The best way to achieve that is to make use of **[`GamesPlayedWhileIdle`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#gamesplayedwhileidle)** configuration property, which will play your chosen appIDs when ASF has no cards to farm. If you'd like to play your games all the time, even if you do have card drops from other games, then you can combine it with **[`FarmPriorityQueueOnly`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#farmingpreferences)**, so ASF will farm only those games for card drops that you explicitly set, or, alternatively, **[`FarmingPausedByDefault`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#farmingpreferences)**, which will cause cards farming module to be paused until you unpause it yourself.

You can also make use of the **[`play`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands#commands-1)** command, which will cause ASF to play your selected games. However, keep in mind that `play` should be used only for games you want to play temporarily, as it's not a persistent state, causing ASF to revert back to default state e.g. upon disconnection from Steam network. Dunque, ti consigliamo di usare `GamesPlayedWhileIdle`, a meno che tu non voglia iniziare i giochi da te selezionati solo per un breve periodo di tempo, e poi tornare al flusso generale.

---

### I'm Linux / macOS user, will ASF farm games that are not available for my OS? Will ASF farm 64-bit games when I'm running it on 32-bit OS?

Sì, ASF non si preoccupa nemmeno di scaricare i file di gioco effettivi, quindi funzionerà con tutte le tue licenze collegate al tuo profilo di Steam, indipendentemente dalla piattaforma o i requisiti tecnici. Dovrebbe funzionare anche per i giochi collegati a una regione specifica (giochi bloccati per regione), anche quando non sei nella regione corrispondente, sebbene ciò non sia garantito (funzionava l'ultima volta che ho provato).

---

## Confronto con strumenti simili

---

### ASF è simile a Idle Master?

The only similarity is the general purpose of both programs, which is farming Steam games in order to receive card drops. Everything else, including the actual farming method, program structure, functionality, compatibility, used algorithms, especially the source code itself, is entirely different and those two programs have nothing common with each other, even the core foundation - IM is running on .NET Framework, ASF on .NET (Core). ASF è stato creato per risolvere i problemi di IM, impossibili da risolvere con una semplice modifica del codice, ecco perché ASF è stato scritto da zero, senza usare una singola linea di codice o persino idea generale da IM, poiché il codice e tali idee erano completamente difettosi. IM e ASF sono come Windows e Linux, sono entrambi sistemi operativi installabili sul tuo PC, ma non condividono quasi niente tra loro, se non il servire uno scopo simile.

Questo è anche il motivo per cui non dovresti comparare ASF a IM basandoti sulle aspettative di IM. Dovresti trattare ASF e IM come programmi interamente indipendenti con le proprie esclusive serie di funzionalità. Senza dubbio, alcune di esse si sovrappongono e puoi trovare in entrambe una funzionalità in particolare, ma molto raramente, poiché ASF serve il suo scopo con un approccio interamente differente se comparato a IM.

---

### Vale la pena usare ASF, se correntemente uso Idle Master e funziona bene per me?

**Sì**. ASF is much more reliable and includes many built-in functions that are **crucial** regardless of the way how you farm, that IM simply doesn't offer.

ASF has proper logic for **unreleased games** - IM will attempt to farm games that have cards added already, even if they weren't released yet. Of course, it's not possible to farm those games until release date, so your farming process will be stuck. Questo ti richiederà di aggiungerli alla blacklist, attendere il rilascio o saltarli manualmente. Neither of those solutions is good, and all of them require your attention - ASF automatically skips farming of unreleased games (temporarily), and returns back to them later when they are, completely avoiding the problem and dealing with it efficiently.

ASF ha anche una logica propria delle **serie** di video. There are many videos on Steam that have cards, yet are announced with wrong `appID` on the badges page, such as **[Double Fine Adventure](https://store.steampowered.com/app/402590)** - IM will falsely farm wrong `appID`, which will yield no drops and process being stuck. Ancora una volta, dovrai inserirli nella blacklist o saltarli manualmente, richiedendo in entrambi i casi la tua attenzione. ASF automatically discovers proper `appID` for farming which does result in card drops.

Oltre a ciò, ASF è **molto più stabile e affidabile** quando si tratta di problemi di rete e stranezze di Steam, funziona gran parte del tempo e non richiede la tua attenzione una volta configurato, mentre IM spesso si rompe per molte persone, richiede correzioni extra o semplicemente non funziona. Inoltre è anche totalmente dipendente dal client di Steam, il che significa che questo non funziona se il tuo client di Steam sta riscontrando problemi seri. ASF funziona propriamente finché può connettersi alla rete di Steam e non richiede l'esecuzione o persino l'installazione del client di Steam.

Those are 3 **very important** points why you should consider using ASF, as they directly affect everybody farming Steam cards and there is no way to say "this doesn't consider me", since Steam maintenances and quirks are happening to everybody. Ci sono dozzina di motivi extra più o meno importanti che potresti scoprire nel resto della FAQ. In breve, **sì**, dovresti usare ASF anche quando non necessiti di alcuna funzionalità extra di ASF disponibile quando comparato a IM.

Oltre a ciò, IM è ufficialmente interrotto e potrebbe rompersi completamente in futuro, senza nessuno che si preoccupi di correggerlo, considerando l'esistenza di soluzioni molto più potenti (non solo ASF). IM già non funziona per molte persone e quel numero è in aumento, non in discesa. Dovresti evitare di usare software obsoleti in primo luogo, non solo IM ma anche tutti gli altri programmi deprecati. No active maintainer means that nobody cares whether it works or not and **nobody is responsible for its functionality**, which is a crucial matter in terms of security. Basta dire che se ci dovesse essere un bug critico che causi problemi reali all'infrastruttura di Steam, senza nessuno pronto a risolverlo, Steam potrebbe emettere un'altra ondata di ban da cui saresti colpito senza nemmeno sapere di essere un problema, come già successo a molte persone che usavano, indovinate un po', versioni obsolete di ASF.

---

### Che funzionalità interessanti ha ASF da offrire che Idle Master non ha?

Dipende da cosa consideri "interessante" per te. If you plan to farm more accounts than one then the answer is already obvious since ASF allows you to farm all of them with one superior solution, saving resources, hassle, and compatibility issues. Tuttavia, se stai facendo questa domanda probabilmente non hai questa particolare esigenza, quindi valutiamo altri benefici che si applicano a un profilo singolo usato in ASF.

First and foremost, you have some built-in features mentioned **[above](#is-it-worth-it-to-use-asf-if-im-currently-using-idle-master-and-it-works-fine-for-me)** that are core for farming regardless of your end-goal, and very often that alone is already enough to consider using ASF. Ma questo lo sai già, quindi spostiamoci su alcune funzionalità più importanti:

- **You can farm offline** (`OnlineStatus` in `Offline` setting). Farming offline makes it possible for you to skip your Steam in-game status entirely, which allows you to farm with ASF while showing "Online" on Steam at the same time, without your friends even noticing that ASF is playing a game on your behalf. Questa è una funzionalità superiore, che ti consente di rimanere online nel tuo client di Steam, senza infastidire i tuoi amici con cambi di gioco costanti, o ingannandoli nel pensare che stai giocando a un gioco mentre in realtà non lo stai facendo. Questo punto da solo rende valido usare ASF se rispetti i tuoi amici, ma è solo l'inizio. Bello notare anche che questa funzionalità non ha nulla a che fare con le impostazioni di privacy di Steam: se lanci da solo il gioco, allora ti mostrerai propriamente come in gioco ai tuoi amici, rendendo ASF la parte invisibile e senza influenzare affatto il tuo profilo.

- **You can skip refundable games** (`SkipRefundableGames` in bot's `FarmingPreferences` feature). ASF has proper built-in logic for refundable games and you can configure ASF to not farm refundable games automatically. This allows you to evaluate yourself if your newly-bought game from Steam store was worth your money, without ASF trying to drop cards from it as soon as possible. If you play it for 2+ hours, or 2 weeks pass since your purchase, then ASF will proceed with that game as it's not refundable anymore. Until then you have full control whether you enjoy it or not and you can easily refund it if needed, without having to manually blacklist that game or not use ASF for entire duration.

- **You can skip unplayed games** (`SkipUnplayedGames` in bot's `FarmingPreferences` feature). ASF has proper built-in logic for hours in games and you can configure ASF to not farm unplayed games automatically. This allows you to control yourself the games you play and farm, without having to manually blacklist all of them or skip using ASF entirely.

- **You can automatically mark new items as received** (`BotBehaviour` of `DismissInventoryNotifications` feature). Farming with ASF will result in your account receiving new card drops. You already know that this is going to happen, so let ASF clear that useless notification for you, ensuring that only important things will raise your attention. Of course, only if you want to.

- **You can customize preferred farming order with more available options** (`FarmingOrders` feature). Perhaps you want to farm your newly bought games first? Or your oldest ones? According to number of card drops? Badge levels you already crafted? Played hours? Alphabetically? According to AppIDs? Or maybe fully random? That's entirely up to you to decide.

- **ASF can help you complete your sets** (`TradingPreferences` with `SteamTradeMatcher` feature). With a bit more advanced tinkering, you can convert your ASF into fully-featured user-bot that will automatically accept **[STM](https://www.steamtradematcher.com)** offers, helping you each day to match your sets without any user interaction. ASF even includes its very own ASF 2FA module allowing you to import your Steam mobile authenticator and let you fully automate the entire process with accepting confirmations as well. Or, maybe you want to accept manually and let ASF only prepare those trades for you? That's once again, fully up to you to decide.

- **ASF can redeem keys in background for you** (**[background games redeemer](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)** feature). Maybe you have a hundred of keys from various bundles that you're too lazy to redeem yourself, going through bunch of windows and agreeing to Steam terms and conditions over and over again. Why not copy-paste your list of keys into ASF and let it do its job? ASF will automatically redeem all of those keys in background, providing you with appropriate output to let you know how each redeem attempt turned out. Moreover, if you have hundreds of keys, you're guaranteed to get rate-limited by Steam sooner or later, and ASF also knows about that, it'll patiently wait for the rate-limit to go away, and resume where it left.

We could now go on and on with entire **[ASF wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home)**, pointing out every single feature of the program, but we have to draw a line somewhere. This is it, this is a list of features that you can enjoy as ASF user, where just one of those could easily be considered a major reason to never look back, and we actually listed **a lot** of them, with even more you can learn about on the rest of our wiki. Ah yes, and we didn't even go into detail with things like ASF's API allowing you to script your own commands, or awesome bots management, since we wanted to keep it simple.

---

### Is ASF faster than Idle Master?

**Yes**, although the explanation is rather complicated.

On each new process spawned and terminated on your system, steam client automatically sends a request containing all of your games that you're currently playing - this way steam network can calculate hours and make cards drop. However, steam network counts your time played in 1-second intervals, and sending new request resets the current status. In other words, if you did spawn/kill new process every 0.5 second, you'd never drop any card because every 0.5 second steam client would send a new request and steam network would never count even 1 second of play time. Moreover, because of how operating system works, it's actually quite common to see new processes being spawned/terminated without you even doing anything, so even if you're doing nothing on your PC - there are many processes still working in the background, spawning/terminating other processes all the time. Idle master is based on steam client, so this mechanism affects you if you're using it.

ASF is not based on steam client, it has its own steam client implementation. Thanks to that, what ASF is doing, is not spawning any process, but actually sending one, real request to steam network that we started playing a game. This is the same request that would be sent by steam client, but because we have actual control over the ASF steam client, we don't need to spawn new processes, and we're not mimicking steam client regarding send request on every process change, so the mechanism explained above doesn't affect us. Thanks to that, we never interrupt that 1 second interval on steam web side, and that enhances our farming speed.

---

### But is the difference really noticeable?

No. The interrupts that are happening with normal steam client and idle master have negligible effect on the card drops, so it's not any noticeable difference that would make ASF superior.

However, there **is** a difference, and you can clearly notice that, as depending on how busy your OS is, cards **will** drop faster, from a few seconds to even a few minutes, if you're extremely unlucky. Although I wouldn't consider using ASF only because it drops cards faster, as both ASF and Idle Master are affected by how steam web works, ASF just interacts with steam web more effectively, while Idle Master can't control what steam client is actually doing (so it's not Idle Master's fault, but steam client's itself).

---

### Can ASF farm multiple games at once?

**Yes**, although ASF knows better when to use that feature, based on selected **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**. Card drops rate when farming multiple games is close to zero, this is why ASF is using multiple games farming exclusively for hours in order to overcome `HoursUntilCardDrops` faster, for up to `32` games at once. This is also why you should focus on configuration part of the ASF, and let algorithms decide what is the best way to achieve the goal - what you think is right, is not necessarily right in reality, farming multiple games at once will not provide you with any card drops.

---

### ASF può saltare i giochi velocemente?

**No**, ASF non supporta né incoraggia l'uso di **[glitch di Steam](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance#steam-glitches)**.

---

### Can ASF automatically play each game for X hours before cards are added?

**No**, the whole point of Steam cards system change was to fight with false statistics and ghost players. ASF won't contribute towards that more than necessary, adding such feature is not planned and won't happen. If your game receives card drops in usual way, ASF will farm them as soon as possible.

---

### Can I play a game while ASF is farming?

**No**. ASF unlike IM has independent Steam client included, and Steam network allows only **one Steam client at a time** to play a game. You can however disconnect ASF any time you like by starting a game (and clicking "OK" when asked if Steam network should disconnect other client) - ASF will then patiently wait till you're done playing, and resume the process afterwards. Alternatively, you can still play in offline mode anytime you like, if that is satisfying for you.

Keep in mind that cards drop rate when playing multiple games is close to 0 anyway, therefore there are no direct benefits from being able to do that with IM, while there are strong benefits of no interfering with other games launched with ASF, which is crucial e.g. VAC-wise.

---

## Sicurezza / Privacy / VAC / Ban / TdS

---

### Can I get VAC ban for using this?

No, it's not possible because ASF (unlike Idle Master or SAM) does not interfere in any way with steam client nor its processes. It's physically impossible to get VAC ban for using ASF, even during playing on secured servers while ASF is running - this is because **ASF doesn't even require Steam Client being installed at all** in order to work properly. ASF is one of the only few farming programs that can currently guarantee being VAC-free.

---

### Can using ASF prevent me from playing on VAC-secured servers, as stated **[here](https://help.steampowered.com/faqs/view/22C0-03D0-AE4B-04E8)**?

ASF does not require Steam client being running or even installed at all. According to this concept, it should **not** cause any VAC-related issues, because ASF guarantees lack of interfering with Steam client and all its processes - this is the main point when talking about VAC-free guarantee that ASF offers.

According to users and best of my knowledge, this is the case right now, as nobody reported any issues like stated in the link above while using ASF. We couldn't reproduce the issue above with ASF as well, while clearly reproducing it with Idle Master.

However, keep in mind that Valve could still add ASF to the blacklist at some point, but it's a complete nonsense as even if they do that, you could still play VAC-secured games from your PC, and use ASF at the same time e.g. on your server, so I'm pretty sure that they know very well that ASF should not be a suspect VAC-wise, and they won't make our lifes harder by blacklisting ASF for no reason. Still, in the worst case you'll be unable to play, like stated above, because VAC-free guarantee of ASF is still here regardless if Steam blacklists ASF binary, or not (and you can still launch ASF on any other machine without Steam client being installed at all). Right now there is no need to do any of that, and let's hope it stays like this.

---

### Is it safe?

If you ask if ASF is safe as a software, which means that it won't cause any damage to your computer, won't steal your private data, install viruses or any other stuff like that - it is safe. ASF is free of malware, data stealing, cryptocurrency miners and any (and all) other doubtful behaviour that can be considered malicious or unwanted by the user. In addition to that we have a dedicated **[remote communication](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication)** section which covers our privacy policy and ASF behaviour that goes beyond what you configured the program to do yourself.

Our code is open-source, and distributed binaries are always compiled from **[publicly available sources](https://en.wikipedia.org/wiki/Open-source_software)** by **[automated and trusted continuous integration systems](https://en.wikipedia.org/wiki/Build_automation)**, and not even developers themselves. Each build is reproducible by following our build script and will result in exactly the same, **[deterministic](https://en.wikipedia.org/wiki/Deterministic_system)** IL (binary) code. If you for whatever reason don't trust our builds, you can always compile and use ASF from source, including all libraries that ASF is using (such as SteamKit2), which are open-source too.

In the end however, it's always a matter of trust to the developer(s) of your application, so you should decide yourself if you consider ASF safe or not, potentially supporting your decision with technical arguments specified above. Do not blindly believe something only because I said so - check yourself, as that's the only way to make sure.

---

### Can I get banned for this?

In order to answer that question, we should take a closer look at **[Steam ToS](https://store.steampowered.com/subscriber_agreement)**. Steam doesn't prohibit using of multiple accounts, in fact, **[it allows it](https://help.steampowered.com/faqs/view/7EFD-3CAE-64D3-1C31#share)** implying that you can use same mobile authenticator on more than one account. What it however doesn't allow is sharing accounts with other people, but we're not doing that here.

The only real point that considers ASF is the following:
> You may not use Cheats, automation software (bots), mods, hacks, or any other unauthorized third-party software, to modify or automate any Subscription Marketplace process.

The question is what in fact is Subscription Marketplace process. As we can read:

> An example of a Subscription Marketplace is the Steam Community Market

We're not modifying or automating subscription marketplace process, if by subscription marketplace we understand steam community market or steam store. However...

> Valve may cancel your Account or any particular Subscription(s) at any time in the event that (a) Valve ceases providing such Subscriptions to similarly situated Subscribers generally, or (b) you breach any terms of this Agreement (including any Subscription Terms or Rules of Use).

Therefore, as with every Steam software, ASF is not authorized by Valve and I cannot guarantee that you won't be suspended if Valve suddenly decides that they're banning accounts using ASF now. This is extremely unlikely considering the fact that ASF is being used on more than a few million of Steam accounts, but still a possibility, regardless of actual probability.

Especially because:
> In regard to all Subscriptions, Content and Services that are not authored by Valve, Valve does not screen such third-party content available on Steam or through other sources. Valve assumes no responsibility or liability for such third party content. Some third-party application software is capable of being used by businesses for business purposes - however, you may only acquire such software via Steam for private personal use.

However, Valve clearly acknowledges "Steam idlers" existing, as stated **[here](https://help.steampowered.com/faqs/view/22C0-03D0-AE4B-04E8)**, so if you asked me, I'm pretty sure that if they weren't fine with them, they'd already do something instead of pointing out that they could cause problems VAC-wise. The key word here is **Steam** idlers, for example ASF, and not **game** idlers.

Please note that above is only our interpretation of **[Steam ToS](https://store.steampowered.com/subscriber_agreement)** and various points - ASF is licensed under Apache 2.0 License, which clearly states:

```
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
```

You're using this software at your own risk. It's extremely unlikely that you can get banned for that, but if you do, you can blame only yourself for that.

---

### Did anybody get banned for it?

**Yes**, we had at least a few incidents so far that resulted in some kind of Steam suspension. Whether ASF itself was the root cause or not is entirely different story that we'll probably never get to know.

First case involved a guy with over 1000+ bots getting trade banned (together with all bots), most likely due to excessive usage of `loot ASF` executed on all bots at once, or other suspicious one-side amount of trades in a very short time.

> Hello XXX, Thank you for contacting Steam Support. It looks like this account was used to manage a network of bot accounts. Botting is a violation of the Steam Subscriber Agreement.

Please, use some common sense and don't assume that you can do such crazy things only because ASF allows you to do that. Doing `loot ASF` on over 1k of bots can be easily considered a **[DDoS](https://en.wikipedia.org/wiki/DDoS)** attack, and personally I'm not shocked that somebody got banned for such a thing. Keep minimum of fair use in regards to Steam service, and **most likely** you'll be fine.

Second case involved a guy with 170+ bots getting banned during Steam's 2017 Winter Sale.

> Your account was blocked for violation of the agreement of the subscriber Steam. Judging by the exchanges and other factors, this account was used to illegally collect collectible cards on Steam, as well as related and not only commercial activities. The account has been permanently blocked and Steam Support can not provide additional support on this issue.

This case is once again very hard to analyze, because of vague response from Steam support that barely offers any details. Based on my personal thoughts, this user probably exchanged Steam cards for some kind of money (level up bot?) or in some other way tried to cash out on Steam. In case you were unaware, this is also illegal according to **[Steam ToS](https://store.steampowered.com/subscriber_agreement)**. We're not in position to tell you what you can do with the trading cards obtained through ASF - but the user in question definitely didn't just craft badges with them.

Third case involved user with 120+ bots being banned for breach of **[Steam online conduct](https://store.steampowered.com/online_conduct?l=english)**.

> Hello XXX, Thank you for contacting Steam Support. This and other accounts were used for flooding our network infrastructure, which is a violation of Steam online conduct. The account has been permanently blocked and Steam Support can not provide additional support on this issue.

This case is a bit easier to analyze because of extra details provided by the user. Apparently the user was using **a very outdated ASF version** that included a bug causing ASF to send excessive number of requests to Steam servers. The bug itself did not exist at first but was activated due to Steam breaking change that was fixed in future version. **ASF is supported only in [latest stable version](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest) released on GitHub**. Software is written by humans, and humans tend to make mistakes. If the mistake has a global scope, it's quickly being patched up and released to all users as a bugfix. Valve won't suddenly ban over a million of ASF users due to my mistake, for obvious reasons. However, if you intentionally resign from using up-to-date ASF, then by definition you're in a very small minority of users that are **exposed to incidents like these** due to **no support**, as there is nobody watching over your outdated version of ASF, nobody fixing it and nobody ensuring that you won't get outright banned by just launching it. **Please use up-to-date software**, not only ASF, but all other applications as well.

The most recent case happened around June of 2021, according to the user:

> Using your program, I have been making booster packages with 28 accounts for 3 years and with 128 accounts for the last 6 months. I was online with maximum 15 accounts simultaneously to make booster packs and send them to the main account. Last month, I increased the number of online accounts simultaneously to 20, and 1 week after that, all of my accounts were banned. This email is not to blame you, on the contrary, I was always aware of the consequences. I wanted you to know what types of behavior result in a permanent ban.

It's hard to say whether increase in concurrent accounts online was the direct reason for the ban, I wouldn't count on that, instead I believe that the number of accounts alone was the main culprit, increased concurrency of online accounts probably just drew attention to the user in question, as he clearly had far more bots than our recommendation.

---

All of the incidents above have one thing in common - ASF is just a tool and it's **your** decision how you're going to make use of it. You do not get banned for using ASF directly, but for **how** you're using it. It can be a helper tool farming just one single account, or a massive farming network made from thousands of bots. In any case, I'm not offering legal advice, and you should decide yourself about your ASF usage in the first place. I'm not hiding any information that could help you, e.g. the fact that ASF got some people banned (and in what context), as I have no reason to - it's your choice what you want to do with that information.

If you ask me - use some common sense, avoid owning more bots than our recommendation, do not send hundreds of trades at the same time, always use up-to-date ASF version and you _should_ be fine. Every single incident of this nature for **some reason** always happened to people that have disregarded our recommendation and decided that they know better than us how many bots they can run. Whether it's just a coincidence or some actual factor, that's up to you to decide. I'm not offering any legal advice, only giving you my thoughts that you can find useful, or disregard them entirely and operate only on facts linked above.

---

### What privacy information ASF discloses?

You can find detailed explanation in **[remote communication](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication)** section. You should review it if you care about your privacy, e.g. if you're wondering why accounts being used in ASF are joining our Steam group. ASF doesn't collect any sensitive information, and doesn't share it with any third-parties.

---

## Varie

---

### I'm using unsupported OS such as 32-bit Windows, can I still use the latest version of ASF?

Yes, and that version is not unsupported in any way, just not officially built. Check out **[compatibility](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility)** section for generic variant. ASF doesn't have any strong dependency upon the OS, and it can work anywhere where you can get a working .NET runtime, which includes 32-bit Windows, even if there is no `win-x86` OS-specific package from us.

---

### ASF is great! Can I make a donation?

Yes, and we're very happy to hear that you're enjoying our project! You can find various donation possibilities under every **[release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** and also **[on the main page](https://github.com/JustArchiNET/ArchiSteamFarm)**. It's nice to note that in addition to generic money donations we also accept Steam items, so nothing is stopping you from donating skins, keys or a small part of the cards that you've farmed with ASF if you'd like to. Thank you in advance for your generosity!

---

### I'm using Steam parental PIN to protect my account, do I need to input it somewhere?

Yes, you must set it in `SteamParentalCode` bot config property. This is mainly because ASF does access many protected parts of your Steam account and it's impossible for ASF to operate without it.

---

### I don't want ASF to farm any games by default, yet I want to use extra ASF features. Is this possible?

Yes, if you just want to start ASF with paused cards farming module, you can set `FarmingPausedByDefault` in `FarmingPreferences` bot config property in order to achieve that. This will allow you to `resume` it during runtime.

If you want to completely disable cards farming module and ensure that it'll never run without you explicitly telling it otherwise, then we recommend to set `FarmPriorityQueueOnly` in bot's `FarmingPreferences`, which instead of just pausing it, will disable the farming completely until you add the games to idle priority queue yourself.

With cards farming module paused/disabled, you can make use of extra ASF features, such as `GamesPlayedWhileIdle`.

---

### Can ASF minimize to tray?

ASF is a console app, there is no window to be minimized, because window is created for you by your OS. You can however use any third-party tool capable of doing so, such as **[RBTray](https://github.com/benbuck/rbtray)** for Windows, or **[screen](https://linux.die.net/man/1/screen)** for Linux/macOS. Those are only examples, there are many other apps with similar functionality.

---

### Does using ASF preserve eligibility for receiving booster packs?

**Sì**. ASF is using the same method to log in to Steam network as the official client, therefore it also preserves ability to receive booster packs for accounts that are being used in ASF. Moreover, preserving that ability doesn't even require logging in into Steam community, so you can safely use `OnlineStatus` in `Offline` setting if you'd like to.

---

### Is there any way to communicate with ASF?

Yes, through several different ways. Check out **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** section for more info.

---

### I'd like to help with ASF translation, what do I need to do?

Thank you for your interest! You can find all details in our **[localization](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization)** section.

---

### I have only one (main) account added to ASF, can I still issue commands through steam chat?

**Yes**, it's explained in **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands#notes)** section. You can do so through Steam group chat, although using **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)** could be easier for you.

---

### ASF seems to be working, but I'm not receiving any card drops!

Cards farming rate differs from game to game, as you can read in **[performance](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**. It takes a while, usually **several hours per game**, and you shouldn't expect cards to drop in a few minutes since launching a program. If you can see that ASF actively checks cards status, and switches the game after current one is fully farmed, then everything works fine. It's possible that you've enabled an option such as `DismissInventoryNotifications` of `BotBehaviour` which automatically dismisses inventory notifications. Check out **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** for details.

---

### How to completely stop ASF process for my account?

Simply shutdown the ASF process, for example by clicking [X] on Windows. If instead you want to stop a particular bot of your choice but keep other ones running, then take a look at `Enabled` **[bot config property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)**, or `stop` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. If you instead want to stop automatic farming process, yet keep ASF running for your account, then that's what `FarmingPausedByDefault` option of `FarmingPreferences` in **[bot config property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)** and `pause` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** is for.

---

### How many bots can I run with ASF?

ASF as a program doesn't have any hard upper limit of bot instances, so you can run as much as you have memory on your machine, however, you're still being limited by the Steam network and other Steam services. Currently you can run up to 100-200 bots with a single IP and a single ASF instance. It's possible to run more bots with more IPs and more ASF instances, by working around IP limitations. Keep in mind that if you're using that big amount of bots, you should control their number yourself, such as making sure that all of them in fact are logging in and working at the same time. ASF was not tweaked for that huge number of bots, and the general rule applies that **the more bots you have, the more issues you'll encounter**. Also notice that the limit above in general depends on many internal factors, it's approximation rather than a strict limit - you will most likely be able to run more/less bots than specified above.

ASF team suggests **owning** up to **10 Steam accounts in total**, and therefore also running up to **10 bots in total**. Anything above is not supported and done at your own risk, against our suggestion made here. This recommendation is based on internal Valve guidelines, as well as our own suggestions. Whether you're going to comply with this rule or not is your choice, ASF as a tool will not go against your own will, even if it'll result in your Steam accounts being suspended for doing so. Therefore, ASF will display you a warning if you'll go above what we recommend, but still allow you to run anything you want at your own risk and lack of our support.

---

### Can I run more ASF instances then?

You can run as many ASF instances on one machine as you like, assuming every instance has its own directory and its own configs, and account used in one instance is not used in another one. However, ask yourself why you want to do that. ASF is optimized to handle more than a hundred of accounts at the same time, and launching that hundred of bots in their own ASF instances degrades performance, takes more OS resources (such as CPU and memory), and causes a potential synchronization issues between standalone ASF instances, as ASF is forced to share its limiters with other instances.

Therefore, my **strong suggestion** is, always run maximum of one ASF instance per one IP/interface. If you have more IPs/interfaces, by all means you can run more ASF instances, with every instance using its own IP/interface or unique `WebProxy` setting. If you don't, launching more ASF instances is totally pointless, as you won't gain anything from launching more than 1 instance per a single IP/interface. Steam will not magically allow you to run more bots just because you've launched them in another ASF instance, and ASF doesn't limit you to begin with.

Of course, there are still valid use cases for multiple ASF instances on the same network interface, such as hosting ASF service for your friends with each friend having its own unique ASF instance in order to guarantee isolation between bots and even the ASF processes themselves, however, you're not circumventing any Steam limitations this way, that's entirely different purpose.

---

### What is the meaning of status when redeeming a key?

Status indicates how given redeem attempt turned out. There are many different statuses possible, most common ones include:

| Status                  | Description                                                                                                                                                                                                                    |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| NoDetail                | "OK" status indicating success - the key was successfully redemeed.                                                                                                                                                            |
| Timeout                 | Steam network didn't respond in given time, we don't know if the key was redeemed, or not (most likely was, but you can try again).                                                                                            |
| BadActivationCode       | The provided key is invalid (not recognized as any valid key by Steam network).                                                                                                                                                |
| DuplicateActivationCode | The provided key was already redeemed by some other account, or revoked by developer/publisher.                                                                                                                                |
| AlreadyPurchased        | Your account already owns `packageID` that is connected with this key. Keep in mind that this does not indicate whether the key is `DuplicateActivationCode` or not - only that it's valid and it wasn't used in this attempt. |
| RestrictedCountry       | This is region-locked key and your account is not in the valid region that is permitted to redeem it.                                                                                                                          |
| DoesNotOwnRequiredApp   | You can't redeem that key as you're missing some other app - mainly base game when you're attempting to redeem DLC package.                                                                                                    |
| RateLimited             | You made too many redeem attempts and your account was temporarily blocked. Try again in an hour.                                                                                                                              |

---

### Are you affiliated with any cards farming/idling service?

**No**. ASF is not affiliated with any service and all such claims are false. Your Steam account is your property and you can use your account in whatever way you wish, but Valve clearly stated in **[official ToS](https://store.steampowered.com/subscriber_agreement)** that:

> You are responsible for the confidentiality of your login and password and for the security of your computer system. Valve is not responsible for the use of your password and Account or for all of the communication and activity on Steam that results from use of your login name and password by you, or by any person to whom you may have intentionally or by negligence disclosed your login and/or password in violation of this confidentiality provision.

ASF is licensed on liberal Apache 2.0 License, which allows other developers to further integrate ASF with their own projects and services legally. However, such third-party projects utilizing ASF are not guaranteed to be secure, reviewed, appropriate or legal according to **[Steam ToS](https://store.steampowered.com/subscriber_agreement)**. If you want to know our opinion, **we strongly encourage you to NOT share ANY of your account details with third-party services**. If such service turns out to be a **typical scam**, you'll be left alone with the problem, most likely without your Steam account and ASF won't take any responsibility for third-party services claiming to be safe and secure, because ASF team did not authorize neither reviewed any of those. In other words, **you're using them at your own risk, against our suggestion made above**.

In addition to that, official **[Steam ToS](https://store.steampowered.com/subscriber_agreement)** clearly states that:

> You may not reveal, share or otherwise allow others to use your password or Account except as otherwise specifically authorized by Valve.

It's your account and your choice. Just don't say that nobody warned you. ASF as a program meets all rules mentioned above, as you're not sharing your account details with anyone, and you're using the program for your own personal use, but any other "cards farming service" does require from you your account credentials, so it also violates the rule above (actually several of them). Like with **[Steam ToS](https://store.steampowered.com/subscriber_agreement)** evaluation, we're not offering any legal advice, and you should decide yourself if you want to use those services, or not - according to us **it directly violates [Steam ToS](https://store.steampowered.com/subscriber_agreement)** and may result in suspension if Valve finds out. Like pointed out above, **we strongly recommend to NOT use any of such services**.

---

## Problemi

---

### One of my games is being farmed for more than 10 hours now, but I still didn't get any cards from it!

The reason for that could be related to known issue of Steam, which happens when you have two licenses for the same game, one of which has card drops limited. This usually happens when you activate game for free during a mass giveaway on Steam, and then activate a key for the same game (but without limitations), e.g. from a paid bundle. If such situation happens, Steam reports on badge page that game still has cards to drop, but no matter how much you play the game - cards will never drop due to free license on your account. Since it's not an ASF issue, but a Steam one, we can't somehow circumvent it on ASF's side, and you need to solve it yourself.

There are two ways to solve the issue. Firstly, you can blacklist this game in ASF, either with `fbadd` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** or with `Blacklist` **[configuration property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**. This will prevent ASF from trying to farm cards from this game, but will not solve the underlying issue which prevents you from obtaining card drops from the affected game. Secondly, you can use Steam support self-service tool to remove free license from your account, leaving only full license that includes the card drops. In order to do so, firstly visit your **[licenses and product key activations](https://store.steampowered.com/account/licenses)** page and locate both free and paid license for the affected game. Usually it's fairly easy - both have similar name, but free one has "limited free promotional package" or other "promo" in the license name, plus "complimentary" in "acquisition method" field. Sometimes it might be more tricky, for example if free package was in some bundle and has a different name. If you have found two licenses like that - then it's indeed the issue described here, and you can safely remove free license without losing the game.

In order to remove the free license from your account, visit **[Steam support page](https://help.steampowered.com/wizard/HelpWithGame)** and put the affected game name into the search field, the game should be available in "products" section, click on it. Alternatively, you can just use `https://help.steampowered.com/wizard/HelpWithGame?appid=<appID>` link and replace `<appID>` with appID of the game that causes troubles. Afterwards, click on "I want to permanently remove this game from my account" and then select the faulty free license that you've found above, usually the one with "limited free promotional package" in the name (or similar). After removal of the free license, ASF should be able to drop cards from the affected game without issues, you should restart the farming operation after the removal just to be sure that Steam picks up the right license this time.

---

### ASF doesn't detect game `X` as available for farming, yet I know it includes Steam trading cards!

There are two main reasons here. First and most obvious reason is the fact that you're referring to **Steam store** where given game is announced as card drops enabled game. This is **wrong** assumption, as it simply states that the game **has** card drops included, but not necessarily this function for that game is **enabled** right away. You can read more about this in **[official announcement](https://steamcommunity.com/games/593110/announcements/detail/1954971077935370845)**.

In short, card drops icon in Steam store doesn't mean anything, check your **[badge pages](https://steamcommunity.com/my/badges)** for confirmation whether a game has card drops enabled or not - this is also what ASF is doing. If your game doesn't appear on the list as a game with cards possible to drop, then this game is **not** possible to farm, regardless of reason.

Second issue is less obvious, and it's the situation when you can see that your game indeed is available with card drops on your badge page, yet it's not being farmed by ASF right away. Unless you're hitting some other bug, such as ASF being unable to check badge pages (described below), it's simply a cache effect and on ASF side Steam is still reporting outdated badges page. This issue should solve itself sooner or later, when cache gets invalidated. There is also no way to fix this on our side.

Of course, all of that assumes that you're running ASF with default untouched settings, since you could also add this game to farming blacklist, use selected `FarmingPreferences` such as `FarmPriorityQueueOnly` or `SkipRefundableGames`, and so on.

---

### Why playtime of games farmed through ASF doesn't increase?

It does, but **not in real-time**. Steam records your playtime in fixed intervals and schedules update for it, but you're not guaranteed to have it updated immediately the moment you quit the session, let alone during such. Just because the playtime isn't updated in real-time doesn't mean that it's not recorded, it's usually updated every 30 minutes or so.

---

### What is the difference between a warning and an error in the log?

ASF writes to its log a bunch of information on various logging levels. Our objective is to explain **precisely** what ASF is doing, including what Steam issues it has to deal with, or other problems to overcome. Most of the time not everything is relevant, this is why we have two major levels being used in ASF in terms of problems - a warning level, and error level.

General ASF rule is that warnings are **not** errors, therefore they should **not** be reported. A warning is an indicator to you that something potentially unwanted happen. Whether it was Steam not reacting, API throwing errors or your network connection being down - it's a warning, and it means we expected it to happen, so don't bother ASF development with it. Of course you're free to ask about them or get help by using our support, but you shouldn't assume that those are ASF errors worth reporting (unless we confirm otherwise).

Errors on the other hand indicate a situation that should not happen, therefore they're worth reporting as long as you made sure that it's not you who is causing them. If it's a common situation that we expect to happen, then it'll be converted to a warning instead. Otherwise, it's possibly a bug that should be corrected, not silently ignored, assuming it's not a result of your own technical issue. For example, putting invalid content in `ASF.json` file will throw an error, as ASF won't be able to parse it, but it was you who put it there, so you should not report that error to us (unless you confirmed that ASF is wrong and your structure is in fact absolutely correct).

In one TL;DR sentence - report errors, don't report warnings. You can still ask about warnings and receive help in our support sections.

---

### ASF doesn't start, the program window closes immediately!

In normal conditions, any ASF crash or exit will generate a `log.txt` in the program's directory for you to view, which can be used for finding the cause of that. In addition to that, a few last log files are also archived in `logs` directory, since the main `log.txt` file is overwritten with each ASF run.

However, if even .NET runtime isn't able to boot on your machine, then `log.txt` will not be generated. If that happens to you then you most likely forgot to install .NET prerequisites, as stated in **[setting up](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up#os-specific-setup)** guide. Other common problems include trying to launch wrong ASF variant for your OS, or in other way missing native .NET runtime dependencies. If the console window closes too soon for you to read the message, then open independent console and launch ASF binary from there. For example on Windows, open ASF directory, hold `Shift`, right click inside the folder and choose "*open command window here*" (or *powershell*), then type into the console `.\ArchiSteamFarm.exe` and confirm with enter. This way you'll get precise message why ASF is not starting properly.

---

### ASF is kicking my Steam Client session while I'm playing! / *This account is logged on another PC*

This shows up as a message in Steam overlay that the account is being used somewhere else while you're playing. This issue can have two different reasons.

One reason is caused by broken packages (games) that specifically don't hold a playing lock properly, yet expect that lock to be possesed by the client. An example of such package would be Skyrim SE. Your Steam client launches the game properly, but that game doesn't register itself as being used. Because of that, ASF sees that it's free to resume the process, which it does, and that kicks you out of Steam network, as Steam suddenly detects that the account is being used in another place.

Second reason could come up if you're playing on your PC while ASF is waiting (especially on another machine) and you lose your network connection. In this case, Steam network marks you as offline and releases playing lock (like above), which triggers ASF (e.g. on another machine) into resuming farming. When your PC comes back online, Steam can't acquire playing lock anymore (that is now held by ASF, also similar to above) and shows the same message.

Both causes on the ASF side are actually very hard to workaround, as ASF simply resumes farming once Steam network informs it that account is free to be used again. This is what is happening normally when you close the game, but with broken packages this can happen immediately, even if your game is still running. ASF has no way to know whether you got disconnected, stopped playing a game or that you're still playing a game that doesn't hold playing lock appropriately.

The only proper solution to this problem is manually pausing your bot with `pause` before you start playing, and resuming it with `resume` once you're done. Alternatively you can just ignore the problem and act the same as if you played with offline Steam client.

---

### `Disconnected from Steam!` - I can't establish connection with Steam servers.

ASF can only **try** to establish connection with Steam servers, and it can fail due to many reasons, including lack of internet connection, Steam being down, your firewall blocking connection, third-party tools, incorrectly configured routes or temporary failures. You can enable `Debug` mode to check out more verbose log stating exact failure reasons, although usually it's simply caused by your own actions, such as using "CS:GO MM Server Picker" that blacklists a lot of Steam IPs, making it very hard for you to actually reach Steam network.

ASF will do its best to establish connection, which includes not only asking about updated list of servers but also trying another IP when last one fails, so if it's truly a temporary problem with some specific server or route, ASF will connect sooner or later. However, if you're behind firewall or in some other way unable to reach Steam servers, then obviously you need to fix it yourself, with potential help of `Debug` mode.

It's also possible that your machine is not able to establish connection with Steam servers using default protocol in ASF. You can alter protocols that ASF is permitted to use by modifying `SteamProtocols` global configuration property. For example, if you have problems reaching Steam with `UDP` protocol (e.g. due to firewalls), perhaps you'll have more luck with `TCP` or `WebSocket`.

In a very unlikely situation of having incorrect servers being cached, for example because of moving ASF `config` folder from one machine to another machine located in entirely different country, deleting `ASF.db` in order to refresh Steam servers on the next launch may help. Very often it's not needed and doesn't have to be done, as that list is automatically refreshed on first launch, as well as when the connection is established - we're just mentioning it as a way to purge anything related to list of Steam servers cached by ASF.

---

### `Unable to login to Steam: TryAnotherCM/Invalid`, `ServiceUnavailable/Invalid`

As per above, but this time the server you've connected with is explicitly unavailable. Usually happens during Steam maintenance window, there is nothing you can do about this, ASF will automatically retry with a different server until one happens to accept its request. It should not last longer than an hour maximum.

---

### `Could not get badges information, will try again later!`

Usually it means that you're using Steam parental PIN to access your account, yet you forgot to put it in ASF config. You must put valid PIN in `SteamParentalCode` bot config property, otherwise ASF will not be able to access most of web content, therefore will not be able to work properly. Head over to **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** in order to learn more about `SteamParentalCode`.

Other reasons include temporary Steam problem, network issue or likewise. If issue won't solve itself after several hours and you're sure that you configured ASF appropriately, feel free to let us know about that.

---

### ASF is failing with `Request failed after 5 tries` errors!

Usually it means that you're using Steam parental PIN to access your account, yet you forgot to put it in ASF config. You must put valid PIN in `SteamParentalCode` bot config property, otherwise ASF will not be able to access most of web content, therefore will not be able to work properly. Head over to **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** in order to learn more about `SteamParentalCode`.

If parental PIN is not the reason, then this is a most common error, and you should get used to that, it simply means that ASF sent a request to Steam Network, and didn't get a valid response, 5 times in a row. Usually it means that Steam is either down or is having some difficulties or maintenance - ASF is aware of such issues and you should not worry about them, unless they're happening constantly for longer than several hours, and other users do not have such problems.

How to check if Steam is being down? **[Steam Status](https://steamstat.us)** is an excellent source of checking if Steam **should be** up, if you notice errors, especially related to Community or Web API, then Steam is having difficulties. You may want to leave ASF alone and let it do its job after a short while of downtime, or quit it and wait yourself.

That's however not always the case, as in some situations Steam issues may not be detected by Steam Status, for example such case happened when Valve broke HTTPS support for Steam Community 7th June 2016 - accessing **[SteamCommunity](https://steamcommunity.com)** through HTTPS was throwing an error. Therefore, do not blindly trust Steam Status either, it's best to check yourself if everything works as supposed to.

In addition to that, Steam includes various rate-limiting measures which will temporarily ban your IP if you make excessive number of requests at once. ASF is aware of that and offers you several different limiters in the config, which you should make use of. Default settings were tweaked based on **sane** amount of bots, if you're using so huge amount that even Steam is telling you to go away, then you either tweak them until it no longer tells you to, or you do as you're told. I assume second way is not an option to you, so go read on that topic and pay special attention to `WebLimiterDelay` which is a general limiter that applies to all web requests.

There is no "golden rule" that works for everybody, because blocks are heavily influenced by third-party factors, that's why you have to experiment yourself and find a value that works for you. You can also ignore what I say and use something like `10000` which is guaranteed to work correctly, but then don't complain how your ASF reacts to everything in 10 seconds and how badge parsing takes 5 minutes. In addition to that, it's entirely possible that no limiter will do anything because you have so huge amount of bots that you're hitting **[hard limit](#how-many-bots-can-i-run-with-asf)** that was mentioned above. Yes, it's entirely possible that you'll be able to log in without issues into Steam network (client), but Steam web (website) will refuse to listen to you if you have 100 sessions established at once. ASF requires both Steam network and Steam web to be cooperative, it takes just one down to make you issues you won't recover from.

If nothing helps and you have no clue what is broken, you can always enable `Debug` mode and see yourself in ASF log why exactly requests are failing. Per esempio:

```text
InternalRequest() HEAD https://steamcommunity.com/my/edit/settings
InternalRequest() Forbidden <- HEAD https://steamcommunity.com/my/edit/settings
```

See that `Forbidden` code? This means that you got temporarily banned for excessive amount of requests, because you didn't tweak `WebLimiterDelay` properly yet (assuming you get the same error code for all other requests as well). There could be other reasons listed there, such as `InternalServerError`, `ServiceUnavailable` and timeouts that indicate Steam maintenance/issues. You can always try to visit the link mentioned by ASF yourself and check if it works - if it doesn't, then you know why ASF can't access that either. If it does, and the same error doesn't go away after a day or two, it may be worth investigating and reporting.

Before doing that you should **make sure that the error is worth reporting in the first place**. If it's mentioned in this FAQ, such as trading-related issue, then that's out. If it's temporary issue that happened once or twice, especially when your network was unstable or Steam was down - that's out. However, if you were able to reproduce your issue several times in a row, across 2 days, restarted ASF as well as your machine in the process and made sure that there is no FAQ entry here to help resolve it, then this may be worth asking about.

---

### ASF seems to freeze and doesn't print anything on the console until I press a key!

You're most likely using Windows and your console has QuickEdit mode enabled. Refer to **[this](https://stackoverflow.com/questions/30418886/how-and-why-does-quickedit-mode-in-command-prompt-freeze-applications)** question on StackOverflow for technical explanation. You should disable QuickEdit mode by right clicking your ASF console window, opening properties, and unchecking appropriate checkbox.

---

### ASF can't accept or send trades!

Obvious thing first - new accounts start as limited. Until you unlock account by loading its wallet or spending $5 in the store, ASF can't accept neither send trades using this account. In this case, ASF will state that inventory seems empty, because every card that is in it is non-tradable.

Next, if you do not use **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**, it's possible that ASF in fact accepted/sent trade, but you need to confirm it via your e-mail. Likewise, if you use classic 2FA, you need to confirm the trade via your authenticator. Confirmations are **mandatory** now, so if you don't want to accept them by yourself, consider importing your authenticator into ASF 2FA.

Also notice that you can trade only with your friends, and people with known trade link. If you're trying to initiate *Bot -> Master* trade, such as `loot`, then you need to either have your bot on your friendlist, or your `SteamTradeToken` declared in Bot's config. Make sure that the token is valid - otherwise, you won't be able to send a trade.

Lastly, remember that new devices have 7-days trade lock, so if you've just added your account to ASF, wait at least 7 days - everything should work after that period. That limitation includes **both** accepting **and** sending trades. It does not always trigger, and there are people who can send and accept trades instantly. Majority of the people are affected though, and the lock **will** happen, even if you can send and accept trades through your steam client on the same machine. Just wait patiently, there's nothing you can do to make it faster. Likewise, you may get similar lock for removing/changing various Steam security-related settings, such as 2FA, SteamGuard, password, e-mail and likewise. In general, check if you can send a trade from that account yourself, if yes, very likely it's classic 7-days lock from new device.

And finally, keep in mind that one account can have only 5 pending trades to another one, so ASF will fail to send trades if you have 5 (or more) pending ones from that one bot to accept already. This is rarely a problem, but it's also worth mentioning, especially if you set ASF to auto-send trades, yet you're not using ASF 2FA and forgot to actually confirm them.

If nothing helped, you can always enable `Debug` mode and check yourself why requests are failing. Please note that Steam talks nonsense most of the time, and provided reason may not make any logical sense, or can be even entirely incorrect - if you decide to interpret that reason, make sure you have decent knowledge about Steam and its quirks. It's also quite common to see that issue with no logical reason, and the only suggested solution in this case is to re-add account to ASF (and wait 7 days again). Sometimes this issue also fixes itself *magically*, the same way it breaks. However, usually it's just either 7-days trade lock, temporary steam problem, or both. It's best to give it a few days before manually checking what is wrong, unless you have some urge to debug the real cause (and usually you'll be forced to wait anyway, because error message won't make any sense, neither help you in the slightest).

In any case, ASF can only **try** to send a proper request to Steam in order to accept/send trade. Whether Steam accepts that request, or not, is out of the scope of ASF, and ASF will not magically make it work. There's no bug related to that feature, and there is also nothing to improve, because logic is happening outside of ASF. Therefore, do not ask for fixing stuff that is not broken, and also do not ask why ASF can't accept or send trades - **I don't know, and ASF doesn't know either**. Either deal with it, or fix yourself, if you know better.

---

### Why do I have to put 2FA/SteamGuard code on each login? / *Removed expired login key*

ASF uses login keys (if you kept `UseLoginKeys` enabled) for keeping credentials valid, the same mechanism that Steam uses - 2FA/SteamGuard token is required only once. However, due to Steam network issues and quirks, it's entirely possible that login key is not saved in the network, we've already seen such issues not only with ASF, but with regular steam client as well (a need to input login + password on each run, regardless of "remember me" option).

You could remove `BotName.db` and `BotName.bin` (if available) of affected account and try to link ASF to your account once again, but that likely won't do anything. Some users have reported that **[deauthorizing all devices](https://store.steampowered.com/twofactor/manage)** on Steam side should help, changing password will do the same. However, those are only workarounds that are not even guaranteed to work, the real ASF-based solution is to import your authenticator as **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** - this way ASF can generate tokens automatically when they're needed, and you don't have to input them manually. Usually the issue magically solves itself after some time, so you can simply wait for that to happen. Of course you can also ask Valve for solution, because I can't force Steam network to accept our login keys.

As a side note, you can also turn off login keys with `UseLoginKeys` config property set to `false`, but this will not solve the problem, only skip the initial login key failure. ASF is already aware of the issue explained here and will try its best to not use login keys if it can guarantee itself all login credentials, so there is no need to tweak `UseLoginKeys` manually if you can provide all login details together with using ASF 2FA.

---

### I'm getting error: *Unable to login to Steam: `InvalidPassword` or `RateLimitExceeded`*

This error can mean a lot of things, some of them include:

- Invalid Login/Password combination (obviously)
- Expired login key used by ASF for logging in
- Too many failed login attempts in short period of time (anti-bruteforce)
- Too many login attempts in short period of time (rate-limiting)
- Requirement of captcha to log in (very likely to be caused by two reasons above)
- Any other reason Steam Network may have preventing you from logging in

In case of anti-bruteforce and rate-limiting, problem will disappear after some time, so just wait and don't attempt to log in in the meantime. If you hit that issue frequently, perhaps it's wise to increase `LoginLimiterDelay` config property of ASF. Excessive program restarts and other intentional/non-intentional login requests definitely won't help with that issue, so try to avoid it if possible.

In case of expired login key - ASF will remove old one and ask for new one on next login (which will require from you putting 2FA token if your account is 2FA-protected. If your account is using ASF 2FA, token will be generated and used automatically). This can naturally happen over time, but if you get this issue on each login, it's possible that Steam for some reason decided to ignore our login key save requests, as mentioned in the issue **[above](#why-do-i-have-to-put-2fasteamguard-code-on-each-login--removed-expired-login-key)**. You can of course disable `UseLoginKeys` entirely, but that won't solve the issue, only avoid a need of removing expired login keys each time. The real solution, as per the issue above, is to use ASF 2FA.

And lastly, if you used wrong login + password combination, obviously you need to correct this, or disable bot that is attempting to connect using those credentials. ASF can't guess on its own whether `InvalidPassword` means invalid credentials, or any of the reasons listed above, therefore it'll keep trying until it succeeds.

Keep in mind that ASF has its own built-in system to react accordingly to steam quirks, eventually it will connect and resume its job, therefore it's not required to do anything if the issue is temporary. Restarting ASF in order to magically fix problems will only make things worse (as new ASF won't know previous ASF state of not being able to log in, and try to connect instead of waiting), so avoid doing that unless you know what you're doing.

Finally, as with every Steam request - ASF can only **try** to log in, using your provided credentials. Whether that request will succeed or not is out of the scope and logic of ASF - there is no bug, and nothing can be fixed neither improved in this regard.

---

### I can't input login details that ASF is asking for
### `System.InvalidOperationException: Cannot read keys when either application does not have a console or when console input has been redirected`
### `System.IO.IOException: Input/output error`
### `RequestInput() input is invalid!`

If this error happened during ASF input (e.g. you can see `GetUserInput()` in the stacktrace) then it's caused by your environment which prohibits ASF from reading standard input of your console. That can occur due to a lot of reasons, but the most common one is you running ASF in the wrong environment (e.g. in `systemd`, `nohup` or `&` background instead of `screen` on Linux). If ASF can't access its standard input, then you'll see this error logged and ASF's inability to use your details during runtime.

Normally you should correct the above issue, i.e. allow ASF to access standard input so you can supply the details. However, if you **expect** this to happen, so you **intend** to run ASF in input-less environment, then you should explicitly tell ASF that it's the case, by setting **[`Headless`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#headless)** mode appropriately. This will tell ASF to never ask for user input under any circumstance, allowing you to run ASF in input-less environments safely. You can answer selected input prompts through other means in this mode, e.g. in ASF-ui.

---

### `System.Net.Http.WinHttpException: A security error occurred`

This error happens when ASF can't establish secure connection with given server, almost exclusively because of SSL certificate mistrust.

In almost all cases this error is caused by **wrong date/time on your machine**. Every SSL certificate has issued date and expiry date. If your date is invalid and out of those two bounds then the certificate can't be trusted due to a potential **[MITM](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)** attack and ASF refuses to make a connection.

Obvious solution is to set the date on your machine appropriately. It's highly recommended to use automatic date synchronization, such as native synchronization available on Windows, or `ntpd` on Linux.

If you made sure that the date on your machine is appropriate and the error doesn't want to go away, SSL certificates that your system trusts could be out-of-date or invalid. In this case you should ensure that your machine can establish secure connections, for example by checking if you can access `https://github.com` with any browser of your choice, or CLI tool such as `curl`. If you confirmed that this works properly, feel free to ask us for support.

---

### `System.Threading.Tasks.TaskCanceledException: A task was canceled`

This warning means that Steam did not answer to ASF request in given time. Usually it's caused by Steam networking hiccups and does not affect ASF in any way. In other cases it's the same as request failing after 5 tries. Reporting this issue makes no sense most of the time, as we can't force Steam to respond to our requests.

---

### `The type initializer for 'System.Security.Cryptography.CngKeyLite' threw an exception`

This problem is almost exclusively caused by disabled/stopped `CNG Key Isolation` Windows service, which provides core cryptography functionality for ASF, without which the program isn't able to run. You can fix this issue by launching `services.msc` and ensuring that `CNG Key Isolation` Windows service doesn't have disabled startup and is currently running.

---

### ASF is being detected as a malware by my AntiVirus! What's going on?

**Ensure that you downloaded ASF from trusted source**. The only official and trusted source is **[ASF releases](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** page on GitHub (and this is also the source for ASF auto-updates) - **any other source is untrusted by definition and can contain malware added by other people** - you should not trust any other download location, ensure that your ASF always comes from us.

If you confirmed that ASF is downloaded from trusted source, then very likely it's simply a false positive. This **happened in the past**, **is happening right now**, and **will happen in the future**. If you're worried about actual safety when using ASF, then I suggest scanning ASF with many different AVs for actual detection ratio, for example through **[VirusTotal](https://www.virustotal.com)** (or any other web service of your choice like this).

If the AV that you're using falsely detects ASF as a malware, then **it's a good idea to send this file sample back to developers of your AV, so they can analyze it and improve their detection engine**, as clearly it's not working as good as you think it does. There is no issue in ASF code, and there is also nothing to fix for us, since we're not distributing malware in the first place, therefore it doesn't make any sense to report those false-positives to us. We highly recommend to send ASF sample for further analysis like stated above, but if you don't want to bother with it, then you can always add ASF to some kind of AV exceptions, disable your AV or simply use another one. Sadly, we're used to AVs being stupid, as every once in a while some AV detects ASF as a virus, which usually lasts very short and is being patched up quickly by the devs, but like we pointed out above - **it happened**, **happens** and **will happen** all the time. ASF doesn't include any malicious code, you can review ASF code and even compile from source yourself. We're not hackers to obfuscate ASF code in order to hide from AV heuristics and false positives, so do not expect from us to fix what is not broken - there is no "virus" for us to fix.
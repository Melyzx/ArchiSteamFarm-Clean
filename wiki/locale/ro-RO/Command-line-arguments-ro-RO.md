# Argumente din linie comandă

ASF include suport pentru mai multe argumente de linie de comandă care pot afecta rularea programului. Acestea pot fi folosite de utilizatorii avansați pentru a specifica cum ar trebui să ruleze programul. În comparație cu modul implicit al fișierului de configurare `ASF.json`, argumentele din linie de comanda sunt folosite pentru inițializarea de bază (de ex. `--path`), setări specifice platformei (ex. `--system-required`) sau date sensibile (ex. `--cryptkey`).

---

## Utilizare

Utilizarea depinde de sistemul de operare și versiune ASF.

Generic:

```shell
dotnet ArchiSteamFarm.dll --argument --altArgument
```

Windows:

```powershell
.\ArchiSteamFarm.exe --argument --altArgument
```

Linux/macOS:

```shell
./ArchiSteamFarm --argument --altArgument
```

Argumentele din linie de comandă sunt de asemenea acceptate în script-uri generice de ajutor cum ar fi `ArchiSteamFarm.cmd` sau `ArchiSteamFarm.sh`. În plus față de asta, poți folosi și proprietatea de mediu `ASF_ARGS`, așa cum este menționat în secțiunile noastre de **[management](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#environment-variables)** și **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker#command-line-arguments)**.

Dacă argumentul tău include spații, nu uita să folosești ghilimele. Aceste două sunt greșite:

```shell
./ArchiSteamFarm --path /home/archi/My Downloads/ASF # Greșit!
./ArchiSteamFarm --path /home/archi/My Downloads/ASF # Greșit!
```

Cu toate acestea, următoarele sunt complet în regulă:

```shell
./ArchiSteamFarm --path "/home/archi/My Downloads/ASF" # OK
./ArchiSteamFarm "--path=/home/archi/My Downloads/ASF" # OK
```

## Argumente

`--cryptkey <key>` sau `--cryptkey=<key>` - va începe ASF cu cheia criptografică personalizată de `<key>`. Această opțiune afectează **[securitatea](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** și va face ca ASF să utilizeze cheia personalizată oferită `<key>` în loc de cea implicită codificată în executabil. Deoarece această proprietate afectează cheia de criptare implicită (în scopuri de criptare) şi valoarea salt (în scopuri de hashing), ține cont de faptul că totu ce este criptat/hashed cu această cheie necesită ca ea să fie transmisă la fiecare rulare ASF.

Nu există nicio cerință privind lungimea sau caracterele pentru `<key>`, dar din motive de securitate, recomandăm să alegi o frază de acces suficient de lungă, formată, de exemplu, din 32 de caractere aleatorii, utilizând comanda `tr -dc A-Za-z0-9 < /dev/urandom | head -c 32; echo` pe Linux.

Este bine de menționat că există și alte două modalități de a furniza acest detaliu: `--cryptkey-file` și `--input-cryptkey`.

Datorită naturii acestei proprietăți, este posibilă și setarea cheii de criptare prin declararea variabilei de mediu `ASF_CRYPTKEY`, care ar putea fi mai potrivită pentru persoanele care ar dori să evite detaliile sensibile în argumentele procesului.

---

`--cryptkey-file <path>` sau `--cryptkey-file=<path>` - va porni ASF cu o cheie criptografică personalizată citită din fișierul `<path>`. Acesta servește același scop ca `--cryptkey <key>` explicat mai sus, doar că mecanismul diferă, deoarece această proprietate va citi `<key>` din `<path>` furnizat în schimb. Dacă folosești aceasta împreună cu `--path`, ia în considerare faptul că calea relativă va fi diferită în funcție de ordinea argumentelor, adică dacă schimbi `--path` înainte sau după `--cryptkey-file`.

Datorită naturii acestei proprietăți, este posibil și să setezi fișierul cryptkey declarând variabila de mediu `ASF_CRYPTKEY_FILE`, ceea ce poate fi mai potrivit pentru persoanele care doresc să evite detaliile sensibile în argumentele procesului.

---

`--ignore-unsupported-environment` - va face ca ASF să ignore problemele legate de rularea într-un mediu nesuportat, care de obicei sunt semnalate cu o eroare și o ieșire forțată. Un mediu nesuportat include, de exemplu, rularea unei versiuni specifice de sistem de operare `win-x64` pe `linux-x64`. Deși acest flag va permite ASF să încerce să ruleze în astfel de scenarii, te rugăm să fii conștient că nu le susținem oficial și că forțezi ASF să facă acest lucru pe **propriul tău risc**. Este important să subliniem că **toate** scenariile de mediu nesuportate **pot fi corectate**. Recomandăm cu tărie să rezolvi problemele nerezolvate în loc să declari acest argument.

---

`--input-cryptkey` - va face ca ASF să ceară cheia `--cryptkey` la pornire. Această opțiune ar putea fi utilă dacă, în loc să furnizezi cryptkey-ul, fie prin variabile de mediu, fie printr-un fișier, ai prefera să nu fie salvat nicăieri și să-l introduci manual la fiecare rulare a ASF.

---

`--minimized` - va face ca fereastra de consolă ASF să se minimizeze la scurt timp după pornire. Utilă în principal în scenariile de auto-start, dar poate fi folosită și în afacerea acestora. Această opțiune necesită suport adecvat din partea mediului – s-ar putea să nu funcționeze corect în toate scenariile posibile.

---

`--network-group <group>` sau `--network-group=<group>` - va determina ASF să își inițializeze limitele cu un grup de rețea personalizat de `<group>`. Această opțiune afectează rularea ASF în **[mai multe instanțe](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#multiple-instances)** prin semnalizarea că o anumită instanță este dependentă doar de instanțele care partajează același grup de rețea, independent de restul. De obicei vrei să folosești această proprietate doar dacă direcționezi cereri ASF prin mecanisme personalizate (de ex. adrese IP diferite) și doriți să setați dvs. grupuri de rețea; fără a se baza pe ASF pentru a face acest lucru automat (care include în prezent doar luarea în considerare a `WebProxy`). Rețineți că atunci când utilizați un grup de rețea personalizat, acesta este un identificator unic în cadrul mașinii locale, iar ASF nu va lua în considerare alte detalii, cum ar fi valoarea `WebProxy`, care permite de ex. pornirea a două instanțe cu valori `WebProxy` diferite, care sunt încă dependente una de cealaltă.

Datorită naturii acestei proprietăți, este posibilă și setarea cheii de criptare prin declararea variabilei de mediu `ASF_NETWORK_GROUP`, care ar putea fi mai potrivită pentru persoanele care ar dori să evite detaliile sensibile în argumentele procesului.

---

`--no-config-migrate` - în mod implicit, ASF va migra automat fișierele tale de configurare la ultima sintaxă. Migrarea include conversia proprietăților dezactivate în cele mai recente, eliminarea proprietăților cu valori implicite (deoarece nu au efect), precum și curățarea fișierului în general (corectarea indentării și altele de acest tip). Acesta este aproape întotdeauna o idee bună, dar s-ar putea să ai o situație particulară în care ai prefera ca ASF să nu scrie niciodată fișierele de configurare automat. De exemplu, ai putea dori să `chmod 400` fișierele tale de configurare (permisiune de citire doar pentru proprietar) sau să aplici `chattr +i` asupra acestora, interzicând accesul la scriere pentru toată lumea, de exemplu, ca măsură de securitate. De obicei, recomandăm să păstrezi migrarea configurației activată, dar dacă ai un motiv anume pentru a o dezactiva și ai prefera ca ASF să nu facă acest lucru, poți folosi acest comutator pentru a atinge acest scop. Ține cont însă că, de acum înainte, furnizarea setărilor corecte pentru ASF va deveni responsabilitatea ta, mai ales în ceea ce privește deprecierea și refactorizarea proprietăților în versiunile viitoare ale ASF.

---

`--no-config-watch` - în mod implicit, ASF configurează un `FileSystemWatcher` pe directorul tău `config` pentru a asculta evenimentele legate de modificările fișierelor, astfel încât să poată să se adapteze interactiv la acestea. De exemplu, aceasta include oprirea boților la ștergerea configurației, repornirea boților atunci când configurația este modificată sau încărcarea cheilor în **[BGR](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)** odată ce le adaugi în directorul `config`. Acest comutator îți permite să dezactivezi un astfel de comportament, ceea ce va face ca ASF să ignore complet toate modificările din directorul `config`, necesitându-ți să efectuezi astfel de acțiuni manual, dacă este considerat potrivit (ceea ce de obicei înseamnă repornirea procesului). Recomandăm să păstrezi evenimentele de configurare activate, dar dacă ai un motiv anume pentru a le dezactiva și ai prefera ca ASF să nu facă acest lucru, poți folosi acest comutator pentru a atinge acest scop.

---

`--no-restart` - acest comutator este folosit în principal de containere **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker)** şi forţează `AutoRepornire` să fie `false`. În afară de cazul în care ai o nevoie particulară, ar trebui să configurezi direct proprietatea `AutoRestart` în configurația ta. Acest comutator există pentru ca scriptul nostru Docker să nu fie nevoit să modifice configurația ta globală pentru a o adapta la propriul său mediu. Desigur, dacă rulați ASF într-un script, puteți folosi acest comutator (altfel sunteți mai bine cu proprietatea configurării globale).

---

`--no-steam-parental-generation` - în mod implicit, ASF va încerca automat să genereze PIN-uri parentale Steam, așa cum este descris în proprietatea de configurare **[`SteamParentalCode`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#steamparentalcode)**. Totuși, deoarece acest lucru poate necesita o cantitate excesivă de resurse ale sistemului de operare, acest comutator îți permite să dezactivezi acest comportament, ceea ce va face ca ASF să sară peste auto-generare și să ceară direct PIN-ul utilizatorului, ceea ce ar trebui să se întâmple de obicei doar dacă auto-generarea a eșuat. De obicei, recomandăm să păstrezi generarea activată, dar dacă ai un motiv anume pentru a o dezactiva și ai prefera ca ASF să nu facă acest lucru, poți folosi acest comutator pentru a atinge acest scop.

---

`--path <path>` sau `--path<path>` - ASF navigează întotdeauna spre propriul său director la pornire. Prin specificarea acestui argument, ASF va naviga către directorul dat după inițializare, ceea ce îți permite să folosești o cale personalizată pentru diferite părți ale aplicației (inclusiv directoarele `config`, `logs`, `plugins` și `www`, precum și fișierul `NLog.config`), fără a fi necesar să duplici binarul în același loc. Poate fi folositor mai ales dacă doriţi să separaţi binarul de configurarea reală, aşa cum este făcut în ambalajul din Linux - în acest fel puteţi folosi un binar (actualizat) cu mai multe configurări diferite. Calea poate fi relativă în funcție de locul curent al sistemului binar ASF, sau absolut. Ține cont că această comandă indică noul "ASF home" - directorul care are aceeași structură ca ASF-ul original, cu directorul `config` în interior, vezi mai jos un exemplu pentru explicație.

Datorită naturii acestei proprietăți, este posibil să setezi calea așteptată prin declararea variabilei `ASF_PATH`, care ar putea fi mai potrivite pentru persoanele care ar dori să evite detaliile sensibile în argumentele procesului.

Dacă intenționezi să folosești acest argument de linie de comandă pentru a rula mai multe instanțe de ASF, îți recomandăm să citești pagina noastră de **[management](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#multiple-instances)** pe această temă.

Exemple:

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/DirectorDorit # Calea absolută
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../DirectorDorit # Calea relativă funcționează de asemenea
ASF_PATH=/opt/DirectorDorit dotnet /opt/ASF/ArchiSteamFarm.dll # La fel ca variabila env
```

```text
├── 📁 /opt
│     ├── 📁 ASF
│     │     ├── ⚙️ ArchiSteamFarm.dll
│     │     └── ...
│     └── 📁 TargetDirectory
│           ├── 📁 config
│           ├── 📁 logs (generat)
│           ├── 📁 plugins (opțional)
│           ├── 📁 www (opțional)
│           ├── 📄 log.txt (generat)
│           └── 📄 NLog.config (opțional)
└── ...
```

---

`--service` - acest comutator este folosit în principal de serviciul nostru `systemd` și forțează `Headless` la `true`. Cu excepția cazului în care ai o nevoie specifică, ar trebui să configurezi proprietatea `Headless` direct în fișierul tău de configurare. Acest comutator există pentru ca serviciul nostru `systemd` să nu fie nevoit să modifice configurația ta globală pentru a o adapta propriului său mediu. Desigur, dacă ai o nevoie similară, poți folosi și tu acest comutator (altfel, este mai bine să folosești proprietatea din configurația globală).

---

`--system-required` - declararea acestui întrerupător va determina ASF să încerce să semnalizeze sistemului de operare că procesul necesită ca sistemul să fie funcțional pe toată durata sa de viață. În prezent, acest comutator are efect numai pe mașinile Windows unde va interzice sistemului să intre în modul de suspendare atâta timp cât procesul rulează. Acest lucru poate fi deosebit de util atunci când faci farming pe PC sau laptop în timpul nopții, deoarece ASF va putea menține sistemul activ cât timp rulează.
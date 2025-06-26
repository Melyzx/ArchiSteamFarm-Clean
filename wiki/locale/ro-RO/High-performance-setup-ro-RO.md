# Configurare de înaltă performanță

Acest lucru este exact opusul **[setării cu memorie mică](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)** și de obicei doriți să urmați aceste sfaturi dacă doriți să creșteți în continuare performanța ASF (în termeni de viteză CPU), cu potențialul cost de utilizare crescută a memoriei.

---

ASF încearcă deja să prefere performanța în ceea ce privește reglarea generală echilibrată; prin urmare nu puteţi face multe pentru a-i creşte performanţa, deși aveţi anumite opţiuni care pot fi configurate. Cu toate acestea, aveți în vedere faptul că aceste opțiuni nu sunt activate în mod implicit, ceea ce înseamnă că nu sunt suficient de bune pentru a le considera echilibrate pentru majoritatea utilizărilor; de aceea trebuie să vă decideţi dacă creşterea memoriei indusă de acestea este acceptabilă pentru dumneavoastră.

---

## Reglaj Runtime (avansat)

Below tricks **involve serious memory increase** and should therefore be used with caution.

Modul recomandat de aplicare a acestor setări este prin proprietățile de mediu `DOTNET_`. Bineînțeles, ați putea folosi și alte metode, de ex. `runtimeconfig.json`, dar unele setări sunt imposibil de stabilit în acest fel, și pe deasupra ASF va înlocui fișierul personalizat `runtimeconfig.json` cu cel propriu la următoarea actualizare, de aceea recomandăm proprietățile de mediu pe care le puteți seta cu ușurință înainte de a lansa procesul.

Runtime-ul .NET îți permite să **[ajustezi colectorul de gunoi (GC)](https://docs.microsoft.com/dotnet/core/run-time-config/garbage-collector)** în multe moduri, reglând eficient procesul de GC în funcție de nevoile tale. Am documentat mai jos proprietățile care sunt, în opinia noastră, deosebit de importante.

### [`gcServer`](https://docs.microsoft.com/dotnet/core/run-time-config/garbage-collector#flavors-of-garbage-collection)

> Configurează dacă aplicaţia se foloseşte de colectarea gunoiului de la staţia de lucru sau de colectarea gunoiului de pe server.

Puteti citi specificul serverului GC la **[fundamentale pentru colectarea de gunoi](https://docs.microsoft.com/dotnet/standard/garbage-collection/fundamentals)**.

ASF folosește implicit colectorul de gunoi de tip workstation. Acest lucru se datorează în principal unui echilibru bun între consumul de memorie și performanță, fiind mai mult decât suficient pentru doar câțiva boți, deoarece, de obicei, un singur thread GC de fundal concurent este suficient de rapid pentru a gestiona întreaga memorie alocată de ASF.

Totuși, în prezent avem multe nuclee CPU de care ASF poate beneficia semnificativ, având un thread GC dedicat pentru fiecare vCore CPU disponibil. Acest lucru poate îmbunătăți considerabil performanța în timpul sarcinilor solicitante ale ASF, cum ar fi analizarea paginilor de insigne sau a inventarului, deoarece fiecare vCore al CPU-ului poate contribui, spre deosebire de doar 2 (principal și GC). GC-ul de tip server este recomandat pentru mașinile cu 3 vCore-uri CPU sau mai multe, GC-ul de tip workstation este forțat automat dacă sistemul tău are doar 1 vCore CPU, iar dacă ai exact 2, poți lua în considerare testarea ambelor variante (rezultatele pot varia).

GC-ul de tip server nu duce în sine la o creștere foarte mare a memoriei doar prin activare, însă are dimensiuni mult mai mari ale generațiilor și, prin urmare, este mult mai „leneș” în a returna memoria către sistemul de operare. Te-ai putea afla într-un punct ideal în care GC-ul de tip server îți oferă o creștere semnificativă a performanței și ai dori să continui să-l folosești, dar, în același timp, nu îți permiți creșterea mare de memorie care vine odată cu utilizarea lui. Din fericire pentru tine, există o setare de tipul „ce e mai bun din ambele lumi”, folosind GC-ul de tip server cu proprietatea de configurare **[`GCLatencyLevel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gclatencylevel)** setată la `0`, ceea ce va păstra activ GC-ul server, dar va limita dimensiunile generațiilor și se va concentra mai mult pe memorie. Alternativ, poți experimenta și cu o altă proprietate, **[`GCHeapHardLimitPercent`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gcheaphardlimitpercent)**, sau chiar cu ambele în același timp.

Totuși, dacă memoria nu reprezintă o problemă pentru tine (deoarece GC ia în considerare memoria disponibilă și se ajustează automat), este o idee mult mai bună să nu modifici deloc aceste proprietăți, obținând astfel o performanță superioară.

---

Poți activa proprietățile selectate setând variabilele de mediu corespunzătoare. De exemplu, pe Linux (shell):

```shell
export DOTNET_gcServer=1

./ArchiSteamFarm # For OS-specific build
./ArchiSteamFarm.sh # For generic build
```

Sau pe Windows (powershell):

```powershell
$Env:DOTNET_gcServer=1

.\ArchiSteamFarm.exe # For OS-specific build
.\ArchiSteamFarm.cmd # For generic build
```

---

## Optimizare recomandată

- Asigură-te că folosești valoarea implicită a `OptimizationMode`, care este `MaxPerformance`. Aceasta este, de departe, cea mai importantă setare, deoarece utilizarea valorii `MinMemoryUsage` are efecte dramatice asupra performanței.
- Activează GC server. GC server poate fi imediat observat ca fiind activ prin creșterea semnificativă a memoriei comparativ cu GC-ul de stație de lucru. Aceasta va lansa un thread GC pentru fiecare thread CPU pe care îl are mașina ta, pentru a efectua operațiuni GC în paralel, cu viteză maximă.
- Dacă nu îți permiți creșterea memoriei din cauza GC server, ia în considerare ajustarea **[`GCLatencyLevel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gclatencylevel)** și/sau **[`GCHeapHardLimitPercent`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gcheaphardlimitpercent)** pentru a obține „ceea ce este mai bun din ambele lumi”. Cu toate acestea, dacă memoria ta își permite, este mai bine să o lași la valoarea implicită – GC server se ajustează deja în timpul execuției și este suficient de inteligent pentru a folosi mai puțină memorie atunci când sistemul tău de operare o va necesita cu adevărat.

Aplicarea recomandărilor de mai sus îți va permite să obții o performanță superioară a ASF, care ar trebui să fie extrem de rapidă chiar și cu sute sau mii de boți activi. CPU-ul nu ar trebui să mai fie un punct de blocaj, deoarece ASF este capabil să utilizeze întreaga putere a CPU-ului tău atunci când este necesar, reducând timpul necesar la minimul absolut. The next step would be CPU and RAM upgrades, or splitting workload across several servers and ASF instances.
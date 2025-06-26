# Befehlszeilenargumente

ASF unterstützt mehrere Befehlszeilenargumente, die die Programm-Runtime beeinflussen können. Diese können von fortgeschrittenen Benutzern verwendet werden, um festzulegen, wie das Programm arbeiten soll. Im Vergleich zum gängigen Standardweg über die Konfigurationsdatei `ASF.json` werden Befehlszeilenargumente für die Kerninitialisierung (z. B. `--path`), plattformspezifische Einstellungen (z. B. `--system-required`) oder sensible Daten (z. B. `--cryptkey`) verwendet.

---

## Verwendung

Die Nutzung ist abhängig vom verwendeten Betriebssystem und der ASF-Variante.

Generisch:

```shell
dotnet ArchiSteamFarm.dll --argument --weiteresArgument
```

Windows:

```powershell
.\ArchiSteamFarm.exe --argument --weiteresArgument
```

Linux/macOS:

```shell
./ArchiSteamFarm --argument --weiteresArgument
```

Die Befehlszeilenargumente werden auch in allgemeinen Hilfsskripten wie zum Beispiel `ArchiSteamFarm.cmd` oder `ArchiSteamFarm.sh` unterstützt. Darüber hinaus können Sie auch die Umgebungsvariable `ASF_ARGS` verwenden, wie es in den Abschnitten **[Verwaltung ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management-de-DE#umgebungsvariablen)** und **[Docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker-de-DE#befehlszeilenargumente)** erläutert ist.

Vergessen Sie nicht, Anführungszeichen zu setzen, falls Ihr Argument Leerzeichen enthält. Diese beiden Beispiele sind fehlerhaft:

```shell
./ArchiSteamFarm --path /home/archi/Meine Downloads/ASF # Falsch!
./ArchiSteamFarm --path=/home/archi/Meine Downloads/ASF # Falsch!
```

Aber diese zwei sind völlig in Ordnung:

```shell
./ArchiSteamFarm --path "/home/archi/Meine Downloads/ASF" # OK
./ArchiSteamFarm "--path=/home/archi/Meine Downloads/ASF" # OK
```

## Argumente

`--cryptkey <key>` oder `--cryptkey=<key>` – ASF startet mit dem benutzerdefinierten kryptografiischen Schlüssel `<key>`. Diese Option wirkt sich auf die **[Sicherheit](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security-de-DE#sicherheit)** aus und veranlasst ASF, den von Ihnen bereitgestellten `<key>` Schlüssel anstelle des standardmäßig fest in die ausführbare Datei einprogrammierten Schlüssels zu verwenden. Beachten Sie bitte, dass sämtliche Verschlüsselungen/Hashs bei jedem ASF-Lauf weitergegeben wird, da diese Variable den Standard-Schlüssel (für Verschlüsselungszwecke), sowie SALT (für Hash-Zwecke) betrifft.

`<key>` erfordert keine Voraussetzungen bezüglich der Länge oder Zeichenart, jedoch empfehlen wir das Passwort etwa unter Verwendung von zufälligen Zeichen zu generieren (beispielsweise mit dem `tr -dc A-Za-z0-9 < /dev/urandom | head -c 32; echo` Kommando unter Linux).

Darüber hinaus gibt es auch zwei weitere Möglichkeiten, um diese Angaben bereitzustellen: `--cryptkey-file` und `--input-cryptkey`.

Aufgrund der Art dieser Variable, ist es auch möglich einen cryptkey zu setzen, indem man die Umgebungsvariable `ASF_CRYPTKEY` deklariert, was für Personen, die sensible Details in den Prozessargumenten vermeiden möchten, besser geeignet sein kann.

---

`--cryptkey-file <path>` oder `--cryptkey-file=<path>` – ASF startet mit dem benutzerdefinierten kryptografischen Schlüssel, der aus der Datei `<path>` gelesen wird. Dies dient dem gleichen Zweck wie `--cryptkey <key>` (oben erklärt) mit dem Unterschied, dass mit dieser Variable der `<key>` aus dem angegebenen `<path>` gelesen wird. Wenn Sie dies zusammen mit `--path` verwenden, beachtenSie die Tatsache, dass der relative Pfad basierend auf der Reihenfolge der Argumente sich unterscheiden wird. Beispielsweise je nachdem, ob Sie `--path` vor oder nach `--cryptkey-file` ändern.

Durch die Natur dieser Option ist, es möglich die Umgebungsvariable `ASF_CRYPTKEY_FILE` für den cryptkey zu bestimmen; welcher für jene angemessen ist, die sensible Details in den Verarbeitungsargumenten vermeiden möchten.

---

`--ignore-unsupported-environment` – sorgt dafür, dass ASF die Erkennung von nicht unterstützten Umgebungen ignoriert; die normalerweise mit einem Fehler signalisiert und das Beenden erzwingen. Eine nicht unterstützte Umgebung beinhaltet beispielsweise das Ausführen des `win-x64` OS-spezifischen Builds auf `linux-x64`. Obwohl diese Option ASF die Ausführung in solchen Situationen erlaubt; sollten Sie dennoch beachten, dass wir dies offiziell nicht unterstützen und Sie dies vollständig **auf eigene Gefahr** tun. Es ist wichtig darauf hinzuweisen, dass **jede** der nicht unterstützten Umgebungsszenarien **korrigiert werden kann**. Wir empfehlen Ihnen dringend, die offenen Probleme zu beheben, anstatt dieses Argument anzugeben.

---

`--input-cryptkey` – lässt ASF während des Starts nach `--cryptkey` fragen. Diese Option könnte für Sie nützlich sein; sofern es Ihr Wunsch ist, cryptkey nicht zu speichern (in Umgebungsvariablen oder einer Datei), sondern stattdessen bei jedem ASF-Lauf manuell einzugeben.

---

`--minimized` – minimiert das ASF-Konsolenfenster direkt nach dem Start. Hauptsächlich in Autostart-Szenarien nützlich, kann auch außerhalb dieser genutzt werden. Diese Option erfordert eine angemessene Umgebungsunterstützung - sie funktioniert möglicherweise nicht in allen möglichen Szenarien.

---

`--network-group <group>` oder `--network-group=<group>` – führt dazu, dass ASF seine Beschränkung (Limiter) mit einer benutzerdefinierten Netzwerkgruppe mit einem Wert `<group>` initialisiert. Diese Option wirkt sich auf die Ausführung von ASF in **[mehreren Instanzen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management-de-DE#mehrere-instanzen)** aus, indem signalisiert wird, dass diese Instanz nur von Instanzen abhängig ist, die dieselbe Netzwerkgruppe teilen und unabhängig vom Rest. In der Regel sollte diese Variable nur verwendet werden, wenn Sie ASF-Anfragen über einen benutzerdefinierten Mechanismus (z. B. verschiedene IP-Adressen) und eine Netzwerkgruppen selbst einstellen möchten, ohne sich darauf zu verlassen, dass ASF dies automatisch macht (dies berücksichtigt derzeit nur `WebProxy`). Beachten Sie, dass es sich bei der Verwendung einer benutzerdefinierten Netzwerkgruppe um einen eindeutigen Bezeichner innerhalb des lokalen Rechners handelt und ASF keine weiteren Details berücksichtigt, z. B. den Wert vom `WebProxy`, wodurch Sie z. B. zwei Instanzen mit unterschiedlichen `WebProxy` Werten starten können, die immernoch voneinander abhängig sind.

Aufgrund der Art dieser Variable ist es auch möglich, den Wert zu setzen, indem man die Umgebungsvariable `ASF_NETWORK_GROUP` deklariert, was für Personen, die sensible Details in den Prozessargumenten vermeiden möchten, besser geeignet sein kann.

---

`--no-config-migrate` – ASF migriert standardmäßig Ihre Konfigurationsdateien automatisch in die neueste Syntax. Diese Option verhindert das. Migration beinhaltet die Umwandlung veralteter Eigenschaften in Neue, das Entfernen von Eigenschaften mit Standardwerten (da diese keine Wirkung haben), sowie die Bereinigung der Datei im Allgemeinen (Korrektur der Einrückung und ähnliches). Dies ist fast immer eine gute Idee, aber Sie könnten eine bestimmte Situation haben, in der Sie es vorziehen würden, wenn ASF die Konfigurationsdateien niemals automatisch überschreibt. Zum Beispiel: Vielleicht möchten Sie `chmod 400` (nur für den Eigentümer lesbar) auf Ihre Konfigurationsdateien anwenden oder `chattr +i` anwenden; was dazu führt, dass für jeden Schreibzugriff verweigert wird, z. B. als Sicherheitsmaßnahme. Wir empfehlen die Konfigurationsdatei-Migration aktiviert zu lassen, es sei denn Sie haben einen guten Grund diese zu deaktivieren, und wünschen, dass ASF diese auslässt. Denken Sie jedoch daran, dass die korrekte Einstellung von ASF künftig zu Ihrer neuen Verantwortung wird; vorwiegend in Bezug auf die Überarbeitung und Abschaffung von Variablen in zukünftigen ASF-Versionen.

---

`--no-config-watch` – legt ASF standardmäßig einen `FileSystemWatcher` für Ihre `Konfiguration` fest, um auf Ereignisse im Zusammenhang mit Änderungen der Datei zu hören, sodass diese sich interaktiv anpasst. Dies beinhaltet beispielsweise das Stoppen von Bots beim Löschen der Konfiguration, den Neustart des Bots bei der Änderung der Konfiguration, oder laden von Schlüsseln in **[BGR](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer-de-DE#hintergrundproduktschl%C3%BCsselaktivierer-bgrR)** sobald Sie sie in das `Verzeichnis` eingegeben werden. Dieser Schalter erlaubt es Ihnen, dieses Verhalten zu deaktivieren, sodass ASF alle Änderungen im `config`-Ordner ignoriert, wodurch alle Aktionen manuell ausgeführt werden müssen, sofern sie erforderlich sein (meistens das Neustarten des Vorgangs). Wir empfehlen die Konfigurationsereignisse aktiviert zu lassen; haben Sie jedoch einen guten Grund, um dies für ASF zu deaktivieren, so erreichen Sie dies mit diesem Schalter.

---

`--no-restart` – Diese Option wird hauptsächlich in unseren **[Docker](https://github.com/JustArchi/ArchiSteamFarm/wiki/Docker-de-DE)**-Containern genutzt und setzt `AutoRestart` auf `false`. Wenn Sie keinen besonderen Grund haben, sollten Sie stattdessen `AutoRestart` Variable direkt in Ihrer Konfiguration konfigurieren. Dieser Schalter dient dazu, dass unser Docker-Skript nicht Ihre globale Konfiguration berühren muss, um es an dessen eigene Umgebung anzupassen. Selbstverständlich können Sie diese Option auch verwenden, wenn Sie ASF in einem Skript ausführen; ansonsten ist die globale Konfigurationseigenschaft besser geeignet.

---

`--no-steam-parental-generation` – standardmäßig wird ASF automatisch versuchen, die Steam-Familien-PINs zu generieren, wie in der Konfigurationseigenschaft **[` SteamParentalCode`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-de#steamparentalcode)** erläutert. Da dies jedoch eine übermäßige Menge an Betriebssystem-Ressourcen erfordern kann, erlaubt Ihnen dieser Schalter dieses Verhalten zu deaktivieren, was dazu führt, dass ASF die automatische Generierung überspringt, und stattdessen den Benutzer direkt nach der entsprechenden PIN fragt. Dies würde sonst nur passieren, wenn die Auto-Generierung versagt. Wir empfehlen die Generierung aktiviert zu lassen, es sei denn Sie haben einen guten Grund diese zu deaktivieren, und wünschen, dass ASF dies nicht tut.

---

`--path <path>` oder `--path=<path>` – ASF wechselt beim Start immer in sein eigenes Verzeichnis. Wird dieser Parameter angegeben, so wird ASF nach der Initialisierung zu dem gegebenem Programmverzeichnis navigieren. Dies erlaubt Ihnen andere Verzeichnisse (z. B. `config`, `logs`, `plugins` oder `www` – incl. der Datei `NLog.config`) für unterschiedliche Teile der Applikation zu nutzen. Das Kopieren der Binärdateien an diese Stellen ist dadurch nicht mehr nötig. Dies kann besonders nützlich sein, wenn Sie die Binärdatei von der eigentlichen Konfiguration trennen möchten, wie es in Linux-ähnlichen Paketen geschieht. So können Sie eine (aktuelle) Binärdatei mit mehreren verschiedenen Konfigurationen verwenden. Der Pfad kann entweder relativ zum aktuellen Ort der ASF-Binärdatei oder absolut sein. Bedenke Sie auch, dass dieser Befehl auf ein neues „ASF-Startverzeichnis“ zeigt – ein Verzeichnis, welches die gleiche Struktur wie der ursprüngliche ASF-Ordner hat, mit einem Verzeichnis `config` darin (siehe Beispiel unten).

Aufgrund der Art dieser Variable ist es auch möglich, den erwarteten Pfad zu setzen, indem man die Umgebungsvariable `ASF_PATH` deklariert wird, was für Personen, die sensible Details in den Prozessargumenten vermeiden möchten, besser geeignet sein kann.

Sollten Sie erwägen, dieses Befehlszeilenargument für die Ausführung mehrerer ASF-Instanzen zu verwenden, empfehlen wir Ihnen, sich mit dem Thema **[Verwaltung](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management-de-DE#mehrere-instanzen)** zu befassen.

Beispiele:

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/Zielverzeichnis #Absoluter Pfad
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../Zielverzeichnis #Relativer Pfad funktioniert ebenfalls
ASF_PATH=/opt/Zielverzeichnis dotnet /opt/ASF/ArchiSteamFarm.dll #Identisch mit Umgebungsvariable
```

```text
├── 📁 /opt
│     ├── 📁 ASF
│     │     ├── ⚙️ ArchiSteamFarm.dll
│     │     └── ...
│     └── 📁 TargetDirectory
│           ├── 📁 config
│           ├── 📁 logs (erstellt)
│           ├── 📁 plugins (optional)
│           ├── 📁 www (optional)
│           ├── 📄 log.txt (erstellt )
│           └── 📄 NLog.config (optional)
└── ...
```

---

`--service`- Dieser Schalter wird hauptsächlich für unseren `systemd` Dienst und erzwingt den Wert von `Headless` auf `true`. Wenn Sie keinen besonderen Grund haben, sollten Sie stattdessen die `headless` Variable direkt in Ihrer Konfiguration einrichten. Dieser Schalter existiert, damit unser `systemd` Dienst Ihre globale Konfiguration nicht berühren muss, um sich an seine eigene Umgebung anzupassen. Selbstverständlich können Sie diese Option auch verwenden, wenn Sie einen ähnlichen Anwendungsfall benötigen; ansonsten ist die globale Konfigurationseigenschaft besser geeignet.

---

`--system-required` – Die Verwendung dieser Option veranlasst ASF, dem Betriebssystem zu signalisieren, dass das System für die gesamte Lebensdauer des Prozesses betriebsbereit sein muss. Derzeit hat dieser Schalter nur Auswirkungen auf Windows-Geräte, auf denen es Ihrem System untersagt, in den Schlafmodus zu gehen, solange der Prozess läuft. Dies kann besonders nützlich sein, wenn Sie ASF über Nacht auf Ihrem PC oder Laptop verwenden, da ASF das System davon abhält, in den Ruhemodus zu gelangen.
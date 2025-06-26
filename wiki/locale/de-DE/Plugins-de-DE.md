# Erweiterungen (Plugins)

ASF bietet Unterstützung für benutzerdefinierte Plugins, die während der Ausführung geladen werden können. Erweiterungen ermöglichen es Ihnen, das ASF-Verhalten anzupassen, z. B. durch Hinzufügen von benutzerdefinierten Befehlen, einer benutzerdefinierten Handelslogik oder der vollständigen Integration mit Diensten und APIs von Drittanbietern.

Diese Seite beschreibt ASF-Erweiterungen aus der Nutzerperspektive - Erklärung, Verwendung und Ähnliches. Wenn Sie an einer Entwicklerperspektive interessiert sind, folgen sie **[diesem](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-development)** link.

---

## Verwendung

ASF lädt Erweiterungen aus dem Verzeichnis `plugins`, das sich in Ihrem ASF-Ordner befindet. Es ist eine empfohlene (und für die automatische Erweiterungsaktualisierung auch notwendige) Praxis für jede Erweiterung, die Sie verwenden möchten, ein eigenes Verzeichnis zu erstellen, das auf dessen Namen basiert, z. B. `MeinPlugin`. Dies führt zur finalen Verzeichnisstruktur `plugins/MeinPlugin`. Schließlich sollten alle Binärdateien der Erweiterungen in diesem speziellen Ordner abgelegt werden. ASF wird Ihre Erweiterung nach dem Neustart ordnungsgemäß erkennen und verwenden.

In der Regel veröffentlichen Plugin-Entwickler ihre Erweiterungen in Form einer `zip`-Datei mit Binärdateien. Demzufolge sollten Sie diese Zip-Datei in ihr eigenes dediziertes Unterverzeichnis innerhalb des Ordners `plugins` entpacken.

Wenn die Erweiterung erfolgreich geladen wurde, dann sehen Sie dessen Namen und Version in Ihrem Protokoll. Sie sollten den Plugin-Entwickler konsultieren, wenn es um Fragen, Probleme oder die Verwendung der Erweiterungen geht, die Sie verwenden.

Sie finden einige der vorgestellten Erweiterungen in unserem Abschnitt **[Drittanbieter](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party-de-DE#asf-plugins)**.

**Bitte beachten Sie, dass ASF-Erweiterungen möglicherweise bösartig sein können**. You should always ensure that you're using plugins made by developers that you can trust, even those from the third-party section above. Es ist den ASF-Entwicklern nicht mehr möglich, Ihnen die üblichen ASF-Vorteile (z. B. Schutz vor Schadsoftware oder VAC-Freiheit) zu gewähren, sobald Sie sich dafür entscheiden, individuelle Erweiterungen einzusetzen. Sie müssen verstehen, dass Erweiterungen die volle Kontrolle über den ASF-Prozess haben, sobald diese geladen wurden. Wir können keine Installationen mit benutzerdefinierten Erweiterungen unterstützen, da Sie keinen »Vanilla« ASF Code mehr verwenden.

---

## Kompabilität

Basierend auf der Komplexität der Erweiterung, ihres Umfangs und einer Menge anderer Faktoren, ist es möglich, dass Sie genötigt sind, anstatt der sonst empfohlenen **[betriebssystemspezifischen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up#os-specific-setup)**, die **[generische](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up#generic-setup)<0> Variante von ASF zu verwenden. Dies ist, weil die betriebssystemspezifischen Varianten nur mit der Kernfunktionalität von ASF selbst kommen und die Erweiterung möglicherweise Teile benötigt, die außerhalb des Hauptfokus fallen, wodurch es inkompatibel mit den beschnittenen betriebssystemspezifischen Applikationen wird.</p>

Generell, wenn sie Drittparteienerweiterungen verwenden, empfehlen wir die generische ASF Variante für ein Maximum an Kompatibilität. Allerdings wird dies nicht von allen Erweiterungen benötigt - Bitte lesen Sie die Informationen bezüglich Ihrer Erweiterung um herauszufinden, ob sie die generische ASF Variante benötigen oder nicht.
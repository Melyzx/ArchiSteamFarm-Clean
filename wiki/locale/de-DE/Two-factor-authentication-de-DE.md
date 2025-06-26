# Zwei-Faktor-Authentifizierung (2FA)

Steam includes two-factor authentication system that requires extra details for various account-related activity. Sie können **[hier](https://help.steampowered.com/faqs/view/2E6E-A02C-5581-8904)** und **[hier](https://help.steampowered.com/faqs/view/34A1-EA3F-83ED-54AB)** mehr darüber lesen. Diese Seite befasst sich mit dem 2FA-System sowie unserer Lösung, die mit ihm integriert wird (ASF-2FA).

---

# ASF-Logik

Regardless if you use ASF 2FA or not, ASF includes proper logic and is fully aware of accounts protected by 2FA on Steam. Es fragt Sie nach den erforderlichen Details, sobald diese benötigt werden (z. B. beim Anmelden). While you can manually provide that information, certain ASF functionalities (such as `MatchActively`) require ASF 2FA to be operative on your bot account, which can automatically respond to 2FA prompts, automatically, whenever required by ASF.

---

# ASF-2FA

ASF-2FA ist ein eingebautes Modul, das für die Bereitstellung von 2FA-Funktionen für den ASF-Prozess verantwortlich ist, wie dem Erzeugen von Codes und das Annehmen von Bestätigungen. It can work either in standalone mode, or by duplicating your existing authenticator details (so that you can use your current authenticator and ASF 2FA at the same time).

Sie können überprüfen, ob Ihr Bot-Konto bereits ASF-2FA verwendet, indem Sie den **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** `2fa` ausführen. Without setting up ASF 2FA, all standard `2fa` commands will be non-operative, which means that your bot is unavailable for advanced ASF features that require the module to be operative.

---

# Empfehlungen

Wir bieten Ihnen eine Vielzahl an Möglichkeiten, ASF 2FA operativ zu behandeln. Unsere Empfehlungen basieren auf Ihrer aktuellen Situation:

- If you're already using unofficial third-party app that allows you to extract 2FA details with ease, just **[import](#import)** those to ASF.
- Wenn Sie die offizielle App verwenden und Sie nichts dagegen haben, Ihre 2FA-Anmeldeinformationen zurückzusetzen; dann ist der beste Weg, 2FA zu deaktivieren, dann **[](#erstellung)** neue 2FA-Anmeldedaten unter Verwendung des **[gemeinsamen Authentifikators](#gemeinsamer-authentifikator)**, mit der Sie die offizielle App und ASF-2FA verwenden können. This method doesn't require root or advanced knowledge, barely following instructions written here, and is arguably superior for this scenario.
- Wenn Sie die offizielle App verwenden und Ihre 2FA-Anmeldeinformationen nicht neu erstellen möchten, sind Ihre Optionen sehr begrenzt; typischerweise benötigen Sie *root* und zusätzliches Experimentieren, um diese Details **[zu importieren](#importieren)**, und sogar mit diesem könnte es unmöglich sein.
- If you're not using 2FA yet and don't care, we recommend you to use ASF 2FA with **[standalone authenticator](#standalone-authenticator)** or **[joint authenticator](#joint-authenticator)** with official app (same as above).

Im Folgenden diskutieren wir alle möglichen Optionen und bekannte Methoden.

---

## Erstellung

ASF comes with an official `MobileAuthenticator` **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** that further extends ASF 2FA, allowing you to link a completely new 2FA authenticator. Dies kann hilfreich sein, wenn Sie nicht bereit oder in der Lage sind, andere Tools zu verwenden und es keine Einwände gibt, dass ASF-2FA Ihr (möglicherweise einziger) Hauptauthentifikator sein wird. Creation process is also used in joint-authenticator method, naturally in this scenario your authenticator can co-exist in two places at once - both will generate the same codes and both will be able to confirm the same confirmations.

### Gemeinsame Schritte für beide Szenarien

No matter if you plan to use ASF as the standalone or joint authenticator, you need to do those initialization steps:

1. Erstellen Sie einen ASF-Bot für das Zielkonto, starten Sie ihn und melden Sie sich an (was Sie wahrscheinlich bereits getan haben).
2. Assign a working and operational phone number to the account **[here](https://store.steampowered.com/phone/manage)** to be used by the bot. This will allow you to receive SMS code and allow recovery if needed. This step is not mandatory in all scenarios, however, we recommend it unless you know what you're doing.
3. Stellen Sie sicher, dass Sie 2FA bisher nicht für Ihr Konto verwenden; deaktivieren Sie es zuerst, falls doch. This **will** put your account on temporary trade-hold, there is no way around it, only **[import](#import)** process can skip it.
4. Führen Sie den Befehl `2fainit [Bot]` aus, indem Sie `[Bot]` durch den Namen Ihres Bots ersetzen.

Angenommenen, Sie haben eine erfolgreiche Antwort erhalten; dann sind die folgenden zwei Dinge passiert:

- Eine neue Datei `<Bot>.maFile.PENDING` wurde von ASF in Ihrem `Konfigurations`-Verzeichnis generiert.
- SMS wurde von Steam an die Telefonnummer gesendet, die Sie für das oben genannte Konto zugewiesen haben. Wenn Sie keine Telefonnummer angegeben haben, wurde stattdessen eine E-Mail an die E-Mail-Adresse des Kontos gesendet.

Die Authentifizierungsdetails sind noch nicht funktionsfähig, aber Sie können die generierte Datei überprüfen, wenn Sie es wünschen. Für den Fall, dass Sie doppelte Sicherheit wünschen, können Sie den Widerrufscode bereits notieren. Die nächsten Schritte hängen von Ihrem gewählten Szenario ab.

### Eigenständiger Authentifikator

If you want to use ASF as your main (or even only) authenticator, now you need to do the final finalization step:

5. Führt den Befehl `2fafinalize [Bot] <ActivationCode>` aus; ersetzt `[Bot]` mit dem Namen Ihres Bots und `<ActivationCode>` mit dem Code, den Sie im vorherigen Schritt per SMS oder E-Mail erhalten haben.

### Gemeinsamer Authentifikator

If you want to have the same authenticator in both ASF and the official Steam mobile app, now you need to do the next, more tricky steps:

5. Ignore the SMS or e-mail code that you've received in the previous step.
6. Installieren Sie die mobilen Steam App, falls sie bisher nicht installiert ist, und öffnen Sie diese. Navigieren Sie zur Registerkarte „Steam Guard“ und fügen Sie einen neuen Authentifikator hinzu, indem Sie den Anweisungen der App folgen.
7. Nachdem Ihr Authentifikator in der mobilen App hinzugefügt wurde und funktioniert, kehren Sie zu ASF zurück. Now, instead of finalization, we only need to inform ASF that mobile app already activated our previously-generated details:
 - Wait until the next 2FA code is shown in the Steam mobile app, and use the command `2fafinalized [Bot] <2FACodeFromApp>` replacing `[Bot]` with your bot's name and `<2FACodeFromApp>` with the code you currently see in the Steam mobile app. If the code generated by ASF and the code you provided are equal, ASF will assume that an authenticator was added correctly and proceed with importing your newly created authenticator.
 - Wir empfehlen dringend, die vorherige Schritte zu befolgen, um sicherzustellen, dass Ihre Anmeldedaten gültig sind. Wenn Sie jedoch nicht überprüfen möchten (oder können), ob Codes identisch sind und Sie wissen, was Sie tun; dann können Sie stattdessen den Befehl `2fafinalizedforce [Bot]` verwenden (`[Bot]` mit dem Namen Ihres Bots ersetzen). ASF geht davon aus, dass der Authentifikator korrekt eingefügt und Sie nun mit dem Import Ihres neuen Authentifikators beginnen werden. Beachten Sie, dass ASF in diesem Modus nicht überprüfen kann, ob die Codes übereinstimmen, was bedeutet, dass Sie möglicherweise ungültige (nicht aktivierte) Zugangsdaten importieren können.

### Nach Abschluss

Angenommen, alles funktionierte richtig, wurde die zuvor generierte Datei `<Bot>.maFile.PENDING` in `<Bot>.maFile.NEW` umbenannt. Dies belegt, dass Ihre 2FA-Anmeldedaten nun gültig und aktiv sind. We recommend that you move that file to **a secure and safe location**. In addition to that, if you've decided to use standalone authenticator, then we recommend you to open the file in your editor of choice and write down the `revocation_code`, which will allow you to, as the name implies, revoke the authenticator in case you lose it. In joint-authenticator method, you should've already done that in Steam mobile app, but feel free to do the same in case you need to.

In regards to technical details, the generated `maFile` includes all details that we've received from the Steam server during linking the authenticator, and in addition to that, the `device_id` field, which may be needed for other (third-party) authenticators, if you ever decide to import that `maFile` into them.

ASF importiert automatisch Ihren Authentifikator. Sobald die Prozedur abgeschlossen ist, sollten `2fa` und andere verwandte Befehle nun für das Bot-Konto funktionieren, mit dem Sie den Authentifikator verbunden haben. We recommend you to verify that.

---

## Importieren

Der Import erfordert bereits einen verknüpften und funktionsfähigen Authentifikator, der von ASF unterstüzt wird. We have instructions for a few different official and unofficial sources of 2FA, on top of manual method which allows you to provide required credentials yourself. Please note that those instructions should be used **only** if you're already using given solution - since process here involves third-party apps and tools, **we do not recommend using them**, and we're mentioning it exclusively for people that already decided to use them and would like to import generated details into ASF 2FA.

Alle folgenden Anleitungen erfordern von Ihnen, dass Sie bereits einen **funktionierenden und betriebsbereiten** Authentifikator haben, der mit dem gegebenen Programm/Werkzeug verwendet wird. ASF-2FA funktioniert nicht ordnungsgemäß, wenn Sie ungültige Daten importieren. Stelle daher sicher, dass Ihr Authentifikator ordnungsgemäß funktioniert, bevor Sie versuchen ihn zu importieren. Dies beinhaltet das Testen und Überprüfen, ob die folgenden Authentifikator-Funktionen ordnungsgemäß funktionieren:
- Sie können Codes generieren und diese Codes werden vom Steam-Netzwerk akzeptiert (Sie können sich damit anmelden)
- Sie können Bestätigungen abrufen und diese kommen auf Ihrem mobilen Authentifikator an
- You can react to those confirmations, and they're properly recognized by Steam network as confirmed/rejected

Ensure that your authenticator works by checking if above actions work - if they don't, then they won't work in ASF either.

---

### Android-Handy

Im Allgemeinen benötigen Sie für den Import des Authentifikators von Ihrem Android-Handy **[root](https://de.wikipedia.org/wiki/Rooting_(Android_OS))**-Zugriff. Die folgenden Anweisungen erfordern von Ihnen gewisse Kenntnisse aus der „Android Modding-Welt“; wir werden definitiv nicht jeden Schritt hier erklären. Besuchen Sie **[XDA](https://xdaforums.com)** und andere Ressourcen für zusätzliche Informationen/Hilfe zum unten stehenden.

Assuming you have official **[Steam app](https://play.google.com/store/apps/details?id=com.valvesoftware.android.steam.community)** working and operational (requires rooting your device):

- Installieren Sie **[Magisk](https://topjohnwu.github.io/Magisk/install.html)** und aktivieren Sie Zygisk in den Einstellungen.
- Installieren Sie **[LSPosed](https://github.com/LSPosed/LSPosed?tab=readme-ov-file#install)** (für Zygisk) und stellen Sie sicher, dass es funktioniert.
- Installieren Sie das LPosed-Modul **[SteamGuardExtractor](https://github.com/hax0r31337/SteamGuardExtractor?tab=readme-ov-file#usage)** und aktivieren Sie es in den LSPosed-Einstellungen.
- Erzwingen Sie das Beenden der Steam-App, und öffnen Sie diesen (den Extraktor). Ein **[Fenster mit extrahierten Details](https://github.com/JustArchiNET/ArchiSteamFarm/assets/1069029/ab5ab71e-d664-4e49-9eb4-9f4d9ba32aa2)** sollte erscheinen. Klicken Sie auf *Copy* (kopieren).

Nachdem Sie die benötigten Details erfolgreich extrahiert haben, deaktivieren Sie das Modul, um zu verhindern, dass das Fenster jedes Mal angezeigt wird; kopieren Sie dann den Wert von `shared_secret` und `identity_secret` des Kontos, das Sie zu ASF-2FA hinzufügen möchten, in eine neue Textdatei mit folgender Struktur:

```json
{
  "shared_secret": "STRING",
  "identity_secret": "STRING"
}
```

Ersetzen Sie jeden `STRING` Wert durch einen passenden privaten Schlüssel aus den extrahierten Details. Benennen Sie die Datei in `BotName.maFile` um, wobei `BotName` der Name Ihres Bots ist, zu dem Sie ASF-2FA hinzufügen und legen Sie es in ASF’s Verzeichnis `config` ab, wenn Sie es bislang nicht getan haben.

Launch ASF, which should notice your file and import it. Assuming that you've imported the correct file with valid secrets, everything should work properly, which you can verify by using `2fa` commands. Falls Sie einen Fehler gemacht haben, können Sie `Bot.db` jederzeit entfernen und bei Bedarf neu beginnen.

---

### SteamDesktopAuthenticator (SDA)

Wenn Sie Ihren Authentifikator bereits in SDA nutzen, dann sollten Sie die Datei `steamID.maFile` im Ordner `maFiles` beachten. Vergewissern Sie sich, dass `maFile` in unverschlüsselter Form vorliegt, da ASF diese SDA-Dateien nicht entschlüsseln kann – unverschlüsselte Dateiinhalte sollten mit dem Zeichen `{` beginnen und endet mit `}`. Falls notwendig, können Sie die Verschlüsselung zuerst aus den SDA-Einstellungen entfernen und im Anschluss erneut aktivieren, sobald Sie fertig sind. Sobald die Datei in unverschlüsselter Form vorliegt, kopieren Sie sie in das ASF-Verzeichnis `config`.

Sie sollten nun `steamID.maFile` in `BotName.maFile` im ASF-Konfigurationsverzeichnis umbenennen, wobei `BotName` der Name Ihres Bots darstellt, den Sie ASF-2FA hinzufügen. Alternativ können Sie es so lassen wie es ist; ASF wird es dann nach dem Einloggen automatisch auswählen. Die Umbenennung der Datei erlaubt ASF die Verwendung der ASF-2FA vor dem Einloggen. Falls Sie dies unterlassen, dann kann die Datei erst ausgewählt werden, sobald ASF sich erfolgreich einloggt hat (da ASF `steamID` Ihres Kontos nicht kennt, bevor Sie sich tatsächlich anmelden).

Launch ASF, which should notice your file and import it. Assuming that you've imported the correct file with valid secrets, everything should work properly, which you can verify by using `2fa` commands. Falls Sie einen Fehler gemacht haben, können Sie `Bot.db` jederzeit entfernen und bei Bedarf neu beginnen.

---

### WinAuth

Erstellen Sie zunächst eine neue, leere `BotName.maFile`-Datei im ASF-Konfigurationsverzeichnis, wobei `BotName` der Name Ihres Bot darstellt, dem Sie ASF-2FA hinzufügen. Falls Sie einen falschen Namen angeben, wird dieser nicht von ASF ausgewählt.

Nun starten Sie WinAuth wie gewohnt. Nach einem Rechtsklick auf das Steam-Symbol wählen Sie *»Show SteamGuard and Recovery Code«*. Dann setzen Sie einen Haken bei *»Allow copy«*. Ihnen sollte die JSON-Struktur am unteren Rand des Fensters vertraut vorkommen – beginnend mit `{`. Kopieren Sie den gesamten Text in die Datei `BotName.maFile`, die Sie im vorherigen Schritt erstellt haben.

Launch ASF, which should notice your file and import it. Assuming that you've imported the correct file with valid secrets, everything should work properly, which you can verify by using `2fa` commands. Falls Sie einen Fehler gemacht haben, können Sie `Bot.db` jederzeit entfernen und bei Bedarf neu beginnen.

---

### Manual

Wenn Sie ein fortgeschrittener Benutzer sind, können Sie die maFile-Datei auch manuell generieren. Dies kann in solchen Fällen verwendet werden, wenn Sie den Authentifikator aus anderen, als den oben erläuterten Quellen, importieren möchten. Es sollte eine **[gültige JSON-Struktur](https://jsonlint.com)** aufweisen:

```json
{
  "shared_secret": "STRING",
  "identity_secret": "STRING"
}
```

Standard-Authentifikatordaten beinhalten mehr Felder – sie werden von ASF beim Import völlig ignoriert, da sie nicht benötigt werden. You don't need to remove them - ASF only requires valid JSON with 2 mandatory fields described above, and will ignore additional fields (if any). Natürlich müssen Sie im obigen Beispiel `STRING` Platzhalter durch gültige Werte für Ihr Konto ersetzen. Jede `STRING` sollte eine base64 kodierte Darstellung von Bytes sein, aus denen der entsprechende private Schlüssel besteht.

---

## FAQ (häufig gestellte Fragen)

### Wie nutzt ASF das 2FA-Modul?

Wenn ASF-2FA verfügbar ist, wird ASF es für die automatische Bestätigung von Handelsangeboten verwenden, die von ASF gesendet oder akzeptiert werden. Es wird auch in der Lage sein, automatisch 2FA-Codes zu generieren, z. B. um sich anzumelden. Überdies ermöglicht ASF-2FA auch die Verwendung von `2fa`-Befehlen.

---

### How can I obtain 2FA token?

Sie benötigen einen 2FA-Code, um auf das 2FA-geschützte Konto zuzugreifen, das auch jedes Konto mit ASF-2FA einbezieht. If you've decided to use standalone authenticator, then you should use `2fa <BotNames>` command to generate temporary token for given bot instances. In all other scenarios, we recommend to use original authenticator that you've used, although you can use the command as well if it's more convenient to you.

---

### Kann ich nach dem Import zu ASF-2FA meinen Original-Authentifikator verwenden?

Ja, Ihr Original-Authentifikator bleibt funktionsfähig und Sie können ihn zusammen mit ASF-2FA verwenden. Keep in mind however that if you invalidate it through any method, then linked ASF 2FA credentials will also no longer be valid.

---

### Wie entferne ich ASF-2FA?

Stoppen Sie einfach ASF und entfernen Sie die zugehörige Datei `BotName.db` des Bots mit der verlinkten ASF-2FA, die Sie entfernen möchten. This option will remove associated imported 2FA with ASF, but will NOT invalidate (unlink) your authenticator. If you instead want to invalidate your authenticator, apart from removing it from ASF (firstly), you should unlink it in original authenticator of your choice. If you can't do that for some reason, for example because you're using ASF 2FA in standalone mode, then use revocation code that you've received during setup, on the Steam website. It's not possible to invalidate your authenticator through ASF.

---

### I linked authenticator in third-party app, then imported to ASF. Can I now link it again on my phone?

**Nein**. Doing so will invalidate the previously imported credentials and your ASF 2FA will stop functioning (by generating codes no longer being accepted by Steam). Firstly decide where you want to have your original or third-party authenticator located, then import it as ASF 2FA.

---

### Is using ASF 2FA better than third-party authenticator set to accept all confirmations?

**Ja**, auf unterschiedlicher Weise. Die erste und wichtigste – die Verwendung von ASF-2FA erhöht **erheblich** Ihre Sicherheit, da das ASF-2FA-Modul sicherstellt, dass ASF nur automatisch seine eigenen Bestätigungen akzeptiert; sodass, wenngleich der Angreifer ein schädliches Handelsangebot sendet, ASF-2FA dieses Handelsangebot **nicht** akzeptiert, da er nicht von ASF generiert wurde. In addition to security part, using ASF 2FA also brings performance/optimization benefits, as ASF 2FA fetches and accepts confirmations immediately after they're generated, and only then, as opposed to inefficient polling for confirmations each X minutes which is achieved by other solutions. There is no reason to use third-party authenticator over ASF 2FA, if you plan on automating confirmations generated by ASF - that's exactly what ASF 2FA is for, and using it does not conflict with you confirming everything else in authenticator of your choice. We strongly recommend to use ASF 2FA for entire ASF activity.
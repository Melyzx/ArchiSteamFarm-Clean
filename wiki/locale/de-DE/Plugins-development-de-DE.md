# Erweiterungsentwicklung

ASF bietet Unterstützung für benutzerdefinierte Plugins, die während der Ausführung geladen werden können. Erweiterungen ermöglichen es Ihnen, das ASF-Verhalten anzupassen, z. B. durch Hinzufügen von benutzerdefinierten Befehlen, einer benutzerdefinierten Handelslogik oder der vollständigen Integration mit Diensten und APIs von Drittanbietern.

Diese Seite beschreibt ASF Erweiterungen aus der Perspektive der Entwickler - Erstellung, Wartung, Veröffentlichung und ähnliches. Wenn Sie die Benutzerperspektive sehen wollen, klicken Sie **[hier.](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)**

---

## Grundlegendes

Plugins sind standardmäßige .NET Bibliotheken, welche eine Klasse definieren, die von der in ASF deklarierten gemeinsamen `IPlugin` Schnittstelle erbt. Sie können Plugins völlig unabhängig von der ASF-Hauptversion entwickeln und sie in aktuellen und zukünftigen ASF-Versionen wiederverwenden, solange die interne ASF-API kompatibel bleibt. Das in ASF verwendete Plugin-System basiert auf `System.Composition`, früher bekannt als **[Managed Extensibility Framework](https://docs.microsoft.com/dotnet/framework/mef)**, das ASF erlaubt, Ihre Bibliotheken während der Laufzeit zu erkennen und zu laden.

---

### Erste Schritte

Wir haben ein **[ASF-PluginTemplate](https://github.com/JustArchiNET/ASF-PluginTemplate)** für Sie vorbereitet, das Sie als Basis für Ihr Plugin-Projekt verwenden können (und sollten). Die Verwendung der Vorlage ist **keine Voraussetzung** (da Sie alles von Grund auf neu machen können), aber wir empfehlen dringend, sie zu verwenden, da sie Ihre Entwicklung drastisch beschleunigen und die Zeit, die Sie benötigen, um alles richtig zu machen, reduzieren kann. Schauen Sie sich einfach die **[README](https://github.com/JustArchiNET/ASF-PluginTemplate/blob/main/README.md)** der Vorlage an, dort finden Sie weitere Anleitungen. Unabhängig davon werden wir im Folgenden die Grundlagen abdecken, für den Fall, dass Sie ganz von vorne anfangen wollen oder die in der Plugin-Vorlage verwendeten Konzepte besser verstehen wollen - das ist normalerweise nicht nötig, wenn Sie sich für unsere Plugin-Vorlage entschieden haben.

Ihr Projekt sollte eine Standard-.NET-Bibliothek sein, die auf das entsprechende Framework Ihrer Ziel-ASF-Version abzielt, wie im Abschnitt **[Kompilierung](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compilation)** angegeben.

Das Projekt muss auf die Haupt-Assembly `ArchiSteamFarm` verweisen, entweder auf die vorgefertigte Bibliothek `ArchiSteamFarm.dll`, die Sie als Teil der Version heruntergeladen haben, oder auf das Quellprojekt (z.B. wenn Sie sich entschieden haben, den ASF-Baum als Submodul hinzuzufügen). Dadurch können Sie auf ASF-Strukturen, Methoden und Eigenschaften zugreifen und diese entdecken, insbesondere die Kernschnittstelle `IPlugin`, von der Sie im nächsten Schritt erben müssen. Das Projekt muss auch mindestens `System.Composition.AttributedModel` referenzieren, was Ihnen erlaubt, Ihr `IPlugin` für ASF zu `exportieren`. Überdies kann/muss man auf andere gemeinsame Bibliotheken verweisen, um die Datenstrukturen zu interpretieren, die man in manchen Schnittstellen erhält, aber wenn man sie nicht explizit benötigt, reicht das fürs Erste.

Wenn Sie alles richtig gemacht haben, wird Ihr `csproj` ähnlich wie unten aussehen:

```csproj
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net9.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <!-- Since you're loading plugin into the ASF process, which already includes this dependency, IncludeAssets="compile" allows you to omit it from the final output -->
    <PackageReference Include="System.Composition.AttributedModel" IncludeAssets="compile" Version="9.0.0" />
  </ItemGroup>

  <ItemGroup>
    <ProjectReference Include="C:\\Path\To\ArchiSteamFarm\ArchiSteamFarm.csproj" ExcludeAssets="all" Private="false" />

    <!-- If building with downloaded DLL binary, use this instead of <ProjectReference> above -->
    <!-- <Reference Include="ArchiSteamFarm" HintPath="C:\\Path\To\Downloaded\ArchiSteamFarm.dll" /> -->
  </ItemGroup>
</Project>
```

Codeseitig muss Ihre Plugin-Klasse von der Schnittstelle `IPlugin` (entweder explizit oder implizit durch Erben einer spezielleren Schnittstelle wie `IASF`) und `[Export(typeof(IPlugin))]` erben, um von ASF zur Laufzeit erkannt zu werden. Das einfachste Beispiel, das dies ermöglicht, ist das folgende:

```csharp
using System;
using System.Composition;
using System.Threading.Tasks;
using ArchiSteamFarm;
using ArchiSteamFarm.Plugins;

namespace YourNamespace.YourPluginName;

[Export(typeof(IPlugin))]
public sealed class YourPluginName : IPlugin {
	public string Name => nameof(YourPluginName);
	public Version Version => typeof(YourPluginName).Assembly.GetName().Version;

	public Task OnLoaded() {
		ASF.ArchiLogger.LogGenericInfo("Hello World!");

		return Task.CompletedTask;
	}
}
```

Um Ihre Erweiterung verwenden zu können, müssen Sie diese zunächst kompilieren. Sie können das entweder von Ihrer Entwicklungsumgebung aus oder aus dem Stammverzeichnis Ihres Projekts mittels eines Befehls bewältigen:

```shell
# Wenn Ihr Projekt eigenständig ist (es ist nicht notwendig, seinen Namen zu definieren, da es das Einzige ist)
dotnet publish -c "Release" -o "out"

# Falls Ihr Projekt Teil des ASF-Quellbaums ist (um das Kompilieren unnötiger Teile zu vermeiden)
dotnet publish DeinPluginName -c "Release" -o "out"
```

Danach ist Ihre Erweiterung einsatzbereit. Es bleibt Ihnen überlassen, wie genau Sie Ihr Plugin verteilen und veröffentlichen wollen, aber wir empfehlen, ein Zip-Archiv zu erstellen, in dem Sie Ihr kompiliertes Plugin zusammen mit seinen **[Abhängigkeiten](#plugin-dependencies)** ablegen. Auf diese Weise müssen die Benutzer Ihr Zip-Archiv lediglich in ein eigenständiges Unterverzeichnis innerhalb ihres `plugins`-Verzeichnisses entpacken und nichts weiter tun.

Dies ist nur das grundlegendste Szenario, um zu beginnen. Wir haben ein **[`BeispielPlugin`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.CustomPlugins.ExamplePlugin)** Projekt, das Ihnen Beispielschnittstellen und Aktionen zeigt, die Sie in Ihrem eigenen Plugin ausführen können, einschließlich hilfreicher Kommentare. Werfen Sie einen Blick darauf, wenn Sie von einem funktionierenden Code lernen möchten, oder entdecken Sie selbst den `ArchiSteamFarm.Plugins`-Bereich und lesen Sie die mitgelieferte Dokumentation für alle verfügbaren Optionen. Im Folgenden werden wir einige Kernkonzepte näher erläutern, um sie besser zu verstehen.

Wenn Sie statt eines Beispiel-Plugins lieber von echten Projekten lernen möchten, gibt es mehrere offizielle Plugins, die von uns entwickelt wurden, z.B. **[`ItemsMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.ItemsMatcher)**, **[`MobileAuthenticator`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.MobileAuthenticator)** oder **[`SteamTokenDumper`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.SteamTokenDumper)**. Darüber hinaus gibt es auch Plugins, die von anderen Entwicklern entwickelt wurden, in unserem **[Drittanbieter](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party#asf-plugins)** Bereich.

---

### API Verfügbarkeit

ASF stellt Ihnen neben dem, worauf Sie in den Schnittstellen selbst Zugriff haben, viele seiner internen APIs zur Verfügung, die Sie verwenden können, um die Funktionalität zu erweitern. Wenn Sie etwa eine neue Art einer Steam-Web-Anfrage senden möchten, dann müssen Sie nicht alles von Grund auf neu implementieren; insbesondere nicht all den Problemen, mit denen wir uns vor Ihnen beschäftigt haben. Verwenden Sie einfach unseren `Bot.ArchiWebHandler`, der bereits eine Menge von `UrlWithSession()`-Methoden für Sie bereitstellt, die alle untergeordneten Dinge wie Authentifizierung, Sitzungsaktualisierung oder Web-Limitierung für Sie erledigen. Auch für das Senden von Web-Anfragen außerhalb der Steam-Plattform könnte man die Standard-.NET-Klasse `HttpClient` verwenden, aber es ist eine viel bessere Idee, `Bot.ArchiWebHandler.WebBrowser` zu verwenden, die für Sie zur Verfügung steht, die Ihnen wiederum eine hilfreiche Hand bietet, zum Beispiel in Bezug auf die Wiederholung von fehlgeschlagenen Anfragen.

Wir haben eine sehr offene Politik in Bezug auf unsere API-Verfügbarkeit. Wenn Sie also etwas nutzen möchten, das bereits im ASF-Code enthalten ist, **[eröffnen Sie einfach ein Problem](https://github.com/JustArchiNET/ArchiSteamFarm/issues)** und erklären Sie darin Ihren geplanten Anwendungsfall unserer ASF-internen API. Wir werden höchstwahrscheinlich nichts dagegen haben, solange Ihr Anwendungsfall Sinn ergibt. Dies schließt auch alle Vorschläge in Bezug auf neue „Plugin“-Schnittstellen ein, die sinnvollerweise hinzugefügt werden könnten, um bestehende Funktionen zu erweitern.

Unabhängig von der Verfügbarkeit der ASF-API hält Sie jedoch nichts davon ab, z. B. die Bibliothek „Discord.Net“ in Ihre Anwendung einzubinden und eine Brücke zwischen Ihrem Discord-Bot und ASF-Befehlen zu schlagen, da Ihr Plugin auch eigene Abhängigkeiten haben kann. Die Möglichkeiten sind endlos, und wir haben unser Bestes getan, um Ihnen so viel Freiheit und Flexibilität wie möglich innerhalb Ihres Plugins zu geben, so dass es keine künstlichen Grenzen gibt - Ihr Plugin wird in den ASF-Hauptprozess geladen und kann alles tun, was realistischerweise innerhalb von C#-Code möglich ist.

---

### API Kompatibilität

Es ist wichtig zu betonen, dass ASF eine Verbraucher-Anwendung ist und nicht eine typische Bibliothek mit fester API-Oberfläche, auf die Sie sich bedingungslos verlassen können. This means that you can't assume that your plugin once compiled will keep working with all future ASF releases regardless, it's simply impossible if we want to keep developing the program further, and being unable to adapt to ever-ongoing Steam changes for the sake of backwards compatibility is just not appropriate for our case. Das sollte für Sie logisch sein, aber es ist wichtig, diese Tatsache hervorzuheben.

We'll do our best to keep public parts of ASF working and stable, but we'll not be afraid to break the compatibility if good enough reasons arise, following our **[deprecation](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Deprecation)** policy in the process. This is especially important in regards to internal ASF structures that are exposed to you as part of ASF infrastructure (e.g. `ArchiWebHandler`) which could be improved (and therefore rewritten) as part of ASF enhancements in one of the future versions. Wir werden unser Bestes tun, um Sie in den Änderungsprotokollen angemessen zu informieren und während der Laufzeit (runtime) entsprechende Warnungen über veraltete Funktionen zu geben. Wir beabsichtigen nicht alles neu zu schreiben, um es neu zu schreiben, also können Sie ziemlich sicher sein, dass die nächste kleinere ASF-Version Ihre Erweiterung nicht einfach nur deshalb komplett zerstört, weil es eine höhere Versionsnummer hat. Aber es ist eine gute Idee ein Auge auf Änderungsprotokolle zu werfen und gelegentlich zu überprüfen, ob alles einwandfrei funktioniert.

---

### Plugin-Abhängigkeiten

Your plugin will include at least two dependencies by default, `ArchiSteamFarm` reference for internal API (`IPlugin` at the minimum), and `PackageReference` of `System.Composition.AttributedModel` that is required for being recognized as ASF plugin to begin with (`[Export]` clause). In addition to that, it may include more dependencies in regards to what you've decided to do in your plugin (e.g. `Discord.Net` library if you've decided to integrate with Discord).

The output of your build will include your core `YourPluginName.dll` library, as well as all the dependencies that you've referenced. Since you're developing a plugin to already-working program, you don't have to, and even **shouldn't** include dependencies that ASF already includes, for example `ArchiSteamFarm`, `SteamKit2` or `AngleSharp`. Das Entfernen von geteilten ASF-Abhängigkeiten in Ihrem Build ist nicht unbedingt erforderlich für die Funktionsfähigkeit einer Erweiterung, aber dies wird den Speicherbedarf, die Größe dieser drastisch reduzieren, und gleichzeitig die Leistung erhöhen; da ASF seine eigenen Abhängigkeiten mit Ihnen teilt und nur die Bibliotheken lädt, die es nicht über sich selbst kennt.

Generell wird empfohlen, nur Bibliotheken einzubinden, die ASF entweder gar nicht oder nur in einer falschen/inkompatiblen Version enthält. Examples of those would be obviously `YourPluginName.dll`, but for example also `Discord.Net.dll` if you decided to depend on it, as ASF doesn't include it itself. Bundling libraries that are shared with ASF can still make sense if you want to ensure API compatibility (e.g. being sure that `AngleSharp` which you depend on in your plugin will always be in version `X` and not the one that ASF ships with), but obviously doing that comes for a price of increased memory/size and worse performance, and therefore should be carefully evaluated.

If you know that the dependency which you need is included in ASF, you can mark it with `IncludeAssets="compile"` as we showed you in the example `csproj` above. Dies wird dem Compiler mitteilen, die Veröffentlichung der referenzierter Bibliothek selbst zu vermeiden, da ASF diese bereits beinhaltet. Likewise, notice that we reference the ASF project with `ExcludeAssets="all" Private="false"` which works in a very similar way - telling the compiler to not produce any ASF files (as the user already has them). This applies only when referencing ASF project, since if you reference a `dll` library, then you're not producing ASF files as part of your plugin.

If you're confused about above statement and you don't know better, check which `dll` libraries are included in `ASF-generic.zip` package and ensure that your plugin includes only those that are not part of it yet. This will be only `YourPluginName.dll` for the most simple plugins. Fügen Sie auch die betroffenen Bibliotheken hinzu, wenn Sie während der Laufzeit Probleme in Bezug auf einige Bibliotheken. Wenn alles andere fehlschlägt, können Sie sich jederzeit entscheiden, alles zu bündeln.

---

### Native Abhängigkeiten

Native Abhängigkeiten werden als Teil von betriebssystemspezifischen Builds generiert, da auf dem Host keine .NET Runtime verfügbar ist und ASF seine eigene .NET Runtime nutzt, die als Teil des betriebssystemspezifischen Builds gebündelt wird. Um die Größe des Builds zu minimieren, reduziert ASF seine nativen Abhängigkeiten so, dass nur der Programmcode berücksichtigt wird, der innerhalb des Programms erreichbar ist, was die ungenutzten Teile der Laufzeit effektiv reduziert. This can create a potential problem for you in regards to your plugin, if suddenly you find out yourself in a situation where your plugin depends on some .NET feature that isn't being used in ASF, and therefore OS-specific builds can't execute it properly, usually throwing `System.MissingMethodException` or `System.Reflection.ReflectionTypeLoadException` in the process. As your plugin grows in size and becomes more and more complex, sooner or later you'll hit the surface that is not covered by our OS-specific build.

Dies ist bei generischen Builds nie ein Problem, da es sich hierbei nie um native Abhängigkeiten handelt (da diese die gesamte lauffähige Runtime auf dem Host haben und ASF ausführen). This is why it's a recommended practice to **use your plugin in generic builds exclusively**, but obviously that has its own downside of cutting your plugin from users that are running OS-specific builds of ASF. Falls Sie sich fragen, ob Ihr Problem im Zusammenhang mit nativen Abhängigkeiten steht, können Sie diese Methode auch zur Überprüfung verwenden. Lade Ihre Erweiterung in der generischen ASF Variante und schauen Sie, ob es funktioniert. Wenn dies der Fall ist, sind die Plugin-Abhängigkeiten abgedeckt, sodass native Abhängigkeiten Probleme verursachen.

Wir mussten entscheiden, ob wir die gesamte Laufzeit als Teil unserer OS-spezifischen Builds veröffentlichen oder es von nicht genutzten Funktionen trennen. Das würde die Builds um über 80 MB im Vergleich zu einer vollständigen Version reduzieren. Wir haben die letzte Option ausgewählt, und es ist leider unmöglich für Sie, die fehlenden Laufzeitfunktionen zusammen mit Ihrer Erweiterung hinzuzufügen. If your project requires access to runtime features that are left out, you have to include full .NET runtime that you depend on, and that means running your plugin in `generic` ASF flavour. Sie können Ihre Erweiterung nicht in OS-spezifischen Builds ausführen, da diese einfach die benötigte Laufzeit-Funktion vermisst, und .NET Runtime ist derzeit nicht in der Lage native Abhängigkeiten „zusammenzuführen“ (unsere zur Verfügung gestellten mit Ihrer). Vielleicht wird es sich eines Tages bessern, aber zum jetzigen Zeitpunkt ist es einfach nicht möglich.

OS-spezifische -Builds von ASF enthalten das Minimum an zusätzlicher Funktionalität, die für unsere offiziellen Erweiterungen erforderlich ist. Abgesehen von dieser Möglichkeit, ergänzt dies auch die Oberfläche leicht um zusätzliche Abhängigkeiten für die grundlegendsten Erweiterungen. Als Konsequenz müssen sich nicht alle Erweiterungen um native Abhängigkeiten kümmern – nur jene, die über das hinausgehen, was ASF (bzw. unsere offiziellen Erweiterungen) direkt benötigen. Dies geschieht als Extra; da wir selbst bereits zusätzliche native Abhängigkeiten für unsere eigenen Anwendungsfälle einbeziehen müssen, können wir diese auch direkt mit ASF bündeln, sodass sie leichter abdeckbar bleiben, auch für Sie. Leider reicht das nicht immer aus; und da Ihre Erweiterung größer und komplexer wird, erhöht sich die Wahrscheinlichkeit, dass sie mit eingeschränkter Funktionalität ausführbar ist. Therefore, we usually recommend you to run your custom plugins in `generic` ASF flavour exclusively. Sie können immer noch manuell überprüfen, ob OS-spezifische ASF-Versionen alles haben, was die Erweiterung für seine Funktionalität benötigt – aber da sich dies sowohl bei Ihren, als auch bei unserem Update ändert, könnte es schwierig sein dieses zu pflegen.

Sometimes it may be possible to "workaround" missing features by either using alternative options or reimplementing them in your plugin. This is however not always possible or viable, especially if the missing feature comes from third-party dependencies that you include as part of your plugin. You can always try to run your plugin in OS-specific build and attempt to make it work, but it might become too much hassle in the long-run, especially if you want from your code to just work, rather than fight with ASF surface.

---

## Automatic updates

ASF offers you two interfaces for implementing automatic updates in your plugin:

- `IGitHubPluginUpdates` providing you easy way to implement GitHub-based updates similar to general ASF update mechanism
- `IPluginUpdates` providing you lower-level functionality that allows for custom update mechanism, if you require something more complex

---

### **[`IGithubPluginUpdates`](https://github.com/JustArchiNET/ArchiSteamFarm/blob/main/ArchiSteamFarm/Plugins/Interfaces/IGitHubPluginUpdates.cs)**

The minimum checklist of things that are required for updates to work:

- You need to declare `RepositoryName`, which defines where the updates are pulled from.
- You need to make use of tags and releases provided by GitHub. Tag must be in format parsable to a plugin version, e.g. `1.0.0.0`.
- `Version` property of the plugin must match tag that it comes from. This means that binary available under tag `1.2.3.4` must present itself as `1.2.3.4`.
- Each tag should have appropriate release available on GitHub with zip file release asset that includes your plugin binary files. The binary files that include your `IPlugin` classes should be available in the root directory within the zip file.

This will allow the mechanism in ASF to:

- Resolve current `Version` of your plugin, e.g. `1.0.1`.
- Use GitHub API to fetch latest `tag` available in `RepositoryName` repo, e.g. `1.0.2`.
- Determine that `1.0.2` > `1.0.1`, check release of `1.0.2` tag to find `.zip` file with the plugin update.
- Download that `.zip` file, extract it, and put its content in the directory that included `YourPlugin.dll` before.
- Restart ASF to load your plugin in `1.0.2` version.

Additional notes:

- Plugin updates may require appropriate ASF config values, namely **[`PluginsUpdateMode`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#pluginsupdatemode)** and/or **[`PluginsUpdateList`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#pluginsupdatelist)**.
- Our plugin template already includes everything you need, simply **[rename](https://github.com/JustArchiNET/ASF-PluginTemplate?tab=readme-ov-file#renaming-myawesomeplugin)** the plugin to proper values, and it'll work out of the box.
- You can use combination of latest release as well as pre-releases, they'll be used as per `UpdateChannel` the user has defined.
- There is `CanUpdate` boolean property you can override for disabling/enabling plugins update on your side, for example if you want to skip updates temporarily, or through any other reason.
- It's possible to have multiple zip files in the release if you want to target different ASF versions. See `GetTargetReleaseAsset()` **[remarks](https://github.com/JustArchiNET/ArchiSteamFarm/blob/main/ArchiSteamFarm/Plugins/Interfaces/IGitHubPluginUpdates.cs#L79-L106)**. For example, you can have `MyPlugin-V6-0.zip` as well as `MyPlugin.zip`, which will cause ASF in version `V6.0.x.x` to pick the first one, while all other ASF versions will use the second one.
- If the above is not sufficient to you and you require custom logic for picking the correct `.zip` file, you can override `GetTargetReleaseAsset()` function and provide the artifact for ASF to use.
- If your plugin needs to do some extra work before/after update, you can override `OnPluginUpdateProceeding()` and/or `OnPluginUpdateFinished()` methods.

---

### **[`IPluginUpdates`](https://github.com/JustArchiNET/ArchiSteamFarm/blob/main/ArchiSteamFarm/Plugins/Interfaces/IPluginUpdates.cs)**

This interface allows you to implement custom logic for updates if by any reason `IGithubPluginUpdates` is not sufficient to you, for example because you have tags that do not parse to versions, or because you're not using GitHub platform at all.

There is a single `GetTargetReleaseURL()` function for you to override, which expects from you `Uri?` of target plugin update location. ASF provides you some core variables that relate to the context the function was called with, but other than that, you're responsible for implementing everything you need in that function and providing ASF the URL that should be used, or `null` if the update is not available.

Other than that, it's similar to GitHub updates, where the URL should point to a `.zip` file with the binary files available in the root directory.
# Plugins development

ASF includes support for custom plugins that can be loaded during runtime. Eklentiler ASF'in davranışlarını özelleştirmeye yarar, örnek olarak özel komutlar ekleyerek, özel takas mantığı ekleyerek veya üçüncü parti servisleri ve uygulama programlama arayüzü ile entegrasyon ekleyerek.

This page describes ASF plugins from developers perspective - creating, maintaining, publishing and likewise. If you want to view user's perspective, move **[here](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** instead.

---

## Temel

Plugins are standard .NET libraries that define a class inheriting from common `IPlugin` interface declared in ASF. You can develop plugins entirely independently of mainline ASF and reuse them in current and future ASF versions, as long as internal ASF API remains compatible. Plugin system used in ASF is based on `System.Composition`, formerly known as **[Managed Extensibility Framework](https://docs.microsoft.com/dotnet/framework/mef)** which allows ASF to discover and load your libraries during runtime.

---

### Başlarken

We've prepared **[ASF-PluginTemplate](https://github.com/JustArchiNET/ASF-PluginTemplate)** for you, which you can (and should) use as a base for your plugin project. Using the template is **not a requirement** (as you can do everything from scratch), but we heavily recommend to pick it up as it can drastically kickstart your development and cut on time required to get all things right. Simply check out the **[README](https://github.com/JustArchiNET/ASF-PluginTemplate/blob/main/README.md)** of the template and it'll guide you further. Regardless, we'll cover the basics below in case you wanted to start from scratch, or get to understand better the concepts used in the plugin template - you don't typically need to do any of that if you've decided to use our plugin template.

Your project should be a standard .NET library targetting appropriate framework of your target ASF version, as specified in the **[compilation](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compilation)** section.

The project must reference main `ArchiSteamFarm` assembly, either its prebuilt `ArchiSteamFarm.dll` library that you've downloaded as part of the release, or the source project (e.g. if you decided to add ASF tree as a submodule). This will allow you to access and discover ASF structures, methods and properties, especially core `IPlugin` interface which you'll need to inherit from in the next step. The project must also reference `System.Composition.AttributedModel` at the minimum, which allows you to `[Export]` your `IPlugin` for ASF to use. Ek olarak, bazı arayüzlerde size verilen veri yapılarını yorumlamak için diğer ortak kütüphanelere başvurmak isteyebilir/ihtiyaç duyabilirsiniz, ancak bunlara açıkça ihtiyaç duymadığınız sürece şimdilik yeterli olacaktır.

If you did everything properly, your `csproj` will be similar to below:

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

From the code side, your plugin class must inherit from `IPlugin` interface (either explicitly, or implicitly by inheriting from more specialized interface, such as `IASF`) and `[Export(typeof(IPlugin))]` in order to be recognized by ASF during runtime. Bunu başaran en açık örnek şudur:

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

Eklentinizi kullanmak için öncelikle onu derlemelisiniz. Bunu IDE'nizden veya projenizin kök dizininden bir komutla yapabilirsiniz:

```shell
# Projeniz tek başına çalışan bileşen ise (tek proje olduğu için adını tanımlamanıza gerek yoktur.)
dotnet publish -c "Release" -o "out"
# Projeniz ASF kaynak ağacının parçasıysa (gereksiz parçaları derlemekten kaçınmak için)
dotnet publish YourPluginName -c "Release" -o "out"
```

Daha sonra eklentiniz dağıtım için hazırdır. It's up to you how exactly you want to distribute and publish your plugin, but we recommend creating a zip archive where you'll put your compiled plugin together with its **[dependencies](#plugin-dependencies)**. This way user will simply need to unpack your zip archive into a standalone subdirectory inside their `plugins` directory and do nothing else.

Bu, başlamanıza yardımcı olacak en basit senaryodur. We have **[`ExamplePlugin`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.CustomPlugins.ExamplePlugin)** project that shows you example interfaces and actions that you can do within your own plugin, including helpful comments. Feel free to take a look if you'd like to learn from a working code, or discover `ArchiSteamFarm.Plugins` namespace yourself and refer to the included documentation for all available options. We'll also further elaborate on some core concepts below to explain them better.

If instead of example plugin you'd want to learn from real projects, there are several official plugins developed by us, e.g. **[`ItemsMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.ItemsMatcher)**, **[`MobileAuthenticator`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.MobileAuthenticator)** or **[`SteamTokenDumper`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.SteamTokenDumper)**. In addition to that, there are also plugins developed by other developers, in our **[third-party](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party#asf-plugins)** section.

---

### API kullanılabilirliği

ASF, arabirimlerde eriştiğiniz şeylerin yanı sıra, işlevselliği genişletmek için kullanabileceğiniz birçok dahili API'yi size sunar. Örneğin, Steam web'e bir tür yeni istek göndermek istiyorsanız, her şeyi en baştan uygulamanıza gerek yoktur, özellikle de sizden uğraşmak zorunda olduğumuz tüm sorunlarla. Simply use our `Bot.ArchiWebHandler` which already exposes a lot of `UrlWithSession()` methods for you to use, handling all the lower-level stuff such as authentication, session refresh or web limiting for you. Likewise, for sending web requests outside of Steam platform, you could use standard .NET `HttpClient` class, but it's much better idea to use `Bot.ArchiWebHandler.WebBrowser` that is available for you, which once again offers you a helpful hand, for example in regards to retrying failed requests.

We have a very open policy in terms of our API availability, so if you'd like to make use of something that ASF code already includes, simply **[open an issue](https://github.com/JustArchiNET/ArchiSteamFarm/issues)** and explain in it your planned use case of our ASF's internal API. Kullanım durumunuz mantıklı olduğu sürece, büyük olasılıkla buna karşı bir şeyimiz olmayacak. This also includes all suggestions in regards to new `IPlugin` interfaces that could make sense to be added in order to extend existing functionality.

Regardless of ASF API availability however, nothing is stopping you from e.g. including `Discord.Net` library in your application and creating a bridge between your Discord bot and ASF commands, since your plugin can also have dependencies on its own. The possibilities are endless, and we made our best to give you as much freedom and flexibility as possible within your plugin, so there are no artificial limits on anything - your plugin is loaded into the main ASF process and can do anything that is realistically possible to do from within C# code.

---

### API uyumluluğu

ASF'in bir tüketici uygulaması olduğunu ve koşulsuz olarak güvenebileceğiniz sabit API yüzeyine sahip tipik bir kitaplık olmadığını vurgulamak önemlidir. This means that you can't assume that your plugin once compiled will keep working with all future ASF releases regardless, it's simply impossible if we want to keep developing the program further, and being unable to adapt to ever-ongoing Steam changes for the sake of backwards compatibility is just not appropriate for our case. Bu sizin için mantıklı olmalı, ama bu gerçeği vurgulamak önemli.

We'll do our best to keep public parts of ASF working and stable, but we'll not be afraid to break the compatibility if good enough reasons arise, following our **[deprecation](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Deprecation)** policy in the process. This is especially important in regards to internal ASF structures that are exposed to you as part of ASF infrastructure (e.g. `ArchiWebHandler`) which could be improved (and therefore rewritten) as part of ASF enhancements in one of the future versions. Değişiklik kayıtlarında sizi uygun şekilde bilgilendirmek için elimizden gelenin en iyisini yapacağız ve eski özellikler hakkında çalışma süresi boyunca uygun uyarıları dahil edeceğiz. Yeniden yazma uğruna her şeyi yeniden yazma niyetinde değiliz, bu nedenle bir sonraki küçük ASF sürümünün yalnızca daha yüksek bir sürüm numarasına sahip olduğu için eklentinizi tamamen yok etmeyeceğinden emin olabilirsiniz, ancak değişiklik kayıtlarına göz atıp her şeyin yolunda olup olmadığını kontrol etmek iyi bir fikirdir.

---

### Eklenti gereksinimleri

Your plugin will include at least two dependencies by default, `ArchiSteamFarm` reference for internal API (`IPlugin` at the minimum), and `PackageReference` of `System.Composition.AttributedModel` that is required for being recognized as ASF plugin to begin with (`[Export]` clause). In addition to that, it may include more dependencies in regards to what you've decided to do in your plugin (e.g. `Discord.Net` library if you've decided to integrate with Discord).

The output of your build will include your core `YourPluginName.dll` library, as well as all the dependencies that you've referenced. Since you're developing a plugin to already-working program, you don't have to, and even **shouldn't** include dependencies that ASF already includes, for example `ArchiSteamFarm`, `SteamKit2` or `AngleSharp`. ASF ile paylaşılan derleme gereksinimlerinizi ortadan kaldırmak, eklentinizin çalışması için mutlak bir zorunluluk değildir, ama bunu yapmak, ASF'in sizinle kendi gereksinimlerini paylaşacağı ve yalnızca kendisi hakkında bilmediği kitaplıkları yükleyeceği için, performansı artırmanın yanı sıra bellek ayak izini ve eklentinizin boyutunu önemli ölçüde azaltacaktır.

Genel olarak, yalnızca ASF'in içermediği veya yanlış/uyumsuz sürüme sahip olan kitaplıkların dahil edilmesi önerilen bir uygulamadır. Examples of those would be obviously `YourPluginName.dll`, but for example also `Discord.Net.dll` if you decided to depend on it, as ASF doesn't include it itself. Bundling libraries that are shared with ASF can still make sense if you want to ensure API compatibility (e.g. being sure that `AngleSharp` which you depend on in your plugin will always be in version `X` and not the one that ASF ships with), but obviously doing that comes for a price of increased memory/size and worse performance, and therefore should be carefully evaluated.

If you know that the dependency which you need is included in ASF, you can mark it with `IncludeAssets="compile"` as we showed you in the example `csproj` above. Bu, derleyiciye başvurulan kitaplığı yayınlamaktan kaçınmasını söyleyecektir, eğer ASF içerisinde o kitaplığı içeriyor ise. Likewise, notice that we reference the ASF project with `ExcludeAssets="all" Private="false"` which works in a very similar way - telling the compiler to not produce any ASF files (as the user already has them). This applies only when referencing ASF project, since if you reference a `dll` library, then you're not producing ASF files as part of your plugin.

If you're confused about above statement and you don't know better, check which `dll` libraries are included in `ASF-generic.zip` package and ensure that your plugin includes only those that are not part of it yet. This will be only `YourPluginName.dll` for the most simple plugins. Runtime sırasında bazı kitaplıklarla ilgili herhangi bir sorunla karşılaşırsanız, etkilenen kitaplıkları da dahil edin. Bunlarda başarısız olursa, her şeyi bir araya getirmeye karar verebilirsiniz.

---

### Yerel gereksinimler

Native dependencies are generated as part of OS-specific builds, as there is no .NET runtime available on the host and ASF is running through its own .NET runtime that is bundled as part of OS-specific build. Derleme boyutunu en aza indirmek için ASF, yerel gereksinimlerini yalnızca program içinde erişilebilecek kodu içerecek şekilde kırpar ve bu da runtime'ın kullanılmayan kısımlarını etkin bir şekilde keser. This can create a potential problem for you in regards to your plugin, if suddenly you find out yourself in a situation where your plugin depends on some .NET feature that isn't being used in ASF, and therefore OS-specific builds can't execute it properly, usually throwing `System.MissingMethodException` or `System.Reflection.ReflectionTypeLoadException` in the process. As your plugin grows in size and becomes more and more complex, sooner or later you'll hit the surface that is not covered by our OS-specific build.

Bu, genel derlemelerde bir sorun değildir, çünkü bunlar asla ilk etapta yerel gereksinimler ile uğraşmazlar(ana bilgisayarda ASF'yi çalıştıracak tam bir runtime'a sahip oldukları için). This is why it's a recommended practice to **use your plugin in generic builds exclusively**, but obviously that has its own downside of cutting your plugin from users that are running OS-specific builds of ASF. Sorununuzun yerel gereksinimler ile ilgili olup olmadığını merak ediyorsanız, bu yöntemi doğrulama için de kullanabilir, eklentinizi ASF genel yapısına yükleyebilir ve çalışıp çalışmadığını görebilirsiniz. Eğer çalışıyorsa eklenti gereksinimlerinin hepsini sağlamışsınızdır ve problem yalnızca yerel gereksinimlerdedir.

Ne yazık ki, runtime'ı bütünüyle işletim sistemine bağlı yapılarda kullanmak ve runtime'ın gereksiz özelliklerinin kesilmiş haliyle işletim sistemine bağlı yapılarda kullanmak arasında bir seçim yapmamız gerekiyordu, bunu yapmak yapıyı FULL runtime'a göre 80 MB lik küçülmeye sonuçlandı. İkinci seçeneği seçtik ve ne yazık ki eksik çalışma zamanı özelliklerini eklentinizle birlikte eklemeniz imkansız. If your project requires access to runtime features that are left out, you have to include full .NET runtime that you depend on, and that means running your plugin in `generic` ASF flavour. You can't run your plugin in OS-specific builds, as those builds are simply missing a runtime feature that you need, and .NET runtime as of now is unable to "merge" native dependency that you could've provided with our own. Belki gelecekte bir gün gelişebilir, ama şu an itibariyle bu mümkün değil.

ASF'in işletim sistemine özgü yapıları, resmi eklentilerimizi çalıştırmak için gerekli olan minimum ek işlevselliği içerir. Bunun mümkün olmasının yanı sıra, bu aynı zamanda yüzeyi en temel eklentiler için gereken ekstra gereksinimlerle biraz genişletir. Bu nedenle, tüm eklentilerin başlangıçta yerel gereksinimler hakkında endişelenmesine gerek yoktur - yalnızca ASF ve resmi eklentilerimizin doğrudan ihtiyaç duyduklarının ötesine geçen eklentilerin ihtiyacı vardır. Buradan itibaren anlatacağım şey ekstra olarak yapılır, zaten kendi kullanım durumlarımız için ek yerel gereksinimler eklememiz gerekirse bunları doğrudan ASF ile de gönderebiliriz, bu da onları kullanılabilir hale getirir ve böylece sizin için de daha kolay olur. Unfortunately, this is not always enough, and as your plugin gets bigger and more complex, the likelihood of running into trimmed functionality increases. Therefore, we usually recommend you to run your custom plugins in `generic` ASF flavour exclusively. You can still manually verify that OS-specific build of ASF has everything that your plugin requires for its functionality - but since that changes on your updates as well as ours, it might be tricky to maintain.

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

Other than that, it's similar to GitHub updates, where the URL should point to a `.zip` file with the binary files available in the root directory. You also have `OnPluginUpdateProceeding()` and `OnPluginUpdateFinished()` methods available.
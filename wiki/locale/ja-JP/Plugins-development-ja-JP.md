# Plugins development

ASF includes support for custom plugins that can be loaded during runtime. Plugins allow you to customize ASF behaviour, for example by adding custom commands, custom trading logic or whole integration with third-party services and APIs.

This page describes ASF plugins from developers perspective - creating, maintaining, publishing and likewise. If you want to view user's perspective, move **[here](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** instead.

---

## コア

Plugins are standard .NET libraries that define a class inheriting from common `IPlugin` interface declared in ASF. You can develop plugins entirely independently of mainline ASF and reuse them in current and future ASF versions, as long as internal ASF API remains compatible. Plugin system used in ASF is based on `System.Composition`, formerly known as **[Managed Extensibility Framework](https://docs.microsoft.com/dotnet/framework/mef)** which allows ASF to discover and load your libraries during runtime.

---

### Getting started

We've prepared **[ASF-PluginTemplate](https://github.com/JustArchiNET/ASF-PluginTemplate)** for you, which you can (and should) use as a base for your plugin project. Using the template is **not a requirement** (as you can do everything from scratch), but we heavily recommend to pick it up as it can drastically kickstart your development and cut on time required to get all things right. Simply check out the **[README](https://github.com/JustArchiNET/ASF-PluginTemplate/blob/main/README.md)** of the template and it'll guide you further. Regardless, we'll cover the basics below in case you wanted to start from scratch, or get to understand better the concepts used in the plugin template - you don't typically need to do any of that if you've decided to use our plugin template.

Your project should be a standard .NET library targetting appropriate framework of your target ASF version, as specified in the **[compilation](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compilation)** section.

The project must reference main `ArchiSteamFarm` assembly, either its prebuilt `ArchiSteamFarm.dll` library that you've downloaded as part of the release, or the source project (e.g. if you decided to add ASF tree as a submodule). This will allow you to access and discover ASF structures, methods and properties, especially core `IPlugin` interface which you'll need to inherit from in the next step. The project must also reference `System.Composition.AttributedModel` at the minimum, which allows you to `[Export]` your `IPlugin` for ASF to use. In addition to that, you may want/need to reference other common libraries in order to interpret the data structures that are given to you in some interfaces, but unless you need them explicitly, that will be enough for now.

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

From the code side, your plugin class must inherit from `IPlugin` interface (either explicitly, or implicitly by inheriting from more specialized interface, such as `IASF`) and `[Export(typeof(IPlugin))]` in order to be recognized by ASF during runtime. The most bare example that achieves that would be the following:

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

In order to make use of your plugin, you must firstly compile it. You can do that either from your IDE, or from within the root directory of your project via a command:

```shell
# If your project is standalone (no need to define its name since it's the only one)
dotnet publish -c "Release" -o "out"

# If your project is part of ASF source tree (to avoid compiling unnecessary parts)
dotnet publish YourPluginName -c "Release" -o "out"
```

Afterwards, your plugin is ready for deployment. It's up to you how exactly you want to distribute and publish your plugin, but we recommend creating a zip archive where you'll put your compiled plugin together with its **[dependencies](#plugin-dependencies)**. This way user will simply need to unpack your zip archive into a standalone subdirectory inside their `plugins` directory and do nothing else.

This is only the most basic scenario to get you started. We have **[`ExamplePlugin`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.CustomPlugins.ExamplePlugin)** project that shows you example interfaces and actions that you can do within your own plugin, including helpful comments. Feel free to take a look if you'd like to learn from a working code, or discover `ArchiSteamFarm.Plugins` namespace yourself and refer to the included documentation for all available options. We'll also further elaborate on some core concepts below to explain them better.

If instead of example plugin you'd want to learn from real projects, there are several official plugins developed by us, e.g. **[`ItemsMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.ItemsMatcher)**, **[`MobileAuthenticator`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.MobileAuthenticator)** or **[`SteamTokenDumper`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.SteamTokenDumper)**. In addition to that, there are also plugins developed by other developers, in our **[third-party](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party#asf-plugins)** section.

---

### API availability

ASF, apart from what you have access to in the interfaces themselves, exposes to you a lot of its internal APIs that you can make use of, in order to extend the functionality. For example, if you'd like to send some kind of new request to Steam web, then you do not need to implement everything from scratch, especially dealing with all the issues we've had to deal with before you. Simply use our `Bot.ArchiWebHandler` which already exposes a lot of `UrlWithSession()` methods for you to use, handling all the lower-level stuff such as authentication, session refresh or web limiting for you. Likewise, for sending web requests outside of Steam platform, you could use standard .NET `HttpClient` class, but it's much better idea to use `Bot.ArchiWebHandler.WebBrowser` that is available for you, which once again offers you a helpful hand, for example in regards to retrying failed requests.

We have a very open policy in terms of our API availability, so if you'd like to make use of something that ASF code already includes, simply **[open an issue](https://github.com/JustArchiNET/ArchiSteamFarm/issues)** and explain in it your planned use case of our ASF's internal API. We most likely won't have anything against, as long as your use case makes sense. This also includes all suggestions in regards to new `IPlugin` interfaces that could make sense to be added in order to extend existing functionality.

Regardless of ASF API availability however, nothing is stopping you from e.g. including `Discord.Net` library in your application and creating a bridge between your Discord bot and ASF commands, since your plugin can also have dependencies on its own. The possibilities are endless, and we made our best to give you as much freedom and flexibility as possible within your plugin, so there are no artificial limits on anything - your plugin is loaded into the main ASF process and can do anything that is realistically possible to do from within C# code.

---

### API compatibility

It's important to emphasize that ASF is a consumer application and not a typical library with fixed API surface that you can depend on unconditionally. This means that you can't assume that your plugin once compiled will keep working with all future ASF releases regardless, it's simply impossible if we want to keep developing the program further, and being unable to adapt to ever-ongoing Steam changes for the sake of backwards compatibility is just not appropriate for our case. This should be logical for you, but it's important to highlight that fact.

We'll do our best to keep public parts of ASF working and stable, but we'll not be afraid to break the compatibility if good enough reasons arise, following our **[deprecation](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Deprecation)** policy in the process. This is especially important in regards to internal ASF structures that are exposed to you as part of ASF infrastructure (e.g. `ArchiWebHandler`) which could be improved (and therefore rewritten) as part of ASF enhancements in one of the future versions. We'll do our best to inform you appropriately in the changelogs, and include appropriate warnings during runtime about obsolete features. We do not intend to rewrite everything for the sake of rewriting it, so you can be fairly sure that the next minor ASF version won't just simply destroy your plugin entirely only because it has a higher version number, but keeping an eye on changelogs and occasional verification if everything works fine is a very good idea.

---

### Plugin dependencies

Your plugin will include at least two dependencies by default, `ArchiSteamFarm` reference for internal API (`IPlugin` at the minimum), and `PackageReference` of `System.Composition.AttributedModel` that is required for being recognized as ASF plugin to begin with (`[Export]` clause). In addition to that, it may include more dependencies in regards to what you've decided to do in your plugin (e.g. `Discord.Net` library if you've decided to integrate with Discord).

The output of your build will include your core `YourPluginName.dll` library, as well as all the dependencies that you've referenced. Since you're developing a plugin to already-working program, you don't have to, and even **shouldn't** include dependencies that ASF already includes, for example `ArchiSteamFarm`, `SteamKit2` or `AngleSharp`. Stripping down your build off dependencies shared with ASF is not the absolute requirement for your plugin to work, but doing so will dramatically cut the memory footprint and the size of your plugin, together with increasing the performance, due to the fact that ASF will share its own dependencies with you, and will load only those libraries that it doesn't know about itself.

In general, it's a recommended practice to include only those libraries that ASF either doesn't include, or includes in the wrong/incompatible version. Examples of those would be obviously `YourPluginName.dll`, but for example also `Discord.Net.dll` if you decided to depend on it, as ASF doesn't include it itself. Bundling libraries that are shared with ASF can still make sense if you want to ensure API compatibility (e.g. being sure that `AngleSharp` which you depend on in your plugin will always be in version `X` and not the one that ASF ships with), but obviously doing that comes for a price of increased memory/size and worse performance, and therefore should be carefully evaluated.

If you know that the dependency which you need is included in ASF, you can mark it with `IncludeAssets="compile"` as we showed you in the example `csproj` above. This will tell the compiler to avoid publishing referenced library itself, as ASF already includes that one. Likewise, notice that we reference the ASF project with `ExcludeAssets="all" Private="false"` which works in a very similar way - telling the compiler to not produce any ASF files (as the user already has them). This applies only when referencing ASF project, since if you reference a `dll` library, then you're not producing ASF files as part of your plugin.

If you're confused about above statement and you don't know better, check which `dll` libraries are included in `ASF-generic.zip` package and ensure that your plugin includes only those that are not part of it yet. This will be only `YourPluginName.dll` for the most simple plugins. If you get any issues during runtime in regards to some libraries, include those affected libraries as well. If all else fails, you can always decide to bundle everything.

---

### Native dependencies

Native dependencies are generated as part of OS-specific builds, as there is no .NET runtime available on the host and ASF is running through its own .NET runtime that is bundled as part of OS-specific build. In order to minimize the build size, ASF trims its native dependencies to include only the code that can be possibly reached within the program, which effectively cuts the unused parts of the runtime. This can create a potential problem for you in regards to your plugin, if suddenly you find out yourself in a situation where your plugin depends on some .NET feature that isn't being used in ASF, and therefore OS-specific builds can't execute it properly, usually throwing `System.MissingMethodException` or `System.Reflection.ReflectionTypeLoadException` in the process. As your plugin grows in size and becomes more and more complex, sooner or later you'll hit the surface that is not covered by our OS-specific build.

This is never a problem with generic builds, because those are never dealing with native dependencies in the first place (as they have complete working runtime on the host, executing ASF). This is why it's a recommended practice to **use your plugin in generic builds exclusively**, but obviously that has its own downside of cutting your plugin from users that are running OS-specific builds of ASF. If you're wondering if your issue is related to native dependencies, you can also use this method for verification, load your plugin in ASF generic build and see if it works. If it does, you have plugin dependencies covered, and it's the native dependencies causing issues.

Unfortunately, we had to make a hard choice between publishing whole runtime as part of our OS-specific builds, and deciding to cut it out of unused features, making the build over 80 MB smaller compared to the full one. We've picked the second option, and it's unfortunately impossible for you to include the missing runtime features together with your plugin. If your project requires access to runtime features that are left out, you have to include full .NET runtime that you depend on, and that means running your plugin in `generic` ASF flavour. You can't run your plugin in OS-specific builds, as those builds are simply missing a runtime feature that you need, and .NET runtime as of now is unable to "merge" native dependency that you could've provided with our own. Perhaps it'll improve one day in the future, but as of now it's simply not possible.

ASF's OS-specific builds include the bare minimum of additional functionality which is required to run our official plugins. Apart of that being possible, this also slightly extends the surface to extra dependencies required for the most basic plugins. Therefore not all plugins will need to worry about native dependencies to begin with - only those that go beyond what ASF and our official plugins directly need. This is done as an extra, since if we need to include additional native dependencies ourselves for our own use cases anyway, we can as well ship them directly with ASF, making them available, and therefore easier to cover, also for you. Unfortunately, this is not always enough, and as your plugin gets bigger and more complex, the likelihood of running into trimmed functionality increases. Therefore, we usually recommend you to run your custom plugins in `generic` ASF flavour exclusively. You can still manually verify that OS-specific build of ASF has everything that your plugin requires for its functionality - but since that changes on your updates as well as ours, it might be tricky to maintain.

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
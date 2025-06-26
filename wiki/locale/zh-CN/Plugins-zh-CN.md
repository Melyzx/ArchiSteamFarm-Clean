# 插件

ASF 支持在运行时加载自定义插件。 插件允许您自定义 ASF 行为，例如添加自定义命令、自定义交易逻辑或者与其他第三方服务与 API 进行整体集成。

此页面从用户视角描述 ASF 插件——解释、用法等等。 如果您要查看开发者视角，请前往[**这里**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-development-zh-CN)。

---

## 用法

ASF 会从 ASF 目录内的 `plugins` 文件夹加载插件。 建议（如果启用插件自动更新，则是强制要求）您根据插件名称，将每个您需要使用的插件放在专门的文件夹内，例如 `MyPlugin`。 这样最终的文件夹结构为 `plugins/MyPlugin`。 最后，这个插件的所有二进制文件都应该被放在这个专门的文件夹内，而 ASF 会在重启之后自动发现并启用您的插件。

通常，插件开发者会以 `zip` 文件形式发布插件，其中包含二进制文件，这意味着您需要将 zip 文件解压到 `plugins` 文件夹下的某个单独的子文件夹。

如果插件成功加载，您将会在日志内看到它的名称和版本。 在遇到与您使用的插件有关的问题、漏洞或者对其用法有疑问时，您应该咨询相应插件的开发人员。

我们在&#8203;**[第三方项目](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party-zh-CN#asf-插件)**&#8203;章节列出了一些精选插件。

**请注意，ASF 插件可能是有害的**。 您应该始终确保您使用插件来自于您可以信任的开发人员，即使是来自上方第三方项目章节的插件。 如果您决定使用任何自定义插件，ASF 开发者将不再保证您的正常 ASF 权益（例如不保证没有恶意软件或者 VAC 安全）。 您应当理解，一旦被加载，插件就可以完全控制 ASF 进程，因此我们亦无法对使用自定义插件的 ASF 提供支持，因为您运行的不再是标准 ASF 代码。

---

## 兼容性

取决于插件的复杂度、作用范围和许多其他因素，有时您必须使用 ASF 的 **[Generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up-zh-CN#安装-generic-包)** 包，而不是通常推荐的[**操作系统包**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up-zh-CN#安装操作系统包)。 这是因为操作系统包仅自带 ASF 自身所需的核心功能，而插件则可能需要 ASF 功能之外的组件，这会导致插件与经过剪裁的操作系统包不兼容。

一般来说，为了兼容性，当使用第三方插件时，我们建议使用 ASF Generic 版本。 但并非所有插件都有这样的需求——请参考插件提供的说明，以确定您是否需要使用 ASF Generic 版本。
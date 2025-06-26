# 外掛程式開發

ASF支援可在執行期間載入的自訂外掛程式。 外掛程式使您能夠自訂ASF的行為，例如加入自訂指令、自訂交易邏輯，或與第三方服務或是API進行整體整合。

本頁描述了開發人員視角的ASF外掛程式⸺建立、維護、發布等其他資訊。 若您想從使用者的角度來看，請前往[**這裡**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-zh-TW)。

---

## 核心

外掛程式是標準.NET函式庫，定義了一個由ASF宣告的通用`IPlugin`介面中繼承的類別。 只要ASF的內部API保持相容，您就可以完全獨立於主線ASF來開發外掛程式，並可以在現在及未來的ASF版本中重複使用它們。 ASF使用的外掛程式系統基於`System.Composition`，前稱[**Managed Extensibility Framework**](https://learn.microsoft.com/zh-tw/dotnet/framework/mef)，可以使ASF在執行期間偵測並載入您的程式庫。

---

### 開始使用

我們為您準備了[**ASF外掛程式模板**](https://github.com/JustArchiNET/ASF-PluginTemplate)，您可以（也應該）依此作為外掛程式專案的基礎。 使用模板並**非**強制性（因為您可以從頭開始建立），但我們強烈建議使用，因為它能夠極大加速您的開發過程，節省各種事情所需的時間。 查看模板的[**讀我檔案**](https://github.com/JustArchiNET/ASF-PluginTemplate/blob/main/README.md)，它會給您更進一步的指南。 不論如何，如果您想從零開始，或更好地理解外掛程式模板中的概念，我們也會在下面介紹相關基礎⸺但如果您想要使用我們的外掛程式模板，一般而言，就不需要執行這些步驟。

您的專案應該是一個標準.NET程式庫，並對應目標ASF版本所使用的框架版本，如[**編譯**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compilation-zh-TW)章節所述。

專案必須引用`ArchiSteamFarm`主程式集，或您先前下載的發布中所包含的預組建`ArchiSteamFarm.dll`程式庫，或原始專案（例如您決定將ASF Tree作為子模組）。 這將使您可以存取與檢查ASF的結構、方法及屬性，特別是您接下來將需要繼承的`IPlugin`核心介面。 專案也必須至少引用`System.Composition.AttributedModel`，使您能夠`[Export]`您的`IPlugin`給ASF使用。 除此之外，您可能還希望／需要引用其他公開程式庫，來解譯在某些介面中提供給您的資料結構，但除非您確實需要它們，否則現在這樣就夠了。

若您所做一切正確，您的`csproj`應類似於下面：

```csproj
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net9.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <!-- 您正在載入外掛程式至 ASF 程序中，由於其已包含了相依套件，請 IncludeAssets="compile" 來允許在輸出中略過它 -->
    <PackageReference Include="System.Composition.AttributedModel" IncludeAssets="compile" Version="9.0.0" />
  </ItemGroup>

  <ItemGroup>
    <ProjectReference Include="C:\\Path\To\ArchiSteamFarm\ArchiSteamFarm.csproj" ExcludeAssets="all" Private="false" />

    <!-- 若使用下載的 DLL 二進制檔案進行組建，請使用上述的 <ProjectReference> -->
    <!-- <Reference Include="ArchiSteamFarm" HintPath="C:\\Path\To\Downloaded\ArchiSteamFarm.dll" /> -->
  </ItemGroup>
</Project>
```

從程式碼方面來說，您的外掛程式類別必須繼承自`IPlugin`介面（顯式，或透過例如`IASF`等更專門的介面來進行隱式繼承）與`[Export(typeof(IPlugin))]`，使ASF能在執行期間進行辨識。 達成這一點最簡單的範例如下：

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

為了使用您的外掛程式，您必須先編譯它。 您可以使用您的IDE，或在您的專案的根目錄下執行此命令來編譯：

```shell
# 若您的專案是獨立的（不需要定義它的名稱，因為它是唯一的）
dotnet publish -c "Release" -o "out"

# 若您的專案屬於ASF的Source Tree的一部份（用以防止編譯不需要的部分）
dotnet publish 您外掛程式的名稱 -c "Release" -o "out"
```

在這之後，您的外掛程式就已準備好部署。 如何轉發及發布外掛程式完全取決於您自己，但我們建議建立一個.zip壓縮檔，在裡面放入您已編譯好的外掛程式及它的[**相依程式**](#外掛程式相依性)。 這樣，使用者只需要將您的.zip解壓縮至`plugins`資料夾內的獨立子資料夾中即可，而不需要進行其他動作。

這只是讓您入門的最基礎情境。 我們提供了[**`ExamplePlugin`**](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.CustomPlugins.ExamplePlugin)專案，向您展示能在您自己的外掛程式中可實作的介面及功能，並在其中撰寫了對您有幫助的註解。 若您想從工作碼中學習，或自行探索`ArchiSteamFarm.Plugins`命名空間，可以隨意閱覽並參考所包含的文件，以了解所有可用的選項。 我們也會在接下來解釋一些核心觀念，使您能夠更進一步地了解它們。

若您更希望從實際的專案中，而不是範例外掛程式中學習，我們還開發了幾個官方外掛程式，例如[**`ItemsMatcher`**](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.ItemsMatcher)、[**`MobileAuthenticator`**](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.MobileAuthenticator)、[**`SteamTokenDumper`**](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.SteamTokenDumper)。 如此之外，也有一些由其他開發人員所撰寫的外掛程式，在我們的[**第三方工具**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party-zh-TW#asf-外掛程式)章節中。

---

### API 可用性

除了您可以在介面中存取的內容之外，ASF還向您公開了許多您可以使用的內部API，以便擴展功能。 舉例來說，若您想向Steam網站傳送某種新請求，您不需從頭開始實作所有內容，特別是處理我們先前已處理過的所有問題。 只需使用我們的`Bot.ArchiWebHandler`，它已經公開了許多`UrlWithSession()`方法以供您使用，並已為您完成了所有的底層工作，例如身分驗證、連線階段重新整理或處理網路限制等。 同樣地，若要向Steam平台以外傳送Web請求，您可以使用標準的.NET `HttpClient`類別，但最好還是使用我們提供給您的`Bot.ArchiWebHandler.WebBrowser`，它能為您提供許多幫助，例如重試失敗的請求。

我們在API可用性的方面採取非常開放的政策，所以若您想使用ASF程式碼中已有的功能，請[**提出一個Issue**](https://github.com/JustArchiNET/ArchiSteamFarm/issues)，在裡面說明您需要使用的ASF內部API，並解釋您計劃使用的範例情境。 只要您的範例情境有意義，我們不太可能會反對。 這也包含關於新的`IPlugin`介面的所有建議，只要用來擴充現有功能時加入它是有意義的。

不論ASF API是否可用，但因您的外掛程式也可以擁有自己的相依程式，所以沒有什麼能夠阻止您。例如您可以在您的應用程式中加入`Discord.Net`程式庫，並在您的Discord Bot及ASF指令間搭起一座橋梁。 能力是無限的，我們會盡量為您的外掛程式提供自由度及靈活性，因此任何行為都沒有人為限制⸺您的外掛程式會被載入至ASF主程序中，可以執行任何C#程式碼能做到的事情。

---

### API 相容性

需要特別為您強調，ASF是一個使用者應用程式，而非一個您能無條件依賴、具有穩定API介面的程式庫。 這代表您無法假定您的外掛程式一經編譯，就能夠在未來所有的ASF版本中持續運作。若我們想進一步開發程式，這將是不可能的，我們無法只為了反向相容性，就放棄去適應不斷變化的Steam。 對您來說這應該合乎邏輯，但強調這一點事實很重要。

我們會盡最大努力，保持ASF公開的部分能夠正常且穩定運作。但如果有足夠的理由，我們不會害怕去破壞相容性，並且在這個過程中，會遵循我們的[**棄用**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Deprecation-zh-TW)政策。 這對於作為ASF基礎架構的一部份公開給您的內部ASF結構來說特別重要（例如`ArchiWebHandler`），在未來某個版本中，它們可能會作為ASF增強的一部份而被改進（或因此而被重寫）。 我們將會盡最大努力在更新日誌中適當通知您，並在執行期間適時顯示與過時功能相關的警告。 我們不會故意為了重寫而重寫，因此您可以相信，下一個ASF次版更新不會只因為版本號碼增加，而讓您的外掛程式完全失效。但仍最好留意更新日誌，並偶爾驗證一切是否正常運作。

---

### 外掛程式相依性

預設情形下，您的外掛程式會至少含有兩個相依套件，`ArchiSteamFarm`用於內部API，以及`System.Composition.AttributedModel`的`PackageReference`，這是被辨識成ASF外掛程式所必需的。 除此之外，依據您的外掛程式功能，它可能還需要更多的相依套件（例如若您決定與Discord整合，就需要`Discord.Net`程式庫）。

組建的輸出將包含您的`您的外掛程式名稱.dll`核心程式庫，及所有您引用的相依套件。 因為您是為已在運作的程式開發外掛程式，所以您不必，也**不應該**包含ASF已含有的相依套件，例如`ArchiSteamFarm`、`SteamKit2`或`AngleSharp`。 精簡與ASF共用的相依套件並不是外掛程式運作的必要措施，但這樣能使記憶體使用及外掛程式大小顯著減少，同時使效能增加，因為ASF會與您共用它自己的相依套件，並只載入那些它所不知道的程式庫。

在一般情形下，建議是只封裝ASF不包含的，或與ASF包含版本不同／不相容的程式庫。 明顯的範例有`您的外掛程式名稱.dll`；另一個範例是`Discord.Net.dll`，若您決定整合Discord，因ASF並不含有它，故您也需要將它封裝在內。 若您想確保API相容性，那麼與ASF共用的附隨程式庫依然有意義（例如確保您外掛程式中相依的`AngleSharp`永遠處於`X`版本，而不是ASF所搭載的版本）。但顯然這樣做的代價是增加了記憶體／檔案大小，並使效能更差，因此應該要仔細評估。

若您已知您所需的相依套件已經包含在ASF中，您可以使用`IncludeAssets="compile"`標示它，就如上述範例`csproj`所示。 這將會告訴編譯器不發布被引用的程式庫，因為ASF已經將它包含在內。 同樣地，請注意我們使用了`ExcludeAssets="all" Private="false"`來引用ASF專案，其運作原理非常相似⸺告訴編譯器不要產生任何ASF檔案（因為使用者已經擁有它們了）。 這只在引用ASF專案時適用，因為如果您是引用`dll`程式庫，您不會將ASF檔案作為您外掛程式的一部份而產生出來。

若您對上述陳述感到困惑，且您無法理解，請查閱`ASF-generic.zip`套件中含有哪些`dll`程式庫，並確保您的外掛程式只包含那些尚未被包含在內的部分。 對於最簡單的外掛程式來說，這唯一一個會是`您的外掛程式名稱.dll`。 若您在執行期間遇到有關某些程式庫的問題，請將那些受影響的程式庫也包含在內。 若一切嘗試都失敗了，您仍可以選擇附隨所有東西。

---

### 本機相依性

原生相依套件是作為特定作業系統組建的一部份生成的，因為主機上沒有可用的.NET執行環境，ASF就需要經由自己的.NET執行環境來執行，該環境附隨於特定作業系統組建的一部份。 為了最小化組建的大小，ASF會修整自己的原生相依性，只包含程式中可能用到的程式碼，這樣就會有效減少執行期間未使用到的部分。 這可能會給您的外掛程式帶來問題，若您突然發現您的外掛程式相依於ASF中未使用的某些.NET功能，特定作業系統的組建會無法正確執行它，程序通常會擲回`System.MissingMethodException`或`System.Reflection.ReflectionTypeLoadException`。 當您的外掛程式愈趨於龐大且複雜，您遲早會遇到我們適用於特定作業系統的組建版本所無法覆蓋到的範圍。

這對於Generic組建版本而言從來都不是問題，因為它們一開始就不會處理原生相依性（因為它們在主機上擁有完整的執行環境來執行ASF）。 這也是為什麼要**將您的外掛程式只供Generic組建版本使用**，但顯然這具有根本性的缺點，特定作業系統組建的ASF使用者就無法使用您的外掛程式。 若您想知道您遇到的問題是否與原生相依性有關，您也可以使用這個方法來驗證，在ASF Generic組建版本中載入您的外掛程式，並看它是否能夠運作。 如果可以，則說明您的外掛程式已有相依套件，而問題在於原生相依性。

不幸的是，我們不得不做出一個艱難的決定：是將整個執行環境發布成適用於特定作業系統建置的一部分，或是決定刪除其未使用的功能，使得建置檔案比完整版的建置小80 MB以上。 我們選擇了後者，所以您無法讓您的外掛程式直接使用缺少的執行環境功能。 若您的專案需要存取被省略的執行環境功能，您就必須包含要相依的完整.NET執行環境，這代表您的外掛程式需要與`Generic` ASF版本一起執行。 您無法在適用於特定作業系統的組建中執行您的外掛程式，因為那些組建版本缺少您所需的執行環境功能，且.NET執行環境到目前為止還無法支援「合併」您自行提供的原生相依套件。 或許在未來的某一天會有此功能，但目前而言還無法做到。

ASF的適用於特定作業系統的組建已含有執行我們官方的外掛程式最基礎的附加功能。 除此之外，這也稍微擴充了大多數基礎外掛程式可以使用的額外相依性。 因此，不是所有外掛程式都需要擔心原生相依性⸺只有那些超出ASF與官方外掛程式的外掛程式才需要去擔憂。 這是額外提供的，因為如果我們不論如何都需要為自己的範例加入額外的本機相依性，我們也可以直接將它們搭載到ASF上，使您能夠更輕易地使用它們。 但很遺憾，這並不總是足夠的。隨著您的外掛程式越來越大也越來越複雜，用到被修整的功能的可能性就會增加。 因此，我們通常建議您只在`Generic` ASF版本中執行您的自訂外掛程式。 您仍然可以手動驗證適用於特定作業系統的ASF組建版本是否具有您的外掛程式功能所需的一切⸺但因為您的外掛程式及我們的ASF都會不斷更新而造成變化，因此維護起來可能會相當困難。

有時，透過使用替代選項或在外掛程式中重新實作，或許是獲得缺失功能的變通方法。 但是，這並不總能達成或可行，特別是當缺失的功能來自於您外掛程式所包含的第三方相依項時。 您總能嘗試在適用於特定作業系統的組建中執行您的外掛程式，並試著讓它運作，但長期來看，這樣可能會造成更多麻煩，特別是您想讓程式碼正常運作，而非與ASF對抗時。

---

## 自動更新

ASF提供您兩種介面，可在您的外掛程式實作自動更新：

- `IGitHubPluginUpdates`提供您基於GitHub的簡易實作方法，與我們ASF的更新機制相似
- `IPluginUpdates`提供您底層功能，使您能夠在更複雜的情形下自訂更新機制

---

### [**`IGithubPluginUpdates`**](https://github.com/JustArchiNET/ArchiSteamFarm/blob/main/ArchiSteamFarm/Plugins/Interfaces/IGitHubPluginUpdates.cs)

使更新能夠正常運作的檢查清單如下：

- 您需要宣告`RepositoryName`，定義從哪拉取更新。
- 您需要使用GitHub提供的標籤及版本功能。 標籤必須是可被剖析的外掛程式版本，例如`1.0.0.0`。
- 外掛程式的`Version`屬性必須與來源標籤相符。 這代表`1.2.3.4`標籤下的二進制檔案必須將自身標示成`1.2.3.4`。
- 每個標籤在GitHub上皆擁有對應的版本，且.zip資源檔案中有您外掛程式的二進制檔案。 含有`IPlugin`類別的二進制檔案，應放置於.zip檔的根資料夾中。

這會使ASF的機制能夠：

- 解析您外掛程式的當前`Version`，例如`1.0.1`。
- 使用GitHub API提取`RepositoryName`儲存庫中可以使用的最新`tag`，例如`1.0.2`。
- 判斷`1.0.2` > `1.0.1`，檢查帶有`1.0.2`標籤的版本，並找到擁有外掛程式更新的`.zip`檔。
- 下載該`.zip`檔，解壓縮，並將內容放到先前含有`YourPlugin.dll`的資料夾中。
- 重新啟動ASF，以載入您的`1.0.2`版本外掛程式。

補充說明：

- 外掛程式更新可能需要適當的ASF設定值，即[**`PluginsUpdateMode`**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#pluginsupdatemode外掛程式更新模式)和／或[**`PluginsUpdateList`**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#pluginsupdatelist外掛程式更新清單)。
- 我們的外掛程式模板已包含了您需要的一切，只需將外掛程式[**重新命名**](https://github.com/JustArchiNET/ASF-PluginTemplate?tab=readme-ov-file#renaming-myawesomeplugin)成適當的值就能立即使用。
- 您可以適切地使用最新版本及預覽版本，它們會依使用者設定的`UpdateChannel`被選擇。
- 您能夠覆寫`CanUpdate`布林屬性來停用／啟用外掛程式更新，例如當您想要暫時跳過更新，或其他任何原因時。
- 若您想要針對ASF的不同版本，在每個版本中能夠含有多個.zip檔。 查閱`GetTargetReleaseAsset()`[**說明**](https://github.com/JustArchiNET/ArchiSteamFarm/blob/main/ArchiSteamFarm/Plugins/Interfaces/IGitHubPluginUpdates.cs#L79-L106)。 舉例來說，您能夠同時提供`MyPlugin-V6.0.zip`及`MyPlugin.zip`，這能使`V6.0.x.x`版本的ASF選擇前者，而其餘版本的ASF選擇後者。
- 若上述方式還無法滿足您的需求，且您需要自訂邏輯以選擇正確的`.zip`檔，您能夠覆寫`GetTargetReleaseAsset()`函數並提供所需的檔案給ASF使用。
- 若您的外掛程式需要在更新前／後做一些額外的行為，您可以覆寫`OnPluginUpdateProceeding()`及／或`OnPluginUpdateFinished()`方法。

---

### [**`IPluginUpdates`**](https://github.com/JustArchiNET/ArchiSteamFarm/blob/main/ArchiSteamFarm/Plugins/Interfaces/IPluginUpdates.cs)

若出於任何原因`IGithubPluginUpdates`無法滿足您的需求，這個介面使您能夠實作自訂更新邏輯，例如您擁有無法被剖析成版本的標籤，或您根本不是在使用GitHub平台時。

`GetTargetReleaseURL()`函數能夠讓您覆寫，您需要提供目標外掛程式更新位置的`Uri?`。 ASF提供您一些呼叫函數上下文相關的核心變數，但除此之外，您需要自行實作該函數的所有功能，並提供ASF須使用的URL，或在無法使用更新時設定成`null`。

此外，與GitHub更新相似，URL應指向一個在根資料夾中含有二進制檔案的`.zip`檔， 您同樣可以使用`OnPluginUpdateProceeding()`及`OnPluginUpdateFinished()`方法。
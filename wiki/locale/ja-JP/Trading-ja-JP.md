# 取引

ASFは、Steamの非対話型（オフライン）取引をサポートしています。 受信（承認/拒否）および送信の両方の取引がすぐに利用可能で、特別な設定は不要ですが、当然ながら制限のないSteamアカウント（すでにストアで5ドル以上消費しているアカウント）が必要です。 制限付きアカウントでは、利用できる取引機能が一部に限られます。

---

## ロジック

ASFは、`Master`（またはそれ以上）のアクセス権を持つユーザーから送信された取引は、アイテムの内容に関わらず常に承認します。 これにより、ボットインスタンスで集めたSteamカードを簡単に回収できるだけでなく、ボットがインベントリに保管しているSteamアイテム（CS:GOなど他のゲームのアイテムも含む）を簡単に管理できます。

ASFは、取引モジュールからブラックリストに登録された（マスター以外の）ユーザーからの取引オファーは、内容に関わらず拒否します。 ブラックリストは標準の`BotName.db`データベースに保存され、`tb`、`tbadd`、`tbrm`**[コマンド](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**で管理できます。 これはSteamが提供する標準のユーザーブロックの代替として機能しますが、慎重に使用してください。

ASFは、`TradingPreferences`で`DontAcceptBotTrades`が指定されていない限り、ボット間で送信されるすべての`loot`系の取引も承認します。 つまり、デフォルトの`TradingPreferences`である`None`では、ASFはボットへの`Master`アクセス権を持つユーザーからの取引（上記で説明）と、ASFプロセスに参加している他のボットからのすべての寄付取引を自動的に承認します。

First of all, it's possible to disable **all** incoming trade offers, by using `DisableIncomingTradesParsing` flag in `BotBehaviour`. Using that, as the name implies, will disable all functionality related to incoming trades parsing, which includes above logic, as well as all extra features available below which depend on reacting to the incoming trade offer. Since default settings are already non-intrusive, you should consider using that option only if you have absolutely no intent from ASF to do anything related to the incoming trades at all.

It's possible to disable donation trades from other bots, through `DontAcceptBotTrades` in your `TradingPreferences`.

`TradingPreferences`で`AcceptDonations`を有効にすると、ASFは寄付取引（ボットアカウントがアイテムを失わない取引）も承諾します。 このプロパティは非ボットアカウントにのみ影響し、ボットアカウントは`DontAcceptBotTrades`の影響を受けます。 `AcceptDonations`を使用すると、他の人やASFプロセスに参加していないボットからの寄付を簡単に承諾できます。

`AcceptDonations`は**[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**を必要としないことに注意してください。アイテムを失わない場合は確認が必要ないためです。

`TradingPreferences`を適切に変更することで、ASFの取引機能をさらにカスタマイズすることもできます。 主要な`TradingPreferences`機能の1つは`SteamTradeMatcher`オプションで、これによりASFは欠けているバッジの完成に役立つ取引を承諾するための組み込みロジックを使用します。これは**[SteamTradeMatcher](https://www.steamtradematcher.com)**の公開リストとの連携で特に有用ですが、それなしでも機能します。 詳細は以下で説明されています。

---

## `SteamTradeMatcher`

`SteamTradeMatcher`がアクティブな場合、ASFは取引がSTMルールを通過し、少なくとも私たちにとって中立であるかどうかをチェックする非常に複雑なアルゴリズムを使用します。 実際のロジックは以下の通りです：

- `MatchableTypes`で指定されたアイテムタイプ以外のものを失う場合は取引を拒否する。
- ゲーム、タイプ、レアリティごとに少なくとも同じ数のアイテムを受け取らない場合は取引を拒否する。
- ユーザーが特別なSteam夏/冬セールカードを要求し、取引保留がある場合は取引を拒否する。
- 取引保留期間が`MaxTradeHoldDuration`グローバル設定プロパティを超える場合は取引を拒否する。
- `MatchEverything`が設定されておらず、私たちにとって中立より悪い場合は取引を拒否する。
- 上記のポイントのいずれかで拒否されなかった場合は取引を承諾する。

ASFはオーバーペイもサポートしていることに注意してください - 上記のすべての条件が満たされている限り、ユーザーが取引に何か追加のものを加えている場合でも、ロジックは適切に機能します。

最初の4つの拒否述語は誰にとっても明らかなはずです。 最後のものは、私たちのインベントリの現在の状態をチェックし、取引のステータスが何であるかを決定する実際の重複ロジックを含んでいます。

- セットの完成に向けた進捗が進む場合、トレードは**良好 (good)**です。 例：A A（前）->A B（後）
- セットの完成に向けた進捗が維持される場合、トレードは**中立 (neutral)**です。 例：A B（前） -> A C（後）
- セットの完成に向けた進捗が悪化する場合、トレードは**不良 (bad)**です。 例：A C（前） -> A A（後）

STMは良好 (good)トレードのみを対象としています。つまり、STMを使って重複カードの交換を行うユーザーは、常に自分にとって良好 (good)トレードのみを提案する必要があります。 しかし、ASFはリベラルな設計で、中立 (neutral)トレードも承認します。なぜなら、その場合は実際に何も失っていないため、拒否する理由がないからです。 これは、友人同士であればSTMを使わずとも余剰カードを交換できるため便利です（セット進捗を失わない限り）。

デフォルトでは、ASFは不良 (bad)トレードを拒否します。これは通常、ユーザーにとって望ましい動作です。 ただし、`TradingPreferences`で`MatchEverything`を有効にすると、ASFは**不良 (bad)**トレードも含めてすべての重複カードトレードを承認します。 これは、1:1トレードボットを運用したい場合のみ有用です。この場合、**ASFはバッジ完成をサポートしなくなり、完成済みセットを同じカードN枚と交換して失うリスクがあります**。 意図的に「**絶対に**セットを完成させず、全インベントリをすべてのユーザーに提供する」トレードボットを運用したい場合のみ、このオプションを有効にしてください。

どの`TradingPreferences`を選択しても、ASFによってトレードが拒否された場合でも、自分で手動で承認することは可能です。 `BotBehaviour`のデフォルト値（`RejectInvalidTrades`を含まない）を維持していれば、ASFはこれらのトレードを無視するだけで、興味があれば自分で判断できます。 `MatchableTypes`以外のアイテムを含むトレードやその他すべてについても同様です。このモジュールはSTMトレードの自動化を支援するものであり、「良いトレード／悪いトレード」を決定するものではありません。 唯一の例外は、`tbadd`コマンドでトレードモジュールのブラックリストに登録したユーザーからのトレードで、これらは`BotBehaviour`設定に関わらず即座に拒否されます。

SteamTradeMatcherオプションを有効にする場合は、**[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**の利用を強く推奨します。この機能は、すべてのトレードを手動で承認する場合、その利便性が大きく損なわれるためです。 `SteamTradeMatcher`は、トレードの承認機能がなくても動作しますが、承認が遅れると確認待ちが溜まる可能性があります。
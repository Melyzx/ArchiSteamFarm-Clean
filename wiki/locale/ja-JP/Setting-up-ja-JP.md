# セットアップ

はじめての方、ようこそ！ 私たちのプロジェクトに興味を持っていただき感謝します。**ASFの使用方法をしっかり学べば**、Steam関連の様々な自動化タスクを実行できます。 ただし、学習曲線が急なため、このWikiを慎重に読むことを強く推奨します。

あなたがまだここにいるということは、上記のテキストに耐えてくれたということですね。嬉しい。 これをサボっていたら、すぐに&#8203;**[嫌な思い](https://youtu.be/LCZUMw00XHU?t=1)**&#8203;をすることになりますよ… ともあれ、ASFはコンソールアプリであり、一般的に慣れているような親しみやすいGUIは（少なくとも最初からは）ありません。 ASF は主にサーバ上で実行されることを想定していたので、デスクトップアプリではなくサービス（デーモン）として動作します。

しかし、これはあなたの PC で使用できなかったり、使用することが通常よりも複雑だったりすることを意味するものではありません。 ASF はインストールを必要としないスタンドアロンのプログラムで、OOTB（箱から出してすぐに動作します）が、使えるようにするには設定が必要です。 設定とは、ASF を起動した後に何をさせるかを指示することです。 設定なしで起動しても 、ASF は何もしません。<br><br> 訳注（用語説明）：<br> ・.NET Core の要件・依存関係 (.NET Core prerequisites)、.NET Coreアプリが作動する環境。<br> ・OS固有バリアント・OS固有パッケージ(OS-specific variant/OS-specific package)、コードをOSが実行できるよう予め作られたやつ、ユーザーがダウンロードするやつ。<br> （もっといい訳し方を知っている方はぜひ拙訳を直していただきたい。）<br>

---

## OS 固有の設定

一般的には、次のようにします。
- **[.NET prerequisites](#net-prerequisites)** をインストール。
- **[最新の ASF リリース](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**&#8203;を適切な OS 固有バリアントでダウンロードします。
- アーカイブを新しい場所に解凍。
- **[ASF を設定します](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-ja-JP)**
- ASF を起動し、奇跡の瞬間を目に焼き付けなさい。

簡単っしょ？ では、早速やってみましょう。

---

### .NET prerequisites

まずは、お使いの OS で ASF が正しく起動できるかを確認して下さい。 ASFはC#で書かれており、.NETプラットフォーム上で動作し、まだあなたの環境に存在しないネイティブライブラリを必要とすることがあります。 Windows、Linux、macOSのいずれを使っていても、必要なものは**[.NET prerequisites](https://docs.microsoft.com/dotnet/core/install)** ドキュメントに記載されています。 これはマイクロソフトによる公式的な説明ですが、簡単にするために、必要なものを以下にも書きますので、リンク先の全文を読まなくて結構です。

ASF が依存するソフトが、他のソフトによってすでに一部または全てインストールされていることが普通です。 それでも、ASFが確実に作動するために、依存関係を再度インストールすることが必要とされることもあります。

OS固有ビルドの場合、.NET SDKやランタイムのインストールなど、他に何もする必要がないことを覚えておいてください。OS固有パッケージにはそれらがすべて含まれているからです。 ASFに含まれる.NETランタイムを実行するために必要なのは、.NET prerequisites（依存関係）のみです。

#### **[Windows](https://docs.microsoft.com/dotnet/core/install/windows)**
- **[Microsoft Visual C++ Redistributable Update](https://docs.microsoft.com/cpp/windows/latest-supported-vc-redist?view=msvc-170#visual-studio-2015-2017-2019-and-2022)** (**[x64](https://aka.ms/vs/17/release/vc_redist.x64.exe)** for 64-bit, **[x86](https://aka.ms/vs/17/release/vc_redist.x86.exe)** for 32-bit or **[arm64](https://aka.ms/vs/17/release/vc_redist.arm64.exe)** for 64-bit ARM)
- すべてのWindowsアップデートがすでにインストールされていることを確認することを強くお勧めします。 有効にしていない場合は、少なくとも**[KB2533623](https://support.microsoft.com/en-us/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)**と**[KB2999226](https://support.microsoft.com/en-us/help/2999226/update-for-universal-c-runtime-in-windows)**が必要ですが、さらに多くのアップデートが必要な場合があります。 Windowsが最新の状態であれば、これらをインストールする必要はありません。

#### **[Linux](https://docs.microsoft.com/dotnet/core/install/linux)**
パッケージ名は使用している Linux ディストリビューションに依存しますが、ここでは最も一般的なものをリストアップしました。 あなたの OS のネイティブのパッケージマネージャ（例えば Debian なら `apt`、CentOS なら `yum`）を使っても、これらをインストールすることが可能です。

- `ca-certificates`（HTTPS接続を行うための標準的な信頼できるSSL証明書）
- `libc6` (`libc`)
- `libgcc-s1` (`libgcc1`, `libgcc`)
- `libicu`（`icu-libs`、ディストリビューション用の最新バージョン、例：`libicu72`）
- `libgssapi-krb5-2` (`libkrb5-3`, `krb5-libs`)
- `libssl3`（`libssl`、`openssl-libs`、ディストリビューション用の最新バージョン、少なくとも`1.1.X`）
- `libstdc++6`（`libstdc++`、バージョン`5.0`以上）
- `zlib1g` (`zlib`)

これらの大部分は、すでにシステム上でネイティブに利用可能であるはずです。 Debian stableの最小構成では、`libicu72`のみ必要でした。

#### **[macOS](https://docs.microsoft.com/dotnet/core/install/macos)**
- 現在のところなし、ただし最新バージョンのmacOS、少なくとも12.0+がインストールされている必要があります

---

### ダウンロード

前の節で必要な依存関係はすべて揃っているはずので、本節では&#8203;**[最新の ASF リリース](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**&#8203;をダウンロードします。 ASF には多くの種類がありますが、ご利用の OS やアーキテクチャに合うパッケージを選んで下さい。 例えば、`64` ビット `Win`dows を使用している場合、`ASF-win-x64` パッケージをダウンロードして下さい。 利用可能なバリアントの詳細については、&#8203;**[互換性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-ja-JP)**&#8203;セクションを参照してください。 ASFは **32ビットWindows** のような、我々がOS固有パッケージを提供していないOSでも動作することができます。詳しくは、&#8203;**[汎用設定](#generic-setup)**&#8203;を参照して下さい。

![Assets](https://i.imgur.com/Ym2xPE5.png)

ダウンロードしたら、まずは zip ファイルをお好きなフォルダに解凍して下さい。 特定のツールが必要な場合は、**[7-zip](https://www.7-zip.org)**が使えますが、Linux/macOSの`unzip`のような標準ユーティリティでも問題なく動作するはずです。

ASF を&#8203;**ほかに何も入っていないディレクトリ**&#8203;に解凍することをお勧めします。ASF の自動更新機能はアップグレード時に古いファイルや関係のないファイルをすべて削除するようになっているので、ASF ディレクトリにある関係のないファイルが削除される可能性があります。 ASFで使用するほかのスクリプトやファイルは、親ディレクトリに入れてください。

構造はこんな感じ。

```text
C:\ASF（ご自身のものを置く場所、例）
    ├── ASFのショートカット.lnk（例）
    ├── 設定のショートカット.lnk（例）
    ├── コマンドの下書き.txt（例）
    ├── 自分が書いたスクリプト.bat（例）
    ├── (...)（任意のほかのファイル）
    └── Core（ASF専用のディレクトリ、圧縮ファイルが解凍される場所）
         ├── ArchiSteamFarm(.exe）
         ├── config
         ├── logs
         ├── plugins
         └──(...)
```

---

### 設定

最後に設定を行います。 これが最も重要かつ複雑なステップですが、簡単な例と説明を交えて解説します。

まず、&#8203;**[設定](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-ja-JP)**&#8203;ページでこれに関する&#8203;**全て**&#8203;が書かれているが、あまりにも情報量が莫大で、今はまだ読まなくていい。 代わりに、実際に必要な情報を得る方法を教えよう。

設定方法は少なくとも3つあります。Web Config Generator（推奨）、ASF-ui（IPCフロントエンド）、手動編集です。 この部分も&#8203;**[設定](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-ja-JP)**&#8203;ページで詳しく書かれている、詳しく知りたい方はそちらを。 ここではWeb Config Generatorを使います。

ブラウザで**[Web Config Generator](https://justarchinet.github.io/ASF-WebConfigGenerator)**のページにアクセスして下さい。javascript を手動で無効化していたら有効にして下さい。 Chrome か Firefox を使うことをお勧めしますが、他のブラウザでも問題ないはず。

ページを開いたら、「Bot」タブに切り替えて下さい。 下のようなページが表示されているはずです。

![Bot tab](https://i.imgur.com/aF3k8Rg.png)

もしダウンロードした ASF のバージョンがWeb Config Generatorのデフォルトのバージョンよりも古い場合は、ドロップダウンメニューからお使いの ASF のバージョンを選択してください。 これは、Web Config Generatorは、まだ安定していないプレリリース版の ASF の設定も生成できるからである。 安定版 ASF は検証済みなので、そちらをダウンロードして下さい。

まずは赤くハイライトされた入力欄にボットの名前を入力する。 ニックネーム、ユーザー名、数字、何でもよくて、お好きなように名前を付けて下さい。 但し、`ASF` とだけは名付けてはいけません。 その他にも、ASFはこれを無視するので、ボット名を「`.`」（点）で始まる名前にしてはいけません。 また、スペースの使用も避けたほうがいいと思われます。スペースの代わりに `_` を使って下さい。

名前が決まったら、`Enabled` スイッチをオンにしてください。このスイッチは、ASF 起動後に自動的にこのボットを起動するかどうかを定義するものです。

ここで、下の 2 つの中のどれにするか決めて下さい。
- Steamのアカウント名とパスワードをそれぞれ `SteamLogin` と `SteamPassword` 欄に入力する。
- これらを空欄のままにする。

前者を選んだ場合、ASFは起動時に自動的にあなたのアカウント認証情報を使用し、毎回毎回手動で入力する必要はありません。 後者なら、アカウント情報は保存されませんので、ASFが起動するたびに手動でこれらを入力する手間が掛かります。

ASF には Steam クライアントの独自の実装が含まれているため、Steam 公式アプリと同じくログイン認証情報を必要とします。 ログイン認証情報はユーザーの PC にある ASF の下の `config` ディレクトリ内にのみ保存されます。Web Config Generatorはクライアント・ベースで、ブラウザ上でローカルにコードを実行します。ユーザーの認証情報はどこにも送られませんので、ユーザー情報が知られる心配はありません。 それでも、何らかの理由で認証情報をそこに書きたくない場合は（うん、わかる）、後で生成されたファイルに手動で書き入れるか、あるいは完全に省略して、起動するたんびに ASF コマンド・プロンプトに手動で入力することもできます。 セキュリティに関する詳細は**[設定&#8203;](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-ja-JP)**&#8203;ページを参照して下さい。

また、`SteamPassword` のように 1 つだけを空欄にするのも可能だ。その場合、ASF は自動的にログインしようとするが、Steam アプリと同様にパスワードを要求してきます。 ファミリービュー（保護者による制限）が有効化されたアカウントなら、`SteamParentalCode` 欄にそれに必要な コード を入力して下さい。

必要な操作をすべて完了させたら、Webページは以下のようになります。

![Bot tab 2](https://i.imgur.com/yf54Ouc.png)

「ダウンロード」ボタンを押すと、ボットの名前に基づいて新しい `json` ファイルが生成されます。 このファイルを前に解凍したフォルダにある `config` ディレクトリに保存して下さい。

`config` ディレクトリはこのようになります。

![Structure 2](https://i.imgur.com/crWdjcp.png)

ヨシッ！ これで ASF ボットの基本的な設定が完了しました。 このあとで詳しく説明するが、今のところこれで大丈夫です。

---

### ASF の実行

これでプログラムを初めて起動する準備ができました。 ASF ディレクトリ内の `ArchiSteamFarm` バイナリをダブルクリックして下さい。 コンソールから起動してもOKです。

最初のステップで必要な依存関係をすべてインストールしていたら、正常なら ASF は起動します。生成された設定を `config` ディレクトリに置くのを忘れていなければ、最初のボットを検出し、ASF はログインしようとします。

![ASF](https://i.imgur.com/u5hrSFz.png)

`SteamLogin` と `SteamPassword` を記入していたら、ASF は設定により Steamガード（メール、２ファクタ認証若しくはなし）だけを要求してくる。 それを記入していない場合、パスワードも要求してきます。

ASFがプログラムされていることが気になる場合は、 **[リモート通信](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication)** セクションを確認してください。 Steamグループに加入するなど、あなたの名前にも含まれています。

初回ログインに成功し、ボットがアカウントにログインすると、ASFはデフォルト設定でファーミングを開始します。

![ASF 2](https://i.imgur.com/Cb7DBl4.png)

つまり、ASF はあなたのアカウントを使って、正常に動作していることを意味します。プログラムを最小化にして他のことをしてもいいってわけね。 十分な時間（&#8203;**[パフォーマンス](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-ja-JP)**&#8203;による）が経過すると、Steam トレーディングカードがドロップしてきます。 Steamの**[バッジページ](https://steamcommunity.com/my/badges)**で「このゲームからあとX枚カードが入手可能」と表示されるゲームが必要です。該当ゲームがなければ、ASFは「やることがない」と表示します。詳しくは、 **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#what-is-asf)**を参照してください。

基本的な初期設定はここまでだ。 ASF の設定をさらにいじるのもよし、デフォルト設定のままでも使えます。 ここからはより高度的な設定になりますが、ASF を完全にマスターしたいなら、Wiki のほかのページもご覧下さい（まだ訳していないけど）。

---

### 拡張設定

#### 複数アカウントのファーミング

ASFは複数アカウントの同時ファーミングをサポートしています。これはASFの主機能のひとつです。 ボット設定ファイルを複数作成し、configフォルダに保存してください。 注意点は2つ：

- ボット名はすべて一意であること（例：「MainAccount」という名前のボットがすでにある場合、同じ名前は不可）。
- 有効なログイン情報（`SteamLogin`、`SteamPassword`、`SteamParentalCode`（必要な場合））を入力すること。

つまり、設定手順をもう一度繰り返すだけです。 ボットごとに異なる名前を付けるのを忘れないでください。

---

#### 設定の変更

既存の設定を変更する場合も、同じ手順です。新しい設定ファイルを生成し、古いファイルを上書きするだけです。 Web Config Generatorで「詳細設定を表示」してみてください。 例として`CustomGamePlayedWhileFarming`設定（ファーミング中に表示されるゲーム名をカスタムテキストに変更）を変更します。

デフォルト設定でASFを実行すると、Steamアカウントはゲーム中と表示されます。

![Steam](https://i.imgur.com/1VCDrGC.png)

変えてみましょう。 Toggle advanced settings in web config generator and find `CustomGamePlayedWhileFarming`. そうしたら、好きなテキスト（例："Idling cards"）を入力します。

![Bot tab 3](https://i.imgur.com/gHqdEqb.png)

新しい設定ファイルをダウンロードし、古いファイルを上書きしてください。ASFを再起動すると、カスタムテキストが表示されるようになります。 古い設定ファイルを削除し、新しい設定ファイルをその代わりに置いてもOKです。

ASFを再起動すると、カスタムテキストが表示されるようになります。

![Steam 2](https://i.imgur.com/vZg0G8P.png)

設定が正常に編集されたことが確認できました。 同じ方法で、グローバルASF設定も変更できます。「Bot」タブから「ASF」タブに切り替え、生成された`ASF.json`を`config`ディレクトリに保存してください。

ASFの設定ファイル編集は、後述するASF-uiフロントエンドを使うとさらに簡単に行えます。

---

#### ASF-uiの利用

ASFはコンソールアプリでGUIは標準搭載されていません。 ですが、**[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)**フロントエンドをIPCインターフェース経由で利用できます。これは非常に使いやすいGUIです。

利用には`IPC`が有効（デフォルト）である必要があります。 ASFを起動すると、自動的にIPCインターフェースが開始されます。

![プロセス間通信](https://i.imgur.com/ZmkO8pk.png)

**[こちら](http://localhost:1242)**からIPCインターフェースにアクセスできます。同一マシン上でのみ利用可能です。 ASF-uiでは設定ファイルの編集や**[コマンド](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**送信などが可能です。 色々試してみてください。

![ASF-ui](https://raw.githubusercontent.com/JustArchiNET/ASF-ui/main/.github/previews/bots.png)

---

### まとめ

Steamアカウントを使用するためのASFのセットアップに成功し、すでに少し自分好みにカスタマイズしたことでしょう。 私たちのガイドに全て従ったのであれば、ASF-uiインターフェイスを通してASFを調整し、ASFには実際にある種のGUIがあることを知ったでしょう。 今、あなたが見てきた設定が実際に何をするのか、ASFが何を提供するのかを学ぶために、私たちの全体の **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**セクションを読む良い機会です。 もし何か問題につまずいたり、一般的な質問がある場合は、 **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)**を読んでください。 ASFについて、そしてASFがどのようにあなたの生活を楽にするのか、すべてを学びたいのであれば、**[我々のwiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home)**の残りの部分をご覧ください。 私たちのプログラムがあなたにとって有用であることがわかり、そのために費やされた膨大な労力に感謝されるのであれば、**[寄付](https://github.com/JustArchiNET/ArchiSteamFarm?tab=readme-ov-file#archisteamfarm)**もご検討ください。 いずれにせよ、楽しんでください！

---

## 汎用設定

この設定は、**[generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#generic)**バリアントでASFをセットアップしたい上級者向けです。 **[OS固有バリアント](#os-specific-setup)**より手間はかかりますが、追加のメリットもあります。

`generic`バリアントを使う主なケース（もちろん他でも利用可能）：
- OS固有パッケージが用意されていないOS（例：32ビットWindows）を使っている
- すでに.NET Runtime/SDKがインストール済み、または自分で管理したい
- ASFの構造サイズやメモリ消費を最小限にしたい
- 特定の**[プラグイン](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)**が`generic`セットアップでのみ動作する

ただし、この場合.NETランタイムの管理はあなた自身の責任です。 .NET SDK（ランタイム）が未導入・古い・壊れているとASFは動作しません。 カジュアルユーザーには推奨しません。OS固有パッケージなら、開発側がバンドルした.NETランタイムで動作保証できます。

`generic`パッケージでは、上記OS固有ガイドとほぼ同じですが、2点だけ異なります。 .NET prerequisitesに加え.NET SDKもインストールし、OS固有の`ArchiSteamFarm(.exe)`ではなく`ArchiSteamFarm.dll`を使用します。 他は同じです。

追加手順：
- **[.NET prerequisites](#net-prerequisites)** をインストール
- **[.NET SDK](https://www.microsoft.com/net/download)**（またはASP.NET Core・.NETランタイム）をOSに合わせてインストール。 ほとんどの場合、インストーラを使用するのが好ましいです。 どのバージョンか不明な場合は**[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**参照。
- **[最新ASFリリース](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**を`generic`バリアントでダウンロード。
- アーカイブを新しい場所に展開
- **[ASF を設定します](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-ja-JP)**
- ヘルパースクリプトまたは`dotnet /path/to/ArchiSteamFarm.dll`コマンドでASFを起動

ヘルパースクリプト（`ArchiSteamFarm.cmd`（Windows）、`ArchiSteamFarm.sh`（Linux/macOS））は`ArchiSteamFarm.dll`の隣にあります（`generic`バリアントのみ）。 手動で `dotnet` コマンドを実行したくない場合に使用できます。 .NET SDKをインストールしておらず、`PATH`に`dotnet`の実行ファイルがない場合、ヘルパースクリプトは動作しません。 ヘルパースクリプトの使用は任意ですので、手動で`dotnet /path/to/ArchiSteamFarm.dll`でもOKです。
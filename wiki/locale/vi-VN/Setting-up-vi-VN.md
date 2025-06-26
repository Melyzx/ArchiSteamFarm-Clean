# Thiết lập

Nếu đây là lần đầu bạn tới đây, thì chào mừng bạn! Chúng tôi rất vui khi thấy thêm một du khách có hứng thú với dự án của chúng tôi, cơ mà hãy lưu ý rằng với sức mạnh lớn thì đi kèm đó là trách nhiệm lớn - ASF có thể làm rất nhiều thứ liên quan đến Steam, nhưng chỉ khi bạn **đủ sự quan tâm để học cách sử dụng nó**. Quá trình học cách dùng nó sẽ rất khó khăn, và chúng tôi mong bạn sẽ đọc wiki về vấn đề này, trong đó sẽ giải thích chi tiết cách mọi thứ hoạt động.

Nếu bạn vẫn còn ở đây tức nghĩa là bạn đã sẵn sàng để học rồi, điều đó thật tuyệt. Trừ khi bạn bỏ qua chuyện này, thì bạn sẽ có khoảng **[thời gian tệ hại](https://www.youtube.com/watch?v=WJgt6m6njVw)** sớm thôi... Dù sao thì ASF là một ứng dụng console, tức nghĩa là phần mềm này không có giao diện đồ họa người dùng thân thiện mà bạn thường đã quen dùng, ít nhất là khi ban đầu mới cài đặt thôi. ASF thường được dùng chính trong việc chạy trên các máy chủ, nên nó hoạt động như một dịch vụ (trình nền/daemon) và không phải dạng phần mềm máy tính để bàn.

Nhưng đây không có nghĩa là bạn không thể dùng nó trên máy tính cá nhân của bạn hoặc là dùng nó sẽ phức tạp hơn cách dùng thông thường. ASF là một phần mềm độc lập không cần cài đặt và có thể hoạt động ngay và luôn, nhưng cần quá trình cấu hình trước đó để nó có thể sử dụng được. Cấu hình là cách bảo cho ASF là nó nên làm gì sau khi bạn khởi động nó. Nếu bạn khởi động nó mà không cài cấu hình, thì đơn giản là ASF sẽ không làm gì cả đâu.

---

## Cài đặt tùy theo hệ điều hành

Thông thường, đây sẽ là những việc chúng ta sẽ làm trong vài phút tiếp theo:
- Cài đặt **[phần phụ thuộc .NET](#net-prerequisites)**.
- Tải về **[phiên bản ASF mới nhất](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** theo dạng hệ điều hành tương ứng.
- Giải nén tập tin lưu trữ vào một vị trí mới.
- **[Tùy chỉnh cấu hình ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.
- Khởi động ASF và nhìn thấy sự kì diệu của nó.

Nghe cũng đơn giản lắm phải không? Vậy thì cùng bắt đầu thôi nào.

---

### Phần phụ thuộc .NET

Bước đầu tiên là đảm bảo hệ điều hành của bạn ít nhất có thể khởi động ASF đúng cách. ASF được viết bằng ngôn ngữ C#, dựa trên nền tảng .NET và có thể yêu cầu các thư viện gốc không có sẵn trên nền tảng của bạn. Depending on whether you use Windows, Linux or macOS, you will have different requirements, although all of them are listed in **[.NET prerequisites](https://docs.microsoft.com/dotnet/core/install)** document that you should follow. This is our reference material that should be used, but for the sake of simplicity we've also detailed all needed packages below, so you don't need to read the full document.

It's perfectly normal that some (or even all) dependencies already exist on your system due to being installed by third-party software that you're using. Still, you should ensure that it's truly the case by running appropriate installer for your OS - without those dependencies ASF won't launch at all.

Keep in mind that you don't need to do anything else for OS-specific builds, especially installing .NET SDK or even runtime, since OS-specific package includes all of that already. You need only .NET prerequisites (dependencies) to run .NET runtime included in ASF.

#### **[Windows](https://docs.microsoft.com/dotnet/core/install/windows)**:
- **[Microsoft Visual C++ Redistributable Update](https://docs.microsoft.com/cpp/windows/latest-supported-vc-redist?view=msvc-170#visual-studio-2015-2017-2019-and-2022)** (**[x64](https://aka.ms/vs/17/release/vc_redist.x64.exe)** for 64-bit, **[x86](https://aka.ms/vs/17/release/vc_redist.x86.exe)** for 32-bit or **[arm64](https://aka.ms/vs/17/release/vc_redist.arm64.exe)** for 64-bit ARM)
- It's highly recommended to ensure that all Windows updates are already installed. If you don't have them enabled, then at the very least you need **[KB2533623](https://support.microsoft.com/en-us/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)** and **[KB2999226](https://support.microsoft.com/en-us/help/2999226/update-for-universal-c-runtime-in-windows)**, but more updates may be needed. You don't need to install them if your Windows is up-to-date.

#### **[Linux](https://docs.microsoft.com/dotnet/core/install/linux)**:
Package names depend on the Linux distribution that you're using, we've listed the most common ones. You can obtain all of them with native package manager for your OS (such as `apt` for Debian or `yum` for CentOS).

- `ca-certificates` (standard trusted SSL certificates to make HTTPS connections)
- `libc6` (`libc`)
- `libgcc-s1` (`libgcc1`, `libgcc`)
- `libicu` (`icu-libs`, latest version for your distribution, for example `libicu72`)
- `libgssapi-krb5-2` (`libkrb5-3`, `krb5-libs`)
- `libssl3` (`libssl`, `openssl-libs`, latest version for your distribution, at least `1.1.X`)
- `libstdc++6` (`libstdc++`, in version `5.0` or higher)
- `zlib1g` (`zlib`)

At least a majority of those should be already natively available on your system. The minimal installation of Debian stable required only `libicu72`.

#### **[macOS](https://docs.microsoft.com/dotnet/core/install/macos)**:
- Hiện không cần gì cả, nhưng bạn phải cài đặt phiên bản macOS mới nhất, ít nhất bản 12.0 trở lên.

---

### Tải về

Since we have all required dependencies already, the next step is downloading **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**. ASF is available in many variants, but you're interested in package that matches your operating system and architecture. For example, if you're using `64`-bit `Win`dows, then you want `ASF-win-x64` package. For more information about available variants, visit **[compatibility](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility)** section. ASF is also able to run on OSes that we're not building OS-specific package for, such as **32-bit Windows**, head over to **[generic setup](#generic-setup)** for that.

![Assets](https://i.imgur.com/Ym2xPE5.png)

After download, start from extracting the zip file into its own folder. If you require specific tool for that, **[7-zip](https://www.7-zip.org)** will do it, but all standard utilities like `unzip` from Linux/macOS should work without problems as well.

Be advised to unpack ASF to **its own directory** and not to any existing directory you're already using for something else - ASF's auto-updates feature will delete all old and unrelated files when upgrading, which may lead to you losing anything unrelated you put in ASF directory. If you have any extra scripts or files that you want to use with ASF, put them in one folder above.

Cấu trúc ví dụ sẽ nhìn như thế này:

```text
C:\ASF (nơi bạn để đồ riêng của bạn)
    ├── ASF shortcut.lnk (tùy ý)
    ├── Config shortcut.lnk (tùy ý)
    ├── Commands.txt (tùy ý)
    ├── MyExtraScript.bat (tùy ý)
    ├── (...) (những tệp tin khác theo ý bạn, tùy ý)
    └── Core (dành riêng cho ASF, nơi bạn giải tệp tin nén ra)
         ├── ArchiSteamFarm(.exe)
         ├── config
         ├── logs
         ├── plugins
         └── (...)
```

---

### Cấu hình

We're now ready to do the very last step, the configuration. This is by far the most complicated step, since it involves a lot of new information you're not familiar with yet, so we'll try to provide some easy to understand examples and simplified explanation here.

First and foremost, there is **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** page that explains **everything** that relates to configuration, but it's a massive amount of new information, a lot of which we don't need to know right away. Instead, we'll teach you how to get the information you're actually looking for.

ASF configuration can be done in at least three ways - through our web config generator, ASF-ui or manually. This is explained in-depth in **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section, so refer to that if you want more detailed information. We'll use web config generator as a starting point.

Navigate to our **[web config generator](https://justarchinet.github.io/ASF-WebConfigGenerator)** page with your favourite browser, you'll need to have javascript enabled in case you manually disabled it. We recommend Chrome or Firefox, but it should work on all most popular browsers.

After opening the page, switch to "Bot" tab. You should now see a page similar to the one below:

![Bot tab](https://i.imgur.com/aF3k8Rg.png)

If by any chance the version of ASF that you've just downloaded is older than what config generator is set to use by default, simply choose your ASF version from the dropdown menu. This can happen as the config generator can be used for generating configs to newer (pre-release) ASF versions that weren't marked as stable yet. You've downloaded latest stable release of ASF that is verified to work reliably.

Start from putting name for your bot into the field highlighted as red. This can be any name you'd like to use, such as your nickname, account name, a number, or anything else. There is only one word that you can't use, `ASF`, as that keyword is reserved for global config file. In addition to that your bot name can't start with a dot (ASF intentionally ignores those files). We also recommend that you avoid using spaces, you can use `_` as a word separator if needed.

After you decided about your name, change `Enabled` switch to be on, this defines whether your bot is supposed to be started by ASF automatically after launch (of the program).

Now you can decide upon two things:
- You can put your login in `SteamLogin` field and your password in `SteamPassword` field
- Or you can leave them empty

Doing the first thing will allow ASF to automatically use your account credentials during startup, so you won't need to input them manually each time ASF needs them. You can however decide to omit them, in which case they're not being saved, so ASF won't be able to automatically start without your help and you'll need to input them during runtime.

ASF requires your login credentials because it includes its own implementation of Steam client and needs the same details to log in as the one that you use yourself. Your login credentials are not saved anywhere but on your PC in ASF `config` directory only, our web config generator is client-based which means that the code is run locally in your browser to generate valid ASF configs, without details you're inputting ever leaving your PC in the first place, so there is no need to worry about any possible sensitive data leak. Still, if you for whatever reason don't want to put your credentials there, we understand that, and you can put them manually later in generated files, or omit them entirely and put them only in ASF command prompt. More on security matter can be found in **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section.

You can also decide to leave just one field empty, such as `SteamPassword`, ASF will then be able to use your login automatically, but will still ask for password (similar to Steam Client). If you're using Steam parental to unlock the account, you'll need to put it into `SteamParentalCode` field.

After the decision and optional details, your web page will now look similar to the one below:

![Bot tab 2](https://i.imgur.com/yf54Ouc.png)

You can now hit "download" button and our web config generator will generate new `json` file based on your chosen name. Save that file into `config` directory which is located in the folder where you've extracted our zip file in the previous step.

Your `config` directory will now look like this:

![Structure 2](https://i.imgur.com/crWdjcp.png)

Xin chúc mừng! Bạn vừa mới hoàn thành việc cấu hình bot ASF cơ bản. Chúng ta sẽ mở rộng vấn đề này sau, hiện giờ đây là mọi thứ bạn cần rồi đấy.

---

### Khởi động ASF

Bây giờ bạn đã sẵn sàng để khởi động phần mềm lần đầu tiên. Simply double-click `ArchiSteamFarm` binary in ASF directory. Bạn cũng có thể khởi động nó thông qua console.

After doing so, assuming you installed all required dependencies in the first step, ASF should launch properly, notice your first bot (if you didn't forget to put generated config in `config` directory), and attempt to log in:

![ASF](https://i.imgur.com/u5hrSFz.png)

If you supplied `SteamLogin` and `SteamPassword` for ASF to use, you'll be asked for your SteamGuard token only (e-mail, 2FA or none, depending on your Steam settings). If you didn't, you'll also be asked for your Steam login and password.

Now would be a good time to review our **[remote communication](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication)** section if you're concerned about stuff ASF is programmed to do, including actions it'll take in your name, such as joining our Steam group.

After passing through initial login gate, assuming your details are correct, you'll successfully log in, and ASF will start farming using default settings that you didn't change as of now:

![ASF 2](https://i.imgur.com/Cb7DBl4.png)

This proves that ASF is now successfully doing its job on your account, so you can now minimize the program and do something else. After enough of time (depending on **[performance](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**), you'll see Steam trading cards slowly being dropped. Of course, for that to happen you must have valid games to farm, showing as "you can get X more card drops from playing this game" on your **[badges page](https://steamcommunity.com/my/badges)** - if there are no games to farm, then ASF will state that there is nothing to do, as stated in our **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#what-is-asf)**.

This concludes our very basic setting up guide. You can now decide whether you want to configure ASF further, or let it do its job in default settings. We'll cover a few more basic details, then leave you entire wiki for discovery.

---

### Thiết lập mở rộng

#### Cày nhiều tài khoản cùng một lúc

ASF supports farming more than one account at a time, which is its primary function. You can add more accounts to ASF by generating more bot config files, in exactly the same way as you've generated your first one just a few minutes ago. You need to ensure only two things:

- Unique bot name, if you already have your first bot named "MainAccount", you can't have another one with the same name.
- Valid login details, such as `SteamLogin`, `SteamPassword` and `SteamParentalCode` (if using Steam parental settings)

In other words, simply jump to configuration again and do exactly the same, just for your second or third account. Remember to use unique names for all of your bots.

---

#### Thay đổi cài đặt

You change existing settings in exactly the same way - by generating a new config file. If you didn't close our web config generator yet, click on "toggle advanced settings" and see what is there for you to discover. For this tutorial we'll change `CustomGamePlayedWhileFarming` setting, which allows you to set custom name being displayed when ASF is farming, instead of showing actual game.

So let's do that, if you run ASF and start farming, in default settings you'll see that your Steam account is in-game now:

![Steam](https://i.imgur.com/1VCDrGC.png)

Vậy ta cùng thay đổi nó nào. Toggle advanced settings in web config generator and find `CustomGamePlayedWhileFarming`. Once you do that, put your own custom text there that you want to display, such as "Idling cards":

![Bot tab 3](https://i.imgur.com/gHqdEqb.png)

Now download the new config file in exactly the same way, then **overwrite** your old config file with new one. You can also delete your old config file and put new one in its place of course.

Once you do that and start ASF again, you'll notice that ASF now displays your custom text in previous place:

![Steam 2](https://i.imgur.com/vZg0G8P.png)

This confirms that you've successfully edited your config. In exactly the same way you can change global ASF properties, by switching from bot tab to "ASF" tab, downloading generated `ASF.json` config file and putting it in your `config` directory.

Editing your ASF configs can be done much easier by using our ASF-ui frontend, which will be explained further below.

---

#### Sử dụng ASF-ui

ASF là một ứng dụng console và không bao gồm một giao diện đồ họa người dùng. However, we're actively working on **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)** frontend to our IPC interface, which can be a very decent and user-friendly way to access various ASF features.

In order to use ASF-ui, you need to have `IPC` enabled, which is the default option. Once you launch ASF, you should be able to confirm that it properly started the IPC interface automatically:

![IPC](https://i.imgur.com/ZmkO8pk.png)

Bạn có thể truy cập giao diện IPC thông qua liên kết **[này](http://localhost:1242)**, miễn là ASF đang chạy ngay trên máy tính đó. You can use ASF-ui for various purposes, e.g. editing the config files in-place or sending **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. Feel free to take a look around in order to find out all ASF-ui functionalities.

![ASF-ui](https://raw.githubusercontent.com/JustArchiNET/ASF-ui/main/.github/previews/bots.png)

---

### Tóm tắt lại

You've successfully set up ASF to use your Steam accounts and you've already customized it to your liking a little. If you followed our entire guide, then you also managed to tweak ASF through our ASF-ui interface and found out that ASF actually has a GUI of some sort. Now is a good time to read our entire **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section in order to learn what all those different settings you've seen actually do, and what ASF has to offer. If you've stumbled upon some issue or you have some generic question, read our **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)** instead which should cover all, or at least a vast majority of questions that you may have. If you want to learn everything about ASF and how it can make your life easier, head over to the rest of **[our wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home)**. If you found out our program to be useful for you and you appreciate the massive amount of work that was put into it, you can also consider a **[donation](https://github.com/JustArchiNET/ArchiSteamFarm?tab=readme-ov-file#archisteamfarm)** to our cause. Dù sao chăng nữa, thì chúc vui vẻ nhé!

---

## Cài đặt phiên bản chung (Generic)

This setup is for advanced users that want to set up ASF to run in **[generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#generic)** variant. While being more troublesome in usage than **[OS-specific variants](#os-specific-setup)**, they also come with additional benefits.

You want to use `generic` variant mainly in those situations (but of course you can use it regardless):
- When you're using OS that we don't build OS-specific package for (such as 32-bit Windows)
- When you already have .NET Runtime/SDK, or want to install and use one
- When you want to minimize ASF structure size and memory footprint by handling runtime requirements yourself
- When you want to use a custom **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** which requires a `generic` setup of ASF to run properly (due to missing native dependencies)

However, keep in mind that you're in charge of .NET runtime in this case. This means that if your .NET SDK (runtime) is unavailable, outdated or broken, ASF won't work. This is why we don't recommend this setup for casual users, since you now need to ensure that your .NET SDK (runtime) matches ASF requirements and can run ASF, as opposed to **us** ensuring that our .NET runtime bundled with ASF can do so.

Với gói `generic`, bạn có thể làm theo hướng dẫn riêng cho hệ điều hành đã ghi ở trên, với 2 thay đổi nhỏ. Ngoài việc cài đặt phần phụ thuộc .NET, bạn còn phải cài đặt SDK (Bộ công cụ phát triển phần mềm) .NET, và thay vì bạn có tệp tin thực thi `ArchiSteamFarm(.exe)` dựa theo hệ điều hành, bạn giờ chỉ có tệp tin nhị phân chung `ArchiSteamFarm.dll` thôi. Mọi thứ khác đều giống nhau hoàn toàn.

Với các bước thêm sau đây:
- Install **[.NET prerequisites](#net-prerequisites)**.
- Install **[.NET SDK](https://www.microsoft.com/net/download)** (or at least ASP.NET Core and .NET runtimes) appropriate for your OS. You most likely want to use an installer. Refer to **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)** if you're not sure which version to install.
- Download **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** in `generic` variant.
- Extract the archive into new location.
- **[Configure ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.
- Launch ASF by either using a helper script or executing `dotnet /path/to/ArchiSteamFarm.dll` manually from your favourite shell.

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/macOS) are located next to `ArchiSteamFarm.dll` binary - those are included in `generic` variant only. You can use them if you don't want to execute `dotnet` command manually. Obviously helper scripts won't work if you didn't install .NET SDK and you don't have `dotnet` executable available in your `PATH`. Helper scripts are entirely optional to use, you can always `dotnet /path/to/ArchiSteamFarm.dll` manually.
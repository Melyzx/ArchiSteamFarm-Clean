# Giao tiếp từ xa

Phần này sẽ mô tả kĩ càng hơn về việc giao tiếp từ xa có sẵn trong ASF, bao gồm cả việc giải thích thêm về cách bạn có thể thay đổi chúng. Dù chúng tôi không tin những gì được mô tả ở dưới là nội dung độc hại hoặc không mong muốn, và về mặt pháp lý thì chúng tôi không có nghĩa vụ phải tiết lộ nó, nhưng chúng tôi hy vọng bạn sẽ hiểu rõ hơn về chức năng của chương trình này, đặc biệt liên quan đến việc đến quyền riêng tư và dữ liệu được chia sẻ của bạn.

## Steam

ASF giao tiếp với mạng lưới Steam (**[Máy chủ CM](https://api.steampowered.com/ISteamDirectory/GetCMList/v1?cellid=0)** (content management - quản lý nội dung)), cùng với **[API Steam](https://steamcommunity.com/dev)**, **[Cửa hàng Steam](https://store.steampowered.com)** và **[Cộng đồng Steam](https://steamcommunity.com)**.

Bạn không thể vô hiệu hóa bất kì phần giao tiếp nào ở trên cả, vì đây là cơ sở cho ASF để nó có thể cung cấp các chức năng cơ bản. Bạn sẽ phải chọn không sử dụng ASF nếu bạn không thấy thoải mái về những phần giao tiếp ở trên.

## Nhóm Steam

ASF giao tiếp với **[Nhóm Steam](https://steamcommunity.com/groups/archiasf)** của chúng tôi. The group provides you with announcements, especially new versions, critical issues, Steam problems and other things that are important to keep community updated. It also allows you to use our technical support, by asking questions, resolving problems, reporting issues or suggesting improvements. By default, accounts used in ASF will automatically join the group upon login.

You can decide to opt-out of joining the group by disabling `SteamGroup` flag in bot's **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** settings.

## GitHub

ASF giao tiếp với **[API của GitHub](https://api.github.com)** để có thể lấy **[các bản phát hành của ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** cho chức năng cập nhật. This is done as part of auto-updates (if you've kept **[`UpdatePeriod`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#updateperiod)** enabled), as well as `update` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. You can influence ASF's communication with GitHub through **[`UpdateChannel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#updatechannel)** property - setting it to `None` will result in disabling entire update functionality, including GitHub communication in this regard.

## Máy chủ của ASF

ASF giao tiếp với **[máy chủ của chúng tôi](https://asf.justarchi.net)** để có thể cung cấp các chức năng nâng cao. Cụ thể sẽ bao gồm:
- Verifying checksums of ASF builds downloaded from GitHub against our own independent database to ensure that all downloaded builds are legitimate (free of malware, MITM attacks or other tampering)
- Fetching list of bad bots for filtering if you've kept `FilterBadBots` global config setting enabled.
- Announcing your bot in **[our listing](https://asf.justarchi.net/STM)** if you've enabled `SteamTradeMatcher` in **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)** and meet other criteria
- Downloading currently available bots to trade from **[our listing](https://asf.justarchi.net/STM)** if you've enabled `MatchActively` in **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)** and meet other criteria

Vì là một biện pháp bảo mật, bạn không thể tắt xác minh giá trị tổng kiểm cho các bản dựng ASF. However, you can disable auto-updates entirely if you'd like to avoid this, as described above in the GitHub section.

You can disable `FilterBadBots` setting if you want to avoid fetching the list from the server.

You can decide to opt-out of being announced in the listing by disabling `PublicListing` flag in bot's **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** settings. This might be useful if you'd like to run `SteamTradeMatcher` bot without being announced at the same time.

Downloading bots from our listing is mandatory for `MatchActively` setting, you'll need to disable that setting if you're unwilling to accept that.
# Bên thứ ba

Mục này chứa một vài phần mở rộng của bên thứ ba được viết dành riêng (hoặc hầu hết) dành cho việc hoạt động cùng ASF. Chúng bao gồm từ các phần mở rộng ASF, thông qua các ứng dụng web đơn giản và thư viện nội bộ để tích hợp, đến các bot đầy đủ tính năng cho các nền tảng khác. Nếu bạn muốn thêm thứ gì đó vào danh sách này, hãy cho chúng tôi biết trên Discord hoặc trên nhóm Steam của chúng tôi.

Xin lưu ý rằng các chương trình bên dưới **không** được duy trì bởi các nhà phát triển ASF và do đó **chúng tôi không đảm bảo về bất kỳ chương trình nào**, đặc biệt là về bảo mật, an toàn hoặc tuân thủ Điều khoản Dịch vụ Steam. Danh sách này được duy trì chỉ để tham khảo. Bạn phải luôn đảm bảo rằng tiện ích của bên thứ ba mà bạn sắp sử dụng đủ an toàn và hợp pháp đối với bạn, vì bạn đang tự chịu rủi ro khi sử dụng tất cả các tiện ích đó.

---

## Phần mở rộng của ASF

### **[CatPoweredPlugins](https://github.com/CatPoweredPlugins)** (**[Rudokhvist](https://github.com/Rudokhvist)**)

- **[ASFAchievementManager](https://github.com/CatPoweredPlugins/ASFAchievementManager)**, plugin for ASF that allows you to manage Steam achievements.
- **[BirthdayPlugin](https://github.com/CatPoweredPlugins/BirthdayPlugin)**, plugin for ASF to receive birthday congratulations.
- **[CaseInsensitiveASF](https://github.com/CatPoweredPlugins/CaseInsensitiveASF)**, plugin for ASF to make bot names case-insensitive.
- **[CommandAliasPlugin](https://github.com/CatPoweredPlugins/CommandAliasPlugin)**, plugin for ASF to setup custom command aliases for ASF commands.
- **[CommandlessRedeem](https://github.com/CatPoweredPlugins/CommandlessRedeem)**, plugin for ASF to re-implement key redeeming without a command.
- **[ItemDispenser](https://github.com/CatPoweredPlugins/ItemDispenser)**, plugin for ASF to automatically accept trade request for certain type(s) of items.
- **[SelectiveLootAndTransferPlugin](https://github.com/CatPoweredPlugins/SelectiveLootAndTransferPlugin)**, plugin for ASF providing advanced `transfer` command for transferring Steam items.

### **[Citrinate](https://github.com/Citrinate)**

- **[BoosterManager](https://github.com/Citrinate/BoosterManager)**, phần mở rộng cho ASF cung cấp một giao diện dễ dùng để chuyển ngọc thành gói bổ sung cũng như là nhiều chức năng để quản lý kho đồ và các lệnh mua bán.
- **[CS2Interface](https://github.com/Citrinate/CS2Interface)** là phần mở rộng dành cho ASF cho phép bạn tương tác với Counter-Strike 2 bằng cách sử dụng **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** (Giao tiếp liên tiến trình).
- **[FreePackages](https://github.com/Citrinate/FreePackages)** là phần mở rộng dành cho ASF chuyên đi tìm gói miễn phí trên Steam và thêm vào tài khoản của bạn.

### **[Vita](https://github.com/ezhevita)**

- **[FriendAccepter](https://github.com/ezhevita/FriendAccepter)** là phần mở rộng dành cho ASF để nó tự động chấp nhận tất cả lời mời kết bạn.
- **[GameRemover](https://github.com/ezhevita/GameRemover)** là phần mở rộng dành cho ASF triển khai lệnh xoá giấy phép Steam đối với các phiên bản bot đã chọn.
- **[GetEmail](https://github.com/ezhevita/GetEmail)** là phần mở rộng dành cho ASF triển khai lệnh tìm nạp địa chỉ thư điện tử của các phiên bản bot nhất định trực tiếp từ Steam.
- **[ResetAPIKey](https://github.com/ezhevita/ResetAPIKey)** là phần mở rộng dành cho ASF triển khai lệnh đặt lại mã API cho các phiên bản bot đã chọn.

### Khác

- **[ASFEnhance](https://github.com/chr233/ASFEnhance)** là phần mở rộng dành cho ASF giúp nâng cao nó với nhiều tính năng mới, đặc biệt là các câu lệnh.
- **[ASFFreeGames](https://github.com/maxisoft/ASFFreeGames)** là phần mở rộng dành cho ASF cho phép ai đó tự động nhận trò chơi Steam miễn phí được đăng trên Reddit.

---

## Tích hợp

- **[ASFBot](https://github.com/dmcallejo/ASFBot)**, bot telegram được viết bằng python có tích hợp ASF.
- **[ASF STM userscript](https://greasyfork.org/vi/scripts/404754-asf-stm)**, dành cho những người muốn gửi đề nghị giao dịch tự động tới bot trên **[danh sách ASF STM](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin-vi-VN#publiclisting)** của chúng tôi thông qua trình duyệt web mà không sử dụng tính năng `MatchActively` do ASF cung cấp.
- **[simple-asf-bot](https://github.com/deluxghost/simple-asf-bot)**, another (minimal) telegram bot written in python featuring ASF integration.

---

## Thư viện

- **[ASF-IPC](https://github.com/deluxghost/ASF_IPC)** là thư viện Python để tích hợp thêm với giao diện IPC của ASF.

---

## Đóng gói

- **[Kho AUR #1](https://aur.archlinux.org/packages/asf)** cho phép bạn dễ dàng cài đặt ASF trên Arch Linux.
- **[Kho AUR #2](https://aur.archlinux.org/packages/archisteamfarm-bin)** cho phép bạn dễ dàng cài đặt ASF trên Arch Linux.
- **[Homebrew](https://formulae.brew.sh/formula/archi-steam-farm)** cho phép bạn dễ dàng cài đặt ASF trên macOS.
- **[Nix](https://search.nixos.org/packages?channel=unstable&show=ArchiSteamFarm&from=0&size=50&sort=relevance&type=packages&query=ArchiSteamFarm)** cho phép bạn dễ dàng cài đặt ASF trên các bản phân phối với Nix.
- **[NixOS](https://search.nixos.org/options?channel=unstable&from=0&size=50&sort=relevance&type=packages&query=ArchiSteamFarm)** cho phép bạn định cấu hình và cài đặt ASF với NixOS.
- **[Winget](https://github.com/microsoft/winget-pkgs/tree/master/manifests/j/JustArchiNET/ArchiSteamFarm)**, cho phép bạn dễ dàng cài đặt ASF trên Windows.

---

## Công cụ

- **[Trình trích mã](https://umaim.github.io/SKE)** cho phép bạn sao chép-dán mã ở nhiều định dạng khác nhau và tạo lệnh `redeem` cho ASF. Kiểm tra **[kho GitHub](https://github.com/PixvIO/SKE)** để biết thêm chi tiết.
- **[Trình biên tập Cấu hình ASF Hàng loạt](https://github.com/genesix-eu/ASF_MCE)** cho phép bạn quản lý nhiều cấu hình ASF dễ dàng hơn.

---

## Bạn muốn tìm thêm?

Chúng tôi đề xuất chủ đề **[ArchiSteamFarm](https://github.com/topics/archisteamfarm)** trên GitHub cho tất cả các dự án tích hợp với ASF.
# SteamTokenDumperPlugin

`SteamTokenDumperPlugin` (Phần mở rộng Xuất token Steam) là một **[phần mở rộng](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** chính thức cho ASF được phát triển bởi chúng tôi, cho phép bạn đóng góp với dự án **[SteamDB](https://steamdb.info)** bằng cách chia sẻ token gói, token ứng dụng và khóa depot mà tài khoản Steam của bạn có quyền truy cập vào. Thông tin thêm về những dữ liệu được thu thập và vì sao SteamDB lại cần chúng có thể tìm thấy ở trên trang **[Token Dumper](https://steamdb.info/tokendumper)** (Trình xuất Token) của SteamDB. Phần dữ liệu đã được gửi đi không bao gồm các thông tin có thể nhạy cảm nào, và không gây ra rủi ro bảo mật/riêng tư nào, như đã được nhắc đến ở phần mô tả ở trên.

---

## Kích hoạt phần mở rộng

ASF đã mang kèm sẵn `SteamTokenDumperPlugin` trong gói phát hành, nhưng mặc định thì phần mở rộng này không được kích hoạt. Bạn có thể kích hoạt phần mở rộng bằng cách chỉnh tùy chọn `SteamTokenDumperPluginEnabled` (Kích hoạt STDP) trong thuộc tính tùy chỉnh chung của ASF thành `true` (đúng), theo cú pháp JSON sau đây:

```json
{
  "SteamTokenDumperPluginEnabled": true
}
```

On the launch of the ASF program, the plugin will let you know whether it was enabled successfully through standard ASF logging mechanism. You can also enable the plugin through our web-based config generator.

---

## Thông tin kỹ thuật

Upon enabling, the plugin will use the bots that you're running in ASF for data gathering in form of package tokens, app tokens and depot keys that your bots have access to. Data gathering module includes passive and active routines that are supposed to minimize the additional overhead caused by collecting data.

In order to fulfill the planned use case, in addition to data gathering routine explained above, submission routine is initialized as being responsible for determining what data needs to be submitted to SteamDB on periodic basis. This routine will fire in up to `1` hour since your ASF start, and will repeat itself every `24` hours. The plugin will do its best to minimize the amount of data that needs to be sent, therefore it's possible that some data which the plugin will collect will be determined as useless to submit, and therefore skipped (for example app update which doesn't change the access token).

The plugin uses a persistent cache database saved in `config/SteamTokenDumper.cache` location, which serves a similar purpose to `config/ASF.db` for ASF. The file is used in order to record the gathered and submitted data and minimize the amount of work that has to be done across different ASF runs. Removing the file causes the process to be restarted from scratch, which should be avoided if possible.

---

## Dữ liệu

ASF includes the contributor `steamID` in the request, which is determined as `SteamOwnerID` that you set in ASF, or in case you didn't, the Steam ID of the bot which owns the most licenses. The announced contributor might receive some additional perks from SteamDB for continuous help (e.g. donator rank on the website), but that is entirely up to SteamDB's discretion.

In any case, SteamDB staff would like to thank you in advance for your help. The submitted data allows SteamDB to operate, in particular to track info about packages, apps and depots, which would no longer be possible without your help.

---

## Lệnh

STD plugin comes with an extra ASF command, `std [Bots]`, which allows you to trigger refresh and submission for selected bots on demand. Using the command doesn't require enabled config, which allows you to skip automatic gathering and submission, and control the process yourself manually. Naturally it can also be executed with enabled config, which will simply trigger the usual gathering and submission procedures earlier than expected.

We recommend `!std ASF` in order to trigger refresh for all available bots. However, you can also trigger it for selected ones if you'd like to.

---

## Tùy chỉnh nâng cao

Phần mở rộng của chúng tôi hỗ trợ tùy chỉnh nâng cao, có thể giúp ích cho những người muốn chỉnh sửa thông số trong phần mở rộng này theo ý muốn của họ.

Phần tùy chỉnh nâng cao có cấu trúc sau đây được nằm trong tệp tin `ASF.json`:

```json
{
  "SteamTokenDumperPlugin": {
    "Enabled": false,
    "SecretAppIDs": [],
    "SecretDepotIDs": [],
    "SecretPackageIDs": [],
    "SkipAutoGrantPackages": true
  }
}
```

Mọi tùy chọn đều được giải thích dưới đây:

### `Enabled`

`bool` type with default value of `false`. This property acts the same as `SteamTokenDumperPluginEnabled` root-level property explained above, and can be used instead, dedicated to people that would prefer to have entire plugin-related config in its own structure (so most likely those already using other advanced properties explained below).

---

### `SecretAppIDs`

`ImmutableHashSet<uint>` type with default value of being empty. This property specifies `appIDs` that the plugin won't resolve, and if they're already resolved, won't submit the token for. This property can be useful for people with access to potentially-sensitive information about unpublished items, especially the developers, publishers or closed beta testers.

---

### `SecretDepotIDs`

`ImmutableHashSet<uint>` type with default value of being empty. This property specifies `depotIDs` that the plugin won't resolve, and if they're already resolved, won't submit the key for. This property can be useful for people with access to potentially-sensitive information about unpublished items, especially the developers, publishers or closed beta testers.

---

### `SecretPackageIDs`

`ImmutableHashSet<uint>` type with default value of being empty. This property specifies `packageIDs` (also known as `subIDs`) that the plugin won't resolve, and if they're already resolved, won't submit the token for. This property can be useful for people with access to potentially-sensitive information about unpublished items, especially the developers, publishers or closed beta testers.

---

### `SkipAutoGrantPackages`

`bool` type with default value of `true`. Thuộc tính này hoạt động rất tương đồng với `SecretPackageIDs` và khi kích hoạt, sẽ khiến cho gói với `EPaymentMethod` (phương pháp thanh toán điện tử) có giá trị là `AutoGrant` (Tự động được cung cấp) được bỏ qua trong quá trình giải quyết được giải thích dưới đây. `AutoGrant` payment method is used by **[Steamworks](https://partner.steamgames.com)** to automatically grant packages on developer accounts. While this is not as explicit as other `Secret` options, and therefore doesn't guarantee anything (since you might have other packages than `AutoGrant` that you still don't want to submit), it should be good enough for skipping majority, if not all, of the secret packages. This option is enabled by default, as people that actually have access to `AutoGrant` packages will almost never want to leak those to general public, and therefore using value of `false` is very situational.

---

## Giải thích thêm

At the root level, every Steam account owns a set of packages (licenses, subscriptions), which are classified by their `packageID` (also known as `subID`). Every package may contain several apps classified by their `appID`. Every app may then include several depots classified by their `depotID`.

```text
├── sub/124923
│     ├── app/292030
│     │     ├── depot/292031
│     │     ├── depot/378648
│     │     └── ...
│     ├── app/378649
│     └── ...
└── ...
```

Our plugin includes two routines which take into account skipped items - the resolve routine and submission routine.

The resolve routine is responsible for resolving the tree you can see above. By blacklisting the packages/apps/depots in advance, you'll effectively cut the tree in the place of blacklisted branch/leaf without additional need of specifying the remaining parts of it. In our example above, if `124923` package was ignored, whether by `SecretPackageIDs` or `SkipAutoGrantPackages`, and it was the only package you own which linked to the `292030` appID, then appID `292030` wouldn't get resolved either, and by definition, if there were no other resolved apps which linked to the `292031` and `378648` depots, then they wouldn't get resolved either. However, keep in mind that if the plugin has already resolved the tree, then effectively this will only stop given item from being updated (e.g. new apps added), it will not make the plugin "forget" about the existing items that were already resolved (e.g. apps found in that package before you decided to blacklist it). If you've just enabled some skipping options, and would like to ensure that ASF doesn't traverse the already-resolved tree, you may consider one-time removing `config/SteamTokenDumper.cache` file where the plugin keeps its cache.

The submission routine is responsible for submitting package tokens, app tokens and depot keys of already resolved items (by the resolve routine above). Here your blacklist has immediate effect, as even if the plugin has already resolved the info, the submission routine will not actually submit it over to SteamDB if you have it blacklisted, regardless if it has been resolved or not. Keep in mind however that we're not talking about the tree anymore at this point, the submission routine does not know whether the information about the app comes from this or that package, so it only skips information about particular, blacklisted items, regardless of the relation they're in with other.

For majority of the developers and publishers, it should be enough to enable `SkipAutoGrantPackages`, potentially empowered with `SecretPackageIDs` only, as it effectively cuts the tree at the beginning branch and guarantees that the apps and depots included further will not get submitted as long as there is no other package linking to the same app. If you want to be double sure, in addition to that you can also use `SecretAppIDs`, which will skip the resolve of the app even if there are some other licenses you didn't blacklist linking to it. Using `SecretDepotIDs` should not be needed, unless you have a particular, specific need (such as skipping only a particular depot while still submitting info about packages and apps), or if you want to add yet another layer to be triple safe.
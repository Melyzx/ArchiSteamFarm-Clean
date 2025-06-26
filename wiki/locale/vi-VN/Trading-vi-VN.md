# Trao đổi

ASF có bao gồm hỗ trợ cho các yêu cầu trao đổi không tương tác (rời mạng) trên Steam. Cả việc nhận (chấp thuận/từ chối) và việc gửi yêu cầu trao đổi đã có sẵn ngay luôn và không yêu cầu tùy chỉnh đặc biệt gì, nhưng sẽ chắc chắn yêu cầu tài khoản Steam không bị giới hạn (là những tài khoản đã tiêu 5$ (đô la Mỹ) trong cửa hàng). Những tài khoản bị giới hạn chỉ có thể có chức năng trao đổi một cách bị giới hạn.

---

## Logic

ASF sẽ luôn chấp nhận mọi loại yêu cầu trao đổi từ người dùng có quyền hạn truy cập vào bot cấp độ `Master` (hoặc cao hơn), cho dù bất kỳ vật phẩm gì đi chăng nữa. Việc này không chỉ cho phép việc dễ dàng thu thập thẻ Steam được cày bởi thực thể bot, nhưng cũng cho phép dễ dàng quản lý vật phẩm Steam mà bot đã giữ trong túi đồ - bao gồm những thứ từ các trò chơi khác (chẳng hạn CS:GO).

ASF sẽ từ chối yêu cầu trao đổi từ người dùng (không phải Master) đã bị đưa vào danh sách đen của môđun trao đổi. Danh sách đen được trữ trong tệp cơ sở dữ liệu tiêu chuẩn `BotName.db`, và có thể được quản lý thông qua **[những câu lệnh](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** `tb`, `tbadd` và `tbrm`. Điều này có thể hoạt động như một cách khác ngoài cách chặn người dùng tiêu chuẫn được Steam cung cấp - nên hãy thận trọng khi sử dụng.

ASF sẽ chấp nhận tất cả yêu cầu trao đổi như `loot` được gửi giữa các bot, trừ khi thuộc tính `DontAcceptBotTrades` (Không chấp nhận trao đổi từ bot) được chỉ định trong `TradingPreferences` (Sở thích trao đổi). Tóm lại, thuộc tính `TradingPreferences` mặc định là `None` (Không có) sẽ khiến ASF tự động chấp nhận yêu cầu trao đổi từ các người dùng với quyền hạn `Master` (đã giải thích ở trên), và tất cả các yêu cầu trao đổi dạng đóng góp từ các bot khác đang tham gia vào quá trình ASF.

First of all, it's possible to disable **all** incoming trade offers, by using `DisableIncomingTradesParsing` flag in `BotBehaviour`. Using that, as the name implies, will disable all functionality related to incoming trades parsing, which includes above logic, as well as all extra features available below which depend on reacting to the incoming trade offer. Since default settings are already non-intrusive, you should consider using that option only if you have absolutely no intent from ASF to do anything related to the incoming trades at all.

It's possible to disable donation trades from other bots, through `DontAcceptBotTrades` in your `TradingPreferences`.

Khi bạn chọn tùy chỉnh `AcceptDonations` (Chấp nhận trao đổi đóng góp) trong phần `TradingPreferences` của bạn, ASF cũng sẽ chấp nhận mọi trao đổi đóng góp (dạng trao đổi với việc tài khoản bot không mất gì) bất kỳ. Thuộc tính này chỉ ảnh hưởng những tài khoản không phải bot, vì các tài khoản bot đều bị ảnh hưởng bởi thuộc tính `DontAcceptBotTrades`. `AcceptDonations` cho phép bạn dễ dàng chấp nhận đóng góp từ những người khác, và cả những bot đang không tham gia vào quá trình ASF.

Cũng phải lưu ý rằng `AcceptDonations` không yêu cầu **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** (Xác nhận 2 bước bằng ASF), vì không cần thiết có xác nhận nếu chúng ta không mất đi vật phẩm nào.

Bạn cũng có thể tùy chỉnh thêm về khả năng trao đổi của ASF bằng cách thay đổi thông số `TradingPreferences` theo ý muốn. Một trong những chức năng chính của `TradingPreferences` là tùy chọn `SteamTradeMatcher` khiến cho ASF sử dụng logic có sẵn để chấp nhận các yêu cầu trao đổi giúp bạn hoàn thành các huy hiệu đang thiếu, rất hữu dụng khi dùng chung với danh sách lệnh mua bán công khai của **[SteamTradeMatcher](https://www.steamtradematcher.com)**, nhưng cũng có thể hoạt động nếu không có nó. Nó sẽ được giải thích thêm ở dưới.

---

## `SteamTradeMatcher`

Khi `SteamTradeMatcher` được kích hoạt, ASF sẽ dùng một thuật toán khá phức tạp để kiểm tra nếu yêu cầu trao đổi tuân thủ các quy tắc của STM và ít nhất có sự cân bằng với chúng ta hay không. Logic cụ thể ở đây là:

- Từ chối yêu cầu trao đổi nếu chúng ta mất vật phẩm nào đó không được được liệt kê trong danh sách `MatchableTypes`.
- Từ chối yêu cầu trao đổi nếu chúng ta không nhận ít nhất cùng số lượng vật phẩm căn cứ theo trò chơi, theo loại vật phẩm và theo độ hiếm.
- Từ chối yêu cầu trao đổi nếu người dùng yêu cầu thẻ trao đổi Steam thuộc sự kiện Ưu đãi mùa Hè/Đông, và đang có một yêu cầu trao đổi khác đang chờ.
- Từ chối yêu cầu trao đổi nếu số lượng yêu cầu trao đổi đang chờ vượt mức thuộc tính tùy chỉnh chung `MaxTradeHoldDuration` (Thời lượng yêu cầu trao đổi đang chờ tối đa).
- Từ chối yêu cầu trao đổi nếu chúng ta không đặt tùy chỉnh `MatchEverything` (chấp nhận mọi giao dịch trùng khớp), và khi yêu cầu xấu hơn tiêu chí cân bằng cho chúng ta.
- Chấp nhận yêu cầu trao đổi nếu chúng ta không từ chối nó theo những yêu cầu ở trên.

Thật tốt để lưu ý là ASF cũng hỗ trợ việc trao đổi vượt mức - logic vẫn sẽ hoạt động đúng cách khi người dùng thêm nhiều vật phẩm hơn trong yêu cầu trao đổi, miễn là vẫn đáp ứng các yêu cầu ở trên.

4 tiêu chí từ chối đầu tiên chắc cũng đã rõ nghĩa cho mọi người rồi. Tiêu chí cuối cùng bao gồm logic trùng lặp thực tế với việc kiểm tra tình trạng hiện tại của kho đồ và quyết định trạng thái của yêu cầu trao đổi là gì.

- Yêu cầu trao đổi được xét là **tốt** khi tiến trình tiến tới hoàn thành bộ thẻ được tăng lên. Ví dụ: A A (trước khi đổi) -> A B (sau khi đổi)
- Yêu cầu trao đổi được xét là **cân bằng** khi tiến trình tiến tới hoàn thành bộ thẻ không bị thay đổi. Ví dụ: A B (trước khi đổi) -> A C (sau khi đổi)
- Yêu cầu trao đổi được xét là **xấu** khi tiến trình tiến tới hoàn thành bộ thẻ bị giảm đi. Ví dụ: A C (trước khi đổi) -> A A (sau khi đổi)

STM chỉ xử lý các yêu cầu trao đổi tốt, tức nghĩa là các người dùng sử dụng STM để khớp thẻ trùng lặp chỉ có thể đề xuất các yêu cầu trao đổi tốt cho chúng ta. Tuy nhiên ASF có sự khoan dung hơn, và nó cũng sẽ chấp nhận các yêu cầu trao đổi cân bằng, bởi vì trong đó chúng ta không thật sự mất gì, nên không có lý do thực tế nào để từ chối chúng. Điều này rất hữu dụng cho những người bạn bè của bạn, bởi vì họ có thể hoán đổi các thẻ dư thừa của bạn mà không cần sử dụng STM, miễn là bạn không bị mất tiến trình nào cả.

Theo mặc định ASF sẽ từ chối các trao đổi xấu - là điều mà bạn là người dùng gần như luôn luôn muốn. Nhưng mà bạn cũng có thể tự kích hoạt tùy chỉnh `MatchEverything` trong phần `TradingPreferences` của bạn để khiến ASF chấp nhận mọi yêu cầu trao đổi trùng lặp, ngay cả các **trao đổi loại xấu**. Việc này chỉ hữu dụng nếu bạn muốn chạy một bot trao đổi 1:1 trong tài khoản của bạn, vì bạn hiểu rằng **ASF sẽ không giúp đỡ bạn tiến triển việc hoàn thành huy hiệu nữa, và khiến bạn có thể mất hoàn toàn bộ thẻ hoàn thiện vì số lượng N thẻ trùng lặp của một thẻ nào đó**. Nếu bạn muốn cố tình chạy bot trao đổi **không bao giờ** có ý định hoàn thành bộ thẻ nào, và sẵn sàng cung cấp kho đồ của nó cho mọi người dùng có hứng thú với việc trao đổi, thì bạn có thể mở tùy chọn đó.

Dù bất kể tùy theo `TradingPreferences` mà bạn đã đặt như thế nào, thì yêu cầu trao đổi bị ASF từ chối không có nghĩa là bạn không thể tự chấp nhận nó. Nếu bạn để giá trị `BotBehaviour` theo mặc định, với việc không bao gồm `RejectInvalidTrades` (Từ chối yêu cầu trao đổi không giá trị), ASF sẽ chỉ bỏ qua những yêu cầu trao đổi này - cho phép bạn quyết định xem nếu bạn có hứng thú với chúng không. Tương tự với các yêu cầu trao đổi với vật phẩm ngoài danh sách `MatchableTypes`, và cũng như là mọi thứ khác - môđun này chỉ được dùng để giúp bạn tự động hóa các yêu cầu trao đổi STM, chứ không quyết định được yêu cầu trao đổi nào là tốt và không tốt với bạn. Ngoại lệ duy nhất khỏi quy luật này là khi nói về các người dùng mà bạn đã cho vào danh sách đen của môđun trao đổi bằng cách dùng lệnh `tbadd` - yêu cầu trao đổi từ những người dùng đó sẽ bị từ chối tức thì dù bạn đặt tùy chỉnh `BotBehaviour` ra sao đi chăng nữa.

Chúng tôi khuyến khích sử dụng **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** khi bạn mở tùy chọn này, vì chức năng này sẽ mất đi khả năng của nó nếu bạn quyết định xác nhận tất cả yêu cầu trao đổi một cách thủ công. `SteamTradeMatcher` sẽ hoạt động bình thường dù không có khả năng xác nhận yêu cầu trao đổi, nhưng nó có thể tạo ra rất nhiều yêu cầu xác nhận tồn đọng nếu bạn không chấp nhận chúng kịp lúc.
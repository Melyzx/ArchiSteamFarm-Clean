# Lệnh khởi chạy

ASF bao gồm hỗ trợ cho một số đối số dòng lệnh có thể ảnh hưởng đến runtime của chương trình. Những người dùng nâng cao có thể sử dụng chúng để chỉ định cách chạy chương trình. So với cách mặc định dùng tệp cấu hình `ASF.json`, các đối số dòng lệnh được sử dụng để khởi tạo phần chính (ví dụ: `--path`), cài đặt dành riêng cho nền tảng (ví dụ: <0 >--system-required</code>) hoặc dữ liệu nhạy cảm (ví dụ: `--cryptkey`).

---

## Mức sử dụng

Cách sử dụng tuỳ thuộc vào hệ điều hành và thị hiếu ASF của bạn.

Chung:

```shell
dotnet ArchiSteamFarm.dll --argument --otherOne
```

Windows:

```powershell
.\ArchiSteamFarm.exe --argument --otherOne
```

Linux/macOS:

```shell
./ArchiSteamFarm --argument --otherOne
```

Đối số dòng lệnh cũng được hỗ trợ trong tập lệnh trợ giúp thông thường chẳng hạn như `ArchiSteamFarm.cmd` hoặc `ArchiSteamFarm.sh`. Ngoài ra, bạn cũng có thể sử dụng thuộc tính môi trường `ASF_ARGS`, như đã nêu trong các phần **[quản lý](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#environment-variables)** và **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker#command-line-arguments)** của chúng tôi.

Nếu đối số của bạn bao gồm khoảng trắng, đừng quên đặt nó trong ngoặc kép. Hai cái đó sai:

```shell
./ArchiSteamFarm --path /home/archi/Tải xuống của Tôi/ASF # Tồi!
./ArchiSteamFarm --path=/home/archi/Tải xuống của Tôi/ASF # Tồi!
```

Tuy nhiên, hai cái đó hoàn toàn ổn:

```shell
./ArchiSteamFarm --path "/home/archi/Tải xuống của Tôi/ASF" # OK
./ArchiSteamFarm "--path=/home/archi/Tải xuống của Tôi/ASF" # OK
```

## Đối số

`--cryptkey <key>` hoặc `--cryptkey=<key>` - sẽ khởi động ASF bằng khoá mật mã tuỳ chỉnh có giá trị `<key>`. Tuỳ chọn này ảnh hưởng đến **[bảo mật](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** và sẽ khiến ASF sử dụng khoá `<key>` tùy chỉnh do bạn cung cấp thay vì khoả mặc định được code cứng trong tệp thực thi. Vì thuộc tính này ảnh hưởng đến khoá mã hoá mặc định (cho mục đích mã hoá) cũng như salt (cho mục đích băm), hãy nhớ rằng mọi thứ được mã hoá/băm bằng khoá này sẽ yêu cầu nó được truyền qua mỗi lần chạy ASF.

Không có yêu cầu về độ dài hay ký tự của `<key>`, nhưng vì lý do bảo mật, chúng tôi khuyên bạn chọn một mật khẩu dài với 32 ký tự ngẫu nhiên, chẳng hạn như sử dụng lệnh  `tr -dc A-Za-z0-9 < /dev/urandom | head -c 32; echo` trên Linux.

Thật tuyệt khi đề cập rằng cũng có hai cách khác để cung cấp thông tin chi tiết này: `--cryptkey-file` và `--input-cryptkey`.

Do tính chất của thuộc tính này, bạn cũng có thể đặt khoá mã hoá bằng cách khai báo biến môi trường `ASF_CRYPTKEY`, điều này có thể phù hợp hơn cho những người muốn tránh các khía cạnh nhạy cảm trong các đối số của tiến trình.

---

`--cryptkey-file <path>` hoặc `--cryptkey-file=<path>` - sẽ khởi động ASF bằng khoá mật mã tuỳ chỉnh được đọc từ tập tin `<path>`. Điều này phục vụ cùng một mục đích như `--cryptkey <key>` đã giải thích ở trên, chỉ có cơ chế là khác, vì thuộc tính này sẽ đọc `<key>` từ `<path>` được cung cấp để thay thế. Nếu bạn sử dụng cái này cùng với `--path`, hãy xem kĩ lại vì đường dẫn tương đối sẽ khác nhau tùy thuộc vào thứ tự của các đối số, ví dụ như bạn thay đổi vị trí của `--path` trước hay sau `--cryptkey-file`.

Do tính chất của thuộc tính này, bạn cũng có thể đặt tệp khoá mã hoá bằng cách khai báo biến môi trường `ASF_CRYPTKEY_FILE`, điều này có thể phù hợp hơn cho những người muốn tránh các khía cạnh nhạy cảm trong các đối số của tiến trình.

---

`--ignore-unsupported-environment` - sẽ khiến ASF bỏ qua các sự cố liên quan đến việc chạy trong môi trường không được hỗ trợ, thường được báo hiệu bằng lỗi và buộc phải thoát. Các môi trường không được hỗ trợ bao gồm như chạy bản dựng của `win-x64` trên `linux-x64`. Mặc dù cờ này sẽ cho phép ASF thử chạy trong các tình huống như vậy, nhưng xin lưu ý rằng chúng tôi không hỗ trợ chính thức những trường hợp đó và bạn đang buộc ASF thực hiện điều đó hoàn toàn **tự chịu rủi ro**. Quan trọng nữa là **tất cả** các kịch bản môi trường không được hỗ trợ **đều có thể sửa được**. Chúng tôi thực sự khuyên bạn nên khắc phục các sự cố còn tồn tại thay vì sử dụng đối số này.

---

`--input-cryptkey` - sẽ khiến ASF hỏi về `--cryptkey` trong khi khởi động. Tuỳ chọn này có thể hữu ích cho bạn nếu thay vì cung cấp khoá mật mã, dù cho trong biến môi trường hay tệp, bạn lại không muốn lưu nó ở bất kỳ đâu và thay vào đó nhập nó theo cách thủ công trong mỗi lần chạy ASF.

---

`--minimized` - sẽ cực tiểu hoá cửa sổ giao diện dòng lệnh của ASF ngay sau khi khởi động. Hữu ích chủ yếu trong các tình huống tự khởi động, nhưng cũng có thể được sử dụng bên ngoài các tình huống đó. Tùy chọn này cần phải có hỗ trợ môi trường phù hợp - nó có thể không hoạt động đúng cách trong tất cả các kịch bản có thể xảy ra.

---

`--network-group <group>` hoặc `--network-group=<group>` - sẽ khiến ASF khởi tạo các bộ giới hạn của nó với nhóm mạng tùy chỉnh có giá trị là `<group>`. Tùy chọn này sẽ ảnh hưởng đến việc chạy ASF trong **[nhiều phiên cùng lúc](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#multiple-instances)** bằng cách chỉ ra rằng phiên bản đã cho chỉ phụ thuộc vào các phiên bản chia sẻ cùng một nhóm mạng và sẽ độc lập so với các phần còn lại. Thông thường thì bạn chỉ nên sử dụng thuộc tính này nếu bạn cần định tuyến các yêu cầu của ASF qua cơ chế tùy chỉnh (ví dụ: địa chỉ IP khác nhau) và bạn muốn tự thiết lập các nhóm mạng mà không để ASF tự động chỉnh (hiện tại chỉ tính đến `WebProxy` thôi). Hãy nhớ rằng khi sử dụng một nhóm mạng tùy chỉnh, đây là mã định danh duy nhất trong máy cục bộ, và ASF sẽ không tính đến bất kỳ chi tiết nào khác. Chẳng hạn như giá trị `WebProxy`, nó cho phép bạn ví dụ như khởi động hai phiên với các giá trị `WebProxy` khác nhau nhưng vẫn phụ thuộc vào nhau.

Do tính chất của thuộc tính này, ta cũng có thể thiết lập giá trị bằng cách khai báo biến môi trường `ASF_NETWORK_GROUP`, cách này có thể sẽ phù hợp hơn với những người muốn tránh phải thấy thông tin nhạy cảm trong các đối số quá trình.

---

`--no-config-migrate` - theo mặc định, ASF sẽ tự động di chuyển các tệp cấu hình của bạn sang cú pháp mới nhất. Việc di chuyển bao gồm chuyển đổi các thuộc tính không còn được hỗ trợ sang các thuộc tính mới nhất, loại bỏ đi các thuộc tính có giá trị mặc định (vì chúng không còn tác dụng), cũng như dọn dẹp tệp nói chung (như là chỉnh sửa thụt lề và các nội dung tương tự). Hầu hết thì cách này luôn là một ý tưởng tốt, nhưng bạn có thể gặp một trường hợp cụ thể nào đó mà bạn muốn ASF không bao giờ tự động ghi đè các tệp cấu hình. Ví dụ, bạn muốn sử dụng lệnh `chmod 400` cho các tệp cấu hình của mình (chỉ cấp quyền đọc cho chủ sở hữu) hoặc áp dụng lệnh `chattr +i` để ngăn chặn quyền ghi cho tất cả mọi người như là một biện pháp bảo mật. Thông thường thì chúng tôi khuyên bạn để việc di chuyển cấu hình được bật, nhưng nếu bạn một có lý do nào đó để tắt nó và muốn ASF không thực hiện điều đó, bạn có thể sử dụng tùy chọn này để đạt được mục đích này. Tuy nhiên, bạn hãy nhớ rằng việc phải cung cấp các thiết lập chính xác cho ASF sẽ trở thành trách nhiệm mới của bạn, đặc biệt là liên quan đến việc loại bỏ và tái cấu trúc các thuộc tính không còn được sử dụng trong các phiên bản ASF trong tương lai.

---

`--no-config-watch` - theo mặc định, ASF sẽ thiết lập `FileSystemWatcher` trên thư mục `config` của bạn để lắng nghe các sự kiện liên quan đến việc thay đổi tệp, giúp nó có thể tương tác và điều chỉnh theo cách linh hoạt. Ví dụ, cái này bao gồm việc dừng bot khi tệp cấu hình bị xóa, khởi động lại bot khi tệp cấu hình bị được thay đổi, hoặc tải key vô **[BGR](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)** khi bạn thả chúng vào thư mục `config`. Tùy chọn này cho phép bạn tắt hành vi đó, khiến ASF hoàn toàn không để ý tới mọi thay đổi xảy ra trong thư mục `config`, yêu cầu bạn thực hiện các hành động như vậy một cách thủ công khi cần thiết (thường thì chỉ là khởi động lại quá trình). Chúng tôi khuyên bạn để cho các sự kiện cấu hình được bật, nhưng nếu bạn có lý do để tắt chúng và muốn ASF không thực hiện điều đó, bạn có thể sử dụng tùy chọn này.

---

`--no-restart` - tùy chọn này chủ yếu được sử dụng bởi các container **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker)** của ASF và nó cũng buộc `AutoRestart` thành `false`. Trừ khi bạn thực sự cần thiết, bạn nên tùy chỉnh `AutoRestart` thẳng trong tệp tùy chỉnh. Tùy chọn này chỉ có ở đây để kịch bản docker của chúng tôi không phải chỉnh sửa cấu hình toàn cục của bạn để thích ứng với môi trường của nó. Tất nhiên, nếu bạn đang chạy ASF bên trong một kịch bản, bạn cũng có thể sử dụng tùy chọn này (nếu không, bạn nên sử dụng thuộc tính cấu hình toàn cục).

---

`--no-steam-parental-generation` - theo mặc định thì ASF sẽ tự động tạo mã PIN Steam parental, như mô tả trong thuộc tính cấu hình **[`SteamParentalCode`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#steamparentalcode)**. Tuy nhiên, vì điều này có thể yêu cầu một lượng tài nguyên hệ điều hành lớn, tùy chọn này cho phép bạn tắt hành vi đó, dẫn đến việc ASF bỏ qua quá trình tự động tạo mã PIN và sẽ yêu cầu người dùng nhập mã PIN thay vào đó, điều này thường chỉ xảy ra nếu việc tạo mã PIN tự động thất bại. Chúng tôi khuyên bạn để nó bật, nhưng nếu bạn có lý do và cần phải tắt nó thì bạn có thể sử dụng tùy chọn này.

---

`--path <path>` or `--path=<path>` - ASF always navigates to its own directory on startup. By specifying this argument, ASF will navigate to given directory after initialization, which allows you to use custom path for various application parts (including `config`, `logs`, `plugins` and `www` directories, as well as `NLog.config` file), without a need of duplicating binary in the same place. It may come especially useful if you'd like to separate binary from actual config, as it's done in Linux-like packaging - this way you can use one (up-to-date) binary with several different setups. The path can be either relative according to current place of ASF binary, or absolute. Keep in mind that this command points to new "ASF home" - the directory that has the same structure as original ASF, with `config` directory inside, see below example for explanation.

Due to the nature of this property, it's also possible to set expected path by declaring `ASF_PATH` environment variable, which may be more appropriate for people that would want to avoid sensitive details in the process arguments.

If you're considering using this command-line argument for running multiple instances of ASF, we recommend reading our **[management page](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#multiple-instances)** on this manner.

Ví dụ:

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/TargetDirectory # Absolute path
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../TargetDirectory # Relative path works as well
ASF_PATH=/opt/TargetDirectory dotnet /opt/ASF/ArchiSteamFarm.dll # Same as env variable
```

```text
├── 📁 /opt
│     ├── 📁 ASF
│     │     ├── ⚙️ ArchiSteamFarm.dll
│     │     └── ...
│     └── 📁 TargetDirectory
│           ├── 📁 config
│           ├── 📁 logs (generated)
│           ├── 📁 plugins (optional)
│           ├── 📁 www (optional)
│           ├── 📄 log.txt (generated)
│           └── 📄 NLog.config (optional)
└── ...
```

---

`--service` - this switch is mainly used by our `systemd` service and forces `Headless` of `true`. Unless you have a particular need, you should instead configure `Headless` property directly in your config. This switch is here so our `systemd` service won't need to touch your global config in order to adapt it to its own environment. Of course, if you have a similar need then you may also make use of this switch (otherwise you're better with global config property).

---

`--system-required` - declaring this switch will cause ASF to try signalizing the OS that the process requires system to be up and running for its entire lifetime. Currently this switch has effect only on Windows machines where it'll forbid your system from going into sleep mode as long as the process is running. This can be proven especially useful when farming on your PC or laptop during night, as ASF will be able to keep your system awake while it's running.
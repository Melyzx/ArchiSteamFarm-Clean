# Двухфакторная аутентификация

Steam включает в себя систему двухфакторной аутентификации, которая требует дополнительных данных для различных действий, связанных с учетной записью. Вы можете прочитать об этом подробнее **[тут](https://help.steampowered.com/faqs/view/2E6E-A02C-5581-8904)** и **[тут](https://help.steampowered.com/faqs/view/34A1-EA3F-83ED-54AB)**. На этой странице рассматривается система 2FA, а также наше решение, которое интегрируется с ней, называемое ASF 2FA.

---

# Логика работы ASF

Независимо от того, используете ли вы ASF 2FA или нет, ASF включает в себя всю нужную логику и способен определять учетные записи, защищенные 2FA в Steam. У вас будут запрашиваться необходимые данные, когда они нужны (как например при входе). While you can manually provide that information, certain ASF functionalities (such as `MatchActively`) require ASF 2FA to be operative on your bot account, which can automatically respond to 2FA prompts, automatically, whenever required by ASF.

---

# ASF 2FA

ASF 2FA это встроенный модуль, отвечающий за реализацию функций 2ФА для ASF, таких, как генерация кодов и принятие подтверждений. It can work either in standalone mode, or by duplicating your existing authenticator details (so that you can use your current authenticator and ASF 2FA at the same time).

Вы можете проверить, используется ли на боте ASF 2FA путём исполнения **[команд](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-ru-RU)** `2fa`. Without setting up ASF 2FA, all standard `2fa` commands will be non-operative, which means that your bot is unavailable for advanced ASF features that require the module to be operative.

---

# Рекомендации

Существует множество способов обеспечить работоспособность ASF 2FA, здесь мы приводим наши рекомендации, исходя из вашей текущей ситуации:

- Если вы уже используете неофициальное стороннее приложение, позволяющее с легкостью извлекать данные 2FA, просто **[импортируйте](#import)** их в ASF.
- Если вы используете официальное приложение и не против сбросить учетные данные 2FA, то лучше всего отключить 2FA, затем **[создать](#creation)**новые учетные данные 2FA с помощью **[совместного аутентификатора](#joint-authenticator)**, что позволит вам использовать официальное приложение и ASF 2FA. This method doesn't require root or advanced knowledge, barely following instructions written here, and is arguably superior for this scenario.
- Если вы используете официальное приложение и не хотите заново создавать свои учетные данные 2FA, то ваши возможности очень ограничены: обычно для **[импорта](#import)** этих данных требуется root и дополнительные действия, и даже это может оказаться невозможным.
- If you're not using 2FA yet and don't care, we recommend you to use ASF 2FA with **[standalone authenticator](#standalone-authenticator)** or **[joint authenticator](#joint-authenticator)** with official app (same as above).

Ниже мы рассмотрим все возможные варианты и известные нам способы.

---

## Создание

ASF поставляется с официальным `плагином` **[MobileAuthenticator](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)**, который расширяет ASF 2FA и позволяет привязать абсолютно новый аутентификатор. Это может быть полезно в том случае, если вы не можете или не хотите использовать другие инструменты и не против, чтобы ASF 2FA стал вашим основным (а может быть, и единственным) аутентификатором. Creation process is also used in joint-authenticator method, naturally in this scenario your authenticator can co-exist in two places at once - both will generate the same codes and both will be able to confirm the same confirmations.

### Общие действия для обоих сценариев

No matter if you plan to use ASF as the standalone or joint authenticator, you need to do those initialization steps:

1. Создайте бота ASF для нужного аккаунта, запустите его и войдите в систему, что вы, вероятно, уже сделали.
2. Assign a working and operational phone number to the account **[here](https://store.steampowered.com/phone/manage)** to be used by the bot. This will allow you to receive SMS code and allow recovery if needed. Этот шаг не всегда обязателен, но мы рекомендуем его, если вы не уверены в своих действиях.
3. Убедитесь, что вы еще не используете 2FA для своей учетной записи, а если используете, то сначала отключите ее. This **will** put your account on temporary trade-hold, there is no way around it, only **[import](#import)** process can skip it.
4. Выполните команду `2fainit [Bot]`, заменив `[Bot]` именем вашего бота.

Если вы получили успешный ответ, то произошли две следующие вещи:

- Новый файл `<Bot>.maFile.PENDING` был сгенерирован ASF в директории `config`.
- Из Steam было отправлено SMS на номер телефона, который вы указали выше. Если вы не указали номер телефона, то письмо было отправлено на адрес электронной почты аккаунта.

Данные аутентификатора пока не используются, однако при желании можно просмотреть сгенерированный файл. Если вы хотите обезопасить себя вдвойне, то можете, например, уже сейчас записать код восстановления. Дальнейшие действия будут зависеть от выбранного сценария.

### Автономный аутентификатор

Если вы хотите использовать ASF как основной (или даже единственный) аутентификатор, то вам нужно сделать окончательный шаг:

5. Выполните команду `2fafinalize [Bot] <ActivationCode>`, заменив `[Bot]` именем вашего бота и `<ActivationCode>` кодом, который вы получили через SMS или почту в предыдущем шаге.

### Joint authenticator

Если вы хотите использовать тот же аутентификатор в ASF и официальном мобильном приложении Steam, вам нужно выполнить следующие, более сложные шаги:

5. Проигнорируйте код из SMS или с почты, который вы получили на предыдущем шаге.
6. Установите мобильное приложение Steam, если оно еще не установлено, и откройте его. Перейдите на вкладку Steam Guard и добавьте новый аутентификатор, следуя инструкциям приложения.
7. После того как вы добавили аутентификатор в мобильное приложение и он заработал, вернитесь в ASF. Now, instead of finalization, we only need to inform ASF that mobile app already activated our previously-generated details:
 - Wait until the next 2FA code is shown in the Steam mobile app, and use the command `2fafinalized [Bot] <2FACodeFromApp>` replacing `[Bot]` with your bot's name and `<2FACodeFromApp>` with the code you currently see in the Steam mobile app. Если код, сгенерированный ASF, совпадает с указанным вами, ASF сочтёт аутентификатор добавленным корректно и продолжит импорт только что созданного аутентификатора.
 - We strongly recommend to do the above in order to ensure that your credentials are valid. However, if you don't want to (or can't) check if codes are the same and you know what you're doing, you can instead use the command `2fafinalizedforce [Bot]`, replacing `[Bot]` with your bot's name. ASF will assume that the authenticator was added correctly and proceed with importing your newly created authenticator. Be aware that in this mode ASF is unable to verify if the codes match, which means that you can potentially import invalid (not activated) credentials.

### After finalization

Если все работало правильно, ранее сгенерированный `<Bot>.maFile.PENDING` файл должен быть переименован в `<Bot>.maFile.NEW`. Это означает, что учетные данные 2ФА действительны и активны. We recommend that you move that file to **a secure and safe location**. In addition to that, if you've decided to use standalone authenticator, then we recommend you to open the file in your editor of choice and write down the `revocation_code`, which will allow you to, as the name implies, revoke the authenticator in case you lose it. In joint-authenticator method, you should've already done that in Steam mobile app, but feel free to do the same in case you need to.

In regards to technical details, the generated `maFile` includes all details that we've received from the Steam server during linking the authenticator, and in addition to that, the `device_id` field, which may be needed for other (third-party) authenticators, if you ever decide to import that `maFile` into them.

ASF automatically imports your authenticator once the procedure is done, and therefore `2fa` and other related commands should now be operational for the bot account you linked the authenticator to. Мы рекомендуем вам подтвердить это.

---

## Импорт

Процесс импорта требует привязанного и рабочего аутентификатора, который поддерживается ASF. We have instructions for a few different official and unofficial sources of 2FA, on top of manual method which allows you to provide required credentials yourself. Please note that those instructions should be used **only** if you're already using given solution - since process here involves third-party apps and tools, **we do not recommend using them**, and we're mentioning it exclusively for people that already decided to use them and would like to import generated details into ASF 2FA.

Все последующие руководства требуют чтобы у вас уже был **настроенный и работающий** аутентификатор в одном из заданных приложений. 2ФА ASF не будет работать если вы импортируете неверные данные, поэтому убедитесь что аутентификатор правильно работает перед тем как его импортировать. Это включает в себя тестирование и проверку того, что следующие функции аутентифиатора работают правильно:
- You can generate tokens and those tokens are accepted by Steam network (you can log in with them)
- Вы можете получить список операций, требующих подтверждения, и они отображаются в вашем мобильном аутентификаторе
- You can react to those confirmations, and they're properly recognized by Steam network as confirmed/rejected

Ensure that your authenticator works by checking if above actions work - if they don't, then they won't work in ASF either.

---

### Android-смартфон

In general for importing authenticator from your Android phone you will need **[root](https://en.wikipedia.org/wiki/Rooting_(Android_OS))** access. The below instructions require from you fairly decent knowledge in Android modding world, we're definitely not going to explain every step here, visit **[XDA](https://xdaforums.com)** and other resources for additional information/help with below.

Assuming you have official **[Steam app](https://play.google.com/store/apps/details?id=com.valvesoftware.android.steam.community)** working and operational (requires rooting your device):

- Install **[Magisk](https://topjohnwu.github.io/Magisk/install.html)** and enable Zygisk in the settings.
- Install **[LSPosed](https://github.com/LSPosed/LSPosed?tab=readme-ov-file#install)** (for Zygisk) and ensure it works.
- Install **[SteamGuardExtractor](https://github.com/hax0r31337/SteamGuardExtractor?tab=readme-ov-file#usage)** LSPosed module and enable it in LSPosed settings.
- Force kill Steam app, then open it, a **[window with extracted details](https://github.com/JustArchiNET/ArchiSteamFarm/assets/1069029/ab5ab71e-d664-4e49-9eb4-9f4d9ba32aa2)** should pop up, click copy.

Now that you've successfully extracted required details, disable the module to prevent the window from showing each time, then copy value of `shared_secret` and `identity_secret` of the account that you intend to add to ASF 2FA, into a new text file with below structure:

```json
{
  "shared_secret": "STRING",
  "identity_secret": "STRING"
}
```

Replace each `STRING` value with appropriate private key from extracted details. Once you do that, rename the file to `BotName.maFile`, where `BotName` is the name of your bot you're adding ASF 2FA to, and put it in ASF's `config` directory if you haven't yet.

Запустите ASF, он обнаружит ваш файл и импортирует его. Если вы импортировали правильный файл с корректными данными, всё должно работать правильно, вы можете это проверить использовав команды `2fa`. Если вы где-то ошиблись, вы всегда можете удалить `Bot.db` и начать всё с начала.

---

### SteamDesktopAuthenticator

Если у вас уже есть аутентифицатор, работающий в SDA, вы можете заметить что у вас уже есть файл `steamID.maFile` в папке `maFiles`. Убедитесь, что этот `.maFile` находится в незашифрованном виде, поскольку ASF не умеет расшифровывать файлы SDA: незашифрованный файл должен начинаться с символа `{` и заканчиваться на  `}`. При необходимости вы можете сначала удалить шифрование из настроек SDA и включить его снова, когда закончите. Как только файл будет в расшифрованном виде, скопируйте его в папку `config` ASF.

Теперь вы можете переименовать `steamID.maFile` в `BotName.maFile` в папке config ASF, где `BotName` - это имя вашего бота, в который вы добавляете ASF 2FA. Вы можете оставить всё как есть, ASF обнаружит этот файл автоматически после входа в Steam. Переименование файла помогает ASF, сделав возможным использование ASF 2FA перед входом, если вы этого не сделаете, затем файл может быть выбран только после успешного входа ASF (так как ASF не знает `steamID` вашей учетной записи до фактического входа).

Запустите ASF, он обнаружит ваш файл и импортирует его. Если вы импортировали правильный файл с корректными данными, всё должно работать правильно, вы можете это проверить использовав команды `2fa`. Если вы где-то ошиблись, вы всегда можете удалить `Bot.db` и начать всё с начала.

---

### WinAuth

Сначала создайте новый пустой файл `BotName.maFile` в папке `config` вашего ASF, где `BotName` это имя вашего бота, для которого вы хотите добавить 2ФА ASF. Если имя файла будет неверным, ASF не обнаружит его.

Теперь запустите WinAuth как обычно. Кликните правой кнопкой мыши на иконке Steam и выберите "Show SteamGuard and Recovery Code". Поставьте отметку в поле "Allow copy". Вы должны увидеть уже знакомую вам JSON-структуру в нижней части окна, начинающуюся с `{`. Скопируйте весь текст в файл `BotName.maFile`, созданный на предыдущем этапе.

Запустите ASF, он обнаружит ваш файл и импортирует его. Если вы импортировали правильный файл с корректными данными, всё должно работать правильно, вы можете это проверить использовав команды `2fa`. Если вы где-то ошиблись, вы всегда можете удалить `Bot.db` и начать всё с начала.

---

### Ручной режим

Если вы продвинутый пользователь, вы можете также создать maFile вручную. Это может быть использовано, если вы хотите импортировать аутентификатор из иных источников чем те, что описаны выше. Он должен иметь **[корректную JSON структуру](https://jsonlint.com)**, состоящую из:

```json
{
  "shared_secret": "STRING",
  "identity_secret": "STRING"
}
```

В стандартных аутентификаторах имеется больше параметров - они полностью игнорируются ASF в процессе импорта, поскольку нам они не нужны. You don't need to remove them - ASF only requires valid JSON with 2 mandatory fields described above, and will ignore additional fields (if any). Разумеется, вам нужно заменить `STRING` в этом примере на корректные для вашего аккаунта значения. Каждая `STRING` должна быть base64-кодированным представлением байт, из которой сделан соответствующий приватный ключ.

---

## ЧАВО

### Как ASF использует модуль 2ФА?

Если 2ФА ASF настроена, ASF будет автоматически подтверждать все обмены которые отправляет/получает ASF. Также будет возможно автоматически при необходимости создавать коды 2FA, например чтобы выполнить вход. В добавок к этому, наличие 2ФА ASF делает доступными команды `2fa` для вашего удобства.

---

### Как получить 2FA токен?

Вам нужен код 2ФА для любого аккаунта, защищённого 2ФА, это также включает в себя каждый аккаунт с 2ФА ASF. If you've decided to use standalone authenticator, then you should use `2fa <BotNames>` command to generate temporary token for given bot instances. In all other scenarios, we recommend to use original authenticator that you've used, although you can use the command as well if it's more convenient to you.

---

### Могу ли я пользоваться своим обычным аутентификатором после импорта в 2ФА ASF?

Да, ваш аутентификатор останется полностью работоспособным и вы можете использовать его одновременно с 2ФА ASF. Keep in mind however that if you invalidate it through any method, then linked ASF 2FA credentials will also no longer be valid.

---

### Как удалить ASF 2FA?

Просто остановите ASF и удалите файл `BotName.db` соответствующий боту 2ФА ASF которого вы хотите удалить. This option will remove associated imported 2FA with ASF, but will NOT invalidate (unlink) your authenticator. If you instead want to invalidate your authenticator, apart from removing it from ASF (firstly), you should unlink it in original authenticator of your choice. If you can't do that for some reason, for example because you're using ASF 2FA in standalone mode, then use revocation code that you've received during setup, on the Steam website. It's not possible to invalidate your authenticator through ASF.

---

### I linked authenticator in third-party app, then imported to ASF. Can I now link it again on my phone?

**Нет**. Doing so will invalidate the previously imported credentials and your ASF 2FA will stop functioning (by generating codes no longer being accepted by Steam). Firstly decide where you want to have your original or third-party authenticator located, then import it as ASF 2FA.

---

### Is using ASF 2FA better than third-party authenticator set to accept all confirmations?

**Да**, по ряду причин. Первое, и самое главное - использование 2ФА ASF **значительно** увеличивает безопасность, поскольку модуль 2ФА в ASF будет следить чтобы ASF подтверждал автоматически только собственные обмены, так что даже если атакующий попытается запросить обмен, который вреден для вас, 2ФА ASF **не** примет такой обмен, поскольку он не был создан с помощью ASF. In addition to security part, using ASF 2FA also brings performance/optimization benefits, as ASF 2FA fetches and accepts confirmations immediately after they're generated, and only then, as opposed to inefficient polling for confirmations each X minutes which is achieved by other solutions. There is no reason to use third-party authenticator over ASF 2FA, if you plan on automating confirmations generated by ASF - that's exactly what ASF 2FA is for, and using it does not conflict with you confirming everything else in authenticator of your choice. We strongly recommend to use ASF 2FA for entire ASF activity.
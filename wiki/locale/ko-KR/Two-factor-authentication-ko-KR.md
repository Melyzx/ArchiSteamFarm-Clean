# 2단계 인증

Steam includes two-factor authentication system that requires extra details for various account-related activity. 자세한 내용은 **[여기](https://help.steampowered.com/faqs/view/2E6E-A02C-5581-8904)** 와 **[여기](https://help.steampowered.com/faqs/view/34A1-EA3F-83ED-54AB)** 에 있습니다. This page considers that 2FA system as well as our solution that integrates with it, called ASF 2FA.

---

# ASF 논리 구조

Regardless if you use ASF 2FA or not, ASF includes proper logic and is fully aware of accounts protected by 2FA on Steam. 예를 들어 로그인 중에도 필요하면 세부사항을 입력하도록 요청할 것입니다. While you can manually provide that information, certain ASF functionalities (such as `MatchActively`) require ASF 2FA to be operative on your bot account, which can automatically respond to 2FA prompts, automatically, whenever required by ASF.

---

# ASF 2단계 인증(2FA)

ASF 2FA is a built-in module responsible for providing 2FA features to the ASF process, such as generating tokens and accepting confirmations. It can work either in standalone mode, or by duplicating your existing authenticator details (so that you can use your current authenticator and ASF 2FA at the same time).

`2fa` **[명령어](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-ko-KR)** 를 실행하여 봇 계정이 이미 ASF 2단계 인증을 사용중인지 확인할 수 있습니다. Without setting up ASF 2FA, all standard `2fa` commands will be non-operative, which means that your bot is unavailable for advanced ASF features that require the module to be operative.

---

# Recommendations

There are a lot of ways to make ASF 2FA operative, here we include our recommendations based on your current situation:

- If you're already using unofficial third-party app that allows you to extract 2FA details with ease, just **[import](#import)** those to ASF.
- If you're using official app and you don't mind resetting your 2FA credentials, the best way is to disable 2FA, then **[create](#creation)** new 2FA credentials by using **[joint authenticator](#joint-authenticator)**, which will allow you to use official app and ASF 2FA. This method doesn't require root or advanced knowledge, barely following instructions written here, and is arguably superior for this scenario.
- If you're using official app and don't want to recreate your 2FA credentials, your options are very limited, typically you'll need root and extra fiddling around to **[import](#import)** those details, and even with that it might be impossible.
- If you're not using 2FA yet and don't care, we recommend you to use ASF 2FA with **[standalone authenticator](#standalone-authenticator)** or **[joint authenticator](#joint-authenticator)** with official app (same as above).

Below we discuss all possible options and known to us methods.

---

## Creation

ASF comes with an official `MobileAuthenticator` **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** that further extends ASF 2FA, allowing you to link a completely new 2FA authenticator. This can be useful in case you're unable or unwilling to use other tools and do not mind ASF 2FA becoming your main (and maybe only) authenticator. Creation process is also used in joint-authenticator method, naturally in this scenario your authenticator can co-exist in two places at once - both will generate the same codes and both will be able to confirm the same confirmations.

### Common steps for both scenarios

No matter if you plan to use ASF as the standalone or joint authenticator, you need to do those initialization steps:

1. Create an ASF bot for the target account, start it, and log in, which you probably already did.
2. Assign a working and operational phone number to the account **[here](https://store.steampowered.com/phone/manage)** to be used by the bot. This will allow you to receive SMS code and allow recovery if needed. This step is not mandatory in all scenarios, however, we recommend it unless you know what you're doing.
3. Ensure you're not using 2FA yet for your account, if you do, disable it first. This **will** put your account on temporary trade-hold, there is no way around it, only **[import](#import)** process can skip it.
4. Execute the `2fainit [Bot]` command, replacing `[Bot]` with your bot's name.

Assuming you got a successful reply, the following two things have happened:

- A new `<Bot>.maFile.PENDING` file was generated by ASF in your `config` directory.
- SMS was sent from Steam to the phone number you have assigned for the account above. If you didn't set a phone number, then an email was sent instead to the account e-mail address.

The authenticator details are not operational yet, however, you can review the generated file if you'd like to. If you want to be double safe, you can, for example, already write down the revocation code. The next steps will depend on your selected scenario.

### Standalone authenticator

If you want to use ASF as your main (or even only) authenticator, now you need to do the final finalization step:

5. Execute the `2fafinalize [Bot] <ActivationCode>` command, replacing `[Bot]` with your bot's name and `<ActivationCode>` with the code you've received through SMS or e-mail in the previous step.

### Joint authenticator

If you want to have the same authenticator in both ASF and the official Steam mobile app, now you need to do the next, more tricky steps:

5. Ignore the SMS or e-mail code that you've received in the previous step.
6. Install the Steam mobile app if it's not installed yet, and open it. Navigate to the Steam Guard tab and add a new authenticator by following the app's instructions.
7. After your authenticator in the mobile app is added and working, return to ASF. Now, instead of finalization, we only need to inform ASF that mobile app already activated our previously-generated details:
 - Wait until the next 2FA code is shown in the Steam mobile app, and use the command `2fafinalized [Bot] <2FACodeFromApp>` replacing `[Bot]` with your bot's name and `<2FACodeFromApp>` with the code you currently see in the Steam mobile app. If the code generated by ASF and the code you provided are equal, ASF will assume that an authenticator was added correctly and proceed with importing your newly created authenticator.
 - We strongly recommend to do the above in order to ensure that your credentials are valid. However, if you don't want to (or can't) check if codes are the same and you know what you're doing, you can instead use the command `2fafinalizedforce [Bot]`, replacing `[Bot]` with your bot's name. ASF will assume that the authenticator was added correctly and proceed with importing your newly created authenticator. Be aware that in this mode ASF is unable to verify if the codes match, which means that you can potentially import invalid (not activated) credentials.

### After finalization

Assuming everything worked properly, the previously generated `<Bot>.maFile.PENDING` file was renamed to `<Bot>.maFile.NEW`. This indicates that your 2FA credentials are now valid and active. We recommend that you move that file to **a secure and safe location**. In addition to that, if you've decided to use standalone authenticator, then we recommend you to open the file in your editor of choice and write down the `revocation_code`, which will allow you to, as the name implies, revoke the authenticator in case you lose it. In joint-authenticator method, you should've already done that in Steam mobile app, but feel free to do the same in case you need to.

In regards to technical details, the generated `maFile` includes all details that we've received from the Steam server during linking the authenticator, and in addition to that, the `device_id` field, which may be needed for other (third-party) authenticators, if you ever decide to import that `maFile` into them.

ASF automatically imports your authenticator once the procedure is done, and therefore `2fa` and other related commands should now be operational for the bot account you linked the authenticator to. We recommend you to verify that.

---

## 불러오기

Import process requires already linked and operational authenticator that is supported by ASF. We have instructions for a few different official and unofficial sources of 2FA, on top of manual method which allows you to provide required credentials yourself. Please note that those instructions should be used **only** if you're already using given solution - since process here involves third-party apps and tools, **we do not recommend using them**, and we're mentioning it exclusively for people that already decided to use them and would like to import generated details into ASF 2FA.

아래의 모든 가이드는 **정상동작하고 있는** 인증기를 필요로 합니다. 유효하지 않은 데이터를 가져오면 ASF 2단계 인증은 정상적으로 동작하지 않을것이므로 데이터를 가져오기 전에 인증기가 정상 작동하는지 확인하여야 합니다. 다음의 인증기 기능이 정상 작동하는지 테스트하고 확인하십시오.
- You can generate tokens and those tokens are accepted by Steam network (you can log in with them)
- 확인사항을 가져오고, 모바일 인증기에 도착할수 있어야 함
- You can react to those confirmations, and they're properly recognized by Steam network as confirmed/rejected

Ensure that your authenticator works by checking if above actions work - if they don't, then they won't work in ASF either.

---

### 안드로이드 전화기

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

Launch ASF, which should notice your file and import it. Assuming that you've imported the correct file with valid secrets, everything should work properly, which you can verify by using `2fa` commands. 실수를 했다면, 언제든 필요한만큼 `Bot.db`를 삭제하고 다시 시작할 수 있습니다.

---

### SteamDesktopAuthenticator

이미 SDA에서 실행되는 인증기가 있다면 `maFiles` 폴더에 `steamID.maFile` 파일이 있음을 알것입니다. Make sure that `maFile` is in unencrypted form, as ASF can't decrypt SDA files - unencrypted file content should start with `{` and end with `}` character. If needed, you can remove the encryption from SDA settings first, and enable it again when you're done. Once the file is in unencrypted form, copy it to `config` directory of ASF.

You can now rename `steamID.maFile` to `BotName.maFile` in ASF config directory, where `BotName` is the name of your bot you're adding ASF 2FA to. 또는 그대로 둘 수도 있습니다. ASF는 로그인 후 자동으로 선택할 것입니다. Renaming the file helps ASF by making it possible to use ASF 2FA before logging in, if you don't do that, then the file can be picked only after ASF successfully logs in (as ASF doesn't know `steamID` of your account before in fact logging in).

Launch ASF, which should notice your file and import it. Assuming that you've imported the correct file with valid secrets, everything should work properly, which you can verify by using `2fa` commands. 실수를 했다면, 언제든 필요한만큼 `Bot.db`를 삭제하고 다시 시작할 수 있습니다.

---

### WinAuth

먼저 새로운 빈 `봇이름.maFile` 파일을 ASF 환경설정 디렉토리에 생성하십시오. `봇이름`은 ASF 2단계 인증을 추가하려는 봇의 이름입니다. 부정확한 이름을 넣으면 ASF가 선택하지 않습니다.

이제 평소처럼 WinAuth를 실행하십시오. Steam 아이콘을 오른쪽 클릭하고 "Show SteamGuard and Recovery Code"를 선택하십시오. 그리고 "Allow copy"에 체크합니다. `{`로 시작하는 친숙한 JSON 구조가 창의 아래쪽에 나타납니다. 이전 단계에서 생성한 `봇이름.maFile` 파일에 전체 텍스트를 복사해 넣습니다.

Launch ASF, which should notice your file and import it. Assuming that you've imported the correct file with valid secrets, everything should work properly, which you can verify by using `2fa` commands. 실수를 했다면, 언제든 필요한만큼 `Bot.db`를 삭제하고 다시 시작할 수 있습니다.

---

### Manual

고급사용자의 경우, 직접 maFile을 수동으로 생성할 수 있습니다. This can be used in case you'd want to import authenticator from other sources than the ones we've described above. 파일은 **[유효한 JSON 구조](https://jsonlint.com)**를 가져야 합니다:

```json
{
  "shared_secret": "STRING",
  "identity_secret": "STRING"
}
```

표준 인증기 데이터는 더 많은 항목이 있지만, ASF의 가져오기 단계에서 모두 무시되므로 필요하지 않습니다. You don't need to remove them - ASF only requires valid JSON with 2 mandatory fields described above, and will ignore additional fields (if any). Of course, you need to replace `STRING` placeholder in the example above with valid values for your account. Each `STRING` should be base64-encoded representation of bytes the appropriate private key is made of.

---

## 자주 묻는 질문(FAQ)

### ASF가 2단계 인증 모듈을 어떻게 사용합니까?

ASF 2단계 인증을 사용할 수 있으면 ASF는 ASF가 보내거나 받는 거래의 자동 수락에 2단계 인증을 사용합니다. 예를 들어 로그인 같이 필요에 따라 2단계 인증 토큰을 자동으로 생성할 수도 있습니다. 또한, ASF 2단계 인증은 `2fa` 명령어를 사용가능하게 합니다.

---

### How can I obtain 2FA token?

2단계 인증으로 보호된 계정에 접근하려면 2단계 인증 토큰이 필요합니다. 이는 ASF 2단계 인증이 있는 모든 계정을 포함합니다. If you've decided to use standalone authenticator, then you should use `2fa <BotNames>` command to generate temporary token for given bot instances. In all other scenarios, we recommend to use original authenticator that you've used, although you can use the command as well if it's more convenient to you.

---

### ASF 2단계 인증으로 가져온 후에도 원래 인증기를 사용할 수 있습니까?

네. 원래 인증기도 정상작동하며 ASF 2단계 인증과 함께 사용할 수 있습니다. Keep in mind however that if you invalidate it through any method, then linked ASF 2FA credentials will also no longer be valid.

---

### ASF 2단계 인증을 제거하려면 어떻게 해야 합니까?

그냥 ASF를 중지하고 제거하려는 ASF 2단계 인증과 연결된 봇의 `봇이름.db` 파일을 삭제하십시오. This option will remove associated imported 2FA with ASF, but will NOT invalidate (unlink) your authenticator. If you instead want to invalidate your authenticator, apart from removing it from ASF (firstly), you should unlink it in original authenticator of your choice. If you can't do that for some reason, for example because you're using ASF 2FA in standalone mode, then use revocation code that you've received during setup, on the Steam website. It's not possible to invalidate your authenticator through ASF.

---

### I linked authenticator in third-party app, then imported to ASF. Can I now link it again on my phone?

**아니오**. Doing so will invalidate the previously imported credentials and your ASF 2FA will stop functioning (by generating codes no longer being accepted by Steam). Firstly decide where you want to have your original or third-party authenticator located, then import it as ASF 2FA.

---

### Is using ASF 2FA better than third-party authenticator set to accept all confirmations?

여러가지 측면에서 **그렇습니다**. 첫째, 그리고 가장 중요한 것은 ASF 2단계 인증은 보안을 **상당히** 증가시킵니다. ASF 2단계 인증 모듈은 ASF 자체의 확인사항만을 수락하므로, 공격자가 해로운 거래를 요청한다고 해도 ASF 2단계 인증은 ASF가 생성한 것이 아니기 때문에 그러한 거래를 수락하지 **않습니다**. In addition to security part, using ASF 2FA also brings performance/optimization benefits, as ASF 2FA fetches and accepts confirmations immediately after they're generated, and only then, as opposed to inefficient polling for confirmations each X minutes which is achieved by other solutions. There is no reason to use third-party authenticator over ASF 2FA, if you plan on automating confirmations generated by ASF - that's exactly what ASF 2FA is for, and using it does not conflict with you confirming everything else in authenticator of your choice. We strongly recommend to use ASF 2FA for entire ASF activity.
# Parancssori argumentumok

Az ASF számos paranacssori argumentumot támogat, amikkel a program futását lehet befolyásolni. Haladó felhasználók számára ajánlott, akik finomítani szeretnék a program futását. Az alapértelmezett `ASF.json` konfigurációs fájlhoz képest a parancssori argumentumokkal az inicializációt (pl.: `--path`), platform specifikus beállításokat (pl.: `--system-required`), valamint érzékeny adatokat (pl.: `--cryptkey`) lehet beállítani.

---

## Használat

A használat függ az operációs rendszeredtől és az ASF verziódtól.

Általános használat:

```shell
dotnet ArchiSteamFarm.dll --argumentum --másikArgumentum
```

Windows esetén:

```powershell
.\ArchiSteamFarm.exe --argumentum --másikArgumentum
```

Linux/macOS:

```shell
./ArchiSteamFarm --argumentum --másikArgumentum
```

A parancsori argumentumok az általános segítő szkriptekben is támogatottak, mint például `ArchiSteamFarm.cmd`, vagy `ArchiSteamFarm.sh`. In addition to that, you can also use `ASF_ARGS` environment property, like stated in our **[management](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#environment-variables)** and **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker#command-line-arguments)** sections.

Ha az argumentumod szóközt is tartalmaz, ne felejts el idézőjelet használni. Ez a két sor hibás:

```shell
./ArchiSteamFarm --path /home/archi/Saját letöltések/ASF # Rossz!
./ArchiSteamFarm --path=/home/archi/Saját letöltések/ASF # Rossz!
```

Viszont ez a két sor jó:

```shell
./ArchiSteamFarm --path "/home/archi/Saját letöltések/ASF" # OK 
./ArchiSteamFarm "--path=/home/archi/Saját letöltések/ASF" # OK
```

## Argumentumok

`--cryptkey <key>` vagy `--cryptkey=<key>` - az ASF-t egyedi titkosító kulccsal fogja elindítani, melynek értéke `<key>`. Ez a beállítás a **[biztonságot](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** befolyásolja, és kényszeríteni fogja az ASF-t, hogy az általad megadott `<key>` kulcsot használja az alapértelmezett helyett, ami a futtatható állományba van beleégetve. Since this property affects default encryption key (for encrypting purposes) as well as salt (for hashing purposes), keep in mind that everything encrypted/hashed with this key will require it to be passed on each ASF run.

There is no requirement on `<key>` length or characters, but for security reasons we recommend to pick long enough passphrase made out of e.g. random 32 characters, for example by using `tr -dc A-Za-z0-9 < /dev/urandom | head -c 32; echo` command on Linux.

It's nice to mention that there are also two other ways to provide this detail: `--cryptkey-file` and `--input-cryptkey`.

A titkosító kulcsot lehetőség van úgy is beállítani, hogy az `ASF_CRYPTKEY` környezeti változót használod, ami azon emberek számára lehet hasznos, akik nem szeretnének érzékeny adatokat argumentumokon keresztül átadni.

---

`--cryptkey-file <path>` or `--cryptkey-file=<path>` - will start ASF with custom cryptographic key read from `<path>` file. This serves the same purpose as `--cryptkey <key>` explained above, only the mechanism differs, as this property will read `<key>` from provided `<path>` instead. If you're using this together with `--path`, consider the fact that relative path will be different depending on the order of arguments, i.e. whether you switch `--path` before or after `--cryptkey-file`.

Due to the nature of this property, it's also possible to set cryptkey file by declaring `ASF_CRYPTKEY_FILE` environment variable, which may be more appropriate for people that would want to avoid sensitive details in the process arguments.

---

`--ignore-unsupported-environment` - will cause ASF to ignore problems related to running in unsupported environment, which normally is signalized with an error and a forced exit. Unsupported environment includes for example running `win-x64` OS-specific build on `linux-x64`. While this flag will allow ASF to attempt running in such scenarios, be advised that we do not support those officially and you're forcing ASF to do it entirely **at your own risk**. It's important to point out that **all** of the unsupported environment scenarios **can be corrected**. We strongly recommend to fix the outstanding problems instead of declaring this argument.

---

`--input-cryptkey` - will make ASF ask about the `--cryptkey` during startup. This option might be useful for you if instead of providing cryptkey, whether in environment variables or a file, you'd prefer to not have it saved anywhere and instead input it manually on each ASF run.

---

`--minimized` - will make ASF console window minimize shortly after start. Useful mainly in auto-start scenarios, but can also be used outside of those. This option requires appropriate environment support - it may not work properly in all possible scenarios.

---

`--network-group <group>` or `--network-group=<group>` - will cause ASF to init its limiters with a custom network group of `<group>` value. This option affects running ASF in **[multiple instances](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#multiple-instances)** by signalizing that given instance is dependent only on instances sharing the same network group, and independent of the rest. Typically you want to use this property only if you're routing ASF requests through custom mechanism (e.g. different IP addresses) and you want to set networking groups yourself, without relying on ASF to do it automatically (which currently includes taking into account `WebProxy` only). Keep in mind that when using a custom network group, this is unique identifier within the local machine, and ASF will not take into account any other details, such as `WebProxy` value, allowing you to e.g. start two instances with different `WebProxy` values which are still dependent on each other.

Due to the nature of this property, it's also possible to set the value by declaring `ASF_NETWORK_GROUP` environment variable, which may be more appropriate for people that would want to avoid sensitive details in the process arguments.

---

`--no-config-migrate` - by default ASF will automatically migrate your config files to latest syntax. Migration includes conversion of deprecated properties into latest ones, removing properties with default values (as they have no effect), as well as cleaning up the file in general (correcting indentation and likewise). This is almost always a good idea, but you might have a particular situation where you'd prefer ASF to never overwrite the config files automatically. For example, you might want to `chmod 400` your config files (read permission for the owner only) or put `chattr +i` over them, in result denying write access for everyone, e.g. as a security measure. Usually we recommend to keep the config migration enabled, but if you have a particular reason for disabling it and would instead prefer ASF to not do that, you can use this switch for achieving that purpose. Keep in mind however, that providing correct settings to ASF will become from now on your new responsibility, especially in regards to deprecations and refactors of properties in future ASF versions.

---

`--no-config-watch` - by default ASF sets up a `FileSystemWatcher` over your `config` directory in order to listen for events related to file changes, so it can interactively adapt to them. For example, this includes stopping bots on config deletion, restarting bot on config being changed, or loading keys into **[BGR](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)** once you drop them into the `config` directory. This switch allows you to disable such behaviour, which will cause ASF to completely ignore all the changes in `config` directory, requiring from you to do such actions manually, if deemed appropriate (which usually means restarting the process). We recommend to keep the config events enabled, but if you have a particular reason for disabling them and would instead prefer ASF to not do that, you can use this switch for achieving that purpose.

---

`--no-restart` - ezt a kapcsolót főként a **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker)** tárolóink használják és kikényszerítik, hogy az `AutoRestart` értéke `false` legyen. Unless you have a particular need, you should instead configure `AutoRestart` property directly in your config. This switch is here so our docker script won't need to touch your global config in order to adapt it to its own environment. Természetesen ha az ASF-t egy szkripten belül futtatod, akkor lehet még ennek a kapcsolónak létjogosultsága (de egyébként jobban jársz a globális konfigurációs tulajdonsággal).

---

`--no-steam-parental-generation` - by default ASF will automatically attempt to generate Steam parental PINs, as described in **[`SteamParentalCode`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#steamparentalcode)** configuration property. However, since that might require excessive amount of OS resources, this switch allows you to disable that behaviour, which will result in ASF skipping auto-generation and go straight to asking user for PIN instead, which is what would normally happen only if the auto-generation has failed. Usually we recommend to keep the generation enabled, but if you have a particular reason for disabling it and would instead prefer ASF to not do that, you can use this switch for achieving that purpose.

---

`--path <path>` or `--path=<path>` - alapértelmezetten az ASF mindig a saját könyvtárába fog navigálni induláskor. By specifying this argument, ASF will navigate to given directory after initialization, which allows you to use custom path for various application parts (including `config`, `logs`, `plugins` and `www` directories, as well as `NLog.config` file), without a need of duplicating binary in the same place. Jól jöhet, ha szeretnéd külön választani a binárist a konfigurációtól, ahogy az a linux-szerű csomagokban megszokott dolog - így egy binárist több különféle beállítással is használhatsz. Az útvonal lehet relatív az ASF bináris helyéhez képest, vagy abszolút. Keep in mind that this command points to new "ASF home" - the directory that has the same structure as original ASF, with `config` directory inside, see below example for explanation.

Due to the nature of this property, it's also possible to set expected path by declaring `ASF_PATH` environment variable, which may be more appropriate for people that would want to avoid sensitive details in the process arguments.

If you're considering using this command-line argument for running multiple instances of ASF, we recommend reading our **[management page](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#multiple-instances)** on this manner.

Examples:

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

`--system-required` - ha beállítod ezt a kapcsolót, akkor az ASF jelezni fog az operációs rendszer számára, hogy a szüksége van arra, hogy az fusson a teljes élettartama alatt. Jelenleg ennek a kapcsolónak csak windowsos gépeken van értelme, mivel ott meg lehet tiltani, hogy a rendszer alvó módba menjen, amíg a processz futna. This can be proven especially useful when farming on your PC or laptop during night, as ASF will be able to keep your system awake while it's running.
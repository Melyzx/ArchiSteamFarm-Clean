# Συλλογή

Compilation is the process of creating executable file. This is what you want to do if you want to add your own changes to ASF, or if you for whatever reason don't trust executable files provided in official **[releases](https://github.com/JustArchiNET/ArchiSteamFarm/releases)**. If you're user and not a developer, most likely you want to use already precompiled binaries, but if you'd like to use your own ones, or learn something new, continue reading.

ASF can be compiled on any currently supported platform, as long as you have all needed tools to do so.

---

## .NET SDK

Regardless of platform, you need full .NET SDK (not just runtime) in order to compile ASF. Installation instructions can be found on **[.NET download page](https://dotnet.microsoft.com/download)**. You need to install appropriate .NET SDK version for your OS. After successful installation, `dotnet` command should be working and operative. You can verify if it works with `dotnet --info`. Also ensure that your .NET SDK matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**.

---

## Συλλογή

Assuming you have .NET SDK operative and in appropriate version, simply navigate to source ASF directory (cloned or downloaded and unpacked ASF repo) and execute:

```shell
dotnet publish ArchiSteamFarm -c "Release" -o "out/generic"
```

If you're using Linux/macOS, you can instead use `cc.sh` script which will do the same, in a bit more complex manner.

If compilation ended successfully, you can find your ASF in `source` flavour in `out/generic` directory. This is the same as official `generic` ASF build, but it has forced `UpdateChannel` and `UpdatePeriod` of `0`, which is appropriate for self-builds.

### OS-specific

You can also generate OS-specific .NET package if you have a specific need. In general you shouldn't do that because you've just compiled `generic` flavour that you can run with your already-installed .NET runtime that you've used for the compilation in the first place, but just in case you want to:

```shell
dotnet publish ArchiSteamFarm -c "Release" -o "out/linux-x64" -r "linux-x64" --self-contained
```

Of course, replace `linux-x64` with OS-architecture that you want to target, such as `win-x64`. This build will also have updates disabled. When building `--self-contained` you can also optionally declare two more switches: `-p:PublishTrimmed=true` will produce trimmed build, while `-p:PublishSingleFile=true` will produce a single file. Adding both will result in the same settings we use for our own builds.

### ASF-ui

While the above steps are everything that is required to have a fully working build of ASF, you may *also* be interested in building **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)**, our graphical web interface. From ASF side, all you need to do is dropping ASF-ui build output in standard `ASF-ui/dist` location, then building ASF with it (again, if needed).

ASF-ui is part of ASF's source tree as a **[git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules)**, ensure that you've cloned the repo with `git clone --recursive`, as otherwise you'll not have the required files. You'll also need a working NPM, **[Node.js](https://nodejs.org)** comes with it. If you're using Linux/macOS, we recommend our `cc.sh` script, which will automatically cover building and shipping ASF-ui (if possible, that is, if you're meeting the requirements we've just mentioned).

In addition to the `cc.sh` script, we also attach the simplified build instructions below, refer to **[ASF-ui repo](https://github.com/JustArchiNET/ASF-ui)** for additional documentation. From ASF's source tree location, so as above, execute the following commands:

```shell
rm -rf "ASF-ui/dist" # ASF-ui doesn't clean itself after old build

npm ci --prefix ASF-ui
npm run-script deploy --prefix ASF-ui

rm -rf "out/generic/www" # Ensure that our build output is clean of the old files
dotnet publish ArchiSteamFarm -c "Release" -o "out/generic" # Or accordingly to what you need as per the above
```

You should now be able to find the ASF-ui files in your `out/generic/www` folder. ASF will be able to serve those files to your browser.

Alternatively, you can simply build ASF-ui, whether manually or with the help of our repo, then copy the build output over to `${OUT}/www` folder manually, where `${OUT}` is the output folder of ASF that you've specified with `-o` parameter. This is exactly what ASF is doing as part of the build process, it copies `ASF-ui/dist` (if exists) over to `${OUT}/www`, nothing fancy.

---

## Development

If you'd like to edit ASF code, you can use any .NET compatible IDE for that purpose, although even that is optional, since you can as well edit with a notepad and compile with `dotnet` command described above.

If you don't have a better pick, we can recommend **[latest Visual Studio Code](https://code.visualstudio.com/download)**, which is sufficient for even more advanced needs. Of course you can use whatever you want to, for reference we use **[JetBrains Rider](https://www.jetbrains.com/rider)** for ASF development, although it's not a free solution.

---

## Tags

`main` branch is not guaranteed to be in a state that allows successful compilation or flawless ASF execution in the first place, since it's development branch just like stated in our **[release cycle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)**. If you want to compile or reference ASF from source, then you should use appropriate **[tag](https://github.com/JustArchiNET/ArchiSteamFarm/tags)** for that purpose, which guarantees at least successful compilation, and very likely also flawless execution (if build was marked as stable release). In order to check the current "health" of the tree, you can use our CI - **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/ci.yml?query=branch%3Amain)**.

---

## Official releases

Official ASF releases are compiled by **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)**, with latest .NET SDK that matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**. After passing tests, all packages are deployed as the release, also on GitHub. This also guarantees transparency, since GitHub always uses official public source for all builds, and you can compare checksums of GitHub artifacts with GitHub release assets. ASF developers do not compile or publish builds themselves, except for private development process and debugging.

In addition to the above, ASF maintainers manually validate and publish build checksums on independent from GitHub, remote ASF server, as additional security measure. This step is mandatory for existing ASFs to consider the release as a valid candidate for auto-update functionality.
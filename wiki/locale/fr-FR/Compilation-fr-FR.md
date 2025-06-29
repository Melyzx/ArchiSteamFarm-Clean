# Compilation

La compilation est le processus de création d'un fichier exécutable. C’est ce que vous souhaitez faire si vous souhaitez ajouter vos propres modifications à ASF ou si, pour une raison quelconque, vous ne faites pas confiance aux fichiers exécutables fournis dans les **

 versions officielles </ 0>. Si vous êtes utilisateur et non développeur, vous voudrez probablement utiliser des binaires déjà précompilés, mais si vous souhaitez utiliser vos propres binaires ou apprendre quelque chose de nouveau, continuez à lire.</p> 

ASF peut être compilé sur n’importe quelle plate-forme actuellement prise en charge, à condition que vous disposiez de tous les outils nécessaires pour le faire.



---



## .NET SDK

Regardless of platform, you need full .NET SDK (not just runtime) in order to compile ASF. Installation instructions can be found on **[.NET download page](https://dotnet.microsoft.com/download)**. You need to install appropriate .NET SDK version for your OS. Une fois l'installation réussie, la commande ` dotnet </ 0> devrait fonctionner et être opérationnelle. Vous pouvez vérifier si cela fonctionne avec <code> dotnet --info </ 0>. Also ensure that your .NET SDK matches ASF <strong x-id="1"><a href="https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements">runtime requirements</a></strong>.</p>

<hr />

<h2 spaces-before="0">Compilation</h2>

<p spaces-before="0">Assuming you have .NET SDK operative and in appropriate version, simply navigate to source ASF directory (cloned or downloaded and unpacked ASF repo) and execute:</p>

<pre><code class="shell">dotnet publish ArchiSteamFarm -c "Release" -o "out/generic"
`</pre> 

If you're using Linux/macOS, you can instead use `cc.sh` script which will do the same, in a bit more complex manner.

If compilation ended successfully, you can find your ASF in `source` flavour in `out/generic` directory. This is the same as official `generic` ASF build, but it has forced `UpdateChannel` and `UpdatePeriod` of `0`, which is appropriate for self-builds.



### OS-spécifique

You can also generate OS-specific .NET package if you have a specific need. In general you shouldn't do that because you've just compiled `generic` flavour that you can run with your already-installed .NET runtime that you've used for the compilation in the first place, but just in case you want to:



```shell
dotnet publish ArchiSteamFarm -c "Release" -o "out/linux-x64" -r "linux-x64" --self-contained
```


Bien sûr, remplacez ` linux-x64 ` par l'architecture du système d'exploitation que vous souhaitez cibler, tel que ` win-x64 `. Cette mise à jour aura également des mises à jour désactivées. When building `--self-contained` you can also optionally declare two more switches: `-p:PublishTrimmed=true` will produce trimmed build, while `-p:PublishSingleFile=true` will produce a single file. Adding both will result in the same settings we use for our own builds.



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



## Développement

If you'd like to edit ASF code, you can use any .NET compatible IDE for that purpose, although even that is optional, since you can as well edit with a notepad and compile with `dotnet` command described above.

If you don't have a better pick, we can recommend **[latest Visual Studio Code](https://code.visualstudio.com/download)**, which is sufficient for even more advanced needs. Of course you can use whatever you want to, for reference we use **[JetBrains Rider](https://www.jetbrains.com/rider)** for ASF development, although it's not a free solution.



---



## Tags

`main` branch is not guaranteed to be in a state that allows successful compilation or flawless ASF execution in the first place, since it's development branch just like stated in our **[release cycle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)**. If you want to compile or reference ASF from source, then you should use appropriate **[tag](https://github.com/JustArchiNET/ArchiSteamFarm/tags)** for that purpose, which guarantees at least successful compilation, and very likely also flawless execution (if build was marked as stable release). In order to check the current "health" of the tree, you can use our CI - **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/ci.yml?query=branch%3Amain)**.



---



## Versions Officielles

Official ASF releases are compiled by **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)**, with latest .NET SDK that matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**. After passing tests, all packages are deployed as the release, also on GitHub. This also guarantees transparency, since GitHub always uses official public source for all builds, and you can compare checksums of GitHub artifacts with GitHub release assets. Les développeurs ASF ne compilent, ni ne publient les versions manuellement, sauf pour les processus de développement privés, y compris le débogage.

In addition to the above, ASF maintainers manually validate and publish build checksums on independent from GitHub, remote ASF server, as additional security measure. This step is mandatory for existing ASFs to consider the release as a valid candidate for auto-update functionality.
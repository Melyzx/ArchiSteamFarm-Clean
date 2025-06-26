# Arguments de ligne de commande

ASF prend en charge plusieurs arguments de ligne de commande qui peuvent affecter l'exécution du programme. Ils peuvent être utilisés par des utilisateurs avancés pour spécifier au programme son mode de fonctionnement. Par rapport au fichier de configuration `ASF.json` par défaut, les arguments de ligne de commande sont utilisés pour l'initialisation principale (e.g. `--path`). paramètres spécifiques à la plate-forme (e.g. `--system-required`) ou données sensibles (e.g. `--cryptkey`).

---

## Utilisation

Son utilisation dépend de votre OS et ASF.

Générique :

```shell
dotnet ArchiSteamFarm.dll --argument --otherOne
```

Windows :

```powershell
.\ArchiSteamFarm.exe --argument --otherOne
```

Linux/macOS:

```shell
./ArchiSteamFarm --argument --otherOne
```

Les arguments de ligne de commande sont également pris en charge dans les scripts d'assistance génériques tels que ` ArchiSteamFarm.cmd </ 0> ou <code> ArchiSteamFarm.sh </ 0>. De plus, vous pouvez aussi utiliser la propriété d'environnement <code>ASF_ARGS` comme indiqué dans nos sections **[gestion](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#environment-variables)** et **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker#command-line-arguments)**.

Si votre argument contient des espaces, n'oubliez pas de le mettre entre guillemets. Ces deux exemples ne fonctionnent pas :

```shell
./ArchiSteamFarm --path /home/archi/My Downloads/ASF # Ne fonctionne pas !
./ArchiSteamFarm --path=/home/archi/My Downloads/ASF # Ne fonctionne pas !
```

Mais ces deux exemples suivants sont parfaitement valides :

```shell
./ArchiSteamFarm --path "/home/archi/My Downloads/ASF" # OK
./ArchiSteamFarm "--path=/home/archi/My Downloads/ASF" # OK
```

## Arguments

`--cryptkey <key>` ou `--cryptkey=<key>` - démarrera ASF avec une clé cryptographique personnalisée de la valeur `<key>`. Cette option affecte la **

 sécurité </ 0> et obligera ASF à utiliser votre clé `<key>` fournie à la place de la clé par défaut codée dans l'exécutable. Puisque cette propriété affecte la clé de chiffrement par défaut (pour le chiffrement) ainsi que son salt (pour le hachage), Gardez à l'esprit que tout ce qui est chiffré/haché avec cette clé nécessitera qu'elle soit transmise à chaque exécution d'ASF.</p> 

There is no requirement on `<key>` length or characters, but for security reasons we recommend to pick long enough passphrase made out of e.g. random 32 characters, for example by using `tr -dc A-Za-z0-9 < /dev/urandom | head -c 32; echo` command on Linux.

It's nice to mention that there are also two other ways to provide this detail: `--cryptkey-file` and `--input-cryptkey`.

En raison de la nature de cette propriété, il est également possible de définir la clé de chiffrement en déclarant la variable d'environnement `ASF_CRYPTKEY`, qui peut être plus appropriée pour les personnes qui voudraient éviter de divulguer des informations privées dans les arguments du processus.



---

`--cryptkey-file <path>` or `--cryptkey-file=<path>` - will start ASF with custom cryptographic key read from `<path>` file. This serves the same purpose as `--cryptkey <key>` explained above, only the mechanism differs, as this property will read `<key>` from provided `<path>` instead. If you're using this together with `--path`, consider the fact that relative path will be different depending on the order of arguments, i.e. whether you switch `--path` before or after `--cryptkey-file`.

Due to the nature of this property, it's also possible to set cryptkey file by declaring `ASF_CRYPTKEY_FILE` environment variable, which may be more appropriate for people that would want to avoid sensitive details in the process arguments.



---

`--ignore-unsupported-environment` - fera ignorer à ASF les problèmes liés à l'exécution dans un environnement non pris en charge, ce qui est normalement signalisé avec une erreur et une sortie forcée. Unsupported environment includes for example running `win-x64` OS-specific build on `linux-x64`. Même si ce flag laissera ASF s'exécuter dans de tels scénarios, soyez avertis; Nous ne prenons pas en charge ces éléments officiellement et vous forcez ASF à le faire entièrement **à vos risques et périls**. It's important to point out that **all** of the unsupported environment scenarios **can be corrected**. Nous recommandons fortement de résoudre les problèmes précédents plutôt que d'utiliser cet argument.



---

`--input-cryptkey` - will make ASF ask about the `--cryptkey` during startup. This option might be useful for you if instead of providing cryptkey, whether in environment variables or a file, you'd prefer to not have it saved anywhere and instead input it manually on each ASF run.



---

`--minimized` - will make ASF console window minimize shortly after start. Useful mainly in auto-start scenarios, but can also be used outside of those. This option requires appropriate environment support - it may not work properly in all possible scenarios.



---

`--network-group <group>` or `--network-group=<group>` - will cause ASF to init its limiters with a custom network group of `<group>` value. This option affects running ASF in **[multiple instances](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#multiple-instances)** by signalizing that given instance is dependent only on instances sharing the same network group, and independent of the rest. Typically you want to use this property only if you're routing ASF requests through custom mechanism (e.g. different IP addresses) and you want to set networking groups yourself, without relying on ASF to do it automatically (which currently includes taking into account `WebProxy` only). Keep in mind that when using a custom network group, this is unique identifier within the local machine, and ASF will not take into account any other details, such as `WebProxy` value, allowing you to e.g. start two instances with different `WebProxy` values which are still dependent on each other.

Due to the nature of this property, it's also possible to set the value by declaring `ASF_NETWORK_GROUP` environment variable, which may be more appropriate for people that would want to avoid sensitive details in the process arguments.



---

`--no-config-migrate` - par défaut, ASF migrera automatiquement vos fichiers de configuration vers la dernière syntaxe. La migration inclut la conversion des propriétés obsolètes en dernières propriétés, supprimant les propriétés avec des valeurs par défaut (comme elles n'ont pas d'effet), ainsi que le nettoyage du fichier en général (correction de l'indentation et autres). C'est presque toujours une bonne idée, mais vous pourriez avoir une situation particulière où vous préféreriez qu'ASF n'écrase jamais automatiquement les fichiers de configuration. Par exemple, vous pouvez vouloir `chmod 400` vos fichiers de configuration (permission de lecture uniquement pour le propriétaire) ou mettre `chattr +i` sur eux, dans le résultat en refusant l'accès en écriture pour tout le monde, par exemple en tant que mesure de sécurité. Habituellement, nous recommandons de garder la migration de configuration activée, mais si vous avez une raison particulière de le désactiver et préférerait plutôt qu'ASF ne le fasse pas, vous pouvez utiliser cette option. Gardez toutefois à l’esprit que fournir des paramètres corrects à ASF deviendra désormais votre nouvelle responsabilité, en particulier en ce qui concerne les dépréciations et changements de propriétés dans les futures versions d'ASF.



---

`--no-config-watch` - ASF configure par défaut un `FileSystemWatcher` sur votre répertoire `config` afin d'écouter les événements liés aux changements de fichiers, pour qu'il puisse s'adapter interactivement à eux. Par exemple, cela inclut l'arrêt des bots lors de la suppression de la configuration, le redémarrage du bot à la modification de la configuration, ou chargement des clés dans `<1>BGR</1>` une fois que vous les retirez du répertoire <2>config</2>. Cette option vous permet de désactiver ce comportement, ce qui fera qu'ASF ignorera complètement tous les changements dans le répertoire `config`, exigeant de votre part de faire ces actions manuellement, si elles sont jugées appropriées (ce qui signifie généralement redémarrer le processus). Nous recommandons de garder les événements de configuration activés, mais si vous avez une raison particulière de les désactiver et préférerait plutôt qu'ASF ne le fasse pas, vous pouvez utiliser cette option pour atteindre cet objectif.



---

`--no-restart` - cette fonctionnalité est principalement utilisé par nos **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker)** ` et force  <code>AutoRestart` de <0> false</ 0>. À moins que vous n'ayez un besoin particulier, il vaut mieux configurer la propriété `AutoRestart` directement dans votre configuration. Ce commutateur est là pour que notre script docker n'ait pas besoin de toucher à votre configuration globale pour l'adapter à son propre environnement. Bien sûr, si vous exécutez ASF dans un script, vous pouvez également utiliser cette fonctionnalité (sinon, la configuration globale est préférable).



---

`--no-steam-parental-generation` - par défaut ASF essaiera automatiquement de générer des codes PIN parentaux Steam, comme décrit dans **[`SteamParentalCode`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#steamparentalcode)**. Cependant, comme cela peut nécessiter une quantité excessive de ressources d'OS, cette option vous permet de désactiver ce comportement. ASF ignorera la génération automatique et demandera le PIN à l'utilisateur, ce qui ne se produirait normalement que si la génération automatique a échoué. Habituellement, nous recommandons de garder la génération activée, mais si vous avez une raison particulière de le désactiver et préférerait plutôt qu'ASF ne le fasse pas, vous pouvez utiliser cette option.



---

`--path <path>` ou `--path=<path>` - ASF navigue toujours vers son propre répertoire au démarrage. En spécifiant cet argument, ASF naviguera vers un répertoire donné après l’initialisation, ce qui vous permet d’utiliser un chemin personnalisé pour plusieurs parties de l'application (dont les répertoires `config`, <0>plugins</0>, et <0>www</0>, mais aussi le ficher <0>NLog.config</0>), sans avoir besoin de dupliquer le binaire au même endroit. Cela peut s'avérer particulièrement utile si vous souhaitez séparer le binaire de la configuration réelle, comme cela est fait dans un paquet de type Linux - de cette façon, vous pouvez utiliser un binaire (à jour) avec plusieurs configurations différentes. Le chemin peut être relatif en fonction de l'emplacement actuel du binaire ASF ou en absolu. N'oubliez pas que cette commande pointe vers le nouveau "ASF home" - le répertoire qui a la même structure que l'ASF d'origine, avec le répertoire `config` à l'intérieur.

En raison de la nature de cet propriété, il est aussi possible de définir le chemin attendu en déclarant la variable d'environnement `ASF_PATH`, ce qui peut être plus approprié pour les personnes voulant éviter des données sensibles dans les arguments du processus.

Si vous comptez utiliser cette commande pour exécuter plusieurs instances d'ASF, il est recommandé de lire la page traitant de la <0><1>Compatibilité</1></0>.

Exemples:



```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/DossierVoulu # Chemin absolu
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../DossierVoulu # Les chemins relatifs fonctionnent aussi
ASF_PATH=/opt/DossierVoulu dotnet /opt/ASF/ArchiSteamFarm.dll # Pareil qu'avec les variables environnementales
```




```text
├── 📁 /opt
│     ├── 📁 ASF
│     │     ├── ⚙️ ArchiSteamFarm.dll
│     │     └── ...
│     └── 📁 TargetDirectory
│           ├── 📁 config
│           ├── 📁 logs (généré)
│           ├── 📁 plugins (optionnel)
│           ├── 📁 www (optionnel)
│           ├── 📄 log.txt (généré)
│           └── 📄 NLog.config (optionnel)
└── ...
```




---

`--service` - ce commutateur est principalement utilisé par notre service `systemd` et force `Headless` à `true`. À moins que vous n'ayez un besoin particulier, il vaut mieux configurer la propriété `Headless` directement dans votre configuration. Ce commutateur est là pour que notre service `systemd` n'ait pas besoin de toucher à votre configuration globale pour l'adapter à son propre environnement. Bien sûr, si vous avez un besoin similaire, vous pouvez aussi utiliser cette option (sinon vous êtes mieux avec la configuration globale).



---

--system-required</ 0> - la déclaration de ce commutateur incitera ASF à signaler au système d'exploitation que le processus nécessite que le système soit opérationnel pendant toute sa durée de vie. Actuellement, ce commutateur n'a d'effet que sur les machines Windows où il sera interdit à votre système de passer en mode veille tant que le processus est en cours d'exécution. Cela peut être prouvé particulièrement utile lorsque vous fermez sur votre PC ou votre ordinateur portable pendant la nuit, car ASF sera en mesure de garder votre système éveillé pendant son exécution.</p>
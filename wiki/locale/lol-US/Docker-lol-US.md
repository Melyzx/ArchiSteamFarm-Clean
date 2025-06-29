# DOCKR

ASF IZ AVAILABLE AS **[DOCKR CONTAINR](https://www.docker.com/what-container)**. R DOCKR PACKAGEZ R CURRENTLY AVAILABLE ON **[GHCR.IO](https://github.com/JustArchiNET/ArchiSteamFarm/pkgs/container/archisteamfarm)** AS WELL AS **[DOCKR HUB](https://hub.docker.com/r/justarchi/archisteamfarm)**.

IZ IMPORTANT 2 NOWT DAT RUNNIN ASF IN DOCKR CONTAINR IZ CONSIDERD **ADVANCD SETUP**, WHICH IZ **NOT NEEDD** 4 VAST MAJORITY OV USERS, AN TYPICALLY GIVEZ **NO ADVANTAGEZ** OVAR CONTAINR-LES SETUP. IF URE CONSIDERIN DOCKR AS SOLUSHUN 4 RUNNIN ASF AS SERVICE, 4 EXAMPLE MAKIN IT START AUTOMATICALLY WIF UR OS, DEN U SHUD CONSIDR READIN **[MANAGEMENT](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management-lol-us#systemd-service-4-linux)** SECSHUN INSTEAD AN SET UP PROPR `systemd` SERVICE WHICH WILL BE **ALMOST ALWAYS** BETTR IDEA THAN RUNNIN ASF IN DOCKR CONTAINR.

RUNNIN ASF IN DOCKR CONTAINR USUALLY INVOLVEZ **SEVERAL NEW PROBLEMS AN ISSUEZ** DAT ULL HAS 2 FACE AN RESOLVE YOURSELF. DIS AR TEH Y WE **STRONGLY** RECOMMEND U 2 AVOID IT UNLES U ALREADY HAS DOCKR KNOWLEDGE AN DOAN NED HALP UNDERSTANDIN ITZ INTERNALS, BOUT WHICH WE WONT ELABORATE HER ON ASF WIKI. DIS SECSHUN IZ MOSTLY 4 VALID USE CASEZ OV VRY COMPLEX SETUPS, 4 EXAMPLE IN REGARDZ 2 ADVANCD NETWORKIN OR SECURITY BEYOND STANDARD SANDBOXIN DAT ASF COMEZ WIF IN `systemd` SERVICE (WHICH ALREADY ENSUREZ SUPERIOR PROCES ISOLASHUN THRU VRY ADVANCD SECURITY MECHANICS). 4 DOSE HANDFUL AMOUNT OV PEEPS, HER WE EXPLAIN BETTR ASF CONCEPTS IN REGARDZ 2 ITZ DOCKR COMPATIBILITY, AN ONLY DAT, URE ASSUMD 2 HAS ADEQUATE DOCKR KNOWLEDGE YOURSELF IF U DECIDE 2 USE IT TOGETHR WIF ASF.

---

## TAGS

ASF IZ AVAILABLE THRU 4 MAIN TYPEZ OV **[TAGS](https://hub.docker.com/r/justarchi/archisteamfarm/tags)**:


### `main`

DIS TAG ALWAYS POINTS 2 TEH ASF BUILT FRUM LATEST COMMIT IN `main` BRANCH, WHICH WERKZ TEH SAME AS GRABBIN LATEST ARTIFACT DIRECTLY FRUM R **[CI](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/publish.yml?query=branch%3Amain)** PIPELINE. TYPICALLY U SHUD AVOID DIS TAG, AS IZ TEH HIGHEST LEVEL OV BUGGD SOFTWARE DEDICATD 2 DEVELOPERS AN ADVANCD USERS 4 DEVELOPMENT PURPOSEZ. TEH IMAGE IZ BEAN UPDATD WIF EACH COMMIT IN DA `main` GITHUB BRANCH, THEREFORE U CAN EXPECT VRY OFTEN UPDATEZ (AN STUFF BEAN BROKD). IZ HER 4 US 2 MARK CURRENT STATE OV ASF PROJECT, WHICH IZ NOT NECESARILY GUARANTED 2 BE STABLE OR TESTD, JUS LIEK POINTD OUT IN R RELEASE CYCLE. DIS TAG SHUD NOT BE USD IN ANY PRODUCSHUN ENVIRONMENT.


### `released`

VRY SIMILAR 2 TEH ABOOV, DIS TAG ALWAYS POINTS 2 TEH LATEST **[RELEASD](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** ASF VERSHUN, INCLUDIN PRE-RELEASEZ. COMPARD 2 `main` TAG, DIS IMAGE IZ BEAN UPDATD EACH TIEM NEW GITHUB TAG IZ PUSHD. DEDICATD 2 ADVANCD/POWR USERS DAT LUV 2 LIV ON TEH EDGE OV WUT CAN BE CONSIDERD STABLE AN FRESH AT TEH SAME TIEM. DIS AR TEH WUT WED RECOMMEND IF U DOAN WANTS 2 USE `latest` TAG. IN PRACTICE, IT WERKZ TEH SAME AS ROLLIN TAG POINTIN 2 TEH MOST RESENT `A.B.C.D` RELEASE AT TEH TIEM OV PULLIN. PLZ NOWT DAT USIN DIS TAG IZ EQUAL 2 USIN R **[PRE-RELEASEZ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-lol-US)**.


### `latest`

DIS TAG IN COMPARISON WIF OTHERS, IZ TEH ONLY WAN DAT INCLUDEZ ASF AUTO-UPDATEZ FEACHUR AN POINTS 2 TEH LATEST **[STABLE](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** ASF VERSHUN. TEH OBJECTIV OV DIS TAG IZ 2 PROVIDE SANE DEFAULT DOCKR CONTAINR DAT IZ CAPABLE OV RUNNIN SELF-UPDATIN, OS-SPECIFIC BUILD OV ASF. CUZ OV DAT, TEH IMAGE DOESNT HAS 2 BE UPDATD AS OFTEN AS POSIBLE, AS INCLUDD ASF VERSHUN WILL ALWAYS BE CAPABLE OV UPDATIN ITSELF IF NEEDD. OV COURSE, `UpdatePeriod` CAN BE SAFELY TURND OFF (SET 2 `0`), BUT IN DIS CASE U SHUD PROBABLY USE FROZEN `A.B.C.D` RELEASE INSTEAD. LIKEWIZE, U CAN MODIFY DEFAULT `UpdatePeriod` IN ORDR 2 MAK AUTO-UPDATIN `released` TAG INSTEAD.

DUE 2 TEH FACT DAT TEH `latest` IMAGE COMEZ WIF CAPABILITY OV AUTO-UPDATEZ, IT INCLUDEZ BARE OS WIF OS-SPECIFIC `linux` ASF VERSHUN, CONTRARY 2 ALL OTHR TAGS DAT INCLUDE OS WIF .NET RUNTIME AN `generic` ASF VERSHUN. DIS AR TEH CUZ NEWR (UPDATD) ASF VERSHUN MITE ALSO REQUIRE NEWR RUNTIME THAN TEH WAN TEH IMAGE CUD POSIBLY BE BUILT WIF, WHICH WUD OTHERWIZE REQUIRE IMAGE 2 BE RE-BUILT FRUM SCRATCH, NULLIFYIN TEH PLANND USE-CASE.

### `A.B.C.D`

IN COMPARISON WIF ABOOV TAGS, DIS TAG IZ COMPLETELY FROZEN, WHICH MEANZ DAT TEH IMAGE WONT BE UPDATD ONCE PUBLISHD. DIS WERKZ SIMILAR 2 R GITHUB RELEASEZ DAT R NEVR TOUCHD AFTR TEH INITIAL RELEASE, WHICH GUARANTEEZ U STABLE AN FROZEN ENVIRONMENT. TYPICALLY U SHUD USE DIS TAG WHEN U WANTS 2 USE SUM SPECIFIC ASF RELEASE AN U DOAN WANTS 2 USE ANY KIND OV AUTO-UPDATEZ (E.G. DOSE OFFERD IN `latest` TAG).

---

## WHICH TAG IZ TEH BEST 4 ME?

DAT DEPENDZ ON WUT URE LOOKIN 4. 4 MAJORITY OV USERS, `latest` TAG SHUD BE TEH BEST WAN AS IT OFFERS EGSAKTLY WUT DESKTOP ASF DOEZ, JUS IN SPESHUL DOCKR CONTAINR AS SERVICE. PEEPS DAT R REBUILDIN THEIR IMAGEZ QUITE OFTEN AN WUD INSTEAD PREFR FULL CONTROL WIF ASF VERSHUN TID 2 GIVEN RELEASE R WELCOM 2 USE `released` TAG. IF U INSTEAD WANTS 2 USE SUM SPECIFIC FROZEN ASF VERSHUN DAT WILL NEVR CHANGE WITHOUT UR CLEAR INTENSHUN, `A.B.C.D` RELEASEZ R AVAILABLE 4 U AS FIXD ASF MILESTONEZ U CAN ALWAYS FALL BAK 2.

WE GENERALLY DISCOURAGE TRYIN `main` BUILDZ, AS DOSE R HER 4 US 2 MARK CURRENT STATE OV ASF PROJECT. NOTHIN GUARANTEEZ DAT SUCH STATE WILL WERK PROPERLY, BUT OV COURSE URE MOAR THAN WELCOM 2 GIV THEM TRY IF URE INTERESTD IN ASF DEVELOPMENT.

---

## ARCHITECTUREZ

ASF DOCKR IMAGE IZ CURRENTLY BUILT ON `linux` PLATFORM TARGETTIN 3 ARCHITECTUREZ - `x64`, `arm` AN `arm64`. U CAN READ MOAR BOUT THEM IN **[COMPATIBILITY](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-lol-US)** SECSHUN.

R TAGS R USIN MULTI-PLATFORM MANIFEST, WHICH MEANZ DAT DOCKR INSTALLD ON UR MACHINE WILL AUTOMATICALLY SELECT TEH PROPR IMAGE 4 UR PLATFORM WHEN PULLIN TEH IMAGE. IF BY ANY CHANCE UD LIEK 2 PULL SPECIFIC PLATFORM IMAGE WHICH DOESNT MATCH TEH WAN URE CURRENTLY RUNNIN, U CAN DO DAT THRU `--platform` SWITCH IN APPROPRIATE DOCKR COMMANDZ, SUCH AS `docker run`. C DOCKR DOCUMENTASHUN ON **[IMAGE MANIFEST](https://docs.docker.com/registry/spec/manifest-v2-2)** 4 MOAR INFO.

---

## USAGE

4 COMPLETE REFERENCE U SHUD USE **[OFFISHUL DOCKR DOCUMENTASHUN](https://docs.docker.com/engine/reference/commandline/docker)**, WELL COVR ONLY BASIC USAGE IN DIS GUIDE, URE MOAR THAN WELCOM 2 DIG DEEPR.

### Y HALO THAR ASF!

FIRSTLY WE SHUD VERIFY IF R DOCKR IZ EVEN WERKIN RITE, DIS WILL SERVE AS R ASF "Y HALO THAR WURLD":

```shell
docker run -it --name asf --pull always --rm justarchi/archisteamfarm
```

`docker run` CREATEZ NEW ASF DOCKR CONTAINR 4 U AN RUNS IT IN DA FOREGROUND (`-it`). `--pull always` ENSUREZ DAT UP-2-DATE IMAGE WILL BE PULLD FURST, AN `--rm` ENSUREZ DAT R CONTAINR WILL BE PURGD ONCE STOPPD, SINCE WERE JUS TESTIN IF EVRYTHIN WERKZ FINE 4 NAO.

IF EVRYTHIN ENDD SUCCESFULLY, AFTR PULLIN ALL LAYERS AN STARTIN CONTAINR, U SHUD NOTICE DAT ASF PROPERLY STARTD AN INFORMD US DAT THAR R NO DEFIND BOTS, WHICH IZ GUD - WE VERIFID DAT ASF IN DOCKR WERKZ PROPERLY. HIT `CTRL+C` 2 TERMINATE TEH ASF PROCES AN THEREFORE ALSO TEH CONTAINR.

IF U TAEK CLOSR LOOK AT TEH COMMAND DEN ULL NOTICE DAT WE DIDNT DECLARE ANY TAG, WHICH AUTOMATICALLY DEFAULTD 2 `latest` WAN. IF U WANTS 2 USE OTHR TAG THAN `latest`, 4 EXAMPLE `released`, DEN U SHUD DECLARE IT EXPLICITLY:

```shell
docker run -it --name asf --pull always --rm justarchi/archisteamfarm:released
```

---

## USIN VOLUME

IF URE USIN ASF IN DOCKR CONTAINR DEN OBVIOUSLY U NED 2 CONFIGURE TEH PROGRAM ITSELF. U CAN DO IT IN VARIOUS DIFFERENT WAYS, BUT TEH RECOMMENDD WAN WUD BE 2 CREATE ASF `config` DIRECTORY ON LOCAL MACHINE, DEN MOUNT IT AS SHARD VOLUME IN ASF DOCKR CONTAINR.

4 EXAMPLE, WELL ASSUME DAT UR ASF CONFIG FOLDR IZ IN `/home/archi/ASF/config` DIRECTORY. DIS DIRECTORY CONTAINS CORE `ASF.json` AS WELL AS BOTS DAT WE WANTS 2 RUN. NAO ALL WE NED 2 DO IZ SIMPLY ATTACHIN DAT DIRECTORY AS SHARD VOLUME IN R DOCKR CONTAINR, WER ASF EXPEX ITZ CONFIG DIRECTORY (`/app/config`).

```shell
docker run -it -v /home/archi/ASF/config:/app/config --name asf --pull always justarchi/archisteamfarm
```

AN THAZ IT, NAO UR ASF DOCKR CONTAINR WILL USE SHARD DIRECTORY WIF UR LOCAL MACHINE IN READ-RITE MODE, WHICH IZ EVRYTHIN U NED 4 CONFIGURIN ASF. IN SIMILAR, WAI U CAN MOUNT OTHR VOLUMEZ DAT UD LIEK 2 SHARE WIF ASF, SUCH AS `/app/logs` OR `/app/plugins`.

OV COURSE, DIS AR TEH JUS WAN SPECIFIC WAI 2 ACHIEVE WUT WE WANTS, NOTHIN IZ STOPPIN U FRUM E.G. CREATIN UR OWN `Dockerfile` DAT WILL COPY UR CONFIG FILEZ INTO `/app/config` DIRECTORY INSIDE ASF DOCKR CONTAINR. WERE ONLY COVERIN BASIC USAGE IN DIS GUIDE.

### VOLUME PERMISHUNS

ASF CONTAINR BY DEFAULT IZ INITIALIZD WIF DEFAULT `root` USR, WHICH ALLOWS IT 2 HANDLE TEH INTERNAL PERMISHUNS STUFF AN DEN EVENTUALLY SWITCH 2 `asf` (UID `1000`) USR 4 DA REMAININ PART OV TEH MAIN PROCES. WHILE DIS SHUD BE SATISFYIN 4 DA VAST MAJORITY OV USERS, IT DOEZ AFFECT TEH SHARD VOLUME AS NEWLY-GENERATD FILEZ WILL BE NORMALLY OWND BY `asf` USR, WHICH CUD NOT BE DESIRD SITUASHUN IF UD LIEK SUM OTHR USR 4 UR SHARD VOLUME.

THAR R 2 WAYS U CAN CHANGE TEH USR ASF IZ RUNNIN UNDR. TEH FURST WAN, RECOMMENDD, IZ 2 DECLARE `ASF_USER` ENVIRONMENT VARIABLE WIF TARGET UID U WANTS 2 RUN UNDR. SECOND, ALTERNATIV WAN, IZ 2 PAS `--user` **[FLAG](https://docs.docker.com/engine/reference/run/#user)**, WHICH IZ DIRECTLY SUPPORTD BY DOCKR.

U CAN CHECK UR `uid` 4 EXAMPLE WIF `id -u` COMMAND, DEN DECLARE IT AS SPECIFID ABOOV. 4 EXAMPLE, IF UR TARGET USR HAS `uid` OV 1001:

```shell
docker run -it -e ASF_USER=1001 -v /home/archi/ASF/config:/app/config --name asf --pull always justarchi/archisteamfarm

# ALTERNATIVELY, IF U UNDERSTAND TEH LIMITASHUNS BELOW
docker run -it -u 1001 -v /home/archi/ASF/config:/app/config --name asf --pull always justarchi/archisteamfarm
```

TEH DIFFERENCE TWEEN `ASF_USER` AN `--user` FLAG IZ SUBTLE, BUT IMPORTANT. `ASF_USER` IZ CUSTOM MECHANISM SUPPORTD BY ASF, IN DIS SCENARIO DOCKR CONTAINR STILL STARTS AS `root`, AN DEN ASF STARTUP SCRIPT STARTS MAIN BINARY UNDR `ASF_USER`. WHEN USIN `--user` FLAG, URE STARTIN WHOLE PROCES, INCLUDIN ASF STARTUP SCRIPT AS GIVEN USR. FURST OPSHUN ALLOWS ASF STARTUP SCRIPT 2 HANDLE PERMISHUNS AN OTHR STUFF AUTOMATICALLY 4 U, RESOLVIN SUM COMMON ISSUEZ DAT U MITEVE CAUSD, 4 EXAMPLE IT ENSUREZ DAT UR `/app` AN `/asf` DIRECTORIEZ R AKSHULLY OWND BY `ASF_USER`. IN SECOND SCENARIO, SINCE WERE NOT RUNNIN AS `root`, WE CANT DO DAT, AN URE EXPECTD 2 HANDLE ALL OV DAT YOURSELF IN ADVANCE.

IF UVE DECIDD 2 USE `--user` FLAG, U NED 2 CHANGE OWNERSHIP OV ALL ASF FILEZ FRUM DEFAULT `asf` 2 UR NEW CUSTOM USR. U CAN DO SO BY EXECUTIN COMMAND BELOW:

```shell
# EXECUTE ONLY IF URE NOT USIN ASF_USER
docker exec -u root asf chown -hR 1001 /app /asf
```

DIS HAS 2 BE DUN ONLY ONCE AFTR U CREATD UR CONTAINR WIF `docker run`, AN ONLY IF U DECIDD 2 USE CUSTOM USR THRU `--user` DOCKR FLAG. ALSO DOAN FORGET 2 CHANGE `1001` ARGUMENT IN COMMAND ABOOV 2 TEH `UID` U AKSHULLY WANTS 2 RUN ASF UNDR.

### VOLUME WIF SELINUX

IF URE USIN SELINUX IN ENFORCD STATE ON UR OS, WHICH IZ TEH DEFAULT 4 EXAMPLE ON RHEL-BASD DISTROS, DEN U SHUD MOUNT TEH VOLUME APPENDIN `:Z` OPSHUN, WHICH WILL SET CORRECT SELINUX CONTEXT 4 IT.

```
docker run -it -v /home/archi/ASF/config:/app/config:Z --name asf --pull always justarchi/archisteamfarm
```

DIS WILL ALLOW ASF 2 CREATE FILEZ TARGETTIN TEH VOLUME WHILE INSIDE DOCKR CONTAINR.

---

## MULTIPLE INSTANCEZ SYNCHRONIZASHUN

ASF INCLUDEZ SUPPORT 4 MULTIPLE INSTANCEZ SYNCHRONIZASHUN, AS STATD IN **[MANAGEMENT](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management-lol-US#multiple-instancez)** SECSHUN. WHEN RUNNIN ASF IN DOCKR CONTAINR, U CAN OPSHUNALLY "OPT-IN" INTO TEH PROCES, IN CASE URE RUNNIN MULTIPLE CONTAINERS WIF ASF AN UD LIEK 4 THEM 2 SYNCHRONIZE WIF EACH OTHR.

BY DEFAULT, EACH ASF RUNNIN INNA DOCKR CONTAINR IZ STANDALONE, WHICH MEANZ DAT NO SYNCHRONIZASHUN TAKEZ PLACE. IN ORDR 2 ENABLE SYNCHRONIZASHUN TWEEN THEM, U MUST BIND `/tmp/ASF` PATH IN EVRY ASF CONTAINR DAT U WANTS 2 SYNCHRONIZE, 2 WAN, SHARD PATH ON UR DOCKR HOST, IN READ-RITE MODE. DIS AR TEH ACHIEVD EGSAKTLY TEH SAME AS BINDIN VOLUME WHICH WUZ DESCRIBD ABOOV, JUS WIF DIFFERENT PATHS:

```shell
mkdir -p /tmp/ASF-g1
docker run -v /tmp/ASF-g1:/tmp/ASF -v /home/archi/ASF/config:/app/config --name asf1 --pull always justarchi/archisteamfarm
docker run -v /tmp/ASF-g1:/tmp/ASF -v /home/john/ASF/config:/app/config --name asf2 --pull always justarchi/archisteamfarm
# AN SO ON, ALL ASF CONTAINERS R NAO SYNCHRONIZD WIF EACH OTHR
```

WE RECOMMEND 2 BIND ASFS `/tmp/ASF` DIRECTORY ALSO 2 TEMPORARY `/tmp` DIRECTORY ON UR MACHINE, BUT OV COURSE URE FREE 2 CHOOSE ANY OTHR WAN DAT SATISFIEZ UR USAGE. EACH ASF CONTAINR DAT IZ EXPECTD 2 BE SYNCHRONIZD SHUD HAS ITZ `/tmp/ASF` DIRECTORY SHARD WIF OTHR CONTAINERS DAT R TAKIN PART IN DA SAME SYNCHRONIZASHUN PROCES.

AS UVE PROBABLY GUESD FRUM EXAMPLE ABOOV, IZ ALSO POSIBLE 2 CREATE 2 OR MOAR "SYNCHRONIZASHUN GROUPS", BY BINDIN DIFFERENT DOCKR HOST PATHS INTO ASFS `/tmp/ASF`.

MOUNTIN `/tmp/ASF` IZ COMPLETELY OPSHUNAL AN AKSHULLY NOT RECOMMENDD, UNLES U EXPLICITLY WANTS 2 SYNCHRONIZE 2 OR MOAR ASF CONTAINERS. WE DO NOT RECOMMEND MOUNTIN `/tmp/ASF` 4 SINGLE-CONTAINR USAGE, AS IT BRINGS ABSOLUTELY NO BENEFITS IF U EXPECT 2 RUN JUS WAN ASF CONTAINR, AN IT MITE AKSHULLY CAUSE ISSUEZ DAT CUD OTHERWIZE BE AVOIDD.

---

## COMMAND-LINE ARGUMENTS

ASF ALLOWS U 2 PAS **[COMMAND-LINE ARGUMENTS ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-lol-US)** IN DOCKR CONTAINR THRU ENVIRONMENT VARIABLEZ. U SHUD USE SPECIFIC ENVIRONMENT VARIABLEZ 4 SUPPORTD SWITCHEZ, AN `ASF_ARGS` 4 DA REST. DIS CAN BE ACHIEVD WIF `-e` SWITCH ADDD 2 `docker run`, 4 EXAMPLE:

```shell
docker run -it -e "ASF_CRYPTKEY=MyPassword" -e "ASF_ARGS=--no-config-migrate" --name asf --pull always justarchi/archisteamfarm
```

DIS WILL PROPERLY PAS UR `--cryptkey` ARGUMENT 2 ASF PROCES BEAN RUN INSIDE DOCKR CONTAINR, AS WELL AS OTHR ARGS. OV COURSE, IF URE ADVANCD USR, DEN U CAN ALSO MODIFY `ENTRYPOINT` OR ADD `CMD` AN PAS UR CUSTOM ARGUMENTS YOURSELF.

UNLES U WANTS 2 PROVIDE CUSTOM ENCRYPSHUN KEY OR OTHR ADVANCD OPSHUNS, USUALLY U DOAN NED 2 INCLUDE ANY SPESHUL ENVIRONMENT VARIABLEZ, AS R DOCKR CONTAINERS R ALREADY CONFIGURD 2 RUN WIF SANE EXPECTD DEFAULT OPSHUNS OV  `--no-restart` `--system-required`, SO DOSE FLAGS DO NOT NED 2 BE SPECIFID EXPLICITLY IN `ASF_ARGS`.

---

## IPC

ASSUMIN U DIDNT CHANGE TEH DEFAULT VALUE 4 `IPC` **[GLOBAL CONFIGURASHUN PROPERTY](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-lol-US#global-config)**, IZ ALREADY ENABLD. HOWEVR, U MUST DO 2 ADDISHUNAL THINGS 4 IPC 2 WERK IN DOCKR CONTAINR. FIRSTLY, U MUST USE `IPCPassword` OR MODIFY DEFAULT `KnownNetworks` IN CUSTOM `IPC.config` 2 ALLOW U 2 CONNECT FRUM TEH OUTSIDE WITHOUT USIN WAN. UNLES U RLY KNOE WUT URE DOIN, JUS USE `IPCPassword`. SECONDLY, U HAS 2 MODIFY DEFAULT LISTENIN ADDRES OV `localhost`, AS DOCKR CANT ROUTE OUTSIDE TRAFFIC 2 LOOPBACK INTERFACE. AN EXAMPLE OV SETTIN DAT WILL LISTEN ON ALL INTERFACEZ WUD BE `http://*:1242`. OV COURSE, U CAN ALSO USE MOAR RESTRICTIV BINDINGS, SUCH AS LOCAL LAN OR VPN NETWORK ONLY, BUT IT HAS 2 BE ROUTE ACCESIBLE FRUM TEH OUTSIDE - `localhost` WONT DO, AS TEH ROUTE IZ ENTIRELY WITHIN GUEST MACHINE.

4 DOIN TEH ABOOV U SHUD USE **[CUSTOM IPC CONFIG](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-lol-US#custom-configurashun)** SUCH AS TEH WAN BELOW:

```json
{
    "Kestrel": {
        "Endpoints": {
            "HTTP": {
                "Url": "http://*:1242"
            }
        }
    }
}
```

ONCE WE SET UP IPC ON NON-LOOPBACK INTERFACE, WE NED 2 TELL DOCKR 2 MAP ASFS `1242/tcp` PORT EITHR WIF `-P` OR `-p` SWITCH.

4 EXAMPLE, DIS COMMAND WUD EXPOSE ASF IPC INTERFACE 2 HOST MACHINE (ONLY):

```shell
docker run -it -p 127.0.0.1:1242:1242 -p [::1]:1242:1242 --name asf --pull always justarchi/archisteamfarm
```

IF U SET EVRYTHIN PROPERLY, `docker run` COMMAND ABOOV WILL MAK **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-lol-US)** INTERFACE WERK FRUM UR HOST MACHINE, ON STANDARD `localhost:1242` ROUTE DAT IZ NAO PROPERLY REDIRECTD 2 UR GUEST MACHINE. IZ ALSO NICE 2 NOWT DAT WE DO NOT EXPOSE DIS ROUTE FURTHR, SO CONNECSHUN CAN BE DUN ONLY WITHIN DOCKR HOST, AN THEREFORE KEEPIN IT SECURE. OV COURSE, U CAN EXPOSE TEH ROUTE FURTHR IF U KNOE WUT URE DOIN AN ENSURE APPROPRIATE SECURITY MEASUREZ.

---

### COMPLETE EXAMPLE

COMBININ WHOLE KNOWLEDGE ABOOV, AN EXAMPLE OV COMPLETE SETUP WUD LOOK LIEK DIS:

```shell
docker run -p 127.0.0.1:1242:1242 -p [::1]:1242:1242 -v /home/archi/ASF/config:/app/config -v /home/archi/ASF/plugins:/app/plugins --name asf --pull always justarchi/archisteamfarm
```

DIS ASSUMEZ DAT ULL USE SINGLE ASF CONTAINR, WIF ALL ASF CONFIG FILEZ IN `/home/archi/ASF/config`. U SHUD MODIFY TEH CONFIG PATH 2 TEH WAN DAT MATCHEZ UR MACHINE. IZ ALSO POSIBLE 2 PROVIDE CUSTOM PLUGINS 4 ASF, WHICH U CAN PUT IN `/home/archi/ASF/plugins`. DIS SETUP IZ ALSO READY 4 OPSHUNAL IPC USAGE IF UVE DECIDD 2 INCLUDE `IPC.config` IN UR CONFIG DIRECTORY WIF CONTENT LIEK BELOW:

```json
{
    "Kestrel": {
        "Endpoints": {
            "HTTP": {
                "Url": "http://*:1242"
            }
        }
    }
}
```

---

## PRO TIPS

WHEN U ALREADY HAS UR ASF DOCKR CONTAINR READY, U DOAN HAS 2 USE `docker run` EVRY TIEM. U CAN EASILY STOP/START ASF DOCKR CONTAINR WIF `docker stop asf` AN `docker start asf`. KEEP IN MIND DAT IF URE NOT USIN `latest` TAG DEN USIN UP-2-DATE ASF WILL STILL REQUIRE FRUM U 2 `docker stop`, `docker rm` AN `docker run` AGAIN. DIS AR TEH CUZ U MUST REBUILD UR CONTAINR FRUM FRESH ASF DOCKR IMAGE EVRY TIEM U WANTS 2 USE ASF VERSHUN INCLUDD IN DAT IMAGE. IN `latest` TAG, ASF HAS INCLUDD CAPABILITY 2 AUTO-UPDATE ITSELF, SO REBUILDIN TEH IMAGE IZ NOT NECESARY 4 USIN UP-2-DATE ASF (BUT IZ STILL GUD IDEA 2 DO IT FRUM TIEM 2 TIEM IN ORDR 2 USE FRESH .NET RUNTIME DEPENDENCIEZ AN TEH UNDERLYIN OS, WHICH MITE BE NEEDD WHEN JUMPIN ACROS MAJOR ASF VERSHUN UPDATEZ).

AS HINTD BY ABOOV, ASF IN TAG OTHR THAN `latest` WONT AUTOMATICALLY UPDATE ITSELF, WHICH MEANZ DAT **U** R IN CHARGE OV USIN UP-2-DATE `justarchi/archisteamfarm` REPO. DIS HAS LOTZ DA ADVANTAGEZ AS TYPICALLY TEH APP SHUD NOT TOUCH ITZ OWN CODE WHEN BEAN RUN, BUT WE ALSO UNDERSTAND CONVENIENCE DAT COMEZ FRUM NOT HAVIN 2 WORRY BOUT ASF VERSHUN IN UR DOCKR CONTAINR. IF U CARE BOUT GUD PRACTICEZ AN PROPR DOCKR USAGE, `released` TAG IZ WUT WED SUGGEST INSTEAD OV `latest`, BUT IF U CANT BE BOTHERD WIF IT AN U JUS WANTS 2 MAK ASF BOTH WERK AN AUTO-UPDATE ITSELF, DEN `latest` WILL DO.

U SHUD TYPICALLY RUN ASF IN DOCKR CONTAINR WIF `Headless: true` GLOBAL SETTIN. DIS WILL CLEARLY TELL ASF DAT URE NOT HER 2 PROVIDE MISIN DETAILS AN IT SHUD NOT ASK 4 DOSE. OV COURSE, 4 INITIAL SETUP U SHUD CONSIDR LEAVIN DAT OPSHUN AT `false` SO U CAN EASILY SET UP THINGS, BUT IN LONG-RUN URE TYPICALLY NOT ATTACHD 2 ASF CONSOLE, THEREFORE ITD MAK SENSE 2 INFORM ASF BOUT DAT AN USE `input` **[COMMAND](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-lol-US)** IF NED ARISEZ. DIS WAI ASF WONT HAS 2 WAIT INFINITELY 4 USR INPUT DAT WILL NOT HAPPEN (AN WASTE RESOURCEZ WHILE DOIN SO). IT WILL ALSO ALLOW ASF 2 RUN IN NON-INTERACTIV MODE INSIDE CONTAINR, WHICH IZ CRUSHUL E.G. IN REGARDZ 2 FORWARDIN SIGNALS, MAKIN IT POSIBLE 4 ASF 2 GRACEFULLY CLOSE ON `docker stop asf` REQUEST.
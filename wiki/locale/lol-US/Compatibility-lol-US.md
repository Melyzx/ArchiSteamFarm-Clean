# COMPATIBILITY

ASF IZ C# APPLICASHUN DAT IZ RUNNIN ON .NET PLATFORM. DIS MEANZ DAT ASF IZ NOT COMPILD DIRECTLY INTO **[MACHINE CODE](https://en.wikipedia.org/wiki/Machine_code)** DAT IZ RUNNIN ON UR CPU, BUT INTO **[CIL](https://en.wikipedia.org/wiki/Common_Intermediate_Language)** DAT REQUIREZ CIL-COMPATIBLE RUNTIME 4 EXECUTIN IT.

DIS APPROACH HAS GIGANTIC AMOUNT OV ADVANTAGEZ, AS CIL IZ PLATFORM-INDEPENDENT, WHICH IZ Y ASF CAN RUN NATIVELY ON LOTZ DA AVAILABLE OSEZ, ESPECIALLY WINDOWS, LINUX AN MACOS. THAR IZ NOT ONLY NO EMULASHUN NEEDD, BUT ALSO SUPPORT 4 ALL PLATFORM-RELATD AN HARDWARE-RELATD OPTIMIZASHUNS, SUCH AS CPU SSE INSTRUCSHUNS. THX 2 DAT, ASF CAN ACHIEVE SUPERIOR PERFORMANCE AN OPTIMIZASHUN, WHILE STILL OFFERIN PERFIK COMPATIBILITY AN RELIABILITY.

DIS ALSO MEANZ DAT ASF HAS **NO SPECIFIC OS REQUIREMENT**, CUZ IT REQUIREZ WERKIN **RUNTIME** ON DAT OS AN NOT OS ITSELF. AS LONG AS DAT RUNTIME IZ EXECUTIN ASF CODE PROPERLY, IT DOEZ NOT MATTR WHETHR UNDERLYIN OS IZ WINDOWS, LINUX, MACOS, BSD, SONY PLAYSTASHUN 4, NINTENDO WII OR UR TOASTR - AS LONG AS THAR IZ **[.NET 4 IT](https://dotnet.microsoft.com/download/dotnet)**, THAR IZ **[ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** 4 IT (IN `generic` VARIANT).

HOWEVR, REGARDLES OV WER U RUN ASF, U MUST ENSURE DAT UR TARGET PLATFORM HAS **[.NET PREREQUIZIETS](https://github.com/dotnet/core/blob/main/Documentation/prereqs.md)** INSTALLD. DOSE R LOW-LEVEL LIBRARIEZ REQUIRD 4 PROPR RUNTIME FUNCSHUNALITY AN ABSOLUTELY CORE 4 ASF 2 WERK IN DA FURST PLACE. VRY LIKELY U CAN HAS SUM OV THEM (OR EVEN ALL) ALREADY INSTALLD.

---

## ASF PACKAGIN

ASF COMEZ IN 2 MAIN FLAVOURS - GENERIC PACKAGE AN OS-SPECIFIC. FUNCSHUNALITY-WIZE BOTH PACKAGEZ R EGSAKTLY TEH SAME, THEYRE BOTH ALSO CAPABLE OV AUTOMATICALLY UPDATIN THEMSELVEZ. TEH ONLY DIFFERENCE TWEEN THEM IZ WHETHR OR NOT ASF **GENERIC** PACKAGE ALSO COMEZ WIF **OS-SPECIFIC** RUNTIME 2 POWR IT.

---

### GENERIC

GENERIC PACKAGE IZ PLATFORM-AGNOSTIC BUILD DAT DOESNT INCLUDE ANY MACHINE-SPECIFIC CODE. DIS SETUP REQUIREZ FRUM U 2 HAS .NET RUNTIME ALREADY INSTALLD ON UR OS **IN APPROPRIATE VERSHUN**. WE ALL KNOE HOW TROUBLESOME IT 2 KEEP DEPENDENCIEZ UP-2-DATE, THEREFORE DIS PACKAGE IZ HER MAINLY 4 PEEPS DAT **ALREADY USE** .NET AN DOAN WANTS 2 DUPLICATE THEIR RUNTIME SOLELY 4 ASF IF THEY CAN MAK USE OV WUT THEY HAS INSTALLD ALREADY. GENERIC PACKAGE ALSO ALLOWS U 2 RUN ASF **ANYWHERE, AS LONG AS U CAN OBTAIN WERKIN IMPLEMENTASHUN OV .NET RUNTIME**, REGARDLES IF THAR EXISTS OS-SPECIFIC ASF BUILD 4 IT, OR NOT.

IZ NOT RECOMMENDD 2 USE GENERIC FLAVR IF URE CASUAL OR EVEN ADVANCD USR DAT JUS WANTS 2 MAK ASF WERK AN NOT DIG INTO .NET TECHNICAL DETAILS. IN OTHR WERDZ - IF U KNOE WUT DIS AR TEH, U CAN USE IT, OTHERWIZE IZ MUTCH BETTR 2 USE OS-SPECIFIC PACKAGE EXPLAIND BELOW.

---

### OS-SPECIFIC

OS-SPECIFIC PACKAGE, APART FRUM MANAGD CODE INCLUDD IN GENERIC PACKAGE, ALSO INCLUDEZ NATIV CODE 4 GIVEN PLATFORM. IN OTHR WERDZ, OS-SPECIFIC PACKAGE **ALREADY INCLUDEZ PROPR .NET RUNTIME INSIDE**, WHICH ALLOWS U 2 ENTIRELY SKIP TEH WHOLE INSTALLASHUN MES AN JUS LAUNCH ASF DIRECTLY. OS-SPECIFIC PACKAGE, AS U CAN GUES FRUM TEH NAYM, IZ OS-SPECIFIC AN EVRY OS REQUIREZ ITZ OWN VERSHUN - 4 EXAMPLE WINDOWS REQUIREZ PE32+ `ArchiSteamFarm.exe` BINARY WHILE LINUX WERKZ WIF UNIX ELF `ArchiSteamFarm` BINARY. AS U CUD KNOE, DOSE 2 TYPEZ R NOT COMPATIBLE WIF EACH OTHR.

ASF IZ CURRENTLY AVAILABLE IN FOLLOWIN OS-SPECIFIC VARIANTS:

- `linux-arm` WERKZ ON 32-BIT ARM-BASD (ARMV7+) GNU/LINUX OSEZ WIF GLIBC 2.35/MUSL 1.2.2 AN NEWR. DIS VARIANT COVERS PLATFORMS SUCH AS RASPBERRY PI 2 (AN NEWR), IT WILL **NOT** WERK WIF OLDR ARM ARCHITECTUREZ, SUCH AS ARMV6 FINDZ IN RASPBERRY PI 0 & 1, IT WILL ALSO NOT WERK WIF OSEZ DAT DO NOT IMPLEMENT REQUIRD GNU/LINUX ENVIRONMENT (SUCH AS ANDROID).
- `linux-arm64` WERKZ ON 64-BIT ARM-BASD (ARMV8+) GNU/LINUX OSEZ WIF GLIBC 2.23/MUSL 1.2.2 AN NEWR. DIS VARIANT COVERS PLATFORMS SUCH AS RASPBERRY PI 3 (AN NEWR), IT WILL **NOT** WERK WIF 32-BIT OSEZ DAT DO NOT HAS REQUIRD 64-BIT LIBRARIEZ AVAILABLE (SUCH AS 32-BIT RASPBERRY PI OS), IT WILL ALSO NOT WERK WIF OSEZ DAT DO NOT IMPLEMENT REQUIRD GNU/LINUX ENVIRONMENT (SUCH AS ANDROID).
- `linux-x64` WERKZ ON 64-BIT GNU/LINUX OSEZ WIF GLIBC 2.23/MUSL 1.2.2 AN NEWR.
- `osx-arm64` WERKZ ON 64-BIT ARM-BASD (APPLE SILICON) MACOS OSEZ IN VERSHUN 13 AN NEWR.
- `osx-x64` WERKZ ON 64-BIT MACOS OSEZ IN VERSHUN 13 AN NEWR.
- `win-arm64` WERKZ ON 64-BIT ARM-BASD (ARMV8+) WINDOWS OSEZ IN VERSHUN 10, 11 AN NEWR.
- `win-x64` WERKZ ON 64-BIT WINDOWS OSEZ IN VERSHUN 10, 11, SERVR 2012+ AN NEWR.

OV COURSE, EVEN IF U DOAN HAS OS-SPECIFIC PACKAGE AVAILABLE 4 UR OS-ARCHITECCHUR COMBINASHUN, U CAN ALWAYS INSTALL APPROPRIATE .NET RUNTIME YOURSELF AN RUN GENERIC ASF FLAVR, WHICH IZ ALSO TEH MAIN REASON Y IT EXISTS IN DA FURST PLACE. GENERIC ASF BUILD IZ PLATFORM-AGNOSTIC AN WILL RUN ON ANY PLATFORM DAT HAS WERKIN .NET RUNTIME. DIS AR TEH IMPORTANT 2 NOWT - ASF REQUIREZ .NET RUNTIME, NOT SUM SPECIFIC OS OR ARCHITECCHUR. 4 EXAMPLE, IF URE RUNNIN 32-BIT WINDOWS DEN DESPITE OV NO DEDICATD `win-x86` ASF VERSHUN, U CAN STILL INSTALL .NET SDK IN `win-x86` VERSHUN AN RUN GENERIC ASF JUS FINE. WE SIMPLY CANT TARGET EVRY OS-ARCHITECCHUR COMBINASHUN DAT EXISTS AN IZ USD BY SOMEBODY, SO WE HAS 2 DRAW LINE SOMEWHERE. X86 IZ GUD EXAMPLE OV DAT LINE, AS IZ OBSOLETE ARCHITECCHUR SINCE AT LEAST 2004.

4 COMPLETE LIST OV ALL SUPPORTD PLATFORMS AN OSEZ BY .NET 9.0, VISIT **[RELEASE NOTEZ](https://github.com/dotnet/core/blob/main/release-notes/9.0/supported-os.md)**.

---

## RUNTIME REQUIREMENTS

IF URE USIN OS-SPECIFIC PACKAGE DEN U DOAN NED 2 WORRY BOUT RUNTIME REQUIREMENTS, CUZ ASF ALWAYS SHIPS WIF REQUIRD AN UP-2-DATE RUNTIME DAT WILL WERK PROPERLY AS LONG AS U HAS **[.NET PREREQUIZIETS](https://github.com/dotnet/core/blob/main/Documentation/prereqs.md)** INSTALLD AN UP-2-DATE. IN OTHR WERDZ, **U DOAN NED 2 INSTALL .NET RUNTIME OR SDK**, AS OS-SPECIFIC BUILDZ REQUIRE ONLY NATIV OS DEPENDENCIEZ (PREREQUIZIETS) AN NOTHIN ELSE.

HOWEVR, IF URE TRYIN 2 RUN **GENERIC** ASF PACKAGE DEN U MUST ENSURE DAT UR .NET RUNTIME SUPPORTS PLATFORM REQUIRD BY ASF.

ASF AS PROGRAM IZ TARGETIN **.NET 9.0** (`net9.0`) RITE NAO, BUT IT CUD TARGET NEWR PLATFORM IN DA FUCHUR. `net9.0` IZ SUPPORTD SINCE 9.0.100 SDK (9.0.0 RUNTIME), ALTHOUGH ASF MITE PREFR **LATEST RUNTIME AT TEH MOMENT OV COMPILASHUN**, SO U SHUD ENSURE DAT U HAS **[LATEST SDK](https://dotnet.microsoft.com/download)** (OR AT LEAST RUNTIME) AVAILABLE 4 UR MACHINE. GENERIC ASF VARIANT CUD REFUSE 2 LAUNCH IF UR RUNTIME IZ OLDR THAN TEH SPECIFID MINIMUM SUPPORTD WAN DURIN COMPILASHUN.

IF IN DOUBT, CHECK WUT R **[CONTINUOUS INTEGRASHUN USEZ](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/publish.yml?query=branch%3Amain)** 4 COMPILIN AN DEPLOYIN ASF RELEASEZ ON GITHUB. U CAN FIND `dotnet --info` OUTPUT IN EVRY BUILD AS PART OV .NET VERIFICASHUN STEP.
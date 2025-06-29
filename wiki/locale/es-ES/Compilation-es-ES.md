# Compilación

La compilación es el proceso de crear un archivo ejecutable. Esto es lo que necesitas hacer si deseas añadir tus propios cambios a ASF, o si por cualquier razón no confías en los archivos ejecutables proporcionados en las **[versiones](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** oficiales. Si eres usuario y no un desarrollador, lo más probable es que quieras usar ejecutables ya compilados, pero si quieres usar los tuyos propios, a aprender algo nuevo, continua leyendo.

ASF puede ser compilado en cualquier plataforma soportada actualmente, siempre que tengas todas las herramientas para hacerlo.

---

## .NET SDK

Independientemente de la plataforma, necesitas .NET SDK completo (no solo runtime) para compilar ASF. Las instrucciones para la instalación se pueden encontrar en la **[página de descarga de .NET](https://dotnet.microsoft.com/download)**. Necesitas instalar la versión apropiada de .NET SDK para tu sistema operativo. Después de una instalación exitosa, el comando `dotnet` debería estar funcionando y operativo. Puedes verificar si funciona con `dotnet --info`. También asegúrate de que tu .NET SDK coincida con los **[requisitos de runtime](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-es-ES#requisitos-de-runtime)** de ASF.

---

## Compilación

Suponiendo que tienes .NET SDK operativo y en la versión apropiada, simplemente navega al directorio fuente de ASF (repositorio de ASF clonado o descargado y desempaquetado) y ejecuta:

```shell
dotnet publish ArchiSteamFarm -c "Release" -o "out/generic"
```

Si usas Linux/macOS, en su lugar puedes usar el script `cc.sh` que hará lo mismo, de forma un poco más compleja.

Si la compilación terminó con éxito, podrás encontrar ASF en su variante `source` en el directorio `out/generic`. Esto es lo mismo que la compilación `generic` oficial de ASF, pero está forzado a `UpdateChannel` y `UpdatePeriod` de `0`, lo que es apropiado para versiones autocompiladas.

### Sistema operativo específico

Si tienes una necesidad específica también puedes generar un paquete .NET específico para un sistema operativo. En general no deberías hacer eso porque acabas de compilar la variante `generic` que puedes ejecutar con el ya instalado .NET runtime que usaste para la compilación en primer lugar, pero en caso de que lo quieras hacer:

```shell
dotnet publish ArchiSteamFarm -c "Release" -o "out/linux-x64" -r "linux-x64" --self-contained
```

Por supuesto, reemplaza `linux-x64` con la arquitectura del sistema operativo que tienes por objetivo, tal como `win-x64`. Esta compilación también tendrá las actualizaciones deshabilitadas. Al compilar `--self-contained` opcionalmente también puedes declarar dos interruptores más: `-p:PublishTrimmed=true` producirá una compilación recortada, y `-p:PublishSingleFile=true` producirá un solo archivo. Añadir ambos resultará en la misma configuración que usamos en nuestras compilaciones.

### ASF-ui

Aunque los pasos anteriores son todo lo necesario para tener una compilación de ASF completamente funcional, *también* podría interesarte compilar **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-es-ES#asf-ui)**, nuestra interfaz web gráfica. Desde el lado de ASF, todo lo que necesitas hacer es poner una compilación de ASF-ui en la ubicación estándar `ASF-ui/dist`, luego compilar ASF con ella (de nuevo, si es necesario).

ASF-ui es parte del árbol fuente de ASF como un **[submódulo git](https://git-scm.com/book/en/v2/Git-Tools-Submodules)**, asegúrate de que has clonado el repositorio con `git clone --recursive`, ya que de lo contrario no tendrás los archivos necesarios. También necesitarás un NPM funcional, **[Node.js](https://nodejs.org)** viene con él. Si estás usando Linux/macOS, recomendamos nuestro script `cc.sh`, que automáticamente cubrirá la compilación y envío de ASF-ui (si es posible, es decir, si cumples con los requisitos que acabamos de mencionar).

Además del script `cc.sh`, abajo también adjuntamos las instrucciones de compilación simplificada, consulta **[ASF-ui repo](https://github.com/JustArchiNET/ASF-ui)** para documentación adicional. Desde la ubicación del árbol fuente de ASF, así como arriba, ejecuta los siguientes comandos:

```shell
rm -rf "ASF-ui/dist" # ASF-ui no limpia sus archivos después de la compilación antigua

npm ci --prefix ASF-ui
npm run-script deploy --prefix ASF-ui

rm -rf "out/generic/www" # Asegura que la salida de nuestra compilación limpie los archivos viejos
dotnet publish ArchiSteamFarm -c "Release" -o "out/generic" # O de acuerdo a lo que necesites según lo anterior
```

Ahora deberías ser capaz de encontrar los archivos de ASF-ui en tu carpeta `out/generic/www`. ASF podrá enviar esos archivos a tu navegador.

Alternativamente, puedes compilar solamente ASF-ui, ya sea manualmente o con la ayuda de nuestro repositorio, luego copia manualmente la compilación a la carpeta `${OUT}/www`, donde `${OUT}` es la carpeta de salida de ASF que especificaste con el parámetro `-o`. Esto es exactamente lo que ASF está haciendo como parte del proceso de compilación, copia `ASF-ui/dist` (si existe) a `${OUT}/www`, nada extraño.

---

## Desarrollo

Si deseas editar el código de ASF, para ello puedes usar cualquier IDE (entorno de desarrollo integrado) compatible con .NET, aunque incluso eso es opcional, ya que también puedes editarlo con un bloc de notas y compilarlo con el comando `dotnet` descrito arriba.

Si no tienes una mejor opción, recomendamos el **[más reciente Visual Studio Code](https://code.visualstudio.com/download)**, el cual es suficiente incluso para las necesidades más avanzadas. Por supuesto, puedes usar cualquiera que prefieras, para referencia nosotros usamos **[JetBrains Rider](https://www.jetbrains.com/rider)** para el desarrollo de ASF, aunque no es una solución gratuita.

---

## Etiquetas

La rama `main` no garantiza estar en un estado que permita una compilación exitosa o una ejecución de ASF sin fallas, ya que es una rama de desarrollo como indicamos en nuestro **[ciclo de lanzamiento](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-es-ES)**. Si quieres compilar o referenciar ASF desde la fuente, entonces debes usar la **[etiqueta](https://github.com/JustArchiNET/ArchiSteamFarm/tags)** apropiada para ese fin, lo que garantiza una compilación exitosa, y muy probablemente también una ejecución sin fallas (si la compilación fue marcada como versión estable). Para comprobar la "salud" actual del árbol, puedes usar nuestra integración continua - **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/ci.yml?query=branch%3Amain)**.

---

## Versiones oficiales

Las versiones oficiales de ASF son compiladas por **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)** con el último .NET SDK que coincida con los **[requisitos de runtime](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-es-ES#requisitos-de-runtime)** de ASF. Después de pasar las pruebas, todos los paquetes son distribuidos como la versión, también en GitHub. Esto también garantiza transparencia, ya que GitHub siempre usa una fuente pública oficial para todas las compilaciones, y puedes comparar las sumas de verificación (checksums) de los artefactos de GitHub con los recursos de publicación de GitHub. Los desarrolladores de ASF no compilan ni publican versiones por cuenta propia, excepto para el proceso de desarrollo privado y depuración.

Adicionalmente, los mantenedores de ASF validan y publican manualmente las sumas de verificación de las compilaciones en un servidor remoto de ASF, independiente de GitHub, como medida de seguridad adicional. Este paso es obligatorio para que los ASF existentes consideren la versión como un candidato válido para la funcionalidad de actualización automática.
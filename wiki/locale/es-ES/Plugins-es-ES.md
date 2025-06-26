# Plugins

ASF incluye soporte para plugins personalizados que pueden ser cargados durante el tiempo de ejecución. Los plugins permiten personalizar el comportamiento de ASF, por ejemplo añadiendo comandos personalizados, lógica de intercambio personalizada o integración con servicios de terceros y APIs.

Esta página describe los plugins de ASF desde la perspectiva de los usuarios - explicación, uso y demás. Si deseas ver la perspectiva de desarrollador, dirígete **[aquí](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-development-es-ES)**.

---

## Uso

ASF carga los plugins desde el directorio `plugins` ubicado en tu carpeta de ASF. Es una práctica recomendada (que se vuelve obligatoria con las actualizaciones automáticas de los plugins) mantener un directorio dedicado para cada plugin que desees usar, que puede estar basado en su nombre, como `MyPlugin`. Hacerlo de este modo resultará en la estructura final de `plugins/MyPlugin`. Finalmente, todos los archivos binarios deben ser colocados dentro de esa carpeta dedicada, y ASF descubrirá y utilizará tu plugin después de reiniciar.

Generalmente los desarrolladores de plugins los publican en forma de archivo `zip` con los binarios dentro, lo que significa que debes descomprimir ese archivo zip en su propio subdirectorio dedicado dentro del directorio `plugins`.

Si el plugin se cargó con éxito, verás su nombre y versión en el registro. Debes consultar al desarrollador de tu plugin en caso de preguntas, problemas o uso relacionados con los plugins que hayas decidido usar.

Puedes encontrar algunos plugins destacados en nuestra sección **[aplicaciones de terceros](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party-es-ES#plugins-de-asf)**.

**Por favor, ten en cuenta que los plugins de ASF podrían ser maliciosos**. Siempre debes asegurarte de que estás usando plugins hechos por desarrolladores en los que puedes confiar, incluso los de la sección de aplicaciones de terceros mencionada previamente. Los desarrolladores de ASF ya no pueden garantizar los beneficios usuales de ASF (como ausencia de malware y ser libre de VAC) si decides usar cualquier plugin personalizado. Debes entender que una vez cargados, los plugins tienen control total sobre el proceso de ASF, por ello tampoco podemos ofrecer soporte a configuraciones que utilicen plugins personalizados, ya que no estás ejecutando una versión "vainilla" del código de ASF.

---

## Compatibilidad

Dependiendo de la complejidad, alcance y muchos otros factores, es posible que requiera que uses la variante **[generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up-es-ES#configuración-genérica)**, en lugar de la recomendada de **[sistema operativo específico](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up-es-ES#configuración-de-sistema-operativo-específico)**. Esto debido a que la variante de sistema operativo específico solo viene con las funcionalidades requeridas para el propio ASF, y tu plugin podría requerir partes que están fuera del alcance principal de ASF, resultando en que no sea complatible con compilaciones recortadas para sistema operativo específico.

En general, al usar plugins de terceros, recomendamos usar la variante genérica de ASF para máxima compatibilidad. Sin embargo, no todos los plugins podrían requerirlo - consulta la información de tu plugin para saber si necesitas usar la variante genérica o no.
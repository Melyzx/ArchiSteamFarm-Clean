# Desarrollo de plugins

ASF incluye soporte para plugins personalizados que pueden ser cargados durante el tiempo de ejecución. Los plugins permiten personalizar el comportamiento de ASF, por ejemplo añadiendo comandos personalizados, lógica de intercambio personalizada o integración con servicios de terceros y APIs.

Esta página describe los plugins de ASF desde la perspectiva de los desarrolladores - creación, mantenimiento, publicación y demás. Si deseas ver la perspectiva del usuario, dirígete **[aquí](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-es-ES)**.

---

## Principal

Los plugins son bibliotecas .NET estándar que definen una clase heredada de la interfaz común `IPlugin` declarada en ASF. Puedes desarrollar plugins de forma totalmente independiente de la línea principal de ASF y reutilizarlos en la versión actual y futuras de ASF, siempre que la API interna de ASF siga siendo compatible. El sistema de plugins usado en ASF está basado en `System.Composition`, anteriormente conocido como **[Managed Extensibility Framework](https://docs.microsoft.com/es-es/dotnet/framework/mef)** que permite a ASF descubrir y cargar tus bibliotecas durante el tiempo de ejecución.

---

### Comenzando

Hemos preparado la plantilla **[ASF-PluginTemplate](https://github.com/JustArchiNET/ASF-PluginTemplate)**, que puedes (y deberías) usar como base para tu proyecto de plugin. Usar la plantilla **no es un requisito** (ya que puedes hacer todo desde cero), pero recomendamos hacerlo ya que puede dar un impulso inicial significativo a tu desarrollo y reducir el tiempo requerido para hacer todo bien. Simplemente revisa el **[README](https://github.com/JustArchiNET/ASF-PluginTemplate/blob/main/README.md)** de la plantilla para guiarte. Independientemente, cubriremos lo básico a continuación en caso de que desees empezar desde cero, o quieras entender mejor los conceptos usados en la plantilla de plugin - normalmente no necesitas hacer nada de eso si decides usar nuestra plantilla.

Tu proyecto debe ser una biblioteca estándar .NET que tenga como objetivo el framework apropiado para tu versión objetivo de ASF, como se especifica en la sección de **[compilación](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compilation-es-ES)**.

El proyecto debe hacer referencia al ensamblado principal `ArchiSteamFarm`, ya sea una biblioteca precompilada `ArchiSteamFarm.dll` que hayas descargado como parte de la versión, o el proyecto fuente (por ejemplo, si decidiste añadir el árbol ASF como un submódulo). Esto te permitirá acceder y descubrir estructuras, métodos y propiedades de ASF, especialmente la interfaz principal `IPlugin` que necesitarás para heredar como se indica en el siguiente paso. El proyecto también debe referenciar `System.Composition.AttributedModel` como mínimo, lo que te permite `[Export]` (exportar) tu `IPlugin` para que ASF utilice. Además, tal vez quieras/necesites referenciar otras bibliotecas comunes para interpretar las estructuras de datos que se te presentan en algunas interfaces, pero a menos que las necesites explícitamente, eso sería suficiente por ahora.

Si hiciste todo correctamente, tu `csproj` será similar al siguiente:

```csproj
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net9.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <!-- Since you're loading plugin into the ASF process, which already includes this dependency, IncludeAssets="compile" allows you to omit it from the final output -->
    <PackageReference Include="System.Composition.AttributedModel" IncludeAssets="compile" Version="9.0.0" />
  </ItemGroup>

  <ItemGroup>
    <ProjectReference Include="C:\\Path\To\ArchiSteamFarm\ArchiSteamFarm.csproj" ExcludeAssets="all" Private="false" />

    <!-- If building with downloaded DLL binary, use this instead of <ProjectReference> above -->
    <!-- <Reference Include="ArchiSteamFarm" HintPath="C:\\Path\To\Downloaded\ArchiSteamFarm.dll" /> -->
  </ItemGroup>
</Project>
```

Por el lado del código, la clase de tu plugin debe heredar de la interfaz `IPlugin` (ya sea explícitamente, o implícitamente heredando de una interfaz más especializada, tal como `IASF`) y `[Export(typeof(IPlugin))]` para ser reconocido por ASF durante el tiempo de ejecución. El ejemplo más sencillo que consigue eso sería el siguiente:

```csharp
using System;
using System.Composition;
using System.Threading.Tasks;
using ArchiSteamFarm;
using ArchiSteamFarm.Plugins;

namespace YourNamespace.YourPluginName;

[Export(typeof(IPlugin))]
public sealed class YourPluginName : IPlugin {
	public string Name => nameof(YourPluginName);
	public Version Version => typeof(YourPluginName).Assembly.GetName().Version;

	public Task OnLoaded() {
		ASF.ArchiLogger.LogGenericInfo("Hello World!");

		return Task.CompletedTask;
	}
}
```

Para usar tu plugin, primero debes compilarlo. Puedes hacerlo ya sea desde tu IDE (Entorno de Desarrollo Integrado), o desde el directorio raíz de tu proyecto mediante un comando:

```shell
# Si tu proyecto es individual (no hay necesidad de definir su nombre ya que es el único)
dotnet publish -c "Release" -o "out"

# Si tu proyecto es parte del árbol fuente de ASF (para evitar la compilación de partes innecesarias)
dotnet publish YourPluginName -c "Release" -o "out"
```

Después, tu plugin está listo para usarse. Depende de ti decidir cómo deseas distribuir y publicar tu plugin, pero recomendamos crear un archivo zip donde pongas tu plugin compilado junto con sus **[dependencias](#dependencias-de-plugin)**. De esta manera el usuario simplemente necesitará descomprimir el archivo zip en un subdirectorio independiente dentro de su directorio `plugins` y nada más.

Este solo es el escenario más básico para empezar. Tenemos el proyecto **[`ExamplePlugin`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.CustomPlugins.ExamplePlugin)** que muestra interfaces de ejemplo y acciones que puedes hacer dentro de tu plugin, incluyendo comentarios útiles. Siéntete libre de echar un vistazo si quieres aprender de un código funcional, o descubrir el namespace (espacio de nombres) `ArchiSteamFarm.Plugins` y dirígete a la documentación incluida para todas las opciones disponibles. También profundizaremos en algunos conceptos básicos a continuación para explicarlos mejor.

Si en lugar del plugin de ejemplo deseas aprender de proyectos reales, hay varios plugins oficiales desarrollados por nosotros, por ejemplo, **[`ItemsMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.ItemsMatcher)**, **[`MobileAuthenticator`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.MobileAuthenticator)** o **[`SteamTokenDumper`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.SteamTokenDumper)**. Además, hay varios plugins hechos por otros desarrolladores, en nuestra sección de **[terceros](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party-es-ES#plugins-de-asf)**

---

### Disponibilidad de API

ASF, además de a lo que tienes acceso en las interfaces, te expone un montón de APIs internas de las que puedes hacer uso, para ampliar la funcionalidad. Por ejemplo, si quisieras enviar algún tipo de nueva solicitud a la web de Steam, entonces no necesitas implementar todo desde cero, especialmente al tratar con todos los problemas con los que hemos tenido que tratar antes que tú. Simplemente usa nuestro `Bot.ArchiWebHandler` el cual ya expone un montón de métodos `UrlWithSession()` para que utilices, manejando por ti todas las cosas de bajo nivel, como la autenticación, actualización de sesión o limitación web. Del mismo modo, para enviar solicitudes web fuera de la plataforma Steam, podrías usar la clase estándar .NET `HttpClient`, pero es mucho mejor idea usar `Bot.ArchiWebHandler.WebBrowser` que está disponible, lo que una vez más ofrece ayuda, por ejemplo en lo que respecta a reintentar solicitudes fallidas.

Tenemos una política muy abierta en términos de la disponibilidad de nuestra API, si deseas usar algo incluido en el código de ASF, simplemente **[abre un reporte](https://github.com/JustArchiNET/ArchiSteamFarm/issues)** y explica tu caso de uso planeado para nuestra API interna. Seguramente no tendremos nada en contra, siempre que tu uso previsto tenga sentido. Esto también incluye todas las sugerencias respecto a nuevas interfaces `IPlugin` que podría tener sentido añadir para ampliar la funcionalidad existente.

Independientemente de la disponibilidad de la API de ASF, nada te impide, por ejemplo incluir la biblioteca `Discord.Net` en tu aplicación y crear un puente entre tu bot de Discord y los comandos de ASF, ya que tu plugin también puede tener sus propias dependencias. Las posibilidades son infinitas, y hemos hecho todo lo posible para darte tanta libertad y flexibilidad como sea posible dentro de tu plugin, así que no hay límites artificiales - tu plugin está cargado en el proceso principal de ASF y puede hacer cualquier cosa que sea realistamente posible hacer desde el código C#.

---

### Compatibilidad de API

Es importante destacar que ASF es una aplicación de consumo y no una biblioteca típica con una superficie de API fija de la que puedes depender incondicionalmente. Esto significa que no puedes suponer que tu plugin una vez compilado seguirá funcionando con todas las futuras versiones de ASF, es simplemente imposible si queremos seguir desarrollando el programa, y ser incapaces de adaptarse a los constantes cambios de Steam por el bien de la retrocompatibilidad simplemente no es adecuado para nuestro caso. Esto debería ser lógico para ti, pero es importante destacar ese hecho.

Haremos todo lo posible para mantener las partes públicas de ASF funcionales y estables, pero no dudaremos en romper la compatibilidad si surgen buenas razones para ello, siguiendo nuestra política de **[obsolescencia](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Deprecation-es-ES)** en el proceso. Esto es especialmente importante en lo que se refiere a las estructuras internas de ASF que están expuestas como parte de su infraestructura, (por ejemplo, `ArchiWebHandler`) que podría ser mejorada (y por lo tanto reescrita) como parte de las mejoras de ASF en una de las versiones futuras. Haremos todo lo posible para informarte adecuadamente en los registros de cambio, e incluir las advertencias apropiadas durante el tiempo de ejecución sobre las características obsoletas. No pretendemos reescribir todo solo por reescribirlo, así que puedes estar bastante seguro de que la siguiente versión menor de ASF simplemente no destruirá tu plugin solo porque tiene un número de versión superior, pero es buena idea estar pendiente de los registros de cambio y verificar ocasionalmente que todo funciona bien.

---

### Dependencias de plugin

Tu plugin incluirá al menos dos dependencias por defecto, referencia de `ArchiSteamFarm` para la API interna (`IPlugin` como mínimo), y `PackageReference` de `System.Composition.AttributedModel` requerida para ser reconocida como un plugin de ASF (cláusula `[Export]`). Además, podría incluir más dependencias de acuerdo a lo que hayas decidido hacer en tu plugin (por ejemplo, la biblioteca `Discord.Net` si decidiste integrar con Discord).

La salida de tu compilación incluirá tu biblioteca `YourPluginName.dll`, así como todas las dependencias que hayas referenciado. Como estás desarrollando un plugin para un programa ya funcional, no tienes que, e incluso **no deberías** incluir dependencias que ASF ya incluye, por ejemplo `ArchiSteamFarm`, `SteamKit2` o `AngleSharp`. Reducir las dependencias de tu compilación compartidas con ASF no es un requisito absoluto para que funcione tu plugin, pero hacerlo reducirá drásticamente la huella de memoria y el tamaño de tu plugin, además de aumentar el rendimiento, debido al hecho de que ASF compartirá sus propias dependencias, y cargará solo aquellas bibliotecas que no conozca.

En general, es una práctica recomendada incluir solo las bibliotecas que ASF no incluye, o las incluye en la versión incorrecta/incompatible. Ejemplos de esto sería obviamente `YourPluginName.dll`, pero como ejemplo también está `Discord.Net.dll` si decidiste depender de ella, ya que ASF no la incluye. Agrupar bibliotecas que se comparten con ASF aún puede tener sentido si quieres asegurar la compatibilidad de API (por ejemplo, asegurar que `AngleSharp` del que dependes en tu plugin siempre estará en la versión `X` y no aquella con la que se publica ASF), obviamente hacer eso viene con un precio de memoria/tamaño mayor y peor rendimiento, y por lo tanto debe ser evaluado cuidadosamente.

Si sabes que la dependencia que necesitas está incluida en ASF, puedes marcarla con `IncludeAssets="compile"` como mostramos en el ejemplo `csproj` anteriormente. Esto le indicará al compilador que evite publicar la biblioteca referenciada, ya que ASF la incluye. Del mismo modo, ten en cuenta que referenciamos el proyecto ASF con `ExcludeAssets="all" Private="false"` lo que funciona de una forma muy similar - indicándole al compilador que no produzca ningún archivo de ASF (puesto que el usuario ya los tiene). Esto solo aplica al hacer referencia al proyecto ASF, ya que si referencias una biblioteca `dll`, entonces no estás produciendo archivos de ASF como parte de tu plugin.

Si estás confundido por la declaración anterior y no tienes una mejor idea, comprueba qué bibliotecas `dll` están incluidas en el paquete `ASF-generic.zip` y asegúrate de que tu plugin incluye solo las que todavía no son parte de él. Esto solo será `YourPluginName.dll` para los plugins más simples. Si tienes algún problema durante el tiempo de ejecución en relación a algunas bibliotecas, incluye también esas bibliotecas afectadas. Si todo lo demás falla, siempre puedes decidir agrupar todo.

---

### Dependencias nativas

Las dependencias nativas se generan como parte de las compilaciones de sistema operativo específico, ya que no hay .NET runtime disponible en el host y ASF se ejecuta a través de su propio .NET runtime el cual está incluido como parte de la compilación de sistema operativo específico. Para minimizar el tamaño de la compilación, ASF recorta sus dependencias nativas para incluir solo el código accesible dentro del programa, lo que reduce efectivamente las partes no usadas del tiempo de ejecución. Esto puede crear un problema en lo que respecta a tu plugin, si de repente te encuentras en una situación en la que tu plugin depende de alguna característica de .NET que no está siendo usada en ASF, y por lo tanto las compilaciones de sistema operativo específico no pueden ejecutarla correctamente, normalmente arrojando `System.MissingMethodException` o `System.Reflection.ReflectionTypeLoadException` en el proceso. A medida que tu plugin crece en tamaño y se vuelve cada vez más complejo, tarde o temprano alcanzarás la superficie que no está cubierta por nuestra compilación de sistema operativo específico.

Esto nunca es un problema con las compilaciones genéricas, porque estas nunca tratan con dependencias nativas en primer lugar (ya que tienen un tiempo de ejecución completamente funcional en el host, ejecutando ASF). Por eso es una práctica recomendada **usar tu pugin exclusivamente en compilaciones genéricas**, pero obviamente eso tiene el inconveniente de privar de tu plugin a usuarios que ejecuten compilaciones de ASF de sistema operativo específico. Si te estás preguntando si tu problema está relacionado con dependencias nativas, también puedes usar este método como verificación, carga tu plugin en una compilación genérica de ASF y ve si funciona. Si funciona, tienes cubiertas las dependencias de tu plugin, y son las dependencias nativas las que causan problemas.

Desafortunadamente, tuvimos que elegir entre publicar todo runtime como parte de nuestras compilaciones para sistemas operativos específicos, y eliminar características no utilizadas, haciendo que la compilación sea cerca de 80 MB más pequeña en comparación con la completa. Elegimos la segunda opción, y por desgracia es imposible incluir las características faltantes de runtime junto con tu plugin. Si tu proyecto requiere acceso a características de runtime que no están disponibles, tienes que incluir todo el .NET runtime del que dependes, y eso significa ejecutar tu plugin en la variante `generic` de ASF. No puedes ejecutar tu plugin en compilaciones para sistemas operativos específicos, ya que esas compilaciones carecen de una característica de runtime que necesitas, y .NET runtime no puede "fusionar" la dependencia nativa que podrías haber proporcionado con la nuestra. Tal vez mejore en un futuro, pero por ahora simplemente no es posible.

Las compilaciones de ASF para sistemas operativos específicos incluyen el mínimo de funcionalidad adicional requerida para ejecutar nuestros plugins oficiales. Además de hacer eso posible, también amplía ligeramente la superficie a dependencias adicionales requeridas para los plugins más básicos. Por lo tanto, no todos los plugins necesitarán preocuparse por las dependencias nativas - solo aquellos que vayan más allá de lo que ASF y nuestros plugins oficiales necesitan directamente. Esto se hace de forma adicional, ya que si de todos modos necesitamos incluir dependencias nativas adicionales para nuestro uso, también podemos publicarlas directamente con ASF, haciéndolas disponibles, y por lo tanto más fáciles de cubrir para ti. Desafortunadamente, esto no siempre es suficiente, y a medida que tu plugin se hace más grande y complejo, aumenta la probabilidad de encontrar funcionalidades recortadas. Por lo tanto, recomendamos que ejecutes tus plugins personalizados exclusivamente en la variante `generic` de ASF. Todavía puedes verificar manualmente que la compilación de sistema operativo específico tiene todo lo que tu plugin requiere para su funcionalidad - pero ya que esto cambia tanto en tus actualizaciones como en las nuestras, podría ser complicado de mantener.

A veces es posible "remediar" las características faltantes ya sea usando opciones alternativas o reimplementándolas en tu plugin. Sin embargo, esto no siempre es posible o viable, especialmente si la característica faltante viene de dependencias de terceros que incluiste como parte de tu plugin. Siempre puedes intentar ejecutar tu plugin en una compilación de sistema operativo específico e intentar que funcione, pero podría llegar a ser muy molesto a largo plazo, especialmente si deseas que tu código simplemente funcione, en lugar de batallar con la superficie de ASF.

---

## Actualizaciones automáticas

ASF ofrece dos interfaces para implementar actualizaciones automáticas en tu plugin.

- `IGitHubPluginUpdates` que proporciona una forma fácil de implementar actualizaciones basadas en GitHub similar al mecanismo de actualizaciones de ASF.
- `IPluginUpdates` que proporciona funcionalidad de menor nivel que permite un mecanismo de actualizaciones personalizado, en caso de que requieras algo más complejo

---

### **[`IGithubPluginUpdates`](https://github.com/JustArchiNET/ArchiSteamFarm/blob/main/ArchiSteamFarm/Plugins/Interfaces/IGitHubPluginUpdates.cs)**

La lista de cosas mínimas requeridas para que funcionen las actualizaciones:

- Necesitas declarar `RepositoryName`, que define de dónde son extraídas las actualizaciones.
- Necesitas hacer uso de las etiquetas y versiones proporcionadas por GitHub. La etiqueta debe estar en formato que sea analizable a una versión de plugin, por ejemplo `1.0.0.0`.
- La propiedad `Version` del plugin debe coincidir con la etiqueta de la que proviene. Esto significa que el binario disponible con la etiqueta `1.2.3.4` debe presentarse como `1.2.3.4`.
- Cada etiqueta debe tener la versión apropiada disponible en GitHub con el recurso de versión en archivo zip que incluya los archivos binarios de tu plugin. Los archivos binarios que incluyen tus clases `IPlugin` deberían estar disponibles en el directorio raíz dentro del archivo zip.

Esto permitirá al mecanismo en ASF:

- Determinar la `Version` versión de tu plugin, por ejemplo `1.0.1`.
- Usar la API de GitHub para obtener la última `tag` etiqueta disponible en el repositorio `RepositoryName`, por ejemplo `1.0.2`.
- Determinar que `1.0.2` > `1.0.1`, y comprobar la versión de la etiqueta `1.0.2` para encontrar el archivo `.zip` con la actualización del plugin.
- Descargar ese archivo `.zip`, extraerlo, y colocar su contenido en el directorio que antes incluía `YourPlugin.dll`.
- Reiniciar ASF para cargar tu plugin en su versión `1.0.2`.

Notas adicionales:

- Las actualizaciones de plugins pueden requerir los valores de configuración de ASF apropiados, es decir **[`PluginsUpdateMode`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-es-ES#pluginsupdatemode)** y/o **[`PluginsUpdateList`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-es-ES#pluginsupdatelist)**.
- Nuestra plantilla de plugin ya incluye todo lo que necesitas, simplemente **[renombra](https://github.com/JustArchiNET/ASF-PluginTemplate?tab=readme-ov-file#renaming-myawesomeplugin)** el plugin a los valores adecuados, y estará listo para funcionar.
- Puedes usar una combinación de última versión así como prelanzamientos, serán usados según `UpdateChannel` el canal de actualizaciones que el usuario haya definido.
- Hay una propiedad booleana `CanUpdate` que puedes modificar para deshabilitar/habilitar las actualizaciones de plugin desde tu lado, por ejemplo si deseas omitir temporalmente las actualizaciones, o por cualquier otra razón.
- Es posible tener múltiples archivos zip en la versión si deseas apuntar a diferentes versiones de ASF. Ve los **[comentarios](https://github.com/JustArchiNET/ArchiSteamFarm/blob/main/ArchiSteamFarm/Plugins/Interfaces/IGitHubPluginUpdates.cs#L79-L106)** `GetTargetReleaseAsset()`. Por ejemplo, puedes tener `MyPlugin-V6-0.zip` así como `MyPlugin.zip`, esto causará que ASF en versión `V6.0.x.x` seleccione el primero, mientras que todas las demás versiones de ASF usarán el segundo.
- Si lo anterior no es suficiente y necesitas una lógica personalizada para elegir el archivo `.zip` correcto, puedes anular la función `GetTargetReleaseAsset()` y proporcionar el artefacto para que use ASF.
- Si tu plugin necesita hacer algo de trabajo adicional antes/después de la actualización, puedes modificar los métodos `OnPluginUpdateProceeding()` y/o `OnPluginUpdateFinished()`.

---

### **[`IPluginUpdates`](https://github.com/JustArchiNET/ArchiSteamFarm/blob/main/ArchiSteamFarm/Plugins/Interfaces/IPluginUpdates.cs)**

Esta interfaz te permite implementar una lógica personalizada para actualizaciones si por alguna razón `IGithubPluginUpdates` no es suficiente para ti, por ejemplo, que tengas etiquetas que no sean analizables a versiones, o porque no estás usando la plataforma de GitHub.

Hay una sola función `GetTargetReleaseURL()` que puedes modificar, que espera que introduzcas `Uri?` o ubicación destino para la actualización del plugin. ASF proporciona algunas variables que se relacionan con el contexto con el que se llamó a la función, pero además de eso, eres responsable de implementar todo lo que necesites en esa función y proporcionar a ASF la URL que debe ser usada, o `null` si la actualización no está disponible.

Además de eso, es similar a las actualizaciones de GitHub, donde la URL debe apuntar a un archivo `.zip` con los archivos binarios disponibles en el directorio raíz. También tienes disponibles los métodos `OnPluginUpdateProceeding()` y `OnPluginUpdateFinished()`.
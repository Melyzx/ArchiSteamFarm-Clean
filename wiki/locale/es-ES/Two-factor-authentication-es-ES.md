# Autenticación de dos factores

Steam incluye el sistema de autenticación de dos factores que requiere detalles adicionales para varias actividades relacionadas con la cuenta. Puedes leer más al respecto **[aquí](https://help.steampowered.com/es/faqs/view/2E6E-A02C-5581-8904)** y **[aquí](https://help.steampowered.com/es/faqs/view/34A1-EA3F-83ED-54AB)**. Esta página considera ese sistema 2FA así como nuestra solución que se integra a él, llamada ASF 2FA.

---

# Lógica de ASF

Independientemente de si usas ASF 2FA o no, ASF incluye la lógica apropiada y es plenamente consciente de las cuentas protegidas por 2FA en Steam. Te pedirá la información requerida cuando se necesite (como durante el inicio de sesión). Aunque puedes proporcionar manualmente esa información, ciertas funciones de ASF (como `MatchActively`) requieren que ASF 2FA esté operativo en tu cuenta bot, que pueden responder a solicitudes 2FA automáticamente, cuando sea requerido por ASF.

---

# ASF 2FA

ASF 2FA es un módulo integrado responsable de proveer características 2FA al proceso de ASF, tal como generar códigos y aceptar confirmaciones. Puede funcionar ya sea en modo independiente, o duplicando los detalles de tu autenticador existente (para que puedas usar tu autenticador actual y ASF 2FA al mismo tiempo).

Puedes verificar si tu cuenta bot ya está usando ASF 2FA ejecutando **[comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-es-ES)** `2fa`. Sin configurar ASF 2FA, ningún comando `2fa` será funcional, lo que significa que tu bot no estará disponible para las funciones avanzadas de ASF que requieran que el módulo esté operativo.

---

# Recomendaciones

Hay varias maneras de hacer ASF 2FA sea operativo, aquí incluimos nuestras recomendaciones dependiendo de tu situación:

- Si ya estás usando una aplicación de terceros no oficial que te permita extraer los detalles 2FA fácilmente, simplemente **[importa](#importar)** esos detalles a ASF.
- Si estás usando la aplicación oficial y no te importa restablecer tus credenciales 2FA, la mejor manera es desactivar 2FA, luego **[crear](#creación)** nuevas credenciales 2FA usando **[autenticador conjunto](#autenticador-conjunto)**, lo que te permitirá usar la aplicación oficial y ASF 2FA. Este método no requiere rooteo o conocimientos avanzados, será suficiente con seguir las instrucciones descritas aquí, y es razonablemente superior para este escenario.
- Si estás usando la aplicación oficial y no quieres restablecer tus credenciales 2FA, tus opciones son muy limitadas, normalmente necesitarías hacer root y pasos adicionales para **[importar](#importar)** esos detalles, e incluso con eso podría ser imposible.
- Si aún no estás usando 2FA y no te importa, recomendamos usar ASF 2FA con un **[autenticador independiente](#autenticador-independiente)** o **[autenticador conjunto](#autenticador-conjunto)** con la aplicación oficial (igual que arriba).

A continuación discutimos todas las opciones posibles y los métodos que conocemos.

---

## Creación

ASF incluye el **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-es-ES)** oficial `MobileAuthenticator` que amplía más ASF 2FA, permitiéndote vincular un autenticador 2FA completamente nuevo. Esto puede ser útil en caso de que no puedas o no quieras usar otras herramientas y no te importe que ASF 2FA se convierta en tu principal (y tal vez único) autenticador. El proceso de creación también se utiliza en el método de autenticador conjunto, naturalmente en este escenario tu autenticador puede coexistir en dos lugares a la vez - ambos generarán los mismos códigos y ambos podrán confirmar las mismas confirmaciones.

### Pasos comunes para ambos escenarios

No importa si planeas usar ASF como autenticador independiente o conjunto, necesitas realizar estos pasos de inicialización:

1. Crea un bot de ASF para la cuenta objetivo, inicia el bot e inicia sesión, lo que probablemente ya hayas hecho.
2. Asigna un número de teléfono operativo y funcional a la cuenta **[aquí](https://store.steampowered.com/phone/manage)** que será usada por el bot. Esto te permite recibir el código SMS y permitirá la recuperación en caso de ser necesario. Este paso no es obligatorio en todos los escenarios, sin embargo, lo recomendamos a menos que sepas lo que estás haciendo.
3. Asegúrate de que no estás usando 2FA para tu cuenta, si lo haces, primero deshabilítalo. Esto **pondrá** tu cuenta en una restricción de intercambio temporal, no hay forma de evitarlo, solo con el proceso **[importación](#importar)** se puede omitir.
4. Ejecuta el comando `2fainit [Bot]`, reemplazando `[Bot]` con el nombre de tu bot.

Suponiendo que tengas una respuesta exitosa, las siguientes dos cosas deben haber ocurrido:

- Un nuevo archivo `<Bot>.maFile.PENDING` fue generado por ASF en tu directorio `config`.
- Un SMS fue enviado por Steam al número de teléfono que asignaste para la cuenta. Si no estableciste un número de teléfono, entonces se envió un correo a la dirección de correo electrónico de la cuenta.

Los detalles del autenticador todavía no son funcionales, sin embargo, puedes revisar el archivo generado si lo deseas. Si quieres estar doblemente seguro, puedes anotar el código de revocación. Los siguientes pasos dependerán de tu escenario seleccionado.

### Autenticador independiente

Si quieres usar ASF como tu principal (o incluso único) autenticador, ahora necesitas hacer el último paso de finalización:

5. Ejecuta el comando `2fafinalize [Bot] <ActivationCode>`, reemplazando `[Bot]` con el nombre de tu bot y `<ActivationCode>` con el código que recibiste por SMS o correo electrónico en el paso anterior.

### Autenticador conjunto

Si quieres tener el mismo autenticador tanto en ASF como en la aplicación oficial de Steam, ahora necesitas hacer los siguientes pasos, un poco más complicados:

5. Ignora el código por SMS o correo electrónico que recibiste en el paso anterior.
6. Instala la aplicación de Steam si aún no lo has hecho, y ábrela. Dirígete a la pestaña de Steam Guard y añade un nuevo autenticador siguiente las instrucciones de la aplicación.
7. Después de añadir tu autenticador en la aplicación y que esté funcionando, regresa a ASF. Ahora, en lugar de finalizar, solo necesitamos informar a ASF que la aplicación móvil ya ha activado nuestros detalles generados previamente:
 - Espera hasta que se muestre el siguiente código 2FA en la aplicación de Steam, y usa el comando `2fafinalized [Bot] <2FACodeFromApp>`, reemplazando `[Bot]` con el nombre de tu bot y `<2FACodeFromApp>` con el código que veas en la aplicación de Steam. Si el código generado por ASF y el código que proporcionaste son iguales, ASF asumirá que un autenticador fue añadido correctamente y procederá a importar tu autenticador recién creado.
 - Recomendamos hacer lo anterior para asegurar que las credenciales son válidas. Sin embargo, si no quieres (o no puedes) comprobar si los códigos son el mismo y sabes lo que estás haciendo, en su lugar puedes usar el comando `2fafinalizedforce [Bot]`, reemplazando `[Bot]` con el nombre de tu bot. ASF asumirá que el autenticador fue añadido correctamente y procederá a importar tu autenticador recién creado. Ten en cuenta que en este modo ASF no puede verificar si los códigos coinciden, lo que significa que podrías importar credenciales no válidas (no activadas).

### Después de la finalización

Suponiendo que todo funcionó correctamente, el archivo `<Bot>.maFile.PENDING` previamente generado fue renombrado a `<Bot>.maFile.NEW`. Esto indica que tus credenciales 2FA ahora son válidas y están activas. Recomendamos que muevas ese archivo a **un lugar seguro y confiable**. Además, si decidiste usar el autenticador independiente, entonces te recomendamos abrir el archivo en el editor de tu elección y apunta el `revocation_code` (código de revocación), que te permitirá, como el nombre lo indica, revocar el autenticador en caso de que lo pierdas. En el método de autenticador conjunto, ya deberías haber hecho eso en la aplicación de Steam, pero siéntete libre de hacer lo mismo en caso necesario.

En cuanto a los detalles técnicos, el archivo `maFile` generado incluye todos los detalles que hemos recibido del servidor de Steam durante la vinculación del autenticador, además del campo `device_id`, que podría ser necesario para otros autenticadores (de terceros), si alguna vez decides importar a ellos el archivo `maFile`.

ASF importa automáticamente tu autenticador una vez terminado el procedimiento, por lo tanto `2FA` y otros comandos relacionados ahora deberían ser operativos para la cuenta bot a la que vinculaste el autenticador. Te recomendamos que lo verifiques.

---

## Importar

El proceso de importación requiere que ya tengas un autenticador vinculado y funcional que sea soportado por ASF. Tenemos instrucciones para algunas fuentes oficiales y no oficiales de 2FA, además del método manual que te permite proporcionar las credenciales. Ten en cuenta que esas instrucciones deben utilizarse **solo** si ya estás usando una solución determinada - ya que el proceso aquí implica aplicaciones y herramientas de terceros, **no recomendamos usarlas**, y lo mencionamos exlcusivamente para las personas que ya decidieron usarlas y quieren importar los detalles generados a ASF 2FA.

Las siguientes guías requieren que ya estés usando un autenticador **funcional y operativo** con una herramienta/aplicación determinada. ASF 2FA no funcionará correctamente si importas datos inválidos, por lo tanto asegúrate de que tu autenticador funciona correctamente antes de intentar importarlo. Esto incluye probar y verificar que las siguientes funciones del autenticador trabajan correctamente:
- Puedes generar códigos y que estos sean aceptados por la red de Steam (puedes iniciar sesión con ellos)
- Puedes obtener confirmaciones, y están llegando a tu autenticador móvil
- Puedes reaccionar a esas confirmaciones, y son reconocidas apropiadamente por la red de Steam como confirmadas/rechazadas

Asegúrate de que tu autenticador funciona comprobando si las acciones anteriores funcionan - si no lo hacen, entonces tampoco funcionarán en ASF.

---

### Teléfono Android

Normalmente, para importar el autenticador de tu teléfono Android necesitarás acceso **[root](https://es.wikipedia.org/wiki/Root_(Android))**. Las instrucciones siguientes requieren que tengas un conocimiento decente en el mundo del "modding" de Android, definitivamente no vamos a explicar todos los pasos, visita **[XDA](https://xdaforums.com)** y otras fuentes para información adicional o ayuda con lo siguiente.

Asumiendo que tienes la  **[aplicación de Steam](https://play.google.com/store/apps/details?id=com.valvesoftware.android.steam.community)** oficial funcionando y operativa (requiere rootear tu dispositivo):

- Instala **[Magisk](https://topjohnwu.github.io/Magisk/install.html)** y habilita Zygisk en los ajustes.
- Instala **[LSPosed](https://github.com/LSPosed/LSPosed?tab=readme-ov-file#install)** (para Zygisk) y comprueba que funciona.
- Instala el módulo de LSPosed **[SteamGuardExtractor](https://github.com/hax0r31337/SteamGuardExtractor?tab=readme-ov-file#usage)** y habilítalo en la configuración de SLPosed.
- Fuerza el cierre de la aplicación de Steam, luego ábrela, una **[ventana con los detalles extraídos](https://github.com/JustArchiNET/ArchiSteamFarm/assets/1069029/ab5ab71e-d664-4e49-9eb4-9f4d9ba32aa2)** debería aparecer, haz clic en copiar.

Ahora que has extraído exitosamente los detalles necesarios, deshabilita el módulo para prevenir que la ventana aparezca cada vez, luego copia el valor de `shared_secret` e `identity_secret` de la cuenta que quieres añadir a ASF 2FA, en un nuevo archivo de texto con la siguiente estructura:

```json
{
  "shared_secret": "STRING",
  "identity_secret": "STRING"
}
```

Reemplaza cada valor `STRING` con la clave privada de los detalles extraídos. Ahora, renombra el archivo a `BotName.maFile`, donde `BotName` es el nombre del bot que estás añadiendo a ASF 2FA, y colócalo en el directorio `config` de ASF si aún no lo has hecho.

Ejecuta ASF, que debería detectar tu archivo e importarlo. Asumiendo que has importado el archivo correcto con los detalles confidenciales válidos, todo debería funcionar correctamente, lo que puedes verificar usando los comandos `2fa`. Si cometiste algún error, puedes eliminar `Bot.db` y empezar de cero si es necesario.

---

### SteamDesktopAuthenticator

Si ya tienes tu autenticador en SDA, deberías notar que el archivo `steamID.maFile` está disponible en la carpeta `maFiles`. Asegúrate de que el archivo `maFile` está en formato no cifrado, ya que ASF no puede descifrar los archivos de SDA - el contenido de un archivo no cifrado debería comenzar con el carácter `{` y terminar con `}`. Si es necesario, primero puedes eliminar el cifrado desde las opciones de SDA, y habilitarlo de nuevo cuando hayas terminado. Una vez que el archivo esté en formato no cifrado, cópialo al directorio `config` de ASF.

Ahora puedes renombrar el archivo `steamID.maFile` a `BotName.maFile` en el directorio de configuraciones de ASF, donde `BotName` es el nombre del bot al que estás añadiendo ASF 2FA. Alternativamente, puedes dejarlo como está, ASF lo seleccionará automáticamente después de iniciar sesión. Renombrar el archivo ayuda a ASF haciendo posible usar ASF 2FA antes de iniciar sesión, si no lo haces, entonces el archivo solo puede ser seleccionado después de que ASF haya iniciado sesión exitosamente (ya que ASF no sabe el `steamID` de tu cuenta antes de iniciar sesión).

Ejecuta ASF, que debería detectar tu archivo e importarlo. Asumiendo que has importado el archivo correcto con los detalles confidenciales válidos, todo debería funcionar correctamente, lo que puedes verificar usando los comandos `2fa`. Si cometiste algún error, puedes eliminar `Bot.db` y empezar de cero si es necesario.

---

### WinAuth

Primero crea un nuevo archivo `BotName.maFile` vacío en el directorio config de ASF, donde `BotName` es el nombre del bot al que le estás añadiendo ASF 2FA. Si proporcionas un nombre incorrecto, no será seleccionado por ASF.

Ahora ejecuta WinAuth normalmente. Haz clic derecho en el ícono de Steam y selecciona "Show SteamGuard and Recovery Code". Luego marca la casilla "Allow copy". Deberías notar una estructura JSON familiar al fondo de la ventana, que empieza con `{`. Copia todo el texto al archivo `BotName.maFile` que creaste en el paso anterior.

Ejecuta ASF, que debería detectar tu archivo e importarlo. Asumiendo que has importado el archivo correcto con los detalles confidenciales válidos, todo debería funcionar correctamente, lo que puedes verificar usando los comandos `2fa`. Si cometiste algún error, puedes eliminar `Bot.db` y empezar de cero si es necesario.

---

### Manual

Si eres un usuario avanzado, también puedes generar los archivos maFile manualmente. Esto puede ser usado en caso de que quieras importar el autenticador desde fuentes distintas a las descritas anteriormente. Debe tener una **[estructura JSON válida](https://jsonlint.com)** de:

```json
{
  "shared_secret": "STRING",
  "identity_secret": "STRING"
}
```

Los datos de un autenticador estándar tienen más campos - son ignorados completamente por ASF durante la importación, ya que no son necesarios. No necesitas eliminarlos - ASF solo requiere un JSON válido con los 2 campos obligatorios descritos arriba, e ignorará campos adicionales (si los hay). Por supuesto, necesitas reemplazar el `STRING` del ejemplo anterior con los valores válidos para tu cuenta. Cada `STRING` debería ser una representación codificada de bytes en base 64 que componen la clave privada.

---

## Preguntas frecuentes

### ¿Cómo hace uso ASF del módulo 2FA?

Si ASF 2FA está disponible, ASF lo usará para la confirmación automática de intercambios que sean enviados/aceptados por ASF. También será capaz de generar automáticamente códigos 2FA cuando sea necesario, por ejemplo para iniciar sesión. Además, tener ASF 2FA también habilita para su uso los comandos `2fa`.

---

### ¿Cómo puedo obtener un código 2FA?

Necesitarás códigos 2FA para acceder a cuentas protegidas por 2FA, eso también incluye todas las cuentas con ASF 2FA. Si decidiste usar el autenticador independiente, debes usar el comando `2fa <BotNames>` para generar códigos temporales para las instancias de bot determinadas. En todos los demás escenarios, recomendamos usar el autenticador original que has utilizado, aunque también puedes usar el comando si es más conveniente para ti.

---

### ¿Puedo usar mi autenticador original después de importarlo como ASF 2FA?

Sí, tu autenticador original sigue siendo funcional y lo puedes usar junto con ASF 2FA. Sin embargo, ten en cuenta, que si lo invalidas por cualquier método, entonces las credenciales de ASF 2FA vinculadas también dejarán de ser válidas.

---

### ¿Cómo puedo eliminar ASF 2FA?

Simplemente cierra ASF y elimina el archivo `BotName.db` del bot con ASF 2FA que quieras eliminar. Esta opción eliminará el 2FA importado asociado con ASF, pero NO invalidará (desvincular) tu autenticador. Si en cambio quieres invalidar tu autenticador, además de eliminarlo de ASF (primero), debes desvincularlo en el autenticador original de tu elección. Si por cualquier razón no lo puedes hacer, por ejemplo, porque estás usando ASF 2FA en modo independiente, entonces usar el código de revocación que recibiste durante la configuración, en la página de Steam. No es posible invalidar tu autenticador mediante ASF.

---

### Vinculé el autenticador en una aplicación de terceros y luego lo importé a ASF. ¿Ahora puedo vincularlo de nuevo en mi teléfono?

**No**. Hacerlo invalidará las credenciales previamente importadas y tu ASF 2FA dejará de funcionar (generando códigos que ya no sean aceptados por Steam). Primero decide dónde quieres tener ubicado tu autenticador original o de terceros, luego impórtalo como ASF 2FA.

---

### ¿Usar ASF 2FA es mejor que un autenticador de terceros configurado para aceptar todas las confirmaciones?

**Sí**, de varias maneras. Primero y más importante - usar ASF 2FA aumenta tu seguridad **significativamente**, ya que el módulo ASF 2FA asegura que ASF solo aceptará automáticamente sus propias confirmaciones, por lo que incluso si un atacante solicita un intercambio perjudicial, ASF 2FA **no** aceptará dicho intercambio, ya que no fue generado por ASF. Además de la seguridad, usar ASF 2FA también conlleva beneficios de rendimiento/optimización, ya que ASF 2FA analiza y acepta las confirmaciones inmediatamente después de ser generadas, contrario a la ineficiente comprobación cada X minutos que es usada por otras soluciones. No hay razón para usar un autenticador de terceros sobre ASF 2FA, si planeas automatizar las confirmaciones generadas por ASF - exactamente para eso es ASF 2FA, y usarlo no causa conflicto con que confirmes todo lo demás en el autenticador de tu elección. Recomendamos usar ASF 2FA para toda la actividad de ASF.
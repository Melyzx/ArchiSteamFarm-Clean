# Préstamo familiar de Steam

ASF soporta el Préstamo Familiar de Steam - tanto el antiguo como el nuevo sistema. Para entender cómo funciona ASF con este, primero deberías leer cómo href="https://store.steampowered.com/promotion/familysharing">Steam funciona el Préstamo Familiar de Steam</a></strong>, que se encuentra disponible en la tienda de Steam. Además, revisa  **[las noticias](https://store.steampowered.com/news/app/593110/view/4149575031735702628)** para el nuevo sistema de Préstamo Familia de Steam, con el que ASF también es compatible.

---

## ASF

El soporte para el Préstamo Familiar de Steam en ASF es transparente, esto significa que no introduce ninguna propiedad de configuración nueva para el bot/proceso - funciona inmediatamente como una funcionalidad integrada adicional.

ASF incluye la lógica apropiada para saber si la biblioteca está bloqueada por usuarios del préstamo familiar, por lo tanto no los "expulsará" de su sesión al ejecutar un juego. ASF actuará exactamente igual como si la cuenta principal estuviese jugando, por lo tanto si ese estado es mantenido por tu cliente de Steam, o por uno de los usuarios del préstamo familiar, ASF no intentará recolectar, en su lugar, esperará que se libere. Esto está relacionado principalmente con el sistema antiguo - el nuevo sistema permite a los usuarios de tu préstamo familiar jugar juegos diferentes a los que ASF está recolectando actualmente.

Además, después de iniciar sesión, ASF accederá a tus sistemas de préstamo familiar (antiguo y nuevo), de donde extraerá a los usuarios (ID de Steam) con permiso para usar tu biblioteca. A estos usuarios se les otorga el permiso `FamilySharing` para usar **[comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-es-ES)**, especialmente permitiéndoles usar el comando `pause~` en el bot que esté compartiendo juegos con ellos, lo que permite pausar el módulo automático de recolección de cromos para poder ejecutar un juego que puede ser compartido - esto mayormente aplica para el sistema antiguo, pero podría ser usado con el nuevo sistema en caso de que ASF esté recolectando el juego que tus usuarios quieran jugar.

Conectar las dos funcionalidades descritas arriba permite a tus amigos usar el comando `pause~` para pausar el proceso de recolección de cromos, iniciar un juego, jugar el tiempo que deseen, y después de que hayan terminado de jugar, el proceso de recolección de cromos será reanudado automáticamente por ASF. Por supuesto, enviar el comando `pause~` no es necesario si ASF no está recolectando nada, porque tus amigos pueden ejecutar el juego directamente, y la lógica descrita antes asegura que no serán expulsados de la sesión.

---

## Limitaciones

A la red de Steam le gusta engañar a ASF transmitiendo actualizaciones de estado falsas, lo que puede conducir a que ASF crea que está bien reanudar el proceso, y en consecuencia expulsar a tu amigo antes de tiempo. Este es exactamente el mismo problema que el ya explicado en **[esta entrada de las preguntas frecuentes](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ-es-ES#asf-est%C3%A1-expulsando-mi-sesi%C3%B3n-en-el-cliente-de-steam-mientras-estoy-jugando--esta-cuenta-tiene-iniciada-una-sesi%C3%B3n-en-otro-equipo)**. Recomendamos exactamente las mismas soluciones, principalmente otorgar a tus amigos el permiso `Operator` (o superior) y pedirles que usen los comandos `pause` y `resume`.
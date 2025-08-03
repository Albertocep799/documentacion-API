---
icon: dungeon
layout:
  width: default
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: false
---

# Gateway

La API de Gateway permite a los clientes abrir conexiones WebSocket seguras con Discord para recibir eventos sobre acciones que ocurren en recursos a los que tienen acceso, como cuando un canal se actualiza o se crea un rol. Hay algunos casos donde los clientes _también_ usan conexiones Gateway para actualizar o solicitar recursos, como cuando actualizan el estado de voz.

{% hint style="info" %}
En la mayoría de los casos, las operaciones REST sobre recursos de Discord se realizan utilizando la API HTTP en lugar de la API del Gateway.
{% endhint %}

El Gateway es la forma de comunicación en tiempo real de Discord usada por los clientes. La API para interactuar con Gateways es compleja y bastante implacable, así que asegúrate de leer la siguiente documentación en su totalidad para que entiendas los secretos sagrados del Gateway.

La documentación aquí presente es solo para la última versión de la API, a menos que se especifique lo contrario.

### Eventos Gateway <a href="#eventos-gateway" id="eventos-gateway"></a>

Los eventos Gateway son cargas útiles enviadas sobre una conexión Gateway, ya sea de un cliente a Discord, o de Discord a un cliente. Un cliente típicamente _envía_ eventos cuando se conecta y gestiona su conexión al Gateway, y _recibe_ eventos cuando escucha acciones que ocurren en un recurso.

Todos los eventos Gateway están encapsulados en una carga útil de evento Gateway.

Una lista completa de eventos Gateway y sus detalles están en la documentación de eventos Gateway.

Los detalles sobre las cargas útiles de eventos Gateway están en la documentación de eventos Gateway.

#### Enviando eventos

Cuando envías un evento Gateway (como cuando realizas un handshake inicial o actualizas presencia), tu cliente debe enviar un objeto de carga útil de evento con un Opcode válido (`op`) y objeto de datos interno (`d`).

{% hint style="info" %}
Se aplican límites de velocidad específicos al enviar eventos, los cuales puedes consultar en la sección de l**ímites de velocidad**.
{% endhint %}

Las cargas útiles de eventos enviadas sobre una conexión Gateway:

* Deben estar serializadas en JSON de texto plano o ETF binario.
* No deben exceder 15 KiB. Si una carga útil de evento _excede_ este límite, la conexión se cerrará con código de cierre `4002`.

Todos los eventos que puedes enviar sobre el Gateway están en la documentación de eventos Gateway.

#### Recibiendo eventos

Recibir un evento Gateway de Discord (como cuando se añade una reacción a un mensaje) es mucho más común (y ligeramente más complejo) que enviarlos.

Mientras que algunos eventos se envían a tu cliente independientemente de los intents, recibir la mayoría de eventos es configurable especificando intents al identificarse. Los intents son valores bitwise que pueden ser ORed (`|`) para indicar qué eventos (o grupos de eventos) quieres que Discord envíe a tu cliente. Los intents se usan más comúnmente (y son requeridos para) bots, ya que las cuentas de usuario usualmente quieren recibir todos los grupos de eventos, y evitar eventos innecesarios usando otra funcionalidad. Una lista de intents y sus eventos correspondientes están listados en la sección de intents.

Cuando recibes eventos, también puedes configurar _cómo_ se enviarán los eventos a tu cliente, como la codificación y compresión, o si el sharding debería estar habilitado.

Todos los eventos que puedes recibir sobre el Gateway están en la documentación de eventos Gateway.

#### Eventos Dispatch

Los eventos Dispatch Opcode 0 son el tipo más común de evento que recibirás. Los eventos Gateway que representan acciones que ocurren en un servidor serán enviados a tu cliente como eventos Dispatch.

Cuando tu cliente está analizando un evento Dispatch:

* El campo `t` puede usarse para determinar qué evento Gateway representa la carga útil y los datos que puedes esperar en el campo `d`.
* El campo `s` representa el número de secuencia del evento, que es el orden relativo en el que ocurrió. Necesitas cachear el valor `s` no nulo más reciente para heartbeats, y para pasar cuando Reanudar una conexión.

### Conexiones <a href="#conexiones" id="conexiones"></a>

Las conexiones Gateway son WebSockets persistentes que introducen más complejidad que enviar peticiones HTTP. Cuando interactúas con el Gateway, tu cliente debe saber cómo abrir la conexión inicial, así como mantenerla y manejar cualquier desconexión.

#### Ciclo de vida de conexión

{% hint style="info" %}
Existen matices que no se incluyen en el resumen siguiente. Puedes encontrar más detalles sobre cada paso y evento en las secciones individuales que aparecen más abajo.
{% endhint %}

A un alto nivel, las conexiones Gateway consisten en el siguiente ciclo:

<figure><img src="https://media.discordapp.net/attachments/756858637753909319/1400878781199286292/gateway-lifecycle.png?ex=688e3d88&#x26;is=688cec08&#x26;hm=7856c45a7e34b9a493a9c12c674333a55620d76918f24c503fadee979e3e175d&#x26;=&#x26;width=1318&#x26;height=1007" alt=""><figcaption></figcaption></figure>

1. El cliente establece una conexión con el Gateway después de obtener y cachear una URL WebSocket usando el endpoint Get Gateway o Get Gateway Bot.
2. Discord envía al cliente un evento Opcode 10 Hello conteniendo un intervalo de heartbeat en milisegundos. **Lee la sección sobre conectarse.**
3. El cliente inicia la tarea de heartbeat: envía un evento Opcode 1 Heartbeat, luego continúa enviándolos cada intervalo de heartbeat hasta que la conexión se cierre.

* Discord responderá a cada evento Heartbeat con un evento Opcode 11 Heartbeat ACK para confirmar que fue recibido. Si el cliente no recibe un Heartbeat ACK antes del siguiente intervalo de heartbeat, debería cerrar la conexión y reconectarse.
* Discord puede enviar al cliente un evento Opcode 1 Heartbeat, en cuyo caso debería responder con un evento Opcode 1 Heartbeat inmediatamente.

4. El cliente envía un evento Opcode 2 Identify para realizar el handshake inicial con el Gateway. El cliente no tiene que esperar al evento Opcode 10 Hello antes de identificarse.
5. Discord envía al cliente un evento Ready que indica que el handshake fue exitoso y la conexión está establecida. El evento Ready contiene un `resume_gateway_url` que el cliente debería rastrear para determinar la URL WebSocket que debería usar para reanudar. **Lee la sección sobre el evento Ready.**
6. La conexión puede perderse por varias razones. Si el cliente puede reanudar la conexión o si debe re-identificarse se determina por varios factores como el OPcode y código de cierre que recibe. **Lee la sección sobre desconectarse.**
7. Si el cliente **puede** reanudar/reconectarse, debería abrir una nueva conexión usando `resume_gateway_url`, luego enviar un evento Opcode 6 Resume. Si **no puede** reanudar/reconectarse, debería abrir una nueva conexión usando la URL cacheada del paso #1, luego repetir todo el ciclo Gateway. _¡Yupi!_

#### Conectándose

Antes de que puedas establecer una conexión al Gateway, deberías llamar al endpoint Get Gateway o Get Gateway Bot. Cualquier endpoint devolverá una carga útil con un campo `url` cuyo valor es la URL que puedes usar para abrir una conexión WebSocket. Además de la URL, Get Gateway Bot contiene información adicional sobre el número recomendado de shards y los límites de inicio de sesión para tu usuario.

Cuando inicialmente llames a Get Gateway o Get Gateway Bot, deberías cachear el valor del campo `url` y solo re-solicitarlo después de fallar al establecer una conexión al Gateway.

Cuando te conectes a la URL, es buena idea pasar explícitamente la versión de API y codificación como parámetros de consulta. También puedes incluir opcionalmente si Discord debería comprimir datos que envía a tu aplicación. Por ejemplo, `wss://gateway.discord.gg/?v=9&encoding=json&compress=zlib-stream` es una URL que un cliente puede usar para conectarse al Gateway.

#### Parámetros de cadena de consulta

| Campo     | Tipo    | Descripción                                                               |
| --------- | ------- | ------------------------------------------------------------------------- |
| v         | integer | Versión de API a usar.                                                    |
| encoding  | string  | La codificación de paquetes gateway recibidos (`json` o `etf`).           |
| compress? | string  | La compresión de transporte opcional de paquetes gateway (`zlib-stream`). |

#### Evento Hello

Una vez conectado, el cliente recibirá un evento Opcode 10 Hello, con información sobre el intervalo de heartbeat de la conexión (`heartbeat_interval`).

El intervalo de heartbeat indica con qué frecuencia (en milisegundos) debes enviar un heartbeat para mantener la conexión activa. El heartbeating se detalla en la sección de enviar heartbeats.

#### Enviando heartbeats

Los heartbeats son pings usados para que Discord sepa que tu cliente está usando activamente una conexión Gateway. Después de conectarte al Gateway, tu cliente debería enviar heartbeats (como se describe abajo) en un proceso en segundo plano hasta que la conexión Gateway se cierre.

Para enviar un heartbeat, se debe usar Opcode 1 Heartbeat u Opcode 40 QoS Heartbeat. Ambos son funcionalmente iguales respecto al ciclo de vida del Gateway, pero el QoS Heartbeat también rastrea estadísticas de Calidad de Servicio (QoS). El QoS Heartbeat se recomienda para todos los clientes, pero no es requerido.

#### Intervalo de Heartbeat

Al recibir Opcode 10 Hello, tu cliente debería enviar inmediatamente su primer evento heartbeat (con jitter opcional). Desde ese punto hasta que la conexión se cierre, tu cliente debe enviar continuamente a Discord un heartbeat cada `heartbeat_interval` milisegundos. Si tu cliente falla en enviar un evento heartbeat a tiempo, tu conexión se cerrará y serás forzado a reanudar.

Cuando envíes un heartbeat, tu cliente necesitará incluir el último número de secuencia que tu cliente recibió. El número de secuencia se envía en cada carga útil de evento Dispatch (en el campo `s`). Si no has recibido eventos aún, deberías pasar `null`.

Adicionalmente, cada 30 minutos y al conectar al Gateway, se debería enviar un evento Opcode 41 Update Time Spent Session ID. Esto se recomienda, pero no es requerido.

{% hint style="info" %}
El primer latido (heartbeat) puede retrasarse un valor entre 0 y el intervalo de latido (heartbeat\_interval) para evitar que demasiados clientes reconecten sus sesiones al mismo tiempo exacto (lo que podría causar un aumento de tráfico).
{% endhint %}

_Puedes_ enviar heartbeats antes de que el `heartbeat_interval` transcurra, pero deberías evitar hacerlo a menos que sea necesario. Ya hay tolerancia en el `heartbeat_interval` que cubrirá la latencia de red, así que no necesitas tenerla en cuenta en tu implementación.

Cuando envías un heartbeat, el Gateway responderá con un Opcode 11 Heartbeat ACK, que es un reconocimiento de que el heartbeat fue recibido.

Si un cliente no recibe un heartbeat ACK entre sus intentos de enviar heartbeats, esto puede deberse a una conexión fallida o "zombi". El cliente debería terminar inmediatamente la conexión con cualquier código de cierre excepto `1000` o `1001`, luego reconectarse e intentar reanudar.

{% hint style="info" %}
En caso de una interrupción del servicio en la que permanezcas conectado al Gateway, debes continuar enviando _heartbeats_ y recibiendo ACKs. El Gateway responderá eventualmente y emitirá una sesión cuando pueda hacerlo. Al recuperarse de una interrupción, el Gateway puede responder con un Opcode 9 _Invalid Session_ al intentar reanudar o identificar la sesión. Esto es para limitar la oleada de clientes que intentan reconectarse al mismo tiempo después de la interrupción. Si esto ocurre, debes mantener la conexión con el Gateway e intentar identificarte nuevamente. El Gateway responderá finalmente con un evento _Ready_ cuando pueda procesar tu solicitud.
{% endhint %}

#### Solicitudes de heartbeat

Además del intervalo de Heartbeat, el Gateway puede solicitar heartbeats adicionales de tu cliente enviando un evento Opcode 1 Heartbeat. Al recibir el evento, tu cliente debería enviar heartbeat inmediatamente sin esperar el resto del intervalo actual.

Como normalmente, el Gateway responderá con un evento Opcode 11 Heartbeat ACK.

#### Identificándose

Después de que la conexión esté abierta y estés enviando heartbeats, tu cliente debería enviar un Opcode 2 Identify. Este evento es el handshake inicial con el Gateway que se requiere antes de que tu cliente pueda empezar a enviar o recibir la mayoría de eventos Gateway.

Los usuarios están limitados por la concurrencia máxima (en el objeto límite de inicio de sesión) al identificarse. Si tu cliente excede este límite, el Gateway responderá con un evento Opcode 9 Invalid Session.

Después de que tu cliente envíe una carga útil identify válida, el Gateway responderá con un evento Ready que indica un estado conectado exitosamente con el Gateway. El evento Ready se envía como un Opcode 0 Dispatch estándar.

{% hint style="warning" %}
Los bots tienen un límite de 1000 llamadas de _identify_ al WebSocket en un período de 24 horas. Este límite es global y abarca todos los shards, pero no incluye las llamadas de _resume_. Al alcanzar este límite, todas las sesiones activas del bot serán terminadas, el token del bot será restablecido y recibirás una notificación por correo electrónico. Será tu responsabilidad actualizar tu bot con el nuevo token. Las cuentas de usuario no se ven afectadas por este límite.
{% endhint %}

#### Evento Ready

Como se mencionó arriba, el evento Ready se envía a tu cliente después de una identificación exitosa. El evento Ready incluye estado, como los servidores en los que estás, que necesitas para empezar a interactuar con el resto de la plataforma.

El evento Ready también incluye campos que necesitarás cachear para eventualmente reanudar tu conexión después de desconexiones. Dos campos en particular son importantes de señalar:

* `resume_gateway_url` es una URL WebSocket que deberías usar para reanudar después de una desconexión. El `resume_gateway_url` debería usarse en lugar de la URL usada al conectarse.
* `session_id` es el ID para la sesión Gateway para la conexión. Se requiere saber qué flujo de eventos estaba asociado con tu conexión.

Los detalles completos sobre el evento Ready están en la documentación de eventos Gateway.

#### Desconectándose

Internet es un lugar aterrador. Las desconexiones Gateway ocurren por varias razones, ya sea iniciadas por Discord o por ti.

#### Manejando una desconexión

Debido a la arquitectura de Discord, las desconexiones son un evento semi-regular y deberían esperarse y manejarse. Cuando tu cliente encuentra una desconexión, típicamente recibirá un código de cierre que puede usarse para determinar si puedes reconectarte y reanudar la sesión, o si tienes que empezar de nuevo y re-identificarte.

Después de determinar si puedes o no reconectarte, debes hacer una de las siguientes:

* Si determinas que _puedes_ reconectarte y reanudar la sesión anterior, entonces deberías reconectarte usando el `resume_gateway_url` y `session_id` del evento Ready. Los detalles sobre cuándo y cómo reanudar se pueden encontrar en la sección de reanudación.
* Si **no puedes** reconectarte **o la reconexión falla**, deberías abrir una nueva conexión usando la URL cacheada de la llamada inicial a Get Gateway o Get Gateway Bot. En el caso de una reconexión fallida, tendrás que re-identificarte después de abrir una nueva conexión.

Una lista completa de códigos de cierre se puede encontrar en la documentación de códigos de cierre.

#### Iniciando una desconexión

Cuando cierras la conexión al gateway con código de cierre `1000` o `1001`, tu sesión será invalidada y tu usuario aparecerá desconectado.

Si simplemente cierras la conexión TCP o usas un código de cierre diferente, la sesión permanecerá activa y expirará después de unos minutos. Esto puede ser útil cuando estás reanudando la sesión anterior.

#### Reanudando

Cuando tu cliente se desconecta, Discord tiene un proceso para reconectarse y reanudar, que permite a tu cliente reproducir cualquier evento perdido empezando desde el último número de secuencia que recibió. Después de reanudar, recibirás los eventos perdidos de la misma manera que los habrías tenido si la conexión hubiera permanecido activa. A diferencia de la conexión inicial, tu cliente **no** necesita re-identificarse al reanudar.

Hay un puñado de escenarios cuando deberías intentar reanudar:

* Recibes un evento Opcode 7 Reconnect
* Te desconectas con un código de cierre que indica que puedes reconectarte, o no recibes _ningún_ código de cierre.
* Recibes un evento Opcode 9 Invalid Session con el campo `d` establecido a `true`. Este es un escenario poco probable, pero es posible.

#### Preparándose para reanudar

Antes de que tu cliente pueda enviar un evento Opcode 6 Resume, necesitará tres valores: el `session_id` y el `resume_gateway_url` del evento Ready, y el número de secuencia (`s`) del último evento Opcode 0 Dispatch que recibió antes de la desconexión.

Después de que la conexión se cierre, tu cliente debería abrir una nueva conexión usando `resume_gateway_url` en lugar de la URL usada para conectarse inicialmente. Si no usas el `resume_gateway_url` cuando te reconectes, experimentarás desconexiones a una tasa más alta de lo normal.

Una vez que la nueva conexión esté abierta, tu cliente debería enviar un evento Opcode 6 Resume usando el `session_id` y `seq` mencionados arriba.

Cuando reanudes, no necesitas enviar un evento identify después de abrir la conexión.

Si es exitoso, el Gateway enviará los eventos perdidos en orden, terminando con un evento Resumed para señalar que la reproducción de eventos ha terminado y que todos los eventos subsecuentes serán nuevos.

Si el cliente no se reconecta a tiempo, el buffer de reproducción se llenará en exceso y la sesión será invalidada. En este caso, el cliente recibirá un Opcode 9 Invalid Session y debería desconectarse. Después de la desconexión, el cliente debería crear una nueva conexión con la URL cacheada del endpoint Get Gateway o Get Gateway Bot, luego identificarse.

### OAuth2 y el Gateway <a href="#oauth2-y-el-gateway" id="oauth2-y-el-gateway"></a>

Los tokens Bearer con los scopes OAuth2 `identify` y `gateway.connect` o `voice` pueden usarse para conectarse al Gateway en nombre de un usuario. El token debe tener el prefijo `Bearer` al identificarse, como en las peticiones HTTP. Los intentos de Gateway disponibles y eventos diferirán dependiendo de los scopes autorizados.

Ten en cuenta que los comandos soportados y estructuras de carga útil pueden diferir de los descritos en esta documentación. Los objetos pueden ser más parciales de lo esperado o pueden no contener campos requeridos. Como esta función no es estable, estas diferencias a menudo cambian y aún no están documentadas. Procede con precaución.

### Capacidades Gateway <a href="#capacidades-gateway" id="capacidades-gateway"></a>

{% hint style="info" %}
Aunque las _capabilities_ son compatibles para bots, no son muy funcionales y no se recomienda su uso.
{% endhint %}

El Gateway de Discord proporciona un sistema para habilitar y deshabilitar flags de funcionalidad personalizadas por conexión. Estas flags, o capacidades, se pasan en el parámetro `capabilities` al identificarse. Las capacidades son una forma para que los clientes elijan cambios de carga útil y funcionalidad de manera compatible hacia atrás, o deshabiliten funciones Gateway que no necesitan. Las capacidades no están para nada relacionadas con los intents Gateway.

Los efectos de capacidades específicas están documentados en la sección de eventos Gateway, bajo cada evento que es afectado. Sin embargo, la documentación generalmente asume que los clientes eligen todas las nuevas capacidades de funcionalidad.

#### Lista de capacidades

Abajo hay una lista de todas las capacidades y una descripción general de sus efectos. Para más información, consulta la documentación para el evento o funcionalidad específica que es afectada.

<sup>1</sup> Requiere `DEDUPE_USER_OBJECTS`. Sin él, la capacidad será ignorada.

<sup>2</sup> Reemplaza `PASSIVE_GUILD_UPDATE`. Si está habilitado, `PASSIVE_GUILD_UPDATE` será ignorado.

| Valor   | Nombre                                   | Descripción                                                                                                                                                                                                                                                                                |
| ------- | ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1 << 0  | LAZY\_USER\_NOTES                        | Elimina el campo `notes` del evento Ready.                                                                                                                                                                                                                                                 |
| 1 << 1  | NO\_AFFINE\_USER\_IDS                    | Previene la sincronización de miembros/presencia y eventos Presence Update para relaciones implícitas.                                                                                                                                                                                     |
| 1 << 2  | VERSIONED\_READ\_STATES                  | Habilita estados de lectura versionados, cambiando el campo `read_state` en el evento Ready a un objeto y permitiendo que sea cacheado al re-identificarse.                                                                                                                                |
| 1 << 3  | VERSIONED\_USER\_GUILD\_SETTINGS         | Habilita configuraciones de servidor de usuario versionadas, cambiando el campo `user_guild_settings` en el evento Ready a un objeto y permitiendo que sea cacheado al re-identificarse.                                                                                                   |
| 1 << 4  | DEDUPE\_USER\_OBJECTS                    | Deshidrata la carga útil Ready, moviendo todos los objetos de usuario al campo `users` y reemplazándolos en varios lugares en la carga útil con `user_id` o `recipient_id`, y fusionando los campos `members` de todos los servidores en un solo campo `merged_members`.                   |
| 1 << 5  | PRIORITIZED\_READY\_PAYLOAD 1            | Separa la carga útil Ready en dos partes (Ready y Ready Supplemental) permitiendo al cliente recibir la carga útil Ready más rápido y luego recibir el resto de la carga útil después.                                                                                                     |
| 1 << 6  | MULTIPLE\_GUILD\_EXPERIMENT\_POPULATIONS | Cambia la entrada `populations` de `guild_experiments` en el evento Ready para ser un array de poblaciones en lugar de una sola población.                                                                                                                                                 |
| 1 << 7  | NON\_CHANNEL\_READ\_STATES               | Incluye estados de lectura vinculados a recursos no-canal (ej. eventos programados de servidor y centro de notificaciones) en el campo `read_states` del evento Ready.                                                                                                                     |
| 1 << 8  | AUTH\_TOKEN\_REFRESH                     | Habilita renovación de token de autenticación, permitiendo al cliente recibir opcionalmente un nuevo token de autenticación en el campo `auth_token` del evento Ready.                                                                                                                     |
| 1 << 9  | USER\_SETTINGS\_PROTO                    | Elimina el campo `user_settings` del evento Ready y previene eventos User Settings Update; se usa en su lugar el campo `user_settings_proto` y evento User Settings Proto Update.                                                                                                          |
| 1 << 10 | CLIENT\_STATE\_V2                        | Habilita caché de estado de cliente v2.                                                                                                                                                                                                                                                    |
| 1 << 11 | PASSIVE\_GUILD\_UPDATE                   | Habilita actualizaciones pasivas de servidor, permitiendo al cliente recibir eventos Passive Update v1 en lugar de eventos Channel Unreads Update para servidores a los que no está suscrito.                                                                                              |
| 1 << 12 | AUTO\_CALL\_CONNECT                      | Conecta al cliente a todas las llamadas pre-existentes al conectarse al Gateway; esto significa que los clientes recibirán eventos Call Create para todas las llamadas creadas antes de que la conexión Gateway fuera establecida sin necesidad de enviar un Request Call Connect primero. |
| 1 << 13 | DEBOUNCE\_MESSAGE\_REACTIONS             | Debouncea eventos de reacción de mensajes, previniendo que el cliente reciba múltiples eventos Message Reaction Add para el mismo mensaje dentro de un período corto de tiempo; los clientes recibirán un solo evento Message Reaction Add Many en su lugar.                               |
| 1 << 14 | PASSIVE\_GUILD\_UPDATE\_V2 2             | Habilita actualizaciones pasivas de servidor v2, permitiendo al cliente recibir eventos Passive Update v2 en lugar de eventos Channel Unreads Update para servidores a los que no está suscrito.                                                                                           |
| 1 << 16 | AUTO\_LOBBY\_CONNECT                     | Añade un campo `lobbies` al evento Ready conteniendo lobbies pre-existentes y deja de enviar eventos Lobby Create al conectarse al Gateway.                                                                                                                                                |

### Intentos de la gateway <a href="#intents-gateway" id="intents-gateway"></a>

{% hint style="info" %}
Para bots, los _intents_ son opcionalmente compatibles en la API v6, pero son obligatorios a partir de la v8. Las cuentas de usuario no requieren _intents_ y no se recomienda su uso para ellas.
{% endhint %}

Mantener una aplicación con estado puede ser difícil cuando se trata de la cantidad de datos que se espera que proceses sobre una conexión Gateway, especialmente a escala. Los intents Gateway son un sistema para ayudarte a reducir la carga computacional.

Los intents son valores bitwise pasados en el parámetro `intents` al identificarse que se correlacionan con un conjunto de eventos relacionados. Por ejemplo, el evento enviado cuando se crea un servidor (Guild Create) y cuando se actualiza un canal (Channel Update) ambos requieren el mismo intent `GUILDS (1 << 0)` (como se lista en la tabla abajo). Si no especificas un intent al identificarte, no recibirás _ninguno_ de los eventos Gateway asociados con ese intent.

Para bots, existen dos tipos de intents:

* **Los intents estándar** pueden pasarse por defecto. No necesitas permisos o configuraciones adicionales.&#x20;
* **Los intents privilegiados** deben habilitarse antes de que puedan usarse. Más información sobre intents privilegiados se puede encontrar en su sección abajo.

Tu conexión Gateway se cerrará si pasas intents inválidos (código de cierre `4013`), o un intent privilegiado que no ha sido configurado o aprobado para tu bot (código de cierre `4014`). Las cuentas de usuario no requieren aprobación y pueden usar cualquier intent, aunque con advertencias propias.

#### Lista de intentos

Abajo hay una lista de todos los intents y los eventos Gateway asociados con ellos. Cualquier evento _no_ listado significa que no está asociado con un intent y siempre será enviado a tu cliente.

Todos los eventos, incluyendo aquellos que no están asociados con un intent, están en la documentación de eventos Gateway.

<details>

<summary>Lista de intentos</summary>

<sup>1</sup> Thread Members Update contiene datos diferentes dependiendo de qué intents se usen.

<sup>2</sup> Para bots, eventos bajo los intents `GUILD_PRESENCES` y `GUILD_MEMBERS` están **desactivados por defecto en todas las versiones de API**. Los bots usando API v7 y anteriores recibirán eventos asociados con los intents privilegiados que tienen _sin_ pasar esos intents en el parámetro `intents` al identificarse. Los bots usando **API v8** y superiores deben especificar todos los intents al identificarse (privilegiados o no). Las cuentas de usuario tienen por defecto todos los intents activados.

<sup>3</sup> `MESSAGE_CONTENT` no representa eventos individuales, sino que afecta qué datos están presentes para eventos que podrían contener campos de contenido de mensaje. Más información está en la sección intent de contenido de mensaje.

<sup>4</sup> Este intent no puede ser usado por bots.

<sup>5</sup> Este intent solo es aplicable en contextos OAuth2.

> GUILDS (1 << 0)
>
> * GUILD\_CREATE
> * GUILD\_UPDATE
> * GUILD\_DELETE
> * GUILD\_ROLE\_CREATE
> * GUILD\_ROLE\_UPDATE
> * GUILD\_ROLE\_DELETE
> * CHANNEL\_CREATE
> * CHANNEL\_UPDATE
> * CHANNEL\_DELETE
> * VOICE\_CHANNEL\_STATUS\_UPDATE
> * CHANNEL\_PINS\_UPDATE
> * THREAD\_CREATE
> * THREAD\_UPDATE
> * THREAD\_DELETE
> * THREAD\_LIST\_SYNC
> * THREAD\_MEMBER\_UPDATE
> * THREAD\_MEMBERS\_UPDATE ¹
> * STAGE\_INSTANCE\_CREATE
> * STAGE\_INSTANCE\_UPDATE
> * STAGE\_INSTANCE\_DELETE
>
> GUILD\_MEMBERS (1 << 1)
>
> * GUILD\_MEMBER\_ADD
> * GUILD\_MEMBER\_UPDATE
> * GUILD\_MEMBER\_REMOVE
> * THREAD\_MEMBERS\_UPDATE ¹
>
> GUILD\_MODERATION (1 << 2)
>
> * GUILD\_AUDIT\_LOG\_ENTRY\_CREATE
> * GUILD\_BAN\_ADD
> * GUILD\_BAN\_REMOVE
>
> GUILD\_EMOJIS\_AND\_STICKERS (1 << 3)
>
> * GUILD\_EMOJIS\_UPDATE
> * GUILD\_STICKERS\_UPDATE
>
> GUILD\_INTEGRATIONS (1 << 4)
>
> * GUILD\_INTEGRATIONS\_UPDATE
> * INTEGRATION\_CREATE
> * INTEGRATION\_UPDATE
> * INTEGRATION\_DELETE
>
> GUILD\_WEBHOOKS (1 << 5)
>
> * WEBHOOKS\_UPDATE
>
> GUILD\_INVITES (1 << 6)
>
> * INVITE\_CREATE
> * INVITE\_DELETE
>
> GUILD\_VOICE\_STATES (1 << 7)
>
> * VOICE\_STATE\_UPDATE
> * VOICE\_CHANNEL\_EFFECT\_SEND
>
> GUILD\_PRESENCES (1 << 8) ²
>
> * PRESENCE\_UPDATE
>
> GUILD\_MESSAGES (1 << 9)
>
> * MESSAGE\_CREATE
> * MESSAGE\_UPDATE
> * MESSAGE\_DELETE
> * MESSAGE\_DELETE\_BULK
>
> GUILD\_MESSAGE\_REACTIONS (1 << 10)
>
> * MESSAGE\_REACTION\_ADD
> * MESSAGE\_REACTION\_ADD\_MANY
> * MESSAGE\_REACTION\_REMOVE
> * MESSAGE\_REACTION\_REMOVE\_ALL
> * MESSAGE\_REACTION\_REMOVE\_EMOJI
>
> GUILD\_MESSAGE\_TYPING (1 << 11)
>
> * TYPING\_START
>
> DIRECT\_MESSAGES (1 << 12)
>
> * MESSAGE\_CREATE
> * MESSAGE\_UPDATE
> * MESSAGE\_DELETE
> * CHANNEL\_PINS\_UPDATE
>
> DIRECT\_MESSAGE\_REACTIONS (1 << 13)
>
> * MESSAGE\_REACTION\_ADD
> * MESSAGE\_REACTION\_ADD\_MANY
> * MESSAGE\_REACTION\_REMOVE
> * MESSAGE\_REACTION\_REMOVE\_ALL
> * MESSAGE\_REACTION\_REMOVE\_EMOJI
>
> DIRECT\_MESSAGE\_TYPING (1 << 14)
>
> * TYPING\_START
>
> MESSAGE\_CONTENT (1 << 15) ³
>
> GUILD\_SCHEDULED\_EVENTS (1 << 16)
>
> * GUILD\_SCHEDULED\_EVENT\_CREATE
> * GUILD\_SCHEDULED\_EVENT\_UPDATE
> * GUILD\_SCHEDULED\_EVENT\_DELETE
> * GUILD\_SCHEDULED\_EVENT\_USER\_ADD
> * GUILD\_SCHEDULED\_EVENT\_USER\_REMOVE
>
> GUILD\_EMBEDDED\_ACTIVITIES (1 << 17)
>
> * EMBEDDED\_ACTIVITY\_UPDATE\_V2
>
> PRIVATE\_CHANNELS (1 << 18) ⁴
>
> * CHANNEL\_CREATE
> * CHANNEL\_UPDATE
> * CHANNEL\_DELETE
> * CHANNEL\_RECIPIENT\_ADD
> * CHANNEL\_RECIPIENT\_REMOVE
>
> CALLS (1 << 19) ⁴ ⁵
>
> * AUDIO\_SETTINGS\_UPDATE
> * CALL\_CREATE
> * CALL\_UPDATE
> * CALL\_DELETE
> * VOICE\_STATE\_UPDATE
>
> AUTO\_MODERATION\_CONFIGURATION (1 << 20)
>
> * AUTO\_MODERATION\_RULE\_CREATE
> * AUTO\_MODERATION\_RULE\_UPDATE
> * AUTO\_MODERATION\_RULE\_DELETE
>
> AUTO\_MODERATION\_EXECUTION (1 << 21)
>
> * AUTO\_MODERATION\_ACTION\_EXECUTION
>
> USER\_RELATIONSHIPS (1 << 22) ⁴
>
> * RELATIONSHIP\_ADD
> * RELATIONSHIP\_UPDATE
> * RELATIONSHIP\_REMOVE
> * GAME\_RELATIONSHIP\_ADD
> * GAME\_RELATIONSHIP\_REMOVE
>
> USER\_PRESENCE (1 << 23) ⁴
>
> * PRESENCE\_UPDATE
>
> GUILD\_MESSAGE\_POLLS (1 << 24)
>
> * MESSAGE\_POLL\_VOTE\_ADD
> * MESSAGE\_POLL\_VOTE\_REMOVE
>
> DIRECT\_MESSAGE\_POLLS (1 << 25)
>
> * MESSAGE\_POLL\_VOTE\_ADD
> * MESSAGE\_POLL\_VOTE\_REMOVE
>
> DIRECT\_EMBEDDED\_ACTIVITIES (1 << 26)
>
> * EMBEDDED\_ACTIVITY\_UPDATE\_V2
>
> LOBBIES (1 << 27)
>
> * LOBBY\_CREATE
> * LOBBY\_UPDATE
> * LOBBY\_MEMBER\_ADD
> * LOBBY\_MEMBER\_UPDATE
> * LOBBY\_MEMBER\_REMOVE
> * LOBBY\_MESSAGE\_CREATE
> * LOBBY\_MESSAGE\_UPDATE
> * LOBBY\_MESSAGE\_DELETE
> * LOBBY\_VOICE\_SERVER\_UPDATE
> * LOBBY\_VOICE\_STATE\_UPDATE
>
> LOBBY\_DELETE (1 << 28)
>
> * LOBBY\_DELETE

</details>

#### Advertencias

Cualquier evento no definido en un intent se considera "passthrough" y siempre será enviado a ti.

Guild Member Update se envía para actualizaciones del usuario actual independientemente de si el intent `GUILD_MEMBERS` está establecido.

Message Create, Message Update, y Message Delete se envían para mensajes en MDs grupales independientemente de cualquier intent.

Guild Create y Request Guild Members son únicamente afectados por intents. Consulta estas secciones para más información.

Thread Members Update por defecto solo incluye si el usuario actual fue añadido o eliminado de un hilo. Para recibir estas actualizaciones para otros usuarios, solicita el intent Gateway `GUILD_MEMBERS`.

#### Intents privilegiados

Algunos intents se definen como privilegiados debido a la naturaleza sensible de los datos. Actualmente, esos intents incluyen:

* `GUILD_PRESENCES`
* `GUILD_MEMBERS`
* `MESSAGE_CONTENT`

Para bots, para especificar intents privilegiados en su carga útil `IDENTIFY`, deben estar autorizados para ellos. Las aplicaciones verificadas solo pueden usar intents privilegiados _después_ de que hayan sido aprobados para ellos. Las aplicaciones no verificadas pueden simplemente activarlos. Las cuentas de usuario no tienen el concepto de intents privilegiados, y pueden simplemente usar cualquier intent; sin embargo, tienen sus propias advertencias.

#### Habilitando intents privilegiados

Antes de que los bots puedan usar intents privilegiados, deben habilitarse en las flags de su aplicación. Las variantes limitadas de estos intents (usables por aplicaciones no verificadas) pueden simplemente activarse. Las variantes completas (usables por aplicaciones verificadas) requieren aprobación.

Las aplicaciones que califican para verificación deben primero ser verificadas, y puedes solicitar acceso a estos intents durante el proceso de verificación. Si la aplicación ya está verificada y necesitas solicitar intents privilegiados adicionales, puedes contactar soporte.

#### Restricciones Gateway

Los intents privilegiados afectan qué eventos Gateway los bots tienen permitido recibir. Para bots usando **API v8** y superior, todos los intents (privilegiados y no) deben especificarse en el parámetro `intents` al identificarse. Pasar un intent privilegiado en el parámetro `intents` sin tenerlo habilitado llevará a que la conexión Gateway se cierre con código de cierre (`4014`).

{% hint style="info" %}
Los eventos asociados con los _intents_ **GUILD\_PRESENCES** y **GUILD\_MEMBERS** están desactivados por defecto en todas las versiones de la API. Los bots que usan la API v7 y versiones anteriores recibirán eventos asociados con los _intents_ privilegiados que tengan, sin necesidad de incluir esos _intents_ en el parámetro de _intents_ al identificarse.
{% endhint %}

#### Restricciones HTTP

Además de las restricciones Gateway, los intents privilegiados también afectan los endpoints de API HTTP que los bots tienen permitido llamar, y los datos que pueden recibir. Por ejemplo, para usar el endpoint Get Guild Members, un bot debe tener el intent `GUILD_MEMBERS` habilitado. Si el bot no tiene el intent habilitado, el endpoint devolverá un error `403 Forbidden`.

Las restricciones de API HTTP son independientes de las restricciones Gateway, y no son afectadas por intents pasados en el parámetro `intents` al identificarse.

#### Intento de contenido de mensaje

`MESSAGE_CONTENT (1 << 15)` es un intent privilegiado único que no está directamente asociado con ningún evento Gateway. En su lugar, el acceso a `MESSAGE_CONTENT` permite a los usuarios recibir datos de contenido de mensaje a través de las APIs.

Mientras que las cuentas de usuario también pueden activar el intent `MESSAGE_CONTENT`, no están sujetas a las mismas restricciones que los bots. Las cuentas de usuario no tienen el concepto de intents privilegiados, y por lo tanto no necesitan ser aprobadas para el intent. Adicionalmente, las cuentas de usuario deben activar explícitamente el intent en todas las versiones de API si están utilizando intents.

Cualquier campo afectado por el intent de contenido de mensaje se nota en la documentación relevante. Por ejemplo, los campos `content`, `embeds`, `attachments`, `components`, y `poll` en objetos de mensaje todos contienen contenido de mensaje y por lo tanto requieren el intent.

Los usuarios **sin** el intent recibirán valores vacíos en campos que contienen contenido ingresado por usuario con pocas excepciones:

* Contenido en mensajes que el usuario envía
* Contenido en MDs con el usuario
* Contenido en el que el usuario es mencionado
* Contenido del mensaje en el que se usa un comando de menú contextual de mensaje

### Limitación de tasa <a href="#limitacin-de-tasa" id="limitacin-de-tasa"></a>

{% hint style="info" %}
Esta sección trata sobre los límites de velocidad del Gateway, no sobre los límites de velocidad de la API HTTP.
{% endhint %}

Los clientes pueden enviar 120 eventos Gateway por conexión cada 60 segundos, significando un promedio de 2 comandos por segundo. Los clientes que sobrepasen el límite son inmediatamente desconectados del Gateway. Similar a otros límites de tasa, los reincidentes tendrán su acceso a la API revocado.

Los clientes también tienen un límite para solicitudes identify concurrentes permitidas por 5 segundos. Si alcanzas este límite, el Gateway responderá con un Opcode 9 Invalid Session.

### Codificación y compresión <a href="#codificacin-y-compresin" id="codificacin-y-compresin"></a>

Al establecer una conexión al Gateway, los clientes pueden usar el parámetro `encoding` para elegir si comunicarse con Discord usando codificación JSON de texto plano o [ETF](https://www.erlang.org/doc/apps/erts/erl_ext_dist.html) binaria. Puedes elegir el tipo de codificación con el que te sientas más cómodo, pero ambos tienen sus propias peculiaridades. Si no estás seguro de qué codificación usar, JSON generalmente se recomienda.

Los clientes también pueden habilitar opcionalmente compresión para recibir paquetes comprimidos con zlib. La compresión de carga útil solo puede habilitarse cuando se usa codificación JSON, pero la compresión de transporte puede usarse independientemente del tipo de codificación.

#### Compresión de carga útil

{% hint style="warning" %}
Si un cliente está utilizando compresión de carga útil (_payload compression_), no puede usar compresión de transporte (_transport compression_).
{% endhint %}

La compresión de carga útil habilita compresión opcional por paquete para _algunos_ eventos cuando Discord está enviando eventos sobre la conexión.

La compresión de carga útil usa el formato zlib (ver [RFC1950 2.2](https://datatracker.ietf.org/doc/html/rfc1950#section-2.2)) al enviar cargas útiles. Para habilitar compresión de carga útil, tu cliente puede establecer `compress` a `true` al identificarse. Ten en cuenta que incluso cuando la compresión de carga útil está habilitada, no todas las cargas útiles serán comprimidas.

Cuando la compresión de carga útil está habilitada, tu cliente _debe_ detectar y descomprimir estas cargas útiles a JSON de texto plano antes de intentar analizarlas. Si estás usando compresión de carga útil, el Gateway no implementa un contexto de compresión compartido entre eventos enviados.

La compresión de carga útil será deshabilitada si usas compresión de transporte.

#### Usando codificación ETF

Cuando uses codificación ETF (External Term Format), hay algunos comportamientos específicos que deberías saber:

* Los IDs Snowflake se transmiten como enteros de 64 bits o strings.
* Tu cliente no puede enviar mensajes comprimidos al servidor.
* Al enviar cargas útiles, debes usar claves string. Usar claves atom resultará en código de cierre `4002`.

Ver [erlpack](https://github.com/discord/erlpack) para un ejemplo de implementación ETF.

#### Compresión de transporte

La compresión de transporte habilita compresión opcional para todos los paquetes cuando Discord está enviando eventos sobre la conexión. La única opción de compresión de transporte actualmente disponible es `zlib-stream`.

Cuando la compresión de transporte está habilitada, tu cliente necesita procesar datos recibidos a través de una sola conexión Gateway usando un contexto zlib compartido. Sin embargo, cada conexión Gateway debería usar su propio contexto zlib único.

Al procesar datos comprimidos por transporte, deberías empujar datos recibidos a un buffer hasta que recibas el sufijo de 4 bytes `Z_SYNC_FLUSH` (`00 00 ff ff`). Después de recibir el sufijo `Z_SYNC_FLUSH`, puedes entonces descomprimir el buffer.

#### Ejemplo de compresión de transporte

```python
# Z_SYNC_FLUSH suffix
ZLIB_SUFFIX = b'\x00\x00\xff\xff'
# initialize a buffer to store chunks
buffer = bytearray()
# create a shared zlib inflation context to run chunks through
inflator = zlib.decompressobj()
# ...
def on_websocket_message(msg):
    # always push the message data to your cache
    buffer.extend(msg)
    # check if the last four bytes are equal to ZLIB_SUFFIX
    if len(msg) < 4 or msg[-4:] != ZLIB_SUFFIX:
        return
    # if the message *does* end with ZLIB_SUFFIX,
    # get the full message by decompressing the buffers
    # NOTE: the message is utf-8 encoded.
    msg = inflator.decompress(buffer)
    buffer = bytearray()
    # here you can treat `msg` as either JSON or ETF encoded,
    # depending on your `encoding` param
```

### Rastreando estado <a href="#rastreando-estado" id="rastreando-estado"></a>

La mayoría del estado de un cliente se proporciona durante el evento Ready inicial (y opcionalmente los eventos Guild Create que siguen inmediatamente).

Mientras los recursos continúan siendo creados, actualizados, y eliminados, se envían eventos Gateway para notificar al cliente de estos cambios y proporcionar datos asociados. Para evitar llamadas API excesivas, los clientes deberían cachear tantos estados de recursos relevantes como sea posible, y actualizarlos cuando se reciban nuevos eventos.

Un ejemplo de rastreo de estado puede considerarse en el caso de un bot que quiere rastrear el estado de miembros: cuando se conecta inicialmente al Gateway, el bot recibirá información sobre el estado en línea de miembros del servidor (si están en línea, inactivos, en no molestar, o desconectados). Para mantener el estado actualizado, el bot rastreará y analizará eventos Presence Update cuando se reciban, luego actualizará los objetos de miembro cacheados en consecuencia.

### Disponibilidad de servidor <a href="#disponibilidad-de-servidor" id="disponibilidad-de-servidor"></a>

Cuando te conectas al Gateway como usuario bot, los servidores de los que el bot es parte empezarán como no disponibles. ¡No te asustes! El Gateway automáticamente intentará reconectarse en tu nombre. Mientras los servidores se vuelvan disponibles para ti, recibirás eventos Guild Create. Por otro lado, los usuarios empiezan con todos los servidores posibles disponibles para ellos.

### Sharding <a href="#sharding" id="sharding"></a>

Mientras los bots crecen y se añaden a un número creciente de servidores, algunos desarrolladores pueden encontrar necesario romper o dividir porciones de las operaciones de sus bots en procesos lógicos separados. Como tal, el Gateway implementa un método de sharding de servidor controlado por usuario que permite dividir eventos a través de un número de conexiones Gateway. El sharding de servidor es completamente controlado por usuario, y no requiere compartir estado entre conexiones separadas para operar.

El sharding es requerido para todos los bots en más de 2500 servidores. Mientras todos los bots y cuentas de usuario pueden utilizar sharding, está principalmente destinado para bots grandes. Nunca es necesario para cuentas de usuario, ya que están limitadas a un máximo de 200 servidores.

Para habilitar sharding en una conexión, el cliente debería enviar el array `shard` en la carga útil Identify. El primer elemento en este array debería ser el valor entero basado en cero del shard actual, mientras que el segundo representa el número total de shards. Los MDs solo se enviarán al shard 0.

{% hint style="info" %}
El endpoint **Get Gateway Bot** proporciona un número recomendado de _shards_ para tu cliente en el campo `shards`.
{% endhint %}

Para calcular qué eventos se enviarán a qué shard, la siguiente fórmula puede usarse:

#### Fórmula de sharding

```python
shard_id = (guild_id >> 22) % num_shards
```

Como ejemplo, si quisieras dividir la conexión entre tres shards, usarías los siguientes valores para `shard` para cada conexión: ``, `[1]`, y`` . Ten en cuenta que solo el primer shard (\`\`) recibiría MDs.

Ten en cuenta que `num_shards` no se relaciona con (o limita) el número total de sesiones potenciales. Solo se usa para _enrutar_ tráfico. Como tal, las sesiones no tienen que identificarse de manera uniformemente distribuida al hacer sharding. Puedes establecer múltiples sesiones con el mismo `[shard_id, num_shards]`, o sesiones con diferentes valores `num_shards`. Esto te permite crear sesiones que manejarán más o menos tráfico que otras para un balanceamiento de carga más fino, o para orquestar escalamiento/actualización de "tiempo de inactividad cero" entregando tráfico a un nuevo despliegue de sesiones con un conteo `num_shards` más alto o más bajo que se preparan en paralelo.

#### Concurrencia máxima

Si tienes múltiples shards, puedes iniciarlos concurrentemente basado en el valor `max_concurrency` devuelto a ti al inicio de sesión. Qué shards puedes iniciar concurrentemente se asignan basado en una clave para cada shard. La clave de límite de tasa para un shard dado puede computarse con

```python
rate_limit_key = shard_id % max_concurrency
```

Esto pone tus shards en "buckets" de tamaño `max_concurrency`. Cuando inicias tu bot, puedes iniciar hasta `max_concurrency` shards a la vez, y debes iniciarlos por "bucket" **en orden**. Para explicar de otra manera, digamos que tienes 16 shards, y tu `max_concurrency` es 16:

```python
shard_id: 0, rate limit key (0 % 16): 0
shard_id: 1, rate limit key (1 % 16): 1
shard_id: 2, rate limit key (2 % 16): 2
shard_id: 3, rate limit key (3 % 16): 3
shard_id: 4, rate limit key (4 % 16): 4
shard_id: 5, rate limit key (5 % 16): 5
shard_id: 6, rate limit key (6 % 16): 6
shard_id: 7, rate limit key (7 % 16): 7
shard_id: 8, rate limit key (8 % 16): 8
shard_id: 9, rate limit key (9 % 16): 9
shard_id: 10, rate limit key (10 % 16): 10
shard_id: 11, rate limit key (11 % 16): 11
shard_id: 12, rate limit key (12 % 16): 12
shard_id: 13, rate limit key (13 % 16): 13
shard_id: 14, rate limit key (14 % 16): 14
shard_id: 15, rate limit key (15 % 16): 15
```

Puedes iniciar todos los 16 de tus shards a la vez, porque cada uno tiene un `rate_limit_key` que llena el bucket de 16 shards. Sin embargo, digamos que tenías 32 shards:

```python
shard_id: 0, rate limit key (0 % 16): 0
shard_id: 1, rate limit key (1 % 16): 1
shard_id: 2, rate limit key (2 % 16): 2
shard_id: 3, rate limit key (3 % 16): 3
shard_id: 4, rate limit key (4 % 16): 4
shard_id: 5, rate limit key (5 % 16): 5
shard_id: 6, rate limit key (6 % 16): 6
shard_id: 7, rate limit key (7 % 16): 7
shard_id: 8, rate limit key (8 % 16): 8
shard_id: 9, rate limit key (9 % 16): 9
shard_id: 10, rate limit key (10 % 16): 10
shard_id: 11, rate limit key (11 % 16): 11
shard_id: 12, rate limit key (12 % 16): 12
shard_id: 13, rate limit key (13 % 16): 13
shard_id: 14, rate limit key (14 % 16): 14
shard_id: 15, rate limit key (15 % 16): 15
shard_id: 16, rate limit key (16 % 16): 0
shard_id: 17, rate limit key (17 % 16): 1
shard_id: 18, rate limit key (18 % 16): 2
shard_id: 19, rate limit key (19 % 16): 3
shard_id: 20, rate limit key (20 % 16): 4
shard_id: 21, rate limit key (21 % 16): 5
shard_id: 22, rate limit key (22 % 16): 6
shard_id: 23, rate limit key (23 % 16): 7
shard_id: 24, rate limit key (24 % 16): 8
shard_id: 25, rate limit key (25 % 16): 9
shard_id: 26, rate limit key (26 % 16): 10
shard_id: 27, rate limit key (27 % 16): 11
shard_id: 28, rate limit key (28 % 16): 12
shard_id: 29, rate limit key (29 % 16): 13
shard_id: 30, rate limit key (30 % 16): 14
shard_id: 31, rate limit key (31 % 16): 15
```

En este caso, debes iniciar los buckets de shard **en "orden"**. Eso significa que puedes iniciar shard 0 -> shard 15 concurrentemente, y luego puedes iniciar shard 16 -> shard 31.

#### Sharding para bots grandes

Para bots en más de 150,000 servidores, hay algunas consideraciones adicionales que debes tomar en cuenta sobre sharding. Discord migrará tu bot a sharding de bot grande cuando empiece a acercarse al umbral de sharding de bot grande. El propietario(s) del bot recibirá un MD del sistema y email confirmando que este movimiento se ha completado así como qué número de shard ha sido asignado.

El número de shards que ejecutes debe ser un múltiplo del número de shard proporcionado al contactarte. Si intentas iniciar tu bot con un número inválido de shards, tu conexión Gateway se cerrará con código de cierre `4010`.

El endpoint Get Gateway Bot siempre devolverá la cantidad correcta de shards, así que si ya estás usando este endpoint para determinar tu número de shards, no deberías requerir cambios.

El límite de inicio de sesión para estos bots también será incrementado de 1000 a `max(2000, (guild_count / 1000) * 5)` por día. También recibes un `max_concurrency` incrementado, el número de shards que puedes iniciar concurrentemente.

### Objeto límite de inicio de sesión <a href="#objeto-lmite-de-inicio-de-sesin" id="objeto-lmite-de-inicio-de-sesin"></a>

#### Estructura de límite de inicio de sesión

| Campo            | Tipo    | Descripción                                                          |
| ---------------- | ------- | -------------------------------------------------------------------- |
| total            | integer | Número total de inicios de sesión que el usuario tiene permitido.    |
| remaining        | integer | Número restante de inicios de sesión que el usuario tiene permitido. |
| reset\_after     | integer | Número de milisegundos después de los cuales el límite se resetea.   |
| max\_concurrency | integer | Número de solicitudes identify permitidas por 5 segundos.            |

### Endpoints <a href="#endpoints" id="endpoints"></a>

#### Get Gateway

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /gateway

Devuelve un objeto con una sola URL WebSocket válida, que el cliente puede usar para Conectarse. Los clientes **deberían** cachear este valor y solo llamar a este endpoint para recuperar una nueva URL si no pueden establecer adecuadamente una conexión usando la cacheada.

#### Cuerpo de respuesta

| Campo | Tipo   | Descripción                                                   |
| ----- | ------ | ------------------------------------------------------------- |
| url   | string | La URL WebSocket que puede usarse para conectarse al Gateway. |

#### Ejemplo de respuesta

```python
{
  "url": "wss://gateway.discord.gg"
}
```

#### Get Gateway Bot

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /gateway/bot

Devuelve un objeto basado en la información en Get Gateway, más metadatos adicionales que pueden ayudar durante la operación de bots grandes o con sharding. A diferencia de Get Gateway, esta ruta no debería cachearse por períodos extendidos de tiempo ya que el valor no está garantizado a ser el mismo por llamada, y cambia mientras el usuario se une/sale de servidores.

#### Cuerpo de respuesta

| Campo                 | Tipo                       | Descripción                                                   |
| --------------------- | -------------------------- | ------------------------------------------------------------- |
| url                   | string                     | La URL WebSocket que puede usarse para conectarse al Gateway. |
| shards                | integer                    | El número recomendado de shards a usar al conectarse.         |
| session\_start\_limit | session start limit object | Información sobre el límite actual de inicio de sesión.       |

#### Ejemplo de respuesta

```python
{
  "url": "wss://gateway.discord.gg",
  "shards": 9,
  "session_start_limit": {
    "total": 1000,
    "remaining": 999,
    "reset_after": 14400000,
    "max_concurrency": 1
  }
}
```

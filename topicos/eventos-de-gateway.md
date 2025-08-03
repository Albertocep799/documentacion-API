---
icon: calendar-star
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

# Eventos de gateway

Las conexiones Gateway son WebSockets, lo que significa que son bidireccionales y cualquier lado del WebSocket puede enviar eventos al otro. Los siguientes eventos se dividen en dos tipos:

* **Los eventos de envío** son eventos Gateway enviados por un cliente a Discord (como cuando se identifica con el Gateway).
* **Los eventos de recepción** son eventos Gateway que son enviados por Discord a un cliente. Estos eventos típicamente representan algo que ocurre dentro de un servidor donde el usuario es miembro, como un canal siendo actualizado.

Todos los eventos Gateway están encapsulados en una carga útil Gateway.

Para más información sobre interactuar con el Gateway, puedes referenciar la documentación Gateway.

#### Nombres de eventos

En la práctica, los nombres de eventos están en MAYÚSCULAS con guiones\_bajos uniendo cada palabra en el nombre. Por ejemplo, Channel Create sería `CHANNEL_CREATE` y Voice State Update sería `VOICE_STATE_UPDATE`.

Para legibilidad, los nombres de eventos en la siguiente documentación típicamente se dejan en Título Case.

#### Estructura de carga útil Gateway

Las cargas útiles de eventos Gateway tienen una estructura común, pero el contenido de los datos asociados (`d`) varía entre los diferentes eventos.

<sup>1</sup> Estos campos solo se reciben, y son `null` cuando el `op` no es `DISPATCH`.

| Campo           | Tipo        | Descripción                                                                 |
| --------------- | ----------- | --------------------------------------------------------------------------- |
| op              | integer     | Opcode Gateway, que indica el tipo de carga útil.                           |
| d               | ?JSON value | Datos del evento.                                                           |
| s?<sup>1</sup>  | ?integer    | Número de secuencia del evento usado para reanudar sesiones y heartbeating. |
| t?<sup>1</sup>  | ?string     | Nombre del evento para esta carga útil (solo Opcode `DISPATCH`).            |

#### **Ejemplo de carga útil Gateway (Envío)**

```json
{
  "op": 2,
  "d": {}
}
```

#### **Ejemplo de carga útil Gateway (Recepción)**

```json
{
  "op": 0,
  "d": {},
  "s": 42,
  "t": "GATEWAY_EVENT_NAME"
}
```

### Eventos de envío

Los eventos de envío son eventos Gateway encapsulados en una carga útil de evento, y son enviados por un cliente a Discord a través de una conexión Gateway.

| Nombre                       | Descripción                                                                               |
| ---------------------------- | ----------------------------------------------------------------------------------------- |
| Identify                     | Disparar el handshake inicial con el Gateway.                                             |
| Resume                       | Reanudar una conexión Gateway perdida.                                                    |
| Heartbeat                    | Mantener una conexión Gateway activa.                                                     |
| Update Presence              | Actualizar la presencia del cliente.                                                      |
| Update Voice State           | Unirse, mover, o desconectar el cliente de un canal de voz o llamada.                     |
| Ping Voice Server            | Hacer ping a los servidores de voz de Discord.                                            |
| Create Stream                | Crear un stream para el cliente (Go Live).                                                |
| Watch Stream                 | Ver el stream de un usuario.                                                              |
| Set Stream Paused            | Pausar/reanudar un stream del cliente.                                                    |
| Delete Stream                | Terminar un stream del cliente.                                                           |
| Ping Stream Server           | Hacer ping al servidor de voz del stream de un usuario.                                   |
| Request Guild Members        | Solicitar miembros para uno o más servidores.                                             |
| Request Call Connect         | Solicitar información de llamada pre-existente de un canal privado.                       |
| Update Lobby Voice States    | Actualizar estados de voz para múltiples lobbies.                                         |
| Update Guild Subscriptions   | Actualizar suscripciones para un servidor.                                                |
| Request Forum Unreads        | Solicitar conteos de no leídos de canales solo-hilo.                                      |
| Remote Command               | Enviar un mensaje a otra sesión Gateway.                                                  |
| Request Deleted Entity IDs   | Solicitar IDs de entidades eliminadas que no coincidan con un hash dado para un servidor. |
| Request Soundboard Sounds    | Solicitar sonidos de soundboard para uno o más servidores.                                |
| Request Last Messages        | Solicitar últimos mensajes para los canales de un servidor.                               |
| Search Recent Members        | Solicitar miembros recientemente unidos para un servidor.                                 |
| Request Channel Statuses     | Solicitar estados de canales de voz para un servidor.                                     |
| Request Channel Member Count | Solicitar el número de miembros que pueden ver un canal.                                  |
| QoS Heartbeat                | Mantener una conexión Gateway activa con métricas QoS.                                    |
| Update Time Spent Session ID | Rastrear el tiempo pasado en la sesión actual.                                            |

#### **Identidad**

Usado para disparar el handshake inicial con el Gateway.

Los detalles sobre identificarse están en la documentación Gateway.

#### **Estructura identificación**

<sup>1</sup> Para cuentas de usuario, el `status` y `activities` especificados pueden no siempre ser respetados. El campo solo debería usarse cuando se re-identifica para comunicar la última presencia conocida del usuario. De otro modo, el status debería ser `unknown` y activities un array vacío.

<sup>2</sup> Requerido para bots en API v8 y superior.

<sup>3</sup> No soportado en contextos OAuth2.

| Campo                                | Tipo                     | Descripción                                                                                                                                                                                                                                                                       |
| ------------------------------------ | ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| token                                | string                   | Token de autenticación.                                                                                                                                                                                                                                                           |
| properties                           | client properties object | Información del cliente y sistema.                                                                                                                                                                                                                                                |
| compress?                            | boolean                  | Si esta conexión usa compresión de carga útil legacy (por defecto false).                                                                                                                                                                                                         |
| large\_threshold?                    | integer                  | Número total de miembros donde, para bots, el Gateway dejará de enviar miembros desconectados en la lista de miembros del servidor, o, para usuarios, dejar de enviar eventos no-stateful para servidores sin suscripción (25-250, por defecto 25 para bots y 250 para usuarios). |
| shard?                               | array\[integer, integer] | El shard de la conexión (shard\_id, num\_shards), usado para sharding de conexión.                                                                                                                                                                                                |
| presence? <sup>1</sup> <sup>3</sup>  | update presence object   | Información de presencia inicial.                                                                                                                                                                                                                                                 |
| intents? <sup>2</sup>                | integer                  | Los intents Gateway que deseas recibir.                                                                                                                                                                                                                                           |
| capabilities?                        | integer                  | Las capacidades Gateway que deseas habilitar.                                                                                                                                                                                                                                     |
| client\_state? <sup>3</sup>          | client state object      | El estado de caché actual del cliente, usado para reducir transmisión de información innecesaria en re-identificación.                                                                                                                                                            |

#### **Ejemplo identificación**

```json
{
  "op": 2,
  "d": {
    "token": "my_token",
    "properties": {
      "os": "linux",
      "browser": "disco",
      "device": "disco"
    },
    "compress": false,
    "presence": {
      "activities": [],
      "status": "unknown",
      "since": 0,
      "afk": false
    },
    "capabilities": 16381,
    "client_state": {
      "api_code_version": 0,
      "guild_versions": {}
    }
  }
}
```

#### **Resumen**

Usado para reproducir eventos perdidos cuando un cliente desconectado se reanuda.

Los detalles sobre reanudar están en la documentación Gateway.

#### **Estructura de resumen**

| Campo       | Tipo    | Descripción                          |
| ----------- | ------- | ------------------------------------ |
| token       | string  | Token de autenticación.              |
| session\_id | string  | ID de sesión existente.              |
| seq         | integer | Último número de secuencia recibido. |

#### **Ejemplo de resumen**

```json
{
  "op": 6,
  "d": {
    "token": "randomstring",
    "session_id": "30f32c5d54ae86130fc4a215c7474263",
    "seq": 1337
  }
}
```

#### **Heartbeat**

Usado para mantener una conexión Gateway activa. Debe enviarse cada `heartbeat_interval` milisegundos después de que se reciba la carga útil Opcode 10 Hello. La clave interna `d` es el último número de secuencia—`s`—recibido por el cliente. Si aún no has recibido uno, envía `null`. Dispara un evento Gateway Heartbeat ACK.

Los detalles sobre heartbeats están en la documentación Gateway.

#### **Ejemplo de Heartbeat**

```json
{
  "op": 1,
  "d": 251
}
```

#### **Actualizar presencia**

Enviado por el cliente para indicar una actualización de presencia.

{% hint style="warning" %}
Los clientes solo pueden actualizar su presencia 5 veces cada 20 segundos.
{% endhint %}

{% hint style="warning" %}
Todos los eventos entrantes del Gateway se pausarán hasta que la presencia actualizada se haya propagado.
{% endhint %}

#### **Estructura de actualización de presencia**

| Campo      | Tipo                    | Descripción                                                                                  |
| ---------- | ----------------------- | -------------------------------------------------------------------------------------------- |
| activities | array\[activity object] | Las actividades del usuario.                                                                 |
| status     | string                  | El nuevo estado del usuario.                                                                 |
| since      | integer                 | Timestamp Unix (en milisegundos) de cuándo el cliente se volvió inactivo, o 0 si no lo está. |
| afk        | boolean                 | Si el cliente está AFK o no, usado para determinar si despachar notificaciones push móviles. |

#### **Ejemplo de actualización de presencia**

```json
{
  "op": 3,
  "d": {
    "since": 0,
    "activities": [
      {
        "application_id": "383226320970055681",
        "assets": {
          "large_image": "565945350846939145",
          "large_text": "Editing a TEXT file",
          "small_image": "565945770067623946",
          "small_text": "Visual Studio Code"
        },
        "buttons": ["View Repository"],
        "created_at": "1695164784863",
        "details": "Editing index.astro",
        "flags": 0,
        "id": "d11307d8c0abb135",
        "name": "Visual Studio Code",
        "session_id": "30f32c5d54ae86130fc4a215c7474263",
        "state": "Workspace: vendicated.dev",
        "timestamps": {
          "start": "1695164482423"
        },
        "type": 0
      }
    ],
    "status": "online",
    "afk": false
  }
}
```

#### **Actualizar estado de voz**

Enviado cuando un cliente quiere unirse, moverse, o desconectarse de un canal de voz. Dispara un Voice State Update y opcionalmente un evento Gateway Voice Server Update.

#### **Estructura de actualización del estado de voz**

| Campo               | Tipo           | Descripción                                                                                 |
| ------------------- | -------------- | ------------------------------------------------------------------------------------------- |
| guild\_id           | ?snowflake     | El ID del servidor en el que está el canal de voz, si aplica.                               |
| channel\_id         | ?snowflake     | El ID del canal de voz o privado al que el cliente quiere unirse (`null` si se desconecta). |
| self\_mute          | boolean        | Si el cliente está silenciado.                                                              |
| self\_deaf          | boolean        | Si el cliente está ensordecido.                                                             |
| self\_video?        | boolean        | Si el cliente está transmitiendo video al canal.                                            |
| preferred\_region?  | string         | El ID de región de voz preferida para el canal de voz.                                      |
| preferred\_regions? | array\[string] | Los IDs de región de voz clasificados para el canal de voz.                                 |
| flags?              | integer        | Las flags de voz del cliente.                                                               |

#### **Ejemplo actualización del estado de voz**

```json
{
  "op": 4,
  "d": {
    "guild_id": "41771983423143937",
    "channel_id": "127121515262115840",
    "self_mute": false,
    "self_deaf": false,
    "self_video": false,
    "preferred_region": "newark",
    "preferred_regions": ["newark", "us-central", "us-east", "atlanta", "us-south"],
    "flags": 3
  }
}
```

#### **Conexión del servidor de voz**

Enviado cuando un cliente quiere solicitar que el Gateway haga ping a un servidor de voz que se comporta mal. Esto forzará una verificación del lado del servidor y potencialmente reasignará el servidor de voz. La clave interna `d` debería establecerse a `null`. Puede disparar un evento Gateway Voice Server Update.

#### **Ejemplo de conexión del servidor de voz**

```json
{
  "op": 5,
  "d": null
}
```

#### **Actualizar estado del canal de voz**

Enviado por el cliente para actualizar el estado de voz del usuario en múltiples lobbies. Dispara un Lobby Voice State Update y opcionalmente evento Gateway Lobby Voice Server Update.

La clave interna `d` es un array de los siguientes objetos:

#### **Estructura de actualización de estado del canal de voz**

| Campo               | Tipo           | Descripción                                          |
| ------------------- | -------------- | ---------------------------------------------------- |
| lobby\_id           | snowflake      | El ID del lobby.                                     |
| self\_mute          | boolean        | Si el cliente está silenciado.                       |
| self\_deaf          | boolean        | Si el cliente está ensordecido.                      |
| self\_video?        | boolean        | Si el cliente está transmitiendo video al canal.     |
| preferred\_region?  | string         | El ID de región de voz preferida para el lobby.      |
| preferred\_regions? | array\[string] | Los IDs de región de voz clasificados para el lobby. |

#### **Crear directo**

Enviado por el cliente para crear un stream en un canal de voz dado. Requiere el permiso `STREAM` en el canal dado. Dispara un Stream Create y evento Gateway Stream Server Update.

#### **Estructura creación de directo**

| Campo              | Tipo       | Descripción                                            |
| ------------------ | ---------- | ------------------------------------------------------ |
| type               | string     | El tipo de stream a crear.                             |
| guild\_id?         | ?snowflake | El ID del servidor en el que hacer stream, si aplica.  |
| channel\_id        | snowflake  | El ID del canal de voz en el que hacer stream.         |
| preferred\_region? | string     | El ID de región de voz preferida para el canal de voz. |

#### **Ejemplo creación de directo**

```json
{
  "op": 18,
  "d": {
    "type": "call",
    "guild_id": null,
    "channel_id": "1142105002492575794",
    "preferred_region": "us-east"
  }
}
```

#### Ver directo

Enviado por el cliente para empezar a ver el stream de un usuario. El usuario debe estar conectado al canal de voz asociado. Dispara un Stream Create y Stream Server Update o un evento Gateway Stream Delete.

#### **Estructura visualizar directo**

| Campo       | Tipo   | Descripción               |
| ----------- | ------ | ------------------------- |
| stream\_key | string | La clave de stream a ver. |

#### **Ejemplo visualizar directo**

```json
{
  "op": 20,
  "d": {
    "stream_key": "call:1110739331624210483:852892297661906993"
  }
}
```

#### **Establecer pausa de directo**

Enviado por el cliente para pausar o reanudar su stream. El usuario debe ser el propietario del stream. Dispara un evento Gateway Stream Server Update.

#### **Estructura de directo pausado**

| Campo       | Tipo    | Descripción                                 |
| ----------- | ------- | ------------------------------------------- |
| stream\_key | string  | La clave de stream a pausar o reanudar.     |
| paused      | boolean | Si el stream debería pausarse o reanudarse. |

#### **Ejemplo de establecer directo pausado**

```json
{
  "op": 22,
  "d": {
    "stream_key": "call:1110739331624210483:852892297661906993",
    "paused": true
  }
}
```

**Eliminar directo**

Enviado por el cliente para desconectarse de un stream. Si el cliente es el propietario del stream, el stream será eliminado. Dispara un evento Gateway Stream Delete.

**Estructura de eliminación de directo**

| Campo       | Tipo   | Descripción                    |
| ----------- | ------ | ------------------------------ |
| stream\_key | string | La clave de stream a eliminar. |

**Ejemplo de eliminación de directo**

```json
{
  "op": 19,
  "d": {
    "stream_key": "call:1110739331624210483:852892297661906993"
  }
}
```

Mención de directo de servidor

Enviado cuando un cliente quiere solicitar que el Gateway haga ping a un servidor de stream que se comporta mal. Esto forzará una verificación del lado del servidor y potencialmente reasignará el servidor de stream. Puede disparar un evento Gateway Stream Server Update.

**Estructura de m**ención de directo de servidor

| Campo       | Tipo   | Descripción                      |
| ----------- | ------ | -------------------------------- |
| stream\_key | string | La clave de stream a hacer ping. |

**Ejemplo de m**ención de directo de servidor

```json
{
  "op": 21,
  "d": {
    "stream_key": "call:1110739331624210483:852892297661906993"
  }
}
```

**Solicitud de miembros de servidor**

Usado para solicitar todos los miembros para un servidor o una lista de servidores. Cuando se conecta inicialmente, si no tienes el intent Gateway `GUILD_PRESENCES`, o si el servidor tiene más de 75k miembros, solo enviará miembros que están en voz, más el miembro para ti (el usuario que se conecta).

De otro modo, si un servidor tiene más de `large_threshold` miembros (valor en el Gateway Identify), solo enviará miembros que están en línea, tienen un rol, tienen un apodo, o están en un canal de voz, y si tiene menos de `large_threshold` miembros, enviará todos los miembros.

Si un cliente desea recibir miembros adicionales, necesita solicitarlos explícitamente a través de esta operación. Dispara múltiples eventos Guild Members Chunk con hasta 1000 miembros por chunk hasta que todos los miembros que coincidan con la solicitud hayan sido enviados.

Debido a preocupaciones de privacidad e infraestructura con esta función, hay algunas limitaciones que aplican:

* Para bots, se requiere el intent privilegiado `GUILD_PRESENCES` para solicitar presencias
* Para bots, se requiere el intent privilegiado `GUILD_MEMBERS` para solicitar la lista completa de miembros (query de "" y limit de 0)
* Para usuarios, se requieren los permisos `MANAGE_ROLES`, `KICK_MEMBERS`, o `BAN_MEMBERS` para solicitar la lista completa de miembros (query de "" y limit de 0)
* Para bots, solo se puede solicitar un ID de servidor a la vez
* Solicitar un prefijo (parámetro `query`) devolverá un máximo de 100 miembros
* `user_ids` será limitado a 100 miembros

**Estructura de solicitud de miembros de servidor**

<sup>1</sup> Se requiere uno de `query` o `user_ids`.

<sup>2</sup> Requerido cuando se especifica `query`.

<sup>3</sup> El nonce solo puede ser de hasta 32 bytes. Si envías un nonce inválido, será ignorado, y el chunk(s) de miembros de respuesta no tendrá un nonce establecido.

| Campo                   | Tipo                             | Descripción                                                                                               |
| ----------------------- | -------------------------------- | --------------------------------------------------------------------------------------------------------- |
| guild\_id               | array\[snowflake] \| snowflake   | ID(s) del servidor(s) para el que obtener miembros.                                                       |
| query? <sup>1</sup>     | string                           | String con el que el nombre de usuario/apodo empieza, o un string vacío para devolver todos los miembros. |
| limit? <sup>2</sup>     | integer                          | Número máximo de miembros a enviar que coincidan con el `query` (0-100, debe ser 0 con un `query` vacío). |
| presences?              | boolean                          | Si la presencia de miembros coincidentes será devuelta.                                                   |
| user\_ids? <sup>1</sup> | snowflake or array of snowflakes | Los IDs de usuario a solicitar (máx 100).                                                                 |
| nonce? <sup>3</sup>     | string                           | Nonce para identificar la respuesta Guild Members Chunk.                                                  |

**Ejemplo solicitud de miembros de servidor**

```json
{
  "op": 8,
  "d": {
    "guild_id": ["41771983444115456"],
    "query": "",
    "limit": 0
  }
}
```

**Solicitud de conexión de llamada**

Usado para solicitar datos de llamada pre-existentes de un canal privado, creados antes de que la conexión Gateway fuera establecida. Dispara un evento Gateway Call Create si se encuentra una llamada.

{% hint style="info" %}
Al utilizar la capacidad del Gateway **AUTO\_CALL\_CONNECT**, ya no es necesario solicitar explícitamente una conexión a la llamada. En su lugar, el cliente recibirá automáticamente eventos **Call Create** para las llamadas preexistentes al conectarse al Gateway.
{% endhint %}

**Estructura de conexión de llamada**

| Campo       | Tipo      | Descripción                  |
| ----------- | --------- | ---------------------------- |
| channel\_id | snowflake | ID del canal MD o MD grupal. |

**Ejemplo de solicitud de conexión de llamada**

```json
{
  "op": 13,
  "d": {
    "channel_id": "957057010334048288"
  }
}
```

**Comando remoto (Envío)**

Usado para enviar un mensaje a otra sesión Gateway. Típicamente, esto se usa para controlar sesiones embebidas, como una sesión Xbox o PlayStation. Dispara un evento Gateway Remote Command en la sesión objetivo.

**Estructura de comando remoto**

<sup>1</sup> La carga útil enviada no está estandarizada o validada por el servidor.

| Campo               | Tipo   | Descripción                      |
| ------------------- | ------ | -------------------------------- |
| target\_session\_id | string | ID de la sesión a la que enviar. |
| payload 1           | any    | La carga útil a enviar.          |

**Ejemplo de comando remoto**

```json
{
  "op": 29,
  "d": {
    "target_session_id": "30f32c5d54ae86130fc4a215c7474263",
    "payload": {
      "type": "VOICE_STATE_UPDATE",
      "self_mute": false,
      "self_deaf": false
    }
  }
}

```

**Solicitud de sonidos de mesa de sonidos**

Usado para solicitar sonidos de soundboard para una lista de servidores. Dispara un evento Gateway Soundboard Sounds para cada servidor en respuesta.

**Estructura de solicitud de sonidos de mesa de sonidos**

| Campo      | Tipo              | Descripción                                                           |
| ---------- | ----------------- | --------------------------------------------------------------------- |
| guild\_ids | array\[snowflake] | Los IDs de los servidores para los que obtener sonidos de soundboard. |

**Ejemplo de solicitud de sonidos de mesa de sonidos**

```json
{
  "op": 31,
  "d": {
    "guild_ids": ["41771983444115456", "1015060230222131221", "811255666990907402"]
  }
}
```

**Solicitud de últimos mensajes**

Usado para solicitar los últimos mensajes (indicados por el campo `last_message_id`) de canales. El usuario debe ser miembro del servidor. Dispara un evento Gateway Last Messages con hasta 100 mensajes que coincidan con la solicitud.

**Estructura de solicitud de últimos mensajes**

| Campo        | Tipo              | Descripción                                                               |
| ------------ | ----------------- | ------------------------------------------------------------------------- |
| guild\_id    | snowflake         | El ID del servidor.                                                       |
| channel\_ids | array\[snowflake] | Los IDs de los canales para los que solicitar últimos mensajes (máx 100). |

**Ejemplo de solicitud de últimos mensajes**

```json
{
  "op": 34,
  "d": {
    "guild_id": "957057010334048288",
    "channel_ids": ["1145501524013895733", "1145501524013895734"]
  }
}
```

**Buscar miembros recientes**

Usado para buscar los 10,000 miembros más recientemente unidos en un servidor. El usuario debe ser miembro del servidor. Dispara un evento Gateway Guild Members Chunk con hasta 1000 miembros que coincidan con la solicitud.

**Estructura de búsqueda de miembros recientes**

<sup>1</sup> Cuando se proporciona una query, los resultados se limitan a un miembro.

<sup>2</sup> La paginación se basa en el campo `joined_at` descendente. Para paginar, proporcionas el ID de miembro del miembro que se unió más temprano en el Guild Members Chunk recibido previamente. Solo puedes paginar hasta 10,000 miembros hacia atrás desde el miembro más recientemente unido.

<sup>3</sup> El nonce solo puede ser de hasta 32 bytes. Si envías un nonce inválido, será ignorado, y el chunk de miembros de respuesta no tendrá un nonce establecido.

| Campo                            | Tipo       | Descripción                                                                                               |
| -------------------------------- | ---------- | --------------------------------------------------------------------------------------------------------- |
| guild\_id                        | snowflake  | El ID del servidor en el que buscar miembros.                                                             |
| query <sup>1</sup>               | string     | String con el que el nombre de usuario/apodo empieza, o un string vacío para devolver todos los miembros. |
| continuation\_token <sup>2</sup> | ?snowflake | El ID de miembro desde el que continuar la paginación.                                                    |
| nonce? <sup>3</sup>              | string     | Nonce para identificar la respuesta Guild Members Chunk.                                                  |

**Ejemplo de búsqueda de miembros recientes**

```json
{
  "op": 35,
  "d": {
    "guild_id": "957057010334048288",
    "query": "",
    "continuation_token": null
  }
}
```

**Solicitar estado de los canales**

Usado para solicitar los estados de canales de voz para un servidor. Dispara un evento Gateway Channel Statuses con los estados solicitados.

**Estructura de solicitud de estado de los canales**

| Campo     | Tipo      | Descripción         |
| --------- | --------- | ------------------- |
| guild\_id | snowflake | El ID del servidor. |

**Ejemplo de solicitud de estado de los canales**

```json
{
  "op": 36,
  "d": {
    "guild_id": "957057010334048288"
  }
}
```

**Solicitud de cantidad de miembros de un canal**

Usado para solicitar el número de miembros que pueden ver un canal de servidor, así como cuántos están en línea en el momento dado. Requiere el permiso `VIEW_CHANNEL` en el canal dado. Dispara un evento Gateway Channel Member Count Update con el conteo solicitado.

**Estructura de solicitud de cantidad de miembros de un canal**

| Campo       | Tipo      | Descripción         |
| ----------- | --------- | ------------------- |
| guild\_id   | snowflake | El ID del servidor. |
| channel\_id | snowflake | El ID del canal.    |

**Ejemplo de solicitud de cantidad de miembros de un canal**

```json
{
  "op": 39,
  "d": {
    "guild_id": "957057010334048288",
    "channel_id": "1145501524013895733"
  }
}
```

**Calidad de servicio de Heartbeat**

Igual que un heartbeat normal, pero también rastrea estadísticas de Calidad de Servicio (QoS).

Debe enviarse cada `heartbeat_interval` milisegundos después de que se reciba la carga útil Opcode 10 Hello. Dispara un evento Gateway Heartbeat ACK.

Los detalles sobre heartbeats están en la documentación Gateway.

**Estructura de calidad de servicio de Heartbeat**

| Campo | Tipo        | Descripción                            |
| ----- | ----------- | -------------------------------------- |
| seq   | ?integer    | Último número de secuencia recibido.   |
| qos   | QoS payload | La carga útil QoS para este heartbeat. |

**Estructura QoS Payload**

| Campo   | Tipo           | Descripción                                                       |
| ------- | -------------- | ----------------------------------------------------------------- |
| ver     | integer        | La versión de heartbeat del cliente (actualmente `25`).           |
| active  | boolean        | Si la sesión está actualmente activa (tiene razones de servicio). |
| reasons | array\[string] | Razones de Servicio.                                              |

**Razones de servicio**

| Nombre         | Descripción                                    |
| -------------- | ---------------------------------------------- |
| foregrounded   | La ventana está enfocada.                      |
| rtc\_connected | El usuario está conectado a la llamada de voz. |

**Ejemplo de calidad de servicio de Heartbeat**

```json
{
  "op": 40,
  "d": {
    "seq": 1337,
    "qos": {
      "ver": 25,
      "active": true,
      "reasons": ["foregrounded", "rtc_connected"]
    }
  }
}
```

#### Actualizar el ID de sesión de tiempo invertido

Enviado cada 30 minutos (si el cliente está enfocado o está en una llamada de voz) y al conectarse para rastrear el tiempo que el usuario ha pasado en la sesión actual.

Cuando esto se envía, debería enviarse un QoS Heartbeat acompañante.

**Estructura de calidad de servicio de Heartbeat**

| Campo                     | Tipo    | Descripción                                                                                          |
| ------------------------- | ------- | ---------------------------------------------------------------------------------------------------- |
| initialization\_timestamp | integer | Timestamp Unix (en milisegundos) de cuándo se generó el ID de sesión.                                |
| session\_id               | string  | Un UUID generado por el cliente, igual que `client_heartbeat_session_id` en propiedades del cliente. |
| client\_launch\_id        | string  | Un UUID generado por el cliente, igual que `client_launch_id` en propiedades del cliente.            |

#### Ejemplo de actualización del ID de sesión de tiempo invertido

```json
{
  "op": 41,
  "d": {
    "initialization_timestamp": 1753221861697,
    "session_id": "fe223885-211d-4826-ba7b-395373da4213",
    "client_launch_id": "2bf1c669-4710-4b76-8c99-76f918fd36fc"
  }
}
```

### Eventos de recepción

Los eventos recibidos son eventos Gateway encapsulados en una carga útil de evento, y son enviados por Discord a un cliente a través de una conexión Gateway. La mayoría de eventos recibidos corresponden a eventos dispatch que ocurren relevantes al usuario actual y los servidores de los que es miembro.

| Nombre          | Descripción                                                                          |
| --------------- | ------------------------------------------------------------------------------------ |
| Hello           | Define el intervalo de heartbeat.                                                    |
| Heartbeat ACK   | Reconoce un heartbeat de cliente recibido.                                           |
| Reconnect       | Indica que el servidor se va, el cliente debería reconectarse al Gateway y reanudar. |
| Invalid Session | Respuesta de fallo a Identify o Resume, o indica una sesión activa inválida.         |
| Dispatch        | Despacha un evento al cliente.                                                       |

**Bienvenida**

Enviado al conectarse al WebSocket. Define el intervalo de heartbeat al que el cliente debería hacer heartbeat.

**Estructura de bienvenida**

| Campo               | Tipo           | Descripción                                                                                       |
| ------------------- | -------------- | ------------------------------------------------------------------------------------------------- |
| \_trace             | array\[string] | Un array de valores JSON stringificados representando la traza de conexión, usado para debugging. |
| heartbeat\_interval | integer        | El intervalo (en milisegundos) al que el cliente debería hacer heartbeat.                         |

**Ejemplo de bienvenida**

```json
{
  "op": 10,
  "d": {
    "heartbeat_interval": 41250,
    "_trace": ["[\"gateway-prd-us-east1-c-6w69\",{\"micros\":0.0}]"]
  },
  "s": null,
  "t": null
}
```

**Heartbeat ACK**

Enviado en respuesta a recibir un heartbeat para reconocer que ha sido recibido.

Los detalles sobre heartbeats están en la documentación Gateway.

**Ejemplo de Heartbeat ACK**

```json
{
  "op": 11,
  "d": null,
  "s": null,
  "t": null
}
```

**Reconexión**

El evento reconnect se despacha cuando un cliente debería reconectarse al Gateway (y reanudar su sesión existente, si tienen una). Este evento usualmente ocurre durante despliegues para migrar sesiones grácilmente de hosts antiguos.

**Ejemplo de reconexión**

```json
{
  "op": 7,
  "d": null,
  "s": null,
  "t": null
}
```

#### Sesión inválida

Enviado para indicar una de al menos tres situaciones diferentes:

* El Gateway no pudo inicializar una sesión después de recibir un Opcode 2 Identify
* El Gateway no pudo reanudar una sesión previa después de recibir un Opcode 6 Resume
* El Gateway ha invalidado una sesión activa y está solicitando acción del cliente

La clave interna `d` es un booleano que indica si la sesión puede ser reanudable. Ver Conectarse y Reanudar para más información.

**Ejemplo de sesión inválida**

{% hint style="info" %}
Ten en cuenta que, a menos que el Gateway cierre la conexión, este evento no significa que el cliente deba iniciar una nueva conexión WebSocket. El cliente puede continuar utilizando la conexión existente al seguir el flujo de Connecting o Resuming.
{% endhint %}

```json
{
  "op": 9,
  "d": false,
  "s": null,
  "t": null
}
```

### Eventos Dispatch

Estos eventos corresponden directamente con una acción específica o cambio de estado que ha ocurrido en la plataforma.

| Nombre                                  | Descripción                                                                                      |
| --------------------------------------- | ------------------------------------------------------------------------------------------------ |
| Ready                                   | Información del estado inicial.                                                                  |
| Ready Supplemental                      | Información suplementaria para el estado inicial, no crítica para comenzar a usar la plataforma. |
| Resumed                                 | Reconoce un Resume exitoso.                                                                      |
| Remote Command                          | Recibió un mensaje de otra sesión Gateway.                                                       |
| Auth Session Change                     | El ID de sesión de autenticación asociado de la sesión actual cambió.                            |
| Authenticator Create                    | Se creó un autenticador WebAuthn.                                                                |
| Authenticator Update                    | Se actualizó un autenticador WebAuthn.                                                           |
| Authenticator Delete                    | Se eliminó un autenticador WebAuthn.                                                             |
| Application Command Permissions Update  | Se actualizó un permiso de comando de aplicación.                                                |
| Auto Moderation Rule Create             | Se creó una regla de AutoMod.                                                                    |
| Auto Moderation Rule Update             | Se actualizó una regla de AutoMod.                                                               |
| Auto Moderation Rule Delete             | Se eliminó una regla de AutoMod.                                                                 |
| Auto Moderation Action Execution        | Se disparó una regla de AutoMod y se ejecutó una acción (ej. un mensaje fue bloqueado).          |
| Auto Moderation Mention Raid Detection  | Se detectó un incidente de raid de menciones de AutoMod.                                         |
| Call Create                             | Se creó una llamada de canal privado.                                                            |
| Call Update                             | Se actualizó una llamada de canal privado.                                                       |
| Call Delete                             | Se eliminó una llamada de canal privado.                                                         |
| Channel Create                          | Se creó un nuevo canal de servidor.                                                              |
| Channel Update                          | Se actualizó un canal.                                                                           |
| Channel Delete                          | Se eliminó un canal.                                                                             |
| Channel Statuses                        | Respuesta a Request Channel Statuses.                                                            |
| Voice Channel Status Update             | Se actualizó el estado de un canal de voz.                                                       |
| Channel Member Count Update             | Respuesta a Request Channel Member Count.                                                        |
| Channel Pins Update                     | Se fijó o desfijó un mensaje.                                                                    |
| Channel Recipient Add                   | Un usuario se unió a un canal MD grupal.                                                         |
| Channel Recipient Remove                | Un usuario fue removido de un canal MD grupal.                                                   |
| Console Command Update                  | Se actualizó un comando de consola.                                                              |
| Conversation Summary Update             | Se actualizaron los resúmenes de conversación para un canal de texto.                            |
| DM Settings Upsell Show                 | Se disparó el modal de upsell de configuraciones de privacidad de MD.                            |
| Thread Create                           | Se creó un hilo, también enviado cuando se añade a un hilo privado.                              |
| Thread Update                           | Se actualizó un hilo.                                                                            |
| Thread Delete                           | Se eliminó un hilo.                                                                              |
| Thread List Sync                        | Enviado cuando se obtiene acceso a un canal, contiene todos los hilos activos en ese canal.      |
| Thread Member Update                    | Se actualizó el miembro del hilo para el usuario actual.                                         |
| Thread Members Update                   | Se añadieron o removieron usuario(s) de un hilo.                                                 |
| Entitlement Create                      | Se creó un derecho.                                                                              |
| Entitlement Update                      | Se actualizó un derecho.                                                                         |
| Entitlement Delete                      | Se eliminó un derecho.                                                                           |
| Friend Suggestion Create                | Se creó una sugerencia de amigo.                                                                 |
| Friend Suggestion Delete                | Se eliminó una sugerencia de amigo.                                                              |
| Gift Code Create                        | Se creó un código de regalo.                                                                     |
| Gift Code Update                        | Se actualizó un código de regalo.                                                                |
| Guild Create                            | El servidor se volvió disponible o el usuario se unió a un nuevo servidor.                       |
| Guild Update                            | Se actualizó el servidor.                                                                        |
| Guild Delete                            | El servidor se volvió no disponible, o el usuario salió/fue removido de un servidor.             |
| Guild Applied Boosts Update             | Se creó o actualizó una suscripción premium de servidor.                                         |
| Guild Audit Log Entry Create            | Se creó una entrada del log de auditoría del servidor.                                           |
| Guild Ban Add                           | Un usuario fue baneado de un servidor.                                                           |
| Guild Ban Remove                        | Un usuario fue desbaneado de un servidor.                                                        |
| Guild Directory Entry Create            | Se creó una entrada del directorio del servidor.                                                 |
| Guild Directory Entry Update            | Se actualizó una entrada del directorio del servidor.                                            |
| Guild Directory Entry Delete            | Se eliminó una entrada del directorio del servidor.                                              |
| Guild Emojis Update                     | Se actualizaron los emojis del servidor.                                                         |
| Guild Stickers Update                   | Se actualizaron los stickers del servidor.                                                       |
| Guild Join Request Create               | Se creó una solicitud de unión al servidor.                                                      |
| Guild Join Request Update               | Se actualizó una solicitud de unión al servidor.                                                 |
| Guild Join Request Delete               | Se eliminó una solicitud de unión al servidor.                                                   |
| Guild Member Add                        | Un usuario se unió a un servidor.                                                                |
| Guild Member Update                     | Se actualizó un miembro del servidor.                                                            |
| Guild Member Remove                     | Un usuario fue removido de un servidor.                                                          |
| Guild Members Chunk                     | Respuesta a Request Guild Members.                                                               |
| Guild Powerup Entitlements Create       | Se añadieron powerups del servidor.                                                              |
| Guild Powerup Entitlements Delete       | Se removieron powerups del servidor.                                                             |
| Guild Role Create                       | Se creó un rol del servidor.                                                                     |
| Guild Role Update                       | Se actualizó un rol del servidor.                                                                |
| Guild Role Delete                       | Se eliminó un rol del servidor.                                                                  |
| Guild Scheduled Event Create            | Se creó un evento programado del servidor.                                                       |
| Guild Scheduled Event Update            | Se actualizó un evento programado del servidor.                                                  |
| Guild Scheduled Event Delete            | Se eliminó un evento programado del servidor.                                                    |
| Guild Scheduled Event Exception Create  | Se creó una excepción de evento programado del servidor.                                         |
| Guild Scheduled Event Exception Update  | Se actualizó una excepción de evento programado del servidor.                                    |
| Guild Scheduled Event Exception Delete  | Se eliminó una excepción de evento programado del servidor.                                      |
| Guild Scheduled Event Exceptions Delete | Se eliminaron todas las excepciones de evento programado del servidor.                           |
| Guild Scheduled Event User Add          | Un usuario se suscribió a un evento programado del servidor o excepción.                         |
| Guild Scheduled Event User Remove       | Un usuario se desuscribió de un evento programado del servidor o excepción.                      |
| Guild Soundboard Sound Create           | Se creó un sonido de soundboard del servidor.                                                    |
| Guild Soundboard Sound Update           | Se actualizó un sonido de soundboard del servidor.                                               |
| Guild Soundboard Sound Delete           | Se eliminó un sonido de soundboard del servidor.                                                 |
| Soundboard Sounds                       | Respuesta a Request Soundboard Sounds.                                                           |
| Guild Integrations Update               | Se actualizó una integración del servidor.                                                       |
| Integration Create                      | Se creó una integración del servidor.                                                            |
| Integration Update                      | Se actualizó una integración del servidor.                                                       |
| Integration Delete                      | Se eliminó una integración del servidor.                                                         |
| Interaction Create                      | Un usuario usó una interacción, como un Comando de Aplicación.                                   |
| Invite Create                           | Se creó una invitación del servidor a un canal.                                                  |
| Invite Delete                           | Se eliminó una invitación del servidor a un canal.                                               |
| Message Create                          | Se creó un mensaje.                                                                              |
| Message Update                          | Se editó un mensaje.                                                                             |
| Message Delete                          | Se eliminó un mensaje.                                                                           |
| Message Delete Bulk                     | Se eliminaron múltiples mensajes a la vez.                                                       |
| Message Poll Vote Add                   | Un usuario votó en una encuesta.                                                                 |
| Message Poll Vote Remove                | Un usuario removió un voto en una encuesta.                                                      |
| Message Reaction Add                    | Un usuario reaccionó a un mensaje.                                                               |
| Message Reaction Add Many               | Muchos usuarios reaccionaron a un mensaje.                                                       |
| Message Reaction Remove                 | Un usuario removió una reacción de un mensaje.                                                   |
| Message Reaction Remove All             | Todas las reacciones fueron explícitamente removidas de un mensaje.                              |
| Message Reaction Remove Emoji           | Todas las reacciones para un emoji dado fueron explícitamente removidas de un mensaje.           |
| Recent Mention Delete                   | Se reconoció y eliminó un mensaje reciente que mencionó al usuario actual.                       |
| Last Messages                           | Respuesta a Request Last Messages.                                                               |
| Notification Settings Update            | Se actualizaron las configuraciones de notificación del usuario.                                 |
| OAuth2 Token Revoke                     | Se desautorizó una aplicación OAuth2.                                                            |
| Presence Update                         | Se actualizó la presencia del usuario.                                                           |
| Quests User Status Update               | Se actualizó el estado del usuario en una quest.                                                 |
| Quests User Completion Update           | Se actualizó la elegibilidad de completación de quest del usuario.                               |
| Relationship Add                        | Un usuario tuvo una relación añadida.                                                            |
| Relationship Update                     | Un usuario tuvo una relación actualizada.                                                        |
| Relationship Remove                     | Un usuario tuvo una relación removida.                                                           |
| Game Relationship Add                   | Un usuario tuvo una relación de juego añadida.                                                   |
| Game Relationship Remove                | Un usuario tuvo una relación de juego removida.                                                  |
| Lobby Create                            | Se creó un lobby.                                                                                |
| Lobby Update                            | Se actualizó un lobby.                                                                           |
| Lobby Delete                            | Se eliminó un lobby.                                                                             |
| Lobby Member Add                        | Se añadió un usuario a un lobby.                                                                 |
| Lobby Member Update                     | Se actualizó un miembro del lobby.                                                               |
| Lobby Member Remove                     | Se removió un usuario de un lobby.                                                               |
| Lobby Message Create                    | Se creó un mensaje en un lobby.                                                                  |
| Lobby Message Update                    | Se actualizó un mensaje en un lobby.                                                             |
| Lobby Message Delete                    | Se eliminó un mensaje en un lobby.                                                               |
| Lobby Voice State Update                | Un usuario se unió, salió, o movió lobbies.                                                      |
| Lobby Voice Server Update               | Se actualizó el servidor de conexión de voz del lobby.                                           |
| Saved Message Create                    | Se guardó un mensaje en marcadores.                                                              |
| Saved Message Delete                    | Se eliminó un mensaje guardado en marcadores.                                                    |
| Sessions Replace                        | Se actualizó la lista de sesiones del usuario.                                                   |
| Stage Instance Create                   | Se creó una instancia de escenario.                                                              |
| Stage Instance Update                   | Se actualizó una instancia de escenario.                                                         |
| Stage Instance Delete                   | Se eliminó o cerró una instancia de escenario.                                                   |
| Stream Create                           | Se creó un stream.                                                                               |
| Stream Server Update                    | Se actualizó el servidor de stream.                                                              |
| Stream Update                           | Se actualizó un stream.                                                                          |
| Stream Delete                           | Se eliminó un stream.                                                                            |
| Typing Start                            | Un usuario empezó a escribir en un canal.                                                        |
| User Update                             | El usuario actual cambió.                                                                        |
| User Application Update                 | El usuario instaló una aplicación o se actualizó la aplicación.                                  |
| User Application Remove                 | El usuario desinstaló una aplicación.                                                            |
| User Connections Update                 | Se creó, actualizó, o eliminó una conexión del usuario.                                          |
| User Guild Settings Update              | Se actualizaron las configuraciones del servidor del usuario.                                    |
| User Merge Operation Completed          | El usuario ha sido fusionado con una cuenta provisional.                                         |
| User Note Update                        | Se actualizó una nota del usuario.                                                               |
| User Required Action Update             | Se actualizó la acción requerida del usuario.                                                    |
| User Settings Update                    | Se actualizaron las configuraciones del usuario.                                                 |
| Audio Settings Update                   | Se actualizaron las configuraciones de audio.                                                    |
| Voice State Update                      | Un usuario se unió, salió, o movió un canal de voz.                                              |
| Voice Server Update                     | Se actualizó el servidor de conexión de voz.                                                     |
| Voice Channel Effect Send               | Un usuario envió un efecto en un canal de voz al que el usuario actual está conectado.           |
| Webhooks Update                         | Se creó, actualizó, o eliminó un webhook del canal.                                              |

**Preparado**

Enviado cuando un cliente ha completado el handshake inicial con el Gateway (para nuevas sesiones). El evento Ready es el evento más grande y complejo que el Gateway enviará, ya que contiene todo el estado requerido para que un cliente comience a interactuar con el resto de la plataforma.

**Estructura de preparación**

<sup>1</sup> La característica no está disponible para o no se rastrea para bots. Este campo puede estar vacío, omitido, o `null`.

<sup>2</sup> Para bots, los servidores comienzan como no disponibles cuando se conectan al Gateway. Conforme se vuelven disponibles, el bot será notificado vía eventos Guild Create.

<sup>3</sup> Requiere la capacidad Gateway `AUTH_TOKEN_REFRESH`.

<sup>4</sup> Omitido cuando se usa la capacidad Gateway `USER_SETTINGS_PROTO`.

<sup>5</sup> Omitido cuando se usa la capacidad Gateway `LAZY_USER_NOTES`.

<sup>6</sup> Cuando se usa la capacidad Gateway `DEDUPE_USER_OBJECTS`, `presences`, así como el array `presences` de cada servidor, es reemplazado por `merged_presences`. Además, el array `members` de cada servidor será colapsado en `merged_members`. Finalmente, el array `users` contendrá los objetos de usuario para cada usuario en el evento. Cualquier objeto de usuario en el evento será omitido, con un ID dejado en su lugar (ej. `user_id` en objetos de miembro, `recipient_ids` en objetos de canal privado, etc.).

<sup>7</sup> Cuando se usa la capacidad Gateway `PRIORITIZED_READY_PAYLOAD`, `merged_members` en Ready solo incluirá el objeto de miembro del cliente para cada servidor. El resto será enviado en el evento Ready Supplemental. Ver la documentación del objeto gateway guild para más información sobre datos incluidos.

<sup>8</sup> El estado del tutorial se limpia después de un período de inactividad. Un tutorial `null` significa que no se mostrarán indicadores.

<sup>9</sup> El campo será un array versionado si la capacidad Gateway `VERSIONED_USER_GUILD_SETTINGS` está habilitada. De otro modo, será un array regular.

<sup>10</sup> Solo disponible en contextos OAuth2.

<sup>11</sup> Requiere la capacidad Gateway `AUTO_LOBBY_CONNECT`.

| Campo                                                 | Tipo                                         | Descripción                                                                                                                                                                                        |
| ----------------------------------------------------- | -------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \_trace                                               | array\[string]                               | Un array de valores JSON stringificados que representan la traza de conexión, usado para debugging.                                                                                                |
| v                                                     | integer                                      | Versión de la API.                                                                                                                                                                                 |
| user                                                  | user object                                  | El usuario conectado.                                                                                                                                                                              |
| user\_settings <sup>1</sup>  <sup>4</sup>  (obsoleto) | user settings object                         | Las configuraciones del cliente para el usuario.                                                                                                                                                   |
| user\_settings\_proto? <sup>1</sup>                   | string                                       | Las configuraciones de usuario preloaded protobuf serializadas codificadas en base 64 para el usuario, (si faltan, se deben usar los valores por defecto).                                         |
| notification\_settings <sup>1</sup>                   | notification settings object                 | Las configuraciones de notificación para el usuario.                                                                                                                                               |
| user\_guild\_settings <sup>1</sup>  <sup>9</sup>      | versioned array\[user guild settings object] | Las configuraciones de usuario para cada servidor.                                                                                                                                                 |
| guilds <sup>2</sup>  <sup>6</sup>                     | array\[gateway guild object]                 | Los servidores en los que está el usuario.                                                                                                                                                         |
| guild\_join\_requests <sup>1</sup>                    | array\[partial guild join request]           | Solicitudes de unión a servidor activas que tiene el usuario.                                                                                                                                      |
| relationships <sup>1</sup>                            | array\[relationship]                         | Las relaciones que el usuario tiene con otros usuarios.                                                                                                                                            |
| game\_relationships <sup>1</sup>                      | array\[game relationship]                    | Las relaciones de juego que el usuario tiene con otros usuarios.                                                                                                                                   |
| friend\_suggestion\_count? <sup>1</sup>               | integer                                      | El número de sugerencias de amigos que tiene el usuario.                                                                                                                                           |
| private\_channels <sup>1</sup>                        | array\[channel object]                       | Los MDs y MDs grupales en los que participa el usuario.                                                                                                                                            |
| connected\_accounts                                   | array\[connection]                           | Las cuentas de terceros que el usuario ha vinculado.                                                                                                                                               |
| notes <sup>1</sup>  <sup>5</sup>                      | map\[snowflake, string]                      | Un mapeo de IDs de usuario a notas que el usuario ha hecho para ellos.                                                                                                                             |
| presences <sup>6</sup>                                | array\[presence object]                      | Las presencias de los amigos no-desconectados del usuario y relaciones implícitas (dependiendo de la capacidad Gateway `NO_AFFINE_USER_IDS`).                                                      |
| merged\_presences <sup>6</sup>                        | merged presences object                      | Las presencias de los amigos no-desconectados del usuario y relaciones implícitas (dependiendo de la capacidad Gateway `NO_AFFINE_USER_IDS`), y cualquier presencia de servidor enviada al inicio. |
| merged\_members <sup>6</sup>  <sup>7</sup>            | array\[array\[guild member object]]          | Los miembros de los servidores del usuario, en el mismo orden que el array `guilds`.                                                                                                               |
| users <sup>6</sup>                                    | array\[partial user object]                  | Los usuarios desduplicados a través de todos los objetos en el evento.                                                                                                                             |
| application?                                          | gateway application object                   | La aplicación del usuario conectado o aplicación OAuth2.                                                                                                                                           |
| scopes? <sup>10</sup>                                 | array\[string]                               | Los scopes OAuth2 que el usuario ha autorizado para la aplicación.                                                                                                                                 |
| session\_id                                           | string                                       | ID de sesión único, usado para reanudar conexiones.                                                                                                                                                |
| session\_type                                         | string                                       | El tipo de sesión que fue iniciada.                                                                                                                                                                |
| sessions <sup>1</sup>                                 | array\[session object]                       | Las sesiones que están actualmente activas para el usuario.                                                                                                                                        |
| static\_client\_session\_id                           | string                                       | Un identificador único para la sesión del cliente, usado para claves públicas DAVE persistentes.                                                                                                   |
| auth\_session\_id\_hash <sup>1</sup>                  | string                                       | El hash del ID de sesión de autenticación correspondiente al token de autenticación usado para conectarse.                                                                                         |
| auth\_token? <sup>1</sup>  <sup>3</sup>               | string                                       | El token de autenticación refrescado para este usuario; si está presente, el cliente debe descartar el token de autenticación actual y usar este en solicitudes subsecuentes a la API.             |
| analytics\_token <sup>1</sup>                         | string                                       | El token usado para solicitudes de seguimiento analítico.                                                                                                                                          |
| authenticator\_types                                  | array\[integer]                              | Los tipos de autenticadores multi-factor que el usuario tiene habilitados.                                                                                                                         |
| required\_action? <sup>1</sup>                        | string                                       | La acción que un usuario debe realizar antes de continuar usando Discord.                                                                                                                          |
| country\_code <sup>1</sup>                            | string                                       | El código de país ISO 3166-1 alpha-2 detectado de la dirección IP actual del usuario.                                                                                                              |
| geo\_ordered\_rtc\_regions                            | array\[string]                               | Una lista geo-ordenada de regiones RTC que pueden usarse al establecer el `rtc_region` de un canal de voz o actualizar el estado de voz del cliente.                                               |
| consents <sup>1</sup>                                 | consents                                     | Las características de seguimiento a las que el usuario ha consentido.                                                                                                                             |
| tutorial <sup>1</sup>  <sup>8</sup>                   | ?tutorial object                             | El estado del tutorial del usuario, si hay alguno.                                                                                                                                                 |
| shard?                                                | array\[integer, integer]                     | La información de shard (shard\_id, num\_shards) asociada con esta sesión, si está sharded.                                                                                                        |
| resume\_gateway\_url                                  | string                                       | URL WebSocket para reanudar conexiones.                                                                                                                                                            |
| api\_code\_version <sup>1</sup>                       | integer                                      | La versión del código de la API, usada al re-identificarse con estado de cliente v2.                                                                                                               |
| experiments <sup>1</sup>                              | array\[user experiment object]               | Rollouts de experimento de usuario para el usuario.                                                                                                                                                |
| guild\_experiments <sup>1</sup>                       | array\[guild experiment object]              | Rollouts de experimento de servidor para el usuario.                                                                                                                                               |
| explicit\_content\_scan\_version                      | integer                                      | La última versión de la característica del filtro de escaneo de contenido explícito.                                                                                                               |
| av\_sf\_protocol\_floor? <sup>10</sup>                | integer                                      | La versión mínima soportada del protocolo DAVE en conexión de voz elegible.                                                                                                                        |
| feature\_flags? <sup>10</sup>                         | gateway feature flags object                 | Flags de características del SDK de capa social.                                                                                                                                                   |
| lobbies? <sup>10</sup>  <sup>11</sup>                 | array\[lobby object]                         | Los lobbies en los que está el usuario conectado.                                                                                                                                                  |

**Estructura versionada**

Un objeto genérico usado para representar datos versionados. Depende de capacidades específicas para habilitar versionado para ciertos campos en el evento preparación.

| Campo   | Tipo           | Descripción                       |
| ------- | -------------- | --------------------------------- |
| entries | array\[object] | Las entradas.                     |
| partial | boolean        | Si el campo `entries` es parcial. |
| version | integer        | La versión del objeto.            |

**Estructura de presencias fusionadas**

<sup>1</sup> Ver la documentación del objeto gateway guild para más información sobre datos incluidos.

| Campo                | Tipo                            | Descripción                                                                                                              |
| -------------------- | ------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| friends              | array\[presence object]         | Presencias de los amigos del usuario y relaciones implícitas (dependiendo de la capacidad Gateway `NO_AFFINE_USER_IDS`). |
| guilds <sup>1</sup>  | array\[array\[presence object]] | Presencias de los servidores del usuario, en el mismo orden que el array `guilds` en Ready.                              |

**Estructura de tutorial**

| Campo                  | Tipo           | Descripción                                                           |
| ---------------------- | -------------- | --------------------------------------------------------------------- |
| indicators\_suppressed | boolean        | Si el usuario ha suprimido todos los indicadores de tutorial.         |
| indicators\_confirmed  | array\[string] | Un array de los indicadores de tutorial que el usuario ha confirmado. |

**Estructura de aplicación Gateway**

<sup>1</sup> Solo disponible en contextos OAuth2.

| Campo   | Tipo      | Descripción                 |
| ------- | --------- | --------------------------- |
| id      | snowflake | El ID de la aplicación.     |
| flags   | integer   | Las flags de la aplicación. |
| name? 1 | string    | El nombre de la aplicación. |

**Estructura de bandera de características de Gateway**

| Campo                     | Tipo           | Descripción                                                                            |
| ------------------------- | -------------- | -------------------------------------------------------------------------------------- |
| disabled\_functions       | array\[string] | Funciones que están actualmente deshabilitadas en el SDK debido a inestabilidad.       |
| disabled\_gateway\_events | array\[string] | Eventos Gateway que están actualmente deshabilitados en el SDK debido a inestabilidad. |

**Tipo de sesión**

| Valor  | Descripción                |
| ------ | -------------------------- |
| normal | Una sesión Gateway normal. |
| oauth  | Una sesión Gateway OAuth2. |

#### Comlemento de preparación

Enviado poco después de Ready, con datos adicionales que no son críticos para comenzar a interactuar con la plataforma.

Si este evento está habilitado, cualquier campo recibido en él que también esté presente en Ready no será recibido en Ready (a menos que se especifique lo contrario).

{% hint style="info" %}
Este evento requiere la capacidad del Gateway **PRIORITIZED\_READY\_PAYLOAD**.
{% endhint %}

**Estructura de Ready Supplemental**

¹ Ver la documentación del objeto gateway guild para más información sobre los datos incluidos.

| Campo                   | Tipo                              | Descripción                                                                                                                                                                              |
| ----------------------- | --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| guilds                  | array\[supplemental guild object] | Los guilds en los que está el usuario                                                                                                                                                    |
| merged\_members¹        | array\[array\[member object]]     | Los miembros de los guilds del usuario, en el mismo orden que el array `guilds`                                                                                                          |
| merged\_presences¹      | merged presences object           | Las presencias de los amigos no offline del usuario y relaciones implícitas (dependiendo de la capacidad Gateway `NO_AFFINE_USER_IDS`), y cualquier presencia de guild enviada al inicio |
| lazy\_private\_channels | array\[channel object]            | DMs y group DMs adicionales en los que participa el usuario, omitidos de Ready porque ya estaban en el caché del estado del cliente                                                      |
| disclose                | array\[string]                    | Próximos cambios que el cliente debe revelar al usuario                                                                                                                                  |

**Estructura del Guild Suplementario**

{% hint style="warning" %}
Las guilds no disponibles solo incluirán el campo `id`.
{% endhint %}

| Campo         | Tipo                       | Descripción                                       |
| ------------- | -------------------------- | ------------------------------------------------- |
| id            | snowflake                  | El ID del guild                                   |
| voice\_states | array\[voice state object] | Estados de miembros actualmente en canales de voz |

#### Resumed

Enviado cuando un cliente ha enviado un payload de resume al Gateway (para reanudar sesiones existentes). Significa el final de la reproducción de eventos.

**Estructura de Resumed**

| Campo   | Tipo           | Descripción                                                                                          |
| ------- | -------------- | ---------------------------------------------------------------------------------------------------- |
| \_trace | array\[string] | Un array de valores JSON stringificados que representan el rastro de conexión, usado para depuración |

**Ejemplo de Resumed:**

```json
{
  "_trace": [
    "[\"gateway-prd-us-east1-c-6w69\",{\"micros\":4493,\"calls\":[\"id_created\",{\"micros\":0,\"calls\":[]},\"session_lookup_time\",{\"micros\":4163,\"calls\":[]},\"session_lookup_finished\",{\"micros\":17,\"calls\":[]},\"discord-sessions-prd-2-31\",{\"micros\":66}]}]"
  ]
}
```

#### Comando remoto (Recepción)

Enviado cuando se recibe un mensaje de otra sesión del Gateway. El payload interno es el mensaje enviado.

#### Autenticación

#### Cambio de sesión de autenticación

Enviado cuando el ID de sesión de auth asociado a la sesión actual cambia.

**Estructura de Auth Session Change**

| Campo                   | Tipo   | Descripción                                                                           |
| ----------------------- | ------ | ------------------------------------------------------------------------------------- |
| auth\_session\_id\_hash | string | El hash del ID de sesión de auth correspondiente al token de auth usado para conectar |

#### Crear autenticación

Enviado cuando se crea un autenticador WebAuthn. El payload interno es un objeto authenticator.

#### Actualizar autenticación

Enviado cuando se actualiza un autenticador WebAuthn. El payload interno es un objeto authenticator.

#### Eliminar autenticación

Enviado cuando se elimina un autenticador WebAuthn.

**Estructura de eliminación de autenticación**

| Campo | Tipo   | Descripción             |
| ----- | ------ | ----------------------- |
| id    | string | El ID del autenticador  |
| type  | string | El tipo de autenticador |

#### Comandos de aplicación

#### Actualización de permisos de comando de aplicación

Enviado cuando se actualizan los permisos de un comando de aplicación. El payload interno es un objeto application command permissions.

#### Auto-Moderación

#### Crear regla de Auto-Moderación

Enviado cuando se crea una regla. El payload interno es un objeto automod rule. Requiere el permiso `MANAGE_GUILD`.

{% hint style="warning" %}
Este evento no es recibido por cuentas de usuario.
{% endhint %}

#### Actualizar regla de Auto-Moderación

Enviado cuando se actualiza una regla. El payload interno es un objeto automod rule. Requiere el permiso `MANAGE_GUILD`.

{% hint style="warning" %}
Este evento no es recibido por cuentas de usuario.
{% endhint %}

#### Eliminar regla de Auto-Moderación

Enviado cuando se elimina una regla. El payload interno es un objeto automod rule. Requiere el permiso `MANAGE_GUILD`.

{% hint style="warning" %}
Este evento no es recibido por cuentas de usuario.
{% endhint %}

#### Ejecución de regla de Auto-Moderación

Enviado cuando se dispara una regla y se ejecuta una acción (ej. un mensaje es bloqueado). Requiere el permiso `MANAGE_GUILD`.

{% hint style="warning" %}
Este evento no es recibido por cuentas de usuario.
{% endhint %}

**Estructura de ejecución de regla de Auto-Moderación**

| Campo                       | Tipo                  | Descripción                                                                      |
| --------------------------- | --------------------- | -------------------------------------------------------------------------------- |
| guild\_id                   | snowflake             | El ID del guild donde se ejecutó la acción                                       |
| action                      | automod action object | La acción que se ejecutó                                                         |
| rule\_id                    | snowflake             | El ID de la regla que se disparó                                                 |
| rule\_trigger\_type         | integer               | El tipo de disparador de la regla que se disparó                                 |
| user\_id                    | snowflake             | El ID del usuario que generó el contenido que disparó la regla                   |
| channel\_id?                | snowflake             | El ID del canal donde se publicó el contenido del usuario                        |
| message\_id?                | snowflake             | El ID del mensaje que disparó la regla                                           |
| alert\_system\_message\_id? | snowflake             | El ID del mensaje del sistema de AutoMod publicado como resultado de esta acción |
| content                     | string                | El contenido del mensaje del usuario                                             |
| matched\_keyword            | ?string               | La palabra o frase configurada que disparó la regla                              |
| matched\_content            | ?string               | La subcadena en el contenido que disparó la regla                                |

#### Detección de raid de menciones de Auto-Moderación

Enviado cuando se detecta un raid de menciones. Requiere el permiso `MANAGE_GUILD`.

**Estructura de detección de raid de menciones de Auto-Moderación**

| Campo                                | Tipo              | Descripción                                                   |
| ------------------------------------ | ----------------- | ------------------------------------------------------------- |
| guild\_id                            | snowflake         | El ID del guild donde se detectó el raid de menciones         |
| decision\_id                         | string            | El ID de la decisión que se ejecutó                           |
| suspicious\_mention\_activity\_until | ISO8601 timestamp | Cuándo terminarán las restricciones de actividad de menciones |

#### Llamadas

#### Crear llamada

Enviado cuando un usuario crea una llamada en un canal privado, o para informar al cliente de una llamada existente después de una Request Call Connect.

**Estructura de creación de llamada**

| Campo         | Tipo                       | Descripción                                                                |
| ------------- | -------------------------- | -------------------------------------------------------------------------- |
| channel\_id   | snowflake                  | El ID del canal privado donde ocurre esta llamada                          |
| message\_id   | snowflake                  | El ID del mensaje asociado con la llamada                                  |
| region        | string                     | El ID de región de voz desde donde se hospeda la llamada                   |
| ringing       | array\[snowflake]          | Los IDs de los usuarios que están siendo llamados para unirse a la llamada |
| voice\_states | array\[voice state object] | Los estados de voz de los usuarios ya en la llamada                        |

#### Actualizar llamada

Enviado cuando cambian los metadatos de una llamada.

**Estructura de actualización de llamada**

| Campo       | Tipo      | Descripción                                                                |
| ----------- | --------- | -------------------------------------------------------------------------- |
| channel\_id | snowflake | El ID del canal privado donde ocurre esta llamada                          |
| message\_id | snowflake | El ID del mensaje asociado con la llamada                                  |
| region      | string    | El ID de región de voz desde donde se hospeda la llamada                   |
| ringing     | array     | Los IDs de los usuarios que están siendo llamados para unirse a la llamada |

#### Eliminación de llamada

Enviado cuando una llamada se elimina, o se vuelve no disponible debido a una interrupción.

**Estructura de eliminación de llamada**

| Campo        | Tipo      | Descripción                                                |
| ------------ | --------- | ---------------------------------------------------------- |
| channel\_id  | snowflake | El ID del canal privado donde ocurre esta llamada          |
| unavailable? | boolean   | Si la llamada no está disponible debido a una interrupción |

#### Canales

#### Crear canal

Enviado cuando se crea un nuevo canal de guild, relevante para el usuario actual. El payload interno es un objeto channel.

**Campos extra de la estructura de creación de canal**

| Campo                | Tipo      | Descripción                                                       |
| -------------------- | --------- | ----------------------------------------------------------------- |
| origin\_channel\_id? | snowflake | El ID del canal privado desde el cual se creó este canal de grupo |

#### Actualizar canal

Enviado cuando se actualiza un canal. El payload interno es un objeto channel. Esto no se envía cuando se altera el campo `last_message_id` o `status`. Para hacer seguimiento de los cambios de `last_message_id`, debes escuchar eventos Message Create (o Thread Create para canales solo de hilos). Para hacer seguimiento de los cambios de `status`, debes escuchar eventos Voice Channel Status Update.

Este evento puede referenciar roles o miembros de guild que ya no existen en un guild.

#### Channel Delete

Enviado cuando se elimina un canal relevante para el usuario actual. El payload interno es un objeto channel.

{% hint style="warning" %}
Para canales privados, el objeto de canal recibido será parcial.
{% endhint %}

#### Estados de canal

Enviado en respuesta a Request Channel Statuses. Contiene los estados para todos los canales de voz que tienen uno establecido.

**Estructura de estados de canal**

| Campo     | Tipo                          | Descripción                    |
| --------- | ----------------------------- | ------------------------------ |
| guild\_id | snowflake                     | El ID del guild                |
| channels  | array\[channel status object] | Los canales de voz con estados |

**Estructura de estados de canal**

| Campo  | Tipo      | Descripción                                     |
| ------ | --------- | ----------------------------------------------- |
| id     | snowflake | El ID del canal                                 |
| status | string    | El estado del canal de voz (máx 500 caracteres) |

#### Actualizacion de estados de canal

Enviado cuando se actualiza el estado de un canal de voz.

**Estructura de actualización de estados de canal**

| Campo     | Tipo      | Descripción                                     |
| --------- | --------- | ----------------------------------------------- |
| id        | snowflake | El ID del canal de voz                          |
| guild\_id | snowflake | El ID del guild                                 |
| status    | ?string   | El estado del canal de voz (máx 500 caracteres) |

#### Actualización de miembros de canal

Enviado en respuesta a Request Channel Member Count. Contiene el número de miembros que actualmente pueden ver el canal.

**Estructura de actualización de miembros de canal**

| Campo           | Tipo      | Descripción                                                                  |
| --------------- | --------- | ---------------------------------------------------------------------------- |
| guild\_id       | snowflake | El ID del guild                                                              |
| channel\_id     | snowflake | El ID del canal                                                              |
| member\_count   | integer   | El número de miembros que actualmente pueden ver el canal                    |
| presence\_count | integer   | El número de miembros que actualmente pueden ver el canal y no están offline |

#### Actualización de fijados de canal

Enviado cuando un mensaje se fija o se desfija en un canal de texto. Esto no se envía cuando se elimina un mensaje fijado.

**Estructura de actualización de fijados de canal**

| Campo                 | Tipo               | Descripción                                   |
| --------------------- | ------------------ | --------------------------------------------- |
| guild\_id?            | snowflake          | El ID del guild                               |
| channel\_id           | snowflake          | El ID del canal                               |
| last\_pin\_timestamp? | ?ISO8601 timestamp | Cuándo se fijó el mensaje fijado más reciente |

#### Añadir recipiente al canal

Enviado cuando se agrega un usuario a un canal de mensaje directo grupal.

**Estructura de añadido de recipiente al canal**

| Campo       | Tipo                | Descripción                      |
| ----------- | ------------------- | -------------------------------- |
| channel\_id | snowflake           | El ID del canal                  |
| user        | partial user object | El usuario que fue agregado      |
| nick?       | string              | El apodo del usuario en el canal |

#### Eliminar recipiente de canal

Enviado cuando se remueve un usuario de un canal de mensaje directo grupal.

**Estructura de eliminación de recipiente de canal**

| Campo       | Tipo                | Descripción                 |
| ----------- | ------------------- | --------------------------- |
| channel\_id | snowflake           | El ID del canal             |
| user        | partial user object | El usuario que fue removido |

#### Consolas

#### Actuar comandos de consola

Enviado cuando se actualiza un comando de consola.

**Estructura de actualización de comandos de consola**

¹ Si el comando falló, `result` se establecerá como `failed` o `n/a`.&#x20;

² El objeto contendrá solo una clave `code`.

| Campo   | Tipo                                | Descripción              |
| ------- | ----------------------------------- | ------------------------ |
| id      | snowflake                           | El ID del comando        |
| result¹ | string                              | El resultado del comando |
| error²  | ?partial JSON error response object | El error                 |

#### Actualizar resumen de conversación

Enviado cuando se actualizan los resúmenes de conversación para un canal de texto. Solo se envían resúmenes nuevos o actualizados.

**Estructura de actualización de resumen de conversación**

| Campo       | Tipo                                | Descripción                                              |
| ----------- | ----------------------------------- | -------------------------------------------------------- |
| guild\_id   | snowflake                           | El ID del guild                                          |
| channel\_id | snowflake                           | El ID del canal de texto                                 |
| summaries   | array\[conversation summary object] | Los resúmenes de conversación actualizados para el canal |

#### Mostrar promoción de configuración de mensajes directos

Puede enviarse cuando un usuario rechaza una solicitud de mensaje.

**Estructura de promoción de configuración de mensajes directos**

| Campo     | Tipo      | Descripción     |
| --------- | --------- | --------------- |
| guild\_id | snowflake | El ID del guild |

#### Hilo creado

Enviado cuando se crea un hilo, relevante para el usuario actual, o cuando el usuario actual se agrega a un hilo. El payload interno es un objeto channel.

* Cuando se crea un hilo, incluye un campo booleano adicional `newly_created`.
* Cuando se agrega a un hilo privado existente, incluye el campo opcional `member`.

**Campos extra de la estructura de creación de hilos**

| Campo           | Tipo    | Descripción                    |
| --------------- | ------- | ------------------------------ |
| newly\_created? | boolean | Si el hilo acaba de ser creado |

#### Actualización de hilo

Enviado cuando se actualiza un hilo. El payload interno es un objeto channel. Esto no se envía cuando se altera el campo `last_message_id`. Para hacer seguimiento de los cambios de `last_message_id`, debes escuchar eventos Message Create.

#### Eliminación de hilo

Enviado cuando se elimina un hilo relevante para el usuario actual. El payload interno es un subconjunto del objeto channel, conteniendo solo los campos `id`, `guild_id`, `parent_id` y `type`.

#### Sincronizar lista de hilos

Enviado para sincronizar la lista de hilos activos de un guild, o para sincronizar listas de canales específicos cuando el usuario actual _gana_ acceso a un canal dentro de un guild.

Para bots, todos los hilos activos se sincronizan al inicio. Para cuentas de usuario, solo se sincronizan los hilos unidos, y la lista completa se envía a través de este evento en la suscripción.

**Estructura de sincronización de lista de hilos**

| Campo         | Tipo                         | Descripción                                                                                                                                                                                                                    |
| ------------- | ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| guild\_id     | snowflake                    | El ID del guild                                                                                                                                                                                                                |
| channel\_ids? | array\[snowflake]            | Los IDs de canales padre cuyos hilos se están sincronizando (puede contener IDs de canales que no tienen hilos activos para que sepas limpiar esos datos); si se omite, entonces los hilos se sincronizaron para todo el guild |
| threads       | array\[channel object]       | Todos los hilos activos en los canales dados a los que el usuario actual puede acceder                                                                                                                                         |
| members       | array\[thread member object] | Todos los objetos thread member de los hilos sincronizados para el usuario actual, indicando a qué hilos se ha agregado el usuario actual                                                                                      |

#### Actualizar miembro de hilo

Enviado cuando se actualiza el objeto thread member para el usuario actual. El payload interno es un objeto thread member con un campo extra `guild_id`. Para bots, este evento es en gran medida solo una señal de que eres miembro del hilo. Ver la documentación de hilos para más detalles.

**Campos extra de la estructura de actualización de miembro de hilo**

| Campo     | Tipo      | Descripción     |
| --------- | --------- | --------------- |
| guild\_id | snowflake | El ID del guild |

#### Actualizar miembros de hilo

Enviado cuando alguien se agrega o remueve de un hilo. Si el usuario actual no tiene el intent Gateway `GUILD_MEMBERS`, entonces este evento solo se enviará si el usuario actual fue agregado o removido del hilo.

**Estructura de actualización de miembros de hilo**

¹ También incluye campos `member` y `presence` nullable.

| Campo                 | Tipo                         | Descripción                                                |
| --------------------- | ---------------------------- | ---------------------------------------------------------- |
| id                    | snowflake                    | El ID del hilo                                             |
| guild\_id             | snowflake                    | El ID del guild                                            |
| member\_count         | integer                      | El número aproximado de miembros en el hilo, limitado a 50 |
| added\_members?¹      | array\[thread member object] | Los usuarios que fueron agregados al hilo                  |
| removed\_member\_ids? | array of snowflakes          | Los IDs de los usuarios que fueron removidos del hilo      |

#### Derechos

#### Creación de derechos

Enviado cuando se crea un entitlement. El payload interno es un objeto entitlement.

#### Actualización de derechos

Enviado cuando se actualiza un entitlement. El payload interno es un objeto entitlement.

Para entitlements de suscripción, este evento se dispara solo cuando termina la suscripción de un usuario, proporcionando un timestamp `ends_at` que indica el final del entitlement.

#### Eliminación de derechos

Enviado cuando se elimina un entitlement. El payload interno es un objeto entitlement.

Las eliminaciones de entitlements son infrecuentes, y ocurren cuando:

* Discord emite un reembolso para una suscripción
* Discord remueve un entitlement de un usuario vía herramientas internas
* Discord elimina un entitlement gestionado por la app que crearon
* Se elimina un entitlement de prueba

Los entitlements _no_ se eliminan cuando expiran.

#### Sugerencias de amigos

#### Crear sugerencia de amigos

Enviado cuando se crea una sugerencia de amigo. El payload interno es un objeto friend suggestion.

#### Eliminar sugerencia de amigos

Enviado cuando se elimina una sugerencia de amigo.

**Estructura de eliminación de sugerencias de amigos**

| Campo               | Tipo      | Descripción                |
| ------------------- | --------- | -------------------------- |
| suggested\_user\_id | snowflake | El ID del usuario sugerido |

#### Códigos de regalo

#### Objeto de código de regalo de Gateway

Un objeto gift code recibido sobre el Gateway tiene atributos diferentes a la API REST.

**Estructura de código de regalo de Gateway**

| Campo        | Tipo      | Descripción                                            |
| ------------ | --------- | ------------------------------------------------------ |
| code         | string    | El código de regalo                                    |
| sku\_id      | snowflake | El ID del SKU que otorga el código de regalo           |
| uses         | integer   | El número de veces que se ha usado el código de regalo |
| channel\_id? | snowflake | El ID del canal donde se envió el código de regalo     |
| guild\_id?   | snowflake | El ID del guild donde se envió el código de regalo     |

#### Crear código de regalo

Enviado cuando el usuario crea un código de regalo. El payload interno es un objeto gateway gift code.

#### Actualizar código de regalo

Enviado cuando se actualiza un código de regalo, ya sea por el usuario o en un canal que el usuario puede ver. El payload interno es un objeto gateway gift code.

#### Servidores

#### Objeto de Gateway de servidor

Los objetos Guild recibidos sobre el Gateway tienen atributos extendidos que no se proporcionan en la API REST.

**Estructura de Gateway de servidor**

¹ Para bots sin el intent Gateway `GUILD_PRESENCES`, cuentas de usuario, o guilds con más de 75k miembros, esto solo incluirá el miembro del cliente y usuarios en canales de voz. Las cuentas de usuario adicionalmente reciben amigos y relaciones implícitas (dependiendo de la capacidad `NO_AFFINE_USER_IDS` Gateway), así como usuarios con los que tienen un DM abierto.

² Las cuentas de usuario solo reciben presencias para amigos no offline y relaciones implícitas (dependiendo de la capacidad `NO_AFFINE_USER_IDS` Gateway), así como usuarios con los que tienen un DM abierto. Los bots con el intent Gateway `GUILD_PRESENCES` reciben todas las presencias.

³ Las cuentas de usuario solo sincronizan hilos a los que han sido agregados. Los bots sincronizan todos los hilos.

⁴ Omitido cuando se usa la capacidad `PRIORITIZED_READY_PAYLOAD` Gateway.

⁵ Requiere la capacidad `CLIENT_STATE_V2` Gateway. Sin la capacidad, el campo `properties` se fusionará en el objeto principal, junto con el resto de los atributos extendidos.

⁶ Los guilds geo-restringidos también se marcarán como no disponibles y no serán accesibles o unibles.

| Campo                        | Tipo                                 | Descripción                                                                                      |
| ---------------------------- | ------------------------------------ | ------------------------------------------------------------------------------------------------ |
| joined\_at                   | ISO8601 timestamp                    | Cuándo se unió a este guild                                                                      |
| large                        | boolean                              | Si esto se considera un guild grande                                                             |
| unavailable?                 | boolean                              | Si el guild no está disponible debido a una interrupción                                         |
| geo\_restricted?⁶            | boolean                              | Si el guild no está disponible en tu región actual                                               |
| member\_count                | integer                              | Número total de miembros en este guild                                                           |
| voice\_states⁴               | array\[voice state object]           | Estados de miembros actualmente en canales de voz                                                |
| members¹                     | array\[guild member object]          | Miembros en el guild                                                                             |
| channels                     | array\[channel object]               | Canales en el guild                                                                              |
| threads³                     | array\[channel object]               | Todos los hilos activos en el guild que el usuario actual tiene permiso para ver                 |
| presences²                   | array\[presence object]              | Presencias de miembros de guild incluidos; solo incluirá miembros no offline para guilds grandes |
| stage\_instances             | array\[stage instance object]        | Instancias de escenario en el guild                                                              |
| guild\_scheduled\_events     | array\[guild scheduled event object] | Eventos programados en el guild                                                                  |
| data\_mode⁵                  | string                               | El modo de datos para este objeto                                                                |
| properties⁵                  | partial guild object                 | Las propiedades del guild; un objeto guild normal que carece de los campos de abajo              |
| stickers                     | array\[sticker object]               | Stickers personalizados del guild                                                                |
| roles                        | array\[role object]                  | Roles en el guild                                                                                |
| emojis                       | array\[emoji object]                 | Emojis personalizados del guild                                                                  |
| premium\_subscription\_count | integer                              | El número de suscripciones premium (boosts) que el guild tiene actualmente                       |

#### Objeto de servidor no disponible

Un objeto guild parcial. Representa un guild offline, o un guild al que el cliente aún no está conectado.

**Estructura de servidor no disponible**

¹ Solo incluido cuando `geo_restricted` es `true`.

| Campo            | Tipo      | Descripción                                              |
| ---------------- | --------- | -------------------------------------------------------- |
| id               | snowflake | El ID del guild                                          |
| unavailable?     | boolean   | Si el guild no está disponible debido a una interrupción |
| geo\_restricted? | boolean   | Si el guild no está disponible en tu región actual       |
| name?¹           | string    | El nombre del guild (2-100 caracteres)                   |
| icon?¹           | ?string   | El hash del icono del guild                              |

**Ejemplo de servidor no disponible:**

```json
{
  "id": "41771983423143937",
  "unavailable": true
}
```

**Modo de datos**

| Valor       | Descripción                                                         |
| ----------- | ------------------------------------------------------------------- |
| full        | Se envía el objeto guild completo                                   |
| partial     | Se omiten los datos del guild ya en el caché del estado del cliente |
| unavailable | El guild no está disponible debido a una interrupción               |

#### Crear servidor

Este evento puede enviarse en tres escenarios diferentes:

1. Cuando un bot se está conectando inicialmente, para cargar perezosamente y rellenar información para todos los guilds no disponibles enviados en el evento Ready. Los guilds que no están disponibles debido a una interrupción o geo-restringidos enviarán un evento Guild Delete.
2. Cuando un Guild se vuelve disponible de nuevo para el cliente.
3. Cuando el usuario actual se une a un nuevo Guild.

{% hint style="info" %}
Durante una interrupción, el objeto guild en los escenarios 1 y 3 puede marcarse como no disponible.
{% endhint %}

El payload interno puede ser:

* Un guild disponible: un objeto Gateway guild.
* Un guild no disponible: un objeto unavailable guild.

#### Actualizar servidor

Enviado cuando se actualiza un guild. El payload interno es un objeto guild.

#### Eliminar servidor

Enviado cuando un guild se vuelve o ya estaba no disponible debido a una interrupción, o cuando el usuario sale o es removido de un guild. El payload interno es un objeto unavailable guild. Si el campo `unavailable` no está establecido, el usuario fue removido del guild.

#### Actualizar mejoras aplicadas

Enviado cuando se actualiza una suscripción premium de guild, significando que un usuario aplicó un boost o se actualizó un boost existente. El payload interno es un objeto premium guild subscription.

#### Crear entrada de registro de auditoría de servidor

Enviado cuando se crea una entrada del registro de auditoría del guild. El payload interno es un objeto Audit Log Entry. Requiere el permiso `VIEW_AUDIT_LOG`.

#### Añadir baneo de servidor

Enviado cuando un usuario es baneado de un guild.

**Estructura de añadido de baneo de servidor**

| Campo                 | Tipo                | Descripción                                           |
| --------------------- | ------------------- | ----------------------------------------------------- |
| guild\_id             | snowflake           | El ID del guild                                       |
| user                  | partial user object | El usuario baneado                                    |
| delete\_message\_secs | integer             | Número de segundos por los que se eliminaron mensajes |

#### Eliminar de baneo de servidor

Enviado cuando un usuario es desbaneado de un guild.

**Estructura de eliminación de baneo de servidor**

| Campo     | Tipo                | Descripción           |
| --------- | ------------------- | --------------------- |
| guild\_id | snowflake           | El ID del guild       |
| user      | partial user object | El usuario desbaneado |

#### Crear entrada de directorio de servidor

Enviado cuando se crea una entrada de directorio de guild. El payload interno es un objeto directory entry.

#### Actualizar entrada de directorio de servidor

Enviado cuando se actualiza una entrada de directorio de guild. El payload interno es un objeto directory entry.

#### Eliminar entrada de directorio de servidor

Enviado cuando se elimina una entrada de directorio de guild.

**Estructura de eliminación de entrada de directorio de servidor**

| Campo                  | Tipo      | Descripción                                             |
| ---------------------- | --------- | ------------------------------------------------------- |
| type                   | integer   | El tipo de entrada de directorio                        |
| directory\_channel\_id | snowflake | El ID del canal de directorio en el que está la entrada |
| guild\_id              | snowflake | El ID del guild en el que está la entrada               |
| entity\_id             | snowflake | El ID del guild o evento programado                     |
| created\_at            | string    | Cuándo se creó la entrada                               |
| primary\_category\_id? | integer   | La categoría principal de la entrada                    |
| description            | ?string   | La descripción de la entrada                            |
| author\_id             | snowflake | El ID del usuario que creó la entrada                   |

#### Actualizar de emoticonos de servidor

Enviado cuando se han actualizado los emojis de un guild.

**Estructura de actualización de emoticonos de servidor**

| Campo     | Tipo                 | Descripción            |
| --------- | -------------------- | ---------------------- |
| guild\_id | snowflake            | El ID del guild        |
| emojis    | array\[emoji object] | Los emojis en el guild |

#### Actualizar pegatinas de servidor

Enviado cuando se han actualizado los stickers de un guild.

**Estructura de actualización de pegatinas de servidor**

| Campo     | Tipo                   | Descripción              |
| --------- | ---------------------- | ------------------------ |
| guild\_id | snowflake              | El ID del guild          |
| stickers  | array\[sticker object] | Los stickers en el guild |

#### Crear solicitud de unión de servidor

Enviado cuando un usuario crea una solicitud de unión al guild. Requiere el permiso `KICK_MEMBERS` para usuarios distintos al usuario actual.

{% hint style="info" %}
Los eventos para solicitudes de unión a guilds creadas con un estado de **STARTED** se envían solo al usuario actual. Con respecto a otros usuarios, este evento solo se envía para solicitudes de unión enviadas. Si una solicitud de unión es creada y luego enviada, solo se recibirá un evento de **Guild Join Request Update** por parte de los moderadores.
{% endhint %}

{% hint style="info" %}
Si se crea una solicitud de unión para un usuario con el permiso **KICK\_MEMBERS**, este evento se enviará al usuario dos veces: una por la creación de la solicitud y otra por recibirla como moderador.
{% endhint %}

**Estructura de creación de solicitud de unión de servidor**

| Campo     | Tipo                      | Descripción                        |
| --------- | ------------------------- | ---------------------------------- |
| guild\_id | snowflake                 | El ID del guild                    |
| request   | guild join request object | La solicitud de unión creada       |
| status    | string                    | El estado de la solicitud de unión |

#### Actualizar de solicitud de unión de servidor

Enviado cuando se actualiza una solicitud de unión al guild. Requiere el permiso `KICK_MEMBERS` para usuarios distintos al usuario actual.

{% hint style="info" %}
Si se actualiza una solicitud de unión para un usuario con el permiso **KICK\_MEMBERS**, este evento se enviará al usuario dos veces: una por ser el usuario que creó la solicitud de unión y otra por recibirla como moderador.
{% endhint %}

**Estructura de actualización de solicitud de unión de se**rvidor

| Campo     | Tipo                      | Descripción                        |
| --------- | ------------------------- | ---------------------------------- |
| guild\_id | snowflake                 | El ID del guild                    |
| request   | guild join request object | La solicitud de unión actualizada  |
| status    | string                    | El estado de la solicitud de unión |

#### Eliminar solicitud de unión de servidor

Enviado cuando se elimina una solicitud de unión al guild. Requiere el permiso `KICK_MEMBERS` para usuarios distintos al usuario actual.

**Estructura de e**liminación de solicitud de unión de servidor

| Campo     | Tipo      | Descripción                                      |
| --------- | --------- | ------------------------------------------------ |
| guild\_id | snowflake | El ID del guild                                  |
| id        | snowflake | El ID de la solicitud de unión                   |
| user\_id  | snowflake | El ID del usuario que creó la solicitud de unión |

#### Añadir miembro al servidor

{% hint style="warning" %}
Si se utilizan Gateway intents, se requerirá el intent **GUILD\_MEMBERS** para recibir este evento.
{% endhint %}

Enviado cuando un nuevo usuario se une a un guild. El payload interno es un objeto guild member con una clave extra `guild_id`:

Para cuentas de usuario, este evento solo se envía para ellos mismos. Los usuarios no se suscriben automáticamente a amigos y relaciones implícitas o usuarios con los que tienen un DM abierto al unirse.

**Campos extra de la estructura de añadir miembro al servidor**

| Campo     | Tipo      | Descripción     |
| --------- | --------- | --------------- |
| guild\_id | snowflake | El ID del guild |

#### Actualizar miembro de servidor

{% hint style="warning" %}
Si se utilizan Gateway intents, se requerirá el intent **GUILD\_MEMBERS** para recibir este evento.
{% endhint %}

Enviado cuando se actualiza un miembro del guild. Esto también se disparará cuando cambie el objeto user de un miembro del guild. Los campos opcionales solo se incluirán si cambiaron.

Para cuentas de usuario, este evento solo se envía para miembros a los que están suscritos, y en respuesta a acciones cometidas por el usuario. Los usuarios se suscriben automáticamente a amigos y relaciones implícitas (dependiendo de la capacidad `NO_AFFINE_USER_IDS` Gateway), así como usuarios con los que tienen un DM abierto.

**Estructura de actualización de miembro de servidor**

¹ No se incluirá en contextos donde es imposible que exista un miembro pendiente.&#x20;

² Si el valor es un tiempo en el pasado, el timeout del miembro ha expirado y puede comunicarse de nuevo. No se enviará un evento cuando esto suceda.

| Campo                           | Tipo                           | Descripción                                                                                                                                                |
| ------------------------------- | ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| guild\_id                       | snowflake                      | El ID del guild                                                                                                                                            |
| user                            | partial user object            | El usuario que este miembro del guild representa                                                                                                           |
| nick?                           | ?string                        | El apodo específico del guild del miembro (1-32 caracteres)                                                                                                |
| avatar                          | ?string                        | El hash del avatar del guild del miembro                                                                                                                   |
| avatar\_decoration\_data?       | ?avatar decoration data object | La decoración del avatar del guild del miembro                                                                                                             |
| roles                           | array\[snowflake]              | Los IDs de roles asignados a este miembro                                                                                                                  |
| joined\_at                      | ISO8601 timestamp              | Cuándo el usuario se unió al guild                                                                                                                         |
| premium\_since                  | ?ISO8601 timestamp             | Cuándo el miembro se suscribió (comenzó a boostear) al guild                                                                                               |
| deaf?                           | boolean                        | Si el miembro está silenciado en canales de voz                                                                                                            |
| mute?                           | boolean                        | Si el miembro está muteado en canales de voz                                                                                                               |
| pending?¹                       | boolean                        | Si el miembro aún no ha pasado los requisitos de verificación de miembros del guild                                                                        |
| communication\_disabled\_until² | ?ISO8601 timestamp             | Cuándo expirará el timeout del usuario y el usuario podrá comunicarse en el guild de nuevo, null o un tiempo en el pasado si el usuario no está en timeout |
| flags                           | integer                        | Las banderas del miembro                                                                                                                                   |

#### Eliminar miembro de servidor

{% hint style="warning" %}
Si se utilizan Gateway intents, se requerirá el intent **GUILD\_MEMBERS** para recibir este evento.
{% endhint %}

Enviado cuando un usuario es removido de un guild (salir/kick/ban).

Para cuentas de usuario, este evento solo se envía para ellos mismos, miembros a los que están suscritos, y en respuesta a acciones cometidas por el usuario. Los usuarios se suscriben automáticamente a amigos y relaciones implícitas (dependiendo de la capacidad `NO_AFFINE_USER_IDS` Gateway), así como usuarios con los que tienen un DM abierto.

**Estructura de eliminación de miembro de servidor**

| Campo     | Tipo        | Descripción         |
| --------- | ----------- | ------------------- |
| guild\_id | snowflake   | El ID del guild     |
| user      | user object | El usuario removido |

#### Carga de miembro de servidor

Enviado en respuesta a Request Guild Members. Puedes usar `chunk_index` y `chunk_count` para calcular cuántos chunks quedan para tu solicitud.

**Estructura de carga de miembro de servidor**

| Campo        | Tipo                        | Descripción                                                                                       |
| ------------ | --------------------------- | ------------------------------------------------------------------------------------------------- |
| guild\_id    | snowflake                   | El ID del guild                                                                                   |
| members      | array\[guild member object] | Miembros del guild en chunks                                                                      |
| chunk\_index | integer                     | El índice del chunk en los chunks esperados para esta respuesta (0 ≤ chunk\_index < chunk\_count) |
| chunk\_count | integer                     | El número total de chunks esperados para esta respuesta                                           |
| not\_found?  | array                       | Los IDs pasados que no fueron encontrados                                                         |
| presences?   | array\[presence object]     | Las presencias de los miembros devueltos, si se solicitó                                          |
| nonce?       | string                      | El nonce usado en Request Guild Members o Search Recent Members, si hay alguno                    |

#### Crear de derechos de potenciación de servidor

Enviado cuando se crean entitlements de powerup del guild.

**Estructura de creación de derechos de potenciación de servidor**

| Campo        | Tipo                       | Descripción                         |
| ------------ | -------------------------- | ----------------------------------- |
| guild\_id    | snowflake                  | El ID del guild                     |
| entitlements | array\[entitlement object] | Los entitlements de powerup creados |

#### Eliminar de derechos de potenciación de servidor

Enviado cuando se eliminan entitlements de powerup del guild.

**Estructura de eliminación de derechos de potenciación de servidor**

| Campo        | Tipo                       | Descripción                            |
| ------------ | -------------------------- | -------------------------------------- |
| guild\_id    | snowflake                  | El ID del guild                        |
| entitlements | array\[entitlement object] | Los entitlements de powerup eliminados |

#### Crear rol de servidor

Enviado cuando se crea un rol del guild.

**Estructura de creación de rol de servidor**

| Campo     | Tipo        | Descripción     |
| --------- | ----------- | --------------- |
| guild\_id | snowflake   | El ID del guild |
| role      | role object | El rol creado   |

#### &#x20;Actualizar rol de servidor

Enviado cuando se actualiza un rol del guild.

**Estructura de actualización de rol de servidor**

| Campo     | Tipo        | Descripción        |
| --------- | ----------- | ------------------ |
| guild\_id | snowflake   | El ID del guild    |
| role      | role object | El rol actualizado |

#### Eliminar rol de servidor

Enviado cuando se elimina un rol del guild.

**Estructura de eliminación de rol de servidor**

| Campo     | Tipo      | Descripción     |
| --------- | --------- | --------------- |
| guild\_id | snowflake | El ID del guild |
| role\_id  | snowflake | El ID del rol   |

#### Eventos programados del servidor

#### Crear evento programado del servidor

Enviado cuando se crea un evento programado del guild. El payload interno es un objeto guild scheduled event.

#### Actualizar evento programado del servidor

Enviado cuando se actualiza un evento programado del guild. El payload interno es un objeto guild scheduled event.

#### Eliminar evento programado del servidor

Enviado cuando se elimina un evento programado del guild. El payload interno es un objeto guild scheduled event.

#### Crear excepción de evento programado del servidor

Enviado cuando se crea una excepción de evento programado del guild. El payload interno es un objeto guild scheduled event exception.

#### Actualizar excepción de evento programado del servidor

Enviado cuando se actualiza una excepción de evento programado del guild. El payload interno es un objeto guild scheduled event exception.

#### Eliminar excepción de evento programado del servidor

Enviado cuando se elimina una excepción de evento programado del guild. El payload interno es un objeto guild scheduled event exception.

{% hint style="info" %}
Este evento no se utiliza actualmente. Consulta **Guild Scheduled Event Exception Create** en su lugar.
{% endhint %}

#### Eliminar excepciones de evento programado del servidor

Enviado cuando se eliminan todas las excepciones de eventos programados del guild.

**Estructura de eliminación de excepciones de evento programado del servidor**

| Campo     | Tipo      | Descripción                           |
| --------- | --------- | ------------------------------------- |
| guild\_id | snowflake | El ID del guild                       |
| event\_id | snowflake | El ID del evento programado del guild |

#### Añadir usuario de evento programado del servidor

Enviado cuando un usuario se ha suscrito a un evento programado del guild o excepción. El payload interno es un objeto guild scheduled event user.

#### Eliminar usuario de evento programado del servidor

Enviado cuando un usuario se ha desuscrito de un evento programado del guild o excepción. El payload interno es un objeto guild scheduled event user.

#### Panel de sonido del servidor

#### Crear sonido para el panel de sonidos del servidor

Enviado cuando se crea un sonido de soundboard del guild. El payload interno es un objeto soundboard sound.

#### Actualizar sonido para el panel de sonidos del servidor&#x20;

Enviado cuando se actualiza un sonido de soundboard del guild. El payload interno es un objeto soundboard sound.

#### Eliminar sonido del panel de sonidos del servidor

Enviado cuando se elimina un sonido de soundboard del guild.

**Estructura de eliminación de sonido del panel de sonidos del servidor**

| Campo     | Tipo      | Descripción                     |
| --------- | --------- | ------------------------------- |
| guild\_id | snowflake | El ID del guild                 |
| sound\_id | snowflake | El ID del sonido del soundboard |

#### Sonidos del panel de sonidos

Enviado en respuesta a Request Soundboard Sounds.

**Campos del evento de sonidos del panel de sonidos**

| Campo              | Tipo                            | Descripción                                    |
| ------------------ | ------------------------------- | ---------------------------------------------- |
| guild\_id          | snowflake                       | El ID del guild fuente                         |
| soundboard\_sounds | array\[soundboard sound object] | Sonidos de soundboard personalizados del guild |

#### Integraciones

#### Actualización de integración de servidor

Enviado cuando se actualiza una integración del guild. Esto se envía además de uno de los eventos de abajo.

**Estructura de actualización de integración de servidor**

| Campo     | Tipo      | Descripción                                             |
| --------- | --------- | ------------------------------------------------------- |
| guild\_id | snowflake | El ID del guild cuyas integraciones fueron actualizadas |

#### Crear integración

Enviado cuando se crea una integración. El payload interno es un objeto integration con una clave adicional `guild_id`:

**Campos extra de la estructura de creación de integración**

| Campo     | Tipo      | Descripción     |
| --------- | --------- | --------------- |
| guild\_id | snowflake | El ID del guild |

#### Actualizar integración

Enviado cuando se actualiza una integración. El payload interno es un objeto integration con una clave adicional `guild_id`:

**Campos extra de la estructura de actualización de integración**

| Campo     | Tipo      | Descripción     |
| --------- | --------- | --------------- |
| guild\_id | snowflake | El ID del guild |

#### Eliminar integración

Enviado cuando se elimina una integración.

**Estructura de eliminación de integración**

| Campo            | Tipo      | Descripción                                            |
| ---------------- | --------- | ------------------------------------------------------ |
| id               | snowflake | El ID de la integración                                |
| guild\_id        | snowflake | El ID del guild                                        |
| application\_id? | snowflake | El ID de la aplicación OAuth2 integrada, si hay alguna |

#### Invitaciones

#### Crear invitación

{% hint style="warning" %}
Este evento no es recibido por cuentas de usuario.
{% endhint %}

Enviado cuando se crea una nueva invitación a un canal del guild. Requiere el permiso `MANAGE_CHANNELS`.

**Estructura de creación de invitación**

| Campo                | Tipo                       | Descripción                                                                                   |
| -------------------- | -------------------------- | --------------------------------------------------------------------------------------------- |
| code                 | string                     | El código de invitación (ID único)                                                            |
| type                 | integer                    | El tipo de invitación                                                                         |
| channel\_id          | snowflake                  | El ID del canal para el que es la invitación                                                  |
| guild\_id            | snowflake                  | El ID del guild para el que es esta invitación                                                |
| inviter?             | partial user object        | El usuario que creó la invitación                                                             |
| target\_type?        | integer                    | El tipo de objetivo para esta invitación del guild                                            |
| target\_user?        | partial user object        | El usuario cuyo stream mostrar para esta invitación de stream de canal de voz                 |
| target\_application? | partial application object | La aplicación embebida para abrir para esta invitación de aplicación embebida de canal de voz |
| expires\_at          | ?ISO8601 timestamp         | La fecha de expiración de la invitación, si expira                                            |
| created\_at          | ISO8601 timestamp          | Cuándo se creó esta invitación                                                                |
| uses                 | integer                    | Número de veces que se ha usado esta invitación                                               |
| max\_uses            | integer                    | Número máximo de veces que se puede usar esta invitación                                      |
| max\_age             | integer                    | Duración (en segundos) después de la cual expira la invitación                                |
| temporary            | boolean                    | Si esta invitación solo otorga membresía temporal                                             |

#### Eliminar invitación

{% hint style="warning" %}
Este evento no es recibido por cuentas de usuario.
{% endhint %}

Enviado cuando se elimina una invitación del guild. Requiere el permiso `MANAGE_CHANNELS`.

**Estructura de eliminación de invitación**

| Campo       | Tipo      | Descripción                                    |
| ----------- | --------- | ---------------------------------------------- |
| code        | string    | El código de invitación (ID único)             |
| channel\_id | snowflake | El ID del canal para el que es la invitación   |
| guild\_id   | snowflake | El ID del guild para el que es esta invitación |

#### Mensajes

{% hint style="warning" %}
A diferencia de los mensajes persistentes, los mensajes efímeros se envían directamente al usuario y al bot que envió el mensaje, en lugar de hacerlo a través del canal de la guild. Debido a esto, los mensajes efímeros están vinculados al intent **DIRECT\_MESSAGES**, y el objeto del mensaje no incluirá `guild_id` ni `member`.
{% endhint %}

**Campos extra del objeto del mensaje**

¹ Los mensajes efímeros no incluirán estos campos.&#x20;

² Los mensajes enviados por webhooks no incluirán este campo, ya que los webhooks no son miembros del guild.

| Campo         | Tipo                        | Descripción                                                                                                                |
| ------------- | --------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| channel\_type | integer                     | El tipo de canal en el que se envió este mensaje                                                                           |
| guild\_id?¹   | snowflake                   | El ID del guild en el que se envió el mensaje                                                                              |
| member? ¹ ²   | partial guild member object | Datos del miembro del guild para el autor de este mensaje                                                                  |
| mentions¹     | array\[partial user object] | Usuarios específicamente mencionados en el mensaje, con una clave extra `member` representando datos del miembro del guild |
| metadata?     | object                      | Metadatos personalizados para el mensaje (máx 25 claves, 1024 caracteres por clave y valor)                                |

#### Crear mensaje

Enviado cuando se crea un mensaje. El payload interno es un objeto message con la estructura extra de arriba.

#### Actualizar mensaje

{% hint style="warning" %}
El valor de `tts` siempre será `false` en las actualizaciones de mensajes.
{% endhint %}

Enviado cuando se actualiza un mensaje. El payload interno es un objeto message con la estructura extra de arriba.

#### Eliminar mensaje

Enviado cuando se elimina un mensaje.

**Estructura de eliminación de mensaje**

| Campo       | Tipo      | Descripción       |
| ----------- | --------- | ----------------- |
| id          | snowflake | El ID del mensaje |
| channel\_id | snowflake | El ID del canal   |
| guild\_id?  | snowflake | El ID del guild   |

#### Eliminación masiva de mensajes

Enviado cuando se eliminan múltiples mensajes de una vez.

**Estructura de eliminación masiva de mensajes**

| Campo       | Tipo              | Descripción                       |
| ----------- | ----------------- | --------------------------------- |
| ids         | array\[snowflake] | Los IDs de los mensajes removidos |
| channel\_id | snowflake         | El ID del canal                   |
| guild\_id?  | snowflake         | El ID del guild                   |

#### Adición de voto en encuesta

Enviado cuando un usuario vota en una encuesta. Si la encuesta permite selección múltiple, se enviará un evento por respuesta.

**Campos de adición de voto en encuesta**

| Campo       | Tipo      | Descripción        |
| ----------- | --------- | ------------------ |
| user\_id    | snowflake | ID del usuario     |
| channel\_id | snowflake | ID del canal       |
| message\_id | snowflake | ID del mensaje     |
| guild\_id?  | snowflake | ID del guild       |
| answer\_id  | integer   | ID de la respuesta |

#### Eliminación de voto en encuesta

Enviado cuando un usuario remueve su voto en una encuesta. Si la encuesta permite selecciones múltiples, se enviará un evento por respuesta.

**Campos de eliminación de voto en encuesta**

| Campo       | Tipo      | Descripción        |
| ----------- | --------- | ------------------ |
| user\_id    | snowflake | ID del usuario     |
| channel\_id | snowflake | ID del canal       |
| message\_id | snowflake | ID del mensaje     |
| guild\_id?  | snowflake | ID del guild       |
| answer\_id  | integer   | ID de la respuesta |

#### Añadir reacción a mensaje

Enviado cuando un usuario agrega una reacción a un mensaje.

**Estructura de añadir reacción a mensaje**

| Campo               | Tipo                 | Descripción                                                      |
| ------------------- | -------------------- | ---------------------------------------------------------------- |
| user\_id            | snowflake            | El ID del usuario                                                |
| channel\_id         | snowflake            | El ID del canal                                                  |
| message\_id         | snowflake            | El ID del mensaje                                                |
| message\_author\_id | snowflake            | El ID del autor del mensaje                                      |
| guild\_id?          | snowflake            | El ID del guild                                                  |
| member?             | member object        | El miembro que reaccionó                                         |
| emoji               | partial emoji object | El emoji usado para reaccionar                                   |
| type                | integer              | El tipo de reacción                                              |
| burst\_colors?      | array\[string]       | Los colores codificados en hex para renderizar la reacción burst |

#### Añadir múltiples reacciones a mensaje

{% hint style="info" %}
Este evento requiere la capacidad del Gateway **DEBOUNCE\_MESSAGE\_REACTIONS**.
{% endhint %}

Enviado cuando múltiples usuarios agregan reacciones a un mensaje en un período corto de tiempo.

**Estructura de añadir múltiples reacciones a mensaje**

| Campo       | Tipo                              | Descripción                         |
| ----------- | --------------------------------- | ----------------------------------- |
| channel\_id | snowflake                         | El ID del canal                     |
| message\_id | snowflake                         | El ID del mensaje                   |
| guild\_id?  | snowflake                         | El ID del guild                     |
| reactions   | array\[debounced reaction object] | Las reacciones agregadas al mensaje |

**Estructura de reacción con retardo**

| Campo | Tipo                 | Descripción                                             |
| ----- | -------------------- | ------------------------------------------------------- |
| users | array\[snowflake]    | Los IDs de los usuarios que reaccionaron con este emoji |
| emoji | partial emoji object | El emoji usado para reaccionar                          |

#### Eliminar reacción de mensaje

Enviado cuando un usuario remueve una reacción de un mensaje.

**Estructura de eliminación de reacción de mensaje**

| Campo       | Tipo                   | Descripción                    |
| ----------- | ---------------------- | ------------------------------ |
| user\_id    | snowflake              | El ID del usuario              |
| channel\_id | snowflake              | El ID del canal                |
| message\_id | snowflake              | El ID del mensaje              |
| guild\_id?  | snowflake              | El ID del guild                |
| emoji       | a partial emoji object | El emoji usado para reaccionar |
| type        | integer                | El tipo de reacción            |

#### Eliminar todas las reacciones de mensaje

Enviado cuando un usuario remueve explícitamente todas las reacciones de un mensaje.

**Estructura de Message Reaction Remove All**

| Campo       | Tipo      | Descripción       |
| ----------- | --------- | ----------------- |
| channel\_id | snowflake | El ID del canal   |
| message\_id | snowflake | El ID del mensaje |
| guild\_id?  | snowflake | El ID del guild   |

#### Eliminar emoticono de reacción de mensaje

Enviado cuando un usuario remueve todas las instancias de un emoji dado de las reacciones de un mensaje.

**Estructura de eliminación emoticono de reacción de mensaje**

| Campo       | Tipo                 | Descripción               |
| ----------- | -------------------- | ------------------------- |
| channel\_id | snowflake            | El ID del canal           |
| message\_id | snowflake            | El ID del mensaje         |
| guild\_id?  | snowflake            | El ID del guild           |
| emoji       | partial emoji object | El emoji que fue removido |

#### Eliminar mención reciente

Enviado cuando un mensaje que mencionó al usuario actual en la última semana es reconocido y eliminado.

**Estructura de eliminación de mención reciente**

| Campo       | Tipo      | Descripción       |
| ----------- | --------- | ----------------- |
| message\_id | snowflake | El ID del mensaje |

#### Últimos mensajes

Enviado en respuesta a Request Last Messages.

**Estructura de últimos mensajes**

| Campo     | Tipo                   | Descripción                                 |
| --------- | ---------------------- | ------------------------------------------- |
| guild\_id | snowflake              | El ID del guild                             |
| messages  | array\[message object] | Últimos mensajes de los canales solicitados |

#### Configuraciones de notificación

#### Actualizar configuración de notificaciones

Enviado cuando se actualizan las configuraciones de notificación de un usuario. El payload interno es un objeto notification settings.

#### OAuth2

#### Revocación de token de OAuth2

Enviado cuando se desautoriza una aplicación OAuth2.

**Estructura de revocación de token de OAuth2**

| Campo           | Tipo      | Descripción                                         |
| --------------- | --------- | --------------------------------------------------- |
| access\_token   | string    | El token de acceso que fue revocado                 |
| application\_id | snowflake | La aplicación OAuth2 cuya autorización fue revocada |

#### Presencia

#### Actualizar presencia

Este evento se envía cuando se actualiza la presencia o información de un usuario, como nombre o avatar. El payload interno es un objeto presence.

Para cuentas de usuario, este evento solo se envía para presencias a las que están suscritos. Los usuarios se suscriben automáticamente a la presencia general del usuario y a cada presencia por guild de amigos y relaciones implícitas (dependiendo de la capacidad `NO_AFFINE_USER_IDS` Gateway), así como a cada presencia por guild de usuarios con los que tienen un DM abierto.

{% hint style="warning" %}
Si estás utilizando Gateway inten&#x74;_&#x73;_, debes especificar el intent **GUILD\_PRESENCES** para poder recibir eventos de **Presence Update**.
{% endhint %}

{% hint style="warning" %}
Los eventos **Presence Update** nunca se reciben para la propia presencia del usuario. Debes rastrear tu propia presencia localmente o mediante el evento **Sessions Replace**.
{% endhint %}

{% hint style="warning" %}
El objeto `user` dentro de este evento puede estar muy incompleto. El único campo garantizado es el campo `id`, todo lo demás es opcional. Junto con esta limitación, no se requiere ningún campo específico, y los tipos de los campos no se validan. Tu cliente debe estar preparado para cualquier combinación de campos y tipos dentro de este evento.
{% endhint %}

#### Misiones

#### Actualizar estado de misiones de usuario

Enviado cuando se actualiza el estado de quest de un usuario.

**Estructura de actualización estado de misiones de usuario**

| Campo        | Tipo                     | Descripción                      |
| ------------ | ------------------------ | -------------------------------- |
| user\_status | quest user status object | El progreso de quest del usuario |

#### Actualizar finalización de misión del usuario

Enviado cuando se actualiza la eligibilidad de completación de quest de un usuario.

**Estructura de a**ctualización de finalización de misión del usuario

| Campo                             | Tipo               | Descripción                                            |
| --------------------------------- | ------------------ | ------------------------------------------------------ |
| quest\_enrollment\_blocked\_until | ?ISO8601 timestamp | Cuándo el usuario puede inscribirse en quests de nuevo |

#### Relaciones

**Estructura de relación parcial**

| Campo              | Tipo              | Descripción                                                                                                        |
| ------------------ | ----------------- | ------------------------------------------------------------------------------------------------------------------ |
| id                 | snowflake         | El ID del usuario objetivo                                                                                         |
| type               | integer           | El tipo de relación                                                                                                |
| nickname           | ?string           | El apodo del usuario en esta relación (1-32 caracteres)                                                            |
| since?             | ISO8601 timestamp | Cuándo el usuario solicitó una relación                                                                            |
| stranger\_request? | boolean           | Si la solicitud de amistad fue enviada por un usuario sin un amigo mutuo o guild mutuo pequeño (por defecto false) |
| user\_ignored      | boolean           | Si el usuario objetivo ha sido ignorado por el usuario actual                                                      |

#### Añadir relación

Enviado cuando se crea una relación, relevante para el usuario actual. El payload interno es un objeto relationship.

**Campos extra de la estructura de añadir relación**

| Campo           | Tipo    | Descripción                                                             |
| --------------- | ------- | ----------------------------------------------------------------------- |
| should\_notify? | boolean | Si el cliente debe notificar al usuario de la creación de esta relación |

#### Actualizar relación

Enviado cuando se actualiza una relación, relevante para el usuario actual (ej. cambió el apodo del amigo). El payload interno es un objeto partial relationship.

{% hint style="warning" %}
Esto no es enviado cuando el tipo de relación cambia; revisa **añadir relación** para ello.
{% endhint %}

#### Eliminar relación

Enviado cuando se remueve una relación, relevante para el usuario actual. El payload interno es un objeto partial relationship.

#### Añadir relación de juego

Enviado cuando se crea una relación de juego, relevante para el usuario actual. El payload interno es un objeto game relationship.

#### Eliminar relación de juego

Enviado cuando se remueve una relación de juego, relevante para el usuario actual.

**Estructura de eliminación de relación de juego**

| Campo            | Tipo              | Descripción                                           |
| ---------------- | ----------------- | ----------------------------------------------------- |
| id               | string            | El ID del usuario objetivo                            |
| application\_id  | snowflake         | El ID de la aplicación cuyo juego originó la relación |
| type             | integer           | El tipo de relación                                   |
| since            | ISO8601 timestamp | Cuándo el usuario solicitó una relación               |
| dm\_access\_type | integer           | El nivel de acceso a DM para la relación              |
| user\_id         | snowflake         | El ID del usuario actual                              |

#### Sala

#### Objeto de sala de Gateway

Los objetos Lobby recibidos sobre el Gateway tienen atributos extendidos que no se proporcionan en la API REST.

**Campos extra de la estructura de sala de Gateway**

| Campo         | Tipo                             | Descripción                                            |
| ------------- | -------------------------------- | ------------------------------------------------------ |
| voice\_states | array\[lobby voice state object] | Los estados de voz de los usuarios ya en el lobby      |
| region        | string                           | El ID de región de voz desde donde se hospeda el lobby |
| metadata      | ?map\[string, string]            | Los metadatos del lobby                                |

**Razón de eliminación de la sala**

| Valor   | Descripción             |
| ------- | ----------------------- |
| deleted | El lobby fue eliminado  |
| removed | El usuario fue removido |

#### Crear sala

Enviado cuando se actualiza un lobby. El payload interno es un objeto gateway lobby.

#### Actualizar sala

Enviado cuando se actualiza un lobby. El payload interno es un objeto gateway lobby.

#### Eliminar sala

Enviado cuando se elimina un lobby o el usuario es removido de un lobby.

**Estructura de eliminación de sala**

| Campo  | Tipo      | Descripción                       |
| ------ | --------- | --------------------------------- |
| id     | snowflake | El ID del lobby                   |
| reason | string    | La razón de eliminación del lobby |

#### Añadir miembro a sala

Enviado cuando un usuario se une a un lobby.

**Estructura de añadir miembro a sala**

| Campo           | Tipo                | Descripción                              |
| --------------- | ------------------- | ---------------------------------------- |
| member          | lobby member object | El miembro agregado                      |
| lobby\_id       | snowflake           | El ID del lobby                          |
| application\_id | snowflake           | El ID de la aplicación que creó el lobby |

#### Conectar miembro a sala

Enviado cuando un usuario se une a una llamada de lobby.

**Estructura de conectar miembro a sala**

| Campo     | Tipo                | Descripción                                   |
| --------- | ------------------- | --------------------------------------------- |
| member    | lobby member object | El miembro que se unió a la llamada del lobby |
| lobby\_id | snowflake           | El ID del lobby                               |

#### Desconectar miembro a sala

Enviado cuando un usuario sale de una llamada de lobby.

**Estructura de desconectar miembro a sala**

| Campo     | Tipo                | Descripción                                  |
| --------- | ------------------- | -------------------------------------------- |
| member    | lobby member object | El miembro que salió de la llamada del lobby |
| lobby\_id | snowflake           | El ID del lobby                              |

#### Actualizar miembro a sala

Enviado cuando se actualiza un miembro del lobby.

**Estructura de actualizar miembro a sala**

| Campo           | Tipo                | Descripción                              |
| --------------- | ------------------- | ---------------------------------------- |
| member          | lobby member object | El miembro actualizado                   |
| lobby\_id       | snowflake           | El ID del lobby                          |
| application\_id | snowflake           | El ID de la aplicación que creó el lobby |

#### Eliminar miembro a sala

Enviado cuando se remueve un miembro del lobby.

**Estructura de eliminar miembro a sala**

| Campo           | Tipo                | Descripción                              |
| --------------- | ------------------- | ---------------------------------------- |
| member          | lobby member object | El miembro removido                      |
| lobby\_id       | snowflake           | El ID del lobby                          |
| application\_id | snowflake           | El ID de la aplicación que creó el lobby |

#### Crear mensaje de sala

Enviado cuando se crea un mensaje de lobby. El payload interno es un objeto message con la estructura extra de arriba.

{% hint style="warning" %}
Cuando la sala no tiene un canal vinculado, el mensaje será un objeto de mensaje parcial.
{% endhint %}

#### Actualizar mensaje de sala

Enviado cuando se actualiza un mensaje de lobby. El payload interno es un objeto message con la estructura extra de arriba.

{% hint style="warning" %}
Cuando la sala no tiene un canal vinculado, el mensaje será un objeto de mensaje parcial.
{% endhint %}

{% hint style="warning" %}
El valor de `tts` siempre será `false` en las actualizaciones de mensajes.
{% endhint %}

#### Eliminar mensaje de sala

Enviado cuando se elimina un mensaje de lobby.

**Estructura de eliminación de mensaje de sala**

| Campo     | Tipo      | Descripción       |
| --------- | --------- | ----------------- |
| id        | snowflake | El ID del mensaje |
| lobby\_id | snowflake | El ID del lobby   |

#### Actualizar estado de voz de sala

Enviado cuando alguien se une/sale/mueve lobbies. El payload interno es un objeto lobby voice state.

#### Actualizar estado de voz de servidor

Enviado cuando se actualiza el servidor de voz de un lobby. Esto se envía cuando se conecta inicialmente a voz, y cuando la instancia de voz actual falla y se cambia a un nuevo servidor.

{% hint style="warning" %}
A `null` endpoint means that the voice server allocated has gone away and is trying to be reallocated. You should attempt to disconnect from the currently connected voice server, and not attempt to reconnect until a new voice server is allocated.
{% endhint %}

**Estructura de actualización de estado de voz de servidor**

| Campo     | Tipo      | Descripción                                                    |
| --------- | --------- | -------------------------------------------------------------- |
| token     | string    | El token de conexión de voz                                    |
| lobby\_id | snowflake | El lobby para el que es esta actualización del servidor de voz |
| endpoint  | ?string   | El host del servidor de voz                                    |

**Ejemplo de payload de actualización del servidor de voz de la sala:**

```json
{
  "token": "66d29164ee8cd919",
  "lobby_id": "41771983423143937",
  "endpoint": "smart.loyal.discord.media:443"
}
```

#### Mensajes Guardados

#### Crear mensaje guardado

Enviado cuando se guarda un mensaje. El payload interno es un objeto saved message.

#### Eliminar mensaje guardado

Enviado cuando se desmarca un mensaje guardado.

**Estructura de eliminación de mensajes guardados**

| Campo       | Tipo      | Descripción       |
| ----------- | --------- | ----------------- |
| channel\_id | snowflake | El ID del canal   |
| message\_id | snowflake | El ID del mensaje |

#### Sesiones

#### Reemplazar sesión

Enviado cuando se actualiza la lista de sesiones del usuario actual o la presencia. El payload interno es una lista de objetos session.

#### Instancias de sscenario

#### Crear instancia de escenario

Enviado cuando se crea una instancia de escenario (i.e. el escenario está ahora "en vivo"). El payload interno es un objeto stage instance.

#### Actualizar instancia de escenario

Enviado cuando se actualiza una instancia de escenario. El payload interno es un objeto stage instance.

#### Eliminar instancia de escenario

Enviado cuando se elimina una instancia de escenario (i.e. el escenario se ha cerrado). El payload interno es un objeto stage instance.

#### Directos

#### Objeto de directo

**Estructura de directo**

¹ Solo presente en eventos Gateway Stream Create.

| Campo            | Tipo      | Descripción                                                    |
| ---------------- | --------- | -------------------------------------------------------------- |
| stream\_key      | string    | La clave del stream                                            |
| rtc\_server\_id¹ | snowflake | El ID del servidor RTC para el stream, usado al conectar a voz |
| region           | string    | La región de voz en la que está el stream                      |
| viewer\_ids      | array     | Los IDs de los espectadores actualmente viendo el stream       |
| paused           | boolean   | Si el stream está pausado                                      |

**Clave de directo**

La clave del stream es un identificador único para un stream, representado como una lista de valores delimitados por dos puntos. El primer valor es el tipo de stream, seguido por la ubicación del stream, y finalmente el propietario del stream.

**Estructura de clave de directo**

| Campo        | Tipo      | Descripción                                  |
| ------------ | --------- | -------------------------------------------- |
| type         | string    | El tipo de stream                            |
| guild\_id?   | snowflake | El ID del guild al que se está transmitiendo |
| channel\_id? | snowflake | El ID del canal al que se está transmitiendo |
| owner\_id    | snowflake | El ID del usuario que posee el stream        |

**Tipo de directo**

| Valor | Descripción                                                                                 |
| ----- | ------------------------------------------------------------------------------------------- |
| guild | Un stream en un canal de voz de guild                                                       |
| call  | Un stream en una llamada DM                                                                 |
| test  | Un stream usado para pruebas de velocidad; no se encuentra en contextos regulares de stream |

**Ejemplo de clave de directo:**&#x20;

```
 guild:839502008108580904:850360749460553769:852892297661906993
 call:1110739331624210483:852892297661906993
 test:852892297661906993
```

**Ejemplo de Stream:**

```json
{
  "stream_key": "call:1110739331624210483:852892297661906993",
  "rtc_server_id": "1278201813102755892",
  "region": "us-east",
  "viewer_ids": ["193696591125807105"],
  "paused": false
}
```

#### Crear directo

Enviado cuando se crea un stream. El payload interno es un objeto stream.

#### Actualizar directo de servidor

Enviado cuando se actualiza el servidor de voz de un stream. Esto se envía cuando se conecta inicialmente a un stream, y cuando la instancia actual del stream falla y se cambia a un nuevo servidor.

{% hint style="warning" %}
Un endpoint `null` significa que el servidor de voz asignado ha desaparecido y se está intentando reasignar. Debes intentar desconectarte del servidor de voz al que estás conectado actualmente y no intentar reconectarte hasta que se asigne un nuevo servidor de voz.
{% endhint %}

**Estructura de actualización de directo de servidor**

| Campo       | Tipo    | Descripción                 |
| ----------- | ------- | --------------------------- |
| token       | string  | El token de conexión de voz |
| stream\_key | string  | La clave del stream         |
| endpoint    | ?string | El host del servidor de voz |

**Ejemplo de actualización de directo de servidor:**

```json
{
  "token": "91f8016f34a5cd17",
  "stream_key": "call:1110739331624210483:852892297661906993",
  "guild_id": null,
  "endpoint": "smart.loyal.discord.media:443"
}
```

#### Actualizar directo

Enviado cuando se actualiza un stream. El payload interno es un objeto stream.

#### Eliminar directo

Enviado cuando se elimina un stream, o se vuelve no disponible debido a una interrupción.

**Estructura de eliminación de directo**

| Campo        | Tipo    | Descripción                                               |
| ------------ | ------- | --------------------------------------------------------- |
| stream\_key  | string  | La clave del stream                                       |
| reason       | string  | La razón para terminar el stream                          |
| unavailable? | boolean | Si el stream no está disponible debido a una interrupción |

**Razón de eliminación de directo**

| Valor                        | Descripción                                                           |
| ---------------------------- | --------------------------------------------------------------------- |
| user\_requested              | El usuario solicitó terminar el stream                                |
| stream\_ended                | El cliente fue desconectado porque el stream terminó                  |
| stream\_full                 | El cliente intentó unirse a un stream lleno                           |
| unauthorized                 | El cliente no está autorizado para ver el stream                      |
| safety\_guild\_rate\_limited | El stream fue limitado por velocidad debido a restricciones del guild |
| parse\_failed                | Falló el análisis de la clave del stream                              |
| invalid\_channel             | El canal proporcionado no es válido para este tipo de stream          |

**Ejemplo de eliminación de directo:**

```json
{
  "stream_key": "call:1110739331624210483:852892297661906993",
  "reason": "stream_ended"
}
```

#### Escribir

#### Inicio de escritura

Enviado cuando un usuario comienza a escribir en un canal.

**Estructura de inicio de escritura**

| Campo       | Tipo          | Descripción                                                       |
| ----------- | ------------- | ----------------------------------------------------------------- |
| channel\_id | snowflake     | ID del canal                                                      |
| guild\_id?  | snowflake     | ID del guild                                                      |
| user\_id    | snowflake     | ID del usuario                                                    |
| timestamp   | integer       | Tiempo unix (en segundos) de cuándo el usuario comenzó a escribir |
| member?     | member object | El miembro que comenzó a escribir si esto pasó en un guild        |

#### Usuario actual

#### Actualizar usuario

Enviado cuando cambian las propiedades del usuario actual. El payload interno es un objeto user.

#### Actualizar aplicación de usuario

Enviado cuando se autoriza o actualiza una aplicación integrada.

**Estructura de User Application Remove**

| Campo           | Tipo      | Descripción            |
| --------------- | --------- | ---------------------- |
| application\_id | snowflake | El ID de la aplicación |

#### Eliminar aplicación de usuario

Enviado cuando el usuario actual desautoriza una aplicación integrada.

**Estructura de eliminación de aplicación de usuario**

| Campo           | Tipo      | Descripción            |
| --------------- | --------- | ---------------------- |
| application\_id | snowflake | El ID de la aplicación |

#### Actualizar conexiones de usuario

Enviado cuando el usuario actual tiene sus conexiones actualizadas. El payload interno es o un objeto connection, o el siguiente objeto:

**Estructura de actualización conexiones de usuario**

| Campo    | Tipo      | Descripción              |
| -------- | --------- | ------------------------ |
| user\_id | snowflake | El ID del usuario actual |

#### Actualizar configuraciones de usuario de servidor

Enviado cuando se actualizan las configuraciones de usuario de un guild. El payload interno es un objeto user guild settings.

#### Operación de fusión de usuario completada

Enviado cuando el usuario actual tiene su cuenta fusionada con una cuenta provisional. Cuando esto se envía, lo siguiente ha ocurrido:

* Las relaciones de la cuenta provisional se han movido al usuario actual.
* Las relaciones de juego de la cuenta provisional se han movido al usuario actual.
* Las membresías de lobby de la cuenta provisional se han movido al usuario actual.
* Los DMs de la cuenta provisional se han migrado al usuario actual. Si hay un conflicto, se crea un DM grupal.
* Los usuarios que han bloqueado la cuenta provisional ahora bloquean al usuario actual.

**Estructura de operación de fusión de usuario completada**

| Campo                | Tipo      | Descripción                              |
| -------------------- | --------- | ---------------------------------------- |
| merge\_operation\_id | snowflake | El ID de la operación de fusión          |
| source\_user\_id     | snowflake | El ID de la cuenta con la que se fusionó |

#### Actualizar nota de usuario

Enviado cuando se modifica una nota que el usuario actual tiene sobre otro usuario.

**Estructura de actualización de nota de usuario**

| Campo | Tipo      | Descripción                   |
| ----- | --------- | ----------------------------- |
| id    | snowflake | El ID del usuario             |
| note  | string    | La nueva nota para el usuario |

#### Actualizar ajustes de usuario PROTO

Enviado cuando se modifican las configuraciones de usuario protobuf del cliente.

**Estructura de actualización de ajustes de usuario PROTO**

| Campo    | Tipo                       | Descripción                                                                                                     |
| -------- | -------------------------- | --------------------------------------------------------------------------------------------------------------- |
| settings | user settings proto object | Las nuevas configuraciones de usuario                                                                           |
| partial  | boolean                    | Si la actualización de configuraciones es parcial (debe fusionarse con las configuraciones en caché existentes) |

**Estructura de ajustes de usuario PROTO**&#x20;

| Campo | Tipo    | Descripción                                                                 |
| ----- | ------- | --------------------------------------------------------------------------- |
| type  | integer | El tipo de configuraciones de usuario                                       |
| proto | string  | El protobuf de configuraciones de usuario serializado codificado en base 64 |

#### Actualizar ajustes de usuario

Enviado cuando se modifican las configuraciones de usuario del cliente. El payload interno es un objeto user settings.

{% hint style="info" %}
Este evento no se envía al usar la capacidad del Gateway **USER\_SETTINGS\_PROTO**.
{% endhint %}

#### Actualizar ajustes de sonido

Enviado cuando se modifican las configuraciones de contexto de audio. Solo se envían las configuraciones modificadas.

**Estructura de actualización de ajustes de sonido**

| Campo  | Tipo                                          | Descripción                                                  |
| ------ | --------------------------------------------- | ------------------------------------------------------------ |
| user   | map\[snowflake, audio context setting object] | Configuraciones de contexto de audio para usuarios           |
| stream | map\[snowflake, audio context setting object] | Configuraciones de contexto de audio para streams de usuario |

#### Actualizar requisitos de acción de usuario

Enviado cuando un usuario debe completar una cierta acción (como verificar su número de teléfono) antes de continuar usando Discord.

**Estructura de actualización requisitos de acción de usuario**

| Campo            | Tipo    | Descripción                                                                                                   |
| ---------------- | ------- | ------------------------------------------------------------------------------------------------------------- |
| required\_action | ?string | La acción que un usuario debe tomar antes de continuar usando Discord, `null` si ya no se requiere una acción |

#### Voz

#### Actualizar estado de voz

Enviado cuando alguien se une/sale/mueve canales de voz o llamadas. El payload interno es un objeto voice state.

#### Actualizar estado de voz de servidor

Enviado cuando se actualiza el servidor de voz de un guild o llamada. Esto se envía cuando se conecta inicialmente a voz, y cuando la instancia de voz actual falla y se cambia a un nuevo servidor.

{% hint style="warning" %}
Un endpoint `null` significa que el servidor de voz asignado ha desaparecido y se está intentando reasignar. Debes intentar desconectarte del servidor de voz al que estás conectado actualmente y no intentar reconectarte hasta que se asigne un nuevo servidor de voz.
{% endhint %}

**Estructura de actualización de estado de voz de servidor**

| Campo        | Tipo       | Descripción                                                            |
| ------------ | ---------- | ---------------------------------------------------------------------- |
| token        | string     | El token de conexión de voz                                            |
| guild\_id    | ?snowflake | El guild para el que es esta actualización del servidor de voz         |
| channel\_id? | snowflake  | El canal privado para el que es esta actualización del servidor de voz |
| endpoint     | ?string    | El host del servidor de voz                                            |

**Ejemplo de payload de actualización de estado de voz de servidor:**

```json
{
  "token": "66d29164ee8cd919",
  "guild_id": "41771983423143937",
  "endpoint": "smart.loyal.discord.media:443"
}
```

#### Envío de efecto de sonido a canal de voz

Enviado cuando alguien envía un efecto, como una reacción emoji o un sonido de soundboard, en un canal de voz al que está conectado el usuario actual.

**Estructura de envío de efecto de sonido a canal de voz**

| Campo           | Tipo                  | Descripción                                                                |
| --------------- | --------------------- | -------------------------------------------------------------------------- |
| channel\_id     | snowflake             | El ID del canal donde se envió el efecto                                   |
| guild\_id       | snowflake             | El ID del guild donde se envió el efecto                                   |
| user\_id        | snowflake             | El ID del usuario que envió el efecto                                      |
| animation\_type | ?integer              | El tipo de animación emoji                                                 |
| animation\_id   | integer               | El ID de la animación emoji (0-20)                                         |
| emoji           | ?partial emoji object | El emoji enviado, si aplica                                                |
| sound\_id?      | snowflake             | El ID del sonido del soundboard                                            |
| sound\_volume?  | float                 | El volumen del sonido del soundboard (representado como un float de 0 a 1) |

**Ejemplo de payload de envío de efecto de sonido a canal de voz:**

```json
{
  "guild_id": "839502008108580904",
  "animation_id": 0,
  "animation_type": 1,
  "channel_id": "850360749460553769",
  "emoji": {
    "animated": false,
    "id": null,
    "name": "🦆"
  },
  "sound_id": 1,
  "sound_volume": 1,
  "user_id": "852892297661906993"
}
```

#### Webhooks

#### Actualizar webhooks

Enviado cuando se crea, actualiza o elimina un webhook de canal del guild.

**Estructura de actualización de webhooks**

| Campo       | Tipo      | Descripción     |
| ----------- | --------- | --------------- |
| guild\_id   | snowflake | El ID del guild |
| channel\_id | snowflake | El ID del canal |

#### Interacciones

#### Crear interacción

Enviado cuando un usuario usa un Comando de Aplicación o Componente de Mensaje. El payload interno es una Interaction.

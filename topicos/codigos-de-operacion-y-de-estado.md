---
icon: signal-bars
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

# Códigos de operación y de estado

### Gateway

Todos los eventos del Gateway de Discord están etiquetados con un opcode que indica el tipo de carga útil. Tu conexión a la Gateway puede cerrarse en algún momento; cuando ocurre, recibirás un código de cierre que indica el motivo.

#### Opcodes del Gateway

| Código | Nombre                                 | Acción         | Descripción                                                                                  |
| ------ | -------------------------------------- | -------------- | -------------------------------------------------------------------------------------------- |
| 0      | Dispatch                               | Recibir        | Se ha enviado un evento.                                                                     |
| 1      | Heartbeat                              | Enviar/Recibir | Mantiene la conexión WebSocket activa.                                                       |
| 2      | Identify                               | Enviar         | Inicia una nueva sesión durante el saludo inicial.                                           |
| 3      | Presence Update                        | Enviar         | Actualiza la presencia del cliente.                                                          |
| 4      | Voice State Update                     | Enviar         | Unirse/dejar o moverse entre canales y llamadas de voz.                                      |
| 5      | Voice Server Ping                      | Enviar         | Realiza ping a los servidores de voz de Discord.                                             |
| 6      | Resume                                 | Enviar         | Reanuda una sesión previa desconectada.                                                      |
| 7      | Reconnect                              | Recibir        | Debes intentar reconectar y reanudar de inmediato.                                           |
| 8      | Request Guild Members                  | Enviar         | Solicita información sobre miembros del gremio.                                              |
| 9      | Invalid Session                        | Recibir        | La sesión se ha invalidado. Debes reconectar y realizar identify/reanudar según corresponda. |
| 10     | Hello                                  | Recibir        | Se envía inmediatamente tras conectar; contiene el `heartbeat_interval` a usar.              |
| 11     | Heartbeat ACK                          | Recibir        | Reconoce un heartbeat recibido.                                                              |
| ~~12~~ | ~~Guild Sync~~                         | ~~Enviar~~     | ~~Solicita todos los miembros y presencias para gremios.~~                                   |
| 13     | Call Connect                           | Enviar         | Solicita los datos de llamada preexistentes de un canal privado.                             |
| 14     | Guild Subscriptions                    | Enviar         | Actualiza las suscripciones a un gremio.                                                     |
| ~~15~~ | ~~Lobby Connect~~                      | ~~Enviar~~     | ~~Unirse a un lobby.~~                                                                       |
| ~~16~~ | ~~Lobby Disconnect~~                   | ~~Enviar~~     | ~~Salir de un lobby.~~                                                                       |
| 17     | Lobby Voice States                     | Enviar         | Actualiza el estado de voz del cliente en un lobby.                                          |
| 18     | Stream Create                          | Enviar         | Crea una transmisión para el cliente.                                                        |
| 19     | Stream Delete                          | Enviar         | Finaliza una transmisión del cliente.                                                        |
| 20     | Stream Watch                           | Enviar         | Ver la transmisión de otro usuario.                                                          |
| 21     | Stream Ping                            | Enviar         | Ping al servidor de voz de la transmisión.                                                   |
| 22     | Stream Set Paused                      | Enviar         | Pausa o reanuda una transmisión del cliente.                                                 |
| ~~23~~ | ~~LFG Subscriptions~~                  | ~~Enviar~~     | ~~Actualiza suscripciones de un lobby LFG.~~                                                 |
| ~~24~~ | ~~Request Guild Application Commands~~ | ~~Enviar~~     | ~~Solicita comandos de aplicación de gremio.~~                                               |
| ~~25~~ | ~~Embedded Activity Create~~           | ~~Enviar~~     | ~~Inicia una actividad incrustada en un canal de voz o llamada.~~                            |
| ~~26~~ | ~~Embedded Activity Delete~~           | ~~Enviar~~     | ~~Detiene una actividad incrustada.~~                                                        |
| ~~27~~ | ~~Embedded Activity Update~~           | ~~Enviar~~     | ~~Actualiza una actividad incrustada.~~                                                      |
| 28     | Request Forum Unreads                  | Enviar         | Solicita el conteo de hilos no leídos en canales tipo foro.                                  |
| 29     | Remote Command                         | Enviar         | Envía un mensaje a otra sesión Gateway.                                                      |
| 30     | Request Deleted Entity IDs             | Enviar         | Solicita IDs de entidades eliminadas no coincidentes con un hash dado para un gremio.        |
| 31     | Request Soundboard Sounds              | Enviar         | Solicita sonidos del soundboard para gremios.                                                |
| ~~32~~ | ~~Speed Test Create~~                  | ~~Enviar~~     | ~~Crea una prueba de velocidad RTC.~~                                                        |
| ~~33~~ | ~~Speed Test Delete~~                  | ~~Enviar~~     | ~~Elimina una prueba de velocidad RTC.~~                                                     |
| 34     | Request Last Messages                  | Enviar         | Solicita los últimos mensajes para canales de un gremio.                                     |
| 35     | Search Recent Members                  | Enviar         | Solicita información de miembros que se unieron recientemente al gremio.                     |
| 36     | Request Channel Statuses               | Enviar         | Solicita los estados de canales de voz para un gremio.                                       |
| 37     | Guild Subscriptions Bulk               | Enviar         | Actualiza suscripciones para múltiples gremios.                                              |
| 38     | Guild Channels Resync                  | Enviar         | Resincroniza canales accesibles de un gremio.                                                |
| 39     | Request Channel Member Count           | Enviar         | Solicita el número de miembros y conectados en un canal.                                     |
| 40     | QoS Heartbeat                          | Enviar         | Mantiene la conexión WebSocket activa (con métricas QoS).                                    |
| 41     | Update Time Spent Session ID           | Enviar         | Rastrea el tiempo pasado en la sesión actual.                                                |

#### Códigos de cierre de eventos Gateway

| Código   | Descripción                      | Explicación                                                                                                                  |
| -------- | -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| 4000     | Error desconocido.               | No estamos seguros de qué salió mal. ¿Quieres reconectar?                                                                    |
| 4001     | Opcode desconocido.              | Se envió un opcode de Gateway inválido o una carga inválida para un opcode. ¡No hagas eso!                                   |
| 4002     | Error de decodificación.         | Se envió una carga inválida. ¡No hagas eso!                                                                                  |
| 4003     | No autenticado.                  | Se envió una carga antes de identificar, o la sesión quedó invalidada.                                                       |
| 4004     | Fallo de autenticación.          | El token de cuenta enviado al identificar es incorrecto.                                                                     |
| 4005     | Ya autenticado.                  | Se envió más de una carga tipo identify. ¡No hagas eso!                                                                      |
| ~~4006~~ | ~~Sesión no válida.~~            | ~~La sesión ya no es válida.~~                                                                                               |
| 4007     | `seq` inválido.                  | La secuencia enviada al reanudar era inválida. Reconecta y crea nueva sesión.                                                |
| 4008     | Límite de velocidad.             | ¡Estás enviando cargas demasiado rápido! Se desconectará.                                                                    |
| 4009     | Sesión caducada.                 | La sesión caducó. Reconecta y crea una nueva.                                                                                |
| 4010     | Shard inválido.                  | Se envió un shard inválido al identificar.                                                                                   |
| 4011     | Sharding requerido.              | La sesión iba a manejar demasiados gremios; deberás usar shards para conectar.                                               |
| 4012     | Versión API inválida.            | Se envió una versión de Gateway inválida.                                                                                    |
| 4013     | Intención(es) inválida(s).       | Se envió una intención inválida para intents de Gateway. Puede que se calculó mal el valor de bits.                          |
| 4014     | Intención(es) no permitida(s).   | Se envió una intención no permitida; puede que intentaste especificar una intent para la que no tienes permiso o aprobación. |
| 4016     | Solicitud de conexión cancelada. | Se canceló la solicitud de conexión de un dispositivo consola. Ya no se necesita la sesión Gateway.                          |

### Voice <a href="#voice" id="voice"></a>

El Gateway de voz tiene su propio conjunto de opcodes y códigos de cierre.

#### Opcodes de Voz

| Código | Nombre                     | Acción             | Descripción                                                                     |
| ------ | -------------------------- | ------------------ | ------------------------------------------------------------------------------- |
| 0      | Identify                   | Enviar             | Iniciar una nueva conexión WebSocket de voz.                                    |
| 1      | Select Protocol            | Enviar             | Seleccionar el protocolo de voz.                                                |
| 2      | Ready                      | Recibir            | Completar el saludo WebSocket.                                                  |
| 3      | Heartbeat                  | Enviar/Recibir     | Mantener viva la conexión WebSocket de voz.                                     |
| 4      | Session Description        | Recibir            | Describir la sesión.                                                            |
| 5      | Speaking                   | Enviar/Recibir     | Indicar qué usuarios están hablando.                                            |
| 6      | Heartbeat ACK              | Recibir            | Reconocer un heartbeat recibido.                                                |
| 7      | Resume                     | Enviar             | Reanudar una sesión previa desconectada.                                        |
| 8      | Hello                      | Recibir            | Se envía inmediatamente tras conectar, contiene el `heartbeat_interval` a usar. |
| 9      | Resumed                    | Recibir            | Respuesta reconociendo la reanudación exitosa.                                  |
| ~~10~~ | ~~Signal~~                 | ~~Enviar/Recibir~~ | ~~Señalizar a pares WebRTC para conexiones P2P.~~                               |
| ~~11~~ | ~~Reset~~                  | ~~Enviar/Recibir~~ | ~~Reiniciar la conexión de voz.~~                                               |
| 11     | Client Connect             | Recibir            | Indicar que clientes se han conectado al canal de voz.                          |
| 12     | Video                      | Enviar/Recibir     | Describir la sesión de video.                                                   |
| 13     | Client Disconnect          | Recibir            | Indicar que un cliente se ha desconectado del canal de voz.                     |
| 14     | Session Update             | Enviar/Recibir     | Indicar una actualización en la descripción de sesión.                          |
| 15     | Media Sink Wants           | Enviar/Recibir     | Indicar los streams de medios deseados para simulcasting.                       |
| 16     | Voice Backend Version      | Enviar/Recibir     | Información de versión del backend de voz.                                      |
| ~~17~~ | ~~Channel Options Update~~ | ~~Recibir~~        | ~~Indicar una actualización de las propiedades de la conexión de voz.~~         |
| 18     | Client Flags               | Recibir            | Indicar flags de un cliente.                                                    |
| ~~19~~ | ~~Speed Test~~             | ~~Recibir~~        | ~~Indicar resultados de una prueba de velocidad.~~                              |
| 20     | Client Platform            | Recibir            | Indicar la plataforma en la que un cliente está conectado.                      |

#### Códigos de cierre de eventos de Voz

| Código | Descripción                      | Explicación                                                                                                         |
| ------ | -------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| 4001   | Opcode desconocido.              | Se envió un opcode de voz inválido.                                                                                 |
| 4002   | No se pudo decodificar la carga. | Se envió una carga inválida al identificar.                                                                         |
| 4003   | No autenticado.                  | Se envió una carga antes de identificar con el Gateway.                                                             |
| 4004   | Fallo de autenticación.          | El token enviado al identificar es incorrecto.                                                                      |
| 4005   | Ya autenticado.                  | Se envió más de una carga tipo identify.                                                                            |
| 4006   | Sesión ya no válida.             | La sesión ya no es válida.                                                                                          |
| 4009   | Sesión caducada.                 | La sesión ha caducado.                                                                                              |
| 4011   | Servidor no encontrado.          | El servidor al que intentabas conectar no se encontró.                                                              |
| 4012   | Protocolo desconocido.           | No se reconoció el protocolo enviado.                                                                               |
| 4014   | Desconectado.                    | Cliente individual desconectado (por kick, caída de la sesión principal, etc.). No debe reconectar.                 |
| 4015   | Servidor de voz caído.           | El servidor se cayó. Intenta reanudar la conexión de voz.                                                           |
| 4016   | Modo de cifrado desconocido.     | No se reconoció tu modo de cifrado.                                                                                 |
| 4020   | Solicitud incorrecta.            | Se envió una solicitud malformada.                                                                                  |
| 4021   | Límite de velocidad alcanzado.   | Se superó el límite de velocidad. No debe reconectar.                                                               |
| 4022   | Desconectado.                    | Desconectar todos los clientes (canal borrado, servidor de voz cambiado, fin de llamada, etc.). No debe reconectar. |

### HTTP <a href="#http" id="http"></a>

La API devolverá códigos de respuesta HTTP semánticamente válidos según el éxito de tu solicitud. La siguiente tabla puede utilizarse como referencia para los códigos más comunes que se retornan.

#### Códigos de respuesta HTTP

<sup>1</sup> Los errores 502, 504, 507, 522, 523 y 524 deben reintentarse con backoff. El número de fallos puede estar en la cabecera `X-Failed-Requests`. Retentar otros 5xx queda a tu criterio.

| Código                      | Significado                                                                                               |
| --------------------------- | --------------------------------------------------------------------------------------------------------- |
| 200 (OK)                    | La solicitud se completó exitosamente.                                                                    |
| 201 (CREATED)               | La entidad fue creada exitosamente.                                                                       |
| 202 (ACCEPTED)              | La solicitud fue aceptada pero el recurso no está listo. Debe haber una clave `retry_after` en el cuerpo. |
| 204 (NO CONTENT)            | La solicitud se completó correctamente pero no devolvió contenido.                                        |
| 304 (NOT MODIFIED)          | La entidad no se modificó (no se realizó ninguna acción).                                                 |
| 400 (BAD REQUEST)           | La solicitud tenía formato incorrecto o el servidor no pudo entenderla.                                   |
| 401 (UNAUTHORIZED)          | Faltaba la cabecera `Authorization` o era inválida.                                                       |
| 403 (FORBIDDEN)             | No tienes permiso para realizar la acción solicitada.                                                     |
| 404 (NOT FOUND)             | El recurso en la ubicación especificada no existe.                                                        |
| 405 (METHOD NOT ALLOWED)    | El método HTTP usado no es válido para ese recurso.                                                       |
| 409 (CONFLICT)              | No se pudo actualizar el recurso solicitado por el estado actual del recurso objetivo.                    |
| 410 (GONE)                  | El recurso al que se accedía ya no está disponible.                                                       |
| 413 (PAYLOAD TOO LARGE)     | La entidad de la solicitud era demasiado grande.                                                          |
| 415 (UNSUPPORTED MEDIA)     | El tipo de contenido solicitado no se admite (solo CDN).                                                  |
| 422 (UNPROCESSABLE CONTENT) | La solicitud estaba bien formada pero falló la validación (solo servicios Rust).                          |
| 429 (TOO MANY REQUESTS)     | Estás limitado por frecuencia.                                                                            |
| 501 (NOT IMPLEMENTED)       | El método HTTP solicitado no está soportado.                                                              |
| 502 (GATEWAY UNAVAILABLE)   | No había gateway disponible para tu solicitud. Espera y reintenta.                                        |
| 503 (SERVICE UNAVAILABLE)   | El servicio requerido no está disponible. Espera y reintenta.                                             |
| 504 (GATEWAY TIMEOUT)       | El gateway que procesaba tu solicitud expiró. Espera y reintenta.                                         |
| 5xx (SERVER ERROR) 1        | El servidor tuvo un error al procesar tu solicitud.                                                       |

### JSON <a href="#json" id="json"></a>

Además del código HTTP, la API también puede devolver códigos de error más detallados a través de la clave `code` en la respuesta JSON de error. También incluirá una clave `message` con una cadena de error legible. Algunos de estos errores pueden llevar información adicional en un objeto `errors`.

{% hint style="info" %}
Un código especial JSON de error `40333` se devuelve si tu solicitud es bloqueada por Cloudflare. Esto puede deberse a una solicitud malformada o un user agent incorrecto. La respuesta será similar a un error normal, pero no la emite directamente la API:

```python
{
  "message": "internal network error",
  "code": 40333
}
```
{% endhint %}

#### Ejemplo de respuesta JSON de error

```python
{
  "message": "Invalid authentication token",
  "code": 50014
}
```

{% hint style="danger" %}
Actualmente estamos manteniendo esta página en [codigos-de-error.md](../datamining/codigos-de-error.md "mention"). ¿Echas algo en falta? ¡Ayúdanos subiendo los errores!
{% endhint %}

---
icon: reply-clock
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

# Recepción & Respuesta

## Interacciones <a href="#interacciones" id="interacciones"></a>

Una **Interacción** es el mensaje que tu aplicación recibe cuando un usuario usa un comando de aplicación o un componente de mensaje.

Para comandos de barra, incluye los valores que el usuario envió.

Para comandos de usuario y comandos de mensaje, incluye el usuario o mensaje resuelto en el que se tomó la acción.

Para componentes de mensaje incluye información de identificación sobre el componente que se usó. También incluirá algunos metadatos sobre cómo se activó la interacción: el `guild_id`, `channel_id`, `member` y otros campos. Puedes encontrar todos los valores en nuestros modelos de datos.

### Modelos de datos y tipos <a href="#modelos-de-datos-y-tipos" id="modelos-de-datos-y-tipos"></a>

### Objeto de interacción

#### Estructura de interacción

<sup>1</sup> Esto siempre está presente en tipos de interacción de comando de aplicación y componente de mensaje. Es opcional para pruebas futuras contra nuevos tipos de interacción.

<sup>2</sup> `member` se envía cuando la interacción se invoca en un servidor, y `user` se envía cuando se invoca en un MD.

| Campo           | Tipo                | Descripción                                                                    |
| --------------- | ------------------- | ------------------------------------------------------------------------------ |
| id              | snowflake           | ID de la interacción.                                                          |
| application\_id | snowflake           | ID de la aplicación para la que es esta interacción.                           |
| type            | interaction type    | El tipo de interacción.                                                        |
| data? 1         | interaction data    | La carga útil de datos del comando.                                            |
| guild\_id?      | snowflake           | El servidor desde el que se envió.                                             |
| channel\_id?    | snowflake           | El canal desde el que se envió.                                                |
| member? 2       | guild member object | Datos de miembro del servidor para el usuario que invoca, incluyendo permisos. |
| user?           | partial user object | Objeto de usuario para el usuario que invoca, si se invocó en un MD.           |
| token           | string              | Un token de continuación para responder a la interacción.                      |
| version         | integer             | Propiedad de solo lectura, siempre `1`.                                        |
| message?        | message object      | Para componentes, el mensaje al que estaban adjuntos.                          |

#### Tipo de interacción

| Nombre               | Valor |
| -------------------- | ----- |
| PING                 | 1     |
| APPLICATION\_COMMAND | 2     |
| MESSAGE\_COMPONENT   | 3     |

#### Estructura de datos de interacción

| Campo            | Tipo                                                 | Descripción                                                           | Tipo de interacción           |
| ---------------- | ---------------------------------------------------- | --------------------------------------------------------------------- | ----------------------------- |
| id               | snowflake                                            | El `ID` del comando invocado.                                         | Application Command           |
| name             | string                                               | El `name` del comando invocado.                                       | Application Command           |
| type             | integer                                              | El `type` del comando invocado.                                       | Application Command           |
| resolved?        | resolved data                                        | Usuarios + roles + canales convertidos.                               | Application Command           |
| options?         | array of application command interaction data option | Los parámetros + valores del usuario.                                 | Application Command           |
| custom\_id?      | string                                               | El `custom_id` del componente.                                        | Component                     |
| component\_type? | integer                                              | El tipo del componente.                                               | Component                     |
| values?          | array of select option values                        | Los valores que el usuario seleccionó.                                | Component (Select)            |
| target\_id?      | snowflake                                            | ID del usuario o mensaje objetivo de un comando de usuario o mensaje. | User Command, Message Command |

#### Estructura de datos resueltos

{% hint style="info" %}
Si se incluye información de un miembro, también se incluirá la información correspondiente de su usuario.
{% endhint %}

<sup>1</sup> Los objetos `Member` parciales faltan los campos `user`, `deaf` y `mute`.

<sup>2</sup> Los objetos `Channel` parciales solo tienen campos `id`, `name`, `type` y `permissions`. Los hilos también tendrán campos `thread_metadata` y `parent_id`.

| Campo       | Tipo                                          | Descripción                             |
| ----------- | --------------------------------------------- | --------------------------------------- |
| users?      | Map of Snowflakes to partial user objects     | Los IDs y objetos de usuario.           |
| members? 1  | Map of Snowflakes to partial member objects   | Los IDs y objetos de miembro parciales. |
| roles?      | Map of Snowflakes to role objects             | Los IDs y objetos de rol.               |
| channels? 2 | Map of Snowflakes to partial channel objects  | Los IDs y objetos de canal parciales.   |
| messages?   | Map of Snowflakes to partial messages objects | Los IDs y objetos de mensaje parciales. |

### Objeto de interacción de mensaje

Esto se envía en el objeto de mensaje cuando el mensaje es una respuesta a una interacción sin un mensaje existente.

{% hint style="info" %}
Esto significa que las respuestas a los componentes de mensaje no incluyen esta propiedad; en su lugar, incluyen un objeto de referencia de mensaje, ya que los componentes siempre existen en mensajes preexistentes.
{% endhint %}

#### Estructura de interacción de mensaje

| Nombre | Tipo                | Descripción                           |
| ------ | ------------------- | ------------------------------------- |
| id     | snowflake           | ID de la interacción.                 |
| type   | interaction type    | El tipo de interacción.               |
| name   | string              | El nombre del comando de aplicación.  |
| user   | partial user object | El usuario que invocó la interacción. |

### Interacciones y usuarios bot <a href="#interacciones-y-usuarios-bot" id="interacciones-y-usuarios-bot"></a>

Todos estamos acostumbrados a la forma en que los bots de Discord han funcionado durante mucho tiempo. Haces una aplicación en el Portal de Desarrolladores, le añades un usuario bot, y copias el token. Ese token puede usarse para conectarse al gateway y hacer solicitudes contra nuestra API.

Las interacciones traen algo completamente nuevo a la mesa: la capacidad de interactuar con una aplicación _sin necesidad de un usuario bot en el servidor_. Mientras lees esta documentación, verás que los tokens de bot solo se referencian como una alternativa útil para hacer un flujo de autenticación de credenciales de cliente. Responder a interacciones no requiere un token de bot.

En muchos casos, aún puedes necesitar un usuario bot. Si necesitas recibir eventos del gateway, o necesitas interactuar con otras partes de nuestra API (como obtener un servidor, o un canal, o actualizar permisos en un usuario), esas acciones están todas aún vinculadas a tener un token de bot. Sin embargo, si no necesitas ninguna de esas cosas, nunca tienes que añadir un usuario bot a tu aplicación en absoluto.

Bienvenido al nuevo mundo.

### Recibiendo una interacción <a href="#recibiendo-una-interaccin" id="recibiendo-una-interaccin"></a>

Cuando un usuario interactúa con tu aplicación, tu aplicación recibirá una **Interacción**. Tu aplicación puede recibir una interacción de una de dos maneras:

* Vía evento Gateway Interaction Create
* Vía webhook saliente

Estos dos métodos son **mutuamente exclusivos**; _solo_ puedes recibir interacciones de una de las dos maneras. El evento Gateway `INTERACTION_CREATE` puede ser manejado por clientes conectados, mientras que el método webhook detallado a continuación no requiere un cliente conectado.

En tu aplicación en el Portal de Desarrolladores, hay un campo en la página principal llamado "Interactions Endpoint URL". Si quieres recibir interacciones vía webhook saliente, puedes establecer tu URL en este campo. Para que la URL sea válida, debes estar preparado para dos cosas antes de tiempo:

{% hint style="info" %}
Estos pasos solo son necesarios para las interacciones basadas en webhooks. No se requiere para recibirlas a través del gateway.
{% endhint %}

1. Tu endpoint debe estar preparado para ACK un mensaje `PING`
2. Tu endpoint debe estar configurado para manejar adecuadamente encabezados de firma, más sobre eso en Seguridad y Autorización

Si cualquiera de estos no está completo, no validaremos tu URL y fallará al guardar.

Cuando intentas guardar una URL, enviaremos una solicitud `POST` a esa URL con una carga útil `PING`. La carga útil `PING` tiene un `type: 1`. Así que, para ACK adecuadamente la carga útil, devuelve una respuesta `200` con una carga útil de `type: 1`:

```python
@app.route('/', methods=['POST'])
def my_command():
    if request.json["type"] == 1:
        return jsonify({
            "type": 1
        })
```

También necesitarás configurar adecuadamente Seguridad y Autorización en tu endpoint para que la URL sea aceptada. Una vez que ambos estén completos y tu URL haya sido guardada, ¡puedes comenzar a recibir interacciones vía webhook! En este punto, tu aplicación **ya no recibirá interacciones sobre el gateway**. Si quieres recibirlas sobre el gateway nuevamente, simplemente elimina tu URL.

Una interacción incluye metadatos para ayudar a tu aplicación a manejarla así como `data` específicos del tipo de interacción. Puedes encontrar muestras para cada tipo de interacción en sus páginas respectivas:

* Comandos de barra
* Comandos de usuario
* Comandos de mensaje
* Componentes de mensaje

Una explicación de todos los campos se puede encontrar en nuestros modelos de datos.

Ahora que tienes los datos del usuario, es hora de responderles.

### Respondiendo a una interacción <a href="#respondiendo-a-una-interaccin" id="respondiendo-a-una-interaccin"></a>

Las interacciones, tanto recibir como responder, son webhooks bajo el capó. ¡Así que responder a una interacción es como enviar una solicitud de webhook!

Hay varias maneras en que puedes responder a una interacción:

### Objeto de respuesta de interacción

#### Estructura de respuesta de interacción

| Campo | Tipo                      | Descripción                       |
| ----- | ------------------------- | --------------------------------- |
| type  | interaction callback type | El tipo de respuesta.             |
| data? | interaction callback data | Un mensaje de respuesta opcional. |

#### Tipo de callback de interacción

<sup>1</sup> Solo válido para interacciones basadas en componentes

| Nombre                                   | Valor | Descripción                                                                                                      |
| ---------------------------------------- | ----- | ---------------------------------------------------------------------------------------------------------------- |
| PONG                                     | 1     | ACK un `Ping`.                                                                                                   |
| CHANNEL\_MESSAGE\_WITH\_SOURCE           | 4     | Responder a una interacción con un mensaje.                                                                      |
| DEFERRED\_CHANNEL\_MESSAGE\_WITH\_SOURCE | 5     | ACK una interacción y editar una respuesta después, el usuario ve un estado de carga.                            |
| DEFERRED\_UPDATE\_MESSAGE 1              | 6     | Para componentes, ACK una interacción y editar el mensaje original después; el usuario no ve un estado de carga. |
| UPDATE\_MESSAGE 1                        | 7     | Para componentes, editar el mensaje al que el componente estaba adjunto.                                         |

### Estructura de datos de callback de interacción

No todos los campos de mensaje están actualmente soportados.

<sup>1</sup> Ver `subiendo archivos` para más detalles.

| Nombre             | Tipo                                | Descripción                                             |
| ------------------ | ----------------------------------- | ------------------------------------------------------- |
| tts?               | boolean                             | Si la respuesta es TTS.                                 |
| content?           | string                              | Contenido del mensaje.                                  |
| embeds?            | array of embeds                     | Soporta hasta 10 embeds.                                |
| allowed\_mentions? | allowed mentions                    | Objeto de menciones permitidas.                         |
| flags?             | integer                             | Banderas de datos de callback de interacción.           |
| components?        | array of components                 | Componentes de mensaje.                                 |
| attachments? 1     | array of partial attachment objects | Objetos de adjunto con nombre de archivo y descripción. |

#### Banderas de datos de callback de interacción

| Nombre    | Valor  | Descripción                                        |
| --------- | ------ | -------------------------------------------------- |
| EPHEMERAL | 1 << 6 | Solo el usuario que recibe el mensaje puede verlo. |

{% hint style="warning" %}
Aunque las respuestas e interacciones posteriores son _webhooks_, respetan la capacidad de @everyone para mencionar a @everyone / @here. No obstante, si tu aplicación responde con datos de usuario, aún debes usar `allowed_mentions` para filtrar qué menciones en el contenido generan realmente una notificación. Otras diferencias incluyen la posibilidad de enviar enlaces con nombre en el contenido del mensaje (`[texto](url)`).
{% endhint %}

Cuando respondes a una interacción recibida **vía webhook**, tu servidor puede simplemente responder a la solicitud `POST` recibida. Querrás responder con un código de estado `200` (si todo salió bien), así como especificar un `type` y `data`, que es un objeto de respuesta de interacción:

```python
@app.route('/', methods=['POST'])
def my_command():
    if request.json["type"] == 1:
        return jsonify({
            "type": 1
        })

    else:
        return jsonify({
            "type": 4,
            "data": {
                "tts": False,
                "content": "Congrats on sending your command!",
                "embeds": [],
                "allowed_mentions": { "parse": [] }
            }
        })
```

Si estás recibiendo interacciones sobre el gateway, también **necesitarás responder vía HTTP**. Las respuestas a interacciones **no se envían como comandos sobre el gateway**.

Para responder a una interacción del gateway, haz una solicitud `POST` así. `interaction_id` es el ID único de esa interacción individual del payload recibido. `interaction_token` es el token único para esa interacción del payload recibido. **Este endpoint solo es válido para interacciones recibidas sobre el gateway. De otra manera, responde a la solicitud** `POST` **para emitir una respuesta inicial.**

```python
url = "https://discord.com/api/v8/interactions/<interaction_id>/<interaction_token>/callback"

json = {
    "type": 4,
    "data": {
        "content": "Congrats on sending your command!"
    }
}
r = requests.post(url, json=json)
```

{% hint style="info" %}
Los `tokens` de interacción son válidos durante **15 minutos** y pueden utilizarse para enviar mensajes de seguimiento, pero **debes enviar una respuesta inicial dentro de los 3 segundos posteriores a la recepción del evento**. Si se supera el límite de 3 segundos, el token será invalidado.
{% endhint %}

### Mensajes de seguimiento <a href="#mensajes-de-seguimiento" id="mensajes-de-seguimiento"></a>

A veces, tu bot querrá enviar mensajes de seguimiento a un usuario después de responder a una interacción. O, puedes querer editar tu respuesta original. Ya sea que recibas interacciones sobre el gateway o por webhook saliente, puedes usar los siguientes endpoints para editar tu respuesta inicial o enviar mensajes de seguimiento:

* <mark style="color:blue;">`PATCH /webhooks/<application_id>/<interaction_token>/messages/@original`</mark> para editar tu respuesta inicial a una interacción.
* <mark style="color:blue;">`DELETE /webhooks/<application_id>/<interaction_token>/messages/@original`</mark> para eliminar tu respuesta inicial a una interacción.
* <mark style="color:blue;">`POST /webhooks/<application_id>/<interaction_token>`</mark> para enviar un nuevo mensaje de seguimiento.
* <mark style="color:blue;">`PATCH /webhooks/<application_id>/<interaction_token>/messages/<message_id>`</mark> para editar un mensaje enviado con ese `token`.

{% hint style="info" %}
Los webhooks de interacciones comparten las mismas propiedades de límite de velocidad que los webhooks normales.
{% endhint %}

Los tokens de interacción son válidos por **15 minutos**, lo que significa que puedes responder a una interacción dentro de esa cantidad de tiempo.

### Seguridad y autorización <a href="#seguridad-y-autorizacin" id="seguridad-y-autorizacin"></a>

Internet es un lugar aterrador, especialmente para personas que hospedan endpoints abiertos y no autenticados. Si estás recibiendo interacciones vía webhook saliente, hay algunos pasos de seguridad que **debes** tomar antes de que tu aplicación sea elegible para recibir solicitudes.

Cada interacción se envía con los siguientes encabezados:

* `X-Signature-Ed25519` como una firma.
* `X-Signature-Timestamp` como una marca de tiempo.

Usando tu biblioteca de seguridad favorita, **debes validar la solicitud cada vez que recibas una interacción**. Si la firma falla la validación, responde con un código de error `401`. Aquí hay un par de ejemplos de código:

{% tabs %}
{% tab title="Primer ejemplo" %}
```python
const nacl = require("tweetnacl");

// Your public key can be found on your application in the Developer Portal
const PUBLIC_KEY = "APPLICATION_PUBLIC_KEY";

const signature = req.get("X-Signature-Ed25519");
const timestamp = req.get("X-Signature-Timestamp");
const body = req.rawBody; // rawBody is expected to be a string, not raw bytes

const isVerified = nacl.sign.detached.verify(
  Buffer.from(timestamp + body),
  Buffer.from(signature, "hex"),
  Buffer.from(PUBLIC_KEY, "hex"),
);

if (!isVerified) {
  return res.status(401).end("invalid request signature");
}
```
{% endtab %}

{% tab title="Segundo ejemplo" %}
```python
from nacl.signing import VerifyKey
from nacl.exceptions import BadSignatureError

# Your public key can be found on your application in the Developer Portal
PUBLIC_KEY = 'APPLICATION_PUBLIC_KEY'

verify_key = VerifyKey(bytes.fromhex(PUBLIC_KEY))

signature = request.headers["X-Signature-Ed25519"]
timestamp = request.headers["X-Signature-Timestamp"]
body = request.data.decode("utf-8")

try:
    verify_key.verify(f'{timestamp}{body}'.encode(), bytes.fromhex(signature))
except BadSignatureError:
    abort(401, 'invalid request signature')
```
{% endtab %}
{% endtabs %}

Si no estás validando adecuadamente este encabezado de firma, no te permitiremos guardar tu URL de interacciones en el Portal de Desarrolladores. También haremos verificaciones de seguridad automatizadas y rutinarias contra tu endpoint, incluyendo enviarte intencionalmente firmas inválidas. Si fallas la validación, eliminaremos tu URL de interacciones en el futuro y te alertaremos vía correo electrónico y MD del sistema.

### Endpoints <a href="#endpoints" id="endpoints"></a>

{% hint style="info" %}
Para la autorización, todos los endpoints aceptan un token de bot o un token de credenciales de cliente para tu aplicación.
{% endhint %}

#### Crear respuesta de interacción

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /interactions/{interaction.id}/{interaction.token}/callback

`No autenticado`

Crea una respuesta a una interacción desde el gateway. Toma una respuesta de interacción.

Este endpoint también soporta adjuntos de archivos similar a los endpoints de webhook. Consulta subiendo archivos para detalles sobre subir archivos y solicitudes `multipart/form-data`.

#### Obtener respuesta de interacción original

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /webhooks/{application.id}/{interaction.token}/messages/@original

`No autenticado`

Devuelve la respuesta de interacción inicial. Funciona igual que obtener mensaje de webhook.

#### Editar respuesta de interacción original

<kbd><mark style="color:$warning;background-color:$warning;">PATCH<mark style="color:$warning;background-color:$warning;"></kbd> /webhooks/{application.id}/{interaction.token}/messages/@original

`No autenticado`

Edita la respuesta de interacción inicial. Funciona igual que editar mensaje de webhook.

#### Eliminar respuesta de interacción original

<kbd><mark style="color:$danger;background-color:$danger;">DELETE<mark style="color:$danger;background-color:$danger;"></kbd> /webhooks/{application.id}/{interaction.token}/messages/@original

`No autenticado`

Elimina la respuesta de interacción inicial. Devuelve `204` en caso de éxito.

#### Crear mensaje de seguimiento

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /webhooks/{application.id}/{interaction.token}

`No autenticado`

Crea un mensaje de seguimiento para una interacción. Funciona igual que ejecutar webhook, pero `wait` siempre es true, y `flags` puede establecerse a `64` en el cuerpo para enviar un mensaje efímero. El parámetro de consulta `thread_id` no es requerido (y además se ignora) cuando se usa este endpoint para seguimientos de interacción.

#### Obtener mensaje de seguimiento

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /webhooks/{application.id}/{interaction.token}/messages/{message.id}

`No autenticado`

Devuelve un mensaje de seguimiento para una interacción. Funciona igual que obtener mensaje de webhook. No soporta seguimientos efímeros.

#### Editar mensaje de seguimiento

<kbd><mark style="color:$warning;background-color:$warning;">PATCH<mark style="color:$warning;background-color:$warning;"></kbd> /webhooks/{application.id}/{interaction.token}/messages/{message.id}

`No autenticado`

Edita un mensaje de seguimiento para una interacción. Funciona igual que editar mensaje de webhook. No soporta seguimientos efímeros.

#### Eliminar mensaje de seguimiento

<kbd><mark style="color:$danger;background-color:$danger;">DELETE<mark style="color:$danger;background-color:$danger;"></kbd> /webhooks/{application.id}/{interaction.token}/messages/{message.id}

`No autenticado`

Elimina un mensaje de seguimiento para una interacción. Devuelve `204` en caso de éxito. No soporta seguimientos efímeros.

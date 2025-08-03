---
icon: arrow-right-to-bracket
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

# OAuth2

OAuth2 permite a los desarrolladores de aplicaciones crear aplicaciones que utilicen autenticación y datos de la API de Discord. Dentro de Discord, existen múltiples tipos de autenticación OAuth2. Las subvenciones admitidas incluyen la subvención de código de autorización, subvención implícita, subvención de dispositivo, credenciales de cliente y algunos flujos especiales modificados para Discord para bots y webhooks.

### Recursos Compartidos <a href="#recursos-compartidos" id="recursos-compartidos"></a>

El primer paso para implementar OAuth2 es registrar una aplicación de desarrollador y recuperar tu ID de cliente y secreto de cliente. La mayoría de las personas que implementarán OAuth2 querrán encontrar y utilizar una biblioteca en el lenguaje de su elección. Para aquellos que implementen OAuth2 desde cero, consulte RFC 6749 para obtener detalles. Después de crear tu aplicación con Discord, asegúrate de tener tu `client_id` y `client_secret` a mano. El siguiente paso es determinar qué flujo OAuth2 es el adecuado para tus propósitos.

#### Enlaces de OAuth2

| Enlaces                                                                                                    | Descripción                                   |
| ---------------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| [https://discord.com/oauth2/authorize](https://discord.com/oauth2/authorize)                               | URL de autorización.                          |
| [https://discord.com/api/v10/oauth2/authorize/device](https://discord.com/api/v10/oauth2/authorize/device) | URL de autorización de código de dispositivo. |
| [https://discord.com/api/v10/oauth2/token](https://discord.com/api/v10/oauth2/token)                       | URL de token.                                 |
| [https://discord.com/api/v10/oauth2/token/revoke](https://discord.com/api/v10/oauth2/token/revoke)         | URL de revocación de token.                   |
| [https://discord.com/api/v10/oauth2/keys](https://discord.com/api/v10/oauth2/keys)                         | URI JWKS.                                     |
| [https://discord.com/api/v10/oauth2/userinfo](https://discord.com/api/v10/oauth2/userinfo)                 | URL de información de usuario.                |

{% hint style="warning" %}
De acuerdo con las RFCs correspondientes, las URLs de token y revocación de token solo aceptarán un tipo de contenido `x-www-form-urlencoded`. El contenido en formato JSON no está permitido y devolverá un error.
{% endhint %}

#### Ámbitos OAuth2

Estos son todos los ámbitos OAuth2 que Discord admite. Algunos ámbitos requieren aprobación de Discord para usar. Solicitarlos a un usuario sin aprobación de Discord llevará a un comportamiento de error inesperado en el flujo OAuth2.

{% hint style="info" %}
`bot` y `guilds.join` requieren que tengas una cuenta de bot vinculada a tu aplicación. Además, para poder añadir a un usuario a un servidor, tu bot debe pertenecer previamente a dicho servidor.
{% endhint %}

<sup>1</sup> En un contexto de instalación de usuario, este ámbito también permite que la aplicación envíe MD al usuario.

<sup>2</sup> Este ámbito solo está disponible a través del flujo de subvención de credenciales de cliente.

<sup>3</sup> Depende de que el ámbito `identify` también esté autorizado.

<sup>4</sup> Este ámbito solo está disponible a través del flujo de subvención de código de autorización y requiere que la bandera de aplicación `PUBLIC_OAUTH2_CLIENT` no esté establecida.

<sup>5</sup> A menos que la aplicación esté aprobada para acceso RPC general, el ámbito `rpc` está permitido solo para el propietario de la aplicación y usuarios en lista blanca y requiere que la bandera de aplicación `EMBEDDED` no esté establecida.

<sup>6</sup> El acceso a RPC para aplicaciones web requiere aprobación de Discord.

<sup>7</sup> También incluye privilegios `gateway.connect`.

<sup>8</sup> Este ámbito solo está disponible a través del flujo de subvención de código de autorización.

| Valor                                    | Descripción                                                                                                                                    | Público |
| ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| account.global\_name.update              | Permite actualizar el nombre global del usuario.                                                                                               | No      |
| activities.invites.write                 | Permite enviar invitaciones de actividad.                                                                                                      | No      |
| activities.read                          | Permite recuperar datos de presencia y actividad del usuario.                                                                                  | No      |
| activities.write                         | Permite actualizar la presencia del usuario y crear sesiones sin cabeza.                                                                       | No      |
| applications.builds.read                 | Permite leer datos de rama y compilación para las aplicaciones del usuario.                                                                    | Sí      |
| applications.builds.upload               | Permite cargar compilaciones a las aplicaciones del usuario.                                                                                   | No      |
| applications.commands 1                  | Permite usar comandos en un contexto de servidor/usuario.                                                                                      | Sí      |
| applications.commands.permissions.update | Permite actualizar los permisos de comandos propios de la aplicación en servidores donde el usuario tiene permisos.                            | Sí      |
| applications.commands.update 2           | Permite que tu aplicación actualice sus propios comandos.                                                                                      | Sí      |
| applications.entitlements                | Permite gestionar derechos para las aplicaciones del usuario.                                                                                  | Sí      |
| applications.store.update                | Permite gestionar datos de tienda (SKU, listados de tienda, logros, etc.) para las aplicaciones del usuario.                                   | Sí      |
| bot                                      | Añade el bot de la aplicación a un servidor seleccionado por el usuario.                                                                       | Sí      |
| connections                              | Permite recuperar las cuentas conectadas de un usuario, tanto públicas como privadas.                                                          | Sí      |
| dm\_channels.read                        | Permite leer información sobre los MD y MD grupales del usuario.                                                                               | No      |
| dm\_channels.messages.read               | Permite leer mensajes de los MD y MD grupales del usuario.                                                                                     | No      |
| dm\_channels.messages.write              | Permite enviar mensajes a los MD del usuario.                                                                                                  | No      |
| email 3                                  | Permite recuperar la dirección de correo electrónico de un usuario.                                                                            | Sí      |
| gateway.connect 3                        | Permite conectarse al gateway en nombre del usuario.                                                                                           | No      |
| gdm.join                                 | Permite añadir usuarios a MD grupales gestionados.                                                                                             | Sí      |
| guilds                                   | Permite recuperar los servidores del usuario.                                                                                                  | Sí      |
| guilds.channels.read                     | Permite leer los canales en los servidores de un usuario.                                                                                      | No      |
| guilds.join                              | Permite unir usuarios a un servidor.                                                                                                           | Sí      |
| guilds.members.read                      | Permite recuperar información de miembro de un usuario en un servidor.                                                                         | Sí      |
| identify                                 | Permite recuperar el usuario actual.                                                                                                           | Sí      |
| lobbies.write                            | Permite gestionar lobbies.                                                                                                                     | No      |
| messages.read                            | Al usar RPC, permite leer mensajes de todos los canales del cliente (de lo contrario restringido a MD grupales gestionados por la aplicación). | Sí      |
| openid                                   | Permite recuperar información básica del usuario e incluye un token de ID en el intercambio de tokens.                                         | Sí      |
| payment\_sources.country\_code           | Permite recuperar el código de país del usuario.                                                                                               | No      |
| presences.read                           | Permite recuperar la presencia del usuario.                                                                                                    | No      |
| presences.write                          | Permite actualizar la presencia del usuario.                                                                                                   | No      |
| relationships.read                       | Permite recuperar las relaciones de un usuario.                                                                                                | No      |
| relationships.write                      | Permite gestionar las relaciones de un usuario.                                                                                                | No      |
| role\_connections.write 4                | Permite actualizar la conexión de un usuario y metadatos específicos de la aplicación.                                                         | Sí      |
| rpc 5 6                                  | Al usar RPC, permite controlar el cliente local de Discord; también abarca todos los ámbitos RPC a continuación en la mayoría de escenarios.   | No      |
| rpc.activities.write 6                   | Al usar RPC, permite actualizar la actividad de un usuario.                                                                                    | Sí      |
| ~~rpc.api~~                              | ~~Permite acceder a la API REST en nombre del usuario.~~                                                                                       | ~~No~~  |
| rpc.notifications.read 6                 | Al usar RPC, permite recibir notificaciones enviadas al usuario.                                                                               | Sí      |
| rpc.screenshare.read 6                   | Al usar RPC, permite leer el estado de compartir pantalla de un usuario.                                                                       | Sí      |
| rpc.screenshare.write 6                  | Al usar RPC, permite actualizar la configuración de compartir pantalla de un usuario.                                                          | Sí      |
| rpc.video.read 6                         | Al usar RPC, permite leer el estado de vídeo de un usuario.                                                                                    | Sí      |
| rpc.video.write 6                        | Al usar RPC, permite actualizar la configuración de vídeo de un usuario.                                                                       | Sí      |
| rpc.voice.read 6                         | Al usar RPC, permite leer la configuración de voz de un usuario y escuchar eventos de voz.                                                     | Sí      |
| rpc.voice.write 6                        | Al usar RPC, permite actualizar la configuración de voz de un usuario.                                                                         | Sí      |
| voice 3 7                                | Permite conectarse a voz en nombre del usuario y ver todos los miembros de voz en un servidor.                                                 | No      |
| webhook.incoming 8                       | Crea un webhook propiedad de la aplicación en un canal seleccionado por el usuario y lo devuelve en el intercambio de tokens.                  | Sí      |

### Ámbitos OAuth2 Paraguas

Los siguientes ámbitos se consideran "paraguas", lo que significa que se utilizan para solicitar acceso a múltiples ámbitos a la vez. Actualmente, estos solo son utilizados por la integración de capa social.

| Valor                       | Descripción                                                                                                                                                                          |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| sdk.social\_layer\_presence | Incluye los ámbitos `activities.invites.write`, `activities.read`, `activities.write`, `gateway.connect`, `identify`, `relationships.read`, y `relationships.write`.                 |
| sdk.social\_layer           | Incluye todo en `sdk.social_layer_presence`, más `dm_channels.read`, `dm_channels.messages.read`, `dm_channels.messages.write`, `guilds`, `guilds.channels.read`, y `lobbies.write`. |

#### Estructura de enlace de autorización

<sup>1</sup> Requerido a menos que se use el flujo de autorización básico de bot.

<sup>2</sup> Si el ámbito `bot` está seleccionado, el ámbito `applications.commands` se añade automáticamente a la autorización por los clientes oficiales.

<sup>3</sup> Si se especifica un `response_type` y no se especifica `redirect_uri`, el usuario será redirigido a la primera URI de redirección registrada para la aplicación.

<sup>4</sup> Solo acepta `true` o `false`.

| Campo                    | Tipo      | Descripción                                                                                                                                                                                                      |
| ------------------------ | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| client\_id               | snowflake | El ID de la aplicación.                                                                                                                                                                                          |
| response\_type? 1        | string    | El tipo de respuesta a devolver.                                                                                                                                                                                 |
| scope? 2                 | string    | Una lista delimitada por espacios de ámbitos a solicitar; puede omitirse si la aplicación tiene un `integration_types_config` poblado.                                                                           |
| redirect\_uri? 3         | string    | La URL a la cual redirigir después de la autorización; debe coincidir con una de las URI de redirección registradas para la aplicación.                                                                          |
| prompt?                  | string    | El comportamiento de solicitud a usar para el flujo de autorización (por defecto `consent`).                                                                                                                     |
| state?                   | string    | Una cadena única para vincular la solicitud del usuario a su estado autenticado.                                                                                                                                 |
| nonce?                   | string    | Una cadena única para vincular la solicitud del usuario a su estado autenticado; solo aplicable para subvenciones de código de autorización con el ámbito `openid`.                                              |
| code\_challenge?         | string    | Un desafío de código para la extensión PKCE de la subvención de código de autorización; debe usarse con `code_challenge_method`.                                                                                 |
| code\_challenge\_method? | string    | El método usado para generar el desafío de código (debe ser `S256`); solo aplicable para la extensión PKCE de la subvención de código de autorización.                                                           |
| integration\_type?       | integer   | El contexto de instalación para la autorización; solo aplicable cuando `scope` contiene `applications.commands` (por defecto `GUILD_INSTALL`).                                                                   |
| permissions?             | integer   | Los permisos que estás solicitando; solo aplicable cuando `scope` contiene `bot`.                                                                                                                                |
| guild\_id?               | snowflake | El ID de un servidor para prellenar el selector desplegable; solo aplicable cuando `scope` contiene `bot`, `applications.commands`, o `webhook.incoming` y `integration_type` es `GUILD_INSTALL`.                |
| channel\_id?             | snowflake | El ID de un canal para prellenar el selector desplegable; solo aplicable cuando `scope` contiene `webhook.incoming`.                                                                                             |
| disable\_guild\_select?  | boolean 4 | Prohíbe al usuario cambiar el desplegable de servidor; solo aplicable cuando `scope` contiene `bot` o `applications.commands`, o `webhook.incoming` y `integration_type` es `GUILD_INSTALL` (por defecto false). |

#### Tipo de respuesta

| Valor | Descripción                                    |
| ----- | ---------------------------------------------- |
| code  | Flujo de subvención de código de autorización. |
| token | Flujo de subvención implícita.                 |

#### Comportamiento de solicitud

| Valor   | Descripción                                                                                                                       |
| ------- | --------------------------------------------------------------------------------------------------------------------------------- |
| none    | Omite la pantalla de autorización y redirige inmediatamente al usuario; requiere autorización previa con los ámbitos solicitados. |
| consent | Solicita al usuario que reapruebe su autorización.                                                                                |

### Estado y Seguridad <a href="#estado-y-seguridad" id="estado-y-seguridad"></a>

Antes de sumergirnos en la semántica de las diferentes subvenciones OAuth2, debemos detenernos y discutir la seguridad, específicamente el uso del parámetro `state`. La Falsificación de Solicitud entre Sitios, o CSRF, y el Clickjacking son vulnerabilidades de seguridad que deben ser abordadas por individuos que implementen OAuth. Esto típicamente se logra usando el parámetro `state`. `state` se envía en la solicitud de autorización y se devuelve en la respuesta y debe ser un valor que vincule la solicitud del usuario a su estado autenticado. Por ejemplo, `state` podría ser un hash de la cookie de sesión del usuario, o algún otro nonce que pueda vincularse a la sesión del usuario.

Cuando un usuario comienza un flujo de autorización en el cliente, se genera un `state` que es único para la solicitud de ese usuario. Este valor se almacena en algún lugar solo accesible para el cliente y el usuario, es decir, protegido por la política de mismo origen. Cuando el usuario es redirigido, se devuelve el parámetro `state`. El cliente valida la solicitud verificando que el `state` devuelto coincida con el valor almacenado. Si coinciden, es una solicitud de autorización válida. Si no coinciden, es posible que alguien interceptó la solicitud o de alguna manera se autorizó falsamente a los recursos de otro usuario, y la solicitud debe ser denegada.

Para OpenID Connect, el parámetro `nonce` puede usarse de manera similar en su lugar. Al recibir el token de ID, el cliente debe validar que la reclamación `nonce` en el token de ID coincida con el `nonce` enviado en la solicitud de autorización.

Aunque Discord no requiere el uso del parámetro `state`, recomendamos encarecidamente que lo implementes para la seguridad de tus propias aplicaciones y datos.

#### Autorización de código garantizado <a href="#subvencin-de-cdigo-de-autorizacin" id="subvencin-de-cdigo-de-autorizacin"></a>

La subvención de código de autorización es lo que la mayoría de desarrolladores reconocerán como "OAuth2 estándar" e implica recuperar un código de acceso e intercambiarlo por el token de acceso de un usuario. Permite que el servidor de autorización actúe como intermediario entre el cliente y el propietario del recurso, por lo que las credenciales del propietario del recurso nunca se comparten directamente con el cliente.

#### Ejemplo de enlace de autorización

El flujo de concesión de código de autorización es lo que la mayoría de los desarrolladores reconocerán como el "OAuth2 estándar" e implica obtener un código de acceso y canjearlo por un token de acceso del usuario. Este flujo permite que el servidor de autorización actúe como intermediario entre el cliente y el propietario del recurso, de modo que las credenciales del propietario del recurso nunca se comparten directamente con el cliente.

```
https://discord.com/oauth2/authorize?response_type=code&client_id=157730590492196864&scope=identify%20guilds.join&state=15773059ghq9183habn&redirect_uri=https%3A%2F%2Fnicememe.website&prompt=consent&integration_type=0
```

Cuando alguien navega a esta URL, se le pedirá que autorice tu aplicación para los ámbitos solicitados. Al aceptar, serán redirigidos a tu `redirect_uri`, que contendrá un parámetro adicional de cadena de consulta, `code`. `state` también se devolverá si se envió previamente, y debe validarse en este punto.

`prompt` controla cómo el flujo de autorización maneja las autorizaciones existentes. Si un usuario ha autorizado previamente tu aplicación con los ámbitos solicitados y prompt está establecido en `consent`, les solicitará que reaprueben su autorización. Si está establecido en `none`, omitirá la pantalla de autorización y los redirigirá de vuelta a tu URI de redirección sin solicitar su autorización. Para ámbitos de paso, como `bot` y `webhook.incoming`, siempre se requiere autorización.

El parámetro `integration_type` especifica el contexto de instalación para la autorización. El contexto de instalación determina dónde se instalará la aplicación, y solo es relevante cuando `scope` contiene `applications.commands`. Cuando se establece en `GUILD_INSTALL`, la aplicación será autorizada para instalación en un servidor. Cuando se establece en `USER_INSTALL`, la aplicación será autorizada para instalación en un usuario. La aplicación debe estar configurada para admitir el `integration_type` proporcionado.

#### Ejemplo de enlace de redirección

```
https://nicememe.website/?code=NhhvTDYsFcdgNLnnLijcl7Ku7bEEeee&state=15773059ghq9183habn
```

`code` ahora se intercambia por el token de acceso del usuario haciendo una solicitud a la URL del token de la siguiente manera:

#### Ejemplo de intercambio de token de acceso

```python
API_ENDPOINT = 'https://discord.com/api/v10'
CLIENT_ID = '332269999912132097'
CLIENT_SECRET = '937it3ow87i4ery69876wqire'
REDIRECT_URI = 'https://nicememe.website'

def exchange_code(code):
  data = {
    'client_id': CLIENT_ID,
    'client_secret': CLIENT_SECRET,
    'grant_type': 'authorization_code',
    'code': code,
    'redirect_uri': REDIRECT_URI
  }
  headers = {
    'Content-Type': 'application/x-www-form-urlencoded'
  }
  r = requests.post('%s/oauth2/token' % API_ENDPOINT, data=data, headers=headers)
  r.raise_for_status()
  return r.json()
```

En respuesta, recibirás una respuesta de token de acceso:

#### Ejemplo de Token de Acceso

```python
{
  "token_type": "Bearer",
  "access_token": "ODkxNDM2MjMzOTAzOTY0MTYx.6qrZcUqja7812RVdnEKjpzOL4CvHBFG",
  "scope": "identify",
  "expires_in": 604800,
  "refresh_token": "D43f5y0ahjqew82jZ4NViEr2YafMKhue"
}
```

Tener el token de acceso del usuario permite que tu aplicación haga ciertas solicitudes a la API en su nombre, restringidas a cualquier ámbito que se haya solicitado.

#### Actualización de código garantizado <a href="#subvencin-de-token-de-actualizacin" id="subvencin-de-token-de-actualizacin"></a>

`expires_in` es cuánto tiempo, en segundos, hasta que el token de acceso devuelto expire, permitiéndote anticipar la expiración y actualizar el token. Para actualizar, haz otra solicitud a la URL del token con los siguientes parámetros:

#### Ejemplo de Intercambio de Token de Actualización

```python
API_ENDPOINT = 'https://discord.com/api/v10'
CLIENT_ID = '332269999912132097'
CLIENT_SECRET = '937it3ow87i4ery69876wqire'
REDIRECT_URI = 'https://nicememe.website'

def refresh_token(refresh_token):
  data = {
    'client_id': CLIENT_ID,
    'client_secret': CLIENT_SECRET,
    'grant_type': 'refresh_token',
    'refresh_token': refresh_token
  }
  headers = {
    'Content-Type': 'application/x-www-form-urlencoded'
  }
  r = requests.post('%s/oauth2/token' % API_ENDPOINT, data=data, headers=headers)
  r.raise_for_status()
  return r.json()
```

¡Boom; respuesta de token de acceso fresco!

### Garantización implícita <a href="#subvencin-implcita" id="subvencin-implcita"></a>

La subvención OAuth2 implícita es un flujo simplificado optimizado para clientes en navegador. En lugar de emitir al cliente un código de autorización para ser intercambiado por un token de acceso, al cliente se le emite directamente un token de acceso. La URL se formatea de la siguiente manera:

#### Ejemplo de enlace de autorización

```
https://discord.com/oauth2/authorize?response_type=token&client_id=290926444748734499&scope=identify&state=15773059ghq9183habn
```

En la redirección, tu URI de redirección contendrá **fragmentos URI** adicionales que representan una respuesta de token de acceso serializada: `access_token`, `token_type`, `expires_in`, `scope`, y `state` (si se especifica). **Estos no son parámetros de cadena de consulta.** Ten en cuenta el carácter "#":

#### Ejemplo de enlace de redirección

```
https://findingfakeurlsisprettyhard.tv/#access_token=RTfP0OK99U3kbRtHOoKLmJbOn45PjL&token_type=Bearer&expires_in=604800&scope=identify&state=15773059ghq9183habn
```

Hay compensaciones al usar el flujo de subvención implícita. Es tanto más rápido y fácil de implementar, pero en lugar de intercambiar un código y obtener un token devuelto en un cuerpo HTTP seguro, el token de acceso se devuelve en el fragmento URI, lo que lo hace posiblemente expuesto a partes no autorizadas. **Tampoco se te devuelve un token de actualización, por lo que el usuario debe reautorizar explícitamente una vez que su token expire.**

### Garantización de credenciales del cliente <a href="#subvencin-de-credenciales-de-cliente" id="subvencin-de-credenciales-de-cliente"></a>

El flujo de credenciales de cliente es una forma rápida y fácil para que los desarrolladores de bots obtengan sus propios tokens bearer para propósitos de prueba. Al hacer una solicitud a la URL del token con un tipo de concesión de `client_credentials`, se te devolverá un token de acceso para el propietario del bot. Por lo tanto, siempre sé super-extra-muy-no-estamos-bromeando-como-realmente-sé-seguro-asegúrate-de-que-tu-información-no-esté-en-tu-código-fuente cuidadoso con tu `client_id` y `client_secret`. No toleramos impostores por estos lugares.

Puedes especificar ámbitos con el parámetro `scope`, que es una lista de ámbitos OAuth2 separados por espacios:

#### Ejemplo de solicitud de credenciales de cliente

```python
API_ENDPOINT = 'https://discord.com/api/v10'
CLIENT_ID = '332269999912132097'
CLIENT_SECRET = '937it3ow87i4ery69876wqire'

def get_token():
  data = {
    'client_id': CLIENT_ID,
    'client_secret': CLIENT_SECRET,
    'grant_type': 'client_credentials',
    'scope': 'identify connections'
  }
  headers = {
    'Content-Type': 'application/x-www-form-urlencoded'
  }
  r = requests.post('%s/oauth2/token' % API_ENDPOINT, data=data, headers=headers)
  r.raise_for_status()
  return r.json()
```

A cambio, recibirás un token de acceso (sin un token de actualización):

#### Ejemplo de Token de Acceso

```python
{
  "token_type": "Bearer",
  "access_token": "6qrZcUqja7812RVdnEKjpzOL4CvHBFG",
  "scope": "identify connections",
  "expires_in": 604800
}
```

Ten en cuenta que las aplicaciones propiedad de equipos están limitadas a los ámbitos `applications.builds.read`, `applications.builds.upload`, `applications.commands.update`, `applications.entitlements`, `applications.store.update`, e `identify`. Esto es porque estas aplicaciones son propiedad de un pseudo-usuario que no está destinado a ser operado como una cuenta de usuario normal.

### Garantización de código de dispositivo <a href="#subvencin-de-cdigo-de-dispositivo" id="subvencin-de-cdigo-de-dispositivo"></a>

La subvención de código de dispositivo es un flujo que permite a los usuarios autenticarse en dispositivos que no tienen un navegador web o de otra manera no pueden completar el flujo OAuth2 de manera tradicional.&#x20;

Para usar la subvención de código de dispositivo, primero necesitas hacer una solicitud a la URL de autorización de código de dispositivo para recuperar un código de dispositivo y código de usuario:

#### Ejemplo de solicitud de código de dispositivo

```python
API_ENDPOINT = 'https://discord.com/api/v10'
CLIENT_ID = '332269999912132097'
CLIENT_SECRET = '937it3ow87i4ery69876wqire'

def get_device_code():
  data = {
    'client_id': CLIENT_ID,
    'client_secret': CLIENT_SECRET,
    'scope': 'identify connections'
  }
  headers = {
    'Content-Type': 'application/x-www-form-urlencoded'
  }
  r = requests.post('%s/oauth2/authorize/device' % API_ENDPOINT, data=data, headers=headers)
  r.raise_for_status()
  return r.json()
```

A cambio, recibirás una respuesta que contiene la información necesaria para comenzar el proceso de autorización:

#### Ejemplo de respuesta de código de dispositivo

```python
{
  "device_code": "PZVqIuLlME19uotfsUATN65Ytgbejbhj7Eob8qzali",
  "user_code": "ZAW6C586",
  "verification_uri": "https://discord.com/activate",
  "verification_uri_complete": "https://discord.com/activate?user_code=ZAW6C586",
  "expires_in": 300,
  "interval": 5
}
```

Debes mostrar el `user_code` y `verification_uri` al usuario, o incrustar el enlace `verification_uri_complete` en un código QR para que lo escaneen.

Mientras estás solicitando al usuario, también debes comenzar a sondear la URL del token para verificar si el usuario ha completado el proceso de autorización cada `interval` segundos o hasta que haya pasado el tiempo `expires_in`:

#### Ejemplo de Intercambio de Código de Dispositivo

```python
API_ENDPOINT = 'https://discord.com/api/v10'
CLIENT_ID = '332269999912132097'
CLIENT_SECRET = '937it3ow87i4ery69876wqire'

def exchange_device_code(device_code):
  data = {
    'client_id': CLIENT_ID,
    'client_secret': CLIENT_SECRET,
    'grant_type': 'urn:ietf:params:oauth:grant-type:device_code',
    'device_code': device_code
  }
  headers = {
    'Content-Type': 'application/x-www-form-urlencoded'
  }
  r = requests.post('%s/oauth2/token' % API_ENDPOINT, data=data, headers=headers)
  if r.status_code == 200:
    return r.json()
  elif r.status_code == 400:
    data = r.json()
    if r['error'] == 'authorization_pending':
      # The user has not yet completed the authorization process
      return None
    elif r['error'] == 'slow_down':
      # Increase the polling interval
      return None
    elif r['error'] == 'expired_token':
      # The device code has expired, you need to request a new one
      raise Exception('The device code has expired, please request a new one')
    elif r['error'] == 'access_denied':
      # The user has denied the authorization request
      raise Exception('The user has denied the authorization request')
  r.raise_for_status()
```

Si el usuario ha completado el proceso de autorización, recibirás una respuesta de token de acceso como de costumbre.

### PKCE <a href="#pkce" id="pkce"></a>

Discord admite la extensión Proof Key for Code Exchange (PKCE) al flujo de código de autorización OAuth2.

PKCE permite a los usuarios autenticarse con tu aplicación sin compartir tu secreto de cliente. Esto permite que aplicaciones orientadas al usuario como extensiones de navegador o aplicaciones móviles gestionen la autenticación de manera segura. El flujo se ejecuta completamente entre el usuario y Discord, permitiendo que los usuarios actualicen sus propios tokens bearer sin necesidad del secreto de cliente de tu aplicación.

Si quieres que tus clientes puedan actualizar sus propios tokens automáticamente, necesitarás habilitar la bandera de aplicación `PUBLIC_OAUTH2_CLIENT`.

Al usar PKCE, tu aplicación también puede utilizar esquemas personalizados en `redirect_uris`.

#### Verificador de Código

Primero, el cliente necesita crear un verificador de código, `code_verifier`:

* El verificador debe ser una cadena de 43 - 128 caracteres.
* Los caracteres deben ser alfanuméricos (`A-Z`, `a-z`, `0-9`) y guiones `-`, puntos `.`, guiones bajos `_`, y tildes `~`.
* El verificador de código también debe generarse aleatoriamente para cada solicitud de autorización.

La especificación PKCE recomienda que generes una cadena aleatoria de 32 bytes y la codifiques en base64 URL sin relleno, resultando en una cadena de 43 bytes.

{% hint style="info" %}
code\_verifier = base64.urlsafe\_b64encode(os.urandom(32)).rstrip(b'=').decode('utf-8')
{% endhint %}

#### Desafío de código

A continuación, el cliente necesita crear un desafío de código, `code_challenge`, que es el hash SHA256 codificado en base64 URL del `code_verifier`, sin relleno.

SHA256 es el único algoritmo de hash admitido para PKCE en la implementación OAuth2 de Discord.

```
sha256 = hashlib.sha256(code_verifier.encode('utf-8')).digest()
code_challenge = base64.urlsafe_b64encode(sha256).decode('utf-8').rstrip('=')
```

El cliente luego construye la URL de autorización con los parámetros `code_challenge` y `code_challenge_method`:

#### Ejemplo de enlace de autorización

```
https://discord.com/oauth2/authorize?response_type=code&client_id=290926444748734499&scope=identify&code_challenge=CNPVOxIUDw5vcUaWT3Gn8fjrEeZs-kMEqpk2eNzqsmQ&code_challenge_method=S256&state=15773059ghq9183habn
```

Si es exitoso, Discord te enviará a la URL de redirección con el código de autorización usual. La URL de redirección puede reenviarse de manera segura al cliente, ya que el `code_challenge` no es información sensible. Sin embargo, también es aceptable mantener las credenciales en el servidor en su lugar.

Al intercambiar el código de autorización por un token de acceso, el cliente debe incluir el `code_verifier` en la solicitud. Ten en cuenta que para omitir el campo `client_secret` como se muestra a continuación, debes tener la bandera de aplicación `PUBLIC_OAUTH2_CLIENT` establecida.

#### Ejemplo de intercambio de token de acceso

```python
API_ENDPOINT = 'https://discord.com/api/v10'
CLIENT_ID = '332269999912132097'
CODE_VERIFIER = 'Qs-0Scio0ScPJDYOFy1NYsOAsj6Rb6cP-Y12N9pbwV0'
REDIRECT_URI = 'https://nicememe.website'

def exchange_code(code):
  data = {
    'client_id': CLIENT_ID,
    'grant_type': 'authorization_code',
    'code': code,
    'code_verifier': CODE_VERIFIER,
    'redirect_uri': REDIRECT_URI
  }
  headers = {
    'Content-Type': 'application/x-www-form-urlencoded'
  }
  r = requests.post('%s/oauth2/token' % API_ENDPOINT, data=data, headers=headers)
  r.raise_for_status()
  return r.json()
```

Esto te dará una respuesta de token de acceso como de costumbre. Si tienes la bandera de aplicación `PUBLIC_OAUTH2_CLIENT` establecida, ahora puedes actualizar el token en el cliente, omitiendo el campo `client_secret` dependiendo de la bandera de aplicación `PUBLIC_OAUTH2_CLIENT`.

### Bots <a href="#bots" id="bots"></a>

Entonces, ¿qué son las cuentas de bot?

#### Cuentas de bot vs usuario

La API de Discord proporciona un tipo separado de cuenta de usuario dedicada a la automatización, llamada cuenta de bot. Las cuentas de bot pueden crearse a través de la API de aplicaciones y no tienen correo electrónico ni contraseña. A diferencia del flujo OAuth2 normal, las cuentas de bot tienen acceso completo a todas las rutas de la API sin usar tokens bearer, y pueden conectarse al Gateway de Tiempo Real. Automatizar cuentas de usuario normales (generalmente llamadas "self-bots") fuera de la API OAuth2/bot está prohibido, y puede resultar en una terminación de cuenta si se encuentra. No te encuentren :)

Las cuentas de bot tienen algunas diferencias en comparación con las cuentas de usuario normales, principalmente:

* Los bots se añaden a servidores a través de la API OAuth2, y no pueden aceptar invitaciones normales.
* Los bots no pueden tener amigos, ni ser añadidos a o unirse a MD Grupales.
* Los bots verificados no tienen un número máximo de Servidores.
* Los bots tienen un conjunto completamente separado de Límites de Velocidad.

#### Flujo de autorización de bot

La autorización de bot es un flujo OAuth2 especial sin servidor y sin callback que hace fácil para los usuarios añadir bots a servidores. La URL que creas se ve similar a lo que usamos para implementación de pila completa:

#### Ejemplo de enlace de Autorización

```
https://discord.com/oauth2/authorize?client_id=157730590492196864&scope=bot&permissions=1
```

En el caso de bots, el parámetro `scope` debe establecerse en `bot`. También hay un nuevo parámetro, `permissions`, que es un entero correspondiente a los cálculos de permisos para el bot. También notarás la ausencia de `response_type` y `redirect_uri`. La autorización de bot no requiere estos parámetros porque no hay necesidad de recuperar el token de acceso del usuario.

Cuando el usuario navega a esta página, se le pedirá que añada el bot a un servidor en el que tenga permisos apropiados. Al aceptar, el bot será añadido. ¡Súper fácil!

Si casualmente ya conoces el ID del servidor al que el usuario añadirá tu bot, puedes proporcionar este ID en la URL como un parámetro `guild_id=GUILD_ID`. Cuando la página de autorización carga, ese servidor será preseleccionado en el diálogo si ese usuario tiene permisos para añadir el bot a ese servidor. Puedes usar esto junto con el parámetro `disable_guild_select=true` para no permitir que el usuario elija un servidor diferente.

Si tu bot es súper específico para tu casa club privada, o simplemente no te gusta compartir, puedes asegurarte de que `integration_public` esté deshabilitado en tu aplicación. Si no está marcado, solo tú puedes añadir el bot a servidores. Si está marcado como público, cualquiera con el ID de tu bot puede añadirlo a servidores en los que tenga permisos apropiados.

#### Autorización avanzada de bot

Los desarrolladores pueden extender la funcionalidad de autorización de bot. Puedes solicitar ámbitos adicionales fuera de `bot`, lo que solicitará una continuación hacia un flujo completo de subvención de código de autorización y añadirá la capacidad de solicitar el token de acceso del usuario. Si solicitas cualquier ámbito fuera de `bot` o `applications.commands`, `response_type` es nuevamente obligatorio.

Al recibir el código de acceso en la redirección, habrá parámetros adicionales de cadena de consulta de `guild_id` y `permissions`. **Estos parámetros solo deben usarse como pistas, ya que son fácilmente falsificados por usuarios maliciosos.** Para estar seguro de la relación entre tu bot y el servidor, considera habilitar `integration_require_code_grant` en tu aplicación. Habilitarlo requiere que cualquiera que añada tu bot a un servidor pase por un flujo completo de subvención de código de autorización OAuth2, lo que significa que la integración no se creará hasta que tu backend intercambie el código por un token. Cuando recuperes el token de acceso del usuario, también recibirás información sobre el servidor al que se añadió tu bot a través de un objeto `guild` adicional en la respuesta.

#### Requisito de autenticación multi-factor

Para bots con permisos elevados (permisos con un `*` junto a ellos), aplicamos autenticación multi-factor en la cuenta del propietario cuando se añaden a servidores que tienen MFA habilitado en todo el servidor.

### Webhooks <a href="#webhooks" id="webhooks"></a>

El flujo de webhook de Discord es una versión especializada de una implementación de código de autorización. En este caso, el parámetro de cadena de consulta `scope` necesita incluir `webhook.incoming`:

#### Ejemplo de URL de Autorización

```
https://discord.com/oauth2/authorize?response_type=code&client_id=157730590492196864&scope=webhook.incoming&state=15773059ghq9183habn&redirect_uri=https%3A%2F%2Fnicememe.website
```

Cuando el usuario navega a esta URL, se le pedirá que seleccione un canal en el que permitir el webhook. Cuando el webhook se ejecute, publicará su mensaje en este canal. Al aceptar, el usuario será redirigido a tu `redirect_uri`. La URL contendrá el parámetro de cadena de consulta `code` que debe intercambiarse por un token de acceso como de costumbre. A cambio, recibirás una respuesta de token ligeramente modificada, con un objeto `webhook` adicional:

#### Ejemplo de token de acceso

```python
{
  "token_type": "Bearer",
  "access_token": "GNaVzEtATqdh173tNHEXY9ZYAuhiYxvy",
  "scope": "webhook.incoming",
  "expires_in": 604800,
  "refresh_token": "PvPL7ELyMDc1836457XCDh1Y8jPbRm",
  "webhook": {
    "type": 1,
    "id": "347114750880120863",
    "name": "Application Name Here",
    "avatar": "cc7e0aa58a4224a281fbb8217b808a72",
    "channel_id": "345626669224982402",
    "guild_id": "290926792226357250",
    "application_id": "310954232226357250",
    "token": "kKDdjXa1g9tKNs0-_yOwLyALC9gydEWP6gr9sHabuK1vuofjhQDDnlOclJeRIvYK-pj_",
    "url": "https://discord.com/api/webhooks/347114750880120863/kKDdjXa1g9tKNs0-_yOwLyALC9gydEWP6gr9sHabuK1vuofjhQDDnlOclJeRIvYK-pj_"
  }
}
```

De este objeto, debes almacenar el `webhook.id` y `webhook.token`. Consulta la documentación de ejecutar webhook para saber cómo enviar mensajes con el webhook.

Cualquier usuario que desee añadir tu webhook a su canal necesitará pasar por el flujo OAuth2 completo, y se crea un nuevo webhook cada vez. Si deseas enviar un mensaje a todos tus webhooks, necesitarás iterar sobre cada combinación `id:token` almacenada y hacer solicitudes `POST` a cada una. ¡Ten en cuenta nuestros límites de velocidad!

### Objeto de Token de Acceso OAuth2 <a href="#objeto-de-token-de-acceso-oauth2" id="objeto-de-token-de-acceso-oauth2"></a>

#### Estructura de token de acceso

<sup>1</sup> Solo devuelto desde el endpoint Get Provisional Account Token y al usar la subvención de código de autorización con el ámbito `openid`.

| Campo           | Tipo           | Descripción                                                      |
| --------------- | -------------- | ---------------------------------------------------------------- |
| token\_type     | string         | El tipo de token, siempre `Bearer`.                              |
| access\_token   | string         | El token de acceso.                                              |
| id\_token? 1    | string         | El token de ID.                                                  |
| scope           | string         | Los ámbitos que el usuario ha autorizado, separados por espacio. |
| expires\_in     | integer        | Duración (en segundos) después de la cual el token expira.       |
| refresh\_token? | string         | El token de actualización, si aplica.                            |
| guild?          | guild object   | El servidor al que se añadió el bot, si aplica.                  |
| webhook?        | webhook object | El webhook creado, si aplica.                                    |

### Objeto de Autorización OAuth2 <a href="#objeto-de-autorizacin-oauth2" id="objeto-de-autorizacin-oauth2"></a>

#### Estructura de autorización OAuth2

| Campo        | Tipo                       | Descripción                                                              |
| ------------ | -------------------------- | ------------------------------------------------------------------------ |
| id           | snowflake                  | El ID del token.                                                         |
| scopes       | array\[string]             | Los ámbitos que el usuario ha autorizado para la aplicación.             |
| application  | partial application object | La aplicación autorizada.                                                |
| disclosures? | array\[integer]            | Las divulgaciones de aplicación que han sido reconocidas por el usuario. |

### Endpoints <a href="#endpoints" id="endpoints"></a>

#### Obtener Información de Autorización Actual

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /oauth2/@me

Devuelve información sobre la autorización actual.

{% hint style="warning" %}
Este endpoint solo se puede utilizar con un token de acceso OAuth2.
{% endhint %}

#### Cuerpo de respuesta

| Campo       | Tipo                       | Descripción                                                                         |
| ----------- | -------------------------- | ----------------------------------------------------------------------------------- |
| application | partial application object | La aplicación actual.                                                               |
| scopes      | array\[string]             | Los ámbitos que el usuario ha autorizado para la aplicación.                        |
| expires     | ISO8601 timestamp          | Cuando el token de acceso expira.                                                   |
| user?       | partial user object        | El usuario que ha autorizado, si el usuario ha autorizado con el ámbito `identify`. |

#### Obtener claves OpenID connect

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /oauth2/keys

`No autenticado`

Devuelve el Conjunto de Claves Web JSON usado para verificar tokens de ID OpenID Connect emitidos por Discord. Este endpoint cumple con la especificación OpenID Connect.

#### Obtener información de usuario OpenID

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /oauth2/userinfo

`OAuth2`

Devuelve información de usuario OpenID para la autorización actual. Este endpoint cumple con la especificación OpenID Connect.

{% hint style="warning" %}
Este endpoint solo se puede utilizar con un token de acceso OAuth2 que tenga el scope `openid`.
{% endhint %}

#### Cuerpo de respuesta

<sup>1</sup> Requiere el ámbito `email`.

<sup>2</sup> Requiere el ámbito `identify`.

| Campo                 | Tipo    | Descripción                                                 |
| --------------------- | ------- | ----------------------------------------------------------- |
| sub                   | string  | El ID del usuario.                                          |
| email 1               | ?string | La dirección de correo electrónico del usuario.             |
| email\_verified 1     | boolean | Si el correo electrónico en esta cuenta ha sido verificado. |
| preferred\_username 2 | string  | El nombre de usuario del usuario.                           |
| nickname 2            | ?string | El nombre de visualización del usuario.                     |
| picture 2             | string  | La URL del avatar del usuario.                              |
| locale 2              | string  | La configuración regional del usuario.                      |

#### Ejemplo de respuesta

```python
{
  "sub": "852892297661906993",
  "email": "dolfies@amazing.email",
  "email_verified": true,
  "preferred_username": "dolfies",
  "nickname": "Dolfies",
  "picture": "https://cdn.discordapp.com/avatars/852892297661906993/c78ef8fb1db15a3d5f1b4c057856c5c9.png",
  "locale": "en-US"
}
```

#### Obtener código de dispositivo OAuth2

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /oauth2/authorize/device

Recupera un código de dispositivo y código de usuario para el flujo de subvención de código de dispositivo.

#### Parámetros de formulario

<sup>1</sup> También puedes pasar tu `client_id` y `client_secret` como autenticación básica con `client_id` como nombre de usuario y `client_secret` como contraseña.

<sup>2</sup> Requerido si la aplicación no tiene la bandera de aplicación `PUBLIC_OAUTH2_CLIENT`.

| Campo               | Tipo      | Descripción                                               |
| ------------------- | --------- | --------------------------------------------------------- |
| client\_id? 1       | snowflake | El ID de la aplicación.                                   |
| client\_secret? 1 2 | string    | El secreto de cliente de la aplicación.                   |
| scope?              | string    | Una lista delimitada por espacios de ámbitos a solicitar. |

#### Cuerpo de respuesta

| Campo                       | Tipo    | Descripción                                                                                       |
| --------------------------- | ------- | ------------------------------------------------------------------------------------------------- |
| device\_code                | string  | El código de dispositivo a usar para la subvención de código de dispositivo.                      |
| user\_code                  | string  | El código de usuario a mostrar al usuario para autorización.                                      |
| verification\_uri           | string  | La URL a mostrar al usuario para autorización.                                                    |
| verification\_uri\_complete | string  | La URL completa para redirigir al usuario para autorización, incluyendo el código de usuario.     |
| expires\_in                 | integer | La duración (en segundos) después de la cual el código de dispositivo expira.                     |
| interval                    | integer | El intervalo (en segundos) en el que sondear el endpoint de token para el estado de autorización. |

#### Obtener token OAuth2

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /oauth2/token

`No autenticado`

Recupera un token de acceso OAuth2 para las credenciales de aplicación dadas. Implementa varios tipos diferentes de subvenciones. Devuelve un objeto de token de acceso oauth2 en caso de éxito.

#### Parámetros de formulario

<sup>1</sup> También puedes pasar tu `client_id` y `client_secret` como autenticación básica con `client_id` como nombre de usuario y `client_secret` como contraseña.

<sup>2</sup> Requerido si la aplicación no tiene la bandera de aplicación `PUBLIC_OAUTH2_CLIENT`, el `grant_type` es `client_credentials`, o el `grant_type` es `authorization_code` y `code_verifier` no se proporciona.

<sup>3</sup> Proporcionar estos campos fusionará la cuenta provisional asociada con la cuenta de usuario que se está autenticando. Consulta el evento Gateway User Merge Operation Completed para más información.

| Campo                    | Tipo      | Descripción                                                                                                                                                                                    |
| ------------------------ | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| grant\_type              | string    | El tipo de subvención a usar.                                                                                                                                                                  |
| client\_id? 1            | snowflake | El ID de la aplicación.                                                                                                                                                                        |
| client\_secret? 1 2      | string    | El secreto de cliente de la aplicación.                                                                                                                                                        |
| code?                    | string    | El código de autorización a intercambiar por un token.                                                                                                                                         |
| code\_verifier?          | string    | El verificador de código para la extensión PKCE de la subvención de código de autorización.                                                                                                    |
| redirect\_uri?           | string    | La URL a la cual redirigir después de la autorización; debe coincidir con una de las URI de redirección registradas para la aplicación; solo aplicable para subvenciones `authorization_code`. |
| refresh\_token?          | string    | El token de actualización a intercambiar por un nuevo token de acceso.                                                                                                                         |
| device\_code?            | string    | El código de dispositivo a intercambiar por un token de acceso.                                                                                                                                |
| scope?                   | string    | Una lista delimitada por espacios de ámbitos a solicitar; solo aplicable para subvenciones `client_credentials`.                                                                               |
| external\_auth\_type? 3  | string    | El tipo del proveedor de autenticación externa.                                                                                                                                                |
| external\_auth\_token? 3 | string    | El token de autenticación externa.                                                                                                                                                             |

#### Tipo de garantización OAuth2

| Valor                                         | Descripción                                     |
| --------------------------------------------- | ----------------------------------------------- |
| authorization\_code                           | Flujo de subvención de código de autorización.  |
| refresh\_token                                | Flujo de subvención de token de actualización.  |
| client\_credentials                           | Flujo de subvención de credenciales de cliente. |
| urn:ietf:params:oauth:grant-type:device\_code | Flujo de subvención de código de dispositivo.   |

#### Revocar token OAuth2

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /oauth2/token/revoke

`No autenticado`

Revoca el token de acceso o actualización OAuth2 dado. Devuelve un objeto vacío en caso de éxito.

{% hint style="warning" %}
Cuando se revoca cualquier token de acceso o de actualización válido, todos los tokens de acceso y de actualización de la aplicación para ese usuario se invalidan inmediatamente.
{% endhint %}

#### Parámetros de formulario

<sup>1</sup> También puedes pasar tu `client_id` y `client_secret` como autenticación básica con `client_id` como nombre de usuario y `client_secret` como contraseña.

<sup>2</sup> Requerido si la aplicación no tiene la bandera de aplicación `PUBLIC_OAUTH2_CLIENT`.

| Campo               | Tipo      | Descripción                                   |
| ------------------- | --------- | --------------------------------------------- |
| token               | string    | El token de acceso o actualización a revocar. |
| client\_id? 1       | snowflake | El ID de la aplicación.                       |
| client\_secret? 1 2 | string    | El secreto de cliente de la aplicación.       |

### Obtener token de cuenta provisional

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /partner-sdk/token

`No autenticado`

Recupera un token de acceso para una cuenta provisional con las credenciales dadas. Devuelve un objeto de token de acceso oauth2 en caso de éxito. La respuesta solo contendrá un `refresh_token` con ciertos tipos de autenticación externa.

Si no se encuentra una cuenta pero el token de autenticación externa proporcionado es válido, se creará una nueva cuenta provisional.

{% hint style="warning" %}
Al intentar autenticar una cuenta provisional que ha sido fusionada con una cuenta de usuario, la solicitud fallará con un error 400 _bad request_ y un cuerpo de respuesta de error especial:

```python
{
  "message": "User account is non-provisional and should be authed through OAuth2",
  "code": 530010,
  "user_id": "1001086404203389018",
  "provider_type": "OIDC",
  "provider_id": "https://auth.example.com",
  "provider_issued_user_id": "123456789"
}
```

Esto indica que la cuenta provisional ha sido fusionada con una cuenta de usuario, y el usuario debe ser autenticado a través de OAuth2 en su lugar, a menos que la cuenta provisional se separe primero.
{% endhint %}

#### Parámetros JSON

<sup>1</sup> Requerido si la aplicación no tiene la bandera de aplicación `PUBLIC_OAUTH2_CLIENT`.

| Campo                 | Tipo      | Descripción                                     |
| --------------------- | --------- | ----------------------------------------------- |
| client\_id            | snowflake | El ID de la aplicación.                         |
| client\_secret? 1     | string    | El secreto de cliente de la aplicación.         |
| external\_auth\_type  | string    | El tipo del proveedor de autenticación externa. |
| external\_auth\_token | string    | El token de autenticación externa.              |

#### Tipo de autenticación de proveedor externo

| Valor                                 | Descripción                                                                                         |
| ------------------------------------- | --------------------------------------------------------------------------------------------------- |
| OIDC                                  | Token de ID OpenID Connect.                                                                         |
| EPIC\_ONLINE\_SERVICES\_ACCESS\_TOKEN | Token de acceso para Epic Online Services (admite tokens de acceso EOS Auth).                       |
| EPIC\_ONLINE\_SERVICES\_ID\_TOKEN     | Token de ID para Epic Online Services (admite tanto tokens de ID EOS Auth + Connect).               |
| STEAM\_SESSION\_TICKET                | Un ticket de autenticación de Steam para web generado con `discord` establecido como la `identity`. |
| UNITY\_SERVICES\_ID\_TOKEN            | Token de ID para Unity Auth Services.                                                               |

#### Obtener token de cuenta provisional con bot

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /partner-sdk/token/bot

Recupera un token de acceso para una cuenta provisional con las credenciales dadas. Devuelve un objeto de token de acceso oauth2 en caso de éxito.

Si no se encuentra una cuenta, se creará una nueva cuenta provisional.

{% hint style="warning" %}
Al intentar autenticar una cuenta provisional que ha sido fusionada con una cuenta de usuario, la solicitud fallará con un error 400 _bad request_ y un cuerpo de respuesta de error especial:

```python
{
  "message": "User account is non-provisional and should be authed through OAuth2",
  "code": 530010,
  "user_id": "142007603549962240",
  "provider_type": "DISCORD_BOT",
  "provider_issued_user_id": "123456789"
}
```

Esto indica que la cuenta provisional ha sido fusionada con una cuenta de usuario, y el usuario debe autenticarse a través de OAuth2 en su lugar, a menos que la cuenta provisional sea separada primero.
{% endhint %}

{% hint style="warning" %}
Este endpoint no se puede utilizar con cuentas de usuario.
{% endhint %}

#### Parámetros JSON

| Campo                    | Tipo   | Descripción                                                        |
| ------------------------ | ------ | ------------------------------------------------------------------ |
| external\_user\_id       | string | El ID del usuario a autenticar (máximo 1024 caracteres).           |
| preferred\_global\_name? | string | El nombre global preferido para el usuario (máximo 32 caracteres). |

#### Separar cuenta provisional

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /partner-sdk/provisional-accounts/unmerge

`No autenticado`

Separar una cuenta provisional. Devuelve una respuesta vacía 204 en caso de éxito.

{% hint style="warning" %}
La separación invalida todos los tokens de acceso y de actualización del usuario.
{% endhint %}

#### Parámetros JSON

<sup>1</sup> Requerido si la aplicación no tiene la bandera de aplicación `PUBLIC_OAUTH2_CLIENT`.

| Campo                 | Tipo      | Descripción                                     |
| --------------------- | --------- | ----------------------------------------------- |
| client\_id            | snowflake | El ID de la aplicación.                         |
| client\_secret? 1     | string    | El secreto de cliente de la aplicación.         |
| external\_auth\_type  | string    | El tipo del proveedor de autenticación externa. |
| external\_auth\_token | string    | El token de autenticación externa.              |

#### Obtener autorizaciones OAuth2

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /oauth2/tokens

Devuelve una lista de objetos de autorización OAuth2.

#### Obtener autorización OAuth2

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /oauth2/authorize

Devuelve información sobre una posible autorización OAuth2.

{% hint style="warning" %}
Si no está autenticado, este endpoint redirigirá a la página de autorización OAuth2 con los parámetros especificados.
{% endhint %}

#### Parámetros de consulta

<sup>1</sup> Requerido a menos que se use el flujo de autorización básico de bot.

<sup>2</sup> Si se especifica un `response_type` y no se especifica `redirect_uri`, la respuesta contendrá la primera URI de redirección registrada para la aplicación.

| Campo                    | Tipo      | Descripción                                                                                                                                                         |
| ------------------------ | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| client\_id               | snowflake | El ID de la aplicación.                                                                                                                                             |
| response\_type? 1        | string    | El tipo de respuesta a devolver.                                                                                                                                    |
| scope                    | string    | Una lista delimitada por espacios de ámbitos a solicitar; puede omitirse si la aplicación tiene un `integration_types_config` poblado.                              |
| redirect\_uri? 2         | string    | La URL a la cual redirigir después de la autorización; debe coincidir con una de las URI de redirección registradas para la aplicación.                             |
| state?                   | string    | Una cadena única para vincular la solicitud del usuario a su estado autenticado.                                                                                    |
| nonce?                   | string    | Una cadena única para vincular la solicitud del usuario a su estado autenticado; solo aplicable para subvenciones de código de autorización con el ámbito `openid`. |
| code\_challenge?         | string    | Un desafío de código para la extensión PKCE de la subvención de código de autorización; debe usarse con `code_challenge_method`.                                    |
| code\_challenge\_method? | string    | El método usado para generar el desafío de código (debe ser `S256`); solo aplicable para la extensión PKCE de la subvención de código de autorización.              |
| integration\_type?       | integer   | El contexto de instalación para la autorización; solo aplicable cuando `scope` contiene `applications.commands` (por defecto `GUILD_INSTALL`).                      |

#### Cuerpo de respuesta

| Campo             | Tipo                        | Descripción                                                                                                                                 |
| ----------------- | --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| application       | partial application object  | La aplicación que está siendo autorizada.                                                                                                   |
| user              | partial user object         | El usuario que está autorizando.                                                                                                            |
| authorized        | boolean                     | Si el usuario ya ha autorizado la aplicación con estos ámbitos, lo que significa que se puede omitir el consentimiento.                     |
| integration\_type | integer                     | El contexto de instalación para la autorización.                                                                                            |
| redirect\_uri?    | ?string                     | La URL a la cual redirigir después de la autorización; solo presente si se especifica `response_type`.                                      |
| bot?              | partial user object         | El usuario bot que será añadido al servidor; solo presente si se solicita el ámbito `bot`.                                                  |
| guilds?           | array\[oauth2 guild object] | Los servidores del usuario; solo presente si `integration_type` es `GUILD_INSTALL` y se solicita el ámbito `bot` o `applications.commands`. |

#### Estructura de Servidor OAuth2

| Campo       | Tipo      | Descripción                                                            |
| ----------- | --------- | ---------------------------------------------------------------------- |
| id          | snowflake | El ID del servidor.                                                    |
| name        | string    | El nombre del servidor (2-100 caracteres).                             |
| icon        | ?string   | El hash del icono del servidor.                                        |
| mfa\_level  | integer   | Nivel MFA requerido para acciones administrativas dentro del servidor. |
| permissions | string    | Permisos que el usuario tiene en el servidor.                          |

#### Ejemplo de Servidor OAuth2

```python
{
  "id": "81384788765712384",
  "name": "Discord API",
  "icon": "a363a84e969bcbe1353eb2fdfb2e50e6",
  "mfa_level": 1,
  "permissions": "1095530297282240"
}
```

#### Crear autorización OAuth2

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /oauth2/authorize

Autoriza al usuario para la aplicación OAuth2 dada. Puede disparar un evento Gateway Guild Integrations Update, Integration Create, User Application Update, y/o User Connections Update.

#### Parámetros de consulta

<sup>1</sup> Requerido a menos que se use el flujo de autorización básico de bot.

<sup>2</sup> Si se especifica un `response_type` y no se especifica `redirect_uri`, el usuario será redirigido a la primera URI de redirección registrada para la aplicación.

| Campo                    | Tipo      | Descripción                                                                                                                                                         |
| ------------------------ | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| client\_id               | snowflake | El ID de la aplicación.                                                                                                                                             |
| response\_type? 1        | string    | El tipo de respuesta a devolver.                                                                                                                                    |
| scope                    | string    | Una lista delimitada por espacios de ámbitos a solicitar; puede omitirse si la aplicación tiene un `integration_types_config` poblado.                              |
| redirect\_uri? 2         | string    | La URL a la cual redirigir después de la autorización; debe coincidir con una de las URI de redirección registradas para la aplicación.                             |
| state?                   | string    | Una cadena única para vincular la solicitud del usuario a su estado autenticado.                                                                                    |
| nonce?                   | string    | Una cadena única para vincular la solicitud del usuario a su estado autenticado; solo aplicable para subvenciones de código de autorización con el ámbito `openid`. |
| code\_challenge?         | string    | Un desafío de código para la extensión PKCE de la subvención de código de autorización; debe usarse con `code_challenge_method`.                                    |
| code\_challenge\_method? | string    | El método usado para generar el desafío de código (debe ser `S256`); solo aplicable para la extensión PKCE de la subvención de código de autorización.              |

#### Parámetros JSON

<sup>1</sup> Si no se solicitan permisos, no se crea un rol de integración para el usuario bot en el servidor.

| Campo                 | Tipo                           | Descripción                                                                                                                                                                                                                |
| --------------------- | ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| authorize?            | boolean                        | Si autorizar al usuario para la aplicación (por defecto false).                                                                                                                                                            |
| integration\_type?    | integer                        | El contexto de instalación para la autorización (por defecto `GUILD_INSTALL`).                                                                                                                                             |
| permissions? 1        | string                         | Los permisos a solicitar para el usuario bot en el servidor; solo aplicable cuando se solicita el ámbito `bot`.                                                                                                            |
| guild\_id?            | snowflake                      | El ID del servidor al que la aplicación debe añadirse o el webhook debe crearse; solo aplicable cuando se solicita el ámbito `bot`, `applications.commands`, o `webhook.incoming` y `integration_type` es `GUILD_INSTALL`. |
| webhook\_channel\_id? | snowflake                      | El ID del canal donde el webhook debe crearse; solo aplicable cuando se solicita el ámbito `webhook.incoming`.                                                                                                             |
| dm\_settings?         | application DM settings object | La configuración de MD para la aplicación; solo aplicable cuando se solicita el ámbito `applications.commands` y `integration_type` es `USER_INSTALL`.                                                                     |
| location\_context?    | oauth2 location context object | El contexto de ubicación de la autorización dentro del cliente, usado para analíticas.                                                                                                                                     |

#### Estructura de configuración de MD de aplicación

| Campo                | Tipo    | Descripción                                                                               |
| -------------------- | ------- | ----------------------------------------------------------------------------------------- |
| allow\_mobile\_push? | boolean | Si permitir notificaciones push móviles para los MD de la aplicación (por defecto false). |

#### Estructura de contexto de ubicación OAuth2

| Campo          | Tipo      | Descripción                                                 |
| -------------- | --------- | ----------------------------------------------------------- |
| guild\_id?     | snowflake | El ID del servidor donde se está creando la autorización.   |
| channel\_id?   | snowflake | El ID del canal donde se está creando la autorización.      |
| channel\_type? | integer   | El tipo de canal en el que se está creando la autorización. |

#### Cuerpo de respuesta

<sup>1</sup> Si no existe un `redirect_uri`, se usará `https://discord.com`.

| Campo | Tipo   | Descripción                                                                                        |
| ----- | ------ | -------------------------------------------------------------------------------------------------- |
| url 1 | string | La URL para redirigir al usuario, conteniendo el código de autorización, token de acceso, o error. |

### Eliminar Autorización OAuth2

<kbd><mark style="color:$danger;background-color:$danger;">DELETE<mark style="color:$danger;background-color:$danger;"></kbd> /oauth2/tokens/{token.id}

Revoca la autorización dada. Devuelve una respuesta vacía 204 en caso de éxito. Dispara múltiples eventos Gateway OAuth2 Token Revoke y opcionalmente un User Application Remove y User Connections Update.

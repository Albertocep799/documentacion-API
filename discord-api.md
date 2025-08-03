---
icon: book-sparkles
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

# Discord API

La API de Discord está estructurada en torno a dos capas fundamentales: una API REST/HTTPS, orientada a la gestión de operaciones generales, y una conexión WebSocket segura y persistente, utilizada para el envío y la suscripción a eventos en tiempo real. El uso más habitual de la API de Discord consiste en ofrecer servicios o proporcionar acceso a plataformas mediante la utilización de la API [OAuth2](https://oauth.net/2/).

#### Enlaces base

{% hint style="info" %}
Discord ofrece una **API Canary** destinada a probar cambios antes de implementarlos en la API principal. Esta **API Canary** no garantiza estabilidad y puede utilizarse en la URL dedicada o mediante opciones de depuración.
{% endhint %}

{% code overflow="wrap" fullWidth="false" %}
```
Stable: https://discord.com/api
PTB: https://ptb.discord.com/api
Canary:https://canary.discord.com/api
```
{% endcode %}

### <i class="fa-code-branch">:code-branch:</i> Versionado de la API

{% hint style="danger" %}
Algunas versiones de la API y de la gateway ya no funcionan y se encuentran marcadas como "descontinuadas" en la tabla inferior. Intentar utilizar estas versiones resultará en un error **400 Bad Request**.
{% endhint %}

Discord tiene diferentes versiones de su API. Es recomendable especificar la versión que se desea utilizar incluyendo el número de versión en la ruta de la solicitud como <kbd>https://discord.com/api/v{número\_versión}</kbd>. Si se omite este número en la ruta, las solicitudes se redirigirán automáticamente a la versión predeterminada actual.

**Versiones de la API**

| Versión | Estado        | Predeterminado |
| ------- | ------------- | -------------- |
| 10      | Disponible    |                |
| 9       | Disponible    | ✓ Cliente      |
| 8       | Obsoleto      |                |
| 7       | Obsoleto      |                |
| 6       | Obsoleto      | ✓ API          |
| 5       | Descontinuado |                |
| 4       | Descontinuado |                |
| 3       | Descontinuado |                |

### Mensajes de error <a href="#error-messages" id="error-messages"></a>

A partir de la versión 7 de la API, las respuestas a errores de formulario presentan un formato mejorado. La respuesta indicará qué clave JSON contiene el error, el código del error y un mensaje en lenguaje natural fácilmente comprensible. Discord incorpora nuevos mensajes de error con frecuencia, por lo que no resulta viable mantener una lista completa y actualizada; sin embargo, a continuación os dejamos algunos ejemplos:

{% tabs %}
{% tab title="Error de solicitud" %}
{% code overflow="wrap" %}
```json
{
  "code": 50035,
  "message": "Invalid Form Body",
  "errors": {
    "_errors": [
      {
        "code": "APPLICATION_COMMAND_TOO_LARGE",
        "message": "Command exceeds maximum size (4000)"
      }
    ]
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="Error de objeto" %}
{% code overflow="wrap" %}
```json
{
  "code": 50035,
  "errors": {
    "access_token": {
      "_errors": [
        {
          "code": "BASE_TYPE_REQUIRED",
          "message": "This field is required"
        }
      ]
    }
  },
  "message": "Invalid Form Body"
}
```
{% endcode %}
{% endtab %}

{% tab title="Error de array" %}
{% code overflow="wrap" %}
```json
{
  "code": 50035,
  "errors": {
    "activities": {
      "0": {
        "platform": {
          "_errors": [
            {
              "code": "BASE_TYPE_CHOICES",
              "message": "Value must be one of ('desktop', 'android', 'ios')."
            }
          ]
        },
        "type": {
          "_errors": [
            {
              "code": "BASE_TYPE_CHOICES",
              "message": "Value must be one of (0, 1, 2, 3, 4, 5)."
            }
          ]
        }
      }
    }
  },
  "message": "Invalid Form Body"
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Autenticación <a href="#authentication" id="authentication"></a>

La autenticación con la API de Discord puede realizarse de dos formas principales:

1. **Mediante un token de usuario o bot**, obtenido tras [iniciar sesión en una cuenta](https://staffs.gitbook.io/documentation/autentificacion) o [registrar un bot](https://discord.com/developers/applications?new_application=true). Para más detalles sobre la diferencia entre cuentas de usuario y bots, consulta la sección bots vs cuentas de usuario.
2. **A través de un token bearer OAuth2** obtenido mediante la [API OAuth2](https://docs.discord.food/topics/oauth2#oauth2).

En cualquiera de los casos, la <kbd>autenticación</kbd> se realiza enviando el token correspondiente en la cabecera HTTP bajo el siguiente formato: <kbd>Authorization: TIPO\_DE\_TOKEN? TOKEN</kbd>

```
Authorization: MTk4NjIyNDgzNDcxOTI1MjQ4.Cl2FMQ.ZnCjm1XVW7vRze4b7Cq4se7kKWs
```

```
Authorization: Bot MTk4NjIyNDgzNDcxOTI1MjQ4.Cl2FMQ.ZnCjm1XVW7vRze4b7Cq4se7kKWs
```

```
Authorization: Bearer ODkxNDM2MjMzOTAzOTY0MTYx.pXvaXIpme43oSHZLXDCK7IiKA8iAjr
```

### Encriptación <a href="#encryption" id="encryption"></a>

Todos los servicios y protocolos a nivel HTTP (por ejemplo, HTTP, WebSocket) dentro de la API de Discord utilizan TLS 1.2.

### Formato Snowflake <a href="#formato-snowflake" id="formato-snowflake"></a>

Discord utiliza el formato [snowflake](https://github.com/twitter/snowflake/tree/snowflake-2010) de Twitter para los identificadores únicos (IDs). Estos IDs están garantizados como únicos en todo Discord, a menos que en ciertos escenarios concretos los objetos hijo compartan el ID de su padre. Dado que los IDs Snowflake pueden ser de hasta 64 bits (ej., uint64), siempre se devuelven como cadenas en la API HTTP para evitar desbordamientos de enteros en algunos lenguajes. Consulta la documentación del Gateway para más detalles sobre la codificación.

**Desglose binario de un Snowflake ID**

{% code overflow="wrap" %}
```
111111111111111111111111111111111111111111 11111 11111 111111111111
64                                         22    17    12          0
```
{% endcode %}

**Estructura de Snowflake ID (de izquierda a derecha)**

| Campo                    | Bits    | Número de bits | Descripción                                                                | Obtención                           |
| ------------------------ | ------- | -------------- | -------------------------------------------------------------------------- | ----------------------------------- |
| Marca de tiempo          | 63 a 22 | 42 bits        | Milisegundos desde el Discord Epoch, primer segundo de 2015 o 14200704000. | `(snowflake >> 22) + 1420070400000` |
| ID interno de trabajador | 21 a 17 | 5 bits         |                                                                            | `(snowflake & 0x3E0000) >> 17`      |
| ID interno de proceso    | 16 a 12 | 5 bits         |                                                                            | `(snowflake & 0x1F000) >> 12`       |
| Incremento               | 11 a 0  | 12 bits        | Por cada ID generado en ese proceso, este número se incrementa             | `snowflake & 0xFFF`                 |

**Convertir snowflake a fecha-hora**

<figure><img src=".gitbook/assets/0194906e-de3c-488c-b800-ccc16236c2f4_removalai_preview.png" alt=""><figcaption></figcaption></figure>

**IDs tipo Snowflake en la paginación**

Discord utiliza habitualmente los IDs tipo _snowflake_ en muchas rutas de la API para la paginación. El paradigma de paginación estandarizado permite especificar IDs (y combinarlos con otros parámetros) para recuperar la página de resultados deseada. Para detalles concretos, consulta la documentación del endpoint correspondiente.

Es importante señalar que los IDs _snowflake_ son simplemente números con una marca de tiempo. Por ello, si deseas paginar resultados desde el principio de los tiempos (Epoch de Discord, aunque también funciona en otros casos) o antes/después de un momento específico, puedes generar un ID _snowflake_ correspondiente a ese instante.

**Generar un ID Snowflake a partir de una marca de tiempo**

```
(timestamp_ms - DISCORD_EPOCH) << 22
```

### Serialización de ID <a href="#id-serialization" id="id-serialization"></a>

Hay algunos casos en los que la API y el Gateway pueden devolver IDs en un formato inesperado. Internamente, Discord almacena los IDs como snowflakes enteros. Cuando los IDs se serializan a JSON, los **bigints**[<sup>1</sup>](#user-content-fn-1)[^1] se transforman en <kbd>cadenas</kbd>. Dado que todos los IDs de Discord son snowflakes, siempre debes esperar una cadena.

Sin embargo, hay casos en los que, al enviar algo a la API, en su lugar devuelve IDs serializados como un entero; esto sucede cuando envías a la API o al Gateway un valor en un campo `id` que no es de tamaño **bigint**[<sup>2</sup>](#user-content-fn-2)[^2], por ejemplo, cuando solicitas `GUILD_MEMBERS_CHUNK` desde el gateway:

{% code overflow="wrap" %}
```json
// Send
{
  op: 8,
  d: {
    guild_id: [ '308994132968210433' ],
    user_ids: [ '123123' ]
  }
}

// Receive
{
  t: 'GUILD_MEMBERS_CHUNK',
  s: 3,
  op: 0,
  d: {
    not_found: [ 123123 ],
    members: [],
    guild_id: '308994132968210433'
  }
}
```
{% endcode %}

Puedes ver que, en este caso, el valor de <kbd>ID</kbd> enviado no es un **bigint**; por lo tanto, cuando Discord lo serializa de vuelta a JSON, no lo transforma en una cadena. **Esto nunca ocurrirá con IDs que provienen de Discord.** Sin embargo, sí puede pasar si envías datos mal formateados en tus solicitudes.

### Zona horaria ISO8601 <a href="#iso8601-datetime" id="iso8601-datetime"></a>

Discord utiliza el formato [ISO8601](https://www.loc.gov/standards/datetime/iso-tc154-wg5_n0038_iso_wd_8601-1_2016-02-16.pdf) para la mayoría de las fechas y horas retornadas en la API. Este formato se referencia como tipo dentro de las tablas en esta documentación.

### Consistencia <a href="#consistency" id="consistency"></a>

Discord opera a una escala en la que la verdadera consistencia es imposible. Por esta razón, muchas operaciones dentro de la API y entre los distintos servicios de la API son [eventualmente consistentes](https://en.wikipedia.org/wiki/Eventual_consistency). Debido a esto, las acciones del cliente nunca pueden ser serializadas y pueden ejecutarse en cualquier orden (o incluso no ejecutarse). Bajo estas restricciones, los eventos en Discord pueden:

* Nunca ser enviados a un cliente.
* Ser enviados exactamente una vez al cliente.
* Ser enviados hasta N veces a un mismo cliente.

Los clientes deben operar sobre los eventos y resultados recibidos de la API de manera lo más idempotente posible.

### API HTTP <a href="#http-api" id="http-api"></a>

**Agente de usuario**

Los clientes que utilizan la API HTTP deben proporcionar un [**agente de usuario**](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.43) válido que especifique información sobre el cliente. Para los bots, debe contener la cadena `DiscordBot`. Para cuentas de usuario, consulta la sección propiedades de usuario.

**Ejemplo de User Agent para bots de Discord:**

```
User-Agent: DiscordBot ($url, $versionNumber)
```

Los clientes pueden añadir más información y metadatos a esta cadena si lo desean.

**Limitación de velocidad**

La **API HTTP** implementa un proceso para limitar y prevenir solicitudes excesivas de acuerdo con [RFC 6585](https://tools.ietf.org/html/rfc6585#section-4). Los usuarios de la API que excedan e ignoren habitualmente los **límites** tendrán sus claves de API revocadas y serán bloqueados de la plataforma. Para más información sobre la **limitación de solicitudes**, consulta la sección de **Rate Limits**.

**Cadenas de consulta booleanas**

Ciertos endpoints en la API están documentados para aceptar booleanos[<sup>1</sup>](#user-content-fn-3)[^3] en sus parámetros de cadena de consulta.&#x20;Aunque no hay un sistema estándar para la representación booleana en los parámetros de la cadena de consulta, Discord representa estos casos usando <kbd>True</kbd>, <kbd>true</kbd> or <kbd>1</kbd> para verdadero y <kbd>False</kbd>, <kbd>false</kbd> o <kbd>0</kbd> para falso.

**Depuración**

La API HTTP acepta la cabecera opcional `X-Debug-Options` con una lista separada por comas de opciones de depuración. Las opciones pueden ser arbitrarias, pero las siguientes surten efecto:

| Valor  | Descripción                                             |
| ------ | ------------------------------------------------------- |
| canary | Fuerza el enrutamiento de la solicitud a la API canary. |
| trace  | Fuerza el rastreo de la solicitud con APM               |

Todas las respuestas incluyen la cabecera `X-Discord-Features` con el nombre de la funcionalidad interna de la API asociada.

**Rastreo APM**

Discord ofrece un servicio de **APM (Supervisión del Rendimiento de Aplicaciones)** que permite a los empleados de Discord ver los _traces_ (rastros) de las solicitudes realizadas a la API. Esto es útil para depurar problemas con la API, y puede activarse configurando `trace` en el encabezado `X-Debug-Options`, como se describió anteriormente.

Al rastrear solicitudes, debes proporcionar un encabezado adicional `X-Client-Trace-ID` con un identificador único para la solicitud. Este identificador puede generarse con el siguiente **pseudocódigo**:

{% code overflow="wrap" %}
```python
import base64
import random
import struct
import time

class IDGenerator:
    def __init__(self):
        self.prefix = random.randint(0, 0xFFFFFFFF) & 0xFFFFFFFF
        self.creation_time = int(time.time() * 1000)
        self.sequence = 0

    def generate(self, user_id: int = 0):
        uuid = bytearray(24)
        # Lowest signed 32 bits
        struct.pack_into("<I", uuid, 0, user_id & 0xFFFFFFFF)
        struct.pack_into("<I", uuid, 4, user_id >> 32)
        struct.pack_into("<I", uuid, 8, self.prefix)
        # Lowest signed 32 bits
        struct.pack_into("<I", uuid, 12, self.creation_time & 0xFFFFFFFF)
        struct.pack_into("<I", uuid, 16, self.creation_time >> 32)
        struct.pack_into("<I", uuid, 20, self.sequence)
        self.sequence += 1
        return base64.b64encode(uuid).decode("utf-8")

if __name__ == "__main__":
    generator = IDGenerator()
    # Use 0 for user_id if you don't have one
    generator.generate(user_id or 0)
```
{% endcode %}

Una vez que se completa la solicitud, el _trace_ puede visualizarse navegando a (reemplazando `<trace-id>` con el valor que proporcionaste).

### API del Gateway <a href="#gateway-api" id="gateway-api"></a>

La **Gateway API** de Discord se utiliza para mantener conexiones WebSocket seguras, persistentes y con estado entre tu cliente y los servidores de Discord. Estas conexiones se emplean para enviar y recibir eventos en tiempo real que permiten al cliente seguir y actualizar el estado local. La Gateway API utiliza conexiones seguras WebSocket según [RFC 6455](https://tools.ietf.org/html/rfc6455). Para obtener información sobre cómo abrir conexiones Gateway, consulta la sección Gateway API.

### Propiedades del cliente <a href="#client-properties" id="client-properties"></a>

Las propiedades del cliente, o **super properties**, contienen información de seguimiento sobre el cliente actual. Este seguimiento se utiliza para análisis y pruebas A/B. Estas propiedades se envían al identificarse con la Gateway y se incluyen en cada solicitud HTTP saliente con la cabecera `X-Super-Properties`, que es un objeto JSON codificado en base64.

Si estas propiedades no se proporcionan, Discord intentará extraer algunos de estos datos desde la cabecera `User-Agent` de la petición.

{% hint style="warning" %}
Aunque incluir esta cabecera no es obligatorio, es **altamente recomendable** debido a su importancia en los sistemas antiabuso. Además, muchas funciones experimentales requieren que se indique un número de compilación reciente del cliente.
{% endhint %}

**Propiedades de la estructura del cliente**

Debido a la naturaleza de las propiedades del cliente, la estructura de este objeto no está bien definida, y ningún campo es verdaderamente obligatorio. Además, los tipos de los campos no se verifican, y todos los _enums_ documentados son simplemente convenciones. Los campos se marcan como obligatorios si se observa que se envían en todas las propiedades oficiales del cliente.

<details>

<summary><strong>Tabla de propiedades del cliente</strong></summary>

<sup>1</sup> Estas propiedades se utilizan para activar funciones experimentales y pueden ser necesarias para ciertos endpoints. Se usa `client_version` en lugar de `client_build_number` para experimentos móviles. Consulta la documentación de experimentos para más información.

<sup>2</sup> Si se especifica, este valor debe coincidir con el encabezado <kbd>User-Agent</kbd> enviado por el cliente.

<sup>3</sup> Estas propiedades solo se envían al [identificarse](https://docs.discord.food/topics/gateway#identifying) con el Gateway y no se incluyen en el encabezado <kbd>X-Super-Properties</kbd>.

| Campo                                           | Tipo     | Descripción                                                                                                                                        |
| ----------------------------------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| os <sup>1</sup>                                 | string   | El sistema operativo del cliente.                                                                                                                  |
| os\_version?                                    | string   | La versión del sistema operativo (versión del kernel en Linux, versión SDK en Android).                                                            |
| os\_sdk\_version?                               | string   | La versión del SDK del sistema operativo.                                                                                                          |
| os\_arch?                                       | string   | La arquitectura del sistema operativo.                                                                                                             |
| app\_arch?                                      | string   | La arquitectura de la aplicación de escritorio.                                                                                                    |
| browser                                         | string   | El navegador que utiliza el cliente.                                                                                                               |
| browser\_user\_agent <sup>2</sup>               | string   | El user-agent del navegador del cliente (puede estar vacío en clientes móviles).                                                                   |
| browser\_version                                | string   | La versión del navegador del cliente (puede estar vacío en clientes móviles).                                                                      |
| client\_build\_number <sup>1</sup>              | integer  | El número de compilación del cliente.                                                                                                              |
| native\_build\_number?                          | ?integer | La versión de metadatos nativos del cliente de escritorio, si usa el sistema nuevo de actualizaciones.                                             |
| client\_version? <sup>1</sup>                   | string   | La versión del cliente móvil.                                                                                                                      |
| client\_event\_source?                          | ?string  | La fuente alternativa del evento desde la que se originó esta solicitud.                                                                           |
| client\_app\_state? <sup>3</sup>                | string   | El estado de enfoque del cliente cuando se inició la sesión del Gateway.                                                                           |
| client\_launch\_id?                             | string   | Un UUID generado por el cliente utilizado para identificar el lanzamiento del cliente.                                                             |
| client\_heartbeat\_session\_id?                 | ?string  | Un UUID generado por el cliente que representa el latido de análisis persistente actual, regenerado cada 30 minutos.                               |
| client\_performance\_cpu? **(obsoleto)**        | integer  | La utilización total de la CPU del dispositivo móvil (en porcentaje).                                                                              |
| client\_performance\_memory? **(obsoleto)**     | integer  | La utilización total de la memoria del dispositivo móvil (en kilobytes).                                                                           |
| cpu\_core\_count? **(obsoleto)**                | integer  | El número de núcleos de CPU disponibles para el dispositivo móvil.                                                                                 |
| release\_channel <sup>1</sup>                   | string   | El canal de lanzamiento del cliente.                                                                                                               |
| system\_locale                                  | string   | El idioma principal del sistema.                                                                                                                   |
| device?                                         | string   | El modelo del dispositivo móvil en el que el cliente se está ejecutando.                                                                           |
| device\_vendor\_id?                             | string   | Un identificador único para el dispositivo móvil (UUID en Android, IdentifierForVendor en iOS).                                                    |
| device\_advertiser\_id? **(obsoleto)**          | string   | El ID de anunciante del dispositivo móvil.                                                                                                         |
| design\_id?                                     | integer  | El ID de diseño del cliente.                                                                                                                       |
| accessibility\_support\_enabled? **(obsoleto)** | boolean  | Si el soporte de accesibilidad está habilitado.                                                                                                    |
| accessibility\_features? **(obsoleto)**         | integer  | Las características de accesibilidad habilitadas en el cliente.                                                                                    |
| window\_manager?                                | string   | El gestor de ventanas de Linux (`env.XDG_CURRENT_DESKTOP ?? "unknown" + "," + env.GDMSESSION ?? "unknown")`                                        |
| distro?                                         | string   | La distribución de Linux (`output of lsb_release -ds)`                                                                                             |
| referrer?                                       | string   | La URL que originalmente refirió al usuario a Discord.                                                                                             |
| referrer\_current?                              | string   | Igual que <kbd>referrer</kbd>, pero para la sesión actual.                                                                                         |
| referring\_domain?                              | string   | El dominio de la URL que originalmente refirió al usuario a Discord.                                                                               |
| referring\_domain\_current?                     | string   | Igual que <kbd>referring\_domain</kbd>, pero para la sesión actual.                                                                                |
| search\_engine?                                 | string   | El motor de búsqueda que originalmente refirió al usuario a Discord, analizado desde la URL de referencia.                                         |
| search\_engine\_current?                        | string   | Igual que <kbd>search\_engine</kbd>, pero para la sesión actual.                                                                                   |
| mp\_keyword?                                    | string   | La consulta del motor de búsqueda que originalmente refirió al usuario a Discord, analizado desde el parámetro `q` o `p` de la URL de referencia.  |
| mp\_keyword\_current?                           | string   | Igual que <kbd>mp\_keyword</kbd>, pero para la sesión actual.                                                                                      |
| utm\_campaign?                                  | string   | La campaña UTM que originalmente refirió al usuario a Discord, analizada desde el parámetro `utm_campaign` de la URL de referencia.                |
| utm\_campaign\_current?                         | string   | Igual que <kbd>utm\_campaign</kbd>, pero para la sesión actual.                                                                                    |
| utm\_content?                                   | string   | El contenido UTM que originalmente refirió al usuario a Discord, analizado desde el parámetro `utm_content` de la URL de referencia.               |
| utm\_content\_current?                          | string   | Igual que <kbd>utm\_content</kbd>, pero para la sesión actual.                                                                                     |
| utm\_medium?                                    | string   | El medio UTM que originalmente refirió al usuario a Discord, analizado desde el parámetro `utm_medium` de la URL de referencia.                    |
| utm\_medium\_current?                           | string   | Igual que <kbd>utm\_medium</kbd>, pero para la sesión actual.                                                                                      |
| utm\_source?                                    | string   | La fuente UTM que originalmente refirió al usuario a Discord, analizada desde el parámetro `utm_source` de la URL de referencia.                   |
| utm\_source\_current?                           | string   | Igual que <kbd>utm\_source</kbd>, pero para la sesión actual.                                                                                      |
| utm\_term?                                      | string   | El término UTM que originalmente refirió al usuario a Discord, analizado desde el parámetro `utm_term` de la URL de referencia.                    |
| utm\_term\_current?                             | string   | Igual que <kbd>utm\_term</kbd>, pero para la sesión actual.                                                                                        |
| has\_client\_mods?                              | boolean  | Si el cliente conectado tiene modificaciones (por ejemplo, Vencord).                                                                               |
| is\_fast\_connect? <sup>3</sup>                 | boolean  | Si la sesión del Gateway envió la identificación utilizando <kbd>fast connect</kbd> (un script de precarga se ejecutó antes de cargar el cliente). |
| version? <sup>3</sup>                           | string   | La versión del protocolo de propiedades del cliente.                                                                                               |



</details>

{% tabs fullWidth="true" %}
{% tab title="Sistema operativo" %}
| Valor          | Descripción                                        |
| -------------- | -------------------------------------------------- |
| Android        | El cliente funciona en Android.                    |
| BlackBerry     | El cliente funciona en BlackBerry OS.              |
| Mac OS X       | El cliente funciona en macOS.                      |
| iOS            | El cliente funciona en iOS.                        |
| Linux          | El cliente funciona con Linux distribution.        |
| Windows Mobile | El cliente funciona en Windows Mobile.             |
| Windows        | El cliente funciona en Windows.                    |
| Playstation    | El cliente funciona en PlayStation.                |
| Xbox           | El cliente funciona en Xbox.                       |
| Desconocido    | El cliente funciona en un dispositivo desconocido. |
{% endtab %}

{% tab title="Buscador" %}
| Valor              | Descripción                                         | Estado del tipo de cliente |
| ------------------ | --------------------------------------------------- | -------------------------- |
| Cliente de Discord | Cliente de ordenador                                |                            |
| Discord Android    | Cliente de Android                                  |                            |
| Discord iOS        | Cliente de iOS                                      |                            |
| Discord Embedded   | Cliente embedido (ej. Xbox)                         |                            |
| Android Chrome     | Google Chrome de Android                            |                            |
| Android Mobile     | Buscador Android genérico                           |                            |
| BlackBerry         | Buscador de BlackBerry                              |                            |
| Chrome             | Google Chrome de ordenador                          |                            |
| Chrome iOS         | Google Chrome de iOS                                |                            |
| ~~Edge~~           | ~~Microsoft Edge de escritorio (versión heredada)~~ |                            |
| Facebook Mobile    | Buscador de móvil de Facebook                       |                            |
| Firefox            | Mozilla Firefox                                     |                            |
| Internet Explorer  | Microsoft Internet Explorer                         |                            |
| Konqueror          | KDE Konqueror                                       |                            |
| Mobile Safari      | Safari iOS                                          |                            |
| Mozilla            | Buscador genérico con estilo Mozilla                |                            |
| Opera              | Opera                                               |                            |
| Opera Mini         | Opera Mini                                          |                            |
| Safari             | Safari de ordenador                                 |                            |
{% endtab %}

{% tab title="Estado app de cliente" %}
| Valor     | Descripción                                  |
| --------- | -------------------------------------------- |
| focused   | El cliente está enfocado (primer plano).     |
| unfocused | El cliente está desenfocado (segundo plano). |
{% endtab %}

{% tab title="Canal lanzamiento" %}
| Valor              | Descripción                                                                                                                          |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| stable             | Estable                                                                                                                              |
| ptb                | PTB                                                                                                                                  |
| canary             | Canary                                                                                                                               |
| staging            | Staging                                                                                                                              |
| internal           | Interno                                                                                                                              |
| googleRelease      | Estable de Google Play Store                                                                                                         |
| ~~samsungRelease~~ | ~~Estable de Samsung Galaxy Store~~                                                                                                  |
| billingRelease     | Facturación de Android (desconocido)                                                                                                 |
| betaRelease        | Beta de Android                                                                                                                      |
| canaryRelease      | Alpha de Android                                                                                                                     |
| internalRelease    | Lanzamiento solo para empleados internos                                                                                             |
| developerRelease   | Lanzamiento solo para desarrolladores internos                                                                                       |
| adhocRelease       | [Lanzamiento ad-hoc de iOS](https://developer.apple.com/la/help/account/provisioning-profiles/create-an-ad-hoc-provisioning-profile) |
| N/A                | Lanzamiento no aplicable (desconocido)                                                                                               |
| unknown            | Lanzamiento desconocido                                                                                                              |
{% endtab %}

{% tab title="Eventos cliente" %}
| Valor   | Descripción                                                |
| ------- | ---------------------------------------------------------- |
| OVERLAY | La solicitud se originó desde la superposición de Discord. |
{% endtab %}
{% endtabs %}

**ID de diseño**

| Valor | Nombre           | Descripción                             |
| ----- | ---------------- | --------------------------------------- |
| 0     | CLASSIC\_IA      | Diseño clásico (predeterminado).        |
| 1     | DESIGN\_IA       | Rediseño completo de móvil.             |
| 2     | DESIGN\_TABS\_IA | El rediseño móvil con pestañas.         |
| 3     | YOU\_BAR\_IA     | El rediseño móvil con la barra de 'tú'. |

**Banderas de características de accesibilidad**

<details>

<summary><strong>Banderas de características de accesibilidad</strong></summary>

| Valor   | Nombre                                  | Descripción                                                                                                        |
| ------- | --------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| 1 << 0  | SCREENREADER                            | El usuario tiene un lector de pantalla habilitado.                                                                 |
| 1 << 1  | REDUCED\_MOTION                         | El usuario tiene la opción de movimiento reducido habilitada.                                                      |
| 1 << 2  | REDUCED\_TRANSPARENCY                   | El usuario tiene la transparencia reducida habilitada.                                                             |
| 1 << 3  | HIGH\_CONTRAST                          | El usuario tiene el alto contraste habilitado.                                                                     |
| 1 << 4  | BOLD\_TEXT                              | El usuario tiene el texto en negrita habilitado.                                                                   |
| 1 << 5  | GRAYSCALE                               | El usuario tiene los colores en escala de grises habilitados.                                                      |
| 1 << 6  | INVERT\_COLORS                          | El usuario tiene los colores invertidos habilitados.                                                               |
| 1 << 7  | PREFERS\_COLOR\_SCHEME\_LIGHT           | El usuario prefiere un esquema de color claro.                                                                     |
| 1 << 8  | PREFERS\_COLOR\_SCHEME\_DARK            | El usuario prefiere un esquema de color oscuro.                                                                    |
| 1 << 9  | CHAT\_FONT\_SCALE\_INCREASED            | El usuario ha aumentado el tamaño de fuente del chat.                                                              |
| 1 << 10 | CHAT\_FONT\_SCALE\_DECREASED            | El usuario ha disminuido el tamaño de fuente del chat.                                                             |
| 1 << 11 | ZOOM\_LEVEL\_INCREASED                  | El usuario ha aumentado el nivel de zoom.                                                                          |
| 1 << 12 | ZOOM\_LEVEL\_DECREASED                  | El usuario ha disminuido el nivel de zoom.                                                                         |
| 1 << 13 | MESSAGE\_GROUP\_SPACING\_INCREASED      | El usuario ha aumentado el espaciado entre grupos de mensajes.                                                     |
| 1 << 14 | MESSAGE\_GROUP\_SPACING\_DECREASED      | El usuario ha disminuido el espaciado entre grupos de mensajes.                                                    |
| 1 << 15 | DARK\_SIDEBAR                           | El usuario tiene una barra lateral oscura habilitada.                                                              |
| 1 << 16 | REDUCED\_MOTION\_FROM\_USER\_SETTINGS   | El usuario ha habilitado el movimiento reducido explícitamente desde la configuración del usuario.                 |
| 1 << 17 | SATURATION\_LEVEL\_DECREASED            | El usuario ha disminuido el nivel de saturación.                                                                   |
| 1 << 18 | FORCED\_COLORS                          | El usuario tiene habilitados colores forzados de alto contraste del sistema.                                       |
| 1 << 19 | FORCED\_COLORS\_FROM\_USER\_SETTINGS    | El usuario tiene habilitados colores forzados de alto contraste explícitamente desde la configuración del usuario. |
| 1 << 20 | ROLE\_STYLE\_ADJUSTED                   | El usuario ha ajustado los estilos de roles.                                                                       |
| 1 << 21 | SYNC\_PROFILE\_THEME\_WITH\_USER\_THEME | El usuario ha habilitado la sincronización del tema del perfil de usuario con el del cliente.                      |
| 1 << 22 | REDUCED\_MOTION\_PREFERS\_CROSSFADES    | El usuario tiene movimiento reducido habilitado y prefiere fundidos cruzados.                                      |
| 1 << 23 | CONTRAST\_LEVEL\_INCREASED              | El usuario ha aumentado el nivel de contraste.                                                                     |
| 1 << 24 | CONTRAST\_LEVEL\_DECREASED              | El usuario ha disminuido el nivel de contraste.                                                                    |



</details>

**Motores de búsqueda**

| Valor      | Descripción                                                 |
| ---------- | ----------------------------------------------------------- |
| Google     | [Motor de búsqueda de Google](https://www.google.com/).     |
| Bing       | [Motor de búsqueda de Bing](https://www.bing.com/).         |
| Yahoo      | [Motor de búsqueda de Yahoo](https://www.yahoo.com/).       |
| DuckDuckGo | [Motor de búsqueda de DuckDuckGo](https://duckduckgo.com/). |

**Ejemplos de propiedades de cliente**

{% tabs fullWidth="true" %}
{% tab title="Web" %}
{% code overflow="wrap" %}
```json
{
  "os": "Windows",
  "browser": "Chrome",
  "device": "",
  "system_locale": "en-US",
  "browser_user_agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/136.0.0.0 Safari/537.36",
  "browser_version": "136.0.0.0",
  "os_version": "10",
  "referrer": "https://www.reddit.com/",
  "referring_domain": "www.reddit.com",
  "referrer_current": "https://www.google.com/",
  "referring_domain_current": "www.google.com",
  "search_engine_current": "google",
  "mp_keyword_current": "discord",
  "release_channel": "stable",
  "client_build_number": 396858,
  "client_event_source": null,
  "has_client_mods": false,
  "client_launch_id": "9a65c85b-401d-4cf0-9c17-9676a68a482c",
  "client_heartbeat_session_id": "2fc3cabd-3ca0-4716-b399-496780c302d1"
}
```
{% endcode %}
{% endtab %}

{% tab title="Windows" %}
{% code overflow="wrap" %}
```json
{
  "os": "Windows",
  "browser": "Discord Client",
  "release_channel": "canary",
  "client_version": "1.0.328",
  "os_version": "10.0.26100",
  "os_arch": "x64",
  "app_arch": "x64",
  "system_locale": "en-US",
  "has_client_mods": false,
  "client_launch_id": "466a709c-1d91-442d-a35f-34e8c834736e",
  "browser_user_agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) discord/1.0.328 Chrome/134.0.6998.179 Electron/35.1.5 Safari/537.36",
  "browser_version": "35.1.5",
  "os_sdk_version": "26100",
  "client_build_number": 397417,
  "native_build_number": 63309,
  "client_event_source": null,
  "client_heartbeat_session_id": "b789aacc-2579-489a-9dc8-2aa440519ae6"
}
```
{% endcode %}
{% endtab %}

{% tab title="MacOS" %}
{% code overflow="wrap" %}
```json
{
  "os": "Mac OS X",
  "browser": "Discord Client",
  "release_channel": "ptb",
  "client_version": "0.0.171",
  "os_version": "24.2.0",
  "os_arch": "arm64",
  "app_arch": "arm64",
  "system_locale": "en-US",
  "has_client_mods": false,
  "client_launch_id": "c1f90baa-5390-43bd-a5eb-36a77d0c17c1",
  "browser_user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) discord/0.0.171 Chrome/134.0.6998.179 Electron/35.1.5 Safari/537.36",
  "browser_version": "35.1.5",
  "os_sdk_version": "24",
  "client_build_number": 397030,
  "native_build_number": null,
  "client_event_source": null,
  "client_heartbeat_session_id": "63efcf64-bfd4-48b3-bdb6-57e643c7f1e0"
}
```
{% endcode %}
{% endtab %}

{% tab title="Linux" %}
{% code overflow="wrap" %}
```json
{
  "os": "Linux",
  "browser": "Discord Client",
  "release_channel": "canary",
  "client_version": "0.0.670",
  "os_version": "5.15.153.1-microsoft-standard-WSL2",
  "os_arch": "x64",
  "app_arch": "x64",
  "system_locale": "en-US",
  "has_client_mods": false,
  "client_launch_id": "ee3d1bb9-761a-426e-a9e4-4962295ca5df",
  "browser_user_agent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) discord/0.0.670 Chrome/134.0.6998.179 Electron/35.1.5 Safari/537.36",
  "browser_version": "35.1.5",
  "window_manager": "Hyprland,unknown",
  "distro": "Ubuntu 24.04.4 LTS",
  "client_build_number": 397417,
  "native_build_number": null,
  "client_event_source": null,
  "client_heartbeat_session_id": "9345ac0b-2bbb-43f8-89c1-5cf190fbb61d"
}
```
{% endcode %}
{% endtab %}

{% tab title="Android" %}
{% hint style="info" %}
Dada una versión de cliente <kbd>280.2</kbd> y un canal de lanzamiento <kbd>alpha</kbd>, la versión del user agent se calcula así:\
<kbd>280</kbd> para la versión mayor + <kbd>0/1/2</kbd> (según sea stable/beta/alpha) + <kbd>02</kbd> para la versión menor = <kbd>280202</kbd>.
{% endhint %}

{% code overflow="wrap" %}
```json
{
  "os": "Android",
  "browser": "Discord Android",
  "device": "a20e", // Samsung Galaxy A20e
  "system_locale": "en-US",
  "has_client_mods": false,
  "client_version": "280.2 - rn",
  "release_channel": "alpha",
  "device_vendor_id": "17503929-a4b8-4490-87bf-0222adfdadc8",
  "design_id": 2,
  "browser_user_agent": "", // Aunque no se proporciona aquí, el encabezado User-Agent es Discord-Android/280202;RNA
  "browser_version": "",
  "os_version": "34", // Android 14
  "client_build_number": 4025,
  "client_event_source": null,
  "client_launch_id": "9a65c85b-401d-4cf0-9c17-9676a68a482c",
  "client_heartbeat_session_id": "2fc3cabd-3ca0-4716-b399-496780c302d1"
}
```
{% endcode %}
{% endtab %}

{% tab title="iOS" %}
{% code overflow="wrap" %}
```json
{
  "os": "iOS",
  "browser": "Discord iOS",
  "device": "iPhone14,5", // iPhone 13
  "system_locale": "en-US",
  "has_client_mods": false,
  "client_version": "227.0",
  "release_channel": "stable",
  "device_vendor_id": "AFF0710F-CEC0-4671-B2CF-0A03D72B5FD0",
  "design_id": 2,
  "browser_user_agent": "", // Aunque no se proporciona aquí, el encabezado User-Agent es Discord/58755 CFNetwork/1494.0.7 Darwin/23.4.0
  "browser_version": "",
  "os_version": "17.4.1",
  "client_build_number": 58755,
  "client_event_source": null,
  "client_launch_id": "9a65c85b-401d-4cf0-9c17-9676a68a482c",
  "client_heartbeat_session_id": "2fc3cabd-3ca0-4716-b399-496780c302d1"
}
```
{% endcode %}
{% endtab %}

{% tab title="Embedded" %}
{% code overflow="wrap" %}
```json
{
  "browser": "Discord Embedded",
  "browser_user_agent": "Discord Embedded/0.0.8",
  "browser_version": "0.0.8",
  "client_build_number": 4440,
  "design_id": 0,
  "os": "Windows",
  "release_channel": "unknown"
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Formato de mensajes <a href="#message-formatting" id="message-formatting"></a>

Discord utiliza un subconjunto de markdown para renderizar el contenido de los mensajes en sus clientes, mientras que también agrega algunas funcionalidades personalizadas para habilitar cosas como mencionar usuarios y canales. Esta funcionalidad usa los siguientes formatos:

**Formatos**

| Tipo                              | Estructura                    | Ejemplo                         |
| --------------------------------- | ----------------------------- | ------------------------------- |
| Usuario                           | `<@USER_ID>`                  | `<@80351110224678912>`          |
| Usuario <sup>1</sup>              | `<@!USER_ID>`                 | `<@!80351110224678912>`         |
| Canal                             | `<#CHANNEL_ID>`               | `<#103735883630395392>`         |
| Rol                               | `<@&ROLE_ID>`                 | `<@&165511591545143296>`        |
| Comando de barra <sup>2</sup>     | `</NAME:COMMAND_ID>`          | `</airhorn:816437322781949972>` |
| Emoji predeterminado              | Carácteres unicode o `:NAME:` | 💯 or `:100:`                   |
| Emoji personalizado               | `<:NAME:ID>`                  | `<:mmLol:216154654256398347>`   |
| Emoji personalizado (animado)     | `<a:NAME:ID>`                 | `<a:b1nzy:392938283556143104>`  |
| Marca de tiempo UNIX              | `<t:TIMESTAMP>`               | `<t:1618953630>`                |
| Marca de tiempo UNIX (estilizada) | `<t:TIMESTAMP:STYLE>`         | `<t:1618953630:d>`              |
| Navegación de servidor            | `<id:TYPE>`                   | `<id:customize>`                |
| Correo <sup>3</sup>               | `<USERNAME@DOMAIN>`           | `<advaith@discordapp.com>`      |
| Número de teléfono <sup>4</sup>   | `<+PHONE_NUMBER>`             | `<+1 (555) 123 4567>`           |

Usar el markdown para usuarios, roles o canales normalmente mencionará al/los destinatarios correspondientes, pero esto se puede suprimir utilizando el parámetro `allowed_mentions` al crear un mensaje. Los emoji estándar actualmente se renderizan usando [Twemoji](https://emojipedia.org/es/twitter) en escritorio/Android y los emoji nativos de Apple en iOS.

> Las marcas de tiempo se expresan en segundos y muestran la marca temporal indicada en la zona horaria y el idioma del usuario.

<sup>1</sup> Las menciones a usuarios con signo de exclamación están obsoletas y deben manejarse igual que cualquier otra mención de usuario.

<sup>2</sup> Los subcomandos y grupos de subcomandos también se pueden mencionar usando respectivamente `</NOMBRE SUBCOMANDO:ID>` y `</NOMBRE GRUPO_SUBCOMANDO SUBCOMANDO:ID>`.

<sup>3</sup> Puede ir opcionalmente precedido por `mailto:`, siguiendo el mismo formato que el esquema de URI [mailto:](https://en.wikipedia.org/wiki/Mailto). No se admiten varias direcciones separadas por comas. Soporta cabeceras (ejemplo: `<advaith@discord.com?subject=Título&body=Contenido%20del%20mensaje>`).

<sup>4</sup> Puede ir opcionalmente precedido por `tel:`/`sms:`. Se ignoran los espacios en blanco y no es necesario el prefijo de marcación `+` cuando se proporciona un esquema.

**Estilos de marcas de tiempo**

| Estilo         | Ejemplo de salida            | Descripción      |
| -------------- | ---------------------------- | ---------------- |
| t              | 16:20                        | Hora corta       |
| T              | 16:20:30                     | Hora larga       |
| d              | 20/04/2021                   | Fecha corta      |
| D              | 20 April 2021                | Fecha larga      |
| f <sup>1</sup> | 20 April 2021 16:20          | Fecha/Hora corta |
| F              | Tuesday, 20 April 2021 16:20 | Fecha/Hora larga |
| R              | 2 months ago                 | Tiempo relativo  |

<sup>1</sup> Este es el predeterminado.

**Tipos de navegación de servidor**

Los tipos de navegación de la guild enlazan al recurso correspondiente dentro de la guild actual.

| Tipo                  | Descripción                                                                                                        |
| --------------------- | ------------------------------------------------------------------------------------------------------------------ |
| customize             | _Pestaña personalizada_ con los mensajes de incorporación del servidor.                                            |
| browse                | Pestaña de explorar canales.                                                                                       |
| guide                 | [Guía del servidor](https://support.discord.com/hc/es/articles/13497665141655-Gu%C3%ADa-del-servidor)              |
| home **(obsoleto)**   | Igual que la `guía`.                                                                                               |
| linked-roles          | [Roles vinculados](https://support.discord.com/hc/es/articles/10388356626711-Conexiones-y-roles-vinculados-Admins) |
| linked-roles:ROLE\_ID | _Conexión de rol_ vinculado.                                                                                       |

### **Formato de CDN**

**Enlace base de CDN**

```
https://cdn.discordapp.com/
```

Discord utiliza IDs y _hashes_ para renderizar imágenes y otros contenidos del CDN en el cliente. Estos _hashes_ pueden recuperarse a través de diversas solicitudes a la API, como **Get User**. Debajo se presentan los formatos, limitaciones de tamaño y _endpoints_ del CDN para el contenido en Discord. El formato de los archivos devueltos puede modificarse cambiando la extensión al final de la URL. Para las imágenes, el tamaño devuelto puede ajustarse añadiendo una cadena de consulta `?size=desired_size` a la URL. El tamaño de la imagen puede ser cualquier potencia de dos entre **16 y 4096**, con algunos valores adicionales aceptados que se detallan más abajo.

En el caso de los _endpoints_ que admiten imágenes animadas, el _hash_ comenzará con `a_` si está disponible en formato **APNG** o **GIF** (por ejemplo: `a_1269e74af4df7417b13759eae50c83dc`).

**Formato de archivos**

| Nombre | Extensión   |
| ------ | ----------- |
| JPEG   | .jpg, .jpeg |
| PNG    | .png        |
| SVG    | .svg        |
| WebP   | .webp       |
| WebM   | .webm       |
| GIF    | .gif        |
| Lottie | .json       |
| MP3    | .mp3        |
| MP4    | .mp4        |
| OGG    | .ogg        |

**Endpoints de CDN**

{% tabs %}
{% tab title="Endpoints de CDN" %}
<sup>1</sup> El valor de `user_index` en la ruta para usuarios migrados debe ser el ID del usuario desplazado 22 bits a la derecha, módulo 6 (es decir, `user_id >> 22 % 6`).\
El valor para usuarios no migrados es el _discriminator_ del usuario módulo 5 (por ejemplo, `Test#1337` sería `1337 % 5`, lo que da **2**).\
El valor para equipos debe ser el ID del equipo módulo 5 (es decir, `team_id % 5`). Consulta la sección sobre el nuevo sistema de nombres de usuario de Discord para más información.

<sup>2</sup> El tamaño de las imágenes devueltas es constante, y el parámetro `size` en la cadena de consulta es ignorado.

<sup>3</sup> La pegatina estará disponible como **PNG** si su `format_type` es **PNG** o **APNG**, como **GIF** si su `format_type` es **GIF**, y como **Lottie** si su `format_type` es **LOTTIE**.\
Las pegatinas en formato GIF no están disponibles a través del CDN y deben accederse en:\
`https://media.discordapp.net/stickers/{sticker_id}.gif`

<sup>4</sup> El archivo adjunto solo estará disponible en el formato en el que fue subido, el cual puede ser cualquier formato.

<sup>5</sup> La ruta del tema solo se utiliza para obtener los recursos `game_tile` y `logotype`. Solo se admiten los temas `dark` y `light`.

<sup>6</sup> Los nombres de recursos válidos están limitados a: `img.png`, `static.png`, `asset.webm`.

<sup>7</sup> La ruta del emblema de clan es un alias de la ruta del emblema de etiqueta de servidor (_guild tag badge_), y existe por compatibilidad con versiones anteriores.

| Tipo                                                       | Ruta                                                                                                                                                                                                                                                                                                                                                                                                                  | Soporta                                              |
| ---------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| Recurso de enlace de actividad                             | /attachments-quick-links/[{asset\_path}](https://docs.discord.food/resources/application#activity-link-object)                                                                                                                                                                                                                                                                                                        | Uploaded format <sup>4</sup>                         |
| Icono de logro                                             | /app-assets/[{application\_id}](https://docs.discord.food/resources/application#application-object)/achievements/{achievement\_id}/icons/icon\_hash.png                                                                                                                                                                                                                                                               | PNG, JPEG, WebP                                      |
| Icono de aplicación                                        | /app-icons/[{application\_id}](https://docs.discord.food/resources/application#application-object)/[{application\_icon}](https://docs.discord.food/resources/application#application-object).png                                                                                                                                                                                                                      | PNG, JPEG, WebP                                      |
| Portada de aplicación                                      | /app-icons/[{application\_id}](https://docs.discord.food/resources/application#application-object)/[{application\_cover\_image}](https://docs.discord.food/resources/application#application-object).png                                                                                                                                                                                                              | PNG, JPEG, WebP                                      |
| Splash de aplicación                                       | /app-icons/[{application\_id}](https://docs.discord.food/resources/application#application-object)/[{application\_splash}](https://docs.discord.food/resources/application#application-object).png                                                                                                                                                                                                                    | PNG, JPEG, WebP                                      |
| Recurso de aplicación                                      | /app-assets/[{application\_id}](https://docs.discord.food/resources/application#application-object)/[{asset\_id}](https://docs.discord.food/resources/application#application-asset-object).png                                                                                                                                                                                                                       | PNG, JPEG, WebP                                      |
| Colección de directorio de aplicaciones                    | /application-directory/collection-items/{item\_id}/{item\_hash}.png                                                                                                                                                                                                                                                                                                                                                   | PNG, JPEG, WebP                                      |
| Adjunto <sup>2</sup>                                       | /attachments/[{channel\_id}](https://docs.discord.food/resources/channel#channel-object)/\[{message\_id]/[attachment\_id}](https://docs.discord.food/resources/message#attachment-object)/[{attachment\_filename}](https://docs.discord.food/resources/message#attachment-object)                                                                                                                                     | Formato de subida <sup>4</sup>                       |
| Adjunto efímero <sup>2</sup>                               | /ephemeral-attachments/[{application\_id}](https://docs.discord.food/resources/application#application-object)/[{attachment\_id}](https://docs.discord.food/resources/message#attachment-object)/[{attachment\_filename}](https://docs.discord.food/resources/message#attachment-object)                                                                                                                              | Formato de subida <sup>4</sup>                       |
| Preset de decoración de avatar                             | /avatar-decoration-presets/[{avatar\_decoration\_data\_asset}](https://docs.discord.food/resources/user#avatar-decoration-data-structure).png                                                                                                                                                                                                                                                                         | PNG, JPEG, WebP                                      |
| Icono de canal                                             | /channels/[{channel\_id}](https://docs.discord.food/resources/channel#channel-object)/icons/[{channel\_icon}](https://docs.discord.food/resources/channel#channel-object).png                                                                                                                                                                                                                                         | PNG, JPEG, WebP                                      |
| Insignia de servidor <sup>7</sup>                          | /clan-badges/[{guild\_id}](https://docs.discord.food/resources/guild#guild-object)/[{badge\_hash}](https://docs.discord.food/resources/discovery#guild-profile-object).png                                                                                                                                                                                                                                            | PNG                                                  |
| Banner de servidor                                         | /clan-banners/[{guild\_id}](https://docs.discord.food/resources/guild#guild-object)/[{banner\_hash}](https://docs.discord.food/resources/discovery#guild-profile-object).png                                                                                                                                                                                                                                          | PNG                                                  |
| Emoji personalizado                                        | /emojis/[{emoji\_id}](https://docs.discord.food/resources/emoji#emoji-object).png                                                                                                                                                                                                                                                                                                                                     | PNG, JPEG, WebP, GIF                                 |
| Icono de servidor                                          | /icons/[{guild\_id}](https://docs.discord.food/resources/guild#guild-object)/[{guild\_icon}](https://docs.discord.food/resources/guild#guild-object).png                                                                                                                                                                                                                                                              | PNG, JPEG, WebP, GIF                                 |
| Imagen de bienvenida del servidor                          | /splashes/[{guild\_id}](https://docs.discord.food/resources/guild#guild-object)/[{guild\_splash}](https://docs.discord.food/resources/guild#guild-object).png                                                                                                                                                                                                                                                         | PNG, JPEG, WebP                                      |
| Imagen de descubrimiento del servidor                      | /discovery-splashes/[{guild\_id}](https://docs.discord.food/resources/guild#guild-object)/[{guild\_discovery\_splash}](https://docs.discord.food/resources/guild#guild-object).png                                                                                                                                                                                                                                    | PNG, JPEG, WebP                                      |
| Banner de servidor                                         | /banners/[{guild\_id}](https://docs.discord.food/resources/guild#guild-object)/[{guild\_banner}](https://docs.discord.food/resources/guild#guild-object).png                                                                                                                                                                                                                                                          | PNG, JPEG, WebP, GIF                                 |
| Encabezado de inicio del servidor                          | /guilds/[{guild\_id}](https://docs.discord.food/resources/guild#guild-object)/home-headers/[{guild\_home\_header}](https://docs.discord.food/resources/guild#guild-object).png                                                                                                                                                                                                                                        | PNG, JPEG, WebP                                      |
| Portada de evento programado del servidor                  | /guild-events/[{scheduled\_event\_id}](https://docs.discord.food/resources/guild-scheduled-event#guild-scheduled-event-object)/[{scheduled\_event\_cover\_image}](https://docs.discord.food/resources/guild-scheduled-event#guild-scheduled-event-object).png                                                                                                                                                         | PNG, JPEG, WebP                                      |
| Avatar de miembro del servidor                             | /guilds/[{guild\_id}](https://docs.discord.food/resources/guild#guild-object)/users/[{user\_id}](https://docs.discord.food/resources/user#user-object)/avatars/[{user\_avatar}](https://docs.discord.food/resources/user#user-object).png                                                                                                                                                                             | PNG, JPEG, WebP, GIF                                 |
| Banner de miembro del servidor                             | /guilds/[{guild\_id}](https://docs.discord.food/resources/guild#guild-object)/users/[{user\_id}](https://docs.discord.food/resources/user#user-object)/banners/[{user\_banner}](https://docs.discord.food/resources/user#user-object).png                                                                                                                                                                             | PNG, JPEG, WebP, GIF                                 |
| Icono de acción de nuevo miembro del servidor              | /new-member-actions/[{channel\_id}](https://docs.discord.food/resources/guild#new-member-action-structure)/[{action\_icon}](https://docs.discord.food/resources/guild#new-member-action-structure).png                                                                                                                                                                                                                | PNG, JPEG, WebP                                      |
| Icono de canal de recursos del servidor                    | /resource-channels/[{channel\_id}](https://docs.discord.food/resources/guild#resource-channel-structure)/[{channel\_icon}](https://docs.discord.food/resources/guild#resource-channel-structure).png                                                                                                                                                                                                                  | PNG, JPEG, WebP                                      |
| Insignia de etiqueta de servidor <sup>7</sup>              | /guild-tag-badges/[{guild\_id}](https://docs.discord.food/resources/guild#guild-object)/[{badge\_hash}](https://docs.discord.food/resources/discovery#guild-profile-object).png                                                                                                                                                                                                                                       | PNG                                                  |
| Placa de nombre                                            | /assets/collectibles/[{asset\_path}](https://docs.discord.food/resources/user#nameplate-data-structure){asset\_name} <sup>6</sup>                                                                                                                                                                                                                                                                                     | PNG, WebM                                            |
| Insignia de perfil                                         | /badge-icons/[{badge\_icon}](https://docs.discord.food/resources/user#profile-badge-structure).png                                                                                                                                                                                                                                                                                                                    | PNG                                                  |
| Recurso de misión <sup>5</sup>                             | /assets/quests/[{quest\_id}](https://docs.discord.food/resources/quests#quest-object)/[{asset\_name}](https://docs.discord.food/resources/quests#quest-assets-structure), /assets/quests/[{quest\_id}](https://docs.discord.food/resources/quests#quest-object)/[{theme}](https://docs.discord.food/resources/user-settings#theme)/[{asset\_name}](https://docs.discord.food/resources/quests#quest-assets-structure) | PNG, JPEG, GIF, SVG, WebP, WebM, MP4, M3U8, VTT, TXT |
| Icono de rol                                               | /roles/[{role\_id}](https://docs.discord.food/resources/guild#role-object)/icons/[{role\_icon}](https://docs.discord.food/resources/guild#role-object).png                                                                                                                                                                                                                                                            | PNG, JPEG, WebP                                      |
| Sonido de soundboard                                       | /soundboard-sounds/{sound\_id}                                                                                                                                                                                                                                                                                                                                                                                        | MP3, OGG                                             |
| Sticker <sup>2</sup> <sup>3</sup>                          | /stickers/[{sticker\_id}](https://docs.discord.food/resources/sticker#sticker-object).png                                                                                                                                                                                                                                                                                                                             | PNG, Lottie, GIF                                     |
| Banner de paquete de stickers                              | /app-assets/710982414301790216/store/[{asset\_id}](https://docs.discord.food/resources/sticker#sticker-pack-object).png                                                                                                                                                                                                                                                                                               | PNG, JPEG, WebP                                      |
| Vista previa de stream                                     | /streams/[{stream\_key}](https://docs.discord.food/topics/gateway-events#stream-key)/{thumbnail\_hash}.png                                                                                                                                                                                                                                                                                                            | PNG, JPEG, WebP                                      |
| Icono de equipo                                            | /team-icons/[{team\_id}](https://docs.discord.food/resources/team#team-object)/[{team\_icon}](https://docs.discord.food/resources/team#team-object).png                                                                                                                                                                                                                                                               | PNG, JPEG, WebP                                      |
| Avatar de usuario                                          | /avatars/[{user\_id}](https://docs.discord.food/resources/user#user-object)/[{user\_avatar}](https://docs.discord.food/resources/user#user-object).png                                                                                                                                                                                                                                                                | PNG, JPEG, WebP, GIF                                 |
| Avatar archivado de usuario                                | /avatars/[{user\_id}](https://docs.discord.food/resources/user#user-object)/archived/[{avatar\_id}](https://docs.discord.food/resources/user#avatar-structure)/[{avatar\_storage\_hash}](https://docs.discord.food/resources/user#avatar-structure).png                                                                                                                                                               | PNG, JPEG, WebP, GIF                                 |
| Avatar predeterminado de usuario <sup>1</sup> <sup>2</sup> | /embed/avatars/[{user\_index}](https://docs.discord.food/resources/user#user-object).png                                                                                                                                                                                                                                                                                                                              | PNG                                                  |
| Banner de usuario                                          | /banners/[{user\_id}](https://docs.discord.food/resources/user#user-object)/[{user\_banner}](https://docs.discord.food/resources/user#user-object).png                                                                                                                                                                                                                                                                | PNG, JPEG, WebP, GIF                                 |
| Filtro de video                                            | /users/[{user\_id}](https://docs.discord.food/resources/user#user-object)/video-filter-assets/[{video\_filter\_id}](https://docs.discord.food/resources/user-settings-proto#video-filter-asset-structure)/[{video\_filter\_asset}](https://docs.discord.food/resources/user-settings-proto#video-filter-asset-structure).png                                                                                          | PNG, JPEG, GIF, MP4, WEBP                            |
{% endtab %}

{% tab title="Parámetros de CDN" %}
El CDN admite varios parámetros de consulta. Estos parámetros son opcionales y pueden usarse para modificar el recurso que se devuelve. Los parámetros solo son aplicables a recursos de imagen.

<sup>1</sup> El CDN solo acepta parámetros booleanos como `true` o `false` (no como `1` o `0`).

| Campo                | Tipo                 | Descripción                                                                                                                                                                                                                                                          |
| -------------------- | -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| size?                | integer              | El tamaño de la imagen a devolver (si se omite, se usa un tamaño por defecto); el tamaño puede ser cualquier potencia de dos entre 16 y 4096, y adicionalmente 20, 22, 24, 28, 40, 44, 48, 56, 60, 80, 96, 100, 160, 240, 300, 320, 480, 600, 640, 1280, 1536, 3072. |
| quality?             | string               | La calidad de la imagen a devolver; no es compatible con todos los endpoints ni con todos los tipos de imagen.                                                                                                                                                       |
| keep\_aspect\_ratio? | boolean <sup>1</sup> | Si la imagen será redimensionada a la relación de aspecto forzada por el endpoint (por defecto, false).                                                                                                                                                              |
| passthrough?         | boolean <sup>1</sup> | Si la imagen será devuelta en la calidad y formato original definido por Discord (normalmente APNG) si es posible (por defecto, true); solo soportado en endpoints específicos.                                                                                      |
| animated?            | boolean <sup>1</sup> | Si la imagen será devuelta en su formato animado (si aplica) (por defecto, false); solo soportado con el formato WebP.                                                                                                                                               |
{% endtab %}
{% endtabs %}

**Calidad de imagen**

La calidad de la imagen a devolver. Solo es compatible con los formatos WebP y JPG (el resto siempre son sin pérdida). La calidad puede ser cualquiera de los siguientes valores:

| Valor    | Descripción                                                            |
| -------- | ---------------------------------------------------------------------- |
| lossless | La imagen se devolverá en su formato original, sin pérdida de calidad. |
| high     | La imagen se devolverá en formato de alta calidad (por defecto).       |
| low      | La imagen se devolverá en formato de baja calidad.                     |

**Enlaces firmados de adjuntos**

Los archivos adjuntos subidos al CDN de Discord (como imágenes de usuario) tienen URLs firmadas con un tiempo de caducidad predefinido. Esto incluye archivos adjuntos efímeros. Discord actualiza automáticamente las URLs de adjuntos que aparecen en el cliente, así que cuando recibas un payload con una URL firmada (por ejemplo, al hacer fetch de un mensaje), será válida.

Al pasar URLs del CDN en campos de la API, como url en un objeto de imagen de embed o avatar\_url para webhooks, o incluso al enviar un mensaje, puedes pasar la URL del CDN sin parámetros adicionales y Discord la renderizará y actualizará automáticamente.

Si necesitas refrescar manualmente una URL de adjunto, puedes usar el endpoint Refresh Attachment URLs con cualquier URL.

Los demás endpoints del CDN mencionados anteriormente no están firmados, por lo que no expiran.

**Parámetros de consulta de enlace adjunto**

| Parámetro | Descripción                                                                             |
| --------- | --------------------------------------------------------------------------------------- |
| ex        | Marca de tiempo en hexadecimal que indica cuándo expirará la URL del adjunto en el CDN. |
| is        | Marca de tiempo en hexadecimal que indica cuándo se emitió la URL.                      |
| hm        | Firma única que permanece válida hasta que expire la URL.                               |

**Ejemplo de enlace de archivo**

```
https://cdn.discordapp.com/attachments/1012345678900020080/1234567891233211234/my_image.png?ex=65d903de&is=65c68ede&hm=2481f30dd67f503f54d020ae3b5533b9987fae4e55f2b4e3926e08a3fa3ee24f&
```

### CDN Data <a href="#cdn-data" id="cdn-data"></a>

El campo "CDN data" utiliza un [esquema de URI](https://es.wikipedia.org/wiki/Esquema_de_URI_de_datos) de tipo data para representar archivos como JPG, GIF y PNG. Esto permite incrustar la imagen directamente en el valor, por ejemplo, para imágenes de perfil de usuario, usando una URI con la siguiente estructura:

{% code overflow="wrap" %}
```python
data: image / jpeg;
base64, BASE64_ENCODED_JPEG_IMAGE_DATA;
```
{% endcode %}

Es importante asegurarse de usar el tipo de contenido apropiado como `image/jpeg`, `image/png`, o `image/gif`, que coincida exactamente con el tipo de datos de imagen suministrada

### Subida de archivos <a href="#uploading-files" id="uploading-files"></a>

<details>

<summary>Límite de subida<br><sub>¿Cómo es calculado el límite de tamaño de archivos?</sub></summary>

El límite de tamaño para subir archivos se aplica a **cada archivo** de una solicitud. El límite predeterminado es **10 MiB** para todos los usuarios, pero puede ser mayor dependiendo de tu [tipo de suscripción premium](https://support.discord.com/hc/en-us/articles/115000435108) o del [nivel premium del servidor](https://support.discord.com/hc/en-us/articles/360028038352-Server-Boosting-FAQ), el que sea más alto.

{% hint style="info" %}
Ten en cuenta que **Cloudflare** impone un tamaño máximo de solicitud inferior a **200 MiB**.\
Si excedes este límite, la subida fallará con el error `413 Payload Too Large`. Para subir archivos mayores a 200 MiB, debes utilizar las subidas a **Google Cloud**.
{% endhint %}

| Recursos        | Nivel    | Límite      |
| --------------- | -------- | ----------- |
| Nitro Clásico   | `TIER_1` | **50 MiB**  |
| Nitro Booster   | `TIER_2` | **500 MiB** |
| Nitro Básico    | `TIER_3` | **50 MiB**  |
| Servidor Tier 2 | `TIER_2` | **50 MiB**  |
| Servidor Tier 3 | `TIER_3` | **100 MiB** |

</details>

Algunos endpoints admiten adjuntar archivos, lo cual se indica mediante el parámetro `files[n]`. Para añadir uno o más archivos, el cuerpo estándar `application/json` debe ser reemplazado por un cuerpo `multipart/form-data`. El cuerpo JSON del mensaje puede proporcionarse opcionalmente utilizando el parámetro `payload_json`.

Todos los parámetros `files[n]` deben incluir una subcabecera válida `Content-Disposition` con un `filename` y un parámetro `name` único. Cada archivo debe tener un nombre único con el formato `files[n]`, como `files`, `files[1]` o `files`. El sufijo `n` actúa como un marcador de posición (_snowflake placeholder_) que puede utilizarse en el campo `attachments`, el cual puede pasarse al parámetro `payload_json` (o a los Callback Data Payloads).

Las imágenes también pueden ser referenciadas en metadatos _embed_ usando la URL `attachment://filename`. El `filename` de estas URLs debe ser ASCII alfanumérico, con guiones bajos, guiones o puntos. A continuación se muestra un ejemplo de payload.

**Edición de archivos adjuntos en mensajes**

El parámetro JSON `attachments` incluye todos los archivos que se agregarán al mensaje, incluyendo los nuevos archivos y sus respectivos marcadores (_placeholders_). Al realizar una petición `PATCH`, solo los archivos listados en `attachments` se agregarán al mensaje. Todos los archivos previamente añadidos que no estén en la lista serán eliminados.

**Ejemplo de cuerpos de petición (multipart/form-data)**

Ten en cuenta que estos ejemplos son pequeñas secciones de una solicitud HTTP, solo para demostrar el comportamiento de este endpoint; las librerías cliente establecerán sus propios límites de formulario (`boundary`, que aquí solo es un ejemplo). Para más información, consulta la [especificación de multipart/form-data](https://tools.ietf.org/html/rfc7578#section-4).

Este ejemplo muestra el uso del endpoint **sin** `payload_json`.

{% code overflow="wrap" %}
```http
-boundary
Content-Disposition: form-data; name="content"
Hello, World!
-boundary
Content-Disposition: form-data; name="tts"
true
-boundary--
```
{% endcode %}

Este ejemplo demuestra el uso del endpoint con `payload_json` y todos los campos de contenido (`content`, `embeds`, `files[n]`) configurados.

{% code overflow="wrap" %}
```http
-boundary
Content-Disposition: form-data; name="payload_json"
Content-Type: application/json
{
  "content": "Hello, World!",
  "embeds": [{
    "title": "Hello, Embed!",
    "description": "This is an embedded message.",
    "thumbnail": {
      "url": "attachment://myfile.png"
    },
    "image": {
      "url": "attachment://mygif.gif"
    }
  }],
  "message_reference": {
    "type": 0,
    "channel_id": "233648473390448640",
    "message_id": "233648473390448641"
  },
  "attachments": [{
      "id": 0,
      "description": "Image of a cute little cat",
      "filename": "myfile.png"
  }, {
      "id": 1,
      "description": "Rickroll gif",
      "filename": "mygif.gif"
  }]
}
-boundary
Content-Disposition: form-data; name="files[0]"; filename="myfile.png"
Content-Type: image/png
[image bytes]
-boundary
Content-Disposition: form-data; name="files[1]"; filename="mygif.gif"
Content-Type: image/gif
[image bytes]
-boundary--
```
{% endcode %}

Usar adjuntos dentro de embeds

Puedes subir archivos adjuntos al crear un mensaje y utilizar esos adjuntos dentro de tu embed. Para ello, deberás subir los archivos como parte de tu cuerpo `multipart/form-data`. Asegúrate de subir archivos que contengan un nombre de archivo (_filename_), ya que será necesario referenciarlo en tu payload. Ten en cuenta que Discord normaliza los nombres de archivo, por lo que enviar caracteres exóticos en el nombre podría producir resultados inesperados.

{% hint style="warning" %}
Solo se permiten archivos con extensión `.jpg`, `.jpeg`, `.png`, `.webp` y `.gif` por el momento. Otros tipos de archivos no son compatibles.
{% endhint %}

Dentro de un objeto _embed_, puedes establecer una imagen para que utilice un adjunto como su URL, utilizando la siguiente sintaxis de esquema de adjunto:\
`attachment://filename.png`

Por ejemplo:

{% code overflow="wrap" %}
```json
{
  "embeds": [
    {
      "image": {
        "url": "attachment://screenshot.png"
      }
    }
  ]
}
```
{% endcode %}

**Subida a Google Cloud**

Puedes subir archivos adjuntos grandes rápidamente y de forma directa al bucket de almacenamiento de Google Cloud de Discord, utilizando el endpoint de **crear archivos** para generar enlaces de subida, y enviando a cada enlace generado una solicitud `PUT` con el adjunto como cuerpo.

Esto te permite evitar el límite de tamaño de solicitud de Discord de 200 MiB y subir archivos de hasta 500 MiB directamente a Google Cloud. Ten en cuenta que los límites de tamaño de archivo seguirán aplicando al enviar el adjunto en un mensaje.

Un ejemplo de implementación en pseudocódigo Python sería:

{% code overflow="wrap" %}
```python
import requests
import os

url = "https://discord.com/api/v10/channels/<channel_id>/attachments"

headers = {
    "Authorization": "Bot <bot_token>"
}

filename = "cat.png"
size = os.stat(filename).st_size

json = {
    "files": [{
        "file_size": size,
        "filename": filename
    }]
}

r = requests.post(url, headers=headers, json=json)
r.raise_for_status()

upload = r.json()['attachments'][0]
with open(filename, "r") as f:
    data = f.read()
    r = requests.put(upload['upload_url'], data=data)
    r.raise_for_status()

upload_filename = upload['upload_filename']
```
{% endcode %}

Ahora, en vez de enviar el archivo adjunto nuevamente en el cuerpo del formulario dentro de la solicitud, ¡puedes simplemente enviar el `upload_filename`! Por ejemplo:

{% code overflow="wrap" %}
```python
{
  "content": "look at my cute cat!",
  "attachments": [
    {
      "id": "0",
      "filename": "cat.png",
      "uploaded_filename": "6a08e58a-265f-485a-8c85-5cd4df0edde0/cat.png"
    }
  ]
}
```
{% endcode %}

### Locales <a href="#locales" id="locales"></a>

| Local  | Nombre del idioma      | Nombre nativo             |
| ------ | ---------------------- | ------------------------- |
| ar     | Árabe                  | العربية                   |
| bg     | Búlgaro                | български                 |
| cs     | Checo                  | Čeština                   |
| da     | Danés                  | Dansk                     |
| de     | Alemán                 | Deutsch                   |
| el     | Griego                 | Ελληνικά                  |
| en-GB  | Inglés, Reino Unido    | English, UK               |
| en-US  | Inglés, Estados Unidos | English, US               |
| es-ES  | Español                | Español                   |
| es-419 | Español, LATAM         | Español de América Latina |
| fi     | Finés                  | Suomi                     |
| fr     | Francés                | Français                  |
| hi     | Hindi                  | हिन्दी                    |
| hr     | Croata                 | Hrvatski                  |
| hu     | Húngaro                | Magyar                    |
| id     | Indonés                | Bahasa Indonesia          |
| it     | Italiano               | Italiano                  |
| ja     | Japonés                | 日本語                       |
| ko     | Coreano                | 한국어                       |
| lt     | Lituano                | Lietuviškai               |
| nl     | Neerlandés             | Nederlands                |
| no     | Noruego                | Norsk                     |
| pl     | Polaco                 | Polski                    |
| pt-BR  | Portugués, Brasil      | Português do Brasil       |
| ro     | Rumano                 | Română                    |
| ru     | Ruso                   | Pусский                   |
| sv-SE  | Sueco                  | Svenska                   |
| th     | Tailandés              | ไทย                       |
| tr     | Turco                  | Türkçe                    |
| uk     | Ucraniano              | Українська                |
| vi     | Vietnamita             | Tiếng Việt                |
| zh-CN  | Chino, China           | 中文                        |
| zh-TW  | Chino, Taiwán          | 繁體中文                      |

Claro, aquí tienes la traducción profesional al español del fragmento completo, respetando el formato y terminología técnica:

***

<h3 align="center">Referencia de documentación</h3>

Esta documentación utiliza varias convenciones para describir la API. A continuación se presentan algunas convenciones comunes que puedes encontrar:

#### Campos de recursos nulos y opcionales

Los campos de recurso que pueden contener un valor `null` tienen tipos que se indican con un signo de interrogación como prefijo. Los campos que son opcionales tienen nombres que se indican con un signo de interrogación como sufijo.

{% hint style="info" %}
Los campos que son `null` o que pueden faltar solo en casos excepcionales **no** se marcarán como arriba. En su lugar, se marcarán con una nota al pie, y la situación se explicará más abajo.
{% endhint %}

***

#### Ejemplo de campos nulos y opcionales

¹ Puede ser inesperadamente `null` a las 3:00 AM de los martes para cuentas creadas el 23 de junio de 2017.

| Campo                           | Tipo    |
| ------------------------------- | ------- |
| optional\_field?¹               | string  |
| nullable\_field                 | ?string |
| optional\_and\_nullable\_field? | ?string |

***

#### Límites de los campos

Se hace un esfuerzo razonable por documentar los límites de los campos cuando sea posible (por ejemplo, la longitud máxima de una cadena, el número máximo de elementos en un array). Estos límites no son exhaustivos y pueden no cubrir todos los casos extremos.

Se utiliza un límite por defecto de **1521 elementos en arrays** y **152133 caracteres** cuando no existe un límite bien definido.

***

#### Campos obsoletos/eliminados

Los campos obsoletos se marcarán con un aviso **(obsoleto)** junto a ellos. Los campos eliminados (que se mantienen por motivos de completitud, como en los _enums_) se marcarán con **tachado**.

***

#### Campos de eventos

Ciertos campos solo están presentes en estructuras recibidas a través del Gateway. Estos campos normalmente se documentan en la sección del Gateway en lugar de en la estructura principal.

***

#### Insignias

Algunos _endpoints_ pueden tener insignias que describen comportamientos adicionales en forma resumida. A continuación algunos ejemplos:

***

**MFA requerida**

Este _endpoint_ puede requerir que los usuarios con autenticación multifactor habilitada se autentiquen para ciertas acciones. Consulta la documentación de verificación MFA para la implementación.

> Esto **no aplica** a solicitudes mediante OAuth2 ni a bots.

***

**Motivo del registro en el log de auditoría**

El _endpoint_ puede utilizarse con una cabecera `X-Audit-Log-Reason`. Esta cabecera se usa para proporcionar un motivo para la acción que se está realizando. Ese motivo se mostrará en la entrada del registro de auditoría correspondiente. Consulta la documentación del log de auditoría para más información.

***

#### **Solicitud no autenticada**

Estas solicitudes no requieren que la cabecera `Authorization` esté presente.

***

#### **Solicitud OAuth2**

El _endpoint_ puede usarse con un _bearer token_ en la cabecera `Authorization`. El _scope_ específico requerido (si lo hay) se mostrará al pasar el cursor sobre la insignia.

***

#### **Endpoint obsoleto**

Este _endpoint_ sigue activo y funcionalmente igual, pero se recomienda evitarlo en lo posible, ya que podría eliminarse en una futura versión de la API.

[^1]: `BigInt` es un tipo numérico especial que provee soporte a enteros de tamaño arbitrario.

[^2]: Un `BigInt` emplea 8 bytes (64 bits) de memoria.

[^3]: Los booleanos son un tipo de dato que solo puede tener dos valores: verdadero o falso.

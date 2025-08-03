---
icon: virus-slash
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

# Límite de frecuencias

Los límites de frecuencia existen en las APIs de Discord para prevenir spam, abuso y sobrecarga del servicio. Los límites se aplican tanto en rutas individuales como de forma global por usuario. Los individuos se determinan usando la autenticación de la petición, por ejemplo, un token de usuario. Si una petición se realiza sin autenticación, los límites de frecuencia se aplican a la dirección IP.

{% hint style="info" %}
Como estos límites dependen de varios factores y están sujetos a cambios, **no se deben hardcodear**. Mejor, analiza los encabezados de respuesta (si los hay) para evitar sobrepasar el límite y responde adecuadamente si ocurre.
{% endhint %}

**Los límites de frecuencia por ruta** existen para muchos endpoints individuales, y pueden incluir el método HTTP (`GET`, `POST`, `PUT` o `DELETE`). En algunos casos, los límites de ruta se comparten entre un conjunto de endpoints similares, indicado por la cabecera `X-RateLimit-Bucket` para bots. Si existe, se recomienda usar esta cabecera como identificador único de un límite, lo que te permitirá agrupar límites compartidos según los detectes.

Durante el cálculo, los límites de frecuencia por ruta suelen contabilizar los recursos de nivel superior en el path usando un identificador, por ejemplo, `guild_id` al llamar a `/guilds/{guild.id}/channels`. Actualmente, los recursos de nivel superior limitados son canales (`channel_id`), gremios (`guild_id`) y webhooks (`webhook_id` o `webhook_id + webhook_token`). Esto significa que un endpoint con dos recursos de nivel superior diferentes puede calcular límites de forma independiente. Por ejemplo, si excedes el límite de una ruta en `/channels/1234`, aún podrás llamar a otro endpoint similar como `/channels/9876` sin problema.

**Los límites de frecuencia globales** se aplican al número total de solicitudes que hace un usuario, independientemente de los límites por ruta. Puedes leer más sobre los límites globales más abajo.

{% hint style="warning" %}
Las rutas para controlar emojis no siguen las convenciones normales de límites de frecuencia. Estas rutas están específicamente limitadas por gremio para prevenir abuso, lo cual hace que la cuota devuelta por nuestras APIs pueda ser inexacta y se puedan producir respuestas **429**.
{% endhint %}

### Formato de cabecera <a href="#formato-de-cabecera" id="formato-de-cabecera"></a>

Para la mayoría de las peticiones API realizadas con autorización de bot o OAuth2, Discord retorna headers HTTP opcionales con el límite de frecuencia encontrado durante tu solicitud. La autorización de usuario normalmente solo retorna los headers **Retry-After**, **X-RateLimit-Global** y **X-RateLimit-Scope**.

#### Ejemplo de encabezados de límite de frecuencia

```
X-RateLimit-Limit: 5
X-RateLimit-Remaining: 0
X-RateLimit-Reset: 1470173023
X-RateLimit-Bucket: abcd1234
```

* **Retry-After** - Se retorna solo en respuestas **429**: el número de segundos que queda para que se reinicie el bucket completo.
* **X-RateLimit-Global** - Se retorna solo en una respuesta **429** si el límite encontrado es el global (no por ruta).
* **X-RateLimit-Limit** - El número de solicitudes que se pueden hacer.
* **X-RateLimit-Remaining** - El número de solicitudes restantes que se pueden hacer.
* **X-RateLimit-Reset** - Tiempo epoch (segundos desde 00:00:00 UTC del 1 de enero de 1970) en el que el límite de frecuencia se reinicia.
* **X-RateLimit-Reset-After** - Tiempo total (en segundos) restante hasta que el bucket actual de límite de frecuencia se reinicie; puede contener decimales para mantener precisión en milisegundos.
* **X-RateLimit-Bucket** - Cadena única que denota el límite de frecuencia encontrado (sin incluir parámetros mayores en el path de la ruta).
* **X-RateLimit-Scope** - Se retorna solo en respuestas **429**: puede tomar los valores `user` (límite por usuario), `global` (límite global por usuario) o `shared` (límite por recurso compartido).

### Exceder un límite de frecuencia <a href="#exceder-un-lmite-de-frecuencia" id="exceder-un-lmite-de-frecuencia"></a>

En caso de exceder un límite de frecuencia, la API devolverá un código de respuesta **429** con un cuerpo JSON.

#### Estructura de respuesta al exceder el límite de frecuencia

| Campo        | Tipo    | Descripción                                                  |
| ------------ | ------- | ------------------------------------------------------------ |
| message      | string  | Un mensaje diciendo que estás limitado por frecuencia.       |
| retry\_after | float   | Número de segundos a esperar antes de enviar otra solicitud. |
| global       | boolean | Indica si estás limitado globalmente o no.                   |
| code?        | integer | Un código de error para límites especiales.                  |

Ten en cuenta que los headers normales de límite de frecuencia de la ruta también se enviarán en esta respuesta. La respuesta de límite de frecuencia se verá así:

<h4 align="center">Ejemplos de respuesta excediendo el límite</h4>

{% tabs %}
{% tab title="Límite de usuario" %}
```python
< HTTP/1.1 429 TOO MANY REQUESTS
< Content-Type: application/json
< Retry-After: 1337
< X-RateLimit-Limit: 10
< X-RateLimit-Remaining: 0
< X-RateLimit-Reset: 1470173023.123
< X-RateLimit-Reset-After: 1337.57
< X-RateLimit-Bucket: abcd1234
< X-RateLimit-Scope: user
{
  "message": "You are being rate limited.",
  "retry_after": 776,
  "global": false
}
```
{% endtab %}

{% tab title="Límite de recurso" %}
```python
< HTTP/1.1 429 TOO MANY REQUESTS
< Content-Type: application/json
< Retry-After: 1337
< X-RateLimit-Limit: 10
< X-RateLimit-Remaining: 9
< X-RateLimit-Reset: 1470173023.123
< X-RateLimit-Reset-After: 1337.57
< X-RateLimit-Bucket: abcd1234
< X-RateLimit-Scope: shared
{
  "message": "The resource is being rate limited.",
  "retry_after": 776.57,
  "global": false
}
```
{% endtab %}

{% tab title="Límite global" %}
```python
< HTTP/1.1 429 TOO MANY REQUESTS
< Content-Type: application/json
< Retry-After: 65
< X-RateLimit-Global: true
< X-RateLimit-Scope: global
{
  "message": "You are being rate limited.",
  "retry_after": 65,
  "global": true
}
```
{% endtab %}
{% endtabs %}

### Límite de frecuencia global <a href="#lmite-de-frecuencia-global" id="lmite-de-frecuencia-global"></a>

Todos los usuarios pueden hacer hasta 50 solicitudes por segundo a nuestra API. Si no se proporciona cabecera de autorización, el límite se aplica por dirección IP. Esto es independiente de cualquier límite individual de ruta. Si un bot crece lo suficiente, puede ser imposible mantenerse por debajo de 50 solicitudes por segundo durante operaciones normales.

Los problemas con el límite global suelen aparecer cuando un bot recibe baneos frecuentes del API de Discord al arrancar. Si un bot es temporalmente baneado por Cloudflare del API de Discord ocasionalmente, probablemente **no** es un problema de límite global, sino un pico de errores no gestionados correctamente que disparó el umbral de errores.

Si el dueño de un bot experimenta baneos repetidos de Cloudflare durante el uso normal del bot, puede ponerse en contacto con soporte para ver si califica para aumentar el límite global hasta 1,200 solicitudes por segundo. El contacto es en [https://dis.gd/rate-limit](https://dis.gd/rate-limit).

Los webhooks no están sujetos al límite de solicitudes global del usuario.

### Límite de solicitudes inválidas, también conocido como baneo de Cloudflare <a href="#lmite-de-solicitudes-invlidas-tambin-conocido-como" id="lmite-de-solicitudes-invlidas-tambin-conocido-como"></a>

Direcciones IP que hagan demasiadas solicitudes HTTP inválidas serán restringidas temporalmente del acceso al API de Discord. Actualmente, este límite es **10,000 por cada 10 minutos** y lleva a un **baneo de 24 horas**. Una solicitud inválida es aquella que da como resultado un estado **401**, **403** o **429**.

Todos los usuarios deben intentar evitar razonablemente realizar solicitudes inválidas. Por ejemplo:

* Las respuestas **401** se evitan proporcionando un token válido en la cabecera de autorización cuando sea necesario y deteniendo solicitudes adicionales después de que un token se vuelva inválido.
* Las respuestas **403** se evitan inspeccionando los permisos de rol o canal, no realizando solicitudes restringidas por estos permisos.
* Las respuestas **429** se evitan inspeccionando los headers de límite de frecuencia antes explicados y evitando hacer solicitudes en cubos agotados hasta que se hayan reiniciado; los errores 429 recibidos con `X-RateLimit-Scope: shared` no se cuentan en tu contra.

Los bots grandes, particularmente aquellos que puedan hacer 10,000 solicitudes en 10 minutos (16-17 solicitudes por segundo sostenidas), deberían considerar llevar registro y seguimiento de la tasa de solicitudes inválidas para evitar alcanzar este límite.

Además, se espera que tengas en cuenta otros estados inválidos razonablemente. Por ejemplo, si un webhook devuelve un estado **404** no debes intentar usarlo nuevamente; si lo haces repetidamente recibirás una restricción temporal.

Existen otros límites adicionales de Cloudflare en ciertos endpoints que no están documentados aquí. A veces estos límites retornan una cabecera `Retry-After`. En estos casos, debes respetar la cabecera y no realizar más solicitudes hasta que el tiempo haya pasado. Si no hay cabecera `Retry-After`, no deberías volver a intentarlo automáticamente de forma programada.

#### Recursos no disponibles <a href="#recursos-no-disponibles" id="recursos-no-disponibles"></a>

En algunos casos, los clientes pueden realizar una solicitud API para la cual el servidor aún no tiene respuesta. En estos casos, la API devolverá un código de respuesta **202** con un cuerpo JSON.

### Estructura de respuesta de recurso no disponible

<sup>1</sup> Si el plazo dado falta o es `0`, el cliente deberá esperar un breve período antes de intentar de nuevo (típicamente 5 segundos).

| Campo           | Tipo    | Descripción                                                       |
| --------------- | ------- | ----------------------------------------------------------------- |
| message         | string  | Un mensaje diciendo que el recurso aún no está disponible.        |
| code            | integer | Un código de error (siempre empezará por `11`).                   |
| retry\_after? 1 | float   | El número de segundos a esperar antes de realizar otra solicitud. |

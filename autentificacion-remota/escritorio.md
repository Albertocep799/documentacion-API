---
icon: desktop
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

# Escritorio

En el contexto de autenticación remota, el cliente de escritorio es el dispositivo que desea ser autenticado por el cliente móvil que ya inició sesión. Consulta la introducción para más información.

### Gateway de autenticación remota

La implementación de escritorio de la autenticación remota utiliza conexiones WebSocket, funcionando de modo similar a la conexión Gateway. Sin embargo, tiene su propio conjunto de cargas útiles y eventos, así como una estructura de paquetes simplificada.

#### Versiones del Gateway

| Versión | Estado        |
| ------- | ------------- |
| 2       | Disponible    |
| 1       | Descontinuado |

### Cargas útiles del Gateway

En la práctica, los opcodes están en minúsculas y con guion bajo. Por ejemplo, "Nonce Proof" sería `nonce_proof`.

Para mejorar la legibilidad, los opcodes en esta documentación suelen usar mayúsculas iniciales.

Las cargas útiles de eventos del Gateway de autenticación remota son paquetes planos, con el campo `op` indicando el tipo de carga útil y el resto siendo los datos de la carga útil.

#### Ejemplo de carga útil de Gateway

```python
{
  "op": "hello",
  "timeout_ms": 142637,
  "heartbeat_interval": 41250
}
```

#### Comandos del Gateway

| Nombre      | Descripción                                           |
| ----------- | ----------------------------------------------------- |
| Init        | Iniciar una nueva sesión de autenticación remota.     |
| Heartbeat   | Mantener una conexión WebSocket activa.               |
| Nonce Proof | Enviar una prueba criptográfica del apretón de manos. |

#### Eventos del Gateway

| Nombre              | Descripción                                             |
| ------------------- | ------------------------------------------------------- |
| Hello               | Define los intervalos de latido y tiempo de espera.     |
| Heartbeat ACK       | Confirma la recepción de un latido del cliente.         |
| Nonce Proof         | Solicita una prueba criptográfica del apretón de manos. |
| Pending Remote Init | Confirmación de apretón de manos exitoso.               |
| Pending Ticket      | Confirmación de creación exitosa de sesión móvil.       |
| Pending Login       | Indica que la sesión móvil fue finalizada.              |
| Cancel              | Indica que la sesión móvil fue cancelada.               |

#### Códigos de evento de cierre del Gateway

<sup>1</sup> Este código puede recibirse erróneamente por errores de protocolo.

| Código | Descripción                  | Explicación                                                                |
| ------ | ---------------------------- | -------------------------------------------------------------------------- |
| 1000 1 | Cierre normal                | La sesión de autenticación remota se finalizó o canceló con éxito.         |
| 4000   | Versión inválida             | Enviada una versión no válida para el gateway de autenticación remota.     |
| 4001   | Error de decodificación      | Se ha enviado una carga útil no válida. ¡No hagas eso!                     |
| 4002   | Fallo en el apretón de manos | El apretón de manos inicial falló. Tal vez reconecta e inténtalo de nuevo. |
| 4003   | Tiempo agotado               | La sesión ha expirado por tiempo. Reconecta e inicia una nueva.            |

#### Conexión

Para el Gateway de autenticación remota, la URL para abrir una conexión WebSocket es estática.

Al conectarse a la URL, se debe añadir explícitamente la versión de la API como parámetro de consulta. Por ejemplo, `wss://remote-auth-gateway.discord.gg/?v=2` es válida para conectarse al Gateway.

{% hint style="warning" %}
Es obligatorio especificar la cabecera `Origin`, configurada a uno de los siguientes valores: `https://discord.com`, `https://ptb.discord.com` o `https://canary.discord.com`. Si el origen es inválido, la conexión será rechazada. Esto previene que sitios maliciosos intenten secuestrar cuentas usando autenticación remota.
{% endhint %}

#### Endpoint

> wss://remote-auth-gateway.discord.gg/

#### Parámetros de la cadena de consulta

| Campo | Tipo    | Descripción                   |
| ----- | ------- | ----------------------------- |
| v     | integer | Versión de la API a utilizar. |

### Handshaking

Cuando abres una conexión al Gateway, recibirás una carga útil con el opcode Hello que contiene el intervalo de latido (heartbeat) y la duración de tiempo de espera. Consulta la sección de heartbeating para más información.

En este punto, debes generar un par de claves RSA-OAEP de 2048 bits que se utilizarán en las comunicaciones posteriores. El Gateway verificará esto usando una prueba de nonce. Ejemplo en pseudocódigo de Python:

```python
import base64
from cryptography.hazmat.primitives import serialization, hashes
from cryptography.hazmat.primitives.asymmetric import rsa, padding

private_key = rsa.generate_private_key(
    public_exponent=65537,
    key_size=2048,
)
public_key = private_key.public_key()
```

#### Estructura Hello

<sup>1</sup> Cuando pasa el tiempo de espera, el Gateway cerrará con el código \[`4003`].

| Campo               | Tipo    | Descripción                                                                                                                             |
| ------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| heartbeat\_interval | integer | El intervalo mínimo (en milisegundos) al que el cliente debe enviar latidos.                                                            |
| timeout\_ms 1       | integer | La vida útil de la sesión de autenticación remota (en milisegundos) antes de que se cierre la conexión, normalmente unos pocos minutos. |

#### Ejemplo Hello

```python
{
  "op": "hello",
  "timeout_ms": 142637,
  "heartbeat_interval": 41250
}
```

Al recibir la carga útil Opcode Hello, debes enviar inmediatamente una carga útil Opcode Init que contiene la clave pública con la que se encriptará toda la comunicación futura.

Ejemplo en pseudocódigo de Python:

```
spki = public_key.public_bytes(
    encoding=serialization.Encoding.DER,
    format=serialization.PublicFormat.SubjectPublicKeyInfo,
)
encoded_public_key = base64.b64encode(spki).decode("utf-8")
```

#### Estructura init

| Campo                | Tipo   | Descripción                                                                         |
| -------------------- | ------ | ----------------------------------------------------------------------------------- |
| encoded\_public\_key | string | El SPKI codificado en base64 de la clave pública RSA-OAEP de 2048 bits del cliente. |

#### Ejemplo init

```python
{
  "op": "init",
  "encoded_public_key": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAo2PGAKj4v6r6sPJtgJe2eIDCM8uEHKpYCSDmp+pun9vqiqPt4pDToS1vGtwTwc5hKKqtIo+I/5veBpGWSD/veuB0xVb/JbkPn847Q+mXAb6c9vRMJVkA7l9GaZdN49U5bnGJi009aNBoy9cAcP/19H6TLpHmZ9RojnqGqlCUdyAiqceTDTzPqov4ST3GJSyKPydL3ZVpPf5P/PGyNfISuESKA2CxGCoBvB4H6/FH7cwSFelyqhwwHPZcyxBjF/3iXx+k1PdS01y0NoTRun4p76bE9rWnecIWONPFvCkby8Xs/OqQ8QcAoLkfVj5L29Ut1+Kmwwfg3nzc4glZa6RuTwIDAQAB"
}
```

Si el Gateway acepta la clave pública, responderá con una carga útil Opcode Nonce Proof que contiene un nonce que deberás descifrar con tu clave privada.

#### Estructura nonce proof (Recibir)

| Campo            | Tipo   | Descripción                                                             |
| ---------------- | ------ | ----------------------------------------------------------------------- |
| encrypted\_nonce | string | El nonce codificado en base64 cifrado con la clave pública del cliente. |

#### Ejemplo nonce proof (Recibir)

```python
{
  "op": "nonce_proof",
  "encrypted_nonce": "GrOYi2wz9athue0mTNrdlnWGqJYkqu8tPe2uGr9V7CRwqVWDjgUI06gsPKszkORgB92P1P84V04fo7hG0tkQ/kDVNbVguACKfE4AIUUQXQFSkTVaGbZ2+FsItsoqOd+955EvkBK2oMz+kWlYILTQcISip9g5ZrY9SoKQvk7HDW9DliSteZivHzXQOc5RyecyeexOcV8oyC1zTk+uyVUTv3g7fcZ1Y81AK6u4+hvnEKGjOyEn+lbOotkNwcMC02xyVBX3IysSuNXf/f8/6gPMBNUHXEtlvhYx9AMCsPrPKkiilV7HpLN3oIvAfsZnyxbYcNiC6YS7z7VIPRaXEWW76w=="
}
```

Luego debes enviar una carga útil Opcode Nonce Proof conteniendo una representación codificada en base64URL del digest SHA-256 del nonce descifrado.

Ejemplo en pseudocódigo de Python:

```python
nonce = private_key.decrypt(
    base64.b64decode(encrypted_nonce),
    padding.OAEP(mgf=padding.MGF1(algorithm=hashes.SHA256()), algorithm=hashes.SHA256(), label=None),
)
nonce_proof = base64.urlsafe_b64encode(
    hashes.Hash(hashes.SHA256()).update(nonce).finalize()
).decode("utf-8").rstrip("=")
```

#### Estructura nonce proof (enviar)

| Campo | Tipo   | Descripción                                                     |
| ----- | ------ | --------------------------------------------------------------- |
| nonce | string | El digest SHA-256 codificado en base64URL del nonce descifrado. |

#### Ejemplo nonce proof (enviar)

```python
{
  "op": "nonce_proof",
  "nonce": "1xFdzsYTFMtZuUarGNKpBpMANyqNpRXpDFaxl8wFeQBUnVuY60d9j-or80f36xusAVTmUL90jwHrxMVPGZXJ8ahqDAiXByEOjkreJXZdPRbnvDkHHyqUP0QgHQBvrKq5Cba9-SDO8pFSc9T1YMWGut7n34xx6txX-QR7wDcZAoghq5EBl04WRlnt1DWfX2wMA7NuL1GFIZdw10IedBZ13E72BXnDasX97XMX_ldbxKwee6ABGf18zde2oHbTqw"
}
```

{% hint style="info" %}
Si alguna parte del apretón de manos falla, el Gateway cerrará con el código \[`4001`].
{% endhint %}

Si el Gateway acepta la prueba de nonce, responderá con una carga útil Opcode Pending Remote Init, indicando que el intercambio de claves fue exitoso y la sesión de autenticación remota está lista.

En este punto, la huella digital puede usarse para crear una sesión de autenticación remota en el cliente móvil. Habitualmente se comunica mediante un código QR con el siguiente formato: `https://discord.com/ra/`.

{% hint style="info" %}
Los clientes deben verificar que la huella enviada es la misma que el digest SHA-256 codificado en base64URL de la clave pública del cliente, como medida de seguridad. Si las huellas no coinciden, el cliente debe cerrar la conexión y volver a intentar.
{% endhint %}

#### Estructura pending remote init

| Campo       | Tipo   | Descripción                                                                |
| ----------- | ------ | -------------------------------------------------------------------------- |
| fingerprint | string | El digest SHA-256 codificado en base64URL de la clave pública del cliente. |

#### Ejemplo ending remote init

```python
{
  "op": "pending_remote_init",
  "fingerprint": "UZ0-kOVzXDZTFVV5_QlpURSO2BQHrtkKWHNpIGoDI0k"
}
```

### Heartbeating

Para mantener la conexión WebSocket, es necesario enviar latidos (heartbeats) constantemente en el intervalo determinado por el opcode Hello.

Este intervalo de latido es el mínimo en el que debes enviar heartbeats. Puedes enviar latidos en intervalos menores si lo deseas.

Tras recibir el opcode Hello, debes enviar el opcode Heartbeat cada intervalo transcurrido:

{% hint style="info" %}
El primer heartbeat puede retrasarse entre 0 y el `heartbeat_interval` para evitar que demasiados clientes se conecten exactamente a la vez (lo que podría causar una avalancha de tráfico).
{% endhint %}

A cambio, recibirás un opcode Heartbeat ACK. Si el cliente no recibe un ACK entre los intentos de enviarlos, puede deberse a una conexión fallida o "zombied". En tal caso, el cliente debe terminar la conexión y reconectar inmediatamente.

#### Ejemplo heartbeat

```python
{ "op": "heartbeat" }
```

#### Ejemplo heartbeat ACK

```python
{ "op": "heartbeat_ack" }
```

### Finalización

Una vez que el cliente móvil crea una sesión de autenticación remota (al escanear el código QR), el Gateway enviará una carga útil Opcode Pending Ticket, que contiene la información del usuario que intenta autenticarse.

#### Estructura pending ticket

| Campo                    | Tipo   | Descripción                                                                                  |
| ------------------------ | ------ | -------------------------------------------------------------------------------------------- |
| encrypted\_user\_payload | string | La carga útil de usuario codificada en base64 y encriptada con la clave pública del cliente. |

#### Estructura de la carga útil de usuario

La carga útil de usuario es una cadena separada por dos puntos, con este formato: `852892297661906993:0:05145cc5646fbcba277b6d5ea2030610:dolfies`.

Incluye los siguientes campos:

<sup>1</sup> El hash del avatar será `0` para representar un avatar `null`.

| Campo         | Tipo      | Descripción                              |
| ------------- | --------- | ---------------------------------------- |
| id            | snowflake | El ID del usuario.                       |
| discriminator | string    | El tag Discord de 4 dígitos del usuario. |
| avatar 1      | string    | El hash de avatar del usuario.           |
| username      | string    | El nombre de usuario (2-32 caracteres).  |

#### Ejemplo pending ticket

```python
{
  "op": "pending_ticket",
  "encrypted_user_payload": "MeJm9TeLa9S+/gUYZlm69TQqT3eqz1sG6f5Ym84lGCH7Hde/Dpv/knXX+wWp8WeyLPHYGn1smpTaGxwHmuvee1IZv8ybP9WeVscsBnrXpO7Fg2aT3hZJfPStncJxI0Uq+JhjqQ1V2lLuhoraeBrZbl/e0CBNhv0wmIGzFow6G078MQvikg20Jr+/2wRgY/buXuipqfW9RYIZUyr3Dl+MRW8EodKU3SOBgqaVpHETDLXkmb6rsvYU+O78iUu+cvTK3A0GkajxmhJ2nfg79iQBuLU6qJm7sN9mHy2uKAExa5TUJfeeCqXjTr1uROozlyBmCMjeP+Srrg37r0y2pkjYCg=="
}
```

En este punto, el cliente de escritorio recibirá un evento Opcode Pending Login o Cancel. Ambos eventos harán que el Gateway cierre con código \[`1000`]. Si ninguno ocurre (el cliente móvil no finaliza ni cancela), el Gateway cerrará eventualmente por timeout con el código \[`4003`].

Si el móvil finaliza la autenticación remota, el Gateway enviará una carga útil Opcode Pending Login, indicando que la autenticación fue exitosa.

#### Estructura pending login

| Campo  | Tipo   | Descripción                                                     |
| ------ | ------ | --------------------------------------------------------------- |
| ticket | string | Ticket que puede usarse para obtener un token de autenticación. |

#### Ejemplo pending login

```python
{
  "op": "pending_login",
  "ticket": "ODUyODkyMjk3NjYxOTA2OTkz.HYoNwT.1X5Qs3Sd2Z2sDf3sFFFwd22_MccjcmwY"
}
```

Si el móvil cancela la autenticación remota, el Gateway enviará una carga útil Opcode Cancel, indicando que la autenticación fue cancelada.

#### Ejemplo de cancel

```python
{ "op": "cancel" }
```

### Endpoints <a href="#endpoints" id="endpoints"></a>

#### Intercambiar el ticket de autenticación remota

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /users/@me/remote-auth/login

Intercambia un ticket de autenticación remota por un token de autenticación.

#### Parámetros JSON

| Campo  | Tipo   | Descripción                                           |
| ------ | ------ | ----------------------------------------------------- |
| ticket | string | El ticket obtenido del flujo de autenticación remota. |

#### Cuerpo de respuesta

| Campo            | Tipo   | Descripción                                                            |
| ---------------- | ------ | ---------------------------------------------------------------------- |
| encrypted\_token | string | El token de autenticación encriptado con la clave pública del cliente. |

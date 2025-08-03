---
icon: user-group-crown
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

# Autenticación

Una característica distintiva principal de las cuentas de usuario es que se obtienen los tokens de autenticación iniciando sesión mediante una clásica combinación de correo electrónico y contraseña. Sin embargo, el flujo completo de inicio de sesión y registro es bastante complejo e involucra varios pasos.

{% hint style="warning" %}
Se debe tener especial cuidado al manejar las credenciales de los usuarios e implementar el flujo de inicio de sesión y registro de Discord. Discord no tolera el spam en sus endpoints de autenticación, y una implementación incorrecta o solicitudes sospechosas pueden llevar a la terminación de cuentas.
{% endhint %}

### Huellas digitales <a href="#huellas-digitales" id="huellas-digitales"></a>

Discord utiliza huellas digitales (fingerprints) para persistir experimentos durante todo el flujo de autenticación. Para más información sobre fingerprints, consulta la documentación de experimentos correspondiente.

Si el cliente no está autenticado, se debe generar un fingerprint usando Get Experiment Assignments e incluirse en la cabecera `X-Fingerprint` así como en cualquier parámetro JSON `fingerprint` aplicable hasta que se complete la autenticación.

### Sesiones <a href="#sesiones" id="sesiones"></a>

Discord utiliza sesiones para rastrear el estado de autenticación del usuario. Una sesión se crea cuando el usuario inicia sesión y se invalida cuando cierra sesión o la sesión expira. Las sesiones son únicas para un solo token de autenticación y llevan un registro de la última ubicación desde la que se usaron. Las sesiones sospechosas pueden ser marcadas por Discord y llevar a que la cuenta sea bloqueada, requiriendo que el usuario restablezca su contraseña.

El ID de sesión de autenticación correspondiente a la sesión de Gateway actual se entrega en el campo `auth_session_id_hash` en el evento Ready. Si alguna vez la sesión de autenticación del Gateway actual cambia (por ejemplo, debido a un restablecimiento de contraseña), el cliente recibirá un evento Auth Session Change Gateway.

#### Objeto de sesión de autenticación

#### Estructura de sesión de autenticación

| Campo                    | Tipo                            | Descripción                                       |
| ------------------------ | ------------------------------- | ------------------------------------------------- |
| id\_hash                 | string                          | El hash del ID de sesión.                         |
| approx\_last\_used\_time | ISO8601 timestamp               | Cuándo se usó por última vez la sesión.           |
| client\_info             | auth session client info object | El cliente asociado por última vez con la sesión. |

#### Estructura de información de cliente de sesión de autenticación

| Campo    | Tipo    | Descripción                                       |
| -------- | ------- | ------------------------------------------------- |
| os       | ?string | El sistema operativo del cliente.                 |
| platform | ?string | El nombre del cliente o plataforma del navegador. |
| location | ?string | La ubicación aproximada del cliente.              |

#### Ejemplo de sesión de autenticación

```python
{
  "id_hash": "y3vq9yYww3Y1yAhc4ChuRtCOHADRu0gTPoIU5y3DcS4=",
  "approx_last_used_time": "2024-03-30T21:30:58.100231+00:00",
  "client_info": {
    "os": "Windows",
    "platform": "Discord Client",
    "location": "San Francisco, California, United States"
  }
}
```

#### Endpoints

#### Obtener sesiones de autenticación

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /auth/sessions

Devuelve hasta 50 de las sesiones de autenticación activas del usuario.

#### Cuerpo de respuesta

| Campo          | Tipo                        | Descripción                                        |
| -------------- | --------------------------- | -------------------------------------------------- |
| user\_sessions | array\[auth session object] | Sesiones de autenticación activas para el usuario. |

#### Cerrar sesión de autenticación

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /auth/sessions/logout

`MFA Requerido`

Invalida una lista de sesiones de autenticación. Devuelve una respuesta vacía 204 en caso de éxito.

#### Parámetros JSON

| Campo               | Tipo           | Descripción                                    |
| ------------------- | -------------- | ---------------------------------------------- |
| session\_id\_hashes | array\[string] | Los hashes de ID de sesión a invalidar (1-64). |

### Inicio de sesión <a href="#inicio-de-sesin" id="inicio-de-sesin"></a>

El proceso de inicio de sesión es el primer paso para autenticar a un usuario.

Para iniciar sesión, un cliente debe primero usar el endpoint Login Account con el correo electrónico/número de teléfono del usuario y contraseña. Si el usuario tiene habilitada la autenticación de múltiples factores, el cliente debe usar el endpoint Verify MFA Login para completar el proceso después de una respuesta exitosa.

Si se devuelve un código de error JSON conocido, el cliente debe manejar el error en consecuencia (por ejemplo, solicitar al usuario que recupere su cuenta o verifique su número de teléfono). Si se devuelve un error de formulario, el cliente debe mostrar el mensaje de error al usuario.

Una vez que el usuario haya iniciado sesión, el cliente debe guardar el token de autenticación para evitar tener que iniciar sesión nuevamente en el futuro.

#### Inicio de sesión sin contraseña

Una alternativa al flujo tradicional de inicio de sesión por correo/contraseña es el flujo Conditional UI de WebAuthn. Esto permite a los usuarios iniciar sesión usando un dispositivo WebAuthn (por ejemplo, un llavero de seguridad) en vez de una contraseña.

Para usar este flujo, el cliente debe primero generar un desafío usando el endpoint Start Passwordless Login. Tras completar el usuario el desafío de WebAuthn, el cliente debe usar el endpoint Finish Passwordless Login para completar el proceso. Usando este flujo, igual se debe manejar cualquier error según lo descrito anteriormente.

#### Transferencia de autenticación

El proceso de transferencia se usa para transferir de forma segura el estado de autenticación de un usuario entre dos clientes, comúnmente usado para autenticar el sitio web de Discord desde un cliente nativo y viceversa.

Para transferir autenticación a otro cliente, el cliente origen debe generar un token de transferencia (handoff) mediante el endpoint Create Handoff Token. El token se pasa al cliente destino, que puede usar el endpoint Exchange Handoff Token para obtener un nuevo token de autenticación.

Los tokens de transferencia son de un solo uso y solo son válidos para la dirección IP desde la que se generaron.

#### Endpoints

#### Iniciar sesión en la cuenta

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /auth/login

`No autenticado`

Recupera un token de autenticación para las credenciales dadas.

{% hint style="info" %}
Si este endpoint se solicita con un token de autenticación válido, se devolverá una respuesta de éxito independientemente del cuerpo de la solicitud.
{% endhint %}

{% hint style="warning" %}
Cuando se inicia sesión por primera vez en una cuenta (sin MFA habilitada) desde una nueva ubicación, Discord puede rechazar la solicitud de inicio de sesión y requerir que el usuario verifique el intento mediante correo electrónico o número de teléfono.

Si se inicia sesión mediante correo electrónico, el usuario recibirá un enlace que redirige al cliente oficial de Discord con un token de verificación presente en el fragmento de la URL (ej. `https://discord.com/authorize-ip#token=Wzg1Mjg5MjI5NzY2MTkwNjk5MywiMTI3LjAuMC4x8J+RvSJd.kdI8zppMIeTZsIBva3zZslaz_58`).

Si se inicia sesión mediante número de teléfono, la solicitud fallará con un código de error JSON `70007`, y el usuario recibirá un código de verificación por SMS. Se debe obtener un token de verificación verificando el número de teléfono.

Después de autorizar la IP con este token de verificación, se debe reintentar la solicitud de inicio de sesión. No debería requerirse un nuevo desafío CAPTCHA.
{% endhint %}

{% hint style="warning" %}
Si la cuenta del usuario está suspendida por Discord, la solicitud de inicio de sesión fallará con un error **403 Forbidden** y un cuerpo de respuesta de error especial, similar a una respuesta exitosa:

```python
{
  "user_id": "852892297661906993",
  "suspended_user_token": "ODUyODkyMjk3NjYxOTA2OTkz.Hcj0Nl.YEJSsjeq_vJKLpOofd5QMksqw32e"
}
```

Los tokens de autenticación suspendidos no pueden utilizarse como tokens de autenticación normales. Solo pueden usarse para ver el estado de la cuenta y apelar la suspensión de la cuenta.
{% endhint %}

#### Parámetros JSON

<sup>1</sup> Si obtienes un código de error JSON de cuenta deshabilitada (`20013`) o marcada para eliminación (`20011`), puedes recuperar la cuenta configurando este parámetro.

| Campo                | Tipo    | Descripción                                                                              |
| -------------------- | ------- | ---------------------------------------------------------------------------------------- |
| login                | string  | El correo electrónico del usuario o número de teléfono en formato E.164.                 |
| password             | string  | La contraseña del usuario (8-72 caracteres).                                             |
| undelete?            | boolean | Si se debe recuperar una cuenta auto-deshabilitada o auto-eliminada (por defecto falso). |
| login\_source?       | ?string | El origen de la solicitud de inicio de sesión.                                           |
| gift\_code\_sku\_id? | ?string | El SKU ID del código de regalo que inició la solicitud.                                  |

#### Origen de inicio de sesión

Desde dónde se inicia el inicio de sesión fuera del flujo normal:

| Valor                       | Descripción                                                      |
| --------------------------- | ---------------------------------------------------------------- |
| gift                        | Solicitud iniciada por un código de regalo.                      |
| guild\_template             | Solicitud iniciada por una plantilla de servidor.                |
| guild\_invite               | Solicitud iniciada por una invitación a servidor.                |
| dm\_invite                  | Solicitud iniciada por una invitación a MD grupal.               |
| friend\_invite              | Solicitud iniciada por una invitación de amigo.                  |
| role\_subscription          | Solicitud iniciada por una redirección de suscripción de rol.    |
| role\_subscription\_setting | Solicitud iniciada desde la configuración de suscripción de rol. |

#### Cuerpo de respuesta

| Campo              | Tipo                  | Descripción                                                          |
| ------------------ | --------------------- | -------------------------------------------------------------------- |
| user\_id           | snowflake             | El ID del usuario autenticado.                                       |
| token?             | string                | El token de autenticación, si el inicio de sesión fue completado.    |
| user\_settings?    | login settings object | Preferencias parciales del usuario, si el login fue completado.      |
| required\_actions? | array\[string]        | Acciones requeridas antes de continuar usando Discord.               |
| ticket?            | string                | Ticket utilizado en el flujo de autenticación de múltiples factores. |
| mfa?               | boolean               | Si es necesario MFA para iniciar sesión (por defecto falso).         |
| totp?              | boolean               | Si el usuario tiene habilitado MFA basado en TOTP.                   |
| sms?               | boolean               | Si el usuario tiene habilitado MFA basado en SMS.                    |
| backup?            | boolean               | Si se pueden usar códigos de respaldo para el MFA.                   |
| webauthn?          | ?string               | Cadena JSON de challenge de WebAuthn.                                |

#### Estructura de preferencias de inicio de sesión

| Campo  | Tipo   | Descripción                                                      |
| ------ | ------ | ---------------------------------------------------------------- |
| locale | string | Opción de idioma elegida por el usuario.                         |
| theme  | string | Tema del cliente seleccionado por el usuario (`dark` o `light`). |

#### Tipo de acción obligatoria de inicio de sesión

| Valor            | Descripción                                                                          |
| ---------------- | ------------------------------------------------------------------------------------ |
| update\_password | El usuario debe cambiar su contraseña para cumplir los nuevos requisitos de Discord. |

#### Ejemplo de respuesta (completado)

```python
{
  "user_id": "852892297661906993",
  "token": "ODUyODkyMjk3NjYxOTA2OTkz.GX5Xdp.22jsdSqEiHLUYEJSsjeq_vJKLpOofd5QMksqw32e",
  "user_settings": {
    "locale": "en-US",
    "theme": "dark"
  },
  "required_actions": ["update_password"]
}
```

#### Ejemplo de respuesta (requiere MFA)

```python
{
  "user_id": "852892297661906993",
  "mfa": true,
  "sms": true,
  "ticket": "ODUyODkyMjk3NjYxOTA2OTkz.H2Rpq0.WrhGhYEhM3lHUPN61xF6JcQKwVutk8fBvcoHjo",
  "backup": true,
  "totp": true,
  "webauthn": "{\"publicKey\":{\"challenge\":\"a8a1cHP7_zYheggFG68zKUkl8DwnEqfKvPE-GOMvhss\",\"timeout\":60000,\"rpId\":\"discord.com\",\"allowCredentials\":[{\"type\":\"public-key\",\"id\":\"izrvF80ogrfg9dC3RmWWwW1VxBVBG0TzJVXKOJl__6FvMa555dH4Trt2Ub8AdHxNLkQsc0unAGcn4-hrJHDKSO\"}],\"userVerification\":\"preferred\"}}"
}
```

#### Verificar inicio de sesión con MFA <a href="#verificar-inicio-de-sesin-con-mfa" id="verificar-inicio-de-sesin-con-mfa"></a>

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /auth/mfa/{authenticator\_type}

Verifica un inicio de sesión de múltiples factores y recupera un token de autenticación usando el tipo de autenticador especificado.

{% hint style="info" %}
Para verificar mediante MFA por SMS, primero debes enviar un código al número de teléfono del usuario utilizando el endpoint **Send MFA SMS**.
{% endhint %}

{% hint style="warning" %}
Si la cuenta del usuario está suspendida por Discord, la solicitud de inicio de sesión fallará con un error **403 Forbidden** y un cuerpo de respuesta de error especial, similar a una respuesta exitosa:

```python
{ "suspended_user_token": "ODUyODkyMjk3NjYxOTA2OTkz.Hcj0Nl.YEJSsjeq_vJKLpOofd5QMksqw32e" }
```

Los tokens de autenticación suspendidos no pueden utilizarse como tokens de autenticación normales. Solo pueden usarse para ver el estado de la cuenta y apelar la suspensión de la cuenta.
{% endhint %}

#### Parámetros JSON

| Campo                | Tipo    | Descripción                                                    |
| -------------------- | ------- | -------------------------------------------------------------- |
| ticket               | string  | El ticket de MFA recibido de la solicitud de inicio de sesión. |
| code                 | string  | El código MFA (TOTP, SMS, backup, o WebAuthn) a verificar.     |
| login\_source?       | ?string | El origen de la solicitud de inicio de sesión.                 |
| gift\_code\_sku\_id? | ?string | El ID de SKU del código de regalo.                             |

1 Para WebAuthn, el parámetro `code` debe ser un objeto JSON stringificado de la respuesta de credencial de clave pública.

#### Cuerpo de respuesta

| Campo          | Tipo                  | Descripción                             |
| -------------- | --------------------- | --------------------------------------- |
| token          | string                | El token de autenticación.              |
| user\_settings | login settings object | Las preferencias parciales del usuario. |

#### Ejemplo respuesta (completado):

```python
{
  "token": "ODUyODkyMjk3NjYxOTA2OTkz.GX5Xdp.22jsdSqEiHLUYEJSsjeq_vJKLpOofd5QMksqw32e",
  "user_settings": {
    "locale": "en-US",
    "theme": "dark"
  }
}
```

#### Inicio de sesión sin contraseña <a href="#inicio-de-sesin-sin-contrasea" id="inicio-de-sesin-sin-contrasea"></a>

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /auth/conditional/start

`No autenticado`

Genera un challenge para iniciar el flujo Conditional UI de WebAuthn.

{% hint style="info" %}
Si este endpoint se solicita con un token de autenticación válido, se devolverá un token de autenticación independientemente del cuerpo de la solicitud.
{% endhint %}

#### Cuerpo de respuesta

| Campo     | Tipo   | Descripción                                                                                    |
| --------- | ------ | ---------------------------------------------------------------------------------------------- |
| ticket    | string | Ticket para el login WebAuthn.                                                                 |
| challenge | string | Challenge stringificado JSON para opciones de request de credencial de clave pública WebAuthn. |

#### Ejemplo de respuesta

```python
{
  "challenge": "{\"publicKey\":{\"challenge\":\"sLPruFUWBzowZjYy5d2caF2067pw44butrN0iHm_8k4\",\"timeout\":60000,\"rpId\":\"discord.com\",\"allowCredentials\":[],\"userVerification\":\"required\",\"extensions\":{\"uvm\":true}},\"mediation\":\"conditional\"}",
  "ticket": "b9f98b82-c3a7-49b6-b881-f83418fa2dbe"
}
```

#### Finalizar inicio de sesión sin contraseña

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /auth/conditional/finish

`No autenticado`

Recupera un token de autenticación para las credenciales de WebAuthn proporcionadas.

#### Parámetros JSON

| Campo                | Tipo    | Descripción                                                                |
| -------------------- | ------- | -------------------------------------------------------------------------- |
| ticket               | string  | Ticket de login WebAuthn recibido previamente.                             |
| credential           | string  | Respuesta de credencial de clave pública WebAuthn como JSON stringificado. |
| login\_source?       | ?string | El origen de la solicitud.                                                 |
| gift\_code\_sku\_id? | ?string | El SKU ID del código de regalo.                                            |

#### Cuerpo de respuesta

| Campo              | Tipo                  | Descripción                                                        |
| ------------------ | --------------------- | ------------------------------------------------------------------ |
| user\_id           | snowflake             | El ID del usuario que inició sesión.                               |
| token              | string                | El token de autenticación.                                         |
| user\_settings     | login settings object | Preferencias parciales del usuario.                                |
| required\_actions? | array\[string]        | Acciones adicionales requeridas antes de continuar usando Discord. |

#### Ejemplo de respuesta

```python
{
  "user_id": "852892297661906993",
  "token": "ODUyODkyMjk3NjYxOTA2OTkz.GX5Xdp.22jsdSqEiHLUYEJSsjeq_vJKLpOofd5QMksqw32e",
  "user_settings": {
    "locale": "en-US",
    "theme": "dark"
  },
  "required_actions": ["update_password"]
}
```

#### Autorizar dirección IP <a href="#autorizar-direccin-ip" id="autorizar-direccin-ip"></a>

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /auth/authorize-ip

`No autenticado`

Autoriza la dirección IP del cliente para el inicio de sesión. Retorna una respuesta vacía 204 en caso de éxito.

#### Parámetros JSON

| Campo | Tipo   | Descripción                                                                     |
| ----- | ------ | ------------------------------------------------------------------------------- |
| token | string | Token de verificación recibido del proceso de verificación de email o teléfono. |

#### Crear token de transferencia <a href="#crear-token-de-transferencia" id="crear-token-de-transferencia"></a>

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /auth/handoff\
`No autenticado`

Crea un token de transferencia para pasar el estado de autenticación del usuario a otro cliente.

**Parámetros JSON**

| CAMPO | TIPO   | DESCRIPCIÓN                                                                                      |
| ----- | ------ | ------------------------------------------------------------------------------------------------ |
| key   | string | Una clave única para identificar la solicitud de transferencia (por ejemplo, un UUID aleatorio). |

**Cuerpo de respuesta**

| CAMPO          | TIPO   | DESCRIPCIÓN                                                                                 |
| -------------- | ------ | ------------------------------------------------------------------------------------------- |
| handoff\_token | string | El token de transferencia para pasar al destinatario; esto no es un token de autenticación. |

### Canjear token de transferencia <a href="#canjear-token-de-transferencia" id="canjear-token-de-transferencia"></a>

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /auth/handoff/exchange\
`No autenticado`

Canjea un token de transferencia por un token de autenticación. Los tokens de transferencia solo pueden ser canjeados una vez y solo son válidos desde la misma IP en la que fueron creados.

**Parámetros JSON**

| CAMPO          | TIPO   | DESCRIPCIÓN                                                                   |
| -------------- | ------ | ----------------------------------------------------------------------------- |
| key            | string | La clave única usada para crear el token de transferencia.                    |
| handoff\_token | string | El token de transferencia recibido del endpoint Crear token de transferencia. |

**Cuerpo de respuesta**

| CAMPO | TIPO                   | DESCRIPCIÓN                     |
| ----- | ---------------------- | ------------------------------- |
| token | string                 | El token de autenticación.      |
| user  | objeto parcial usuario | El usuario que fue autenticado. |

### Registro <a href="#registro" id="registro"></a>

Los usuarios potenciales deben registrar una cuenta antes de poder usar la mayoría de la plataforma.

El proceso completo incluye elegir un nombre de usuario, nombre para mostrar, correo o teléfono, contraseña y fecha de nacimiento. El cliente puede omitir algunos pasos para experiencias más rápidas (por ejemplo, al ver una invitación).

Para registrarse, primero el cliente debe recuperar la metadata de localización del usuario para determinar si se requiere consentimiento explícito. Luego, puede usar el endpoint Register Account. Durante el proceso, puede usar Get Unique Username Suggestions para sugerencias y Get Unique Username Eligibility para validarlas.

Una vez registrado, el cliente debe guardar el token de autenticación para evitar tener que iniciar sesión nuevamente después.

#### Registro por teléfono

Como alternativa, el cliente puede registrar una cuenta con un número de teléfono. Para hacerlo, primero debe usar Register Account with Phone Number, luego verificar el número con el código recibido y completar el registro usando Register Account normalmente.

#### Endpoints de registro

#### Registrar cuenta

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /auth/register

Crea una nueva cuenta y recupera un token de autenticación para las credenciales proporcionadas.

#### Parámetros JSON

<sup>1</sup> Uno de `username` o `global_name` debe ser provisto. Un nombre válido se puede obtener con el endpoint Get Unique Username Suggestions y validar con Get Unique Username Eligibility. Mira la sección de nombres de usuario y apodos para más detalles de restricciones.

<sup>2</sup> Este valor debe estar configurado igual que el fingerprint usado durante el flujo de autenticación. Con un registro válido, la nueva cuenta compartirá el mismo ID que el fingerprint para asegurar continuidad de experimentos.

<sup>3</sup> Si la invitación es válida, se acepta automáticamente al registrar.

<sup>4</sup> Se puede saber si se requiere consentimiento explícito usando el endpoint Get Location Metadata.

| Campo                        | Tipo         | Descripción                                                                        |
| ---------------------------- | ------------ | ---------------------------------------------------------------------------------- |
| username?                    | string       | El nombre de usuario a registrar (por defecto aleatorio).                          |
| global\_name?                | ?string      | El nombre para mostrar a registrar.                                                |
| email?                       | string       | El correo electrónico del usuario.                                                 |
| phone\_token?                | string       | El token de verificación telefónica recibido del flujo de registro por teléfono.   |
| password?                    | string       | Contraseña del usuario (8-72 caracteres).                                          |
| date\_of\_birth?             | ISO8601 date | Fecha de nacimiento del usuario.                                                   |
| fingerprint?                 | string       | Fingerprint a usar para el registro.                                               |
| invite?                      | ?string      | Código de invitación que inició el registro.                                       |
| guild\_template\_code?       | ?string      | Código de plantilla de servidor que inició el registro.                            |
| gift\_code\_sku\_id?         | ?string      | El SKU ID del código de regalo que inició el registro.                             |
| consent?                     | boolean      | Si el usuario acepta los Términos de Servicio y Política de Privacidad de Discord. |
| promotional\_email\_opt\_in? | boolean      | Si el usuario acepta o no recibir correos promocionales de Discord.                |

#### Cuerpo de respuesta

| Campo                     | Tipo    | Descripción                                                                                   |
| ------------------------- | ------- | --------------------------------------------------------------------------------------------- |
| token                     | string  | El token de autenticación.                                                                    |
| show\_verification\_form? | boolean | Si el usuario debe ver el formulario de verificación de miembros del servidor al que se unió. |

#### Ejemplo de respuesta

```python
{
  "token": "ODUyODkyMjk3NjYxOTA2OTkz.GX5Xdp.22jsdSqEiHLUYEJSsjeq_vJKLpOofd5QMksqw32e",
  "show_verification_form": true
}
```

#### Registrar con número de teléfono

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /auth/register/phone

Envía un código de verificación al número de teléfono del usuario para registrar una cuenta. Retorna vacía 204 si es exitoso. Primero debe usarse el código para verificar el teléfono antes del registro final.

#### Parámetros JSON

| Campo | Tipo   | Descripción                            |
| ----- | ------ | -------------------------------------- |
| phone | string | Teléfono del usuario en formato E.164. |

#### Obtener metadata de localización

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /auth/location-metadata

`No autenticado`

Retorna la metadata de localización para la IP del usuario.

#### Cuerpo de respuesta

| Campo                       | Tipo                              | Descripción                                                                     |
| --------------------------- | --------------------------------- | ------------------------------------------------------------------------------- |
| country\_code               | string                            | Código de país ISO 3166-1 alpha-2 de la IP.                                     |
| consent\_required           | boolean                           | Si el usuario debe aceptar expresamente Términos y Privacidad para registrarse. |
| promotional\_email\_opt\_in | promotional email metadata object | Metadata de consentimiento para promocionales.                                  |

#### Estructura de metadatos promocionales

| Campo        | Tipo    | Descripción                                                                           |
| ------------ | ------- | ------------------------------------------------------------------------------------- |
| required     | boolean | Si el usuario debe aceptar expresamente recibir correos promocionales.                |
| pre\_checked | boolean | Si la casilla debe venir seleccionada por defecto en caso de requerir consentimiento. |

#### Ejemplo de respuesta

```python
{
  "consent_required": false,
  "country_code": "CA",
  "promotional_email_opt_in": {
    "required": true,
    "pre_checked": false
  }
}
```

#### Validar fortaleza de contraseña

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /auth/password/validate

`No autenticado`

Valida la seguridad de una contraseña y retorna un puntaje basado en su complejidad. Se usa para asegurar que la contraseña cumpla los requisitos de seguridad de Discord.

#### Parámetros JSON

| Campo    | Tipo   | Descripción                             |
| -------- | ------ | --------------------------------------- |
| password | string | Contraseña a validar (8-72 caracteres). |

#### Cuerpo de respuesta

| Campo              | Tipo    | Descripción                               |
| ------------------ | ------- | ----------------------------------------- |
| valid              | boolean | Si la contraseña es válida según Discord. |
| password\_strength | integer | Puntaje de robustez (0-4).                |

#### Sugerencias de nombre de usuario único

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /unique-username/username-suggestions-unauthed

`No autenticado`

Retorna una sugerencia de nombre de usuario único para registro.

#### Parámetros query string

| Campo         | Tipo   | Descripción                                               |
| ------------- | ------ | --------------------------------------------------------- |
| global\_name? | string | Nombre base para las sugerencias (por defecto aleatorio). |

#### Cuerpo de respuesta

| Campo    | Tipo   | Descripción                      |
| -------- | ------ | -------------------------------- |
| username | string | Sugerencia de nombre de usuario. |

#### Ejemplo de respuesta

```python
{ "username": "gnarp.gnap" }
```

#### Comprobar disponibilidad de nombre de usuario

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /unique-username/username-attempt-unauthed

`No autenticado`

Comprobar si un nombre está disponible para registrar.

{% hint style="info" %}
Consulta la sección de **nombres de usuario y apodos** para obtener más información sobre las restricciones de nombres de usuario.
{% endhint %}

#### Parámetros JSON

| Campo    | Tipo   | Descripción                    |
| -------- | ------ | ------------------------------ |
| username | string | Nombre de usuario a verificar. |

#### Cuerpo de respuesta

| Campo | Tipo     | Descripción                |
| ----- | -------- | -------------------------- |
| taken | ?boolean | Si el nombre está ocupado. |

#### Ejemplo

```python
{ "taken": true }
```

### Cerrar sesión

Para cerrar sesión, el cliente debe usar el endpoint de **Logout** con el token de autenticación del usuario. Esto se utiliza para invalidar el token y evitar que se envíen más notificaciones push al cliente. Consulta la sección de notificaciones push para más información.

#### Endpoints

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /auth/logout

Invalida la sesión y desregistra el token de notificaciones push dado.

#### Parámetros JSON

<sup>1</sup> Los tokens de notificaciones push específicos de VOIP solo se utilizan con PushKit en iOS.

| Campo           | Tipo   | Descripción                                              |
| --------------- | ------ | -------------------------------------------------------- |
| provider?       | string | Proveedor de push a revocar.                             |
| token?          | string | Token a desregistrar.                                    |
| voip\_provider? | string | Proveedor VOIP de push a revocar (solo PushKit en iOS).  |
| voip\_token?    | string | Token VOIP de push a desregistrar (solo PushKit en iOS). |

### Recuperar contraseña <a href="#recuperar-contrasea" id="recuperar-contrasea"></a>

Si un usuario olvida la contraseña, puede restablecerla usando su correo o teléfono. Para iniciar el proceso usar el endpoint Forgot Password.

El usuario debe obtener el token de reseteo de correo o SMS y luego usar Reset Password para definir una nueva contraseña.

#### Endpoints

#### Iniciar recuperación

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /auth/forgot

`No autenticado`

Inicia el proceso enviando correo o SMS al usuario. Responde vacía 204 si es exitoso.

{% hint style="info" %}
Si se proporciona un correo electrónico, el usuario recibirá un enlace que redirige al cliente oficial de Discord con un token de verificación presente en el fragmento de la URL (por ejemplo: `https://discord.com/reset#token=eyJpZCI6ODUyODkyMjk3NjYxOTA2OTkzLCJlbWFpbCI6Im5lbGx5QGRpc2NvcmRhcHAuY29tIn0.Z6pQDg.pKCZBaaiodflO6FZhdttm6B_z74`).

Si se proporciona un número de teléfono, la solicitud fallará con un código de error JSON `70007`, y el usuario recibirá un código de verificación vía SMS. Se debe obtener un token de verificación verificando el número de teléfono.

Si el usuario no es elegible para restablecer su contraseña mediante número de teléfono, la solicitud de verificación del número fallará con un código de error JSON `70009` y el usuario recibirá un enlace para restablecer su contraseña por correo electrónico.
{% endhint %}

#### Parámetros JSON

| Campo | Tipo   | Descripción                                                |
| ----- | ------ | ---------------------------------------------------------- |
| login | string | Correo electrónico o número de teléfono E.164 del usuario. |

#### Restablecer contraseña

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /auth/reset

`No autenticado`

Resetea la contraseña y recupera un nuevo token de autenticación.

{% hint style="info" %}
Al intentar restablecer la contraseña de un usuario que tiene habilitada la autenticación multifactor (MFA), la solicitud devolverá una respuesta similar a la de **MFA Requerida**.

Para completar el restablecimiento de la contraseña, el cliente debe reintentar la solicitud especificando los parámetros `ticket` y `code`.
{% endhint %}

#### Parámetros JSON

<sup>1</sup> Para WebAuthn, el parámetro `code` debe ser un objeto JSON stringificado.

| Campo    | Tipo   | Descripción                                                          |
| -------- | ------ | -------------------------------------------------------------------- |
| token    | string | Token recibido por email o SMS.                                      |
| password | string | Nueva contraseña (8-72 caracteres).                                  |
| source?  | string | Ruta fuente desde donde se inició el reseteo (por ejemplo `/reset`). |
| method?  | string | Tipo de autenticación para MFA.                                      |
| ticket?  | string | Ticket de MFA previo.                                                |
| code?    | string | Código MFA (TOTP, SMS, backup, o WebAuthn) a verificar.              |

#### Cuerpo de respuesta

| Campo     | Tipo      | Descripción                                               |
| --------- | --------- | --------------------------------------------------------- |
| token?    | string    | Token de autenticación si el reset se completó.           |
| user\_id? | snowflake | ID del usuario si se requiere MFA.                        |
| ticket?   | string    | Ticket para reintentar con MFA.                           |
| mfa?      | boolean   | Si es requerido MFA para restablecer (por defecto falso). |
| totp?     | boolean   | Si el usuario tiene habilitado TOTP.                      |
| sms?      | boolean   | Si tiene habilitado MFA por SMS.                          |
| backup?   | boolean   | Si se pueden usar backup codes.                           |
| webauthn? | ?string   | Cadena JSON de opciones WebAuthn.                         |

#### Ejemplo completado

```python
{ "token": "ODUyODkyMjk3NjYxOTA2OTkz.GX5Xdp.22jsdSqEiHLUYEJSsjeq_vJKLpOofd5QMksqw32e" }
```

#### Ejemplo con MFA requerido

```python
{
  "user_id": "852892297661906993",
  "mfa": true,
  "sms": true,
  "ticket": "ODUyODkyMjk3NjYxOTA2OTkz.H2Rpq0.WrhGhYEhM3lHUPN61xF6JcQKwVutk8fBvcoHjo",
  "backup": true,
  "totp": true,
  "webauthn": "{\"publicKey\":{\"challenge\":\"a8a1cHP7_zYheggFG68zKUkl8DwnEqfKvPE-GOMvhss\",\"timeout\":60000,\"rpId\":\"discord.com\",\"allowCredentials\":[{\"type\":\"public-key\",\"id\":\"izrvF80ogrfg9dC3RmWWwW1VxBVBG0TzJVXKOJl__6FvMa555dH4Trt2Ub8AdHxNLkQsc0unAGcn4-hrJHDKSO\"}],\"userVerification\":\"preferred\"}}"
}
```

### Recuperación de cuenta <a href="#recuperacin-de-cuenta" id="recuperacin-de-cuenta"></a>

La recuperación de cuenta es una función pensada para recuperar cuentas después de una toma de control, cuando un atacante ya ha cambiado el email, contraseña, teléfono o ajustes de MFA.

Cuando se cambia el email, Discord enviará un mensaje notificando el cambio y ofreciendo al usuario la opción de iniciar la recuperación de cuenta. El correo incluirá un enlace que redirige al cliente oficial de Discord con un token de reversión en la URL (ejemplo: `https://discord.com/wasntme/BIGJWTHERE`). Este token es válido por dos días.

Se puede usar este token con el endpoint Revert Account para revertir el estado previo de la cuenta. Al finalizar, se elimina el número actual de la cuenta y todos los anteriores quedan en lista negra para prevenir posteriores ataques SIM.

#### Endpoints

#### Revertir cuenta

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /auth/revert

Recupera una cuenta después de una toma de control. Este endpoint:

* Invalida el token de reversión usado
* Invalida todos los tokens de autorización y sesiones Gateway asociadas
* Devuelve el email original a la cuenta
* Actualiza la contraseña al valor dado
* Elimina el MFA de la cuenta
* Elimina y pone en lista negra todos los teléfonos asociados previamente

{% hint style="warning" %}
Este proceso fallará si la cuenta fue eliminada previamente (ej. anti-spam o un empleado de Discord) o si el correo electrónico original fue asignado a una cuenta diferente.
{% endhint %}

#### Parámetros JSON

| Campo    | Tipo   | Descripción                                       |
| -------- | ------ | ------------------------------------------------- |
| token    | string | El token de reversión del correo de recuperación. |
| password | string | La nueva contraseña de la cuenta.                 |

#### Cuerpo de respuesta

| Campo  | Tipo   | Descripción                        |
| ------ | ------ | ---------------------------------- |
| string | string | El correo restaurado de la cuenta. |

#### Ejemplo de respuesta

```python
{ "email": "alien@shiroko.me" }
```

### Verificación MFA <a href="#verificacin-mfa" id="verificacin-mfa"></a>

En algunos casos se requerirá verificación MFA antes de realizar operaciones sensibles. Cuando esto suceda, recibirás un 401 con un cuerpo especial:

#### Estructura de respuesta MFA requerida

| Campo   | Tipo                            | Descripción                                              |
| ------- | ------------------------------- | -------------------------------------------------------- |
| message | string                          | Mensaje indicando que se requiere MFA para la operación. |
| code    | integer                         | Código de error (siempre será `60003`).                  |
| mfa     | MFA verification request object | Objeto de solicitud de verificación MFA.                 |

#### Estructura de solicitud de verificación MFA

| Campo   | Tipo   | Descripción                                      |
| ------- | ------ | ------------------------------------------------ |
| ticket  | string | Ticket MFA.                                      |
| methods | array  | Métodos MFA permitidos para verificar identidad. |

#### Estructura de método MFA

| Campo                   | Tipo    | Descripción                                           |
| ----------------------- | ------- | ----------------------------------------------------- |
| type                    | string  | El tipo de método MFA.                                |
| challenge?              | string  | Challenge JSON para WebAuthn.                         |
| backup\_codes\_allowed? | boolean | Si se pueden usar códigos de respaldo además de TOTP. |

#### Tipos de autenticador

<sup>1</sup> La contraseña solo se usa si el usuario no tiene otro MFA habilitado.

| Valor    | Descripción                               |
| -------- | ----------------------------------------- |
| totp     | Verificación usando TOTP o backup code.   |
| sms      | Verificación con código vía SMS.          |
| backup   | Verificación usando código de respaldo.   |
| webauthn | Verificación usando dispositivo WebAuthn. |
| password | Verificación usando la contraseña.        |

#### Ejemplo de respuesta MFA requerida

```python
{
  "message": "Two factor is required for this operation",
  "code": 60003,
  "mfa": {
    "ticket": "ODUyODkyMjk3NjYxOTA2OTkz.H2Rpq0.WrhGhYEhM3lHUPN61xF6JcQKwVutk8fBvcoHjo",
    "methods": [
      {
        "type": "webauthn",
        "challenge": "{\"publicKey\":{\"challenge\":\"a8a1cHP7_zYheggFG68zKUkl8DwnEqfKvPE-GOMvhss\",\"timeout\":60000,\"rpId\":\"discord.com\",\"allowCredentials\":[{\"type\":\"public-key\",\"id\":\"izrvF80ogrfg9dC3RmWWwW1VxBVBG0TzJVXKOJl__6FvMa555dH4Trt2Ub8AdHxNLkQsc0unAGcn4-hrJHDKSO\"}],\"userVerification\":\"preferred\"}}"
      },
      {
        "type": "totp",
        "backup_codes_allowed": true
      },
      {
        "type": "sms"
      },
      {
        "type": "backup"
      }
    ]
  }
}
```

Para verificar, usa el endpoint Verify MFA con el ticket recibido. El JWT recibido podrá insertarse en la cabecera `X-Discord-MFA-Authorization` y reintentar la petición original.

Al elevar exitosamente, el JWT se entregará en la cookie `__Secure-recent_mfa`, permitiendo saltar MFA 5 minutos.

#### Endpoints

#### Verificar MFA

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /mfa/finish

Verifica la identidad del usuario usando autenticación multifactor. Si tiene éxito, retorna una cookie permitiendo saltar MFA durante 5 minutos.

#### Parámetros JSON

<sup>1</sup> Para WebAuthn, el dato debe ser un JSON stringificado.

| Campo     | Tipo   | Descripción                                                           |
| --------- | ------ | --------------------------------------------------------------------- |
| ticket    | string | Ticket MFA recibido de la respuesta de MFA requerida.                 |
| mfa\_type | string | El tipo de autenticador usado para verificar.                         |
| data      | string | El dato de MFA (TOTP, SMS, backup, WebAuthn, o password) a verificar. |

#### Cuerpo de respuesta

| Campo | Tipo   | Descripción                                    |
| ----- | ------ | ---------------------------------------------- |
| token | string | JWT de verificación MFA (expira en 5 minutos). |

#### Ejemplo de respuesta

```python
{
  "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJpYXQiOjE3MTA4MDY4MDQsIm5iZiI6MTcxMDgwNjgwNCwiZXhwIjoxNzEwODA3MTA0LCJpc3MiOiJ1cm46ZGlzY29yZC1hcGkiLCJhdWQiOiJ1cm46ZGlzY29yZC1tZmEtcmVwcm9tcHQiLCJ1c2VyIjo4NTI4OTIyOTc2NjE5MDY5OTN9.vOCStK0Aj873VaF_cLmSlcnAfw7SO0jrwSeCpkSUvO3li-1jxzwewxY4Ak4fyZvb6VeJtSW-r8_Pfw8HTj8P6w"
}
```

#### Enviar SMS MFA

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /auth/mfa/sms/send

`No autenticado`

Envía un código MFA por SMS para la verificación.

#### Parámetros JSON

| Campo  | Tipo   | Descripción                                                    |
| ------ | ------ | -------------------------------------------------------------- |
| ticket | string | Ticket MFA recibido del login o de la respuesta MFA requerida. |

#### Cuerpo de respuesta

| Campo | Tipo   | Descripción                                            |
| ----- | ------ | ------------------------------------------------------ |
| phone | string | Número de teléfono enmascarado al que se envió el SMS. |

#### Ejemplo de respuesta

```python
{ "phone": "+*******0085" }
```

---
icon: envelope-open-text
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

# Verificación de correo

Las cuentas de Discord normalmente requieren verificación de correo electrónico para desbloquear toda la funcionalidad. Aunque esto normalmente se realiza durante el registro de la cuenta, los usuarios pueden añadir o cambiar su dirección de correo electrónico en cualquier momento.

### Registrar con una dirección de correo <a href="#undefined" id="undefined"></a>

Al registrar una nueva cuenta, los usuarios normalmente proporcionan una dirección de correo electrónico para verificar su cuenta durante el proceso de registro estándar.

### Añadir una dirección de correo <a href="#undefined" id="undefined"></a>

Los usuarios pueden añadir una dirección de correo electrónico a su cuenta en cualquier momento. Esto es requerido para ciertas funciones o, en algunos casos, por razones antiabuso. Es recomendable tener una dirección de correo asociada a tu cuenta.

Para añadir una dirección de correo, los clientes deben primero enviar una solicitud al endpoint `modificar usuario actual` con la dirección que deseen añadir. Esto enviará un enlace de verificación a esa dirección, que redirige al cliente oficial de Discord con un token de verificación en el fragmento de la URL (por ejemplo: `https://discord.com/verify#token=eyJpZCI6ODUyODkyMjk3NjYxOTA2OTkzLCJlbWFpbCI6Im5lbGx5QGRpc2NvcmRhcHAuY29tIn0.Z6pQDg.pKCZBaaiodflO6FZhdttm6B_z74`). Después de recibir el token, los clientes pueden proceder a verificar el cambio para completar el proceso.

### Cambiar tu dirección de correo <a href="#undefined" id="undefined"></a>

Los usuarios pueden cambiar su dirección de correo en cualquier momento. Si la dirección actual está verificada, el usuario deberá tener acceso a ella para iniciar el cambio. En ese caso, los clientes deben primero enviar una solicitud al endpoint `enviar desafío de cambio de correo`, lo que enviará un código de verificación al correo actual del usuario. Después de recibir el código, los clientes pueden enviar una solicitud al endpoint `verificar código de cambio de correo` con ese código para recibir un token de verificación que se puede usar para cambiar el correo mediante el endpoint `modificar usuario actual`.

Si la dirección actual NO está verificada, los clientes pueden directamente enviar una solicitud al endpoint `modificar usuario actual` con el nuevo correo, de la misma manera que al añadir una dirección de correo.

Finalmente, el usuario debe verificar el nuevo correo como se explica en el flujo anterior.

### Reverificar tu dirección de correo <a href="#undefined" id="undefined"></a>

A los usuarios se les puede pedir que reverifiquen su correo por razones antiabuso.

En ese caso, se enviará un enlace de verificación automáticamente a la dirección registrada. Si el usuario no ha recibido un enlace, los clientes pueden elegir reenviarlo. El enlace debe usarse para verificar la dirección como se explica en el flujo de añadir correo.

### Eliminar tu dirección de correo <a href="#undefined" id="undefined"></a>

Los usuarios no pueden eliminar el correo registrado en su cuenta. En su lugar, deben cambiarlo por uno nuevo o eliminar su cuenta por completo.

### Endpoints <a href="#undefined" id="undefined"></a>

#### Verificar dirección de correo

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /auth/verify

[Sin Autenticar](../discord-api.md#solicitud-no-autenticada)

Verifica una dirección de correo y la vincula a la cuenta del usuario. Dispara un evento Gateway Actualización de Usuario.

#### Parámetros JSON

| Campo | Tipo   | Descripción                                 |
| ----- | ------ | ------------------------------------------- |
| token | string | El token de verificación recibido por email |

#### Cuerpo de Respuesta

| Campo    | Tipo    | Descripción                                  |
| -------- | ------- | -------------------------------------------- |
| user\_id | string  | El ID del usuario cuyo correo fue verificado |
| token 1  | ?string | El token de autenticación para el usuario    |

Nota: No se devuelve un token si el usuario tiene MFA activado y no se proporciona autorización válida.

#### Reenviar correo de verificación

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /auth/verify/resend

Reenvía un enlace de verificación a la dirección de correo del usuario. Requiere que la cuenta tenga un correo marcado como no verificado. Devuelve una respuesta vacía 204 en caso de éxito.

#### Enviar desafío de cambio de correo

<kbd><mark style="color:$warning;background-color:$warning;">PUT<mark style="color:$warning;background-color:$warning;"></kbd> /users/@me/email

Envía un correo al usuario actual con un código de verificación para iniciar el proceso de cambio de correo. Devuelve una respuesta vacía 204 en caso de éxito.

#### Verificar código de cambio de correo

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /users/@me/email/verify-code

Verifica el código de cambio de correo enviado al correo del usuario. Si es correcto, el token devuelto puede usarse con el endpoint `modificar usuario actual` para cambiar el correo.

#### Parámetros JSON

| Campo | Tipo   | Descripción                |
| ----- | ------ | -------------------------- |
| code  | string | El código de verificación. |

#### Cuerpo de Respuesta

| Campo | Tipo   | Descripción                                     |
| ----- | ------ | ----------------------------------------------- |
| token | string | El token a usar para la verificación de correo. |

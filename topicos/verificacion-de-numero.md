---
icon: mobile-notch
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

# Verificación de número

Discord permite a los usuarios añadir un número de teléfono a su cuenta por varios motivos, como seguridad de la cuenta, verificación y sincronización de contactos. Existen distintos flujos de verificación telefónica según el contexto en el que se use el número.

### Registrarse con un número de teléfono <a href="#registrarse-con-un-nmero-de-telfono" id="registrarse-con-un-nmero-de-telfono"></a>

Al registrar una cuenta nueva, los usuarios pueden proporcionar un número de teléfono para verificar su cuenta. Este flujo se detalla en la documentación de registro de cuenta.

### Añadir un número de teléfono <a href="#aadir-un-nmero-de-telfono" id="aadir-un-nmero-de-telfono"></a>

Los usuarios pueden añadir un número de teléfono a su cuenta en cualquier momento. Esto es necesario para ciertas funciones, o, en algunos casos, por motivos de prevención de abuso.

Para añadir un número de teléfono, los clientes primero deben enviar una solicitud al endpoint de Añadir número de teléfono con el número que desean añadir y, preferiblemente, una razón para el cambio. Esto enviará un código de verificación al número vía SMS. Tras recibir el código, los clientes deben utilizar el endpoint de Verificar número de teléfono con el número y el código para recibir un token de verificación. Al recibir el token, los clientes deben usar de nuevo el endpoint de Añadir número de teléfono, proporcionando el token, la contraseña del usuario y la misma razón para completar el proceso.

{% hint style="info" %}
Los números de teléfono solo pueden asociarse con una cuenta de Discord a la vez. Añadir un número ya asociado a otra cuenta puede hacer que se elimine de esa otra cuenta. Registrar una cuenta nueva con un número ya asociado a otra cuenta fallará, y el usuario tendrá que eliminar el número de la otra cuenta primero.
{% endhint %}

### Reverificar tu número de teléfono <a href="#reverificar-tu-nmero-de-telfono" id="reverificar-tu-nmero-de-telfono"></a>

Es posible que se le pida a los usuarios que reverifiquen su teléfono por motivos de prevención de abuso.

En este caso, los clientes deben seguir el mismo flujo que para añadir un número, pero en vez del endpoint de Añadir número de teléfono, deben usar el endpoint de Reverificar número de teléfono.

### Quitar tu número de teléfono <a href="#quitar-tu-nmero-de-telfono" id="quitar-tu-nmero-de-telfono"></a>

Los usuarios pueden eliminar el número de teléfono de su cuenta en cualquier momento. Ten en cuenta que si el número fue añadido por motivos de prevención de abuso, puede que se requiera añadir otro número antes de seguir usando la cuenta.

Para quitar un número, los clientes deben enviar una solicitud al endpoint de Quitar número de teléfono junto con la contraseña del usuario y una razón opcional para la eliminación.

#### Motivo para cambiar el teléfono

| Valor                  | Descripción                                                                   |
| ---------------------- | ----------------------------------------------------------------------------- |
| user\_action\_required | El número se requiere por motivos de prevención de abuso.                     |
| user\_settings\_update | El número se añade manualmente por el usuario.                                |
| guild\_phone\_required | Se requiere el número para cumplir requisitos de verificación de un servidor. |
| mfa\_phone\_update     | Se desea el número para activar MFA vía SMS.                                  |
| contact\_sync          | Se desea el número para sincronizar contactos.                                |

### Endpoints <a href="#endpoints" id="endpoints"></a>

#### Verificar número de teléfono

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /phone-verifications/verify&#x20;

<sub>No autenticado</sub>

Verifica un número de teléfono para su uso en Discord. El código debe enviarse primero al número como parte de un flujo de verificación.

#### Parámetros JSON

| Campo | Tipo   | Descripción                                                   |
| ----- | ------ | ------------------------------------------------------------- |
| phone | string | El número de teléfono en formato E.164 que se va a verificar. |
| code  | string | El código recibido por SMS.                                   |

#### Cuerpo de la respuesta

| Campo | Tipo   | Descripción                                         |
| ----- | ------ | --------------------------------------------------- |
| token | string | El token para usar en la verificación del teléfono. |

#### Reenviar código de verificación

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /phone-verifications/resend&#x20;

<sub>No autenticado</sub>

Reenvía un código de verificación al número dado. Devuelve un 204 (respuesta vacía) en caso de éxito.

#### Parámetros JSON

| Campo | Tipo   | Descripción                                                       |
| ----- | ------ | ----------------------------------------------------------------- |
| phone | string | El número de teléfono en formato E.164 al que reenviar el código. |

#### Añadir número de teléfono

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /users/@me/phone

Añade un número de teléfono a la cuenta del usuario actual. Devuelve un 204 (respuesta vacía) en caso de éxito. Dispara un evento de Gateway de Actualización de usuario y, opcionalmente, de Actualización de acción requerida por el usuario.

{% hint style="info" %}
Al proporcionar el parámetro `phone`, se enviará un código de verificación al número. Tras verificar el número, la petición se debe repetir proporcionando `phone_token` y `password` (y NO `phone`) para completar el proceso.
{% endhint %}

#### Parámetros JSON

| Campo                  | Tipo   | Descripción                                                                            |
| ---------------------- | ------ | -------------------------------------------------------------------------------------- |
| phone?                 | string | El número de teléfono en formato E.164 al que se enviará el código.                    |
| phone\_token?          | string | El token de verificación obtenido en el flujo de registro del teléfono.                |
| password?              | string | La contraseña actual del usuario; si la cuenta no tiene contraseña, esto la establece. |
| change\_phone\_reason? | string | El motivo (véase tabla anterior) para añadir el número.                                |

#### Reverificar número de teléfono

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /users/@me/phone/reverify

Reverifica un número de teléfono para el usuario actual. Solo se debe usar este endpoint cuando se recibe una acción requerida relevante. El número proporcionado debe coincidir con el que ya está asociado a la cuenta. Dispara evento de Actualización de usuario y, opcionalmente, Actualización de acción requerida por el usuario.

{% hint style="info" %}
Al proporcionar el parámetro `phone`, se enviará un código de verificación al número. Tras verificar el número, la petición se debe repetir proporcionando `phone_token` y `password` (y NO `phone`) para completar el proceso.
{% endhint %}

#### Parámetros JSON

| Campo                  | Tipo   | Descripción                                                                            |
| ---------------------- | ------ | -------------------------------------------------------------------------------------- |
| phone?                 | string | El número de teléfono en formato E.164 al que se enviará el código.                    |
| phone\_token?          | string | El token de verificación obtenido en el flujo de registro del teléfono.                |
| password?              | string | La contraseña actual del usuario; si la cuenta no tiene contraseña, esto la establece. |
| change\_phone\_reason? | string | El motivo para añadir el número (véase tabla anterior).                                |

#### Quitar número de teléfono

<kbd><mark style="color:$danger;background-color:$danger;">DELETE<mark style="color:$danger;background-color:$danger;"></kbd> /users/@me/phone&#x20;

<sub>MFA Required</sub>

Elimina el número de teléfono del usuario actual. Devuelve un 204 (respuesta vacía) en caso de éxito. Dispara evento de Actualización de usuario y, opcionalmente, Actualización de acción requerida por el usuario.

#### Parámetros JSON

| Campo                 | Tipo   | Descripción                                                                           |
| --------------------- | ------ | ------------------------------------------------------------------------------------- |
| password              | string | La contraseña actual del usuario; si la cuenta no tiene contraseña, esto la establece |
| change\_phone\_reason | string | El motivo para la eliminación (véase tabla anterior)                                  |

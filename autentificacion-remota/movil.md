---
icon: mobile-button
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

# Móvil

En el contexto de la autenticación remota, el cliente móvil es el dispositivo que está autenticado y dispuesto a transferir las credenciales al cliente de escritorio. Consulta el [resumen](resumen.md) para más información.

### Protocolo

Tras escanear el código QR, el cliente móvil debe extraer la huella del enlace recibido (el enlace debe tener el formato `https://discord.com/ra/`).

Una vez que el cliente tiene una huella, puede crear una nueva sesión de autenticación remota usando `crear sesión de autenticación remota`. Después de establecer la sesión, el cliente debe pedir al usuario aceptar o denegar la solicitud, y después realizar dicha acción usando `finalizar autenticación remota` o `cancelar autenticación remota`.

### Endpoints

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /users/@me/remote-auth

Crea una nueva sesión de autenticación remota. Esto envía la información del usuario actual al cliente de escritorio.

**Parámetros JSON**

| Campo       | Tipo   | Descripción                                                                 |
| ----------- | ------ | --------------------------------------------------------------------------- |
| fingerprint | string | La huella correspondiente al cliente de autenticación remota de escritorio. |

**Cuerpo de respuesta**

| Campo            | Tipo   | Descripción                                                                                      |
| ---------------- | ------ | ------------------------------------------------------------------------------------------------ |
| handshake\_token | string | El token de apretón de manos ("handshake") que puede usarse para finalizar o cancelar la sesión. |

**Ejemplo de respuesta**

```
{
  "handshake_token": ".eJwVkcmSokAARP_Fq0MrICIdExNRYOECsigt4GUCpCgKZC9A6Jh_H_qWkS_z9L4XRRmhxeeiQXlJERN0NGFwQNEQjEzVREwekILBYk8XvxYViebl7_UHy0nbD4Hjtn9-2i58keffDI0zvJxO8ikFhoyzOsnIQRrWMrChCoCpAHsHfriCtTlD0Efmmm6yIp0IyU3Xqh_x-7gStVU3VncD0YPlbZUq13bQTZ5Vfuv5UociKa9o4tsrpW0YxlN5o8hJ3mxgIs3NuDxrB5t04uT52zwAV2cP-4to-ZrFtkF8O661rsKm8CBRYZPaIJCeVff4bNG46YYG5KEEy57FUFar4LDVAOxPHvsKRzGwJQrfkn69ZVjodc5MdyvNp1ZZi6p98KaXEeovaADgAGq4S8vBUTRekH6_sRKHU37ZjpMAqnOw11Ez8nIb5piEvhBmPZryM1ajJnf5DutdXTzEOnoWp7sq6Ks0lvda7bylOG-Mwq2VFCciai8edFvS3Aek1PoxvTRLtnv1fKoNpz2wgTx7iUmBUVM1pKCzmC_iHGiQbiNlE2mGs4OR5COg7_ma7mJmCr0g4fBd9ouv9fztUdOSslh8cv_-A8Mft7I.ZSQp6A.dPkJdzlOjDn1hIxolxZfDu2595k"
}
```

**Finalizar autenticación remota**

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /users/@me/remote-auth/finish

Finaliza una sesión de autenticación remota. Esto finaliza la sesión enviando un token de autenticación al cliente de escritorio. Devuelve una respuesta vacía 204 en caso de éxito.

**Parámetros JSON**

<sup>1</sup> Los tokens de autenticación con expiración todavía no están soportados.

| Campo               | Tipo    | Descripción                                                                    |
| ------------------- | ------- | ------------------------------------------------------------------------------ |
| handshake\_token    | string  | El token de apretón de manos que representa la sesión de autenticación remota. |
| temporary\_token? 1 | boolean | Si el token de autenticación debe expirar (por defecto false).                 |

**Cancelar autenticación remota**

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /users/@me/remote-auth/cancel

Cancela una sesión de autenticación remota. Esto termina la sesión sin enviar un token de autenticación. Devuelve una respuesta vacía 204 en caso de éxito.

**Parámetros JSON**

| Campo            | Tipo   | Descripción                                                                    |
| ---------------- | ------ | ------------------------------------------------------------------------------ |
| handshake\_token | string | El token de apretón de manos que representa la sesión de autenticación remota. |

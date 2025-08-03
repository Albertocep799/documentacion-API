---
icon: bell-ring
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

# Notificaciones push

Las notificaciones push se utilizan para avisar a los usuarios sobre eventos que ocurren en segundo plano, como mensajes o llamadas entrantes.

Después de autenticarse, un cliente móvil puede registrar un token de notificación push del dispositivo en el servidor utilizando el punto final Registrar dispositivo. Este token se usa para enviar notificaciones push al dispositivo del cliente.

#### Proveedor de notificaciones push

<sup>1</sup> Los proveedores de notificaciones push específicos de VOIP se utilizan para proporcionar notificaciones enriquecidas para llamadas VOIP en iOS.

| Valor                  | Descripción                                                     |
| ---------------------- | --------------------------------------------------------------- |
| gcm                    | Google Cloud Messaging (Android).                               |
| apns                   | Apple Push Notification Service (iOS).                          |
| apns\_internal         | Servicio interno de notificaciones push de Apple (iOS interno). |
| apns\_voip 1           | Servicio de notificaciones push VOIP de Apple (iOS).            |
| apns\_internal\_voip 1 | Servicio de notificaciones push VOIP de Apple (iOS interno).    |

### Endpoints <a href="#puntos-finales" id="puntos-finales"></a>

#### Registrar dispositivo

POST /users/@me/devices

Registra un token de notificaciones push GCM/APNs para el dispositivo del cliente. Devuelve una respuesta vacía 204 en caso de éxito.

#### Parámetros JSON

<sup>1</sup> Los tokens de notificación push específicos de VOIP solo se utilizan con PushKit en iOS.

| Campo                                  | Tipo    | Descripción                                                                                                                |
| -------------------------------------- | ------- | -------------------------------------------------------------------------------------------------------------------------- |
| provider                               | string  | El proveedor de notificaciones push del dispositivo.                                                                       |
| token                                  | string  | El token de notificación push que se va a registrar.                                                                       |
| voip\_provider? 1                      | string  | El proveedor de notificaciones push VOIP del dispositivo.                                                                  |
| voip\_token? 1                         | string  | El token de notificación push VOIP que se va a registrar.                                                                  |
| bypass\_server\_throttling\_supported? | boolean | Indica si el cliente soporta evitar la limitación de frecuencia del servidor para notificaciones push (por defecto false). |
| bundle\_id?                            | string  | El ID del paquete de la aplicación (por defecto com.discord).                                                              |

#### Eliminar registro de dispositivo

<kbd><mark style="color:$danger;background-color:$danger;">DELETE<mark style="color:$danger;background-color:$danger;"></kbd> /users/@me/devices

Elimina el registro de un token de notificaciones push GCM/APNs para el dispositivo del cliente. Devuelve una respuesta vacía 204 en caso de éxito.

### Parámetros JSON

| Campo    | Tipo   | Descripción                                          |
| -------- | ------ | ---------------------------------------------------- |
| provider | string | El proveedor de notificaciones push del dispositivo. |
| token    | string | El token de notificación push que se va a eliminar.  |

#### Obtener token de sincronización de dispositivo

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /users/@me/devices/sync-token

Devuelve un token de sincronización de notificaciones push para el usuario actual. Este token puede usarse para sincronizar tokens de notificaciones push entre múltiples cuentas.

### Cuerpo de la respuesta

| Campo | Tipo   | Descripción                                        |
| ----- | ------ | -------------------------------------------------- |
| token | string | El token de sincronización de notificaciones push. |

### Ejemplo de respuesta

```javascript
{ "token": "ODUyODkyMjk3NjYxOTA2OTkz.ZfoufA.rHvCtpfHjr9kdRab1ZTl83PRhhZ" }
```

#### Sincronizar dispositivos

<kbd><mark style="color:$warning;background-color:$warning;">PUT<mark style="color:$warning;background-color:$warning;"></kbd> /users/@me/devices/sync

Sincroniza el token de notificaciones push GCM/APNs del cliente entre varias cuentas.

#### Parámetros JSON

<sup>1</sup> Se puede obtener un token de sincronización de dispositivo para cada cuenta utilizando el punto final Obtener token de sincronización de dispositivo.

| Campo                | Tipo            | Descripción                                                |
| -------------------- | --------------- | ---------------------------------------------------------- |
| provider             | string          | El proveedor de notificaciones push del dispositivo.       |
| token                | string          | El token de notificación push del dispositivo.             |
| push\_sync\_tokens 1 | array \[string] | Tokens de sincronización de dispositivos para cada cuenta. |

#### Cuerpo de la respuesta

| Campo                       | Tipo            | Descripción                                                |
| --------------------------- | --------------- | ---------------------------------------------------------- |
| invalid\_push\_sync\_tokens | array \[string] | Tokens de sincronización de dispositivo que son inválidos. |

#### Ejemplo de respuesta

```javascript
{ "invalid_push_sync_tokens": ["ODUyODkyMjk3NjYxOTA2OTkz.ZfoufA.rHvCtpfHjr9kdRab1ZTl83PRhhZ"] }
```

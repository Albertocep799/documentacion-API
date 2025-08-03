---
icon: house-window
layout:
  width: default
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: false
  pagination:
    visible: true
  metadata:
    visible: false
---

# Salas

Las salas son **grupos de usuarios** que pueden comunicarse entre sí mediante texto y voz. Los usuarios pueden estar en **varias salas al mismo tiempo**. Una sala también puede tener metadatos (un _blob_ JSON arbitrario) asociados tanto a la sala como a cada usuario.

Las salas cuentan con un canal de chat de texto que todos los miembros pueden usar para comunicarse. Los mensajes se envían a todos los miembros de la sala. Las salas también admiten llamadas de voz. Aunque una sala puede tener hasta **1,000 miembros**, no es recomendable iniciar llamadas de voz en salas tan grandes (alrededor de **25 es un buen número**).

### Objeto de sala.

Una sala de juegos dentro de Discord.

**Estructura de la sala**

| Campo            | Tipo                        | Descripción                                                                     |
| ---------------- | --------------------------- | ------------------------------------------------------------------------------- |
| id               | snowflake                   | El ID de la sala.                                                               |
| application\_id  | snowflake                   | El ID de la aplicación que creó la sala.                                        |
| metadata         | ?map\[string, string]       | Los metadatos de la sala (máximo 25 claves, 1024 caracteres por clave y valor). |
| members          | array\[lobby member object] | Los miembros de la sala (máximo 1000).                                          |
| flags            | integer                     | Las flags de la sala.                                                           |
| linked\_channel? | channel object              | El canal de servidor vinculado a la sala.                                       |

### Parámetros de la sala

<table><thead><tr><th width="83">Valor</th><th width="348">Nombre</th><th>Descripción</th></tr></thead><tbody><tr><td>1 &#x3C;&#x3C; 0</td><td>REQUIRE_APPLICATION_AUTHORIZATION</td><td>Los usuarios deben autorizar la aplicación para enviar mensajes en el canal vinculado.</td></tr></tbody></table>

**Ejemplo de objeto de sala**

{% code overflow="wrap" %}
```javascript
{
  "id": "1350184413060661332",
  "application_id": "891436243903728565",
  "metadata": {
    "topic": "balls"
  },
  "members": [
    {
      "id": "852892297661906993",
      "metadata": null,
      "flags": 1
    }
  ]
}
```
{% endcode %}

### Objeto miembro de la sala

Representa a un miembro de una sala.

**Estructura de miembro de una sala**

<sup>1</sup> No incluido cuando se devuelve por Gateway.\
<sup>2</sup> Solo se incluye en objetos de lobby devueltos por Gateway.

| Campo                  | Tipo                   | Descripción                                                                              |
| ---------------------- | ---------------------- | ---------------------------------------------------------------------------------------- |
| id <sup>1</sup>        | snowflake              | El ID del usuario.                                                                       |
| metadata?              | ?map \[string, string] | Los metadatos del miembro de la sala (máx 25 claves, 1024 caracteres por clave y valor). |
| flags?                 | integer                | Banderas del miembro de la sala.                                                         |
| connected <sup>2</sup> | boolean                | Si el miembro está conectado a una llamada en la sala.                                   |

**Parámetros del miembro de la sala**

| Valor  | Nombre           | Descripción                                            |
| ------ | ---------------- | ------------------------------------------------------ |
| 1 << 0 | CAN\_LINK\_LOBBY | El miembro puede vincular un canal de texto a la sala. |

### Endpoints

**Crear sala**

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /lobbies

Crea un nuevo lobby. Devuelve un objeto de sala en caso de éxito. Dispara un evento `creación de sala` por Gateway. Los clientes no podrán unirse o salir de una sala creada usando esta API.

{% hint style="warning" %}
Este endpoint no puede ser usado por cuentas de usuario.
{% endhint %}

**Parámetros JSON**

| Campo                   | Tipo                                         | Descripción                                                                                     |
| ----------------------- | -------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| metadata?               | ?map \[string, string]                       | Los metadatos de la sala (máx 25 claves, 1024 caracteres por clave y valor).                    |
| members?                | array\[objeto parcial de miembro de la sala] | Los miembros de la sala (máx 25).                                                               |
| idle\_timeout\_seconds? | integer                                      | Cuánto esperar (en segundos) antes de cerrar una sala tras quedar inactiva (mín 5, máx 604800). |
| flags?                  | integer                                      | Las banderas de la sala.                                                                        |

**Unirse o crear una sala**

<kbd><mark style="color:$warning;background-color:$warning;">PUT<mark style="color:$warning;background-color:$warning;"></kbd> /lobbies

`OAuth2`

Se une a una sala existente o crea una nueva. Devuelve un objeto de sala en caso de éxito. Puede disparar un evento `creación de sala` o `agregar un miembro a la sala` por Gateway.

{% hint style="warning" %}
Este endpoint no puede ser usado por cuentas de usuario.
{% endhint %}

**Parámetros JSON**

<sup>1</sup> Los valores de secreto caducan tras 30 días. Después de este período la sala seguirá existiendo, pero los nuevos usuarios no podrán unirse.

| Campo                   | Tipo                   | Descripción                                                                                                     |
| ----------------------- | ---------------------- | --------------------------------------------------------------------------------------------------------------- |
| secret 1                | string                 | El secreto del lobby (máx 250 caracteres)                                                                       |
| lobby\_metadata?        | ?map \[string, string] | Los metadatos del lobby (máx 25 claves, 1024 caracteres por clave y valor)                                      |
| member\_metadata?       | ?map \[string, string] | Los metadatos del miembro del lobby (máx 25 claves, 1024 caracteres por clave y valor)                          |
| idle\_timeout\_seconds? | integer                | Cuánto esperar (en segundos) antes de cerrar un lobby tras quedar inactivo (mín 5, máx 604800, por defecto 300) |
| flags?                  | integer                | Las [banderas del lobby](https://docs.discord.food/resources/lobby#lobby-flags)                                 |

**Obtener sala**

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /lobbies/{lobby.id}

Devuelve un objeto de sala para el ID dado. El usuario debe ser miembro de la sala.

{% hint style="warning" %}
Este endpoint no puede ser usado por cuentas de usuario.
{% endhint %}

**Modificar sala**

<kbd><mark style="color:$warning;background-color:$warning;">PATCH<mark style="color:$warning;background-color:$warning;"></kbd> /lobbies/{lobby.id}

Modifica la configuración de la sala. La aplicación debe ser la creadora de la sala. Devuelve el objeto de sala actualizado en caso de éxito. Dispara un evento `actualización de la sala` por Gateway.

{% hint style="warning" %}
Este endpoint no puede ser usado por cuentas de usuario.
{% endhint %}

| Campo                   | Tipo                                 | Descripción                                                                                     |
| ----------------------- | ------------------------------------ | ----------------------------------------------------------------------------------------------- |
| metadata?               | ?map \[string, string]               | Los metadatos del lobby (máx 25 claves, 1024 caracteres por clave y valor).                     |
| members?                | array\[objeto de miembro de la sala] | Los miembros del lobby (máx 25).                                                                |
| idle\_timeout\_seconds? | integer                              | Cuánto esperar (en segundos) antes de cerrar un lobby tras quedar inactivo (mín 5, máx 604800). |
| flags?                  | integer                              | Las banderas del lobby                                                                          |

**Eliminar sala**

<kbd><mark style="color:$danger;background-color:$danger;">DELETE<mark style="color:$danger;background-color:$danger;"></kbd> /lobbies/{lobby.id}

Elimina una sala. La aplicación debe ser la creadora de la sala. Devuelve una respuesta vacía **204** en _éxito_. Dispara un evento `eliminación de sala` por Gateway.

{% hint style="warning" %}
Este endpoint no puede ser usado por cuentas de usuario.
{% endhint %}

Agregar un miembro a la sala

<kbd><mark style="color:$warning;background-color:$warning;">PUT<mark style="color:$warning;background-color:$warning;"></kbd> /lobbies/{lobby.id}/members/{user.id}

Agrega un miembro a la sala. Devuelve el objeto miembro de la sala. Dispara un evento `agregar un miembro a la sala` o `actualizar un miembro de la sala` por Gateway.

{% hint style="warning" %}
Este endpoint no puede ser usado por cuentas de usuario.
{% endhint %}

{% hint style="info" %}
Todos los parámetros para este endpoint son opcionales y pueden ser nulos. Omitir o establecer un valor `null` lo dejará por defecto. Si el usuario ya es miembro de la sala, esto lo actualizará y disparará un evento `actualizar un miembro de la sala` en vez de agregarlo.
{% endhint %}

Parámetros JSON

| Campo     | Tipo                   | Descripción                                                                              |
| --------- | ---------------------- | ---------------------------------------------------------------------------------------- |
| metadata? | ?map \[string, string] | Los metadatos del miembro de la sala (máx 25 claves, 1024 caracteres por clave y valor). |
| flags?    | integer                | Banderas del miembro de la sala.                                                         |

**Eliminar miembro de la sala**

<kbd><mark style="color:$danger;background-color:$danger;">DELETE<mark style="color:$danger;background-color:$danger;"></kbd> /lobbies/{lobby.id}/members/{user.id}

Elimina a un miembro de la sala. Devuelve una respuesta vacía **204** en éxito. Dispara un evento `eliminar miembro de la sala` por Gateway.

{% hint style="warning" %}
Este endpoint no puede ser usado por cuentas de usuario.
{% endhint %}

**Abandonar la sala**

<kbd><mark style="color:$danger;background-color:$danger;">DELETE<mark style="color:$danger;background-color:$danger;"></kbd> /lobbies/{lobby.id}/members/@me

`OAuth2`

Elimina al usuario actual de la sala. Devuelve una respuesta vacía **204** en éxito. Dispara un evento `eliminar sala` y `eliminar miembro de la sala` por Gateway.

{% hint style="warning" %}
Este endpoint solo puede ser usado con un token **OAuth2** con el alcance `lobbies.write`.
{% endhint %}

**Actualización masiva de miembros de la sala**

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /lobbies/{lobby.id}/members/bulk

Modifica múltiples miembros de una sala en una sola solicitud. Acepta una lista de 1 a 25 objetos de actualización de miembros de la sala. Devuelve una lista de objetos miembros de la sala en caso de éxito. Puede disparar varios eventos `agregar un miembro a la sala` y/o `eliminar miembro de la sala` por Gateway.

{% hint style="warning" %}
Este endpoint no puede ser usado por cuentas de usuario.
{% endhint %}

{% hint style="info" %}
Todos los campos en objetos de miembro de la sala enviados a este endpoint son opcionales y pueden ser nulos. Omitir o establecer a `null` lo dejará por defecto (excepto `remove_member`). Si el usuario ya es miembro de la sala, esto lo actualizará y disparará un evento `actualizar un miembro de la sala` por Gateway en lugar de agregarlo.
{% endhint %}

**Estructura de actualización de miembro de la sala**

| Campo           | Tipo                   | Descripción                                                                               |
| --------------- | ---------------------- | ----------------------------------------------------------------------------------------- |
| id              | snowflake              | El ID del miembro de la sala.                                                             |
| metadata?       | ?map \[string, string] | Los metadatos del miembro del la sala (máx 25 claves, 1024 caracteres por clave y valor). |
| flags?          | integer                | Banderas del miembro de la sala.                                                          |
| remove\_member? | boolean                | Si se elimina el miembro de la sala.                                                      |

**Crear invitación a la sala para el usuario actual**

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /lobbies/{lobby.id}/members/@me/invites

`OAuth2`

Crea una invitación para el canal vinculado a la sala que solo puede aceptar el usuario actual. Dispara un evento `creación de invitación` por Gateway.

{% hint style="warning" %}
Este endpoint solo puede ser usado con un token **OAuth2** con el alcance `lobbies.write`.
{% endhint %}

**Cuerpo de respuesta**

| Campo | Tipo   | Descripción              |
| ----- | ------ | ------------------------ |
| code  | string | Código de la invitación. |

**Crear invitación a la sala**

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /lobbies/{lobby.id}/members/{user.id}/invites

Crea una invitación para el canal vinculado a la sala que solo puede aceptar el usuario especificado. Dispara un evento `creación de invitación` por Gateway.

{% hint style="warning" %}
Este endpoint solo puede ser usado con un token **OAuth2** con el alcance `lobbies.write`.
{% endhint %}

**Cuerpo de Respuesta**

| Campo | Tipo   | Descripción              |
| ----- | ------ | ------------------------ |
| code  | string | Código de la invitación. |

**Modificar canal vinculado a la sala**

<kbd><mark style="color:$warning;background-color:$warning;">PATCH<mark style="color:$warning;background-color:$warning;"></kbd> /lobbies/{lobby.id}/channel-linking

`OAuth2`

Vincula o desvincula un canal a la sala. Devuelve un objeto de sala en caso de éxito. Dispara un evento `actualización de la sala` y `actualización del canal` por Gateway.

La aplicación debe ser la creadora de la sala o el miembro debe tener la bandera `CAN_LINK_LOBBY`. Requiere el permiso `MANAGE_CHANNELS` en el canal objetivo.

{% hint style="warning" %}
Este endpoint solo puede ser usado con un token **OAuth2** con el alcance `lobbies.write`.
{% endhint %}

**Parámetros JSON**

| Campo        | Tipo       | Descripción                        |
| ------------ | ---------- | ---------------------------------- |
| channel\_id? | ?snowflake | ID del canal a vincular a la sala. |

**Obtener mensajes de la sala**

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /lobbies/{lobby.id}/messages

`OAuth2`

Devuelve una lista de objetos de mensaje parcial de la sala en orden inverso cronológico.

{% hint style="warning" %}
Este endpoint solo puede ser usado con un token **OAuth2** con el alcance `lobbies.write`.
{% endhint %}

**Parámetros de consulta**

| Campo  | Tipo    | Descripción                                            |
| ------ | ------- | ------------------------------------------------------ |
| limit? | integer | Máximo de mensajes a devolver (1-200, por defecto 50). |

**Crear mensaje en la sala**

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /lobbies/{lobby.id}/messages

`OAuth2`

Publica un mensaje en una sala. Devuelve un objeto de mensaje parcial en caso de éxito. Dispara los eventos `creación de mensaje en una sala` y `creación de mensaje` por Gateway. Funcionalmente idéntico a `crear mensaje`, pero para salas en un contexto **OAuth2** y con parámetros adicionales.

{% hint style="warning" %}
Este endpoint solo puede ser usado con un token **OAuth2** con el alcance `lobbies.write`.
{% endhint %}

**Parámetros del JSON Extra**

| Campo     | Tipo   | Descripción                                                                                  |
| --------- | ------ | -------------------------------------------------------------------------------------------- |
| metadata? | object | Metadatos personalizados para el mensaje (máx 25 claves, 1024 caracteres por clave y valor). |

---
icon: square-terminal
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

# Comandos de aplicación

## Comandos de aplicación <a href="#comandos-de-aplicacin" id="comandos-de-aplicacin"></a>

Los comandos de aplicación son comandos que una aplicación puede registrar en Discord. Proporcionan a los usuarios una forma de primera clase de interactuar directamente con tu aplicación que se siente profundamente integrada en Discord.

#### Objeto de comando de aplicación <a href="#objeto-de-comando-de-aplicacin" id="objeto-de-comando-de-aplicacin"></a>

{% hint style="info" %}
Los nombres de los comandos **CHAT\_INPUT** deben estar en minúsculas y cumplir con la expresión regular `^[\w-]{1,32}$`. Los comandos **USER** y **MESSAGE** pueden tener mayúsculas y minúsculas mezcladas y pueden incluir espacios.
{% endhint %}

#### Estructura de comando de aplicación

| Campo                | Tipo                                | Descripción                                                                                                | Tipos válidos |
| -------------------- | ----------------------------------- | ---------------------------------------------------------------------------------------------------------- | ------------- |
| id                   | snowflake                           | ID único del comando.                                                                                      | all           |
| type?                | one of application command type     | El tipo de comando, por defecto `1` si no se establece.                                                    | all           |
| application\_id      | snowflake                           | ID único de la aplicación padre.                                                                           | all           |
| guild\_id?           | snowflake                           | ID del servidor del comando, si no es global.                                                              | all           |
| name                 | string                              | Nombre de 1-32 caracteres.                                                                                 | all           |
| description          | string                              | Descripción de 1-100 caracteres para comandos `CHAT_INPUT`, cadena vacía para comandos `USER` y `MESSAGE`. | all           |
| options?             | array of application command option | Los parámetros para el comando, máximo 25.                                                                 | CHAT\_INPUT   |
| default\_permission? | boolean (default `true`)            | Si el comando está habilitado por defecto cuando la aplicación se añade a un servidor.                     | all           |

#### Tipos de comandos de aplicación

| Nombre      | Tipo | Descripción                                                                                |
| ----------- | ---- | ------------------------------------------------------------------------------------------ |
| CHAT\_INPUT | 1    | Comandos de barra; un comando basado en texto que aparece cuando un usuario escribe `/`.   |
| USER        | 2    | Un comando basado en interfaz que aparece cuando haces clic derecho o tocas en un usuario. |
| MESSAGE     | 3    | Un comando basado en interfaz que aparece cuando haces clic derecho o tocas en un mensaje. |

#### Estructura de opción de comando de aplicación

{% hint style="info" %}
Las `opciones` obligatorias deben aparecer antes que las opciones opcionales.
{% endhint %}

| Campo       | Tipo                                       | Descripción                                                                                                 |
| ----------- | ------------------------------------------ | ----------------------------------------------------------------------------------------------------------- |
| type        | one of application command option type     | El tipo de opción.                                                                                          |
| name        | string                                     | Nombre de 1-32 caracteres en minúsculas que coincida con `^[\w-]{1,32}$`.                                   |
| description | string                                     | Descripción de 1-100 caracteres.                                                                            |
| required?   | boolean                                    | Si el parámetro es requerido u opcional, por defecto `false`.                                               |
| choices?    | array of application command option choice | Opciones para tipos `STRING`, `INTEGER`, y `NUMBER` para que el usuario elija, máximo 25.                   |
| options?    | array of application command option        | Si la opción es un tipo de subcomando o grupo de subcomandos, estas opciones anidadas serán los parámetros. |

#### Tipo de opción de comando de aplicación

| Nombre              | Valor | Nota                                             |
| ------------------- | ----- | ------------------------------------------------ |
| SUB\_COMMAND        | 1     |                                                  |
| SUB\_COMMAND\_GROUP | 2     |                                                  |
| STRING              | 3     |                                                  |
| INTEGER             | 4     | Cualquier entero entre -2^53 y 2^53.             |
| BOOLEAN             | 5     |                                                  |
| USER                | 6     |                                                  |
| CHANNEL             | 7     | Incluye todos los tipos de canales + categorías. |
| ROLE                | 8     |                                                  |
| MENTIONABLE         | 9     | Incluye usuarios y roles.                        |
| NUMBER              | 10    | Cualquier doble entre -2^53 y 2^53.              |

#### Estructura de opción de elección de comando de aplicación

Si especificas `choices` para una opción, son los **únicos** valores válidos para que un usuario elija.

| Campo | Tipo                       | Descripción                                              |
| ----- | -------------------------- | -------------------------------------------------------- |
| name  | string                     | Nombre de elección de 1-100 caracteres.                  |
| value | string, integer, or double | Valor de la elección, hasta 100 caracteres si es string. |

#### Estructura de opción de datos de interacción de comando de aplicación

Todas las opciones tienen nombres, y una opción puede ser un parámetro y valor de entrada, en cuyo caso `value` será establecido, o puede denotar un subcomando o grupo, en cuyo caso contendrá una clave de nivel superior y otro array de `options`.

`value` y `options` son mutuamente exclusivos.

| Campo    | Tipo                                                 | Descripción                                        |
| -------- | ---------------------------------------------------- | -------------------------------------------------- |
| name     | string                                               | El nombre del parámetro.                           |
| type     | integer                                              | Valor del tipo de opción de comando de aplicación. |
| value?   | application command option type                      | El valor del par.                                  |
| options? | array of application command interaction data option | Presente si esta opción es un grupo o subcomando.  |

### Autorizando tu aplicación <a href="#autorizando-tu-aplicacin" id="autorizando-tu-aplicacin"></a>

Los comandos de aplicación no dependen de un usuario bot en el servidor; usan el modelo de interacciones. Para crear comandos en un servidor, tu aplicación debe estar autorizada con el ámbito `applications.commands`.

{% hint style="danger" %}
Para que los comandos funcionen dentro de una guild, la guild debe autorizar tu aplicación con el _scope_ `applications.commands`. El _scope_ `bot` no es suficiente.
{% endhint %}

Al solicitar este ámbito, "atajamos" el flujo OAuth2 similar a añadir un bot. No necesitas completar el flujo, intercambiar por un token, ni nada de eso.

Si tu aplicación no requiere un usuario bot dentro del servidor para que sus comandos funcionen, **ya no necesitas añadir el ámbito bot o permisos específicos**.

### Registrando un comando <a href="#registrando-un-comando" id="registrando-un-comando"></a>

{% hint style="info" %}
Los comandos solo pueden registrarse a través del endpoint HTTP.
{% endhint %}

Los comandos pueden tener ámbito global o para un servidor específico. Los comandos globales están disponibles para cada servidor que añada tu aplicación. Los comandos globales de una aplicación individual también están disponibles en MD si esa aplicación tiene un bot que comparte un servidor mutuo con el usuario.

Los comandos de servidor son específicos del servidor que especifiques al crearlos. Los comandos de servidor no están disponibles en MD. Los nombres de comandos son únicos por aplicación, por tipo, dentro de cada ámbito (global y servidor). Eso significa:

* Tu aplicación **no puede** tener dos comandos `CHAT_INPUT` globales con el mismo nombre
* Tu aplicación **no puede** tener dos comandos `CHAT_INPUT` de servidor con el mismo nombre **en el mismo servidor**
* Tu aplicación **no puede** tener dos comandos `USER` globales con el mismo nombre
* Tu aplicación **puede** tener un comando `CHAT_INPUT` global y de servidor con el mismo nombre
* Tu aplicación **puede** tener un comando `CHAT_INPUT` y `USER` global con el mismo nombre
* Múltiples aplicaciones **pueden** tener comandos con los mismos nombres

Esta lista no es exhaustiva. En general, recuerda que los nombres de comandos deben ser únicos por aplicación, por tipo, y dentro de cada ámbito (global y servidor).

Una aplicación puede tener el siguiente número de comandos:

* 100 comandos `CHAT_INPUT` globales
* 5 comandos `USER` globales
* 5 comandos `MESSAGE` globales

Así como la misma cantidad de comandos específicos de servidor por servidor.

{% hint style="danger" %}
Existe un límite global de 200 creaciones de comandos de aplicación por día, por servidor.
{% endhint %}

### Creando un comando global

Los comandos globales están disponibles en _todos_ los servidores de tu aplicación. Los comandos globales se almacenan en caché durante **1 hora**. Eso significa que los nuevos comandos globales se extenderán lentamente por todos los servidores, y se garantiza que se actualicen en una hora.

Los comandos globales tienen funcionalidad de reparación de lectura inherente. Eso significa que si haces una actualización a un comando global, y un usuario trata de usar ese comando antes de que se haya actualizado para ellos, Discord hará una verificación de versión interna y rechazará el comando, y activará una recarga para ese comando.

Para hacer un comando **global**, haz una llamada HTTP POST así:

```python
import requests


url = "https://discord.com/api/v8/applications/<my_application_id>/commands"

# This is an example CHAT_INPUT or Slash Command, with a type of 1
json = {
    "name": "blep",
    "type": 1,
    "description": "Send a random adorable animal photo",
    "options": [
        {
            "name": "animal",
            "description": "The type of animal",
            "type": 3,
            "required": True,
            "choices": [
                {
                    "name": "Dog",
                    "value": "animal_dog"
                },
                {
                    "name": "Cat",
                    "value": "animal_cat"
                },
                {
                    "name": "Penguin",
                    "value": "animal_penguin"
                }
            ]
        },
        {
            "name": "only_smol",
            "description": "Whether to show only baby animals",
            "type": 5,
            "required": False
        }
    ]
}

# For authorization, you can use either your bot token
headers = {
    "Authorization": "Bot <my_bot_token>"
}

# or a client credentials token for your app with the applications.commands.update scope
headers = {
    "Authorization": "Bearer <my_credentials_token>"
}

r = requests.post(url, headers=headers, json=json)
```

### Creando un comando de servidor

Los comandos de servidor están disponibles solo dentro del servidor especificado en la creación. Los comandos de servidor se actualizan **instantáneamente**. Recomendamos que uses comandos de servidor para pruebas rápidas, y comandos globales cuando estén listos para uso público.

Para hacer un comando de **servidor**, haz una llamada HTTP POST similar, pero limítalo a un `guild_id` específico:

```python
import requests


url = "https://discord.com/api/v8/applications/<my_application_id>/guilds/<guild_id>/commands"

# This is an example USER command, with a type of 2
json = {
    "name": "High Five",
    "type": 2
}

# For authorization, you can use either your bot token
headers = {
    "Authorization": "Bot <my_bot_token>"
}

# or a client credentials token for your app with the applications.commands.update scope
headers = {
    "Authorization": "Bearer <my_credentials_token>"
}

r = requests.post(url, headers=headers, json=json)
```

### Actualizando y eliminando un comando <a href="#actualizando-y-eliminando-un-comando" id="actualizando-y-eliminando-un-comando"></a>

Los comandos pueden ser eliminados y actualizados haciendo llamadas `DELETE` y `PATCH` al endpoint del comando. Esos endpoints son

* &#x20;`applications/<my_application_id>/commands/<command_id>` para comandos globales, o
* `applications/<my_application_id>/guilds/<guild_id>/commands/<command_id>` para comandos de servidor

Porque los comandos tienen nombres únicos dentro de un tipo y ámbito, tratamos las solicitudes `POST` para nuevos comandos como upserts. Eso significa **hacer un nuevo comando con un nombre ya usado para tu aplicación actualizará el comando existente**.

La documentación completa de endpoints se puede encontrar aquí.

### Permisos <a href="#permisos" id="permisos"></a>

### Objeto de permisos de comando de aplicación

#### Estructura de permisos de comando de aplicación de servidor

Devuelto al obtener los permisos para un comando en un servidor.

| Campo           | Tipo                                     | Descripción                                           |
| --------------- | ---------------------------------------- | ----------------------------------------------------- |
| id              | snowflake                                | El ID del comando.                                    |
| application\_id | snowflake                                | El ID de la aplicación a la que pertenece el comando. |
| guild\_id       | snowflake                                | El ID del servidor.                                   |
| permissions     | array of application command permissions | Los permisos para el comando en el servidor.          |

#### Estructura de permisos de comando de aplicación

Los permisos de comando de aplicación te permiten habilitar o deshabilitar comandos para usuarios o roles específicos dentro de un servidor.

| Campo      | Tipo                                | Descripción                                     |
| ---------- | ----------------------------------- | ----------------------------------------------- |
| id         | snowflake                           | El ID del rol o usuario.                        |
| type       | application command permission type | Rol o usuario.                                  |
| permission | boolean                             | `true` para permitir, `false` para no permitir. |

#### Tipo de permiso de comando de aplicación

| Nombre | Valor |
| ------ | ----- |
| ROLE   | 1     |
| USER   | 2     |

¿Necesitas mantener algunos de tus comandos a salvo de ojos curiosos, o solo disponibles para las personas correctas? ¡Los comandos soportan sobreescrituras de permisos! Para comandos globales _y_ de servidor de todos los tipos, puedes habilitar o deshabilitar a un usuario o rol específico en un servidor de usar un comando.

{% hint style="info" %}
Por ahora, si no tienes permiso para usar un comando, aparecerá en el selector de comandos como deshabilitado e inutilizable. No se ocultarán.
{% endhint %}

También puedes establecer un `default_permission` en tus comandos si quieres que estén deshabilitados por defecto cuando tu aplicación se añada a un nuevo servidor. Establecer `default_permission` a `false` no permitirá a _nadie_ en un servidor usar el comando, incluso Administradores y propietarios de servidor, a menos que se configure una sobreescritura específica. También deshabilitará el comando de ser usable en MD.

Por ejemplo, este comando no será usable por nadie en ningún servidor por defecto:

```python
{
  "name": "permissions_test",
  "description": "A test of default permissions",
  "type": 1,
  "default_permission": false
}
```

Para habilitarlo solo para un rol de moderador:

```python
MODERATOR_ROLE_ID = "<moderator_role_id>"
url = "https://discord.com/api/v8/applications/<my_application_id>/guilds/<my_guild_id>/commands/<my_command_id>/permissions"

json = {
    "permissions": [
        {
            "id": MODERATOR_ROLE_ID,
            "type": 1,
            "permission": True
        }
    ]
}

headers = {
    "Authorization": "Bot <my_bot_token>"
}

r = requests.put(url, headers=headers, json=json)
```

### Comandos de barra <a href="#comandos-de-barra" id="comandos-de-barra"></a>

Los comandos de barra, el tipo `CHAT_INPUT`, son un tipo de comando de aplicación. Están compuestos de un nombre, descripción, y un bloque de `options`, que puedes pensar como argumentos a una función. El nombre y descripción ayudan a los usuarios a encontrar tu comando entre muchos otros, y las `options` validan la entrada del usuario mientras llenan tu comando.

Los comandos de barra también pueden tener grupos y subcomandos para organizar mejor los comandos. Más sobre eso después.

{% hint style="info" %}
Los comandos slash pueden tener un máximo de 4000 caracteres combinados entre las propiedades de nombre, descripción y valor para cada comando, sus subcomandos y grupos.
{% endhint %}

#### Ejemplo de comando de barra

```python
{
  "name": "blep",
  "type": 1,
  "description": "Send a random adorable animal photo",
  "options": [
    {
      "name": "animal",
      "description": "The type of animal",
      "type": 3,
      "required": true,
      "choices": [
        {
          "name": "Dog",
          "value": "animal_dog"
        },
        {
          "name": "Cat",
          "value": "animal_cat"
        },
        {
          "name": "Penguin",
          "value": "animal_penguin"
        }
      ]
    },
    {
      "name": "only_smol",
      "description": "Whether to show only baby animals",
      "type": 5,
      "required": false
    }
  ]
}
```

Cuando alguien usa un comando de barra, tu aplicación recibirá una interacción:

#### Ejemplo de interacción de comando de barra

```python
{
  "type": 2,
  "token": "A_UNIQUE_TOKEN",
  "member": {
    "user": {
      "id": "53908232506183680",
      "username": "mason",
      "global_name": "Mason",
      "avatar": "a_d5efa99b3eeaa7dd43acca82f5692432",
      "discriminator": "0",
      "public_flags": 131141,
      "avatar_decoration_data": null,
      "primary_guild": null
    },
    "roles": ["539082325061836999"],
    "premium_since": null,
    "permissions": "2147483647",
    "pending": false,
    "nick": null,
    "mute": false,
    "joined_at": "2017-03-13T19:19:14.040000+00:00",
    "deaf": false
  },
  "id": "786008729715212338",
  "guild_id": "290926798626357999",
  "data": {
    "options": [
      {
        "name": "cardname",
        "value": "The Gitrog Monster"
      }
    ],
    "name": "cardsearch",
    "id": "771825006014889984"
  },
  "channel_id": "645027906669510667"
}
```

### Subcomandos y grupos de subcomandos <a href="#subcomandos-y-grupos-de-subcomandos" id="subcomandos-y-grupos-de-subcomandos"></a>

{% hint style="warning" %}
Actualmente, los subcomandos y grupos de subcomandos aparecen todos en el nivel superior en el explorador de comandos. Esto podría cambiar en el futuro para incluirlos como opciones anidadas de autocompletado.
{% endhint %}

Para aquellos desarrolladores que buscan hacer grupos de comandos más organizados y complejos, no busquen más allá de subcomandos y grupos.

**Los subcomandos** organizan tus comandos **especificando acciones dentro de un comando o grupo**.

**Los grupos de subcomandos** organizan tus **subcomandos** **agrupando subcomandos por acción similar o recurso dentro de un comando**.

Estas no son reglas obligatorias. Eres libre de usar subcomandos y grupos como quieras; es solo cómo pensamos sobre ellos.

{% hint style="danger" %}
Usar subcomandos o grupos de subcomandos hará que tu comando base sea inutilizable. No puedes enviar el comando base `/permissions` como un comando válido si también tienes `/permissions add` o `/permissions remove` como subcomandos o grupos de subcomandos.
{% endhint %}

Soportamos anidamiento de un nivel de profundidad dentro de un grupo, lo que significa que tu comando de nivel superior puede contener grupos de subcomandos, y esos grupos pueden contener subcomandos. **Ese es el único tipo de anidamiento soportado.** Aquí hay algunos ejemplos visuales:

```
VALID

  command
  |
  |__ subcommand
  |
  |__ subcommand

  #######

  command
  |
  |__ subcommand-group
      |
      |__ subcommand
  |
  |__ subcommand-group
      |
      |__ subcommand

#######

INVALID

  command
  |
  |__ subcommand-group
      |
      |__ subcommand-group
  |
  |__ subcommand-group
      |
      |__ subcommand-group

  #######

  command
  |
  |__ subcommand
      |
      |__ subcommand-group
  |
  |__ subcommand
      |
      |__ subcommand-group
```

### Ejemplo de tutorial

Veamos un ejemplo. Imaginemos que manejas un bot de moderación. Quieres hacer un comando `/permissions` que pueda hacer lo siguiente:

* Obtener los permisos del servidor para un usuario o un rol
* Obtener los permisos para un usuario o un rol en un canal específico
* Cambiar los permisos del servidor para un usuario o un rol
* Cambiar los permisos para un usuario o un rol en un canal específico

Comenzaremos definiendo la información de nivel superior para `/permissions`:

```python
{
    "name": "permissions",
    "description": "Get or edit permissions for a user or a role",
    "options": []
}
```

Ahora tenemos un comando llamado `permissions`. Queremos que este comando pueda afectar usuarios y roles. En lugar de hacer dos comandos separados, podemos usar grupos de subcomandos. Queremos usar grupos de subcomandos aquí porque estamos agrupando comandos en un recurso similar: `user` o `role`.

```python
{
    "name": "permissions",
    "description": "Get or edit permissions for a user or a role",
    "options": [
        {
            "name": "user",
            "description": "Get or edit permissions for a user",
            "type": 2 // 2 is type SUB_COMMAND_GROUP
        },
        {
            "name": "role",
            "description": "Get or edit permissions for a role",
            "type": 2
        }
    ]
}
```

Notarás que un comando como este **no aparecerá** en el explorador de comandos. Eso es porque los grupos son efectivamente "carpetas" para comandos, y hemos hecho dos carpetas vacías. Así que continuemos.

Ahora que hemos hecho efectivamente carpetas `user` y `role`, queremos poder tanto `get` como `edit` permisos. Dentro de los grupos de subcomandos, podemos hacer subcomandos para `get` y `edit`:

```python
{
    "name": "permissions",
    "description": "Get or edit permissions for a user or a role",
    "options": [
        {
            "name": "user",
            "description": "Get or edit permissions for a user",
            "type": 2, // 2 is type SUB_COMMAND_GROUP
            "options": [
                {
                    "name": "get",
                    "description": "Get permissions for a user",
                    "type": 1 // 1 is type SUB_COMMAND
                },
                {
                    "name": "edit",
                    "description": "Edit permissions for a user",
                    "type": 1
                }
            ]
        },
        {
            "name": "role",
            "description": "Get or edit permissions for a role",
            "type": 2,
            "options": [
                {
                    "name": "get",
                    "description": "Get permissions for a role",
                    "type": 1
                },
                {
                    "name": "edit",
                    "description": "Edit permissions for a role",
                    "type": 1
                }
            ]
        }
    ]
}
```

Ahora, ¡necesitamos algunos argumentos! Si elegimos `user`, necesitamos poder elegir un usuario; si elegimos `role`, necesitamos poder elegir un rol. También queremos poder elegir entre permisos de nivel de servidor y permisos específicos de canal. Para eso, podemos usar argumentos opcionales:

```python
{
    "name": "permissions",
    "description": "Get or edit permissions for a user or a role",
    "options": [
        {
            "name": "user",
            "description": "Get or edit permissions for a user",
            "type": 2, // 2 is type SUB_COMMAND_GROUP
            "options": [
                {
                    "name": "get",
                    "description": "Get permissions for a user",
                    "type": 1, // 1 is type SUB_COMMAND
                    "options": [
                        {
                            "name": "user",
                            "description": "The user to get",
                            "type": 6, // 6 is type USER
                            "required": true
                        },
                        {
                            "name": "channel",
                            "description": "The channel permissions to get. If omitted, the guild permissions will be returned",
                            "type": 7, // 7 is type CHANNEL
                            "required": false
                        }
                    ]
                },
                {
                    "name": "edit",
                    "description": "Edit permissions for a user",
                    "type": 1,
                    "options": [
                        {
                            "name": "user",
                            "description": "The user to edit",
                            "type": 6,
                            "required": true
                        },
                        {
                            "name": "channel",
                            "description": "The channel permissions to edit. If omitted, the guild permissions will be edited",
                            "type": 7,
                            "required": false
                        }
                    ]
                }
            ]
        },
        {
            "name": "role",
            "description": "Get or edit permissions for a role",
            "type": 2,
            "options": [
                {
                    "name": "get",
                    "description": "Get permissions for a role",
                    "type": 1,
                    "options": [
                        {
                            "name": "role",
                            "description": "The role to get",
                            "type": 8, // 8 is type ROLE
                            "required": true
                        },
                        {
                            "name": "channel",
                            "description": "The channel permissions to get. If omitted, the guild permissions will be returned",
                            "type": 7,
                            "required": false
                        }
                    ]
                },
                {
                    "name": "edit",
                    "description": "Edit permissions for a role",
                    "type": 1,
                    "options": [
                        {
                            "name": "role",
                            "description": "The role to edit",
                            "type": 8,
                            "required": true
                        },
                        {
                            "name": "channel",
                            "description": "The channel permissions to edit. If omitted, the guild permissions will be edited",
                            "type": 7,
                            "required": false
                        }
                    ]
                }
            ]
        }
    ]
}
```

¡Y listo! El JSON se ve un poco complicado, pero lo que hemos terminado es un solo comando que puede tener ámbito a múltiples acciones, y luego ámbito adicional a un recurso particular, y luego ámbito _adicional_ con argumentos opcionales. Aquí se ve todo junto.

### Comandos de usuario <a href="#comandos-de-usuario" id="comandos-de-usuario"></a>

Los comandos de usuario son comandos de aplicación que aparecen en el menú contextual (clic derecho o toque) de usuarios. Son una gran manera de mostrar acciones rápidas para tu aplicación que se dirigen a usuarios. No toman argumentos, y devolverán el usuario en quien hiciste clic o tocaste en la respuesta de interacción.

{% hint style="danger" %}
El campo de `descripción` no está permitido al crear comandos de usuario. Sin embargo, para evitar cambios disruptivos en los modelos de datos, la `descripción` será una **cadena vacía** (en lugar de `null`) al obtener los comandos.
{% endhint %}

#### Ejemplo de comando de usuario

```python
{
  "name": "High Five",
  "type": 2
}
```

Cuando alguien usa un comando de usuario, tu aplicación recibirá una interacción:

#### Ejemplo de interacción de comando de usuario

```python
{
  "application_id": "775799577604522054",
  "channel_id": "772908445358620702",
  "data": {
    "id": "866818195033292850",
    "name": "context-menu-user-2",
    "resolved": {
      "members": {
        "809850198683418695": {
          "avatar": null,
          "joined_at": "2021-02-12T18:25:07.972000+00:00",
          "nick": null,
          "pending": false,
          "permissions": "246997699136",
          "premium_since": null,
          "roles": []
        }
      },
      "users": {
        "809850198683418695": {
          "id": "809850198683418695",
          "username": "voltydemo",
          "global_name": "VoltyDemo",
          "avatar": "afc428077119df8aabbbd84b0dc90c74",
          "discriminator": "0",
          "public_flags": 0,
          "bot": true,
          "avatar_decoration_data": null,
          "primary_guild": null
        }
      }
    },
    "target_id": "809850198683418695",
    "type": 2
  },
  "guild_id": "772904309264089089",
  "id": "867794291820986368",
  "member": {
    "avatar": null,
    "deaf": false,
    "joined_at": "2020-11-02T20:46:57.364000+00:00",
    "mute": false,
    "nick": null,
    "pending": false,
    "permissions": "274877906943",
    "premium_since": null,
    "roles": ["785609923542777878"],
    "user": {
      "id": "167348773423415296",
      "username": "ian",
      "global_name": "Ian",
      "avatar": "a_f03401914fb4f3caa9037578ab980920",
      "discriminator": "0",
      "public_flags": 131141,
      "avatar_decoration_data": null,
      "primary_guild": null
    }
  },
  "token": "UNIQUE_TOKEN",
  "type": 2,
  "version": 1
}
```

### Comandos de mensaje <a href="#comandos-de-mensaje" id="comandos-de-mensaje"></a>

Los comandos de mensaje son comandos de aplicación que aparecen en el menú contextual (clic derecho o toque) de mensajes. Son una gran manera de mostrar acciones rápidas para tu aplicación que se dirigen a mensajes. No toman argumentos, y devolverán el mensaje en el cual hiciste clic o tocaste en la respuesta de interacción.

{% hint style="danger" %}
El campo de `descripción` no está permitido al crear comandos de usuario. Sin embargo, para evitar cambios disruptivos en los modelos de datos, la `descripción` será una **cadena vacía** (en lugar de `null`) al obtener los comandos.
{% endhint %}

#### Ejemplo de comando de mensaje

```python
{
  "name": "Bookmark",
  "type": 3
}
```

Cuando alguien usa un comando de mensaje, tu aplicación recibirá una interacción:

#### Ejemplo de interacción de comando de mensaje

```python
{
  "application_id": "775799577604522054",
  "channel_id": "772908445358620702",
  "data": {
    "id": "866818195033292851",
    "name": "context-menu-message-2",
    "resolved": {
      "messages": {
        "867793854505943041": {
          "attachments": [],
          "author": {
            "id": "167348773423415296",
            "username": "ian",
            "global_name": "Ian",
            "avatar": "a_f03401914fb4f3caa9037578ab980920",
            "discriminator": "0",
            "public_flags": 131141,
            "avatar_decoration_data": null,
            "primary_guild": null
          },
          "channel_id": "772908445358620702",
          "components": [],
          "content": "some message",
          "edited_timestamp": null,
          "embeds": [],
          "flags": 0,
          "id": "867793854505943041",
          "mention_everyone": false,
          "mention_roles": [],
          "mentions": [],
          "pinned": false,
          "timestamp": "2021-07-22T15:42:57.744000+00:00",
          "tts": false,
          "type": 0
        }
      }
    },
    "target_id": "867793854505943041",
    "type": 3
  },
  "guild_id": "772904309264089089",
  "id": "867793873336926249",
  "member": {
    "avatar": null,
    "deaf": false,
    "joined_at": "2020-11-02T20:46:57.364000+00:00",
    "mute": false,
    "nick": null,
    "pending": false,
    "permissions": "274877906943",
    "premium_since": null,
    "roles": ["785609923542777878"],
    "user": {
      "id": "167348773423415296",
      "username": "ian",
      "global_name": "Ian",
      "avatar": "a_f03401914fb4f3caa9037578ab980920",
      "discriminator": "0",
      "public_flags": 131141,
      "avatar_decoration_data": null,
      "primary_guild": null
    }
  },
  "token": "UNIQUE_TOKEN",
  "type": 2,
  "version": 1
}
```

### Endpoints <a href="#endpoints" id="endpoints"></a>

{% hint style="info" %}
Para la autorización, todos los endpoints aceptan un token de bot o un token de credenciales de cliente para tu aplicación.
{% endhint %}

#### Obtener comandos de aplicación globales

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /applications/{application.id}/commands

Obtiene todos los comandos globales para tu aplicación. Devuelve un array de objetos de comando de aplicación.

#### Crear comando de aplicación global

<mark style="color:$success;background-color:$success;">POST</mark> /applications/{application.id}/commands

{% hint style="danger" %}
Crear un comando con el mismo nombre que un comando existente para tu aplicación sobrescribirá el comando antiguo.
{% endhint %}

Crea un nuevo comando global. Los nuevos comandos globales estarán disponibles en todos los servidores después de 1 hora. Devuelve `201` y un objeto de comando de aplicación.

#### Parámetros JSON

| Campo                | Tipo                                | Descripción                                                                            |
| -------------------- | ----------------------------------- | -------------------------------------------------------------------------------------- |
| name                 | string                              | Nombre de 1-32 caracteres en minúsculas que coincida con `^[\w-]{1,32}$`.              |
| description          | string                              | Descripción de 1-100 caracteres.                                                       |
| options?             | array of application command option | Los parámetros para el comando.                                                        |
| default\_permission? | boolean (default `true`)            | Si el comando está habilitado por defecto cuando la aplicación se añade a un servidor. |
| type?                | one of application command type     | El tipo de comando, por defecto `1` si no se establece.                                |

#### Obtener comando de aplicación global

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /applications/{application.id}/commands/{command.id}

Obtiene un comando global para tu aplicación. Devuelve un objeto de comando de aplicación.

#### Editar comando de aplicación global

<kbd><mark style="color:$warning;background-color:$warning;">PATCH<mark style="color:$warning;background-color:$warning;"></kbd> /applications/{application.id}/commands/{command.id}

Edita un comando global. Las actualizaciones estarán disponibles en todos los servidores después de 1 hora. Devuelve `200` y un objeto de comando de aplicación.

#### Parámetros JSON

| Campo                | Tipo                                | Descripción                                                                            |
| -------------------- | ----------------------------------- | -------------------------------------------------------------------------------------- |
| name?                | string                              | Nombre de 1-32 caracteres en minúsculas que coincida con `^[\w-]{1,32}$`.              |
| description?         | string                              | Descripción de 1-100 caracteres.                                                       |
| options?             | array of application command option | Los parámetros para el comando.                                                        |
| default\_permission? | boolean (default `true`)            | Si el comando está habilitado por defecto cuando la aplicación se añade a un servidor. |

#### Eliminar comando de aplicación global

<kbd><mark style="color:$danger;background-color:$danger;">DELETE<mark style="color:$danger;background-color:$danger;"></kbd> /applications/{application.id}/commands/{command.id}

Elimina un comando global. Devuelve `204`.

### Sobreescribir en masa comandos de aplicación globales

<kbd><mark style="color:$warning;background-color:$warning;">PUT<mark style="color:$warning;background-color:$warning;"></kbd> /applications/{application.id}/commands

Toma una lista de comandos de aplicación, sobreescribiendo comandos existentes que están registrados globalmente para esta aplicación. Las actualizaciones estarán disponibles en todos los servidores después de 1 hora. Devuelve `200` y una lista de objetos de comando de aplicación. Los comandos que no existen ya contarán hacia los límites diarios de creación de comandos de aplicación.

{% hint style="danger" %}
Esto sobrescribirá **todos** los tipos de comandos de aplicación: comandos slash, comandos de usuario y comandos de mensaje.
{% endhint %}

### Obtener comandos de aplicación de servidor

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /applications/{application.id}/guilds/{guild.id}/commands

Obtiene todos los comandos de servidor para tu aplicación para un servidor específico. Devuelve un array de objetos de comando de aplicación.

#### Crear comando de aplicación de servidor

<kbd><mark style="color:$success;background-color:$success;">POST<mark style="color:$success;background-color:$success;"></kbd> /applications/{application.id}/guilds/{guild.id}/commands

{% hint style="danger" %}
Crear un comando con el mismo nombre que un comando existente para tu aplicación sobrescribirá el comando antiguo.
{% endhint %}

Crea un nuevo comando de servidor. Los nuevos comandos de servidor estarán disponibles en el servidor inmediatamente. Devuelve `201` y un objeto de comando de aplicación. Si el comando no existía ya, contará hacia los límites diarios de creación de comandos de aplicación.

#### Parámetros JSON

| Campo                | Tipo                                | Descripción                                                                            |
| -------------------- | ----------------------------------- | -------------------------------------------------------------------------------------- |
| name                 | string                              | Nombre de 1-32 caracteres en minúsculas que coincida con `^[\w-]{1,32}$`.              |
| description          | string                              | Descripción de 1-100 caracteres.                                                       |
| options?             | array of application command option | Los parámetros para el comando.                                                        |
| default\_permission? | boolean (default `true`)            | Si el comando está habilitado por defecto cuando la aplicación se añade a un servidor. |
| type?                | one of application command type     | El tipo de comando, por defecto `1` si no se establece.                                |

#### Obtener comando de aplicación de servidor

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /applications/{application.id}/guilds/{guild.id}/commands/{command.id}

Obtiene un comando de servidor para tu aplicación. Devuelve un objeto de comando de aplicación.

#### Editar comando de aplicación de servidor

<kbd><mark style="color:$warning;background-color:$warning;">PATCH<mark style="color:$warning;background-color:$warning;"></kbd> /applications/{application.id}/guilds/{guild.id}/commands/{command.id}

Edita un comando de servidor. Las actualizaciones para comandos de servidor estarán disponibles inmediatamente. Devuelve `200` y un objeto de comando de aplicación.

#### Parámetros JSON

| Campo                | Tipo                                | Descripción                                                                            |
| -------------------- | ----------------------------------- | -------------------------------------------------------------------------------------- |
| name?                | string                              | Nombre de 1-32 caracteres en minúsculas que coincida con `^[\w-]{1,32}$`.              |
| description?         | string                              | Descripción de 1-100 caracteres.                                                       |
| options?             | array of application command option | Los parámetros para el comando.                                                        |
| default\_permission? | boolean (default `true`)            | Si el comando está habilitado por defecto cuando la aplicación se añade a un servidor. |

#### Eliminar comando de aplicación de servidor

<kbd><mark style="color:$danger;background-color:$warning;">DELETE<mark style="color:$danger;background-color:$warning;"></kbd> /applications/{application.id}/guilds/{guild.id}/commands/{command.id}

Elimina un comando de servidor. Devuelve `204` en caso de éxito.

#### Sobreescribir en masa comandos de aplicación de servidor

<kbd><mark style="color:$warning;background-color:$warning;">PUT<mark style="color:$warning;background-color:$warning;"></kbd> /applications/{application.id}/guilds/{guild.id}/commands

Toma una lista de comandos de aplicación, sobreescribiendo comandos existentes para el servidor. Devuelve `200` y una lista de objetos de comando de aplicación.

{% hint style="danger" %}
Esto sobrescribirá todos los tipos de comandos de aplicación: comandos slash, comandos de usuario y comandos de mensaje.
{% endhint %}

#### Obtener permisos de comando de aplicación de servidor

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /applications/{application.id}/guilds/{guild.id}/commands/permissions

Obtiene permisos de comando para todos los comandos para tu aplicación en un servidor. Devuelve un array de objetos de permisos de comando de aplicación de servidor.

#### Obtener permisos de comando de aplicación

<kbd><mark style="color:$primary;background-color:$primary;">GET<mark style="color:$primary;background-color:$primary;"></kbd> /applications/{application.id}/guilds/{guild.id}/commands/{command.id}/permissions

Obtiene permisos de comando para un comando específico para tu aplicación en un servidor. Devuelve un objeto de permisos de comando de aplicación de servidor.

### Editar permisos de comando de aplicación

<kbd><mark style="color:$warning;background-color:$warning;">PUT<mark style="color:$warning;background-color:$warning;"></kbd> /applications/{application.id}/guilds/{guild.id}/commands/{command.id}/permissions

{% hint style="warning" %}
Este endpoint sobrescribirá los permisos existentes para el comando en esa guild.
{% endhint %}

Edita permisos de comando para un comando específico para tu aplicación en un servidor. Solo puedes añadir hasta 10 sobreescrituras de permisos para un comando. Devuelve un objeto `GuildApplicationCommandPermissions`.

{% hint style="warning" %}
Eliminar o renombrar un comando eliminará permanentemente todos los permisos asociados a ese comando.
{% endhint %}

### Parámetros JSON

| Campo       | Tipo                                     | Descripción                                  |
| ----------- | ---------------------------------------- | -------------------------------------------- |
| permissions | array of application command permissions | Los permisos para el comando en el servidor. |

### Editar en lotes permisos de comando de aplicación

PUT /applications/{application.id}/guilds/{guild.id}/commands/permissions

{% hint style="warning" %}
Este endpoint sobrescribirá todos los permisos existentes para todos los comandos en una guild, incluidos los comandos slash, los comandos de usuario y los comandos de mensaje.
{% endhint %}

Edita en lotes permisos para todos los comandos en un servidor. Toma un array de objetos parciales de permisos de comando de aplicación de servidor incluyendo `id` y `permissions`.

Solo puedes añadir hasta 10 sobreescrituras de permisos para un comando.

Devuelve un array de objetos `GuildApplicationCommandPermissions`.

#### Ejemplo

```python
FIRST_COMMAND_ID = "<first_command_id>"
SECOND_COMMAND_ID = "<second_command_id>"
ADMIN_ROLE_ID = "<admin_role_id>"

url = "https://discord.com/api/v8/applications/<my_application_id>/guilds/<my_guild_id>/commands/permissions"

json = [
    {
        "id": FIRST_COMMAND_ID,
        "permissions": [
            {
                "id": ADMIN_ROLE_ID,
                "type": 1,
                "permission": True
            }
        ]
    },
    {
        "id": SECOND_COMMAND_ID,
        "permissions": [
            {
                "id": ADMIN_ROLE_ID,
                "type": 1,
                "permission": False
            }
        ]
    }
]

headers = {
    "Authorization": "Bot <my_bot_token>"
}

r = requests.put(url, headers=headers, json=json)
```

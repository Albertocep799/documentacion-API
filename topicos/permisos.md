---
icon: shield-check
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

# Permisos

Los permisos son una manera de limitar y otorgar ciertas capacidades a los usuarios en Discord. Se puede configurar un conjunto base de permisos a nivel de gremio para distintos roles. Al asignar estos roles a los usuarios, se otorgan o revocan privilegios específicos dentro del gremio. Además de los permisos a nivel de gremio, Discord soporta reescrituras de permisos que pueden asignarse a roles o usuarios individuales por canal.

{% hint style="info" %}
Los permisos para comandos de aplicación permiten habilitar o deshabilitar comandos específicos para canales completos, además de para roles o usuarios concretos.
{% endhint %}

Los permisos se almacenan como enteros de longitud variable serializados a cadena y se calculan usando operaciones a nivel de bits. Por ejemplo, el valor de permisos 123 se serializa como “123”. Para garantizar estabilidad a largo plazo, se recomienda deserializarlos utilizando librerías de enteros grandes (Big Integer) en tu lenguaje preferido. El valor total de permisos se calcula utilizando el operador OR (`|`) sobre cada valor individual, y los flags se pueden comprobar usando AND (`&`).

En API v8 y superiores, todos los permisos se serializan como cadenas, incluidos los campos `allow` y `deny` en las reescrituras. Todos los permisos nuevos también se agregan al campo base.

{% hint style="info" %}
En API v7 y anteriores (ya obsoletas), los campos `permissions`, `allow` y `deny` en roles y reescrituras aún se serializan como número, pero estos números no superan los 31 bits. Durante el tiempo de vida restante de estas APIs, los nuevos bits solo estarán en `permissions_new`, `allow_new` y `deny_new` (solo en la respuesta, las solicitudes siguen usando los campos originales con número o cadena).
{% endhint %}

```python
# Permissions value that can Send Messages (0x800) and Add Reactions (0x40):
permissions = 0x40 | 0x800 # 2112

# Checking for flags that are set:
(permissions & 0x40) == 0x40   # True
(permissions & 0x800) == 0x800 # True

# Kick Members (0x2) was not set:
(permissions & 0x2) == 0x2 # False
```

Se requiere lógica adicional cuando hay reescrituras de permisos, esto se explica más abajo. Para más información sobre operaciones a nivel de bits y flags, consulta [esta página](https://en.wikipedia.org/wiki/Bit_field).

A continuación se muestra una tabla con todos los permisos actuales, sus valores hexadecimales, una breve descripción de los privilegios que otorgan y el tipo de canal al que aplican, si corresponde:

#### Banderas de permisos a nivel de bits

| Valor               | Nombre                                   | Descripción                                                                                                                        | Tipo de canal |
| ------------------- | ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| 0x0001 (1 << 0)     | CREATE\_INSTANT\_INVITE                  | Permite crear invitaciones instantáneas.                                                                                           | T, V, S       |
| 0x0002 (1 << 1)     | KICK\_MEMBERS 1                          | Permite expulsar miembros.                                                                                                         |               |
| 0x0004 (1 << 2)     | BAN\_MEMBERS 1                           | Permite banear miembros.                                                                                                           |               |
| 0x0008 (1 << 3)     | ADMINISTRATOR 1                          | Otorga todos los permisos y omite las reescrituras de permisos de canal.                                                           |               |
| 0x00010 (1 << 4)    | MANAGE\_CHANNELS 1                       | Permite gestionar y editar canales.                                                                                                | T, V, S       |
| 0x00020 (1 << 5)    | MANAGE\_GUILD 1                          | Permite administrar y editar el gremio.                                                                                            |               |
| 0x00040 (1 << 6)    | ADD\_REACTIONS                           | Permite añadir reacciones a mensajes.                                                                                              | T, V, S       |
| 0x00080 (1 << 7)    | VIEW\_AUDIT\_LOG                         | Permite ver los registros de auditoría.                                                                                            |               |
| 0x000100 (1 << 8)   | PRIORITY\_SPEAKER                        | Permite usar el altavoz prioritario en canales de voz.                                                                             | V             |
| 0x000200 (1 << 9)   | STREAM                                   | Permite usar video y transmitir (go live) en un canal de voz.                                                                      | V, S          |
| 0x000400 (1 << 10)  | VIEW\_CHANNEL                            | Permite a los miembros del gremio ver un canal, lo que incluye leer mensajes en texto y unirse a canales de voz.                   | T, V, S       |
| 0x000800 (1 << 11)  | SEND\_MESSAGES                           | Permite enviar mensajes en un canal y crear hilos en un foro (no permite enviar mensajes en hilos).                                | T, V, S       |
| 0x0001000 (1 << 12) | SEND\_TTS\_MESSAGES                      | Permite enviar mensajes `/tts`.                                                                                                    | T, V, S       |
| 0x0002000 (1 << 13) | MANAGE\_MESSAGES 1                       | Permite eliminar mensajes de otros usuarios.                                                                                       | T, V, S       |
| 0x0004000 (1 << 14) | EMBED\_LINKS                             | Los enlaces enviados por usuarios con este permiso se auto-incrustarán.                                                            | T, V, S       |
| 0x0008000 (1 << 15) | ATTACH\_FILES                            | Permite subir imágenes y archivos.                                                                                                 | T, V, S       |
| 0x0001000 (1 << 16) | READ\_MESSAGE\_HISTORY                   | Permite leer el historial de mensajes.                                                                                             | T, V, S       |
| 0x0002000 (1 << 17) | MENTION\_EVERYONE                        | Permite utilizar la etiqueta @everyone para notificar a todos en un canal, y la @here para notificar a los conectados en un canal. | T, V, S       |
| 0x0004000 (1 << 18) | USE\_EXTERNAL\_EMOJIS                    | Permite el uso de emojis personalizados de otros servidores.                                                                       | T, V, S       |
| 0x0008000 (1 << 19) | VIEW\_GUILD\_INSIGHTS                    | Permite ver estadísticas del gremio.                                                                                               |               |
| 0x0001000 (1 << 20) | CONNECT                                  | Permite unirse a un canal de voz.                                                                                                  | V, S          |
| 0x0002000 (1 << 21) | SPEAK                                    | Permite hablar en un canal de voz.                                                                                                 | V             |
| 0x0004000 (1 << 22) | MUTE\_MEMBERS                            | Permite silenciar miembros en un canal de voz.                                                                                     | V, S          |
| 0x0008000 (1 << 23) | DEAFEN\_MEMBERS                          | Permite ensordecer a miembros en un canal de voz.                                                                                  | V             |
| 0x0001000 (1 << 24) | MOVE\_MEMBERS                            | Permite mover miembros entre canales de voz.                                                                                       | V, S          |
| 0x0002000 (1 << 25) | USE\_VAD                                 | Permite el uso de detección de voz en un canal de voz.                                                                             | V             |
| 0x0004000 (1 << 26) | CHANGE\_NICKNAME                         | Permite modificar tu propio apodo.                                                                                                 |               |
| 0x0008000 (1 << 27) | MANAGE\_NICKNAMES                        | Permite modificar los apodos de otros usuarios.                                                                                    |               |
| 0x0001000 (1 << 28) | MANAGE\_ROLES 1                          | Permite administrar y editar roles.                                                                                                | T, V, S       |
| 0x0002000 (1 << 29) | MANAGE\_WEBHOOKS 1                       | Permite administrar y editar webhooks.                                                                                             | T, V, S       |
| 0x0004000 (1 << 30) | MANAGE\_EXPRESSIONS 1 3                  | Permite editar y eliminar emojis, stickers y sonidos del soundboard.                                                               |               |
| 0x0008000 (1 << 31) | USE\_APPLICATION\_COMMANDS               | Permite usar comandos de aplicación, incluidas barras y menú contextual.                                                           | T, V, S       |
| 0x0001000 (1 << 32) | REQUEST\_TO\_SPEAK                       | Permite solicitar hablar en canales de escenario.                                                                                  | S             |
| 0x0002000 (1 << 33) | MANAGE\_EVENTS 3                         | Permite editar y eliminar eventos programados.                                                                                     | V, S          |
| 0x0004000 (1 << 34) | MANAGE\_THREADS 1                        | Permite eliminar y archivar hilos y ver todos los hilos privados.                                                                  | T             |
| 0x0008000 (1 << 35) | CREATE\_PUBLIC\_THREADS                  | Permite crear hilos públicos y de anuncios.                                                                                        | T             |
| 0x0001000 (1 << 36) | CREATE\_PRIVATE\_THREADS                 | Permite crear hilos privados.                                                                                                      | T             |
| 0x0002000 (1 << 37) | USE\_EXTERNAL\_STICKERS                  | Permite el uso de stickers personalizados de otros servidores.                                                                     | T, V, S       |
| 0x0004000 (1 << 38) | SEND\_MESSAGES\_IN\_THREADS              | Permite enviar mensajes en hilos.                                                                                                  | T             |
| 0x0008000 (1 << 39) | USE\_EMBEDDED\_ACTIVITIES                | Permite usar actividades integradas en un canal de voz.                                                                            | T, V          |
| 0x0001000 (1 << 40) | MODERATE\_MEMBERS 2                      | Permite silenciar usuarios para impedirles enviar o reaccionar a mensajes en chat, hilos o hablar en voz y escenario.              |               |
| 0x0002000 (1 << 41) | VIEW\_CREATOR\_MONETIZATION\_ANALYTICS 1 | Permite ver estadísticas de membresías de rol del gremio.                                                                          |               |
| 0x0004000 (1 << 42) | USE\_SOUNDBOARD                          | Permite el uso del soundboard en un canal de voz.                                                                                  | V             |
| 0x0008000 (1 << 43) | CREATE\_EXPRESSIONS 3                    | Permite crear emojis, stickers y sonidos del soundboard, y editar/borrar los propios.                                              |               |
| 0x0001000 (1 << 44) | CREATE\_EVENTS 3                         | Permite crear eventos programados y editar/borrar los propios.                                                                     |               |
| 0x0004000 (1 << 45) | USE\_EXTERNAL\_SOUNDS                    | Permite usar sonidos del soundboard de otros servidores.                                                                           | V             |
| 0x0004000 (1 << 46) | SEND\_VOICE\_MESSAGES                    | Permite enviar mensajes de voz en un canal.                                                                                        | T, V, S       |
| 0x0008000 (1 << 47) | USE\_CLYDE\_AI                           | Permite usar la integración con Clyde AI.                                                                                          | T, V, S       |
| 0x0001000 (1 << 48) | SET\_VOICE\_CHANNEL\_STATUS              | Permite establecer estado de canal de voz.                                                                                         | V             |
| 0x0002000 (1 << 49) | SEND\_POLLS                              | Permite enviar encuestas.                                                                                                          | T, V, S       |
| 0x0004000 (1 << 50) | USE\_EXTERNAL\_APPS                      | Permite el uso de aplicaciones instaladas por el usuario sin respuestas efímeras forzadas.                                         | T, V, S       |

<sup>1</sup> Estos permisos requieren que el usuario o el propietario del bot utilicen autenticación multifactor cuando se usen en una guild que tenga habilitada la MFA a nivel de servidor.

<sup>2</sup> Consulta Permisos para Miembros en Tiempo de Espera / en Cuarentena para entender cómo se modifican temporalmente los permisos para los usuarios en tiempo de espera o en cuarentena.

<sup>3</sup> Los eventos separados para la creación de recursos solo están disponibles para los clientes que especifiquen un número de compilación reciente del cliente. De lo contrario, se ignoran y se aplican los permisos de gestión.

Nota que los nombres de los permisos pueden referirse de manera diferente en el cliente de Discord. Por ejemplo, "Administrar permisos" se refiere a `MANAGE_ROLES`, "Usar actividad de voz" se refiere a `USE_VAD`, y "Silenciar temporalmente a miembros" se refiere a `MODERATE_MEMBERS`.

Las abreviaturas de tipo de canal indican lo siguiente:

| Tipo de canal | Valores                                                      |
| ------------- | ------------------------------------------------------------ |
| T             | GUILD\_TEXT, GUILD\_ANNOUNCEMENT, GUILD\_FORUM, GUILD\_MEDIA |
| V             | GUILD\_VOICE                                                 |
| S             | GUILD\_STAGE\_VOICE                                          |

### Jerarquía de permisos <a href="#jerarqua-de-permisos" id="jerarqua-de-permisos"></a>

La forma en la que se aplican los permisos puede parecer intuitiva a primera vista, pero existen restricciones ocultas que evitan que los usuarios realicen ciertas acciones inapropiadas en función del rol más alto de un usuario en comparación al objetivo. El rol más alto de un usuario es aquel con mayor prioridad de orden en el gremio, empezando por @everyone con prioridad 0. Los permisos siguen reglas jerárquicas según:

{% hint style="info" %}
Los roles de gremio se ordenan usando la clave (`posición`, `id`). Así, si varios roles tienen la misma posición, se ordenan por ID ascendente.
{% endhint %}

* Un usuario puede conceder roles a otros usuarios que estén en una posición inferior a la de su propio rol más alto.
* Un usuario puede editar roles de posición inferior a su rol más alto, pero solo puede concederles permisos que él mismo posea.
* Un usuario sólo puede ordenar roles inferiores a su rol más alto.
* Un usuario sólo puede expulsar, banear y editar apodos de usuarios cuyo rol más alto sea inferior al suyo.

En otros casos, los permisos no respetan la jerarquía de roles. Por ejemplo, si un usuario tiene dos roles: A y B. A niega el permiso `VIEW_CHANNEL` en un canal #coolstuff. B concede el permiso `VIEW_CHANNEL` en el mismo canal. El usuario podrá ver el canal #coolstuff sin importar la posición de los roles.

### Reescritura de permisos <a href="#reescritura-de-permisos" id="reescritura-de-permisos"></a>

Las reescrituras permiten aplicar ciertos permisos a roles o usuarios en canales individuales. Los permisos aplicables se indican por **T** (texto), **V** (voz), o **S** (escenario) en la tabla anterior.

Cuando se utilizan reescrituras, puede darse el caso de colisiones: el usuario puede tener reescrituras contradictorias respecto a sus permisos a nivel de gremio. Considerando esto, los permisos se aplican con la siguiente jerarquía:

1. Los permisos base dados a @everyone se aplican a nivel de gremio.
2. Los permisos otorgados a un usuario por sus roles se aplican a nivel de gremio.
3. Las reescrituras que niegan permisos para @everyone se aplican a nivel de canal.
4. Las reescrituras que conceden permisos para @everyone se aplican a nivel de canal.
5. Las reescrituras negativas específicas para roles se aplican a nivel de canal.
6. Las reescrituras positivas específicas para roles se aplican a nivel de canal.
7. Las reescrituras negativas para el miembro se aplican a nivel de canal.
8. Las reescrituras positivas para el miembro se aplican a nivel de canal.

El siguiente pseudocódigo ilustra el proceso programáticamente:

```python
def compute_base_permissions(member, guild):
    if guild.is_owner(member):
        return ALL

    role_everyone = guild.get_role(guild.id)  # get @everyone role
    permissions = role_everyone.permissions

    for role in member.roles:
        permissions |= role.permissions

    if permissions & ADMINISTRATOR == ADMINISTRATOR:
        return ALL

    return permissions

def compute_overwrites(base_permissions, member, channel):
    # ADMINISTRATOR overrides any potential permission overwrites, so there is nothing to do here.
    if base_permissions & ADMINISTRATOR == ADMINISTRATOR:
        return ALL

    permissions = base_permissions
    overwrite_everyone = overwrites.get(channel.guild_id)  # Find (@everyone) role overwrite and apply it.
    if overwrite_everyone:
        permissions &= ~overwrite_everyone.deny
        permissions |= overwrite_everyone.allow

    # Apply role specific overwrites.
    overwrites = channel.permission_overwrites
    allow = NONE
    deny = NONE
    for role_id in member.roles:
        overwrite_role = overwrites.get(role_id)
        if overwrite_role:
            allow |= overwrite_role.allow
            deny |= overwrite_role.deny

    permissions &= ~deny
    permissions |= allow

    # Apply member specific overwrite if it exist.
    overwrite_member = overwrites.get(member.user_id)
    if overwrite_member:
        permissions &= ~overwrite_member.deny
        permissions |= overwrite_member.allow

    return permissions

def compute_permissions(member, channel):
    base_permissions = compute_base_permissions(member, channel.guild)
    return compute_overwrites(base_permissions, member, channel)
```

### Permisos implícitos <a href="#permisos-implcitos" id="permisos-implcitos"></a>

Algunos permisos en Discord se niegan o conceden de forma implícita según el uso lógico. Los dos casos principales son `VIEW_CHANNEL` y `SEND_MESSAGES` en canales de texto. Negar `VIEW_CHANNEL` a un usuario o rol en un canal, implícitamente niega otros permisos sobre ese canal. Aunque permisos como `SEND_MESSAGES` no estén negados explícitamente, se ignoran porque el usuario no puede leer el canal.

Negar `SEND_MESSAGES` niega implícitamente `MENTION_EVERYONE`, `SEND_TTS_MESSAGES`, `ATTACH_FILES` y `EMBED_LINKS`. De nuevo, no están negados explícitamente en los cálculos, pero se ignoran porque el usuario no puede realizar la acción base.

En canales de voz y escenario, negar el permiso `CONNECT` también niega implícitamente permisos como `MANAGE_CHANNELS`.

Puede haber otros casos donde un permiso niegue o conceda otros de forma implícita, siempre basados en la lógica de cómo debe o no debe interactuar un usuario con Discord según sus permisos.

### Permisos heredados (hilos) <a href="#permisos-heredados-hilos" id="permisos-heredados-hilos"></a>

Los hilos heredan los permisos del canal padre donde se crearon, con una excepción: el permiso `SEND_MESSAGES` no se hereda; los usuarios deben tener `SEND_MESSAGES_IN_THREADS` para enviar mensajes en un hilo, lo que les permite participar en hilos como en canales de anuncios.

Los usuarios deben tener el permiso `VIEW_CHANNEL` para ver cualquier hilo en el canal, aunque hayan sido mencionados o añadidos directamente al hilo.

### Permisos para miembros silenciados o en cuarentena <a href="#permisos-para-miembros-silenciados-o-en-cuarentena" id="permisos-para-miembros-silenciados-o-en-cuarentena"></a>

Los miembros silenciados temporalmente perderán todos los permisos excepto `VIEW_CHANNEL` y `READ_MESSAGE_HISTORY`. Esto también aplica a los miembros en cuarentena, con la adición de `CHANGE_NICKNAME`. Los dueños y usuarios con `ADMINISTRATOR` están exentos.

### Permisos en gremios descubribles <a href="#permisos-en-gremios-descubribles" id="permisos-en-gremios-descubribles"></a>

Los gremios definidos como descubribles tienen una configuración de permisos especial para permitir que no-miembros "ronden" por el gremio sin unirse. Los no-miembros heredan los permisos `VIEW_CHANNEL` y `READ_MESSAGE_HISTORY` del rol @everyone, lo que les permite ver canales públicos y leer mensajes. Sin embargo, no pueden interactuar con el gremio de ninguna manera, como enviar mensajes, reaccionar o unirse a canales de voz.

Si un no-miembro está rondando un gremio descubrible con una instancia de escenario pública activa, heredará además los permisos `CONNECT`, `REQUEST_TO_SPEAK`, `SPEAK` y `USE_VAD` en ese canal de escenario.

### Sincronización de permisos <a href="#sincronizacin-de-permisos" id="sincronizacin-de-permisos"></a>

Los permisos respecto a categorías y canales dentro de categorías funcionan mediante sincronización, no herencia directa. Si un canal hijo tiene los mismos permisos y reescrituras (o ninguna) que su categoría padre, se considera "sincronizado" con la categoría. Cualquier cambio futuro en la **categoría padre** se reflejará en sus canales sincronizados. Cualquier cambio en el **canal hijo** hará que deje de estar sincronizado, y sus permisos ya no cambiarán junto con los de la categoría padre.

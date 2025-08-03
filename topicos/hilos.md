---
icon: reel
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

# Hilos

Los hilos son una nueva característica de Discord. Los hilos pueden pensarse como sub-canales temporales dentro de un canal existente, para ayudar a organizar mejor la conversación en un canal ocupado.

Los hilos han sido diseñados para ser muy similares a los objetos de canal, y este tema agrega toda la información sobre hilos, lo que debería ayudar a hacer la migración muy directa.

### Compatibilidad hacia atrás <a href="#compatibilidad-hacia-atrs" id="compatibilidad-hacia-atrs"></a>

Los hilos solo están disponibles en API v9. Los usuarios que no actualicen a API v9 no recibirán la mayoría de eventos Gateway para hilos, o cosas que suceden en hilos (como Message Create). Los usuarios en API v8 aún recibirán eventos Gateway para interacciones.

La lista de eventos Gateway que pueden descartarse incluye, pero no se limita a:

* MESSAGE\_CREATE
* MESSAGE\_DELETE
* MESSAGE\_DELETE\_BULK
* MESSAGE\_REACTION\_ADD
* MESSAGE\_REACTION\_REMOVE
* MESSAGE\_REACTION\_REMOVE\_ALL
* MESSAGE\_REACTION\_REMOVE\_EMOJI
* MESSAGE\_UPDATE
* THREAD\_CREATE
* THREAD\_UPDATE
* THREAD\_DELETE
* THREAD\_MEMBER\_UPDATE
* THREAD\_MEMBERS\_UPDATE

### Campos de hilo <a href="#campos-de-hilo" id="campos-de-hilo"></a>

Dado que los hilos son un nuevo tipo de canal, comparten y reutilizan varios de los campos existentes en un objeto de canal:

* `id`, `guild_id`, `type`, `name`, `last_message_id`, `last_pin_timestamp`, `rate_limit_per_user` están siendo reutilizados
* `owner_id` ha sido reutilizado para almacenar el ID del usuario que inició el hilo
* `parent_id` ha sido reutilizado para almacenar el ID del canal `GUILD_TEXT` o `GUILD_NEWS` en el que se creó el hilo

Además, hay algunos campos nuevos que solo están disponibles en hilos:

* `member_count` almacena un conteo aproximado de miembros, pero deja de contar en 50
* `message_count` y `total_message_sent` almacenan el número de mensajes en un hilo. La diferencia es que cuando se elimina un mensaje, `message_count` se decrementa, pero `total_message_sent` no lo hará (los hilos creados antes del 1 de julio de 2022 dejan de contar ambos valores en 50).
* `thread_metadata` contiene algunos campos específicos de hilo, `archived`, `archive_timestamp`, `auto_archive_duration`, `locked`. `archive_timestamp` se cambia al crear, archivar o desarchivar un hilo, y al cambiar el campo `auto_archive_duration`.

### Hilos públicos y privados <a href="#hilos-pblicos-y-privados" id="hilos-pblicos-y-privados"></a>

Los hilos públicos son visibles para todos los que pueden ver el canal padre del hilo. Los hilos públicos deben crearse desde un mensaje existente, pero pueden quedar "huérfanos" si ese mensaje se elimina. El hilo creado y el mensaje desde el que se inició compartirán el mismo ID. El tipo de hilo creado coincide con el tipo del canal padre. Los canales `GUILD_TEXT` crean `PUBLIC_THREAD` y los canales `GUILD_NEWS` crean `NEWS_THREAD`.

Los hilos privados se comportan similar a los MD grupales, pero en un servidor. Los hilos privados siempre se crean con el tipo `PRIVATE_THREAD` y solo se pueden crear en canales `GUILD_TEXT`.

### Hilos activos y archivados <a href="#hilos-activos-y-archivados" id="hilos-activos-y-archivados"></a>

Cada hilo puede estar activo o archivado. Cambiar un hilo de archivado -> activo se refiere como desarchivar el hilo. Los hilos que tienen `locked` establecido en true solo pueden ser desarchivados por un usuario con el permiso `MANAGE_THREADS`.

Además de ayudar a desenredar la interfaz para los usuarios, el archivado existe para limitar el conjunto de trabajo de hilos que necesitan mantenerse. Dado que el número de hilos archivados puede ser bastante grande, mantenerlos todos en memoria puede ser bastante prohibitivo. Por lo tanto, los servidores están limitados a un cierto número de hilos activos, y solo los hilos activos pueden ser manipulados. Los usuarios no pueden editar mensajes, añadir reacciones, usar comandos de aplicación, o unirse a hilos archivados. La única operación que debería suceder dentro de un hilo archivado es que se eliminen mensajes. Enviar un mensaje desarchivará automáticamente el hilo, a menos que el hilo haya sido bloqueado por un moderador.

Debido a esta restricción, el protocolo Gateway está diseñado para asegurar que los usuarios puedan tener una vista precisa del conjunto completo de hilos activos, pero los hilos archivados no se sincronizan por adelantado vía el gateway.

Los hilos no cuentan contra el límite de canales máximos en un servidor, pero habrá un nuevo límite en el número máximo de hilos activos en un servidor.

Los hilos se archivan automáticamente después de un período de inactividad (mientras un servidor se acerca al límite máximo de hilos, este temporizador se reducirá automáticamente, pero nunca por debajo de `auto_archive_duration`). "Actividad" se define como enviar un mensaje, desarchivar un hilo, o cambiar el tiempo de auto-archivado. El campo `auto_archive_duration` anteriormente controlaba cuánto tiempo podía permanecer activo un hilo, pero ahora se reutiliza para controlar cuánto tiempo permanece el hilo en la lista de canales. Los canales también pueden establecer `default_auto_archive_duration`, que es usado por los clientes oficiales para preseleccionar un valor diferente de `auto_archive_duration` cuando un usuario crea un hilo.

### Permisos <a href="#permisos" id="permisos"></a>

Los hilos generalmente heredan permisos del canal padre (por ejemplo, si puedes añadir reacciones en el canal padre, puedes hacer eso en un hilo también).

Tres bits de permiso son específicos para hilos: `CREATE_PUBLIC_THREADS`, `CREATE_PRIVATE_THREADS`, y `SEND_MESSAGES_IN_THREADS`.

{% hint style="warning" %}
El permiso **SEND\_MESSAGES** no tiene efecto en los hilos; los usuarios deben tener **SEND\_MESSAGES\_IN\_THREADS** para poder hablar en un hilo.
{% endhint %}

Los hilos privados son similares a los MD grupales, pero en un servidor: Debes ser invitado al hilo para poder verlo o participar en él, o ser un moderador (permiso `MANAGE_THREADS`).

Finalmente, los hilos son tratados ligeramente diferente de los canales en el protocolo Gateway. Los clientes no serán informados de un hilo a través del Gateway si no tienen permiso para ver ese hilo.

### Eventos gateway <a href="#eventos-gateway" id="eventos-gateway"></a>

* Guild Create contiene un nuevo campo, `threads`, que es un array de objetos de canal. Para bots, esto representa todos los hilos activos en el servidor que el usuario actual puede ver. Para cuentas de usuario, solo se envían hilos unidos.
* Cuando se crea, actualiza o elimina un hilo, se envía un evento Thread Create, Thread Update, o Thread Delete. Como sus contrapartes de canal, estos solo contienen un hilo.
* Dado que el Gateway solo sincroniza hilos activos que el usuario puede ver, si un usuario _gana_ acceso a un canal, entonces el Gateway puede necesitar sincronizar los hilos activos en ese canal al usuario. Enviará un evento Thread List Sync para esto.

### Membresía de hilo <a href="#membresa-de-hilo" id="membresa-de-hilo"></a>

Cada hilo rastrea membresía explícita. Hay dos casos de uso principales para estos datos:

* Los clientes usan _su propio_ miembro de hilo para calcular estados de lectura y configuraciones de notificación.
* Saber a todos los que están en un hilo.

La membresía se rastrea en un array de objetos de miembro de hilo. Estos tienen cuatro campos, `id` (el ID del hilo), `user_id`, `join_timestamp`, y `flags`. Actualmente las únicas `flags` son para configuraciones de notificación, pero otras pueden añadirse en futuras actualizaciones.

#### Sincronización para el usuario actual

* Un evento Gateway Thread Members Update siempre se envía cuando el usuario actual es añadido o eliminado de un hilo.
* Un evento Gateway Thread Member Update se envía cada vez que se actualiza el objeto de miembro de hilo del usuario actual.
* Ciertas llamadas API, como listar hilos archivados y búsqueda devolverán un array de objetos de miembro de hilo para cualquier hilo devuelto del que el usuario actual sea miembro. Otras llamadas API, como obtener un canal devolverán el objeto de miembro de hilo para el usuario actual como una propiedad en el canal, si el usuario actual es miembro del hilo.
* El evento Gateway Guild Create contendrá un objeto de miembro de hilo como una propiedad en cualquier hilo devuelto del que el actual sea miembro.
* El evento Gateway Thread Create contendrá un objeto de miembro de hilo como una propiedad del hilo si el usuario actual es miembro, y el usuario ha ganado recientemente acceso para ver el canal padre.
* El evento Gateway Thread List Sync contendrá un array de objetos de miembro de hilo para cualquier hilo devuelto del que el usuario actual sea miembro.

#### Sincronización para otros usuarios

{% hint style="info" %}
Estos requieren el intent **GUILD\_MEMBERS** del Gateway.
{% endhint %}

* Una llamada API `GET` a `/channels/<channel_id>/thread-members` que devuelve un array de objetos de miembro de hilo.
* El evento Gateway Thread Members Update que incluirá a todos los usuarios que fueron añadidos o eliminados de un hilo por una acción.

### Editando y eliminando hilos <a href="#editando-y-eliminando-hilos" id="editando-y-eliminando-hilos"></a>

Los hilos pueden ser editados y eliminados con los endpoints existentes `PATCH` y `DELETE` para editar un canal.

* Eliminar un hilo requiere el permiso `MANAGE_THREADS`.
* Editar un hilo para establecer `archived` a `false` solo requiere que el usuario actual ya haya sido añadido al hilo. Si `locked` es true, entonces el usuario debe tener `MANAGE_THREADS`
* Editar un hilo para cambiar los campos `name`, `archived`, `auto_archive_duration` requiere `MANAGE_THREADS` o que el usuario actual sea el creador del hilo.
* Editar un hilo para cambiar `rate_limit_per_user` o `locked` requiere `MANAGE_THREADS`. `locked` también puede establecerse a `true` por el creador del hilo.

### Hilos NSFW <a href="#hilos-nsfw" id="hilos-nsfw"></a>

Los hilos no establecen explícitamente el campo `nsfw`. Todos los hilos en un canal marcado como `nsfw` heredan esa configuración.

### Nuevos tipos de mensaje <a href="#nuevos-tipos-de-mensaje" id="nuevos-tipos-de-mensaje"></a>

Los hilos introducen algunos nuevos tipos de mensaje, y reutilizan algunos otros:

* `RECIPIENT_ADD` y `RECIPIENT_REMOVE` han sido reutilizados para también enviar cuando un usuario es añadido o eliminado de un hilo por alguien más.
* `CHANNEL_NAME_CHANGE` ha sido reutilizado y se envía cuando el nombre del hilo cambia.
* `THREAD_CREATED` es un nuevo mensaje enviado al canal padre `GUILD_TEXT`, usado para informar a los usuarios que se ha creado un hilo. Actualmente solo se envía en un caso: cuando se crea un `PUBLIC_THREAD` desde un mensaje más antiguo (más antiguo aún está por determinar, pero actualmente está establecido a un valor muy pequeño). El mensaje contiene una referencia de mensaje con el `guild_id` y `channel_id` del hilo. El `content` del mensaje es el `name` del hilo.
* `THREAD_STARTER_MESSAGE` es un nuevo mensaje enviado como el primer mensaje en hilos que se inician desde un mensaje existente en el canal padre. _Solo_ contiene un campo de referencia de mensaje que apunta al mensaje desde el cual se inició el hilo.

### Enumerando hilos <a href="#enumerando-hilos" id="enumerando-hilos"></a>

Hay muchas rutas `GET` para enumerar hilos en un canal específico:

* <mark style="color:blue;">`/guilds/<guild_id>/threads/active`</mark> devuelve todos los hilos activos en un servidor que el usuario actual puede acceder, incluye hilos públicos y privados.
* <mark style="color:blue;">`/channels/<channel_id>/threads/active`</mark> devuelve todos los hilos activos en un canal que el usuario actual puede acceder, incluye hilos públicos y privados.
* <mark style="color:blue;">`/channels/<channel_id>/threads/search`</mark> devuelve todos los hilos activos y archivados en un canal que el usuario actual puede acceder, incluye hilos públicos y privados y puede ser filtrado por consulta.
* <mark style="color:blue;">`/channels/<channel_id>/users/@me/threads/archived/private`</mark> devuelve todos los hilos archivados y privados en un canal, del que el usuario actual es miembro, ordenados por ID de hilo descendente.
* <mark style="color:blue;">`/channels/<channel_id>/threads/archived/public`</mark> devuelve todos los hilos archivados y públicos en un canal, ordenados por marca de tiempo de archivo descendente.
* <mark style="color:blue;">`/channels/<channel_id>/threads/archived/private`</mark> devuelve todos los hilos archivados y privados en un canal, ordenados por marca de tiempo de archivo descendente.

### Webhooks <a href="#webhooks" id="webhooks"></a>

Los webhooks pueden enviar mensajes a hilos usando el parámetro de consulta `thread_id`. Consulta la documentación Execute Webhook para más detalles.

Aunque los hilos son en su mayoría similares a los canales en términos de estructura y cómo se sincronizan, hay dos requisitos de producto importantes que llevan a diferencias en cómo se sincronizan los hilos y canales. Esta sección ayuda a explicar el comportamiento detrás de las dispatches Thread List Sync y Thread Create al revisar esos problemas y cómo se resuelven.

Los dos requisitos de producto son: El Gateway solo sincronizará hilos a un cliente que el cliente tenga permiso para ver, y solo sincronizará esos hilos una vez que el cliente se haya "suscrito" al servidor. Para contexto, en los clientes oficiales de Discord, una suscripción sucede cuando el usuario visita un canal en el servidor.

Como se mencionó, estos llevan a un par de casos extremos que vale la pena explicar:

### Detalles sobre acceso y sincronización de hilos <a href="#detalles-sobre-acceso-y-sincronizacin-de-hilos" id="detalles-sobre-acceso-y-sincronizacin-de-hilos"></a>

Aunque la sincronización de hilos es similar a los canales, hay dos diferencias importantes que son relevantes para eventos Thread List Sync y Thread Create:

* El Gateway solo sincronizará hilos que el usuario tenga permiso para ver.
* El Gateway solo sincronizará hilos una vez que el usuario se haya "suscrito" al servidor. Para contexto, en los clientes oficiales de Discord, una suscripción sucede cuando el usuario visita un canal en el servidor.

Estas diferencias significan que hay algún comportamiento único que vale la pena explicar.

#### Acceso a hilos

#### Ganando acceso a hilos privados

Cuando un usuario es añadido a un hilo privado, probablemente no tenga ese hilo en memoria aún ya que no tiene permiso para verlo.

Los hilos privados solo se sincronizan contigo si eres miembro o moderador. Cada vez que un usuario es añadido a un hilo privado, el Gateway también envía un evento Thread Create. Esto asegura que el cliente siempre tenga un valor no nulo para ese hilo.

{% hint style="info" %}
El evento crear hilo también se envía cuando el usuario es moderador (y por lo tanto ya tendría el canal en memoria).
{% endhint %}

#### Ganando acceso a hilos públicos

Cuando un cliente es añadido a un hilo público, pero aún no se ha suscrito a hilos, puede que no tenga ese hilo público en memoria aún. Esto es en realidad solo un problema para cuentas de usuario, y no para bots. El Gateway auto-suscribirá bots a todas las dispatches de hilos y hilos activos al conectarse. Pero las cuentas de usuario solo reciben hilos que están activos y también se han unido al conectarse para reducir la cantidad de datos necesarios en la conexión inicial. Pero esto significa que cuando una cuenta de usuario es añadida a un hilo, ese hilo ahora se convierte en un hilo "activo-unido" y necesita sincronizarse al cliente. Para resolver esto, cada vez que un usuario es añadido a _cualquier_ hilo, el Gateway también envía una dispatch Thread Create.

#### Acceso a canales

#### Ganando acceso a canales

Cuando un usuario gana acceso a un canal (por ejemplo, se le da el rol de moderador), probablemente no tendrá los hilos en memoria para ese canal ya que el Gateway solo sincroniza hilos que el cliente tiene permiso para ver. Para explicar esto, se envía un evento Thread List Sync.

#### Perdiendo acceso a canales

Cuando un usuario pierde acceso a un canal, el Gateway **no** envía un evento Thread Delete (o cualquier evento equivalente específico de hilo). En su lugar, el usuario recibirá el evento que causó que cambiaran sus permisos en el canal.

Si un usuario quisiera rastrear cuándo perdió acceso a cualquier hilo, es posible pero difícil ya que necesitaría manejar todos los casos correctamente. Usualmente, los eventos que causan cambios de permisos son un evento Guild Role Update, Guild Member Update o Channel Update.

{% hint style="info" %}
Los clientes oficiales de Discord verifican primero sus permisos al realizar una acción. De esta manera, incluso si tienen datos obsoletos, no actúan basándose en ellos.
{% endhint %}

Además, cuando un usuario pierde acceso a un canal, no son eliminados del hilo y continuarán siendo reportados como miembro de ese hilo. Sin embargo, **no** recibirán ningún evento Gateway nuevo a menos que sean eliminados del hilo, en cuyo caso recibirán un evento Thread Members Update.

#### Desarchivando un hilo

Cuando un hilo es desarchivado, como las cuentas de usuario solo cargan hilos activos en memoria al inicio, no hay garantía de que un usuario tenga el hilo o su estado de miembro en memoria. Para explicar esto, el Gateway enviará dos eventos (en el orden listado):

* Un evento Thread Update, que contiene el objeto de canal completo.
* Un evento Thread Member Update, que se envía a todos los miembros del hilo desarchivado, para que los usuarios sepan que son miembros y cuál es su configuración de notificación.

#### Foros <a href="#foros" id="foros"></a>

Un canal `GUILD_FORUM` es similar a un canal `GUILD_TEXT`, excepto que _solo_ se pueden crear hilos en ellos. A menos que se indique lo contrario, los hilos en canales de foro se comportan de la misma manera que en canales de texto, lo que significa que usan los mismos endpoints y reciben los mismos eventos Gateway.

{% hint style="info" %}
Más información sobre los canales de foro y cómo aparecen en Discord se puede encontrar en las preguntas frecuentes sobre canales de foro (FAQ canales de foro).
{% endhint %}

### Canales de medios <a href="#canales-de-medios" id="canales-de-medios"></a>

Un canal `GUILD_MEDIA` es similar a un canal `GUILD_FORUM`. Similar al canal de foro, solo se pueden crear hilos en ellos. A menos que se indique lo contrario, los hilos en canales de medios se comportan de la misma manera que en canal de foro, lo que significa que usan los mismos endpoints y reciben los mismos eventos Gateway.

{% hint style="info" %}
Más información sobre los canales de medios y cómo aparecen en Discord se puede encontrar en las preguntas frecuentes sobre canales de medios (FAQ canales multimedia).
{% endhint %}

#### Creando hilos en canales solo-hilos

Dentro de un canal solo-hilos, los hilos aparecen como publicaciones. Pueden crearse usando el endpoint Create Thread como hilos en canales de texto, pero con parámetros ligeramente diferentes. Por ejemplo, al crear hilos en un canal solo-hilos, se crea un mensaje que tiene el mismo ID que el hilo. Esto requiere que pases parámetros tanto para un hilo _como_ para un mensaje.

Los hilos en un canal solo-hilos tienen el mismo comportamiento de permisos que los hilos en un canal de texto, heredando todos los permisos del canal padre, con una excepción: crear un hilo en un canal solo-hilos solo requiere el permiso `SEND_MESSAGES`.

#### Campos de canal solo-hilos

Vale la pena señalar algunos detalles sobre campos específicos de canales solo-hilos que pueden ser importantes tener en cuenta:

* El campo `last_message_id` es el ID del hilo creado más recientemente en ese canal. Como con los mensajes, no recibirás un evento Channel Update cuando el campo cambie. En su lugar, los clientes deberían actualizar el valor al recibir eventos Thread Create.
* El campo `topic` es lo que se muestra en la sección "Guidelines" dentro de los clientes.
* El campo `rate_limit_per_user` limita qué tan frecuentemente se pueden crear hilos. Hay un nuevo campo `default_thread_rate_limit_per_user` en canales solo-hilos también, que limita qué tan a menudo se pueden enviar mensajes _en un hilo_. Este campo se copia en `rate_limit_per_user` en el hilo al momento de creación.
* El campo `available_tags` puede establecerse al crear o actualizar un canal, lo que determina qué etiquetas pueden establecerse en hilos individuales dentro del campo `applied_tags` del hilo.

Todos los campos para canales, incluyendo canales solo-hilos, se pueden encontrar en el objeto Channel.

#### Campos de hilo de canal solo-hilos

Un hilo puede fijarse dentro de un canal solo-hilos, lo que se representa por la bandera `PINNED`. Un hilo que está fijado tendrá la bandera establecida, y archivar ese hilo desfijará la bandera. Un hilo fijado _no_ se auto-archivará.

Los campos `message_count` y `total_message_sent` en hilos en canales solo-hilos se incrementarán en eventos Message Create, y se decrementarán en eventos Message Delete y Message Delete Bulk. No habrá un evento Channel Update específico que te notifique de cambios a esos campos, en su lugar, deberías actualizar esos valores al recibir eventos correspondientes.

Todos los campos para hilos en canales solo-hilos se pueden encontrar en la documentación del recurso de canal.

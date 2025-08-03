---
icon: microphone-stand
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

# Instancias de escenario

A _stage instance_ holds information about a live stage. For more information on stages, see the channel object.

## Definitions

Below are some definitions related to stages.

* **Liveness:** A stage channel is considered _live_ when there is an associated stage instance. Conversely, a stage channel is _not live_ when there is no associated stage instance.
* **Speakers:** A participant of a stage channel is a _speaker_ when their voice stateis not `suppress`ed, and has no `request_to_speak_timestamp`.
* **Moderators**: A member of the guild is a _moderator_ of a stage channel if they have all of the following permissions:
  * `MANAGE_CHANNELS`
  * `MUTE_MEMBERS`
  * `MOVE_MEMBERS`
* **Topic**: This is the blurb that gets shown below the channel's name, among other places.
* **Public**: A stage instance is public when it has a `privacy_level` of `PUBLIC`. While a guild has a public stage instance:
  * Lurkers may join any stage channel with a public stage instance.
  * Users in the stage can have the stage show in their activities.
  * Invites to the stage channel will have the `stage_instance` field.

## Auto Closing

When a stage channel has no speakers for a certain period of time (on the order of minutes) the stage instance will be automatically deleted.

### Stage Instance Object

**Stage Instance Structure**

\| Field | Type | Description | | -------------------------------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------ | | id | snowflake | The ID of the stage instance | | guild\_id | snowflake | The guild ID of the associated stage channel | | channel\_id | snowflake | The ID of the associated stage channel | | topic | string | The topic of the stage instance (1-120 characters) | | privacy\_level | integer | The privacy level of the stage instance | | invite\_code | ?string | The invite code that can be used to join the stage channel, if the stage instance is public | | discoverable\_disabled **(deprecated)** | boolean | Whether or not stage discovery is disabled | | guild\_scheduled\_event\_id | ?snowflake | The ID of the scheduled event for this stage instance |

**Example Stage Instance**

\`json

```
```

````
```json

````

```json
{

```

```
```

```
```

```
```

"invite\_code": "xdMaxHJqp8" } \`

> 📋 HEADER: Create Stage Instance

Creates a new stage instance associated to a stage channel. Requires the user to be a moderator of the stage channel. Returns a [stage instance](instancias-de-escenario.md#stage-instance-object) object. Fires a Stage Instance Create and optionally an Invite Create and Guild Scheduled Event Update Gateway event.

**JSON Params**

\| Field | Type | Description | | ----------------------------- | --------- | ---------------------------------------------------------------------------------------------------------------------- | | channel\_id | snowflake | The ID of the stage channel | | topic | string | The topic of the stage instance (1-120 characters) | | privacy\_level? | integer | The privacy level of the stage instance (default `GUILD_ONLY`) | | guild\_scheduled\_event\_id? ^1^ | snowflake | The ID of the scheduled event for this stage instance | | send\_start\_notification? ^2^ | boolean | Notify @everyone that a stage instance has started (default false) |

^1^ Creating a stage instance for a scheduled event will set the scheduled event's `status` to `ACTIVE`.

^2^ The stage moderator must have the `MENTION_EVERYONE` permission for this notification to be sent.

## Endpoints

> 📋 HEADER: Get Stage Instance

Gets the stage instance associated with the stage channel, if it exists. Returns a [stage instance](instancias-de-escenario.md#stage-instance-object) object.

> 📋 HEADER: Modify Stage Instance

Updates fields of an existing stage instance. Requires the user to be a moderator of the stage channel. Returns the updated [stage instance](instancias-de-escenario.md#stage-instance-object) object on success. Fires a Stage Instance Update Gateway event.

**JSON Params**

\| Field | Type | Description | | -------------- | ------- | ------------------------------------------------------------------------- | | topic? | string | The topic of the stage instance (1-120 characters) | | privacy\_level? | integer | The privacy level of the stage instance |

> 📋 HEADER: Delete Stage Instance

Deletes the stage instance and associated invite. Requires the user to be a moderator of the stage channel. Returns a 204 empty response on success. Fires a Stage Instance Delete and optionally an Invite Delete and Guild Scheduled Event Update Gateway event.

> ⚠️ ALERTA:

Deleting a stage instance will automatically delete the associated invite and set the associated scheduled event's `status` to `COMPLETED`.

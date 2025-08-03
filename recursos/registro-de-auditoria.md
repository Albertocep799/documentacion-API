---
icon: rectangle-history
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

# Registro de auditoría

### [Audit Log](registro-de-auditoria.md#audit-log) <a href="#audit-log" id="audit-log"></a>

When an administrative action is performed in a guild, an entry is added to its audit log. Viewing audit logs requires the permission and can be fetched using the [Get Guild Audit Log](registro-de-auditoria.md#get-guild-audit-log) endpoint. All audit log entries are stored for 45 days.

#### [Audit Reason](registro-de-auditoria.md#audit-reason) <a href="#audit-reason" id="audit-reason"></a>

When performing an eligible action using the API, users can pass an header to indicate why the action was taken. More information is in the [audit log entry](registro-de-auditoria.md#audit-log-entry-object) section.

#### [Audit Log Entry Object](registro-de-auditoria.md#audit-log-entry-object) <a href="#audit-log-entry-object" id="audit-log-entry-object"></a>

Each audit log entry represents a single administrative action (or [event](registro-de-auditoria.md#audit-log-action-type)), indicated by . Most entries contain one to many changes in the array that affected an entity in Discord—whether that's a user, channel, guild, emoji, or something else.

The information (and structure) of an entry's changes will be different depending on its type. For example, in events there is only one change: a member is either added or removed from a specific role. However, in events there are many changes, including (but not limited to) the channel's name, type, and permission overwrites added. More details are in the [change object](registro-de-auditoria.md#audit-log-change-object) section.

Users can specify why an administrative action is being taken by passing an request header, which will be stored as the audit log entry's field. The header supports up to 512 URL-encoded UTF-8 characters. Reasons are visible in the client and when fetching audit log entries with the API.

[**Audit Log Entry Structure**](registro-de-auditoria.md#audit-log-entry-structure)

| Field        | Type                                                                                   | Description                                                                        |
| ------------ | -------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| target\_id   | ?snowflake                                                                             | ID of the affected entity (webhook, user, role, etc.)                              |
| changes?     | array\[[audit log change](registro-de-auditoria.md#audit-log-change-object) object]    | Changes made to the \`target\_id\`\`                                               |
| user\_id     | ?snowflake                                                                             | The user who made the changes                                                      |
| id           | snowflake                                                                              | The ID of the entry                                                                |
| action\_type | integer                                                                                | The [type of action](registro-de-auditoria.md#audit-log-action-type) that occurred |
| options?     | [optional audit entry info](registro-de-auditoria.md#optional-audit-entry-info) object | Additional info for certain action types                                           |
| reason?      | string                                                                                 | The reason for the change (max 512 characters)                                     |

[**Audit Log Action Type**](registro-de-auditoria.md#audit-log-action-type)

The table below lists audit log events and values (the field) that you may receive.

The **Object Changed** column notes which object's values may be included in the entry. Though there are exceptions, possible keys in the array typically correspond to the object's fields. The descriptions and types for those fields can be found in the linked documentation for the object.

If no object is noted, there won't be a array in the entry, though other fields like the still exist and many have fields in the [array](registro-de-auditoria.md#optional-audit-entry-info).

| Value   | Name                                            | Description                                               | Object Changed                                                                                                                                                           |
| ------- | ----------------------------------------------- | --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1       | GUILD\_UPDATE                                   | Guild settings were updated                               | [Guild](https://docs.discord.food/resources/guild#guild-object)                                                                                                          |
| 10      | CHANNEL\_CREATE                                 | Channel was created                                       | [Channel](https://docs.discord.food/resources/channel#channel-object)                                                                                                    |
| 11      | CHANNEL\_UPDATE                                 | Channel settings were updated                             | [Channel](https://docs.discord.food/resources/channel#channel-object)                                                                                                    |
| 12      | CHANNEL\_DELETE                                 | Channel was deleted                                       | [Channel](https://docs.discord.food/resources/channel#channel-object)                                                                                                    |
| 13      | CHANNEL\_OVERWRITE\_CREATE                      | Permission overwrite was added to a channel               | [Channel Overwrite](https://docs.discord.food/resources/channel#permission-overwrite-object)                                                                             |
| 14      | CHANNEL\_OVERWRITE\_UPDATE                      | Permission overwrite was updated for a channel            | [Channel Overwrite](https://docs.discord.food/resources/channel#permission-overwrite-object)                                                                             |
| 15      | CHANNEL\_OVERWRITE\_DELETE                      | Permission overwrite was deleted from a channel           | [Channel Overwrite](https://docs.discord.food/resources/channel#permission-overwrite-object)                                                                             |
| 20      | MEMBER\_KICK                                    | Member was removed from guild                             |                                                                                                                                                                          |
| 21      | MEMBER\_PRUNE                                   | Members were pruned from guild                            |                                                                                                                                                                          |
| 22      | MEMBER\_BAN\_ADD                                | Member was banned from guild                              |                                                                                                                                                                          |
| 23      | MEMBER\_BAN\_REMOVE                             | Member was unbanned from guild                            |                                                                                                                                                                          |
| 24      | MEMBER\_UPDATE                                  | Member was updated in guild                               | [Member](https://docs.discord.food/resources/guild#guild-member-object)                                                                                                  |
| 25      | MEMBER\_ROLE\_UPDATE                            | Member was added or removed from a role                   | [Partial Role](registro-de-auditoria.md#partial-role-object) <sup>1</sup>                                                                                                |
| 26      | MEMBER\_MOVE                                    | Member was moved to a different voice channel             |                                                                                                                                                                          |
| 27      | MEMBER\_DISCONNECT                              | Member was disconnected from a voice channel              |                                                                                                                                                                          |
| 28      | BOT\_ADD                                        | Bot user was added to guild                               |                                                                                                                                                                          |
| 30      | ROLE\_CREATE                                    | Role was created                                          | [Role](https://docs.discord.food/resources/guild#role-object)                                                                                                            |
| 31      | ROLE\_UPDATE                                    | Role was edited                                           | [Role](https://docs.discord.food/resources/guild#role-object)                                                                                                            |
| 32      | ROLE\_DELETE                                    | Role was deleted                                          | [Role](https://docs.discord.food/resources/guild#role-object)                                                                                                            |
| 40      | INVITE\_CREATE                                  | Guild invite was created                                  | [Invite](https://docs.discord.food/resources/invite#invite-object) and [Invite Metadata](https://docs.discord.food/resources/invite#invite-metadata-object) <sup>1</sup> |
| 41      | INVITE\_UPDATE                                  | Guild invite was updated                                  | [Invite](https://docs.discord.food/resources/invite#invite-object) and [Invite Metadata](https://docs.discord.food/resources/invite#invite-metadata-object) <sup>1</sup> |
| 42      | INVITE\_DELETE                                  | Guild invite was deleted                                  | [Invite](https://docs.discord.food/resources/invite#invite-object) and [Invite Metadata](https://docs.discord.food/resources/invite#invite-metadata-object) <sup>1</sup> |
| 50      | WEBHOOK\_CREATE                                 | Webhook was created                                       | [Webhook](https://docs.discord.food/resources/webhook#webhook-object) <sup>1</sup>                                                                                       |
| 51      | WEBHOOK\_UPDATE                                 | Webhook properties or channel were updated                | [Webhook](https://docs.discord.food/resources/webhook#webhook-object) <sup>1</sup>                                                                                       |
| 52      | WEBHOOK\_DELETE                                 | Webhook was deleted                                       | [Webhook](https://docs.discord.food/resources/webhook#webhook-object) <sup>1</sup>                                                                                       |
| 60      | EMOJI\_CREATE                                   | Emoji was created                                         | [Emoji](https://docs.discord.food/resources/emoji#emoji-object)                                                                                                          |
| 61      | EMOJI\_UPDATE                                   | Emoji name was updated                                    | [Emoji](https://docs.discord.food/resources/emoji#emoji-object)                                                                                                          |
| 62      | EMOJI\_DELETE                                   | Emoji was deleted                                         | [Emoji](https://docs.discord.food/resources/emoji#emoji-object)                                                                                                          |
| 72      | MESSAGE\_DELETE                                 | Single message was deleted                                |                                                                                                                                                                          |
| 73      | MESSAGE\_BULK\_DELETE                           | Multiple messages were deleted                            |                                                                                                                                                                          |
| 74      | MESSAGE\_PIN                                    | Message was pinned to a channel                           |                                                                                                                                                                          |
| 75      | MESSAGE\_UNPIN                                  | Message was unpinned from a channel                       |                                                                                                                                                                          |
| 80      | INTEGRATION\_CREATE                             | Integration was added to guild                            | [Integration](https://docs.discord.food/resources/integration#integration-object)                                                                                        |
| 81      | INTEGRATION\_UPDATE                             | Integration was updated (e.g. its scopes were updated)    | [Integration](https://docs.discord.food/resources/integration#integration-object)                                                                                        |
| 82      | INTEGRATION\_DELETE                             | Integration was removed from guild                        | [Integration](https://docs.discord.food/resources/integration#integration-object)                                                                                        |
| 83      | STAGE\_INSTANCE\_CREATE                         | Stage instance was created (stage channel becomes live)   | [Stage Instance](https://docs.discord.food/resources/stage-instance#stage-instance-object)                                                                               |
| 84      | STAGE\_INSTANCE\_UPDATE                         | Stage instance details were updated                       | [Stage Instance](https://docs.discord.food/resources/stage-instance#stage-instance-object)                                                                               |
| 85      | STAGE\_INSTANCE\_DELETE                         | Stage instance was deleted (stage channel no longer live) | [Stage Instance](https://docs.discord.food/resources/stage-instance#stage-instance-object)                                                                               |
| 90      | STICKER\_CREATE                                 | Sticker was created                                       | [Sticker](https://docs.discord.food/resources/sticker#sticker-object)                                                                                                    |
| 91      | STICKER\_UPDATE                                 | Sticker details were updated                              | [Sticker](https://docs.discord.food/resources/sticker#sticker-object)                                                                                                    |
| 92      | STICKER\_DELETE                                 | Sticker was deleted                                       | [Sticker](https://docs.discord.food/resources/sticker#sticker-object)                                                                                                    |
| 100     | GUILD\_SCHEDULED\_EVENT\_CREATE                 | Event was created                                         | [Guild Scheduled Event](https://docs.discord.food/resources/guild-scheduled-event#guild-scheduled-event-object)                                                          |
| 101     | GUILD\_SCHEDULED\_EVENT\_UPDATE                 | Event was updated                                         | [Guild Scheduled Event](https://docs.discord.food/resources/guild-scheduled-event#guild-scheduled-event-object)                                                          |
| 102     | GUILD\_SCHEDULED\_EVENT\_DELETE                 | Event was cancelled                                       | [Guild Scheduled Event](https://docs.discord.food/resources/guild-scheduled-event#guild-scheduled-event-object)                                                          |
| 110     | THREAD\_CREATE                                  | Thread was created in a channel                           | [Thread](https://docs.discord.food/resources/channel#thread-metadata-object)                                                                                             |
| 111     | THREAD\_UPDATE                                  | Thread was updated                                        | [Thread](https://docs.discord.food/resources/channel#thread-metadata-object)                                                                                             |
| 112     | THREAD\_DELETE                                  | Thread was deleted                                        | [Thread](https://docs.discord.food/resources/channel#thread-metadata-object)                                                                                             |
| 121     | APPLICATION\_COMMAND\_PERMISSION\_UPDATE        | Permissions were updated for a command                    | [Application Command Permission](https://docs.discord.food/interactions/application-commands#application-command-permissions-object) <sup>1</sup>                        |
| 130     | SOUNDBOARD\_SOUND\_CREATE                       | Soundboard sound was created                              | [Soundboard Sound](https://docs.discord.food/resources/soundboard#soundboard-sound-object)                                                                               |
| 131     | SOUNDBOARD\_SOUND\_UPDATE                       | Soundboard sound was updated                              | [Soundboard Sound](https://docs.discord.food/resources/soundboard#soundboard-sound-object)                                                                               |
| 132     | SOUNDBOARD\_SOUND\_DELETE                       | Soundboard sound was deleted                              | [Soundboard Sound](https://docs.discord.food/resources/soundboard#soundboard-sound-object)                                                                               |
| 140     | AUTO\_MODERATION\_RULE\_CREATE                  | AutoMod rule was created                                  | [AutoMod Rule](https://docs.discord.food/resources/auto-moderation#automod-rule-object)                                                                                  |
| 141     | AUTO\_MODERATION\_RULE\_UPDATE                  | AutoMod rule was updated                                  | [AutoMod Rule](https://docs.discord.food/resources/auto-moderation#automod-rule-object)                                                                                  |
| 142     | AUTO\_MODERATION\_RULE\_DELETE                  | AutoMod rule was deleted                                  | [AutoMod Rule](https://docs.discord.food/resources/auto-moderation#automod-rule-object)                                                                                  |
| 143     | AUTO\_MODERATION\_BLOCK\_MESSAGE                | Message was blocked by AutoMod                            |                                                                                                                                                                          |
| 144     | AUTO\_MODERATION\_FLAG\_TO\_CHANNEL             | Message was flagged by AutoMod                            |                                                                                                                                                                          |
| 145     | AUTO\_MODERATION\_USER\_COMMUNICATION\_DISABLED | Member was timed out by AutoMod                           |                                                                                                                                                                          |
| 146     | AUTO\_MODERATION\_QUARANTINE\_USER              | Member was quarantined by AutoMod                         |                                                                                                                                                                          |
| 150     | CREATOR\_MONETIZATION\_REQUEST\_CREATED         | Creator monetization request was created                  |                                                                                                                                                                          |
| 151     | CREATOR\_MONETIZATION\_TERMS\_ACCEPTED          | Creator monetization terms were accepted                  |                                                                                                                                                                          |
| 163     | ONBOARDING\_PROMPT\_CREATE                      | Onboarding prompt was created                             | [Onboarding Prompt](https://docs.discord.food/resources/guild#onboarding-prompt-structure)                                                                               |
| 164     | ONBOARDING\_PROMPT\_UPDATE                      | Onboarding prompt was updated                             | [Onboarding Prompt](https://docs.discord.food/resources/guild#onboarding-prompt-structure)                                                                               |
| 165     | ONBOARDING\_PROMPT\_DELETE                      | Onboarding prompt was deleted                             | [Onboarding Prompt](https://docs.discord.food/resources/guild#onboarding-prompt-structure)                                                                               |
| 166     | ONBOARDING\_CREATE                              | Onboarding was initialized                                | [Onboarding](https://docs.discord.food/resources/guild#onboarding-object)                                                                                                |
| 167     | ONBOARDING\_UPDATE                              | Onboarding was updated                                    | [Onboarding](https://docs.discord.food/resources/guild#onboarding-object)                                                                                                |
| ~~171~~ | ~~GUILD\_HOME\_FEATURE\_ITEM~~                  | ~~Item was featured in guild home~~                       |                                                                                                                                                                          |
| ~~172~~ | ~~GUILD\_HOME\_REMOVE\_ITEM~~                   | ~~Item was removed from guild home~~                      |                                                                                                                                                                          |
| ~~180~~ | ~~HARMFUL\_LINKS\_BLOCKED\_MESSAGE~~            | ~~Message blocked by harmful links filter~~               |                                                                                                                                                                          |
| 190     | HOME\_SETTINGS\_CREATE                          | New member welcome was initialized                        | [New Member Welcome](https://docs.discord.food/resources/guild#new-member-welcome-object)                                                                                |
| 191     | HOME\_SETTINGS\_UPDATE                          | New member welcome was updated                            | [New Member Welcome](https://docs.discord.food/resources/guild#new-member-welcome-object)                                                                                |
| 192     | VOICE\_CHANNEL\_STATUS\_CREATE                  | Voice channel status was updated                          | [Channel](https://docs.discord.food/resources/channel#channel-object)                                                                                                    |
| 193     | VOICE\_CHANNEL\_STATUS\_DELETE                  | Voice channel status was deleted                          | [Channel](https://docs.discord.food/resources/channel#channel-object)                                                                                                    |
| ~~194~~ | ~~CLYDE\_AI\_PROFILE\_UPDATE~~                  | ~~Clyde AI profile was updated~~                          |                                                                                                                                                                          |
| 200     | GUILD\_SCHEDULED\_EVENT\_EXCEPTION\_CREATE      | Exception was created for a guild scheduled event         | [Guild Scheduled Event Exception](https://docs.discord.food/resources/guild-scheduled-event#guild-scheduled-event-exception-object)                                      |
| 201     | GUILD\_SCHEDULED\_EVENT\_EXCEPTION\_UPDATE      | Exception was updated for a guild scheduled event         | [Guild Scheduled Event Exception](https://docs.discord.food/resources/guild-scheduled-event#guild-scheduled-event-exception-object)                                      |
| 202     | GUILD\_SCHEDULED\_EVENT\_EXCEPTION\_DELETE      | Exception was deleted for a guild scheduled event         | [Guild Scheduled Event Exception](https://docs.discord.food/resources/guild-scheduled-event#guild-scheduled-event-exception-object)                                      |
| 210     | GUILD\_MEMBER\_VERIFICATION\_UPDATE             | Member verification settings were updated                 | [Member Verification](https://docs.discord.food/resources/guild#member-verification-object) <sup>1</sup>                                                                 |
| 211     | GUILD\_PROFILE\_UPDATE                          | Guild profile was updated                                 | [Guild Profile](https://docs.discord.food/resources/discovery#guild-profile-object) <sup>1</sup>                                                                         |

<sup>1</sup> Object has exception(s) to available keys. See the [exceptions](registro-de-auditoria.md#audit-log-change-exceptions) section below for details.

[**Optional Audit Entry Info**](registro-de-auditoria.md#optional-audit-entry-info)

| Field                                 | Type      | Description                                                                                                                         | Action Type                                                                                                                                          |
| ------------------------------------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| application\_id                       | snowflake | The ID of the application whose permissions were targeted                                                                           | `APPLICATION_COMMAND_PERMISSION_UPDATE`                                                                                                              |
| auto\_moderation\_rule\_name          | string    | The name of the AutoMod rule that was triggered                                                                                     | `AUTO_MODERATION_BLOCK_MESSAGE`, `AUTO_MODERATION_FLAG_TO_CHANNEL`, `AUTO_MODERATION_USER_COMMUNICATION_DISABLED`, `AUTO_MODERATION_QUARANTINE_USER` |
| auto\_moderation\_rule\_trigger\_type | string    | The [trigger type of the AutoMod rule](https://docs.discord.food/resources/auto-moderation#automod-trigger-type) that was triggered | `AUTO_MODERATION_BLOCK_MESSAGE`, `AUTO_MODERATION_FLAG_TO_CHANNEL`, `AUTO_MODERATION_USER_COMMUNICATION_DISABLED`, `AUTO_MODERATION_QUARANTINE_USER` |
| channel\_id                           | snowflake | The channel in which the entities were targeted                                                                                     | `MEMBER_MOVE`, `MESSAGE_PIN`, `MESSAGE_UNPIN`, `MESSAGE_DELETE`, `STAGE_INSTANCE_CREATE`, `STAGE_INSTANCE_UPDATE`, `STAGE_INSTANCE_DELETE`           |
| count?                                | string    | Number of entities that were targeted                                                                                               | `MESSAGE_DELETE`, `MESSAGE_BULK_DELETE`, `MEMBER_DISCONNECT`, `MEMBER_MOVE`                                                                          |
| delete\_member\_days?                 | string    | Number of days after which inactive members were kicked                                                                             | `MEMBER_PRUNE`                                                                                                                                       |
| event\_exception\_id                  | snowflake | The ID of the guild scheduled event exception that was targeted                                                                     | `GUILD_SCHEDULED_EVENT_EXCEPTION_CREATE`, `GUILD_SCHEDULED_EVENT_EXCEPTION_UPDATE`, `GUILD_SCHEDULED_EVENT_EXCEPTION_DELETE`                         |
| id                                    | snowflake | The ID of the overwritten entity                                                                                                    | `CHANNEL_OVERWRITE_CREATE`, `CHANNEL_OVERWRITE_UPDATE`, `CHANNEL_OVERWRITE_DELETE`                                                                   |
| integration\_type?                    | string    | The [type of integration](https://docs.discord.food/resources/integration#integration-type) which performed the action              | `MEMBER_KICK`, `MEMBER_ROLE_UPDATE`                                                                                                                  |
| members\_removed?                     | string    | Number of members removed by the prune                                                                                              | `MEMBER_PRUNE`                                                                                                                                       |
| message\_id                           | snowflake | The ID of the message that was targeted                                                                                             | `MESSAGE_PIN`, `MESSAGE_UNPIN`                                                                                                                       |
| role\_name?                           | string    | The name of the role (only present if type is "0")                                                                                  | `CHANNEL_OVERWRITE_CREATE`, `CHANNEL_OVERWRITE_UPDATE`, `CHANNEL_OVERWRITE_DELETE`                                                                   |
| status                                | string    | The new status of the voice channel                                                                                                 | `VOICE_CHANNEL_STATUS_UPDATE`                                                                                                                        |
| type <sup>1</sup>                     | string    | The [type of overwritten entity](https://docs.discord.food/resources/channel#permission-overwrite-type)                             | `CHANNEL_OVERWRITE_CREATE`, `CHANNEL_OVERWRITE_UPDATE`, `CHANNEL_OVERWRITE_DELETE`                                                                   |

<sup>1</sup> Due to technical limitations, this field is always serialized as a string, not an integer.

#### [Audit Log Change Object](registro-de-auditoria.md#audit-log-change-object) <a href="#audit-log-change-object" id="audit-log-change-object"></a>

Many audit log events include a array in their [entry object](registro-de-auditoria.md#audit-log-entry-structure). The [structure for the individual changes](registro-de-auditoria.md#audit-log-change-structure) varies based on the event type and its changed objects, so apps shouldn't depend on a single pattern of handling audit log events.

[**Audit Log Change Structure**](registro-de-auditoria.md#audit-log-change-structure)

Some events don't follow the same pattern as other audit log events. Details about these exceptions are explained in [the next section](registro-de-auditoria.md#audit-log-change-exceptions).

| Field       | Type                                | Description                                                                                               |
| ----------- | ----------------------------------- | --------------------------------------------------------------------------------------------------------- |
| new\_value? | mixed (matches object field's type) | New value of the key                                                                                      |
| old\_value? | mixed (matches object field's type) | Old value of the key                                                                                      |
| key         | string                              | Name of the changed entity, with a few [exceptions](registro-de-auditoria.md#audit-log-change-exceptions) |

[**Audit Log Change Exceptions**](registro-de-auditoria.md#audit-log-change-exceptions)

For most objects, the change keys may be any field on the changed object. The following table details the exceptions to this pattern.

| Object Changed                                                                                                                                              | Change Key Exceptions                                                                                                                             | Change Object Exceptions                                                                                                                                                                                                                                    |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [AutoMod Rule](https://docs.discord.food/resources/auto-moderation#automod-rule-object)                                                                     | `$add_keyword_filter`, `$remove_keyword_filter`, `$add_regex_patterns`, `$remove_regex_patterns`, `$add_allow_list`, `$remove_allow_list` as keys | `new_value` and `old_value` are arrays of strings representing the keywords, regex patterns, or allow list items that were added or removed                                                                                                                 |
| [Application Command Permission](https://docs.discord.food/interactions/application-commands#application-command-permissions-structure)                     | A snowflake is used as the key                                                                                                                    | The `changes` array contains objects with a `key` field representing the entity whose command was affected (role, channel, or user ID), a previous permissions object (with an `old_value` key), and an updated permissions object (with a `new_value` key) |
| [Guild Member](https://docs.discord.food/resources/guild#guild-member-object)                                                                               | Additional `bypasses_verification` key (instead of object's `flags`)                                                                              | `new_value` and `old_value` are booleans representing whether the member bypasses verification                                                                                                                                                              |
| [Invite](https://docs.discord.food/resources/invite#invite-object) and [Invite Metadata](https://docs.discord.food/resources/invite#invite-metadata-object) | Additional `channel_id` and `inviter_id` keys (instead of object's `channel.id` and `inviter.id`)                                                 |                                                                                                                                                                                                                                                             |
| [Partial Role](registro-de-auditoria.md#partial-role-object)                                                                                                | `$add` and `$remove` as keys                                                                                                                      | `new_value` is an array of objects that contain the role `id` and `name`                                                                                                                                                                                    |
| [Member Verification](https://docs.discord.food/resources/guild#member-verification-object)                                                                 | `verification_enabled` and `manual_approval_enabled` as keys                                                                                      | `new_value` and `old_value` are booleans representing whether verification or manual approval is enabled                                                                                                                                                    |
| [Guild Profile](https://docs.discord.food/resources/discovery#guild-profile-object)                                                                         | Additional `server_tag` key (instead of object's `tag`)                                                                                           |                                                                                                                                                                                                                                                             |

#### [Partial Integration Object](registro-de-auditoria.md#partial-integration-object) <a href="#partial-integration-object" id="partial-integration-object"></a>

[**Partial Integration Structure**](registro-de-auditoria.md#partial-integration-structure)

| Field            | Type                                                                                            | Description                                                                                 |
| ---------------- | ----------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| id               | snowflake                                                                                       | The ID of the integration                                                                   |
| name             | string                                                                                          | The name of the integration                                                                 |
| type             | string                                                                                          | The [type of integration](https://docs.discord.food/resources/integration#integration-type) |
| account          | [account](https://docs.discord.food/resources/integration#integration-account-structure) object | The integration's account information                                                       |
| application\_id? | snowflake                                                                                       | The OAuth2 application for Discord integrations                                             |

[**Example Partial Integration**](registro-de-auditoria.md#example-partial-integration)

```
{  "id": "1029376264039039006",  "type": "discord",  "name": "Good University",  "account": {    "id": "971811349262917662",    "name": "Good University"  },  "application_id": "971811349262917662"}
```

#### [Partial Role Object](registro-de-auditoria.md#partial-role-object) <a href="#partial-role-object" id="partial-role-object"></a>

[**Partial Role Structure**](registro-de-auditoria.md#partial-role-structure)

| Field | Type      | Description          |
| ----- | --------- | -------------------- |
| id    | snowflake | The ID of the role   |
| name  | string    | The name of the role |

[**Example Partial Role**](registro-de-auditoria.md#example-partial-role)

```
{  "name": "I am a role",  "id": "584120723283509258"}
```

### [Endpoints](registro-de-auditoria.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get Guild Audit Log](registro-de-auditoria.md#get-guild-audit-log) <a href="#get-guild-audit-log" id="get-guild-audit-log"></a>

`GET/guilds/{guild.id}/audit-logs`

Returns the audit log for the guild. Requires the permission.

[**Query String Params**](registro-de-auditoria.md#query-string-params)

| Field         | Type      | Description                                                                                |
| ------------- | --------- | ------------------------------------------------------------------------------------------ |
| before?       | snowflake | Get entries before this entry ID                                                           |
| after?        | snowflake | Get entries after this entry ID                                                            |
| limit?        | integer   | Max number of entries to return (1-100, default 50)                                        |
| user\_id?     | snowflake | Get actions made by a specific user                                                        |
| target\_id?   | snowflake | Get actions affecting a specific entity                                                    |
| action\_type? | integer   | The [type of audit log event](registro-de-auditoria.md#audit-log-action-type) to filter by |

[**Response Body**](registro-de-auditoria.md#response-body)

| Field                    | Type                                                                                                                           | Description                                        |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------- |
| audit\_log\_entries      | array\[[audit log entry](registro-de-auditoria.md#audit-log-entry-object) object]                                              | Audit log entries                                  |
| application\_commands    | array\[[application command](https://docs.discord.food/interactions/application-commands#application-command-object) object]   | Application commands referenced in the audit log   |
| auto\_moderation\_rules  | array\[[automod rule](https://docs.discord.food/resources/auto-moderation#automod-rule-object) object]                         | AutoMod rules referenced in the audit log          |
| guild\_scheduled\_events | array\[[guild scheduled event](https://docs.discord.food/resources/guild-scheduled-event#guild-scheduled-event-object) object] | Guild scheduled events referenced in the audit log |
| integrations             | array\[partial [integration](registro-de-auditoria.md#partial-integration-object) object]                                      | Partial integrations referenced in the audit log   |
| threads                  | array\[[channel](https://docs.discord.food/resources/channel#channel-object) object]                                           | Threads referenced in the audit log                |
| users                    | array\[partial [user](https://docs.discord.food/resources/user#user-object) object]                                            | Users referenced in the audit log                  |
| webhooks                 | array\[[webhook](https://docs.discord.food/resources/webhook#webhook-object) object]                                           | Webhooks referenced in the audit log               |

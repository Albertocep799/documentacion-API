---
icon: head-side-gear
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

# Ajustes de usuario

User settings are options that a user can configure to change the behavior of their account or client. User guild settings are used to control notifications and other customization options on a per-guild basis.

### User Settings Object

Contains general user settings.

> ⚠️ ALERTA:

This is deprecated in favor of the protobuf user settings implementation.

**User Settings Structure**

\| Field | Type | Description | | ---------------------------------------------- | ------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------- | | activity\_restricted\_guild\_ids | array\[snowflake] | The IDs of guilds your activity presence will be hidden in | | activity\_joining\_restricted\_guild\_ids | array\[snowflake] | The IDs of guilds that will not be able to join your current activity | | afk\_timeout | integer | Duration (in seconds) the user needs to be inactive until clients update their AFK state | | allow\_accessibility\_detection | boolean | Whether to allow Discord to track screen reader usage | | allow\_activity\_party\_privacy\_friends | boolean | Whether to allow friends to join your activity without sending a request | | allow\_activity\_party\_privacy\_voice\_channel ^1^ | boolean | Whether to allow people in the same voice channel as you to join your activity without sending a request | | animate\_emoji | boolean | Whether to play animated emojis in chat | | animate\_stickers | integer | [When to animate stickers in chat](ajustes-de-usuario.md#sticker-animation-option) | | contact\_sync\_enabled | boolean | Whether to enable contact sync on Discord mobile | | convert\_emoticons | boolean | Whether to convert emoticons into emojis (e.g. `:)` -> `🙂`) | | custom\_status | ?[custom status](ajustes-de-usuario.md#custom-status-structure) object | The overall custom status of the user, used to sync presence across clients | | default\_guilds\_restricted | boolean | Whether to automatically disable DMs between you and members of new guilds you join | | detect\_platform\_accounts | boolean | Whether to automatically detect accounts from services like Steam and Blizzard when opening the Discord client | | developer\_mode | boolean | Whether to enable developer mode in-client | | disable\_games\_tab | boolean | Whether to disable the showing of the Games tab | | enable\_tts\_command | boolean | Whether to allow TTS messages to be sent and played | | explicit\_content\_filter | integer | The [explicit content filter](ajustes-de-usuario.md#explicit-content-filter) for explicit content in all messages | | friend\_discovery\_flags | integer | The user's [friend discovery flags](ajustes-de-usuario.md#friend-discovery-flags) | | friend\_source\_flags | [friend source flags](ajustes-de-usuario.md#friend-source-flags-structure) object | The user's friend source flags (default all false) | | gif\_auto\_play | boolean | Whether GIFs are automatically played when the Discord client is in focus | | guild\_folders | array\[[guild folder](ajustes-de-usuario.md#guild-folder-structure) object] | The guild folders | | inline\_attachment\_media | boolean | Whether to display attachments when they are uploaded in chat | | inline\_embed\_media | boolean | Whether to display videos and images from links posted in chat | | locale | string | The language option chosen by the user | | message\_display\_compact | boolean | Whether to use the compact Discord display mode | | native\_phone\_integration\_enabled | boolean | Whether to enable the new Discord mobile phone number friend requesting feature | | passwordless **(deprecated)** | boolean | Whether to enable passwordless login | | render\_embeds | boolean | Whether to render message embeds | | render\_reactions | boolean | Whether to render message reactions | | restricted\_guilds | array\[snowflake] | The IDs of guilds that you will not receive DMs from | | show\_current\_game | boolean | Whether to display the currently active game in user presence | | slayer\_sdk\_receive\_dms\_in\_game | integer | Setting for receiving in-game DMs via the social layer SDK | | status | string | The overall status of the user, used to sync presence across clients | | stream\_notifications\_enabled | boolean | Whether to receive stream notifications for friends | | theme | string | The user's [client theme](ajustes-de-usuario.md#theme) | | timezone\_offset | integer | The timezone offset from UTC to use (in minutes) | | view\_nsfw\_commands | boolean | Whether NSFW application commands are shown in DMs | | view\_nsfw\_guilds | boolean | Whether NSFW guilds are shown on iOS |

^1^ Does not apply to community guilds.

**Sticker Animation Option**

\| Value | Name | Description | | ----- | ---------------------- | ------------------------------ | | 0 | ALWAYS\_ANIMATE | Always animate stickers | | 1 | ANIMATE\_ON\_INTERACTION | Animate sticker on interaction | | 2 | NEVER\_ANIMATE | Never animate stickers |

**Custom Status Structure**

\| Field | Type | Description | | -------------- | ------------------ | ---------------------------------- | | text | ?string | The custom status text (max 128) | | emoji\_id ^1^ | ?snowflake | The ID of a guild's custom emoji | | emoji\_name ^1^ | ?string | The unicode character of the emoji | | expires\_at | ?ISO8601 timestamp | When the custom status will expire |

^1^ At most one of `emoji_id` and `emoji_name` may be set to a non-null value.

**Theme**

\| Value | Description | | -------- | -------------- | | dark | Dark theme | | light | Light theme | | darker | Darker theme | | midnight | Midnight theme |

**Explicit Content Filter**

Whose messages will be scanned for explicit content.

\| Value | Name | Description | | ----- | ------------ | ------------------------------------------------- | | 0 | DISABLED | Don't scan any direct messages | | 1 | NON\_FRIENDS | Scan all direct messages that aren't from friends | | 2 | ALL\_MESSAGES | Scan all direct messages from everyone |

**Friend Discovery Flags**

Determines how you get recommended friends.

\| Value | Name | Description | | ------ | ------------- | ----------------------------------------------------- | | 1 `<<`1 | FIND\_BY\_PHONE | Whether the current user can be found by phone number | | 1 `<<`2 | FIND\_BY\_EMAIL | Whether the current user can be found by email |

**Friend Source Flags Structure**

Determines who can add the user as a friend.

\| Field | Type | Description | | --------------- | ------- | --------------------------------------------------------------- | | all? | boolean | Whether everyone can add the user as a friend | | mutual\_friends? | boolean | Whether mutual friends can add the user as friend | | mutual\_guilds? | boolean | Whether members in the user's guilds can add the user as friend |

**Guild Folder Structure**

A collection of guilds.

> ⚠️ ALERTA:

If `id` is `null` and `guild_ids` is a single element array, this folder is not displayed and instead represents a single guild's position in the sidebar.

\| Field | Type | Description | | --------- | ---------------- | ---------------------------------------------------------------------------------------- | | color | ?integer | The color of the folder encoded as an integer representation of a hexadecimal color code | | guild\_ids | array\[snowflake] | The IDs of guilds this folder contains | | id | ?integer | The ID of the folder | | name | ?string | The name of the folder (default `', '.join(guild.name for guild in folder.guilds)`) |

### Consents Object

Contains the user's tracking feature consent status.

> ⚠️ ALERTA:

Disabling tracking features will result in reduced exposure to experiments and certain features such as affinities being disabled.

**Consents Structure**

This object is a map of [consent types](ajustes-de-usuario.md#consent-type) to their [status](ajustes-de-usuario.md#consent-status-structure).

**Consent Type**

\| Value | Description | | -------------------- | ---------------------------------------------------------------------------- | | personalization | Whether the user has consented to their data being used for personalization | | usage\_statistics ^1^ | Whether the user has consented to their data being used for usage statistics |

^1^ Only included when fetched from the [Get User Consents](ajustes-de-usuario.md#get-user-consents) endpoint.

**Consent Status Structure**

\| Field | Type | Description | | --------- | ------- | --------------------------------------------- | | consented | boolean | Whether the user has consented to the feature |

### Email Settings Object

Email communication preferences.

**Email Settings Structure**

\| Field | Type | Description | | ----------- | -------------------- | ---------------------------------------------------------------------------------- | | initialized | boolean | Whether the email settings have been initialized | | categories | map\[string, boolean] | The [email settings categories](ajustes-de-usuario.md#email-settings-category) and their enabled status |

**Email Settings Category**

\| Value | Description | | -------------------------- | ----------------------------------------------------------------- | | communication | Receive emails for missed calls and messages | | social | Receive emails for friend requests, friend suggestions, or events | | recommendations\_and\_events | Receive emails for recommended guilds and events | | tips | Receive emails for advice and tricks | | updates\_and\_announcements | Receive emails for updates and new features | | family\_center\_digest | Receive weekly emails for recent family activity |

### Notification Settings Object

User-wide notification settings.

**Notification Settings Structure**

\| Field | Type | Description | | ----- | ------- | --------------------------------------------------------------- | | flags | integer | The [notification settings flags](ajustes-de-usuario.md#notification-settings-flags) |

**Notification Settings Flags**

\| Value | Name | Description | | ------ | ----------------------- | ---------------------------------------------------------------------------------------------------------------------- | | 1 `<<`4 | USE\_NEW\_NOTIFICATIONS | Whether to separate unreads from the overall message notification level | | 1 `<<`5 | MENTION\_ON\_ALL\_MESSAGES | Whether to increment the mention count on all messages in channels with a message notification level of `ALL_MESSAGES` |

**Example Notification Settings**

\`json

```
```

````
```json

````

```json
{ "flags": 16 }
`

### Notification Settings Snapshot Object

A snapshot of the user's guild settings.

###### Notification Settings Snapshot Structure

| Field       | Type              | Description                         |
| ----------- | ----------------- | ----------------------------------- |
| id          | snowflake         | The ID of the snapshot              |
| label       | ?string           | The label of the snapshot           |
| recorded_at | ISO8601 timestamp | When the snapshot was recorded      |
| length      | integer           | The length of the snapshot in bytes |

###### Example Notification Settings Snapshot

`json

```

```
```

```json
```

```json
{

```

```
```

"length": 145750 } \`

### User Guild Settings Object

Guild-specific settings for the current user.

**User Guild Settings Structure**

\| Field | Type | Description | | --------------------- | ------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | | channel\_overrides | array\[[channel override](ajustes-de-usuario.md#channel-override-structure) object] | The overrides for channels | | flags | integer | The [user guild settings flags](ajustes-de-usuario.md#user-guild-settings-flags) | | guild\_id ^1^ | ?snowflake | The ID of the guild | | hide\_muted\_channels | boolean | Whether to hide muted channels from the UI | | message\_notifications | integer | The message notification level for the guild | | mobile\_push | boolean | Whether to send push notifications to mobile clients | | mute\_scheduled\_events | boolean | Whether new guild scheduled event notifications are muted | | muted | boolean | Whether the guild is muted | | mute\_config | ?[mute config](ajustes-de-usuario.md#mute-config-object) object | The mute metadata for the guild | | notify\_highlights | integer | The [highlight notification level](ajustes-de-usuario.md#highlight-level) for the guild | | suppress\_everyone | integer | Whether to suppress @everyone notifications | | suppress\_roles | integer | Whether to suppress role notifications | | version | integer | The version of guild settings |

^1^ A value of `null` is used to indicate these settings are for the user's private channel notifications.

**Partial User Guild Settings Structure**

Used when modifying user guild settings.

\| Field | Type | Description | | ---------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | | channel\_overrides? | map\[snowflake, [channel override](ajustes-de-usuario.md#channel-override-structure) object] | The channel overrides to modify | | flags? | integer | The [user guild settings flags](ajustes-de-usuario.md#user-guild-settings-flags) | | hide\_muted\_channels? | boolean | Whether to hide muted channels from the UI | | message\_notifications? | integer | The message notification level for the guild | | mobile\_push? | boolean | Whether to send push notifications to mobile clients | | mute\_scheduled\_events? | boolean | Whether new guild scheduled event notifications are muted | | muted? | boolean | Whether the guild is muted | | mute\_config? | ?[mute config](ajustes-de-usuario.md#mute-config-object) object | The mute metadata for the guild | | notify\_highlights? | integer | The [highlight notification level](ajustes-de-usuario.md#highlight-level) for the guild | | suppress\_everyone? | integer | Whether to suppress @everyone notifications | | suppress\_roles? | integer | Whether to suppress role notifications |

**Channel Override Structure**

\| Field | Type | Description | | --------------------- | ------------------------------------------ | --------------------------------------------------------------------------------------------- | | channel\_id | snowflake | The ID of the channel | | collapsed | boolean | Whether the category channel is collapsed | | flags? | integer | The [channel override's flags](ajustes-de-usuario.md#channel-override-flags) | | message\_notifications | integer | The message notification level for the channel | | muted | boolean | Whether the channel is muted | | mute\_config | ?[mute config](ajustes-de-usuario.md#mute-config-object) object | The mute metadata for the channel |

**Channel Override Flags**

\| Value | Name | Description | | ------- | ------------------------- | ----------------------------------------------------------------- | | 1 `<<`9 | UNREADS\_ONLY\_MENTIONS ^1^ | Channel is marked unread on mentions | | 1 `<<`10 | UNREADS\_ALL\_MESSAGES ^1^ | Channel is marked unread on new messages | | 1 `<<`11 | FAVORITED | Channel is favorited | | 1 `<<`12 | OPT\_IN\_ENABLED | Channel is shown in the UI | | 1 `<<`13 | NEW\_FORUM\_THREADS\_OFF | Thread-only channel is not marked unread when a thread is created | | 1 `<<`14 | NEW\_FORUM\_THREADS\_ON | Thread-only channel is marked unread when a thread is created |

^1^ When these flags are unset, unreads follow the [message\_notifications field](ajustes-de-usuario.md#channel-override-structure).

**User Guild Settings Flags**

\| Value | Name | Description | | ------- | ------------------------- | ----------------------------------------------- | | 1 `<<`11 | UNREADS\_ALL\_MESSAGES ^1^ | Guild is marked unread on new messages | | 1 `<<`12 | UNREADS\_ONLY\_MENTIONS ^1^ | Guild is marked unread on mentions | | 1 `<<`13 | OPT\_IN\_CHANNELS\_OFF | Whether to show all guild channels in the UI | | 1 `<<`14 | OPT\_IN\_CHANNELS\_ON | Whether to hide non-opted in channels in the UI |

^1^ When these flags are unset, unreads follow the [message\_notifications field](ajustes-de-usuario.md#user-guild-settings-object).

**Highlight Level**

\| Value | Name | Description | | ----- | -------- | --------------------------- | | 0 | DEFAULT | Default (same as `ENABLED`) | | 1 | DISABLED | Suppress highlights | | 2 | ENABLED | Don't suppress highlights |

**Example User Guild Settings**

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

```

"mute_config": {

```

"selected\_time\_window": -1 },

```

"channel_overrides": [
  {

```

```
```

```

"collapsed": true
}
],

```

"version": 2016 } \`

### Mute Config Object

The duration of a mute.

**Mute Config Structure**

\| Field | Type | Description | | --------------------- | ------------------ | ------------------------------------------------------- | | end\_time? | ?ISO8601 timestamp | Timestamp representing when the mute ends | | selected\_time\_window? | integer | Duration of the mute in seconds, or `-1` for indefinite |

### Audio Context Object

**Audio Context Structure**

\| Field | Type | Description | | ---------------- | ------- | ------------------------------------------- | | muted | boolean | Whether the audio is muted | | volume | float | Volume level of the user or stream (0-200) | | soundboard\_muted | boolean | Whether the soundboard is muted (user only) |

**Audio Context Type**

\| Value | Description | | ------ | -------------------- | | user | Voice audio context | | stream | Stream audio context |

## Endpoints

> 📋 HEADER: Get User Settings

Returns the requester's [user settings](ajustes-de-usuario.md#user-settings-object) object.

> 📋 HEADER: Modify User Settings

Modifies the requester's user settings. Returns a [user settings](ajustes-de-usuario.md#user-settings-object) object on success. Fires a User Settings Proto Update and User Settings Update Gateway event.

> ⚠️ ALERTA:

For OAuth2 requests, only the `status` and `custom_status` fields can be modified. Similarly, the response object will only contain these two fields.

**JSON Params**

\| Field | Type | Description | | -------------------------------------- | ------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------- | | activity\_joining\_restricted\_guild\_ids? | array\[snowflake] | The IDs of guilds that will not be able to join your current activity | | activity\_restricted\_guild\_ids? | array\[snowflake] | The IDs of guilds your activity presence will be hidden in | | afk\_timeout? | integer | Duration (in seconds) the user needs to be inactive until clients update their AFK state | | allow\_accessibility\_detection? | boolean | Whether to allow Discord to track screen reader usage | | animate\_emoji? | boolean | Whether to play animated emojis in chat | | animate\_stickers? | integer | [When to animate stickers in chat](ajustes-de-usuario.md#sticker-animation-option) | | contact\_sync\_enabled? | boolean | Whether to enable contact sync on Discord mobile | | convert\_emoticons? | boolean | Whether to convert emoticons into emojis (`:)` -> `🙂`) | | custom\_status? | ?[custom status](ajustes-de-usuario.md#custom-status-structure) object | The custom status of the user, used to sync presence across clients | | default\_guilds\_restricted? | boolean | Whether to automatically disable DMs between you and members of new guilds you join | | detect\_platform\_accounts? | boolean | Whether to automatically detect accounts from services like Steam and Blizzard when opening the Discord client | | developer\_mode? | boolean | Whether to enable developer mode in-client | | disable\_games\_tab? | boolean | Whether to disable the showing of the Games tab | | enable\_tts\_command? | boolean | Whether to allow TTS messages to be played/sent | | explicit\_content\_filter? | integer | The [explicit content filter](ajustes-de-usuario.md#explicit-content-filter) for explicit content in all messages | | friend\_discovery\_flags? | integer | The [friend discovery flags](ajustes-de-usuario.md#friend-discovery-flags) | | friend\_source\_flags? | [friend source flags](ajustes-de-usuario.md#friend-source-flags-structure) object | The friend source flags | | gif\_auto\_play? | boolean | Whether GIFs are automatically played when Discord client is in focus | | guild\_folders? | array\[[guild folder](ajustes-de-usuario.md#guild-folder-structure) object] | The guild folders | | inline\_attachment\_media? | boolean | Whether to display attachments when they are uploaded in chat | | inline\_embed\_media? | boolean | Whether to display videos and images from links posted in chat | | locale? | string | The language option chosen by the user | | message\_display\_compact? | boolean | Whether to use the compact Discord display mode | | native\_phone\_integration\_enabled? | boolean | Whether to enable the new Discord mobile phone number friend requesting feature | | passwordless? **(deprecated)** | boolean | Whether to enable passwordless login | | render\_embeds? | boolean | Whether to render message embeds | | render\_reactions? | boolean | Whether to render message reactions | | restricted\_guilds? | array\[snowflake] | The IDs of guilds that you will not receive DMs from | | show\_current\_game? | boolean | Whether to display the currently active game in user presence | | slayer\_sdk\_receive\_dms\_in\_game? | integer | Setting for receiving in-game DMs via the social layer SDK | | status? | string | The status of the user, used to sync presence across clients | | stream\_notifications\_enabled? | boolean | Whether to receive stream notifications for friends | | theme? | string | The [theme](ajustes-de-usuario.md#theme) | | timezone\_offset? | integer | The timezone offset from UTC to use (in minutes) | | view\_nsfw\_guilds? | boolean | Whether NSFW guilds are shown on iOS |

> 📋 HEADER: Get User Consents

Returns a [consents](ajustes-de-usuario.md#consents-object) object representing the tracking features the requestor has consented to.

> 📋 HEADER: Modify User Consents

Modifies the requestor's tracking consent status. Returns a [consents](ajustes-de-usuario.md#consents-object) object on success.

**JSON Params**

\| Field | Type | Description | | ------ | ------------- | -------------------------------------------- | | grant | array\[string] | The [consent types](ajustes-de-usuario.md#consent-type) to grant | | revoke | array\[string] | The [consent types](ajustes-de-usuario.md#consent-type) to revoke |

> 📋 HEADER: Get Email Settings

Returns the requester's [email settings](ajustes-de-usuario.md#email-settings-object) object.

> 📋 HEADER: Modify Email Settings

Modifies the requester's email settings. Returns an [email settings](ajustes-de-usuario.md#email-settings-object) object on success.

**JSON Params**

\| Field | Type | Description | | -------- | ------------------------------------------------------- | ---------------------------- | | settings | partial [email settings](ajustes-de-usuario.md#email-settings-object) object | The email settings to modify |

> 📋 HEADER: Modify Notification Settings

Replaces the notification settings for the user. Returns a [notification settings](ajustes-de-usuario.md#notification-settings-object) object on success. Fires a Notification Settings Update Gateway event.

> ⚠️ ALERTA:

All parameters to this endpoint are optional and nullable. Omitting or setting a `null` value will set it to default.

**JSON Params**

\| Field | Type | Description | | ------ | -------- | ---------------------------------------------------------------------- | | flags? | ?integer | The [notification settings flags](ajustes-de-usuario.md#notification-settings-flags) to set |

> 📋 HEADER: Get Notification Settings Snapshots

Returns a list of [notification settings snapshot](ajustes-de-usuario.md#notification-settings-snapshot-object) objects for the user.

> 📋 HEADER: Create Notification Settings Snapshot

Creates a new notification settings snapshot for the user. Returns a list of [notification settings snapshot](ajustes-de-usuario.md#notification-settings-snapshot-object) objects on success.

> ⚠️ ALERTA:

A maximum of 5 snapshots or **1 MiB** of data can be stored per user.

**JSON Params**

\| Field | Type | Description | | ------ | ------- | ---------------------------------------------- | | label? | ?string | The label of the snapshot (max 100 characters) |

> 📋 HEADER: Restore Notification Settings Snapshot

Restores a notification settings snapshot. Returns a mapping of guild IDs to [user guild settings](ajustes-de-usuario.md#user-guild-settings-object) objects on success. Fires multiple User Guild Settings Update Gateway events.

> 📋 HEADER: Delete Notification Settings Snapshot

Deletes a notification settings snapshot. Returns a list of [notification settings snapshot](ajustes-de-usuario.md#notification-settings-snapshot-object) objects on success.

> 📋 HEADER: Modify User Guild Settings

Modifies a guild's settings. Accepts a [partial user guild settings](ajustes-de-usuario.md#partial-user-guild-settings-structure) object. Returns a [user guild settings](ajustes-de-usuario.md#user-guild-settings-object) object on success. Fires a User Guild Settings Update Gateway event.

> ⚠️ ALERTA:

A value of `@me` for `guild.id` is used to indicate these settings are for the user's private channel notifications.

> 📋 HEADER: Bulk Modify User Guild Settings

Modifies multiple guilds' settings. Returns a list of [user guild settings](ajustes-de-usuario.md#user-guild-settings-object) objects on success. Fires multiple User Guild Settings Update Gateway events.

> ⚠️ ALERTA:

This endpoint cannot be used to modify the user's private channel notification settings.

**JSON Params**

\| Field | Type | Description | | ------ | -------------------------------------------------------------------------------------------- | --------------------------------- | | guilds | map\[snowflake, [partial user guild settings](ajustes-de-usuario.md#partial-user-guild-settings-structure) object] | The user guild settings to modify |

> 📋 HEADER: Modify Audio Settings

Modifies the user's audio context settings. Returns a 204 empty response on success. Fires a Audio Settings Update Gateway event.

**JSON Params**

\| Field | Type | Description | | ----------------- | ------- | ------------------------------------------- | | muted? | boolean | Whether the audio is muted | | volume? | float | Volume level of the user or stream (0-200) | | soundboard\_muted? | boolean | Whether the soundboard is muted (user only) |

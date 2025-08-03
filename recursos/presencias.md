---
icon: user-beard-bolt
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

# Presencias

### [Presences](presencias.md#presences) <a href="#presences" id="presences"></a>

A user's presence is their current status and activity. Presences are usually per-guild, but user accounts also receive overall user presences for [friends and implicit relationships](https://docs.discord.food/resources/relationships#relationship-object) (depending on the [Gateway capability](https://docs.discord.food/topics/gateway#gateway-capabilities)).

#### [Presence Object](presencias.md#presence-object) <a href="#presence-object" id="presence-object"></a>

[**Presence Structure**](presencias.md#presence-structure)

| Field                                         | Type                                                                        | Description                                                                                        |
| --------------------------------------------- | --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| user                                          | partial [user](https://docs.discord.food/resources/user#user-object) object | The user whose presence is being updated                                                           |
| guild\_id?                                    | snowflake                                                                   | The ID of the guild the presence was updated in, if this is a guild presence                       |
| status                                        | string                                                                      | The [status](presencias.md#status-type) of the user                                                |
| activities                                    | array\[[activity](presencias.md#activity-object) object]                    | The current activities the user is partaking in                                                    |
| hidden\_activities? <sup>1</sup> <sup>2</sup> | array\[[activity](presencias.md#activity-object) object]                    | Activities that are hidden from the public                                                         |
| client\_status                                | [client status](presencias.md#client-status-object) object                  | The platform-dependent status of the user                                                          |
| has\_played\_game? <sup>3</sup>               | boolean                                                                     | Whether the user has authorized the same application the current user's session is associated with |

<sup>1</sup> Activities are hidden when a user is invisible or has their [privacy setting](https://docs.discord.food/resources/user-settings-proto#status-settings-structure) disabled. Some exceptions apply, such as the Spotify activity, which is always shown.

<sup>2</sup> To subscribe to hidden activities for a user, see the [Update Activity Subscriptions](presencias.md#update-activity-subscriptions) endpoint.

<sup>3</sup> Only available in [OAuth2 contexts](https://docs.discord.food/topics/gateway#oauth2-and-the-gateway).

#### [Session Object](presencias.md#session-object) <a href="#session-object" id="session-object"></a>

Represents a specific Gateway session's presence information for the current user.

[**Session Structure**](presencias.md#session-structure)

| Field                           | Type                                                      | Description                                            |
| ------------------------------- | --------------------------------------------------------- | ------------------------------------------------------ |
| session\_id <sup>1</sup>        | string                                                    | The ID of the session                                  |
| client\_info                    | [client info](presencias.md#client-info-structure) object | Information about the client that spawned the session  |
| status                          | string                                                    | The [status](presencias.md#status-type) of the session |
| activities                      | array\[[activity](presencias.md#activity-object) object]  | The current activities the session is partaking in     |
| hidden\_activities <sup>2</sup> | array\[[activity](presencias.md#activity-object) object]  | Activities that are hidden from the public             |
| active?                         | boolean                                                   | Unknown                                                |

<sup>1</sup> [Headless sessions](presencias.md#create-headless-session) will have a session ID beginning with .

<sup>2</sup> Activities are hidden when a user is invisible or has their [privacy setting](https://docs.discord.food/resources/user-settings-proto#status-settings-structure) disabled. Some exceptions apply, such as the Spotify activity, which is always shown.

[**Client Info Structure**](presencias.md#client-info-structure)

| Field   | Type    | Description                                                               |
| ------- | ------- | ------------------------------------------------------------------------- |
| client  | string  | The [type of client](presencias.md#client-type)                           |
| os      | string  | The [operating system](presencias.md#operating-system-type) of the client |
| version | integer | The version of the client type (e.g. `5` for the PS5)                     |

[**Client Type**](presencias.md#client-type)

| Value   | Description      |
| ------- | ---------------- |
| desktop | Desktop client   |
| web     | Web-based client |
| mobile  | Mobile client    |
| unknown | Unknown          |

[**Operating System Type**](presencias.md#operating-system-type)

| Value       | Description |
| ----------- | ----------- |
| windows     | Windows     |
| osx         | macOS       |
| linux       | Linux       |
| android     | Android     |
| ios         | iOS         |
| playstation | PlayStation |
| xbox        | Xbox        |
| unknown     | Unknown     |

#### [Client Status Object](presencias.md#client-status-object) <a href="#client-status-object" id="client-status-object"></a>

Active sessions are indicated with a status per platform. If a user is offline or invisible, the corresponding field is not present.

| Field             | Type   | Description                                                                                                   |
| ----------------- | ------ | ------------------------------------------------------------------------------------------------------------- |
| desktop?          | string | The user's [status](presencias.md#status-type) on an active desktop (Windows, Linux, Mac) application session |
| mobile?           | string | The user's [status](presencias.md#status-type) on an active mobile (iOS, Android) application session         |
| web? <sup>1</sup> | string | The user's [status](presencias.md#status-type) on an active web (browser) application session                 |
| embedded?         | string | The user's [status](presencias.md#status-type) on an active embedded (Xbox, PlayStation) session              |

<sup>1</sup> Used as the default when the platform is not known.

[**Status Type**](presencias.md#status-type)

| Value                  | Description      |
| ---------------------- | ---------------- |
| online                 | Online           |
| dnd                    | Do Not Disturb   |
| idle                   | AFK              |
| invisible <sup>1</sup> | Shown as offline |
| offline <sup>1</sup>   | Offline          |
| unknown <sup>2</sup>   | Unknown          |

<sup>1</sup> can only be sent and never received. can only be received and never sent.

<sup>2</sup> This value can only be sent, and is used when the user's initial presence is unknown and should be assigned by the Gateway.

#### [Activity Object](presencias.md#activity-object) <a href="#activity-object" id="activity-object"></a>

[**Activity Structure**](presencias.md#activity-structure)

| Field                     | Type                                                                      | Description                                                                                                |
| ------------------------- | ------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| id <sup>5</sup>           | string                                                                    | The ID of the activity; only unique across a single user's activities                                      |
| name <sup>1</sup>         | string                                                                    | The name of the activity (2-128 characters)                                                                |
| type                      | integer                                                                   | The [activity type](presencias.md#activity-type)                                                           |
| url? <sup>2</sup>         | ?string                                                                   | The stream URL (max 512 characters)                                                                        |
| created\_at <sup>5</sup>  | integer                                                                   | Unix timestamp (in milliseconds) of when the activity was added to the user's session                      |
| session\_id? <sup>5</sup> | ?string                                                                   | The ID of the session associated with the activity                                                         |
| platform? <sup>3</sup>    | string                                                                    | The [platform](presencias.md#activity-platform-type) the activity is being played on                       |
| supported\_platforms?     | array\[string]                                                            | The [platforms](presencias.md#activity-platform-type) the activity is supported on (max 10)                |
| timestamps?               | [activity timestamps](presencias.md#activity-timestamps-structure) object | Unix timestamps (in milliseconds) for start and/or end of the game                                         |
| application\_id?          | snowflake                                                                 | The ID of the application representing the game the user is playing                                        |
| status\_display\_type?    | ?integer                                                                  | [Which field is displayed](presencias.md#status-display-type) in the user's status text in the member list |
| details? <sup>6</sup>     | ?string                                                                   | What the user is currently doing (max 128 characters)                                                      |
| details\_url?             | ?string                                                                   | URL that is opened when clicking on the details text (max 256 characters)                                  |
| state?                    | ?string                                                                   | The user's current party status, or text used for a custom status (max 128 characters)                     |
| state\_url?               | ?string                                                                   | URL that is opened when clicking on the state text (max 256 characters)                                    |
| sync\_id?                 | string                                                                    | The ID of the synced activity (e.g. Spotify song ID)                                                       |
| flags?                    | integer                                                                   | The [activity's flags](presencias.md#activity-flags)                                                       |
| buttons?                  | array\[string]                                                            | Custom buttons shown in rich presence (max 2)                                                              |
| emoji?                    | ?[activity emoji](presencias.md#activity-emoji-structure) object          | The emoji used for a custom status                                                                         |
| party?                    | [activity party](presencias.md#activity-party-structure) object           | Information for the current party of the user                                                              |
| assets?                   | [activity assets](presencias.md#activity-assets-structure) object         | Images for the presence and their hover texts                                                              |
| secrets? <sup>4</sup>     | [activity secrets](presencias.md#activity-secrets-structure) object       | Secrets for rich presence joining and spectating                                                           |
| metadata? <sup>4</sup>    | [activity metadata](presencias.md#activity-metadata-object) object        | Additional metadata for the activity                                                                       |

<sup>1</sup> The of a activity should always be "Custom Status".

<sup>2</sup> URLs must start with or .

<sup>3</sup> This field is not commonly used for traditional presences (i.e. presences sent by regular clients over the Gateway) and is instead used to differentiate between various headless and embedded activities.

<sup>4</sup> These fields are send-only. For retrieving rich presence metadata, see [Get Activity Metadata](presencias.md#get-activity-metadata). For retrieving secrets, see [Get Activity Secret](presencias.md#get-activity-secret).

<sup>5</sup> These fields are received only and cannot be set.

<sup>6</sup> When provided in a activity, it represents a [Custom Status Label](presencias.md#custom-status-label-type) enumeration.

[**Activity Type**](presencias.md#activity-type)

| Value | Name      | Format                                               | Example                              |
| ----- | --------- | ---------------------------------------------------- | ------------------------------------ |
| 0     | PLAYING   | Playing {name}                                       | "Playing Rocket League"              |
| 1     | STREAMING | Streaming {details}                                  | "Streaming Rocket League"            |
| 2     | LISTENING | Listening to {name}                                  | "Listening to Spotify"               |
| 3     | WATCHING  | Watching {name}                                      | "Watching YouTube Together"          |
| 4     | CUSTOM    | {details ? "{details}: "}{emoji ? "{emoji} "}{state} | "😃 I am cool"                       |
| 5     | COMPETING | Competing in {name}                                  | "Competing in Arena World Champions" |
| ~~6~~ | ~~HANG~~  | ~~{state} or {emoji} {details}~~                     | ~~"Chilling"~~                       |

[**Activity Platform Type**](presencias.md#activity-platform-type)

| Value    | Description               |
| -------- | ------------------------- |
| desktop  | Desktop (headless)        |
| xbox     | Xbox integration          |
| samsung  | Samsung integration       |
| ios      | iOS                       |
| android  | Android                   |
| embedded | Embedded session          |
| ps4      | PlayStation 4 integration |
| ps5      | PlayStation 5 integration |

[**Activity Flags**](presencias.md#activity-flags)

| Value      | Name                                   | Description                                                       |
| ---------- | -------------------------------------- | ----------------------------------------------------------------- |
| 1 << 0     | INSTANCE                               | Activity is an instanced game session (a match that will end)     |
| 1 << 1     | JOIN                                   | Activity can be joined by other users                             |
| 1 << 2     | SPECTATE **(deprecated)** <sup>1</sup> | Activity can be spectated by other users                          |
| ~~1 << 3~~ | ~~JOIN\_REQUEST~~ <sup>2</sup>         | ~~Activity requires a request to join~~                           |
| 1 << 4     | SYNC                                   | Activity can be synced                                            |
| 1 << 5     | PLAY                                   | Activity can be played                                            |
| 1 << 6     | PARTY\_PRIVACY\_FRIENDS                | Activity's party can be joined by friends                         |
| 1 << 7     | PARTY\_PRIVACY\_VOICE\_CHANNEL         | Activity's party can be joined by users in the same voice channel |
| 1 << 8     | EMBEDDED                               | Activity is embedded within the Discord client                    |
| 1 << 9     | CONTEXTLESS                            | Embedded activity is launched without a context                   |

<sup>1</sup> Spectating has been removed from official clients and is no longer supported.

<sup>2</sup> Activities no longer need to be explicitly flagged as join requestable.

[**Activity Action Type**](presencias.md#activity-action-type)

| Value | Name                                                | Description                                          |
| ----- | --------------------------------------------------- | ---------------------------------------------------- |
| 1     | JOIN <sup>1</sup>                                   | Allows others to join a game with the user           |
| 2     | SPECTATE **(deprecated)** <sup>1</sup> <sup>2</sup> | Allows others to spectate a game the user is playing |
| 3     | LISTEN                                              | Allows others to listen to a song with the user      |
| ~~4~~ | ~~WATCH~~                                           | ~~Allows others to join a stream with the user~~     |
| 5     | JOIN\_REQUEST <sup>3</sup>                          | Asks others to invite the user to a game             |

<sup>1</sup> These [rich presence invites](https://docs.discord.food/resources/message#message-activity-object) can be used with the [Get Activity Secret](presencias.md#get-activity-secret) endpoint to join/spectate the activity.

<sup>2</sup> Spectating has been removed from official clients and is no longer supported.

<sup>3</sup> This action type is special in that instead of inviting others to a party, it asks existing party members to invite the user to join their party. Inviting users is done by [sending a message back](https://docs.discord.food/resources/message#create-message) with a [rich presence invite](https://docs.discord.food/resources/message#message-activity-object).

[**Activity Timestamps Structure**](presencias.md#activity-timestamps-structure)

| Field  | Type   | Description                                             |
| ------ | ------ | ------------------------------------------------------- |
| start? | string | Unix time (in milliseconds) of when the activity starts |
| end?   | string | Unix time (in milliseconds) of when the activity ends   |

[**Activity Emoji Structure**](presencias.md#activity-emoji-structure)

| Field     | Type      | Description                    |
| --------- | --------- | ------------------------------ |
| name      | string    | The name of the emoji          |
| id?       | snowflake | The ID of the emoji            |
| animated? | boolean   | Whether this emoji is animated |

[**Activity Party Structure**](presencias.md#activity-party-structure)

| Field | Type                     | Description                                                     |
| ----- | ------------------------ | --------------------------------------------------------------- |
| id?   | string                   | The ID of the party (max 128 characters)                        |
| size? | array\[integer, integer] | The party's current and maximum size (current\_size, max\_size) |

[**Activity Assets Structure**](presencias.md#activity-assets-structure)

| Field         | Type   | Description                                                                               |
| ------------- | ------ | ----------------------------------------------------------------------------------------- |
| large\_image? | string | The large [activity asset image](presencias.md#activity-asset-image) (max 313 characters) |
| large\_text?  | string | Text displayed when hovering over the large image of the activity (max 128 characters)    |
| large\_url?   | string | URL that is opened when clicking on the large image (max 256 characters)                  |
| small\_image? | string | The small [activity asset image](presencias.md#activity-asset-image) (max 313 characters) |
| small\_text?  | string | Text displayed when hovering over the small image of the activity (max 128 characters)    |
| small\_url?   | string | URL that is opened when clicking on the small image (max 256 characters)                  |

[**Activity Asset Image**](presencias.md#activity-asset-image)

Activity asset images are arbitrary strings which usually contain snowflake IDs, URLs, or prefixed image IDs. Treat data within this field carefully, as it is user-specifiable and not sanitized.

| Type              | Format                   | Image URL                                                                                    |
| ----------------- | ------------------------ | -------------------------------------------------------------------------------------------- |
| Application Asset | `{application_asset_id}` | See [application asset image formatting](https://docs.discord.food/reference#cdn-formatting) |
| Media Proxy       | `mp:{image_id}`          | `https://media.discordapp.net/{image_id}`                                                    |
| Twitch            | `twitch:{username}`      | `https://static-cdn.jtvnw.net/previews-ttv/live_user_{username}-{width}x{height}.jpg`        |
| YouTube           | `youtube:{video_id}`     | `https://i.ytimg.com/vi/{video_id}/{thumbnail_type}.jpg`                                     |
| Spotify           | `spotify:{spotify_id}`   | `https://i.scdn.co/image/{spotify_id}`                                                       |

[**Activity Secrets Structure**](presencias.md#activity-secrets-structure)

| Field                                   | Type       | Description                                                        |
| --------------------------------------- | ---------- | ------------------------------------------------------------------ |
| join?                                   | string     | The secret for joining a party (max 128 characters)                |
| spectate? **(deprecated)** <sup>1</sup> | string     | The secret for spectating a game (max 128 characters)              |
| ~~match?~~                              | ~~string~~ | ~~The secret for a specific instanced match (max 128 characters)~~ |

<sup>1</sup> Spectating has been removed from official clients and is no longer supported.

[**Operating System Type**](presencias.md#operating-system-type)

| Value  | Description |
| ------ | ----------- |
| win32  | Windows     |
| darwin | macOS       |
| linux  | Linux       |

[**Custom Status Label Type**](presencias.md#custom-status-label-type)

| Value      | Icon           | Label                                 |
| ---------- | -------------- | ------------------------------------- |
| ~~listen~~ | ~~Music note~~ | ~~On repeat~~                         |
| ~~watch~~  | ~~TV~~         | ~~Watching lateley~~                  |
| ~~play~~   | ~~Controller~~ | ~~Playing lately~~                    |
| question   | Calendar       | Question of the day                   |
| think      | Lightbulb      | Shower thought                        |
| love       | Heart          | Current obsession (was Loving lately) |
| excited    | Shooting stars | Can't wait for                        |
| recommend  | Guide          | Recommendation needed                 |

[**Status Display Type**](presencias.md#status-display-type)

| Value | Name    | Description                 | Example                                |
| ----- | ------- | --------------------------- | -------------------------------------- |
| 0     | NAME    | Display the `name` field    | "Listening to Spotify"                 |
| 1     | STATE   | Display the `state` field   | "Listening to Rick Astley"             |
| 2     | DETAILS | Display the `details` field | "Listening to Never Gonna Give You Up" |

[**Example Activity**](presencias.md#example-activity)

```
{  "id": "d11307d8c0abb136",  "created_at": "1695164784863",  "details": "24H RL Stream for Charity",  "state": "Rocket League",  "name": "Twitch",  "type": 1,  "url": "https://www.twitch.tv/discord",  "assets": {    "large_image": "twitch:discord"  }}
```

[**Example Activity with Rich Presence**](presencias.md#example-activity-with-rich-presence)

```
{  "id": "d11307d8c0abb135",  "name": "Rocket League",  "type": 0,  "created_at": "1695164784863",  "session_id": "30f32c5d54ae86130fc4a215c7474263",  "application_id": "379286085710381999",  "state": "In a Match",  "details": "Ranked Duos: 2-1",  "platform": "xbox",  "flags": 0,  "timestamps": {    "start": "1695164482423"  },  "party": {    "id": "9dd6594e-81b3-49f6-a6b5-a679e6a060d3",    "size": [2, 2]  },  "assets": {    "large_image": "351371005538729000",    "large_text": "DFH Stadium",    "small_image": "351371005538729111",    "small_text": "Silver III"  },  "secrets": {    "join": "025ed05c71f639de8bfaa0d679d7c94b2fdce12f"  }}
```

#### [Activity Metadata Object](presencias.md#activity-metadata-object) <a href="#activity-metadata-object" id="activity-metadata-object"></a>

[**Activity Metadata Structure**](presencias.md#activity-metadata-structure)

| Field         | Type           | Description                                                                 |
| ------------- | -------------- | --------------------------------------------------------------------------- |
| button\_urls? | array\[string] | The URLs corresponding to the custom buttons shown in rich presence (max 2) |
| artist\_ids?  | array\[string] | The Spotify IDs of the artists of the song being played                     |
| album\_id?    | string         | The Spotify ID of the album of the song being played                        |
| context\_uri? | string         | The Spotify URI of the current player context                               |
| type?         | string         | The type of Spotify track being played (`track` or `episode`)               |

### [Endpoints](presencias.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get Presences](presencias.md#get-presences) <a href="#get-presences" id="get-presences"></a>

`GET/presences`

Returns the overall user presence for all of the user's non-offline [friends and implicit relationships](https://docs.discord.food/resources/relationships#relationship-object).

[**Response Body**](presencias.md#response-body)

| Field        | Type                                                                                                     | Description                                                                                             |
| ------------ | -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| guilds       | array\[[voice guild](presencias.md#voice-guild-structure) object]                                        | The guilds that the user's non-offline friends and implicit relationships have active voice sessions in |
| presences    | array\[[presence](presencias.md#presence-object) object]                                                 | The overall user presences of the user's non-offline friends and implicit relationships                 |
| applications | array\[partial [application](https://docs.discord.food/resources/application#application-object) object] | The found game applications in the presences                                                            |

[**Voice Guild Structure**](presencias.md#voice-guild-structure)

| Field           | Type                                                                  | Description                                                                 |
| --------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| guild\_id       | snowflake                                                             | The ID of the guild                                                         |
| guild\_name     | string                                                                | The name of the guild                                                       |
| guild\_icon     | ?string                                                               | The guild's [icon hash](https://docs.discord.food/reference#cdn-formatting) |
| voice\_channels | array\[[voice channel](presencias.md#voice-channel-structure) object] | The voice channels that have active sessions                                |

[**Voice Channel Structure**](presencias.md#voice-channel-structure)

| Field                 | Type                                                                | Description                               |
| --------------------- | ------------------------------------------------------------------- | ----------------------------------------- |
| channel\_id           | snowflake                                                           | The ID of the voice channel               |
| channel\_name         | string                                                              | The name of the voice channel             |
| users                 | array\[snowflake]                                                   | The IDs of the users in the voice channel |
| streams? <sup>1</sup> | array\[[voice stream](presencias.md#voice-stream-structure) object] | The streams of the users in the channel   |

<sup>1</sup> Only included when fetched from the [Get Presences for Xbox](presencias.md#get-presences-for-xbox) endpoint.

[**Voice Stream Structure**](presencias.md#voice-stream-structure)

| Field    | Type      | Description                          |
| -------- | --------- | ------------------------------------ |
| user\_id | snowflake | The ID of the user that is streaming |

[**Example Voice Guild**](presencias.md#example-voice-guild)

```
{  "guild_id": "839502008108580904",  "guild_icon": "4da69b53981d1adfa13590a3f2d856ee",  "guild_name": "Testing Server",  "voice_channels": [    {      "channel_id": "850360749460553769",      "channel_name": "Voice",      "users": ["150745989836308480"],      "streams": [        {          "user_id": "150745989836308480"        }      ]    }  ]}
```

#### [Get Presences for Xbox](presencias.md#get-presences-for-xbox) <a href="#get-presences-for-xbox" id="get-presences-for-xbox"></a>

`GET/consoles/xbox/presences`

Returns the overall user presence for all of the user's non-offline [friends and implicit relationships](https://docs.discord.food/resources/relationships#relationship-object), as well as their Xbox connection information.

[**Response Body**](presencias.md#response-body)

| Field                   | Type                                                                                                     | Description                                                                                             |
| ----------------------- | -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| guilds                  | array\[[voice guild](presencias.md#voice-guild-structure) object]                                        | The guilds that the user's non-offline friends and implicit relationships have active voice sessions in |
| presences               | array\[[presence](presencias.md#presence-object) object]                                                 | The overall user presences of the user's non-offline friends and implicit relationships                 |
| applications            | array\[partial [application](https://docs.discord.food/resources/application#application-object) object] | The found game applications in the presences                                                            |
| connected\_account\_ids | array\[[connected account](presencias.md#connected-account-structure) object]                            | The connected Xbox accounts for each user                                                               |

[**Connected Account Structure**](presencias.md#connected-account-structure)

| Field         | Type           | Description                                         |
| ------------- | -------------- | --------------------------------------------------- |
| user\_id      | snowflake      | The ID of the user                                  |
| provider\_ids | array\[string] | The IDs of the connected Xbox accounts for the user |

#### [Update Presence](presencias.md#update-presence) <a href="#update-presence" id="update-presence"></a>

`POST/presences`

Updates the current user's mobile game activity. Returns a 204 empty response on success. Fires a [Sessions Replace](https://docs.discord.food/topics/gateway-events#sessions-replace) Gateway event.

[**JSON Params**](presencias.md#json-params)

| Field         | Type   | Description                                                                 |
| ------------- | ------ | --------------------------------------------------------------------------- |
| package\_name | string | The package name of the game (e.g. `com.supercell.clashofclans`)            |
| update?       | string | The [type of update](presencias.md#presence-update-type) (default `UPDATE`) |

[**Presence Update Type**](presencias.md#presence-update-type)

| Value  | Description                     |
| ------ | ------------------------------- |
| START  | Start a new game session        |
| UPDATE | Update the current game session |
| STOP   | Stop the current game session   |

#### [Get Global Activity Statistics](presencias.md#get-global-activity-statistics) <a href="#get-global-activity-statistics" id="get-global-activity-statistics"></a>

`GET/activities`

Returns a list of [global activity statistics](presencias.md#global-activity-statistics-structure) objects representing games the user's friends and affine users have recently played.

[**Query String Params**](presencias.md#query-string-params)

| Field               | Type    | Description                                                                                    |
| ------------------- | ------- | ---------------------------------------------------------------------------------------------- |
| with\_users?        | boolean | Whether to include user information in the returned activity statistics (default false)        |
| with\_applications? | boolean | Whether to include application information in the returned activity statistics (default false) |

[**Global Activity Statistics Structure**](presencias.md#global-activity-statistics-structure)

| Field                     | Type                                                                                             | Description                                        |
| ------------------------- | ------------------------------------------------------------------------------------------------ | -------------------------------------------------- |
| user\_id                  | string                                                                                           | The ID of the user playing the game                |
| user? <sup>1</sup>        | partial [user](https://docs.discord.food/resources/user#user-object) object                      | The user playing the game                          |
| application\_id           | string                                                                                           | The ID of the application representing the game    |
| application? <sup>2</sup> | partial [application](https://docs.discord.food/resources/application#application-object) object | The application representing the game              |
| updated\_at               | ISO8601 timestamp                                                                                | When the user last played the game                 |
| duration                  | integer                                                                                          | How long the user played the game for (in seconds) |

<sup>1</sup> Only included when is set to .

<sup>2</sup> Only included when is set to .

[**Example Global Activity Statistics**](presencias.md#example-global-activity-statistics)

```
{  "user_id": "1001086404203389018",  "application_id": "1334568856227942480",  "updated_at": "2025-03-01T14:58:02.284000+00:00",  "duration": 1176}
```

#### [Get Application Activity Statistics](presencias.md#get-application-activity-statistics) <a href="#get-application-activity-statistics" id="get-application-activity-statistics"></a>

`GET/activities/statistics/applications/{application.id}`

Returns a list of [application activity statistics](presencias.md#application-activity-statistics-structure) objects representing the user's friends and affine users that have played the given game.

[**Application Activity Statistics Structure**](presencias.md#application-activity-statistics-structure)

| Field            | Type              | Description                                                 |
| ---------------- | ----------------- | ----------------------------------------------------------- |
| user\_id         | string            | The ID of the user playing the game                         |
| last\_played\_at | ISO8601 timestamp | When the user last played the game                          |
| total\_duration  | integer           | How long the user has ever played the game for (in seconds) |

[**Example Application Activity Statistics**](presencias.md#example-application-activity-statistics)

```
{  "user_id": "343383572805058560",  "last_played_at": "2025-02-20T00:21:07.457000+00:00",  "total_duration": 1180625}
```

#### [Get User Application Activity Statistics](presencias.md#get-user-application-activity-statistics) <a href="#get-user-application-activity-statistics" id="get-user-application-activity-statistics"></a>

`GET/users/@me/activities/statistics/applications`

Returns a list of [user application activity statistics](presencias.md#user-application-activity-statistics-structure) objects representing games the user has played.

[**User Application Activity Statistics Structure**](presencias.md#user-application-activity-statistics-structure)

| Field                         | Type               | Description                                                             |
| ----------------------------- | ------------------ | ----------------------------------------------------------------------- |
| application\_id               | string             | The ID of the application representing the game                         |
| last\_played\_at              | ISO8601 timestamp  | When the user last played the game                                      |
| first\_played\_at             | ?ISO8601 timestamp | When the user first played the game (may not be tracked)                |
| total\_duration               | integer            | How long the user has ever played the game for (in seconds)             |
| total\_discord\_sku\_duration | integer            | How long the user has ever played the game through Discord (in seconds) |

[**Example User Application Activity Statistics**](presencias.md#example-user-application-activity-statistics)

```
{  "application_id": "356875221078245376",  "last_played_at": "2025-02-20T06:24:34.792000+00:00",  "first_played_at": "2024-04-29T22:23:30.307000+00:00",  "total_duration": 246531,  "total_discord_sku_duration": 0}
```

#### [Update Activity Session](presencias.md#update-activity-session) <a href="#update-activity-session" id="update-activity-session"></a>

`POST/activities`

Updates a currently running activity game session for the current user.

[**JSON Params**](presencias.md#json-params)

| Field               | Type      | Description                                                                                     |
| ------------------- | --------- | ----------------------------------------------------------------------------------------------- |
| token?              | string    | The token of the existing session to update                                                     |
| application\_id     | snowflake | The ID of the application representing the game                                                 |
| duration?           | integer   | How long the game has been played for (in seconds) sine the last update (max 1800)              |
| share\_activity?    | boolean   | Whether to share the activity in activity statistics (default true)                             |
| distributor?        | string    | The [distributor](https://docs.discord.food/resources/application#distributor-type) of the game |
| exe\_path?          | string    | The path to the game's executable file (max 128 characters)                                     |
| voice\_channel\_id? | snowflake | The ID of the voice channel the user is in while playing the game                               |
| session\_id?        | string    | The ID of the session associated with the activity                                              |
| media\_session\_id? | string    | The ID of the voice connection media session associated with the activity                       |
| closed?             | boolean   | Whether the game has been closed and the session is over (default false)                        |

[**Response Body**](presencias.md#response-body)

| Field | Type   | Description                      |
| ----- | ------ | -------------------------------- |
| token | string | The updated token of the session |

[**Example Response**](presencias.md#example-response)

```
{  "token": ".eJxNzEEKwyAUBNCrBLdtyqjx6_cc3YvELKShCYmuSu_eWArNcoY38xJ1n7aQk_CdcEY5VootkWQQsxbXTsR1nfMYS16ePyclazkQEzslodXA3GBdUyxTCrE0pKBMD92D7rAezht7I2tAdAE80Bapbt_fw-OIZSlxDqdS2uFf531cthT2Rz0TvD_X3TcU.Z8lKCQ.QvSLefzqmY6lhaBaQNG308fl3e8"}
```

#### [Create Headless Session](presencias.md#create-headless-session) <a href="#create-headless-session" id="create-headless-session"></a>

`POST/users/@me/headless-sessions`

Creates a new headless session for the current user. Fires a [Sessions Replace](https://docs.discord.food/topics/gateway-events#sessions-replace) Gateway event.

[**JSON Params**](presencias.md#json-params)

| Field                   | Type                                                     | Description                                                              |
| ----------------------- | -------------------------------------------------------- | ------------------------------------------------------------------------ |
| activities <sup>1</sup> | array\[[activity](presencias.md#activity-object) object] | An array containing exactly one activity to set for the headless session |
| token?                  | string                                                   | The token of the headless session to update                              |

<sup>1</sup> In addition to and , this activity object must also have a valid and set.

[**Response Body**](presencias.md#response-body)

| Field      | Type                                                     | Description                                                 |
| ---------- | -------------------------------------------------------- | ----------------------------------------------------------- |
| activities | array\[[activity](presencias.md#activity-object) object] | The current activities the headless session is partaking in |
| token      | string                                                   | The token of the headless session                           |

#### [Delete Headless Session](presencias.md#delete-headless-session) <a href="#delete-headless-session" id="delete-headless-session"></a>

`POST/users/@me/headless-sessions/delete`

Deletes a headless session for the current user. Returns a 204 empty response on success. Fires a [Sessions Replace](https://docs.discord.food/topics/gateway-events#sessions-replace) Gateway event.

[**JSON Params**](presencias.md#json-params)

| Field | Type   | Description                       |
| ----- | ------ | --------------------------------- |
| token | string | The token of the headless session |

#### [Get Activity Metadata](presencias.md#get-activity-metadata) <a href="#get-activity-metadata" id="get-activity-metadata"></a>

`GET/users/{user.id}/sessions/{session_id}/activities/{application.id}/metadata`

Returns the [activity metadata](presencias.md#activity-metadata-object) for a given activity, or a 204 empty response if no metadata is found.

#### [Get Activity Secret](presencias.md#get-activity-secret) <a href="#get-activity-secret" id="get-activity-secret"></a>

`GET/users/{user.id}/sessions/{session_id}/activities/{application.id}/{activity_action_type}`

Returns an activity secret that can be used to join or spectate the game. Only supports the and [action types](presencias.md#activity-action-type).

[**Query String Params**](presencias.md#query-string-params)

| Field        | Type      | Description                                                     |
| ------------ | --------- | --------------------------------------------------------------- |
| channel\_id? | snowflake | The ID of the channel the rich presence invite has been sent in |
| message\_id? | snowflake | The ID of the rich presence invite message                      |

[**Response Body**](presencias.md#response-body)

| Field  | Type   | Description         |
| ------ | ------ | ------------------- |
| secret | string | The activity secret |

[**Example Response**](presencias.md#example-response)

```
{ "secret": "025ed05c71f639de8bfaa0d679d7c94b2fdce12f" }
```

#### [Update Activity Subscriptions](presencias.md#update-activity-subscriptions) <a href="#update-activity-subscriptions" id="update-activity-subscriptions"></a>

`POST/users/@me/activities/subscribe`

Updates the current user's subscribed hidden activities. Returns a 204 empty response on success. Fires multiple [Presence Update](https://docs.discord.food/topics/gateway-events#presence-update) Gateway events.

[**JSON Params**](presencias.md#json-params)

| Field         | Type                                                                                  | Description                                             |
| ------------- | ------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| subscriptions | array\[[activity subscription](presencias.md#activity-subscription-structure) object] | The hidden activities the user is subscribed to (1-100) |

[**Activity Subscription Structure**](presencias.md#activity-subscription-structure)

| Field           | Type      | Description                                                |
| --------------- | --------- | ---------------------------------------------------------- |
| user\_id        | snowflake | The ID of the user to subscribe to                         |
| application\_id | snowflake | The ID of the application representing the game            |
| party\_id       | snowflake | The ID of the party the user is in                         |
| message\_id     | snowflake | The ID of the message containing the rich presence invite  |
| channel\_id     | snowflake | The ID of the channel the rich presence invite was sent in |

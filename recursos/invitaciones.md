---
icon: party-horn
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

# Invitaciones

### [Invites](invitaciones.md#invites) <a href="#invites" id="invites"></a>

Invites are used by users to join a guild or group DM, or to add a user to their friends list.

#### [Temporary Invites](invitaciones.md#temporary-invites) <a href="#temporary-invites" id="temporary-invites"></a>

Temporary invites (indicated by the [field](invitaciones.md#invite-object)) grant non-permanent access to a guild. Upon [accepting a temporary invite](invitaciones.md#accept-invite), the user is added to the guild and can interact with it unconditionally until all of their sessions are disconnected. If the user does not have an active session at the time of accepting the invite, they will be removed after the next time they disconnect.

[**Guest Invites**](invitaciones.md#guest-invites)

Guest invites (indicated by the [field](invitaciones.md#invite-object)), similar to temporary invites, also grant non-permanent access to a guild. However, unlike temporary invites, upon [accepting a guest invite](invitaciones.md#accept-invite), the user does not become a member of the guild. The session ID provided during acceptance is dispatched a [Guild Create](https://docs.discord.food/topics/gateway-events#guild-create) event containing only the channel the invite was for, and the user receives no other guild-specific events (except for [Guild Delete](https://docs.discord.food/topics/gateway-events#guild-delete) when they are removed). Guest access only allows using a subset of endpoints required for interacting with voice channels, and access is removed after the user disconnects from the voice channel.

#### [Invite Object](invitaciones.md#invite-object) <a href="#invite-object" id="invite-object"></a>

A code that when used, adds a user to a guild or group DM channel, or creates a relationship between two users.

[**Invite Structure**](invitaciones.md#invite-structure)

| Field                                      | Type                                                                                                                   | Description                                                                                                                                   |
| ------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| code                                       | string                                                                                                                 | The invite code (unique ID)                                                                                                                   |
| type                                       | integer                                                                                                                | The [type of invite](invitaciones.md#invite-type)                                                                                             |
| channel                                    | ?partial [channel](https://docs.discord.food/resources/channel#channel-object) object                                  | The channel this invite is for; `null` for friend invites that did not have a DM channel created                                              |
| guild\_id?                                 | snowflake                                                                                                              | The ID of the guild this invite is for                                                                                                        |
| guild?                                     | [invite guild](invitaciones.md#invite-guild-object) object                                                             | The guild this invite is for                                                                                                                  |
| profile?                                   | [guild profile](https://docs.discord.food/resources/discovery#guild-profile-object) object                             | The profile of the guild this invite is for                                                                                                   |
| inviter?                                   | partial [user](https://docs.discord.food/resources/user#user-object) object                                            | The user who created the invite                                                                                                               |
| flags?                                     | integer                                                                                                                | The [invite's flags](invitaciones.md#invite-flags)                                                                                            |
| target\_type?                              | integer                                                                                                                | The [type of target](invitaciones.md#invite-target-type) for this guild invite                                                                |
| target\_user?                              | partial [user](https://docs.discord.food/resources/user#user-object) object                                            | The user whose stream to display for this voice channel stream invite                                                                         |
| target\_application?                       | partial [application](https://docs.discord.food/resources/application#application-object) object                       | The embedded application to open for this voice channel embedded application invite                                                           |
| approximate\_member\_count? <sup>1</sup>   | integer                                                                                                                | Approximate count of total members in the guild or group DM                                                                                   |
| approximate\_presence\_count? <sup>1</sup> | integer                                                                                                                | Approximate count of non-offline members in the guild                                                                                         |
| expires\_at                                | ?ISO8601 timestamp                                                                                                     | The expiry date of the invite, if it expires                                                                                                  |
| guild\_scheduled\_event?                   | [guild scheduled event](https://docs.discord.food/resources/guild-scheduled-event#guild-scheduled-event-object) object | Guild scheduled event data, only included if `guild_scheduled_event_id` contains a valid guild scheduled event ID                             |
| new\_member? <sup>2</sup>                  | boolean                                                                                                                | Whether the user is a new member of the guild                                                                                                 |
| show\_verification\_form? <sup>2</sup>     | boolean                                                                                                                | Whether the user should be shown the guild's [member verification](https://docs.discord.food/resources/guild#member-verification-object) form |
| is\_nickname\_changeable? <sup>3</sup>     | boolean                                                                                                                | Whether the @everyone role has the `CHANGE_NICKNAME` permission in the guild this invite is for                                               |

<sup>1</sup> Only included when fetched from the [Get Invite](invitaciones.md#get-invite) endpoint with set to . Also included when fetched from the [Accept Invite](invitaciones.md#accept-invite) endpoint on [non-previewable guilds](https://docs.discord.food/resources/guild#guild-previewing).

<sup>2</sup> Only included when fetched from the [Accept Invite](invitaciones.md#accept-invite) endpoint. Note that is erroneously set to for non-guild invites and is missing when accepting an invite to a [non-previewable guild](https://docs.discord.food/resources/guild#guild-previewing).

<sup>3</sup> Only included when fetched from the [Get Invite](invitaciones.md#get-invite) endpoint with set to .

[**Invite Type**](invitaciones.md#invite-type)

| Value | Name      | Description                                                                                                       |
| ----- | --------- | ----------------------------------------------------------------------------------------------------------------- |
| 0     | GUILD     | Joins the user to a [guild](https://docs.discord.food/resources/guild#guild-object)                               |
| 1     | GROUP\_DM | Joins the user to a [group DM](https://docs.discord.food/resources/channel#channel-object)                        |
| 2     | FRIEND    | Adds the user as a [friend](https://docs.discord.food/resources/relationships#relationship-object) to the inviter |

[**Invite Target Type**](invitaciones.md#invite-target-type)

| Value | Name                             | Description                                                                                                                           |
| ----- | -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| 1     | STREAM                           | The invite is for a stream in a [voice channel](https://docs.discord.food/resources/channel#channel-object)                           |
| 2     | EMBEDDED\_APPLICATION            | The invite is for an embedded application (activity) in a [voice channel](https://docs.discord.food/resources/channel#channel-object) |
| 3     | ROLE\_SUBSCRIPTIONS <sup>1</sup> | The invite redirects to the role subscriptions page within a [guild](https://docs.discord.food/resources/guild#guild-object)          |
| 4     | CREATOR\_PAGE <sup>1</sup>       | The invite originates from the creator page of a [guild](https://docs.discord.food/resources/guild#guild-object)                      |
| 5     | LOBBY <sup>1</sup>               | The invite is for a lobby member                                                                                                      |

<sup>1</sup> Invites with these target types are not returned in the [Get Guild Invites](invitaciones.md#get-guild-invites) and [Get Channel Invites](invitaciones.md#get-channel-invites) endpoints. They are also not deletable through [Delete Invite](invitaciones.md#delete-invite).

[**Invite Flags**](invitaciones.md#invite-flags)

| Value  | Name                    | Description                                                                                                                                                                      |
| ------ | ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1 << 0 | IS\_GUEST\_INVITE       | Invite grants one-time access to a voice channel in the guild                                                                                                                    |
| 1 << 1 | IS\_VIEWED              | Invite has been viewed by any user (has been retrieved using [Get Invite](invitaciones.md#get-invite))                                                                           |
| 1 << 2 | IS\_ENHANCED            | Unknown                                                                                                                                                                          |
| 1 << 3 | IS\_APPLICATION\_BYPASS | Invite bypasses [guild join requests](https://docs.discord.food/resources/guild#guild-join-request-object) and adds the user directly to the guild with `pending` set to `false` |

[**Example Invite Object**](invitaciones.md#example-invite-object)

```
{  "type": 0,  "code": "jvuBeT38",  "inviter": {    "id": "852892297661906993",    "username": "alien",    "avatar": "05145cc5646fbcba277b6d5ea2030610",    "discriminator": "0",    "public_flags": 4194432,    "banner": null,    "accent_color": null,    "global_name": "Alien",    "avatar_decoration_data": null,    "primary_guild": null  },  "expires_at": "2023-07-22T18:30:11+00:00",  "guild": {    "id": "1046920999469330512",    "name": "Alien Network",    "splash": "b40e61f7730b8781b9a551964570e0cc",    "banner": "a_98d07f130569f17e8352df80c3a2bc2b",    "description": "Where the 👽s 👽 and sometimes very 👽 things happen 😨.",    "icon": "66b0f4d96c145970fa9d96ada8afadf3",    "features": [],    "verification_level": 2,    "vanity_url_code": "alien",    "premium_subscription_count": 14,    "nsfw": false,    "nsfw_level": 0  },  "guild_id": "1046920999469330512",  "channel": {    "id": "1057241425793798144",    "type": 2,    "name": "alien noises"  },  "target_type": 2,  "target_application": {    "id": "880218394199220334",    "name": "Watch Together",    "icon": "ec48acbad4c32efab4275cb9f3ca3a58",    "description": "Create and watch a playlist of YouTube videos with your friends. Your choice to share the remote or not. ",    "type": null,    "is_monetized": false,    "is_verified": false,    "is_discoverable": false,    "cover_image": "3cc9446876ae9eec6e06ff565703c292",    "bot": {      "id": "880218394199220334",      "username": "Watch Together",      "avatar": "fe2b7fa334817b0346d57416ad75e93b",      "discriminator": "5319",      "public_flags": 0,      "bot": true,      "banner": null,      "accent_color": null,      "global_name": null,      "avatar_decoration_data": null,      "primary_guild": null    },    "summary": "",    "bot_public": false,    "bot_require_code_grant": false,    "terms_of_service_url": "https://discord.com/terms",    "privacy_policy_url": "https://discord.com/privacy",    "verify_key": "e2aaf50fbe2fd9d025ac669035f5efb89099931690fba9dc28efb7eaade7f96d",    "flags": 1179648,    "max_participants": -1,    "tags": ["Video Player", "Watch"],    "hook": true,    "storefront_available": false,    "embedded_activity_config": {      "activity_preview_video_asset_id": "1104184163201990836",      "supported_platforms": ["web", "ios", "android"],      "default_orientation_lock_state": 2,      "tablet_default_orientation_lock_state": 1,      "requires_age_gate": false,      "legacy_responsive_aspect_ratio": false,      "premium_tier_requirement": null,      "free_period_starts_at": null,      "free_period_ends_at": null,      "client_platform_config": {        "ios": { "label_type": 0, "label_until": null, "release_phase": "global_launch" },        "android": { "label_type": 0, "label_until": null, "release_phase": "global_launch" },        "web": { "label_type": 0, "label_until": null, "release_phase": "global_launch" }      },      "shelf_rank": 3,      "has_csp_exception": false,      "displays_advertisements": false    }  },  "approximate_member_count": 100,  "approximate_presence_count": 99,  "is_nickname_changeable": true}
```

#### [Invite Metadata Object](invitaciones.md#invite-metadata-object) <a href="#invite-metadata-object" id="invite-metadata-object"></a>

Extra information about an invite, will extend the [invite](invitaciones.md#invite-object) object.

[**Invite Metadata Structure**](invitaciones.md#invite-metadata-structure)

| Field                   | Type              | Description                                                                                       |
| ----------------------- | ----------------- | ------------------------------------------------------------------------------------------------- |
| uses? <sup>1</sup>      | integer           | Number of times this invite has been used                                                         |
| max\_uses? <sup>1</sup> | integer           | Max number of times this invite can be used                                                       |
| max\_age?               | integer           | Duration (in seconds) after which the invite expires (default 0)                                  |
| temporary? <sup>2</sup> | boolean           | Whether this invite only grants temporary membership (default false for unsupported invite types) |
| created\_at             | ISO8601 timestamp | When this invite was created                                                                      |

<sup>1</sup> This information is not tracked or returned for group DM invites. However, they always have a of 0.

<sup>2</sup> [Temporary invites](invitaciones.md#temporary-invites) are only supported for guilds.

[**Example Invite with Metadata Object**](invitaciones.md#example-invite-with-metadata-object)

```
{  "type": 0,  "code": "jvuBeT38",  "inviter": {    "id": "852892297661906993",    "username": "alien",    "avatar": "05145cc5646fbcba277b6d5ea2030610",    "discriminator": "0",    "public_flags": 4194432,    "banner": null,    "accent_color": null,    "global_name": "Alien",    "avatar_decoration_data": null,    "primary_guild": null  },  "max_age": 604800,  "created_at": "2023-07-15T18:30:11.047000+00:00",  "expires_at": "2023-07-22T18:30:11+00:00",  "guild": {    "id": "1046920999469330512",    "name": "Alien Network",    "splash": "b40e61f7730b8781b9a551964570e0cc",    "banner": "a_98d07f130569f17e8352df80c3a2bc2b",    "description": "Where the 👽s 👽 and sometimes very 👽 things happen 😨.",    "icon": "66b0f4d96c145970fa9d96ada8afadf3",    "features": [],    "verification_level": 2,    "vanity_url_code": "alien",    "nsfw_level": 0,    "nsfw": false,    "premium_subscription_count": 14,    "premium_tier": 3  },  "guild_id": "1046920999469330512",  "channel": {    "id": "1057241425793798144",    "type": 2,    "name": "alien noises"  },  "uses": 0,  "max_uses": 0,  "temporary": false}
```

#### [Invite Guild Object](invitaciones.md#invite-guild-object) <a href="#invite-guild-object" id="invite-guild-object"></a>

The guild an invite is for.

[**Invite Guild Structure**](invitaciones.md#invite-guild-structure)

| Field                         | Type           | Description                                                                                                   |
| ----------------------------- | -------------- | ------------------------------------------------------------------------------------------------------------- |
| id                            | snowflake      | The ID of the guild                                                                                           |
| name                          | string         | The name of the guild (2-100 characters)                                                                      |
| icon                          | ?string        | The guild's [icon hash](https://docs.discord.food/reference#cdn-formatting)                                   |
| description                   | ?string        | The description for the guild (max 300 characters)                                                            |
| banner                        | ?string        | The guild's [banner hash](https://docs.discord.food/reference#cdn-formatting)                                 |
| splash                        | ?string        | The guild's [splash hash](https://docs.discord.food/reference#cdn-formatting)                                 |
| verification\_level           | integer        | The [verification level](https://docs.discord.food/resources/guild#verification-level) required for the guild |
| features                      | array\[string] | Enabled [guild features](https://docs.discord.food/resources/guild#guild-features)                            |
| vanity\_url\_code             | ?string        | The guild's vanity invite code                                                                                |
| premium\_subscription\_count? | integer        | The number of premium subscriptions (boosts) the guild currently has                                          |
| premium\_tier                 | integer        | The guild's [premium tier](https://docs.discord.food/resources/guild#premium-tier) (boost level)              |
| nsfw **(deprecated)**         | boolean        | Whether the guild is considered NSFW (`EXPLICIT` or `AGE_RESTRICTED`)                                         |
| nsfw\_level                   | integer        | The guild's [NSFW level](https://docs.discord.food/resources/guild#nsfw-level)                                |

### [Endpoints](invitaciones.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get Invite](invitaciones.md#get-invite) <a href="#get-invite" id="get-invite"></a>

`GET/invites/{invite.code}`

Returns an [invite](invitaciones.md#invite-object) object for the given code.

[**Query String Params**](invitaciones.md#query-string-params)

| Field                        | Type      | Description                                                                                                               |
| ---------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------- |
| with\_counts?                | boolean   | Whether the invite should contain approximate member counts (and partial recipients for group DM invites) (default false) |
| with\_permissions?           | boolean   | Whether the invite should contain permission-related fields (default false)                                               |
| guild\_scheduled\_event\_id? | snowflake | The guild scheduled event to include with the invite                                                                      |

#### [Accept Invite](invitaciones.md#accept-invite) <a href="#accept-invite" id="accept-invite"></a>

`POST/invites/{invite.code}`

Accepts an invite to a [guild](invitaciones.md#invite-guild-object), [group DM](https://docs.discord.food/resources/channel#channel-object), or [DM](https://docs.discord.food/resources/channel#channel-object). Returns an [invite](invitaciones.md#invite-object) object on success. May fire a [Guild Create](https://docs.discord.food/topics/gateway-events#guild-create), [Guild Member Add](https://docs.discord.food/topics/gateway-events#guild-member-add), [Guild Join Request Create](https://docs.discord.food/topics/gateway-events#guild-join-request-create), [Channel Create](https://docs.discord.food/topics/gateway-events#channel-create), and/or [Relationship Add](https://docs.discord.food/topics/gateway-events#relationship-add) Gateway event.

[**JSON Params**](invitaciones.md#json-params)

| Field        | Type   | Description                                                                                              |
| ------------ | ------ | -------------------------------------------------------------------------------------------------------- |
| session\_id? | string | The session ID that is accepting the invite, required for [guest invites](invitaciones.md#guest-invites) |

#### [Delete Invite](invitaciones.md#delete-invite) <a href="#delete-invite" id="delete-invite"></a>

`DELETE/invites/{invite.code}`

Deletes an invite. Requires the permission on the channel this invite belongs to, or to remove any invite across the guild, if the invite is to a guild. Returns an [invite](invitaciones.md#invite-object) object on success. May fire an [Invite Delete](https://docs.discord.food/topics/gateway-events#invite-delete) Gateway event.

#### [Get Guild Invites](invitaciones.md#get-guild-invites) <a href="#get-guild-invites" id="get-guild-invites"></a>

`GET/guilds/{guild.id}/invites`

Returns a list of [invite](invitaciones.md#invite-object) objects (with [invite metadata](invitaciones.md#invite-metadata-object)) for the guild. Requires the permission.

#### [Get Channel Invites](invitaciones.md#get-channel-invites) <a href="#get-channel-invites" id="get-channel-invites"></a>

`GET/channels/{channel.id}/invites`

Returns a list of [invite](invitaciones.md#invite-object) objects (with [invite metadata](invitaciones.md#invite-metadata-object)) for the channel. Only usable for guild channels and group DMs. Requires the permission if the channel is in a guild.

#### [Create Channel Invite](invitaciones.md#create-channel-invite) <a href="#create-channel-invite" id="create-channel-invite"></a>

`POST/channels/{channel.id}/invites`

Creates a new [invite](invitaciones.md#invite-object) object for the channel. Only usable for guild channels and group DMs. Requires the permission if the channel is in a guild. Returns an [invite](invitaciones.md#invite-object) object (with [invite metadata](invitaciones.md#invite-metadata-object)). Fires an [Invite Create](https://docs.discord.food/topics/gateway-events#invite-create) Gateway event if the channel is in a guild.

[**JSON Params**](invitaciones.md#json-params)

| Field                    | Type      | Description                                                                                                                                                    |
| ------------------------ | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| flags? <sup>1</sup>      | integer   | The [invite's flags](invitaciones.md#invite-flags) (only `IS_GUEST_INVITE` and `IS_APPLICATION_BYPASS` can be set)                                             |
| max\_age? <sup>2</sup>   | integer   | Number of seconds before expiry, or 0 for never (0-604800, default 86400)                                                                                      |
| max\_uses?               | integer   | Max number of uses or 0 for unlimited (0-100, default 0)                                                                                                       |
| temporary?               | boolean   | Whether this invite only grants temporary membership (default false)                                                                                           |
| unique?                  | boolean   | Whether to try to reuse a similar invite (useful for creating many unique one time use invites, default false)                                                 |
| validate?                | ?string   | The invite code to validate and attempt to reuse; if nonexistant, a new invite will be created as usual                                                        |
| target\_type?            | integer   | The [type of target](invitaciones.md#invite-target-type) for this voice channel invite                                                                         |
| target\_user\_id?        | snowflake | The ID of the user whose stream to display for this invite, required if `target_type` is `STREAM`, the user must be streaming in the channel                   |
| target\_application\_id? | snowflake | The ID of the embedded application to open for this invite, required if `target_type` is `EMBEDDED_APPLICATION`, the application must have the `EMBEDDED` flag |

<sup>1</sup> Creating an invite with the flag requires the permission.

<sup>2</sup> For group DMs, is the only supported parameter, and it does not support 0.

#### [Get User Invites](invitaciones.md#get-user-invites) <a href="#get-user-invites" id="get-user-invites"></a>

`GET/users/@me/invites`

Returns a list of friend [invite](invitaciones.md#invite-object) objects (with [invite metadata](invitaciones.md#invite-metadata-object)) for the current user.

#### [Create User Invite](invitaciones.md#create-user-invite) <a href="#create-user-invite" id="create-user-invite"></a>

`POST/users/@me/invites`

Creates a new friend invite. Returns a friend [invite](invitaciones.md#invite-object) object (with [invite metadata](invitaciones.md#invite-metadata-object)) on success.

[**JSON Params**](invitaciones.md#json-params)

| Field | Type   | Description                                                   |
| ----- | ------ | ------------------------------------------------------------- |
| code? | string | The pre-generated friend invite code to create an invite from |

#### [Revoke User Invites](invitaciones.md#revoke-user-invites) <a href="#revoke-user-invites" id="revoke-user-invites"></a>

`DELETE/users/@me/invites`

Revokes all of the current user's friend invites. Returns a list of revoked friend [invite](invitaciones.md#invite-object) objects (with [invite metadata](invitaciones.md#invite-metadata-object)) on success.

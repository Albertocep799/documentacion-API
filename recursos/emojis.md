---
icon: icons
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

# Emojis

### [Emoji](emojis.md#emoji) <a href="#emoji" id="emoji"></a>

Emoji are small images that can be used to convey an idea or emotion. Discord allows users to upload custom emoji to guilds and use them like normal emoji in-chat and when reacting to messages.

#### [Emoji Object](emojis.md#emoji-object) <a href="#emoji-object" id="emoji-object"></a>

[**Emoji Structure**](emojis.md#emoji-structure)

| Field              | Type                                                                        | Description                                                                                |
| ------------------ | --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| id                 | ?snowflake                                                                  | The [ID of the emoji](https://docs.discord.food/reference#cdn-formatting)                  |
| name <sup>2</sup>  | string                                                                      | The name of the emoji (2-32 characters)                                                    |
| roles?             | array\[snowflake]                                                           | The roles allowed to use the emoji                                                         |
| user? <sup>1</sup> | partial [user](https://docs.discord.food/resources/user#user-object) object | The user that uploaded the emoji                                                           |
| require\_colons?   | boolean                                                                     | Whether this emoji must be wrapped in colons                                               |
| managed?           | boolean                                                                     | Whether this emoji is managed                                                              |
| animated?          | boolean                                                                     | Whether this emoji is animated                                                             |
| available?         | boolean                                                                     | Whether this emoji can be used; may be false due to loss of premium subscriptions (boosts) |

<sup>1</sup> Only included for emoji when fetched through the [Get Guild Emojis](emojis.md#get-guild-emojis) or [Get Guild Emoji](emojis.md#get-guild-emoji) endpoints by a user with the permission. Always included when fetched from [Get Application Emojis](emojis.md#get-application-emojis) or [Get Application Emoji](emojis.md#get-application-emoji).

<sup>2</sup> This may be in the case of a custom emoji that has been deleted.

[**Premium Emoji**](emojis.md#premium-emoji)

Roles with the tag being the guild's integration are considered subscription roles.\
An emoji cannot have both subscription roles and non-subscription roles.\
Emoji with subscription roles are considered premium emoji, and count toward a separate limit of 25.\
Emoji cannot be converted between normal and premium after creation.

[**Appication Emoji**](emojis.md#appication-emoji)

An application can own up to 2,000 emoji that can only be used by the app. The permission is not required to use these emoji. These emoji do not support role-locking and always require colons. They are never managed or unavailable.

[**Emoji Example**](emojis.md#emoji-example)

```
{  "id": "41771983429993937",  "name": "LUL",  "roles": ["41771983429993000", "41771983429993111"],  "user": {    "id": "306810730055729152",    "username": "owoer",    "avatar": "b3028be18dc56db5722bd750cf69df4e",    "discriminator": "0",    "public_flags": 4194816,    "banner": null,    "accent_color": null,    "global_name": "Eon",    "avatar_decoration_data": null,    "primary_guild": null  },  "require_colons": true,  "managed": false,  "animated": false}
```

[**Gateway Reaction Standard Emoji Example**](emojis.md#gateway-reaction-standard-emoji-example)

```
{  "id": null,  "name": "🔥"}
```

[**Gateway Reaction Custom Emoji Examples**](emojis.md#gateway-reaction-custom-emoji-examples)

```
{  "id": "41771983429993937",  "name": "LUL",  "animated": true}
```

```
{  "id": "41771983429993937",  "name": null}
```

### [Endpoints](emojis.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get Guild Emojis](emojis.md#get-guild-emojis) <a href="#get-guild-emojis" id="get-guild-emojis"></a>

`GET/guilds/{guild.id}/emojis`

Returns a list of [emoji](emojis.md#emoji-object) objects for the given guild. Includes the field if the user has the or permission.

#### [Get Guild Emoji](emojis.md#get-guild-emoji) <a href="#get-guild-emoji" id="get-guild-emoji"></a>

`GET/guilds/{guild.id}/emojis/{emoji.id}`

Returns an [emoji](emojis.md#emoji-object) object for the given guild and emoji IDs. Includes the field if the user has the or permission.

#### [Get Guild Top Emojis](emojis.md#get-guild-top-emojis) <a href="#get-guild-top-emojis" id="get-guild-top-emojis"></a>

`GET/guilds/{guild.id}/top-emojis`

Returns the most-used emojis for the given guild.

[**Response Body**](emojis.md#response-body)

| Field | Type                                                      | Description                        |
| ----- | --------------------------------------------------------- | ---------------------------------- |
| items | array\[[top emoji](emojis.md#top-emoji-structure) object] | The most-used emojis for the guild |

[**Top Emoji Structure**](emojis.md#top-emoji-structure)

| Field       | Type      | Description                   |
| ----------- | --------- | ----------------------------- |
| emoji\_id   | snowflake | The ID of the emoji           |
| emoji\_rank | integer   | The overall rank of the emoji |

[**Example Response**](emojis.md#example-response)

```
{  "items": [    {      "emoji_id": "1145727546747535412",      "emoji_rank": 1    },    {      "emoji_id": "1174435954090594505",      "emoji_rank": 2    },    {      "emoji_id": "1029462631163117629",      "emoji_rank": 3    },    {      "emoji_id": "1030570693903011921",      "emoji_rank": 4    },    {      "emoji_id": "1077714345825407067",      "emoji_rank": 5    }  ]}
```

#### [Get Emoji Guild](emojis.md#get-emoji-guild) <a href="#get-emoji-guild" id="get-emoji-guild"></a>

`GET/emojis/{emoji.id}/guild`

Returns a [discoverable guild](https://docs.discord.food/resources/discovery#discoverable-guild-object) object for the guild that owns the given emoji. This endpoint requires the guild to be discoverable, not be [auto-removed](https://docs.discord.food/resources/discovery#discoverable-guild-object), and have [guild expression discoverability](https://docs.discord.food/resources/discovery#discovery-metadata-object) enabled.

#### [Get Emoji Source](emojis.md#get-emoji-source) <a href="#get-emoji-source" id="get-emoji-source"></a>

`GET/emojis/{emoji.id}/source`

Returns an object containing information on the guild or application that owns the given emoji. If the source is a guild, this endpoint requires the guild to be discoverable, not be [auto-removed](https://docs.discord.food/resources/discovery#discoverable-guild-object), and have [guild expression discoverability](https://docs.discord.food/resources/discovery#discovery-metadata-object) enabled.

[**Response Body**](emojis.md#response-body)

| Field       | Type                                                               | Description                                             |
| ----------- | ------------------------------------------------------------------ | ------------------------------------------------------- |
| type        | string                                                             | The [type of emoji source](emojis.md#emoji-source-type) |
| guild       | ?[emoji guild](emojis.md#emoji-guild-structure) object             | The guild that owns the given emoji                     |
| application | ?[emoji application](emojis.md#emoji-application-structure) object | The application that owns the given emoji               |

[**Emoji Source Type**](emojis.md#emoji-source-type)

| Value       | Description                             |
| ----------- | --------------------------------------- |
| GUILD       | The emoji is uploaded to a guild        |
| APPLICATION | The emoji is uploaded to an application |

[**Emoji Guild Structure**](emojis.md#emoji-guild-structure)

| Field                        | Type                                           | Description                                                                                      |
| ---------------------------- | ---------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| id                           | snowflake                                      | The ID of the guild                                                                              |
| name                         | string                                         | The name of the guild (2-100 characters)                                                         |
| icon                         | ?string                                        | The guild's [icon hash](https://docs.discord.food/reference#cdn-formatting)                      |
| description                  | ?string                                        | The description for the guild (max 300 characters)                                               |
| features                     | array\[string]                                 | Enabled [guild features](https://docs.discord.food/resources/guild#guild-features)               |
| emojis                       | array\[[emoji](emojis.md#emoji-object) object] | Custom guild emojis                                                                              |
| premium\_tier                | integer                                        | The guild's [premium tier](https://docs.discord.food/resources/guild#premium-tier) (boost level) |
| premium\_subscription\_count | integer                                        | The number of premium subscriptions (boosts) the guild currently has                             |
| approximate\_member\_count   | integer                                        | Approximate number of total members in the guild                                                 |
| approximate\_presence\_count | integer                                        | Approximate number of non-offline members in the guild                                           |

[**Emoji Application Structure**](emojis.md#emoji-application-structure)

| Field | Type      | Description                 |
| ----- | --------- | --------------------------- |
| id    | snowflake | The ID of the application   |
| name  | string    | The name of the application |

[**Example Response**](emojis.md#example-response)

```
{  "guild": {    "id": "322850917248663552",    "name": "Official Fortnite",    "icon": "39e9f70c87a3d20dfe02e5f013b417f4",    "emojis": [],    "approximate_member_count": 1068003,    "approximate_presence_count": 163598,    "description": "The Official Fortnite Discord Server! Join to follow news & updates, LFG, and chat about Fortnite Battle Royale.",    "features": [      "ANIMATED_BANNER",      "ANIMATED_ICON",      "AUTO_MODERATION",      "BANNER",      "CHANNEL_ICON_EMOJIS_GENERATED",      "COMMUNITY",      "COMMUNITY_EXP_LARGE_UNGATED",      "DISCOVERABLE",      "FEATURABLE",      "GUILD_MEMBER_VERIFICATION_EXPERIMENT",      "GUILD_WEB_PAGE_VANITY_URL",      "INVITE_SPLASH",      "MEMBER_PROFILES",      "MEMBER_VERIFICATION_GATE_ENABLED",      "NEWS",      "NEW_THREAD_PERMISSIONS",      "PREVIEW_ENABLED",      "PRIVATE_THREADS",      "ROLE_ICONS",      "SEVEN_DAY_THREAD_ARCHIVE",      "SOUNDBOARD",      "THREADS_ENABLED",      "THREE_DAY_THREAD_ARCHIVE",      "VANITY_URL",      "VERIFIED",      "VIP_REGIONS"    ],    "premium_subscription_count": 378,    "premium_tier": 3  },  "application": null,  "type": "GUILD"}
```

#### [Create Guild Emoji](emojis.md#create-guild-emoji) <a href="#create-guild-emoji" id="create-guild-emoji"></a>

`POST/guilds/{guild.id}/emojis`

Creates a new emoji for the guild. Requires the permission. Returns the new [emoji](emojis.md#emoji-object) object on success. Fires a [Guild Emojis Update](https://docs.discord.food/topics/gateway-events#guild-emojis-update) Gateway event.

<details>

<summary>Emoji LimitsHow are emoji slots calculated?</summary>

Each guild has limits for each type of emoji. The types, or buckets, are:

* **Normal emoji**: Default non-animated emoji
* **Animated emoji**: Animated emoji
* **Premium emoji**: Emoji that is locked to role subscriptions

The default emoji limit is 50. For normal and animated emoji, the maximum is applied individually, and depends on the guild's [premium tier](https://support.discord.com/hc/en-us/articles/360028038352) and [features](https://docs.discord.food/resources/guild#guild-features). Therefore, the maximum number of emoji is calculated as follows:

* **Normal emoji**:
* **Animated emoji**:
* **Premium emoji**: (constant)

If this is confusing, the limits are also summarized in the following table by [premium tier](https://docs.discord.food/resources/guild#premium-tier). Note that if the guild has the [feature](https://docs.discord.food/resources/guild#guild-features), the normal and animated limit is instead 200.

| Premium Tier | Normal Emoji | Animated Emoji | Premium Emoji |
| ------------ | ------------ | -------------- | ------------- |
| `NONE`       | 50           | 50             | 25            |
| `TIER_1`     | 100          | 100            | 25            |
| `TIER_2`     | 150          | 150            | 25            |
| `TIER_3`     | 250          | 250            | 25            |

</details>

[**JSON Params**](emojis.md#json-params)

| Field  | Type                                                       | Description                             |
| ------ | ---------------------------------------------------------- | --------------------------------------- |
| name   | string                                                     | The name of the emoji (2-32 characters) |
| image  | [image data](https://docs.discord.food/reference#cdn-data) | 128x128 emoji image                     |
| roles? | array\[snowflake]                                          | The roles allowed to use this emoji     |

#### [Modify Guild Emoji](emojis.md#modify-guild-emoji) <a href="#modify-guild-emoji" id="modify-guild-emoji"></a>

`PATCH/guilds/{guild.id}/emojis/{emoji.id}`

Modifies the given emoji. For emojis created by the current user, requires either the or permission. For other emojis, requires the permission. Returns the updated [emoji](emojis.md#emoji-object) object on success. Fires a [Guild Emojis Update](https://docs.discord.food/topics/gateway-events#guild-emojis-update) Gateway event.

[**JSON Params**](emojis.md#json-params)

| Field  | Type               | Description                             |
| ------ | ------------------ | --------------------------------------- |
| name?  | string             | The name of the emoji (2-32 characters) |
| roles? | ?array\[snowflake] | The roles allowed to use this emoji     |

#### [Delete Guild Emoji](emojis.md#delete-guild-emoji) <a href="#delete-guild-emoji" id="delete-guild-emoji"></a>

`DELETE/guilds/{guild.id}/emojis/{emoji.id}`

Deletes the given emoji. For emojis created by the current user, requires either the or permission. For other emojis, requires the permission. Returns a 204 empty response on success. Fires a [Guild Emojis Update](https://docs.discord.food/topics/gateway-events#guild-emojis-update) Gateway event.

#### [Get Application Emojis](emojis.md#get-application-emojis) <a href="#get-application-emojis" id="get-application-emojis"></a>

`GET/applications/{application.id}/emojis`

Returns an object containing a list of [emoji](emojis.md#emoji-object) objects for the given application under the key. Includes the field.

[**Response Body**](emojis.md#response-body)

| Field | Type                                           | Description                            |
| ----- | ---------------------------------------------- | -------------------------------------- |
| items | array\[[emoji](emojis.md#emoji-object) object] | The emojis uploaded to the application |

#### [Get Application Emoji](emojis.md#get-application-emoji) <a href="#get-application-emoji" id="get-application-emoji"></a>

`GET/applications/{application.id}/emojis/{emoji.id}`

Returns an [emoji](emojis.md#emoji-object) object for the given application and emoji IDs. Includes the field.

#### [Create Application Emoji](emojis.md#create-application-emoji) <a href="#create-application-emoji" id="create-application-emoji"></a>

`POST/applications/{application.id}/emojis`

Creates a new emoji for the application. Returns the new [emoji](emojis.md#emoji-object) object on success.

[**JSON Params**](emojis.md#json-params)

| Field             | Type                                                       | Description                             |
| ----------------- | ---------------------------------------------------------- | --------------------------------------- |
| name <sup>1</sup> | string                                                     | The name of the emoji (2-32 characters) |
| image             | [image data](https://docs.discord.food/reference#cdn-data) | 128x128 emoji image                     |

<sup>1</sup> The names of application emoji must be unique.

#### [Modify Application Emoji](emojis.md#modify-application-emoji) <a href="#modify-application-emoji" id="modify-application-emoji"></a>

`PATCH/applications/{application.id}/emojis/{emoji.id}`

Modifies the given emoji. Returns the updated [emoji](emojis.md#emoji-object) object on success.

[**JSON Params**](emojis.md#json-params)

| Field              | Type   | Description                             |
| ------------------ | ------ | --------------------------------------- |
| name? <sup>1</sup> | string | The name of the emoji (2-32 characters) |

#### [Delete Application Emoji](emojis.md#delete-application-emoji) <a href="#delete-application-emoji" id="delete-application-emoji"></a>

`DELETE/applications/{application.id}/emojis/{emoji.id}`

Delete the given emoji. Returns a 204 empty response on success.

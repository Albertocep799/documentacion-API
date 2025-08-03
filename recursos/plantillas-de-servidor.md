---
icon: sim-cards
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

# Plantillas de servidor

### [Guild Templates](plantillas-de-servidor.md#guild-templates) <a href="#guild-templates" id="guild-templates"></a>

Templates represent a code that when used, creates a guild based on a snapshot of an existing guild. For a list of official templates, refer to [this Gist](https://gist.github.com/dolfies/f70b1de53a1d185d58563b9ca8bb248d).

#### [Guild Template Object](plantillas-de-servidor.md#guild-template-object) <a href="#guild-template-object" id="guild-template-object"></a>

[**Guild Template Structure**](plantillas-de-servidor.md#guild-template-structure)

| Field                                  | Type                                                                           | Description                                            |
| -------------------------------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------ |
| code                                   | string                                                                         | The code of the template (unique ID)                   |
| name                                   | string                                                                         | The name of the template (1-100 characters)            |
| description                            | ?string                                                                        | The description for the template (max 120 characters)  |
| usage\_count                           | integer                                                                        | Number of times this template has been used            |
| creator\_id                            | snowflake                                                                      | The ID of the user who created the template            |
| creator                                | partial [user](https://docs.discord.food/resources/user#user-object) object    | The user who created the template                      |
| created\_at                            | ISO8601 timestamp                                                              | When this template was created                         |
| updated\_at                            | ISO8601 timestamp                                                              | When this template was last synced to the source guild |
| source\_guild\_id                      | snowflake                                                                      | The ID of the guild this template is based on          |
| serialized\_source\_guild <sup>1</sup> | partial [guild](https://docs.discord.food/resources/guild#guild-object) object | The guild snapshot this template contains              |
| is\_dirty                              | ?boolean                                                                       | Whether the template has unsynced changes              |

<sup>1</sup> This partial guild object is special in that it and the objects within always contain applicable optional fields (even if they're not applicable). This leads to unexpected behavior, such as being serialized for voice channels. Additionally, the main guild object is missing the field, and all other fields are not real snowflakes.

[**Example Guild Template Object**](plantillas-de-servidor.md#example-guild-template-object)

```
{  "code": "2TffvPucqHkN",  "name": "Blank Server",  "description": null,  "usage_count": 34729,  "creator_id": "268473310986240001",  "creator": {    "id": "268473310986240001",    "username": "discordapp",    "avatar": "f749bb0cbeeb26ef21eca719337d20f1",    "discriminator": "0",    "public_flags": 4325376,    "banner": null,    "accent_color": null,    "global_name": null,    "avatar_decoration_data": null,    "primary_guild": null  },  "created_at": "2020-04-17T20:59:35+00:00",  "updated_at": "2020-04-17T20:59:35+00:00",  "source_guild_id": "700811170902179862",  "serialized_source_guild": {    "name": "Blank Server",    "description": null,    "region": "us-west",    "verification_level": 0,    "default_message_notifications": 0,    "explicit_content_filter": 0,    "preferred_locale": "en-US",    "afk_channel_id": null,    "afk_timeout": 300,    "system_channel_id": 2,    "system_channel_flags": 0,    "roles": [      {        "id": 0,        "name": "@everyone",        "permissions": "2248329584430657",        "color": 0,        "hoist": false,        "mentionable": false,        "icon": null,        "unicode_emoji": null      }    ],    "channels": [      {        "id": 1,        "type": 4,        "name": "Text Channels",        "position": 0,        "topic": null,        "bitrate": 64000,        "user_limit": 0,        "nsfw": false,        "rate_limit_per_user": 0,        "parent_id": null,        "default_auto_archive_duration": null,        "permission_overwrites": [],        "available_tags": null,        "template": "",        "default_reaction_emoji": null,        "default_thread_rate_limit_per_user": null,        "default_sort_order": null,        "default_forum_layout": null,        "icon_emoji": null,        "theme_color": null      },      {        "id": 2,        "type": 0,        "name": "general",        "position": 0,        "topic": null,        "bitrate": 64000,        "user_limit": 0,        "nsfw": false,        "rate_limit_per_user": 0,        "parent_id": 1,        "default_auto_archive_duration": null,        "permission_overwrites": [],        "available_tags": null,        "template": "",        "default_reaction_emoji": null,        "default_thread_rate_limit_per_user": null,        "default_sort_order": null,        "default_forum_layout": null,        "icon_emoji": null,        "theme_color": null      },      {        "id": 3,        "type": 4,        "name": "Voice Channels",        "position": 0,        "topic": null,        "bitrate": 64000,        "user_limit": 0,        "nsfw": false,        "rate_limit_per_user": 0,        "parent_id": null,        "default_auto_archive_duration": null,        "permission_overwrites": [],        "available_tags": null,        "template": "",        "default_reaction_emoji": null,        "default_thread_rate_limit_per_user": null,        "default_sort_order": null,        "default_forum_layout": null,        "icon_emoji": null,        "theme_color": null      },      {        "id": 4,        "type": 2,        "name": "General",        "position": 0,        "topic": null,        "bitrate": 64000,        "user_limit": 0,        "nsfw": false,        "rate_limit_per_user": 0,        "parent_id": 3,        "default_auto_archive_duration": null,        "permission_overwrites": [],        "available_tags": null,        "template": "",        "default_reaction_emoji": null,        "default_thread_rate_limit_per_user": null,        "default_sort_order": null,        "default_forum_layout": null,        "icon_emoji": null,        "theme_color": null      }    ]  },  "is_dirty": null}
```

### [Endpoints](plantillas-de-servidor.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get Guild Template](plantillas-de-servidor.md#get-guild-template) <a href="#get-guild-template" id="get-guild-template"></a>

`GET/guilds/templates/{template.code}`

Returns a [guild template](plantillas-de-servidor.md#guild-template-object) object for the given code.

#### [Use Guild Template](plantillas-de-servidor.md#use-guild-template) <a href="#use-guild-template" id="use-guild-template"></a>

`POST/guilds/templates/{template.code}`

Create a new guild based on a template. Returns a [guild](https://docs.discord.food/resources/guild#guild-object) object on success. Fires a [Guild Create](https://docs.discord.food/topics/gateway-events#guild-create) Gateway event.

[**JSON Params**](plantillas-de-servidor.md#json-params)

| Field | Type                                                       | Description                              |
| ----- | ---------------------------------------------------------- | ---------------------------------------- |
| name  | string                                                     | The name of the guild (2-100 characters) |
| icon? | [image data](https://docs.discord.food/reference#cdn-data) | 128x128 image for the guild's icon       |

#### [Get Guild Templates](plantillas-de-servidor.md#get-guild-templates) <a href="#get-guild-templates" id="get-guild-templates"></a>

`GET/guilds/{guild.id}/templates`

Returns an array of [guild template](plantillas-de-servidor.md#guild-template-object) objects. Requires the permission.

#### [Create Guild Template](plantillas-de-servidor.md#create-guild-template) <a href="#create-guild-template" id="create-guild-template"></a>

`POST/guilds/{guild.id}/templates`

Creates a template for the guild. Requires the permission. Returns the created [guild template](plantillas-de-servidor.md#guild-template-object) object on success.

[**JSON Params**](plantillas-de-servidor.md#json-params)

| Field        | Type    | Description                                           |
| ------------ | ------- | ----------------------------------------------------- |
| name         | string  | The name of the template (1-100 characters)           |
| description? | ?string | The description for the template (max 120 characters) |

#### [Sync Guild Template](plantillas-de-servidor.md#sync-guild-template) <a href="#sync-guild-template" id="sync-guild-template"></a>

`PUT/guilds/{guild.id}/templates/{template.code}`

Syncs the template to the guild's current state. Requires the permission. Returns the updated [guild template](plantillas-de-servidor.md#guild-template-object) object on success.

#### [Modify Guild Template](plantillas-de-servidor.md#modify-guild-template) <a href="#modify-guild-template" id="modify-guild-template"></a>

`PATCH/guilds/{guild.id}/templates/{template.code}`

Modifies the template's metadata. Requires the permission. Returns the updated [guild template](plantillas-de-servidor.md#guild-template-object) object on success.

[**JSON Params**](plantillas-de-servidor.md#json-params)

| Field        | Type    | Description                                           |
| ------------ | ------- | ----------------------------------------------------- |
| name?        | string  | The name of the template (1-100 characters)           |
| description? | ?string | The description for the template (max 120 characters) |

#### [Delete Guild Template](plantillas-de-servidor.md#delete-guild-template) <a href="#delete-guild-template" id="delete-guild-template"></a>

`DELETE/guilds/{guild.id}/templates/{template.code}`

Deletes the template. Requires the permission. Returns the deleted [guild template](plantillas-de-servidor.md#guild-template-object) object on success.

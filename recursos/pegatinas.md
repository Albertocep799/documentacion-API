---
icon: note-sticky
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

# Pegatinas

Stickers are embedded images that can be sent along with messages. They can be either standard stickers, which are official, first-party stickers, or guild stickers, which are custom stickers uploaded by users in a guild.

### Sticker Pack Object

A pack of standard stickers.

**Sticker Pack Structure**

\| Field | Type | Description | | ----------------- | ---------------------------------------- | ---------------------------------------------------------------------- | | id | snowflake | The ID of the sticker pack | | stickers | array\[[sticker](pegatinas.md#sticker-object) object] | The stickers in the pack | | name | string | The name of the sticker pack | | sku\_id | snowflake | The ID of the pack's SKU | | cover\_sticker\_id? | snowflake | The ID of a sticker in the pack which is shown as the pack's icon | | description | string | The description for the sticker pack | | banner\_asset\_id? | snowflake | The ID of the sticker pack's banner image |

**Example Sticker Pack**

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

"banner_asset_id": "761773777976819732"
}
`

### Sticker Object

A sticker that can be sent in messages.

###### Sticker Structure

| Field       | Type                                               | Description                                                                                        |
| ----------- | -------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| id          | snowflake                                          | The [ID of the sticker](/reference#cdn-formatting)                                                 |
| pack_id?    | snowflake                                          | For standard stickers, ID of the pack the sticker is from                                          |
| name        | string                                             | The name of the sticker (2-30 characters)                                                          |
| description | ?string                                            | The description for the sticker (max 100 characters)                                               |
| tags ^2^    | string                                             | Autocomplete/suggestion tags for the sticker (1-200 characters)                                    |
| type        | integer                                            | The [type of sticker](#sticker-types)                                                              |
| format_type | integer                                            | The [type of format](#sticker-format-types) for the sticker                                        |
| available?  | boolean                                            | Whether this guild sticker can be used; may be false due to loss of premium subscriptions (boosts) |
| guild_id?   | snowflake                                          | The ID of the guild the sticker is attached to                                                     |
| user? ^1^   | partial [user](/resources/user#user-object) object | The user that uploaded the guild sticker                                                           |
| sort_value? | integer                                            | The standard sticker's sort order within its pack                                                  |

^1^ Only included for guild stickers when fetched through the [Get Guild Stickers](#get-guild-stickers) or [Get Guild Sticker](#get-guild-sticker) endpoints by a user with the `MANAGE_EXPRESSIONS` permission.

^2^ A comma separated list of keywords is the format used in this field by standard stickers, but this is just a convention.
Incidentally, official clients will always use a name generated from an emoji as the value of this field when creating or modifying a guild sticker.

###### Sticker Types

| Value | Name     | Description                                                 |
| ----- | -------- | ----------------------------------------------------------- |
| 1     | STANDARD | An official sticker in a current or legacy purchasable pack |
| 2     | GUILD    | A sticker uploaded to a guild for the guild's members       |

###### Sticker Format Types

> ⚠️ ALERTA: 

GIF stickers are not available through the [CDN](/reference#cdn-formatting), and must be accessed at `https://media.discordapp.net/stickers/{sticker_id}.gif`.


| Value | Name   | Description                                                                                                                                      |
| ----- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1     | PNG    | A PNG image                                                                                                                                      |
| 2     | APNG   | An animated PNG image, using the APNG format                                                                                                     |
| 3     | LOTTIE | A [lottie](https://airbnb.design/lottie/) animation; requires the `VERIFIED` and/or `PARTNERED` [guild feature](/resources/guild#guild-features) |
| 4     | GIF    | An animated GIF image                                                                                                                            |

###### Example Sticker

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

```
```

```
```

```

"sort_value": 12
}
`

### Sticker Item Object

The smallest amount of data required to render a sticker. A partial sticker object.

###### Sticker Item Structure

| Field       | Type      | Description                                                 |
| ----------- | --------- | ----------------------------------------------------------- |
| id          | snowflake | The [ID of the sticker](/reference#cdn-formatting)          |
| name        | string    | The name of the sticker                                     |
| format_type | integer   | The [type of format](#sticker-format-types) for the sticker |

## Endpoints

> 📋 HEADER: 
Get Sticker Packs


Returns the list of [sticker packs](#sticker-pack-object) available to use.

###### Response Body

| Field         | Type                                               | Description                 |
| ------------- | -------------------------------------------------- | --------------------------- |
| sticker_packs | array[[sticker pack](#sticker-pack-object) object] | The sticker packs available |

> 📋 HEADER: 
Get Sticker Pack


Returns a [sticker pack](#sticker-pack-object) object for the given pack ID.

> 📋 HEADER: 
Get Sticker


Returns a [sticker](#sticker-object) object for the given sticker ID.

> 📋 HEADER: 
Get Sticker Guild


Returns a [discoverable guild](/resources/discovery#discoverable-guild-object) object for the guild that owns the given sticker. This endpoint requires the guild to be discoverable, not be [auto-removed](/resources/discovery#discoverable-guild-object), and have [guild expression discoverability](/resources/discovery#discovery-metadata-object) enabled.

> 📋 HEADER: 
Get Guild Stickers


Returns an array of [sticker](#sticker-object) objects for the given guild. Includes the `user` field if the user has the `CREATE_EXPRESSIONS` or `MANAGE_EXPRESSIONS` permission.

> 📋 HEADER: 
Get Guild Sticker


Returns a [sticker](#sticker-object) object for the given guild and sticker IDs. Includes the `user` field if the user has the `CREATE_EXPRESSIONS` or `MANAGE_EXPRESSIONS` permission.

> 📋 HEADER: 
Create Guild Sticker


Creates a new sticker for the guild. Must be a `multipart/form-data` body. Requires the `CREATE_EXPRESSIONS` permission. Returns the new [sticker](#sticker-object) object on success. Fires a [Guild Stickers Update](/topics/gateway-events#guild-stickers-update) Gateway event.

Every guilds has five free sticker slots by default, and each premium tier (boost level) will grant access to more slots.

> ⚠️ ALERTA: 

Lottie stickers can only be uploaded on guilds that have either the `VERIFIED` and/or `PARTNERED` [guild feature](/resources/guild#guild-features).


> ⚠️ ALERTA: 

Stickers have a maximum file size of **500 KiB** (**1.5 MiB** for employees). Attempting to upload a sticker larger than this limit will fail with a 400 bad request.


> ⬇️ COLAPSABLE: 

Sticker limits are applied to the total amount of stickers in the guild, making them a lot simpler than emoji limits. The default sticker limit is 50.
The real limit depends on the guild's [premium tier](https://support.discord.com/hc/en-us/articles/360028038352) and [features](/resources/guild#guild-features).

These limits are summarized in the following table by [premium tier](/resources/guild#premium-tier). Note that if the guild has the [`MORE_STICKERS` feature](/resources/guild#guild-features), the applied limit is always the tier 3 one (60).

| Premium Tier | Sticker Limit |
| ------------ | ------------- |
| `NONE`       | 5             |
| `TIER_1`     | 15            |
| `TIER_2`     | 30            |
| `TIER_3`     | 60            |


###### Form Params

| Field       | Type          | Description                                                               |
| ----------- | ------------- | ------------------------------------------------------------------------- |
| name        | string        | The name of the sticker (2-30 characters)                                 |
| description | string        | The description for the sticker (max 100 characters)                      |
| tags        | string        | Autocomplete/suggestion tags for the sticker (1-200 characters)           |
| file        | file contents | The sticker file to upload, must be a PNG, APNG, GIF, or Lottie JSON file |

> 📋 HEADER: 
Modify Guild Sticker


Modifies the given sticker. For stickers created by the current user, requires either the `CREATE_EXPRESSIONS` or `MANAGE_EXPRESSIONS` permission. For other stickers, requires the `MANAGE_EXPRESSIONS` permission. Returns the updated [sticker](#sticker-object) object on success. Fires a [Guild Stickers Update](/topics/gateway-events#guild-stickers-update) Gateway event.

###### JSON Params

| Field        | Type    | Description                                                     |
| ------------ | ------- | --------------------------------------------------------------- |
| name?        | string  | The name of the sticker (2-30 characters)                       |
| description? | ?string | The description for the sticker (max 100 characters)            |
| tags?        | string  | Autocomplete/suggestion tags for the sticker (1-200 characters) |

> 📋 HEADER: 
Delete Guild Sticker


Deletes the given sticker. For stickers created by the current user, requires either the `CREATE_EXPRESSIONS` or `MANAGE_EXPRESSIONS` permission. For other stickers, requires the `MANAGE_EXPRESSIONS` permission. Returns a 204 empty response on success. Fires a [Guild Stickers Update](/topics/gateway-events#guild-stickers-update) Gateway event.

```

---
icon: circle-nodes
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

# Integraciones

### [Integrations](integraciones.md#integrations) <a href="#integrations" id="integrations"></a>

Integrations represent a connection between a service and a guild. This may include third-party services such as Twitch or YouTube, Discord-housed integrations such as bots, or internal integrations such as role subscriptions.

#### [Integration Object](integraciones.md#integration-object) <a href="#integration-object" id="integration-object"></a>

[**Integration Structure**](integraciones.md#integration-structure)

| Field                                                 | Type                                                                                                                                               | Description                                                                                                  |
| ----------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| id <sup>1</sup>                                       | snowflake                                                                                                                                          | The ID of the integration                                                                                    |
| name                                                  | string                                                                                                                                             | The name of the integration                                                                                  |
| type                                                  | string                                                                                                                                             | The [type of integration](integraciones.md#integration-type)                                                 |
| enabled                                               | boolean                                                                                                                                            | Whether this integration is enabled                                                                          |
| account                                               | [integration account](integraciones.md#integration-account-structure) object                                                                       | Integration account information                                                                              |
| syncing? <sup>2</sup>                                 | boolean                                                                                                                                            | Whether this integration is syncing                                                                          |
| role\_id? <sup>2</sup>                                | snowflake                                                                                                                                          | Role ID that this integration uses for subscribers                                                           |
| enable\_emoticons? <sup>2</sup>                       | boolean                                                                                                                                            | Whether emoticons should be synced for this integration (Twitch only)                                        |
| expire\_behavior? <sup>2</sup>                        | integer                                                                                                                                            | The [behavior of expiring subscribers](integraciones.md#integration-expire-behavior)                         |
| expire\_grace\_period? <sup>2</sup>                   | integer                                                                                                                                            | The grace period before expiring subscribers (one of 1, 3, 7, 14, 30, in days)                               |
| synced\_at? <sup>2</sup>                              | ISO8601 timestamp                                                                                                                                  | When this integration was last synced                                                                        |
| subscriber\_count? <sup>2</sup>                       | integer                                                                                                                                            | How many subscribers this integration has                                                                    |
| revoked? <sup>2</sup>                                 | boolean                                                                                                                                            | Whether this integration has been revoked                                                                    |
| application? <sup>3</sup>                             | [integration application](integraciones.md#integration-application-structure) object                                                               | The integrated OAuth2 application                                                                            |
| scopes? <sup>3</sup>                                  | array\[string]                                                                                                                                     | The [scopes](https://docs.discord.food/topics/oauth2#oauth2-scopes) the application has been authorized with |
| role\_connections\_metadata <sup>3</sup> <sup>4</sup> | array\[[application role connection metadata](https://docs.discord.food/resources/application#application-role-connection-metadata-object) object] | The metadata that the application has set for role connections                                               |
| user? <sup>5</sup>                                    | partial [user](https://docs.discord.food/resources/user#user-object) object                                                                        | The user that added this integration                                                                         |

<sup>1</sup> This field may also be the literal string "twitch-partners" to represent the Twitch Partners integration.

<sup>2</sup> Only provided for Twitch and YouTube integrations.

<sup>3</sup> Only provided for Discord application integrations.

<sup>4</sup> Only included when fetched from [Get Guild Integrations](integraciones.md#get-guild-integrations) with set to .

<sup>5</sup> Only included for integrations when fetched through the [Get Guild Integrations](integraciones.md#get-guild-integrations) endpoint. Some older or internally-created integrations may not have an attached user.

[**Integration Type**](integraciones.md#integration-type)

| Value               | Name                                   |
| ------------------- | -------------------------------------- |
| twitch              | Twitch integration                     |
| youtube             | YouTube integration                    |
| discord             | Discord application integration        |
| guild\_subscription | Internal role subscription integration |

[**Integration Expire Behavior**](integraciones.md#integration-expire-behavior)

| Value | Name         | Description                                            |
| ----- | ------------ | ------------------------------------------------------ |
| 0     | REMOVE\_ROLE | Remove the subscriber role from the user on expiration |
| 1     | KICK         | Remove the user from the guild on expiration           |

[**Integration Account Structure**](integraciones.md#integration-account-structure)

| Field | Type   | Description             |
| ----- | ------ | ----------------------- |
| id    | string | The ID of the account   |
| name  | string | The name of the account |

#### [Integration Application Object](integraciones.md#integration-application-object) <a href="#integration-application-object" id="integration-application-object"></a>

[**Integration Application Structure**](integraciones.md#integration-application-structure)

| Field                                              | Type                                                                                                     | Description                                                                                                                                                                          |
| -------------------------------------------------- | -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| id                                                 | snowflake                                                                                                | The ID of the application                                                                                                                                                            |
| name                                               | string                                                                                                   | The name of the application                                                                                                                                                          |
| description                                        | string                                                                                                   | The description of the application                                                                                                                                                   |
| icon                                               | ?string                                                                                                  | The application's [icon hash](https://docs.discord.food/reference#cdn-formatting)                                                                                                    |
| cover\_image?                                      | string                                                                                                   | The application's default rich presence invite [cover image hash](https://docs.discord.food/reference#cdn-formatting)                                                                |
| splash?                                            | string                                                                                                   | The application's [splash hash](https://docs.discord.food/reference#cdn-formatting)                                                                                                  |
| type                                               | ?integer                                                                                                 | The [type of the application](https://docs.discord.food/resources/application#application-type), if any                                                                              |
| primary\_sku\_id?                                  | snowflake                                                                                                | The ID of the application's primary SKU (game, application subscription, etc.)                                                                                                       |
| bot?                                               | partial [user](https://docs.discord.food/resources/user#user-object) object                              | The bot attached to this application                                                                                                                                                 |
| deeplink\_uri?                                     | ?string                                                                                                  | The URL used for deep linking during [OAuth2 authorization](https://docs.discord.food/topics/oauth2) on mobile devices                                                               |
| third\_party\_skus?                                | array\[[application SKU](https://docs.discord.food/resources/application#application-sku-object) object] | The third party SKUs of the application's game                                                                                                                                       |
| role\_connections\_verification\_url? <sup>1</sup> | ?string                                                                                                  | The role connection verification entry point of the integration; when configured, this will render the application as a verification method in guild role verification configuration |
| is\_verified                                       | boolean                                                                                                  | Whether the application is verified                                                                                                                                                  |
| is\_discoverable                                   | boolean                                                                                                  | Whether the application is discoverable in the application directory                                                                                                                 |
| is\_monetized                                      | boolean                                                                                                  | Whether the application has monetization enabled                                                                                                                                     |

<sup>1</sup> Only present when fetched from the [Get Guild Integrations](integraciones.md#get-guild-integrations) endpoint with set to .

#### [Integration Guild Object](integraciones.md#integration-guild-object) <a href="#integration-guild-object" id="integration-guild-object"></a>

[**Integration Guild Structure**](integraciones.md#integration-guild-structure)

| Field | Type      | Description                                                                 |
| ----- | --------- | --------------------------------------------------------------------------- |
| id    | snowflake | The ID of the guild                                                         |
| name  | string    | The name of the guild (2-100 characters)                                    |
| icon  | ?string   | The guild's [icon hash](https://docs.discord.food/reference#cdn-formatting) |

#### [GIF Object](integraciones.md#gif-object) <a href="#gif-object" id="gif-object"></a>

[**GIF Structure**](integraciones.md#gif-structure)

| Field                  | Type    | Description                                      |
| ---------------------- | ------- | ------------------------------------------------ |
| id                     | string  | The ID of the GIF                                |
| title **(deprecated)** | string  | The title of the GIF                             |
| url                    | string  | The provider source URL of the GIF               |
| src                    | string  | The media URL of the GIF in the requested format |
| gif\_src               | string  | The media URL of the GIF in GIF format           |
| preview                | string  | A preview image of the GIF                       |
| width                  | integer | Width of image                                   |
| height                 | integer | Height of image                                  |

[**GIF Media Format**](integraciones.md#gif-media-format)

| Value     | Description                          |
| --------- | ------------------------------------ |
| mp4       | MP4 video                            |
| tinymp4   | MP4 video in a smaller size          |
| nanomp4   | MP4 video in a very small size       |
| loopedmp4 | MP4 video that loops (same as `mp4`) |
| webm      | WebM video                           |
| tinywebm  | WebM video in a smaller size         |
| nanowebm  | WebM video in a very small size      |
| gif       | GIF image                            |
| mediumgif | GIF image in a medium size           |
| tinygif   | GIF image in a smaller size          |
| nanogif   | GIF image in a very small size       |

[**Example GIF**](integraciones.md#example-gif)

```
{  "id": "12409989992265318124",  "title": "",  "url": "https://tenor.com/view/tasha-steelz-gif-25509948",  "src": "https://media.tenor.com/rDkkJaMgfuwAAAP4/tasha-steelz.webm",  "gif_src": "https://media.tenor.com/rDkkJaMgfuwAAAAC/tasha-steelz.gif",  "width": 150,  "height": 84,  "preview": "https://media.tenor.com/rDkkJaMgfuwAAAAD/tasha-steelz.png"}
```

### [Endpoints](integraciones.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get Guild Integrations](integraciones.md#get-guild-integrations) <a href="#get-guild-integrations" id="get-guild-integrations"></a>

`GET/guilds/{guild.id}/integrations`

Returns a list of [integration](integraciones.md#integration-object) objects for the guild. Requires the permission.

[**Query String Parameters**](integraciones.md#query-string-parameters)

| Field                                 | Type    | Description                                                                                       |
| ------------------------------------- | ------- | ------------------------------------------------------------------------------------------------- |
| has\_commands?                        | boolean | Whether to only include Discord application integrations with registered commands (default false) |
| include\_role\_connections\_metadata? | boolean | Whether to include integration role connection metadata (default false)                           |

#### [Create Guild Integration](integraciones.md#create-guild-integration) <a href="#create-guild-integration" id="create-guild-integration"></a>

`POST/guilds/{guild.id}/integrations`

Enables an integration for the guild. Requires the permission. Returns a 204 empty response on success. Fires [Guild Integrations Update](https://docs.discord.food/topics/gateway-events#guild-integrations-update) and [Integration Create](https://docs.discord.food/topics/gateway-events#integration-create) Gateway events.

[**JSON Params**](integraciones.md#json-params)

| Field | Type   | Description                                                                                                        |
| ----- | ------ | ------------------------------------------------------------------------------------------------------------------ |
| type  | string | The [type of integration](integraciones.md#integration-type) to enable (only `twitch` and `youtube` are supported) |
| id    | string | The ID of the integration account to enable                                                                        |

#### [Sync Guild Integration](integraciones.md#sync-guild-integration) <a href="#sync-guild-integration" id="sync-guild-integration"></a>

`POST/guilds/{guild.id}/integrations/{integration.id}/sync`

Syncs an integration for the guild. Requires the permission. Returns a 204 empty response on success. Fires [Guild Integrations Update](https://docs.discord.food/topics/gateway-events#guild-integrations-update) and [Integration Update](https://docs.discord.food/topics/gateway-events#integration-update) Gateway events.

#### [Modify Guild Integration](integraciones.md#modify-guild-integration) <a href="#modify-guild-integration" id="modify-guild-integration"></a>

`PATCH/guilds/{guild.id}/integrations/{integration.id}`

Modifies the behavior and settings of the integration in the guild. Requires the permission. Returns a 204 empty response on success. Fires [Guild Integrations Update](integraciones.md#integration-object) and [Integration Update](integraciones.md#integration-object) Gateway events.

[**JSON Params**](integraciones.md#json-params)

| Field                  | Type    | Description                                                                          |
| ---------------------- | ------- | ------------------------------------------------------------------------------------ |
| expire\_behavior?      | integer | The [behavior of expiring subscribers](integraciones.md#integration-expire-behavior) |
| expire\_grace\_period? | integer | The grace period before expiring subscribers (one of 1, 3, 7, 14, 30, in days)       |
| enable\_emoticons?     | boolean | Whether emoticons should be synced for this integration (Twitch only)                |

#### [Delete Guild Integration](integraciones.md#delete-guild-integration) <a href="#delete-guild-integration" id="delete-guild-integration"></a>

`DELETE/guilds/{guild.id}/integrations/{integration.id}`

Removes the given integration ID from the guild. Deletes any associated webhooks and kicks the associated bot (if there is one). Requires the permission. Returns a 204 empty response on success. Fires [Guild Integrations Update](https://docs.discord.food/topics/gateway-events#guild-integrations-update), [Integration Delete](https://docs.discord.food/topics/gateway-events#integration-delete), and optionally [Guild Member Remove](https://docs.discord.food/topics/gateway-events#guild-member-remove) and [Webhooks Update](https://docs.discord.food/topics/gateway-events#webhooks-update) Gateway events.

#### [Migrate Guild Command Scope](integraciones.md#migrate-guild-command-scope) <a href="#migrate-guild-command-scope" id="migrate-guild-command-scope"></a>

`POST/guilds/{guild.id}/migrate-command-scope`

Migrates all Discord application integrations in the guild to the [OAuth2 scope](https://docs.discord.food/topics/oauth2#oauth2-scopes). Requires the permission. Fires a [Guild Integrations Update](https://docs.discord.food/topics/gateway-events#guild-integrations-update) and multiple [Integration Update](https://docs.discord.food/topics/gateway-events#integration-update) Gateway events.

[**Response Body**](integraciones.md#response-body)

| Field                                 | Type              | Description                                                                    |
| ------------------------------------- | ----------------- | ------------------------------------------------------------------------------ |
| integration\_ids\_with\_app\_commands | array\[snowflake] | The IDs of migrated integrations that now have application commands registered |

#### [Get Guild Integration Application IDs](integraciones.md#get-guild-integration-application-ids) <a href="#get-guild-integration-application-ids" id="get-guild-integration-application-ids"></a>

`GET/users/@me/guilds/integration-application-ids`

Returns a mapping of guild IDs to lists of application IDs attached to the integrations in the current user's guilds.

[**Example Response**](integraciones.md#example-response)

```
{  "81384788765712384": [    "157858575924985856",    "157889000391180288",    "157873248346832897",    "157947794294833152",    "173805066229252096"  ],  "1046920999469330512": []}
```

#### [Get Channel Integrations](integraciones.md#get-channel-integrations) <a href="#get-channel-integrations" id="get-channel-integrations"></a>

`GET/channels/{channel.id}/integrations`

Returns a list of [integration](integraciones.md#integration-object) objects for the private channel.

#### [Delete Channel Integration](integraciones.md#delete-channel-integration) <a href="#delete-channel-integration" id="delete-channel-integration"></a>

`DELETE/channels/{channel.id}/integrations/{integration.id}`

Removes the given integration ID from the channel. Returns a 204 empty response on success. Fires an [Integration Delete](integraciones.md#integration-object) Gateway event.

#### [Join Integration Guild](integraciones.md#join-integration-guild) <a href="#join-integration-guild" id="join-integration-guild"></a>

`POST/integrations/{integration.id}/join`

Joins the user to the given integration ID's guild. Returns a 204 empty response on success. Fires a [Guild Create](https://docs.discord.food/topics/gateway-events#guild-create) Gateway event.

#### [Search Tenor GIFs](integraciones.md#search-tenor-gifs) <a href="#search-tenor-gifs" id="search-tenor-gifs"></a>

`GET/integrations/tenor/search`

Returns a list of up to 10 [GIFs sourced from Tenor](integraciones.md#tenor-gif-structure) based on the provided query.

[**Query String Parameters**](integraciones.md#query-string-parameters)

| Field | Type   | Description             |
| ----- | ------ | ----------------------- |
| q     | string | The search query to use |

[**Tenor GIF Structure**](integraciones.md#tenor-gif-structure)

| Field  | Type    | Description                        |
| ------ | ------- | ---------------------------------- |
| type   | string  | The type of the GIF (always `gif`) |
| url    | string  | The provider source URL of the GIF |
| src    | string  | The media URL of the GIF           |
| width  | integer | Width of image                     |
| height | integer | Height of image                    |

[**Example Response**](integraciones.md#example-response)

```
[  {    "type": "gif",    "url": "https://tenor.com/bQ3Du.gif",    "src": "https://media.tenor.com/RbG_9Eh9KLoAAAAS/alien-alien-reveal.gif",    "width": 100,    "height": 90  }]
```

#### [Get Trending GIF Search Terms](integraciones.md#get-trending-gif-search-terms) <a href="#get-trending-gif-search-terms" id="get-trending-gif-search-terms"></a>

`GET/gifs/trending-search`

Returns a list of the top trending search terms.

[**Query String Parameters**](integraciones.md#query-string-parameters)

| Field               | Type    | Description                                                    |
| ------------------- | ------- | -------------------------------------------------------------- |
| limit? <sup>1</sup> | integer | The maximum number of search terms to return (1-50, default 5) |
| locale?             | string  | The locale to use in search results (default `en-US`)          |

<sup>1</sup> The limit is only a suggestion; the API may return fewer GIFs.

#### [Get Suggested GIF Search Terms](integraciones.md#get-suggested-gif-search-terms) <a href="#get-suggested-gif-search-terms" id="get-suggested-gif-search-terms"></a>

`GET/gifs/suggest`

Returns a list of recommended search terms based on the provided query.

[**Query String Parameters**](integraciones.md#query-string-parameters)

| Field               | Type    | Description                                                     |
| ------------------- | ------- | --------------------------------------------------------------- |
| q                   | string  | The search query to use                                         |
| limit? <sup>1</sup> | integer | The maximum number of search terms to return (1-50, default 20) |
| locale?             | string  | The locale to use in search results (default `en-US`)           |

<sup>1</sup> The limit is only a suggestion; the API may return fewer GIFs.

#### [Search GIFs](integraciones.md#search-gifs) <a href="#search-gifs" id="search-gifs"></a>

`GET/gifs/search`

Returns a list of [GIF](integraciones.md#gif-structure) objects based on the provided query.

[**Query String Parameters**](integraciones.md#query-string-parameters)

| Field               | Type    | Description                                           |
| ------------------- | ------- | ----------------------------------------------------- |
| q                   | string  | The search query to use                               |
| limit? <sup>1</sup> | integer | The maximum number of GIFs to return (20-500)         |
| media\_format?      | string  | The media format to use (default `mediumgif`)         |
| locale?             | string  | The locale to use in search results (default `en-US`) |

<sup>1</sup> The limit is only a suggestion; the API may return fewer GIFs.

#### [Get Trending GIF Categories](integraciones.md#get-trending-gif-categories) <a href="#get-trending-gif-categories" id="get-trending-gif-categories"></a>

`GET/gifs/trending`

Returns trending GIF categories and their associated preview GIFs.

[**Query String Parameters**](integraciones.md#query-string-parameters)

| Field          | Type   | Description                                           |
| -------------- | ------ | ----------------------------------------------------- |
| media\_format? | string | The media format to use (default `mediumgif`)         |
| locale?        | string | The locale to use in search results (default `en-US`) |

[**Response Body**](integraciones.md#response-body)

| Field      | Type                                                                   | Description                            |
| ---------- | ---------------------------------------------------------------------- | -------------------------------------- |
| categories | array\[[GIF category](integraciones.md#gif-category-structure) object] | The trending GIF categories            |
| gifs       | array\[[GIF](integraciones.md#gif-structure) object]                   | A trending GIF to use as a placeholder |

[**GIF Category Structure**](integraciones.md#gif-category-structure)

| Field | Type   | Description                    |
| ----- | ------ | ------------------------------ |
| name  | string | The name of the category       |
| src   | string | The media URL of a preview GIF |

[**Example Response**](integraciones.md#example-response)

```
{  "categories": [    {      "name": "whatever",      "src": "https://media.tenor.com/97c0UK_cAHMAAAAd/whatever-sassy.gif"    }  ],  "gifs": [    {      "id": "16750982996130936929",      "title": "",      "url": "https://tenor.com/view/peace-out-peace-sign-peace-ice-age-eddie-gif-16750982996130936929",      "src": "https://media.tenor.com/6HdySNL-OGEAAAAC/peace-out-peace-sign.gif",      "gif_src": "https://media.tenor.com/6HdySNL-OGEAAAAC/peace-out-peace-sign.gif",      "width": 498,      "height": 498,      "preview": "https://media.tenor.com/6HdySNL-OGEAAAAD/peace-out-peace-sign.png"    }  ]}
```

#### [Get Trending GIFs](integraciones.md#get-trending-gifs) <a href="#get-trending-gifs" id="get-trending-gifs"></a>

`GET/gifs/trending-gifs`

Returns a list of [GIF](integraciones.md#gif-structure) objects that are currently trending.

[**Query String Parameters**](integraciones.md#query-string-parameters)

| Field               | Type    | Description                                           |
| ------------------- | ------- | ----------------------------------------------------- |
| limit? <sup>1</sup> | integer | The maximum number of GIFs to return (20-500)         |
| media\_format?      | string  | The media format to use (default `mediumgif`)         |
| locale?             | string  | The locale to use in search results (default `en-US`) |

<sup>1</sup> The limit is only a suggestion; the API may return fewer GIFs.

#### [Track Selected GIF](integraciones.md#track-selected-gif) <a href="#track-selected-gif" id="track-selected-gif"></a>

`POST/gifs/select`

Tracks the selection of a GIF by the user. Returns a 204 empty response on success.

[**JSON Params**](integraciones.md#json-params)

| Field | Type   | Description                           |
| ----- | ------ | ------------------------------------- |
| id    | string | The ID of the selected GIF            |
| q     | string | The search query used to find the GIF |

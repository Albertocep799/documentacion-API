---
icon: arrows-rotate-reverse
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

# Cuentas conectadas

### [Connected Accounts](cuentas-conectadas.md#connected-accounts) <a href="#connected-accounts" id="connected-accounts"></a>

Connections are links between third party accounts to Discord accounts.

#### [Connection Object](cuentas-conectadas.md#connection-object) <a href="#connection-object" id="connection-object"></a>

The connection object that the user has attached.

[**Connection Structure**](cuentas-conectadas.md#connection-structure)

| Field                                  | Type                                                                                            | Description                                                                      |
| -------------------------------------- | ----------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| id                                     | string                                                                                          | ID of the connection account                                                     |
| type                                   | string                                                                                          | The [type](cuentas-conectadas.md#connection-type) of the connection              |
| name                                   | string                                                                                          | The username of the connection account                                           |
| verified                               | boolean                                                                                         | Whether the connection is verified                                               |
| metadata?                              | object                                                                                          | Service-specific metadata about the connection                                   |
| metadata\_visibility                   | integer                                                                                         | [Visibility](cuentas-conectadas.md#visibility-type) of the connection's metadata |
| revoked                                | boolean                                                                                         | whether the connection is revoked                                                |
| integrations <sup>1</sup> <sup>2</sup> | array\[[connection integration](cuentas-conectadas.md#connection-integration-structure) object] | The guild integrations attached to the connection                                |
| friend\_sync                           | boolean                                                                                         | Whether friend sync is enabled for this connection                               |
| show\_activity                         | boolean                                                                                         | Whether activities related to this connection will be shown in presence          |
| two\_way\_link                         | boolean                                                                                         | Whether this connection has a corresponding third party OAuth2 token             |
| visibility                             | integer                                                                                         | [Visibility](cuentas-conectadas.md#visibility-type) of the connection            |
| access\_token? <sup>1</sup>            | string                                                                                          | The access token for the connection account                                      |

<sup>1</sup> Not included when [fetching a user's connections](cuentas-conectadas.md#get-user-connections) via OAuth2.

<sup>2</sup> These integrations can be used to [join your own sub-enabled guild or the guild of a creator you are supporting](https://docs.discord.food/resources/integration#join-integration-guild).

[**Partial Connection Structure**](cuentas-conectadas.md#partial-connection-structure)

| Field     | Type    | Description                                                         |
| --------- | ------- | ------------------------------------------------------------------- |
| id        | string  | ID of the connection account                                        |
| type      | string  | The [type](cuentas-conectadas.md#connection-type) of the connection |
| name      | string  | The username of the connection account                              |
| verified  | boolean | Whether the connection is verified                                  |
| metadata? | object  | Service-specific metadata about the connection                      |

[**Example Connection**](cuentas-conectadas.md#example-connection)

```
{  "type": "reddit",  "id": "run&hide",  "name": "alien",  "visibility": 1,  "friend_sync": false,  "show_activity": true,  "verified": true,  "two_way_link": false,  "metadata_visibility": 1,  "metadata": {    "gold": "0",    "mod": "1",    "total_karma": "20223",    "created_at": "2019-05-02T20:28:37"  },  "revoked": false,  "integrations": []}
```

[**Example Partial Connection**](cuentas-conectadas.md#example-partial-connection)

```
{  "type": "reddit",  "id": "run&hide",  "name": "alien",  "verified": true,  "metadata": {    "gold": "0",    "mod": "1",    "total_karma": "20223",    "created_at": "2019-05-02T20:28:37"  }}
```

[**Connection Integration Structure**](cuentas-conectadas.md#connection-integration-structure)

| Field           | Type                                                                                                 | Description                                                                                 |
| --------------- | ---------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| id <sup>1</sup> | snowflake                                                                                            | The ID of the integration                                                                   |
| type            | string                                                                                               | The [type of integration](https://docs.discord.food/resources/integration#integration-type) |
| account         | [account](https://docs.discord.food/resources/integration#integration-account-structure) object      | The integration's account information                                                       |
| guild           | [integration guild](https://docs.discord.food/resources/integration#integration-guild-object) object | The guild that the integration is attached to                                               |

<sup>1</sup> This field may also be the literal string "twitch-partners" to represent the Twitch Partners integration.

[**Example Connection Integration**](cuentas-conectadas.md#example-connection-integration)

```
{  "id": "twitch-partners",  "type": "twitch",  "account": {    "id": "92473777",    "name": "discordapp"  },  "guild": {    "id": "107939014299901952",    "name": "Twitch Partners",    "icon": "62450d21b75478191962d9c4b81831ae"  }}
```

[**Connection Type**](cuentas-conectadas.md#connection-type)

| Value                  | Name                          |
| ---------------------- | ----------------------------- |
| amazon-music           | Amazon Music                  |
| battlenet              | Battle.net                    |
| bluesky                | Bluesky                       |
| bungie                 | Bungie.net                    |
| contacts <sup>2</sup>  | Contact Sync                  |
| crunchyroll            | Crunchyroll                   |
| domain                 | Domain                        |
| ebay                   | eBay                          |
| epicgames              | Epic Games                    |
| facebook               | Facebook                      |
| github                 | GitHub                        |
| instagram <sup>1</sup> | Instagram                     |
| leagueoflegends        | League of Legends             |
| mastodon               | Mastodon                      |
| paypal                 | PayPal                        |
| playstation            | PlayStation Network           |
| playstation-stg        | PlayStation Network (Staging) |
| reddit                 | Reddit                        |
| roblox                 | Roblox                        |
| riotgames              | Riot Games                    |
| samsung <sup>1</sup>   | Samsung Galaxy                |
| soundcloud             | SoundCloud                    |
| spotify                | Spotify                       |
| skype <sup>1</sup>     | Skype                         |
| steam                  | Steam                         |
| tiktok                 | TikTok                        |
| twitch                 | Twitch                        |
| twitter                | Twitter                       |
| xbox                   | Xbox                          |
| youtube                | YouTube                       |

<sup>1</sup> Service can no longer be added by users.

<sup>2</sup> Service is not returned in [Get User Profile](https://docs.discord.food/resources/user#get-user-profile) or when [fetching a user's connections](cuentas-conectadas.md#get-user-connections) via OAuth2.

[**Visibility Type**](cuentas-conectadas.md#visibility-type)

| Value | Name     | Description                                      |
| ----- | -------- | ------------------------------------------------ |
| 0     | NONE     | Invisible to everyone except the user themselves |
| 1     | EVERYONE | Visible to everyone                              |

[Partial connections](cuentas-conectadas.md#partial-connection-structure) always have a visibility of 1.

#### [Console Device Object](cuentas-conectadas.md#console-device-object) <a href="#console-device-object" id="console-device-object"></a>

[**Console Device Structure**](cuentas-conectadas.md#console-device-structure)

| Field    | Type      | Description                                                                                                          |
| -------- | --------- | -------------------------------------------------------------------------------------------------------------------- |
| id       | snowflake | The ID of the device                                                                                                 |
| name     | string    | The name of the device                                                                                               |
| platform | string    | The [console platform](cuentas-conectadas.md#connection-type) (only `playstation` and `playstation-stg` are allowed) |

[**Example Console Device**](cuentas-conectadas.md#example-console-device)

```
{  "id": "1371598138300956672",  "name": "My Gifted PlayStation 5",  "platform": "playstation"}
```

#### [Endpoints](cuentas-conectadas.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Authorize User Connection](cuentas-conectadas.md#authorize-user-connection) <a href="#authorize-user-connection" id="authorize-user-connection"></a>

`GET/connections/{connection.type}/authorize`

Returns an authorization link that can be used for authorizing a new connection.

[**Query String Params**](cuentas-conectadas.md#query-string-params)

| Field                 | Type    | Description                                                                   |
| --------------------- | ------- | ----------------------------------------------------------------------------- |
| two\_way\_link\_type? | ?string | The [type of two-way link](cuentas-conectadas.md#two-way-link-type) to create |
| two\_way\_user\_code? | ?string | The device code to use for the two-way link                                   |
| continuation?         | boolean | Whether this is a continuation of a previous authorization                    |

[**Two Way Link Type**](cuentas-conectadas.md#two-way-link-type)

| Value   | Description                          |
| ------- | ------------------------------------ |
| web     | The connection is linked via web     |
| mobile  | The connection is linked via mobile  |
| desktop | The connection is linked via desktop |

[**Response Body**](cuentas-conectadas.md#response-body)

| Field | Type   | Description                         |
| ----- | ------ | ----------------------------------- |
| url   | string | The authorization link for the user |

#### [Create User Connection Callback](cuentas-conectadas.md#create-user-connection-callback) <a href="#create-user-connection-callback" id="create-user-connection-callback"></a>

`POST/connections/{connection.type}/callback`

Creates a new connection for the current user. Returns a [connection](cuentas-conectadas.md#connection-object) object on success. Fires a [User Connections Update](https://docs.discord.food/topics/gateway-events#user-connections-update) Gateway event.

[**JSON Params**](cuentas-conectadas.md#json-params)

| Field                 | Type    | Description                                 |
| --------------------- | ------- | ------------------------------------------- |
| code                  | string  | The authorization code for the connection   |
| state                 | string  | The state used to authorize the connection  |
| two\_way\_link\_code? | string  | The code to use for two-way linking         |
| insecure?             | boolean | Whether the connection is insecure          |
| friend\_sync?         | boolean | Whether to sync friends over the connection |
| openid\_params?       | object  | Additional parameters for OpenID Connect    |

#### [Create Contact Sync Connection](cuentas-conectadas.md#create-contact-sync-connection) <a href="#create-contact-sync-connection" id="create-contact-sync-connection"></a>

`PUT/users/@me/connections/contacts/{connection.id}`

Creates a new contact sync connection for the current user. Returns a [connection](cuentas-conectadas.md#connection-object) object on success. Fires a [User Connections Update](https://docs.discord.food/topics/gateway-events#user-connections-update) Gateway event.

[**JSON Params**](cuentas-conectadas.md#json-params)

| Field         | Type    | Description                                 |
| ------------- | ------- | ------------------------------------------- |
| name          | string  | The username of the connection account      |
| friend\_sync? | boolean | Whether to sync friends over the connection |

#### [Update External Friend List Entries](cuentas-conectadas.md#update-external-friend-list-entries) <a href="#update-external-friend-list-entries" id="update-external-friend-list-entries"></a>

`PUT/users/@me/connections/contacts/{connection.id}/external-friend-list-entries`

Syncs the user's device contacts to the connection. May fire multiple [Friend Suggestion Create](https://docs.discord.food/topics/gateway-events#friend-suggestion-create) Gateway events.

[**JSON Params**](cuentas-conectadas.md#json-params)

| Field                              | Type                                                                                  | Description                                                                                    |
| ---------------------------------- | ------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| friend\_list\_entries              | array\[[friend list entry](cuentas-conectadas.md#friend-list-entry-structure) object] | The phone numbers to sync (max 10000)                                                          |
| background                         | boolean                                                                               | Whether the request is a background sync (will not return suggestions)                         |
| allowed\_in\_suggestions           | integer                                                                               | The [contact sync suggestions setting](cuentas-conectadas.md#contact-sync-suggestions-setting) |
| include\_mutual\_friends\_count    | boolean                                                                               | Whether to show the mutual friend count of contacts                                            |
| add\_reverse\_friend\_suggestions? | boolean                                                                               | Whether to add users that have contact synced the current user as friend suggestions           |

[**Friend List Entry Structure**](cuentas-conectadas.md#friend-list-entry-structure)

| Field      | Type   | Description                                 |
| ---------- | ------ | ------------------------------------------- |
| friend\_id | string | E.164-formatted phone number of the contact |

[**Contact Sync Suggestions Setting**](cuentas-conectadas.md#contact-sync-suggestions-setting)

| Value | Name                        | Description                                                           |
| ----- | --------------------------- | --------------------------------------------------------------------- |
| 1     | MUTUAL\_CONTACT\_INFO\_ONLY | Users who have contact synced that have the current user as a contact |
| 2     | ANYONE\_WITH\_CONTACT\_INFO | Users who have contact synced                                         |

[**Response Body**](cuentas-conectadas.md#response-body)

| Field               | Type                                                                                                           | Description                                                                                                                |
| ------------------- | -------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| bulk\_add\_token    | ?string                                                                                                        | Token to be used for [bulk adding relationships](https://docs.discord.food/resources/relationships#bulk-add-relationships) |
| friend\_suggestions | array\[[friend suggestion](https://docs.discord.food/resources/relationships#friend-suggestion-object) object] | Suggested users                                                                                                            |

#### [Contact Sync Settings](cuentas-conectadas.md#contact-sync-settings) <a href="#contact-sync-settings" id="contact-sync-settings"></a>

`GET/users/@me/connections/contacts/{connection.id}/external-friend-list-entries/settings`

[**Response Body**](cuentas-conectadas.md#response-body)

| Field                    | Type    | Description                                                                                    |
| ------------------------ | ------- | ---------------------------------------------------------------------------------------------- |
| allowed\_in\_suggestions | integer | The [contact sync suggestions setting](cuentas-conectadas.md#contact-sync-suggestions-setting) |

#### [Create Domain Connection](cuentas-conectadas.md#create-domain-connection) <a href="#create-domain-connection" id="create-domain-connection"></a>

`POST/users/@me/connections/domain/{connection.id}`

Creates a new domain connection for the current user. Returns a [connection](cuentas-conectadas.md#connection-object) object on success. Fires a [User Connections Update](https://docs.discord.food/topics/gateway-events#user-connections-update) Gateway event.

#### [Get User Connections](cuentas-conectadas.md#get-user-connections) <a href="#get-user-connections" id="get-user-connections"></a>

`GET/users/@me/connections`

Returns a list of [connection](cuentas-conectadas.md#connection-object) objects.

#### [Get User Connection Access Token](cuentas-conectadas.md#get-user-connection-access-token) <a href="#get-user-connection-access-token" id="get-user-connection-access-token"></a>

`GET/users/@me/connections/{connection.type}/{connection.id}/access-token`

Returns a new access token for the given connection. Only available for Twitch, YouTube, and Spotify connections. Fires a [User Connections Update](https://docs.discord.food/topics/gateway-events#user-connections-update) Gateway event.

[**Response Body**](cuentas-conectadas.md#response-body)

| Field         | Type   | Description                   |
| ------------- | ------ | ----------------------------- |
| access\_token | string | The connection's access token |

#### [Get User Connection Subreddits](cuentas-conectadas.md#get-user-connection-subreddits) <a href="#get-user-connection-subreddits" id="get-user-connection-subreddits"></a>

`GET/users/@me/connections/reddit/{connection.id}/subreddits`

Returns a list of [subreddits](cuentas-conectadas.md#subreddit-structure) the connected account moderates. Only available for Reddit connections.

[**Subreddit Structure**](cuentas-conectadas.md#subreddit-structure)

| Field       | Type    | Description                       |
| ----------- | ------- | --------------------------------- |
| id          | string  | The subreddit's ID                |
| subscribers | integer | The number of joined Reddit users |
| url         | string  | The subreddit's relative URL      |

[**Example Response**](cuentas-conectadas.md#example-response)

```
[  {    "id": "t5_388p4",    "subscribers": 1044184,    "url": "/r/discordapp/"  }]
```

#### [Refresh User Connection](cuentas-conectadas.md#refresh-user-connection) <a href="#refresh-user-connection" id="refresh-user-connection"></a>

`POST/users/@me/connections/{connection.type}/{connection.id}/refresh`

Refreshes a connection. Returns a 204 empty response on success. Fires a [User Connections Update](https://docs.discord.food/topics/gateway-events#user-connections-update) Gateway event.

#### [Modify User Connection](cuentas-conectadas.md#modify-user-connection) <a href="#modify-user-connection" id="modify-user-connection"></a>

`PATCH/users/@me/connections/{connection.type}/{connection.id}`

Modifies a connection. Returns a [connection](cuentas-conectadas.md#connection-object) object on success. Fires a [User Connections Update](https://docs.discord.food/topics/gateway-events#user-connections-update) Gateway event.

Not all connection types support all parameters.

[**JSON Params**](cuentas-conectadas.md#json-params)

| Field                | Type    | Description                                                                      |
| -------------------- | ------- | -------------------------------------------------------------------------------- |
| name                 | string  | The connection's username                                                        |
| show\_activity       | boolean | Whether activities related to this connection will be shown in presence          |
| friend\_sync         | boolean | Whether friend sync is enabled for this connection                               |
| metadata\_visibility | integer | [Visibility](cuentas-conectadas.md#visibility-type) of the connection's metadata |
| visibility           | integer | [Visibility](cuentas-conectadas.md#visibility-type) of the connection            |

#### [Delete User Connection](cuentas-conectadas.md#delete-user-connection) <a href="#delete-user-connection" id="delete-user-connection"></a>

`DELETE/users/@me/connections/{connection.type}/{connection.id}`

Deletes a connection. Returns a 204 empty response on success. Fires a [User Connections Update](https://docs.discord.food/topics/gateway-events#user-connections-update) and optionally a [Guild Delete](https://docs.discord.food/topics/gateway-events#guild-delete) Gateway event.

#### [Get User Linked Connections](cuentas-conectadas.md#get-user-linked-connections) <a href="#get-user-linked-connections" id="get-user-linked-connections"></a>

`GET/users/@me/linked-connections`

Returns a list of [connection](cuentas-conectadas.md#connection-object) objects that have a two-way link with the application making the request.

#### [Create Console Connection](cuentas-conectadas.md#create-console-connection) <a href="#create-console-connection" id="create-console-connection"></a>

`POST/consoles/connect-request`

Returns a nonce for connecting to voice on PlayStation consoles.

[**JSON Params**](cuentas-conectadas.md#json-params)

| Field                  | Type                                                                                            | Description                       |
| ---------------------- | ----------------------------------------------------------------------------------------------- | --------------------------------- |
| analytics\_properties? | [connect request properties](cuentas-conectadas.md#connect-request-properties-structure) object | The properties used for analytics |

[**Connect Request Properties Structure**](cuentas-conectadas.md#connect-request-properties-structure)

| Field         | Type   | Description                                                            |
| ------------- | ------ | ---------------------------------------------------------------------- |
| handoff\_type | string | The [console handoff type](cuentas-conectadas.md#console-handoff-type) |

[**Console Handoff Type**](cuentas-conectadas.md#console-handoff-type)

| Value                    | Description                                   |
| ------------------------ | --------------------------------------------- |
| CREATE\_NEW\_CALL        | Create a new call on a console device         |
| TRANSFER\_EXISTING\_CALL | Transfer an existing call to a console device |

[**Response Body**](cuentas-conectadas.md#response-body)

| Field | Type   | Description |
| ----- | ------ | ----------- |
| nonce | string | The nonce   |

[**Example Response**](cuentas-conectadas.md#example-response)

```
{ "nonce": "fgnmC0tT" }
```

#### [Cancel Console Connection Request](cuentas-conectadas.md#cancel-console-connection-request) <a href="#cancel-console-connection-request" id="cancel-console-connection-request"></a>

`DELETE/consoles/connect-request/{nonce}`

Cancels a console connection request. Returns a 204 empty response on success.

#### [Get Console Devices](cuentas-conectadas.md#get-console-devices) <a href="#get-console-devices" id="get-console-devices"></a>

`GET/consoles/{connection.type}/devices`

Returns the consoles associated with the given connection type. Only supports and connection types.

[**Response Body**](cuentas-conectadas.md#response-body)

| Field   | Type                                                                         | Description              |
| ------- | ---------------------------------------------------------------------------- | ------------------------ |
| devices | array\[[console device](cuentas-conectadas.md#console-device-object) object] | The user console devices |

#### [Send Console Command](cuentas-conectadas.md#send-console-command) <a href="#send-console-command" id="send-console-command"></a>

`POST/consoles/{connection.type}/devices/{device.id}/commands`

Sends a command to connect to a voice call on a console device.

[**JSON Params**](cuentas-conectadas.md#json-params)

| Field       | Type      | Description                                                                                                   |
| ----------- | --------- | ------------------------------------------------------------------------------------------------------------- |
| command     | string    | The [command type](cuentas-conectadas.md#console-command-type)                                                |
| channel\_id | snowflake | The ID of the channel to connect to                                                                           |
| guild\_id?  | snowflake | The ID of the guild the channel is in                                                                         |
| nonce?      | string    | The nonce obtained from [Create Console Connection](cuentas-conectadas.md#create-console-connection) endpoint |

[**Console Command Type**](cuentas-conectadas.md#console-command-type)

| Value          | Description             |
| -------------- | ----------------------- |
| connect\_voice | Connect to a voice call |

[**Response Body**](cuentas-conectadas.md#response-body)

| Field | Type      | Description                |
| ----- | --------- | -------------------------- |
| id    | snowflake | The ID of the sent command |

#### [Cancel Console Command](cuentas-conectadas.md#cancel-console-command) <a href="#cancel-console-command" id="cancel-console-command"></a>

`DELETE/consoles/{connection.type}/devices/{device.id}/commands/{command.id}`

Cancels a console command. Returns a 204 empty response on success.

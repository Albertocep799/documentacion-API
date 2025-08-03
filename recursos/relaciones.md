---
icon: heart
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

# Relaciones

### [Relationships](relaciones.md#relationships) <a href="#relationships" id="relationships"></a>

Relationships in Discord are used to represent friendships, pending friend requests, and blocked users. Game relationships are used to distinguish special bonds created while in-game.

#### [Relationship Object](relaciones.md#relationship-object) <a href="#relationship-object" id="relationship-object"></a>

A relationship between the current user and another user.

[**Relationship Structure**](relaciones.md#relationship-structure)

| Field                           | Type                                                                 | Description                                                                                                                  |
| ------------------------------- | -------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| id                              | string                                                               | The ID of the target user                                                                                                    |
| type                            | integer                                                              | The [type](relaciones.md#relationship-type) of relationship                                                                  |
| user                            | partial [user](https://docs.discord.food/resources/user#user-object) | The target user                                                                                                              |
| nickname                        | ?string                                                              | The nickname of the user in this relationship (1-32 characters)                                                              |
| is\_spam\_request?              | boolean                                                              | Whether the friend request was flagged as spam (default false)                                                               |
| stranger\_request?              | boolean                                                              | Whether the friend request was sent by a user without a mutual friend or small mutual guild (default false)                  |
| user\_ignored                   | boolean                                                              | Whether the target user has been [ignored](https://support.discord.com/hc/en-us/articles/28084948873623) by the current user |
| origin\_application\_id?        | ?snowflake                                                           | The ID of the application that created the relationship                                                                      |
| since?                          | ISO8601 timestamp                                                    | When the user requested a relationship                                                                                       |
| has\_played\_game? <sup>1</sup> | boolean                                                              | Whether the target user has authorized the same application the current user's session is associated with                    |

<sup>1</sup> Only available in [OAuth2 contexts](https://docs.discord.food/topics/gateway#oauth2-and-the-gateway).

[**Relationship Type**](relaciones.md#relationship-type)

| Value | Name              | Description                                              |
| ----- | ----------------- | -------------------------------------------------------- |
| 0     | NONE              | No relationship exists                                   |
| 1     | FRIEND            | The user is a friend                                     |
| 2     | BLOCKED           | The user is blocked                                      |
| 3     | INCOMING\_REQUEST | The user has sent a friend request to the current user   |
| 4     | OUTGOING\_REQUEST | The current user has sent a friend request to the user   |
| 5     | IMPLICIT          | The user is an affinity of the current user              |
| ~~6~~ | ~~SUGGESTION~~    | ~~The user is a friend suggestion for the current user~~ |

[**Example Relationship**](relaciones.md#example-relationship)

```
{  "id": "852892297661906993",  "type": 3,  "nickname": null,  "user_ignored": false,  "user": {    "id": "852892297661906993",    "username": "alien",    "global_name": "Alien",    "avatar": "14733482e560d9267c0a414b21b2fb8d",    "discriminator": "0",    "public_flags": 64,    "avatar_decoration_data": null,    "primary_guild": null  },  "is_spam_request": false,  "since": "2023-02-10T01:58:05.348000+00:00",  "stranger_request": false}
```

#### [Game Relationship Object](relaciones.md#game-relationship-object) <a href="#game-relationship-object" id="game-relationship-object"></a>

An in-game relationship between the current user and another user, created using the social layer SDK.

[**Game Relationship Structure**](relaciones.md#game-relationship-structure)

| Field             | Type                                                                 | Description                                                              |
| ----------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| id                | string                                                               | The ID of the target user                                                |
| application\_id   | snowflake                                                            | The ID of the application whose game the relationship originated from    |
| type              | integer                                                              | The [type](relaciones.md#game-relationship-type) of relationship         |
| user <sup>1</sup> | partial [user](https://docs.discord.food/resources/user#user-object) | The target user                                                          |
| since             | ISO8601 timestamp                                                    | When the user requested a relationship                                   |
| dm\_access\_type  | integer                                                              | The [DM access level](relaciones.md#dm-access-type) for the relationship |
| user\_id          | snowflake                                                            | The ID of the current user                                               |

<sup>1</sup> Not included when fetching game relationships via OAuth2.

[**Game Relationship Type**](relaciones.md#game-relationship-type)

This enum is a subset of the [relationship type](relaciones.md#relationship-type) enum, supporting only , , and .

[**DM Access Type**](relaciones.md#dm-access-type)

This enum is unknown.

[**Example Game Relationship**](relaciones.md#example-game-relationship)

```
{  "user_id": "852892297661906993",  "application_id": "1237856342484717650",  "id": "1001086404203389018",  "type": 1,  "since": "2025-01-22T00:26:18.616000+00:00",  "dm_access_type": 0,  "user": {    "id": "1001086404203389018",    "username": ".dziurwa",    "global_name": "Dziurwa💕",    "avatar": "f6c0363fbab45668fcf8f88fea56db9c",    "avatar_decoration_data": null,    "discriminator": "0",    "public_flags": 4210944,    "primary_guild": null  }}
```

#### [Friend Suggestion Object](relaciones.md#friend-suggestion-object) <a href="#friend-suggestion-object" id="friend-suggestion-object"></a>

A friend suggestion for the current user.

[**Friend Suggestion Structure**](relaciones.md#friend-suggestion-structure)

| Field                            | Type                                                                                        | Description                                                       |
| -------------------------------- | ------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| suggested\_user                  | partial [user](https://docs.discord.food/resources/user#user-object) object                 | The suggested user                                                |
| reasons                          | array\[[friend suggestion reason](relaciones.md#friend-suggestion-reason-structure) object] | The sources of the suggestion                                     |
| from\_suggested\_user\_contacts? | boolean                                                                                     | Whether the suggested user has the current user in their contacts |

[**Friend Suggestion Reason Structure**](relaciones.md#friend-suggestion-reason-structure)

| Field    | Type    | Description                                                                                                                |
| -------- | ------- | -------------------------------------------------------------------------------------------------------------------------- |
| type     | integer | The [type of reason](relaciones.md#friend-suggestion-reason-type)                                                          |
| platform | string  | The [platform that the suggestion originated from](https://docs.discord.food/resources/connected-accounts#connection-type) |
| name     | string  | The user's name on the platform                                                                                            |

[**Friend Suggestion Reason Type**](relaciones.md#friend-suggestion-reason-type)

| Value | Name             | Description                              |
| ----- | ---------------- | ---------------------------------------- |
| 1     | EXTERNAL\_FRIEND | The user is a friend on another platform |

[**Example Friend Suggestion**](relaciones.md#example-friend-suggestion)

```
{  "suggested_user": {    "id": "852892297661906993",    "username": "alien",    "global_name": "Alien",    "avatar": "14733482e560d9267c0a414b21b2fb8d",    "discriminator": "0",    "public_flags": 64,    "avatar_decoration_data": null,    "primary_guild": null  },  "reasons": [    {      "type": 1,      "platform_type": "contacts",      "name": "Gnarpy"    }  ]}
```

### [Endpoints](relaciones.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get Relationships](relaciones.md#get-relationships) <a href="#get-relationships" id="get-relationships"></a>

`GET/users/@me/relationships`

Returns a list of [relationship](relaciones.md#relationship-object) objects for the current user.

#### [Send Friend Request](relaciones.md#send-friend-request) <a href="#send-friend-request" id="send-friend-request"></a>

`POST/users/@me/relationships`

Sends a friend request to another user, which can be accepted by creating a new relationship of type . Returns a 204 empty response on success. Fires a [Relationship Add](https://docs.discord.food/topics/gateway-events#relationship-add) Gateway event.

[**JSON Params**](relaciones.md#json-params)

| Field                      | Type    | Description                                               |
| -------------------------- | ------- | --------------------------------------------------------- |
| username                   | string  | The username of the user to send a friend request to      |
| discriminator <sup>1</sup> | ?string | The discriminator of the user to send a friend request to |

<sup>1</sup> for migrated users. See the [section on Discord's new username system](https://docs.discord.food/resources/user#unique-usernames) for more information.

#### [Create Relationship](relaciones.md#create-relationship) <a href="#create-relationship" id="create-relationship"></a>

`PUT/users/@me/relationships/{user.id}`

Creates a relationship with another user. Returns a 204 empty response on success. Fires a [Relationship Add](https://docs.discord.food/topics/gateway-events#relationship-add) Gateway event.

[**JSON Params**](relaciones.md#json-params)

| Field                     | Type    | Description                                                                                                                                    |
| ------------------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| type?                     | integer | The [relationship type](relaciones.md#relationship-type) to create (defaults to -1, which accepts an existing or creates a new friend request) |
| from\_friend\_suggestion? | boolean | Whether the relationship was created from a friend suggestion (default false)                                                                  |

#### [Ignore User](relaciones.md#ignore-user) <a href="#ignore-user" id="ignore-user"></a>

`PUT/users/@me/relationships/{user.id}/ignore`

[Ignores](https://support.discord.com/hc/en-us/articles/28084948873623) a user. Returns a 204 empty response on success. Fires a [Relationship Add](https://docs.discord.food/topics/gateway-events#relationship-add) or [Relationship Update](https://docs.discord.food/topics/gateway-events#relationship-update) Gateway event.

#### [Unignore User](relaciones.md#unignore-user) <a href="#unignore-user" id="unignore-user"></a>

`DELETE/users/@me/relationships/{user.id}/ignore`

Unignores a user. Returns a 204 empty response on success. Fires a [Relationship Update](https://docs.discord.food/topics/gateway-events#relationship-update) or [Relationship Remove](https://docs.discord.food/topics/gateway-events#relationship-remove) Gateway event.

#### [Modify Relationship](relaciones.md#modify-relationship) <a href="#modify-relationship" id="modify-relationship"></a>

`PATCH/users/@me/relationships/{user.id}`

Modifies a relationship to another user. Returns a 204 empty response on success. Fires a [Relationship Update](https://docs.discord.food/topics/gateway-events#relationship-update) Gateway event.

[**JSON Params**](relaciones.md#json-params)

| Field                  | Type   | Description                                                     |
| ---------------------- | ------ | --------------------------------------------------------------- |
| nickname? <sup>1</sup> | string | The nickname of the user in this relationship (1-32 characters) |

<sup>1</sup> Only applicable to relationships of type .

#### [Remove Relationship](relaciones.md#remove-relationship) <a href="#remove-relationship" id="remove-relationship"></a>

`DELETE/users/@me/relationships/{user.id}`

Removes a relationship with another user. Returns a 204 empty response on success. Fires a [Relationship Remove](https://docs.discord.food/topics/gateway-events#relationship-remove) Gateway event.

#### [Bulk Remove Relationships](relaciones.md#bulk-remove-relationships) <a href="#bulk-remove-relationships" id="bulk-remove-relationships"></a>

`DELETE/users/@me/relationships`

Removes multiple relationships. Returns a 204 empty response on success. May fire multiple [Relationship Remove](https://docs.discord.food/topics/gateway-events#relationship-remove) Gateway events.

[**Query Params**](relaciones.md#query-params)

| Field                        | Type    | Description                                                                                                                                          |
| ---------------------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| relationship\_type?          | integer | Remove relationships with this [relationship type](relaciones.md#relationship-type) (default `INCOMING_REQUEST`, only `INCOMING_REQUEST` is allowed) |
| only\_spam? **(deprecated)** | boolean | Whether to remove relationships that were flagged as spam (default false)                                                                            |

[**JSON Params**](relaciones.md#json-params)

| Field    | Type            | Description                                                                                                     |
| -------- | --------------- | --------------------------------------------------------------------------------------------------------------- |
| filters? | array\[integer] | The [relationship removal filters](relaciones.md#relationship-removal-filter) to match against, using AND logic |

[**Relationship Removal Filter**](relaciones.md#relationship-removal-filter)

| Value | Name    | Description                                |
| ----- | ------- | ------------------------------------------ |
| 1     | SPAM    | Friend requests flagged by Discord as spam |
| 2     | IGNORED | Ignored users                              |

#### [Bulk Add Relationships](relaciones.md#bulk-add-relationships) <a href="#bulk-add-relationships" id="bulk-add-relationships"></a>

`POST/users/@me/relationships/bulk`

Adds multiple relationships from [contact sync](https://docs.discord.food/resources/connected-accounts#update-external-friend-list-entries). May fire multiple [Relationship Add](https://docs.discord.food/topics/gateway-events#relationship-add) Gateway events.

[**JSON Params**](relaciones.md#json-params)

| Field     | Type              | Description                                                                                                                   |
| --------- | ----------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| user\_ids | array\[snowflake] | IDs of users to add                                                                                                           |
| token     | string            | The [contact sync](https://docs.discord.food/resources/connected-accounts#update-external-friend-list-entries) bulk add token |

[**Response Body**](relaciones.md#response-body)

| Field                | Type              | Description                                        |
| -------------------- | ----------------- | -------------------------------------------------- |
| failed\_requests     | array\[snowflake] | IDs of the users who could not be friend requested |
| successful\_requests | array\[snowflake] | IDs of the users who were friend requested         |

#### [Get Game Relationships](relaciones.md#get-game-relationships) <a href="#get-game-relationships" id="get-game-relationships"></a>

`GET/users/@me/game-relationships`

Returns a list of [game relationship](relaciones.md#game-relationship-object) objects for the current user.

#### [Send Game Friend Request](relaciones.md#send-game-friend-request) <a href="#send-game-friend-request" id="send-game-friend-request"></a>

`POST/users/@me/game-relationships`

Sends a game friend request to another user, which can be accepted in-game by [creating a new game relationship of type ](relaciones.md#create-game-relationship), or [by the user independently](relaciones.md#create-game-relationship-by-application). Returns a 204 empty response on success. Fires a [Game Relationship Add](https://docs.discord.food/topics/gateway-events#game-relationship-add) Gateway event.

[**JSON Params**](relaciones.md#json-params)

| Field    | Type   | Description                                               |
| -------- | ------ | --------------------------------------------------------- |
| username | string | The username of the user to send a game friend request to |

#### [Create Game Relationship](relaciones.md#create-game-relationship) <a href="#create-game-relationship" id="create-game-relationship"></a>

`PUT/users/@me/game-relationships/{user.id}`

Creates a game relationship with another user. Returns a 204 empty response on success. Fires a [Game Relationship Add](https://docs.discord.food/topics/gateway-events#game-relationship-add) Gateway event.

| Field | Type    | Description                                                                                                                                         |
| ----- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| type? | integer | The [relationship type](relaciones.md#game-relationship-type) to create (defaults to -1, which accepts an existing or creates a new friend request) |

#### [Create Game Relationship by Application](relaciones.md#create-game-relationship-by-application) <a href="#create-game-relationship-by-application" id="create-game-relationship-by-application"></a>

`PUT/users/@me/game-relationships/{user.id}/{application.id}`

Accepts a game relationship from another user on a specific application. Returns a 204 empty response on success. Fires a [Game Relationship Add](https://docs.discord.food/topics/gateway-events#game-relationship-add) Gateway event.

| Field | Type    | Description                                                                                        |
| ----- | ------- | -------------------------------------------------------------------------------------------------- |
| type? | integer | The [relationship type](relaciones.md#game-relationship-type) to create (only `FRIEND` is allowed) |

#### [Remove Game Relationship](relaciones.md#remove-game-relationship) <a href="#remove-game-relationship" id="remove-game-relationship"></a>

`DELETE/users/@me/game-relationships/{user.id}`

Removes a game relationship with another user. Returns a 204 empty response on success. Fires a [Game Relationship Remove](https://docs.discord.food/topics/gateway-events#game-relationship-remove) Gateway event.

#### [Remove Game Relationship by Application](relaciones.md#remove-game-relationship-by-application) <a href="#remove-game-relationship-by-application" id="remove-game-relationship-by-application"></a>

`DELETE/users/@me/game-relationships/{user.id}/{application.id}`

Removes a game relationship with another user. Returns a 204 empty response on success. Fires a [Game Relationship Remove](https://docs.discord.food/topics/gateway-events#game-relationship-remove) Gateway event.

#### [Get Friend Suggestions](relaciones.md#get-friend-suggestions) <a href="#get-friend-suggestions" id="get-friend-suggestions"></a>

`GET/friend-suggestions`

Returns a list of [friend suggestion](relaciones.md#friend-suggestion-object) objects for the current user.

#### [Remove Friend Suggestion](relaciones.md#remove-friend-suggestion) <a href="#remove-friend-suggestion" id="remove-friend-suggestion"></a>

`DELETE/friend-suggestions/{user.id}`

Removes a friend suggestion for the current user. Returns a 204 empty response on success. Fires a [Friend Suggestion Delete](https://docs.discord.food/topics/gateway-events#friend-suggestion-delete) Gateway event.

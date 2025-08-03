---
icon: family
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

# Centro familiar

### [Family Center](centro-familiar.md#family-center) <a href="#family-center" id="family-center"></a>

[Family Center](https://support.discord.com/hc/en-us/articles/14155043715735-Family-Center-for-Parents-and-Guardians) acts as Discord's parental controls solution to allow parents to monitor the activities of their teens on Discord. They do not allow parents to view message content, but Discord does share:

* Users messaged (DMs and Group DMs)
* Users called (DMs and Group DMs) in the last week
* Friends added in the last week
* Guilds joined in the last week
* Guilds the teen has sent messages to in the last week

A maximum of 8 accounts can be connected to a single parent.

### [Definitions](centro-familiar.md#definitions) <a href="#definitions" id="definitions"></a>

In line with the API, instead of referring to Family Center users as "parents" or "teens" and links as "family", "connected teens", or "my family", the API terminology will be used instead.

* **Requestor:** This is the user that _sends_ a link request to a different user and acts as the user the linked user is connected to. Can be viewed as the "parent."
* **Linked user:** This is the user that _receives_ or _accepts_ a link request sent by the requestor and acts as the user the "parent" can view the activity of. Can be viewed as the "teen."
* **Link:** Represents the connection between requestor and linked user.

#### [Family Center Object](centro-familiar.md#family-center-object) <a href="#family-center-object" id="family-center-object"></a>

[**Family Center Structure**](centro-familiar.md#family-center-structure)

| Field            | Type                                                                                | Description                                        |
| ---------------- | ----------------------------------------------------------------------------------- | -------------------------------------------------- |
| linked\_users    | array\[[linked user](centro-familiar.md#linked-user-object) object]                 | List of linked users                               |
| teen\_audit\_log | [teen audit log](centro-familiar.md#teen-audit-log-object) object                   | Audit log of the linked users activity             |
| users            | array\[partial [user](https://docs.discord.food/resources/user#user-object) object] | List of requestors the linked user is connected to |

#### [Linked User Object](centro-familiar.md#linked-user-object) <a href="#linked-user-object" id="linked-user-object"></a>

Partial user data of an underage user linked to the requestor via [Family Center](https://support.discord.com/hc/en-us/articles/14155043715735-Family-Center-for-Parents-and-Guardians).

[**Linked User Structure**](centro-familiar.md#linked-user-structure)

| Field                      | Type              | Description                                                          |
| -------------------------- | ----------------- | -------------------------------------------------------------------- |
| created\_at                | ISO8601 timestamp | When the link request was sent                                       |
| updated\_at                | ISO8601 timestamp | When the link status was last updated                                |
| link\_status               | integer           | The [link status](centro-familiar.md#link-status) of the linked user |
| link\_type                 | integer           | The [link type](centro-familiar.md#link-type)                        |
| requestor\_id <sup>1</sup> | snowflake         | The ID of the account the linked user is connected to                |
| user\_id <sup>1</sup>      | snowflake         | The ID of the linked user                                            |

<sup>1</sup> If the link type is , the and will be the same. See [Link Type](centro-familiar.md#link-type) for more information.

[**Link Status**](centro-familiar.md#link-status)

Represents the current state of the link.

| Value | Description                                                    |
| ----- | -------------------------------------------------------------- |
| 1     | The Family Center link request has been sent, but not accepted |
| 2     | The linked user is currently connected to the requestor        |
| 3     | The link has been disconnected                                 |
| 4     | The link request was rejected                                  |

[**Link Type**](centro-familiar.md#link-type)

Represents what part each user played in the connection.

| Value | Description                                                              |
| ----- | ------------------------------------------------------------------------ |
| 1     | The current user accepted the request and is the linked user of the link |
| 2     | The current user sent the request and is the requestor of the link       |

[**Example Linked User**](centro-familiar.md#example-linked-user)

```
{  "created_at": "2024-07-30T19:49:09.800072+00:00",  "updated_at": "2024-07-30T19:55:43.834081+00:00",  "link_type": 2,  "link_status": 3,  "requestor_id": "246877849162743818",  "user_id": "801318363472330772"}
```

#### [Linked Users Object](centro-familiar.md#linked-users-object) <a href="#linked-users-object" id="linked-users-object"></a>

Lists all linked users and requestors. Not to be confused with the [Linked User](centro-familiar.md#linked-user-object) object.

[**Linked Users structure**](centro-familiar.md#linked-users-structure)

| Field         | Type                                                                                | Description                                        |
| ------------- | ----------------------------------------------------------------------------------- | -------------------------------------------------- |
| linked\_users | array\[[linked user](centro-familiar.md#linked-user-object) object]                 | List of linked users                               |
| users         | array\[partial [user](https://docs.discord.food/resources/user#user-object) object] | List of requestors the linked user is connected to |

#### [Teen Audit Log Object](centro-familiar.md#teen-audit-log-object) <a href="#teen-audit-log-object" id="teen-audit-log-object"></a>

Audit log of events of the linked user. Visible to both requestors and linked users.

[**Teen Audit Log Structure**](centro-familiar.md#teen-audit-log-structure)

| Field            | Type                                                                                | Description                                                                      |
| ---------------- | ----------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| teen\_user\_id   | ?snowflake                                                                          | The ID of the linked user                                                        |
| range\_start\_id | ?snowflake                                                                          | A snowflake representing the start time of the current 7-day track range         |
| actions          | array\[[action](centro-familiar.md#action-structure) object]                        | List of [actions](centro-familiar.md#action-structure) the linked user has done  |
| users            | array\[partial [user](https://docs.discord.food/resources/user#user-object) object] | Users referenced in the audit log                                                |
| guilds           | array\[[guild](https://docs.discord.food/resources/guild#guild-object) object]      | Guilds referenced in the audit log                                               |
| totals           | map\[integer, integer]                                                              | Object keyed by [action types](centro-familiar.md#action-type) with their totals |

[**Action Structure**](centro-familiar.md#action-structure)

| Field         | Type      | Description                                                                                                                       |
| ------------- | --------- | --------------------------------------------------------------------------------------------------------------------------------- |
| event\_id     | snowflake | The ID of the event action                                                                                                        |
| user\_id      | snowflake | The ID of the linked user                                                                                                         |
| entity\_id    | snowflake | The ID of the entity the action relates to (user, guild, or group DM) based off the [action type](centro-familiar.md#action-type) |
| display\_type | integer   | The [type of the action](centro-familiar.md#action-type) of the action, detailing what this action involved                       |

[**Action Type**](centro-familiar.md#action-type)

Represents what kind of action the linked user engaged in.

| Value | Description                                                        |
| ----- | ------------------------------------------------------------------ |
| 1     | Users added within the last 7 days                                 |
| 2     | Guild joined within the last 7 days                                |
| 3     | Users messaged within the last 7 days                              |
| 4     | Guilds the linked user has sent messages in within the last 7 days |
| 5     | Users called within the last 7 days                                |

[**Example Teen Audit Log**](centro-familiar.md#example-teen-audit-log)

```
{  "teen_user_id": "801318363472330772",  "range_start_id": "1328607677722394624",  "actions": [    {      "event_id": "1331144363278860318",      "user_id": "801318363472330772",      "entity_id": "246877849162743818",      "display_type": 3    }  ],  "users": [    {      "id": "246877849162743818",      "username": "jay_taelien",      "global_name": "Jay",      "avatar": "91b7bc37e924f78625f7ea582fdbac5d",      "avatar_decoration_data": {        "asset": "a_aa2e1c2b3cf05b24f6ec7b8b4141f5fc",        "sku_id": "1144056631374647458",        "expires_at": null      },      "discriminator": "0",      "public_flags": 16512,      "primary_guild": null    }  ],  "guilds": [],  "totals": {    "1": 0,    "2": 0,    "3": 1,    "4": 0  }}
```

### [Endpoints](centro-familiar.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get Family Center Overview](centro-familiar.md#get-family-center-overview) <a href="#get-family-center-overview" id="get-family-center-overview"></a>

`GET/family-center/@me`

Returns a [Family Center](centro-familiar.md#family-center-object) object.

#### [Get Link Code](centro-familiar.md#get-link-code) <a href="#get-link-code" id="get-link-code"></a>

`GET/family-center/@me/link-code`

Generates the link code for usage in the generated QR code that a linked user receives to give to a requestor.

[**Response Body**](centro-familiar.md#response-body)

| Field      | Type   | Description                                                                                               |
| ---------- | ------ | --------------------------------------------------------------------------------------------------------- |
| link\_code | string | The code used to connect a requestor to a linked user, appended to the end of the URL the QR code encodes |

#### [Get Linked Users](centro-familiar.md#get-linked-users) <a href="#get-linked-users" id="get-linked-users"></a>

`GET/users/@me/linked-users`

Returns a [linked users](centro-familiar.md#linked-users-object) object.

#### [Create Linked User Request](centro-familiar.md#create-linked-user-request) <a href="#create-linked-user-request" id="create-linked-user-request"></a>

`POST/users/@me/linked-users`

Creates a request that appears in the linked user's Family Center. Returns a [linked users](centro-familiar.md#linked-users-object) object on success. Fires a [User Update](https://docs.discord.food/topics/gateway-events#user-update) Gateway event.

[**JSON Params**](centro-familiar.md#json-params)

| Field         | Type      | Description                                          |
| ------------- | --------- | ---------------------------------------------------- |
| recipient\_id | snowflake | The ID of the user the requestor wants to connect to |
| code          | string    | The link code from the linked user's device          |

#### [Modify Linked User](centro-familiar.md#modify-linked-user) <a href="#modify-linked-user" id="modify-linked-user"></a>

`PATCH/users/@me/linked-users`

Modifies the linked user status of a linked user. Can be invoked by either the linked user or the requestor if used for removing the link. Returns an array of [linked user](centro-familiar.md#linked-user-object) objects on success. Fires a [User Update](https://docs.discord.food/topics/gateway-events#user-update) Gateway event.

[**JSON Params**](centro-familiar.md#json-params)

| Field                         | Type    | Description                                                              |
| ----------------------------- | ------- | ------------------------------------------------------------------------ |
| link\_status                  | integer | The new [link status](centro-familiar.md#link-status) of the linked user |
| linked\_user\_id <sup>1</sup> | string  | The ID of the user the linked user or requestor is modifying             |

<sup>1</sup> If this request is sent to remove a link (settingto ), the changes depending on if the requestor or linked user is invoking it. If the linked user invokes the request, is the ID of the requestor, otherwise it's the ID of the linked user.

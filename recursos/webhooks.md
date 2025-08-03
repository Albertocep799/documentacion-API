---
icon: webhook
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

# Webhooks

### [Webhooks](webhooks.md#webhooks) <a href="#webhooks" id="webhooks"></a>

Webhooks are a low-effort way to post messages to channels in Discord. They do not require a bot user or authentication to use.

#### [Webhook Object](webhooks.md#webhook-object) <a href="#webhook-object" id="webhook-object"></a>

Used to represent a webhook.

[**Webhook Structure**](webhooks.md#webhook-structure)

| Field                         | Type                                                                                                 | Description                                                                                        |
| ----------------------------- | ---------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| id                            | snowflake                                                                                            | The ID of the webhook                                                                              |
| type                          | integer                                                                                              | The [type of webhook](webhooks.md#webhook-types)                                                   |
| guild\_id?                    | ?snowflake                                                                                           | The guild ID this webhook is for, if any                                                           |
| channel\_id                   | ?snowflake                                                                                           | The channel ID this webhook is for, if any                                                         |
| user?                         | ?partial [user](https://docs.discord.food/resources/user#user-object) object                         | The user this webhook was created by                                                               |
| name                          | ?string                                                                                              | The default name of the webhook (1-80 characters)                                                  |
| avatar                        | ?string                                                                                              | The default [avatar hash](https://docs.discord.food/reference#cdn-formatting) of the webhook       |
| token? <sup>1</sup>           | string                                                                                               | The secure token of the webhook (returned for `INCOMING` webhooks)                                 |
| application\_id               | ?snowflake                                                                                           | The application that created this webhook                                                          |
| source\_guild? <sup>2</sup>   | [integration guild](https://docs.discord.food/resources/integration#integration-guild-object) object | The guild of the channel that this webhook is following (returned for `CHANNEL_FOLLOWER` webhooks) |
| source\_channel? <sup>2</sup> | [webhook channel](webhooks.md#webhook-channel-structure) object                                      | The channel that this webhook is following (returned for `CHANNEL_FOLLOWER` webhooks)              |
| url? <sup>1</sup>             | string                                                                                               | The URL used for executing the webhook (returned for `INCOMING` webhooks)                          |

<sup>1</sup> For application-owned webhooks, this field is only returned to the application that created the webhook.

<sup>2</sup> These fields will not be included if the webhook creator has since lost access to the followed channel's guild.

[**Webhook Types**](webhooks.md#webhook-types)

| Value | Name              | Description                                                                             |
| ----- | ----------------- | --------------------------------------------------------------------------------------- |
| 1     | INCOMING          | Incoming webhooks can post messages to channels with a generated token                  |
| 2     | CHANNEL\_FOLLOWER | Channel Follower webhooks are internal webhooks used to post new messages into channels |
| 3     | APPLICATION       | Application webhooks are webhooks used with interactions                                |

[**Webhook Channel Structure**](webhooks.md#webhook-channel-structure)

| Field | Type      | Description                                |
| ----- | --------- | ------------------------------------------ |
| id    | snowflake | The ID of the channel                      |
| name  | string    | The name of the channel (1-100 characters) |

[**Example Incoming Webhook**](webhooks.md#example-incoming-webhook)

```
{  "application_id": null,  "avatar": null,  "channel_id": "199737254929760256",  "guild_id": "199737254929760256",  "id": "223704706495545344",  "name": "test webhook",  "type": 1,  "user": {    "id": "828387742575624222",    "username": "jupppper",    "avatar": "e14a7c62b0b38068be88be194b23910f",    "discriminator": "0",    "public_flags": 16384,    "banner": null,    "accent_color": null,    "global_name": "Jup",    "avatar_decoration_data": null,    "primary_guild": null  },  "token": "3d89bb7572e0fb30d8128367b3b1b44fecd1726de135cbe28a41f8b2f777c372ba2939e72279b94526ff5d1bd4358d65cf11",  "url": "https://discord.com/api/webhooks/223704706495545344/3d89bb7572e0fb30d8128367b3b1b44fecd1726de135cbe28a41f8b2f777c372ba2939e72279b94526ff5d1bd4358d65cf11"}
```

[**Example Channel Follower Webhook**](webhooks.md#example-channel-follower-webhook)

```
{  "application_id": null,  "avatar": "bb71f469c158984e265093a81b3397fb",  "channel_id": "561885260615255432",  "guild_id": "56188498421443265",  "id": "752831914402115456",  "name": "Guildy name",  "type": 2,  "source_guild": {    "id": "56188498421476534",    "name": "Guildy name",    "icon": "bb71f469c158984e265093a81b3397fb"  },  "source_channel": {    "id": "5618852344134324",    "name": "announcements"  },  "user": {    "id": "828387742575624222",    "username": "jupppper",    "avatar": "e14a7c62b0b38068be88be194b23910f",    "discriminator": "0",    "public_flags": 16384,    "banner": null,    "accent_color": null,    "global_name": "Jup",    "avatar_decoration_data": null,    "primary_guild": null  }}
```

[**Example Application Webhook**](webhooks.md#example-application-webhook)

```
{  "application_id": "658822586720976555",  "avatar": "689161dc90ac261d00f1608694ac6bfd",  "channel_id": null,  "guild_id": null,  "id": "658822586720976555",  "name": "Clyde",  "type": 3,  "user": null}
```

### [Endpoints](webhooks.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Create Webhook](webhooks.md#create-webhook) <a href="#create-webhook" id="create-webhook"></a>

`POST/channels/{channel.id}/webhooks`

Creates a new webhook. Requires the permission. Returns a [webhook](webhooks.md#webhook-object) object on success. Fires a [Webhooks Update](https://docs.discord.food/topics/gateway-events#webhooks-update) Gateway event.

[**JSON Params**](webhooks.md#json-params)

| Field   | Type                                                        | Description                                       |
| ------- | ----------------------------------------------------------- | ------------------------------------------------- |
| name    | string                                                      | The default name of the webhook (1-80 characters) |
| avatar? | ?[image data](https://docs.discord.food/reference#cdn-data) | The default avatar of the webhook                 |

#### [Get Channel Webhooks](webhooks.md#get-channel-webhooks) <a href="#get-channel-webhooks" id="get-channel-webhooks"></a>

`GET/channels/{channel.id}/webhooks`

Returns a list of channel [webhook](webhooks.md#webhook-object) objects. Requires the permission.

#### [Get Guild Webhooks](webhooks.md#get-guild-webhooks) <a href="#get-guild-webhooks" id="get-guild-webhooks"></a>

`GET/guilds/{guild.id}/webhooks`

Returns a list of guild [webhook](webhooks.md#webhook-object) objects. Requires the permission.

#### [Get Webhook](webhooks.md#get-webhook) <a href="#get-webhook" id="get-webhook"></a>

`GET/webhooks/{webhook.id}`

Returns a [webhook](webhooks.md#webhook-object) object for the given webhook ID. Requires the permission unless the application making the request owns the webhook.

#### [Get Webhook with Token](webhooks.md#get-webhook-with-token) <a href="#get-webhook-with-token" id="get-webhook-with-token"></a>

`GET/webhooks/{webhook.id}/{webhook.token}`

Same as above, except this call does not require authentication.

#### [Modify Webhook](webhooks.md#modify-webhook) <a href="#modify-webhook" id="modify-webhook"></a>

`PATCH/webhooks/{webhook.id}`

Modifies a webhook. Requires the permission. Returns the updated [webhook](webhooks.md#webhook-object) object on success. Fires a [Webhooks Update](https://docs.discord.food/topics/gateway-events#webhooks-update) Gateway event.

[**JSON Params**](webhooks.md#json-params)

| Field        | Type                                                        | Description                                       |
| ------------ | ----------------------------------------------------------- | ------------------------------------------------- |
| name?        | string                                                      | The default name of the webhook (1-80 characters) |
| avatar?      | ?[image data](https://docs.discord.food/reference#cdn-data) | The default avatar of the webhook                 |
| channel\_id? | snowflake                                                   | The channel ID this webhook should be moved to    |

#### [Modify Webhook with Token](webhooks.md#modify-webhook-with-token) <a href="#modify-webhook-with-token" id="modify-webhook-with-token"></a>

`PATCH/webhooks/{webhook.id}/{webhook.token}`

Same as above, except this call does not require authentication and does not accept a parameter.

[**JSON Params**](webhooks.md#json-params)

| Field   | Type                                                        | Description                                       |
| ------- | ----------------------------------------------------------- | ------------------------------------------------- |
| name?   | string                                                      | The default name of the webhook (1-80 characters) |
| avatar? | ?[image data](https://docs.discord.food/reference#cdn-data) | The default avatar of the webhook                 |

#### [Delete Webhook](webhooks.md#delete-webhook) <a href="#delete-webhook" id="delete-webhook"></a>

`DELETE/webhooks/{webhook.id}`

Deletes a webhook permanently. Requires the permission. Returns a 204 empty response on success. Fires a [Webhooks Update](https://docs.discord.food/topics/gateway-events#webhooks-update) Gateway event.

#### [Delete Webhook with Token](webhooks.md#delete-webhook-with-token) <a href="#delete-webhook-with-token" id="delete-webhook-with-token"></a>

`DELETE/webhooks/{webhook.id}/{webhook.token}`

Same as above, except this call does not require authentication.

#### [Execute Webhook](webhooks.md#execute-webhook) <a href="#execute-webhook" id="execute-webhook"></a>

`POST/webhooks/{webhook.id}/{webhook.token}`

Posts a message to the webhook's channel. Returns a [message](https://docs.discord.food/resources/message#message-object) object or 204 empty response, depending on . Fires a [Message Create](https://docs.discord.food/topics/gateway-events#message-create) Gateway event. See [message formatting](https://docs.discord.food/reference#message-formatting) for more information on how to properly format messages.

Files must be attached using a body (or pre-uploaded to Discord's GCP bucket) as described in [Uploading Files](https://docs.discord.food/reference#uploading-files).

[**Limitations**](webhooks.md#limitations)

* When executing on a forum channel, _one of_ or must be provided.
* The maximum request size when sending a message is **100 MiB**.
* For the embed object, you can set every field except (it will be regardless of if you try to set it), , , and any , , or values for images.

[**Query String Params**](webhooks.md#query-string-params)

| Field       | Type      | Description                                                                                                                                                                   |
| ----------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| wait?       | boolean   | Waits for confirmation of message send before response, and returns the created message body (default false; when false a message that is not saved does not return an error) |
| thread\_id? | snowflake | Send a message to the specified thread within a webhook's channel; the thread will automatically be unarchived                                                                |

[**JSON/Form Params**](webhooks.md#json/form-params)

| Field                       | Type                                                                                                | Description                                                                                                                                                         |
| --------------------------- | --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| username?                   | string                                                                                              | The name to override the default username of the webhook with (1-80 characters)                                                                                     |
| avatar\_url?                | string                                                                                              | The avatar URL to override the default avatar of the webhook with                                                                                                   |
| thread\_name?               | string                                                                                              | The name for the thread to create (requires the webhook channel to be a thread-only channel, 1-100 characters)                                                      |
| applied\_tags?              | array\[snowflake]                                                                                   | The IDs of the tags that are applied to the thread (requires the webhook channel to be a thread-only channel, max 5)                                                |
| content?                    | string                                                                                              | The message contents (up to 2000 characters)                                                                                                                        |
| tts?                        | boolean                                                                                             | Whether this is a TTS message                                                                                                                                       |
| embeds?                     | array\[[embed](https://docs.discord.food/resources/message#embed-object) object]                    | Embedded `rich` content (max 6000 characters, max 10)                                                                                                               |
| allowed\_mentions?          | [allowed mention](https://docs.discord.food/resources/message#allowed-mentions-object) object       | Allowed mentions for the message                                                                                                                                    |
| components? <sup>2</sup>    | array\[[message component](https://docs.discord.food/resources/components#component-object) object] | The components to include with the message                                                                                                                          |
| flags?                      | integer                                                                                             | The [message's flags](https://docs.discord.food/resources/message#message-flags) (only `SUPPRESS_EMBEDS`, `SUPPRESS_NOTIFICATIONS`, and `VOICE_MESSAGE` can be set) |
| files\[n]? <sup>1</sup>     | file contents                                                                                       | Contents of the file being sent (max 10)                                                                                                                            |
| payload\_json? <sup>1</sup> | string                                                                                              | JSON-encoded body of non-file params                                                                                                                                |
| attachments? <sup>1</sup>   | array\[partial [attachment](https://docs.discord.food/resources/message#attachment-object) object]  | Partial attachment objects with `filename` and `description` (max 10)                                                                                               |
| poll?                       | [poll create request](https://docs.discord.food/resources/message#poll-create-structure) object     | A poll!                                                                                                                                                             |

<sup>1</sup> See [Uploading Files](https://docs.discord.food/reference#uploading-files) for details.

<sup>2</sup> Requires query parameter. Interactions only work with an application-owned webhook.

#### [Execute Slack-Compatible Webhook](webhooks.md#execute-slack-compatible-webhook) <a href="#execute-slack-compatible-webhook" id="execute-slack-compatible-webhook"></a>

`POST/webhooks/{webhook.id}/{webhook.token}/slack`

Refer to [Slack's documentation](https://api.slack.com/incoming-webhooks) for more information. We do not support Slack's , , , or properties.

[**Query String Params**](webhooks.md#query-string-params)

| Field       | Type      | Description                                                                                                                                                                   | Required |
| ----------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| wait?       | boolean   | Waits for confirmation of message send before response, and returns the created message body (default false; when false a message that is not saved does not return an error) |          |
| thread\_id? | snowflake | Send a message to the specified thread within a webhook's channel; the thread will automatically be unarchived                                                                |          |

#### [Execute GitHub-Compatible Webhook](webhooks.md#execute-github-compatible-webhook) <a href="#execute-github-compatible-webhook" id="execute-github-compatible-webhook"></a>

`POST/webhooks/{webhook.id}/{webhook.token}/github`

[Add a new webhook](https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks) to your GitHub repo (in the repo's settings), and use this endpoint as the "Payload URL". You can choose what events your Discord channel receives by choosing the "Let me select individual events" option and selecting individual events for the new webhook you're configuring. The supported [events](https://docs.github.com/en/webhooks/webhook-events-and-payloads) are , , , , , , , , , , , , , , , , , and .

[**Query String Params**](webhooks.md#query-string-params)

| Field       | Type      | Description                                                                                                                                                                   | Required |
| ----------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| wait?       | boolean   | Waits for confirmation of message send before response, and returns the created message body (default false; when false a message that is not saved does not return an error) |          |
| thread\_id? | snowflake | Send a message to the specified thread within a webhook's channel; the thread will automatically be unarchived                                                                |          |

#### [Get Webhook Message](webhooks.md#get-webhook-message) <a href="#get-webhook-message" id="get-webhook-message"></a>

`GET/webhooks/{webhook.id}/{webhook.token}/messages/{message.id}`

Returns a previously-sent webhook [message](https://docs.discord.food/resources/message#message-object) object from the same token.

#### [Edit Webhook Message](webhooks.md#edit-webhook-message) <a href="#edit-webhook-message" id="edit-webhook-message"></a>

`PATCH/webhooks/{webhook.id}/{webhook.token}/messages/{message.id}`

Edits a previously-sent webhook message from the same token. Returns the updated [message](https://docs.discord.food/resources/message#message-object) object on success. Fires a [Message Update](https://docs.discord.food/topics/gateway-events#message-update) Gateway event.

When the field is edited, the array in the message object will be reconstructed from scratch based on the new content. The field of the edit request controls how this happens. If there is no explicit in the edit request, the content will be parsed with _default_ allowances, that is, without regard to whether or not an was present in the request that originally created the message.

Refer to [Uploading Files](https://docs.discord.food/reference#uploading-files) for details on attachments and requests. Any provided files will be **appended** to the message. To remove or replace files you will have to supply the field which specifies the files to retain on the message after edit.

[**JSON/Form Params**](webhooks.md#json/form-params)

| Field                       | Type                                                                                                | Description                                                                                                                                                         |
| --------------------------- | --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| content?                    | string                                                                                              | The message contents (up to 2000 characters)                                                                                                                        |
| tts?                        | boolean                                                                                             | Whether this is a TTS message                                                                                                                                       |
| embeds?                     | array\[[embed](https://docs.discord.food/resources/message#embed-object) object]                    | Embedded `rich` content (max 6000 characters, max 10)                                                                                                               |
| allowed\_mentions?          | [allowed mention](https://docs.discord.food/resources/message#allowed-mentions-object) object       | Allowed mentions for the message                                                                                                                                    |
| components? <sup>2</sup>    | array\[[message component](https://docs.discord.food/resources/components#component-object) object] | The components to include with the message                                                                                                                          |
| flags?                      | integer                                                                                             | The [message's flags](https://docs.discord.food/resources/message#message-flags) (only `SUPPRESS_EMBEDS`, `SUPPRESS_NOTIFICATIONS`, and `VOICE_MESSAGE` can be set) |
| files\[n]? <sup>1</sup>     | file contents                                                                                       | Contents of the file being sent (max 10)                                                                                                                            |
| payload\_json? <sup>1</sup> | string                                                                                              | JSON-encoded body of non-file params                                                                                                                                |
| attachments? <sup>1</sup>   | array\[partial [attachment](https://docs.discord.food/resources/message#attachment-object) object]  | Partial attachment objects with `filename` and `description` (max 10)                                                                                               |

<sup>1</sup> See [Uploading Files](https://docs.discord.food/reference#uploading-files) for details.

<sup>2</sup> Requires an application-owned webhook.

#### [Delete Webhook Message](webhooks.md#delete-webhook-message) <a href="#delete-webhook-message" id="delete-webhook-message"></a>

`DELETE/webhooks/{webhook.id}/{webhook.token}/messages/{message.id}`

Deletes a message that was created by the webhook. Returns a 204 empty response on success. Fires a [Message Delete](https://docs.discord.food/topics/gateway-events#message-delete) Gateway event.

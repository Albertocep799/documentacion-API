---
icon: message-lines
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

# Mensajes

### [Messages](mensajes.md#messages) <a href="#messages" id="messages"></a>

Messages are the core of Discord. They are the primary way users communicate with each other, and they can contain text, images, and other media. Embeddable content, such as polls, system messages, and calls are also represented as messages.

#### [Message Object](mensajes.md#message-object) <a href="#message-object" id="message-object"></a>

A message sent in a channel within Discord.

[**Message Structure**](mensajes.md#message-structure)

| Field                                          | Type                                                                                                                        | Description                                                                                        |
| ---------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| id                                             | snowflake                                                                                                                   | The ID of the message                                                                              |
| channel\_id                                    | snowflake                                                                                                                   | The ID of the channel the message was sent in                                                      |
| lobby\_id?                                     | snowflake                                                                                                                   | The ID of the lobby the message was sent in                                                        |
| author <sup>1</sup>                            | partial [user](https://docs.discord.food/resources/user#user-object) object                                                 | The author of the message                                                                          |
| content <sup>2</sup>                           | string                                                                                                                      | Contents of the message                                                                            |
| timestamp                                      | ISO8601 timestamp                                                                                                           | When this message was sent                                                                         |
| edited\_timestamp                              | ?ISO8601 timestamp                                                                                                          | When this message was last edited                                                                  |
| tts                                            | boolean                                                                                                                     | Whether this message will be read out by TTS                                                       |
| mention\_everyone                              | boolean                                                                                                                     | Whether this message mentions everyone                                                             |
| mentions                                       | array\[partial [user](https://docs.discord.food/resources/user#user-object) object]                                         | Users specifically mentioned in the message                                                        |
| mention\_roles                                 | array\[snowflake]                                                                                                           | Roles specifically mentioned in this message                                                       |
| mention\_channels? <sup>3</sup>                | array\[partial [channel](https://docs.discord.food/resources/channel#channel-object) object]                                | Channels specifically mentioned in this message                                                    |
| attachments <sup>2</sup>                       | array\[[attachment](mensajes.md#attachment-object) object]                                                                  | The attached files                                                                                 |
| embeds <sup>2</sup>                            | array\[[embed](mensajes.md#embed-object) object]                                                                            | Content embedded in the message                                                                    |
| reactions?                                     | array\[[reaction](mensajes.md#reaction-object) object]                                                                      | Reactions on the message                                                                           |
| nonce?                                         | integer \| string                                                                                                           | The message's nonce, used for message deduplication                                                |
| pinned                                         | boolean                                                                                                                     | Whether this message is pinned                                                                     |
| webhook\_id?                                   | snowflake                                                                                                                   | The ID of the webhook that send the message                                                        |
| type                                           | integer                                                                                                                     | The [type of message](mensajes.md#message-type)                                                    |
| activity?                                      | [message activity](mensajes.md#message-activity-object) object                                                              | The rich presence activity the author is inviting users to                                         |
| application?                                   | [integration application](https://docs.discord.food/resources/integration#integration-application-object) object            | The application of the message's rich presence activity                                            |
| application\_id?                               | snowflake                                                                                                                   | The ID of the application; only sent for interaction responses and messages created through OAuth2 |
| flags                                          | integer                                                                                                                     | The [message's flags](mensajes.md#message-flags)                                                   |
| message\_reference? <sup>4</sup>               | [message reference](mensajes.md#message-reference-object) object                                                            | The source of a crosspost, snapshot, channel follow add, pin, or reply message                     |
| referenced\_message? <sup>5</sup> <sup>4</sup> | ?[message object](mensajes.md#message-object)                                                                               | The message associated with the `message_reference`                                                |
| message\_snapshots? <sup>4</sup>               | array\[[message snapshot](mensajes.md#message-snapshot-object) object]                                                      | The partial message snapshot associated with the `message_reference`                               |
| call?                                          | [message call](mensajes.md#message-call-object) object                                                                      | The private channel call that prompted this message                                                |
| interaction? **(deprecated)**                  | [message interaction](https://docs.discord.food/interactions/receiving-and-responding#message-interaction-structure) object | The interaction the message is responding to                                                       |
| interaction\_metadata?                         | [message interaction metadata](mensajes.md#message-interaction-metadata-object) object                                      | The interaction the message originated from                                                        |
| resolved?                                      | [select menu resolved](https://docs.discord.food/resources/components#select-menu-resolved-object) object                   | The resolved data for the select menu component's interaction                                      |
| thread?                                        | [channel](https://docs.discord.food/resources/channel#channel-object) object                                                | The thread that was started from this message, with thekey representing thread member data         |
| role\_subscription\_data?                      | [message role subscription](mensajes.md#message-role-subscription-object) object                                            | The role subscription purchase or renewal that prompted this message                               |
| purchase\_notification?                        | [message purchase notification](mensajes.md#message-purchase-notification-object) object                                    | The guild purchase that prompted this message                                                      |
| gift\_info?                                    | [message gift info](mensajes.md#message-gift-info-object) object                                                            | Information on the gift that prompted this message                                                 |
| components <sup>2</sup>                        | array\[[message component](https://docs.discord.food/resources/components#component-object) object]                         | The message's components (e.g. buttons, select menus)                                              |
| sticker\_items?                                | array\[[sticker item](https://docs.discord.food/resources/sticker#sticker-item-object) object]                              | The message's sticker items                                                                        |
| stickers?                                      | array\[[sticker](https://docs.discord.food/resources/sticker#sticker-object) object]                                        | Extra rich information for the message's sticker items; only available in some contexts            |
| poll? <sup>2</sup>                             | [poll](mensajes.md#poll-object) object                                                                                      | A poll!                                                                                            |
| changelog\_id?                                 | snowflake                                                                                                                   | The ID of the changelog that prompted this message                                                 |
| soundboard\_sounds?                            | array\[[soundboard sound](https://docs.discord.food/resources/soundboard#soundboard-sound-object) object]                   | The message's soundboard sounds                                                                    |
| potions?                                       | array\[[potion](mensajes.md#potion-object) object]                                                                          | Potions applied to the message                                                                     |

<sup>1</sup> The object follows the structure of the user object, but may not be a valid user in the case where the message is generated by a webhook; you can recognize this by checking for the key on the message object. In these cases, the below notice about the additional keys does not apply as webhooks are not guild members.

<sup>2</sup> Users must configure (or, in the case of bots, be approved for) the [intent](https://docs.discord.food/topics/gateway#message-content-intent) to receive non-empty values for these fields in most situations.

<sup>3</sup> Not all channel mentions in a message will appear in . Only textual channels that are visible to everyone in a lurkable guild will ever be included. Only crossposted messages (via Channel Following) currently include at all. If no mentions in the message meet these requirements, this field will not be sent.

<sup>4</sup> See the [message reference types](mensajes.md#message-reference-type) for more information on which message references have which fields.

<sup>5</sup> This field is only returned for messages with a of , , or . If the message is one of these but the field is not present, the backend did not attempt to fetch the message, so its state is unknown. If the field exists but is , the referenced message was deleted.

[**Partial Message Structure**](mensajes.md#partial-message-structure)

This structure is a subset of the [message](mensajes.md#message-object) object above with the following fields:

| Field                       | Type                                                                          | Description                                      |
| --------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------ |
| id                          | snowflake                                                                     | The ID of the message                            |
| lobby\_id?                  | snowflake                                                                     | The ID of the lobby the message was sent in      |
| channel\_id <sup>1</sup>    | snowflake                                                                     | The ID of the channel the message was sent in    |
| type?                       | integer                                                                       | The [type of message](mensajes.md#message-type)  |
| content                     | string                                                                        | Contents of the message                          |
| author                      | partial [user](https://docs.discord.food/resources/user#user-object) object   | The author of the message                        |
| flags?                      | integer                                                                       | The [message's flags](mensajes.md#message-flags) |
| application\_id?            | snowflake                                                                     | The ID of the application                        |
| channel? <sup>2</sup>       | [channnel](https://docs.discord.food/resources/channel#channel-object) object | The channel the message was sent in              |
| recipient\_id? <sup>2</sup> | snowflake                                                                     | The ID of the other recipient                    |

<sup>1</sup> If the message was sent in a lobby and the lobby has no channel linked, this will be equal to the ID of the lobby.

<sup>2</sup> These fields will be present only in ephemeral DM channels.

[**Message Type**](mensajes.md#message-type)

| Value  | Name                                                       | Description                                                                                                           | Rendered Content                                                                                                                                                                                    | Deletable         |
| ------ | ---------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- |
| 0      | DEFAULT                                                    | A default message (see below)                                                                                         | "{content}"                                                                                                                                                                                         | true              |
| 1      | RECIPIENT\_ADD                                             | A message sent when a user is added to a group DM or thread                                                           | "{author} added {mentions\[0]} to the {group/thread}."                                                                                                                                              | false             |
| 2      | RECIPIENT\_REMOVE                                          | A message sent when a user is removed from a group DM or thread                                                       | "{author} removed {mentions\[0]} from the {group/thread}."                                                                                                                                          | false             |
| 3      | CALL                                                       | A message sent when a user creates a call in a private channel                                                        | participated ? "{author} started a call{ended ? " that lasted {duration}" : " — Join the call"}." : "You missed a call from {author} that lasted {duration}."                                       | false             |
| 4      | CHANNEL\_NAME\_CHANGE                                      | A message sent when a group DM or thread's name is changed                                                            | "{author} changed the {is\_forum ? "post title" : "channel name"}: **{content}**"                                                                                                                   | false             |
| 5      | CHANNEL\_ICON\_CHANGE                                      | A message sent when a group DM's icon is changed                                                                      | "{author} changed the channel icon."                                                                                                                                                                | false             |
| 6      | CHANNEL\_PINNED\_MESSAGE                                   | A message sent when a message is pinned in a channel                                                                  | "{author} pinned a message to this channel."                                                                                                                                                        | true              |
| 7      | USER\_JOIN                                                 | A message sent when a user joins a guild                                                                              | See [user join message type](mensajes.md#user-join-message-type), obtained via the formula `timestamp_ms % 13`                                                                                      | true              |
| 8      | PREMIUM\_GUILD\_SUBSCRIPTION                               | A message sent when a user subscribes to (boosts) a guild                                                             | "{author} just boosted the server{content ? " **{content}** times"}!"                                                                                                                               | true              |
| 9      | PREMIUM\_GUILD\_SUBSCRIPTION\_TIER\_1                      | A message sent when a user subscribes to (boosts) a guild to tier 1                                                   | "{author} just boosted the server{content ? " **{content}** times"}! {guild} has achieved **Level 1!**"                                                                                             | true              |
| 10     | PREMIUM\_GUILD\_SUBSCRIPTION\_TIER\_2                      | A message sent when a user subscribes to (boosts) a guild to tier 2                                                   | "{author} just boosted the server{content ? " **{content}** times"}! {guild} has achieved **Level 2!**"                                                                                             | true              |
| 11     | PREMIUM\_GUILD\_SUBSCRIPTION\_TIER\_3                      | A message sent when a user subscribes to (boosts) a guild to tier 3                                                   | "{author} just boosted the server{content ? " **{content}** times"}! {guild} has achieved **Level 3!**"                                                                                             | true              |
| 12     | CHANNEL\_FOLLOW\_ADD                                       | A message sent when a news channel is followed                                                                        | "{author} has added {content} to this channel. Its most important updates will show up here."                                                                                                       | true              |
| ~~13~~ | ~~GUILD\_STREAM~~                                          | ~~A message sent when a user starts streaming in a guild~~                                                            |                                                                                                                                                                                                     | ~~true~~          |
| 14     | GUILD\_DISCOVERY\_DISQUALIFIED                             | A message sent when a guild is disqualified from discovery                                                            | "This server has been removed from Server Discovery because it no longer passes all the requirements. Check Server Settings for more details."                                                      | true              |
| 15     | GUILD\_DISCOVERY\_REQUALIFIED                              | A message sent when a guild requalifies for discovery                                                                 | "This server is eligible for Server Discovery again and has been automatically relisted!"                                                                                                           | true              |
| 16     | GUILD\_DISCOVERY\_GRACE\_PERIOD\_INITIAL\_WARNING          | A message sent when a guild has failed discovery requirements for a week                                              | "This server has failed Discovery activity requirements for 1 week. If this server fails for 4 weeks in a row, it will be automatically removed from Discovery."                                    | true              |
| 17     | GUILD\_DISCOVERY\_GRACE\_PERIOD\_FINAL\_WARNING            | A message sent when a guild has failed discovery requirements for 3 weeks                                             | "This server has failed Discovery activity requirements for 3 weeks in a row. If this server fails for 1 more week, it will be removed from Discovery."                                             | true              |
| 18     | THREAD\_CREATED                                            | A message sent when a thread is created                                                                               | "{author} started a thread: **{content}**. See all threads."                                                                                                                                        | true              |
| 19     | REPLY                                                      | A message sent when a user replies to a message                                                                       | "{content}"                                                                                                                                                                                         | true              |
| 20     | CHAT\_INPUT\_COMMAND                                       | A message sent when a user uses a slash command                                                                       | "{content}"                                                                                                                                                                                         | true              |
| 21     | THREAD\_STARTER\_MESSAGE                                   | A message sent when a thread starter message is added to a thread                                                     | "{referenced\_message?.content}" ?? "Sorry, we couldn't load the first message in this thread"                                                                                                      | false             |
| 22     | GUILD\_INVITE\_REMINDER                                    | A message sent to remind users to invite friends to a guild                                                           | "Wondering who to invite?\nStart by inviting anyone who can help you build the server!"                                                                                                             | true              |
| 23     | CONTEXT\_MENU\_COMMAND                                     | A message sent when a user uses a context menu command                                                                | "{content}"                                                                                                                                                                                         | true              |
| 24     | AUTO\_MODERATION\_ACTION                                   | A message sent when auto moderation takes an action                                                                   | [Special embed rendered from](https://docs.discord.food/resources/auto-moderation#automod-system-messages)                                                                                          | true <sup>1</sup> |
| 25     | ROLE\_SUBSCRIPTION\_PURCHASE                               | A message sent when a user purchases or renews a role subscription                                                    | "{author} {is\_renewal ? "renewed" : "joined"} **{role\_subscription.tier\_name}** and has been a subscriber of {guild} for {role\_subscription.total\_months\_subscribed} month(?s)!"              | true              |
| 26     | INTERACTION\_PREMIUM\_UPSELL                               | A message sent when a user is upsold to a premium interaction                                                         | "{content}"                                                                                                                                                                                         | true              |
| 27     | STAGE\_START                                               | A message sent when a stage channel starts                                                                            | "{author} started **{content}**"                                                                                                                                                                    | true              |
| 28     | STAGE\_END                                                 | A message sent when a stage channel ends                                                                              | "{author} ended **{content}**"                                                                                                                                                                      | true              |
| 29     | STAGE\_SPEAKER                                             | A message sent when a user starts speaking in a stage channel                                                         | "{author} is now a speaker."                                                                                                                                                                        | true              |
| 30     | STAGE\_RAISE\_HAND                                         | A message sent when a user raises their hand in a stage channel                                                       | "{author} requested to speak."                                                                                                                                                                      | true              |
| 31     | STAGE\_TOPIC                                               | A message sent when a stage channel's topic is changed                                                                | "{author} changed the Stage topic: **{content}**"                                                                                                                                                   | true              |
| 32     | GUILD\_APPLICATION\_PREMIUM\_SUBSCRIPTION                  | A message sent when a user purchases an application premium subscription                                              | "{author} upgraded {application ?? "a deleted application"} to premium for this server!"                                                                                                            | true              |
| ~~33~~ | ~~PRIVATE\_CHANNEL\_INTEGRATION\_ADDED~~                   | ~~A message sent when a user adds an application to group DM~~                                                        | ~~"{author} added {"the {application} app" ?? "a deleted application"}. See our~~ [~~Help Centre~~](https://support.discord.com/hc/en-us/articles/15104189280151-Apps-in-DMs) ~~for more info."~~   | ~~false~~         |
| ~~34~~ | ~~PRIVATE\_CHANNEL\_INTEGRATION\_REMOVED~~                 | ~~A message sent when a user removed an application from a group DM~~                                                 | ~~"{author} removed {"the {application} app" ?? "a deleted application"}. See our~~ [~~Help Centre~~](https://support.discord.com/hc/en-us/articles/15104189280151-Apps-in-DMs) ~~for more info."~~ | ~~false~~         |
| 35     | PREMIUM\_REFERRAL                                          | A message sent when a user gifts a premium (Nitro) referral                                                           | "{content}"                                                                                                                                                                                         | true              |
| 36     | GUILD\_INCIDENT\_ALERT\_MODE\_ENABLED                      | A message sent when a user enabled lockdown for the guild                                                             | "{author} enabled security actions until {content}."                                                                                                                                                | true              |
| 37     | GUILD\_INCIDENT\_ALERT\_MODE\_DISABLED                     | A message sent when a user disables lockdown for the guild                                                            | "{author} disabled security actions."                                                                                                                                                               | true              |
| 38     | GUILD\_INCIDENT\_REPORT\_RAID                              | A message sent when a user reports a raid for the guild                                                               | "{author} reported a raid in {guild}."                                                                                                                                                              | true              |
| 39     | GUILD\_INCIDENT\_REPORT\_FALSE\_ALARM                      | A message sent when a user reports a false alarm for the guild                                                        | "{author} reported a false alarm in {guild}."                                                                                                                                                       | true              |
| 40     | GUILD\_DEADCHAT\_REVIVE\_PROMPT                            | A message sent when no one sends a message in the current channel for 1 hour                                          | "{content}"                                                                                                                                                                                         | true              |
| 41     | CUSTOM\_GIFT                                               | A message sent when a user buys another user a gift                                                                   | Special embed rendered from `embeds[0].url` and                                                                                                                                                     | true              |
| 42     | GUILD\_GAMING\_STATS\_PROMPT                               |                                                                                                                       | "{content}"                                                                                                                                                                                         | true              |
| ~~43~~ | ~~POLL~~                                                   | ~~A message sent when a user posts a poll~~                                                                           |                                                                                                                                                                                                     | ~~true~~          |
| 44     | PURCHASE\_NOTIFICATION                                     | A message sent when a user purchases a guild product                                                                  | "{author} has purchased {purchase\_notification.guild\_product\_purchase.product\_name}!"                                                                                                           | true              |
| ~~45~~ | ~~VOICE\_HANGOUT\_INVITE~~                                 | ~~A message sent when a user invites another user to hangout in a voice channel~~                                     | ~~Special embed rendered from~~                                                                                                                                                                     | ~~true~~          |
| 46     | POLL\_RESULT                                               | A message sent when a poll is finalized                                                                               | [Special embed rendered from](mensajes.md#poll-result-notifications)                                                                                                                                | true              |
| 47     | CHANGELOG                                                  | A message sent by the Discord Updates account when a new changelog is posted                                          | "{content}"                                                                                                                                                                                         | true              |
| 48     | NITRO\_NOTIFICATION                                        | A message sent when a Nitro promotion is triggered                                                                    | Special embed rendered from `content`                                                                                                                                                               | true              |
| 49     | CHANNEL\_LINKED\_TO\_LOBBY                                 | A message sent when a voice channel is linked to a lobby                                                              | "{content}"                                                                                                                                                                                         | true              |
| 50     | GIFTING\_PROMPT                                            | A local-only ephemeral message sent when a user is prompted to gift Nitro to a friend on their friendship anniversary | Special embed                                                                                                                                                                                       | true              |
| 51     | IN\_GAME\_MESSAGE\_NUX                                     | A local-only message sent when a user receives an in-game message NUX                                                 | "{author} messaged you from {application.name}. In-game chat may not include rich messaging features such as images, polls, or apps. [Learn More](https://support.discord.com/hc)"                  | true              |
| 52     | GUILD\_JOIN\_REQUEST\_ACCEPT\_NOTIFICATION <sup>2</sup>    | A message sent when a user accepts a guild join request                                                               | "{join\_request.user}'s application to **{content}** was approved! Welcome!"                                                                                                                        | true              |
| 53     | GUILD\_JOIN\_REQUEST\_REJECT\_NOTIFICATION <sup>2</sup>    | A message sent when a user rejects a guild join request                                                               | "{join\_request.user}'s application to **{content}** was rejected."                                                                                                                                 | true              |
| 54     | GUILD\_JOIN\_REQUEST\_WITHDRAWN\_NOTIFICATION <sup>2</sup> | A message sent when a user withdraws a guild join request                                                             | "{join\_request.user}'s application to **{content}** has been withdrawn."                                                                                                                           | true              |
| 55     | HD\_STREAMING\_UPGRADED                                    | A message sent when a user upgrades to HD streaming                                                                   | "{author} activated **HD Splash Potion**"                                                                                                                                                           | true              |

<sup>1</sup> Can only be deleted by members with the permission.

<sup>2</sup> The join request can be retrieved using the ID of the channel the message was sent in.

[**User Join Message Type**](mensajes.md#user-join-message-type)

The type of rendered message is determined via converting the message's to a unix timestamp with millisecond precision, modulo 13.

| Value | Rendered Content                                |
| ----- | ----------------------------------------------- |
| 0     | "{author} joined the party."                    |
| 1     | "{author} is here."                             |
| 2     | "Welcome, {author}. We hope you brought pizza." |
| 3     | "A wild {author} appeared."                     |
| 4     | "{author} just landed."                         |
| 5     | "{author} just slid into the server."           |
| 6     | "{author} just showed up!"                      |
| 7     | "Welcome {author}. Say hi!"                     |
| 8     | "{author} hopped into the server."              |
| 9     | "Everyone welcome {author}!"                    |
| 10    | "Glad you're here, {author}."                   |
| 11    | "Good to see you, {author}."                    |
| 12    | "Yay you made it, {author}!"                    |

[**Message Flags**](mensajes.md#message-flags)

| Value   | Name                                         | Description                                                                  |
| ------- | -------------------------------------------- | ---------------------------------------------------------------------------- |
| 1 << 0  | CROSSPOSTED                                  | Message has been published to subscribed channels (via Channel Following)    |
| 1 << 1  | IS\_CROSSPOST                                | Message originated from a message in another channel (via Channel Following) |
| 1 << 2  | SUPPRESS\_EMBEDS                             | Embeds will not be included when serializing this message                    |
| 1 << 3  | SOURCE\_MESSAGE\_DELETED                     | Source message for this crosspost has been deleted (via Channel Following)   |
| 1 << 4  | URGENT                                       | Message came from the urgent message system                                  |
| 1 << 5  | HAS\_THREAD                                  | Message has an associated thread, with the same ID as the message            |
| 1 << 6  | EPHEMERAL                                    | Message is only visible to the user who invoked the interaction              |
| 1 << 7  | LOADING                                      | Message is an interaction response and the bot is "thinking"                 |
| 1 << 8  | FAILED\_TO\_MENTION\_SOME\_ROLES\_IN\_THREAD | Some roles were not mentioned and added to the thread                        |
| 1 << 9  | GUILD\_FEED\_HIDDEN                          | Message is hidden from the guild's feed                                      |
| 1 << 10 | SHOULD\_SHOW\_LINK\_NOT\_DISCORD\_WARNING    | Message contains a link that impersonates Discord                            |
| 1 << 12 | SUPPRESS\_NOTIFICATIONS                      | Message will not trigger push and desktop notifications                      |
| 1 << 13 | IS\_VOICE\_MESSAGE                           | Message's audio attachment is rendered as a voice message                    |
| 1 << 14 | HAS\_SNAPSHOT                                | Message has a forwarded message snapshot attached                            |
| 1 << 15 | IS\_COMPONENTS\_V2                           | Message contains components from version 2 of the UI kit                     |
| 1 << 16 | SENT\_BY\_SOCIAL\_LAYER\_INTEGRATION         | Message was triggered by the social layer integration                        |

[**Example Message**](mensajes.md#example-message)

```
{  "id": "1076229630052270231",  "type": 0,  "content": "guh",  "channel_id": "1029316811088478299",  "author": {    "id": "545581357812678656",    "username": "alien",    "global_name": "Alien",    "avatar": "60387de43133809b083fb0f7458d2708",    "avatar_decoration_data": null,    "discriminator": "0",    "public_flags": 4194432,    "primary_guild": null  },  "attachments": [],  "embeds": [],  "mentions": [],  "mention_roles": [],  "pinned": false,  "mention_everyone": false,  "tts": false,  "timestamp": "2023-02-17T19:52:19.184000+00:00",  "edited_timestamp": null,  "flags": 0,  "components": [],  "reactions": [    {      "emoji": { "id": null, "name": "‼️" },      "count": 2,      "count_details": { "burst": 0, "normal": 82 },      "burst_colors": ["#f0ca59", "#4c704f", "#e07f45", "#f0ae59", "#f0bc59", "#f0a059", "#e08e45", "#f0984a"],      "me_burst": false,      "me": false    }  ]}
```

[**Example Crossposted Message**](mensajes.md#example-crossposted-message)

```
{  "id": "1076589465084104805",  "type": 0,  "content": "test",  "channel_id": "909072028911423498",  "author": {    "bot": true,    "id": "999498190359371866",    "username": "Testing Server #News",    "avatar": "47b30f67c7e2c15637936cd180dd681c",    "discriminator": "0000"  },  "attachments": [],  "embeds": [],  "mentions": [],  "mention_roles": [],  "pinned": false,  "mention_everyone": false,  "tts": false,  "timestamp": "2023-02-18T19:42:10.541000+00:00",  "edited_timestamp": null,  "flags": 2,  "components": [],  "webhook_id": "999498190359371866",  "message_reference": {    "type": 0,    "channel_id": "901900332408381450",    "guild_id": "901900332408381449",    "message_id": "1076589456297054320"  }}
```

#### [Message Activity Object](mensajes.md#message-activity-object) <a href="#message-activity-object" id="message-activity-object"></a>

A rich presence invite in a message. See [Get Activity Secret](https://docs.discord.food/resources/presence#get-activity-secret) for more information.

[**Message Activity Structure**](mensajes.md#message-activity-structure)

| Field                    | Type    | Description                                                                                       |
| ------------------------ | ------- | ------------------------------------------------------------------------------------------------- |
| type                     | integer | The [type of activity request](https://docs.discord.food/resources/presence#activity-action-type) |
| session\_id <sup>1</sup> | string  | The session ID associated with this activity                                                      |
| party\_id?               | string  | The activity's party ID                                                                           |

<sup>1</sup> This field is send-only.

#### [Message Call Object](mensajes.md#message-call-object) <a href="#message-call-object" id="message-call-object"></a>

A call in a private channel.

[**Message Call Structure**](mensajes.md#message-call-structure)

| Field                         | Type               | Description                                          |
| ----------------------------- | ------------------ | ---------------------------------------------------- |
| participants                  | array\[snowflake]  | The channel recipients that participated in the call |
| ended\_timestamp <sup>1</sup> | ?ISO8601 timestamp | When the call ended, if it has                       |

<sup>1</sup> This is a best-effort marker and may be unexpectedly in some cases.

#### [Message Interaction Metadata Object](mensajes.md#message-interaction-metadata-object) <a href="#message-interaction-metadata-object" id="message-interaction-metadata-object"></a>

Metadata about the interaction, including the source of the interaction and the relevant guild and users.

[**Message Interaction Metadata Structure**](mensajes.md#message-interaction-metadata-structure)

| Field                              | Type                                                                                   | Description                                                                                                                                                                                                       |
| ---------------------------------- | -------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id                                 | snowflake                                                                              | The ID of the interaction                                                                                                                                                                                         |
| type                               | integer                                                                                | The [type of interaction](https://docs.discord.food/interactions/receiving-and-responding#interaction-type)                                                                                                       |
| name?                              | string                                                                                 | The name of the [application command](https://docs.discord.food/interactions/application-commands#application-command-object) executed (including subcommands and subcommand groups), present only oninteractions |
| command\_type?                     | integer                                                                                | The [type of application command](https://docs.discord.food/interactions/application-commands#application-command-types) executed, present only oninteractions                                                    |
| ephemerality\_reason?              | integer                                                                                | The [reason this interaction is ephemeral](mensajes.md#ephemerality-reason)                                                                                                                                       |
| user                               | partial [user](https://docs.discord.food/resources/user#user-object) object            | The user that initiated the interaction                                                                                                                                                                           |
| authorizing\_integration\_owners   | map\[integer, snowflake]                                                               | IDs for each [installation context](https://docs.discord.food/resources/application#application-integration-type) related to an interaction                                                                       |
| original\_response\_message\_id?   | snowflake                                                                              | The ID of the original response message, present only on [follow-up messages](https://docs.discord.food/interactions/receiving-and-responding#followup-messages)                                                  |
| interacted\_message\_id?           | snowflake                                                                              | ID of the message that contained interactive component, present only on messages created from component interactions                                                                                              |
| triggering\_interaction\_metadata? | [message interaction metadata](mensajes.md#message-interaction-metadata-object) object | Metadata for the interaction that was used to open the modal, present only oninteractions                                                                                                                         |
| target\_user?                      | partial [user](https://docs.discord.food/resources/user#user-object) object            | The user that was targeted by the interaction, present only oninteractions                                                                                                                                        |
| target\_message\_id?               | snowflake                                                                              | The ID of the message that was targeted by the interaction, present only oninteractions                                                                                                                           |

[**Ephemerality Reason**](mensajes.md#ephemerality-reason)

| Value | Name                        | Description                                                                                            |
| ----- | --------------------------- | ------------------------------------------------------------------------------------------------------ |
| 0     | NONE                        | Unknown reason                                                                                         |
| 1     | FEATURE\_LIMITED            | A required feature is temporarily limited                                                              |
| 2     | GUILD\_FEATURE\_LIMITED     | A required feature is temporarily limited for this guild                                               |
| 3     | USER\_FEATURE\_LIMITED      | A required feature is temporarily limited for this user                                                |
| 4     | SLOWMODE                    | The user is sending messages [past their](https://docs.discord.food/resources/channel#channel-object)  |
| 5     | RATE\_LIMIT                 | The user is being rate limited                                                                         |
| 6     | CANNOT\_MESSAGE\_USER       | The user does not have permission to message the target user                                           |
| 7     | USER\_VERIFICATION\_LEVEL   | The user does not meet the [guild ](https://docs.discord.food/resources/guild#guild-object)requirement |
| 8     | CANNOT\_UNARCHIVE\_THREAD   | The user does not have permission to unarchive the thread                                              |
| 9     | CANNOT\_JOIN\_THREAD        | The user does not have permission to join the thread                                                   |
| 10    | MISSING\_PERMISSIONS        | The user does not have permission to send messages in the channel                                      |
| 11    | CANNOT\_SEND\_ATTACHMENTS   | The user does not have permission to send attachments in the channel                                   |
| 12    | CANNOT\_SEND\_EMBEDS        | The user does not have permission to send embeds in the channel                                        |
| 13    | CANNOT\_SEND\_STICKERS      | The user does not have permission to send stickers in the channel                                      |
| 14    | AUTOMOD\_BLOCKED            | The message was blocked by AutoMod                                                                     |
| 15    | HARMFUL\_LINK               | The message contains a link blocked by Discord                                                         |
| 16    | CANNOT\_USE\_COMMAND        | The user does not have permission to use this command in this channel                                  |
| 17    | BETA\_GUILD\_SIZE           | The message is only visible to the user for this beta test                                             |
| 18    | CANNOT\_USE\_EXTERNAL\_APPS | The user does not have permission to use external applications in this channel                         |

#### [Message Role Subscription Object](mensajes.md#message-role-subscription-object) <a href="#message-role-subscription-object" id="message-role-subscription-object"></a>

A role subscription purchase or renewal.

[**Message Role Subscription Structure**](mensajes.md#message-role-subscription-structure)

| Field                           | Type      | Description                                                           |
| ------------------------------- | --------- | --------------------------------------------------------------------- |
| role\_subscription\_listing\_id | snowflake | The ID of the sku and listing that the user is subscribed to          |
| tier\_name                      | string    | The name of the tier that the user is subscribed to                   |
| total\_months\_subscribed       | integer   | The cumulative number of months that the user has been subscribed for |
| is\_renewal                     | boolean   | Whether this notification is for a renewal rather than a new purchase |

#### [Message Purchase Notification Object](mensajes.md#message-purchase-notification-object) <a href="#message-purchase-notification-object" id="message-purchase-notification-object"></a>

A guild product purchase notification.

[**Message Purchase Notification Structure**](mensajes.md#message-purchase-notification-structure)

| Field                    | Type                                                                    | Description                                                            |
| ------------------------ | ----------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| type                     | integer                                                                 | The [type of purchase](mensajes.md#message-purchase-notification-type) |
| guild\_product\_purchase | ?[guild product purchase](mensajes.md#guild-product-purchase-structure) | The guild product purchase that prompted this message                  |

[**Message Purchase Notification Type**](mensajes.md#message-purchase-notification-type)

Determines the type of purchase notification.

| Value | Name           | Description              |
| ----- | -------------- | ------------------------ |
| 0     | GUILD\_PRODUCT | A guild product purchase |

[**Guild Product Purchase Structure**](mensajes.md#guild-product-purchase-structure)

| Field         | Type      | Description                                      |
| ------------- | --------- | ------------------------------------------------ |
| listing\_id   | snowflake | The ID of the product listing that was purchased |
| product\_name | string    | The name of the product that was purchased       |

#### [Message Gift Info Object](mensajes.md#message-gift-info-object) <a href="#message-gift-info-object" id="message-gift-info-object"></a>

Information on a gift that prompted a message. The relevant gift link is provided in the first embed of the message.

[**Message Gift Info Structure**](mensajes.md#message-gift-info-structure)

| Field               | Type                                                                              | Description                        |
| ------------------- | --------------------------------------------------------------------------------- | ---------------------------------- |
| emoji? <sup>1</sup> | partial [emoji](https://docs.discord.food/resources/emoji#emoji-object) object    | The emoji associated with the gift |
| sound?              | [message soundboard sound](mensajes.md#message-soundboard-sound-structure) object | The sound associated with the gift |

<sup>1</sup> The emoji and fields are user-provided and not guaranteed to be valid or accurate. The field is never present.

[**Message Soundboard Sound Structure**](mensajes.md#message-soundboard-sound-structure)

| Field | Type   | Description                    |
| ----- | ------ | ------------------------------ |
| id    | string | The ID of the soundboard sound |

#### [Message Reference Object](mensajes.md#message-reference-object) <a href="#message-reference-object" id="message-reference-object"></a>

A reference to an originating (replied to) message.

Message references are generic attribution on a message. There are multiple message types that have a object.

[**Message Reference Structure**](mensajes.md#message-reference-structure)

| Field                                    | Type                                                                      | Description                                                                                                                |
| ---------------------------------------- | ------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| type? <sup>1</sup>                       | integer                                                                   | The [type of message reference](mensajes.md#message-reference-type) (default `DEFAULT`)                                    |
| message\_id?                             | snowflake                                                                 | The ID of the originating message                                                                                          |
| channel\_id <sup>1</sup>                 | snowflake                                                                 | The ID of the originating channel                                                                                          |
| guild\_id?                               | snowflake                                                                 | The ID of the originating channel's guild                                                                                  |
| fail\_if\_not\_exists? <sup>2</sup>      | boolean                                                                   | Whether to error if the referenced message doesn't exist instead of sending as a normal (non-reply) message (default true) |
| forward\_only? <sup>2</sup> <sup>3</sup> | [message forward only](mensajes.md#message-forward-only-structure) object | What to include in the forwarded message                                                                                   |

<sup>1</sup> Optional when creating a reply, but will always be present when receiving this object. In future API versions this will become a required field.

<sup>2</sup> This field is send-only.

<sup>3</sup> Only applicable to type references.

[**Message Forward Only Structure**](mensajes.md#message-forward-only-structure)

| Field            | Type              | Description                                                     |
| ---------------- | ----------------- | --------------------------------------------------------------- |
| embed\_indices?  | array\[integer]   | The indices of the embeds from the original message to include  |
| attachment\_ids? | array\[snowflake] | The IDs of the attachments from the original message to include |

[**Message Reference Type**](mensajes.md#message-reference-type)

Determines how associated data is populated.

| Value | Name                 | Description                                               | Coupled Message Field |
| ----- | -------------------- | --------------------------------------------------------- | --------------------- |
| 0     | DEFAULT              | A standard reference used by replies and system messages  | `referenced_message`? |
| 1     | FORWARD <sup>1</sup> | A reference used to point to a message at a point in time | `message_snapshot`    |

<sup>1</sup> This can only be used for basic messages, i.e., messages which do not have strong bindings to a non-global entity. Thus it only supports messages with or types, without any polls, calls, or components. This is subject to change in the future.

#### [Message Snapshot Object](mensajes.md#message-snapshot-object) <a href="#message-snapshot-object" id="message-snapshot-object"></a>

A snapshot of a partial message at a point in time.

[**Message Snapshot Structure**](mensajes.md#message-snapshot-structure)

| Field   | Type                                                              | Description                                            |
| ------- | ----------------------------------------------------------------- | ------------------------------------------------------ |
| message | [snapshot message](mensajes.md#snapshot-message-structure) object | A snapshot of the message when the forward was created |

[**Snapshot Message Structure**](mensajes.md#snapshot-message-structure)

| Field                                 | Type                                                                                                      | Description                                                   |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| content <sup>1</sup>                  | string                                                                                                    | Contents of the message                                       |
| timestamp                             | ISO8601 timestamp                                                                                         | When this message was sent                                    |
| edited\_timestamp                     | ?ISO8601 timestamp                                                                                        | when this message was last edited                             |
| mentions                              | array\[partial [user](https://docs.discord.food/resources/user#user-object) object]                       | Users specifically mentioned in the message                   |
| mention\_roles                        | array\[snowflake]                                                                                         | Roles specifically mentioned in this message                  |
| attachments <sup>1</sup>              | array\[[attachment](mensajes.md#attachment-object) object]                                                | The attached files                                            |
| embeds <sup>1</sup>                   | array\[[embed](mensajes.md#embed-object) object]                                                          | Content embedded in the message                               |
| type                                  | integer                                                                                                   | The [type of message](mensajes.md#message-type)               |
| flags                                 | integer                                                                                                   | The [message's flags](mensajes.md#message-flags)              |
| components? <sup>1</sup> <sup>2</sup> | array\[[message component](https://docs.discord.food/resources/components#component-object) object]       | The message's components (e.g. buttons, select menus)         |
| resolved?                             | [select menu resolved](https://docs.discord.food/resources/components#select-menu-resolved-object) object | The resolved data for the select menu component's interaction |
| sticker\_items?                       | array\[[sticker item](https://docs.discord.food/resources/sticker#sticker-item-object) object]            | The message's sticker items                                   |
| soundboard\_sounds?                   | array\[[soundboard sound](https://docs.discord.food/resources/soundboard#soundboard-sound-object) object] | The message's soundboard sounds                               |

<sup>1</sup> Users must configure (or, in the case of bots, be approved for) the [intent](https://docs.discord.food/topics/gateway#message-content-intent) to receive non-empty values for these fields in most situations.

<sup>2</sup> Any interactive components will be artificially marked as disabled in the snapshot.

[**Example Message Snapshot**](mensajes.md#example-message-snapshot)

```
{  "message": {    "type": 0,    "content": "guh",    "mentions": [],    "mention_roles": [],    "attachments": [],    "embeds": [],    "timestamp": "2023-02-17T19:52:19.184000+00:00",    "edited_timestamp": null,    "flags": 0,    "components": []  }}
```

[**Message Types**](mensajes.md#message-types)

There are multiple message types that have a object. Since message references are generic attribution to a previous message, there will be more types of messages which have this information in the future.

[**Crosspost Messages**](mensajes.md#crosspost-messages)

* These are messages that originated from another channel ( flag).
* These messages have all three fields, with data of the original message that was crossposted.

[**Forwarded Messages**](mensajes.md#forwarded-messages)

* These are messages which capture a snapshot of a message, preventing spoofing or tampering ( flag).
* These messages have an array offield containing a copy of the original message. This copy follows the same structure as a message, but has only the minimal set of fields returned required for context/rendering.
* A forwarded message can be identified by looking at it's field.
  * Message snapshots will be the message data associated with the forward. Currently only 1 snapshot is supported.
  * Message snapshots are taken at moment the forward message is created, and are **immutable**; any mutations to the orignal message will not be propagated.
* Forwards are created by including aof atype when sending a message. When sending, , , and are required, and the requester must have permissions.
* You can opt to only include specific attachments and embed in the forward by including the [field in the ](mensajes.md#message-reference-object).
* Stricter rate limits for this feature have been applied based on:
  * Number of forwards sent
  * Total attachment size

[**Channel Follow Add Messages**](mensajes.md#channel-follow-add-messages)

* These are automatic messages sent when a channel is followed into the current channel (type ).
* These messages have the and fields, with data of the followed announcement channel.

[**Pin Messages**](mensajes.md#pin-messages)

* These are automatic messages sent when a message is pinned (type ).
* These messages have and , and if it is in a guild, with data of the message that was pinned.

[**Reply Messages**](mensajes.md#reply-messages)

* These are messages replying to a previous message (type ).
* These messages have and , and if it is in a guild, with data of the message that was replied to. The and will be the same as the reply.
* Replies are created by including a when sending a message. When sending, only is required.

[**Thread Created Messages**](mensajes.md#thread-created-messages)

* These are automatic messages sent when a public thread is created from an old message or without a message (type ).
* These messages have the and fields, with data of the created thread channel.

[**Thread Starter Messages**](mensajes.md#thread-starter-messages)

* These are the first message in public threads created from messages. They point back to the message in the parent channel from which the thread was started (type ).
* These messages have , , and .
* These messages will never have , , or , mainly just the and fields.

[**Voice Messages**](mensajes.md#voice-messages)

Voice messages are messages with the flag. They have the following properties:

* They cannot be edited.
* Only a single audio attachment is allowed. No other attachments, content, embeds, stickers, etc.
* The [attachment](mensajes.md#attachment-object) has the additional fields and . The of the attachment must begin with to respect these fields.

The is intended to be a preview of the entire voice message, with 1 byte per datapoint encoded in base64. Official clients sample the recording at most once per 100 milliseconds, but will downsample so that no more than 256 datapoints are in the waveform.

When uploading a voice message attachment directly, the provided content type must match an audio content type to support the additional fields.

Official clients upload a 1 channel, 48000 Hz, 32 kbps Opus stream in an OGG container. The encoding and waveform details are an implementation detail and may change without warning.

[**Clips**](mensajes.md#clips)

Clips are a special type of attachment that can be sent with a message. They are created by recording a stream and then clipping a portion of the recording.

Clip [attachments](mensajes.md#attachment-object) have the [flag](mensajes.md#attachment-flags) set, and have the additional fields , , and optionally and .

An attachment can be sent as a clip by specifying the , , and fields, and optionally the and fields. For an attachment to be a valid clip, it must be an MP4 file and have the text and the hardcoded UUID of appended to the end of the file in binary format. (represented in hexadecimal bytes as )

This can be done with the following pseudocode:

```
import uuidclip_uuid = uuid.UUID("a1c85299-3346-4db8-88f0-83f57a75a5ef")with open("cat.mp4", "r+b") as file:    file.seek(0, 2)    file.write(b"uuid")    file.write(clip_uuid.bytes)
```

#### [Reaction Object](mensajes.md#reaction-object) <a href="#reaction-object" id="reaction-object"></a>

[**Reaction Structure**](mensajes.md#reaction-structure)

| Field          | Type                                                                           | Description                                                         |
| -------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------- |
| count          | integer                                                                        | Total amount of times this emoji has been used to react             |
| count\_details | [reaction count details](mensajes.md#reaction-count-details-structure) object  | Details about the number of times this emoji has been used to react |
| me             | boolean                                                                        | Whether the current user reacted using this emoji                   |
| me\_burst      | boolean                                                                        | Whether the current user burst-reacted using this emoji             |
| emoji          | partial [emoji](https://docs.discord.food/resources/emoji#emoji-object) object | Reaction emoji information                                          |
| burst\_colors  | array\[string]                                                                 | The hex-encoded colors to render the burst reaction with            |

[**Reaction Count Details Structure**](mensajes.md#reaction-count-details-structure)

| Field  | Type    | Description                                                |
| ------ | ------- | ---------------------------------------------------------- |
| normal | integer | Amount of times this emoji has been used to react normally |
| burst  | integer | Amount of times this emoji has been used to burst-react    |

[**Reaction Type**](mensajes.md#reaction-type)

| Value | Name   | Description              |
| ----- | ------ | ------------------------ |
| 0     | NORMAL | A normal reaction        |
| 1     | BURST  | A burst (super) reaction |

#### [Embed Object](mensajes.md#embed-object) <a href="#embed-object" id="embed-object"></a>

[**Embed Structure**](mensajes.md#embed-structure)

| Field                                             | Type                                                            | Description                                                                             |
| ------------------------------------------------- | --------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| title?                                            | string                                                          | The title of the embed (max 256 characters)                                             |
| type? <sup>1</sup>                                | string                                                          | The [type of embed](mensajes.md#embed-type) (always `rich` for sent embeds)             |
| description?                                      | string                                                          | The description of the embed (max 4096 characters)                                      |
| url?                                              | string                                                          | The URL of the embed (max 2048 characters)                                              |
| timestamp?                                        | ISO8601 timestamp                                               | Timestamp of embed content                                                              |
| color?                                            | integer                                                         | The color of the embed encoded as an integer representation of a hexadecimal color code |
| footer?                                           | [embed footer](mensajes.md#embed-footer-structure) object       | Embed footer information                                                                |
| image?                                            | [embed media](mensajes.md#embed-media-structure) object         | Embed image information                                                                 |
| thumbnail?                                        | [embed media](mensajes.md#embed-media-structure) object         | Embed thumbnail information                                                             |
| video? <sup>1</sup>                               | [embed media](mensajes.md#embed-media-structure) object         | Embed video information                                                                 |
| provider? <sup>1</sup>                            | [embed provider](mensajes.md#embed-provider-structure) object   | Embed provider information                                                              |
| author?                                           | [embed author](mensajes.md#embed-author-structure) object       | Embed author information                                                                |
| fields?                                           | array\[[embed field](mensajes.md#embed-field-structure) object] | The fields of the embed (max 25)                                                        |
| reference\_id? <sup>1</sup>                       | snowflake                                                       | The ID of the message this embed was generated from                                     |
| content\_scan\_version? <sup>1</sup> <sup>2</sup> | integer                                                         | The version of the explicit content scan filter this embed was scanned with             |
| flags? <sup>1</sup>                               | integer                                                         | The [embed's flags](mensajes.md#embed-flags)                                            |

<sup>1</sup> These fields cannot be specified in a rich embed.

<sup>2</sup> This field will be missing if the embed has not yet been scanned. In this case, a scan can be triggered with the [Scan Explicit Media](mensajes.md#scan-explicit-media) endpoint. If the field is present and set to , the embed is not eligible for a scan (e.g. it is a textual embed without media).

[**Embed Type**](mensajes.md#embed-type)

Most embed types are "loosely defined" and, for the most part, are not used by our clients for rendering. Embed attributes power what is rendered.

| Type                                                 | Description                                                                                                        |
| ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| application\_news **(deprecated)**                   | Application news embed                                                                                             |
| article                                              | Article embed                                                                                                      |
| auto\_moderation\_message <sup>1</sup>               | [AutoMod alert](https://docs.discord.food/resources/auto-moderation#automod-alert)                                 |
| auto\_moderation\_notification <sup>1</sup>          | [AutoMod incident notification](https://docs.discord.food/resources/auto-moderation#automod-incident-notification) |
| gift                                                 | Gift embed                                                                                                         |
| gifv                                                 | Animated GIF image rendered as a video embed                                                                       |
| image                                                | Image embed                                                                                                        |
| link                                                 | Link embed                                                                                                         |
| poll\_result <sup>2</sup>                            | Poll result embed                                                                                                  |
| post\_preview <sup>3</sup>                           | Media channel post preview embed                                                                                   |
| rich                                                 | Generic embed rendered from embed attributes                                                                       |
| video                                                | Video embed                                                                                                        |
| age\_verification\_system\_notification <sup>1</sup> | Age verification system message embed                                                                              |

<sup>1</sup> These embed types are system-generated and cannot be sent by users. Their [array](mensajes.md#embed-object) represents the object linked in the description as a list of key-value pairs.

<sup>2</sup> See the [poll result notifications](mensajes.md#poll-result-notifications) section for more information.

<sup>3</sup> This embed type is used to signal to the client to render a [preview of the linked media channel post](mensajes.md#get-channel-media-preview). This embed is created for URLs that link to a media channel post if the channel is a paywalled role subscription benefit. The URL is always in the format .

[**Embed Flags**](mensajes.md#embed-flags)

| Value  | Name                      | Description                                                                                            |
| ------ | ------------------------- | ------------------------------------------------------------------------------------------------------ |
| 1 << 4 | CONTAINS\_EXPLICIT\_MEDIA | Embed was flagged as [sensitive content](https://support.discord.com/hc/en-us/articles/18210995019671) |
| 1 << 5 | CONTENT\_INVENTORY\_ENTRY | Embed is a legacy content inventory reply                                                              |

[**Embed Media Structure**](mensajes.md#embed-media-structure)

| Field                                 | Type                                                                        | Description                                                                                                   |
| ------------------------------------- | --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| url                                   | string                                                                      | Source URL of media (only supports http(s) and attachments) (max 2048 characters)                             |
| proxy\_url? <sup>1</sup>              | string                                                                      | A proxied URL of the media                                                                                    |
| height? <sup>1</sup>                  | integer                                                                     | Height of media                                                                                               |
| width? <sup>1</sup>                   | integer                                                                     | Width of media                                                                                                |
| flags? <sup>1</sup>                   | integer                                                                     | The [media's attachment flags](mensajes.md#attachment-flags)                                                  |
| description? <sup>1</sup>             | string                                                                      | Alt text for the media                                                                                        |
| content\_type? <sup>1</sup>           | string                                                                      | The attachment's [media type](https://en.wikipedia.org/wiki/Media_type)                                       |
| content\_scan\_metadata? <sup>1</sup> | [content scan metadata](mensajes.md#content-scan-metadata-structure) object | The content scan metadata for the media                                                                       |
| placeholder\_version? <sup>1</sup>    | integer                                                                     | The attachment placeholder protocol version (currently 1)                                                     |
| placeholder? <sup>1</sup>             | string                                                                      | A low-resolution [thumbhash](https://github.com/evanw/thumbhash) of the media, to display before it is loaded |

<sup>1</sup> This field is received only and cannot be set.

[**Embed Provider Structure**](mensajes.md#embed-provider-structure)

| Field | Type   | Description                                   |
| ----- | ------ | --------------------------------------------- |
| name? | string | The name of the provider (max 256 characters) |
| url?  | string | URL of the provider (max 2048 characters)     |

[**Embed Author Structure**](mensajes.md#embed-author-structure)

| Field                          | Type   | Description                                                                                   |
| ------------------------------ | ------ | --------------------------------------------------------------------------------------------- |
| name                           | string | The name of the author (max 256 characters)                                                   |
| url?                           | string | URL of the author (only supports http(s)) (max 2048 characters)                               |
| icon\_url?                     | string | Source URL of the author's icon (only supports http(s) and attachments) (max 2048 characters) |
| proxy\_icon\_url? <sup>1</sup> | string | A proxied URL of the author's icon                                                            |

<sup>1</sup> This field is received only and cannot be set.

| Field                          | Type   | Description                                                                                 |
| ------------------------------ | ------ | ------------------------------------------------------------------------------------------- |
| text                           | string | The footer text (max 2048 characters)                                                       |
| icon\_url?                     | string | Source URL of the footer icon (only supports http(s) and attachments) (max 2048 characters) |
| proxy\_icon\_url? <sup>1</sup> | string | A proxied URL of the footer icon                                                            |

<sup>1</sup> This field is received only and cannot be set.

[**Embed Field Structure**](mensajes.md#embed-field-structure)

| Field   | Type    | Description                                              |
| ------- | ------- | -------------------------------------------------------- |
| name    | string  | The name of the field (max 256 characters)               |
| value   | string  | The value of the field (max 1024 characters)             |
| inline? | boolean | Whether this field should display inline (default false) |

[**Content Scan Metadata Structure**](mensajes.md#content-scan-metadata-structure)

| Field   | Type    | Description                                                                 |
| ------- | ------- | --------------------------------------------------------------------------- |
| flags   | integer | The [content scan flags](mensajes.md#content-scan-flags) of the media       |
| version | integer | The version of the explicit content scan filter this media was scanned with |

[**Content Scan Flags**](mensajes.md#content-scan-flags)

| Value  | Name     | Description                               |
| ------ | -------- | ----------------------------------------- |
| 1 << 0 | EXPLICIT | The media was flagged as explicit content |
| 1 << 1 | GORE     | The media was flagged as gore             |

#### [Attachment Object](mensajes.md#attachment-object) <a href="#attachment-object" id="attachment-object"></a>

[**Attachment Structure**](mensajes.md#attachment-structure)

| Field                                             | Type                                                                                              | Description                                                                                                                                                                                   |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id                                                | snowflake                                                                                         | The attachment ID                                                                                                                                                                             |
| filename                                          | string                                                                                            | The name of file attached (max 1024 characters)                                                                                                                                               |
| title?                                            | string                                                                                            | The name of the file without the extension or title of the clip (max 1024 characters, automatically provided when the filename is normalized or randomly generated due to invalid characters) |
| uploaded\_filename? <sup>3</sup>                  | string                                                                                            | The name of the file pre-uploaded to Discord's GCP bucket                                                                                                                                     |
| description?                                      | string                                                                                            | Alt text for the file (max 1024 characters)                                                                                                                                                   |
| content\_type? <sup>2</sup>                       | string                                                                                            | The attachment's [media type](https://en.wikipedia.org/wiki/Media_type)                                                                                                                       |
| size                                              | integer                                                                                           | The size of file in bytes                                                                                                                                                                     |
| url <sup>2</sup>                                  | string                                                                                            | Source URL of the file                                                                                                                                                                        |
| proxy\_url <sup>2</sup> <sup>4</sup>              | string                                                                                            | A proxied url of the file                                                                                                                                                                     |
| height? <sup>2</sup>                              | ?integer                                                                                          | Height of image                                                                                                                                                                               |
| width? <sup>2</sup>                               | ?integer                                                                                          | Width of image                                                                                                                                                                                |
| content\_scan\_version? <sup>2</sup> <sup>5</sup> | integer                                                                                           | The version of the explicit content scan filter this attachment was scanned with                                                                                                              |
| placeholder\_version? <sup>2</sup>                | integer                                                                                           | The attachment placeholder protocol version (currently 1)                                                                                                                                     |
| placeholder? <sup>2</sup>                         | string                                                                                            | A low-resolution [thumbhash](https://github.com/evanw/thumbhash) of the attachment, to display before it is loaded                                                                            |
| ephemeral? <sup>1</sup> <sup>2</sup>              | boolean                                                                                           | Whether this attachment is ephemeral                                                                                                                                                          |
| duration\_secs?                                   | float                                                                                             | Duration of the audio file (if [voice message](mensajes.md#voice-messages))                                                                                                                   |
| waveform?                                         | string                                                                                            | Base64 encoded bytearray representing a sampled waveform (if [voice message](mensajes.md#voice-messages))                                                                                     |
| flags? <sup>6</sup>                               | integer                                                                                           | The [attachment's flags](mensajes.md#attachment-flags)                                                                                                                                        |
| is\_clip? <sup>6</sup> <sup>7</sup> <sup>8</sup>  | boolean                                                                                           | Whether the file being uploaded is a [clipped recording of a stream](https://support.discord.com/hc/en-us/articles/16861982215703-Clips)                                                      |
| is\_thumbnail? <sup>6</sup>                       | boolean                                                                                           | Whether the file being uploaded is a thumbnail                                                                                                                                                |
| is\_remix? <sup>6</sup>                           | boolean                                                                                           | Whether this attachment is a [remixed](https://support.discord.com/hc/en-us/articles/15145601963031-Remix-FAQ) version of another attachment                                                  |
| is\_spoiler? <sup>6</sup>                         | boolean                                                                                           | Whether this attachment is a spoiler                                                                                                                                                          |
| clip\_created\_at? <sup>8</sup>                   | ISO8601 timestamp                                                                                 | When the clip was created                                                                                                                                                                     |
| clip\_participant\_ids? <sup>7</sup> <sup>8</sup> | array\[snowflake]                                                                                 | The IDs of the participants in the clip (max 100)                                                                                                                                             |
| clip\_participants? <sup>8</sup>                  | array\[partial [user](https://docs.discord.food/resources/user#user-object) object]               | The participants in the clip (max 100)                                                                                                                                                        |
| application\_id? <sup>8</sup>                     | snowflake                                                                                         | The ID of the application the clip was taken in                                                                                                                                               |
| application? <sup>8</sup>                         | ?partial [application](https://docs.discord.food/resources/application#application-object) object | The application the clip was taken in                                                                                                                                                         |

<sup>1</sup> Ephemeral attachments will automatically be removed after a set period of time. Ephemeral attachments on messages are guaranteed to be available as long as the message itself exists.

<sup>2</sup> These fields are received only and cannot be set.

<sup>3</sup> This field is send-only. See [uploading to Google Cloud](https://docs.discord.food/reference#uploading-to-google-cloud) for more information.

<sup>4</sup> The proxy URL only supports attachments with a defined and , such as images and videos. For all other attachments, the proxy returns a 415 unsupported media type error.

<sup>5</sup> This field will be missing if the attachment has not yet been scanned. In this case, a scan can be triggered with the [Explicit Content Scan](mensajes.md#scan-explicit-media) endpoint. If the field is present and set to , the attachment is not eligible for a scan.

<sup>6</sup> The field is received only and cannot be set directly. To set flags, use the , , , and send-only fields.

<sup>7</sup> When sending a clip, , , and are required. [See message types](mensajes.md#clips) for more information.

<sup>8</sup> The and fields are send-only. You will receive the and fields back when retrieving the [message](mensajes.md#message-object).

[**Attachment Flags**](mensajes.md#attachment-flags)

| Value  | Name                      | Description                                                                                                         |
| ------ | ------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| 1 << 0 | IS\_CLIP                  | Attachment is a [clipped recording of a stream](https://support.discord.com/hc/en-us/articles/16861982215703-Clips) |
| 1 << 1 | IS\_THUMBNAIL             | Attachment is a thumbnail                                                                                           |
| 1 << 2 | IS\_REMIX                 | Attachment has been [remixed](https://support.discord.com/hc/en-us/articles/15145601963031-Remix-FAQ)               |
| 1 << 3 | IS\_SPOILER               | Attachment is a spoiler                                                                                             |
| 1 << 4 | CONTAINS\_EXPLICIT\_MEDIA | Attachment was flagged as [sensitive content](https://support.discord.com/hc/en-us/articles/18210995019671)         |
| 1 << 5 | IS\_ANIMATED              | Attachment is an animated image                                                                                     |
| 1 << 6 | CONTAINS\_GORE\_CONTENT   | Attachment was flagged as [gore](https://support.discord.com/hc/en-us/articles/18210995019671)                      |

#### [Allowed Mentions Object](mensajes.md#allowed-mentions-object) <a href="#allowed-mentions-object" id="allowed-mentions-object"></a>

The allowed mention field allows for more granular control over mentions without various hacks to the message content. This will always validate against message content to avoid phantom pings (e.g. to ping everyone, you must still have "@everyone" in the message content) and check against user permissions.

[**Allowed Mention Types**](mensajes.md#allowed-mention-types)

| Value    | Description                           |
| -------- | ------------------------------------- |
| roles    | Controls role mentions                |
| users    | Controls user mentions                |
| everyone | Controls @everyone and @here mentions |

[**Allowed Mentions Structure**](mensajes.md#allowed-mentions-structure)

| Field          | Type              | Description                                                                                |
| -------------- | ----------------- | ------------------------------------------------------------------------------------------ |
| parse?         | array\[string]    | The [allowed mention types](mensajes.md#allowed-mention-types) to parse from the content   |
| roles?         | array\[snowflake] | The role IDs to mention (max 100)                                                          |
| users?         | array\[snowflake] | The user IDs to mention (max 100)                                                          |
| replied\_user? | boolean           | For replies, whether to mention the author of the message being replied to (default false) |

[**Allowed Mentions Reference**](mensajes.md#allowed-mentions-reference)

Due to the complexity of possibilities, we have included a set of examples and behavior for the allowed mentions field.

If is _not_ passed in (i.e. the key does not exist), the mentions will be parsed via the content. This corresponds with existing behavior.

In the example below we would ping @here (and also @role124 and @user123)

```
{  "content": "@here Hi there from <@123>, cc <@&124>"}
```

To suppress all mentions in a message use:

```
{  "content": "@everyone hi there, <@&123>",  "allowed_mentions": {    "parse": []  }}
```

This will suppress _all_ mentions in the message (no @everyone or user mention).

The field is mutually exclusive with the other fields. In the example below, we would ping users and role , but _not_ @everyone. Note that passing a falsy value (\[], ) into the field does not trigger a validation error.

```
{  "content": "@everyone <@123> <@&124>",  "allowed_mentions": {    "parse": ["users", "roles"],    "users": []  }}
```

In the next example, we would ping @everyone, (and also users and if they suppressed @everyone mentions), but we would not ping any roles.

```
{  "content": "@everyone <@123> <@124> <@125> <@&200>",  "allowed_mentions": {    "parse": ["everyone"],    "users": ["123", "124"]  }}
```

Due to possible ambiguities, not all configurations are valid. An _invalid_ configuration is as follows

```
{  "content": "@everyone <@123> <@124> <@125> <@&200>",  "allowed_mentions": {    "parse": ["users"],    "users": ["123", "124"]  }}
```

Because and are both present, Discord would throw a validation error. This is because the conditions cannot be fulfilled simultaneously (they are mutually exclusive).

Any entities with an ID included in the list of IDs can be mentioned. Note that the IDs of entities not present in the message's content will simply be ignored. e.g. The following example is valid, and would mention user 123, but _not_ user 125 since there is no mention of user 125 in the content.

```
{  "content": "<@123> Time for some memes.",  "allowed_mentions": {    "users": ["123", "125"]  }}
```

#### [Poll Object](mensajes.md#poll-object) <a href="#poll-object" id="poll-object"></a>

The poll object has a lot of levels and nested structures. It was also designed to support future extensibility, so some fields may appear to be more complex than necessary.

[**Poll Structure**](mensajes.md#poll-structure)

| Field                 | Type                                                            | Description                                                 |
| --------------------- | --------------------------------------------------------------- | ----------------------------------------------------------- |
| question <sup>1</sup> | [poll media](mensajes.md#poll-media-structure) object           | The question of the poll                                    |
| answers               | array\[[poll answer](mensajes.md#poll-answer-structure) object] | The answers available in the poll                           |
| expiry <sup>2</sup>   | ?IS08601 timestamp                                              | When the poll ends                                          |
| allow\_multiselect    | boolean                                                         | Whether a user can select multiple answers                  |
| layout\_type          | integer                                                         | The [layout type](mensajes.md#poll-layout-type) of the poll |
| results?              | [poll results](mensajes.md#poll-results-structure) object       | The results of the poll                                     |

<sup>1</sup> Only is supported.

<sup>2</sup> is marked as nullable to support non-expiring polls in the future, but all polls have an expiry currently.

[**Poll Create Structure**](mensajes.md#poll-create-structure)

This is the request object used when creating a poll across the different endpoints. It is similar but not exactly identical to the main [poll](mensajes.md#poll-structure) object. The main difference is that the request has which eventually becomes .

| Field                 | Type                                                            | Description                                                                     |
| --------------------- | --------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| question <sup>1</sup> | [poll media](mensajes.md#poll-media-structure) object           | The question of the poll                                                        |
| answers               | array\[[poll answer](mensajes.md#poll-answer-structure) object] | Each of the answers available in the poll (max 10)                              |
| duration              | integer                                                         | Number of hours the poll should be open for (max 32 days, default 1)            |
| allow\_multiselect?   | boolean                                                         | Whether a user can select multiple answers (default false)                      |
| layout\_type?         | integer                                                         | The [layout type](mensajes.md#poll-layout-type) of the poll (default `DEFAULT`) |

<sup>1</sup> Only is supported.

[**Poll Layout Type**](mensajes.md#poll-layout-type)

Different layouts for polls will come in the future. For now though, this value will always be .

| Value | Name                     | Description                           |
| ----- | ------------------------ | ------------------------------------- |
| 1     | DEFAULT                  | The default layout type               |
| ~~2~~ | ~~IMAGE\_ONLY\_ANSWERS~~ | ~~Poll answers can have only images~~ |

[**Poll Media Structure**](mensajes.md#poll-media-structure)

The poll media object is a common object that backs both the question and answers. For now, only supports , while answers can have an optional .

| Field               | Type                                                                           | Description                                                                       |
| ------------------- | ------------------------------------------------------------------------------ | --------------------------------------------------------------------------------- |
| text? <sup>1</sup>  | string                                                                         | The text of the field (max 300 characters for question, 55 characters for answer) |
| emoji? <sup>2</sup> | partial [emoji](https://docs.discord.food/resources/emoji#emoji-object) object | The emoji of the field                                                            |

<sup>1</sup> should always be non- for both questions and answers, but do not depend on that in the future.

<sup>2</sup> When creating a poll answer with an emoji, clients only needs to send either the (custom emoji) or (default emoji) as the only field.

[**Poll Answer Structure**](mensajes.md#poll-answer-structure)

The is a number that labels each answer. As an implementation detail, it currently starts at 1 for the first answer and goes up sequentially. We recommend against depending on this sequence.

Currently, there is a maximum of 10 answers per poll.

| Field                   | Type                                                  | Description            |
| ----------------------- | ----------------------------------------------------- | ---------------------- |
| answer\_id <sup>1</sup> | integer                                               | The ID of the answer   |
| poll\_media             | [poll media](mensajes.md#poll-media-structure) object | The data of the answer |

<sup>1</sup> When sending, this field is optional.

[**Poll Results Structure**](mensajes.md#poll-results-structure)

In a nutshell, this contains the number of votes for each answer. The field may be not present in certain responses where, as an implementation detail, Discord does not fetch the poll results in the backend. This should be treated as "unknown results", as opposed to "no results". You can keep using the results if you have previously received them through other means. Due to the intricacies of counting at scale, while a poll is in progress the results may not be perfectly accurate. They usually are accurate, and shouldn't deviate significantly—it's just difficult to make guarantees. To compensate for this, after a poll is finished there is a background job which performs a final, accurate tally of votes. This tally concludes once is . Polls that have ended will also always contain results. If does not contain an entry for a particular answer, then there are no votes for that answer.

| Field          | Type                                                                        | Description                                   |
| -------------- | --------------------------------------------------------------------------- | --------------------------------------------- |
| is\_finalized  | boolean                                                                     | Whether the votes have been precisely counted |
| answer\_counts | array\[[poll answer count](mensajes.md#poll-answer-count-structure) object] | The counts for each answer                    |

[**Poll Answer Count Structure**](mensajes.md#poll-answer-count-structure)

| Field     | Type    | Description                                    |
| --------- | ------- | ---------------------------------------------- |
| id        | integer | The ID of the answer                           |
| count     | integer | The number of votes for this answer            |
| me\_voted | boolean | Whether the current user voted for this answer |

[**Example Poll**](mensajes.md#example-poll)

```
{  "question": {    "text": "Aliens?"  },  "answers": [    {      "answer_id": 1,      "poll_media": {        "text": "Alien"      }    },    {      "answer_id": 2,      "poll_media": {        "text": "Alien 2",        "emoji": {          "id": null,          "name": "👽"        }      }    },    {      "answer_id": 3,      "poll_media": {        "text": "Alien 3",        "emoji": {          "id": "1120790948302033046",          "name": "meowlien"        }      }    }  ],  "expiry": "2024-05-02T10:00:02.039342+00:00",  "allow_multiselect": true,  "layout_type": 1,  "results": {    "answer_counts": [      {        "id": 1,        "count": 1,        "me_voted": false      }    ],    "is_finalized": false  }}
```

#### [Poll Result Notifications](mensajes.md#poll-result-notifications) <a href="#poll-result-notifications" id="poll-result-notifications"></a>

Poll result notifications are sent as standard [messages](mensajes.md#message-object) in the channel with the [message type](mensajes.md#message-type). The author of the message is the user who created the poll, and the message has a reference pointing to the original poll message.

These messages have a special [embed](mensajes.md#embed-object) structure which contains information about the poll results. The custom embed fields below are collapsed as a list of key-value pairs into the array on the [embed object](mensajes.md#embed-object).

[**Poll Result Embed Structure**](mensajes.md#poll-result-embed-structure)

| Field                                     | Type      | Description                                         |
| ----------------------------------------- | --------- | --------------------------------------------------- |
| poll\_question\_text                      | string    | The text of the poll question                       |
| total\_votes                              | integer   | The total number of votes on the poll               |
| victor\_answer\_id? <sup>1</sup>          | integer   | The ID of the winning answer                        |
| victor\_answer\_text? <sup>1</sup>        | string    | The text of the winning answer                      |
| victor\_answer\_emoji\_id? <sup>1</sup>   | snowflake | The ID of the emoji of the winning answer           |
| victor\_answer\_emoji\_name? <sup>1</sup> | string    | The name of the emoji of the winning answer         |
| victor\_answer\_emoji\_animated?          | boolean   | Whether the emoji of the winning answer is animated |
| victor\_answer\_votes                     | integer   | The number of votes on the winning answer           |

<sup>1</sup> If these fields are omitted, the poll did not have a decisive winner.

[**Example Poll Result Embed**](mensajes.md#example-poll-result-embed)

```
{  "type": "poll_result",  "fields": [    {      "name": "poll_question_text",      "value": "aliens?",      "inline": false    },    {      "name": "victor_answer_votes",      "value": "100",      "inline": false    },    {      "name": "total_votes",      "value": "101",      "inline": false    },    {      "name": "victor_answer_id",      "value": "1",      "inline": false    },    {      "name": "victor_answer_text",      "value": "ofc",      "inline": false    },    {      "name": "victor_answer_emoji_id",      "value": "1243729917288386560",      "inline": false    },    {      "name": "victor_answer_emoji_name",      "value": "cheeks",      "inline": false    },    {      "name": "victor_answer_emoji_animated",      "value": "true",      "inline": false    }  ]}
```

#### [Age Verification System Message](mensajes.md#age-verification-system-message) <a href="#age-verification-system-message" id="age-verification-system-message"></a>

Age verification system messages are sent by the official Discord account to notify the user of their results. The message content will contain a localized, user-friendly message explaining their classification with a relevant help center link.

[**Age Verification System Message Embed Structure**](mensajes.md#age-verification-system-message-embed-structure)

| Field         | Type           | Description                                                                                                        |
| ------------- | -------------- | ------------------------------------------------------------------------------------------------------------------ |
| ctas?         | array\[string] | Comma seprated array of the CTAs to display (currently only `retry`)                                               |
| content\_type | string         | The [age verification system message content type](mensajes.md#age-verification-system-message-embed-content-type) |

[**Age Verification System Message Embed Content Type**](mensajes.md#age-verification-system-message-embed-content-type)

| Name            | Description                                          |
| --------------- | ---------------------------------------------------- |
| verified\_adult | User was verified as a adult                         |
| verified\_teen  | User was verified as a teen                          |
| error           | An error occured during the age verification process |

[**Example Age Verification System Message Embed**](mensajes.md#example-age-verification-system-message-embed)

```
{  "type": "age_verification_system_notification",  "fields": [    {      "name": "content_type",      "value": "verified_teen",      "inline": false    },    {      "name": "ctas",      "value": "retry",      "inline": false    }  ]}
```

#### [Potion Object](mensajes.md#potion-object) <a href="#potion-object" id="potion-object"></a>

[**Potion Structure**](mensajes.md#potion-structure)

| Field       | Type                                                                                   | Description                                       |
| ----------- | -------------------------------------------------------------------------------------- | ------------------------------------------------- |
| used\_by    | snowflake                                                                              | The ID of the user who applied the potion         |
| type        | integer                                                                                | The [type of the potion](mensajes.md#potion-type) |
| emoji       | array\[partial [emoji](https://docs.discord.food/resources/emoji#emoji-object) object] | The emoji associated with the potion              |
| created\_at | ISO8601 timestamp                                                                      | When the potion was applied                       |

[**Potion Type**](mensajes.md#potion-type)

| Value | Name     | Description       |
| ----- | -------- | ----------------- |
| 0     | CONFETTI | A Confetti potion |

[**Confetti Potion Structure**](mensajes.md#confetti-potion-structure)

| Field          | Type                                                                           | Description                          |
| -------------- | ------------------------------------------------------------------------------ | ------------------------------------ |
| message\_emoji | partial [emoji](https://docs.discord.food/resources/emoji#emoji-object) object | The emoji associated with the potion |

[**Example Potion**](mensajes.md#example-potion)

```
{  "type": 0,  "created_at": "2025-02-06T12:34:56.855553+00:00",  "used_by": "177424155371634688",  "emoji": [    {      "name": "👽",      "id": null    }  ]}
```

#### [Conversation Summary Object](mensajes.md#conversation-summary-object) <a href="#conversation-summary-object" id="conversation-summary-object"></a>

Conversation summaries are short, LLM-generated descriptions of a channel's activity.

[**Conversation Summary Structure**](mensajes.md#conversation-summary-structure)

| Field        | Type              | Description                                             |
| ------------ | ----------------- | ------------------------------------------------------- |
| id           | snowflake         | The ID of the summary                                   |
| topic        | string            | A short description of the topic of the conversation    |
| summ\_short  | string            | A brief summary of the conversation                     |
| message\_ids | array\[snowflake] | The IDs of the messages included in the summary         |
| people       | array\[snowflake] | The IDs of the users included in the summary            |
| unsafe       | boolean           | Whether the summary contains potentially unsafe content |
| start\_id    | snowflake         | The ID of the first message in the conversation         |
| end\_id      | snowflake         | The ID of the last message in the conversation          |
| count        | integer           | The number of messages included in the summary          |
| source       | integer           | The [source of the summary](mensajes.md#summary-source) |
| type         | integer           | The [type of summary](mensajes.md#summary-type)         |

[**Summary Source**](mensajes.md#summary-source)

| Value | Name      | Description                           |
| ----- | --------- | ------------------------------------- |
| 0     | SOURCE\_0 | The summary was generated by source 0 |
| 1     | SOURCE\_1 | The summary was generated by source 1 |
| 2     | SOURCE\_2 | The summary was generated by source 2 |

[**Summary Type**](mensajes.md#summary-type)

| Value | Name      | Description                           |
| ----- | --------- | ------------------------------------- |
| 0     | UNSET     | The summary type is unset             |
| 1     | SOURCE\_1 | The summary was generated by source 1 |
| 2     | SOURCE\_2 | The summary was generated by source 2 |
| 3     | UNKNOWN   | Unknown                               |

[**Example Conversation Summary**](mensajes.md#example-conversation-summary)

```
{  "topic": "Rare Footage of Alien Cat",  "summ_short": "Conversation about rare footage of an alien cat species.",  "message_ids": ["1314941815144845413", "1314944583397937213"],  "people": ["852892297661906993", "841509053422632990"],  "id": "1315651706670813286",  "unsafe": false,  "start_id": "1314941815144845413",  "end_id": "1315650462522802196",  "count": 2,  "source": 2,  "type": 3}
```

#### [Message Pin Object](mensajes.md#message-pin-object) <a href="#message-pin-object" id="message-pin-object"></a>

[**Message Pin Structure**](mensajes.md#message-pin-structure)

| Field      | Type                                         | Description                                 |
| ---------- | -------------------------------------------- | ------------------------------------------- |
| pinned\_at | ISO8601 timestamp                            | When the message was pinned                 |
| message    | [message](mensajes.md#message-object) object | The pinned message, without `reactions` key |

### [Endpoints](mensajes.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get Messages](mensajes.md#get-messages) <a href="#get-messages" id="get-messages"></a>

`GET/channels/{channel.id}/messages`

Returns a list of [message](mensajes.md#message-object) objects in the channel. Requires the permission if operating on a guild channel. If the current user is missing the permission in the channel then this will return no messages (since they cannot read the message history).

[**Query String Params**](mensajes.md#query-string-params)

| Field   | Type      | Description                                          |
| ------- | --------- | ---------------------------------------------------- |
| around? | snowflake | Get messages around this message ID                  |
| before? | snowflake | Get messages before this message ID                  |
| after?  | snowflake | Get messages after this message ID                   |
| limit   | integer   | Max number of messages to return (1-100, default 50) |

#### [Preload Messages](mensajes.md#preload-messages) <a href="#preload-messages" id="preload-messages"></a>

`POST/channels/preload-messages`

Preloads the last message sent in a series of private channels. Returns a list of [message](mensajes.md#message-object) objects without the key.

[**JSON Body**](mensajes.md#json-body)

| Field        | Type              | Description                                                |
| ------------ | ----------------- | ---------------------------------------------------------- |
| channel\_ids | array\[snowflake] | The IDs of the channels to preload messages from (max 100) |

#### [Search Messages](mensajes.md#search-messages) <a href="#search-messages" id="search-messages"></a>

`GET/guilds/{guild.id}/messages/search OR /channels/{channel.id}/messages/search`

Returns a list of messages without the key that match a search query in the guild or private channel. Requires the permission if operating on a guild channel.

[**Query String Params**](mensajes.md#query-string-params)

| Field                              | Type              | Description                                                                                |
| ---------------------------------- | ----------------- | ------------------------------------------------------------------------------------------ |
| limit?                             | integer           | Max number of messages to return (1-25, default 25)                                        |
| offset?                            | integer           | Number to offset the returned messages by (max 9975)                                       |
| max\_id?                           | snowflake         | Get messages before this message ID                                                        |
| min\_id?                           | snowflake         | Get messages after this message ID                                                         |
| include\_nsfw?                     | boolean           | Whether to include results from NSFW channels (default false)                              |
| content?                           | string            | Filter messages by content                                                                 |
| channel\_id? <sup>1</sup>          | array\[snowflake] | Filter messages by these channels                                                          |
| author\_type?                      | array\[string]    | Filter messages by [author type](mensajes.md#author-type)                                  |
| author\_id?                        | array\[snowflake] | Filter messages by these authors                                                           |
| mentions?                          | array\[snowflake] | Filter messages that mention these users                                                   |
| mention\_everyone?                 | boolean           | Filter messages that do or do not mention @everyone                                        |
| pinned?                            | boolean           | Filter messages by whether they are or are not pinned                                      |
| has?                               | array\[string]    | Filter messages by whether or not they [have specific things](mensajes.md#search-has-type) |
| embed\_type?                       | array\[string]    | Filter messages by [embed type](mensajes.md#search-embed-type)                             |
| embed\_provider?                   | array\[string]    | Filter messages by embed provider (case-sensitive, e.g. `Tenor`)                           |
| link\_hostname?                    | array\[string]    | Filter messages by link hostname (case-sensitive, e.g. `discordapp.com`)                   |
| attachment\_filename? <sup>3</sup> | string            | Filter messages by attachment filename                                                     |
| attachment\_extension?             | array\[string]    | Filter messages by attachment extension (e.g. `txt`)                                       |
| command\_id? <sup>3</sup>          | snowflake         | Filter messages by application command ID                                                  |
| sort\_by? <sup>2</sup>             | string            | The [sorting algorithm](mensajes.md#search-sort-type) to use                               |
| sort\_order? <sup>2</sup>          | string            | The direction to sort (`asc` or `desc`, default `desc`)                                    |

<sup>1</sup> Not applicable when operating on a private channel.

<sup>2</sup> Sort order is not respected when sorting by relevance.

<sup>3</sup> Parameter is unstable and should not be relied on.

[**Author Type**](mensajes.md#author-type)

All types can be negated by prefixing them with , which means results will not include messages that match the type.

| Value   | Description                           |
| ------- | ------------------------------------- |
| user    | Return messages sent by user accounts |
| bot     | Return messages sent by bot accounts  |
| webhook | Return messages sent by webhooks      |

[**Search Has Type**](mensajes.md#search-has-type)

All types can be negated by prefixing them with , which means results will not include messages that match the type.

| Value    | Description                                   |
| -------- | --------------------------------------------- |
| image    | Return messages that have an image            |
| sound    | Return messages that have a sound attachment  |
| video    | Return messages that have a video             |
| file     | Return messages that have an attachment       |
| sticker  | Return messages that have a sent sticker      |
| embed    | Return messages that have an embed            |
| link     | Return messages that have a link              |
| poll     | Return messages that have a poll              |
| snapshot | Return messages that have a forwarded message |

[**Search Embed Type**](mensajes.md#search-embed-type)

These do not correspond 1:1 to actual [embed types](mensajes.md#embed-type) and encompass a wider range of actual types.

| Value                | Description                                |
| -------------------- | ------------------------------------------ |
| image                | Return messages that have an image embed   |
| video                | Return messages that have a video embed    |
| article              | Return messages that have an article embed |
| gif **(deprecated)** | Return messages that have a gif embed      |

[**Search Sort Type**](mensajes.md#search-sort-type)

| Value     | Description                                              |
| --------- | -------------------------------------------------------- |
| timestamp | Sort by the message creation time (default)              |
| relevance | Sort by the relevance of the message to the search query |

[**Response Body**](mensajes.md#response-body)

| Field                          | Type                                                                                             | Description                                                                                     |
| ------------------------------ | ------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------- |
| analytics\_id                  | string                                                                                           | The analytics ID for the search query                                                           |
| doing\_deep\_historical\_index | boolean                                                                                          | The status of the guild/channel's deep historical indexing operation, if any                    |
| documents\_indexed?            | integer                                                                                          | The number of documents that have been indexed during the current index operation, if any       |
| total\_results                 | integer                                                                                          | The total number of results that match the query                                                |
| messages                       | array\[array\[[message](mensajes.md#message-object) object]]                                     | A nested array of messages (with optional surrounding context)<sup>1</sup> that match the query |
| channels? <sup>2</sup>         | array\[[channel](https://docs.discord.food/resources/channel#channel-object) object]             | The channels that contain the returned messages                                                 |
| threads?                       | array\[[channel](https://docs.discord.food/resources/channel#channel-object) object]             | The threads that contain the returned messages                                                  |
| members?                       | array\[[thread member](https://docs.discord.food/resources/channel#thread-member-object) object] | A thread member object for each returned thread the current user has joined                     |

<sup>1</sup> Surrounding context messages are no longer returned.

<sup>2</sup> Only applicable when operating on a private channel.

[**Example Response**](mensajes.md#example-response)

```
{  "analytics_id": "67ab1d6e97c631025fbba32d4eb82a16",  "doing_deep_historical_index": false,  "total_results": 1,  "messages": [    [      {        "id": "1076986676557131916",        "type": 0,        "content": "domainreaction fear",        "channel_id": "885895521909227581",        "author": {          "id": "545581357812678656",          "username": "alien",          "global_name": "Alien",          "avatar": "60387de43133809b083fb0f7458d2708",          "avatar_decoration_data": null,          "collectibles": null,          "discriminator": "0",          "public_flags": 4194432,          "primary_guild": null        },        "attachments": [],        "embeds": [],        "mentions": [],        "mention_roles": [],        "pinned": false,        "mention_everyone": false,        "tts": false,        "timestamp": "2023-02-19T22:00:33.136000+00:00",        "edited_timestamp": null,        "flags": 0,        "components": [],        "hit": true      }    ]  ]}
```

#### [Search Messages by Tab](mensajes.md#search-messages-by-tab) <a href="#search-messages-by-tab" id="search-messages-by-tab"></a>

`POST/guilds/{guild.id}/messages/search/tabs OR /channels/{channel.id}/messages/search/tabs OR /users/@me/messages/search/tabs`

Returns a list of messages without the key that match a set of parallelized search queries in the guild, private channel, or globally across all user private channels. Requires the permission if operating on a guild channel.

[**JSON Params**](mensajes.md#json-params)

| Field                      | Type                                                                | Description                                                                                          |
| -------------------------- | ------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| tabs                       | map\[string, [search tab](mensajes.md#search-tab-structure) object] | A map of [predefined tab names](mensajes.md#search-tab-type) to search queries                       |
| channel\_ids? <sup>1</sup> | array\[snowflake]                                                   | Filter messages by these channels (max 500)                                                          |
| include\_nsfw?             | boolean                                                             | Whether to include results from NSFW channels (default false)                                        |
| track\_exact\_total\_hits? | boolean                                                             | Whether to return accurate total result count information at the cost of performance (default false) |

<sup>1</sup> Only applicable when operating on a guild.

[**Search Tab Type**](mensajes.md#search-tab-type)

While the tab names themselves are enforced, what they are used for is not. Clients are free to use any combinations of filters even if they don't follow the spirit of the tab name.

| Value    | Description  |
| -------- | ------------ |
| messages | Messages tab |
| links    | Links tab    |
| media    | Media tab    |
| files    | Files tab    |
| pins     | Pins tab     |

[**Search Tab Structure**](mensajes.md#search-tab-structure)

| Field                              | Type                                                         | Description                                                                                |
| ---------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------------------------------------ |
| limit?                             | integer                                                      | Max number of messages to return (1-25, default 25)                                        |
| cursor? <sup>1</sup>               | ?[search cursor](mensajes.md#search-cursor-structure) object | The cursor to use for pagination                                                           |
| offset? <sup>1</sup>               | integer                                                      | Number to offset the returned messages by (max 9975)                                       |
| max\_id?                           | snowflake                                                    | Get messages before this message ID                                                        |
| min\_id?                           | snowflake                                                    | Get messages after this message ID                                                         |
| content?                           | string                                                       | Filter messages by content                                                                 |
| author\_type?                      | array\[string]                                               | Filter messages by [author type](mensajes.md#author-type)                                  |
| author\_id?                        | array\[snowflake]                                            | Filter messages by these authors                                                           |
| mentions?                          | array\[snowflake]                                            | Filter messages that mention these users                                                   |
| mention\_everyone?                 | boolean                                                      | Filter messages that do or do not mention @everyone                                        |
| pinned?                            | boolean                                                      | Filter messages by whether they are or are not pinned                                      |
| has?                               | array\[string]                                               | Filter messages by whether or not they [have specific things](mensajes.md#search-has-type) |
| embed\_type?                       | array\[string]                                               | Filter messages by [embed type](mensajes.md#search-embed-type)                             |
| embed\_provider?                   | array\[string]                                               | Filter messages by embed provider (case-sensitive, e.g. `Tenor`)                           |
| link\_hostname?                    | array\[string]                                               | Filter messages by link hostname (case-sensitive, e.g. `discordapp.com`)                   |
| attachment\_filename? <sup>3</sup> | string                                                       | Filter messages by attachment filename                                                     |
| attachment\_extension?             | array\[string]                                               | Filter messages by attachment extension (e.g. `txt`)                                       |
| command\_id? <sup>3</sup>          | snowflake                                                    | Filter messages by application command ID                                                  |
| sort\_by? <sup>2</sup>             | string                                                       | The [sorting algorithm](mensajes.md#search-sort-type) to use                               |
| sort\_order? <sup>2</sup>          | string                                                       | The direction to sort (`asc` or `desc`, default `desc`)                                    |

<sup>1</sup> You may only paginate with either or , not both. If is provided, the [search cursor type](mensajes.md#search-cursor-type) should match the [sorting algorithm](mensajes.md#search-sort-type) used in .

<sup>2</sup> Sort order is not respected when sorting by relevance.

<sup>3</sup> Parameter is unstable and should not be relied on.

[**Search Cursor Structure**](mensajes.md#search-cursor-structure)

| Field      | Type                                                                    | Description                                                                                           |
| ---------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| type       | string                                                                  | The [type of the cursor](mensajes.md#search-cursor-type)                                              |
| timestamp? | snowflake                                                               | The ID of the last message in this page of results; used for pagination by timestamp                  |
| score?     | [search cursor score](mensajes.md#search-cursor-score-structure) object | The relevance metadata for the last message in this page of results; used for pagination by relevance |

[**Search Cursor Score Structure**](mensajes.md#search-cursor-score-structure)

| Field     | Type      | Description                                                     |
| --------- | --------- | --------------------------------------------------------------- |
| timestamp | snowflake | The ID of the last message in this page of results              |
| score?    | float     | The relevance score of the last message in this page of results |

[**Search Cursor Type**](mensajes.md#search-cursor-type)

| Value     | Description                   |
| --------- | ----------------------------- |
| timestamp | Paginate results by timestamp |
| score     | Paginate results by relevance |

[**Response Body**](mensajes.md#response-body)

| Field                          | Type                                                                               | Description                                                                               |
| ------------------------------ | ---------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| analytics\_id                  | string                                                                             | The analytics ID for the search query                                                     |
| doing\_deep\_historical\_index | boolean                                                                            | The status of the guild/channel's deep historical indexing operation, if any              |
| documents\_indexed?            | integer                                                                            | The number of documents that have been indexed during the current index operation, if any |
| tabs                           | map\[string, [search tab results](mensajes.md#search-tab-results-structure) object | A map of [requested tab names](mensajes.md#search-tab-type) to search results             |

[**Search Tab Results Structure**](mensajes.md#search-tab-results-structure)

| Field                       | Type                                                                                             | Description                                                                                     |
| --------------------------- | ------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------- |
| messages                    | array\[array\[[message](mensajes.md#message-object) object]]                                     | A nested array of messages (with optional surrounding context)<sup>1</sup> that match the query |
| channels                    | array\[[channel](https://docs.discord.food/resources/channel#channel-object) object]             | The channels that contain the returned messages                                                 |
| threads?                    | array\[[channel](https://docs.discord.food/resources/channel#channel-object) object]             | The threads that contain the returned messages                                                  |
| members?                    | array\[[thread member](https://docs.discord.food/resources/channel#thread-member-object) object] | A thread member object for each returned thread the current user has joined                     |
| cursor <sup>2</sup>         | [search cursor](mensajes.md#search-cursor-structure) object                                      | The cursor for the next page of results                                                         |
| total\_results <sup>3</sup> | integer                                                                                          | The total number of results that match the query                                                |
| time\_spent\_ms             | integer                                                                                          | Duration taken (in milliseconds) processing the search query                                    |

<sup>1</sup> Surrounding context messages are no longer returned.

<sup>2</sup> Will be an empty object if there is no more data to paginate.

<sup>3</sup> Amount will be capped at 1001 if is set to .

#### [Get Message](mensajes.md#get-message) <a href="#get-message" id="get-message"></a>

`GET/channels/{channel.id}/messages/{message.id}`

Returns a specific [message](mensajes.md#message-object) object in the channel. Requires the permission if operating on a guild channel.

#### [Create Message](mensajes.md#create-message) <a href="#create-message" id="create-message"></a>

`POST/channels/{channel.id}/messages`

Posts a message to a text-based channel. Returns a [message](mensajes.md#message-object) object on success. Fires a [Message Create](https://docs.discord.food/topics/gateway-events#message-create) Gateway event. See [message formatting](https://docs.discord.food/reference#message-formatting) for more information on how to properly format messages.

To create a message as a reply to another message, you can include awith a . The and in the are optional, but will be validated if provided.

Files must be attached using a body (or pre-uploaded to Discord's GCP bucket) as described in [Uploading Files](https://docs.discord.food/reference#uploading-files).

[**Limitations**](mensajes.md#limitations)

* When operating on a guild channel, the current user must have the permission.
* When sending a message with , the current user must have the permission.
* When sending a message with (text-to-speech) set to , the current user must have the permission.
* When creating a message as a reply to another message, the current user must have the permission.
  * The referenced message must exist and cannot be a system message.
* The maximum request size when sending a message is **100 MiB**.
* For the embed object, you can set every field except (it will be regardless of if you try to set it), , , and any , , or values for images.

[**JSON/Form Params**](mensajes.md#json/form-params)

| Field                       | Type                                                                                                | Description                                                                                                                                                                                          |
| --------------------------- | --------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| content?                    | string                                                                                              | The message contents (up to 2000 characters)                                                                                                                                                         |
| tts?                        | boolean                                                                                             | Whether this is a TTS message                                                                                                                                                                        |
| embeds? <sup>2</sup>        | array\[[embed](mensajes.md#embed-object) object]                                                    | Embedded `rich` content (max 6000 characters, max 10)                                                                                                                                                |
| nonce? <sup>3</sup>         | integer \| string                                                                                   | The message's nonce, used for message deduplication (will be present in the returned object and accompanying [Message Create](https://docs.discord.food/topics/gateway-events#message-create) event) |
| allowed\_mentions?          | [allowed mention](mensajes.md#allowed-mentions-object) object                                       | Allowed mentions for the message                                                                                                                                                                     |
| message\_reference?         | [message reference](mensajes.md#message-reference-structure) object                                 | The message being replied to or forwarded                                                                                                                                                            |
| components? <sup>2</sup>    | array\[[message component](https://docs.discord.food/resources/components#component-object) object] | The components to include with the message                                                                                                                                                           |
| sticker\_ids?               | array\[snowflake]                                                                                   | IDs of up to 3 [stickers](https://docs.discord.food/resources/sticker#sticker-object) to send in the message                                                                                         |
| activity?                   | [message activity](mensajes.md#message-activity-object) object                                      | The rich presence activity to invite users to                                                                                                                                                        |
| application\_id?            | snowflake                                                                                           | The application ID of the activity to create a rich presence invite for (defaults to the primary activity if unspecified)                                                                            |
| flags?                      | integer                                                                                             | The [message's flags](mensajes.md#message-flags) (only `SUPPRESS_EMBEDS`, `SUPPRESS_NOTIFICATIONS`, and `VOICE_MESSAGE` can be set)                                                                  |
| files\[n]? <sup>1</sup>     | file contents                                                                                       | Contents of the file being sent (max 10)                                                                                                                                                             |
| payload\_json? <sup>1</sup> | string                                                                                              | JSON-encoded body of non-file params                                                                                                                                                                 |
| attachments? <sup>1</sup>   | array\[partial [attachment](mensajes.md#attachment-object) object]                                  | Partial attachment objects with `filename` and `description` (max 10)                                                                                                                                |
| poll?                       | [poll create](mensajes.md#poll-create-structure) object                                             | A poll!                                                                                                                                                                                              |
| confetti\_potion?           | [confetti potion](mensajes.md#confetti-potion-structure) object                                     | A confetti potion to apply to the message                                                                                                                                                            |

<sup>1</sup> See [Uploading Files](https://docs.discord.food/reference#uploading-files) for details.

<sup>2</sup> Cannot be used by user accounts.

<sup>3</sup> Sending multiple messages in the same channel with the same nonce in a short period of time will result in only the first message being sent.

[**Example Request Body (application/json)**](mensajes.md#example-request-body-\(application/json\))

```
{  "content": "Hello, World!",  "tts": false,  "embeds": [    {      "title": "Hello, Embed!",      "description": "This is an embedded message."    }  ]}
```

Examples for file uploads are available in [Uploading Files](https://docs.discord.food/reference#uploading-files).

#### [Create DM Message](mensajes.md#create-dm-message) <a href="#create-dm-message" id="create-dm-message"></a>

`POST/users/{user.id}/messages`

Posts a message to a text-based channel. Returns a [message](mensajes.md#message-object) object on success. Fires a [Message Create](https://docs.discord.food/topics/gateway-events#message-create) Gateway event.\
Functionally identical to the [Create Message](mensajes.md#create-message) endpoint, but is used for DM channels in an OAuth2 context and has some additional parameters. Check there for more information.

A message can be sent between two users in the following situations:

* Both users are online and have a presence corresponding to the OAuth2 application (i.e. in the game)
* Both users are friends with each other
* Both users share a mutual guild with DMs allowed and have previously DM'd each other on Discord

| Field     | Type   | Description                                                                      |
| --------- | ------ | -------------------------------------------------------------------------------- |
| metadata? | object | Custom metadata for the message (max 25 keys, 1024 characters per key and value) |

#### [Create Greet Message](mensajes.md#create-greet-message) <a href="#create-greet-message" id="create-greet-message"></a>

`POST/channels/{channel.id}/greet`

Posts a greet message to a channel. This endpoint requires the channel is a DM channel or you reply to a system message. Returns a [message](mensajes.md#message-object) object on success. Fires a [Message Create](https://docs.discord.food/topics/gateway-events#message-create) Gateway event.

[**JSON Params**](mensajes.md#json-params)

| Field               | Type                                                                | Description                                                                                                 |
| ------------------- | ------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| sticker\_ids        | array\[snowflake]                                                   | IDs of up to 1 [sticker](https://docs.discord.food/resources/sticker#sticker-object) to send in the message |
| allowed\_mentions?  | [allowed mention](mensajes.md#allowed-mentions-object) object       | Allowed mentions for the message                                                                            |
| message\_reference? | [message reference](mensajes.md#message-reference-structure) object | The message being replied to                                                                                |

#### [Create Attachments](mensajes.md#create-attachments) <a href="#create-attachments" id="create-attachments"></a>

`POST/channels/{channel.id}/attachments`

Creates attachment URLs to upload the intended attachments directly to Discord's GCP storage bucket. Returns an array of [cloud attachment](mensajes.md#cloud-attachment-structure) objects. Requires the same permissions as uploading an attachment inline with a message. See [Uploading to Google Cloud](https://docs.discord.food/reference#uploading-to-google-cloud) for more information.

[**JSON Params**](mensajes.md#json-params)

| Field | Type                                                                        | Description                                                               |
| ----- | --------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| files | array\[[upload attachment](mensajes.md#upload-attachment-structure) object] | The target files to create a URL for, containing the name and size (1-10) |

[**Upload Attachment Structure**](mensajes.md#upload-attachment-structure)

| Field                  | Type    | Description                                                                                                                              |
| ---------------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| id?                    | ?string | The ID of the attachment to reference in the response                                                                                    |
| filename               | string  | The name of the file being uploaded                                                                                                      |
| file\_size             | integer | The size of the file being uploaded in bytes                                                                                             |
| is\_clip? <sup>1</sup> | boolean | Whether the file being uploaded is a [clipped recording of a stream](https://support.discord.com/hc/en-us/articles/16861982215703-Clips) |

<sup>1</sup> When uploading a clip, must be . [See message types](mensajes.md#clips) for more information.

[**Cloud Attachment Structure**](mensajes.md#cloud-attachment-structure)

| Field            | Type    | Description                                                 |
| ---------------- | ------- | ----------------------------------------------------------- |
| id               | ?string | The ID of the attachment upload, if provided in the request |
| upload\_url      | string  | The URL to upload the file to                               |
| upload\_filename | string  | The name of the uploaded file                               |

[**Example Response**](mensajes.md#example-response)

```
{  "attachments": [    {      "id": "23",      "upload_url": "https://discord-attachments-uploads-prd.storage.googleapis.com/87e49c99-43f8-4a33-baad-5a834c94424c/cat.png?upload_id=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",      "upload_filename": "87e49c99-43f8-4a33-baad-5a834c94424c/cat.png"    }  ]}
```

#### [Delete Attachment](mensajes.md#delete-attachment) <a href="#delete-attachment" id="delete-attachment"></a>

`DELETE/attachments/{cloud_attachment.upload_filename}`

Deletes an attachment from Discord's GCP storage bucket. Returns a 204 empty response on success.

This endpoint should be used to delete an uploaded attachment that was not used. See [Uploading to Google Cloud](https://docs.discord.food/reference#uploading-to-google-cloud) for more information.

#### [Refresh Attachment URLs](mensajes.md#refresh-attachment-urls) <a href="#refresh-attachment-urls" id="refresh-attachment-urls"></a>

`POST/attachments/refresh-urls`

Refreshes the URLs of attachments that were uploaded to Discord's CDN. The provided URLs do not have to be valid or signed. Existing query string parameters are preserved.

[**JSON Params**](mensajes.md#json-params)

| Field            | Type           | Description                                   |
| ---------------- | -------------- | --------------------------------------------- |
| attachment\_urls | array\[string] | The URLs of the attachments to refresh (1-50) |

[**Response Body**](mensajes.md#response-body)

| Field           | Type                                                                              | Description        |
| --------------- | --------------------------------------------------------------------------------- | ------------------ |
| refreshed\_urls | array\[[refreshed attachment](mensajes.md#refreshed-attachment-structure) object] | The refreshed URLs |

[**Refreshed Attachment Structure**](mensajes.md#refreshed-attachment-structure)

| Field     | Type   | Description       |
| --------- | ------ | ----------------- |
| original  | string | The provided URL  |
| refreshed | string | The refreshed URL |

[**Example Response**](mensajes.md#example-response)

```
{  "refreshed_urls": [    {      "original": "https://cdn.discordapp.com/attachments/1012345678900020080/1234567891233211234/my_image.png?ex=65d903de&is=65c68ede&hm=2481f30dd67f503f54d020ae3b5533b9987fae4e55f2b4e3926e08a3fa3ee24f&",      "refreshed": "https://cdn.discordapp.com/attachments/1012345678900020080/1234567891233211234/my_image.png?ex=66143372&is=6601be72&hm=5a90a0ac363d9de3619044102ffe963041517f0e2f78baecabfc2f544a14eace&"    }  ]}
```

#### [Scan Explicit Media](mensajes.md#scan-explicit-media) <a href="#scan-explicit-media" id="scan-explicit-media"></a>

`PATCH/channels/{channel.id}/explicit-media`

Scans for explicit media in a list of messages. Returns a 204 empty response on success. Fires multiple [Message Update](https://docs.discord.food/topics/gateway-events#message-update) Gateway events.

This endpoint should be used by users with explicit content filtering enabled to scan for explicit media on messages that have embeds or attachments with a missing or outdated . Invalid message IDs are ignored.

[**JSON Params**](mensajes.md#json-params)

| Field        | Type              | Description                    |
| ------------ | ----------------- | ------------------------------ |
| message\_ids | array\[snowflake] | The message IDs to scan (1-75) |

/messages/explicit-media

#### [Bulk Scan Explicit Media](mensajes.md#bulk-scan-explicit-media) <a href="#bulk-scan-explicit-media" id="bulk-scan-explicit-media"></a>

`PATCH/messages/explicit-media`

Scans for explicit media in a list of messages in multiple channels. Returns a 204 empty response on success. Fires multiple [Message Update](https://docs.discord.food/topics/gateway-events#message-update) Gateway events.

Similar to [Scan Explicit Media](mensajes.md#scan-explicit-media), but allows scanning messages in multiple channels at once. Invalid channel and message IDs are ignored.

[**JSON Params**](mensajes.md#json-params)

| Field    | Type                                                                        | Description                  |
| -------- | --------------------------------------------------------------------------- | ---------------------------- |
| messages | array\[[bulk scan message](mensajes.md#bulk-scan-message-structure) object] | The messages to scan (1-100) |

[**Bulk Scan Message Structure**](mensajes.md#bulk-scan-message-structure)

| Field       | Type      | Description                             |
| ----------- | --------- | --------------------------------------- |
| channel\_id | snowflake | The ID of the channel the message is in |
| message\_id | snowflake | The ID of the message to scan           |

#### [Report Sent Explicit Content False Positive](mensajes.md#report-sent-explicit-content-false-positive) <a href="#report-sent-explicit-content-false-positive" id="report-sent-explicit-content-false-positive"></a>

`POST/attachments/sender-report-false-positive`

Reports an explicit content false positive for a list of uploaded attachments. Returns a 204 empty response on success.

This endpoint should be used after the user attempts to send an message containing attachments but receives a 400 bad request with a [JSON error code](https://docs.discord.food/topics/opcodes-and-status-codes#json-error-codes).

[**JSON Params**](mensajes.md#json-params)

| Field           | Type              | Description                                                                      |
| --------------- | ----------------- | -------------------------------------------------------------------------------- |
| channel\_id     | snowflake         | The ID of the channel the message is in                                          |
| message\_id     | snowflake         | A locally-generated ID representing the message                                  |
| attachment\_ids | array\[snowflake] | The IDs of the attachments that were flagged as explicit content (max 100)       |
| filenames       | array\[string]    | The filenames of the attachments that were flagged as explicit content (max 100) |

#### [Report Explicit Content False Positive](mensajes.md#report-explicit-content-false-positive) <a href="#report-explicit-content-false-positive" id="report-explicit-content-false-positive"></a>

`POST/attachments/report-false-positive`

Reports an explicit content false positive for a message. Returns a 204 empty response on success.

This endpoint should be used upon viewing a message that was flagged as explicit content but was determined to be a false positive.

[**JSON Params**](mensajes.md#json-params)

| Field           | Type              | Description                                                                                                                              |
| --------------- | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| channel\_id     | snowflake         | The ID of the channel the message is in                                                                                                  |
| message\_id     | snowflake         | The ID of the message that was flagged as explicit content                                                                               |
| attachment\_ids | array\[snowflake] | The IDs of the attachments that were flagged as explicit content (max 100)                                                               |
| embed\_ids      | array\[string]    | Locally generated IDs representing the embeds that were flagged as explicit content, in the format `lodash.uniqueId("embed_")` (max 100) |

#### [Acknowledge Message](mensajes.md#acknowledge-message) <a href="#acknowledge-message" id="acknowledge-message"></a>

`POST/channels/{channel.id}/messages/{message.id}/ack`

Sets the channel's latest acknowledged message (marks a message as read) for the current user. Fires a Message Ack Gateway event.

The message ID parameter does not need to be a valid message ID, but it must be a valid snowflake. If the message ID is being set to a message sent prior to the latest acknowledged one, should be or the resulting read state update should be ignored by clients (but is still saved), resulting in undefined behavior. In this case, should also be set to the amount of mentions unacknowledged as it is not automatically calculated by Discord.

[**JSON Params**](mensajes.md#json-params)

| Field                        | Type    | Description                                        |
| ---------------------------- | ------- | -------------------------------------------------- |
| token?                       | ?string | The last received ack token, or `null`             |
| manual?                      | boolean | Whether the acknowleged message ID is manually set |
| mention\_count? <sup>1</sup> | integer | The new unread indicator for the channel           |

<sup>1</sup> Requires to be .

[**Response Body**](mensajes.md#response-body)

| Field | Type    | Description       |
| ----- | ------- | ----------------- |
| token | ?string | The new ack token |

#### [Crosspost Message](mensajes.md#crosspost-message) <a href="#crosspost-message" id="crosspost-message"></a>

`POST/channels/{channel.id}/messages/{message.id}/crosspost`

Crossposts a message in a News Channel to following channels. Requires the permission if the current user sent the message, or additionally the permission for all other messages. Returns a [message](mensajes.md#message-object) object on success. Fires a [Message Update](https://docs.discord.food/topics/gateway-events#message-update) (and possibly multiple [Message Create](https://docs.discord.food/topics/gateway-events#message-create)) Gateway event.

#### [Hide Message from Guild Feed](mensajes.md#hide-message-from-guild-feed) <a href="#hide-message-from-guild-feed" id="hide-message-from-guild-feed"></a>

`POST/channels/{channel.id}/messages/{message.id}/hide-guild-feed`

Hides a message from the feed of the guild the channel belongs to. Returns a 204 empty response on success.

#### [Get Reactions](mensajes.md#get-reactions) <a href="#get-reactions" id="get-reactions"></a>

`GET/channels/{channel.id}/messages/{message.id}/reactions/{emoji}`

Get a list of users that reacted with this emoji. Returns an array of partial [user](https://docs.discord.food/resources/user#user-object) objects. The must be [URL Encoded](https://en.wikipedia.org/wiki/Percent-encoding) or the request will fail. To use custom emoji, you must encode it in the format with the emoji name and emoji ID.

[**Query String Params**](mensajes.md#query-string-params)

| Field  | Type      | Description                                                                            |
| ------ | --------- | -------------------------------------------------------------------------------------- |
| after? | snowflake | Get users after this user ID                                                           |
| limit? | integer   | Max number of users to return (1-100, default 25)                                      |
| type?  | integer   | The [type of reaction](mensajes.md#reaction-type) to get users for (default `REGULAR`) |

#### [Create Reaction](mensajes.md#create-reaction) <a href="#create-reaction" id="create-reaction"></a>

`PUT/channels/{channel.id}/messages/{message.id}/reactions/{emoji}/@me`

Creates a reaction for the message. Requires the permission if operating on a guild channel. Additionally, if nobody else has reacted to the message using this emoji, this endpoint requires the permission. Returns a 204 empty response on success. Fires a [Message Reaction Add](https://docs.discord.food/topics/gateway-events#message-reaction-add) Gateway event. The must be [URL Encoded](https://en.wikipedia.org/wiki/Percent-encoding) or the request will fail. To use custom emoji, you must encode it in the format with the emoji name and emoji ID.

[**Query String Params**](mensajes.md#query-string-params)

| Field | Type    | Description                                                                     |
| ----- | ------- | ------------------------------------------------------------------------------- |
| type? | integer | The [type of reaction](mensajes.md#reaction-type) to create (default `REGULAR`) |

#### [Delete Own Reaction](mensajes.md#delete-own-reaction) <a href="#delete-own-reaction" id="delete-own-reaction"></a>

`DELETE/channels/{channel.id}/messages/{message.id}/reactions/{emoji}/{reaction_type}/@me`

Deletes a reaction the current user has made for the message. Returns a 204 empty response on success. Fires a [Message Reaction Remove](https://docs.discord.food/topics/gateway-events#message-reaction-remove) Gateway event. The must be [URL Encoded](https://en.wikipedia.org/wiki/Percent-encoding) or the request will fail. To use custom emoji, you must encode it in the format with the emoji name and emoji ID.

#### [Delete Reaction](mensajes.md#delete-reaction) <a href="#delete-reaction" id="delete-reaction"></a>

`DELETE/channels/{channel.id}/messages/{message.id}/reactions/{emoji}/{reaction_type}/{user.id}`

Deletes another user's reaction. Requires the permission. Returns a 204 empty response on success. Fires a [Message Reaction Remove](https://docs.discord.food/topics/gateway-events#message-reaction-remove) Gateway event. The must be [URL Encoded](https://en.wikipedia.org/wiki/Percent-encoding) or the request will fail. To use custom emoji, you must encode it in the format with the emoji name and emoji ID.

#### [Delete Reaction Emoji](mensajes.md#delete-reaction-emoji) <a href="#delete-reaction-emoji" id="delete-reaction-emoji"></a>

`DELETE/channels/{channel.id}/messages/{message.id}/reactions/{emoji}`

Deletes all the reactions for a given emoji on a message. Returns a 204 empty response on success. Requires the permission. Fires a [Message Reaction Remove Emoji](https://docs.discord.food/topics/gateway-events#message-reaction-remove-emoji) Gateway event. The must be [URL Encoded](https://en.wikipedia.org/wiki/Percent-encoding) or the request will fail. To use custom emoji, you must encode it in the format with the emoji name and emoji ID.

#### [Delete All Reactions](mensajes.md#delete-all-reactions) <a href="#delete-all-reactions" id="delete-all-reactions"></a>

`DELETE/channels/{channel.id}/messages/{message.id}/reactions`

Deletes all reactions on a message. Returns a 204 empty response on success. Requires the permission. Fires a [Message Reaction Remove All](https://docs.discord.food/topics/gateway-events#message-reaction-remove-all) Gateway event.

#### [Edit Message](mensajes.md#edit-message) <a href="#edit-message" id="edit-message"></a>

`PATCH/channels/{channel.id}/messages/{message.id}`

Edits a previously sent message. All fields can be edited by the original message author. Other users can only edit and only if they have the permission in the corresponding channel. When specifying flags, ensure to include all previously set flags/bits in addition to ones that you are modifying.

When the field is edited, the array in the message object will be reconstructed from scratch based on the new content. The field of the edit request controls how this happens. If there is no explicit in the edit request, the content will be parsed with _default_ allowances, that is, without regard to whether or not an was present in the request that originally created the message.

Returns a [message](mensajes.md#message-object) object. Fires a [Message Update](mensajes.md#message-object) Gateway event.

Refer to [Uploading Files](https://docs.discord.food/reference#uploading-files) for details on attachments and requests. Any provided files will be **appended** to the message. To remove or replace files you will have to supply the field which specifies the files to retain on the message after edit.

[**JSON/Form Params**](mensajes.md#json/form-params)

| Field                      | Type                                                                                                 | Description                                                                                    |
| -------------------------- | ---------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| content?                   | ?string                                                                                              | The message contents (up to 2000 characters)                                                   |
| embeds? <sup>2</sup>       | ?array\[[embed](mensajes.md#embed-object) object]                                                    | Embedded `rich` content (max 6000 characters)                                                  |
| allowed\_mentions?         | ?[allowed mention](mensajes.md#allowed-mentions-object) object                                       | Allowed mentions for the message                                                               |
| components? <sup>2</sup>   | ?array\[[message component](https://docs.discord.food/resources/components#component-object) object] | The components to include with the message                                                     |
| flags?                     | ?integer                                                                                             | The [message's flags](mensajes.md#message-flags) (only `SUPPRESS_EMBEDS` can be set)           |
| files\[n] <sup>1</sup>     | ?file contents                                                                                       | Contents of the file being sent                                                                |
| payload\_json <sup>1</sup> | ?string                                                                                              | JSON-encoded body of non-file params                                                           |
| attachments <sup>1</sup>   | ?array\[partial [attachment](mensajes.md#attachment-object) object]                                  | Partial attachment objects with `filename` and `description`, including attached files to keep |

<sup>1</sup> See [Uploading Files](https://docs.discord.food/reference#uploading-files) for details.

<sup>2</sup> Cannot be used by user accounts.

#### [Edit DM Message](mensajes.md#edit-dm-message) <a href="#edit-dm-message" id="edit-dm-message"></a>

`PATCH/users/{user.id}/messages/{message.id}`

Edits a previously sent message. Messages can be edited by the original message author. Returns a [message](mensajes.md#message-object) object. Fires a [Message Update](mensajes.md#message-object) Gateway event.

[**JSON/Form Params**](mensajes.md#json/form-params)

| Field    | Type    | Description                                  |
| -------- | ------- | -------------------------------------------- |
| content? | ?string | The message contents (up to 2000 characters) |

#### [Delete Message](mensajes.md#delete-message) <a href="#delete-message" id="delete-message"></a>

`DELETE/channels/{channel.id}/messages/{message.id}`

Deletes a message. Requires the permission if operating on a guild channel and trying to delete a message that was not sent by the current user. Returns a 204 empty response on success. Fires a [Message Delete](https://docs.discord.food/topics/gateway-events#message-delete) Gateway event.

#### [Delete DM Message](mensajes.md#delete-dm-message) <a href="#delete-dm-message" id="delete-dm-message"></a>

`DELETE/users/@me/messages/{message.id}`

Deletes a message. Messages can be deleted by the original message author. Returns a 204 empty response on success. Fires a [Message Delete](https://docs.discord.food/topics/gateway-events#message-delete) Gateway event.

#### [Bulk Delete Messages](mensajes.md#bulk-delete-messages) <a href="#bulk-delete-messages" id="bulk-delete-messages"></a>

`POST/channels/{channel.id}/messages/bulk-delete`

Deletes multiple messages from a guild channel in a single request. Requires the permission. Returns a 204 empty response on success. Fires a [Message Delete Bulk](https://docs.discord.food/topics/gateway-events#message-delete-bulk) Gateway event.

Any message IDs given that do not exist or are invalid will count towards the minimum and maximum message count (currently 2 and 100 respectively).

[**JSON Params**](mensajes.md#json-params)

| Field    | Type              | Description                       |
| -------- | ----------------- | --------------------------------- |
| messages | array\[snowflake] | The message IDs to delete (2-100) |

#### [Get Message Pins](mensajes.md#get-message-pins) <a href="#get-message-pins" id="get-message-pins"></a>

`GET/channels/{channel.id}/messages/pins`

Returns all message pins in the channel. Requires the permission.

[**Query String Params**](mensajes.md#query-string-params)

| Field   | Type              | Description                                     |
| ------- | ----------------- | ----------------------------------------------- |
| before? | ISO8601 timestamp | Get messages pinned before this timestamp       |
| limit?  | integer           | Max number of pins to return (1-50, default 50) |

[**Response Body**](mensajes.md#response-body)

| Field     | Type                                                         | Description                                                                                       |
| --------- | ------------------------------------------------------------ | ------------------------------------------------------------------------------------------------- |
| items     | array\[[message pin](mensajes.md#message-pin-object) object] | The message pins in the channel                                                                   |
| has\_more | boolean                                                      | Whether there are potentially additional message pins that could be returned on a subsequent call |

#### [Get Pinned Messages](mensajes.md#get-pinned-messages) <a href="#get-pinned-messages" id="get-pinned-messages"></a>

`GET/channels/{channel.id}/pins`

Returns up to 50 pinned messages in the channel as an array of [message](mensajes.md#message-object) objects without the key.

#### [Pin Message](mensajes.md#pin-message) <a href="#pin-message" id="pin-message"></a>

`PUT/channels/{channel.id}/messages/pins/{message.id}`

Pins a message in a channel. Requires the permission if operating on a guild channel. Returns a 204 empty response on success. Fires a [Channel Pins Update](https://docs.discord.food/topics/gateway-events#channel-pins-update) Gateway event.

#### [Unpin Message](mensajes.md#unpin-message) <a href="#unpin-message" id="unpin-message"></a>

`DELETE/channels/{channel.id}/messages/pins/{message.id}`

Unpins a message in a channel. Requires the permission if operating on a guild channel. Returns a 204 empty response on success. Fires a [Channel Pins Update](https://docs.discord.food/topics/gateway-events#channel-pins-update) Gateway event.

#### [Acknowledge Pinned Messages](mensajes.md#acknowledge-pinned-messages) <a href="#acknowledge-pinned-messages" id="acknowledge-pinned-messages"></a>

`POST/channels/{channel.id}/pins/ack`

Acknowledges the currently pinned messages in a channel. Returns a 204 empty response on success. Fires a Channel Pins Ack Gateway event.

#### [Get Channel Media Preview](mensajes.md#get-channel-media-preview) <a href="#get-channel-media-preview" id="get-channel-media-preview"></a>

`GET/channels/{channel.id}/media-post-preview`

Returns information on the media of a post (thread) in a media channel. The media channel must be a paywalled role subscription benefit. If the user is not in the guild, the guild must be discoverable.

[**Response Body**](mensajes.md#response-body)

| Field | Type                                                        | Description                                            |
| ----- | ----------------------------------------------------------- | ------------------------------------------------------ |
| media | [media preview](mensajes.md#media-preview-structure) object | The preview of the media in the thread's first message |

[**Media Preview Structure**](mensajes.md#media-preview-structure)

| Field                               | Type                                               | Description                                                                                      |
| ----------------------------------- | -------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| guild\_id                           | snowflake                                          | The ID of the guild the media is in                                                              |
| guild\_name                         | string                                             | The name of the guild the media is in                                                            |
| guild\_icon                         | string                                             | The [icon hash](https://docs.discord.food/reference#cdn-formatting) of the guild the media is in |
| channel\_id                         | snowflake                                          | The ID of the thread that represents the media                                                   |
| parent\_channel\_id                 | snowflake                                          | The ID of the media channel the thread is in                                                     |
| message\_id                         | snowflake                                          | The ID of the thread's first message (same as the thread ID)                                     |
| author\_id                          | snowflake                                          | The ID of the author of the thread's first message                                               |
| title                               | string                                             | The name of the thread                                                                           |
| description                         | string                                             | The first 64 characters of the first message's content                                           |
| has\_media\_attachment <sup>1</sup> | boolean                                            | Whether the first message has a media attachment                                                 |
| thumbnail?                          | [attachment](mensajes.md#attachment-object) object | The thumbnail of the thread's first message, if any                                              |

<sup>1</sup> If a media attachment is present, a fake thumbnail will be rendered in the preview as a CTA to subscribe to the role that unlocks the media channel.

[**Example Response**](mensajes.md#example-response)

```
{  "media": {    "guild_id": "1046920999469330512",    "channel_id": "1120793989809967114",    "parent_channel_id": "1120793939562217483",    "message_id": "1120793989809967114",    "title": "feet picers",    "description": "https://tenor.com/view/naomi-bunny-melon-gif-22514980",    "guild_name": "Hood Network",    "guild_icon": "a_78187748cb59baec2bf6a1f8766ff9fc",    "author_id": "728342296696979526",    "thumbnail": {      "url": "https://media.tenor.com/ttfzDdgGOqgAAAAM/naomi-bunny.png",      "proxy_url": "https://images-ext-2.discordapp.net/external/RGh8LSJbaADcUFgzFsvpODM6E2nz3xKVg_Mrg9UE58k/https/media.tenor.com/ttfzDdgGOqgAAAAe/naomi-bunny.png",      "width": 388,      "height": 640    },    "has_media_attachment": true  }}
```

#### [Unfurl Embed](mensajes.md#unfurl-embed) <a href="#unfurl-embed" id="unfurl-embed"></a>

`POST/unfurler/unfurl`

Returns debug information about an embed.

[**JSON Params**](mensajes.md#json-params)

| Field | Type   | Description       |
| ----- | ------ | ----------------- |
| url   | string | The URL to unfurl |

[**Response Body**](mensajes.md#response-body)

| Field           | Type                                                          | Description                                                   |
| --------------- | ------------------------------------------------------------- | ------------------------------------------------------------- |
| response?       | [embed response](mensajes.md#embed-response-structure) object | The response from the website                                 |
| error?          | string                                                        | The error that occurred while parsing the website             |
| context\_errors | array\[string]                                                | The contextual errors that occurred while parsing the website |
| embeds?         | array\[[embed](mensajes.md#embed-object) object]              | The found embeds for the URL                                  |

[**Embed Response Structure**](mensajes.md#embed-response-structure)

| Field   | Type                 | Description                                            |
| ------- | -------------------- | ------------------------------------------------------ |
| headers | map\[string, string] | The relevant headers returned by the website           |
| body    | string               | The body of the website, if used to generate the embed |

#### [Unfurl Embeds](mensajes.md#unfurl-embeds) <a href="#unfurl-embeds" id="unfurl-embeds"></a>

`POST/unfurler/embed-urls`

Returns embed data from a list of URLs.

[**JSON Params**](mensajes.md#json-params)

| Field | Type           | Description                |
| ----- | -------------- | -------------------------- |
| urls  | array\[string] | The URLs to unfurl (max 4) |

[**Response Body**](mensajes.md#response-body)

| Field  | Type                                             | Description                   |
| ------ | ------------------------------------------------ | ----------------------------- |
| embeds | array\[[embed](mensajes.md#embed-object) object] | The found embeds for the URLs |

#### [Get Answer Voters](mensajes.md#get-answer-voters) <a href="#get-answer-voters" id="get-answer-voters"></a>

`GET/channels/{channel.id}/polls/{message.id}/answers/{answer.id}`

Get a list of users that voted for this specific answer.

[**Query String Params**](mensajes.md#query-string-params)

| Field  | Type      | Description                                       |
| ------ | --------- | ------------------------------------------------- |
| after? | snowflake | Get users after this user ID                      |
| limit? | integer   | Max number of users to return (1-100, default 25) |

[**Response Body**](mensajes.md#response-body)

| Field | Type                                                                                | Description                     |
| ----- | ----------------------------------------------------------------------------------- | ------------------------------- |
| users | array\[partial [user](https://docs.discord.food/resources/user#user-object) object] | Users who voted for this answer |

#### [End Poll](mensajes.md#end-poll) <a href="#end-poll" id="end-poll"></a>

`POST/channels/{channel.id}/polls/{message.id}/expire`

Immediately ends the poll. You cannot end polls from other users.

Returns a [message](mensajes.md#message-object) object. Fires a [Message Update](https://docs.discord.food/topics/gateway-events#message-update) Gateway event.

#### [Create Poll Vote](mensajes.md#create-poll-vote) <a href="#create-poll-vote" id="create-poll-vote"></a>

`PUT/channels/{channel.id}/polls/{message.id}/answers/@me`

Submits a poll vote for the current user. Returns a 204 empty response on success. May fire multiple [Message Poll Vote Add](https://docs.discord.food/topics/gateway-events#message-poll-vote-add) and [Message Poll Vote Remove](https://docs.discord.food/topics/gateway-events#message-poll-vote-remove) Gateway events.

[**JSON Params**](mensajes.md#json-params)

| Field       | Type            | Description                            |
| ----------- | --------------- | -------------------------------------- |
| answer\_ids | array\[integer] | Selected answers, empty to clear votes |

#### [Get Conversation Summaries](mensajes.md#get-conversation-summaries) <a href="#get-conversation-summaries" id="get-conversation-summaries"></a>

`GET/channels/{channel.id}/summaries`

Get a list of up to 50 latest conversation summaries for a text channel in reverse chronological order. Requires the permission.

[**Response Body**](mensajes.md#response-body)

| Field     | Type                                                                           | Description                                         |
| --------- | ------------------------------------------------------------------------------ | --------------------------------------------------- |
| summaries | array\[[conversation summary](mensajes.md#conversation-summary-object) object] | The conversation summaries for the channel (max 50) |

#### [Delete Conversation Summary](mensajes.md#delete-conversation-summary) <a href="#delete-conversation-summary" id="delete-conversation-summary"></a>

`DELETE/channels/{channel.id}/summaries/{summary.id}`

Deletes a conversation summary. Requires the permission. Returns a 204 empty response on success. Fires a [Conversation Summary Update](https://docs.discord.food/topics/gateway-events#conversation-summary-update) Gateway event.

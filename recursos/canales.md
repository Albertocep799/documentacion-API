---
icon: message
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

# Canales

### [Channels](canales.md#channels) <a href="#channels" id="channels"></a>

Channels are the primary way users interact with Discord. All conversations, whether over text or voice, in guilds or a group, happen in channels.

#### [Channel Object](canales.md#channel-object) <a href="#channel-object" id="channel-object"></a>

A guild or private channel within Discord.

[**Channel Structure**](canales.md#channel-structure)

| Field                                           | Type                                                                                  | Description                                                                                                                                                                                  |
| ----------------------------------------------- | ------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id <sup>8</sup>                                 | snowflake                                                                             | The ID of the channel                                                                                                                                                                        |
| type                                            | integer                                                                               | The [type of channel](canales.md#channel-type)                                                                                                                                               |
| guild\_id?                                      | snowflake                                                                             | The ID of the guild the channel is in                                                                                                                                                        |
| position?                                       | integer                                                                               | Sorting position of the channel                                                                                                                                                              |
| permission\_overwrites?                         | array\[[permission overwrite](canales.md#permission-overwrite-object) object]         | Explicit permission overwrites for members and roles                                                                                                                                         |
| name?                                           | ?string                                                                               | The name of the channel (1-100 characters)                                                                                                                                                   |
| topic?                                          | ?string                                                                               | The channel topic (max 4096 characters for thread-only channels, max 1024 characters otherwise)                                                                                              |
| nsfw?                                           | boolean                                                                               | Whether the channel is NSFW                                                                                                                                                                  |
| last\_message\_id?                              | ?snowflake                                                                            | The ID of the last message sent (or thread created for thread-only channels, guild added for directory channels) in this channel (may not point to an existing resource)                     |
| bitrate?                                        | integer                                                                               | The bitrate (in bits) of the voice channel                                                                                                                                                   |
| user\_limit? <sup>1</sup>                       | integer                                                                               | The user limit of the voice channel (max 99, 0 refers to no limit)                                                                                                                           |
| rate\_limit\_per\_user? <sup>2</sup>            | integer                                                                               | Duration in seconds seconds a user has to wait before sending another message (max 21600); bots, as well as users with the permission `MANAGE_MESSAGES` or `MANAGE_CHANNELS`, are unaffected |
| recipients?                                     | array\[partial [user](https://docs.discord.food/resources/user#user-object) object]   | The recipients of the private channel, excluding the requesting user                                                                                                                         |
| recipient\_flags? <sup>3</sup>                  | integer                                                                               | The [recipient flags](canales.md#recipient-flags) of the DM                                                                                                                                  |
| icon?                                           | ?string                                                                               | The group DM's [icon hash](https://docs.discord.food/reference#cdn-formatting)                                                                                                               |
| nicks?                                          | array\[[channel nick](canales.md#channel-nick-structure) object]                      | The nicknames of the users in the group DM                                                                                                                                                   |
| managed?                                        | boolean                                                                               | Whether the group DM is managed by an application                                                                                                                                            |
| blocked\_user\_warning\_dismissed? <sup>7</sup> | boolean                                                                               | Whether the user has acknowledged the presence of blocked users in the group DM                                                                                                              |
| safety\_warnings? <sup>3</sup>                  | array\[[safety warning](canales.md#safety-warning-structure) object]                  | The safety warnings for the DM channel                                                                                                                                                       |
| application\_id?                                | snowflake                                                                             | The ID of the application that manages the group DM                                                                                                                                          |
| owner\_id?                                      | snowflake                                                                             | The ID of the owner of the group DM or thread                                                                                                                                                |
| owner?                                          | ?[guild member](https://docs.discord.food/resources/guild#guild-member-object) object | The owner of this thread; only included on certain API endpoints                                                                                                                             |
| parent\_id?                                     | ?snowflake                                                                            | The ID of the parent category/channel for the guild channel/thread                                                                                                                           |
| last\_pin\_timestamp?                           | ?ISO8601 timestamp                                                                    | When the last pinned message was pinned, if any                                                                                                                                              |
| rtc\_region?                                    | ?string                                                                               | The [voice region](https://docs.discord.food/resources/voice#voice-region-object) ID for the voice channel (automatic when `null`)                                                           |
| video\_quality\_mode?                           | integer                                                                               | The camera [video quality mode](canales.md#video-quality-mode) of the voice channel (default `AUTO`)                                                                                         |
| total\_message\_sent?                           | integer                                                                               | The number of messages ever sent in a thread; similar to `message_count` on message creation, but will not decrement the number when a message is deleted                                    |
| message\_count?                                 | integer                                                                               | The number of messages (not including the initial message or deleted messages) in a thread (if the thread was created before July 1, 2022, it stops counting at 50)                          |
| member\_count?                                  | integer                                                                               | An approximate count of users in a thread, stops counting at 50                                                                                                                              |
| member\_ids\_preview?                           | array\[snowflake]                                                                     | The IDs of some of the members in a thread                                                                                                                                                   |
| thread\_metadata?                               | [thread metadata](canales.md#thread-metadata-object) object                           | Thread-specific channel metadata                                                                                                                                                             |
| member?                                         | [thread member](canales.md#thread-member-object) object                               | Thread member object for the current user, if they have joined the thread; only included on certain API endpoints                                                                            |
| default\_auto\_archive\_duration? <sup>4</sup>  | ?integer                                                                              | Default duration in minutes for newly created threads to stop showing in the channel list after inactivity (one of 60, 1440, 4320, 10080)                                                    |
| default\_thread\_rate\_limit\_per\_user?        | integer                                                                               | Default duration in seconds a user has to wait before sending another message in newly created threads; this field is copied to the thread at creation time and does not live update         |
| permissions? <sup>5</sup>                       | string                                                                                | Computed permissions for the invoking user in the channel, including overwrites                                                                                                              |
| flags?                                          | integer                                                                               | The [channel's flags](canales.md#channel-flags)                                                                                                                                              |
| available\_tags?                                | array\[[tag](canales.md#forum-tag-object) object]                                     | The tags that can be used in a thread-only channel (max 20)                                                                                                                                  |
| applied\_tags?                                  | array\[snowflake]                                                                     | The IDs of tags that are applied to a thread in a thread-only channel (max 5)                                                                                                                |
| default\_reaction\_emoji?                       | ?[default reaction](canales.md#default-reaction-object) object                        | The emoji to show in the add reaction button on a thread in a thread-only channel                                                                                                            |
| default\_forum\_layout?                         | integer                                                                               | The default [layout](canales.md#forum-layout-type) of a thread-only channel                                                                                                                  |
| default\_sort\_order?                           | ?integer                                                                              | The default [sort order](canales.md#sort-order-type) of a thread-only channel's threads (default `LATEST_ACTIVITY`)                                                                          |
| default\_tag\_setting?                          | string                                                                                | The default [tag search setting](canales.md#search-tag-setting) for a thread-only channel                                                                                                    |
| icon\_emoji?                                    | ?[icon emoji](canales.md#icon-emoji-object) object                                    | The emoji to show next to the channel name in channels list                                                                                                                                  |
| is\_message\_request?                           | boolean                                                                               | Whether the DM is a message request                                                                                                                                                          |
| is\_message\_request\_timestamp?                | ?ISO8601 timestamp                                                                    | When the message request was created                                                                                                                                                         |
| is\_spam?                                       | boolean                                                                               | Whether the DM is a spam message request                                                                                                                                                     |
| theme\_color?                                   | ?integer                                                                              | The background color of the channel icon emoji encoded as an integer representation of a hexadecimal color code                                                                              |
| status? <sup>6</sup>                            | ?string                                                                               | The status of the voice channel (max 500 characters)                                                                                                                                         |
| hd\_streaming\_until?                           | ?ISO8601 timestamp                                                                    | When the HD streaming entitlement expires for the voice channel                                                                                                                              |
| hd\_streaming\_buyer\_id?                       | ?snowflake                                                                            | The ID of the user who applied the HD streaming entitlement to the voice channel                                                                                                             |
| linked\_lobby?                                  | ?[linked lobby](canales.md#linked-lobby-object) object                                | The lobby linked to the channel                                                                                                                                                              |

<sup>1</sup> The maximum user limit for stage channels is always 10000 and cannot be set to 0.

<sup>2</sup> also applies to thread creation. Users can send one message and create one thread during each interval.

<sup>3</sup> Only included in [Gateway events](https://docs.discord.food/topics/gateway-events#channels).

<sup>4</sup> This field is not automatically copied into new threads created in the channel. It should be manually set when creating a thread.

<sup>5</sup> Only returned when part of the data received in a slash command interaction or when is set to when fetching [Get Guild Channels](canales.md#get-guild-channels).

<sup>6</sup> Only included in the [Gateway Guild](https://docs.discord.food/topics/gateway-events#gateway-guild-object) object.

<sup>7</sup> Only included in [Gateway events](https://docs.discord.food/topics/gateway-events#channels) and when fetched from the [Get Private Channels](canales.md#get-private-channels) endpoint.

<sup>8</sup> For ephemeral DM channels, the channel ID will always equal the ID of the other recipient.

[**Partial Channel Structure**](canales.md#partial-channel-structure)

A channel referenced in an [invite](https://docs.discord.food/resources/invite#invite-object) or [message](https://docs.discord.food/resources/message#message-object).

| Field                    | Type                                                                                | Description                                                                    |
| ------------------------ | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| id                       | snowflake                                                                           | The ID of the channel                                                          |
| type                     | integer                                                                             | The [type of channel](canales.md#channel-type)                                 |
| name                     | ?string                                                                             | The name of the channel (1-100 characters)                                     |
| recipients? <sup>1</sup> | array\[partial [user](https://docs.discord.food/resources/user#user-object) object] | The recipients of the DM; only the `username` field is present                 |
| icon?                    | ?string                                                                             | The group DM's [icon hash](https://docs.discord.food/reference#cdn-formatting) |
| guild\_id? <sup>2</sup>  | ?snowflake                                                                          | The ID of the guild the channel is in                                          |

<sup>1</sup> Only present when the channel is fetched from an [invite](https://docs.discord.food/resources/invite#invite-object) with set to .

<sup>2</sup> Not present when the channel is fetched from an [invite](https://docs.discord.food/resources/invite#invite-object), as it can be inferred from the field.

[**Channel Nick Structure**](canales.md#channel-nick-structure)

| Field | Type      | Description              |
| ----- | --------- | ------------------------ |
| id    | snowflake | The ID of the user       |
| nick  | string    | The nickname of the user |

[**Safety Warning Structure**](canales.md#safety-warning-structure)

| Field              | Type               | Description                                           |
| ------------------ | ------------------ | ----------------------------------------------------- |
| id                 | string             | The ID of the warning                                 |
| type               | integer            | The [type of warning](canales.md#safety-warning-type) |
| expiry             | ISO8601 timestamp  | When the warning expires                              |
| dismiss\_timestamp | ?ISO8601 timestamp | When the warning was dismissed by the user            |

[**Safety Warning Type**](canales.md#safety-warning-type)

| Value | Name                                 | Description                                                                      |
| ----- | ------------------------------------ | -------------------------------------------------------------------------------- |
| 1     | STRANGER\_DANGER                     | User may not want to interact with this person                                   |
| 2     | INAPPROPRIATE\_CONVERSATION\_TIER\_1 | User may not want to interact with this person due to inappropriate conversation |
| 3     | INAPPROPRIATE\_CONVERSATION\_TIER\_2 | User may not want to interact with this person due to inappropriate conversation |
| 4     | LIKELY\_ATO                          | The recipient's account is likely compromised and should be treated with caution |

[**Channel Type**](canales.md#channel-type)

| Value | Name                | Description                                                                                                                                                        |
| ----- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 0     | GUILD\_TEXT         | A text channel within a guild                                                                                                                                      |
| 1     | DM                  | A private channel between two users                                                                                                                                |
| 2     | GUILD\_VOICE        | A voice channel within a guild                                                                                                                                     |
| 3     | GROUP\_DM           | A private channel between multiple users                                                                                                                           |
| 4     | GUILD\_CATEGORY     | An [organizational category](https://support.discord.com/hc/en-us/articles/115001580171-Channel-Categories-101) that contains up to 50 channels                    |
| 5     | GUILD\_NEWS         | Almost identical to `GUILD_TEXT`, a channel that [users can follow and crosspost into their own guild](https://support.discord.com/hc/en-us/articles/360032008192) |
| 6     | GUILD\_STORE        | A channel in which developers can showcase their SKUs                                                                                                              |
| ~~7~~ | ~~GUILD\_LFG~~      | ~~A channel where users can match up for various games~~                                                                                                           |
| ~~8~~ | ~~LFG\_GROUP\_DM~~  | ~~A private channel between multiple users for a group within an LFG channel~~                                                                                     |
| ~~9~~ | ~~THREAD\_ALPHA~~   | ~~The first iteration of the threads feature, never widely used~~                                                                                                  |
| 10    | NEWS\_THREAD        | A temporary sub-channel within a `GUILD_NEWS` channel                                                                                                              |
| 11    | PUBLIC\_THREAD      | a temporary sub-channel within a `GUILD_TEXT`, `GUILD_FORUM`, or `GUILD_MEDIA` channel                                                                             |
| 12    | PRIVATE\_THREAD     | a temporary sub-channel within a `GUILD_TEXT` channel that is only viewable by those invited and those with the `MANAGE_THREADS` permission                        |
| 13    | GUILD\_STAGE\_VOICE | A voice channel for [hosting events with an audience](https://support.discord.com/hc/en-us/articles/1500005513722) in a guild                                      |
| 14    | GUILD\_DIRECTORY    | The main channel in a [hub](https://support.discord.com/hc/en-us/articles/4406046651927-Discord-Student-Hubs-FAQ) containing the listed guilds                     |
| 15    | GUILD\_FORUM        | A channel that can only contain threads                                                                                                                            |
| 16    | GUILD\_MEDIA        | A channel that can only contain threads in a gallery view                                                                                                          |
| 17    | LOBBY               | A game lobby channel                                                                                                                                               |
| 18    | EPHEMERAL\_DM       | A private channel created by the social layer SDK                                                                                                                  |

[**Channel Flags**](canales.md#channel-flags)

| Value       | Name                                               | Description                                                                                                                                                |
| ----------- | -------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1 << 0      | GUILD\_FEED\_REMOVED                               | Guild channel is hidden from the guild's feed                                                                                                              |
| 1 << 1      | PINNED                                             | Thread is pinned to the top of its parent thread-only channel                                                                                              |
| 1 << 2      | ACTIVE\_CHANNELS\_REMOVED                          | Guild channel has been removed from the guild's active channels                                                                                            |
| 1 << 4      | REQUIRE\_TAG                                       | Thread-only channel requires a tag to create threads in                                                                                                    |
| 1 << 5      | IS\_SPAM                                           | Channel is marked as spam                                                                                                                                  |
| 1 << 7      | IS\_GUILD\_RESOURCE\_CHANNEL                       | Guild channel is used as a read-only resource for onboarding and is not shown in the channel list                                                          |
| 1 << 8      | CLYDE\_AI                                          | Channel is created by Clyde AI, which has full access to all message content                                                                               |
| 1 << 9      | IS\_SCHEDULED\_FOR\_DELETION                       | Guild channel is scheduled for deletion and is not shown in the UI                                                                                         |
| ~~1 << 10~~ | ~~IS\_MEDIA\_CHANNEL~~ <sup>1</sup>                | ~~Forum channel is a media channel~~                                                                                                                       |
| 1 << 11     | SUMMARIES\_DISABLED                                | Guild channel has summaries disabled                                                                                                                       |
| ~~1 << 12~~ | ~~APPLICATION\_SHELF\_CONSENT~~                    | ~~Private channel's recipients consented to the application shelf~~                                                                                        |
| 1 << 13     | IS\_ROLE\_SUBSCRIPTION\_TEMPLATE\_PREVIEW\_CHANNEL | Role subscription tier for this guild channel has not been published yet                                                                                   |
| 1 << 14     | IS\_BROADCASTING                                   | Group DM is used for broadcasting a live stream                                                                                                            |
| 1 << 15     | HIDE\_MEDIA\_DOWNLOAD\_OPTIONS                     | Media channel has the embedded download options hidden for media attachments                                                                               |
| 1 << 16     | IS\_JOIN\_REQUEST\_INTERVIEW\_CHANNEL              | Group DM is used for [guild join request interviews](https://support.discord.com/hc/en-us/articles/23187611406999-Guilds-FAQ#h_01HXW2MCD0BT70FB9SRV1YRPBV) |
| 1 << 17     | OBFUSCATED <sup>2</sup>                            | User does not have permission to view the channel                                                                                                          |

<sup>1</sup> Media channels are now represented by the [channel type](canales.md#channel-type).

<sup>2</sup> Obfuscated channel names and topics are always returned as .

[**Recipient Flags**](canales.md#recipient-flags)

| Value  | Name                              | Description                                                                                                    |
| ------ | --------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| 1 << 0 | DISMISSED\_IN\_GAME\_MESSAGE\_NUX | User has dismissed the [message](https://docs.discord.food/resources/message#message-type) for this DM channel |

[**Video Quality Mode**](canales.md#video-quality-mode)

| Value | Name | Description                                         |
| ----- | ---- | --------------------------------------------------- |
| 1     | AUTO | Discord chooses the quality for optimal performance |
| 2     | FULL | 720p quality                                        |

[**Forum Layout Type**](canales.md#forum-layout-type)

| Value | Name    | Description                                    |
| ----- | ------- | ---------------------------------------------- |
| 0     | DEFAULT | No layout type explicitly set                  |
| 1     | LIST    | Threads are displayed in a list                |
| 2     | GRID    | Threads are displayed in a collection of tiles |

[**Sort Order Type**](canales.md#sort-order-type)

| Value | Name             | Description                                                      |
| ----- | ---------------- | ---------------------------------------------------------------- |
| 0     | LATEST\_ACTIVITY | Sort by the most recently active threads                         |
| 1     | CREATION\_TIME   | Sort by when the thread was created (from most recent to oldest) |

[**Search Tag Setting**](canales.md#search-tag-setting)

| Value       | Description                                                        |
| ----------- | ------------------------------------------------------------------ |
| match\_some | Threads with any of the selected tags will be shown in the results |
| match\_all  | Threads with all of the selected tags will be shown in the results |

[**Example Guild Category Channel**](canales.md#example-guild-category-channel)

```
{  "id": "399942396007890945",  "type": 4,  "name": "lounge",  "position": 0,  "flags": 0,  "parent_id": null,  "guild_id": "41771983423143937",  "permission_overwrites": []}
```

[**Example Guild Text Channel**](canales.md#example-guild-text-channel)

```
{  "id": "41771983423143937",  "guild_id": "41771983423143937",  "name": "general",  "type": 0,  "position": 6,  "flags": 0,  "permission_overwrites": [],  "rate_limit_per_user": 2,  "nsfw": true,  "topic": "24/7 chat about how to gank Mike #2",  "last_message_id": "155117677105512449",  "parent_id": "399942396007890945",  "last_pin_timestamp": "2023-02-17T09:22:28+00:00",  "default_auto_archive_duration": 10080,  "default_thread_rate_limit_per_user": 0}
```

[**Example Guild News Channel**](canales.md#example-guild-news-channel)

Users can post or publish messages in this type of channel if they have the proper permissions. These are called "Announcement Channels" in the client.

```
{  "id": "41771983423143937",  "guild_id": "41771983423143937",  "name": "important-news",  "type": 5,  "position": 6,  "flags": 0,  "permission_overwrites": [],  "nsfw": true,  "topic": "Rumors about Half Life 3",  "last_message_id": "155117677105512449",  "parent_id": "399942396007890945",  "last_pin_timestamp": "2023-02-17T09:22:28+00:00",  "default_auto_archive_duration": 10080,  "default_thread_rate_limit_per_user": 0}
```

[**Example Guild Voice Channel**](canales.md#example-guild-voice-channel)

```
{  "id": "155101607195836416",  "last_message_id": "174629835082649376",  "type": 2,  "name": "ROCKET CHEESE",  "position": 5,  "flags": 0,  "parent_id": null,  "bitrate": 96000,  "user_limit": 0,  "rtc_region": null,  "guild_id": "41771983423143937",  "permission_overwrites": [],  "rate_limit_per_user": 0,  "nsfw": false}
```

[**Example Guild Stage Channel**](canales.md#example-guild-stage-channel)

```
{  "id": "1053657210082836620",  "last_message_id": "1075473541174136834",  "type": 13,  "name": "EVENTS",  "position": 2,  "flags": 0,  "parent_id": null,  "topic": "",  "bitrate": 64000,  "user_limit": 10000,  "rtc_region": null,  "guild_id": "41771983423143937",  "permission_overwrites": [],  "rate_limit_per_user": 0,  "nsfw": false}
```

[**Example Guild Forum Channel**](canales.md#example-guild-forum-channel)

```
{  "id": "1074357242700247173",  "last_message_id": "1075957063890509894",  "type": 15,  "name": "forum",  "position": 11,  "flags": 16,  "parent_id": "399942396007890945",  "topic": "",  "guild_id": "41771983423143937",  "permission_overwrites": [],  "rate_limit_per_user": 0,  "nsfw": false,  "available_tags": [    {      "id": "1076275719316983899",      "name": "Alien",      "emoji_id": null,      "emoji_name": "👽",      "moderated": true    }  ],  "default_reaction_emoji": {    "emoji_id": "1066765913208139796",    "emoji_name": null  },  "default_auto_archive_duration": 10080,  "default_thread_rate_limit_per_user": 0,  "default_sort_order": null,  "default_forum_layout": 0,  "default_tag_setting": "match_some"}
```

[**Example DM Channel**](canales.md#example-dm-channel)

```
{  "last_message_id": "3343820033257021450",  "type": 1,  "id": "319674150115610528",  "flags": 0,  "is_message_request": false,  "is_message_request_timestamp": "2023-02-16T00:45:10.270751+00:00",  "is_spam": false,  "recipients": [    {      "id": "728342296696979526",      "username": "splatter",      "avatar": "40ab813a6e1b6170dc4e7d1f2331bfeb",      "discriminator": "0",      "public_flags": 4194304,      "banner": "a_999640fa66eb908d8ec2f969516b97c8",      "accent_color": 11983775,      "global_name": "not splatter",      "avatar_decoration_data": null,      "primary_guild": null    }  ]}
```

[**Example Group DM Channel**](canales.md#example-group-dm-channel)

```
{  "name": "Some test channel",  "icon": null,  "recipients": [    {      "id": "728342296696979526",      "username": "splatter",      "avatar": "40ab813a6e1b6170dc4e7d1f2331bfeb",      "discriminator": "0",      "public_flags": 4194304,      "banner": "a_999640fa66eb908d8ec2f969516b97c8",      "accent_color": 11983775,      "global_name": "not splatter",      "avatar_decoration_data": null,      "primary_guild": null    },    {      "id": "211270674482724864",      "username": "11pixels",      "avatar": "40e250de9c74346c480e7e16da242b47",      "discriminator": "0",      "public_flags": 4194880,      "banner": "785814ab5375e10deafe9e7de256dd0e",      "accent_color": 47615,      "global_name": "12pixels",      "avatar_decoration_data": null,      "primary_guild": null    }  ],  "last_message_id": "3343820033257021450",  "type": 3,  "id": "319674150115710528",  "flags": 0,  "owner_id": "82198810841029460",  "blocked_user_warning_dismissed": true}
```

[**Example Thread Channel**](canales.md#example-thread-channel)

[Threads](https://docs.discord.food/topics/threads) can be either or . Archived threads are generally immutable. To send a message or add a reaction, a thread must first be unarchived. The API will helpfully automatically unarchive a thread when sending a message in that thread.

Unlike with channels, the API will only sync updates to users about threads the current user can view. When receiving a [Guild Create](https://docs.discord.food/topics/gateway-events#guild-create) payload, the API will only include active threads the current user can view. Threads inside of private channels are completely private to the members of that private channel. As such, when _gaining_ access to a channel in a subscribed guild the API sends a [Thread List Sync](https://docs.discord.food/topics/gateway-events#thread-list-sync), which includes all active threads in that channel.

Threads also track membership. Users must be added to a thread before sending messages in them. The API will helpfully automatically add users to a thread when sending a message in that thread.

Guilds have limits on the number of active threads and members per thread. Once these are reached additional threads cannot be created or unarchived, and users cannot be added. Threads do not count against the per-guild channel limit.

The [threads](https://docs.discord.food/topics/threads) topic has some more information.

```
{  "id": "41771983423143937",  "guild_id": "41771983423143937",  "parent_id": "41771983423143937",  "owner_id": "41771983423143937",  "name": "don't buy dota-2",  "type": 11,  "last_message_id": "155117677105512449",  "message_count": 1,  "member_count": 5,  "rate_limit_per_user": 2,  "thread_metadata": {    "archived": false,    "auto_archive_duration": 1440,    "archive_timestamp": "2021-04-12T23:40:39.855793+00:00",    "locked": false  },  "total_message_sent": 1,  "applied_tags": []}
```

[**Example Ephemeral DM Channel**](canales.md#example-ephemeral-dm-channel)

```
{  "flags": 0,  "id": "1001086404203389018",  "last_message_id": "1356768681543209051",  "recipients": [    {      "avatar": "c78ef8fb1db15a3d5f1b4c057856c5c9",      "avatar_decoration_data": null,      "discriminator": "0",      "global_name": "Dolfies",      "id": "852892297661906993",      "primary_guild": null,      "public_flags": 136,      "username": "dolfies"    },    {      "avatar": "f6c0363fbab45668fcf8f88fea56db9c",      "avatar_decoration_data": null,      "discriminator": "0",      "global_name": "Dziurwa💕",      "id": "1001086404203389018",      "primary_guild": null,      "public_flags": 4210944,      "username": ".dziurwa"    }  ],  "type": 18}
```

[**Example Partial Channel**](canales.md#example-partial-channel)

```
{  "id": "1065785999734607943",  "name": null,  "type": 3,  "icon": null,  "recipients": [    {      "username": "alien"    },    {      "username": "alien.2.electric.boogaloo"    }  ]}
```

#### [Followed Channel Object](canales.md#followed-channel-object) <a href="#followed-channel-object" id="followed-channel-object"></a>

An object that represents a channel that has been followed by a webhook.

[**Followed Channel Structure**](canales.md#followed-channel-structure)

| Field       | Type      | Description               |
| ----------- | --------- | ------------------------- |
| channel\_id | snowflake | The source channel ID     |
| webhook\_id | snowflake | Created target webhook ID |

#### [Permission Overwrite Object](canales.md#permission-overwrite-object) <a href="#permission-overwrite-object" id="permission-overwrite-object"></a>

See [permissions](https://docs.discord.food/topics/permissions#permissions) for more information about the and fields.

[**Permission Overwrite Structure**](canales.md#permission-overwrite-structure)

| Field              | Type      | Description                                                            |
| ------------------ | --------- | ---------------------------------------------------------------------- |
| id                 | snowflake | Role or user ID                                                        |
| type               | integer   | The [type of overwritten entity](canales.md#permission-overwrite-type) |
| allow <sup>1</sup> | string    | The bitwise value of all allowed permissions                           |
| deny <sup>1</sup>  | string    | The bitwise value of all disallowed permissions                        |

<sup>1</sup> When sending, these fields are optional and will default to .

[**Permission Overwrite Type**](canales.md#permission-overwrite-type)

| Value | Name   | Description                       |
| ----- | ------ | --------------------------------- |
| 0     | role   | Permissions based on a role       |
| 1     | member | Permissions for a specific member |

#### [Thread Metadata Object](canales.md#thread-metadata-object) <a href="#thread-metadata-object" id="thread-metadata-object"></a>

The thread metadata object contains a number of thread-specific channel fields that are not needed by other channel types.

[**Thread Metadata Structure**](canales.md#thread-metadata-structure)

| Field                   | Type               | Description                                                                                                  |
| ----------------------- | ------------------ | ------------------------------------------------------------------------------------------------------------ |
| archived                | boolean            | Whether the thread is archived                                                                               |
| auto\_archive\_duration | integer            | Duration in minutes to stop showing in the channel list after inactivity (one of 60, 1440, 4320, 10080)      |
| archive\_timestamp      | ISO8601 timestamp  | Timestamp when the thread's archive status was last changed, used for calculating recent activity            |
| locked                  | boolean            | Whether the thread is locked; when a thread is locked, only users with `MANAGE_THREADS` can interact with it |
| invitable?              | boolean            | Whether non-moderators can add other non-moderators to a thread; only available on private threads           |
| create\_timestamp?      | ?ISO8601 timestamp | Timestamp when the thread was created; only populated for threads created after 2022-01-09                   |

#### [Thread Member Object](canales.md#thread-member-object) <a href="#thread-member-object" id="thread-member-object"></a>

A thread member object contains information about a user that has joined a thread.

[**Thread Member Structure**](canales.md#thread-member-structure)

| Field                             | Type                                                                                        | Description                                               |
| --------------------------------- | ------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| id? <sup>1</sup>                  | snowflake                                                                                   | The ID of the thread                                      |
| user\_id? <sup>1</sup>            | snowflake                                                                                   | The ID of the user                                        |
| join\_timestamp                   | ISO8601 timestamp                                                                           | The time the current user last joined the thread          |
| flags                             | integer                                                                                     | The user's [thread flags](canales.md#thread-member-flags) |
| muted? <sup>2</sup>               | boolean                                                                                     | Whether the user has muted the thread                     |
| mute\_config? <sup>2</sup>        | ?[mute config](https://docs.discord.food/resources/user-settings#mute-config-object) object | The mute metadata for the thread                          |
| member? <sup>1</sup> <sup>3</sup> | [guild member](https://docs.discord.food/resources/guild#guild-member-object) object        | The member object for the user                            |

<sup>1</sup> These fields are omitted on the member sent within each thread in the [Guild Create](https://docs.discord.food/topics/gateway-events#guild-create) event.

<sup>2</sup> These fields are omitted for thread members other than the current user.

<sup>3</sup> The field is only present when is set to when fetching [Get Thread Members](canales.md#get-thread-members) or [Get Thread Member](canales.md#get-thread-member).

[**Thread Member Flags**](canales.md#thread-member-flags)

| Value  | Name            | Description                                                      |
| ------ | --------------- | ---------------------------------------------------------------- |
| 1 << 0 | HAS\_INTERACTED | User has interacted with the thread                              |
| 1 << 1 | ALL\_MESSAGES   | User receives notifications for all messages                     |
| 1 << 2 | ONLY\_MENTIONS  | User receives notifications only for messages that @mention them |
| 1 << 3 | NO\_MESSAGES    | User does not receive any notifications                          |

#### [Default Reaction Object](canales.md#default-reaction-object) <a href="#default-reaction-object" id="default-reaction-object"></a>

An object that specifies the emoji to use as the default way to react to a or channel post.

[**Default Reaction Structure**](canales.md#default-reaction-structure)

| Field                    | Type       | Description                        |
| ------------------------ | ---------- | ---------------------------------- |
| emoji\_id <sup>1</sup>   | ?snowflake | The ID of a guild's custom emoji   |
| emoji\_name <sup>1</sup> | ?string    | The unicode character of the emoji |

<sup>1</sup> At most one of and may be set to a non-null value.

#### [Icon Emoji Object](canales.md#icon-emoji-object) <a href="#icon-emoji-object" id="icon-emoji-object"></a>

An object that specifies the emoji to use as the icon displayed next to a channel's name.

[**Icon Emoji Structure**](canales.md#icon-emoji-structure)

| Field             | Type       | Description                        |
| ----------------- | ---------- | ---------------------------------- |
| id <sup>1</sup>   | ?snowflake | The ID of a guild's custom emoji   |
| name <sup>1</sup> | ?string    | The unicode character of the emoji |

<sup>1</sup> At most one of and may be set to a non-null value.

#### [Forum Tag Object](canales.md#forum-tag-object) <a href="#forum-tag-object" id="forum-tag-object"></a>

An object that represents a tag that is able to be applied to a thread in a or channel.

[**Forum Tag Structure**](canales.md#forum-tag-structure)

| Field                    | Type       | Description                                                                                                   |
| ------------------------ | ---------- | ------------------------------------------------------------------------------------------------------------- |
| id                       | snowflake  | The ID of the tag                                                                                             |
| name                     | string     | The name of the tag (max 50 characters)                                                                       |
| moderated                | boolean    | Whether this tag can only be added to or removed from threads by members with the `MANAGE_THREADS` permission |
| emoji\_id <sup>1</sup>   | ?snowflake | The ID of a guild's custom emoji                                                                              |
| emoji\_name <sup>1</sup> | ?string    | The unicode character of the emoji                                                                            |

<sup>1</sup> At most one of and may be set to a non-null value.

#### [Linked Lobby Object](canales.md#linked-lobby-object) <a href="#linked-lobby-object" id="linked-lobby-object"></a>

A lobby linked to a channel.

[**Linked Lobby Structure**](canales.md#linked-lobby-structure)

| Field                               | Type              | Description                                                                  |
| ----------------------------------- | ----------------- | ---------------------------------------------------------------------------- |
| application\_id                     | snowflake         | The ID of the application                                                    |
| lobby\_id                           | snowflake         | The ID of the lobby                                                          |
| linked\_by                          | snowflake         | The ID of the user who linked the lobby                                      |
| linked\_at                          | ISO8601 timestamp | When the lobby was linked to channel                                         |
| require\_application\_authorization | boolean           | Whether users must authorize the application to send messages in the channel |

### [Endpoints](canales.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get Private Channels](canales.md#get-private-channels) <a href="#get-private-channels" id="get-private-channels"></a>

`GET/users/@me/channels`

Returns a list of active [private channel](canales.md#channel-object) objects the user is participating in.

#### [Get DM Channel](canales.md#get-dm-channel) <a href="#get-dm-channel" id="get-dm-channel"></a>

`GET/users/@me/dms/{user.id}`

Returns an existing [DM channel](canales.md#channel-object) object with a user.

#### [Create Private Channel](canales.md#create-private-channel) <a href="#create-private-channel" id="create-private-channel"></a>

`POST/users/@me/channels`

One recipient creates or returns an existing [DM channel](canales.md#channel-object), none or multiple recipients create a [group DM channel](canales.md#channel-object). Returns a [private channel](canales.md#channel-object) object. Fires a [Channel Create](https://docs.discord.food/topics/gateway-events#channel-create) Gateway event.

If multiple channels with a single recipient exist, the most recent channel is returned.

[**JSON Params**](canales.md#json-params)

| Field                           | Type                 | Description                                                                |
| ------------------------------- | -------------------- | -------------------------------------------------------------------------- |
| recipient\_id? **(deprecated)** | snowflake            | The recipient to DM                                                        |
| recipients? <sup>1</sup>        | array\[snowflake]    | The users to include in the private channel                                |
| access\_tokens? <sup>2</sup>    | array\[string]       | The access tokens of users that have granted your app the `gdm.join` scope |
| nicks? <sup>3</sup>             | map\[snowflake, str] | A mapping of user IDs to their respective nicknames                        |

<sup>1</sup> When creating a group DM, the client user's ID can optionally be included in the array. This allows creating a group DM with only one other user.

<sup>2</sup> Only usable by bots for OAuth2 requests, which can only create group DMs.

<sup>3</sup> Requires to be provided.

#### [Get Guild Channels](canales.md#get-guild-channels) <a href="#get-guild-channels" id="get-guild-channels"></a>

`GET/guilds/{guild.id}/channels`

Returns a list of [guild channel](canales.md#channel-object) objects for the guild. Does not include threads. If the user is not in the guild, the guild must be discoverable.

[**Query String Params**](canales.md#query-string-params)

| Field                     | Type    | Description                                                                                    |
| ------------------------- | ------- | ---------------------------------------------------------------------------------------------- |
| permissions? <sup>1</sup> | boolean | Whether to return calculated permissions for the invoking user in each channel (default false) |

<sup>1</sup> Permissions are not returned if the user is not in the guild.

#### [Get Guild Top Read Channels](canales.md#get-guild-top-read-channels) <a href="#get-guild-top-read-channels" id="get-guild-top-read-channels"></a>

`GET/guilds/{guild.id}/top-read-channels`

Returns a list of snowflakes representing up to 10 of the top read channels in the guild. If the user is not in the guild, the guild must be discoverable.

#### [Create Guild Channel](canales.md#create-guild-channel) <a href="#create-guild-channel" id="create-guild-channel"></a>

`POST/guilds/{guild.id}/channels`

Creates a new channel in the guild. Requires the permission. If setting permission overwrites, only permissions you have in the guild can be allowed/denied. Setting permission in channels is only possible for guild administrators. Returns the new [channel](canales.md#channel-object) object on success. Fires a [Channel Create](https://docs.discord.food/topics/gateway-events#channel-create) Gateway event.

<details>

<summary>Bitrate LimitsHow is the maximum bitrate calculated?</summary>

For stage channels, the maximum bitrate is always **64 kbps**. For voice channels, the limit depends on the guild's [premium tier](https://support.discord.com/hc/en-us/articles/360028038352) and [features](https://docs.discord.food/resources/guild#guild-features).

These limits are summarized in the following table by [premium tier](https://docs.discord.food/resources/guild#premium-tier). Note that if the guild has the [feature](https://docs.discord.food/resources/guild#guild-features), the applied limit is always the tier 3 one (**384 kbps**).

| Premium Tier | Bitrate Limit |
| ------------ | ------------- |
| `NONE`       | **96 kbps**   |
| `TIER_1`     | **128 kbps**  |
| `TIER_2`     | **256 kbps**  |
| `TIER_3`     | **384 kbps**  |

</details>

[**JSON Params**](canales.md#json-params)

| Field                                    | Type                                                                           | Description                                                                                                                                                                          | Channel Type                           |
| ---------------------------------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------- |
| name                                     | string                                                                         | The name of the channel (1-100 characters)                                                                                                                                           | All                                    |
| position?                                | ?integer                                                                       | Sorting position of the channel                                                                                                                                                      | All                                    |
| type?                                    | ?integer                                                                       | The [type of channel](canales.md#channel-type) (default `GUILD_TEXT`)                                                                                                                | All                                    |
| topic?                                   | ?string                                                                        | The channel topic (max 1024 characters)                                                                                                                                              | Text, News, Stage, Forum, Media        |
| nsfw?                                    | ?boolean                                                                       | Whether the channel is NSFW                                                                                                                                                          | Text, News, Voice, Stage, Forum, Media |
| rate\_limit\_per\_user?                  | ?integer                                                                       | Duration in seconds a user has to wait before sending another message (max 21600); bots, as well as users with the permission `MANAGE_MESSAGES` or `MANAGE_CHANNELS`, are unaffected | Text, News, Voice, Stage, Forum, Media |
| bitrate? <sup>1</sup>                    | ?integer                                                                       | The bitrate (in bits) of the voice channel                                                                                                                                           | Voice, Stage                           |
| user\_limit? <sup>2</sup>                | ?integer                                                                       | The user limit of the voice channel (max 99, 0 refers to no limit)                                                                                                                   | Voice, Stage                           |
| permission\_overwrites?                  | ?array\[[permission overwrite](canales.md#permission-overwrite-object) object] | Explicit permission overwrites for members and roles                                                                                                                                 | All                                    |
| parent\_id?                              | ?snowflake                                                                     | The ID of the parent category for the guild channel                                                                                                                                  | Text, News, Voice, Stage, Forum, Media |
| rtc\_region?                             | ?string                                                                        | The [voice region](https://docs.discord.food/resources/voice#voice-region-object) ID for the voice channel (automatic when `null`)                                                   | Voice, Stage                           |
| video\_quality\_mode?                    | ?integer                                                                       | The camera [video quality mode](canales.md#video-quality-mode) of the voice channel                                                                                                  | Voice, Stage                           |
| sku\_id                                  | snowflake                                                                      | The ID of the SKU showcased by the store channel                                                                                                                                     | Store                                  |
| branch\_id?                              | ?snowflake                                                                     | The ID of the special branch granted by the store channel                                                                                                                            | Store                                  |
| default\_auto\_archive\_duration?        | ?integer                                                                       | Default duration in minutes for newly created threads to stop showing in the channel list after inactivity (one of 60, 1440, 4320, 10080)                                            | Text, News, Forum, Media               |
| default\_thread\_rate\_limit\_per\_user? | ?integer                                                                       | Default duration in seconds a user has to wait before sending another message in newly created threads; this field is copied to the thread at creation time and does not live update | Text, News, Forum, Media               |
| available\_tags? <sup>3</sup>            | ?array\[partial [forum tag](canales.md#forum-tag-object) object]               | The tags that can be used in a thread-only channel (max 20)                                                                                                                          | Forum, Media                           |
| default\_reaction\_emoji?                | ?[default reaction](canales.md#default-reaction-object) object                 | The emoji to show in the add reaction button on a thread in a thread-only channel                                                                                                    | Forum, Media                           |
| default\_forum\_layout?                  | ?integer                                                                       | The default [layout](canales.md#forum-layout-type) of a forum channel                                                                                                                | Forum                                  |
| default\_sort\_order?                    | ?integer                                                                       | The default [sort order](canales.md#sort-order-type) of a thread-only channel's threads                                                                                              | Forum, Media                           |
| default\_tag\_setting?                   | ?string                                                                        | The default [tag setting](canales.md#search-tag-setting) of a thread-only channel (default `match_some`)                                                                             | Forum, Media                           |

<sup>1</sup> For stage channels, bitrate can only be set up to **64 kbps**. See the bitrate limits section above for more information.

<sup>2</sup> The maximum user limit for stage channels is always 10000 and cannot be set to 0.

<sup>3</sup> Only the field is required.

#### [Modify Guild Channel Positions](canales.md#modify-guild-channel-positions) <a href="#modify-guild-channel-positions" id="modify-guild-channel-positions"></a>

`PATCH/guilds/{guild.id}/channels`

Modifies the positions of a set of [channel](canales.md#channel-object) objects for the guild. Requires the permission. Returns a 204 empty response on success. Fires multiple [Channel Update](https://docs.discord.food/topics/gateway-events#channel-update) Gateway events.

This endpoint takes a JSON array of parameters in the following format:

[**JSON Params**](canales.md#json-params)

| Field              | Type       | Description                                                                      |
| ------------------ | ---------- | -------------------------------------------------------------------------------- |
| id                 | snowflake  | The ID of the channel                                                            |
| position?          | ?integer   | Sorting position of the channel                                                  |
| lock\_permissions? | ?boolean   | Syncs the permission overwrites with the new parent, if moving to a new category |
| parent\_id?        | ?snowflake | The ID of the parent category for the channel                                    |

#### [Get Channel](canales.md#get-channel) <a href="#get-channel" id="get-channel"></a>

`GET/channels/{channel.id}`

Returns a [channel](canales.md#channel-object) object for a given channel ID. Requires the permission for the guild. If the channel is a thread, a [thread member](canales.md#thread-member-object) object is included in the returned result.

#### [Modify Channel](canales.md#modify-channel) <a href="#modify-channel" id="modify-channel"></a>

`PATCH/channels/{channel.id}`

Updates a channel's settings. Returns a [channel](canales.md#channel-object) on success. Fires a [Channel Update](https://docs.discord.food/topics/gateway-events#channel-update) or [Thread Update](https://docs.discord.food/topics/gateway-events#channel-update) Gateway event.

[**JSON Params**](canales.md#json-params)

If modifying a guild channel, requires the permission for the guild. If modifying permission overwrites, the permission is required. Only permissions you have in the guild or parent channel (if applicable) can be allowed/denied (unless you have a overwrite in the channel). Fires a [Channel Update](https://docs.discord.food/topics/gateway-events#channel-update) Gateway event. If modifying a category, individual [Channel Update](https://docs.discord.food/topics/gateway-events#channel-update) events will fire for each child channel that also changes.

If modifying a thread and setting to , when is also , only the permission is required. Otherwise, requires the permission. Requires the thread to have set to or be set to in the request. Fires a [Thread Update](https://docs.discord.food/topics/gateway-events#thread-update) Gateway event.

| Field                                    | Type                                                                           | Description                                                                                                                                                                          | Channel Type                                                       |
| ---------------------------------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------ |
| name?                                    | string                                                                         | The name of the channel (1-100 characters)                                                                                                                                           | Text, News, Voice, Category, Stage, Forum, Media, Thread, Group DM |
| type?                                    | integer                                                                        | The [type of channel](canales.md#channel-type); only conversion between text and news is supported and only in guilds with the "NEWS" feature                                        | Text, News                                                         |
| position?                                | ?integer                                                                       | Sorting position of the channel                                                                                                                                                      | Text, News, Voice, Category, Stage, Forum, Media                   |
| topic?                                   | ?string                                                                        | The channel topic (max 1024 characters)                                                                                                                                              | Text, News, Stage, Forum, Media                                    |
| icon?                                    | [image data](https://docs.discord.food/reference#cdn-data)                     | The group DM's icon                                                                                                                                                                  | Group DM                                                           |
| nsfw?                                    | ?boolean                                                                       | Whether the channel is NSFW                                                                                                                                                          | Text, News, Voice, Stage, Forum                                    |
| rate\_limit\_per\_user?                  | ?integer                                                                       | Duration in seconds a user has to wait before sending another message (max 21600); bots, as well as users with the permission `MANAGE_MESSAGES` or `MANAGE_CHANNELS`, are unaffected | Text, News, Voice, Stage, Forum, Media, Thread                     |
| bitrate? <sup>1</sup>                    | ?integer                                                                       | The bitrate (in bits) of the voice channel                                                                                                                                           | Voice, Stage                                                       |
| user\_limit? <sup>2</sup>                | ?integer                                                                       | the user limit of the voice channel (max 99, 0 refers to no limit)                                                                                                                   | Voice, Stage                                                       |
| permission\_overwrites?                  | ?array\[[permission overwrite](canales.md#permission-overwrite-object) object] | Explicit permission overwrites for members and roles                                                                                                                                 | Text, News, Voice, Category, Stage, Forum, Media                   |
| parent\_id?                              | ?snowflake                                                                     | The ID of the parent category for the guild channel                                                                                                                                  | Text, News, Voice, Stage, Forum, Media                             |
| owner?                                   | ?snowflake                                                                     | The ID of the owner for the group DM                                                                                                                                                 | Group DM                                                           |
| rtc\_region?                             | ?string                                                                        | The [voice region](https://docs.discord.food/resources/voice#voice-region-object) ID for the voice channel (automatic when `null`)                                                   | Voice, Stage                                                       |
| video\_quality\_mode?                    | ?integer                                                                       | The camera [video quality mode](canales.md#video-quality-mode) of the voice channel                                                                                                  | Voice, Stage                                                       |
| default\_auto\_archive\_duration?        | ?integer                                                                       | Default duration in minutes for newly created threads to stop showing in the channel list after inactivity (one of 60, 1440, 4320, 10080)                                            | Text, News, Forum, Media                                           |
| default\_thread\_rate\_limit\_per\_user? | integer                                                                        | Default duration in seconds a user has to wait before sending another message in newly created threads; this field is copied to the thread at creation time and does not live update | Text, News, Forum, Media                                           |
| auto\_archive\_duration?                 | integer                                                                        | Duration in minutes to automatically archive the thread after recent activity (one of 60, 1440, 4320, 10080)                                                                         | Thread                                                             |
| archived?                                | ?boolean                                                                       | Whether the thread is archived                                                                                                                                                       | Thread                                                             |
| locked?                                  | ?boolean                                                                       | Whether the thread is locked; when a thread is locked, only users with `MANAGE_THREADS` can unarchive it                                                                             | Thread                                                             |
| invitable?                               | ?boolean                                                                       | Whether non-moderators can add other non-moderators to a thread                                                                                                                      | Private Thread                                                     |
| flags?                                   | integer                                                                        | The [channel's flags](canales.md#channel-flags) (only `GUILD_FEED_REMOVED`, `PINNED`, `ACTIVE_CHANNELS_REMOVED`, and `REQUIRE_TAG` can be set)                                       | All                                                                |
| available\_tags? <sup>3</sup>            | array\[partial [forum tag](canales.md#forum-tag-object) object]                | The tags that can be used in a thread-only channel (max 20)                                                                                                                          | Forum, Media                                                       |
| applied\_tags?                           | array\[snowflake]                                                              | The IDs of tags that are applied to a thread in a thread-only channel (max 5)                                                                                                        | Thread                                                             |
| default\_reaction\_emoji?                | ?[default reaction](canales.md#default-reaction-object) object                 | The emoji to show in the add reaction button on a thread in a thread-only channel                                                                                                    | Forum, Media                                                       |
| default\_forum\_layout?                  | ?integer                                                                       | The default [layout](canales.md#forum-layout-type) of a forum channel                                                                                                                | Forum                                                              |
| default\_sort\_order?                    | ?integer                                                                       | The default [sort order](canales.md#sort-order-type) of a thread-only channel's threads                                                                                              | Forum, Media                                                       |
| default\_tag\_setting?                   | ?string                                                                        | The default [tag setting](canales.md#search-tag-setting) of a thread-only channel (default `match_some`)                                                                             | Forum, Media                                                       |
| icon\_emoji?                             | ?[icon emoji](canales.md#icon-emoji-object) object                             | The emoji to show next to the channel name in channels list                                                                                                                          | Text, News, Voice, Stage, Forum                                    |
| theme\_color?                            | ?integer                                                                       | The background color of the channel icon emoji encoded as an integer representation of a hexadecimal color code                                                                      | Text, News, Voice, Stage, Forum                                    |

<sup>1</sup> For stage channels, bitrate can only be set up to **64 kbps**. See the bitrate limits section in the [Create Guild Channel endpoint documentation](canales.md#create-guild-channel) for more information.

<sup>2</sup> The maximum user limit for stage channels is always 10000 and cannot be set to 0.

<sup>3</sup> Only the field is required ( may be passed to denote an existing tag).

#### [Delete Channel](canales.md#delete-channel) <a href="#delete-channel" id="delete-channel"></a>

`DELETE/channels/{channel.id}`

Deletes a channel, or closes a private message. Requires the permission for the guild, or if the channel is a thread. Deleting a category does not delete its child channels; they will have their removed and a [Channel Update](https://docs.discord.food/topics/gateway-events#channel-update) Gateway event will fire for each of them. Returns a [channel](canales.md#channel-object) object on success. Fires a [Channel Delete](https://docs.discord.food/topics/gateway-events#channel-delete) or [Thread Delete](https://docs.discord.food/topics/gateway-events#thread-delete) Gateway event.

[**Query String Params**](canales.md#query-string-params)

| Field   | Type    | Description                                                                    |
| ------- | ------- | ------------------------------------------------------------------------------ |
| silent? | boolean | Whether to leave the group DM without sending a system message (default false) |

#### [Delete Read State](canales.md#delete-read-state) <a href="#delete-read-state" id="delete-read-state"></a>

`DELETE/channels/{channel.id}/messages/ack`

Deletes a channel's read state for the current user. Returns a 204 empty response on success.

This should be used to delete various read states of channels the client has not been able to access for a while.

[**JSON Params**](canales.md#json-params)

| Field              | Type    | Description                                                                                                                              |
| ------------------ | ------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| version?           | integer | The version of the read state feature you are using (default 1, should be 2 to allow the usage of read state types other than `CHANNEL`) |
| read\_state\_type? | integer | The read state type to delete (default `CHANNEL`)                                                                                        |

#### [Modify Channel Status](canales.md#modify-channel-status) <a href="#modify-channel-status" id="modify-channel-status"></a>

`PUT/channels/{channel.id}/voice-status`

Sets a voice channel's status. Requires the permission and additionally the permission if the current user is not connected to the voice channel. Returns a 204 empty response on success. Fires a [Voice Channel Status Update](https://docs.discord.food/topics/gateway-events#voice-channel-status-update) Gateway event.

[**JSON Params**](canales.md#json-params)

| Field  | Type    | Description                                          |
| ------ | ------- | ---------------------------------------------------- |
| status | ?string | The status of the voice channel (max 500 characters) |

#### [Modify Channel Permissions](canales.md#modify-channel-permissions) <a href="#modify-channel-permissions" id="modify-channel-permissions"></a>

`PUT/channels/{channel.id}/permissions/{overwrite.id}`

Edits the channel permission overwrites for a user or role in a channel. Only usable for guild channels. Requires the permission. Only permissions you have in the guild or parent channel (if applicable) can be allowed/denied (unless you have a overwrite in the channel). Returns a 204 empty response on success. Fires a [Channel Update](https://docs.discord.food/topics/gateway-events#channel-update) Gateway event. For more information about permissions, see [permissions](https://docs.discord.food/topics/permissions#permissions).

[**JSON Params**](canales.md#json-params)

| Field  | Type    | Description                                                                                                 |
| ------ | ------- | ----------------------------------------------------------------------------------------------------------- |
| type   | integer | Either 0 (role) or 1 (member)                                                                               |
| allow? | string  | The bitwise value of all [allowed permissions](https://docs.discord.food/topics/permissions#permissions)    |
| deny?  | string  | The bitwise value of all [disallowed permissions](https://docs.discord.food/topics/permissions#permissions) |

#### [Delete Channel Permission](canales.md#delete-channel-permission) <a href="#delete-channel-permission" id="delete-channel-permission"></a>

`DELETE/channels/{channel.id}/permissions/{overwrite.id}`

Deletes a channel permission overwrite for a user or role in a channel. Only usable for guild channels. Requires the permission. Returns a 204 empty response on success. Fires a [Channel Update](https://docs.discord.food/topics/gateway-events#channel-update) Gateway event. For more information about permissions, see [permissions](https://docs.discord.food/topics/permissions#permissions)

#### [Follow Channel](canales.md#follow-channel) <a href="#follow-channel" id="follow-channel"></a>

`POST/channels/{channel.id}/followers`

Follows a News Channel to send messages to a target channel. Requires the permission in the target channel. Returns a [followed channel](canales.md#followed-channel-object) object on success. Fires a [Webhooks Update](https://docs.discord.food/topics/gateway-events#webhooks-update) Gateway event for the target channel.

[**JSON Params**](canales.md#json-params)

| Field                | Type      | Description                  |
| -------------------- | --------- | ---------------------------- |
| webhook\_channel\_id | snowflake | The ID of the target channel |

#### [Trigger Typing Indicator](canales.md#trigger-typing-indicator) <a href="#trigger-typing-indicator" id="trigger-typing-indicator"></a>

`POST/channels/{channel.id}/typing`

Posts a typing indicator for the specified channel. Returns a 204 empty response on success. Fires a [Typing Start](https://docs.discord.food/topics/gateway-events#typing-start) Gateway event.

[**Response Body**](canales.md#response-body)

| Field                         | Type   | Description                                                         |
| ----------------------------- | ------ | ------------------------------------------------------------------- |
| message\_send\_cooldown\_ms?  | number | Duration (in milliseconds) before the user can send another message |
| thread\_create\_cooldown\_ms? | number | Duration (in milliseconds) before the user can create a new thread  |

#### [Get Call Eligibility](canales.md#get-call-eligibility) <a href="#get-call-eligibility" id="get-call-eligibility"></a>

`GET/channels/{channel.id}/call`

Checks if the current user is eligible to ring a call in the DM channel.

[**Response Body**](canales.md#response-body)

| Field    | Type    | Description                                                              |
| -------- | ------- | ------------------------------------------------------------------------ |
| ringable | boolean | Whether the user is additionally eligible to ring the other recipient(s) |

#### [Modify Call](canales.md#modify-call) <a href="#modify-call" id="modify-call"></a>

`PATCH/channels/{channel.id}/call`

Modifies the active call in the private channel. Returns a 204 empty response on success. Fires a [Call Update](https://docs.discord.food/topics/gateway-events#call-update) Gateway event.

[**JSON Params**](canales.md#json-params)

| Field   | Type   | Description                                                                                       |
| ------- | ------ | ------------------------------------------------------------------------------------------------- |
| region? | string | The [voice region](https://docs.discord.food/resources/voice#voice-region-object) ID for the call |

#### [Ring Channel Recipients](canales.md#ring-channel-recipients) <a href="#ring-channel-recipients" id="ring-channel-recipients"></a>

`POST/channels/{channel.id}/call/ring`

Rings the recipients of a private channel to notify them of an active call. Returns a 204 empty response on success. Fires a [Call Update](https://docs.discord.food/topics/gateway-events#call-update) Gateway event.

[**JSON Params**](canales.md#json-params)

| Field       | Type               | Description                          |
| ----------- | ------------------ | ------------------------------------ |
| recipients? | ?array\[snowflake] | The recipients to ring (default all) |

#### [Stop Ringing Channel Recipients](canales.md#stop-ringing-channel-recipients) <a href="#stop-ringing-channel-recipients" id="stop-ringing-channel-recipients"></a>

`POST/channels/{channel.id}/call/stop-ringing`

Stops ringing the recipients of a private channel. Returns a 204 empty response on success. Fires a [Call Update](https://docs.discord.food/topics/gateway-events#call-update) Gateway event.

[**JSON Params**](canales.md#json-params)

| Field       | Type               | Description                                           |
| ----------- | ------------------ | ----------------------------------------------------- |
| recipients? | ?array\[snowflake] | The recipients to stop ringing (default current user) |

#### [Add Channel Recipient](canales.md#add-channel-recipient) <a href="#add-channel-recipient" id="add-channel-recipient"></a>

`PUT/channels/{channel.id}/recipients/{user.id}`

Adds a recipient to a private channel.

If operating on a group DM, returns a 204 empty response on success. Fires a [Channel Recipient Add](https://docs.discord.food/topics/gateway-events#channel-recipient-add) Gateway event.

If operating on a DM, returns a [group DM channel](canales.md#channel-object) object on success. Fires a [Channel Create](https://docs.discord.food/topics/gateway-events#channel-create) Gateway event.

[**JSON Params**](canales.md#json-params)

| Field                       | Type   | Description                                                           |
| --------------------------- | ------ | --------------------------------------------------------------------- |
| access\_token? <sup>1</sup> | string | Access token of a user that has granted your app the `gdm.join` scope |
| nick? <sup>2</sup>          | string | Nickname of the user being added                                      |

<sup>1</sup> Only required for OAuth2 requests.

<sup>2</sup> Not applicable when operating on a DM.

#### [Remove Channel Recipient](canales.md#remove-channel-recipient) <a href="#remove-channel-recipient" id="remove-channel-recipient"></a>

`DELETE/channels/{channel.id}/recipients/{user.id}`

Removes a recipient from a group DM. Requires ownership of the target channel. Returns a 204 empty response on success. Fires a [Channel Recipient Remove](https://docs.discord.food/topics/gateway-events#channel-recipient-remove) Gateway event.

#### [Update Message Request](canales.md#update-message-request) <a href="#update-message-request" id="update-message-request"></a>

`PUT/channels/{channel.id}/recipients/@me`

Modifies a message request's status. Returns a [DM channel](canales.md#channel-object) object on success. Fires a [Channel Update](https://docs.discord.food/topics/gateway-events#channel-update) Gateway event.

[**JSON Params**](canales.md#json-params)

| Field           | Type    | Description                                         |
| --------------- | ------- | --------------------------------------------------- |
| consent\_status | integer | The new [consent status](canales.md#consent-status) |

[**Consent Status**](canales.md#consent-status)

| Value | Name        | Description                      |
| ----- | ----------- | -------------------------------- |
| 0     | UNSPECIFIED | The DM isn't a message request   |
| 1     | PENDING     | The message request is pending   |
| 2     | ACCEPTED    | The message request was accepted |
| 3     | REJECTED    | The message request was rejected |

#### [Reject Message Request](canales.md#reject-message-request) <a href="#reject-message-request" id="reject-message-request"></a>

`DELETE/channels/{channel.id}/recipients/@me`

Rejects and deletes a pending message request. Returns a [DM channel](canales.md#channel-object) object on success. Fires [Channel Update](https://docs.discord.food/topics/gateway-events#channel-update), Message ACK, [Channel Delete](https://docs.discord.food/topics/gateway-events#channel-delete), and optionally [DM Settings Upsell Show](https://docs.discord.food/topics/gateway-events#dm-settings-upsell-show) Gateway events.

#### [Batch Reject Message Requests](canales.md#batch-reject-message-requests) <a href="#batch-reject-message-requests" id="batch-reject-message-requests"></a>

`PUT/channels/recipients/@me/batch-reject`

Rejects and deletes multiple pending message requests. Returns a list of [DM channel](canales.md#channel-object) objects on success. Fires multiple [Channel Update](https://docs.discord.food/topics/gateway-events#channel-update), Message ACK, [Channel Delete](https://docs.discord.food/topics/gateway-events#channel-delete), and optionally [DM Settings Upsell Show](https://docs.discord.food/topics/gateway-events#dm-settings-upsell-show) Gateway events.

[**JSON Params**](canales.md#json-params)

| Field        | Type              | Description                                         |
| ------------ | ----------------- | --------------------------------------------------- |
| channel\_ids | array\[snowflake] | The IDs of the message requests to reject (max 100) |

`GET/users/@me/message-requests/supplemental-data`

Returns a list of [supplemental message request](canales.md#supplemental-message-request-structure) objects with the message that triggered each message request.

[**Query String Params**](canales.md#query-string-params)

| Field        | Type              | Description                                     |
| ------------ | ----------------- | ----------------------------------------------- |
| channel\_ids | array\[snowflake] | The IDs of the message requests to fetch (1-25) |

| Field            | Type                                                                         | Description                   |
| ---------------- | ---------------------------------------------------------------------------- | ----------------------------- |
| channel\_id      | snowflake                                                                    | The ID of the message request |
| message\_preview | [message](https://docs.discord.food/resources/message#message-object) object | The trigger message           |

#### [Acknowledge Blocked User Warning](canales.md#acknowledge-blocked-user-warning) <a href="#acknowledge-blocked-user-warning" id="acknowledge-blocked-user-warning"></a>

`POST/channels/{channel.id}/blocked-user-warning-dismissal`

Acknowledges that a group DM contains users the current user has blocked. Returns a 200 empty response on success. Fires a [Channel Update](https://docs.discord.food/topics/gateway-events#channel-update) Gateway event.

#### [Acknowledge Safety Warnings](canales.md#acknowledge-safety-warnings) <a href="#acknowledge-safety-warnings" id="acknowledge-safety-warnings"></a>

`POST/channels/{channel.id}/safety-warnings/ack`

Dismisses safety warnings in a DM. Returns a 200 empty response on success. Fires a [Channel Update](https://docs.discord.food/topics/gateway-events#channel-update) Gateway event.

[**JSON Params**](canales.md#json-params)

| Field        | Type           | Description                                |
| ------------ | -------------- | ------------------------------------------ |
| warning\_ids | array\[string] | The IDs of the warnings to dismiss (1-100) |

#### [Add Safety Warning](canales.md#add-safety-warning) <a href="#add-safety-warning" id="add-safety-warning"></a>

`POST/channels/{channel.id}/add-safety-warning`

Adds a safety warning to a DM. Returns a 200 empty response on success. Fires a [Channel Update](https://docs.discord.food/topics/gateway-events#channel-update) Gateway event.

[**JSON Params**](canales.md#json-params)

| Field                 | Type    | Description                                                         |
| --------------------- | ------- | ------------------------------------------------------------------- |
| safety\_warning\_type | integer | The [type of safety warning](canales.md#safety-warning-type) to add |

#### [Delete Safety Warnings](canales.md#delete-safety-warnings) <a href="#delete-safety-warnings" id="delete-safety-warnings"></a>

`DELETE/channels/{channel.id}/safety-warnings`

Deletes all safety warnings in a DM. Returns a 200 empty response on success. Fires a [Channel Update](https://docs.discord.food/topics/gateway-events#channel-update) Gateway event.

#### [Report Safety Warning False Positive](canales.md#report-safety-warning-false-positive) <a href="#report-safety-warning-false-positive" id="report-safety-warning-false-positive"></a>

`POST/channels/{channel.id}/safety-warning/report-false-positive`

Reports all safety warnings in a DM as false positives. Returns a 200 empty response on success. Fires a [Channel Update](https://docs.discord.food/topics/gateway-events#channel-update) Gateway event.

#### [Get Guild Active Threads](canales.md#get-guild-active-threads) <a href="#get-guild-active-threads" id="get-guild-active-threads"></a>

`GET/guilds/{guild.id}/threads/active`

Returns all active threads in the guild, including public and private threads. Threads are ordered by their , in descending order.

[**Response Body**](canales.md#response-body)

| Field   | Type                                                             | Description                                                                 |
| ------- | ---------------------------------------------------------------- | --------------------------------------------------------------------------- |
| threads | array\[[channel](canales.md#channel-object) object]              | The active threads                                                          |
| members | array\[[thread members](canales.md#thread-member-object) object] | A thread member object for each returned thread the current user has joined |

#### [Get Active Threads](canales.md#get-active-threads) <a href="#get-active-threads" id="get-active-threads"></a>

`GET/channels/{channel.id}/threads/active`

Returns all active threads in the channel, including public and private threads. Threads are ordered by their , in descending order. User must be a member of the guild.

[**Response Body**](canales.md#response-body)

| Field   | Type                                                            | Description                                                                 |
| ------- | --------------------------------------------------------------- | --------------------------------------------------------------------------- |
| threads | array\[[channel](canales.md#channel-object) object]             | The active threads                                                          |
| members | array\[[thread member](canales.md#thread-member-object) object] | A thread member object for each returned thread the current user has joined |

#### [Get Public Archived Threads](canales.md#get-public-archived-threads) <a href="#get-public-archived-threads" id="get-public-archived-threads"></a>

`GET/channels/{channel.id}/threads/archived/public`

Returns archived threads in the channel that are public. When called on a channel, returns threads of [type](canales.md#channel-type) . When called on a channel returns threads of [type](canales.md#channel-type) . Threads are ordered by , in descending order. Requires the permission.

[**Query String Params**](canales.md#query-string-params)

| Field   | Type              | Description                                         |
| ------- | ----------------- | --------------------------------------------------- |
| before? | ISO8601 timestamp | Get threads before this timestamp                   |
| limit?  | integer           | Max number of threads to return (2-100, default 50) |

[**Response Body**](canales.md#response-body)

| Field     | Type                                                            | Description                                                                                  |
| --------- | --------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| threads   | array\[[channel](canales.md#channel-object) object]             | The public, archived threads                                                                 |
| members   | array\[[thread member](canales.md#thread-member-object) object] | A thread member object for each returned thread the current user has joined                  |
| has\_more | boolean                                                         | Whether there are potentially additional threads that could be returned on a subsequent call |

#### [Get Private Archived Threads](canales.md#get-private-archived-threads) <a href="#get-private-archived-threads" id="get-private-archived-threads"></a>

`GET/channels/{channel.id}/threads/archived/private`

Returns archived threads in the channel that are of [type](canales.md#channel-type) . Threads are ordered by , in descending order. Requires both the and permissions.

[**Query String Params**](canales.md#query-string-params)

| Field   | Type              | Description                                         |
| ------- | ----------------- | --------------------------------------------------- |
| before? | ISO8601 timestamp | Get threads before this timestamp                   |
| limit?  | integer           | Max number of threads to return (2-100, default 50) |

[**Response Body**](canales.md#response-body)

| Field     | Type                                                            | Description                                                                                  |
| --------- | --------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| threads   | array\[[channel](canales.md#channel-object) object]             | The private, archived threads                                                                |
| members   | array\[[thread member](canales.md#thread-member-object) object] | A thread member object for each returned thread the current user has joined                  |
| has\_more | boolean                                                         | Whether there are potentially additional threads that could be returned on a subsequent call |

#### [Get Joined Private Archived Threads](canales.md#get-joined-private-archived-threads) <a href="#get-joined-private-archived-threads" id="get-joined-private-archived-threads"></a>

`GET/channels/{channel.id}/users/@me/threads/archived/private`

Returns archived threads in the channel that are of [type](canales.md#channel-type) , and the user has joined. Threads are ordered by their , in descending order. Requires the permission.

[**Query String Params**](canales.md#query-string-params)

| Field   | Type      | Description                                         |
| ------- | --------- | --------------------------------------------------- |
| before? | snowflake | Get threads before this channel ID                  |
| limit?  | integer   | Max number of threads to return (2-100, default 50) |

[**Response Body**](canales.md#response-body)

| Field     | Type                                                            | Description                                                                                  |
| --------- | --------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| threads   | array\[[channel](canales.md#channel-object) object]             | The private, archived threads the current user has joined                                    |
| members   | array\[[thread member](canales.md#thread-member-object) object] | A thread member object for each returned thread the current user has joined                  |
| has\_more | boolean                                                         | Whether there are potentially additional threads that could be returned on a subsequent call |

#### [Search Threads](canales.md#search-threads) <a href="#search-threads" id="search-threads"></a>

`GET/channels/{channel.id}/threads/search`

Returns threads in the channel that match the search parameters. Requires the permission.

[**Query String Params**](canales.md#query-string-params)

| Field         | Type              | Description                                                                                         |
| ------------- | ----------------- | --------------------------------------------------------------------------------------------------- |
| name?         | string            | Search query to look for matching threads (max 100 characters)                                      |
| slop?         | integer           | Max number of words to skip between matching tokens in the search query (max 100, default 2)        |
| tag?          | array\[snowflake] | The tag IDs to filter results by (max 20)                                                           |
| tag\_setting? | string            | [How to restrict](canales.md#search-tag-setting) the returned threads by tag (default `match_some`) |
| archived?     | boolean           | Whether to restrict the search to only active or archived threads (default both)                    |
| sort\_by?     | string            | The [sorting algorithm](canales.md#thread-sort-type) to use                                         |
| sort\_order?  | string            | The direction to sort (`asc` or `desc`, default `desc`)                                             |
| limit?        | integer           | Max number of threads to return (1-25, default 25)                                                  |
| offset?       | integer           | Number of threads to skip before returning results (max 9975)                                       |
| max\_id?      | snowflake         | Get threads before this thread ID                                                                   |
| min\_id?      | snowflake         | Get threads after this thread ID                                                                    |

[**Thread Sort Type**](canales.md#thread-sort-type)

| Value               | Description                                           |
| ------------------- | ----------------------------------------------------- |
| last\_message\_time | Sort by the last message sent in the thread (default) |
| archive\_time       | Sort by when the thread was last archived             |
| relevance           | Sort by relevance to the current user                 |
| creation\_time      | Sort by when the thread was created                   |

[**Response Body**](canales.md#response-body)

| Field                         | Type                                                                                 | Description                                                                                  |
| ----------------------------- | ------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------- |
| threads                       | array\[[channel](canales.md#channel-object) object]                                  | The threads that match the search parameters                                                 |
| members                       | array\[[thread member](canales.md#thread-member-object) object]                      | A thread member object for each returned thread the current user has joined                  |
| has\_more                     | boolean                                                                              | Whether there are potentially additional threads that could be returned on a subsequent call |
| total\_results                | integer                                                                              | The total number of threads that match the search parameters                                 |
| first\_messages? <sup>1</sup> | array\[[message](https://docs.discord.food/resources/message#message-object) object] | The first messages of each thread                                                            |

<sup>1</sup> Only returned in thread-only channels.

#### [Create Thread from Message](canales.md#create-thread-from-message) <a href="#create-thread-from-message" id="create-thread-from-message"></a>

`POST/channels/{channel.id}/messages/{message.id}/threads`

Creates a new thread from an existing message. Returns a [channel](canales.md#channel-object) on success. Fires a [Thread Create](https://docs.discord.food/topics/gateway-events#thread-create) and a [Message Update](https://docs.discord.food/topics/gateway-events#message-update) Gateway event.

When called on a channel, creates a . When called on a channel, creates a .

[**JSON Params**](canales.md#json-params)

| Field                    | Type    | Description                                                                                                                                                                          |
| ------------------------ | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| name                     | string  | The name of the channel (1-100 characters)                                                                                                                                           |
| auto\_archive\_duration? | integer | Duration in minutes to stop showing in the channel list after inactivity (one of 60, 1440, 4320, 10080, default 4320)                                                                |
| rate\_limit\_per\_user?  | integer | Duration in seconds a user has to wait before sending another message (max 21600); bots, as well as users with the permission `MANAGE_MESSAGES` or `MANAGE_CHANNELS`, are unaffected |

#### [Create Thread](canales.md#create-thread) <a href="#create-thread" id="create-thread"></a>

`POST/channels/{channel.id}/threads`

Creates a new thread that is not connected to an existing message. Requires the or permission, depending on the type of thread being created. Returns a [channel](canales.md#channel-object) (with an optional extrakey containing the starter message) on success. Fires a [Thread Create](canales.md#channel-object) Gateway event.

In thread-only channels:

* The type of the created thread is .
* See [message formatting](https://docs.discord.food/reference#message-formatting) for more information on how to properly format messages.
* The current user must have the permission ( is ignored).
* The maximum request size when sending a message is **100 MiB**.
* For the embed object, you can set every field except (it will be regardless of if you try to set it), , , and any , , or values for images.

[**JSON Params**](canales.md#json-params)

| Field                    | Type                                                                                                 | Description                                                                                                                                                                          |
| ------------------------ | ---------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| name                     | string                                                                                               | The name of the channel (1-100 characters)                                                                                                                                           |
| auto\_archive\_duration? | integer                                                                                              | Duration in minutes to stop showing in the channel list after inactivity (one of 60, 1440, 4320, 10080, default 4320)                                                                |
| rate\_limit\_per\_user?  | integer                                                                                              | Duration in seconds a user has to wait before sending another message (max 21600); bots, as well as users with the permission `MANAGE_MESSAGES` or `MANAGE_CHANNELS`, are unaffected |
| type? <sup>1</sup>       | integer                                                                                              | the [type of thread](canales.md#channel-type) to create (default `PRIVATE_THREAD`)                                                                                                   |
| invitable?               | boolean                                                                                              | Whether non-moderators can add other non-moderators to a thread; only available when creating a private thread                                                                       |
| applied\_tags?           | array\[snowflake]                                                                                    | The IDs of the tags that are applied to a thread in a thread-only channel (max 5)                                                                                                    |
| message? <sup>1</sup>    | [thread-only channel message params](canales.md#thread-only-channel-message-params-structure) object | Contents of the first message in the thread                                                                                                                                          |

<sup>1</sup> In API v10, this will be changed to be a required field, with no default.

<sup>2</sup> Required (and only available) when creating a thread in a thread-only channel.

[**Thread-Only Channel Message Params Structure**](canales.md#thread-only-channel-message-params-structure)

| Field                       | Type                                                                                                | Description                                                                                                                                                         |
| --------------------------- | --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| content?                    | string                                                                                              | The message contents (max 2000 characters)                                                                                                                          |
| embeds? <sup>2</sup>        | array\[[embed](https://docs.discord.food/resources/message#embed-object) object]                    | Embedded `rich` content (max 6000 characters, max 10)                                                                                                               |
| allowed\_mentions?          | [allowed mention](https://docs.discord.food/resources/message#allowed-mentions-object) object       | Allowed mentions for the message                                                                                                                                    |
| components? <sup>2</sup>    | array\[[message component](https://docs.discord.food/resources/components#component-object) object] | Components to include with the message                                                                                                                              |
| sticker\_ids?               | array\[snowflake]                                                                                   | IDs of up to 3 [stickers](https://docs.discord.food/resources/sticker#sticker-object) to send in the message                                                        |
| activity?                   | [message activity](https://docs.discord.food/resources/message#message-activity-object) object      | The rich presence activity to invite users to                                                                                                                       |
| application\_id?            | snowflake                                                                                           | The application ID of the activity to create a rich presence invite for (defaults to the primary activity if unspecified)                                           |
| flags?                      | integer                                                                                             | The [message's flags](https://docs.discord.food/resources/message#message-flags) (only `SUPPRESS_EMBEDS`, `SUPPRESS_NOTIFICATIONS`, and `VOICE_MESSAGE` can be set) |
| files\[n]? <sup>1</sup>     | file contents                                                                                       | Contents of the file being sent (max 10)                                                                                                                            |
| payload\_json? <sup>1</sup> | string                                                                                              | JSON-encoded body of non-file params                                                                                                                                |
| attachments? <sup>1</sup>   | array\[partial [attachment](https://docs.discord.food/resources/message#attachment-object) object]  | Partial attachment objects with `filename` and `description` (max 10)                                                                                               |

<sup>1</sup> See [Uploading Files](https://docs.discord.food/reference#uploading-files) for details.

<sup>2</sup> Cannot be used by user accounts.

#### [Get Channel Post Data](canales.md#get-channel-post-data) <a href="#get-channel-post-data" id="get-channel-post-data"></a>

`POST/channels/{channel.id}/post-data`

Returns a mapping of thread IDs to their [post data](canales.md#thread-post-data-structure) in a thread-only channel. Requires the permission.

[**JSON Params**](canales.md#json-params)

| Field       | Type              | Description                                 |
| ----------- | ----------------- | ------------------------------------------- |
| thread\_ids | array\[snowflake] | The IDs of the threads to get post data for |

[**Response Body**](canales.md#response-body)

| Field   | Type   | Description                                                                         |
| ------- | ------ | ----------------------------------------------------------------------------------- |
| threads | object | A mapping of thread IDs to their [post data](canales.md#thread-post-data-structure) |

[**Thread Post Data Structure**](canales.md#thread-post-data-structure)

| Field          | Type                                                                                  | Description                     |
| -------------- | ------------------------------------------------------------------------------------- | ------------------------------- |
| owner          | ?[guild member](https://docs.discord.food/resources/guild#guild-member-object) object | The owner of the thread         |
| first\_message | ?[message](https://docs.discord.food/resources/message#message-object) object         | The first message in the thread |

[**Example Response**](canales.md#example-response)

```
{  "threads": {    "1075957063890509894": {      "first_message": null,      "owner": null    }  }}
```

#### [Get Thread Members](canales.md#get-thread-members) <a href="#get-thread-members" id="get-thread-members"></a>

`GET/channels/{channel.id}/thread-members`

Returns an array of [thread members](canales.md#thread-member-object) objects that are members of the thread. Requires the permission.

[**Query String Params**](canales.md#query-string-params)

| Field         | Type      | Description                                                     |
| ------------- | --------- | --------------------------------------------------------------- |
| with\_member? | boolean   | Whether to include a guild member object for each thread member |
| after?        | snowflake | Get thread members after this user ID                           |
| limit?        | integer   | Max number of thread members to return (1-100, default 100)     |

When is set to , the results will be paginated and each thread member object will include a field containing a [guild member](https://docs.discord.food/resources/guild#guild-member-object) object. Else, pagination is not available.

#### [Get Thread Member](canales.md#get-thread-member) <a href="#get-thread-member" id="get-thread-member"></a>

`GET/channels/{channel.id}/thread-members/{user.id}`

Returns a [thread member](canales.md#thread-member-object) object for the specified user if they are a member of the thread. Requires the permission.

[**Query String Params**](canales.md#query-string-params)

| Field         | Type    | Description                                                    |
| ------------- | ------- | -------------------------------------------------------------- |
| with\_member? | boolean | Whether to include a guild member object for the thread member |

#### [Join Thread](canales.md#join-thread) <a href="#join-thread" id="join-thread"></a>

`PUT/channels/{channel.id}/thread-members/@me`

Adds the current user to a thread. Requires the permission. Also requires the thread is not archived. Returns a 204 empty response on success. Fires a [Thread Members Update](https://docs.discord.food/topics/gateway-events#thread-members-update) and a [Thread Create](https://docs.discord.food/topics/gateway-events#thread-create) Gateway event.

#### [Add Thread Member](canales.md#add-thread-member) <a href="#add-thread-member" id="add-thread-member"></a>

`PUT/channels/{channel.id}/thread-members/{user.id}`

Adds another member to a thread. Requires the permission. Also requires the thread is not archived. Returns a 204 empty response on success. Fires a [Thread Members Update](https://docs.discord.food/topics/gateway-events#thread-members-update) Gateway event.

#### [Modify Thread Settings](canales.md#modify-thread-settings) <a href="#modify-thread-settings" id="modify-thread-settings"></a>

`PATCH/channels/{channel.id}/thread-members/@me/settings`

Updates the current user's thread settings. User must be a member of the thread. Returns a [thread member](canales.md#thread-member-object) on success, or a 204 empty response if nothing changed. Fires a [Thread Member Update](https://docs.discord.food/topics/gateway-events#thread-member-update) Gateway event.

[**JSON Params**](canales.md#json-params)

| Field         | Type                                                                                        | Description                                                                                        |
| ------------- | ------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| flags?        | integer                                                                                     | The user's [thread flags](canales.md#thread-member-object) flags (all except the first can be set) |
| muted?        | boolean                                                                                     | Whether the user has muted the thread                                                              |
| mute\_config? | ?[mute config](https://docs.discord.food/resources/user-settings#mute-config-object) object | The mute metadata for the thread                                                                   |

#### [Leave Thread](canales.md#leave-thread) <a href="#leave-thread" id="leave-thread"></a>

`DELETE/channels/{channel.id}/thread-members/@me`

Removes the current user from a thread. Also requires the thread is not archived. Returns a 204 empty response on success. Fires a [Thread Members Update](https://docs.discord.food/topics/gateway-events#thread-members-update) Gateway event.

#### [Remove Thread Member](canales.md#remove-thread-member) <a href="#remove-thread-member" id="remove-thread-member"></a>

`DELETE/channels/{channel.id}/thread-members/{user.id}`

Removes a member from a thread. Requires the permission, or the creator of the thread if it is a . Also requires the thread is not archived. Returns a 204 empty response on success. Fires a [Thread Members Update](https://docs.discord.food/topics/gateway-events#thread-members-update) Gateway event.

#### [Create Channel Tag](canales.md#create-channel-tag) <a href="#create-channel-tag" id="create-channel-tag"></a>

`POST/channels/{channel.id}/tags`

Creates a new tag in the thread-only channel. Requires the permission. Returns a [channel](canales.md#channel-object) object on success. Fires a [Channel Update](https://docs.discord.food/topics/gateway-events#channel-update) Gateway event.

[**JSON Params**](canales.md#json-params)

| Field                     | Type       | Description                                                                                                                   |
| ------------------------- | ---------- | ----------------------------------------------------------------------------------------------------------------------------- |
| name                      | string     | The name of the tag (max 50 characters)                                                                                       |
| moderated?                | boolean    | Whether this tag can only be added to or removed from threads by members with the `MANAGE_THREADS` permission (default false) |
| emoji\_id? <sup>1</sup>   | ?snowflake | The ID of a guild's custom emoji                                                                                              |
| emoji\_name? <sup>1</sup> | ?string    | The unicode character of the emoji                                                                                            |

<sup>1</sup> At most one of and may be set to a non-null value.

#### [Modify Channel Tag](canales.md#modify-channel-tag) <a href="#modify-channel-tag" id="modify-channel-tag"></a>

`PUT/channels/{channel.id}/tags/{tag.id}`

Replaces a tag in the thread-only channel. Requires the permission. Returns a [channel](canales.md#channel-object) object on success. Fires a [Channel Update](https://docs.discord.food/topics/gateway-events#channel-update) Gateway event.

[**JSON Params**](canales.md#json-params)

| Field                     | Type       | Description                                                                                                                   |
| ------------------------- | ---------- | ----------------------------------------------------------------------------------------------------------------------------- |
| name                      | string     | The name of the tag (max 50 characters)                                                                                       |
| moderated?                | boolean    | Whether this tag can only be added to or removed from threads by members with the `MANAGE_THREADS` permission (default false) |
| emoji\_id? <sup>1</sup>   | ?snowflake | The ID of a guild's custom emoji                                                                                              |
| emoji\_name? <sup>1</sup> | ?string    | The unicode character of the emoji                                                                                            |

<sup>1</sup> At most one of and may be set to a non-null value.

#### [Delete Channel Tag](canales.md#delete-channel-tag) <a href="#delete-channel-tag" id="delete-channel-tag"></a>

`DELETE/channels/{channel.id}/tags/{tag.id}`

Deletes a tag in the thread-only channel. Requires the permission. Returns a [channel](canales.md#channel-object) object on success. Fires a [Channel Update](https://docs.discord.food/topics/gateway-events#channel-update) Gateway event.

#### [Get Channel Linked Accounts](canales.md#get-channel-linked-accounts) <a href="#get-channel-linked-accounts" id="get-channel-linked-accounts"></a>

`GET/channels/{channel.id}/linked-accounts`

Returns the linked accounts for users in a group DM.

[**Query String Params**](canales.md#query-string-params)

| Field      | Type              | Description                         |
| ---------- | ----------------- | ----------------------------------- |
| user\_ids? | array\[snowflake] | User IDs to get linked accounts for |

[**Response Body**](canales.md#response-body)

| Field            | Type                                                                                  | Description                                  |
| ---------------- | ------------------------------------------------------------------------------------- | -------------------------------------------- |
| linked\_accounts | map\[snowflake, array\[[linked account](canales.md#linked-account-structure) object]] | The connected accounts for every linked user |

[**Linked Account Structure**](canales.md#linked-account-structure)

| Field | Type   | Description                  |
| ----- | ------ | ---------------------------- |
| id    | string | The ID of the linked account |
| name  | string | The name of the account      |

[**Example Response**](canales.md#example-response)

```
{  "linked_accounts": {    "150745989836308480": [      {        "id": "3067653496106923",        "name": "Cynosphere"      },      {        "id": "OGE2N2M2MDE4ZWY4YTM1YzI4Y2RkNmU0MDkyZGNiOWE3Y2I0YjhlZTZhNDNkYThkZjQyZjNhZjRhNGRkOGE3YQ",        "name": "Cynosphere"      }    ],    "1001086404203389018": [      {        "id": "3076033886540956",        "name": "Dziurwel14"      }    ]  }}
```

#### [Remove Lobby Link](canales.md#remove-lobby-link) <a href="#remove-lobby-link" id="remove-lobby-link"></a>

`DELETE/channels/{channel.id}/linked-lobby`

Unlinks the linked lobby from the channel. Requires the permission. Returns the updated [channel](canales.md#channel-object) object on success. Fires a [Lobby Update](https://docs.discord.food/topics/gateway-events#lobby-update) and [Channel Update](https://docs.discord.food/topics/gateway-events#channel-update) Gateway event.

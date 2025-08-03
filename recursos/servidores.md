---
icon: server
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

# Servidores

### [Guilds](servidores.md#guilds) <a href="#guilds" id="guilds"></a>

Guilds in Discord represent an isolated collection of users and channels, and are often referred to as "servers" in the UI.

#### [Guild Object](servidores.md#guild-object) <a href="#guild-object" id="guild-object"></a>

[**Guild Structure**](servidores.md#guild-structure)

| Field                                           | Type                                                                                                                | Description                                                                                                             |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| id                                              | snowflake                                                                                                           | The ID of the guild                                                                                                     |
| name                                            | string                                                                                                              | The name of the guild (2-100 characters)                                                                                |
| icon                                            | ?string                                                                                                             | The guild's [icon hash](https://docs.discord.food/reference#cdn-formatting)                                             |
| banner                                          | ?string                                                                                                             | The guild's [banner hash](https://docs.discord.food/reference#cdn-formatting)                                           |
| home\_header                                    | ?string                                                                                                             | The guild's [home header hash](https://docs.discord.food/reference#cdn-formatting), used in new member welcome          |
| splash                                          | ?string                                                                                                             | The guild's [splash hash](https://docs.discord.food/reference#cdn-formatting)                                           |
| discovery\_splash                               | ?string                                                                                                             | The guild's [discovery splash hash](https://docs.discord.food/reference#cdn-formatting)                                 |
| owner\_id                                       | snowflake                                                                                                           | The user ID of the guild's owner                                                                                        |
| application\_id                                 | ?snowflake                                                                                                          | The application ID of the guild's owner, if bot-created                                                                 |
| description                                     | ?string                                                                                                             | The description for the guild (max 300 characters)                                                                      |
| region? **(deprecated)**                        | ?string                                                                                                             | The main [voice region](https://docs.discord.food/resources/voice#voice-region-object) ID of the guild                  |
| afk\_channel\_id                                | ?snowflake                                                                                                          | The ID of the guild's AFK channel; this is where members in voice idle for longer than `afk_timeout` are moved          |
| afk\_timeout                                    | integer                                                                                                             | The AFK timeout of the guild (one of 60, 300, 900, 1800, 3600, in seconds)                                              |
| widget\_enabled?                                | boolean                                                                                                             | Whether the guild widget is enabled                                                                                     |
| widget\_channel\_id?                            | ?snowflake                                                                                                          | The channel ID that the widget will generate an invite to, if any                                                       |
| verification\_level                             | integer                                                                                                             | The [verification level](servidores.md#verification-level) required for the guild                                       |
| default\_message\_notifications                 | integer                                                                                                             | Default [message notification level](servidores.md#message-notification-level) for the guild                            |
| explicit\_content\_filter                       | integer                                                                                                             | [Whose messages](servidores.md#explicit-content-filter-level) are scanned and deleted for explicit content in the guild |
| features                                        | array\[string]                                                                                                      | Enabled [guild features](servidores.md#guild-features)                                                                  |
| roles                                           | array\[[role](servidores.md#role-object) object]                                                                    | Roles in the guild                                                                                                      |
| emojis                                          | array\[[emoji](https://docs.discord.food/resources/emoji#emoji-object) object]                                      | Custom guild emojis                                                                                                     |
| stickers                                        | array\[[sticker](https://docs.discord.food/resources/sticker#sticker-object) object]                                | Custom guild stickers                                                                                                   |
| mfa\_level                                      | integer                                                                                                             | Required [MFA level](servidores.md#mfa-level) for administrative actions within the guild                               |
| system\_channel\_id                             | ?snowflake                                                                                                          | The ID of the channel where system event messages, such as member joins and premium subscriptions (boosts), are posted  |
| system\_channel\_flags                          | integer                                                                                                             | The [flags](servidores.md#system-channel-flags) that limit system event messages                                        |
| rules\_channel\_id                              | ?snowflake                                                                                                          | The ID of the channel where community guilds display rules and/or guidelines                                            |
| public\_updates\_channel\_id                    | ?snowflake                                                                                                          | The ID of the channel where admins and moderators of community guilds receive notices from Discord                      |
| safety\_alerts\_channel\_id                     | ?snowflake                                                                                                          | The ID of the channel where admins and moderators of community guilds receive safety alerts from Discord                |
| max\_presences?                                 | ?integer                                                                                                            | The maximum number of presences for the guild (`null` is usually returned, apart from the largest of guilds)            |
| max\_members?                                   | integer                                                                                                             | The maximum number of members for the guild                                                                             |
| vanity\_url\_code                               | ?string                                                                                                             | The guild's vanity invite code                                                                                          |
| premium\_tier                                   | integer                                                                                                             | The guild's [premium tier](servidores.md#premium-tier) (boost level)                                                    |
| premium\_subscription\_count                    | integer                                                                                                             | The number of premium subscriptions (boosts) the guild currently has                                                    |
| preferred\_locale                               | string                                                                                                              | The preferred locale of the guild; used in discovery and notices from Discord (default "en-US")                         |
| max\_video\_channel\_users?                     | integer                                                                                                             | The maximum number of users in a voice channel while someone has video enabled                                          |
| max\_stage\_video\_channel\_users? <sup>1</sup> | integer                                                                                                             | The maximum number of users in a stage channel while someone has video enabled                                          |
| nsfw **(deprecated)**                           | boolean                                                                                                             | Whether the guild is considered NSFW (`EXPLICIT` or `AGE_RESTRICTED`)                                                   |
| nsfw\_level                                     | integer                                                                                                             | The [NSFW level](servidores.md#nsfw-level) of the guild                                                                 |
| hub\_type                                       | ?integer                                                                                                            | The [type of student hub](servidores.md#hub-type) the guild is, if it is a student hub                                  |
| premium\_progress\_bar\_enabled                 | boolean                                                                                                             | Whether the guild has the premium (boost) progress bar enabled                                                          |
| latest\_onboarding\_question\_id                | ?snowflake                                                                                                          | The ID of the guild's latest [onboarding prompt option](servidores.md#onboarding-prompt-option-structure)               |
| incidents\_data                                 | ?[automod incidents data](https://docs.discord.food/resources/auto-moderation#automod-incidents-data-object) object | Information on the guild's AutoMod incidents                                                                            |
| inventory\_settings **(deprecated)**            | ?[guild inventory settings](servidores.md#guild-inventory-settings-structure) object                                | Settings for emoji packs                                                                                                |
| approximate\_member\_count? <sup>2</sup>        | integer                                                                                                             | Approximate count of total members in the guild                                                                         |
| approximate\_presence\_count? <sup>2</sup>      | integer                                                                                                             | Approximate count of non-offline members in the guild                                                                   |
| premium\_features <sup>3</sup>                  | ?[guild premium features](servidores.md#guild-premium-features-structure) object                                    | The guild's powerup information                                                                                         |
| profile <sup>3</sup>                            | ?[guild identity](servidores.md#guild-identity-structure) object                                                    | The guild's identity                                                                                                    |

<sup>1</sup> This limit also applies to stream viewers within the channel. The value is always 50 for premium tiers 0 to 1, 150 for premium tier 2, and for premium tier 3.

<sup>2</sup> Only included when fetched from the [Get Guild](servidores.md#get-guild) endpoint with set to .

<sup>3</sup> Only included in guild objects returned [over the Gateway](https://docs.discord.food/topics/gateway-events#guilds), through the [Join Guild](servidores.md#join-guild) endpoint, or through [OAuth2](https://docs.discord.food/topics/oauth2#advanced-bot-authorization).

[**Partial Guild Structure**](servidores.md#partial-guild-structure)

| Field                                      | Type                                                                                 | Description                                                                                                    |
| ------------------------------------------ | ------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------- |
| id                                         | snowflake                                                                            | The ID of the guild                                                                                            |
| name                                       | string                                                                               | The name of the guild (2-100 characters)                                                                       |
| icon                                       | ?string                                                                              | The guild's [icon hash](https://docs.discord.food/reference#cdn-formatting)                                    |
| description                                | ?string                                                                              | The description for the guild (max 300 characters)                                                             |
| splash                                     | ?string                                                                              | The guild's [splash hash](https://docs.discord.food/reference#cdn-formatting)                                  |
| discovery\_splash                          | ?string                                                                              | The guild's [discovery splash hash](https://docs.discord.food/reference#cdn-formatting)                        |
| home\_header                               | ?string                                                                              | The guild's [home header hash](https://docs.discord.food/reference#cdn-formatting), used in new member welcome |
| features                                   | array\[string]                                                                       | Enabled [guild features](servidores.md#guild-features)                                                         |
| emojis? <sup>1</sup>                       | array\[[emoji](https://docs.discord.food/resources/emoji#emoji-object) object]       | Custom guild emojis                                                                                            |
| stickers? <sup>1</sup>                     | array\[[sticker](https://docs.discord.food/resources/sticker#sticker-object) object] | Custom guild stickers                                                                                          |
| approximate\_member\_count? <sup>2</sup>   | integer                                                                              | Approximate number of total members in the guild                                                               |
| approximate\_presence\_count? <sup>2</sup> | integer                                                                              | Approximate number of non-offline members in the guild                                                         |

<sup>1</sup> Only included when fetched from the [Get Guild Preview](servidores.md#get-guild-preview) endpoint.

<sup>2</sup> Not included when fetched from the [Get Guild Basic](servidores.md#get-guild-basic) or [Get Join Request Guilds](servidores.md#get-join-request-guilds) endpoints.

[**Guild Identity Structure**](servidores.md#guild-identity-structure)

| Field | Type   | Description                                                                |
| ----- | ------ | -------------------------------------------------------------------------- |
| tag   | string | The tag of the guild (2-4 characters)                                      |
| badge | string | The [guild badge hash](https://docs.discord.food/reference#cdn-formatting) |

[**Guild Inventory Settings Structure**](servidores.md#guild-inventory-settings-structure)

| Field                         | Type    | Description                                                   |
| ----------------------------- | ------- | ------------------------------------------------------------- |
| is\_emoji\_pack\_collectible? | boolean | Allows everyone to collect and use the guild's emoji globally |

[**Guild Premium Features Structure**](servidores.md#guild-premium-features-structure)

| Field                      | Type           | Description                                                             |
| -------------------------- | -------------- | ----------------------------------------------------------------------- |
| features                   | array\[string] | Enabled [powerup-specific guild features](servidores.md#guild-features) |
| additional\_emoji\_slots   | integer        | The number of additional emoji slots available to the guild             |
| additional\_sticker\_slots | integer        | The number of additional sticker slots available to the guild           |
| additional\_sound\_slots   | integer        | The number of additional soundboard slots available to the guild        |

[**Message Notification Level**](servidores.md#message-notification-level)

| Value          | Name           | Description                                                                                                                                          |
| -------------- | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0              | ALL\_MESSAGES  | Receive notifications for all messages                                                                                                               |
| 1              | ONLY\_MENTIONS | Receive notifications only for messages that @mention you                                                                                            |
| 2 <sup>1</sup> | NO\_MESSAGES   | Don't receive notifications                                                                                                                          |
| 3 <sup>1</sup> | INHERIT        | Inherit value from guild settings when in a [channel override](https://docs.discord.food/resources/user-settings#channel-override-structure) context |

<sup>1</sup> Only available inside a [user guild settings](https://docs.discord.food/resources/user-settings#user-guild-settings-object) context.

[**Explicit Content Filter Level**](servidores.md#explicit-content-filter-level)

| Value | Name                    | Description                                                 |
| ----- | ----------------------- | ----------------------------------------------------------- |
| 0     | DISABLED                | Media content will not be scanned                           |
| 1     | MEMBERS\_WITHOUT\_ROLES | Media content sent by members without roles will be scanned |
| 2     | ALL\_MEMBERS            | Media content sent by all members will be scanned           |

[**MFA Level**](servidores.md#mfa-level)

| Value | Name     | Description                                         |
| ----- | -------- | --------------------------------------------------- |
| 0     | NONE     | Guild has no MFA requirement for moderation actions |
| 1     | ELEVATED | Guild has a MFA requirement for moderation actions  |

[**Verification Level**](servidores.md#verification-level)

| Value | Name       | Description                                               |
| ----- | ---------- | --------------------------------------------------------- |
| 0     | NONE       | Unrestricted                                              |
| 1     | LOW        | Must have a verified email on file                        |
| 2     | MEDIUM     | Must be registered on Discord for longer than 5 minutes   |
| 3     | HIGH       | Must be a member of the server for longer than 10 minutes |
| 4     | VERY\_HIGH | Must have a verified phone number on file                 |

[**NSFW Level**](servidores.md#nsfw-level)

| Value | Name            | Description                                                                 |
| ----- | --------------- | --------------------------------------------------------------------------- |
| 0     | DEFAULT         | Guild is not yet rated by Discord                                           |
| 1     | EXPLICIT        | Guild has mature content only suitable for users over 18                    |
| 2     | SAFE            | Guild is safe for work                                                      |
| 3     | AGE\_RESTRICTED | Guild has mildly mature content that may not be suitable for users under 18 |

[**Premium Tier**](servidores.md#premium-tier)

| Value | Name    | Description                                   |
| ----- | ------- | --------------------------------------------- |
| 0     | NONE    | Guild has not unlocked any Server Boost perks |
| 1     | TIER\_1 | Guild has unlocked Server Boost level 1 perks |
| 2     | TIER\_2 | Guild has unlocked Server Boost level 2 perks |
| 3     | TIER\_3 | Guild has unlocked Server Boost level 3 perks |

[**System Channel Flags**](servidores.md#system-channel-flags)

| Value  | Name                                                          | Description                                                   |
| ------ | ------------------------------------------------------------- | ------------------------------------------------------------- |
| 1 << 0 | SUPPRESS\_JOIN\_NOTIFICATIONS                                 | Suppress member join notifications                            |
| 1 << 1 | SUPPRESS\_PREMIUM\_SUBSCRIPTIONS                              | Suppress premium subscription (boost) notifications           |
| 1 << 2 | SUPPRESS\_GUILD\_REMINDER\_NOTIFICATIONS                      | Suppress guild setup tips                                     |
| 1 << 3 | SUPPRESS\_JOIN\_NOTIFICATION\_REPLIES                         | Hide member join sticker reply buttons                        |
| 1 << 4 | SUPPRESS\_ROLE\_SUBSCRIPTION\_PURCHASE\_NOTIFICATIONS         | Suppress role subscription purchase and renewal notifications |
| 1 << 5 | SUPPRESS\_ROLE\_SUBSCRIPTION\_PURCHASE\_NOTIFICATION\_REPLIES | Hide role subscription sticker reply buttons                  |
| 1 << 7 | SUPPRESS\_CHANNEL\_PROMPT\_DEADCHAT                           | Suppress dead chat channel prompts                            |

[**Privacy Level**](servidores.md#privacy-level)

| Value | Name        | Description                                                        |
| ----- | ----------- | ------------------------------------------------------------------ |
| 1     | PUBLIC      | Scheduled event or stage instance is visible publicly              |
| 2     | GUILD\_ONLY | Scheduled event or stage instance is only visible to guild members |

[**Hub Type**](servidores.md#hub-type)

| Value | Name         | Description                                                                   |
| ----- | ------------ | ----------------------------------------------------------------------------- |
| 0     | DEFAULT      | Student hub is not categorized as a high school or post-secondary institution |
| 1     | HIGH\_SCHOOL | Student hub is for a high school                                              |
| 2     | COLLEGE      | Student hub is for a post-secondary institution (college or university)       |

[**Guild Features**](servidores.md#guild-features)

The available guild features, their functionality, and their requirements is subject to arbitrary change. The following table is a best-effort attempt to document the current state of guild features.

| Value                                                                | Description                                                                                                                                                                                                                                                                                                                                           |
| -------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ACTIVITIES\_ALPHA                                                    | Access to alpha embedded activities ([ release phase](https://docs.discord.food/resources/application#embedded-activity-release-phase))                                                                                                                                                                                                               |
| ACTIVITIES\_EMPLOYEE                                                 | Access to employee-released embedded activities ([ release phase](https://docs.discord.food/resources/application#embedded-activity-release-phase))                                                                                                                                                                                                   |
| ACTIVITIES\_INTERNAL\_DEV                                            | Access to internal developer embedded activities ([ release phase](https://docs.discord.food/resources/application#embedded-activity-release-phase))                                                                                                                                                                                                  |
| ACTIVITY\_FEED\_DISABLED\_BY\_USER                                   | [Member list activity feed](https://support.discord.com/hc/en-us/articles/22045487931799-Members-List-Recent-Activity-FAQ) disabled                                                                                                                                                                                                                   |
| ACTIVITY\_FEED\_ENABLED\_BY\_USER                                    | [Member list activity feed](https://support.discord.com/hc/en-us/articles/22045487931799-Members-List-Recent-Activity-FAQ) enabled                                                                                                                                                                                                                    |
| ANIMATED\_BANNER                                                     | Ability to set an animated [guild banner image](https://support.discord.com/hc/en-us/articles/360028716472-Server-Banner-Background-Invite-Splash-Image)                                                                                                                                                                                              |
| ANIMATED\_ICON                                                       | Ability to set an animated guild icon                                                                                                                                                                                                                                                                                                                 |
| ~~APPLICATION\_COMMAND\_PERMISSIONS\_V2~~                            | ~~Guild is using the~~ [~~old application commad permissions configuration behavior~~](https://discord.com/developers/docs/change-log#upcoming-application-command-permission-changes)                                                                                                                                                                |
| AUDIO\_BITRATE\_128\_KBPS                                            | Increased maximum voice channel bitrate (128 kbps)                                                                                                                                                                                                                                                                                                    |
| AUDIO\_BITRATE\_256\_KBPS                                            | Increased maximum voice channel bitrate (256 kbps)                                                                                                                                                                                                                                                                                                    |
| AUDIO\_BITRATE\_384\_KBPS                                            | Increased maximum voice channel bitrate (384 kbps)                                                                                                                                                                                                                                                                                                    |
| AUTO\_MODERATION                                                     | AutoMod feature enabled                                                                                                                                                                                                                                                                                                                               |
| ~~AUTOMOD\_TRIGGER\_KEYWORD\_FILTER~~ <sup>1</sup>                   | ~~Access to the keyword filter trigger for AutoMod~~                                                                                                                                                                                                                                                                                                  |
| ~~AUTOMOD\_TRIGGER\_ML\_SPAM\_FILTER~~ <sup>1</sup>                  | ~~Access to the machine learning-based spam filter trigger for AutoMod~~                                                                                                                                                                                                                                                                              |
| ~~AUTOMOD\_TRIGGER\_SPAM\_LINK\_FILTER~~ <sup>1</sup>                | ~~Access to the spam link filter trigger for AutoMod~~                                                                                                                                                                                                                                                                                                |
| ~~AUTOMOD\_TRIGGER\_USER\_PROFILE~~                                  | ~~Access to the user profile trigger for AutoMod~~                                                                                                                                                                                                                                                                                                    |
| BANNER                                                               | Ability to set a [guild banner image](https://support.discord.com/hc/en-us/articles/360028716472-Server-Banner-Background-Invite-Splash-Image)                                                                                                                                                                                                        |
| BFG                                                                  | Big what now 🤨                                                                                                                                                                                                                                                                                                                                       |
| ~~BOOSTING\_TIERS\_EXPERIMENT\_MEDIUM\_GUILD~~                       | ~~Guild requires a reduced amount of premium subscriptions to go up a premium tier (tier 2 - 7 boosts, tier 3 - 10 boosts)~~                                                                                                                                                                                                                          |
| ~~BOOSTING\_TIERS\_EXPERIMENT\_SMALL\_GUILD~~                        | ~~Guild requires a reduced amount of premium subscriptions to go up a premium tier (tier 2 - 3 boosts, tier 3 - 4 boosts)~~                                                                                                                                                                                                                           |
| BOT\_DEVELOPER\_EARLY\_ACCESS                                        | Access to early feature testing for bot and library developers                                                                                                                                                                                                                                                                                        |
| ~~BURST\_REACTIONS~~ <sup>1</sup>                                    | ~~Access to~~ [~~burst reactions~~](https://docs.discord.food/resources/message#reaction-object)                                                                                                                                                                                                                                                      |
| ~~CHANNEL\_BANNER~~                                                  | ~~Ability to set a channel banner~~                                                                                                                                                                                                                                                                                                                   |
| CHANNEL\_ICON\_EMOJIS\_GENERATED                                     | Guild has channel icon emojis populated                                                                                                                                                                                                                                                                                                               |
| ~~CHANNEL\_HIGHLIGHTS~~                                              | ~~Access to channel highlights~~                                                                                                                                                                                                                                                                                                                      |
| ~~CHANNEL\_HIGHLIGHTS\_DISABLED~~                                    |                                                                                                                                                                                                                                                                                                                                                       |
| ~~CLAN~~                                                             | ~~Guild is a~~ [~~clan~~](https://support.discord.com/hc/en-us/articles/23187611406999-Guilds-FAQ)                                                                                                                                                                                                                                                    |
| ~~CLAN\_DISCOVERY\_DISABLED~~                                        | ~~Clan discovery permanently disabled by Discord~~                                                                                                                                                                                                                                                                                                    |
| ~~CLAN\_PILOT\_GENSHIN~~                                             | ~~Access to clan conversion and clan discovery for Genshin Impact~~                                                                                                                                                                                                                                                                                   |
| ~~CLAN\_PILOT\_VALORANT~~                                            | ~~Access to clan conversion and clan discovery for Valorant~~                                                                                                                                                                                                                                                                                         |
| ~~CLAN\_PREPILOT\_GENSHIN~~                                          |                                                                                                                                                                                                                                                                                                                                                       |
| ~~CLAN\_PREPILOT\_VALORANT~~                                         |                                                                                                                                                                                                                                                                                                                                                       |
| ~~CLAN\_SAFETY\_REVIEW\_DISABLED~~                                   |                                                                                                                                                                                                                                                                                                                                                       |
| ~~CLYDE\_DISABLED~~                                                  | ~~Clyde AI integration opted-out~~                                                                                                                                                                                                                                                                                                                    |
| ~~CLYDE\_ENABLED~~                                                   | ~~Clyde AI integration enabled~~                                                                                                                                                                                                                                                                                                                      |
| ~~CLYDE\_EXPERIMENT\_ENABLED~~                                       | ~~Clyde AI experiment enabled~~                                                                                                                                                                                                                                                                                                                       |
| COMMERCE                                                             | Access to store channels                                                                                                                                                                                                                                                                                                                              |
| COMMUNITY                                                            | Access to [welcome screen](servidores.md#welcome-screen-object), [member verification](servidores.md#member-verification-object), [stage channels](https://docs.discord.food/resources/channel#channel-type), discovery, [community updates reception](https://support.discord.com/hc/en-us/articles/360035969312-Public-Server-Guidelines), and more |
| COMMUNITY\_CANARY                                                    | Early access to experimental community features                                                                                                                                                                                                                                                                                                       |
| COMMUNITY\_EXP\_LARGE\_GATED                                         |                                                                                                                                                                                                                                                                                                                                                       |
| COMMUNITY\_EXP\_LARGE\_UNGATED                                       |                                                                                                                                                                                                                                                                                                                                                       |
| COMMUNITY\_EXP\_MEDIUM                                               |                                                                                                                                                                                                                                                                                                                                                       |
| ~~CREATOR\_ACCEPTED\_NEW\_TERMS~~                                    | ~~Guild owner has accepted the updated monetization agreements~~                                                                                                                                                                                                                                                                                      |
| CREATOR\_MONETIZABLE                                                 | Monetization enabled                                                                                                                                                                                                                                                                                                                                  |
| CREATOR\_MONETIZABLE\_DISABLED                                       | Monetization permanently disabled by Discord                                                                                                                                                                                                                                                                                                          |
| CREATOR\_MONETIZABLE\_PENDING\_NEW\_OWNER\_ONBOARDING                | Monetization features are pending until the new guild owner completes the onboarding process                                                                                                                                                                                                                                                          |
| CREATOR\_MONETIZABLE\_PROVISIONAL                                    | Monetization enabled                                                                                                                                                                                                                                                                                                                                  |
| CREATOR\_MONETIZABLE\_RESTRICTED                                     | Guild has restrictions on monetization features                                                                                                                                                                                                                                                                                                       |
| CREATOR\_MONETIZABLE\_WHITEGLOVE                                     | Guild has a fast-tracked monetization onboarding process                                                                                                                                                                                                                                                                                              |
| CREATOR\_MONETIZATION\_APPLICATION\_ALLOWLIST                        |                                                                                                                                                                                                                                                                                                                                                       |
| CREATOR\_STORE\_PAGE                                                 | Monetization store page enabled                                                                                                                                                                                                                                                                                                                       |
| DEVELOPER\_SUPPORT\_SERVER                                           | Guild is an [application support server](https://support-dev.discord.com/hc/en-us/articles/6378525413143-App-Directory-App-profile-page)                                                                                                                                                                                                              |
| DISCOVERABLE                                                         | Guild is public and discoverable in the directory                                                                                                                                                                                                                                                                                                     |
| DISCOVERABLE\_DISABLED                                               | Discovery permanently disabled by Discord                                                                                                                                                                                                                                                                                                             |
| ENABLED\_DISCOVERABLE\_BEFORE                                        | Guild has previously been discoverable                                                                                                                                                                                                                                                                                                                |
| ENABLED\_MODERATION\_EXPERIENCE\_FOR\_NON\_COMMUNITY                 | Guild has enabled the members tab in the channel list without being a community guild                                                                                                                                                                                                                                                                 |
| ENHANCED\_ROLE\_COLORS                                               | Ability to set gradient role colors                                                                                                                                                                                                                                                                                                                   |
| EXPOSED\_TO\_ACTIVITIES\_WTP\_EXPERIMENT                             |                                                                                                                                                                                                                                                                                                                                                       |
| ~~EXPOSED\_TO\_BOOSTING\_TIERS\_EXPERIMENT~~                         | ~~Guild requires a reduced amount of premium subscriptions to go up a premium tier (see , )~~                                                                                                                                                                                                                                                         |
| ~~FEATURABLE~~                                                       | ~~Guild is featured in discovery~~                                                                                                                                                                                                                                                                                                                    |
| ~~FORCE\_RELAY~~                                                     | ~~Shards connections to the guild to different nodes that relay information between each other~~ (see `RELAY_ENABLED`)                                                                                                                                                                                                                                |
| FORWARDING\_DISABLED                                                 | Guild has disabled forwarding messages to other channels                                                                                                                                                                                                                                                                                              |
| ~~GENSHIN\_L30~~                                                     | ~~Access to clan conversion based on the intensity of Genshin Impact engagement among the guild's members, as depicted by the power user curve~~                                                                                                                                                                                                      |
| GUESTS\_ENABLED                                                      | Guild has used guest invites                                                                                                                                                                                                                                                                                                                          |
| ~~GUILD\_AUTOMOD\_DEFAULT\_LIST~~ <sup>1</sup>                       |                                                                                                                                                                                                                                                                                                                                                       |
| ~~GUILD\_COMMUNICATION\_DISABLED\_GUILDS~~ <sup>1</sup>              | ~~Access to member timeouts~~                                                                                                                                                                                                                                                                                                                         |
| ~~GUILD\_HOME\_DEPRECATION\_OVERRIDE~~                               | ~~Home tab deprecation notice hidden~~                                                                                                                                                                                                                                                                                                                |
| ~~GUILD\_HOME\_OVERRIDE~~                                            | ~~Access to the Home feature, without additionally checking for the matching user experiment~~                                                                                                                                                                                                                                                        |
| ~~GUILD\_HOME\_TEST~~                                                | ~~Access to the Home feature~~                                                                                                                                                                                                                                                                                                                        |
| ~~GUILD\_MEMBER\_VERIFICATION\_EXPERIMENT~~                          |                                                                                                                                                                                                                                                                                                                                                       |
| GUILD\_ONBOARDING                                                    | Onboarding feature enabled                                                                                                                                                                                                                                                                                                                            |
| ~~GUILD\_ONBOARDING\_ADMIN\_ONLY~~                                   | ~~Onboarding only visible to guild admins~~                                                                                                                                                                                                                                                                                                           |
| GUILD\_ONBOARDING\_EVER\_ENABLED                                     | Guild has previously enabled the onboarding feature                                                                                                                                                                                                                                                                                                   |
| GUILD\_ONBOARDING\_HAS\_PROMPTS                                      | Guild has prompts configured in onboarding                                                                                                                                                                                                                                                                                                            |
| GUILD\_PRODUCTS                                                      | Access to guild products                                                                                                                                                                                                                                                                                                                              |
| GUILD\_PRODUCTS\_ALLOW\_ARCHIVED\_FILE                               | Allows uploading archive formats as guild products                                                                                                                                                                                                                                                                                                    |
| ~~GUILD\_ROLE\_SUBSCRIPTIONS~~                                       | ~~Role subscriptions enabled~~ (see `ROLE_SUBSCRIPTIONS_ENABLED`)                                                                                                                                                                                                                                                                                     |
| ~~GUILD\_ROLE\_SUBSCRIPTION\_PURCHASE\_FEEDBACK\_LOOP~~ <sup>1</sup> | ~~Access to~~ [~~, system channel flags~~](servidores.md#system-channel-flags)                                                                                                                                                                                                                                                                        |
| ~~GUILD\_ROLE\_SUBSCRIPTION\_TIER\_TEMPLATE~~ <sup>1</sup>           | ~~Access to role subscriptions tier templates~~                                                                                                                                                                                                                                                                                                       |
| ~~GUILD\_ROLE\_SUBSCRIPTION\_TRIALS~~ <sup>1</sup>                   | ~~Access to role subscriptions trials~~                                                                                                                                                                                                                                                                                                               |
| GUILD\_SERVER\_GUIDE                                                 | New member welcome feature enabled                                                                                                                                                                                                                                                                                                                    |
| GUILD\_TAGS                                                          | Ability to set a guild tag                                                                                                                                                                                                                                                                                                                            |
| GUILD\_WEB\_PAGE\_VANITY\_URL                                        | Guild has an immutable vanity given by the server web page feature                                                                                                                                                                                                                                                                                    |
| HAD\_EARLY\_ACTIVITIES\_ACCESS                                       | Guild previously had access to embedded activities and can bypass the premium tier requirement                                                                                                                                                                                                                                                        |
| HAS\_DIRECTORY\_ENTRY                                                | Guild is listed in a directory channel                                                                                                                                                                                                                                                                                                                |
| HIDE\_FROM\_EXPERIMENT\_UI                                           |                                                                                                                                                                                                                                                                                                                                                       |
| HUB                                                                  | Guild is a [student hub](https://support.discord.com/hc/en-us/articles/4406046651927)                                                                                                                                                                                                                                                                 |
| INCREASED\_THREAD\_LIMIT                                             | Ability to have over 1,000 active threads                                                                                                                                                                                                                                                                                                             |
| INTERNAL\_EMPLOYEE\_ONLY                                             | Restricts guild joining to Discord employees                                                                                                                                                                                                                                                                                                          |
| INVITE\_SPLASH                                                       | Ability to set an invite splash background                                                                                                                                                                                                                                                                                                            |
| LEADERBOARD\_ENABLED                                                 | Guild has [game leaderboard](https://support.discord.com/hc/en-us/articles/27615657704983-Leaderboards-FAQ) enabled                                                                                                                                                                                                                                   |
| INVITES\_DISABLED                                                    | Guild has [paused invites](https://support.discord.com/hc/en-us/articles/8458903738647-Pause-Invites-FAQ), preventing new members from joining                                                                                                                                                                                                        |
| LINKED\_TO\_HUB                                                      | Guild is linked to a [student hub](https://support.discord.com/hc/en-us/articles/4406046651927)                                                                                                                                                                                                                                                       |
| ~~LURKABLE~~                                                         | ~~Ability to preview the guild before joining~~ (see `DISCOVERABLE` and `PREVIEW_ENABLED`)                                                                                                                                                                                                                                                            |
| ~~MARKETPLACES\_CONNECTION\_ROLES~~ <sup>1</sup>                     | ~~Access to guild linked roles~~                                                                                                                                                                                                                                                                                                                      |
| MAX\_FILE\_SIZE\_50\_MB                                              | Increased maximum file upload size (50 MB)                                                                                                                                                                                                                                                                                                            |
| MAX\_FILE\_SIZE\_100\_MB                                             | Increased maximum file upload size (100 MB)                                                                                                                                                                                                                                                                                                           |
| ~~MEDIA\_CHANNEL\_ALPHA~~ <sup>1</sup>                               | ~~Access to media channels~~                                                                                                                                                                                                                                                                                                                          |
| ~~MEMBER\_LIST\_DISABLED~~                                           | ~~Member list access disallowed~~                                                                                                                                                                                                                                                                                                                     |
| ~~MEMBER\_PROFILES~~                                                 | ~~Allows members to customize their per-guild profiles without Nitro~~                                                                                                                                                                                                                                                                                |
| ~~MEMBER\_SAFETY\_PAGE\_ROLLOUT~~                                    | ~~Access to member safety tab~~                                                                                                                                                                                                                                                                                                                       |
| MEMBER\_VERIFICATION\_GATE\_ENABLED                                  | [Member verification](servidores.md#member-verification-object) enabled, requiring new members to pass the verification gate before interacting with the guild                                                                                                                                                                                        |
| MEMBER\_VERIFICATION\_MANUAL\_APPROVAL                               | Membership verification manual approval enabled                                                                                                                                                                                                                                                                                                       |
| ~~MEMBER\_VERIFICATION\_ROLLOUT\_TEST~~                              | ~~Early access to member verification manual approval general availability~~                                                                                                                                                                                                                                                                          |
| ~~MOBILE\_WEB\_ROLE\_SUBSCRIPTION\_PURCHASE\_PAGE~~ <sup>1</sup>     | ~~Allows purchasing role subscriptions tiers via web version of Discord on mobile~~                                                                                                                                                                                                                                                                   |
| ~~MONETIZATION\_ENABLED~~                                            | ~~Monetization enabled~~ (see `CREATOR_MONETIZABLE`)                                                                                                                                                                                                                                                                                                  |
| MORE\_EMOJI                                                          | Increased guild emoji slots (200 each for normal and animated)                                                                                                                                                                                                                                                                                        |
| MORE\_SOUNDBOARD                                                     | Increased guild soundboard slots (96)                                                                                                                                                                                                                                                                                                                 |
| MORE\_STICKERS                                                       | Increased guild sticker slots (60)                                                                                                                                                                                                                                                                                                                    |
| NEWS                                                                 | Access to [news channels](https://support.discord.com/hc/en-us/articles/360028384531-Channel-Following-FAQ)                                                                                                                                                                                                                                           |
| ~~NEW\_THREAD\_PERMISSIONS~~ <sup>1</sup>                            | [~~New thread permissions~~](https://support.discord.com/hc/en-us/articles/4403205878423-Threads-FAQ#h_01FDGC4JW2D665Y230KPKWQZPN) ~~enabled~~                                                                                                                                                                                                        |
| NON\_COMMUNITY\_RAID\_ALERTS                                         | Non-community guild is opted-in to raid alerts                                                                                                                                                                                                                                                                                                        |
| PARTNERED                                                            | Guild is [partnered with Discord](https://discord.com/partners)                                                                                                                                                                                                                                                                                       |
| PREMIUM\_TIER\_3\_OVERRIDE                                           | All guild powerups are force-enabled regardless of premium subscription count                                                                                                                                                                                                                                                                         |
| PREVIEW\_ENABLED                                                     | Guild is accessable (read-only) without passing member verification                                                                                                                                                                                                                                                                                   |
| ~~PRIVATE\_THREADS~~ <sup>1</sup>                                    | ~~Ability to create private threads~~                                                                                                                                                                                                                                                                                                                 |
| PRODUCTS\_AVAILABLE\_FOR\_PURCHASE                                   | Guild has guild products available for purchase                                                                                                                                                                                                                                                                                                       |
| ~~PUBLIC~~                                                           | [~~Access to welcome screen, member verification, stage channels, discovery, and community updates reception~~](https://support.discord.com/hc/en-us/articles/360035969312-Public-Server-Guidelines) (see `COMMUNITY`)                                                                                                                                |
| ~~PUBLIC\_DISABLED~~                                                 | ~~Community features are permanently disabled by Discord~~                                                                                                                                                                                                                                                                                            |
| RAID\_ALERTS\_DISABLED                                               | Raid alerts opted out                                                                                                                                                                                                                                                                                                                                 |
| ~~RAID\_ALERTS\_ENABLED~~                                            | ~~Raid alerts enabled~~ (see `RAID_ALERTS_DISABLED`)                                                                                                                                                                                                                                                                                                  |
| ~~RAPIDASH\_TEST~~                                                   | ~~Access to clan conversion and clan discovery~~                                                                                                                                                                                                                                                                                                      |
| ~~RAPIDASH\_TEST\_REBIRTH~~                                          | ~~Access to clan conversion and clan discovery~~                                                                                                                                                                                                                                                                                                      |
| RELAY\_ENABLED                                                       | Shards connections to the guild to different nodes that relay information between each other                                                                                                                                                                                                                                                          |
| ~~RESTRICT\_SPAM\_RISK\_GUILDS~~ <sup>1</sup>                        | ~~Guild has additional spam risk protections enabled~~                                                                                                                                                                                                                                                                                                |
| ROLE\_ICONS                                                          | Ability to set an image or emoji as a role icon                                                                                                                                                                                                                                                                                                       |
| ROLE\_SUBSCRIPTIONS\_AVAILABLE\_FOR\_PURCHASE                        | Guild has role subscriptions available for purchase                                                                                                                                                                                                                                                                                                   |
| ROLE\_SUBSCRIPTIONS\_ENABLED                                         | Role subscriptions enabled                                                                                                                                                                                                                                                                                                                            |
| ~~ROLE\_SUBSCRIPTIONS\_ENABLED\_FOR\_PURCHASE~~                      | ~~Ability to purchase role subscriptions in the guild~~ (see `ROLE_SUBSCRIPTIONS_AVAILABLE_FOR_PURCHASE`)                                                                                                                                                                                                                                             |
| ~~SEVEN\_DAY\_THREAD\_ARCHIVE~~ <sup>1</sup>                         | ~~Ability to have threads that archive after seven days~~                                                                                                                                                                                                                                                                                             |
| ~~SHARD~~                                                            | ~~Shards~~ [~~student hub~~](https://support.discord.com/hc/en-us/articles/4406046651927) ~~UI~~                                                                                                                                                                                                                                                      |
| SHARED\_CANVAS\_FRIENDS\_AND\_FAMILY\_TEST                           | Access to the shared canvas feature                                                                                                                                                                                                                                                                                                                   |
| SOUNDBOARD                                                           | Guild has a custom [soundboard sound](https://support.discord.com/hc/en-us/articles/12612888127767-Soundboard-FAQ)                                                                                                                                                                                                                                    |
| STAGE\_CHANNEL\_VIEWERS\_50                                          | Increased maximum stage stream viewers (50)                                                                                                                                                                                                                                                                                                           |
| STAGE\_CHANNEL\_VIEWERS\_150                                         | Increased maximum stage stream viewers (150)                                                                                                                                                                                                                                                                                                          |
| STAGE\_CHANNEL\_VIEWERS\_300                                         | Increased maximum stage stream viewers (300)                                                                                                                                                                                                                                                                                                          |
| ~~SUMMARIES\_ENABLED~~                                               | ~~Access to~~ [~~conversation summaries~~](https://support.discord.com/hc/en-us/articles/12926016807575-Conversation-Summaries-AI) (see `SUMMARIES_ENABLED_GA`)                                                                                                                                                                                       |
| SUMMARIES\_ENABLED\_GA                                               | Access to [conversation summaries](https://support.discord.com/hc/en-us/articles/12926016807575-Conversation-Summaries-AI) general access                                                                                                                                                                                                             |
| SUMMARIES\_DISABLED\_BY\_USER                                        | [Conversation summaries](https://support.discord.com/hc/en-us/articles/12926016807575-Conversation-Summaries-AI) opted out                                                                                                                                                                                                                            |
| SUMMARIES\_ENABLED\_BY\_USER                                         | [Conversation summaries](https://support.discord.com/hc/en-us/articles/12926016807575-Conversation-Summaries-AI) enabled                                                                                                                                                                                                                              |
| SUMMARIES\_LONG\_LOOKBACK                                            | [Conversation summaries](https://support.discord.com/hc/en-us/articles/12926016807575-Conversation-Summaries-AI) with a longer lookback period                                                                                                                                                                                                        |
| SUMMARIES\_OPT\_OUT\_EXPERIENCE                                      | [Conversation summaries](https://support.discord.com/hc/en-us/articles/12926016807575-Conversation-Summaries-AI) enabled by default, must be opted out instead of opted in                                                                                                                                                                            |
| STAFF\_LEVEL\_COLLABORATOR\_REQUIRED                                 | Restricts guild joining to users with the [flag](https://docs.discord.food/resources/user#user-flags) or higher                                                                                                                                                                                                                                       |
| STAFF\_LEVEL\_RESTRICTED\_COLLABORATOR\_REQUIRED                     | Restricts guild joining to users with the [flag](https://docs.discord.food/resources/user#user-flags) or higher                                                                                                                                                                                                                                       |
| ~~TEXT\_IN\_STAGE\_ENABLED~~ <sup>1</sup>                            | ~~Access to text in stage (messageable stage channels)~~                                                                                                                                                                                                                                                                                              |
| ~~TEXT\_IN\_VOICE\_ENABLED~~ <sup>1</sup>                            | ~~Access to text in voice (messageable voice channels)~~                                                                                                                                                                                                                                                                                              |
| ~~THREADS\_ENABLED~~ <sup>1</sup>                                    | ~~Access to~~ [~~threads~~](https://docs.discord.food/topics/threads)                                                                                                                                                                                                                                                                                 |
| ~~THREADS\_ENABLED\_TESTING~~ <sup>1</sup>                           | ~~Access to~~ [~~threads~~](https://docs.discord.food/topics/threads) ~~prior to release, meant for bot and library developers to test their code against the new features~~                                                                                                                                                                          |
| ~~THREAD\_DEFAULT\_AUTO\_ARCHIVE\_DURATION~~                         |                                                                                                                                                                                                                                                                                                                                                       |
| ~~THREADS\_ONLY\_CHANNEL~~ <sup>1</sup>                              | ~~Access to~~ [~~forum channels~~](https://docs.discord.food/topics/threads#forums)                                                                                                                                                                                                                                                                   |
| ~~THREE\_DAY\_THREAD\_ARCHIVE~~ <sup>1</sup>                         | ~~Ability to have threads that archive after three days~~                                                                                                                                                                                                                                                                                             |
| ~~TICKETED\_EVENTS\_ENABLED~~                                        | ~~Access to ticketed~~ [~~scheduled events~~](https://docs.discord.food/resources/guild-scheduled-event)                                                                                                                                                                                                                                              |
| ~~TICKETING\_ENABLED~~                                               | ~~Access to ticketed~~ [~~scheduled events~~](https://docs.discord.food/resources/guild-scheduled-event) (see `TICKETED_EVENTS_ENABLED`)                                                                                                                                                                                                              |
| TIERLESS\_BOOSTING                                                   | Guild is using the new powerups-based boosting system                                                                                                                                                                                                                                                                                                 |
| ~~TIERLESS\_BOOSTING\_CLIENT\_TEST~~                                 | ~~Early access to guild powerup management~~                                                                                                                                                                                                                                                                                                          |
| ~~TIERLESS\_BOOSTING\_TEST~~                                         | ~~Early access to guild powerup management~~                                                                                                                                                                                                                                                                                                          |
| ~~VALORANT\_L30~~                                                    | ~~Access to clan conversion based on the intensity of Valorant engagement among the guild's members, as depicted by the power user curve~~                                                                                                                                                                                                            |
| VANITY\_URL                                                          | Ability to set a vanity URL                                                                                                                                                                                                                                                                                                                           |
| VERIFIED                                                             | Guild is [verified](https://discord.com/verification)                                                                                                                                                                                                                                                                                                 |
| VIDEO\_BITRATE\_ENHANCED                                             | Increased camera feed quality (720p)                                                                                                                                                                                                                                                                                                                  |
| VIDEO\_QUALITY\_720\_60FPS                                           | Increased maximum streaming quality (720p, 60fps)                                                                                                                                                                                                                                                                                                     |
| VIDEO\_QUALITY\_1080\_60FPS                                          | Increased maximum streaming quality (1080p, 60fps)                                                                                                                                                                                                                                                                                                    |
| VIP\_REGIONS                                                         | Increased maximum voice channel bitrate (384 kbps)                                                                                                                                                                                                                                                                                                    |
| ~~VOICE\_CHANNEL\_EFFECTS~~ <sup>1</sup>                             | ~~Access to voice channel effects~~                                                                                                                                                                                                                                                                                                                   |
| VOICE\_IN\_THREADS                                                   | Access to voice in threads (calls within threads)                                                                                                                                                                                                                                                                                                     |
| WELCOME\_SCREEN\_ENABLED                                             | Welcome screen enabled                                                                                                                                                                                                                                                                                                                                |

<sup>1</sup> This is now a base feature, and the guild feature has no effect if still present.

[**Mutable Guild Features**](servidores.md#mutable-guild-features)

These guild features are mutable, and can be edited with the [Modify Guild](servidores.md#modify-guild) endpoint.

| Feature                                              | Description                                                                                                                                                   | Required Permissions |
| ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| ACTIVITY\_FEED\_DISABLED\_BY\_USER <sup>1</sup>      | Whether the [member list activity feed](https://support.discord.com/hc/en-us/articles/22045487931799-Members-List-Recent-Activity-FAQ) is explicitly disabled | `MANAGE_GUILD`       |
| ACTIVITY\_FEED\_ENABLED\_BY\_USER <sup>1</sup>       | Whether the [member list activity feed](https://support.discord.com/hc/en-us/articles/22045487931799-Members-List-Recent-Activity-FAQ) is explicitly enabled  | `MANAGE_GUILD`       |
| COMMUNITY <sup>2</sup>                               | Whether community features are available                                                                                                                      | `ADMINISTRATOR`      |
| DISCOVERABLE <sup>3</sup>                            | Whether the guild is public and discoverable in the directory                                                                                                 | `ADMINISTRATOR`      |
| ENABLED\_MODERATION\_EXPERIENCE\_FOR\_NON\_COMMUNITY | Whether the member tab is shown in the channel list for non-community guilds                                                                                  | `MANAGE_GUILD`       |
| INVITES\_DISABLED                                    | Whether joining the guild is disabled                                                                                                                         | `MANAGE_GUILD`       |
| MEMBER\_VERIFICATION\_GATE\_ENABLED <sup>4</sup>     | Whether the member verification gate is enabled                                                                                                               | `MANAGE_GUILD`       |
| NON\_COMMUNITY\_RAID\_ALERTS                         | Whether raid alerts are opted in for non-community guilds                                                                                                     | `MANAGE_GUILD`       |
| RAID\_ALERTS\_DISABLED                               | Whether raid alerts are opted out for community guilds                                                                                                        | `MANAGE_GUILD`       |
| SUMMARIES\_ENABLED\_BY\_USER <sup>5</sup>            | Whether [conversation summaries](https://support.discord.com/hc/en-us/articles/12926016807575-Conversation-Summaries-AI) are enabled                          | `MANAGE_GUILD`       |

<sup>1</sup> Only one of the features can be set at a time.

<sup>2</sup> Modification requires the permission.

<sup>3</sup> Guild must also pass all discovery requirements in order to set.

<sup>4</sup> Feature is only removable, not settable.

<sup>5</sup> Guild must have access to conversation summaries (either the or feature).

[**Example Guild**](servidores.md#example-guild)

```
{  "id": "81384788765712384",  "name": "Discord API",  "icon": "a363a84e969bcbe1353eb2fdfb2e50e6",  "description": null,  "home_header": null,  "splash": null,  "discovery_splash": null,  "features": [    "INVITE_SPLASH",    "VIP_REGIONS",    "PREVIEW_ENABLED",    "VANITY_URL",    "CHANNEL_ICON_EMOJIS_GENERATED",    "COMMUNITY",    "NEW_THREAD_PERMISSIONS",    "WELCOME_SCREEN_ENABLED",    "NEWS",    "ANIMATED_ICON",    "AUTO_MODERATION",    "MEMBER_VERIFICATION_GATE_ENABLED",    "THREADS_ENABLED",    "COMMUNITY_EXP_LARGE_UNGATED",    "SOUNDBOARD",    "THREE_DAY_THREAD_ARCHIVE"  ],  "emojis": [],  "stickers": [],  "banner": null,  "owner_id": "80088516616269824",  "application_id": null,  "region": "deprecated",  "afk_channel_id": null,  "afk_timeout": 3600,  "system_channel_id": "381870553235193857",  "widget_enabled": true,  "widget_channel_id": null,  "verification_level": 3,  "roles": [    {      "id": "81384788765712384",      "name": "@everyone",      "description": null,      "permissions": "110917634608832",      "position": 0,      "color": 0,      "hoist": false,      "managed": false,      "mentionable": false,      "icon": null,      "unicode_emoji": null,      "flags": 0    }  ],  "default_message_notifications": 1,  "mfa_level": 1,  "explicit_content_filter": 2,  "max_presences": null,  "max_members": 500000,  "max_stage_video_channel_users": 50,  "max_video_channel_users": 25,  "vanity_url_code": "discord-api",  "premium_tier": 1,  "premium_subscription_count": 5,  "system_channel_flags": 9,  "preferred_locale": "en-US",  "rules_channel_id": "381898062269775883",  "safety_alerts_channel_id": null,  "public_updates_channel_id": "650136538264502282",  "hub_type": null,  "premium_progress_bar_enabled": false,  "latest_onboarding_question_id": null,  "incidents_data": null,  "inventory_settings": null,  "nsfw": false,  "nsfw_level": 0}
```

[**Example Partial Guild**](servidores.md#example-partial-guild)

```
{  "id": "752630786561409076",  "name": "Elite Creative",  "icon": "278da1c7740e394657c1179f4782aef1",  "description": "The largest Fortnite Creative server across the globe. Join a Creative community offering events, 1v1s. and more!",  "home_header": null,  "splash": "2b4ae5cdd71038b4880b1b57a6e5dacb",  "discovery_splash": "9d7ec672b89b320ef7a51e5b6ae453b8",  "features": [    "ANIMATED_BANNER",    "ANIMATED_ICON",    "AUTO_MODERATION",    "BANNER",    "COMMUNITY",    "DISCOVERABLE",    "ENABLED_DISCOVERABLE_BEFORE",    "GUILD_ONBOARDING_EVER_ENABLED",    "GUILD_WEB_PAGE_VANITY_URL",    "INVITE_SPLASH",    "NEWS",    "PREVIEW_ENABLED",    "RAID_ALERTS_ENABLED",    "ROLE_ICONS",    "VANITY_URL",    "WELCOME_SCREEN_ENABLED"  ],  "approximate_member_count": 155451,  "approximate_presence_count": 7532,  "emojis": [],  "stickers": []}
```

#### [User Guild Object](servidores.md#user-guild-object) <a href="#user-guild-object" id="user-guild-object"></a>

A partial guild object returned from the [Get User Guilds](servidores.md#get-user-guilds) endpoint. Represents a guild the user is a member of.

[**User Guild Structure**](servidores.md#user-guild-structure)

| Field                                      | Type           | Description                                                                   |
| ------------------------------------------ | -------------- | ----------------------------------------------------------------------------- |
| id                                         | snowflake      | The ID of the guild                                                           |
| name                                       | string         | The name of the guild (2-100 characters)                                      |
| icon                                       | ?string        | The guild's [icon hash](https://docs.discord.food/reference#cdn-formatting)   |
| banner                                     | ?string        | The guild's [banner hash](https://docs.discord.food/reference#cdn-formatting) |
| owner                                      | boolean        | Whether the user is the owner of the guild                                    |
| features                                   | array\[string] | Enabled [guild features](servidores.md#guild-features)                        |
| permissions                                | string         | Total permissions for the user in the guild (excludes overwrites)             |
| approximate\_member\_count? <sup>1</sup>   | integer        | Approximate count of total members in the guild                               |
| approximate\_presence\_count? <sup>1</sup> | integer        | Approximate count of non-offline members in the guild                         |

<sup>1</sup> Only included when fetched from the [Get User Guilds](servidores.md#get-user-guilds) endpoint with set to .

[**Example User Guild**](servidores.md#example-user-guild)

```
[  {    "id": "80351110224678913",    "name": "1337 Krew",    "icon": "8342729096ea3675442027381ff50dfe",    "banner": "bb42bdc37653b7cf58c4c8cc622e76cb",    "owner": true,    "permissions": "36953089",    "features": ["COMMUNITY", "NEWS", "ANIMATED_ICON", "INVITE_SPLASH", "BANNER", "ROLE_ICONS"],    "approximate_member_count": 420,    "approximate_presence_count": 69  }]
```

#### [Guild Widget Object](servidores.md#guild-widget-object) <a href="#guild-widget-object" id="guild-widget-object"></a>

An embeddable widget for a guild.

[**Guild Widget Structure**](servidores.md#guild-widget-structure)

| Field           | Type                                                                          | Description                                           |
| --------------- | ----------------------------------------------------------------------------- | ----------------------------------------------------- |
| id              | snowflake                                                                     | The ID of the guild the widget is for                 |
| name            | string                                                                        | The name of the guild the widget is for               |
| instant\_invite | ?string                                                                       | The invite URL for the guild's widget channel, if any |
| presence\_count | integer                                                                       | Approximate count of non-offline members in the guild |
| channels        | array\[[widget channel](servidores.md#guild-widget-channel-structure) object] | The public voice and stage channels in the guild      |
| members         | array\[[widget member](servidores.md#guild-widget-member-structure) object]   | The non-offline guild members (max 100)               |

[**Guild Widget Channel Structure**](servidores.md#guild-widget-channel-structure)

| Field    | Type      | Description                                |
| -------- | --------- | ------------------------------------------ |
| id       | snowflake | The ID of the channel                      |
| name     | string    | The name of the channel (1-100 characters) |
| position | integer   | Sorting position of the channel            |

[**Guild Widget Member Structure**](servidores.md#guild-widget-member-structure)

| Field        | Type                                                                                  | Description                                                                          |
| ------------ | ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| id           | snowflake                                                                             | The incrementing ID of the member                                                    |
| username     | string                                                                                | The display name or censored username of the member                                  |
| avatar\_url  | string                                                                                | The avatar URL of the member                                                         |
| status       | string                                                                                | The [status](https://docs.discord.food/resources/presence#status-type) of the member |
| activity?    | [widget member activity](servidores.md#guild-widget-member-activity-structure) object | The primary activity the member is participating in                                  |
| channel\_id? | snowflake                                                                             | The ID of the voice or stage channel the member is in                                |
| deaf?        | boolean                                                                               | Whether the member is deafened by the guild, if any                                  |
| mute?        | boolean                                                                               | Whether the member is muted by the guild, if any                                     |
| self\_deaf?  | boolean                                                                               | Whether the member is locally deafened                                               |
| self\_mute?  | boolean                                                                               | Whether the member is locally muted                                                  |
| suppress?    | boolean                                                                               | Whether the member's permission to speak is denied                                   |

[**Guild Widget Member Activity Structure**](servidores.md#guild-widget-member-activity-structure)

| Field | Type   | Description              |
| ----- | ------ | ------------------------ |
| name  | string | The name of the activity |

[**Example Guild Widget**](servidores.md#example-guild-widget)

```
{  "id": "1046920999469330512",  "name": "Alien Network",  "instant_invite": "https://discord.com/invite/alien",  "channels": [    {      "id": "1053657210082836620",      "name": "stage",      "position": 2    }  ],  "members": [    {      "id": "0",      "username": "Dolfies",      "discriminator": "0000",      "avatar": null,      "status": "dnd",      "activity": {        "name": "balls lmao"      },      "deaf": false,      "mute": false,      "self_deaf": false,      "self_mute": true,      "suppress": false,      "channel_id": "1057241425793798144",      "avatar_url": "https://cdn.discordapp.com/widget-avatars/zOXeOyhOGqx-bwpzszp-obU-_RFE9xI9HG19HylPyMs/actPlrz4a6Kz4NSBV4jmkd1E4raF_UjDuIXnyCjJgY5DpcbFPZnVS1SvQbRkHbXaVLvKKv7gkZi0PPXX2gdxngxHSyFH7AuxH0Y-8pgQh5xhWQaC9PsmeJ64Bv8xjF9Wb_whrDVNA-I4Eg"    }  ],  "presence_count": 69}
```

[**Guild Widget Settings Structure**](servidores.md#guild-widget-settings-structure)

| Field       | Type       | Description                                                       |
| ----------- | ---------- | ----------------------------------------------------------------- |
| enabled     | boolean    | Whether the widget is enabled                                     |
| channel\_id | ?snowflake | The channel ID that the widget will generate an invite to, if any |

[**Example Guild Widget Settings**](servidores.md#example-guild-widget-settings)

```
{  "enabled": true,  "channel_id": "41771983444115456"}
```

#### [Role Object](servidores.md#role-object) <a href="#role-object" id="role-object"></a>

Roles represent a set of permissions attached to a group of users. Roles have names, colors, and can be "pinned" to the side bar, causing their members to be listed separately. Roles can have separate permission profiles for the global context (guild) and channel context. The @everyone role has the same ID as the guild it belongs to.

[**Role Structure**](servidores.md#role-structure)

| Field                  | Type                                                      | Description                                                                                |
| ---------------------- | --------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| id                     | snowflake                                                 | The ID of the role                                                                         |
| name                   | string                                                    | The name of the role (max 100 characters)                                                  |
| description            | ?string                                                   | The description for the role (max 90 characters)                                           |
| color **(deprecated)** | integer                                                   | The color of the role represented as an integer representation of a hexadecimal color code |
| colors                 | [role colors](servidores.md#role-colors-structure) object | The colors of the role encoded as an integer representation of hexadecimal color codes     |
| hoist                  | boolean                                                   | Whether this role is pinned in the user listing                                            |
| icon?                  | ?string                                                   | The role's [icon hash](https://docs.discord.food/reference#cdn-formatting)                 |
| unicode\_emoji?        | ?string                                                   | The role's unicode emoji                                                                   |
| position               | integer                                                   | Position of this role                                                                      |
| permissions            | string                                                    | The permission bitwise value for the role                                                  |
| managed                | boolean                                                   | Whether this role is managed by an integration                                             |
| mentionable            | boolean                                                   | Whether this role is mentionable                                                           |
| flags?                 | integer                                                   | The [role's flags](servidores.md#role-flags)                                               |
| tags?                  | [role tags](servidores.md#role-tags-structure) object     | The tags this role has                                                                     |

[**Role Colors Structure**](servidores.md#role-colors-structure)

| Field                        | Type     | Description                                                     |
| ---------------------------- | -------- | --------------------------------------------------------------- |
| primary\_color               | integer  | The primary color of the role (matches `color`)                 |
| secondary\_color             | ?integer | The secondary color of the role, creating a two-point gradient  |
| tertiary\_color <sup>1</sup> | ?integer | The tertiary color of the role, creating a three-point gradient |

<sup>1</sup> The only valid three-point gradient is (, , ). Attempting to set with any other values will fail.

[**Role Tags Structure**](servidores.md#role-tags-structure)

| Field                      | Type      | Description                                                   |
| -------------------------- | --------- | ------------------------------------------------------------- |
| bot\_id?                   | snowflake | The ID of the bot this role belongs to                        |
| integration\_id?           | snowflake | The ID of the integration this role belongs to                |
| premium\_subscriber?       | null      | Whether this is the guild's premium subscriber (booster) role |
| subscription\_listing\_id? | snowflake | The ID of this role's subscription SKU and listing            |
| available\_for\_purchase?  | null      | Whether this role is available for purchase                   |
| guild\_connections?        | null      | Whether this role has a connection requirement                |

[**Role Flags**](servidores.md#role-flags)

| Value  | Name       | Description                                                                                     |
| ------ | ---------- | ----------------------------------------------------------------------------------------------- |
| 1 << 0 | IN\_PROMPT | Role is part of an [onboarding prompt option](servidores.md#onboarding-prompt-option-structure) |

[**Example Role**](servidores.md#example-role)

```
{  "id": "1050931799506817125",  "name": "Premium Members",  "description": null,  "permissions": "262144",  "position": 176,  "color": 3447003,  "colors": {    "primary_color": 3447003,    "secondary_color": null,    "tertiary_color": null  },  "hoist": false,  "managed": true,  "mentionable": false,  "icon": null,  "unicode_emoji": "👽",  "flags": 0,  "tags": {    "integration_id": "1055934248995000390"  }}
```

#### [Role Connection Configuration Object](servidores.md#role-connection-configuration-object) <a href="#role-connection-configuration-object" id="role-connection-configuration-object"></a>

Holds configuration for a role's linking requirements.

[**Role Connection Configuration Structure**](servidores.md#role-connection-configuration-structure)

This structure is represented as an array\[array\[[role connection requirement](servidores.md#role-connection-requirement-structure) object]].

The top-level array represents requirements using OR logic, while the inner arrays represent requirements using AND logic. This means that a user must satisfy at least one requirement from the top-level array, but all requirements within that array to be considered eligible for the role.

[**Role Connection Requirement Structure**](servidores.md#role-connection-requirement-structure)

| Field                                     | Type                                                                                                             | Description                                                                                               |
| ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| connection\_type <sup>1</sup>             | string                                                                                                           | The [type of connection](https://docs.discord.food/resources/connected-accounts#connection-type) required |
| connection\_metadata\_field? <sup>2</sup> | ?string                                                                                                          | The metadata field to check for the connection                                                            |
| operator?                                 | ?integer                                                                                                         | The [comparison operator](servidores.md#role-connection-operator-type) to use                             |
| value?                                    | ?string                                                                                                          | The value to compare the metadata field to                                                                |
| application\_id?                          | snowflake                                                                                                        | The ID of the application to check for the connection                                                     |
| application? <sup>3</sup>                 | [integration application](https://docs.discord.food/resources/integration#integration-application-object) object | The application to check for the connection                                                               |
| name? <sup>3</sup>                        | string                                                                                                           | The friendly name of the application's metadata field                                                     |
| description? <sup>3</sup>                 | string                                                                                                           | The description of the application's metadata field                                                       |
| result? <sup>3</sup>                      | boolean                                                                                                          | The result of the connection check                                                                        |

<sup>1</sup> A special connection type of is used to represent application role connection requirements.

<sup>2</sup> In the case of regular connections, this is checked against the provider's [connection metadata](https://docs.discord.food/resources/connected-accounts#connection-object). For application connections, this is checked against the user's [application metadata](https://docs.discord.food/resources/application#application-role-connection-object).

<sup>3</sup> Only included when fetched from the [Get Guild Role Connection Eligibility](servidores.md#get-guild-role-connection-eligibility) endpoint. Received only and cannot be set.

[**Role Connection Operator Type**](servidores.md#role-connection-operator-type)

| Value | Name                               | Description                                                                                                                            |
| ----- | ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| 1     | INTEGER\_LESS\_THAN\_OR\_EQUAL     | The metadata value (`integer`) is less than or equal to the guild's configured value (`integer`)                                       |
| 2     | INTEGER\_GREATER\_THAN\_OR\_EQUAL  | The metadata value (`integer`) is greater than or equal to the guild's configured value (`integer`)                                    |
| 3     | INTEGER\_EQUAL                     | The metadata value (`integer`) is equal to the guild's configured value (`integer`)                                                    |
| 4     | INTEGER\_NOT\_EQUAL                | The metadata value (`integer`) is not equal to the guild's configured value (`integer`)                                                |
| 5     | DATETIME\_LESS\_THAN\_OR\_EQUAL    | The metadata value (`ISO8601 string`) is less than or equal to the guild's configured value (`integer`; `days before current date`)    |
| 6     | DATETIME\_GREATER\_THAN\_OR\_EQUAL | The metadata value (`ISO8601 string`) is greater than or equal to the guild's configured value (`integer`; `days before current date`) |
| 7     | BOOLEAN\_EQUAL                     | The metadata value (`integer`) is equal to the guild's configured value (`integer`; `1`)                                               |
| 8     | BOOLEAN\_NOT\_EQUAL                | The metadata value (`integer`) is not equal to the guild's configured value (`integer`; `1`)                                           |

[**Example Role Connection Configuration**](servidores.md#example-role-connection-configuration)

```
[  [    {      "connection_type": "paypal",      "connection_metadata_field": null,      "operator": null,      "value": null    },    {      "connection_type": "paypal",      "connection_metadata_field": "verified",      "operator": 1,      "value": "1"    },    {      "connection_type": "paypal",      "connection_metadata_field": "created_at",      "operator": 4,      "value": "0"    }  ],  [    {      "connection_type": "spotify",      "connection_metadata_field": null,      "operator": null,      "value": null    }  ]]
```

#### [Guild Member Object](servidores.md#guild-member-object) <a href="#guild-member-object" id="guild-member-object"></a>

A participating user in a [guild](servidores.md#guild-object).

[**Guild Member Structure**](servidores.md#guild-member-structure)

| Field                                        | Type                                                                                                     | Description                                                                                                                                                               |
| -------------------------------------------- | -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| user <sup>1</sup>                            | partial [user](https://docs.discord.food/resources/user#user-object) object                              | The user this guild member represents                                                                                                                                     |
| nick?                                        | ?string                                                                                                  | The guild-specific nickname of the member (1-32 characters)                                                                                                               |
| avatar?                                      | ?string                                                                                                  | The member's [guild avatar hash](https://docs.discord.food/reference#cdn-formatting)                                                                                      |
| avatar\_decoration\_data?                    | ?[avatar decoration data](https://docs.discord.food/resources/user#avatar-decoration-data-object) object | The member's [guild avatar decoration](https://support.discord.com/hc/en-us/articles/13410113109911-Avatar-Decorations)                                                   |
| collectibles?                                | ?[collectibles](https://docs.discord.food/resources/user#collectibles-object) object                     | The member's equipped collectibles                                                                                                                                        |
| banner?                                      | ?string                                                                                                  | The member's [guild banner hash](https://docs.discord.food/reference#cdn-formatting)                                                                                      |
| roles                                        | array\[snowflake]                                                                                        | The role IDs assigned to this member                                                                                                                                      |
| joined\_at                                   | ISO8601 timestamp                                                                                        | When the user joined the guild                                                                                                                                            |
| premium\_since?                              | ?ISO8601 timestamp                                                                                       | When the member subscribed to (started [boosting](https://support.discord.com/hc/en-us/articles/360028038352-Server-Boosting-)) the guild                                 |
| deaf?                                        | boolean                                                                                                  | Whether the member is deafened in voice channels                                                                                                                          |
| mute?                                        | boolean                                                                                                  | Whether the member is muted in voice channels                                                                                                                             |
| pending? <sup>2</sup>                        | boolean                                                                                                  | Whether the member has not yet passed the guild's [member verification](servidores.md#member-verification-object) requirements                                            |
| communication\_disabled\_until? <sup>3</sup> | ?ISO8601 timestamp                                                                                       | When the member's [timeout](https://support.discord.com/hc/en-us/articles/4413305239191-Time-Out-FAQ) will expire and they will be able to communicate in the guild again |
| unusual\_dm\_activity\_until? <sup>3</sup>   | ?ISO8601 timestamp                                                                                       | When the member's unusual DM activity flag will expire                                                                                                                    |
| flags                                        | integer                                                                                                  | The [member's flags](servidores.md#guild-member-flags)                                                                                                                    |
| permissions? <sup>4</sup>                    | string                                                                                                   | Total permissions of the member in the channel, including overwrites                                                                                                      |

<sup>1</sup> Won't be included in the member object attached to [Message Create](https://docs.discord.food/topics/gateway-events#message-create) and [Message Update](https://docs.discord.food/topics/gateway-events#message-update) Gateway events.

<sup>2</sup> Won't be included in contexts that are impossible for a pending member to exist in. If the guild has [previewing disabled](servidores.md#guild-previewing), this field will always be .

<sup>3</sup> If the value is a time in the past, the flag has expired.

<sup>4</sup> Only returned when received in a slash command interaction.

[**Guild Member Flags**](servidores.md#guild-member-flags)

| Value      | Name                                           | Description                                                                                                                                                                                |
| ---------- | ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1 << 0     | DID\_REJOIN                                    | Guild member has left and rejoined the guild                                                                                                                                               |
| 1 << 1     | COMPLETED\_ONBOARDING                          | Guild member has completed onboarding                                                                                                                                                      |
| 1 << 2     | BYPASSES\_VERIFICATION <sup>1</sup>            | Guild member bypasses guild verification requirements and [member verification](servidores.md#member-verification-object)                                                                  |
| 1 << 3     | STARTED\_ONBOARDING                            | Guild member has started onboarding                                                                                                                                                        |
| 1 << 4     | IS\_GUEST                                      | Guild member is a [guest](https://docs.discord.food/resources/invite#guest-invites) and not a true member                                                                                  |
| 1 << 5     | STARTED\_HOME\_ACTIONS                         | Guild member has started the new member actions in the server guide                                                                                                                        |
| 1 << 6     | COMPLETED\_HOME\_ACTIONS                       | Guild member has completed all of the new member actions in the server guide                                                                                                               |
| 1 << 7     | AUTOMOD\_QUARANTINED\_NAME <sup>2</sup>        | Guild member has been indefinitely quarantined by [an AutoMod Rule](https://docs.discord.food/resources/auto-moderation#automod-rule-object) for their username, display name, or nickname |
| ~~1 << 8~~ | ~~AUTOMOD\_QUARANTINED\_BIO ~~<sup>~~2~~</sup> | ~~Guild member has been indefinitely quarantined by~~ [~~an AutoMod Rule~~](https://docs.discord.food/resources/auto-moderation#automod-rule-object) ~~for their bio~~                     |
| 1 << 9     | DM\_SETTINGS\_UPSELL\_ACKNOWLEDGED             | Guild member has acknowledged the DM privacy settings upsell modal                                                                                                                         |
| 1 << 10    | AUTOMOD\_QUARANTINED\_GUILD\_TAG <sup>2</sup>  | Guild member has been indefinitely quarantined by [an AutoMod Rule](https://docs.discord.food/resources/auto-moderation#automod-rule-object) for their primary guild tag                   |

<sup>1</sup> Allows a member who does not meet verification requirements to participate in the guild, and forces the [member's status](servidores.md#guild-member-object) to be . Removing this flag does not make the member pending again, but subjects them to the verification requirements again.

<sup>2</sup> Quarantined users, similar to timed out users, are prevented from interacting with the guild in any way.

[**Example Guild Member**](servidores.md#example-guild-member)

```
{  "avatar": null,  "banner": null,  "communication_disabled_until": null,  "unusual_dm_activity_until": null,  "flags": 34,  "joined_at": "2023-03-22T13:59:47.553000+00:00",  "nick": null,  "pending": false,  "premium_since": null,  "roles": [    "1040221495437299782",    "1029330445336313927",    "1049489484179312691",    "1053820570367701012",    "1029317826755956827",    "1029316630431412287"  ],  "user": {    "id": "828387742575624222",    "username": "jupppper",    "avatar": "e14a7c62b0b38068be88be194b23910f",    "discriminator": "0",    "public_flags": 16384,    "banner": "e45c9b5799fcb46b82bd5f1afc1b30c4",    "global_name": "Jup",    "accent_color": 1,    "avatar_decoration_data": null,    "primary_guild": null  },  "mute": false,  "deaf": false}
```

Additional information about a participating user's join source in a [guild](servidores.md#guild-object).

| Field                                       | Type                                                     | Description                                                             |
| ------------------------------------------- | -------------------------------------------------------- | ----------------------------------------------------------------------- |
| user\_id? <sup>1</sup>                      | snowflake                                                | The ID of the user this guild member represents                         |
| member? <sup>2</sup>                        | [guild member](servidores.md#guild-member-object) object | The associated guild member                                             |
| join\_source\_type?                         | integer                                                  | [How the user joined the guild](servidores.md#join-source-type)         |
| source\_invite\_code?                       | ?string                                                  | The invite code or vanity used to join the guild, if applicable         |
| inviter\_id?                                | ?snowflake                                               | The ID of the user who invited the user to the guild, if applicable     |
| integration\_type?                          | ?integer                                                 | The type of integration that added the user to the guild, if applicable |
| join\_source\_application\_id? <sup>2</sup> | ?snowflake                                               | The ID of the application that owns the linked lobby                    |
| join\_source\_channel\_id? <sup>2</sup>     | ?snowflake                                               | The ID of the channel the lobby is linked to                            |

<sup>1</sup> is only included when fetched from the [Get Guild Members Supplemental](servidores.md#get-guild-members-supplemental) endpoint.

<sup>2</sup> Only included when fetched from the [Search Guild Members](servidores.md#search-guild-members) endpoint.

[**Join Source Type**](servidores.md#join-source-type)

| Value | Name                                        | Description                                                                                       |
| ----- | ------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| 0     | UNSPECIFIED                                 | The user joined the guild through an unknown source                                               |
| 1     | BOT                                         | The user was added to the guild by a bot using the [OAuth2 scope](servidores.md#add-guild-member) |
| 2     | INTEGRATION                                 | The user was added to the guild by an integration (e.g. Twitch)                                   |
| 3     | DISCOVERY                                   | The user joined the guild through guild discovery                                                 |
| 4     | HUB                                         | The user joined the guild through a student hub                                                   |
| 5     | INVITE                                      | The user joined the guild through an invite                                                       |
| 6     | VANITY\_URL                                 | The user joined the guild through a vanity URL                                                    |
| 7     | MANUAL\_MEMBER\_VERIFICATION                | The user was accepted into the guild after applying for membership                                |
| 8     | SOCIAL\_LAYER\_INTEGRATION\_LINKED\_CHANNEL | The user joined the guild through a linked lobby                                                  |

```
{  "user_id": "257496590401536000",  "source_invite_code": "41i3n5",  "join_source_type": 5,  "inviter_id": "1001086404203389018",  "integration_type": null}
```

#### [Ban Object](servidores.md#ban-object) <a href="#ban-object" id="ban-object"></a>

A ban for a [guild](servidores.md#guild-object). Banned users can't rejoin a guild unless [unbanned](servidores.md#delete-guild-ban).

[**Ban Structure**](servidores.md#ban-structure)

| Field  | Type                                                                        | Description            |
| ------ | --------------------------------------------------------------------------- | ---------------------- |
| user   | partial [user](https://docs.discord.food/resources/user#user-object) object | The banned user        |
| reason | ?string                                                                     | The reason for the ban |

[**Example Ban**](servidores.md#example-ban)

```
{  "user": {    "id": "53908232506183680",    "username": "mason",    "avatar": "a_d5efa99b3eeaa7dd43acca82f5692432",    "discriminator": "0",    "public_flags": 4325445,    "banner": "42db4e3be824706cb1304fba05995722",    "accent_color": null,    "global_name": "Mason",    "avatar_decoration_data": null,    "primary_guild": null  },  "reason": "mentioning b1nzy"}
```

#### [Welcome Screen Object](servidores.md#welcome-screen-object) <a href="#welcome-screen-object" id="welcome-screen-object"></a>

[**Welcome Screen Structure**](servidores.md#welcome-screen-structure)

| Field             | Type                                                                                    | Description                                                          |
| ----------------- | --------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| description       | ?string                                                                                 | The welcome message shown in the welcome screen (max 140 characters) |
| welcome\_channels | array\[[welcome screen channel](servidores.md#welcome-screen-channel-structure) object] | The channels shown in the welcome screen (max 5)                     |

[**Welcome Screen Channel Structure**](servidores.md#welcome-screen-channel-structure)

| Field       | Type       | Description                                                                                    |
| ----------- | ---------- | ---------------------------------------------------------------------------------------------- |
| channel\_id | snowflake  | The ID of the channel                                                                          |
| description | string     | The description shown for the channel (1-50 characters)                                        |
| emoji\_id   | ?snowflake | The [emoji ID](https://docs.discord.food/resources/emoji#emoji-object), if the emoji is custom |
| emoji\_name | ?string    | The emoji name if custom, the unicode character if standard, or `null` if no emoji is set      |

[**Example Welcome Screen**](servidores.md#example-welcome-screen)

```
{  "description": "Discord Developers is a place to learn about Discord's API, bots, and SDKs and integrations. This is NOT a general Discord support server.",  "welcome_channels": [    {      "channel_id": "697138785317814292",      "description": "Follow for official Discord API updates",      "emoji_id": null,      "emoji_name": "📡"    },    {      "channel_id": "697236247739105340",      "description": "Get help with Bot Verifications",      "emoji_id": null,      "emoji_name": "📸"    },    {      "channel_id": "697489244649816084",      "description": "Create amazing things with Discord's API",      "emoji_id": null,      "emoji_name": "🔬"    },    {      "channel_id": "613425918748131338",      "description": "Integrate Discord into your game",      "emoji_id": null,      "emoji_name": "🎮"    },    {      "channel_id": "646517734150242346",      "description": "Find more places to help you on your quest",      "emoji_id": null,      "emoji_name": "🔦"    }  ]}
```

#### [Member Verification Object](servidores.md#member-verification-object) <a href="#member-verification-object" id="member-verification-object"></a>

In guilds with [member verification](https://support.discord.com/hc/en-us/articles/1500000466882) enabled, when a member joins, a [Guild Member Add](https://docs.discord.food/topics/gateway-events#guild-member-add) Gateway event will be dispatched but they will initially be restricted from doing any actions in the guild, and will be true in the [guild member](servidores.md#guild-member-object) object. When the member completes the verification, a [Guild Member Update](https://docs.discord.food/topics/gateway-events#guild-member-update) Gateway event will be dispatched and will be false.

To represent a member's progress towards becoming a full member of the guild, a [Guild Join Request Create](https://docs.discord.food/topics/gateway-events#guild-join-request-create) Gateway event will be dispatched. Once the join request is approved, the member's status will be false and they will be able to interact with the guild. Unless using [manual approval](servidores.md#manual-approval), the join request will be automatically approved when the user [submits their join request](servidores.md#create-guild-join-request).

Assigning a member the [flag](servidores.md#guild-member-flags) will force their status to be false and approve their current join request regardless of the join request's status. Note that bot users will always bypass member verification and have their status set to false.

[**Guild Previewing**](servidores.md#guild-previewing)

Discoverable guilds, guilds with member verification enabled, and guilds in a directory entry have thefeature by default. This allows the guild to be lurkable by users who are not members, if discoverable, and allows members who have not passed the verification gate to view the guild without interacting with it.

If a guild has member verification enabled and is not previewable, new joiners will not be able to view the guild until they pass the verification gate, and are therefore not considered members until they do. **This means members do not join the guild until they pass the verification gate.** A [Guild Member Add](https://docs.discord.food/topics/gateway-events#guild-member-add) Gateway event will not be dispatched until the member has passed the verification gate, and will always be false in the [guild member](servidores.md#guild-member-object) object for new joiners. Join requests will continue to be dispatched as normal, but there will not be an associated guild member until the join request is approved.

From the joiner's perspective, they will also not receive a [Guild Create](https://docs.discord.food/topics/gateway-events#guild-create) Gateway event or any other events dispatched from the guild until they pass member verification. Non-previewable pending guilds can only be retrieved with the [Get Join Request Guilds](servidores.md#get-join-request-guilds) endpoint. Their join requests are also sent in the [field of the Ready event](https://docs.discord.food/topics/gateway-events#ready).

Note that the preview disabled paradigm only applies if a guild meets one of the requirements for previewing in the first place.

[**Manual Approval**](servidores.md#manual-approval)

To use [form field types](servidores.md#member-verification-form-field-type) other than , the guild must have the [guild feature](servidores.md#guild-features) enabled. When using these form field types, join requests must be [manually approved](servidores.md#action-guild-join-request) after they are [submitted](servidores.md#create-guild-join-request) before the user can complete member verification.

When only using only [form field types](servidores.md#member-verification-form-field-type), the join request will be automatically approved when the user [submits the request](servidores.md#create-guild-join-request).

[**Member Verification Structure**](servidores.md#member-verification-structure)

| Field                | Type                                                                                                    | Description                                                                                                       |
| -------------------- | ------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| version              | ?ISO8601 timestamp                                                                                      | When the member verification was last modified                                                                    |
| form\_fields         | array\[[member verification form field](servidores.md#member-verification-form-field-structure) object] | Questions for applicants to answer (max 5)                                                                        |
| description          | ?string                                                                                                 | A description of what the guild is about; this can be different than the guild's description (max 300 characters) |
| guild <sup>1</sup>   | ?[member verification guild](servidores.md#member-verification-guild-structure) object                  | The guild this member verification is for                                                                         |
| profile <sup>1</sup> | [guild profile](https://docs.discord.food/resources/discovery#guild-profile-object) object              | The profile of the guild this member verification is for                                                          |

<sup>1</sup> Only included when fetched from the [Get Guild Member Verification](servidores.md#get-guild-member-verification) endpoint with set to .

[**Member Verification Form Field Structure**](servidores.md#member-verification-form-field-structure)

| Field                  | Type                          | Description                                                               |
| ---------------------- | ----------------------------- | ------------------------------------------------------------------------- |
| field\_type            | string                        | The [type of question](servidores.md#member-verification-form-field-type) |
| label                  | string                        | The label for the form field (max 300 characters)                         |
| choices?               | array\[string]                | Multiple choice answers (1-8, max 150 characters)                         |
| values?                | ?array\[string]               | The rules that the user must agree to (1-16, max 300 characters)          |
| response? <sup>1</sup> | ?string \| integer \| boolean | Response for this field                                                   |
| required               | boolean                       | Whether this field is required for a successful application               |
| description            | ?string                       | The subtext of the form field                                             |
| automations            | ?array\[string]               | Unknown (max 300 characters, max 10)                                      |
| placeholder?           | ?string                       | Placeholder text for the field's response area                            |

<sup>1</sup> Not present when fetched from the [Get Guild Member Verification](servidores.md#get-guild-member-verification) endpoint. For [form fields](servidores.md#member-verification-form-field-type), the response should be . For [form field types](servidores.md#member-verification-form-field-type), the response is the index of the selected choice.

[**Member Verification Form Field Type**](servidores.md#member-verification-form-field-type)

| Value            | Description                                                |
| ---------------- | ---------------------------------------------------------- |
| TERMS            | User must agree to the guild rules                         |
| TEXT\_INPUT      | User must respond with a short answer (max 150 characters) |
| PARAGRAPH        | User must respond with a paragraph (max 1000 characters)   |
| MULTIPLE\_CHOICE | User must select one of the provided choices               |
| ~~VERIFICATION~~ | ~~User must verify their email or phone number~~           |

[**Member Verification Guild Structure**](servidores.md#member-verification-guild-structure)

| Field                        | Type                                                                           | Description                                                                                                    |
| ---------------------------- | ------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------- |
| id                           | snowflake                                                                      | The ID of the guild                                                                                            |
| name                         | string                                                                         | The name of the guild (2-100 characters)                                                                       |
| icon                         | ?string                                                                        | The guild's [icon hash](https://docs.discord.food/reference#cdn-formatting)                                    |
| description                  | ?string                                                                        | The description for the guild (max 300 characters)                                                             |
| splash                       | ?string                                                                        | The guild's [splash hash](https://docs.discord.food/reference#cdn-formatting)                                  |
| discovery\_splash            | ?string                                                                        | The guild's [discovery splash hash](https://docs.discord.food/reference#cdn-formatting)                        |
| home\_header                 | ?string                                                                        | The guild's [home header hash](https://docs.discord.food/reference#cdn-formatting), used in new member welcome |
| verification\_level          | integer                                                                        | The [verification level](servidores.md#verification-level) required for the guild                              |
| features                     | array\[string]                                                                 | Enabled [guild features](servidores.md#guild-features)                                                         |
| emojis                       | array\[[emoji](https://docs.discord.food/resources/emoji#emoji-object) object] | Custom guild emojis                                                                                            |
| approximate\_member\_count   | integer                                                                        | Approximate number of total members in the guild                                                               |
| approximate\_presence\_count | integer                                                                        | Approximate number of non-offline members in the guild                                                         |

[**Example Member Verification**](servidores.md#example-member-verification)

```
{  "version": "2022-12-21T16:08:17.822000+00:00",  "form_fields": [    {      "field_type": "TERMS",      "label": "Read and agree to the server rules",      "description": null,      "automations": null,      "required": true,      "values": [        "No spam or self-promotion (server invites, advertisements, etc) without permission from a staff member. This includes DMing fellow members.",        "No age-restricted or obscene content. This includes text, images, or links featuring nudity, sex, hard violence, or other graphically disturbing content.",        "If you see something against the rules or something that makes you feel unsafe, let staff know. We want this server to be a welcoming space!"      ]    },    {      "field_type": "TEXT_INPUT",      "label": "Why do you like aliens?",      "description": null,      "automations": null,      "required": true,      "placeholder": null    },    {      "field_type": "MULTIPLE_CHOICE",      "label": "What is your favorite alien movie?",      "description": null,      "automations": null,      "required": true,      "choices": ["Alien", "E.T."]    }  ],  "description": "Alien",  "guild": {    "id": "1046920999469330512",    "name": "Alien Network",    "icon": "66b0f4d96c145970fa9d96ada8afadf3",    "description": "Where the 👽s 👽 and sometimes very 👽 things happen 😨.",    "home_header": "39ba384a31e9c285649ad00b359946ab",    "splash": "b40e61f7730b8781b9a551964570e0cc",    "discovery_splash": "0e11ae8d9f1c86958be05e61b0c90ac3",    "features": [],    "approximate_member_count": 100,    "approximate_presence_count": 99,    "emojis": [],    "verification_level": 2  }}
```

#### [Guild Join Request Object](servidores.md#guild-join-request-object) <a href="#guild-join-request-object" id="guild-join-request-object"></a>

Guild join requests are an extension of [member verification](servidores.md#member-verification-object) that represent a user's request to join a guild. A join request is created when a user attempts to join a guild with member verification enabled, and the user must complete the verification process to join the guild. All join requests are stored for 180 days.

Most join request features require the [guild feature](servidores.md#guild-features), as join request are automatically actioned when only using the [form field type](servidores.md#member-verification-form-field-type) and do not require manual approval. This includes most of the join request endpoints, such as [Get Guild Join Request](servidores.md#get-guild-join-request) and [Action Guild Join Request](servidores.md#action-guild-join-request).

[**Guild Join Request Structure**](servidores.md#guild-join-request-structure)

| Field                            | Type                                                                                                     | Description                                                                         |
| -------------------------------- | -------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| id                               | snowflake                                                                                                | The ID of the join request                                                          |
| join\_request\_id                | snowflake                                                                                                | The ID of the join request                                                          |
| created\_at                      | ISO8601 timestamp                                                                                        | When the join request was created                                                   |
| application\_status              | string                                                                                                   | The [status of the join request](servidores.md#guild-join-request-status)           |
| guild\_id                        | snowflake                                                                                                | The ID of the guild this join request is for                                        |
| form\_responses? <sup>1</sup>    | ?array\[[member verification form field](servidores.md#member-verification-form-field-structure) object] | Responses to the guild's member verification questions                              |
| last\_seen                       | ?ISO8601 timestamp                                                                                       | When the request was acknowledged by the user                                       |
| actioned\_at? <sup>1</sup>       | snowflake                                                                                                | A snowflake representing when the join request was actioned                         |
| actioned\_by\_user? <sup>1</sup> | partial [user](https://docs.discord.food/resources/user#user-object) object                              | The moderator who actioned the join request                                         |
| rejection\_reason                | ?string                                                                                                  | Why the join request was rejected                                                   |
| user\_id                         | snowflake                                                                                                | The ID of the user who created this join request                                    |
| user? <sup>1</sup> <sup>2</sup>  | partial [user](https://docs.discord.food/resources/user#user-object) object                              | The user who created this join request                                              |
| interview\_channel\_id           | ?snowflake                                                                                               | The ID of a channel where an interview regarding this join request may be conducted |

<sup>1</sup> Only included when fetched from the [Get Guild Join Request](servidores.md#get-guild-join-request) or [Get Guild Join Requests](servidores.md#get-guild-join-requests) endpoints, or when receiving a [related Gateway event](https://docs.discord.food/topics/gateway-events#guild-join-request-create) as a moderator.

<sup>2</sup> Only guaranteed for users other than the current user.

[**Guild Join Request Status**](servidores.md#guild-join-request-status)

| Value     | Description                              |
| --------- | ---------------------------------------- |
| STARTED   | The request is started but not submitted |
| SUBMITTED | The request has been submitted           |
| REJECTED  | The request has been rejected            |
| APPROVED  | The request has been approved            |

[**Example Guild Join Request**](servidores.md#example-guild-join-request)

```
{  "created_at": "2024-05-20T03:45:28.965000+00:00",  "join_request_id": "1241959960003477524",  "id": "1241959960003477524",  "rejection_reason": null,  "application_status": "APPROVED",  "actioned_at": "2024-05-20T03:45:44.547975+00:00",  "actioned_by_user": {    "id": "828387742575624222",    "username": "jupppper",    "avatar": "e14a7c62b0b38068be88be194b23910f",    "discriminator": "0",    "public_flags": 16384,    "banner": "e45c9b5799fcb46b82bd5f1afc1b30c4",    "global_name": "Jup",    "accent_color": 1,    "avatar_decoration_data": null,    "primary_guild": null  },  "form_responses": [    {      "field_type": "TERMS",      "label": "Read and agree to the server rules",      "description": null,      "automations": null,      "required": true,      "values": [        "No spam or self-promotion (server invites, advertisements, etc) without permission from a staff member. This includes DMing fellow members.",        "No age-restricted or obscene content. This includes text, images, or links featuring nudity, sex, hard violence, or other graphically disturbing content.",        "If you see something against the rules or something that makes you feel unsafe, let staff know. We want this server to be a welcoming space!"      ],      "response": true    },    {      "field_type": "TEXT_INPUT",      "label": "Why do you like aliens?",      "description": null,      "automations": null,      "required": true,      "placeholder": null,      "response": "I like aliens because they're cool!"    },    {      "field_type": "MULTIPLE_CHOICE",      "label": "What is your favorite alien movie?",      "description": null,      "automations": null,      "required": true,      "values": ["Alien", "E.T."],      "response": 0    }  ],  "last_seen": "2024-05-20T03:45:44.547975+00:00",  "guild_id": "1046920999469330512",  "user_id": "852892297661906993",  "user": {    "id": "852892297661906993",    "username": "alien",    "global_name": "Alien",    "avatar": "14733482e560d9267c0a414b21b2fb8d",    "discriminator": "0",    "public_flags": 64,    "avatar_decoration_data": null,    "primary_guild": null  },  "interview_channel_id": null}
```

#### [Onboarding Object](servidores.md#onboarding-object) <a href="#onboarding-object" id="onboarding-object"></a>

The [onboarding](https://support.discord.com/hc/en-us/articles/11074987197975-Community-Onboarding-FAQ) flow for a guild.

[**Onboarding Structure**](servidores.md#onboarding-structure)

| Field                 | Type                                                                          | Description                                                               |
| --------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| guild\_id             | snowflake                                                                     | The ID of the guild this onboarding is part of                            |
| prompts               | array\[[onboarding prompt](servidores.md#onboarding-prompt-structure) object] | The prompts shown during onboarding and in community customization        |
| default\_channel\_ids | array\[snowflake]                                                             | The channel IDs that members get opted into automatically                 |
| enabled               | boolean                                                                       | Whether onboarding is enabled in the guild                                |
| below\_requirements   | boolean                                                                       | Whether the guild is below the requirements for onboarding                |
| mode                  | integer                                                                       | The current [criteria mode](servidores.md#onboarding-mode) for onboarding |

[**Onboarding Mode**](servidores.md#onboarding-mode)

Defines the criteria used to satisfy onboarding constraints that are required for enabling.

| Value | Name                 | Description                                              |
| ----- | -------------------- | -------------------------------------------------------- |
| 0     | ONBOARDING\_DEFAULT  | Count only default channels towards constraints          |
| 1     | ONBOARDING\_ADVANCED | Count default channels and questions towards constraints |

[**Onboarding Prompt Structure**](servidores.md#onboarding-prompt-structure)

| Field          | Type                                                                                        | Description                                                                                     |
| -------------- | ------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| id             | snowflake                                                                                   | The ID of the prompt                                                                            |
| type           | integer                                                                                     | The [type of prompt](servidores.md#onboarding-prompt-type)                                      |
| options        | array\[[onboarding prompt option](servidores.md#onboarding-prompt-option-structure) object] | Options available within the prompt                                                             |
| title          | string                                                                                      | The title of the prompt                                                                         |
| single\_select | boolean                                                                                     | Whether users are limited to selecting one option for the prompt                                |
| required       | boolean                                                                                     | Whether the prompt is required before a user completes the onboarding flow                      |
| in\_onboarding | boolean                                                                                     | Wether the prompt is present in the onboarding flow, or only appears in community customization |

[**Onboarding Prompt Option Structure**](servidores.md#onboarding-prompt-option-structure)

| Field        | Type                                                                   | Description                                                      |
| ------------ | ---------------------------------------------------------------------- | ---------------------------------------------------------------- |
| id           | snowflake                                                              | The ID of the prompt option                                      |
| channel\_ids | array\[snowflake]                                                      | The channel IDs a member is added to when the option is selected |
| role\_ids    | array\[snowflake]                                                      | The role IDs assigned to a member when the option is selected    |
| emoji        | [emoji](https://docs.discord.food/resources/emoji#emoji-object) object | Emoji representing the option                                    |
| title        | string                                                                 | The title of the option                                          |
| description  | ?string                                                                | The description for the option                                   |

[**Onboarding Prompt Type**](servidores.md#onboarding-prompt-type)

| Value | Name             | Description                                   |
| ----- | ---------------- | --------------------------------------------- |
| 0     | MULTIPLE\_CHOICE | Prompt offers multiple options to select from |
| 1     | DROPDOWN         | Prompt offers a dropdown menu to select from  |

[**Example Onboarding**](servidores.md#example-onboarding)

```
{  "guild_id": "960007075288915998",  "prompts": [    {      "id": "1067461047608422473",      "title": "What do you want to do in this community?",      "options": [        {          "id": "1067461047608422476",          "title": "Chat with Friends",          "description": "",          "emoji": {            "id": "1070002302032826408",            "name": "chat",            "animated": false          },          "role_ids": [],          "channel_ids": ["962007075288916001"]        },        {          "id": "1070004843541954678",          "title": "Get Gud",          "description": "We have excellent teachers!",          "emoji": {            "id": null,            "name": "😀",            "animated": false          },          "role_ids": ["982014491980083211"],          "channel_ids": []        }      ],      "single_select": false,      "required": false,      "in_onboarding": true,      "type": 0    }  ],  "default_channel_ids": [    "998678771706110023",    "998678693058719784",    "1070008122577518632",    "998678764340912138",    "998678704446263309",    "998678683592171602",    "998678699715067986"  ],  "enabled": true,  "mode": 0,  "below_requirements": false}
```

#### [New Member Welcome Object](servidores.md#new-member-welcome-object) <a href="#new-member-welcome-object" id="new-member-welcome-object"></a>

The welcome experience for new and existing members in a guild, also known as the server guide.

[**New Member Welcome Structure**](servidores.md#new-member-welcome-structure)

| Field                | Type                                                                                    | Description                                                       |
| -------------------- | --------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| guild\_id            | snowflake                                                                               | The ID of the guild this new member welcome is for                |
| enabled              | boolean                                                                                 | Whether the new member welcome experience is enabled              |
| welcome\_message     | [new member welcome message](servidores.md#new-member-welcome-message-structure) object | Welcome message shown to new members of the guild                 |
| new\_member\_actions | array\[[new member action](servidores.md#new-member-action-structure) object]           | Actions shown to new members of the guild (max 5)                 |
| resource\_channels   | array\[[resource channel](servidores.md#resource-channel-structure) object]             | Read-only channels that provide resources for new members (max 7) |

[**New Member Welcome Message Structure**](servidores.md#new-member-welcome-message-structure)

| Field                    | Type              | Description                                                    |
| ------------------------ | ----------------- | -------------------------------------------------------------- |
| author\_ids <sup>1</sup> | array\[snowflake] | The IDs of the users who authored the welcome message (max 10) |
| message <sup>2</sup>     | string            | The welcome message shown to new members (max 300 characters)  |

<sup>1</sup> New member welcome message authors must be guild members with the or permission.

<sup>2</sup> may be used as a placeholder for the member's name.

[**New Member Action Structure**](servidores.md#new-member-action-structure)

| Field        | Type                                                                           | Description                                                                                    |
| ------------ | ------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| channel\_id  | snowflake                                                                      | The ID of the channel where the action is located                                              |
| action\_type | integer                                                                        | The [type of action](servidores.md#new-member-action-type) the user should take in the channel |
| title        | string                                                                         | The title of the action (max 60 characters)                                                    |
| description  | string                                                                         | The description of the action (max 200 characters)                                             |
| emoji?       | partial [emoji](https://docs.discord.food/resources/emoji#emoji-object) object | The emoji representing the action                                                              |
| icon?        | string                                                                         | The [icon hash](https://docs.discord.food/reference#cdn-formatting) representing the action    |

[**New Member Action Type**](servidores.md#new-member-action-type)

| Value | Name | Description                   |
| ----- | ---- | ----------------------------- |
| 0     | VIEW | View the channel              |
| 1     | CHAT | Send a message in the channel |

[**Resource Channel Structure**](servidores.md#resource-channel-structure)

| Field       | Type                                                                           | Description                                                                                           |
| ----------- | ------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------- |
| channel\_id | snowflake                                                                      | The ID of the channel that provides resources for new members                                         |
| title       | string                                                                         | The title of the resource channel (max 60 characters)                                                 |
| description | string                                                                         | The description of the resource channel (max 200 characters)                                          |
| emoji?      | partial [emoji](https://docs.discord.food/resources/emoji#emoji-object) object | The emoji representing the resource channel                                                           |
| icon?       | string                                                                         | The [icon hash](https://docs.discord.food/reference#cdn-formatting) representing the resource channel |

[**Example New Member Welcome**](servidores.md#example-new-member-welcome)

```
{  "guild_id": "1029315212005888060",  "enabled": false,  "welcome_message": {    "author_ids": ["852892297661906993"],    "message": "Hello [@username], 👽👽👽"  },  "new_member_actions": [    {      "channel_id": "1029316811088478299",      "action_type": 0,      "title": "Get your info",      "description": "",      "emoji": {        "id": null,        "name": "👽",        "animated": false      }    }  ],  "resource_channels": [    {      "channel_id": "1029316811088478299",      "title": "Info",      "description": "Absolute cinema"    }  ]}
```

#### [New Member Actions Progress Object](servidores.md#new-member-actions-progress-object) <a href="#new-member-actions-progress-object" id="new-member-actions-progress-object"></a>

A user's progress towards completing the new member actions in a guild.

[**New Member Actions Progress Structure**](servidores.md#new-member-actions-progress-structure)

| Field            | Type                                                                                                     | Description                                                                          |
| ---------------- | -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| guild\_id        | snowflake                                                                                                | The ID of the guild this new member actions progress is for                          |
| user\_id         | snowflake                                                                                                | The ID of the user this new member actions progress is for                           |
| channel\_actions | map\[snowflake, [new member action progress](servidores.md#new-member-action-progress-structure) object] | The progress of the user in each new member action channel they have interacted with |

[**New Member Action Progress Structure**](servidores.md#new-member-action-progress-structure)

| Field     | Type    | Description                                                         |
| --------- | ------- | ------------------------------------------------------------------- |
| completed | boolean | Whether the user has completed the new member action in the channel |

[**Example New Member Actions Progress**](servidores.md#example-new-member-actions-progress)

```
{  "guild_id": "1029315212005888060",  "user_id": "852892297661906993",  "channel_actions": {    "1029316811088478299": {      "completed": true    }  }}
```

#### [Premium Guild Subscription Object](servidores.md#premium-guild-subscription-object) <a href="#premium-guild-subscription-object" id="premium-guild-subscription-object"></a>

Represents a member's premium guild subscription ([boost](https://support.discord.com/hc/en-us/sections/360007875211-Server-Boosting)).

[**Premium Guild Subscription Structure**](servidores.md#premium-guild-subscription-structure)

| Field           | Type                                                                        | Description                                                                                      |
| --------------- | --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| id              | snowflake                                                                   | The ID of the premium guild subscription                                                         |
| guild\_id       | snowflake                                                                   | The ID of the guild this subscription is for                                                     |
| user\_id        | snowflake                                                                   | The ID of the user who created this premium guild subscription                                   |
| ended           | boolean                                                                     | If this premium guild subscription has ended                                                     |
| ends\_at?       | ISO8601 timestamp                                                           | When this premium guild subscription will expire                                                 |
| pause\_ends\_at | ?ISO8601 timestamp                                                          | When the user's overall subscription pause will end, reactivating the premium guild subscription |
| user            | partial [user](https://docs.discord.food/resources/user#user-object) object | The user this premium guild subscription is for                                                  |

[**Example Premium Guild Subscription**](servidores.md#example-premium-guild-subscription)

```
{  "id": "1315132642890350602",  "user_id": "673658900435697665",  "guild_id": "1081635484209520802",  "ended": false,  "pause_ends_at": null,  "user": {    "id": "673658900435697665",    "username": "android",    "global_name": "Android",    "avatar": "08f104f8d5406c4d46916794fe2efeb7",    "avatar_decoration_data": {      "asset": "a_8552f9857793aed0cf816f370e2df3be",      "sku_id": "1232071712695386162",      "expires_at": null    },    "collectibles": null,    "discriminator": "0",    "public_flags": 4194560,    "primary_guild": null  }}
```

### [Endpoints](servidores.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get User Guilds](servidores.md#get-user-guilds) <a href="#get-user-guilds" id="get-user-guilds"></a>

`GET/users/@me/guilds`

Returns a list of [user guild](servidores.md#user-guild-object) objects representing the guilds the current user is a member of.

[**Query String Params**](servidores.md#query-string-params)

| Field                      | Type      | Description                                                               |
| -------------------------- | --------- | ------------------------------------------------------------------------- |
| before?                    | snowflake | Get guilds before this guild ID                                           |
| after?                     | snowflake | Get guilds after this guild ID                                            |
| limit?                     | integer   | Max number of guilds to return (1-200, default 200)                       |
| with\_counts? <sup>1</sup> | boolean   | Whether to include approximate member and presence counts (default false) |

<sup>1</sup> For OAuth2 requests, this parameter requires the additional scope.

#### [Get Join Request Guilds](servidores.md#get-join-request-guilds) <a href="#get-join-request-guilds" id="get-join-request-guilds"></a>

`GET/users/@me/join-request-guilds`

Returns a list of partial [guild](servidores.md#guild-object) objects representing [non-previewable guilds](servidores.md#guild-previewing) the current user has pending join requests for.

#### [Leave Guild](servidores.md#leave-guild) <a href="#leave-guild" id="leave-guild"></a>

`DELETE/users/@me/guilds/{guild.id}`

Leaves the given guild ID. Returns a 204 empty response on success. Fires a [Guild Delete](https://docs.discord.food/topics/gateway-events#guild-delete) and a [Guild Member Remove](https://docs.discord.food/topics/gateway-events#guild-member-remove) Gateway event.

[**JSON Params**](servidores.md#json-params)

| Field    | Type    | Description                                              |
| -------- | ------- | -------------------------------------------------------- |
| lurking? | boolean | Whether the user is lurking in the guild (default false) |

#### [Create Guild](servidores.md#create-guild) <a href="#create-guild" id="create-guild"></a>

`POST/guilds`

Creates a new guild. Returns a [guild](servidores.md#guild-object) object on success. Fires a [Guild Create](https://docs.discord.food/topics/gateway-events#guild-create) Gateway event.

[**JSON Params**](servidores.md#json-params)

| Field                            | Type                                                                                          | Description                                                                                                                                |
| -------------------------------- | --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| name                             | string                                                                                        | The name of the guild (2-100 characters, excluding trailing and leading whitespace)                                                        |
| description?                     | ?string                                                                                       | The description for the guild (max 300 characters)                                                                                         |
| region? **(deprecated)**         | ?string                                                                                       | The main [voice region](https://docs.discord.food/resources/voice#voice-region-object) ID of the guild                                     |
| icon?                            | ?[image data](https://docs.discord.food/reference#cdn-data)                                   | The guild's icon                                                                                                                           |
| verification\_level?             | ?integer                                                                                      | The [verification level](servidores.md#verification-level) required for the guild                                                          |
| default\_message\_notifications? | ?integer                                                                                      | Default [message notification level](servidores.md#message-notification-level) for the guild                                               |
| explicit\_content\_filter?       | ?integer                                                                                      | [Whose messages](servidores.md#explicit-content-filter-level) are scanned and deleted for explicit content in the guild                    |
| preferred\_locale?               | ?string                                                                                       | The preferred locale of the guild (default "en-US")                                                                                        |
| roles? <sup>1</sup>              | ?array\[partial [role](servidores.md#role-object) object]                                     | Roles in the new guild                                                                                                                     |
| channels? <sup>2</sup>           | ?array\[partial [channel](https://docs.discord.food/resources/channel#channel-object) object] | Channels in the new guild                                                                                                                  |
| afk\_channel\_id?                | ?snowflake                                                                                    | The ID of the guild's AFK channel; this is where members in voice idle for longer than `afk_timeout` are moved                             |
| afk\_timeout?                    | ?integer                                                                                      | The AFK timeout of the guild (one of 60, 300, 900, 1800, 3600, in seconds)                                                                 |
| system\_channel\_id?             | ?snowflake                                                                                    | The ID of the channel where system event messages, such as member joins and premium subscriptions (boosts), are posted                     |
| system\_channel\_flags?          | ?integer                                                                                      | The [flags](servidores.md#system-channel-flags) that limit system event messages                                                           |
| guild\_template\_code?           | ?string                                                                                       | The [template](https://docs.discord.food/resources/guild-template#guild-template-object) code that inspired this guild, used for analytics |
| staff\_only? <sup>3</sup>        | boolean                                                                                       | Whether the new guild will only be accessible for Discord employees                                                                        |

<sup>1</sup> The first member of the array is used to change properties of the guild's default (@everyone) role. If you are trying to bootstrap a guild with additional roles, keep this in mind. Additionally, the required field within each role object is an integer placeholder, and will be replaced by the API upon consumption. Its purpose is to allow you to [overwrite](https://docs.discord.food/resources/channel#permission-overwrite-object) a role's permissions in a channel when also passing in channels with the channels array.

<sup>2</sup> The field within each channel object may be set to an integer placeholder, and will be replaced by the API upon consumption. Its purpose is to allow you to create channels by setting the field on any children to the category's field. Category channels must be listed before any children.

<sup>3</sup> Adds the [guild feature](servidores.md#guild-features), making the server only available for Discord employees. Only settable by Discord employees.

[**Example Partial Channel Object**](servidores.md#example-partial-channel-object)

```
{  "name": "my-category",  "type": 4,  "id": 1}{  "name": "naming-things-is-hard",  "type": 0,  "parent_id": 1}
```

#### [Get Guild](servidores.md#get-guild) <a href="#get-guild" id="get-guild"></a>

`GET/guilds/{guild.id}`

Returns a [guild](servidores.md#guild-object) object for the given guild ID. User must be a member of the guild.

[**Query String Params**](servidores.md#query-string-params)

| Field         | Type    | Description                                                               |
| ------------- | ------- | ------------------------------------------------------------------------- |
| with\_counts? | boolean | Whether to include approximate member and presence counts (default false) |

#### [Get Guild Basic](servidores.md#get-guild-basic) <a href="#get-guild-basic" id="get-guild-basic"></a>

`GET/guilds/{guild.id}/basic`

Returns a partial [guild](servidores.md#guild-object) object for the given guild ID. If the user is not in the guild, the guild must be discoverable.

#### [Get Guild Preview](servidores.md#get-guild-preview) <a href="#get-guild-preview" id="get-guild-preview"></a>

`GET/guilds/{guild.id}/preview`

Returns a partial [guild](servidores.md#guild-object) object for the given guild ID with all partial fields. If the user is not in the guild, the guild must be discoverable.

#### [Modify Guild](servidores.md#modify-guild) <a href="#modify-guild" id="modify-guild"></a>

`PATCH/guilds/{guild.id}`

Modifies a guild's settings. Requires the permission. Returns the updated [guild](servidores.md#guild-object) object on success. Fires a [Guild Update](https://docs.discord.food/topics/gateway-events#guild-update) Gateway event.

[**JSON Params**](servidores.md#json-params)

| Field                             | Type                                                        | Description                                                                                                                                                                                  |
| --------------------------------- | ----------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name?                             | string                                                      | The name of the guild (2-100 characters, excluding trailing and leading whitespace)                                                                                                          |
| icon?                             | ?[image data](https://docs.discord.food/reference#cdn-data) | The guild's icon; animated icons are only shown when the guild has the `ANIMATED_ICON` feature                                                                                               |
| banner?                           | ?[image data](https://docs.discord.food/reference#cdn-data) | The guild's banner; banners are only shown when the guild has the `BANNER` feature, animated banners are only shown when the guild has the `ANIMATED_BANNER` feature                         |
| home\_header?                     | ?[image data](https://docs.discord.food/reference#cdn-data) | The guild's home header, used in new member welcome; home headers are only shown when the guild has the `BANNER` feature                                                                     |
| splash?                           | ?[image data](https://docs.discord.food/reference#cdn-data) | The guild's invite splash; splashes are only shown when the guild has the `INVITE_SPLASH` feature                                                                                            |
| discovery\_splash?                | ?[image data](https://docs.discord.food/reference#cdn-data) | The guild's discovery splash                                                                                                                                                                 |
| owner\_id?                        | snowflake                                                   | The user ID of the guild's owner (must be the current owner); if the current owner has an email address associated with their account and does not have MFA enabled, `code` must be provided |
| code? <sup>2</sup>                | string                                                      | The guild ownership transfer code                                                                                                                                                            |
| description?                      | ?string                                                     | The description for the guild (max 300 characters)                                                                                                                                           |
| region? **(deprecated)**          | ?string                                                     | The main [voice region](https://docs.discord.food/resources/voice#voice-region-object) ID of the guild                                                                                       |
| afk\_channel\_id?                 | ?snowflake                                                  | The ID of the guild's AFK channel; this is where members in voice idle for longer than `afk_timeout` are moved                                                                               |
| afk\_timeout?                     | integer                                                     | The AFK timeout of the guild (one of 60, 300, 900, 1800, 3600, in seconds)                                                                                                                   |
| verification\_level?              | integer                                                     | The [verification level](servidores.md#verification-level) required for the guild                                                                                                            |
| default\_message\_notifications?  | integer                                                     | Default [message notification level](servidores.md#message-notification-level) for the guild                                                                                                 |
| explicit\_content\_filter?        | integer                                                     | [Whose messages](servidores.md#explicit-content-filter-level) are scanned and deleted for explicit content in the guild                                                                      |
| features?                         | array\[string]                                              | [Mutable guild features](servidores.md#mutable-guild-features)                                                                                                                               |
| system\_channel\_id? <sup>1</sup> | ?snowflake                                                  | The ID of the channel where system event messages, such as member joins and premium subscriptions (boosts), are posted                                                                       |
| system\_channel\_flags?           | integer                                                     | The [flags](servidores.md#system-channel-flags) that limit system event messages                                                                                                             |
| rules\_channel\_id? <sup>1</sup>  | ?snowflake                                                  | The ID of the channel where community guilds display rules and/or guidelines                                                                                                                 |
| public\_updates\_channel\_id?     | ?snowflake                                                  | The ID of the channel where admins and moderators of community guilds receive notices from Discord                                                                                           |
| safety\_alerts\_channel\_id?      | ?snowflake                                                  | The ID of the channel where admins and moderators of community guilds receive safety alerts from Discord                                                                                     |
| preferred\_locale?                | string                                                      | The preferred locale of the guild; used in discovery and notices from Discord (default "en-US")                                                                                              |
| premium\_progress\_bar\_enabled?  | boolean                                                     | Whether the guild has the premium (boost) progress bar enabled                                                                                                                               |

<sup>1</sup> Setting these to a value of will implicitly create a new "#rules" or "#moderator-only" channel.

<sup>2</sup> This value can be obtained by requesting a verification code with the [Get Guild Ownership Transfer Code](servidores.md#get-guild-ownership-transfer-code) endpoint.

#### [Modify Guild MFA Level](servidores.md#modify-guild-mfa-level) <a href="#modify-guild-mfa-level" id="modify-guild-mfa-level"></a>

`POST/guilds/{guild.id}/mfa`

Modifies the guild's [MFA requirement](servidores.md#mfa-level) for administrative actions within the guild. User must be the owner. Fires a [Guild Update](https://docs.discord.food/topics/gateway-events#guild-update) Gateway event.

[**JSON Params / Response Body**](servidores.md#json-params-/-response-body)

| Field | Type    | Description                                                                               |
| ----- | ------- | ----------------------------------------------------------------------------------------- |
| level | integer | Required [MFA level](servidores.md#mfa-level) for administrative actions within the guild |

[**Example Response**](servidores.md#example-response)

```
{  "level": 1}
```

#### [Get Guild Ownership Transfer Code](servidores.md#get-guild-ownership-transfer-code) <a href="#get-guild-ownership-transfer-code" id="get-guild-ownership-transfer-code"></a>

`PUT/guilds/{guild.id}/pincode`

Sends a verification code to the guild owner's email address to initiate the guild ownership transfer process. User must be the owner. Returns a 204 empty response on success.

#### [Delete Guild](servidores.md#delete-guild) <a href="#delete-guild" id="delete-guild"></a>

`DELETE/guilds/{guild.id}`

Deletes a guild permanently. User must be the owner. Returns a 204 empty response on success. Fires a [Guild Delete](https://docs.discord.food/topics/gateway-events#guild-delete) Gateway event.

#### [Get Guild Members](servidores.md#get-guild-members) <a href="#get-guild-members" id="get-guild-members"></a>

`GET/guilds/{guild.id}/members`

Returns a list of [guild member](servidores.md#guild-member-object) objects that are members of the guild. User must be a member of the guild.

[**Query String Params**](servidores.md#query-string-params)

| Field  | Type      | Description                                         |
| ------ | --------- | --------------------------------------------------- |
| limit? | integer   | Max number of members to return (1-1000, default 1) |
| after? | snowflake | Get members after this member ID                    |

#### [Query Guild Members](servidores.md#query-guild-members) <a href="#query-guild-members" id="query-guild-members"></a>

`GET/guilds/{guild.id}/members/search`

Returns a list of [guild member](servidores.md#guild-member-object) objects whose username or nickname contains a provided string. User must be a member of the guild.

Functionally identical to the [Request Guild Members](https://docs.discord.food/topics/gateway-events#request-guild-members) Gateway Opcode.

[**Query String Params**](servidores.md#query-string-params)

| Field  | Type    | Description                                         |
| ------ | ------- | --------------------------------------------------- |
| query  | string  | Query to match username(s) and nickname(s) against  |
| limit? | integer | Max number of members to return (1-1000, default 1) |

#### [Search Guild Members](servidores.md#search-guild-members) <a href="#search-guild-members" id="search-guild-members"></a>

`POST/guilds/{guild.id}/members-search`

Returns [supplemental guild member](servidores.md#supplemental-guild-member-object) objects containing [guild member](servidores.md#guild-member-object) objects that match a specified query. Requires the permission.

This endpoint utilizes Elasticsearch to power results. This means that while it is very powerful, it's also tricky to use and reliant on the index, meaning results may not be immediately available for a recently-joined member.

[**JSON Params**](servidores.md#json-params)

| Field       | Type                                                                                | Description                                                                               |
| ----------- | ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| limit?      | integer                                                                             | Max number of members to return (1-1000, default 25)                                      |
| sort?       | integer                                                                             | The [sorting algorithm](servidores.md#member-sort-type) to use (default `JOINED_AT_DESC`) |
| or\_query?  | [member filter](servidores.md#member-filter-structure) object                       | The filter criteria to match against members using OR logic                               |
| and\_query? | [member filter](servidores.md#member-filter-structure) object                       | The filter criteria to match against members using AND logic                              |
| before?     | [member pagination filter](servidores.md#member-pagination-filter-structure) object | Get members before this member                                                            |
| after?      | [member pagination filter](servidores.md#member-pagination-filter-structure) object | Get members after this member                                                             |

[**Member Sort Type**](servidores.md#member-sort-type)

| Value | Name             | Description                                                 |
| ----- | ---------------- | ----------------------------------------------------------- |
| 1     | JOINED\_AT\_DESC | Sort by when the user joined the guild descending (default) |
| 2     | JOINED\_AT\_ASC  | Sort by when the user joined the guild ascending            |
| 3     | USER\_ID\_DESC   | Sort by when the user joined Discord descending             |
| 4     | USER\_ID\_ASC    | Sort by when the user joined Discord ascending              |

[**Member Filter Structure**](servidores.md#member-filter-structure)

| Field                 | Type                                                            | Description                                                                                                                    | Queries                 |
| --------------------- | --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | ----------------------- |
| user\_id?             | [query](servidores.md#query-structure) object (snowflake)       | Query to match member IDs against                                                                                              | `or_query`, `range`     |
| usernames?            | [query](servidores.md#query-structure) object (string)          | Query to match display name(s), username(s), and nickname(s) against                                                           | `or_query`              |
| role\_ids?            | [query](servidores.md#query-structure) object (snowflake)       | IDs of roles to match members against                                                                                          | `or_query`, `and_query` |
| guild\_joined\_at?    | [query](servidores.md#query-structure) object (integer)         | When the user joined the guild                                                                                                 | `range`                 |
| safety\_signals?      | [safety signals](servidores.md#safety-signals-structure) object | Safety signals to match members against                                                                                        |                         |
| is\_pending?          | boolean                                                         | Whether the member has not yet passed the guild's [member verification](servidores.md#member-verification-object) requirements | `true`, `false`         |
| did\_rejoin?          | boolean                                                         | Whether the member left and rejoined the guild                                                                                 | `true`, `false`         |
| join\_source\_type?   | [query](servidores.md#query-structure) object (integer)         | [How the user joined the guild](servidores.md#join-source-type)                                                                | `or_query`              |
| source\_invite\_code? | [query](servidores.md#query-structure) object (string)          | The invite code or vanity used to join the guild                                                                               | `or_query`              |

[**Safety Signals Structure**](servidores.md#safety-signals-structure)

| Field                           | Type                                                    | Description                                                                                                                                                                                      | Queries         |
| ------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------- |
| unusual\_dm\_activity\_until?   | [query](servidores.md#query-structure) object (integer) | When the member's unusual DM activity flag will expire                                                                                                                                           | `range`         |
| communication\_disabled\_until? | [query](servidores.md#query-structure) object (integer) | When the member's [timeout](https://support.discord.com/hc/en-us/articles/4413305239191-Time-Out-FAQ) will expire                                                                                | `range`         |
| unusual\_account\_activity?     | boolean                                                 | Whether the user has the [flag](https://docs.discord.food/resources/user#user-flags)                                                                                                             | `true`, `false` |
| automod\_quarantined\_username? | boolean                                                 | Whether the member has been indefinitely quarantined by [an AutoMod Rule](https://docs.discord.food/resources/auto-moderation#automod-rule-object) for their username, display name, or nickname | `true`, `false` |

[**Query Structure**](servidores.md#query-structure)

| Field       | Type                                                      | Description                                                            |
| ----------- | --------------------------------------------------------- | ---------------------------------------------------------------------- |
| or\_query?  | array\[snowflake \| string \| integer]                    | The values to match against using OR logic (1-100 characters, max 10)  |
| and\_query? | array\[snowflake \| string \| integer]                    | The values to match against using AND logic (1-100 characters, max 10) |
| range?      | [range query](servidores.md#range-query-structure) object | The range of values to match against                                   |

[**Range Query Structure**](servidores.md#range-query-structure)

| Field | Type                 | Description                          |
| ----- | -------------------- | ------------------------------------ |
| gte?  | snowflake \| integer | Inclusive lower bound value to match |
| lte?  | snowflake \| integer | Inclusive upper bound value to match |

| Field             | Type      | Description                                     |
| ----------------- | --------- | ----------------------------------------------- |
| user\_id          | snowflake | The ID of the user to paginate past             |
| guild\_joined\_at | integer   | When the user to paginate past joined the guild |

[**Response Body**](servidores.md#response-body)

| Field                | Type                                                                                       | Description                       |
| -------------------- | ------------------------------------------------------------------------------------------ | --------------------------------- |
| guild\_id            | snowflake                                                                                  | The ID of the guild searched      |
| members              | array\[[supplemental guild member](servidores.md#supplemental-guild-member-object) object] | The resulting members             |
| page\_result\_count  | integer                                                                                    | The number of results returned    |
| total\_result\_count | integer                                                                                    | The total number of results found |

`POST/guilds/{guild.id}/members/supplemental`

Returns a list of [supplemental guild member](servidores.md#supplemental-guild-member-object) objects including join source information for the given user IDs. Requires the permission.

[**JSON Params**](servidores.md#json-params)

| Field | Type              | Description                                                               |
| ----- | ----------------- | ------------------------------------------------------------------------- |
| users | array\[snowflake] | The user IDs to fetch supplemental guild member information for (max 200) |

#### [Get Guild Members With Unusual DM Activity](servidores.md#get-guild-members-with-unusual-dm-activity) <a href="#get-guild-members-with-unusual-dm-activity" id="get-guild-members-with-unusual-dm-activity"></a>

`GET/guilds/{guild.id>/members/unusual-dm-activity`

Returns a list of [guild member unusual DM activity](servidores.md#guild-member-unusual-dm-activity-structure) objects representing the members that have ever had unusual DM activity. User must be a member of the guild.

[**Query String Params**](servidores.md#query-string-params)

| Field  | Type      | Description                                             |
| ------ | --------- | ------------------------------------------------------- |
| limit? | integer   | Max number of members to return (max 1000, default 100) |
| after? | snowflake | Get members after this member ID                        |

[**Guild Member Unusual DM Activity Structure**](servidores.md#guild-member-unusual-dm-activity-structure)

| Field                                     | Type              | Description                                          |
| ----------------------------------------- | ----------------- | ---------------------------------------------------- |
| user\_id                                  | snowflake         | The ID of the user with unusual DM activity          |
| guild\_id                                 | snowflake         | The ID of the guild the user is in                   |
| unusual\_dm\_activity\_until <sup>1</sup> | ISO8601 timestamp | When the user's unusual DM activity flag will expire |

<sup>1</sup> If the value is a time in the past, the flag has expired.

[**Example Guild Member Unusual DM Activity**](servidores.md#example-guild-member-unusual-dm-activity)

```
{  "user_id": "934487154330066945",  "guild_id": "81384788765712384",  "unusual_dm_activity_until": "2024-01-15T06:10:37.288219+00:00"}
```

#### [Get Current Guild Member](servidores.md#get-current-guild-member) <a href="#get-current-guild-member" id="get-current-guild-member"></a>

`GET/users/@me/guilds/{guild.id}/member`

Returns the [guild member](servidores.md#guild-member-object) object for the current user in the specified guild.

#### [Get Guild Member](servidores.md#get-guild-member) <a href="#get-guild-member" id="get-guild-member"></a>

`GET/guilds/{guild.id}/members/{user.id}`

Returns a [guild member](servidores.md#guild-member-object) object for the specified user.

#### [Join Guild](servidores.md#join-guild) <a href="#join-guild" id="join-guild"></a>

`PUT/guilds/{guild.id}/members/@me`

Adds the current user to the guild. The guild must be discoverable. If the user is not a member of the guild, returns a [guild](servidores.md#guild-object) object with the extra fields below. Otherwise, returns a 204 empty response. May fire a [Guild Create](https://docs.discord.food/topics/gateway-events#guild-create), [Guild Member Add](https://docs.discord.food/topics/gateway-events#guild-member-add), and/or [Guild Join Request Create](https://docs.discord.food/topics/gateway-events#guild-join-request-create) Gateway event.

For guilds with [member verification](servidores.md#member-verification-object) enabled, this endpoint will default to adding new members as in the [guild member](servidores.md#guild-member-object) object. Members that are will have to complete member verification before they become full members that can talk.

[**Query String Params**](servidores.md#query-string-params)

| Field                     | Type    | Description                                                                                                           |
| ------------------------- | ------- | --------------------------------------------------------------------------------------------------------------------- |
| lurker? <sup>1</sup>      | boolean | Whether the user will lurk the guild (default false)                                                                  |
| session\_id?              | string  | The session ID to lurk with, required for lurking                                                                     |
| location?                 | string  | The analytics location the request initiated from                                                                     |
| recommendation\_load\_id? | string  | The unique identifier for the current guild discovery recommendations (client-generated UUID as a hexadecimal string) |

<sup>1</sup> Lurking a guild allows the user to receive Gateway events for the guild without being a full member for the lifetime of the Gateway session. This is useful for previewing the guild before joining. Lurking requires the [guild feature](servidores.md#guild-features).

[**Response Body Extra Fields**](servidores.md#response-body-extra-fields)

| Field                     | Type                                                         | Description                                                                                                       |
| ------------------------- | ------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------- |
| show\_verification\_form? | boolean                                                      | Whether the user should be shown the guild's [member verification](servidores.md#member-verification-object) form |
| welcome\_screen?          | [welcome screen](servidores.md#welcome-screen-object) object | The guild's welcome screen, shown to new members when joining the guild                                           |

#### [Add Guild Member](servidores.md#add-guild-member) <a href="#add-guild-member" id="add-guild-member"></a>

`PUT/guilds/{guild.id}/members/{user.id}`

Adds a user to the guild, provided you have a valid OAuth2 access token for the user with the scope. Returns the joined [guild member](servidores.md#guild-member-object) object or a 204 empty response (if the user is already a member of the guild) on success. May fire a [Guild Member Add](https://docs.discord.food/topics/gateway-events#guild-member-add) and/or [Guild Join Request Create](https://docs.discord.food/topics/gateway-events#guild-join-request-create) Gateway event.

For guilds with [member verification](servidores.md#member-verification-object) enabled, this endpoint will default to adding new members as in the [guild member](servidores.md#guild-member-object) object. Members that are will have to complete member verification before they become full members that can talk. Note that this endpoint ignores whether [guild previewing](servidores.md#guild-previewing) is enabled and will always join the user as a member.

[**JSON Params**](servidores.md#json-params)

| Field               | Type              | Description                                                                                                              | Permission                                                                  |
| ------------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------- |
| access\_token       | string            | An OAuth2 access token granted with the `guilds.join` to the bot's application for the user you want to add to the guild | `CREATE_INSTANT_INVITE`                                                     |
| nick?               | ?string           | The guild-specific nickname of the member (1-32 characters)                                                              | `MANAGE_NICKNAMES`                                                          |
| roles?              | array\[snowflake] | The role IDs assigned to this member                                                                                     | `MANAGE_ROLES`                                                              |
| mute?               | boolean           | Whether the member is muted in voice channels                                                                            | `MUTE_MEMBERS`                                                              |
| deaf?               | boolean           | Whether the user is deafened in voice channels                                                                           | `DEAFEN_MEMBERS`                                                            |
| flags? <sup>1</sup> | integer           | The [member's flags](servidores.md#guild-member-flags) (only `BYPASSES_VERIFICATION` can be set)                         | `MANAGE_GUILD` or (`MODERATE_MEMBERS` and `KICK_MEMBERS` and `BAN_MEMBERS`) |

<sup>1</sup> For guilds with member verification enabled, assigning the [guild member flag](servidores.md#guild-member-flags) will add the user as a full member ( is false in the [member object](servidores.md#guild-member-object)).

#### [Modify Guild Member](servidores.md#modify-guild-member) <a href="#modify-guild-member" id="modify-guild-member"></a>

`PATCH/guilds/{guild.id}/members/{user.id}`

Modifies attributes of a guild member. User must be a member of the guild. Returns the updated [guild member](servidores.md#guild-member-object) object on success. Fires a [Guild Member Update](https://docs.discord.food/topics/gateway-events#guild-member-update) Gateway event.

[**JSON Params**](servidores.md#json-params)

| Field                                        | Type               | Description                                                                                                                                                                                           | Permission                                                                  |
| -------------------------------------------- | ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| nick?                                        | ?string            | The guild-specific nickname of the member (1-32 characters)                                                                                                                                           | `MANAGE_NICKNAMES`                                                          |
| roles?                                       | array\[snowflake]  | The role IDs assigned to this member                                                                                                                                                                  | `MANAGE_ROLES`                                                              |
| mute? <sup>1</sup>                           | boolean            | Whether the member is muted in voice channels                                                                                                                                                         | `MUTE_MEMBERS`                                                              |
| deaf? <sup>1</sup>                           | boolean            | Whether the user is deafened in voice channels                                                                                                                                                        | `DEAFEN_MEMBERS`                                                            |
| channel\_id? <sup>1</sup>                    | snowflake          | The ID of the voice channel the user is connected to                                                                                                                                                  | `MOVE_MEMBERS`                                                              |
| communication\_disabled\_until? <sup>2</sup> | ?ISO8601 timestamp | When the user's [timeout](https://support.discord.com/hc/en-us/articles/4413305239191-Time-Out-FAQ) will expire and they will be able to communicate in the guild again (up to 28 days in the future) | `MODERATE_MEMBERS`                                                          |
| flags?                                       | integer            | The [member's flags](servidores.md#guild-member-flags) (only `BYPASSES_VERIFICATION` can be set)                                                                                                      | `MANAGE_GUILD` or (`MODERATE_MEMBERS` and `KICK_MEMBERS` and `BAN_MEMBERS`) |

<sup>1</sup> Requires the member to be connected to voice. When moving members to channels, the current user _must_ have permissions to both connect to the channel and have the permission. If the is set to , this will force the target user to be disconnected from voice.

<sup>2</sup> Guild administrators cannot be timed out. If a member is timed out and becomes an administrator before their timeout expires, the timeout will no longer have an effect.

#### [Modify Current Guild Member](servidores.md#modify-current-guild-member) <a href="#modify-current-guild-member" id="modify-current-guild-member"></a>

`PATCH/guilds/{guild.id}/members/@me`

Modifies the current user's member in the guild. Returns the updated [guild member](servidores.md#guild-member-object) object on success (including extra and fields). Fires a [Guild Member Update](https://docs.discord.food/topics/gateway-events#guild-member-update) Gateway event.

[**JSON Params**](servidores.md#json-params)

| Field                        | Type                                                         | Description                                                              | Permission        |
| ---------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------------------ | ----------------- |
| nick?                        | ?string                                                      | The guild-specific nickname of the member (1-32 characters)              | `CHANGE_NICKNAME` |
| avatar?                      | ?[image data](https://docs.discord.food/reference#cdn-data)  | The member's guild avatar; can only be changed for premium (Nitro) users |                   |
| avatar\_decoration\_id?      | ?snowflake                                                   | The ID of the member's guild avatar decoration                           |                   |
| avatar\_decoration\_sku\_id? | ?snowflake                                                   | The SKU ID of the member's guild avatar decoration                       |                   |
| collectibles?                | ?[collectibles](servidores.md#collectibles-structure) object | The member's equipped collectibles                                       |                   |
| pronouns?                    | ?string                                                      | The member's guild pronouns (max 40 characters)                          |                   |
| bio?                         | ?string                                                      | The member's guild bio; can only be changed for premium (Nitro) users    |                   |
| banner?                      | ?[image data](https://docs.discord.food/reference#cdn-data)  | The member's guild banner; can only be changed for premium (Nitro) users |                   |

[**Collectibles Structure**](servidores.md#collectibles-structure)

| Field      | Type                                                             | Description                                                                                           |
| ---------- | ---------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| nameplate? | ?[nameplate data](servidores.md#nameplate-data-structure) object | The member's [nameplate](https://support.discord.com/hc/en-us/articles/30408457944215-Nameplates-FAQ) |

[**Nameplate Data Structure**](servidores.md#nameplate-data-structure)

| Field    | Type      | Description                   |
| -------- | --------- | ----------------------------- |
| id?      | snowflake | The ID of the nameplate       |
| sku\_id? | snowflake | The ID of the nameplate's SKU |

#### [Modify Current Guild Member Nick](servidores.md#modify-current-guild-member-nick) <a href="#modify-current-guild-member-nick" id="modify-current-guild-member-nick"></a>

`PATCH/guilds/{guild.id}/members/@me/nick`

Modifies the current user's member in the guild. Returns the updated [guild member](servidores.md#guild-member-object) object on success (including extra and fields). Fires a [Guild Member Update](https://docs.discord.food/topics/gateway-events#guild-member-update) Gateway event.

See [Modify Current Guild Member](servidores.md#modify-current-guild-member) for more information.

#### [Modify Guild Member Profile](servidores.md#modify-guild-member-profile) <a href="#modify-guild-member-profile" id="modify-guild-member-profile"></a>

`PATCH/guilds/{guild.id}/profile/@me`

Modifies the current user's profile in the guild. Returns a [profile metadata](https://docs.discord.food/resources/user#profile-metadata-object) object on success. Fires a [Guild Member Update](https://docs.discord.food/topics/gateway-events#guild-member-update) Gateway event.

[**JSON Params**](servidores.md#json-params)

| Field                              | Type                                                        | Description                                                                                                                                             |
| ---------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| pronouns?                          | ?string                                                     | The member's guild pronouns (max 40 characters)                                                                                                         |
| bio?                               | ?string                                                     | The member's guild bio (max 190 characters); can only be changed for premium (Nitro) users                                                              |
| banner?                            | ?[image data](https://docs.discord.food/reference#cdn-data) | The member's guild banner; can only be changed for premium (Nitro) users                                                                                |
| accent\_color?                     | ?integer                                                    | The member's guild accent color as a hex integer; can only be changed for premium (Nitro) users                                                         |
| theme\_colors?                     | ?array\[integer, integer]                                   | The member's two guild theme colors encoded as an array of integers representing hexadecimal color codes; can only be changed for premium (Nitro) users |
| popout\_animation\_particle\_type? | ?snowflake                                                  | The member's guild profile popout animation particle type                                                                                               |
| emoji\_id?                         | ?snowflake                                                  | The member's guild profile emoji ID                                                                                                                     |
| profile\_effect\_id?               | ?snowflake                                                  | The member's guild profile effect ID                                                                                                                    |

#### [Add Guild Member Role](servidores.md#add-guild-member-role) <a href="#add-guild-member-role" id="add-guild-member-role"></a>

`PUT/guilds/{guild.id}/members/{user.id}/roles/{role.id}`

Adds a role to a [guild member](servidores.md#guild-member-object). Requires the permission. Returns a 204 empty response on success. Fires a [Guild Member Update](https://docs.discord.food/topics/gateway-events#guild-member-update) Gateway event.

#### [Remove Guild Member Role](servidores.md#remove-guild-member-role) <a href="#remove-guild-member-role" id="remove-guild-member-role"></a>

`DELETE/guilds/{guild.id}/members/{user.id}/roles/{role.id}`

Removes a role from a [guild member](servidores.md#guild-member-object). Requires the permission. Returns a 204 empty response on success. Fires a [Guild Member Update](https://docs.discord.food/topics/gateway-events#guild-member-update) Gateway event.

#### [Remove Guild Member](servidores.md#remove-guild-member) <a href="#remove-guild-member" id="remove-guild-member"></a>

`DELETE/guilds/{guild.id}/members/{user.id}`

Removes a [member](servidores.md#guild-member-object) from a guild. Requires the permission. Returns a 204 empty response on success. Fires a [Guild Member Remove](https://docs.discord.food/topics/gateway-events#guild-member-remove) Gateway event.

#### [Acknowledge DM Settings Upsell Modal](servidores.md#acknowledge-dm-settings-upsell-modal) <a href="#acknowledge-dm-settings-upsell-modal" id="acknowledge-dm-settings-upsell-modal"></a>

`POST/users/@me/guilds/{guild.id}/member/ack-dm-upsell-settings`

Adds the [member flag](servidores.md#guild-member-flags) to the current user. User must be a member of the guild. Returns a 204 empty response on success. Fires a [Guild Member Update](https://docs.discord.food/topics/gateway-events#guild-member-update) Gateway event.

#### [Get Guild Bans](servidores.md#get-guild-bans) <a href="#get-guild-bans" id="get-guild-bans"></a>

`GET/guilds/{guild.id}/bans`

Returns a list of [ban](servidores.md#ban-object) objects for the guild. Requires the permission.

[**Query String Params**](servidores.md#query-string-params)

| Field                | Type      | Description                                                |
| -------------------- | --------- | ---------------------------------------------------------- |
| before? <sup>1</sup> | snowflake | Get bans before this user ID                               |
| after? <sup>1</sup>  | snowflake | Get bans after this user ID                                |
| limit? <sup>2</sup>  | number    | Max number of bans to return (1-1000, default all or 1000) |

<sup>1</sup> Bans will always be returned in ascending order by user ID.

<sup>2</sup> Ban pagination for user accounts is currently optional. It must be explicitly opted into by specifying the query string parameter. Without specifying it, the and query string parameters will not work, and all bans will be returned by default. For bots, pagination is always enabled and 1000 bans are returned by default.

#### [Search Guild Bans](servidores.md#search-guild-bans) <a href="#search-guild-bans" id="search-guild-bans"></a>

`GET/guilds/{guild.id}/bans/search`

Returns a list of [ban](servidores.md#ban-object) objects whose username or display name contains a provided string. Requires the permission.

[**Query String Params**](servidores.md#query-string-params)

| Field  | Type    | Description                                                              |
| ------ | ------- | ------------------------------------------------------------------------ |
| query  | string  | Query to match username(s) and display name(s) against (1-32 characters) |
| limit? | integer | Max number of members to return (1-10, default 10)                       |

#### [Get Guild Ban](servidores.md#get-guild-ban) <a href="#get-guild-ban" id="get-guild-ban"></a>

`GET/guilds/{guild.id}/bans/{user.id}`

Returns a [ban](servidores.md#ban-object) object for the given user. Requires the permission.

#### [Create Guild Ban](servidores.md#create-guild-ban) <a href="#create-guild-ban" id="create-guild-ban"></a>

`PUT/guilds/{guild.id}/bans/{user.id}`

Creates a guild ban and optionally deletes previous messages sent by the banned user. Requires the permission. Returns a 204 empty response on success. Fires a [Guild Ban Add](https://docs.discord.food/topics/gateway-events#guild-ban-add) and optionally a [Guild Member Remove](https://docs.discord.food/topics/gateway-events#guild-member-remove) Gateway event.

[**JSON Params**](servidores.md#json-params)

| Field                                   | Type    | Description                                                    |
| --------------------------------------- | ------- | -------------------------------------------------------------- |
| delete\_message\_days? **(deprecated)** | integer | Number of days to delete messages for (0-7, default 0)         |
| delete\_message\_seconds?               | integer | Number of seconds to delete messages for (0-604800, default 0) |

#### [Bulk Guild Ban](servidores.md#bulk-guild-ban) <a href="#bulk-guild-ban" id="bulk-guild-ban"></a>

`POST/guilds/{guild.id}/bulk-ban`

Create multiple guild bans and optionally delete previous messages sent by the banned users. Requires both the and permissions. Fires multiple [Guild Ban Add](https://docs.discord.food/topics/gateway-events#guild-ban-add) and optionally [Guild Member Remove](https://docs.discord.food/topics/gateway-events#guild-member-remove) Gateway events.

[**JSON Params**](servidores.md#json-params)

| Field                     | Type              | Description                                                    |
| ------------------------- | ----------------- | -------------------------------------------------------------- |
| user\_ids                 | array\[snowflake] | The user IDs to ban (max 200)                                  |
| delete\_message\_seconds? | integer           | Number of seconds to delete messages for (0-604800, default 0) |

[**Response Body**](servidores.md#response-body)

| Field                      | Type              | Description                                |
| -------------------------- | ----------------- | ------------------------------------------ |
| banned\_users              | array\[snowflake] | The user IDs that were successfully banned |
| failed\_users <sup>1</sup> | array\[snowflake] | The user IDs that were not banned          |

<sup>1</sup> A ban will fail if the user is already banned, the user has a higher role than the current user, the user is the owner of the guild, or the user is the current user. If a bulk ban has no successful bans, the response will be an error.

#### [Delete Guild Ban](servidores.md#delete-guild-ban) <a href="#delete-guild-ban" id="delete-guild-ban"></a>

`DELETE/guilds/{guild.id}/bans/{user.id}`

Removes the ban for a user. Requires the permission. Returns a 204 empty response on success. Fires a [Guild Ban Remove](https://docs.discord.food/topics/gateway-events#guild-ban-remove) Gateway event.

#### [Get Guild Roles](servidores.md#get-guild-roles) <a href="#get-guild-roles" id="get-guild-roles"></a>

`GET/guilds/{guild.id}/roles`

Returns a list of [role](servidores.md#role-object) objects for the guild. User must be a member of the guild.

#### [Get Guild Role](servidores.md#get-guild-role) <a href="#get-guild-role" id="get-guild-role"></a>

`GET/guilds/{guild.id}/roles/{role.id}`

Returns a [role](servidores.md#role-object) object for the given role. User must be a member of the guild.

#### [Get Guild Role Member Counts](servidores.md#get-guild-role-member-counts) <a href="#get-guild-role-member-counts" id="get-guild-role-member-counts"></a>

`GET/guilds/{guild.id}/roles/member-counts`

Returns a mapping of role IDs to their respective member counts. User must be a member of the guild.

[**Example Response**](servidores.md#example-response)

```
{  "1040221495437299782": 2,  "1040221495437299783": 1,  "1040221495437299784": 0}
```

#### [Get Guild Role Members](servidores.md#get-guild-role-members) <a href="#get-guild-role-members" id="get-guild-role-members"></a>

`GET/guilds/{guild.id}/roles/{role.id}/member-ids`

Returns a list of member IDs that have the specified [role](servidores.md#role-object), up to a maximum of 100. User must be a member of the guild.

[**Example Response**](servidores.md#example-response)

```
["852892297661906993", "907489667895676928"]
```

#### [Add Guild Role Members](servidores.md#add-guild-role-members) <a href="#add-guild-role-members" id="add-guild-role-members"></a>

`PATCH/guilds/{guild.id}/roles/{role.id}/members`

Adds multiple [guild members](servidores.md#guild-member-object) to a [role](servidores.md#role-object). Requires the permission. Returns a mapping of member IDs to [guild member](servidores.md#guild-member-object) objects. Fires multiple [Guild Member Update](https://docs.discord.food/topics/gateway-events#guild-member-update) Gateway events.

[**JSON Params**](servidores.md#json-params)

| Field       | Type              | Description                                   |
| ----------- | ----------------- | --------------------------------------------- |
| member\_ids | array\[snowflake] | The member IDs to assign the role to (max 30) |

[**Example Response**](servidores.md#example-response)

```
{  "863406480111566858": {    "avatar": null,    "banner": null,    "communication_disabled_until": null,    "unusual_dm_activity_until": null,    "flags": 0,    "joined_at": "2022-10-11T12:31:03.882000+00:00",    "nick": ":~]",    "pending": false,    "premium_since": null,    "roles": ["1040221495437299782"],    "user": {      "id": "863406480111566858",      "username": "leaduck",      "global_name": "LeaDuck",      "avatar": "bb450561133bac3da5c7e201db40af6c",      "discriminator": "0",      "public_flags": 256,      "avatar_decoration_data": null,      "primary_guild": null    },    "mute": false,    "deaf": false  }}
```

#### [Get Guild Role Connections Configurations](servidores.md#get-guild-role-connections-configurations) <a href="#get-guild-role-connections-configurations" id="get-guild-role-connections-configurations"></a>

`GET/guilds/{guild.id}/roles/connections-configurations`

Returns a list of [role connection rule](servidores.md#role-connection-rule-structure) objects representing the role connections for the guild. User must be a member of the guild.

[**Role Connection Rule Structure**](servidores.md#role-connection-rule-structure)

| Field        | Type                                                                                                                       | Description                              |
| ------------ | -------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| role\_id     | snowflake                                                                                                                  | The ID of the linkable role              |
| rules        | [role connection configuration](servidores.md#role-connection-configuration-object) object                                 | The requirements for the linkable role   |
| applications | map\[snowflake, [integration application](https://docs.discord.food/resources/integration#integration-application-object)] | The applications referenced in the rules |

#### [Get Guild Role Connection Configuration](servidores.md#get-guild-role-connection-configuration) <a href="#get-guild-role-connection-configuration" id="get-guild-role-connection-configuration"></a>

`GET/guilds/{guild.id}/roles/{role.id}/connections/configuration`

Returns a [role connection configuration](servidores.md#role-connection-configuration-object) object representing the role connection for the given role. Requires the permission.

#### [Modify Guild Role Connection Configuration](servidores.md#modify-guild-role-connection-configuration) <a href="#modify-guild-role-connection-configuration" id="modify-guild-role-connection-configuration"></a>

`PUT/guilds/{guild.id}/roles/{role.id}/connections/configuration`

Replaces the [role connection configuration](servidores.md#role-connection-configuration-object) for the given role. Requires the permission. Accepts a [role connection configuration](servidores.md#role-connection-configuration-object) object. Returns the updated [role connection configuration](servidores.md#role-connection-configuration-object) object on success.

#### [Get Guild Role Connection Eligibility](servidores.md#get-guild-role-connection-eligibility) <a href="#get-guild-role-connection-eligibility" id="get-guild-role-connection-eligibility"></a>

`GET/guilds/{guild.id}/roles/{role.id}/connections/eligibility`

Returns a [role connection configuration](servidores.md#role-connection-configuration-object) object with extra fields representing the user's eligibility to link the given role. User must be a member of the guild.

#### [Assign Guild Role Connection](servidores.md#assign-guild-role-connection) <a href="#assign-guild-role-connection" id="assign-guild-role-connection"></a>

`PUT/guilds/{guild.id}/roles/{role.id}/connections/assign`

Assigns an eligibile role connection to the current user. Returns a 204 empty response on success. Fires a [Guild Member Update](https://docs.discord.food/topics/gateway-events#guild-member-update) Gateway event.

#### [Unassign Guild Role Connection](servidores.md#unassign-guild-role-connection) <a href="#unassign-guild-role-connection" id="unassign-guild-role-connection"></a>

`POST/guilds/{guild.id}/roles/{role.id}/connections/unassign`

Unassigns a role connection from the current user. Returns a 204 empty response on success. Fires a [Guild Member Update](https://docs.discord.food/topics/gateway-events#guild-member-update) Gateway event.

[**Response Body**](servidores.md#response-body)

#### [Create Guild Role](servidores.md#create-guild-role) <a href="#create-guild-role" id="create-guild-role"></a>

`POST/guilds/{guild.id}/roles`

Creates a new role for the guild. Requires the permission. Returns the new [role](servidores.md#role-object) object on success. Fires a [Guild Role Create](https://docs.discord.food/topics/gateway-events#guild-role-create) Gateway event.

[**JSON Params**](servidores.md#json-params)

| Field                                | Type                                                        | Description                                                                            |
| ------------------------------------ | ----------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| name?                                | ?string                                                     | The name of the role (max 100 characters, default "new role")                          |
| description?                         | ?string                                                     | The description for the role (max 90 characters)                                       |
| color? **(deprecated)** <sup>1</sup> | ?integer                                                    | Integer representation of a hexadecimal color code for the role                        |
| colors? <sup>1</sup> <sup>2</sup>    | ?[role colors](servidores.md#role-colors-structure) object  | The colors of the role encoded as an integer representation of hexadecimal color codes |
| hoist?                               | boolean                                                     | Whether this role is pinned in the user listing (default false)                        |
| icon?                                | ?[image data](https://docs.discord.food/reference#cdn-data) | The role's icon                                                                        |
| unicode\_emoji?                      | ?string                                                     | The role's unicode emoji                                                               |
| permissions?                         | ?string                                                     | The permission bitwise value for the role (default @everyone permissions)              |
| mentionable?                         | boolean                                                     | Whether this role is mentionable (default false)                                       |

<sup>1</sup> If both and are provided, the field will be ignored.

<sup>2</sup> Requires the [guild feature](servidores.md#guild-features).

#### [Modify Guild Role Positions](servidores.md#modify-guild-role-positions) <a href="#modify-guild-role-positions" id="modify-guild-role-positions"></a>

`PATCH/guilds/{guild.id}/roles`

Modifies the positions of a set of [role](servidores.md#role-object) objects for the guild. Requires the permission. Returns a list of all of the guild's [role](servidores.md#role-object) objects on success. Fires multiple [Guild Role Update](https://docs.discord.food/topics/gateway-events#guild-role-update) Gateway events.

This endpoint takes a JSON array of parameters in the following format:

[**JSON Params**](servidores.md#json-params)

| Field     | Type      | Description                  |
| --------- | --------- | ---------------------------- |
| id        | snowflake | The ID of the role           |
| position? | ?integer  | Sorting position of the role |

#### [Modify Guild Role](servidores.md#modify-guild-role) <a href="#modify-guild-role" id="modify-guild-role"></a>

`PATCH/guilds/{guild.id}/roles/{role.id}`

Modifies a guild role. Requires the permission. Returns the updated [role](servidores.md#role-object) on success. Fires a [Guild Role Update](https://docs.discord.food/topics/gateway-events#guild-role-update) Gateway event.

[**JSON Params**](servidores.md#json-params)

| Field                                | Type                                                        | Description                                                                            |
| ------------------------------------ | ----------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| name?                                | ?string                                                     | The name of the role (max 100 characters, default "new role" if `null`)                |
| description?                         | ?string                                                     | The description for the role (max 90 characters)                                       |
| color? **(deprecated)** <sup>1</sup> | ?integer                                                    | Integer representation of a hexadecimal color code for the role                        |
| colors? <sup>1</sup> <sup>2</sup>    | ?[role colors](servidores.md#role-colors-structure) object  | The colors of the role encoded as an integer representation of hexadecimal color codes |
| hoist?                               | ?boolean                                                    | Whether this role is pinned in the user listing                                        |
| icon?                                | ?[image data](https://docs.discord.food/reference#cdn-data) | The role's icon                                                                        |
| unicode\_emoji?                      | ?string                                                     | The role's unicode emoji                                                               |
| permissions?                         | ?string                                                     | The permission bitwise value for the role (default @everyone permissions if `null`)    |
| mentionable?                         | ?boolean                                                    | Whether this role is mentionable                                                       |

<sup>1</sup> If both and are provided, the field will be ignored.

<sup>2</sup> Requires the [guild feature](servidores.md#guild-features).

#### [Delete Guild Role](servidores.md#delete-guild-role) <a href="#delete-guild-role" id="delete-guild-role"></a>

`DELETE/guilds/{guild.id}/roles/{role.id}`

Deletes a guild role. Requires the permission. Returns a 204 empty response on success. Fires a [Guild Role Delete](https://docs.discord.food/topics/gateway-events#guild-role-delete) Gateway event.

#### [Get Guild Prune](servidores.md#get-guild-prune) <a href="#get-guild-prune" id="get-guild-prune"></a>

`GET/guilds/{guild.id}/prune`

Returns the number of members that would be removed in a prune operation. Requires both the and permissions.

By default, prune will not remove users with roles. You can optionally include specific roles in your prune by providing the parameter. Any inactive user that has a subset of the provided role(s) will be counted in the prune and users with additional roles will not.

[**Query String Params**](servidores.md#query-string-params)

| Field           | Type              | Description                                                  |
| --------------- | ----------------- | ------------------------------------------------------------ |
| days?           | integer           | Number of inactive days to count prune for (1-30, default 7) |
| include\_roles? | array\[snowflake] | Additional roles to include                                  |

[**Response Body**](servidores.md#response-body)

| Field  | Type    | Description                                                                                   |
| ------ | ------- | --------------------------------------------------------------------------------------------- |
| pruned | integer | The number of members that would be removed in a prune operation with the provided parameters |

[**Example Response**](servidores.md#example-response)

```
{  "pruned": 42}
```

#### [Prune Guild](servidores.md#prune-guild) <a href="#prune-guild" id="prune-guild"></a>

`POST/guilds/{guild.id}/prune`

Begins a prune operation. Requires both the and permissions. For large guilds, it's recommended to set the option to , allowing the request to return before all members are pruned. Fires multiple [Guild Member Remove](https://docs.discord.food/topics/gateway-events#guild-member-remove) Gateway events.

By default, prune will not remove users with roles. You can optionally include specific roles in your prune by providing the parameter. Any inactive user that has a subset of the provided role(s) will be included in the prune and users with additional roles will not.

[**JSON Params**](servidores.md#json-params)

| Field                  | Type              | Description                                                                |
| ---------------------- | ----------------- | -------------------------------------------------------------------------- |
| days?                  | integer           | Number of inactive days to prune for (1-30, default 7)                     |
| compute\_prune\_count? | boolean           | Whether to wait for the prune to complete before responding (default true) |
| include\_roles?        | array\[snowflake] | Additional roles to include                                                |

[**Response Body**](servidores.md#response-body)

| Field  | Type     | Description                                                                                                         |
| ------ | -------- | ------------------------------------------------------------------------------------------------------------------- |
| pruned | ?integer | The number of members that were removed in the prune operation with the provided parameters; `null` if not computed |

[**Example Response**](servidores.md#example-response)

```
{  "pruned": null}
```

#### [Get Guild Widget Settings](servidores.md#get-guild-widget-settings) <a href="#get-guild-widget-settings" id="get-guild-widget-settings"></a>

`GET/guilds/{guild.id}/widget`

Returns a [guild widget settings](servidores.md#guild-widget-settings-structure) object for the guild. Requires the permission.

#### [Modify Guild Widget](servidores.md#modify-guild-widget) <a href="#modify-guild-widget" id="modify-guild-widget"></a>

`PATCH/guilds/{guild.id}/widget`

Modifies the widget settings for the guild. Requires the permission. Returns the updated [guild widget settings](servidores.md#guild-widget-settings-structure) object on success.

[**JSON Params**](servidores.md#json-params)

| Field        | Type       | Description                                                       |
| ------------ | ---------- | ----------------------------------------------------------------- |
| enabled?     | boolean    | Whether the widget is enabled                                     |
| channel\_id? | ?snowflake | The channel ID that the widget will generate an invite to, if any |

#### [Get Guild Widget](servidores.md#get-guild-widget) <a href="#get-guild-widget" id="get-guild-widget"></a>

`GET/guilds/{guild.id}/widget.json`

Returns a [guild widget](servidores.md#guild-widget-object) object for the given guild ID. The guild must have the widget enabled. May fire an [Invite Create](https://docs.discord.food/topics/gateway-events#invite-create) Gateway event.

#### [Get Guild Widget Image](servidores.md#get-guild-widget-image) <a href="#get-guild-widget-image" id="get-guild-widget-image"></a>

`GET/guilds/{guild.id}/widget.png`

Returns a widget image PNG for the given guild ID. The guild must have the widget enabled.

[**Query String Params**](servidores.md#query-string-params)

| Field  | Type   | Description                                                                                        |
| ------ | ------ | -------------------------------------------------------------------------------------------------- |
| style? | string | [Style of widget image](servidores.md#guild-widget-image-style-option) returned (default `shield`) |

[**Guild Widget Image Style Option**](servidores.md#guild-widget-image-style-option)

| Value   | Description                                                                                                                                                    | Example                                                                              |
| ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| shield  | Shield style widget with Discord icon and online count                                                                                                         | [Example](https://discord.com/api/guilds/81384788765712384/widget.png?style=shield)  |
| banner1 | Large image with guild icon, name and online count; "POWERED BY DISCORD" as the footer of the widget                                                           | [Example](https://discord.com/api/guilds/81384788765712384/widget.png?style=banner1) |
| banner2 | Smaller widget style with guild icon, name and online count; split on the right with Discord logo                                                              | [Example](https://discord.com/api/guilds/81384788765712384/widget.png?style=banner2) |
| banner3 | Large image with guild icon, name and online count; in the footer, Discord logo on the left and "Chat Now" on the right                                        | [Example](https://discord.com/api/guilds/81384788765712384/widget.png?style=banner3) |
| banner4 | Large Discord logo at the top of the widget; guild icon, name and online count in the middle portion of the widget and a "JOIN MY SERVER" button at the bottom | [Example](https://discord.com/api/guilds/81384788765712384/widget.png?style=banner4) |

#### [Get Guild Vanity Invite](servidores.md#get-guild-vanity-invite) <a href="#get-guild-vanity-invite" id="get-guild-vanity-invite"></a>

`GET/guilds/{guild.id}/vanity-url`

Returns the vanity invite for the guild. The guild must have the or feature. Requires the permission.

[**Response Body**](servidores.md#response-body)

| Field | Type    | Description                                   |
| ----- | ------- | --------------------------------------------- |
| code  | ?string | The vanity invite code for the guild          |
| uses  | integer | The number of times this invite has been used |

[**Example Response**](servidores.md#example-response)

```
{  "code": "abc",  "uses": 12}
```

#### [Modify Guild Vanity Invite](servidores.md#modify-guild-vanity-invite) <a href="#modify-guild-vanity-invite" id="modify-guild-vanity-invite"></a>

`PATCH/guilds/{guild.id}/vanity-url`

Modifies the vanity invite for the guild. The guild must have the or feature. Guilds without the feature can only clear their vanity invite. Requires both the and permissions. Fires a [Guild Update](https://docs.discord.food/topics/gateway-events#guild-update) Gateway event.

[**JSON Params**](servidores.md#json-params)

| Field | Type    | Description                                                                  |
| ----- | ------- | ---------------------------------------------------------------------------- |
| code  | ?string | The vanity invite code for the guild (2-25 characters, alphanumeric and `-`) |

[**Response Body**](servidores.md#response-body)

| Field | Type    | Description                                           |
| ----- | ------- | ----------------------------------------------------- |
| code  | ?string | The vanity invite code for the guild                  |
| uses  | integer | The number of times this invite has been used (now 0) |

#### [Get Guild Member Verification](servidores.md#get-guild-member-verification) <a href="#get-guild-member-verification" id="get-guild-member-verification"></a>

`GET/guilds/{guild.id}/member-verification`

Returns the [member verification](servidores.md#member-verification-object) object for the guild if one is set. If the user is not in the guild, the guild must be discoverable or have [guild previewing](servidores.md#guild-previewing) disabled.

[**Query String Params**](servidores.md#query-string-params)

| Field                     | Type    | Description                                                         |
| ------------------------- | ------- | ------------------------------------------------------------------- |
| with\_guild? <sup>1</sup> | boolean | Whether to include the guild object in the response (default false) |
| invite\_code?             | string  | The invite code the verification is fetched from                    |

<sup>1</sup> Requires that the user is not a member of the guild, and that the guild is not full.

#### [Modify Guild Member Verification](servidores.md#modify-guild-member-verification) <a href="#modify-guild-member-verification" id="modify-guild-member-verification"></a>

`PATCH/guilds/{guild.id}/member-verification`

Modifies the member verification for the guild. Requires the permission. Returns the updated [member verification](servidores.md#member-verification-object) object. May fire a [Guild Update](https://docs.discord.food/topics/gateway-events#guild-update) Gateway event.

[**JSON Params**](servidores.md#json-params)

| Field         | Type                                                                                                    | Description                                                                                                       |
| ------------- | ------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| enabled?      | boolean                                                                                                 | Whether the member verification gate is enabled                                                                   |
| form\_fields? | array\[[member verification form field](servidores.md#member-verification-form-field-structure) object] | Questions for applicants to answer (max 5)                                                                        |
| description?  | ?string                                                                                                 | A description of what the guild is about; this can be different than the guild's description (max 300 characters) |

#### [Get Guild Join Requests](servidores.md#get-guild-join-requests) <a href="#get-guild-join-requests" id="get-guild-join-requests"></a>

`GET/guilds/{guild.id}/requests`

Returns a list of join requests for the guild for manual approval. Requires the permission.

[**Query String Params**](servidores.md#query-string-params)

| Field   | Type      | Description                                                                             |
| ------- | --------- | --------------------------------------------------------------------------------------- |
| status  | string    | The [status of the join requests](servidores.md#guild-join-request-status) to filter by |
| limit?  | integer   | Max number of join requests to return (1-100, default 100)                              |
| before? | snowflake | Get join requests before this request ID                                                |
| after?  | snowflake | Get join requests after this request ID                                                 |

[**Response Body**](servidores.md#response-body)

| Field                               | Type                                                                  | Description                                            |
| ----------------------------------- | --------------------------------------------------------------------- | ------------------------------------------------------ |
| guild\_join\_requests? <sup>1</sup> | array\[[guild join request](servidores.md#guild-join-request-object)] | The join requests for the guild                        |
| total?                              | integer                                                               | The total number of join requests that match the query |
| limit                               | integer                                                               | The maximum number of join requests returned           |

<sup>1</sup> Join requests are omitted when retrieving requests with the [status](servidores.md#guild-join-request-status).

#### [Get Guild Join Request](servidores.md#get-guild-join-request) <a href="#get-guild-join-request" id="get-guild-join-request"></a>

`GET/join-requests/{guild_join_request.id}`

Returns a [guild join request](servidores.md#guild-join-request-object) object for the given request ID. Requires the permission if the request is not for the current user.

#### [Get Guild Join Request Cooldown](servidores.md#get-guild-join-request-cooldown) <a href="#get-guild-join-request-cooldown" id="get-guild-join-request-cooldown"></a>

`GET/guilds/{guild.id}/requests/@me/cooldown`

Returns the remaining time until the current user can submit another join request for the guild.

[**Response Body**](servidores.md#response-body)

| Field    | Type    | Description                                                                                 |
| -------- | ------- | ------------------------------------------------------------------------------------------- |
| cooldown | integer | How long (in seconds) the user has to wait until the current user can submit a join request |

#### [Create Guild Join Request](servidores.md#create-guild-join-request) <a href="#create-guild-join-request" id="create-guild-join-request"></a>

`PUT/guilds/{guild.id}/requests/@me`

Submits a request to join a guild. Returns a partial [guild join request](servidores.md#guild-join-request-object) object on success. Fires a [Guild Join Request Create](https://docs.discord.food/topics/gateway-events#guild-join-request-create) or [Guild Join Request Update](https://docs.discord.food/topics/gateway-events#guild-join-request-update) Gateway event.

[**JSON Params**](servidores.md#json-params)

| Field                     | Type                                                                                                    | Description                                                                                                                           |
| ------------------------- | ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| form\_fields <sup>1</sup> | array\[[member verification form field](servidores.md#member-verification-form-field-structure) object] | The answered member verification questions                                                                                            |
| version                   | ?ISO8601 timestamp                                                                                      | When the member verification was last modified, same as [in the member verification object](servidores.md#member-verification-object) |

<sup>1</sup> The array must contain all fields from the guild's [member verification](servidores.md#member-verification-object) object, with a populated field for each field.

#### [Reset Guild Join Request](servidores.md#reset-guild-join-request) <a href="#reset-guild-join-request" id="reset-guild-join-request"></a>

`POST/guilds/{guild.id}/requests/@me`

Resets the current user's join request for the guild. Returns a partial [guild join request](servidores.md#guild-join-request-object) object on success. Fires a [Guild Join Request Delete](https://docs.discord.food/topics/gateway-events#guild-join-request-delete) and [Guild Join Request Create](https://docs.discord.food/topics/gateway-events#guild-join-request-create) Gateway event.

#### [Acknowledge Guild Join Request](servidores.md#acknowledge-guild-join-request) <a href="#acknowledge-guild-join-request" id="acknowledge-guild-join-request"></a>

`POST/guilds/{guild.id}/requests/@me/ack OR /guilds/{guild.id}/requests/{guild_join_request.id}/ack`

Acknowledges an approved join request for the current user. Returns a 204 empty response on success. Fires a [Guild Join Request Update](https://docs.discord.food/topics/gateway-events#guild-join-request-update) Gateway event.

#### [Delete Guild Join Request](servidores.md#delete-guild-join-request) <a href="#delete-guild-join-request" id="delete-guild-join-request"></a>

`DELETE/guilds/{guild.id}/requests/@me`

If the guild has [previewing disabled](servidores.md#guild-previewing), deletes the current user's join request. Else, functions the same as [Reset Guild Join Request](servidores.md#reset-guild-join-request). Returns a 204 empty response if deletion is successful or a partial [guild join request](servidores.md#guild-join-request-object) object if the join request is reset. Fires a [Guild Join Request Delete](https://docs.discord.food/topics/gateway-events#guild-join-request-delete) and optionally a [Guild Join Request Create](https://docs.discord.food/topics/gateway-events#guild-join-request-create) Gateway event.

#### [Create Guild Join Request Interview](servidores.md#create-guild-join-request-interview) <a href="#create-guild-join-request-interview" id="create-guild-join-request-interview"></a>

`POST/join-requests/{guild_join_request.id}/interview`

Creates or returns an existing private interview channel for the join request. Requires the permission. Returns a [group DM channel](https://docs.discord.food/resources/channel#channel-object) object on success. Fires a [Guild Join Request Update](https://docs.discord.food/topics/gateway-events#guild-join-request-update) and [Channel Create](https://docs.discord.food/topics/gateway-events#channel-create) Gateway event.

#### [Action Guild Join Request](servidores.md#action-guild-join-request) <a href="#action-guild-join-request" id="action-guild-join-request"></a>

`PATCH/guilds/{guild.id}/requests/id/{guild_join_request.id}`

Accepts or denies a join request for the guild. Requires the permission. Returns a [guild join request](servidores.md#guild-join-request-object) object on success. Fires a [Guild Join Request Update](https://docs.discord.food/topics/gateway-events#guild-join-request-update) and optionally a [Guild Member Add](https://docs.discord.food/topics/gateway-events#guild-member-add) or [Guild Member Remove](https://docs.discord.food/topics/gateway-events#guild-member-remove) Gateway event.

[**JSON Params**](servidores.md#json-params)

| Field              | Type    | Description                                                                                                                     |
| ------------------ | ------- | ------------------------------------------------------------------------------------------------------------------------------- |
| action             | string  | The [action to take](servidores.md#guild-join-request-status) on the join requests (only `APPROVED` and `REJECTED` can be used) |
| rejection\_reason? | ?string | The reason for rejecting the join request (max 160 characters)                                                                  |

#### [Action Guild Join Request by User](servidores.md#action-guild-join-request-by-user) <a href="#action-guild-join-request-by-user" id="action-guild-join-request-by-user"></a>

`PATCH/guilds/{guild.id}/requests/{user.id}`

Same as above, except this endpoint is keyed by the user ID instead of the guild join request ID.

#### [Bulk Action Guild Join Requests](servidores.md#bulk-action-guild-join-requests) <a href="#bulk-action-guild-join-requests" id="bulk-action-guild-join-requests"></a>

`PATCH/guilds/{guild.id}/requests`

Accepts or denies all pending join requests for the guild. Requires the permission. Returns a 204 empty response on success. May fire multiple [Guild Join Request Update](https://docs.discord.food/topics/gateway-events#guild-join-request-update), [Guild Member Add](https://docs.discord.food/topics/gateway-events#guild-member-add), and [Guild Member Remove](https://docs.discord.food/topics/gateway-events#guild-member-remove) Gateway events.

[**JSON Params**](servidores.md#json-params)

| Field  | Type   | Description                                                                                                                     |
| ------ | ------ | ------------------------------------------------------------------------------------------------------------------------------- |
| action | string | The [action to take](servidores.md#guild-join-request-status) on the join requests (only `APPROVED` and `REJECTED` can be used) |

#### [Get Guild Welcome Screen](servidores.md#get-guild-welcome-screen) <a href="#get-guild-welcome-screen" id="get-guild-welcome-screen"></a>

`GET/guilds/{guild.id}/welcome-screen`

Returns the [welcome screen](servidores.md#welcome-screen-object) object for the guild. Requires the permission if the welcome screen is not yet enabled, otherwise no permission is required.

#### [Modify Guild Welcome Screen](servidores.md#modify-guild-welcome-screen) <a href="#modify-guild-welcome-screen" id="modify-guild-welcome-screen"></a>

`PATCH/guilds/{guild.id}/welcome-screen`

Modifies the guild's welcome screen. Requires the permission. Returns the updated [welcome screen](servidores.md#welcome-screen-object) object. May fire a [Guild Update](https://docs.discord.food/topics/gateway-events#guild-update) Gateway event.

[**JSON Params**](servidores.md#json-params)

| Field              | Type                                                                                     | Description                                                          |
| ------------------ | ---------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| enabled?           | ?boolean                                                                                 | Whether the welcome screen is enabled                                |
| description?       | ?string                                                                                  | The welcome message shown in the welcome screen (max 140 characters) |
| welcome\_channels? | ?array\[[welcome screen channel](servidores.md#welcome-screen-channel-structure) object] | The channels shown in the welcome screen (max 5)                     |

#### [Get Guild Onboarding](servidores.md#get-guild-onboarding) <a href="#get-guild-onboarding" id="get-guild-onboarding"></a>

`GET/guilds/{guild.id}/onboarding`

Returns the [onboarding](servidores.md#onboarding-object) object for the guild. Requires the permission if the feature is disabled, otherwise requires that the user is a member of the guild.

#### [Modify Guild Onboarding](servidores.md#modify-guild-onboarding) <a href="#modify-guild-onboarding" id="modify-guild-onboarding"></a>

`PUT/guilds/{guild.id}/onboarding`

Modifies the onboarding configuration of the guild. Returns the updated [onboarding](servidores.md#onboarding-object) object. Requires the permission. Fires a [Guild Update](https://docs.discord.food/topics/gateway-events#guild-update) Gateway event.

[**JSON Params**](servidores.md#json-params)

| Field                  | Type                                                                          | Description                                                               |
| ---------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| prompts?               | array\[[onboarding prompt](servidores.md#onboarding-prompt-structure) object] | The prompts shown during onboarding and in community customization        |
| default\_channel\_ids? | array\[snowflake]                                                             | The channel IDs that members get opted into automatically                 |
| enabled?               | boolean                                                                       | Whether onboarding is enabled in the guild                                |
| mode?                  | integer                                                                       | The current [criteria mode](servidores.md#onboarding-mode) for onboarding |

#### [Get Guild New Member Welcome](servidores.md#get-guild-new-member-welcome) <a href="#get-guild-new-member-welcome" id="get-guild-new-member-welcome"></a>

`GET/guilds/{guild.id}/new-member-welcome`

If it exists, returns the [new member welcome](servidores.md#new-member-welcome-object) object for the guild. Otherwise, returns a 204 empty response. Requires the permission if the feature is disabled, otherwise no permission is required.

#### [Modify Guild New Member Welcome](servidores.md#modify-guild-new-member-welcome) <a href="#modify-guild-new-member-welcome" id="modify-guild-new-member-welcome"></a>

`PUT/guilds/{guild.id}/new-member-welcome`

Modifies the guild's new member welcome configuration. Requires the permission. Returns the updated [new member welcome](servidores.md#new-member-welcome-object) object. May fire a [Guild Update](https://docs.discord.food/topics/gateway-events#guild-update) Gateway event.

[**JSON Params**](servidores.md#json-params)

| Field                              | Type                                                                                    | Description                                                       |
| ---------------------------------- | --------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| enabled?                           | boolean                                                                                 | Whether the new member welcome experience is enabled              |
| welcome\_message?                  | [new member welcome message](servidores.md#new-member-welcome-message-structure) object | Welcome message shown to new members of the guild                 |
| new\_member\_actions? <sup>1</sup> | array\[partial [new member action](servidores.md#new-member-action-structure) object]   | Actions shown to new members of the guild (max 5)                 |
| resource\_channels? <sup>2</sup>   | array\[partial [resource channel](servidores.md#resource-channel-structure) object]     | Read-only channels that provide resources for new members (max 7) |

<sup>1</sup> Only the , , and fields are required. cannot be set.

<sup>2</sup> Only the and fields are required. cannot be set.

#### [Modify Guild New Member Action](servidores.md#modify-guild-new-member-action) <a href="#modify-guild-new-member-action" id="modify-guild-new-member-action"></a>

`PATCH/guilds/{guild.id}/new-member-actions/{channel.id}`

Modifies a new member action for the guild. Requires the permission. Returns the updated [new member action](servidores.md#new-member-action-structure) object on success.

[**JSON Params**](servidores.md#json-params)

| Field | Type                                                        | Description                                  |
| ----- | ----------------------------------------------------------- | -------------------------------------------- |
| icon? | ?[image data](https://docs.discord.food/reference#cdn-data) | The new member action's icon (max 10000 KiB) |

#### [Modify Guild Resource Channel](servidores.md#modify-guild-resource-channel) <a href="#modify-guild-resource-channel" id="modify-guild-resource-channel"></a>

`PUT/guilds/{guild.id}/resource-channels/{channel.id}`

Modifies a resource channel for the guild. Requires the permission. Returns the updated [resource channel](servidores.md#resource-channel-structure) object on success.

[**JSON Params**](servidores.md#json-params)

| Field | Type                                                        | Description                                 |
| ----- | ----------------------------------------------------------- | ------------------------------------------- |
| icon? | ?[image data](https://docs.discord.food/reference#cdn-data) | The resource channel's icon (max 10000 KiB) |

#### [Get Guild New Member Actions](servidores.md#get-guild-new-member-actions) <a href="#get-guild-new-member-actions" id="get-guild-new-member-actions"></a>

`GET/guilds/{guild.id}/new-member-actions`

If it exists, returns a [new member actions progress](servidores.md#new-member-actions-progress-object) object for the user in the guild, representing the user's progress towards completing the new member actions. Otherwise, returns a 204 empty response.

#### [Complete Guild New Member Action](servidores.md#complete-guild-new-member-action) <a href="#complete-guild-new-member-action" id="complete-guild-new-member-action"></a>

`POST/guilds/{guild.id}/new-member-action/{channel.id}`

Completes a new member action for the user in the guild. Returns the updated [new member actions progress](servidores.md#new-member-actions-progress-object) object on success. May fire a [Guild Member Update](https://docs.discord.food/topics/gateway-events#guild-member-update) Gateway event.

#### [Get Guild Top Games](servidores.md#get-guild-top-games) <a href="#get-guild-top-games" id="get-guild-top-games"></a>

`GET/guilds/{guild.id}/top-games`

Returns up to 20 of the top most played games for the guild. Requires the permission.

[**Response Body**](servidores.md#response-body)

| Field      | Type                                                                  | Description                       |
| ---------- | --------------------------------------------------------------------- | --------------------------------- |
| top\_games | array\[[game activity](servidores.md#game-activity-structure) object] | The top games played in the guild |

[**Game Activity Structure**](servidores.md#game-activity-structure)

| Field                 | Type      | Description                                     |
| --------------------- | --------- | ----------------------------------------------- |
| game\_application\_id | snowflake | The ID of the application representing the game |
| activity\_level       | integer   | The activity level of the guild in the game     |
| activity\_score       | integer   | The activity score of the guild in the game     |

#### [Get Premium Guild Subscriptions](servidores.md#get-premium-guild-subscriptions) <a href="#get-premium-guild-subscriptions" id="get-premium-guild-subscriptions"></a>

`GET/guilds/{guild.id}/premium/subscriptions`

Returns a list of [premium guild subscription](servidores.md#premium-guild-subscription-object) objects for the guild. User must be a member of the guild.

#### [Get Guild Powerups](servidores.md#get-guild-powerups) <a href="#get-guild-powerups" id="get-guild-powerups"></a>

`GET/guilds/{guild.id}/powerups`

Returns a list of [entitlement](https://docs.discord.food/resources/entitlement#entitlement-object) objects representing the guild's applied powerups. Powerups are special SKUs that can be purchased using premium subscriptions (boosts) to enhance the guild's features. User must be a member of the guild.

[**Query String Params**](servidores.md#query-string-params)

| Field              | Type    | Description |
| ------------------ | ------- | ----------- |
| include\_ends\_at? | boolean | Unknown     |

#### [Add Guild Powerup](servidores.md#add-guild-powerup) <a href="#add-guild-powerup" id="add-guild-powerup"></a>

`POST/guilds/{guild.id}/skus/{sku.id}`

Adds the given powerup SKU to the guild. Requires the permission. Returns a 204 empty response on success. Fires a [Guild Update](https://docs.discord.food/topics/gateway-events#guild-update) and [Guild Powerup Entitlements Create](https://docs.discord.food/topics/gateway-events#guild-powerup-entitlements-create) Gateway event.

#### [Remove Guild Powerup](servidores.md#remove-guild-powerup) <a href="#remove-guild-powerup" id="remove-guild-powerup"></a>

`DELETE/guilds/{guild.id}/skus/{sku.id}`

Removes the given powerup SKU from the guild. Requires the permission. Returns a 204 empty response on success. Fires a [Guild Update](https://docs.discord.food/topics/gateway-events#guild-update) and [Guild Powerup Entitlements Delete](https://docs.discord.food/topics/gateway-events#guild-powerup-entitlements-delete) Gateway event.

`GET/guilds/{guild.id}/admin-server-eligibility`

Checks if the user is eligible to join the Discord Admin Community through the guild. Requires the permission.

[**Response Body**](servidores.md#response-body)

| Field                        | Type    | Description                                                                        |
| ---------------------------- | ------- | ---------------------------------------------------------------------------------- |
| eligible\_for\_admin\_server | boolean | Whether the user is eligible to join the Discord Admin Community through the guild |

`POST/guilds/{guild.id}/join-admin-server`

Joins the Discord Admin Community through the guild. Requires the permission. Returns the joined [guild](servidores.md#guild-object) object on success. Fires [Guild Create](https://docs.discord.food/topics/gateway-events#guild-create), [Guild Member Add](https://docs.discord.food/topics/gateway-events#guild-member-add), and optionally [Guild Join Request Create](https://docs.discord.food/topics/gateway-events#guild-join-request-create) Gateway events.

#### [Query Student Hubs](servidores.md#query-student-hubs) <a href="#query-student-hubs" id="query-student-hubs"></a>

`POST/guilds/automations/email-domain-lookup`

Queries the student hubs for the given email.

[**JSON Params**](servidores.md#json-params)

| Field                                 | Type      | Description                                                                                       |
| ------------------------------------- | --------- | ------------------------------------------------------------------------------------------------- |
| email                                 | string    | The email to lookup (max 320 characters)                                                          |
| use\_verification\_code? <sup>1</sup> | boolean   | Whether to email the user a code to verify their student status (default false)                   |
| allow\_multiple\_guilds? <sup>2</sup> | boolean   | Whether to return a list of guilds for the email instead of picking the first one (default false) |
| guild\_id? <sup>2</sup>               | snowflake | The guild ID to email a verification code for                                                     |

<sup>1</sup> should always be set to as the old behavior is deprecated and results in an email asking the user to update their client to join the student hub.

<sup>2</sup> should always be set to as the resultant guild ID is required for the [Join Student Hub](servidores.md#join-student-hub) endpoint. If is set to , the parameter is ignored and an email is sent for the first guild found. If is set to , a second request must be made with a provided to send the email.

[**Response Body**](servidores.md#response-body)

| Field                      | Type                                                                          | Description                                                                          |
| -------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| has\_matching\_guild       | boolean                                                                       | Whether a student hub was found and verification code was sent to the email provided |
| guilds\_info? <sup>1</sup> | array\[[student hub guild](servidores.md#student-hub-guild-structure) object] | The guilds found for the email provided                                              |

<sup>1</sup> Only returned if is set to and no is provided.

[**Student Hub Guild Structure**](servidores.md#student-hub-guild-structure)

| Field | Type      | Description                                                                       |
| ----- | --------- | --------------------------------------------------------------------------------- |
| id    | snowflake | The ID of the student hub                                                         |
| name  | string    | The name of the student hub (2-100 characters)                                    |
| icon  | ?string   | The student hub's [icon hash](https://docs.discord.food/reference#cdn-formatting) |

#### [Join Student Hub](servidores.md#join-student-hub) <a href="#join-student-hub" id="join-student-hub"></a>

`POST/guilds/automations/email-domain-lookup/verify-code`

Verifies the student status of the user and joins them to the student hub guild. May fire [Guild Create](https://docs.discord.food/topics/gateway-events#guild-create), [Guild Member Add](https://docs.discord.food/topics/gateway-events#guild-member-add), and optionally [Guild Join Request Create](https://docs.discord.food/topics/gateway-events#guild-join-request-create) Gateway events.

[**JSON Params**](servidores.md#json-params)

| Field     | Type      | Description                                            |
| --------- | --------- | ------------------------------------------------------ |
| email     | string    | The email to verify (max 320 characters)               |
| guild\_id | snowflake | The ID of the student hub being joined                 |
| code      | string    | The verification code sent to the email (8 characters) |

[**Response Body**](servidores.md#response-body)

| Field  | Type                                | Description                                                |
| ------ | ----------------------------------- | ---------------------------------------------------------- |
| joined | boolean                             | Whether the user successfully joined the student hub guild |
| guild? | [guild](servidores.md#guild-object) | The student hub guild the user joined                      |

#### [Join Student Hub Waitlist](servidores.md#join-student-hub-waitlist) <a href="#join-student-hub-waitlist" id="join-student-hub-waitlist"></a>

`POST/hub-waitlist/signup`

Signs up the user for the student hub waitlist. The user will get an email and system message when a student hub is created for their email domain.

[**JSON Params**](servidores.md#json-params)

| Field  | Type   | Description                                                    |
| ------ | ------ | -------------------------------------------------------------- |
| email  | string | The email to sign up for the waitlist (max 320 characters)     |
| school | string | The name of the school the user is attending (3-20 characters) |

[**Response Body**](servidores.md#response-body)

| Field         | Type      | Description                                                 |
| ------------- | --------- | ----------------------------------------------------------- |
| email         | string    | The email that was signed up for the waitlist               |
| email\_domain | string    | The domain of the email that was signed up for the waitlist |
| school        | string    | The name of the school the user is attending                |
| user\_id      | snowflake | The ID of the user that signed up for the waitlist          |

[**Example Response**](servidores.md#example-response)

```
{  "email": "nelly@discordapp.com",  "user_id": "852892297661906993",  "email_domain": "discordapp.com",  "school": "discord"}
```

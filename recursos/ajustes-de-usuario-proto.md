---
icon: gear-complex
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

# Ajustes de usuario Proto

User settings are options that a user can configure to change the behavior of their account or client. These settings are now stored in a protocol buffer format, which is more efficient and allows for more flexibility. Multiple user settings protos are supported, each with a different purpose.

> ⬇️ COLAPSABLE:

Protobufs are a binary format, so you'll need to decode them before you can read them. If you want to learn more about them, a good start is Google's [official documentation](https://protobuf.dev/). All protobufs transmitted in the API are encoded in base64.

The types documented below follow standard protobuf types (e.g. `uint32`, `bool`, `fixed64`, etc.). Some values are wrapped in containers such as `StringValue` or `BoolValue`. These are wrappers that allow the value to be `null` or unset, but must be unwrapped to access the value. They are provided by the `google.protobuf.wrappers` and `google.protobuf.timestamp` packages.

The order of the documented structures below is the same order used in the wire format.

All protobuf definitions used by Discord are available in [this repository](https://github.com/discord-userdoccers/discord-protos). They are also packaged on [PyPI](https://pypi.org/project/discord-protos) and [npm](https://www.npmjs.com/package/discord-protos) for easy installation.

### User Settings Proto Type

\| Value | Name | Description | Object | | ----- | ------------- | -------------------------------------------------------------------------------------- | ---------------------------------------------------------- | | 1 | PRELOADED | General Discord user settings, sent in the Ready event | [Preloaded User Settings](ajustes-de-usuario-proto.md#preloaded-user-settings-object) | | 2 | FRECENCY | Frecency and favorites storage, used for low-priority, lazy-loaded settings | [Frecency User Settings](ajustes-de-usuario-proto.md#frecency-user-settings-object) | | 3 | TEST\_SETTINGS | Unknown | Unknown |

**Versions Structure**

\| Field | Type | Description | | ------------------ | ------ | -------------------------------------------- | | client\_version | uint32 | The client migration version | | server\_version ^1^ | uint32 | The server migration version (currently `0`) | | data\_version ^1^ | uint32 | The incremental data version |

^1^ Should not be modified by clients. `data_version` is automatically incremented on every change.

### Preloaded User Settings Object

Serialized as `discord_protos.discord_users.v1.PreloadedUserSettings`.

**Preloaded User Settings Structure**

\| Field | Type | Description | | ---------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------------------------------ | | versions | [versions](ajustes-de-usuario-proto.md#versions-structure) object | Version information for the protocol and data | | inbox | [inbox settings](ajustes-de-usuario-proto.md#inbox-settings-structure) object | Settings related to the user's inbox view | | guilds | [all guild settings](ajustes-de-usuario-proto.md#all-guild-settings-structure) object | Settings specific to guilds and channels | | user\_content | [user content settings](ajustes-de-usuario-proto.md#user-content-settings-structure) object | Dismissal states for various types of upsells and promotions | | voice\_and\_video | [voice and video settings](ajustes-de-usuario-proto.md#voice-and-video-settings-structure) object | Voice and video call configuration | | text\_and\_images | [text and images settings](ajustes-de-usuario-proto.md#text-and-images-settings-structure) object | Chat and image rendering settings | | notifications | [notification settings](ajustes-de-usuario-proto.md#notification-settings-structure) object | Notification-related preferences | | privacy | [privacy settings](ajustes-de-usuario-proto.md#privacy-settings-structure) object | User privacy preferences | | debug | [debug settings](ajustes-de-usuario-proto.md#debug-settings-structure) object | Client debug settings | | game\_library | [game library settings](ajustes-de-usuario-proto.md#game-library-settings-structure) object | Game library settings | | status | [status settings](ajustes-de-usuario-proto.md#status-settings-structure) object | User presence customization settings | | localization | [localization settings](ajustes-de-usuario-proto.md#localization-settings-structure) object | Client localization preferences | | appearance | [appearance settings](ajustes-de-usuario-proto.md#appearance-settings-structure) object | Client appearance settings | | guild\_folders | [guild folders](ajustes-de-usuario-proto.md#guild-folders-structure) object | Organization settings for guilds | | favorites | [favorites](ajustes-de-usuario-proto.md#favorites-structure) object | The serialized client favorites pseudo-guild | | audio\_context\_settings | [audio settings](ajustes-de-usuario-proto.md#audio-settings-structure) object | RTC audio context settings | | communities | [communities settings](ajustes-de-usuario-proto.md#communities-settings-structure) object | Community guild settings | | broadcast | [broadcast settings](ajustes-de-usuario-proto.md#broadcast-settings-structure) object | Broadcast settings | | clips | [clips settings](ajustes-de-usuario-proto.md#clips-settings-structure) object | [Clips](https://support.discord.com/hc/en-us/articles/16861982215703) settings | | for\_later | [for later settings](ajustes-de-usuario-proto.md#for-later-settings-structure) object | For Later section settings for bookmarks and reminders | | safety\_settings | [safety settings](ajustes-de-usuario-proto.md#safety-settings-structure) object | User safety settings | | icymi\_settings | [ICYMI settings](ajustes-de-usuario-proto.md#icymi-settings-structure) object | ICYMI (In Case You Missed It) feed settings | | applications | [all application settings](ajustes-de-usuario-proto.md#all-application-settings-structure) object | Application-specific settings |

**Inbox Settings Structure**

\| Field | Type | Description | | --------------- | ---------------------------- | ---------------------------------------------- | | current\_tab | [inbox tab](ajustes-de-usuario-proto.md#inbox-tab) enum | The currently selected tab in the inbox | | viewed\_tutorial | boolean | Whether the user has viewed the inbox tutorial |

**Inbox Tab**

\| Value | Name | Description | | ----- | ------------ | ---------------------- | | 0 | UNSPECIFIED | Default unset value | | 1 | MENTIONS | Mentions tab | | 2 | UNREADS | Unreads tab | | 3 | TODOS | Message reminders tab | | 4 | FOR\_YOU | Notification items tab | | 5 | GAME\_INVITES | Game invites tab | | 6 | BOOKMARKS | Bookmarks tab | | 7 | SCHEDULED | Scheduled messages tab |

**All Guild Settings Structure**

\| Field | Type | Description | | ------------------------- | ---------------------------------------------------------------- | ------------------------------------------- | | guilds | map\[fixed64, [guild settings](ajustes-de-usuario-proto.md#guild-settings-structure) object] | Per-guild personalization settings | | \~\~leaderboards\_disabled\~\~ | \~\~bool\~\~ | \~\~Whether guild leaderboards are disabled\~\~ |

**Guild Settings Structure**

\| Field | Type | Description | | -------------------------------------- | -------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | | channels | map\[fixed64, [channel settings](ajustes-de-usuario-proto.md#channel-settings-structure) object] | Settings specific to channels within the guild | | hub\_progress | uint32 | [Hub progress flags](ajustes-de-usuario-proto.md#hub-progress-flags) | | guild\_onboarding\_progress | uint32 | [Guild onboarding progress flags](ajustes-de-usuario-proto.md#guild-onboarding-progress-flags) | | guild\_recents\_dismissed\_at? | timestamp | When the guild recents were last dismissed | | dismissed\_guild\_content | bytes | Per-guild dismissable content, encoded as a byte array of integer enum values | | join\_sound? | [custom call sound](ajustes-de-usuario-proto.md#custom-call-sound-structure) object | Custom sound played when joining a call in this channel | | mobile\_redesign\_channel\_list\_settings? | [channel list settings](ajustes-de-usuario-proto.md#channel-list-settings-structure) object | Channel list settings | | disable\_raid\_alert\_push | boolean | Whether to disable raid alert push notifications | | disable\_raid\_alert\_nag | boolean | Whether to disable raid alert in-client nag screens | | custom\_notification\_sound\_config? | [custom notification sound config](ajustes-de-usuario-proto.md#custom-notification-sound-config-structure) object | Custom notification sound configuration | | leaderboards\_disabled | boolean | Whether guild leaderboards are disabled |

**Channel Settings Structure**

\| Field | Type | Description | | --------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------- | | collapsed\_in\_inbox | boolean | Whether the channel is collapsed in the inbox | | icon\_emoji? | [channel icon emoji](ajustes-de-usuario-proto.md#channel-icon-emoji-structure) | The custom emoji icon for the channel | | custom\_notification\_sound\_config? | [custom notification sound config](ajustes-de-usuario-proto.md#custom-notification-sound-config-structure) | Custom notification sound configuration for the channel |

**Channel Icon Emoji Structure**

\| Field | Type | Description | | ------ | ----------- | --------------------- | | id? | UInt64Value | The ID of the emoji | | name? | StringValue | The name of the emoji | | color? | UInt64Value | The color of the icon |

**Custom Notification Sound Config Structure**

\| Field | Type | Description | | --------------------------- | ----------- | ---------------------------------------------------------------------- | | notification\_sound\_pack\_id? | StringValue | The ID of the [notification sound pack](ajustes-de-usuario-proto.md#notification-sound-pack) used |

**Hub Progress Flags**

\| Value | Name | Description | | ------ | ------------ | ---------------------------------------- | | 1 `<<`0 | JOIN\_GUILD | User has joined a guild in the hub | | 1 `<<`1 | INVITE\_USER | User has sent an invite for the hub | | 1 `<<`2 | CONTACT\_SYNC | User has accepted the contact sync modal |

**Guild Onboarding Progress Flags**

\| Value | Name | Description | | ------ | -------------- | ----------------------------------------- | | 1 `<<`0 | NOTICE\_SHOWN | User has been shown the onboarding notice | | 1 `<<`1 | NOTICE\_CLEARED | User has cleared the onboarding notice |

**Custom Call Sound Structure**

\| Field | Type | Description | | -------- | ------- | ------------------------------------------ | | sound\_id | fixed64 | The ID of the custom call soundboard sound | | guild\_id | fixed64 | The ID of the guild the sound is in |

**Notification Sound Pack**

\| Value | Description | | -------------- | ---------------------- | | classic | Default Discord sounds | | retro | Retro | | bop | Bubble | | ducky | Ducky | | lofi | Lofi | | asmr | ASMR | | discodo | Discodo easter egg | | halloween | Halloween | | winter\_holiday | Winter holiday |

**Channel List Settings Structure**

\| Field | Type | Description | | ----------------- | ----------- | --------------------------- | | layout? | StringValue | Channel list layout setting | | message\_previews? | StringValue | Message preview setting |

**User Content Settings Structure**

\| Field | Type | Description | | --------------------------------------------- | -------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- | | dismissed\_contents | bytes | User dismissed content, encoded as a byte array of integer enum values | | last\_dismissed\_outbound\_promotion\_start\_date? | StringValue | When the last dismissed outbound promotion started | | premium\_tier\_0\_modal\_dismissed\_at? | Timestamp | When the Nitro Basic promotion modal was dismissed | | guild\_onboarding\_upsell\_dismissed\_at? | Timestamp | When the guild onboarding upsell was dismissed | | safety\_user\_sentiment\_notice\_dismissed\_at? | Timestamp | When the safety user sentiment notice was dismissed | | last\_received\_changelog\_id | fixed64 | The ID of the last received changelog | | recurring\_dismissible\_content\_states | map\[int32, [recurring dismissible content state](ajustes-de-usuario-proto.md#recurring-dismissible-content-state-structure) object] | States of recurring dismissible content entries |

**Recurring Dismissible Content State Structure**

\| Field | Type | Description | | ---------------------- | ------ | ----------------------------------------------------------------------- | | last\_dismissed\_version | uint32 | How many times the content has been dismissed | | last\_dismissed\_at\_ms | uint64 | Unix timestamp (in milliseconds) of when the content was last dismissed |

**Voice And Video Settings Structure**

\| Field | Type | Description | | --------------------------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------ | | blur | [video filter background blur](ajustes-de-usuario-proto.md#video-filter-background-blur-structure) object | Video call background blur settings | | preset\_option | uint32 | The [selected video call background preset](ajustes-de-usuario-proto.md#video-filter-background-preset) | | custom\_asset | [video filter asset](ajustes-de-usuario-proto.md#video-filter-asset-structure) object | Custom video call background asset | | always\_preview\_video? | BoolValue | Whether to always preview video before enabling it (default false) | | afk\_timeout? | UInt32Value | Duration (in seconds) the user needs to be inactive until clients update their AFK state (default 600) | | stream\_notifications\_enabled? | BoolValue | Whether to receive stream notifications for friends (default true) | | native\_phone\_integration\_enabled? | BoolValue | Whether to enable the new Discord mobile phone number friend requesting feature (default true) | | soundboard\_settings? | [soundboard settings](ajustes-de-usuario-proto.md#soundboard-settings-structure) object | Settings for the soundboard feature | | disable\_stream\_previews? | BoolValue | Whether clients should disable sending stream previews (default false) | | soundmoji\_volume? | FloatValue | Volume level for soundmoji playback (0-100, default 100) |

**Video Filter Background Blur Structure**

\| Field | Type | Description | | -------- | ------- | ----------------------------------------------- | | use\_blur | boolean | Whether to apply background blur in video calls |

**Video Filter Asset Structure**

\| Field | Type | Description | | ---------- | ------- | ---------------------------------- | | id | fixed64 | The ID of the video filter asset | | asset\_hash | string | The hash of the video filter asset |

**Soundboard Settings Structure**

\| Field | Type | Description | | ------ | ----- | -------------------------------------------- | | volume | float | Volume level for soundboard playback (0-100) |

**Video Filter Background Preset**

\| Value | Description | | ----- | ----------------- | | 0 | Unset | | 1 | Cybercity | | 2 | Discord the Movie | | 3 | Wumpus Vacation | | 4 | Vaporwave | | 7 | Capernite Day | | 8 | Capernite Night | | 9 | Hacker Den | | 10 | Wumpice |

**Text And Images Settings Structure**

\| Field | Type | Description | | ------------------------------------- | ------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------- | | diversity\_surrogate? | StringValue | Unicode surrogate for diversity emoji | | use\_rich\_chat\_input? **(deprecated)** | BoolValue | Inverse of `use_legacy_chat_input` | | use\_thread\_sidebar? | BoolValue | Whether to display threads in the sidebar (default true) | | render\_spoilers? | StringValue | [How spoilers should be rendered](ajustes-de-usuario-proto.md#spoiler-render-options) | | emoji\_picker\_collapsed\_sections | array\[string] | [Sections of the emoji picker](ajustes-de-usuario-proto.md#emoji-picker-section) that are collapsed | | sticker\_picker\_collapsed\_sections | array\[string] | [Sections of the sticker picker](ajustes-de-usuario-proto.md#sticker-picker-section) that are collapsed | | view\_image\_descriptions? | BoolValue | Whether to show image alt text (default false) | | show\_command\_suggestions? | BoolValue | Whether to show command suggestions in chat (default true) | | inline\_attachment\_media? | BoolValue | Whether to display attachments when they are uploaded in chat (default true) | | inline\_embed\_media? | BoolValue | Whether to display videos and images from links posted in chat (default true) | | gif\_auto\_play? | BoolValue | Whether GIFs are automatically played when the Discord client is in focus (default true) | | render\_embeds? | BoolValue | Whether to render message embeds (default true) | | render\_reactions? | BoolValue | Whether to render message reactions (default true) | | animate\_emoji? | BoolValue | Whether to play animated emojis in chat (default true) | | animate\_stickers? | UInt32Value | When to animate stickers in chat | | enable\_tts\_command? | BoolValue | Whether to allow TTS messages to be sent and played (default true) | | message\_display\_compact? | BoolValue | Whether to use the compact Discord display mode | | explicit\_content\_filter? | UInt32Value | The explicit content filter for explicit content in all messages (default `NON_FRIENDS`) | | view\_nsfw\_guilds? | BoolValue | Whether NSFW guilds are shown on iOS (default false) | | convert\_emoticons? | BoolValue | Whether to convert emoticons into emojis (e.g. `:)` -> `🙂`) (default true) | | expression\_suggestions\_enabled? | BoolValue | Whether to show expression suggestions in chat (default true) | | view\_nsfw\_commands? | BoolValue | Whether NSFW application commands are shown in DMs (default false) | | use\_legacy\_chat\_input? | BoolValue | Whether to use the legacy chat input UI (default false) | | soundboard\_picker\_collapsed\_sections | array\[string] | [Sections of the soundboard picker](ajustes-de-usuario-proto.md#soundboard-picker-section) that are collapsed | | dm\_spam\_filter? **(deprecated)** | UInt32Value | DM spam filter setting | | dm\_spam\_filter\_v2 | [DM spam filter v2](ajustes-de-usuario-proto.md#dm-spam-filter-v2) enum | DM spam filter setting | | include\_stickers\_in\_autocomplete? | BoolValue | Whether to autocomplete stickers in chat (default false) | | explicit\_content\_settings? | [explicit content settings](ajustes-de-usuario-proto.md#explicit-content-settings-structure) object | Explicit content settings | | keyword\_filter\_settings? | [keyword filter settings](ajustes-de-usuario-proto.md#keyword-filter-settings-structure) object | Client-side keyword filter settings | | include\_soundmoji\_in\_autocomplete? | BoolValue | Whether to autocomplete soundmoji in chat (default true) |

**Spoiler Render Options**

\| Value | Description | | ------------ | --------------------------------------------------- | | ALWAYS | Always render spoilers | | ON\_CLICK | Render spoilers on click (default) | | IF\_MODERATOR | Always render spoilers in guilds the user moderates |

**Emoji Picker Section**

This may also be a snowflake to represent a specific guild in the emoji picker.

\| Value | Description | | --------------- | ------------------- | | FAVORITES | Favorite emoji | | TOP\_GUILD\_EMOJI | Top guild emoji | | RECENT | Recently used emoji | | people | People emoji | | nature | Nature emoji | | food | Food emoji | | activity | Activity emoji | | travel | Travel emoji | | objects | Object emoji | | symbols | Symbol emoji | | flags | Flag emoji |

**Sticker Picker Section**

This may also be a snowflake to represent a specific guild in the sticker picker.

\| Value | Description | | -------- | ---------------------- | | FAVORITE | Favorite stickers | | RECENT | Recently used stickers |

**Soundboard Picker Section**

This may also be a snowflake to represent a specific guild in the soundboard sound picker.

\| Value | Description | | ----- | ---------------------- | | 0 | Favorite sounds | | 4 | Default Discord sounds |

**DM Spam Filter**

\| Value | Name | Description | | ----- | ----------------------- | ------------------------------------------------- | | 0 | DISABLED | DM spam filter is disabled | | 1 | NON\_FRIENDS | Apply spam filter to non-friends | | 2 | FRIENDS\_AND\_NON\_FRIENDS | Apply spam filter to both friends and non-friends |

**DM Spam Filter V2**

\| Value | Name | Description | | ----- | ----------------------- | ------------------------------------------------- | | 0 | UNSET | Default unset value (equates to `NON_FRIENDS`) | | 1 | DISABLED | DM spam filter is disabled | | 2 | NON\_FRIENDS | Apply spam filter to non-friends | | 3 | FRIENDS\_AND\_NON\_FRIENDS | Apply spam filter to both friends and non-friends |

**Explicit Content Settings Structure**

\| Field | Type | Description | | ------------------------------ | -------------------------------------------------------------- | ---------------------------------------------------- | | explicit\_content\_guilds | [explicit content redaction](ajustes-de-usuario-proto.md#explicit-content-redaction) enum | Redaction setting for explicit content in guilds | | explicit\_content\_friend\_dm | [explicit content redaction](ajustes-de-usuario-proto.md#explicit-content-redaction) enum | Redaction setting for explicit content in friend DMs | | explicit\_content\_non\_friend\_dm | [explicit content redaction](ajustes-de-usuario-proto.md#explicit-content-redaction) enum | Redaction setting for explicit content in other DMs |

**Explicit Content Redaction**

\| Value | Name | Description | | ----- | ----- | ------------------------------------------------------------------------------------------ | | 0 | UNSET | Default unset value (equates to `BLOCK` for `explicit_content_non_friend_dm`, else `SHOW`) | | 1 | SHOW | Disable explicit content filtering | | 2 | BLUR | Blur explicit content | | 3 | BLOCK | Block explicit content entirely |

**Keyword Filter Settings Structure**

\| Field | Type | Description | | --------------- | --------- | ------------------------------------------------ | | profanity? | BoolValue | Whether to filter profanity (default false) | | sexual\_content? | BoolValue | Whether to filter sexual content (default false) | | slurs? | BoolValue | Whether to filter slurs (default false) |

**Notification Settings Structure**

\| Field | Type | Description | | ---------------------------------------- | -------------------------------------------------------------- | --------------------------------------------------------------------------- | | show\_in\_app\_notifications? | BoolValue | Whether to show in-app notifications (default true) | | notify\_friends\_on\_go\_live? | BoolValue | Whether to notify friends when streaming in eligible guilds (default false) | | notification\_center\_acked\_before\_id | fixed64 | The ID of the last acknowledged notification center item | | \~\~enable\_burst\_reaction\_notifications?\~\~ | \~\~BoolValue\~\~ | \~\~Whether to enable burst reaction notifications\~\~ | | quiet\_mode? | BoolValue | Whether quiet mode is enabled (default false) | | focus\_mode\_expires\_at\_ms | fixed64 | Unix timestamp (in milliseconds) of when focus mode expires | | reaction\_notifications | [reaction notification type](ajustes-de-usuario-proto.md#reaction-notification-type) enum | Reaction notification settings |

**Reaction Notification Type**

\| Value | Name | Description | | ----- | ---------------------- | ----------------------------------------- | | 0 | NOTIFICATIONS\_ENABLED | Enable reaction notifications | | 1 | ONLY\_DMS | Only enable reaction notifications in DMs | | 2 | NOTIFICATIONS\_DISABLED | Disable reaction notifications |

**Privacy Settings Structure**

\| Field | Type | Description | | ----------------------------------------------- | -------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | | allow\_activity\_party\_privacy\_friends? | BoolValue | Whether to allow friends to join your activity without sending a request (default true) | | allow\_activity\_party\_privacy\_voice\_channel? ^1^ | BoolValue | Whether to allow people in the same voice channel as you to join your activity without sending a request (default true) | | restricted\_guild\_ids | array\[fixed64] | The IDs of guilds that you will not receive DMs from | | default\_guilds\_restricted | boolean | Whether to automatically disable DMs between you and members of new guilds you join (default false) | | allow\_accessibility\_detection | boolean | Whether to allow Discord to track screen reader usage (default false) | | detect\_platform\_accounts? | BoolValue | Whether to automatically detect accounts from services like Steam and Blizzard when opening the Discord client (default true) | | passwordless? **(deprecated)** | BoolValue | Whether to enable passwordless login (default true) | | contact\_sync\_enabled? | BoolValue | Whether to enable contact sync on Discord mobile (default false) | | friend\_source\_flags? | UInt32Value | The user's [friend source flags](ajustes-de-usuario-proto.md#friend-source-flags) (default all) | | friend\_discovery\_flags? | UInt32Value | The user's friend discovery flags (default none) | | activity\_restricted\_guild\_ids | array\[fixed64] | The IDs of guilds your activity presence will be hidden in | | default\_guilds\_activity\_restricted? | [guild activity status restriction default](ajustes-de-usuario-proto.md#guild-activity-status-restriction-default) enum | Default activity presence restriction setting for new guilds (default false) | | activity\_joining\_restricted\_guild\_ids | array\[fixed64] | The IDs of guilds that will not be able to join your current activity | | message\_request\_restricted\_guild\_ids | array\[fixed64] | The IDs of guilds whose originating DMs will not be filtered into message requests | | default\_message\_request\_restricted? | BoolValue | Whether to automatically disable message requests in new guilds (default false) | | drops\_opted\_out? **(deprecated)** | BoolValue | Whether to opt out of drops (default false) | | non\_spam\_retraining\_opt\_in? | BoolValue | Whether to help improve Discord spam models when marking messages as non-spam | | family\_center\_enabled? | BoolValue | Whether family center is enabled (default false) | | family\_center\_enabled\_v2? | BoolValue | Whether family center is enabled (default false) | | hide\_legacy\_username? | BoolValue | Whether to hide the user's pre-pomelo username | | inappropriate\_conversation\_warnings? | BoolValue | Whether to warn about inappropriate conversations in DMs (default true) | | recent\_games\_enabled? | BoolValue | Whether to show recent games on your profile (default true) | | guilds\_leaderboard\_opt\_out\_default | [guilds leaderboard opt out default](ajustes-de-usuario-proto.md#guilds-leaderboard-opt-out-default) enum | Whether to opt out of leaderboards for new guilds by default | | allow\_game\_friend\_dms\_in\_discord? | BoolValue | Whether to allow game friend DMs in Discord (default true) | | default\_guilds\_restricted\_v2? | BoolValue | Whether new guilds are restricted by default (default false) | | slayer\_sdk\_receive\_dms\_in\_game | [slayer SDK receive in game DMs](ajustes-de-usuario-proto.md#slayer-sdk-receive-in-game-dms) enum | Setting for receiving in-game DMs via the social layer SDK |

^1^ Does not apply to community guilds.

**Friend Source Flags**

\| Value | Name | Description | | ------ | --------------- | --------------------------------------------------------------- | | 1 `<<`1 | MUTUAL\_FRIENDS | Whether mutual friends can add the user as friend | | 1 `<<`2 | MUTUAL\_GUILDS | Whether members in the user's guilds can add the user as friend | | 1 `<<`3 | NO\_RELATION ^3^ | Whether users with no relation can add the user as friend |

^3^ Requires `MUTUAL_FRIENDS` and `MUTUAL_GUILDS`.

**Guild Activity Status Restriction Default**

\| Value | Name | Description | | ----- | ------------------- | ---------------------------------------------------------- | | 0 | OFF | Do not restrict activity presence for new guilds | | 1 | ON\_FOR\_LARGE\_GUILDS | Restrict activity presence for large new guilds by default | | 2 | ON | Restrict activity presence for all new guilds by default |

**Guilds Leaderboard Opt Out Default**

\| Value | Name | Description | | ----- | ------------------ | -------------------------------------------------------- | | 0 | OFF\_FOR\_NEW\_GUILDS | Do not opt out of leaderboards for new guilds by default | | 1 | ON\_FOR\_NEW\_GUILDS | Opt out of leaderboards for new guilds by default |

**Slayer SDK Receive In Game DMs**

\| Value | Name | Description | | ----- | --------------- | ---------------------------------------------- | | 0 | UNSET | Default unset value (equates to `ALL`) | | 1 | ALL | Receive all in-game DMs | | 2 | USERS\_WITH\_GAME | Receive DMs only from users with the same game | | 3 | NONE | Do not receive any in-game DMs |

**Debug Settings Structure**

\| Field | Type | Description | | ---------------------------- | --------- | ------------------------------------------------------------- | | rtc\_panel\_show\_voice\_states? | BoolValue | Whether to show voice states in the RTC panel (default false) |

**Game Library Settings Structure**

\| Field | Type | Description | | ---------------------------- | --------- | ---------------------------------------------------------------- | | install\_shortcut\_desktop? | BoolValue | Whether to install desktop shortcuts for games (default false) | | install\_shortcut\_start\_menu? | BoolValue | Whether to install start menu shortcuts for games (default true) | | disable\_games\_tab? | BoolValue | Whether to disable the games tab (default false) |

**Status Settings Structure**

\| Field | Type | Description | | -------------------- | ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- | | status? | StringValue | The overall status of the user, used to sync presence across clients (default `unknown`) | | custom\_status? | [Custom Status](ajustes-de-usuario-proto.md#custom-status-structure) | The overall custom status of the user, used to sync presence across clients | | show\_current\_game? | BoolValue | Whether to display the currently active game in user presence (default true) | | status\_expires\_at\_ms | fixed64 | Unix timestamp (in milliseconds) of when the status expires |

**Custom Status Structure**

\| Field | Type | Description | | ------------- | ------- | ------------------------------------------------------------------------------- | | text | string | The custom status text | | emoji\_id | fixed64 | The ID of a guild's custom emoji | | emoji\_name | string | The unicode character of the emoji | | expires\_at\_ms | fixed64 | Timestamp (in milliseconds) of when the custom status expires | | created\_at\_ms | fixed64 | Timestamp (in milliseconds) of when the custom status was created | | label | string | The type of custom status label |

**Localization Settings Structure**

\| Field | Type | Description | | ---------------- | ----------- | ------------------------------------------------------------------------------ | | locale? | StringValue | The language option chosen by the user (default `en-US`) | | timezone\_offset? | Int32Value | The timezone offset from UTC to use (in minutes) |

**Appearance Settings Structure**

\| Field | Type | Description | | ---------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------ | | theme | [theme](ajustes-de-usuario-proto.md#theme) enum | The user's [client theme](ajustes-de-usuario-proto.md#theme) | | developer\_mode | boolean | Whether to enable developer mode in-client (default false) | | client\_theme\_settings? | [client theme settings](ajustes-de-usuario-proto.md#client-theme-settings-structure) object | Custom theme settings | | mobile\_redesign\_disabled | boolean | Whether the mobile redesign is disabled (default false) | | channel\_list\_layout? | StringValue | Channel list layout setting | | message\_previews? | StringValue | Message preview setting | | search\_result\_exact\_count\_enabled? | BoolValue | Whether to show exact counts in tabs search results (default true) | | timestamp\_hour\_cycle | [timestamp hour cycle](ajustes-de-usuario-proto.md#timestamp-hour-cycle) enum | Timestamp clock cycle format setting | | happening\_now\_cards\_disabled? | BoolValue | Whether Happening Now cards are disabled (default false) | | launch\_pad\_mode | [launch pad mode](ajustes-de-usuario-proto.md#launch-pad-mode) enum | Mobile Launch Pad behavior | | ui\_density | [UI density](ajustes-de-usuario-proto.md#ui-density) enum | UI density setting | | swipe\_right\_to\_left\_mode | [swipe right to left mode](ajustes-de-usuario-proto.md#swipe-right-to-left-mode) enum | Mobile swipe right-to-left behavior |

**Theme**

\| Value | Name | Description | | ----- | -------- | --------------------------------------- | | 0 | UNSET | Default unset value (equates to `DARK`) | | 1 | DARK | Dark theme | | 2 | LIGHT | Light theme | | 3 | DARKER | Darker theme | | 4 | MIDNIGHT | Midnight theme |

**Client Theme Settings Structure**

\| Field | Type | Description | | ------------------------------------------- | ----------- | ---------------------------------------- | | primary\_color? **(deprecated)** | UInt32Value | The primary color of the client theme | | background\_gradient\_preset\_id? | UInt32Value | The ID of the background gradient preset | | background\_gradient\_angle? **(deprecated)** | FloatValue | The angle of the background gradient |

**Timestamp Hour Cycle**

\| Value | Name | Description | | ----- | ---- | -------------------------------------------------- | | 0 | AUTO | Automatically determine hour cycle based on locale | | 1 | H12 | Use 12-hour clock | | 2 | H23 | Use 24-hour clock |

**Launch Pad Mode**

\| Value | Name | Description | | ----- | ------------------- | ------------------------------------- | | 0 | DISABLED | Disable mobile launch pad | | 1 | GESTURE\_FULL\_SCREEN | Enable full-screen gesture launch pad | | 2 | GESTURE\_RIGHT\_EDGE | Enable right-edge gesture launch pad | | 3 | PULL\_TAB | Enable pull-tab launch pad |

**UI Density**

\| Value | Name | Description | | ----- | ---------- | --------------------- | | 0 | UNSET | Default unset value | | 1 | COMPACT | Compact UI density | | 2 | COZY | Cozy UI density | | 3 | RESPONSIVE | Responsive UI density |

**Swipe Right To Left Mode**

\| Value | Name | Description | | ----- | --------------- | ------------------------------------- | | 0 | UNSET | Default unset value (default `REPLY`) | | 1 | CHANNEL\_DETAILS | Swipe to open channel details | | 2 | REPLY | Swipe to reply to messages |

**Guild Folders Structure**

\| Field | Type | Description | | -------------------------------- | ----------------------------------------------------- | ---------------------------------- | | folders ^1^ | array\[[guild folder](ajustes-de-usuario-proto.md#guild-folder-structure) object] | Guild folders | | guild\_positions **(deprecated)** | array\[fixed64] | Positions of guilds in the sidebar |

^1^ This will include guilds that are not in a folder as an anonymous folder with a single entry.

**Guild Folder Structure**

\| Field | Type | Description | | --------- | -------------- | ----------------------------------- | | guild\_ids | array\[fixed64] | The IDs of the guilds in the folder | | id? | Int64Value | The ID of the folder | | name? | StringValue | The name of the folder | | color? | UInt64Value | Hex color of the folder |

**Favorites Structure**

\| Field | Type | Description | | ----------------- | -------------------------------------------------------------------- | --------------------------- | | favorite\_channels | map\[fixed64, [favorite channel](ajustes-de-usuario-proto.md#favorite-channel-structure) object] | Map of favorite channels | | muted | boolean | Whether favorites are muted |

**Favorite Channel Structure**

\| Field | Type | Description | | --------- | ----------------------------------------------- | --------------------------------- | | nickname | string | Nickname of the favorite channel | | type | [favorite channel type](ajustes-de-usuario-proto.md#favorite-channel-type) | Type of the favorite channel | | position | uint32 | Position of the favorite channel | | parent\_id | fixed64 | Parent ID of the favorite channel |

**Favorite Channel Type**

\| Value | Description | | ------------------ | -------------------------- | | UNSET | Default unset value | | REFERENCE\_ORIGINAL | References a real channel | | CATEGORY | Contains favorite channels |

**Audio Settings Structure**

\| Field | Type | Description | | ------ | -------------------------------------------------------------- | --------------------------------------- | | user | map\[fixed64, [audio context](ajustes-de-usuario-proto.md#audio-context-structure) object] | Audio context settings for users | | stream | map\[fixed64, [audio context](ajustes-de-usuario-proto.md#audio-context-structure) object] | Audio context settings for user streams |

**Audio Context Structure**

\| Field | Type | Description | | ---------------- | ------- | ---------------------------------------------------------------------- | | muted | boolean | Whether the audio is muted | | volume | float | Volume level of the user or stream (0-200) | | modified\_at | fixed64 | Unix timestamp (in milliseconds) of when the setting was last modified | | soundboard\_muted | boolean | Whether the soundboard is muted (user only) |

**Communities Settings Structure**

\| Field | Type | Description | | --------------------------------------- | --------- | --------------------------------------------------------------- | | disable\_home\_auto\_nav? **(deprecated)** | BoolValue | Whether to disable automatic navigation to Home (default false) |

**Broadcast Settings Structure**

\| Field | Type | Description | | ----------------- | -------------- | ---------------------------------------------------------- | | allow\_friends? | BoolValue | Whether friends are allowed to broadcast to you | | allowed\_guild\_ids | array\[fixed64] | The IDs of guilds where broadcasting is allowed | | allowed\_user\_ids | array\[fixed64] | The IDs of users who are allowed to broadcast | | auto\_broadcast? | BoolValue | Whether to automatically start broadcasting upon streaming |

**Clips Settings Structure**

\| Field | Type | Description | | ---------------------- | --------- | ------------------------------------------------------------------ | | allow\_voice\_recording? | BoolValue | Whether to allow your voice to be recorded in Clips (default true) |

**For Later Settings Structure**

\| Field | Type | Description | | ----------- | ------------------------------------ | --------------------- | | current\_tab | [for later tab](ajustes-de-usuario-proto.md#for-later-tab) enum | For later tab setting |

**For Later Tab**

\| Value | Name | Description | | ----- | ----------- | -------------------------------------------- | | 0 | UNSPECIFIED | Default unset value (equates to `ALL`) | | 1 | ALL | Show all items in the For Later section | | 2 | BOOKMARKS | Show only bookmarks in the For Later section | | 3 | REMINDERS | Show only reminders in the For Later section |

**Safety Settings Structure**

\| Field | Type | Description | | --------------------------------- | ----------------------------------------------------------- | ----------------------------------------------------------------------------------------- | | safety\_settings\_preset | [safety settings preset type](ajustes-de-usuario-proto.md#safety-settings-preset-type) | Preset type for safety settings | | ignore\_profile\_speedbump\_disabled | boolean | Whether to hide the warning shown when viewing a profile you have blocked (default false) |

**Safety Settings Preset Type**

\| Value | Name | Description | | ----- | -------- | ------------------------ | | 0 | UNSET | Default unset value | | 1 | BALANCED | Balanced safety settings | | 2 | STRICT | Strict safety settings | | 3 | RELAXED | Relaxed safety settings | | 4 | CUSTOM | Custom safety settings |

**ICYMI Settings Structure**

\| Field | Type | Description | | ----------------- | ------- | --------------------------------------------------- | | feed\_generated\_at | fixed64 | Unix timestamp (in milliseconds) of feed generation |

**All Application Settings Structure**

\| Field | Type | Description | | ------------ | ---------------------------------------------------------------------------- | ----------------------------- | | app\_settings | map\[fixed64, [application settings](ajustes-de-usuario-proto.md#application-settings-structure) object] | Application-specific settings |

**Application Settings Structure**

\| Field | Type | Description | | ---------------- | ------------------------------------------------------------- | ------------------------------- | | app\_dm\_settings? | [application DM settings](ajustes-de-usuario-proto.md#application-dm-settings-structure) | DM settings for the application |

**Application DM Settings Structure**

\| Field | Type | Description | | ----------- | ------- | ------------------------------------------------------------ | | dm\_disabled | boolean | Whether DMs are disabled for the application (default false) |

### Frecency User Settings Object

Serialized as `discord_protos.discord_users.v1.FrecencyUserSettings`.

**Frecency User Settings Structure**

\| Field | Type | Description | | ---------------------------- | ------------------------------------------------------------------------------ | --------------------------------------------- | | versions | [versions](ajustes-de-usuario-proto.md#versions-structure) object | Version information for the protocol and data | | favorite\_gifs | [favorite gifs](ajustes-de-usuario-proto.md#favorite-gifs-structure) object | Favorite GIFs storage | | favorite\_stickers | [favorite stickers](ajustes-de-usuario-proto.md#favorite-stickers-structure) object | Favorite stickers storage | | sticker\_frecency | [sticker frecency](ajustes-de-usuario-proto.md#sticker-frecency-structure) object | Sticker frecency storage | | favorite\_emojis | [favorite emojis](ajustes-de-usuario-proto.md#favorite-emojis-structure) object | Favorite emojis settings | | emoji\_frecency | [emoji frecency](ajustes-de-usuario-proto.md#emoji-frecency-structure) object | Emoji frecency storage | | application\_command\_frecency | [application command frecency](ajustes-de-usuario-proto.md#application-command-frecency-structure) object | Application command frecency storage | | favorite\_soundboard\_sounds | [favorite soundboard sounds](ajustes-de-usuario-proto.md#favorite-soundboard-sounds-structure) object | Favorite soundboard sounds storage | | application\_frecency | [application frecency](ajustes-de-usuario-proto.md#application-frecency-structure) object | Application frecency storage | | heard\_sound\_frecency | [heard sound frecency](ajustes-de-usuario-proto.md#heard-sound-frecency-structure) object | Heard soundboard sound frecency storage | | played\_sound\_frecency | [played sound frecency](ajustes-de-usuario-proto.md#played-sound-frecency-structure) object | Played soundboard sound frecency storage | | guild\_and\_channel\_frecency | [guild and channel frecency](ajustes-de-usuario-proto.md#guild-and-channel-frecency-structure) object | Guild and channel frecency storage | | emoji\_reaction\_frecency | [emoji reaction frecency](ajustes-de-usuario-proto.md#emoji-reaction-frecency-structure) object | Emoji reaction frecency storage |

**Frecency Item Structure**

\| Field | Type | Description | | ----------- | ------------- | ---------------------------------------------------------------------- | | total\_uses | uint32 | Total uses of the item | | recent\_uses | array\[uint64] | Unix timestamps (in milliseconds) representing recent uses of the item | | frecency | int32 | Frecency score of the item, or `-1` if not applicable | | score | int32 | Weighted score of the item |

**Favorite GIFs Structure**

\| Field | Type | Description | | ------------ | ----------------------------------------------------------- | --------------------------------------------- | | gifs | map\[string, [favorite GIF](ajustes-de-usuario-proto.md#favorite-gif-structure) object] | Map of GIF URLs to favorite GIFs | | hide\_tooltip | boolean | Whether to hide the tooltip for favorite GIFs |

**Favorite GIF Structure**

\| Field | Type | Description | | ------ | -------------------------- | ---------------------------------------- | | format | [GIF type](ajustes-de-usuario-proto.md#gif-type) enum | The type of the GIF (`IMAGE` or `VIDEO`) | | src | string | The media URL of the GIF | | width | uint32 | The width of the GIF | | height | uint32 | The height of the GIF | | order | uint32 | Order of the GIF in the list |

**GIF Type**

\| Value | Name | Description | | ----- | ----- | ------------- | | 0 | NONE | Default unset | | 1 | IMAGE | Image GIF | | 2 | VIDEO | Video GIF |

**Favorite Stickers Structure**

\| Field | Type | Description | | ----------- | -------------- | --------------------------------------- | | sticker\_ids | array\[fixed64] | The IDs of the user's favorite stickers |

**Sticker Frecency Structure**

\| Field | Type | Description | | -------- | -------------------------------------------------------------- | ----------------------------- | | stickers | map\[fixed64, [frecency item](ajustes-de-usuario-proto.md#frecency-item-structure) object] | Map of sticker frecency items |

**Favorite Emojis Structure**

\| Field | Type | Description | | ------ | ------------- | --------------------------------------------------------------------------------------------------- | | emojis | array\[string] | The user's favorite emoji, represented as snowflake IDs or friendly names (e.g. `skull_crossbones`) |

**Emoji Frecency Structure**

\| Field | Type | Description | | ------ | ------------------------------------------------------------- | --------------------------------------------------------------------------------------------- | | emojis | map\[string, [frecency item](ajustes-de-usuario-proto.md#frecency-item-structure) object] | Frecenct used emoji, represented as snowflake IDs or friendly names (e.g. `skull_crossbones`) |

**Application Command Frecency Structure**

\| Field | Type | Description | | -------------------- | ------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | | application\_commands | map\[string, [frecency item](ajustes-de-usuario-proto.md#frecency-item-structure) object] | Frecent application commands, represented as snowflake command IDs with optional null-joined command names and colon-joined guild IDs (e.g. `1221148400637050941\u0000set:550327071407079444`) |

**Favorite Soundboard Sounds Structure**

\| Field | Type | Description | | --------- | -------------- | ------------------------------------------------ | | sound\_ids | array\[fixed64] | The IDs of the user's favorite soundboard sounds |

**Application Frecency Structure**

\| Field | Type | Description | | ------------ | ------------------------------------------------------------- | -------------------------------------------------- | | applications | map\[string, [frecency item](ajustes-de-usuario-proto.md#frecency-item-structure) object] | Frecent applications, represented as snowflake IDs |

**Heard Sound Frecency Structure**

\| Field | Type | Description | | ------------ | ------------------------------------------------------------- | ------------------------------------------------------------- | | heard\_sounds | map\[string, [frecency item](ajustes-de-usuario-proto.md#frecency-item-structure) object] | Frecent heard soundboard sounds, represented as snowflake IDs |

**Played Sound Frecency Structure**

\| Field | Type | Description | | ------------- | ------------------------------------------------------------- | -------------------------------------------------------------- | | played\_sounds | map\[string, [frecency item](ajustes-de-usuario-proto.md#frecency-item-structure) object] | Frecent played soundboard sounds, represented as snowflake IDs |

**Guild and Channel Frecency Structure**

\| Field | Type | Description | | ------------------ | -------------------------------------------------------------- | ---------------------- | | guild\_and\_channels | map\[fixed64, [frecency item](ajustes-de-usuario-proto.md#frecency-item-structure) object] | Frecent guild channels |

**Emoji Reaction Frecency Structure**

\| Field | Type | Description | | ------ | ------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ | | emojis | map\[string, [frecency item](ajustes-de-usuario-proto.md#frecency-item-structure) object] | Frecent used reactions, represented as snowflake IDs or friendly names (e.g. `skull_crossbones`) |

## Endpoints

> 📋 HEADER: Get User Settings Proto

Returns the requester's [user settings protobuf](ajustes-de-usuario-proto.md#user-settings-proto-type) for the specified type.

**Response Body**

\| Field | Type | Description | | -------- | ------ | ----------------------------------------------------- | | settings | string | The base 64-encoded serialized user settings protobuf |

> 📋 HEADER: Modify User Settings Proto

Modifies the requester's user settings protobuf for the specified type. Fires a User Settings Proto Update and User Settings Update Gateway event.

> ⚠️ ALERTA:

When updating protobuf user settings, the entire top-level field being modified must be sent, or any subfields not sent will be reset to their default values.

For example, if updating `PreloadedUserSettings.AppearanceSettings.developer_mode`, the entire `PreloadedUserSettings.AppearanceSettings` field must be sent, or everything in it will be reset.

Unchanged top-level fields can be omitted from the request.

> ⚠️ ALERTA:

This endpoint is ratelimited heavily. Updates should be batched together and sent at intervals.

Infrequent actions do not need a delay. Frequent actions should be delayed by 10 seconds and batched.\
Automated actions (such as migrations or frecency updates) should be delayed by 30 seconds and batched.\
Daily actions (things that change often and are not meaningful, such as emoji frencency) should be delayed by 1 day and batched.

**JSON Params**

\| Field | Type | Description | | -------------------------- | ------- | ------------------------------------------------------------------- | | settings | string | The base-64 encoded serialized user settings protobuf modifications | | required\_data\_version? ^1^ | integer | The required data version of the proto |

^1^ When making offline edits, the required data version of the proto should be set to the last known version. This ensures that the client doesn't overwrite newer edits made on a different client on edit.

**Response Body**

\| Field | Type | Description | | ------------ | ------- | ----------------------------------------------------------------------------------------- | | settings | string | The base-64 encoded serialized user settings protobuf | | out\_of\_date? | boolean | Whether the user settings update was discarded due to an outdated `required_data_version` |

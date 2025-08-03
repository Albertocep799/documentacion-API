---
icon: gavel
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

# Automoderación

### [Auto Moderation](automoderacion.md#auto-moderation) <a href="#auto-moderation" id="auto-moderation"></a>

Auto Moderation, or AutoMod, is a feature which allows each [guild](https://docs.discord.food/resources/guild) to set up rules that trigger based on some criteria. For example, a rule can trigger whenever a message contains a specific keyword.

Rules can be configured to automatically execute actions whenever they trigger. For example, if a user tries to send a message which contains a certain keyword, a rule can trigger and block the message before it is sent.

#### [AutoMod System Messages](automoderacion.md#automod-system-messages) <a href="#automod-system-messages" id="automod-system-messages"></a>

AutoMod system messages are sent as standard [messages](https://docs.discord.food/resources/message#message-object) in the guild with the [message type](https://docs.discord.food/resources/message#message-type). If no user is associated with the system message, the author is the AutoMod system account ().

These messages have a special [embed](https://docs.discord.food/resources/message#embed-object) structure which contains information about the action that was taken. The custom embed fields below are collapsed as a list of key-value pairs into the array on the [embed object](https://docs.discord.food/resources/message#embed-object).

[**AutoMod Alert**](automoderacion.md#automod-alert)

Sent when a [action type](automoderacion.md#automod-action-type) is triggered.

The author of the message is the user which generated the content that triggered the rule. The [embed description](https://docs.discord.food/resources/message#embed-object) contains the user message content that triggered the rule, if applicable.

[**AutoMod Alert Embed Structure**](automoderacion.md#automod-alert-embed-structure)

| Field                            | Type                                                                                          | Description                                                                                                          |
| -------------------------------- | --------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| rule\_name                       | string                                                                                        | The name of the [automod rule](automoderacion.md#automod-rule-object) that was triggered                             |
| decision\_id                     | string                                                                                        | The ID of the decision that was executed                                                                             |
| decision\_reason?                | string                                                                                        | The reason for the decision that was executed                                                                        |
| decision\_outcome                | string                                                                                        | The [outcome of the decision](automoderacion.md#automod-decision-outcome) that triggered the rule                    |
| channel\_id?                     | snowflake                                                                                     | The ID of the channel in which the user content was posted                                                           |
| flagged\_message\_id?            | snowflake                                                                                     | The ID of the message that triggered the rule                                                                        |
| keyword                          | string                                                                                        | The word or phrase configured that triggered the rule                                                                |
| keyword\_matched\_content        | string                                                                                        | The substring in content that triggered the rule                                                                     |
| block\_profile\_update\_type?    | string                                                                                        | The [type of profile update](automoderacion.md#automod-profile-update-type) that was blocked                         |
| quarantine\_user?                | string                                                                                        | The [reason for quarantining the user](automoderacion.md#automod-quarantine-user-reason)                             |
| quarantine\_user\_action?        | string                                                                                        | The [action taken on the quarantined user](automoderacion.md#automod-quarantine-user-action)                         |
| quarantine\_event?               | string                                                                                        | The [user action](automoderacion.md#automod-quarantine-event-type) that triggered the rule                           |
| voice\_channel\_status\_outcome? | string                                                                                        | The [outcome of the voice channel status update](automoderacion.md#automod-decision-outcome) that triggered the rule |
| application\_name?               | string                                                                                        | The name of the user application that triggered the rule                                                             |
| interaction\_user\_id?           | snowflake                                                                                     | The ID of the user that triggered the rule, if the author is a user application                                      |
| interaction\_callback\_type?     | string                                                                                        | The [type of interaction callback](automoderacion.md#automod-interaction-callback-type) that triggered the rule      |
| timeout\_duration?               | integer                                                                                       | Duration (in seconds) after which the timeout expires                                                                |
| alert\_actions\_execution?       | [alert actions execution](automoderacion.md#automod-alert-actions-execution-structure) object | The actions that were executed on the AutoMod alert                                                                  |

[**AutoMod Decision Outcome**](automoderacion.md#automod-decision-outcome)

| Value   | Description                       |
| ------- | --------------------------------- |
| flagged | The action was flagged by AutoMod |
| blocked | The action was blocked by AutoMod |

[**AutoMod Profile Update Type**](automoderacion.md#automod-profile-update-type)

| Value            | Description                                     |
| ---------------- | ----------------------------------------------- |
| nickname\_update | When a user updates their nickname in the guild |
| nickname\_reset  | When a user resets their nickname in the guild  |

[**AutoMod Quarantine User Reason**](automoderacion.md#automod-quarantine-user-reason)

| Value         | Description                                  |
| ------------- | -------------------------------------------- |
| username      | The user's username triggered the rule       |
| display\_name | The user's display name triggered the rule   |
| ~~bio~~       | ~~The user's bio triggered the rule~~        |
| nickname      | The user's guild nickname triggered the rule |
| clan\_tag     | The user's guild tag triggered the rule      |

[**AutoMod Quarantine User Action**](automoderacion.md#automod-quarantine-user-action)

| Value                  | Description                                                                                                                       |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| block\_guest\_join     | The guest was prevented from joining the guild                                                                                    |
| block\_profile\_update | The user was prevented from updating their profile in the guild                                                                   |
| quarantine\_user       | The user was quarantined; quarantined users, similar to timed out users, are prevented from interacting with the guild in any way |

[**AutoMod Quarantine Event Type**](automoderacion.md#automod-quarantine-event-type)

| Value             | Description                              |
| ----------------- | ---------------------------------------- |
| guild\_join       | When a user joins the guild              |
| message\_send     | When a user sends a message in the guild |
| username\_update  | When a user updates their username       |
| clan\_tag\_update | When a user updates their guild tag      |

[**AutoMod Interaction Callback Type**](automoderacion.md#automod-interaction-callback-type)

| Value | Description                                     |
| ----- | ----------------------------------------------- |
| modal | A modal interaction callback triggered the rule |

[**AutoMod Alert Actions Execution Structure**](automoderacion.md#automod-alert-actions-execution-structure)

| Field   | Type                                                                                  | Description                                                                                                              |
| ------- | ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| v       | integer                                                                               | The alert actions execution protocol version (currently 0)                                                               |
| actions | map\[string, [alert action](automoderacion.md#automod-alert-action-structure) object] | The actions that were executed on the AutoMod alert, keyed by [action type](automoderacion.md#automod-alert-action-type) |

[**AutoMod Alert Action Type**](automoderacion.md#automod-alert-action-type)

| Value | Name                  | Description                                       |
| ----- | --------------------- | ------------------------------------------------- |
| 1     | SET\_COMPLETED        | Marks the alert as completed                      |
| 2     | UNSET\_COMPLETED      | Marks the alert as not completed                  |
| 3     | DELETE\_USER\_MESSAGE | Deletes the user message that triggered the alert |
| 4     | SUBMIT\_FEEDBACK      | Reports an issue with the alert to Discord        |

[**AutoMod Alert Action Structure**](automoderacion.md#automod-alert-action-structure)

| Field | Type              | Description                                 |
| ----- | ----------------- | ------------------------------------------- |
| actor | snowflake         | The ID of the user that executed the action |
| ts    | ISO8601 timestamp | When the action was executed                |

[**Example AutoMod Alert Embed**](automoderacion.md#example-automod-alert-embed)

```
{  "type": "auto_moderation_message",  "description": "can i say alien 🥺",  "fields": [    {      "name": "rule_name",      "value": "No aliens",      "inline": false    },    {      "name": "channel_id",      "value": "1121695809839308901",      "inline": false    },    {      "name": "decision_id",      "value": "22a2df4cf7904b81a17faa3e3930af7d",      "inline": false    },    {      "name": "keyword",      "value": "alien",      "inline": false    },    {      "name": "keyword_matched_content",      "value": "alien",      "inline": false    },    {      "name": "flagged_message_id",      "value": "1200705269110411274",      "inline": false    },    {      "name": "timeout_duration",      "value": "600",      "inline": false    },    {      "name": "decision_outcome",      "value": "blocked",      "inline": false    },    {      "name": "alert_actions_execution",      "value": "{\"v\": 0, \"actions\": {\"3\": {\"actor\": \"852892297661906993\", \"ts\": \"2024-01-27T07:34:12.145393+00:00\"}, \"1\": {\"actor\": \"852892297661906993\", \"ts\": \"2024-01-27T07:38:29.292345+00:00\"}}}",      "inline": false    }  ]}
```

[**AutoMod Incident Notification**](automoderacion.md#automod-incident-notification)

Sent when a guild incident activity alert is triggered.

[**AutoMod Incident Notification Embed Structure**](automoderacion.md#automod-incident-notification-embed-structure)

| Field                                 | Type              | Description                                                                                                                                                           |
| ------------------------------------- | ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| notification\_type?                   | string            | The [type of notification](automoderacion.md#automod-incident-notification-type) that was triggered (default `raid`)                                                  |
| decision\_id?                         | string            | The ID of the decision that was executed                                                                                                                              |
| action\_by\_user\_id?                 | snowflake         | The ID of the user that executed the action (only applicable to `activity_alerts_enabled` [notification types](automoderacion.md#automod-incident-notification-type)) |
| raid\_type?                           | string            | The [type of raid](automoderacion.md#automod-raid-type) that was detected                                                                                             |
| raid\_datetime?                       | ISO8601 timestamp | When the raid was detected                                                                                                                                            |
| join\_attempts?                       | integer           | The approximate number of join attempts as part of the raid                                                                                                           |
| dms\_sent?                            | integer           | The approximate number of sent DMs as part of the raid                                                                                                                |
| suspicious\_mention\_activity\_until? | ISO8601 timestamp | When the mention activity restrictions will end (only applicable to `mention_raid` [notification types](automoderacion.md#automod-incident-notification-type))        |
| resolved\_reason?                     | string            | The [reason for resolving the notification](automoderacion.md#automod-raid-resolution-reason)                                                                         |

[**AutoMod Incident Notification Type**](automoderacion.md#automod-incident-notification-type)

| Value                     | Description                                   |
| ------------------------- | --------------------------------------------- |
| activity\_alerts\_enabled | Activity alerts were enabled in the guild     |
| raid                      | A raid was detected                           |
| mention\_raid             | A mention raid was detected                   |
| interaction\_blocked      | An anonymous interaction response was blocked |

[**AutoMod Raid Type**](automoderacion.md#automod-raid-type)

| Value         | Description                 |
| ------------- | --------------------------- |
| JOIN\_RAID    | A join raid was detected    |
| MENTION\_RAID | A mention raid was detected |

[**AutoMod Raid Resolution Reason**](automoderacion.md#automod-raid-resolution-reason)

| Value                | Description                                                                   |
| -------------------- | ----------------------------------------------------------------------------- |
| LEGITIMATE\_ACTIVITY | The increased activity was expected                                           |
| LEGITIMATE\_ACCOUNTS | The increased activity was caused by legitimate accounts                      |
| LEGITIMATE\_DMS      | The increased activity was caused by legitimate DMs                           |
| DM\_SPAM             | The increased activity was caused by DM spam and the spammers were removed    |
| JOIN\_RAID           | The increased activity was caused by a join raid and the raiders were removed |
| OTHER                | The increased activity was caused by another reason                           |

[**Example AutoMod Incident Notification Embed**](automoderacion.md#example-automod-incident-notification-embed)

```
{  "type": "auto_moderation_notification",  "fields": [    {      "name": "notification_type",      "value": "raid",      "inline": false    },    {      "name": "raid_datetime",      "value": "2023-08-15 22:25:33.184657+00:00",      "inline": false    },    {      "name": "raid_type",      "value": "JOIN_RAID",      "inline": false    },    {      "name": "join_attempts",      "value": "25",      "inline": false    },    {      "name": "dms_sent",      "value": "0",      "inline": false    },    {      "name": "resolved_reason",      "value": "LEGITIMATE_ACTIVITY",      "inline": false    }  ]}
```

#### [AutoMod Rule Object](automoderacion.md#automod-rule-object) <a href="#automod-rule-object" id="automod-rule-object"></a>

[**AutoMod Rule Structure**](automoderacion.md#automod-rule-structure)

| Field             | Type                                                                  | Description                                                                         |
| ----------------- | --------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| id                | snowflake                                                             | The ID of the rule                                                                  |
| guild\_id         | snowflake                                                             | The ID of the guild which this rule belongs to                                      |
| name              | string                                                                | The name of the rule                                                                |
| creator\_id       | snowflake                                                             | The ID of the user that created the rule                                            |
| event\_type       | integer                                                               | The [type of event](automoderacion.md#automod-event-type) that triggers the rule    |
| trigger\_type     | integer                                                               | The [type of trigger](automoderacion.md#automod-trigger-type) that invokes the rule |
| trigger\_metadata | [trigger metadata](automoderacion.md#automod-trigger-metadata) object | Metadata used to determine whether the rule should be triggered                     |
| actions           | array\[[action](automoderacion.md#automod-action-object) object]      | The actions that will execute when the rule is triggered                            |
| enabled           | boolean                                                               | Whether the rule is enabled                                                         |
| exempt\_roles     | array\[snowflake]                                                     | The IDs of the roles that won't be affected by the rule (max 20)                    |
| exempt\_channels  | array\[snowflake]                                                     | The IDs of the channels that won't be affected by the rule (max 50)                 |

[**Example AutoMod Rule**](automoderacion.md#example-automod-rule)

```
{  "id": "969707018069872670",  "guild_id": "613425648685547541",  "name": "Keyword Filter 1",  "creator_id": "423457898095789043",  "trigger_type": 1,  "event_type": 1,  "actions": [    {      "type": 1,      "metadata": { "custom_message": "Please keep financial discussions limited to the #finance channel" }    },    {      "type": 2,      "metadata": { "channel_id": "123456789123456789" }    },    {      "type": 3,      "metadata": { "duration_seconds": 60 }    }  ],  "trigger_metadata": {    "keyword_filter": ["cat*", "*dog", "*ana*", "i like c++"],    "regex_patterns": ["(b|c)at", "^(?:[0-9]{1,3}\\.){3}[0-9]{1,3}$"]  },  "enabled": true,  "exempt_roles": ["323456789123456789", "423456789123456789"],  "exempt_channels": ["523456789123456789"]}
```

[**AutoMod Trigger Type**](automoderacion.md#automod-trigger-type)

Characterizes the type of content which can trigger the rule.

| Value | Name              | Description                                                                       |
| ----- | ----------------- | --------------------------------------------------------------------------------- |
| 1     | KEYWORD           | When message content contains words from a user defined list of keywords (max 6)  |
| ~~2~~ | ~~HARMFUL\_LINK~~ | ~~When message content contains any harmful links (max 1)~~                       |
| 3     | SPAM              | When message content represents generic spam (max 1)                              |
| 4     | KEYWORD\_PRESET   | When message content contains words from internal predefined wordsets (max 1)     |
| 5     | MENTION\_SPAM     | When message content contains more unique mentions than allowed (max 1)           |
| 6     | USER\_PROFILE     | When a user's profile contains words from a user defined list of keywords (max 1) |
| 7     | GUILD\_POLICY     | When a user violates the guild rules (max 1)                                      |

[**AutoMod Trigger Metadata**](automoderacion.md#automod-trigger-metadata)

Additional data used to determine whether a rule should be triggered. Different fields are relevant based on the [trigger type](automoderacion.md#automod-trigger-type).

| Field                              | Type            | Description                                                                                                               | Trigger Type                                |
| ---------------------------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| keyword\_filter <sup>1</sup>       | array\[string]  | Substrings which will be searched for in content (1-60 characters, max 1000)                                              | `KEYWORD`, `USER_PROFILE`                   |
| regex\_patterns <sup>2</sup>       | array\[string]  | Regular expression patterns which will be matched against content (1-260 characters, max 10)                              | `KEYWORD`, `USER_PROFILE`                   |
| presets                            | array\[integer] | The [internally predefined wordsets](automoderacion.md#automod-keyword-preset-type) which will be searched for in content | `KEYWORD_PRESET`                            |
| allow\_list <sup>3</sup>           | array\[string]  | Substrings which should not trigger the rule (1-60 characters, max 100 or 1000 respectively)                              | `KEYWORD`, `KEYWORD_PRESET`, `USER_PROFILE` |
| mention\_total\_limit              | integer         | Number of unique role and user mentions allowed per message (max 50)                                                      | `MENTION_SPAM`                              |
| mention\_raid\_protection\_enabled | boolean         | Whether to automatically detect mention raids                                                                             | `MENTION_SPAM`                              |

<sup>1</sup> A keyword can be a phrase which contains multiple words. [Wildcard symbols](automoderacion.md#automod-keyword-matching-strategies) can be used to customize how each keyword will be matched. Each keyword must be 60 characters or less.

<sup>2</sup> Only Rust flavored regex is currently supported, which can be tested in online editors such as [Rustexp](https://rustexp.lpil.uk/). Each regex pattern must be 260 characters or less.

<sup>3</sup> Each keyword can be a phrase which contains multiple words. [Wildcard symbols](automoderacion.md#automod-keyword-matching-strategies) can be used to customize how each keyword will be matched. Rules with [trigger types](automoderacion.md#automod-trigger-type) accept a maximum of 100 keywords. Rules with [trigger types](automoderacion.md#automod-trigger-type) accept a maximum of 1000 keywords.

[**AutoMod Keyword Preset Type**](automoderacion.md#automod-keyword-preset-type)

| Value | Name            | Description                                                  |
| ----- | --------------- | ------------------------------------------------------------ |
| 1     | PROFANITY       | Words that may be considered forms of swearing or cursing    |
| 2     | SEXUAL\_CONTENT | Words that refer to sexually explicit behavior or activity   |
| 3     | SLURS           | Personal insults or words that may be considered hate speech |

[**AutoMod Event Type**](automoderacion.md#automod-event-type)

Indicates in what event context a rule should be checked.

| Value | Name                 | Description                                         | Trigger Type                                                        |
| ----- | -------------------- | --------------------------------------------------- | ------------------------------------------------------------------- |
| 1     | MESSAGE\_SEND        | When a member sends or edits a message in the guild | `KEYWORD`, `SPAM`, `KEYWORD_PRESET`, `MENTION_SPAM`, `GUILD_POLICY` |
| 2     | GUILD\_MEMBER\_EVENT | When a member joins or updates their profile        | `USER_PROFILE`                                                      |

[**AutoMod Keyword Matching Strategies**](automoderacion.md#automod-keyword-matching-strategies)

Use the wildcard symbol () at the beginning or end of a keyword to define how it should be matched. All keywords are case insensitive.

**Prefix** - Word must start with the keyword

| Keyword   | Matches                               |
| --------- | ------------------------------------- |
| cat\*     | **cat**ch, **Cat**apult, **CAt**tLE   |
| tra\*     | **tra**in, **tra**de, **TRA**ditional |
| the mat\* | **the mat**rix                        |

**Suffix** - Word must end with the keyword

| Keyword   | Matches                             |
| --------- | ----------------------------------- |
| \*cat     | wild**cat**, copy**Cat**            |
| \*tra     | ex**tra**, ul**tra**, orches**TRA** |
| \*the mat | brea**the mat**                     |

**Anywhere** - Keyword can appear anywhere in the content

| Keyword     | Matches                     |
| ----------- | --------------------------- |
| \*cat\*     | lo**cat**ion, edu**Cat**ion |
| \*tra\*     | abs**tra**cted, ou**tra**ge |
| \*the mat\* | brea**the mat**ter          |

**Whole Word** - Keyword is a full word or phrase and must be surrounded by whitespace

| Keyword | Matches     |
| ------- | ----------- |
| cat     | **cat**     |
| train   | **train**   |
| the mat | **the mat** |

#### [AutoMod Action Object](automoderacion.md#automod-action-object) <a href="#automod-action-object" id="automod-action-object"></a>

An action which will execute whenever a rule is triggered.

[**AutoMod Action Structure**](automoderacion.md#automod-action-structure)

| Field                  | Type                                                                | Description                                                               |
| ---------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| type                   | integer                                                             | The [type of action](automoderacion.md#automod-action-type)               |
| metadata? <sup>1</sup> | [action metadata](automoderacion.md#automod-action-metadata) object | Additional metadata needed during execution for this specific action type |

<sup>1</sup> See the "Action Type" column in [action metadata](automoderacion.md#automod-action-metadata) to understand which values require to be set.

[**AutoMod Action Type**](automoderacion.md#automod-action-type)

| Value | Name                              | Description                                                                                                                                                                |
| ----- | --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1     | BLOCK\_MESSAGE                    | Block a member's message and prevent it from being posted; a custom explanation can be specified and shown to members whenever their message is blocked                    |
| 2     | SEND\_ALERT\_MESSAGE <sup>1</sup> | Log user content to a specified channel                                                                                                                                    |
| 3     | TIMEOUT\_USER <sup>2</sup>        | Timeout user for a specified duration                                                                                                                                      |
| 4     | QUARANTINE\_USER <sup>3</sup>     | Block guild join, profile update, or quarantine user indefinitely; quarantined users, similar to timed out users, are prevented from interacting with the guild in any way |

<sup>1</sup> Only a action can be set up for [trigger types](automoderacion.md#automod-trigger-type).

<sup>2</sup> A action can only be set up for and [trigger types](automoderacion.md#automod-trigger-type). The permission is required to use the action type.

<sup>3</sup> A action can only be set up for [trigger types](automoderacion.md#automod-trigger-type). The permission is required to use the action type.

[**AutoMod Action Metadata**](automoderacion.md#automod-action-metadata)

Additional data used when an action is executed. Different fields are relevant based on the [action type](automoderacion.md#automod-action-type).

| Field             | Type      | Description                                                                            | Action Type          |
| ----------------- | --------- | -------------------------------------------------------------------------------------- | -------------------- |
| channel\_id       | snowflake | The channel where user content should be logged                                        | SEND\_ALERT\_MESSAGE |
| duration\_seconds | integer   | Duration (in seconds) after which the timeout expires (max 2419200)                    | TIMEOUT\_USER        |
| custom\_message?  | string    | Additional explanation that will be shown to members whenever their message is blocked | BLOCK\_MESSAGE       |

### [AutoMod Incidents Data Object](automoderacion.md#automod-incidents-data-object) <a href="#automod-incidents-data-object" id="automod-incidents-data-object"></a>

[**AutoMod Incidents Data Structure**](automoderacion.md#automod-incidents-data-structure)

| Field                    | Type               | Description                                             |
| ------------------------ | ------------------ | ------------------------------------------------------- |
| raid\_detected\_at       | ?ISO8601 timestamp | When the last raid was detected                         |
| dm\_spam\_detected\_at   | ?ISO8601 timestamp | When the last DM spam was detected                      |
| invites\_disabled\_until | ?ISO8601 timestamp | When invites will be re-enabled (max 24 hours from now) |
| dms\_disabled\_until     | ?ISO8601 timestamp | When DMs will be re-enabled (max 24 hours from now)     |

[**Example AutoMod Incidents Data**](automoderacion.md#example-automod-incidents-data)

```
{  "raid_detected_at": "2024-01-01T18:00:00.000000+00:00",  "dm_spam_detected_at": "2024-01-01T18:00:00.000000+00:00",  "invites_disabled_until": "2024-01-01T18:00:00.000000+00:00",  "dms_disabled_until": "2024-01-01T18:00:00.000000+00:00"}
```

### [Endpoints](automoderacion.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get Guild AutoMod Rules](automoderacion.md#get-guild-automod-rules) <a href="#get-guild-automod-rules" id="get-guild-automod-rules"></a>

`GET/guilds/{guild.id}/auto-moderation/rules`

Returns a list of [automod rule](automoderacion.md#automod-rule-object) objects for the configured rules in the guild. Requires the permission.

#### [Get Guild AutoMod Rule](automoderacion.md#get-guild-automod-rule) <a href="#get-guild-automod-rule" id="get-guild-automod-rule"></a>

`GET/guilds/{guild.id}/auto-moderation/rules/{automod_rule.id}`

Returns an [automod rule](automoderacion.md#automod-rule-object) object for the given rule ID in the guild. Requires the permission.

#### [Create Guild AutoMod Rule](automoderacion.md#create-guild-automod-rule) <a href="#create-guild-automod-rule" id="create-guild-automod-rule"></a>

`POST/guilds/{guild.id}/auto-moderation/rules`

Creates a new automod rule in the guild. Requires the permission. Returns an [automod rule](automoderacion.md#automod-rule-object) on success. Fires an [Auto Moderation Rule Create](https://docs.discord.food/topics/gateway-events#auto-moderation-rule-create) Gateway event.

[**JSON Params**](automoderacion.md#json-params)

| Field                           | Type                                                                     | Description                                                                         |
| ------------------------------- | ------------------------------------------------------------------------ | ----------------------------------------------------------------------------------- |
| name                            | string                                                                   | The name of the rule                                                                |
| event\_type                     | integer                                                                  | The [type of event](automoderacion.md#automod-event-type) that triggers the rule    |
| trigger\_type                   | integer                                                                  | The [type of trigger](automoderacion.md#automod-trigger-type) that invokes the rule |
| trigger\_metadata? <sup>1</sup> | [trigger metadata](automoderacion.md#automod-trigger-metadata)           | Metadata used to determine whether the rule should be triggered                     |
| actions                         | array\[[automod action](automoderacion.md#automod-action-object) object] | The actions that will execute when the rule is triggered                            |
| enabled?                        | boolean                                                                  | Whether the rule is enabled (default false)                                         |
| exempt\_roles?                  | array\[snowflake]                                                        | The IDs of the roles that won't be affected by the rule (max 20)                    |
| exempt\_channels? <sup>2</sup>  | array\[snowflake]                                                        | The IDs of the channels that won't be affected by the rule (max 50)                 |

<sup>1</sup> See the "Trigger Types" column in [trigger metadata](automoderacion.md#automod-trigger-metadata) to understand which [trigger types](automoderacion.md#automod-trigger-type) require to be set.

<sup>2</sup> Only applicable to , , , , and [trigger types](automoderacion.md#automod-trigger-type).

#### [Validate Guild AutoMod Rule](automoderacion.md#validate-guild-automod-rule) <a href="#validate-guild-automod-rule" id="validate-guild-automod-rule"></a>

`POST/guilds/{guild.id}/auto-moderation/rules/validate`

Validates a potential rule request's schema for the guild. Requires the permission.

[**JSON Params**](automoderacion.md#json-params)

| Field                          | Type                                                                  | Description                      |
| ------------------------------ | --------------------------------------------------------------------- | -------------------------------- |
| trigger\_metadata <sup>1</sup> | [trigger metadata](automoderacion.md#automod-trigger-metadata) object | The trigger metadata to validate |

<sup>1</sup> See the "Trigger Types" column in [trigger metadata](automoderacion.md#automod-trigger-metadata) to understand which [trigger types](automoderacion.md#automod-trigger-type) require to be set.

[**Response Body**](automoderacion.md#response-body)

| Field             | Type                                                                  | Description                    |
| ----------------- | --------------------------------------------------------------------- | ------------------------------ |
| trigger\_metadata | [trigger metadata](automoderacion.md#automod-trigger-metadata) object | The validated trigger metadata |

#### [Modify Guild AutoMod Rule](automoderacion.md#modify-guild-automod-rule) <a href="#modify-guild-automod-rule" id="modify-guild-automod-rule"></a>

`PATCH/guilds/{guild.id}/auto-moderation/rules/{automod_rule.id}`

Modifies an existing rule in the guild. Requires the permission. Returns an [automod rule](automoderacion.md#automod-rule-object) on success. Fires an [Auto Moderation Rule Update](https://docs.discord.food/topics/gateway-events#auto-moderation-rule-update) Gateway event.

[**JSON Params**](automoderacion.md#json-params)

| Field                           | Type                                                                     | Description                                                                      |
| ------------------------------- | ------------------------------------------------------------------------ | -------------------------------------------------------------------------------- |
| name?                           | string                                                                   | The name of the rule                                                             |
| event\_type?                    | integer                                                                  | The [type of event](automoderacion.md#automod-event-type) that triggers the rule |
| trigger\_metadata? <sup>1</sup> | [trigger metadata](automoderacion.md#automod-trigger-metadata) object    | Metadata used to determine whether the rule should be triggered                  |
| actions?                        | array\[[automod action](automoderacion.md#automod-action-object) object] | The actions that will execute when the rule is triggered                         |
| enabled?                        | boolean                                                                  | Whether the rule is enabled (default false)                                      |
| exempt\_roles?                  | array\[snowflake]                                                        | The IDs of the roles that won't be affected by the rule (max 20)                 |
| exempt\_channels? <sup>2</sup>  | array\[snowflake]                                                        | The IDs of the channels that won't be affected by the rule (max 50)              |

<sup>1</sup> See the "Trigger Types" column in [trigger metadata](automoderacion.md#automod-trigger-metadata) to understand which [trigger types](automoderacion.md#automod-trigger-type) require to be set.

<sup>2</sup> Only applicable to , , , , and [trigger types](automoderacion.md#automod-trigger-type).

#### [Delete Guild AutoMod Rule](automoderacion.md#delete-guild-automod-rule) <a href="#delete-guild-automod-rule" id="delete-guild-automod-rule"></a>

`DELETE/guilds/{guild.id}/auto-moderation/rules/{automod_rule.id}`

Deletes a rule in the guild. Returns a 204 empty response on success. Requires the permission. Fires an [Auto Moderation Rule Delete](https://docs.discord.food/topics/gateway-events#auto-moderation-rule-delete) Gateway event.

#### [Execute AutoMod Alert Action](automoderacion.md#execute-automod-alert-action) <a href="#execute-automod-alert-action" id="execute-automod-alert-action"></a>

`POST/guilds/{guild.id}/auto-moderation/alert-action`

Executes an alert action on an [AutoMod alert](automoderacion.md#automod-alert). Requires the permission. Returns a 204 empty response on success. Fires a [Message Update](https://docs.discord.food/topics/gateway-events#message-update) Gateway event.

[**JSON Params**](automoderacion.md#json-params)

| Field               | Type      | Description                                                                        |
| ------------------- | --------- | ---------------------------------------------------------------------------------- |
| channel\_id         | snowflake | The ID of the channel where the alert was sent                                     |
| message\_id         | snowflake | The ID of the AutoMod system message                                               |
| alert\_action\_type | integer   | The [type of alert action](automoderacion.md#automod-alert-action-type) to execute |

#### [Modify AutoMod Incident Actions](automoderacion.md#modify-automod-incident-actions) <a href="#modify-automod-incident-actions" id="modify-automod-incident-actions"></a>

`PUT/guilds/{guild.id}/incident-actions`

Sets the incident actions for the guild. Requires the permission. Fires a [Guild Update](https://docs.discord.food/topics/gateway-events#guild-update) Gateway event.

[**JSON Params**](automoderacion.md#json-params)

| Field                     | Type              | Description                                             |
| ------------------------- | ----------------- | ------------------------------------------------------- |
| invites\_disabled\_until? | ISO8601 timestamp | When invites will be re-enabled (max 24 hours from now) |
| dms\_disabled\_until?     | ISO8601 timestamp | When DMs will be re-enabled (max 24 hours from now)     |

[**Response Body**](automoderacion.md#response-body)

| Field                    | Type              | Description                                             |
| ------------------------ | ----------------- | ------------------------------------------------------- |
| invites\_disabled\_until | ISO8601 timestamp | When invites will be re-enabled (max 24 hours from now) |
| dms\_disabled\_until     | ISO8601 timestamp | When DMs will be re-enabled (max 24 hours from now)     |

[**Example Response**](automoderacion.md#example-response)

```
{  "invites_disabled_until": "2024-01-01T18:00:00.000000+00:00",  "dms_disabled_until": "2024-01-01T18:00:00.000000+00:00"}
```

#### [Resolve AutoMod Incident](automoderacion.md#resolve-automod-incident) <a href="#resolve-automod-incident" id="resolve-automod-incident"></a>

`POST/guilds/{guild.id}/auto-moderation/false-alarm`

Resolves an [AutoMod incident](automoderacion.md#automod-incident-notification). Requires the permission. Returns a 204 empty response on success. Fires a [Message Update](https://docs.discord.food/topics/gateway-events#message-update) Gateway event.

[**JSON Params**](automoderacion.md#json-params)

| Field              | Type      | Description                                                                                   |
| ------------------ | --------- | --------------------------------------------------------------------------------------------- |
| alert\_message\_id | snowflake | The ID of the AutoMod system message                                                          |
| reason             | string    | The [reason for resolving the notification](automoderacion.md#automod-raid-resolution-reason) |

#### [Report AutoMod Incident](automoderacion.md#report-automod-incident) <a href="#report-automod-incident" id="report-automod-incident"></a>

`POST/guilds/{guild.id}/auto-moderation/report-raid`

Reports an ongoing raid [AutoMod incident](automoderacion.md#automod-incident-notification). Requires the permission. Returns a 204 empty response on success. Fires a [Message Create](https://docs.discord.food/topics/gateway-events#message-update) Gateway event.

#### [Clear Mention Raid Incident](automoderacion.md#clear-mention-raid-incident) <a href="#clear-mention-raid-incident" id="clear-mention-raid-incident"></a>

`POST/guilds/{guild.id}/auto-moderation/clear-mention-raid`

Clears a mention raid [AutoMod incident](automoderacion.md#automod-incident-notification). Requires the permission. Returns a 204 empty response on success.

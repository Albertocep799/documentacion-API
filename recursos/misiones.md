---
icon: swords
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

# Misiones

### [Quests](misiones.md#quests) <a href="#quests" id="quests"></a>

Quests are a way for Discord to promote games and other content to users. Users can receive rewards for completing quests, such as redeemable codes, in-game items, or collectibles.

#### [Quest Object](misiones.md#quest-object) <a href="#quest-object" id="quest-object"></a>

A sponsored quest.

[**Quest Structure**](misiones.md#quest-structure)

| Field                                           | Type                                                              | Description                                                                      |
| ----------------------------------------------- | ----------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| id                                              | snowflake                                                         | The ID of the quest                                                              |
| config                                          | [quest config](misiones.md#quest-config-object) object            | The configuration and metadata for the quest                                     |
| user\_status                                    | ?[quest user status](misiones.md#quest-user-status-object) object | The user's quest progress, if it has been accepted                               |
| targeted\_content **(deprecated)** <sup>1</sup> | ?array\[integer]                                                  | The [content areas where the quest can be shown](misiones.md#quest-content-type) |
| preview                                         | boolean                                                           | Whether the quest is unreleased and in preview for Discord employees             |

<sup>1</sup> Some quest content areas may be dismissed using the [Dismiss Quest Content](misiones.md#dismiss-quest-content) endpoint.

[**Partial Quest Structure**](misiones.md#partial-quest-structure)

| Field | Type      | Description         |
| ----- | --------- | ------------------- |
| id    | snowflake | The ID of the quest |

#### [Quest Config Object](misiones.md#quest-config-object) <a href="#quest-config-object" id="quest-config-object"></a>

The quest definition.

[**Quest Config Structure**](misiones.md#quest-config-structure)

The config structure has multiple distinct versions with different field sets. Only actively used versions are kept documented. As of now, only the latest version is available.

| Field                | Type                                                                              | Description                                                           |
| -------------------- | --------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| id                   | snowflake                                                                         | The ID of the quest                                                   |
| config\_version      | integer                                                                           | [Quest configuration version](misiones.md#quest-config-version)       |
| starts\_at           | ISO8601 timestamp                                                                 | When the quest period starts                                          |
| expires\_at          | ISO8601 timestamp                                                                 | When the quest period ends                                            |
| features             | array\[integer]                                                                   | The [quest features](misiones.md#quest-feature) enabled for the quest |
| application          | [quest application](misiones.md#quest-application-structure) object               | The application metadata for the quest                                |
| assets               | [quest assets](misiones.md#quest-assets-structure) object                         | Object that holds the quest's assets                                  |
| colors               | [quest gradient](misiones.md#quest-gradient-structure) object                     | The accent colors for the quest                                       |
| messages             | [quest messages](misiones.md#quest-messages-structure) object                     | Human-readable metadata for the quest                                 |
| task\_config         | [quest task config](misiones.md#quest-task-config-structure) object               | The task configuration for the quest                                  |
| rewards\_config      | [quest rewards config](misiones.md#quest-rewards-config-structure) object         | Specifies rewards for the quest (e.g. collectibles)                   |
| video\_metadata?     | [quest video metadata](misiones.md#quest-video-metadata-structure) object         | The configuration for the video quest                                 |
| cosponsor\_metadata? | [quest cosponsor metadata](misiones.md#quest-cosponsor-metadata-structure) object | The configuration for the quest co-sponsor                            |

[**Quest Application Structure**](misiones.md#quest-application-structure)

| Field | Type      | Description                 |
| ----- | --------- | --------------------------- |
| id    | snowflake | The ID of the application   |
| name  | string    | The name of the application |
| link  | string    | The link to the game's page |

[**Quest Assets Structure**](misiones.md#quest-assets-structure)

An object holding [CDN asset names](https://docs.discord.food/reference#cdn-formatting).

| Field                   | Type    | Description                                                                                                                                         |
| ----------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| hero                    | string  | The quest's [hero image](https://en.wiktionary.org/wiki/hero_image)                                                                                 |
| hero\_video             | ?string | A video representation of the hero image                                                                                                            |
| quest\_bar\_hero        | string  | The [hero image](https://en.wiktionary.org/wiki/hero_image) used in the quest popup that appears when launching the game before accepting the quest |
| quest\_bar\_hero\_video | ?string | A video representation of the quest bar hero image                                                                                                  |
| game\_tile              | string  | The game's icon                                                                                                                                     |
| logotype                | string  | The game's [logo](https://en.wikipedia.org/wiki/Logo)                                                                                               |

[**Quest Gradient Structure**](misiones.md#quest-gradient-structure)

A 2-point gradient with a and color.

| Field     | Type   | Description                                     |
| --------- | ------ | ----------------------------------------------- |
| primary   | string | The hex-encoded primary color of the gradient   |
| secondary | string | The hex-encoded secondary color of the gradient |

[**Quest Messages Structure**](misiones.md#quest-messages-structure)

| Field           | Type   | Description                                |
| --------------- | ------ | ------------------------------------------ |
| quest\_name     | string | The name of the quest                      |
| game\_title     | string | The title of the game the quest is for     |
| game\_publisher | string | The publisher of the game the quest is for |

[**Quest Task Config Structure**](misiones.md#quest-task-config-structure)

| Field                       | Type                                                                | Description                                                                                          |
| --------------------------- | ------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| type                        | integer                                                             | The [type of task configuration](misiones.md#quest-task-config-type)                                 |
| join\_operator <sup>1</sup> | string                                                              | The eligibility operator used to join multiple tasks (`and` or `or`)                                 |
| tasks                       | map\[string, [quest task](misiones.md#quest-task-structure) object] | Tasks required to complete the quest, keyed by their [event name](misiones.md#quest-task-event-name) |
| enrollment\_url?            | string                                                              | A link to the third-party quest tasks enrollment page                                                |
| developer\_application\_id? | snowflake                                                           | The ID of the embedded activity for the third-party task                                             |

<sup>1</sup> For a task to be considered complete, the user must complete either all tasks (when is ) or at least one task (when is ).

[**Quest Task Structure**](misiones.md#quest-task-structure)

| Field               | Type           | Description                                                 |
| ------------------- | -------------- | ----------------------------------------------------------- |
| event\_name         | string         | The [type of task event](misiones.md#quest-task-event-name) |
| target <sup>1</sup> | integer        | The required value                                          |
| external\_ids?      | array\[string] | IDs of the target game on console platforms                 |
| title?              | string         | The third-party task title                                  |
| description?        | string         | The third-party task description                            |

<sup>1</sup> While this is an opaque value, for duration-based tasks, this will be a duration in seconds.

[**Quest Task Config Type**](misiones.md#quest-task-config-type)

| Value | Name         | Description               |
| ----- | ------------ | ------------------------- |
| 1     | FIRST\_PARTY | The tasks are first-party |
| 2     | THIRD\_PARTY | The tasks are third-party |

[**Quest Task Event Name**](misiones.md#quest-task-event-name)

| Value                     | Description                                                                                                                                                                                               |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| STREAM\_ON\_DESKTOP       | The user must play and stream the game on desktop to at least one other user for a certain duration (see [Update Activity Session](https://docs.discord.food/resources/presence#update-activity-session)) |
| PLAY\_ON\_DESKTOP         | The user must play the game on desktop for a certain duration (see [Update Activity Session](https://docs.discord.food/resources/presence#update-activity-session))                                       |
| PLAY\_ON\_DESKTOP\_V2     | The user must play the game on desktop for a certain duration (see [Update Activity Session](https://docs.discord.food/resources/presence#update-activity-session))                                       |
| PLAY\_ON\_XBOX            | The user must play the game on Xbox for a certain duration                                                                                                                                                |
| PLAY\_ON\_PLAYSTATION     | The user must play the game on PlayStation for a certain duration                                                                                                                                         |
| WATCH\_VIDEO              | The user must watch a video for a certain duration                                                                                                                                                        |
| WATCH\_VIDEO\_ON\_MOBILE  | The user must watch a video on mobile for a certain duration                                                                                                                                              |
| PLAY\_ACTIVITY            | The user must play the embedded activity for a certain duration                                                                                                                                           |
| ACHIEVEMENT\_IN\_GAME     | The user must complete an achievement in the game                                                                                                                                                         |
| ACHIEVEMENT\_IN\_ACTIVITY | The user must complete an achievement in the embedded activity                                                                                                                                            |
| progress <sup>1</sup>     | The user must complete a certain number of tasks in an embedded activity                                                                                                                                  |

<sup>1</sup> Completion of this task is tracked by the embedded activity itself.

[**Quest Rewards Config Structure**](misiones.md#quest-rewards-config-structure)

| Field               | Type                                                              | Description                                                                     |
| ------------------- | ----------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| assignment\_method  | integer                                                           | [How the rewards are assigned](misiones.md#quest-reward-assignment-method)      |
| rewards             | array\[[quest reward](misiones.md#quest-reward-structure) object] | The possible rewards for the quest, ordered by tier (if applicable)             |
| rewards\_expire\_at | ?ISO8601 timestamp                                                | When the reward claiming period ends                                            |
| platforms           | array\[integer]                                                   | The [platforms](misiones.md#quest-platform-type) the rewards can be redeemed on |

[**Quest Reward Structure**](misiones.md#quest-reward-structure)

| Field                            | Type                                                                        | Description                                                                    |
| -------------------------------- | --------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| type                             | integer                                                                     | The [reward's type](misiones.md#quest-reward-type)                             |
| sku\_id                          | snowflake                                                                   | The ID of the SKU awarded                                                      |
| asset?                           | ?string                                                                     | The reward's [media asset](https://docs.discord.food/reference#cdn-formatting) |
| asset\_video?                    | ?string                                                                     | The reward's [video asset](https://docs.discord.food/reference#cdn-formatting) |
| messages                         | [quest reward messages](misiones.md#quest-reward-messages-structure) object | Human-readable metadata for the reward                                         |
| approximate\_count? <sup>1</sup> | ?integer                                                                    | An approximate count of how many users can claim the reward                    |
| redemption\_link?                | ?string                                                                     | The link to redeem the reward                                                  |
| expires\_at?                     | ?ISO8601 timestamp                                                          | When the reward expires                                                        |
| expires\_at\_premium?            | ?ISO8601 timestamp                                                          | When the reward expires for premium users                                      |
| expiration\_mode?                | integer                                                                     | The [expiration mode](misiones.md#quest-reward-expiration-mode)                |
| orb\_quantity?                   | integer                                                                     | The amount of Discord Orbs awarded                                             |
| quantity?                        | integer                                                                     | The days of fractional premium awarded                                         |

<sup>1</sup> If the amount of users who claimed the awards exceeds this count, then all future claimers will be assigned the next reward tier in the list.

[**Quest Reward Messages Structure**](misiones.md#quest-reward-messages-structure)

| Field                                           | Type                  | Description                                                                             |
| ----------------------------------------------- | --------------------- | --------------------------------------------------------------------------------------- |
| name                                            | string                | The reward's name                                                                       |
| name\_with\_article                             | string                | The article variant of the name (e.g. a Cybernetic Headgear Decoration)                 |
| reward\_redemption\_instructions\_by\_platform? | map\[integer, string] | The instrutions on redeeming the reward [per-platform](misiones.md#quest-platform-type) |

[**Quest Reward Assignment Method**](misiones.md#quest-reward-assignment-method)

The method used to assign the reward to a user.

| Value | Name   | Description                                          |
| ----- | ------ | ---------------------------------------------------- |
| 1     | ALL    | All rewards are assigned to the user upon completion |
| 2     | TIERED | The rewards are assigned in tiers                    |

[**Quest Reward Type**](misiones.md#quest-reward-type)

The type of reward that the user will receive.

| Value | Name                | Description                                                                           |
| ----- | ------------------- | ------------------------------------------------------------------------------------- |
| 1     | REWARD\_CODE        | The reward is a redeemable code                                                       |
| 2     | IN\_GAME            | The reward is automatically given to the user in the promoted game                    |
| 3     | COLLECTIBLE         | The reward is a Discord collectible (e.g. an avatar decoration)                       |
| 4     | VIRTUAL\_CURRENCY   | The reward is a virtual currency (Discord Orbs)                                       |
| 5     | FRACTIONAL\_PREMIUM | The reward is a limited free premium (Nitro) trial for a fraction of a billing period |

[**Quest Reward Expiration Mode**](misiones.md#quest-reward-expiration-mode)

Controls the expiration behavior of rewards.

| Value | Name               | Description                                                                                |
| ----- | ------------------ | ------------------------------------------------------------------------------------------ |
| 1     | NORMAL             | The reward expires after a set period of time                                              |
| 2     | PREMIUM\_EXTENSION | The reward lasts longer for premium (Nitro) users                                          |
| 3     | PREMIUM\_PERMANENT | The reward is permanent for premium (Nitro) users, even after their subscription has ended |

[**Quest Video Metadata Structure**](misiones.md#quest-video-metadata-structure)

| Field    | Type                                                                      | Description                                 |
| -------- | ------------------------------------------------------------------------- | ------------------------------------------- |
| messages | [quest video messages](misiones.md#quest-video-messages-structure) object | Human-readable metadata for the video quest |
| assets   | [quest video assets](misiones.md#quest-video-assets-structure) object     | Object that holds the quest's video assets  |

[**Quest Video Messages Structure**](misiones.md#quest-video-messages-structure)

| Field                          | Type   | Description                                                    |
| ------------------------------ | ------ | -------------------------------------------------------------- |
| video\_title                   | string | The title of the video                                         |
| video\_end\_cta\_title         | string | The title of the call-to-action at the end of the video        |
| video\_end\_cta\_subtitle      | string | The subtitle of the call-to-action at the end of the video     |
| video\_end\_cta\_button\_label | string | The label of the call-to-action button at the end of the video |

[**Quest Video Assets Structure**](misiones.md#quest-video-assets-structure)

| Field                          | Type    | Description                                         |
| ------------------------------ | ------- | --------------------------------------------------- |
| video\_player\_video\_hls      | ?string | The HLS video asset for the video player            |
| video\_player\_video           | string  | The video asset for the video player                |
| video\_player\_thumbnail       | ?string | The thumbnail asset for the video player            |
| video\_player\_video\_low\_res | string  | The low-resolution video asset for the video player |
| video\_player\_caption         | string  | The caption asset for the video player              |
| video\_player\_transcript      | string  | The transcript asset for the video player           |
| quest\_bar\_preview\_video     | ?string | The video asset for the quest bar preview           |
| quest\_bar\_preview\_thumbnail | ?string | The thumbnail asset for the quest bar preview       |
| quest\_home\_video             | ?string | The video asset for the quest home page             |

| Field                    | Type   | Description                              |
| ------------------------ | ------ | ---------------------------------------- |
| name                     | string | The name of the co-sponsor               |
| logotype                 | string | The co-sponsor's logo asset              |
| redemption\_instructions | string | The co-sponsor's redemption instructions |

[**Quest Config Version**](misiones.md#quest-config-version)

The version of the quest configuration.

| Value | Status       |
| ----- | ------------ |
| 2     | Active       |
| 1     | Discontinued |

[**Quest Content Type**](misiones.md#quest-content-type)

Areas where the quest can be shown in the Discord client.

| Value | Name                             | Description                                                         | Dismissable |
| ----- | -------------------------------- | ------------------------------------------------------------------- | ----------- |
| 0     | GIFT\_INVENTORY\_SETTINGS\_BADGE | This quest is shown as a badge in User Settings                     | Yes         |
| 1     | QUEST\_BAR                       | This quest is shown as a bar above the user popout                  | Yes         |
| 2     | QUEST\_INVENTORY\_CARD           | This quest is shown as a card in the user's gift inventory          | No          |
| 3     | QUESTS\_EMBED                    | This quest is shown as an embed in chat                             | No          |
| 4     | ACTIVITY\_PANEL                  | This quest is shown in the Active Now page                          | Yes         |
| 5     | QUEST\_LIVE\_STREAM              | This quest is shown while watching a stream                         | Yes         |
| 6     | MEMBERS\_LIST                    | This quest is shown in the member list                              | No          |
| 7     | QUEST\_BADGE                     | This quest is shown on the quest profile badge upsell               | No          |
| 8     | GIFT\_INVENTORY\_FOR\_YOU        | This quest is featured in the user's gift inventory for you section | No          |
| 9     | GIFT\_INVENTORY\_OTHER           | This quest is featured in the user's gift inventory                 | No          |
| 10    | QUEST\_BAR\_V2                   | This quest is shown in the new quest bar design                     | Yes         |
| 11    | QUEST\_HOME\_DESKTOP             | This quest is shown on the desktop Quest discovery page             | No          |
| 12    | QUEST\_HOME\_MOBILE              | This quest is shown on the mobile Quest discovery page              | No          |
| 13    | QUEST\_BAR\_MOBILE               | This quest is shown in the mobile Quest bar design                  | Yes         |
| 14    | THIRD\_PARTY\_APP                | This quest is shown in a third-party app                            | No          |
| 15    | QUEST\_BOTTOM\_SHEET             | This quest is shown in the bottom sheet                             | No          |
| 16    | QUEST\_EMBED\_MOBILE             | This quest is shown in the mobile Quest embed                       | No          |
| 17    | QUEST\_HOME\_MOVE\_CALLOUT       | This quest is shown in the move callout on the Quest discovery page | No          |
| 18    | DISCOVERY\_SIDEBAR               | This quest is shown in the discovery sidebar                        | No          |
| 19    | QUEST\_SHARE\_LINK               | This quest is eligible to be shared as a link                       | No          |
| 20    | CONNECTIONS\_MODAL               | This quest is shown in the user connections modal                   | No          |
| 21    | DISCOVERY\_COMPASS               | This quest is shown on the discovery button                         | No          |
| 22    | TROPHY\_CASE\_CARD               | This quest is shown as a card in the user's trophy case             | No          |
| 23    | VIDEO\_MODAL                     | This quest has a video modal                                        | No          |
| 24    | VIDEO\_MODAL\_END\_CARD          | This quest has an end card in the video modal                       | No          |
| 25    | REWARD\_MODAL                    | This quest is shown in the reward modal                             | No          |
| 26    | EXCLUDED\_QUEST\_EMBED           | This quest is excluded from the Quest embed                         | No          |
| 27    | VIDEO\_MODAL\_MOBILE             | This quest is shown in the mobile video modal                       | No          |

[**Quest Platform Type**](misiones.md#quest-platform-type)

Specifies the platforms that the quest reward can be redeemed on.

| Value | Name            | Description                                    |
| ----- | --------------- | ---------------------------------------------- |
| 0     | CROSS\_PLATFORM | This reward can be redeemed on all platforms   |
| 1     | XBOX            | This reward can be redeemed on Xbox            |
| 2     | PLAYSTATION     | This reward can be redeemed on PlayStation     |
| 3     | SWITCH          | This reward can be redeemed on Nintendo Switch |
| 4     | PC              | This reward can be redeemed on PC              |

[**Quest Feature**](misiones.md#quest-feature)

A behavioral variant for a quest.

| Value | Name                                 | Description                                         |
| ----- | ------------------------------------ | --------------------------------------------------- |
| 1     | POST\_ENROLLMENT\_CTA                | The quest has a post-enrollment call-to-action      |
| 2     | PLAYTIME\_CRITERIA                   | The quest has a playtime criteria                   |
| 3     | QUEST\_BAR\_V2                       | The quest uses the new quest bar design             |
| ~~4~~ | ~~EXCLUDE\_MINORS~~                  | ~~The quest is not shown to minors~~                |
| 5     | EXCLUDE\_RUSSIA                      | The quest is not shown in Russia                    |
| 6     | IN\_HOUSE\_CONSOLE\_QUEST            | The console quest is first-party                    |
| 7     | MOBILE\_CONSOLE\_QUEST               | The console quest is available on mobile            |
| 8     | START\_QUEST\_CTA                    | The quest has a start call-to-action                |
| 9     | REWARD\_HIGHLIGHTING                 | The quest has reward highlighting                   |
| 10    | FRACTIONS\_QUEST                     | The quest offers fractional rewards                 |
| 11    | ADDITIONAL\_REDEMPTION\_INSTRUCTIONS | The quest has additional redemption instructions    |
| 12    | PACING\_V2                           | The quest uses the new pacing system                |
| 13    | DISMISSAL\_SURVEY                    | The quest presents a survey upon dismissal          |
| 14    | MOBILE\_QUEST\_DOCK                  | The quest is shown in the mobile quest dock         |
| 15    | QUESTS\_CDN                          | The quest uses the CDN for assets                   |
| 16    | PACING\_CONTROLLER                   | The quest uses the pacing controller                |
| 17    | QUEST\_HOME\_FORCE\_STATIC\_IMAGE    | The quest displays a static image on the Quest Home |
| 18    | VIDEO\_QUEST\_FORCE\_HLS\_VIDEO      | The video quest forces HLS video playback           |

[**Example Quest**](misiones.md#example-quest)

```
{  "id": "8206816794116096000",  "config": {    "id": "8206816794116096000",    "config_version": 2,    "starts_at": "2025-02-21T18:00:00+00:00",    "expires_at": "2025-02-28T01:00:00+00:00",    "features": [3, 9, 12, 14, 15, 16],    "experiments": {      "rollout": "2025-02_alien",      "targeting": null,      "preview": "2025-02_alien_preview"    },    "application": {      "link": "https://alien.studios/cyberalien",      "id": "891436233903964161",      "name": "Cyberalien 2077"    },    "assets": {      "hero": "hero.jpg",      "hero_video": "hero.mp4",      "quest_bar_hero": "questbar.jpg",      "quest_bar_hero_video": "questbar.mp4",      "game_tile": "gametile.jpg",      "logotype": "wordmark.png"    },    "colors": {      "primary": "#E944D4",      "secondary": "#5318A7"    },    "messages": {      "quest_name": "Kill the Aliens",      "game_title": "Cyberalien 2077",      "game_publisher": "Alien Studios"    },    "task_config": {      "type": 1,      "join_operator": "or",      "tasks": {        "PLAY_ON_DESKTOP": {          "event_name": "PLAY_ON_DESKTOP",          "target": 900,          "external_ids": []        },        "PLAY_ON_XBOX": {          "event_name": "PLAY_ON_XBOX",          "target": 900,          "external_ids": ["267696969"]        },        "PLAY_ON_PLAYSTATION": {          "event_name": "PLAY_ON_PLAYSTATION",          "target": 900,          "external_ids": ["CUSA42069_00"]        }      }    },    "rewards_config": {      "assignment_method": 1,      "rewards": [        {          "type": 1,          "sku_id": "1342624440894361624",          "asset": "CYBERNETIC_HEADGEAR_HELL_YEAHHH.png",          "asset_video": null,          "messages": {            "name": "Cybernetic Headgear",            "name_with_article": "a Cybernetic Headgear",            "redemption_instructions_by_platform": {              "0": "Reward Instructions:\nGo to https://alien.studios/redeem\nEnter your code\nClaim your reward!"            }          },          "approximate_count": null,          "redemption_link": "https://alien.studios/redeem"        }      ],      "rewards_expire_at": "2025-03-28T00:00:00+00:00",      "platforms": [0]    }  },  "user_status": null,  "targeted_content": [],  "preview": false}
```

#### [Claimed Quest Object](misiones.md#claimed-quest-object) <a href="#claimed-quest-object" id="claimed-quest-object"></a>

A claimed quest.

[**Claimed Quest Structure**](misiones.md#claimed-quest-structure)

| Field        | Type                                                                      | Description                                  |
| ------------ | ------------------------------------------------------------------------- | -------------------------------------------- |
| id           | snowflake                                                                 | The ID of the quest                          |
| config       | [claimed quest config](misiones.md#claimed-quest-config-structure) object | The configuration and metadata for the quest |
| user\_status | [quest user status](misiones.md#quest-user-status-object) object          | The user's quest progress                    |

[**Claimed Quest Config Structure**](misiones.md#claimed-quest-config-structure)

| Field       | Type                                                                              | Description                                                           |
| ----------- | --------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| id          | snowflake                                                                         | The ID of the quest                                                   |
| starts\_at  | ISO8601 timestamp                                                                 | When the quest period starts                                          |
| expires\_at | ISO8601 timestamp                                                                 | When the quest period ends                                            |
| features    | array\[integer]                                                                   | The [quest features](misiones.md#quest-feature) enabled for the quest |
| colors      | [quest gradient](misiones.md#quest-gradient-structure) object                     | The accent colors for the quest                                       |
| assets      | [quest assets](misiones.md#quest-assets-structure) object                         | Object that holds the quest's assets                                  |
| messages    | [quest messages](misiones.md#quest-messages-structure) object                     | Human-readable metadata for the quest                                 |
| rewards     | array\[[claimed quest reward](misiones.md#claimed-quest-reward-structure) object] | The claimed rewards for the quest                                     |

[**Claimed Quest Reward Structure**](misiones.md#claimed-quest-reward-structure)

| Field                 | Type               | Description                                                                    |
| --------------------- | ------------------ | ------------------------------------------------------------------------------ |
| type                  | integer            | The [reward's type](misiones.md#quest-reward-type)                             |
| sku\_id               | snowflake          | The ID of the SKU awarded                                                      |
| name                  | string             | The reward's name                                                              |
| name\_with\_article   | string             | The article variant of the name (e.g. a Cybernetic Headgear Decoration)        |
| asset                 | string             | The reward's [media asset](https://docs.discord.food/reference#cdn-formatting) |
| asset\_video          | ?string            | The reward's [video asset](https://docs.discord.food/reference#cdn-formatting) |
| orb\_quantity         | ?integer           | The amount of Discord Orbs awarded                                             |
| collectible\_product? | collectible object | The collectible product awarded                                                |

[**Example Claimed Quest**](misiones.md#example-claimed-quest)

```
{  "id": "8206816794116096000",  "config": {    "id": "8206816794116096000",    "starts_at": "2025-02-21T18:00:00+00:00",    "expires_at": "2025-02-28T01:00:00+00:00",    "features": [3, 9, 12, 14, 15, 16],    "colors": {      "primary": "#E944D4",      "secondary": "#5318A7"    },    "assets": {      "hero": "hero.jpg",      "hero_video": "hero.mp4",      "quest_bar_hero": "questbar.jpg",      "quest_bar_hero_video": "questbar.mp4",      "game_tile": "gametile.jpg",      "logotype": "wordmark.png"    },    "messages": {      "quest_name": "Kill the Aliens",      "game_title": "Cyberalien 2077",      "game_publisher": "Alien Studios"    },    "rewards": [      {        "sku_id": "1342624440894361624",        "type": 1,        "name": "Cybernetic Headgear",        "name_with_article": "a Cybernetic Headgear",        "asset": "CYBERNETIC_HEADGEAR_HELL_YEAHHH.png",        "asset_video": null,        "orb_quantity": null      }    ]  },  "user_status": null}
```

#### [Quest User Status Object](misiones.md#quest-user-status-object) <a href="#quest-user-status-object" id="quest-user-status-object"></a>

The user's quest progression.

[**Quest User Status Structure**](misiones.md#quest-user-status-structure)

| Field                                     | Type                                                                                  | Description                                                                                                    |
| ----------------------------------------- | ------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| user\_id                                  | snowflake                                                                             | The ID of the user                                                                                             |
| quest\_id?                                | snowflake                                                                             | The ID of the quest                                                                                            |
| enrolled\_at                              | ?ISO8601 timestamp                                                                    | When the user accepted the quest                                                                               |
| completed\_at                             | ?ISO8601 timestamp                                                                    | When the user completed the quest                                                                              |
| claimed\_at                               | ?ISO8601 timestamp                                                                    | When the user claimed the quest's reward                                                                       |
| claimed\_tier?                            | ?integer                                                                              | Which reward tier the user has claimed, if the quest'sis                                                       |
| last\_stream\_heartbeat\_at? <sup>1</sup> | ?ISO8601 timestamp                                                                    | When the last heartbeat was received                                                                           |
| stream\_progress\_seconds? <sup>1</sup>   | ISO8601 timestamp                                                                     | Duration (in seconds) the user has streamed the game for since the quest was accepted                          |
| dismissed\_quest\_content?                | integer                                                                               | The [content areas the user has dismissed](misiones.md#dismissible-quest-content-flags) for the quest          |
| progress                                  | map\[string, [quest task progress](misiones.md#quest-task-progress-structure) object] | The user's progress for each task in the quest, keyed by their [event name](misiones.md#quest-task-event-name) |

<sup>1</sup> These fields are only used for quest config version 1, where the event is always .

[**Quest Task Progress Structure**](misiones.md#quest-task-progress-structure)

| Field                   | Type                                                                       | Description                                                 |
| ----------------------- | -------------------------------------------------------------------------- | ----------------------------------------------------------- |
| event\_name             | string                                                                     | The [type of task event](misiones.md#quest-task-event-name) |
| value <sup>1</sup>      | integer                                                                    | The current task value                                      |
| updated\_at             | ISO8601 timestamp                                                          | When the task was last updated                              |
| completed\_at           | ?ISO8601 timestamp                                                         | When the task was completed                                 |
| heartbeat? <sup>2</sup> | ?[quest task heartbeat](misiones.md#quest-task-heartbeat-structure) object | The task's heartbeat data                                   |

<sup>1</sup> While this is an opaque value, for duration-based tasks, this will be a duration in seconds. To complete the task, this value must match the value.

<sup>2</sup> Heartbeats are only present for events , , , and .

[**Quest Task Heartbeat Structure**](misiones.md#quest-task-heartbeat-structure)

| Field          | Type               | Description                          |
| -------------- | ------------------ | ------------------------------------ |
| last\_beat\_at | ISO8601 timestamp  | When the last heartbeat was received |
| expires\_at    | ?ISO8601 timestamp | When the task progress expires       |

[**Dismissible Quest Content Flags**](misiones.md#dismissible-quest-content-flags)

Dismissed [quest content areas](misiones.md#quest-content-type).

| Value  | Name                             | Description                                           |
| ------ | -------------------------------- | ----------------------------------------------------- |
| 1 << 0 | GIFT\_INVENTORY\_SETTINGS\_BADGE | User has dismissed the quest from User Settings       |
| 1 << 1 | QUEST\_BAR <sup>1</sup>          | User has dismissed the quest from the Quest Bar       |
| 1 << 2 | ACTIVITY\_PANEL                  | User has dismissed the quest from the Active Now page |
| 1 << 3 | QUEST\_LIVE\_STREAM              | User has dismissed the quest from the stream overlay  |

<sup>1</sup> This flag dismisses any content area, including , , and .

[**Example Quest User Status**](misiones.md#example-quest-user-status)

```
{  "user_id": "222069018507345921",  "quest_id": "8206816794116096000",  "enrolled_at": "2077-01-01T11:59:59+00:00",  "completed_at": "2077-01-01T11:59:59+00:00",  "claimed_at": "2077-01-01T11:59:59+00:00",  "claimed_tier": null,  "last_stream_heartbeat_at": null,  "stream_progress_seconds": 0,  "dismissed_quest_content": 0,  "progress": {    "PLAY_ON_DESKTOP": {      "value": 900,      "event_name": "PLAY_ON_DESKTOP",      "updated_at": "2025-03-11T18:19:54.189229+00:00",      "completed_at": "2025-03-11T18:19:54.189231+00:00",      "heartbeat": {        "last_beat_at": "2077-01-01T11:59:59+00:000",        "expires_at": null      }    }  }}
```

#### [Quest Reward Code Object](misiones.md#quest-reward-code-object) <a href="#quest-reward-code-object" id="quest-reward-code-object"></a>

An object that holds the quest's reward code.

[**Quest Reward Code Structure**](misiones.md#quest-reward-code-structure)

| Field       | Type              | Description                                                                 |
| ----------- | ----------------- | --------------------------------------------------------------------------- |
| quest\_id   | snowflake         | The ID of the quest                                                         |
| code        | string            | The redeem code                                                             |
| platform    | string            | The [platform this redeem code applies to](misiones.md#quest-platform-type) |
| user\_id    | snowflake         | The ID of the user who this code belongs to                                 |
| claimed\_at | ISO8601 timestamp | When the user claimed the quest's reward                                    |
| tier        | ?integer          | Which reward tier the code belongs to, if the quest'sis set to              |

[**Example Quest Reward Code**](misiones.md#example-quest-reward-code)

```
{  "quest_id": "8206816794116096000",  "code": "111-1111111",  "platform": 0,  "user_id": "222069018507345921",  "claimed_at": "2077-01-01T18:41:29.706194+00:00",  "tier": null}
```

### [Endpoints](misiones.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get Current Quests](misiones.md#get-current-quests) <a href="#get-current-quests" id="get-current-quests"></a>

`GET/quests/@me`

Returns information on the current [quests](misiones.md#quest-object) for the current user.

[**Response Body**](misiones.md#response-body)

| Field                             | Type                                                     | Description                                    |
| --------------------------------- | -------------------------------------------------------- | ---------------------------------------------- |
| quests                            | array\[[quest](misiones.md#quest-object) object]         | The current quests for the user                |
| excluded\_quests                  | array\[partial [quest](misiones.md#quest-object) object] | The quests that the user cannot participate in |
| quest\_enrollment\_blocked\_until | ?ISO8601 timestamp                                       | When the user can enroll in quests again       |

#### [Get Claimed Quests](misiones.md#get-claimed-quests) <a href="#get-claimed-quests" id="get-claimed-quests"></a>

`GET/quests/@me/claimed`

Returns information on the [claimed quests](misiones.md#claimed-quest-object) for the current user.

[**Response Body**](misiones.md#response-body)

| Field  | Type                                                             | Description                     |
| ------ | ---------------------------------------------------------------- | ------------------------------- |
| quests | array\[[claimed quest](misiones.md#claimed-quest-object) object] | The claimed quests for the user |

#### [Get Quest Config](misiones.md#get-quest-config) <a href="#get-quest-config" id="get-quest-config"></a>

`GET/quests/{quest.id}`

Returns a [quest config](misiones.md#quest-config-object) object for the specified quest. Quest must be currently active.

#### [Get Quest Placement](misiones.md#get-quest-placement) <a href="#get-quest-placement" id="get-quest-placement"></a>

`GET/quests/decision`

Returns the sponsored quest that should be shown to the user in a specific placement.

[**Query Params**](misiones.md#query-params)

| Field                                        | Type    | Description                                                                       |
| -------------------------------------------- | ------- | --------------------------------------------------------------------------------- |
| placement                                    | integer | The [quest placement area](misiones.md#quest-placement-area) to get the quest for |
| client\_heartbeat\_session\_id? <sup>1</sup> | string  | A client-generated UUID representing the current persisted analytics heartbeat    |

<sup>1</sup> This value is also sent in the [client properties](https://docs.discord.food/reference#client-properties).

[**Quest Placement Area**](misiones.md#quest-placement-area)

| Value | Name                          | Description              |
| ----- | ----------------------------- | ------------------------ |
| 1     | DESKTOP\_ACCOUNT\_PANEL\_AREA | Account panel on desktop |
| 2     | MOBILE\_HOME\_DOCK\_AREA      | Home dock on mobile      |

[**Response Body**](misiones.md#response-body)

| Field           | Type                                                                       | Description                                           |
| --------------- | -------------------------------------------------------------------------- | ----------------------------------------------------- |
| request\_id     | string                                                                     | The advertisement decision ID                         |
| quest           | ?[quest](misiones.md#quest-object) object                                  | The quest to show to the user                         |
| ad\_identifiers | ?[quest ad identifiers](misiones.md#quest-ad-identifiers-structure) object | The advertisement identifiers for the delivered quest |

| Field          | Type      | Description                          |
| -------------- | --------- | ------------------------------------ |
| campaign\_id   | snowflake | The ID of the advertisement campaign |
| adset\_id      | snowflake | The ID of the advertisement set      |
| ad\_id         | snowflake | The ID of the advertisement          |
| creative\_id   | snowflake | The ID of the advertisement creative |
| creative\_type | integer   | The type of advertisement creative   |
| quest\_id      | snowflake | The ID of the delivered quest        |

#### [Accept Quest](misiones.md#accept-quest) <a href="#accept-quest" id="accept-quest"></a>

`POST/quests/{quest.id}/enroll`

Accepts a quest and returns a [quest user status](misiones.md#quest-user-status-object) object. Fires a [Quests User Status Update](https://docs.discord.food/topics/gateway-events#quests-user-status-update) Gateway event.

[**JSON Params**](misiones.md#json-params)

| Field    | Type    | Description                                                                           |
| -------- | ------- | ------------------------------------------------------------------------------------- |
| location | integer | The [content location](misiones.md#quest-content-type) where the action was initiated |

#### [Claim Quest Reward](misiones.md#claim-quest-reward) <a href="#claim-quest-reward" id="claim-quest-reward"></a>

`POST/quests/{quest.id}/claim-reward`

Claims the quest's rewards, setting the and fields of the [quest user status](misiones.md#quest-user-status-object) to the current timestamp. Fires a [Quests User Status Update](https://docs.discord.food/topics/gateway-events#quests-user-status-update) Gateway event.

[**JSON Params**](misiones.md#json-params)

| Field    | Type    | Description                                                                           |
| -------- | ------- | ------------------------------------------------------------------------------------- |
| location | integer | The [content location](misiones.md#quest-content-type) where the action was initiated |
| platform | string  | The [platform](misiones.md#quest-platform-type) to claim the reward for               |

[**Response Body**](misiones.md#response-body)

| Field                             | Type                                                                                                             | Description                                       |
| --------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| claimed\_at                       | ?ISO8601 timestamp                                                                                               | When the rewards were claimed                     |
| entitlement\_expiration\_metadata | map\[snowflake, [entitlement expiration metadata](misiones.md#entitlement-expiration-metadata-structure) object] | The expiration metadata for each entitlement      |
| entitlements                      | array\[[entitlement](https://docs.discord.food/resources/entitlement#entitlement-object) object]                 | The entitlements the user received                |
| errors                            | array\[[JSON error](https://docs.discord.food/topics/opcodes-and-status-codes#json) object]                      | The errors that occured while claiming the reward |

[**Entitlement Expiration Metadata Structure**](misiones.md#entitlement-expiration-metadata-structure)

| Field      | Type    | Description                                                                        |
| ---------- | ------- | ---------------------------------------------------------------------------------- |
| extended   | boolean | Whether the entitlement expiration has been extended due to a premium subscription |
| extendable | boolean | Whether the entitlement expiration can be extended due to a premium subscription   |

[**Example Response**](misiones.md#example-response)

```
{  "claimed_at": "2024-04-17T23:30:41.000321+00:00",  "entitlement_expiration_metadata": {    "1230299425620885624": {      "extended": false,      "extendable": true    }  },  "entitlements": [    {      "id": "1230299425620885624",      "sku_id": "1226939756617793606",      "application_id": "1242265603276800000",      "user_id": "222069018507345921",      "deleted": false,      "starts_at": null,      "ends_at": null,      "type": 10,      "tenant_metadata": {},      "gift_code_flags": 0,      "promotion_id": null    }  ],  "errors": []}
```

#### [Get Quest Reward Code](misiones.md#get-quest-reward-code) <a href="#get-quest-reward-code" id="get-quest-reward-code"></a>

`GET/quests/{quest.id}/reward-code`

Retrieves the reward code for the specified platform. Returns a [quest reward code](misiones.md#quest-reward-code-object) object on success.

#### [Send Quest Heartbeat](misiones.md#send-quest-heartbeat) <a href="#send-quest-heartbeat" id="send-quest-heartbeat"></a>

`POST/quests/{quest.id}/heartbeat`

Tells the server to update the and fields of the current task. Used for keeping track of how long the stream has been running for, and for checking if the user has met the [task duration requirement](misiones.md#quest-task-structure). Returns a [quest user status](misiones.md#quest-user-status-object) object on success. Fires a [Quests User Status Update](https://docs.discord.food/topics/gateway-events#quests-user-status-update) Gateway event.

[**JSON Params**](misiones.md#json-params)

| Field                    | Type    | Description                                                                                                                                                         |
| ------------------------ | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| stream\_key <sup>1</sup> | string  | The [encoded key of the stream](https://docs.discord.food/topics/gateway-events#stream-key) (e.g `guild:169256939211980800:1050497861969793164:222069018507345921`) |
| terminal?                | boolean | Whether this is the last heartbeat in the sequence                                                                                                                  |

<sup>1</sup> For heartbeats without an associated stream, a special stream key of should be used, where is the ID of the quest.

#### [Send Quest Video Progress](misiones.md#send-quest-video-progress) <a href="#send-quest-video-progress" id="send-quest-video-progress"></a>

`POST/quests/{quest.id}/video-progress`

Tells the server to update the field of the current video task. Used for keeping track of how long the video has been watched for, and for checking if the user has met the [task duration requirement](misiones.md#quest-task-structure). Returns a [quest user status](misiones.md#quest-user-status-object) object on success. Fires a [Quests User Status Update](https://docs.discord.food/topics/gateway-events#quests-user-status-update) Gateway event.

[**JSON Params**](misiones.md#json-params)

| Field     | Type    | Description                                     |
| --------- | ------- | ----------------------------------------------- |
| timestamp | integer | How far into the video the user is (in seconds) |

#### [Start Console Quest](misiones.md#start-console-quest) <a href="#start-console-quest" id="start-console-quest"></a>

`POST/quests/{quest.id}/console/start`

Starts completing a quest on console. Fires a [Quests User Status Update](https://docs.discord.food/topics/gateway-events#quests-user-status-update) Gateway event.

[**Query Params**](misiones.md#query-params)

| Field    | Type    | Description                                          |
| -------- | ------- | ---------------------------------------------------- |
| preview? | boolean | Whether the quest is in preview mode (default false) |

[**Response Body**](misiones.md#response-body)

| Field               | Type                                                                       | Description                                       |
| ------------------- | -------------------------------------------------------------------------- | ------------------------------------------------- |
| started             | boolean                                                                    | Whether the quest was successfully started        |
| quest\_user\_status | ?[quest user status](misiones.md#quest-user-status-object) object          | The user's quest progress                         |
| error\_hints        | ?array\[string]                                                            | The errors that occurred while starting the quest |
| error\_hints\_v2    | ?array\[[quest error hint](misiones.md#quest-error-hint-structure) object] | The errors that occurred while starting the quest |

[**Quest Error Hint Structure**](misiones.md#quest-error-hint-structure)

| Field                    | Type      | Description                                                                                                                           |
| ------------------------ | --------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| type                     | string    | The type of error                                                                                                                     |
| message                  | string    | The error message                                                                                                                     |
| connected\_account\_id   | snowflake | The ID of the [connection](https://docs.discord.food/resources/connected-accounts#connection-object) the console account is linked to |
| connected\_account\_type | string    | The type of [connection](https://docs.discord.food/resources/connected-accounts#connection-object) the console account is linked to   |

[**Example Response**](misiones.md#example-response)

```
{  "started": false,  "quest_user_status": null,  "error_hints": ["Xbox account DiscordGamer seems to be offline."],  "error_hints_v2": [    {      "type": "no_game_offline",      "message": "Xbox account DiscordGamer seems to be offline.",      "connected_account_id": "3076467402341699",      "connected_account_type": "xbox"    }  ]}
```

#### [Stop Console Quest](misiones.md#stop-console-quest) <a href="#stop-console-quest" id="stop-console-quest"></a>

`POST/quests/{quest.id}/console/stop`

Stops completing a quest on console. Returns a 204 empty response on success. Fires a [Quests User Status Update](https://docs.discord.food/topics/gateway-events#quests-user-status-update) Gateway event.

#### [Complete Quest](misiones.md#complete-quest) <a href="#complete-quest" id="complete-quest"></a>

`POST/quests/{quest.id}/preview/complete`

Forcefully completes the quest for the current user. Returns a [quest user status](misiones.md#quest-user-status-object) object. Fires a [Quests User Status Update](https://docs.discord.food/topics/gateway-events#quests-user-status-update) Gateway event.

#### [Reset Quest](misiones.md#reset-quest) <a href="#reset-quest" id="reset-quest"></a>

`DELETE/quests/{quest.id}/preview/status`

Resets the quest's status for the current user. Returns a [quest user status](misiones.md#quest-user-status-object) object. Fires a [Quests User Status Update](https://docs.discord.food/topics/gateway-events#quests-user-status-update) Gateway event.

#### [Dismiss Quest Content](misiones.md#dismiss-quest-content) <a href="#dismiss-quest-content" id="dismiss-quest-content"></a>

`POST/quests/{quest.id}/dismissible-content/{quest_content_type}/dismiss`

Dismisses the specified [quest content area](misiones.md#quest-content-type) for the current user. Not all content areas can be dismissed. Returns a [quest user status](misiones.md#quest-user-status-object) object. Fires a [Quests User Status Update](https://docs.discord.food/topics/gateway-events#quests-user-status-update) Gateway event.

#### [Reset Quest Dismissibility](misiones.md#reset-quest-dismissibility) <a href="#reset-quest-dismissibility" id="reset-quest-dismissibility"></a>

`DELETE/quests/{quest.id}/preview/dismissibility`

Resets the dismissibility of the quest's content areas for the current user (sets the [field](misiones.md#quest-user-status-object) to 0). Returns a [quest user status](misiones.md#quest-user-status-object) object. Fires a [Quests User Status Update](https://docs.discord.food/topics/gateway-events#quests-user-status-update) Gateway event.

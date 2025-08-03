---
icon: eye
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

# Descubrimiento

### [Discovery](descubrimiento.md#discovery) <a href="#discovery" id="discovery"></a>

Discovery is a feature that allows users to find and join new communities on Discord, open to all guilds that meet the requirements. Discovery happens within the client or the [marketing page](https://discord.com/servers).

### [Definitions](descubrimiento.md#definitions) <a href="#definitions" id="definitions"></a>

A guild is considered discoverable by the API if it has the [guild feature](https://docs.discord.food/resources/guild#guild-features). Additionally, any guilds within a directory channel the user has access to (guilds that have the [guild feature](https://docs.discord.food/resources/guild#guild-features)), like in a student hub, are also considered discoverable relative to the user.

### [Searching Discovery](descubrimiento.md#searching-discovery) <a href="#searching-discovery" id="searching-discovery"></a>

Discord utilizes [Algolia](https://www.algolia.com/) to power search for discovery. You can search for guilds by name, description, keywords, and more. Reference the [Algolia Search documentation](https://www.algolia.com/doc/rest-api/search/) for more information on how to search. A [proxied version of the Algolia Search API](descubrimiento.md#search-discoverable-guilds) is available, with limited parameter support.

[**Algolia Credentials**](descubrimiento.md#algolia-credentials)

These credentials are for production only.

```
Application ID: NKTZZ4AIZUAPI Key: <...>
```

#### [Discoverable Guild Object](descubrimiento.md#discoverable-guild-object) <a href="#discoverable-guild-object" id="discoverable-guild-object"></a>

A partial guild object returned from discovery, [Get Emoji Guild](https://docs.discord.food/resources/emoji#get-emoji-guild), [Get Sticker Guild](https://docs.discord.food/resources/sticker#get-sticker-guild), and [Get Soundboard Sound Guild](https://docs.discord.food/resources/soundboard#get-soundboard-sound-guild).

[**Discoverable Guild Structure**](descubrimiento.md#discoverable-guild-structure)

| Field                           | Type                                                                                 | Description                                                                                                 |
| ------------------------------- | ------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------- |
| id                              | snowflake                                                                            | The ID of the guild                                                                                         |
| name                            | string                                                                               | The name of the guild (2-100 characters)                                                                    |
| icon                            | ?string                                                                              | The guild's [icon hash](https://docs.discord.food/reference#cdn-formatting)                                 |
| description                     | ?string                                                                              | The description for the guild (max 300 characters)                                                          |
| banner                          | ?string                                                                              | The guild's [banner hash](https://docs.discord.food/reference#cdn-formatting)                               |
| splash                          | ?string                                                                              | The guild's [splash hash](https://docs.discord.food/reference#cdn-formatting)                               |
| discovery\_splash               | ?string                                                                              | The guild's [discovery splash hash](https://docs.discord.food/reference#cdn-formatting)                     |
| features                        | array\[string]                                                                       | Enabled [guild features](https://docs.discord.food/resources/guild#guild-features)                          |
| vanity\_url\_code               | ?string                                                                              | The guild's vanity invite code                                                                              |
| preferred\_locale               | string                                                                               | The preferred locale of the guild; used in discovery and notices from Discord (default "en-US")             |
| premium\_subscription\_count    | integer                                                                              | The number of premium subscriptions (boosts) the guild currently has                                        |
| approximate\_member\_count      | integer                                                                              | Approximate number of members in the guild                                                                  |
| approximate\_presence\_count    | integer                                                                              | Approximate number of online members in the guild                                                           |
| emojis? <sup>1</sup>            | array\[[emoji](https://docs.discord.food/resources/emoji#emoji-object) object]       | Custom guild emojis; limited to 30 entries                                                                  |
| emoji\_count? <sup>1</sup>      | integer                                                                              | Total number of custom guild emojis                                                                         |
| stickers? <sup>1</sup>          | array\[[sticker](https://docs.discord.food/resources/sticker#sticker-object) object] | Custom guild stickers; limited to 30 entries                                                                |
| sticker\_count? <sup>1</sup>    | integer                                                                              | Total number of custom guild stickers                                                                       |
| auto\_removed                   | boolean                                                                              | Whether the guild has automatically been removed from discovery for not hitting required targets            |
| primary\_category\_id           | integer                                                                              | The ID of the primary [discovery category](descubrimiento.md#discovery-category-object) set for the guild   |
| primary\_category? <sup>3</sup> | [discovery category](descubrimiento.md#discovery-category-object)                    | The primary [discovery category](descubrimiento.md#discovery-category-object) set for the guild             |
| keywords                        | ?array\[string]                                                                      | The discovery search keywords for the guild (max 30 characters, max 10)                                     |
| is\_published                   | boolean                                                                              | Whether the guild's landing web page is currently published                                                 |
| reasons\_to\_join? <sup>2</sup> | array\[[discovery reason object](descubrimiento.md#discovery-reason-structure)]      | The reasons to join the guild shown in the discovery web page (max 4)                                       |
| social\_links? <sup>2</sup>     | ?array\[string]                                                                      | The guild's social media links shown in the discovery web page (max 256 characters, max 9)                  |
| about? <sup>2</sup>             | ?string                                                                              | The guild's long description shown in the discovery web page (max 2400 characters)                          |
| category\_ids? <sup>2</sup>     | array\[snowflake]                                                                    | The IDs of [discovery subcategories](descubrimiento.md#discovery-category-object) set for the guild (max 5) |
| categories? <sup>3</sup>        | array\[[discovery category](descubrimiento.md#discovery-category-object)]            | The [discovery categories](descubrimiento.md#discovery-category-object) set for the guild (max 5)           |
| created\_at? <sup>2</sup>       | ISO8601 timestamp                                                                    | When the guild was created                                                                                  |

<sup>1</sup> The presence of these fields is dependent on the endpoint used to retrieve the guild. [Get Emoji Guild](https://docs.discord.food/resources/emoji#get-emoji-guild) will return emoji-related fields, and [Get Sticker Guild](https://docs.discord.food/resources/sticker#get-guild-sticker) will return sticker-related fields.

<sup>2</sup> Only included when fetched from the [Get Discovery Slug](descubrimiento.md#get-discovery-slug) endpoint.

<sup>3</sup> Only included when [searching discovery](descubrimiento.md#searching-discovery).

[**Example Discoverable Guild**](descubrimiento.md#example-discoverable-guild)

```
{  "id": "752630786561409076",  "name": "Elite Creative",  "description": "The largest Fortnite Creative server across the globe. Join a Creative community offering events, 1v1s. and more!",  "icon": "278da1c7740e394657c1179f4782aef1",  "splash": "2b4ae5cdd71038b4880b1b57a6e5dacb",  "banner": "57e939e67aebaf232d28205603020b55",  "approximate_presence_count": 21125,  "approximate_member_count": 155455,  "premium_subscription_count": 17,  "preferred_locale": "en-US",  "auto_removed": false,  "discovery_splash": "9d7ec672b89b320ef7a51e5b6ae453b8",  "primary_category_id": 1,  "vanity_url_code": "creative",  "is_published": true,  "keywords": [    "Fortnite",    "Creative",    "Fortnite Creative",    "Boxfights",    "Zonewars",    "Buildfights",    "1v1",    "EU",    "NA",    "Creator"  ],  "features": [    "ANIMATED_BANNER",    "ANIMATED_ICON",    "AUTO_MODERATION",    "BANNER",    "COMMUNITY",    "DISCOVERABLE",    "ENABLED_DISCOVERABLE_BEFORE",    "GUILD_ONBOARDING_EVER_ENABLED",    "GUILD_WEB_PAGE_VANITY_URL",    "INVITE_SPLASH",    "NEWS",    "PREVIEW_ENABLED",    "RAID_ALERTS_ENABLED",    "ROLE_ICONS",    "VANITY_URL",    "WELCOME_SCREEN_ENABLED"  ],  "emojis": [],  "emoji_count": 339}
```

#### [Discovery Requirements Object](descubrimiento.md#discovery-requirements-object) <a href="#discovery-requirements-object" id="discovery-requirements-object"></a>

A guild's progress on meeting the requirements of joining discovery.

[**Discovery Requirements Structure**](descubrimiento.md#discovery-requirements-structure)

| Field                                           | Type                                                                                      | Description                                                                                                                     |
| ----------------------------------------------- | ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| guild\_id?                                      | snowflake                                                                                 | The ID of the guild                                                                                                             |
| safe\_environment?                              | boolean                                                                                   | Whether the guild has not been flagged by Trust & Safety                                                                        |
| healthy?                                        | boolean                                                                                   | Whether the guild meets activity requirements                                                                                   |
| health\_score\_pending?                         | boolean                                                                                   | Whether the guild's activity metrics have not yet been calculated                                                               |
| size?                                           | boolean                                                                                   | Whether the guild meets the minimum member count requirement                                                                    |
| nsfw\_properties?                               | [discovery NSFW properties](descubrimiento.md#discovery-nsfw-properties-structure) object | Disallowed terms found in the guild's name, description, and channel names                                                      |
| protected?                                      | boolean                                                                                   | Whether the guild has the [MFA requirement for moderation actions](https://docs.discord.food/resources/guild#mfa-level) enabled |
| sufficient <sup>1</sup>                         | boolean                                                                                   | Whether the guild meets the requirements to be in Discovery                                                                     |
| sufficient\_without\_grace\_period <sup>1</sup> | boolean                                                                                   | Whether the grace period can allow the guild to remain in Discovery                                                             |
| valid\_rules\_channel?                          | boolean                                                                                   | Whether the guild has a rules channel set                                                                                       |
| retention\_healthy?                             | boolean                                                                                   | Whether the guild meets the new member retention requirement                                                                    |
| engagement\_healthy?                            | boolean                                                                                   | Whether the guild meets the weekly visitor and communicator requirements                                                        |
| age?                                            | boolean                                                                                   | Whether the guild meets the minimum age requirement                                                                             |
| minimum\_age?                                   | ?integer                                                                                  | The minimum guild age requirement (in days)                                                                                     |
| health\_score?                                  | [discovery health score](descubrimiento.md#discovery-health-score-structure) object       | The guild's activity metrics                                                                                                    |
| minimum\_size?                                  | ?integer                                                                                  | The minimum guild member count requirement                                                                                      |
| grace\_period\_end\_date?                       | ISO8601 timestamp                                                                         | When the guild's grace period ends                                                                                              |

<sup>1</sup> Certain guilds, such as those that are [verified](https://discord.com/verification), are exempt from discovery requirements. These guilds will not have a fully populated discovery requirements object, and are guaranteed to receive only and .

[**Discovery NSFW Properties Structure**](descubrimiento.md#discovery-nsfw-properties-structure)

| Field                          | Type                            | Description                                                    |
| ------------------------------ | ------------------------------- | -------------------------------------------------------------- |
| channels?                      | array\[snowflake]               | The IDs of the channels with names containing disallowed terms |
| channel\_banned\_keywords?     | map\[snowflake, array\[string]] | The disallowed terms found in the given channel names          |
| name?                          | string                          | The guild name, if it contains disallowed terms                |
| name\_banned\_keywords?        | array\[string]                  | The disallowed terms found in the guild name                   |
| description?                   | string                          | The guild description, if it contains disallowed terms         |
| description\_banned\_keywords? | array\[string]                  | The disallowed terms found in the guild description            |

[**Discovery Health Score Structure**](descubrimiento.md#discovery-health-score-structure)

| Field                      | Type    | Description                                                                                |
| -------------------------- | ------- | ------------------------------------------------------------------------------------------ |
| avg\_nonnew\_communicators | ?string | Average weekly number of users who talk in the guild and have been on Discord for 8 weeks+ |
| avg\_nonnew\_participators | ?string | Average weekly number of users who view the guild and have been on Discord for 8 weeks+    |
| num\_intentful\_joiners    | ?string | Average number of users who join the guild per week                                        |
| perc\_ret\_w1\_intentful   | ?float  | Percentage of new members who remain in the guild for at least a week                      |

[**Example Discovery Requirements**](descubrimiento.md#example-discovery-requirements)

```
{  "guild_id": "1046920999469330512",  "safe_environment": true,  "healthy": true,  "health_score_pending": false,  "size": true,  "nsfw_properties": {    "channels": ["1060703057651978261"],    "channels_banned_keywords": {      "1060703057651978261": ["risque"]    },    "name": "Alien Network",    "name_banned_keywords": ["alien"],    "description": "Where the 👽s 👽 and sometimes very 👽 things happen 😨.",    "description_banned_keywords": ["👽"]  },  "protected": true,  "sufficient": false,  "sufficient_without_grace_period": true,  "valid_rules_channel": true,  "retention_healthy": true,  "engagement_healthy": true,  "age": true,  "minimum_age": 56,  "health_score": {    "avg_nonnew_participators": "1738",    "avg_nonnew_communicators": "348",    "num_intentful_joiners": "834",    "perc_ret_w1_intentful": 0.37651924871356596  },  "minimum_size": 1000,  "grace_period_end_date": "2037-07-01T17:47:49.974000+00:00"}
```

#### [Discovery Metadata Object](descubrimiento.md#discovery-metadata-object) <a href="#discovery-metadata-object" id="discovery-metadata-object"></a>

A guild's discovery settings.

[**Discovery Metadata Structure**](descubrimiento.md#discovery-metadata-structure)

| Field                           | Type                                                                            | Description                                                                                                 |
| ------------------------------- | ------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| guild\_id                       | snowflake                                                                       | The ID of the guild                                                                                         |
| primary\_category\_id           | integer                                                                         | The ID of the primary [discovery category](descubrimiento.md#discovery-category-object) set for the guild   |
| keywords                        | ?array\[string]                                                                 | The discovery search keywords for the guild (max 30 characters, max 10)                                     |
| emoji\_discoverability\_enabled | boolean                                                                         | Whether the guild is shown as a source through custom guild expressions                                     |
| partner\_actioned\_timestamp    | ?ISO8601 timestamp                                                              | When the guild's partner application was actioned by an employee                                            |
| partner\_application\_timestamp | ?ISO8601 timestamp                                                              | When the guild applied for partnership                                                                      |
| is\_published                   | boolean                                                                         | Whether the guild's landing web page is currently published                                                 |
| reasons\_to\_join               | array\[[discovery reason object](descubrimiento.md#discovery-reason-structure)] | The reasons to join the guild shown in the discovery web page (max 4)                                       |
| social\_links                   | ?array\[string]                                                                 | The guild's social media links shown in the discovery web page (max 256 characters, max 9)                  |
| about                           | ?string                                                                         | The guild's long description shown in the discovery web page (max 2400 characters)                          |
| category\_ids                   | array\[snowflake]                                                               | The IDs of [discovery subcategories](descubrimiento.md#discovery-category-object) set for the guild (max 5) |

[**Discovery Reason Structure**](descubrimiento.md#discovery-reason-structure)

| Field       | Type       | Description                                                                                |
| ----------- | ---------- | ------------------------------------------------------------------------------------------ |
| reason      | string     | The reason to join the guild                                                               |
| emoji\_id   | ?snowflake | The [ID of a guild's custom emoji](https://docs.discord.food/resources/emoji#emoji-object) |
| emoji\_name | ?string    | The unicode character of the emoji                                                         |

[**Example Discovery Metadata**](descubrimiento.md#example-discovery-metadata)

```
{  "guild_id": "1046920999469330512",  "primary_category_id": 49,  "keywords": ["test"],  "emoji_discoverability_enabled": true,  "partner_actioned_timestamp": null,  "partner_application_timestamp": null,  "is_published": false,  "reasons_to_join": [    { "reason": "Alien", "emoji_id": null, "emoji_name": "👽" },    { "reason": "Alien", "emoji_id": null, "emoji_name": "👽" },    { "reason": "Alien", "emoji_id": null, "emoji_name": "👽" },    { "reason": "Alien", "emoji_id": null, "emoji_name": "👽" }  ],  "social_links": ["https://twitter.com/alien"],  "about": "Alien\nAlien\nAlien\nAlien\nAlien\nAlien",  "category_ids": [48]}
```

#### [Discovery Category Object](descubrimiento.md#discovery-category-object) <a href="#discovery-category-object" id="discovery-category-object"></a>

[**Discovery Category Structure**](descubrimiento.md#discovery-category-structure)

| Field       | Type    | Description                                                    |
| ----------- | ------- | -------------------------------------------------------------- |
| id          | integer | The ID of the category                                         |
| name        | string  | The name of the category                                       |
| is\_primary | boolean | Whether the category can be used as a guild's primary category |

[**Example Discovery Category**](descubrimiento.md#example-discovery-category)

```
{  "id": 1,  "name": "Gaming",  "is_primary": true}
```

#### [Guild Profile Object](descubrimiento.md#guild-profile-object) <a href="#guild-profile-object" id="guild-profile-object"></a>

[**Guild Profile Structure**](descubrimiento.md#guild-profile-structure)

| Field                         | Type                                                                               | Description                                                                                      |
| ----------------------------- | ---------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| id                            | snowflake                                                                          | The ID of the guild                                                                              |
| name                          | string                                                                             | The name of the guild (2-100 characters)                                                         |
| icon\_hash                    | ?string                                                                            | The guild's [icon hash](https://docs.discord.food/reference#cdn-formatting)                      |
| member\_count                 | integer                                                                            | Approximate count of total members in the guild                                                  |
| online\_count                 | integer                                                                            | Approximate count of non-offline members in the guild                                            |
| description                   | string                                                                             | The description for the guild (max 300 characters)                                               |
| brand\_color\_primary         | string                                                                             | The guild's accent color as a hexadecimal color string                                           |
| banner\_hash **(deprecated)** | ?string                                                                            | The guild's [clan banner hash](https://docs.discord.food/reference#cdn-formatting)               |
| game\_application\_ids        | array\[snowflake]                                                                  | The IDs of the applications representing the games the guild plays (max 20)                      |
| game\_activity                | map\[snowflake, [game activity](descubrimiento.md#game-activity-structure) object] | The activity of the guild in each game                                                           |
| tag                           | ?string                                                                            | The tag of the guild (2-4 characters)                                                            |
| badge                         | integer                                                                            | The [badge shown on the guild's tag](descubrimiento.md#guild-badge-type)                         |
| badge\_color\_primary         | string                                                                             | The primary color of the badge as a hexadecimal color string                                     |
| badge\_color\_secondary       | string                                                                             | The secondary color of the badge as a hexadecimal color string                                   |
| badge\_hash                   | string                                                                             | The [guild tag badge hash](https://docs.discord.food/reference#cdn-formatting)                   |
| traits                        | array\[[guild trait](descubrimiento.md#guild-trait-structure) object]              | Terms used to describe the guild's interest and personality (max 5)                              |
| features <sup>1</sup>         | array\[string]                                                                     | Enabled [guild features](https://docs.discord.food/resources/guild#guild-features)               |
| visibility                    | integer                                                                            | The [visibility level](descubrimiento.md#guild-visibility) of the guild                          |
| custom\_banner\_hash          | ?string                                                                            | The guild's [discovery splash hash](https://docs.discord.food/reference#cdn-formatting)          |
| premium\_subscription\_count  | integer                                                                            | The number of premium subscriptions (boosts) the guild currently has                             |
| premium\_tier                 | integer                                                                            | The guild's [premium tier](https://docs.discord.food/resources/guild#premium-tier) (boost level) |

<sup>1</sup> This is not a complete list of all the features the guild has, and is limited to community features (, , , , , ).

[**Game Activity Structure**](descubrimiento.md#game-activity-structure)

| Field           | Type    | Description                                 |
| --------------- | ------- | ------------------------------------------- |
| activity\_level | integer | The activity level of the guild in the game |
| activity\_score | integer | The activity score of the guild in the game |

[**Guild Trait Structure**](descubrimiento.md#guild-trait-structure)

| Field           | Type       | Description                                    |
| --------------- | ---------- | ---------------------------------------------- |
| emoji\_id       | ?snowflake | ID of the emoji associated with the trait      |
| emoji\_name     | ?string    | Name of the emoji associated with the trait    |
| emoji\_animated | boolean    | Whether the associated emoji is animated       |
| label           | string     | Name of the trait                              |
| position        | integer    | Position of the trait in the array for sorting |

[**Guild Badge Type**](descubrimiento.md#guild-badge-type)

| Value | Name        |
| ----- | ----------- |
| 0     | SWORD       |
| 1     | WATER\_DROP |
| 2     | SKULL       |
| 3     | TOADSTOOL   |
| 4     | MOON        |
| 5     | LIGHTNING   |
| 6     | LEAF        |
| 7     | HEART       |
| 8     | FIRE        |
| 9     | COMPASS     |
| 10    | CROSSHAIRS  |
| 11    | FLOWER      |
| 12    | FORCE       |
| 13    | GEM         |
| 14    | LAVA        |
| 15    | PSYCHIC     |
| 16    | SMOKE       |
| 17    | SNOW        |
| 18    | SOUND       |
| 19    | SUN         |
| 20    | WIND        |

[**Guild Visibility**](descubrimiento.md#guild-visibility)

| Value | Name                      | Description                                                                          |
| ----- | ------------------------- | ------------------------------------------------------------------------------------ |
| 1     | PUBLIC                    | This guild is considered public and can be viewed by anyone                          |
| 2     | RESTRICTED                | This guild is considered private but cannot be viewed and joining requires an invite |
| 3     | PUBLIC\_WITH\_RECRUITMENT | The guild is considered public, allowing anyone to view it and submit a join request |

[**Example Guild Profile**](descubrimiento.md#example-guild-profile)

```
{  "id": "1241115476021481582",  "name": "Fehlerjäger",  "icon_hash": "b47f6747d7d6548b6f3eaf8c8e8af20c",  "member_count": 131,  "online_count": 53,  "description": "Do you enjoy finding those creepy crawlies? 🐛 We seek those with a keen eye and ability for uncovering hidden gems 🔎",  "brand_color_primary": "#7cf895",  "banner_hash": "1468ceeb0f9c384826b982b7eddbfa6f",  "game_application_ids": ["356869127241072640"],  "game_activity": {    "356869127241072640": {      "activity_level": 1,      "activity_score": 45    }  },  "tag": "BUG",  "badge": 6,  "badge_color_primary": "#32a070",  "badge_color_secondary": "#57b59e",  "badge_hash": "6082c2553b03b47ccaea5203567df3cf",  "traits": [    {      "emoji_id": null,      "emoji_name": null,      "emoji_animated": false,      "label": "Bug Hunting",      "position": 0    }  ],  "features": ["MEMBER_VERIFICATION_MANUAL_APPROVAL", "COMMUNITY", "MEMBER_VERIFICATION_GATE_ENABLED"],  "visibility": 1,  "custom_banner_hash": null,  "premium_subscription_count": 13,  "premium_tier": 2}
```

### [Endpoints](descubrimiento.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get Discoverable Guilds](descubrimiento.md#get-discoverable-guilds) <a href="#get-discoverable-guilds" id="get-discoverable-guilds"></a>

`GET/discoverable-guilds`

Returns a list of [discoverable guild](descubrimiento.md#discoverable-guild-object) objects representing the guilds that are available for the current user to discover.

[**Query String Params**](descubrimiento.md#query-string-params)

| Field                          | Type              | Description                                                                                             |
| ------------------------------ | ----------------- | ------------------------------------------------------------------------------------------------------- |
| guild\_ids? <sup>1</sup>       | array\[snowflake] | The IDs of the discoverable guilds to return (max 48)                                                   |
| application\_ids? <sup>1</sup> | array\[snowflake] | The IDs of the applications to return matching discoverable guilds for (max 48)                         |
| categories? <sup>1</sup>       | array\[integer]   | The IDs of the [discovery categories](descubrimiento.md#discovery-category-object) to filter results by |
| limit? <sup>2</sup>            | integer           | The maximum number of guilds to return (max 48, default 48)                                             |
| offset? <sup>2</sup>           | integer           | Number of guilds to skip before returning guilds                                                        |

<sup>1</sup> Only one of , , or may be specified. If both are specified, only or is respected.

<sup>2</sup> Pagination parameters are ignored if or are specified.

[**Response Body**](descubrimiento.md#response-body)

| Field  | Type                                                                             | Description                                          |
| ------ | -------------------------------------------------------------------------------- | ---------------------------------------------------- |
| guilds | array\[[discoverable guild](descubrimiento.md#discoverable-guild-object) object] | The guilds that match the query                      |
| total  | integer                                                                          | The total number of guilds that match the query      |
| limit  | integer                                                                          | The number of guilds returned in the response        |
| offset | integer                                                                          | The number of guilds skipped before returning guilds |

#### [Search Discoverable Guilds](descubrimiento.md#search-discoverable-guilds) <a href="#search-discoverable-guilds" id="search-discoverable-guilds"></a>

`GET/discoverable-guilds/search`

Returns a list of [discoverable guild](descubrimiento.md#discoverable-guild-object) objects that match the query.

[**Query String Params**](descubrimiento.md#query-string-params)

| Field         | Type    | Description                                                                                          |
| ------------- | ------- | ---------------------------------------------------------------------------------------------------- |
| query         | string  | The query to match (max 100 characters)                                                              |
| limit?        | integer | The maximum number of guilds to return (max 48, default 24)                                          |
| offset?       | integer | Number of guilds to skip before returning guilds (max 2999)                                          |
| category\_id? | integer | The ID of the [discovery category](descubrimiento.md#discovery-category-object) to filter results by |

#### [Search Published Guilds](descubrimiento.md#search-published-guilds) <a href="#search-published-guilds" id="search-published-guilds"></a>

`GET/discovery/search`

Returns a list of [discoverable guild](descubrimiento.md#discoverable-guild-object) objects that have a landing web page and match the query. This endpoint is a proxy for [searching using the Algolia API](descubrimiento.md#searching-discovery). See the [Algolia Search documentation](https://www.algolia.com/doc/rest-api/search/) for more information.

[**Query String Params**](descubrimiento.md#query-string-params)

| Field   | Type    | Description                                                 |
| ------- | ------- | ----------------------------------------------------------- |
| query?  | string  | The query to match                                          |
| limit?  | integer | The maximum number of guilds to return (1-48, default 48)   |
| offset? | integer | Number of guilds to skip before returning guilds (max 2999) |

#### [Get Discovery Slug](descubrimiento.md#get-discovery-slug) <a href="#get-discovery-slug" id="get-discovery-slug"></a>

`GET/discovery/{guild.id}`

Returns information about a guild's landing web page or monetization store page. This endpoint requires the guild to either be discoverable and [published](descubrimiento.md#discovery-metadata-object) or have the [guild feature](https://docs.discord.food/resources/guild#guild-features).

[**Response Body**](descubrimiento.md#response-body)

| Field        | Type                                                                                  | Description                                                                                                     |
| ------------ | ------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| slug         | string                                                                                | The guild's discovery slug; can be appended to `https://discord.com/servers/` to get the guild's discovery page |
| guild?       | [discoverable guild](descubrimiento.md#discoverable-guild-object) object              | The guild information, if the guild is discoverable                                                             |
| store\_page? | [monetization store page](descubrimiento.md#monetization-store-page-structure) object | The guild's monetization store page, if enabled                                                                 |

[**Monetization Store Page Structure**](descubrimiento.md#monetization-store-page-structure)

| Field              | Type                                                                                            | Description                               |
| ------------------ | ----------------------------------------------------------------------------------------------- | ----------------------------------------- |
| guild              | [store page guild](descubrimiento.md#store-page-guild-structure) object                         | The guild information                     |
| role\_subscription | [store page role subscription](descubrimiento.md#store-page-role-subscription-structure) object | The guild's role subscription information |

[**Store Page Guild Structure**](descubrimiento.md#store-page-guild-structure)

| Field                        | Type                                                                               | Description                                                                                     |
| ---------------------------- | ---------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| id                           | snowflake                                                                          | The ID of the guild                                                                             |
| name                         | string                                                                             | The name of the guild (2-100 characters)                                                        |
| icon\_hash                   | ?string                                                                            | The guild's [icon hash](https://docs.discord.food/reference#cdn-formatting)                     |
| approximate\_member\_count   | integer                                                                            | Approximate number of members in the guild                                                      |
| approximate\_presence\_count | integer                                                                            | Approximate number of online members in the guild                                               |
| locked\_server               | boolean                                                                            | Whether the entire guild is locked behind a role subscription                                   |
| invite                       | ?partial [invite](https://docs.discord.food/resources/invite#invite-object) object | A [special invite](https://docs.discord.food/resources/invite#invite-target-type) for the guild |

[**Store Page Role Subscription Structure**](descubrimiento.md#store-page-role-subscription-structure)

| Field                  | Type                                                                                         | Description                                                                                     |
| ---------------------- | -------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| settings               | role subscription settings object                                                            | The guild's role subscription settings                                                          |
| group\_listings        | array\[role subscription group listing object]                                               | The guild's role subscription group listings                                                    |
| trials                 | array\[role subscription trial object]                                                       | The guild's role subscription trials                                                            |
| subscriber\_count      | ?integer                                                                                     | The number of subscribers to the guild's role subscriptions, if public                          |
| benefit\_channels      | array\[partial [channel](https://docs.discord.food/resources/channel#channel-object) object] | The channels that are unlocked by role subscriptions                                            |
| benefit\_emojis        | array\[[emoji](https://docs.discord.food/resources/emoji#emoji-object) object]               | The emojis that are unlocked by role subscriptions                                              |
| purchase\_page\_invite | ?partial [invite](https://docs.discord.food/resources/invite#invite-object) object           | A [special invite](https://docs.discord.food/resources/invite#invite-target-type) for the guild |

#### [Get Discovery Categories](descubrimiento.md#get-discovery-categories) <a href="#get-discovery-categories" id="get-discovery-categories"></a>

`GET/discovery/categories`

Returns a list of [discovery category](descubrimiento.md#discovery-category-object) objects representing the available discovery categories.

[**Query String Params**](descubrimiento.md#query-string-params)

| Field          | Type    | Description                                                                                               |
| -------------- | ------- | --------------------------------------------------------------------------------------------------------- |
| locale?        | string  | The [language](https://docs.discord.food/reference#locales) to return category names in (default "en-US") |
| primary\_only? | boolean | Whether to only return categories that can be set as a guild's primary category (default false)           |

#### [Validate Discovery Search Term](descubrimiento.md#validate-discovery-search-term) <a href="#validate-discovery-search-term" id="validate-discovery-search-term"></a>

`GET/discovery/valid-term`

Checks if a discovery search term is allowed.

[**Query String Params**](descubrimiento.md#query-string-params)

| Field | Type   | Description                 |
| ----- | ------ | --------------------------- |
| term  | string | The search term to validate |

[**Response Body**](descubrimiento.md#response-body)

| Field | Type    | Description                        |
| ----- | ------- | ---------------------------------- |
| valid | boolean | Whether the provided term is valid |

[**Example Response**](descubrimiento.md#example-response)

```
{ "valid": true }
```

#### [Get Guild Discovery Requirements](descubrimiento.md#get-guild-discovery-requirements) <a href="#get-guild-discovery-requirements" id="get-guild-discovery-requirements"></a>

`GET/guilds/{guild.id}/discovery-requirements`

Returns the [discovery requirements](descubrimiento.md#discovery-requirements-object) object for the guild. Requires the permission.

#### [Get Guild Discovery Metadata](descubrimiento.md#get-guild-discovery-metadata) <a href="#get-guild-discovery-metadata" id="get-guild-discovery-metadata"></a>

`GET/guilds/{guild.id}/discovery-metadata`

Returns the [discovery metadata](descubrimiento.md#discovery-metadata-object) object for the guild. Requires the permission.

#### [Modify Guild Discovery Metadata](descubrimiento.md#modify-guild-discovery-metadata) <a href="#modify-guild-discovery-metadata" id="modify-guild-discovery-metadata"></a>

`PATCH/guilds/{guild.id}/discovery-metadata`

Replaces the discovery metadata for the guild. Requires the permission. Returns the updated [discovery metadata](descubrimiento.md#discovery-metadata-object) object on success.

[**JSON Params**](descubrimiento.md#json-params)

| Field                            | Type                                                                             | Description                                                                                                           |
| -------------------------------- | -------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| primary\_category\_id?           | ?integer                                                                         | The ID of the primary [discovery category](descubrimiento.md#discovery-category-object) set for the guild (default 0) |
| keywords?                        | ?array\[string]                                                                  | The discovery search keywords for the guild (max 10)                                                                  |
| emoji\_discoverability\_enabled? | ?boolean                                                                         | Whether the guild is shown as a source through custom emojis and stickers (default true)                              |
| is\_published?                   | ?boolean                                                                         | Whether the guild's landing web page is currently published (default false)                                           |
| reasons\_to\_join?               | ?array\[[discovery reason](descubrimiento.md#discovery-reason-structure) object] | The reasons to join the guild shown in the discovery web page (max 4)                                                 |
| social\_links?                   | ?array\[string]                                                                  | The guild's social media links shown in the discovery web page (max 256 characters, max 9)                            |
| about?                           | ?string                                                                          | The guild's long description shown in the discovery web page (max 2400 characters)                                    |

#### [Add Guild Discovery Subcategory](descubrimiento.md#add-guild-discovery-subcategory) <a href="#add-guild-discovery-subcategory" id="add-guild-discovery-subcategory"></a>

`PUT/guilds/{guild.id}/discovery-categories/{category.id}`

Adds a [discovery subcategory](descubrimiento.md#discovery-category-object) to the guild. Requires the permission. Returns a 204 empty response on success.

#### [Remove Guild Discovery Subcategory](descubrimiento.md#remove-guild-discovery-subcategory) <a href="#remove-guild-discovery-subcategory" id="remove-guild-discovery-subcategory"></a>

`DELETE/guilds/{guild.id}/discovery-categories/{category.id}`

Removes a [discovery subcategory](descubrimiento.md#discovery-category-object) from the guild. Requires the permission. Returns a 204 empty response on success.

#### [Get Guild Profile](descubrimiento.md#get-guild-profile) <a href="#get-guild-profile" id="get-guild-profile"></a>

`GET/guilds/{guild.id}/profile`

Returns a [guild profile](descubrimiento.md#guild-profile-object) object for the given guild ID. User must be a member of the guild or the guild must be discoverable or have a [or visibility](descubrimiento.md#guild-visibility).

#### [Modify Guild Profile](descubrimiento.md#modify-guild-profile) <a href="#modify-guild-profile" id="modify-guild-profile"></a>

`PATCH/guilds/{guild.id}/profile`

Modifies the [guild profile](descubrimiento.md#guild-profile-object) for the given guild ID. Requires the permission. Returns the updated [guild profile](descubrimiento.md#guild-profile-object) object on success.

[**JSON Params**](descubrimiento.md#json-params)

| Field                                | Type                                                                  | Description                                                                                    |
| ------------------------------------ | --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| name?                                | string                                                                | The name of the guild (2-100 characters)                                                       |
| icon?                                | ?[image data](https://docs.discord.food/reference#cdn-data)           | The guild's icon; animated icons are only shown when the guild has the `ANIMATED_ICON` feature |
| description?                         | ?string                                                               | The description for the guild (max 300 characters)                                             |
| brand\_color\_primary?               | string                                                                | The guild's accent color as a hexadecimal color string                                         |
| game\_application\_ids?              | array\[snowflake]                                                     | The IDs of the applications representing the games the guild plays (max 20)                    |
| tag? <sup>1</sup>                    | ?string                                                               | The tag of the guild (2-4 characters)                                                          |
| badge <sup>1</sup>                   | integer                                                               | The [badge shown on the guild's tag](descubrimiento.md#guild-badge-type)                       |
| badge\_color\_primary <sup>1</sup>   | ?string                                                               | The primary color of the badge as a hexadecimal color string                                   |
| badge\_color\_secondary <sup>1</sup> | ?string                                                               | The secondary color of the badge as a hexadecimal color string                                 |
| traits?                              | array\[[guild trait](descubrimiento.md#guild-trait-structure) object] | Terms used to describe the guild's interest and personality (max 5)                            |
| visibility?                          | integer                                                               | The [visibility level](descubrimiento.md#guild-visibility) of the guild                        |
| custom\_banner                       | ?[image data](https://docs.discord.food/reference#cdn-data)           | The guild's discovery splash                                                                   |

<sup>1</sup> Requires the [guild feature](https://docs.discord.food/resources/guild#guild-features).

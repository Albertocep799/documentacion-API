---
icon: rocket-launch
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

# Aplicaciones

### [Applications](aplicaciones.md#applications) <a href="#applications" id="applications"></a>

Applications are Discord entities that represent games, services, and other integrations that can be added to a guild. Applications can be used for a variety of purposes, including OAuth2 authentication, rich presence, bots, and much more.

#### [Application Object](aplicaciones.md#application-object) <a href="#application-object" id="application-object"></a>

[**Application Structure**](aplicaciones.md#application-structure)

| Field                                                    | Type                                                                                                                                      | Description                                                                                                                                                                                                                      |
| -------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id                                                       | snowflake                                                                                                                                 | The ID of the application                                                                                                                                                                                                        |
| name                                                     | string                                                                                                                                    | The name of the application                                                                                                                                                                                                      |
| description                                              | string                                                                                                                                    | The description of the application                                                                                                                                                                                               |
| icon                                                     | ?string                                                                                                                                   | The application's [icon hash](https://docs.discord.food/reference#cdn-formatting)                                                                                                                                                |
| cover\_image?                                            | string                                                                                                                                    | The application's default rich presence invite [cover image hash](https://docs.discord.food/reference#cdn-formatting)                                                                                                            |
| splash?                                                  | string                                                                                                                                    | The application's [splash hash](https://docs.discord.food/reference#cdn-formatting)                                                                                                                                              |
| type                                                     | ?integer                                                                                                                                  | The [type of the application](aplicaciones.md#application-type), if any                                                                                                                                                          |
| flags                                                    | integer                                                                                                                                   | The [application's flags](aplicaciones.md#application-flags) (including private)                                                                                                                                                 |
| primary\_sku\_id? <sup>1</sup>                           | snowflake                                                                                                                                 | The ID of the application's primary SKU (game, application subscription, etc.)                                                                                                                                                   |
| verify\_key                                              | string                                                                                                                                    | The hex encoded client public key for verification in interactions and the GameSDK's `GetTicket`                                                                                                                                 |
| guild\_id?                                               | snowflake                                                                                                                                 | The ID of the guild linked to the application                                                                                                                                                                                    |
| eula\_id?                                                | snowflake                                                                                                                                 | The ID of the EULA required to play the application's game                                                                                                                                                                       |
| slug? <sup>1</sup>                                       | string                                                                                                                                    | The URL slug that links to the primary store page of the application                                                                                                                                                             |
| aliases?                                                 | array\[string]                                                                                                                            | Other names the application's game is associated with                                                                                                                                                                            |
| executables?                                             | array\[[application executable](aplicaciones.md#application-executable-object) object]                                                    | The unique executables of the application's game                                                                                                                                                                                 |
| third\_party\_skus?                                      | array\[[application SKU](aplicaciones.md#application-sku-object) object]                                                                  | The third party SKUs of the application's game                                                                                                                                                                                   |
| hook                                                     | boolean                                                                                                                                   | Whether the Discord client is allowed to hook into the application's game directly                                                                                                                                               |
| overlay?                                                 | boolean                                                                                                                                   | Whether the application's game supports the [Discord overlay](https://support.discord.com/hc/en-us/articles/217659737-Game-Overlay-101) (default false)                                                                          |
| overlay\_methods?                                        | integer                                                                                                                                   | The [methods of overlaying](aplicaciones.md#overlay-method-flags) that the application's game supports                                                                                                                           |
| overlay\_warn?                                           | boolean                                                                                                                                   | Whether the [Discord overlay](https://support.discord.com/hc/en-us/articles/217659737-Game-Overlay-101) is known to be problematic with this application's game (default false)                                                  |
| overlay\_compatibility\_hook?                            | boolean                                                                                                                                   | Whether to use the compatibility hook for the overlay (default false)                                                                                                                                                            |
| bot?                                                     | partial [user](https://docs.discord.food/resources/user#user-object) object                                                               | The bot attached to this application                                                                                                                                                                                             |
| owner                                                    | partial [user](https://docs.discord.food/resources/user#user-object) object                                                               | The owner of the application                                                                                                                                                                                                     |
| team? <sup>2</sup>                                       | ?[team](https://docs.discord.food/resources/team#team-object) object                                                                      | The team that owns the application                                                                                                                                                                                               |
| developers?                                              | array\[[company](https://docs.discord.food/resources/team#company-object) object]                                                         | The companies that developed the application                                                                                                                                                                                     |
| publishers?                                              | array\[[company](https://docs.discord.food/resources/team#company-object) object]                                                         | The companies that published the application                                                                                                                                                                                     |
| rpc\_origins?                                            | array\[string]                                                                                                                            | The whitelisted RPC origin URLs for the application, if RPC is enabled                                                                                                                                                           |
| redirect\_uris                                           | array\[string]                                                                                                                            | The whitelisted URLs for redirecting to during [OAuth2 authorization](https://docs.discord.food/topics/oauth2) (max 10)                                                                                                          |
| deeplink\_uri?                                           | string                                                                                                                                    | The URL used for deep linking during [OAuth2 authorization](https://docs.discord.food/topics/oauth2) on mobile devices                                                                                                           |
| integration\_public                                      | boolean                                                                                                                                   | Whether only the application owner can add the integration                                                                                                                                                                       |
| integration\_require\_code\_grant                        | boolean                                                                                                                                   | Whether the integration will only be added upon completion of a full OAuth2 token exchange                                                                                                                                       |
| bot\_public? <sup>3</sup> **(deprecated)**               | boolean                                                                                                                                   | Whether only the application owner can add the bot                                                                                                                                                                               |
| bot\_require\_code\_grant? <sup>3</sup> **(deprecated)** | boolean                                                                                                                                   | Whether the application's bot will only be added upon completion of a full OAuth2 token exchange                                                                                                                                 |
| bot\_disabled?                                           | boolean                                                                                                                                   | Whether the application's bot is disabled by Discord (default false)                                                                                                                                                             |
| bot\_quarantined?                                        | boolean                                                                                                                                   | Whether the application's bot is [quarantined](https://support.discord.com/hc/en-us/articles/6461420677527-Limited-Access-FAQ) by Discord; quarantined bots cannot join more guilds or start new direct messages (default false) |
| approximate\_guild\_count?                               | integer                                                                                                                                   | Approximate count of guilds the application's bot is in                                                                                                                                                                          |
| approximate\_user\_install\_count                        | integer                                                                                                                                   | Approximate count of users that have authorized the application with the `applications.commands` scope                                                                                                                           |
| approximate\_user\_authorization\_count                  | integer                                                                                                                                   | Approximate count of users that have OAuth2 authorizations for the application                                                                                                                                                   |
| internal\_guild\_restriction                             | integer                                                                                                                                   | What guilds the [application can be authorized in](aplicaciones.md#internal-guild-restriction)                                                                                                                                   |
| terms\_of\_service\_url?                                 | string                                                                                                                                    | The URL to the application's terms of service                                                                                                                                                                                    |
| privacy\_policy\_url?                                    | string                                                                                                                                    | The URL to the application's privacy policy                                                                                                                                                                                      |
| role\_connections\_verification\_url                     | ?string                                                                                                                                   | The role connection verification entry point of the integration; when configured, this will render the application as a verification method in guild role verification configuration                                             |
| interactions\_endpoint\_url                              | string                                                                                                                                    | The URL of the application's [interactions endpoint](https://docs.discord.food/interactions/receiving-and-responding#receiving-an-interaction)                                                                                   |
| interactions\_version                                    | integer                                                                                                                                   | The [version of the application's interactions endpoint implementation](aplicaciones.md#application-interactions-version)                                                                                                        |
| interactions\_event\_types <sup>4</sup>                  | array\[string]                                                                                                                            | The enabled [event webhook types](aplicaciones.md#event-webhooks-type) to send to the interaction endpoint                                                                                                                       |
| event\_webhooks\_status?                                 | integer                                                                                                                                   | Whether [event webhooks are enabled](aplicaciones.md#event-webhooks-status)                                                                                                                                                      |
| event\_webhooks\_url?                                    | string                                                                                                                                    | The URL of the application's event webhooks endpoint                                                                                                                                                                             |
| event\_webhooks\_types?                                  | array\[string]                                                                                                                            | The enabled [event webhook types](aplicaciones.md#event-webhooks-type) to send to the event webhooks endpoint                                                                                                                    |
| explicit\_content\_filter                                | integer                                                                                                                                   | [Whether uploaded media content](aplicaciones.md#explicit-content-filter-level) used in application commands is scanned and deleted for explicit content                                                                         |
| tags?                                                    | array\[string]                                                                                                                            | Tags describing the content and functionality of the application (max 20 characters, max 5)                                                                                                                                      |
| install\_params?                                         | [application install params](aplicaciones.md#application-install-params-object) object                                                    | The default in-app authorization link for the integration                                                                                                                                                                        |
| custom\_install\_url?                                    | string                                                                                                                                    | The default custom authorization link for the integration                                                                                                                                                                        |
| integration\_types\_config?                              | map\[integer, ?[application integration type configuration](aplicaciones.md#application-integration-type-configuration-structure) object] | The configuration for each [integration type](aplicaciones.md#application-integration-type) supported by the application                                                                                                         |
| is\_verified                                             | boolean                                                                                                                                   | Whether the application is verified                                                                                                                                                                                              |
| verification\_state                                      | integer                                                                                                                                   | The current [verification state](aplicaciones.md#application-verification-state) of the application                                                                                                                              |
| store\_application\_state                                | integer                                                                                                                                   | The current [store approval state](aplicaciones.md#store-application-state) of the commerce application                                                                                                                          |
| rpc\_application\_state                                  | integer                                                                                                                                   | The current [RPC approval state](aplicaciones.md#rpc-application-state) of the application                                                                                                                                       |
| creator\_monetization\_state <sup>5</sup>                | integer                                                                                                                                   | The current guild [creator monetization state](aplicaciones.md#creator-monetization-state) of the application                                                                                                                    |
| is\_discoverable                                         | boolean                                                                                                                                   | Whether the application is discoverable in the application directory                                                                                                                                                             |
| discoverability\_state                                   | integer                                                                                                                                   | The current [application directory discoverability state](aplicaciones.md#application-discoverability-state) of the application                                                                                                  |
| discovery\_eligibility\_flags                            | integer                                                                                                                                   | The current [application directory eligibility flags](aplicaciones.md#application-discovery-eligibility-flags) for the application                                                                                               |
| is\_monetized                                            | boolean                                                                                                                                   | Whether the application has monetization enabled                                                                                                                                                                                 |
| storefront\_available                                    | boolean                                                                                                                                   | Whether the application has public subscriptions or products available for purchase                                                                                                                                              |
| monetization\_state                                      | integer                                                                                                                                   | The current [application monetization state](aplicaciones.md#application-monetization-state) of the application                                                                                                                  |
| monetization\_eligibility\_flags? <sup>2</sup>           | integer                                                                                                                                   | The current [application monetization eligibility flags](aplicaciones.md#application-monetization-eligibility-flags) for the application                                                                                         |
| max\_participants? <sup>6</sup>                          | integer                                                                                                                                   | The maximum possible participants in the application's embedded activity (-1 for no limit)                                                                                                                                       |
| embedded\_activity\_config? <sup>6</sup>                 | [embedded activity config](aplicaciones.md#embedded-activity-config-object) object                                                        | The configuration for the application's embedded activity                                                                                                                                                                        |

<sup>1</sup> The and fields can be combined to form a URL to the application's primary store page like so: .

<sup>2</sup> Only present when fetched from the [Get Current Application](aplicaciones.md#get-current-application), [Get Application](aplicaciones.md#get-application), or [Transfer Application](aplicaciones.md#transfer-application) endpoints.

<sup>3</sup> In some cases, these fields may still be provided instead of and . These fields will not be present if the application does not have a bot.

<sup>4</sup> The sending of Gateway events over the interactions endpoint requires [interactions version 2](aplicaciones.md#application-interactions-version).

<sup>5</sup> Only applicable for applications of type.

<sup>6</sup> Only applicable for applications with the [flag](aplicaciones.md#application-flags).

[**Partial Application Structure**](aplicaciones.md#partial-application-structure)

| Field                                                    | Type                                                                                                                                      | Description                                                                                                                                                                     |
| -------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id                                                       | snowflake                                                                                                                                 | The ID of the application                                                                                                                                                       |
| name                                                     | string                                                                                                                                    | The name of the application                                                                                                                                                     |
| description                                              | string                                                                                                                                    | The description of the application                                                                                                                                              |
| icon                                                     | ?string                                                                                                                                   | The application's [icon hash](https://docs.discord.food/reference#cdn-formatting)                                                                                               |
| cover\_image?                                            | string                                                                                                                                    | The application's default rich presence invite [cover image hash](https://docs.discord.food/reference#cdn-formatting)                                                           |
| splash?                                                  | string                                                                                                                                    | The application's [splash hash](https://docs.discord.food/reference#cdn-formatting)                                                                                             |
| type                                                     | ?integer                                                                                                                                  | The [type of the application](aplicaciones.md#application-type), if any                                                                                                         |
| flags                                                    | integer                                                                                                                                   | The [application's flags](aplicaciones.md#application-flags) (including private)                                                                                                |
| primary\_sku\_id? <sup>1</sup>                           | snowflake                                                                                                                                 | The ID of the application's primary SKU (game, application subscription, etc.)                                                                                                  |
| verify\_key                                              | string                                                                                                                                    | The hex encoded client public key for verification in interactions and the GameSDK's `GetTicket`                                                                                |
| guild\_id?                                               | snowflake                                                                                                                                 | The ID of the guild linked to the application                                                                                                                                   |
| guild? <sup>2</sup>                                      | partial [guild](https://docs.discord.food/resources/guild#guild-object) object                                                            | The guild linked to the application                                                                                                                                             |
| eula\_id?                                                | snowflake                                                                                                                                 | The ID of the EULA required to play the application's game {/ _todo: link this here_ /}                                                                                         |
| slug? <sup>1</sup>                                       | string                                                                                                                                    | The URL slug that links to the primary store page of the application                                                                                                            |
| aliases?                                                 | array\[string]                                                                                                                            | Other names the application's game is associated with                                                                                                                           |
| executables?                                             | array\[[application executable](aplicaciones.md#application-executable-object) object]                                                    | The unique executables of the application's game                                                                                                                                |
| third\_party\_skus?                                      | array\[[application SKU](aplicaciones.md#application-sku-object) object]                                                                  | The third party SKUs of the application's game                                                                                                                                  |
| hook                                                     | boolean                                                                                                                                   | Whether the Discord client is allowed to hook into the application's game directly                                                                                              |
| overlay?                                                 | boolean                                                                                                                                   | Whether the application's game supports the [Discord overlay](https://support.discord.com/hc/en-us/articles/217659737-Game-Overlay-101) (default false)                         |
| overlay\_methods?                                        | integer                                                                                                                                   | The [methods of overlaying](aplicaciones.md#overlay-method-flags) that the application's game supports                                                                          |
| overlay\_warn?                                           | boolean                                                                                                                                   | Whether the [Discord overlay](https://support.discord.com/hc/en-us/articles/217659737-Game-Overlay-101) is known to be problematic with this application's game (default false) |
| overlay\_compatibility\_hook?                            | boolean                                                                                                                                   | Whether to use the compatibility hook for the overlay (default false)                                                                                                           |
| bot?                                                     | partial [user](https://docs.discord.food/resources/user#user-object) object                                                               | The bot attached to this application                                                                                                                                            |
| team? <sup>3</sup>                                       | ?[team](https://docs.discord.food/resources/team#team-object) object                                                                      | The team that owns the application                                                                                                                                              |
| developers?                                              | array\[[company](https://docs.discord.food/resources/team#company-object) object]                                                         | The companies that developed the application                                                                                                                                    |
| publishers?                                              | array\[[company](https://docs.discord.food/resources/team#company-object) object]                                                         | The companies that published the application                                                                                                                                    |
| rpc\_origins?                                            | array\[string]                                                                                                                            | The whitelisted RPC origin URLs for the application, if RPC is enabled                                                                                                          |
| deeplink\_uri?                                           | string                                                                                                                                    | The URL used for deep linking during [OAuth2 authorization](https://docs.discord.food/topics/oauth2) on mobile devices                                                          |
| integration\_public?                                     | boolean                                                                                                                                   | Whether only the application owner can add the integration                                                                                                                      |
| integration\_require\_code\_grant?                       | boolean                                                                                                                                   | Whether the integration will only be added upon completion of a full OAuth2 token exchange                                                                                      |
| bot\_public? <sup>4</sup> **(deprecated)**               | boolean                                                                                                                                   | Whether only the application owner can add the bot                                                                                                                              |
| bot\_require\_code\_grant? <sup>4</sup> **(deprecated)** | boolean                                                                                                                                   | Whether the application's bot will only be added upon completion of a full OAuth2 token exchange                                                                                |
| terms\_of\_service\_url?                                 | ?string                                                                                                                                   | The URL to the application's terms of service                                                                                                                                   |
| privacy\_policy\_url?                                    | ?string                                                                                                                                   | The URL to the application's privacy policy                                                                                                                                     |
| tags?                                                    | array\[string]                                                                                                                            | Tags describing the content and functionality of the application (max 20 characters, max 5)                                                                                     |
| install\_params?                                         | [application install params](aplicaciones.md#application-install-params-object) object                                                    | The default in-app authorization link for the integration                                                                                                                       |
| custom\_install\_url?                                    | string                                                                                                                                    | The default custom authorization link for the integration                                                                                                                       |
| integration\_types\_config?                              | map\[integer, ?[application integration type configuration](aplicaciones.md#application-integration-type-configuration-structure) object] | The configuration for each [integration type](aplicaciones.md#application-integration-type) supported by the application                                                        |
| is\_verified                                             | boolean                                                                                                                                   | Whether the application is verified                                                                                                                                             |
| is\_discoverable                                         | boolean                                                                                                                                   | Whether the application is discoverable in the application directory                                                                                                            |
| is\_monetized                                            | boolean                                                                                                                                   | Whether the application has monetization enabled                                                                                                                                |
| storefront\_available                                    | boolean                                                                                                                                   | Whether the application has public subscriptions or products available for purchase                                                                                             |
| max\_participants? <sup>5</sup>                          | integer                                                                                                                                   | The maximum possible participants in the application's embedded activity (-1 for no limit)                                                                                      |
| embedded\_activity\_config? <sup>5</sup>                 | [embedded activity config](aplicaciones.md#embedded-activity-config-object) object                                                        | The configuration for the application's embedded activity                                                                                                                       |

<sup>1</sup> The and fields can be combined to form a URL to the application's primary store page like so: .

<sup>2</sup> Only present when fetched from the [Get Partial Application](aplicaciones.md#get-partial-application) endpoint with set to . The guild must be discoverable.

<sup>3</sup> Only present when fetched from the [Get Guild Applications](aplicaciones.md#get-guild-applications) endpoint. You must own the application or be a member of the owning team to receive this information.

<sup>4</sup> In some cases, these fields may still be provided instead of and . These fields will not be present if the application does not have a bot.

<sup>5</sup> Only applicable for applications with the [flag](aplicaciones.md#application-flags).

[**Application Type**](aplicaciones.md#application-type)

| Value          | Name                  | Description                                                                       |
| -------------- | --------------------- | --------------------------------------------------------------------------------- |
| 1              | GAME                  | A game integrating with Discord                                                   |
| ~~2~~          | ~~MUSIC~~             | ~~A music service integrating with Discord~~                                      |
| 3 <sup>1</sup> | TICKETED\_EVENTS      | A limited application used for ticketed event SKUs                                |
| 4 <sup>1</sup> | CREATOR\_MONETIZATION | A limited application used for creator monetization (e.g. role subscription) SKUs |

<sup>1</sup> Applications of these types cannot be used through most of the regular applications APIs outlined here.

[**Application Flags**](aplicaciones.md#application-flags)

| Value       | Name                                       | Description                                                                                                                                                                         | Public  |
| ----------- | ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| 1 << 1      | EMBEDDED\_RELEASED                         | Embedded application is released to the public (see also [release phases](aplicaciones.md#embedded-activity-release-phase))                                                         | Yes     |
| 1 << 2      | MANAGED\_EMOJI                             | Application can create managed emoji                                                                                                                                                | No      |
| 1 << 3      | EMBEDDED\_IAP                              | Embedded application can use in-app purchases                                                                                                                                       | Yes     |
| 1 << 4      | GROUP\_DM\_CREATE                          | Application can create group DMs without limit                                                                                                                                      | No      |
| ~~1 << 5~~  | ~~RPC\_PRIVATE\_BETA~~                     | ~~Application can access the client RPC server~~                                                                                                                                    | ~~No~~  |
| 1 << 6      | AUTO\_MODERATION\_RULE\_CREATE\_BADGE      | Application has created 100+ AutoMod rules                                                                                                                                          | Yes     |
| 1 << 7      | GAME\_PROFILE\_DISABLED                    | Application has its game profile page disabled                                                                                                                                      | Yes     |
| 1 << 8      | PUBLIC\_OAUTH2\_CLIENT                     | Application's OAuth2 credentials are considered public and a client secret is not required                                                                                          | Yes     |
| 1 << 9      | CONTEXTLESS\_ACTIVITY                      | Embedded application's activity can be launched without a context                                                                                                                   | Yes     |
| 1 << 10     | SOCIAL\_LAYER\_INTEGRATION\_LIMITED        | Application has limited access to the social layer SDK                                                                                                                              | Yes     |
| 1 << 11     | CLOUD\_GAMING\_DEMO                        | Application is trialing cloud gaming features                                                                                                                                       | Yes     |
| 1 << 12     | GATEWAY\_PRESENCE                          | Intent required for bots in **100 or more guilds** to receive [Presence Update](https://docs.discord.food/topics/gateway-events#presence-update) Gateway events                     | Yes     |
| 1 << 13     | GATEWAY\_PRESENCE\_LIMITED                 | Intent required for bots in **under 100 guilds** to receive [Presence Update](https://docs.discord.food/topics/gateway-events#presence-update) Gateway events                       | Yes     |
| 1 << 14     | GATEWAY\_GUILD\_MEMBERS                    | Intent required for bots in **100 or more guilds** to receive guild member-related events like [Guild Member Add](https://docs.discord.food/topics/gateway-events#guild-member-add) | Yes     |
| 1 << 15     | GATEWAY\_GUILD\_MEMBERS\_LIMITED           | Intent required for bots in **under 100 guilds** to receive guild member-related events like [Guild Member Add](https://docs.discord.food/topics/gateway-events#guild-member-add)   | Yes     |
| 1 << 16     | VERIFICATION\_PENDING\_GUILD\_LIMIT        | Indicates unusual growth of an application that prevents verification                                                                                                               | Yes     |
| 1 << 17     | EMBEDDED                                   | Application can be embedded within the Discord client                                                                                                                               | Yes     |
| 1 << 18     | GATEWAY\_MESSAGE\_CONTENT                  | Intent required for bots in **100 or more guilds** to receive [message content](https://support-dev.discord.com/hc/en-us/articles/4404772028055)                                    | Yes     |
| 1 << 19     | GATEWAY\_MESSAGE\_CONTENT\_LIMITED         | Intent required for bots in **under 100 guilds** to receive [message content](https://support-dev.discord.com/hc/en-us/articles/4404772028055)                                      | Yes     |
| 1 << 20     | EMBEDDED\_FIRST\_PARTY                     | Embedded application is created by Discord                                                                                                                                          | Yes     |
| 1 << 21     | APPLICATION\_COMMAND\_MIGRATED             | Unknown                                                                                                                                                                             | Yes     |
| 1 << 23     | APPLICATION\_COMMAND\_BADGE                | Application has registered global application commands                                                                                                                              | Yes     |
| 1 << 24     | ACTIVE                                     | Application has had at least one global application command used in the last 30 days                                                                                                | No      |
| 1 << 25     | ACTIVE\_GRACE\_PERIOD <sup>1</sup>         | Application has not had any global application commands used in the last 30 days and has lost the `ACTIVE` flag                                                                     | No      |
| 1 << 26     | IFRAME\_MODAL                              | Application can use IFrames within modals                                                                                                                                           | Yes     |
| 1 << 27     | SOCIAL\_LAYER\_INTEGRATION                 | Application can use the social layer SDK                                                                                                                                            | Yes     |
| 1 << 29     | PROMOTED                                   | Application is promoted by Discord in the application directory                                                                                                                     | Yes     |
| 1 << 30     | PARTNER                                    | Application is a Discord partner                                                                                                                                                    | Yes     |
| ~~1 << 8~~  | ~~ALLOW\_ASSETS~~                          | ~~Application can use activity assets~~                                                                                                                                             | ~~No~~  |
| ~~1 << 9~~  | ~~ALLOW\_ACTIVITY\_ACTION\_SPECTATE~~      | ~~Application can enable spectating activities~~                                                                                                                                    | ~~No~~  |
| ~~1 << 10~~ | ~~ALLOW\_ACTIVITY\_ACTION\_JOIN\_REQUEST~~ | ~~Application can enable activity join requests~~                                                                                                                                   | ~~No~~  |
| ~~1 << 11~~ | ~~RPC\_HAS\_CONNECTED~~                    | ~~Application has accessed the client RPC server before~~                                                                                                                           | ~~Yes~~ |

<sup>1</sup> The active grace period lasts for 30 days, after which the [user flag](https://docs.discord.food/resources/user#user-flags) will be removed from developers that claimed it.

[**Overlay Method Flags**](aplicaciones.md#overlay-method-flags)

| Value  | Name             | Description                            |
| ------ | ---------------- | -------------------------------------- |
| 1 << 0 | OUT\_OF\_PROCESS | Overlay can be rendered out of process |

[**Internal Guild Restriction**](aplicaciones.md#internal-guild-restriction)

| Value | Name                 | Description                                                                                                                            |
| ----- | -------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| 1     | JOIN\_ALL            | The application can be authorized in any guild                                                                                         |
| 2     | JOIN\_EXTERNAL\_ONLY | The application can only be authorized in guilds without the [guild feature](https://docs.discord.food/resources/guild#guild-features) |
| 3     | JOIN\_INTERNAL\_ONLY | The application can only be authorized in guilds with the [guild feature](https://docs.discord.food/resources/guild#guild-features)    |

[**Application Interactions Version**](aplicaciones.md#application-interactions-version)

| Value | Name       | Description                                                                                                                           |
| ----- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| 1     | VERSION\_1 | Only [Interaction Create](https://docs.discord.food/topics/gateway-events#interaction-create) events are sent as documented (default) |
| 2     | VERSION\_2 | A selection of chosen events are sent                                                                                                 |

[**Event Webhooks Status**](aplicaciones.md#event-webhooks-status)

| Value | Name     | Description                 |
| ----- | -------- | --------------------------- |
| 1     | DISABLED | Event webhooks are disabled |
| 2     | ENABLED  | Event webhooks are enabled  |

[**Event Webhooks Type**](aplicaciones.md#event-webhooks-type)

| Value                   | Description                                 |
| ----------------------- | ------------------------------------------- |
| APPLICATION\_AUTHORIZED | Sent when a user authorizes the application |
| ENTITLEMENT\_CREATE     | Sent when a user creates an entitlement     |
| QUEST\_USER\_ENROLLMENT | Sent when a user enrolls in a quest         |

[**Explicit Content Filter Level**](aplicaciones.md#explicit-content-filter-level)

| Value | Name    | Description                                                                                                             |
| ----- | ------- | ----------------------------------------------------------------------------------------------------------------------- |
| 0     | INHERIT | Inherits the guild's [explicit content filter](https://docs.discord.food/resources/guild#explicit-content-filter-level) |
| 1     | ALWAYS  | Media content will always be scanned                                                                                    |

[**Application Verification State**](aplicaciones.md#application-verification-state)

| Value | Name                    | Description                                                                                          |
| ----- | ----------------------- | ---------------------------------------------------------------------------------------------------- |
| 1     | INELIGIBLE              | This application is ineligible for verification                                                      |
| 2     | UNSUBMITTED             | This application has not yet been applied for verification                                           |
| 3     | SUBMITTED               | This application has submitted a verification request                                                |
| 4     | APPROVED\_MANUALLY      | This application has been verified manually from Discord staff or using the old verification process |
| 5     | BLOCKED                 | This application is blocked and cannot be verified                                                   |
| 6     | APPROVED\_AUTOMATICALLY | This application has been verified automatically through the Stripe identity verification process    |

[**Store Application State**](aplicaciones.md#store-application-state)

| Value | Name      | Description                                                                                |
| ----- | --------- | ------------------------------------------------------------------------------------------ |
| 1     | NONE      | This application does not have a commerce license                                          |
| 2     | PAID      | This application has a commerce license but has not yet submitted a store approval request |
| 3     | SUBMITTED | This application has submitted a store approval request                                    |
| 4     | APPROVED  | This application has been approved for the store                                           |
| 5     | REJECTED  | This application has been rejected from the store                                          |

[**RPC Application State**](aplicaciones.md#rpc-application-state)

| Value | Name        | Description                                              |
| ----- | ----------- | -------------------------------------------------------- |
| 0     | DISABLED    | This application does not have access to RPC             |
| 1     | UNSUBMITTED | This application has not yet been applied for RPC access |
| 2     | SUBMITTED   | This application has submitted a RPC access request      |
| 3     | APPROVED    | This application has been approved for RPC access        |
| 4     | REJECTED    | This application has been rejected from RPC access       |

[**Application Discoverability State**](aplicaciones.md#application-discoverability-state)

| Value | Name              | Description                                                                   |
| ----- | ----------------- | ----------------------------------------------------------------------------- |
| 1     | INELIGIBLE        | This application is ineligible for the application directory                  |
| 2     | NOT\_DISCOVERABLE | This application is not listed in the application directory                   |
| 3     | DISCOVERABLE      | This application is listed in the application directory                       |
| 4     | FEATUREABLE       | This application is featurable in the application directory                   |
| 5     | BLOCKED           | This application has been blocked from appearing in the application directory |

[**Application Discovery Eligibility Flags**](aplicaciones.md#application-discovery-eligibility-flags)

| Value   | Name                      | Description                                                                                                                                                                                                            |
| ------- | ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1 << 0  | VERIFIED                  | Application is verified                                                                                                                                                                                                |
| 1 << 1  | TAG                       | Application has at least one tag set                                                                                                                                                                                   |
| 1 << 2  | DESCRIPTION               | Application has a description                                                                                                                                                                                          |
| 1 << 3  | TERMS\_OF\_SERVICE        | Application has terms of service set                                                                                                                                                                                   |
| 1 << 4  | PRIVACY\_POLICY           | Application has a privacy policy set                                                                                                                                                                                   |
| 1 << 5  | INSTALL\_PARAMS           | Application has a custom install URL or install parameters                                                                                                                                                             |
| 1 << 6  | SAFE\_NAME                | Application's name is safe for work                                                                                                                                                                                    |
| 1 << 7  | SAFE\_DESCRIPTION         | Application's description is safe for work                                                                                                                                                                             |
| 1 << 8  | APPROVED\_COMMANDS        | Application has the [message content intent](https://docs.discord.food/topics/gateway#message-content-intent) approved or utilizes [application commands](https://docs.discord.food/interactions/application-commands) |
| 1 << 9  | SUPPORT\_GUILD            | Application has a support guild set                                                                                                                                                                                    |
| 1 << 10 | SAFE\_COMMANDS            | Application's commands are safe for work                                                                                                                                                                               |
| 1 << 11 | MFA                       | Application's owner has MFA enabled                                                                                                                                                                                    |
| 1 << 12 | SAFE\_DIRECTORY\_OVERVIEW | Application's directory long description is safe for work                                                                                                                                                              |
| 1 << 13 | SUPPORTED\_LOCALES        | Application has at least one supported locale set                                                                                                                                                                      |
| 1 << 14 | SAFE\_SHORT\_DESCRIPTION  | Application's directory short description is safe for work                                                                                                                                                             |
| 1 << 15 | SAFE\_ROLE\_CONNECTIONS   | Application's role connections metadata is safe for work                                                                                                                                                               |

[**Application Monetization State**](aplicaciones.md#application-monetization-state)

| Value | Name    | Description                                        |
| ----- | ------- | -------------------------------------------------- |
| 1     | NONE    | This application does not have monetization set up |
| 2     | ENABLED | This application has monetization set up           |
| 3     | BLOCKED | This application has been blocked from monetizing  |

[**Creator Monetization State**](aplicaciones.md#creator-monetization-state)

The values of this enum are currently unknown. Help us by figuring them out and [submitting a pull request](https://github.com/discord-userdoccers/discord-userdoccers/edit/master/pages/resources/application.mdx)!

[**Application Monetization Eligibility Flags**](aplicaciones.md#application-monetization-eligibility-flags)

| Value   | Name                           | Description                                                                                                                                                                                                            |
| ------- | ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1 << 0  | VERIFIED                       | Application is verified                                                                                                                                                                                                |
| 1 << 1  | HAS\_TEAM                      | Application is owned by a team                                                                                                                                                                                         |
| 1 << 2  | APPROVED\_COMMANDS             | Application has the [message content intent](https://docs.discord.food/topics/gateway#message-content-intent) approved or utilizes [application commands](https://docs.discord.food/interactions/application-commands) |
| 1 << 3  | TERMS\_OF\_SERVICE             | Application has terms of service set                                                                                                                                                                                   |
| 1 << 4  | PRIVACY\_POLICY                | Application has a privacy policy set                                                                                                                                                                                   |
| 1 << 5  | SAFE\_NAME                     | Application's name is safe for work                                                                                                                                                                                    |
| 1 << 6  | SAFE\_DESCRIPTION              | Application's description is safe for work                                                                                                                                                                             |
| 1 << 7  | SAFE\_ROLE\_CONNECTIONS        | Application's role connections metadata is safe for work                                                                                                                                                               |
| 1 << 8  | USER\_IS\_TEAM\_OWNER          | User is the owner of the team that owns the application                                                                                                                                                                |
| 1 << 9  | NOT\_QUARANTINED               | Application is not quarantined                                                                                                                                                                                         |
| 1 << 10 | USER\_LOCALE\_SUPPORTED        | User's locale is supported by monetization                                                                                                                                                                             |
| 1 << 11 | USER\_AGE\_SUPPORTED           | User is old enough to use monetization                                                                                                                                                                                 |
| 1 << 12 | USER\_DATE\_OF\_BIRTH\_DEFINED | User has a date of birth defined on their account                                                                                                                                                                      |
| 1 << 13 | USER\_MFA\_ENABLED             | User has MFA enabled                                                                                                                                                                                                   |
| 1 << 14 | USER\_EMAIL\_VERIFIED          | User's email is verified                                                                                                                                                                                               |
| 1 << 15 | TEAM\_MEMBERS\_EMAIL\_VERIFIED | All members of the team that owns the application have verified emails                                                                                                                                                 |
| 1 << 16 | TEAM\_MEMBERS\_MFA\_ENABLED    | All members of the team that owns the application have MFA enabled                                                                                                                                                     |
| 1 << 17 | NO\_BLOCKING\_ISSUES           | This application has no issues blocking monetization                                                                                                                                                                   |
| 1 << 18 | VALID\_PAYOUT\_STATUS          | Owning team has a valid payout status                                                                                                                                                                                  |

[**Example Application**](aplicaciones.md#example-application)

```
{  "id": "891436243903728565",  "name": "Socket",  "icon": "26f3dcdc6e6371b52c384c812c30546c",  "description": "Socket",  "type": null,  "is_monetized": false,  "is_verified": false,  "is_discoverable": false,  "bot": {    "id": "891436243903728565",    "username": "Socket",    "avatar": "26f3dcdc6e6371b52c384c812c30546c",    "discriminator": "0001",    "public_flags": 0,    "bot": true,    "banner": null,    "accent_color": null,    "global_name": null,    "avatar_decoration_data": null,    "primary_guild": null  },  "deeplink_uri": "https://google.com/search?q=power+sockets+near+me",  "bot_public": true,  "bot_require_code_grant": false,  "verify_key": "852634a9ed80c0c5ac81e3c46d4b10a05400cb71898ea0484e7b63ac3a27096a",  "flags": 27828224,  "tags": ["60hz", "AC", "", "120v"],  "hook": true,  "storefront_available": false,  "redirect_uris": ["http://localhost:5000/callback"],  "interactions_endpoint_url": null,  "role_connections_verification_url": "https://google.com/search?q=power+sockets+near+me",  "owner": {    "id": "1110738998453837384",    "username": "team1110738998453837384",    "avatar": null,    "discriminator": "0000",    "public_flags": 1024,    "banner": null,    "accent_color": null,    "global_name": null,    "avatar_decoration_data": null,    "primary_guild": null  },  "approximate_guild_count": 100,  "approximate_user_install_count": 1000,  "interactions_event_types": [],  "interactions_version": 1,  "explicit_content_filter": 1,  "rpc_application_state": 0,  "store_application_state": 1,  "creator_monetization_state": 1,  "verification_state": 1,  "integration_public": true,  "integration_require_code_grant": false,  "discoverability_state": 1,  "discovery_eligibility_flags": 36294,  "monetization_state": 1,  "monetization_eligibility_flags": 0,  "publishers": [{ "id": "1058932127820939295", "name": "AlienTec" }],  "developers": [{ "id": "1058932084854509568", "name": "Alien Games" }],  "team": {    "id": "1110738998453837384",    "icon": null,    "name": "Power",    "owner_user_id": "852892297661906993",    "members": [      {        "user": {          "id": "852892297661906993",          "username": "dolfies",          "avatar": "c78ef8fb1db15a3d5f1b4c057856c5c9",          "discriminator": "0",          "public_flags": 136,          "banner": null,          "accent_color": null,          "global_name": "Dolfies",          "avatar_decoration_data": null,          "primary_guild": null        },        "team_id": "1110738998453837384",        "membership_state": 2,        "role": "admin"      }    ]  },  "internal_guild_restriction": 1}
```

[**Example Partial Application**](aplicaciones.md#example-partial-application)

```
{  "id": "880218394199220334",  "name": "Watch Together",  "icon": "ec48acbad4c32efab4275cb9f3ca3a58",  "description": "Create and watch a playlist of YouTube videos with your friends. Your choice to share the remote or not. ",  "type": null,  "is_monetized": false,  "is_verified": false,  "is_discoverable": false,  "cover_image": "3cc9446876ae9eec6e06ff565703c292",  "bot": {    "id": "880218394199220334",    "username": "Watch Together",    "avatar": "fe2b7fa334817b0346d57416ad75e93b",    "discriminator": "5319",    "public_flags": 0,    "bot": true,    "banner": null,    "accent_color": null,    "global_name": null,    "avatar_decoration_data": null,    "primary_guild": null  },  "summary": "",  "bot_public": false,  "bot_require_code_grant": false,  "terms_of_service_url": "https://discord.com/terms",  "privacy_policy_url": "https://discord.com/privacy",  "verify_key": "e2aaf50fbe2fd9d025ac669035f5efb89099931690fba9dc28efb7eaade7f96d",  "flags": 1179648,  "max_participants": -1,  "tags": ["Video Player", "Watch"],  "hook": true,  "storefront_available": false,  "embedded_activity_config": {    "activity_preview_video_asset_id": "1104184163201990836",    "supported_platforms": ["web", "ios", "android"],    "default_orientation_lock_state": 2,    "tablet_default_orientation_lock_state": 1,    "requires_age_gate": false,    "legacy_responsive_aspect_ratio": false,    "premium_tier_requirement": null,    "free_period_starts_at": null,    "free_period_ends_at": null,    "client_platform_config": {      "ios": { "label_type": 0, "label_until": null, "release_phase": "global_launch" },      "android": { "label_type": 0, "label_until": null, "release_phase": "global_launch" },      "web": { "label_type": 0, "label_until": null, "release_phase": "global_launch" }    },    "shelf_rank": 3,    "has_csp_exception": false,    "displays_advertisements": false  }}
```

#### [Application Executable Object](aplicaciones.md#application-executable-object) <a href="#application-executable-object" id="application-executable-object"></a>

[**Application Executable Structure**](aplicaciones.md#application-executable-structure)

| Field        | Type    | Description                                                                                                               |
| ------------ | ------- | ------------------------------------------------------------------------------------------------------------------------- |
| os           | string  | The [operating system](https://docs.discord.food/resources/presence#operating-system-type) the executable can be found on |
| name         | string  | The name of the executable                                                                                                |
| is\_launcher | boolean | Whether the executable is for a game launcher                                                                             |

[**Example Application Executable**](aplicaciones.md#example-application-executable)

```
{  "os": "win32",  "name": "spaceship looter/spaceship_looter.exe",  "is_launcher": false}
```

#### [Application SKU Object](aplicaciones.md#application-sku-object) <a href="#application-sku-object" id="application-sku-object"></a>

[**Application SKU Structure**](aplicaciones.md#application-sku-structure)

| Field       | Type    | Description                                                     |
| ----------- | ------- | --------------------------------------------------------------- |
| id          | ?string | The ID of the game                                              |
| sku         | ?string | The SKU of the game                                             |
| distributor | string  | The [distributor](aplicaciones.md#distributor-type) of the game |

[**Distributor Type**](aplicaciones.md#distributor-type)

| Value            | Description         |
| ---------------- | ------------------- |
| discord          | Discord Store       |
| steam            | Steam               |
| twitch           | Twitch              |
| uplay            | Ubisoft Connect     |
| battlenet        | Battle.net          |
| origin           | Origin              |
| gog              | GOG.com             |
| epic             | Epic Games Store    |
| google\_play     | Google Play Store   |
| nvidia\_gdn\_app | NVIDIA Cloud Gaming |

[**Example Application SKU**](aplicaciones.md#example-application-sku)

```
{  "id": "445220",  "sku": "445220",  "distributor": "steam"}
```

#### [Application Install Params Object](aplicaciones.md#application-install-params-object) <a href="#application-install-params-object" id="application-install-params-object"></a>

[**Application Install Params Structure**](aplicaciones.md#application-install-params-structure)

| Field       | Type           | Description                                                                                                           |
| ----------- | -------------- | --------------------------------------------------------------------------------------------------------------------- |
| scopes      | array\[string] | The [scopes](https://docs.discord.food/topics/oauth2#oauth2-scopes) to authorize the integration with                 |
| permissions | string         | The [permissions](https://docs.discord.food/topics/permissions) to request for the application's bot integration role |

[**Application Integration Type**](aplicaciones.md#application-integration-type)

An application's supported installation contexts.

| Value | Name           | Description                |
| ----- | -------------- | -------------------------- |
| 0     | GUILD\_INSTALL | Guild installation context |
| 1     | USER\_INSTALL  | User installation context  |

[**Application Integration Type Configuration Structure**](aplicaciones.md#application-integration-type-configuration-structure)

| Field                    | Type                                                                                      | Description                                                        |
| ------------------------ | ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| oauth2\_install\_params? | [application install params](aplicaciones.md#application-install-params-structure) object | The default in-app authorization link for the installation context |

[**Example Application Install Params**](aplicaciones.md#example-application-install-params)

```
{  "scopes": ["applications.commands", "bot"],  "permissions": "8"}
```

#### [Application Proxy Config Object](aplicaciones.md#application-proxy-config-object) <a href="#application-proxy-config-object" id="application-proxy-config-object"></a>

The application proxy makes it possible to proxy requests to a domain through the Discord activity proxy. This is used by embedded activities to be able to make requests without being blocked by Discord's content security policy (CSP).

Mapped URLs are available at .

[**Application Proxy Config Structure**](aplicaciones.md#application-proxy-config-structure)

| Field    | Type                                                                                            | Description                  |
| -------- | ----------------------------------------------------------------------------------------------- | ---------------------------- |
| url\_map | array\[[application proxy mapping](aplicaciones.md#application-proxy-mapping-structure) object] | The URLs mapped to the proxy |

[**Application Proxy Mapping Structure**](aplicaciones.md#application-proxy-mapping-structure)

| Field  | Type   | Description             |
| ------ | ------ | ----------------------- |
| prefix | string | The prefix on the proxy |
| target | string | The domain to proxy     |

[**Example Application Proxy Config**](aplicaciones.md#example-application-proxy-config)

```
{  "url_map": [    {      "prefix": "/api",      "target": "api.example.com"    }  ]}
```

#### [Embedded Activity Config Object](aplicaciones.md#embedded-activity-config-object) <a href="#embedded-activity-config-object" id="embedded-activity-config-object"></a>

[**Embedded Activity Config Structure**](aplicaciones.md#embedded-activity-config-structure)

| Field                                       | Type                                                                                                                  | Description                                                                                                                     |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| application\_id? <sup>1</sup>               | snowflake                                                                                                             | The ID of the application this embedded activity is for                                                                         |
| activity\_preview\_video\_asset\_id         | ?snowflake                                                                                                            | The ID of the application asset to preview the activity with                                                                    |
| supported\_platforms                        | array\[string]                                                                                                        | The [platforms this activity is supported on](aplicaciones.md#embedded-activity-platform-type)                                  |
| default\_orientation\_lock\_state           | integer                                                                                                               | The default [orientation lock state](aplicaciones.md#embedded-activity-orientation-lock-state-type) for the activity on mobile  |
| tablet\_default\_orientation\_lock\_state   | integer                                                                                                               | The default [orientation lock state](aplicaciones.md#embedded-activity-orientation-lock-state-type) for the activity on tablets |
| requires\_age\_gate                         | boolean                                                                                                               | Whether the activity is age gated                                                                                               |
| legacy\_responsive\_aspect\_ratio           | boolean                                                                                                               | Whether the activity uses a responsive aspect ratio instead of a dynamic aspect ratio                                           |
| premium\_tier\_requirement **(deprecated)** | ?integer                                                                                                              | The minimum [guild premium tier](https://docs.discord.food/resources/guild#premium-tier) required to use the activity, if any   |
| free\_period\_starts\_at **(deprecated)**   | ?ISO8601 timestamp                                                                                                    | When the current free period for the activity starts, if any                                                                    |
| free\_period\_ends\_at **(deprecated)**     | ?ISO8601 timestamp                                                                                                    | When the current free period for the activity ends, if any                                                                      |
| client\_platform\_config                    | map\[string, [embedded activity platform config](aplicaciones.md#embedded-activity-platform-config-structure) object] | The release configuration for the activity on each [platform](aplicaciones.md#embedded-activity-platform-type)                  |
| shelf\_rank                                 | integer                                                                                                               | The rank of the activity in the activity shelf sort order                                                                       |
| has\_csp\_exception                         | boolean                                                                                                               | Whether the activity is not routed through the Discord activity proxy                                                           |
| displays\_advertisements                    | boolean                                                                                                               | Whether the activity displays advertisements                                                                                    |

<sup>1</sup> Omitted in the [application object](aplicaciones.md#application-object).

[**Embedded Activity Orientation Lock State Type**](aplicaciones.md#embedded-activity-orientation-lock-state-type)

| Value | Name      | Description              |
| ----- | --------- | ------------------------ |
| 1     | UNLOCKED  | Unrestricted orientation |
| 2     | PORTRAIT  | Portrait only            |
| 3     | LANDSCAPE | Landscape only           |

[**Embedded Activity Platform Type**](aplicaciones.md#embedded-activity-platform-type)

| Value   | Description |
| ------- | ----------- |
| web     | Web         |
| android | Android     |
| ios     | iOS         |

[**Embedded Activity Platform Config Structure**](aplicaciones.md#embedded-activity-platform-config-structure)

| Field                       | Type               | Description                                                                                |
| --------------------------- | ------------------ | ------------------------------------------------------------------------------------------ |
| label\_type                 | integer            | The [type of release label](aplicaciones.md#embedded-activity-label-type) for the platform |
| label\_until?               | ?ISO8601 timestamp | When the release label expires                                                             |
| release\_phase              | string             | The [release phase](aplicaciones.md#embedded-activity-release-phase) for the platform      |
| omit\_badge\_from\_surfaces | array\[string]     | The [surfaces](aplicaciones.md#embedded-activity-surface) to omit the activity badge from  |

[**Embedded Activity Label Type**](aplicaciones.md#embedded-activity-label-type)

| Value | Name    | Description                            |
| ----- | ------- | -------------------------------------- |
| 0     | NONE    | No special label                       |
| 1     | NEW     | The activity is new                    |
| 2     | UPDATED | The activity has been recently updated |

[**Embedded Activity Release Phase**](aplicaciones.md#embedded-activity-release-phase)

| Value                    | Description                                                                                                            |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| in\_development          | The activity is still in development                                                                                   |
| activities\_team         | The activity is available to guilds with the [guild feature](https://docs.discord.food/resources/guild#guild-features) |
| employee\_release        | The activity is available to guilds with the [guild feature](https://docs.discord.food/resources/guild#guild-features) |
| soft\_launch             | The activity is available to all guilds                                                                                |
| soft\_launch\_multi\_geo | The activity is available to all guilds                                                                                |
| global\_launch           | The activity is available to all guilds                                                                                |

[**Embedded Activity Surface**](aplicaciones.md#embedded-activity-surface)

| Value           | Description                                          |
| --------------- | ---------------------------------------------------- |
| voice\_launcher | The activity launcher in the voice channel interface |
| text\_launcher  | The activity launcher in the text channel interface  |

[**Example Embedded Activity Config**](aplicaciones.md#example-embedded-activity-config)

```
{  "activity_preview_video_asset_id": "1104184163201990836",  "supported_platforms": ["web", "ios", "android"],  "default_orientation_lock_state": 2,  "tablet_default_orientation_lock_state": 1,  "requires_age_gate": false,  "legacy_responsive_aspect_ratio": false,  "premium_tier_requirement": null,  "free_period_starts_at": null,  "free_period_ends_at": null,  "client_platform_config": {    "android": {      "label_type": 0,      "label_until": null,      "release_phase": "global_launch",      "omit_badge_from_surfaces": []    },    "ios": {      "label_type": 0,      "label_until": null,      "release_phase": "global_launch",      "omit_badge_from_surfaces": []    },    "web": {      "label_type": 0,      "label_until": null,      "release_phase": "global_launch",      "omit_badge_from_surfaces": []    }  },  "shelf_rank": 3,  "has_csp_exception": false,  "displays_advertisements": false,  "application_id": "880218394199220334"}
```

#### [Application Asset Object](aplicaciones.md#application-asset-object) <a href="#application-asset-object" id="application-asset-object"></a>

[**Application Asset Structure**](aplicaciones.md#application-asset-structure)

| Field | Type    | Description                                                     |
| ----- | ------- | --------------------------------------------------------------- |
| id    | string  | The ID of the asset                                             |
| type  | integer | The [type of the asset](aplicaciones.md#application-asset-type) |
| name  | string  | The name of the asset                                           |

[**Application Asset Type**](aplicaciones.md#application-asset-type)

| Value | Name | Description |
| ----- | ---- | ----------- |
| 1     | ONE  | Unknown     |
| 2     | TWO  | Unknown     |

[**Example Application Asset**](aplicaciones.md#example-application-asset)

```
{  "id": "1131721726514954381",  "type": 1,  "name": "alien"}
```

#### [Application Role Connection Object](aplicaciones.md#application-role-connection-object) <a href="#application-role-connection-object" id="application-role-connection-object"></a>

The role connection object that an application has attached to a user.

[**Application Role Connection Structure**](aplicaciones.md#application-role-connection-structure)

| Field                  | Type                                                                                                               | Description                                                                                                                                                                                                                 |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| platform\_name         | ?string                                                                                                            | The vanity name of the platform a bot has connected (max 50 characters)                                                                                                                                                     |
| platform\_username     | ?string                                                                                                            | The username on the platform a bot has connected (max 100 characters)                                                                                                                                                       |
| metadata               | object                                                                                                             | Object mapping [application role connection metadata](aplicaciones.md#application-role-connection-metadata-object) keys to their `string`-ified value (max 100 characters) for the user on the platform a bot has connected |
| application?           | [integration application](https://docs.discord.food/resources/integration#integration-application-object) object   | The application that owns the role connection                                                                                                                                                                               |
| application\_metadata? | array\[[application role connection metadata](aplicaciones.md#application-role-connection-metadata-object) object] | The metadata that the application has set for the role connection                                                                                                                                                           |

#### [Application Role Connection Metadata Object](aplicaciones.md#application-role-connection-metadata-object) <a href="#application-role-connection-metadata-object" id="application-role-connection-metadata-object"></a>

A representation of role connection metadata for an [application](aplicaciones.md#application-object).

When a guild has added an application integration and that integration has configured its, the application will render as a potential verification method in the guild's role verification configuration.

If an application has configured role connection metadata, its metadata will appear in the role verification configuration when the application has been added as a verification method to the role.

When a user connects their account using the integration's, the integration will [update a user's role connection with metadata](aplicaciones.md#modify-user-application-role-connection) using the OAuth2 scope.

[**Application Role Connection Metadata Structure**](aplicaciones.md#application-role-connection-metadata-structure)

| Field                       | Type           | Description                                                                                                   |
| --------------------------- | -------------- | ------------------------------------------------------------------------------------------------------------- |
| type                        | integer        | The [type of metadata value](https://docs.discord.food/resources/guild#role-connection-operator-type)         |
| key                         | string         | Key for the metadata field (1-50 characters, must be `a-z`, `0-9`, or `_`)                                    |
| name                        | string         | The name of the metadata field (1-100 characters)                                                             |
| name\_localizations?        | map\[str, str] | Translations of the name with keys in [available locales](https://docs.discord.food/reference#locales)        |
| description                 | string         | The description of the metadata field (1-200 characters)                                                      |
| description\_localizations? | map\[str, str] | Translations of the description with keys in [available locales](https://docs.discord.food/reference#locales) |

#### [Activity Link Object](aplicaciones.md#activity-link-object) <a href="#activity-link-object" id="activity-link-object"></a>

[**Activity Link Structure**](aplicaciones.md#activity-link-structure)

| Field                          | Type      | Description                                      |
| ------------------------------ | --------- | ------------------------------------------------ |
| application\_id                | snowflake | The application ID                               |
| link\_id <sup>1</sup>          | string    | The link ID                                      |
| asset\_path? <sup>2</sup>      | string    | The hash of the application quick link asset     |
| asset\_id? <sup>2</sup>        | snowflake | The ID of the application asset                  |
| title                          | string    | The title of the activity link                   |
| description                    | string    | The description of the activity link             |
| custom\_id?                    | ?string   | A custom ID for the activity link                |
| primary\_cta? **(deprecated)** | ?string   | The primary call to action for the activity link |

<sup>1</sup> The link ID is in the format where is the [activity link type](aplicaciones.md#activity-link-type) and is the snowflake ID.

<sup>2</sup> is only present on quick links, while is only present on managed links.

[**Activity Link Type**](aplicaciones.md#activity-link-type)

| Value | Name          | Description                                      |
| ----- | ------------- | ------------------------------------------------ |
| 0     | MANAGED\_LINK | Managed by the application and last indefinitely |
| 1     | QUICK\_LINK   | Made by the user and last for 30 days            |

[**Example Activity Link**](aplicaciones.md#example-activity-link)

```
{  "application_id": "891436233903964161",  "link_id": "1-1385320255148003439",  "asset_path": "74420086a1aee57564cd6fc9a28461b1",  "title": "Alien",  "description": "aliens",  "primary_cta": null,  "custom_id": "button"}
```

### [Endpoints](aplicaciones.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get Applications](aplicaciones.md#get-applications) <a href="#get-applications" id="get-applications"></a>

`GET/applications`

Returns a list of [application](aplicaciones.md#application-object) objects that the current user has.

[**Query String Params**](aplicaciones.md#query-string-params)

| Field                     | Type    | Description                                                                                                        |
| ------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------ |
| with\_team\_applications? | boolean | Whether to include applications that a [team](https://docs.discord.food/resources/team) the user is a part of owns |

#### [Get Applications with Assets](aplicaciones.md#get-applications-with-assets) <a href="#get-applications-with-assets" id="get-applications-with-assets"></a>

`GET/applications-with-assets`

Returns a list of [application](aplicaciones.md#application-object) objects that the current user has, additionally including the application's assets.

[**Query String Params**](aplicaciones.md#query-string-params)

| Field                     | Type    | Description                                                                                                        |
| ------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------ |
| with\_team\_applications? | boolean | Whether to include applications that a [team](https://docs.discord.food/resources/team) the user is a part of owns |

[**Response Body**](aplicaciones.md#response-body)

| Field        | Type                                                                                          | Description                     |
| ------------ | --------------------------------------------------------------------------------------------- | ------------------------------- |
| applications | array\[[application](aplicaciones.md#application-object)]                                     | The applications the user has   |
| assets       | map\[snowflake, array\[[application asset](aplicaciones.md#application-asset-object) object]] | The assets for each application |

[**Example Response**](aplicaciones.md#example-response)

```
{  "applications": [    {      "id": "891436243903728565",      "name": "Lightbulb",      "icon": "546242649e3b09a97af7e8f29983837b",      "description": "💡 Let there be light",      "summary": "",      "type": null,      "is_monetized": false,      "is_verified": false,      "is_discoverable": false,      "cover_image": "75bc61df60fc74c46b32fde3532f662b",      "deeplink_uri": "https://google.com/search?q=lightbulbs+near+me",      "hook": true,      "guild_id": "1029315212005888060",      "storefront_available": false,      "bot_public": true,      "bot_require_code_grant": false,      "terms_of_service_url": "https://google.com/search?q=lightbulbs+near+me",      "privacy_policy_url": "https://google.com/search?q=lightbulbs+near+me",      "integration_types_config": {        "0": {},        "1": {}      },      "verify_key": "852634a9ed80c0c5ac81e3c46d4b10a05400cb71898ea0484e7b63ac3a27096a",      "owner": {        "id": "1110738998453837384",        "username": "team1110738998453837384",        "global_name": null,        "avatar": null,        "avatar_decoration_data": null,        "discriminator": "0000",        "public_flags": 1024,        "primary_guild": null,        "flags": 1024      },      "flags": 27959296,      "redirect_uris": ["http://localhost:5000/callback"],      "rpc_application_state": 0,      "store_application_state": 1,      "verification_state": 1,      "interactions_endpoint_url": null,      "interactions_event_types": [],      "interactions_version": 1,      "integration_public": true,      "integration_require_code_grant": false,      "explicit_content_filter": 1,      "discoverability_state": 1,      "discovery_eligibility_flags": 36830,      "monetization_state": 1,      "role_connections_verification_url": "https://google.com/search?q=lightbulbs+near+me",      "internal_guild_restriction": 1,      "bot": {        "id": "891436243903728565",        "username": "Lightbulb",        "global_name": null,        "avatar": "546242649e3b09a97af7e8f29983837b",        "avatar_decoration_data": null,        "discriminator": "5312",        "public_flags": 0,        "primary_guild": null,        "bot": true      },      "approximate_guild_count": 100,      "approximate_user_install_count": 1000,      "max_participants": -1,      "embedded_activity_config": {        "activity_preview_video_asset_id": "1131721726514954381",        "supported_platforms": ["web", "android", "ios"],        "default_orientation_lock_state": 1,        "tablet_default_orientation_lock_state": 1,        "requires_age_gate": false,        "legacy_responsive_aspect_ratio": false,        "premium_tier_requirement": null,        "free_period_starts_at": null,        "free_period_ends_at": null,        "client_platform_config": {          "web": {            "label_type": 0,            "label_until": null,            "release_phase": "in_development"          },          "android": {            "label_type": 0,            "label_until": null,            "release_phase": "in_development"          },          "ios": {            "label_type": 0,            "label_until": null,            "release_phase": "in_development"          }        },        "shelf_rank": 2147483647,        "has_csp_exception": false,        "displays_advertisements": false      },      "tags": ["", "100W", "EnergyStar", "LED"]    }  ],  "assets": {    "891436243903728565": [      {        "id": "1223782285833273507",        "type": 1,        "name": "embedded_background"      },      {        "id": "1223782287091564634",        "type": 1,        "name": "embedded_cover"      }    ]  }}
```

#### [Create Application](aplicaciones.md#create-application) <a href="#create-application" id="create-application"></a>

`POST/applications`

Creates a new application. Returns an [application](aplicaciones.md#application-object) object on success. Users can have a maximum of 50 applications, with each team able to have a maximum of 25.

[**JSON Params**](aplicaciones.md#json-params)

| Field           | Type                                                        | Description                                                                                                                                                                       |
| --------------- | ----------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name            | string                                                      | The name of the application                                                                                                                                                       |
| type?           | integer                                                     | The [type of the application](aplicaciones.md#application-type) (only `CREATOR_MONETIZATION` is supported)                                                                        |
| team\_id?       | snowflake                                                   | The ID of the [team](https://docs.discord.food/resources/team) to create this application under                                                                                   |
| description?    | ?string                                                     | The description of the application                                                                                                                                                |
| icon?           | ?[image data](https://docs.discord.food/reference#cdn-data) | The application's icon                                                                                                                                                            |
| cover\_image?   | ?[image data](https://docs.discord.food/reference#cdn-data) | The application's default rich presence invite cover image                                                                                                                        |
| flags?          | integer                                                     | the [application's flags](aplicaciones.md#application-flags) (only `GATEWAY_GUILD_MEMBERS_LIMITED`, `GATEWAY_PRESENCE_LIMITED`, and `GATEWAY_MESSAGE_CONTENT_LIMITED` can be set) |
| guild\_id?      | ?snowflake                                                  | The ID of the guild linked to the application                                                                                                                                     |
| redirect\_uris? | ?array\[string]                                             | The whitelisted URLs for redirecting to during [OAuth2 authorization](https://docs.discord.food/topics/oauth2) (max 10)                                                           |
| deeplink\_uri?  | ?string                                                     | The URL used for deep linking during [OAuth2 authorization](https://docs.discord.food/topics/oauth2) on mobile devices                                                            |

#### [Get Application](aplicaciones.md#get-application) <a href="#get-application" id="get-application"></a>

`GET/applications/{application.id}`

Returns an [application](aplicaciones.md#application-object) object for the given ID. User must be the owner of the application or member of the current team.

#### [Get Current Application](aplicaciones.md#get-current-application) <a href="#get-current-application" id="get-current-application"></a>

`GET/applications/@me`

Returns the [application](aplicaciones.md#application-object) object associated with the requestor.

#### [Modify Application](aplicaciones.md#modify-application) <a href="#modify-application" id="modify-application"></a>

`PATCH/applications/{application.id}`

Modifies an application. User must be the owner of the application or developer of the current team. Returns the updated [application](aplicaciones.md#application-object) object on success.

[**JSON Params**](aplicaciones.md#json-params)

| Field                                       | Type                                                                                                                                      | Description                                                                                                                                                                                               |
| ------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name?                                       | string                                                                                                                                    | The name of the application                                                                                                                                                                               |
| description?                                | ?string                                                                                                                                   | The description of the application                                                                                                                                                                        |
| icon?                                       | ?[image data](https://docs.discord.food/reference#cdn-data)                                                                               | The application's icon                                                                                                                                                                                    |
| cover\_image?                               | ?[image data](https://docs.discord.food/reference#cdn-data)                                                                               | The application's default rich presence invite cover image                                                                                                                                                |
| flags?                                      | integer                                                                                                                                   | The [application's flags](aplicaciones.md#application-flags) (only `PUBLIC_OAUTH2_CLIENT`, `GATEWAY_GUILD_MEMBERS_LIMITED`, `GATEWAY_PRESENCE_LIMITED`, and `GATEWAY_MESSAGE_CONTENT_LIMITED` can be set) |
| guild\_id?                                  | ?snowflake                                                                                                                                | The ID of the guild linked to the application                                                                                                                                                             |
| developer\_ids?                             | ?array\[snowflake]                                                                                                                        | The IDs of the companies that developed the application                                                                                                                                                   |
| publisher\_ids?                             | ?array\[snowflake]                                                                                                                        | The IDs of the companies that published the application                                                                                                                                                   |
| rpc\_origins?                               | ?array\[string]                                                                                                                           | The whitelisted RPC origin URLs for the application, if RPC is enabled                                                                                                                                    |
| redirect\_uris?                             | ?array\[string]                                                                                                                           | The whitelisted URLs for redirecting to during [OAuth2 authorization](https://docs.discord.food/topics/oauth2) (max 10)                                                                                   |
| deeplink\_uri?                              | ?string                                                                                                                                   | The URL used for deep linking during [OAuth2 authorization](https://docs.discord.food/topics/oauth2) on mobile devices                                                                                    |
| integration\_public?                        | boolean                                                                                                                                   | Whether only the application owner can add the integration                                                                                                                                                |
| integration\_require\_code\_grant?          | boolean                                                                                                                                   | Whether the integration will only be added upon completion of a full OAuth2 token exchange                                                                                                                |
| bot\_public? **(deprecated)**               | boolean                                                                                                                                   | Whether only the application owner can add the bot                                                                                                                                                        |
| bot\_require\_code\_grant? **(deprecated)** | boolean                                                                                                                                   | Whether the application's bot will only be added upon completion of a full OAuth2 token exchange                                                                                                          |
| terms\_of\_service\_url?                    | ?string                                                                                                                                   | The URL to the application's terms of service                                                                                                                                                             |
| privacy\_policy\_url?                       | ?string                                                                                                                                   | The URL to the application's privacy policy                                                                                                                                                               |
| role\_connections\_verification\_url        | ?string                                                                                                                                   | The role connection verification entry point of the integration; when configured, this will render the application as a verification method in guild role verification configuration                      |
| interactions\_endpoint\_url?                | string                                                                                                                                    | The URL of the application's [interactions endpoint](https://docs.discord.food/interactions/receiving-and-responding#receiving-an-interaction)                                                            |
| interactions\_version?                      | integer                                                                                                                                   | The [version of the application's interactions endpoint implementation](aplicaciones.md#application-interactions-version)                                                                                 |
| interactions\_event\_types? <sup>1</sup>    | ?array\[string]                                                                                                                           | The enabled [Gateway events](https://docs.discord.food/topics/gateway-events) to send to the interaction endpoint                                                                                         |
| explicit\_content\_filter?                  | integer                                                                                                                                   | [Whether uploaded media content](aplicaciones.md#explicit-content-filter-level) used in application commands is scanned and deleted for explicit content                                                  |
| tags?                                       | ?array\[string]                                                                                                                           | Tags describing the content and functionality of the application (max 20 characters, max 5)                                                                                                               |
| install\_params?                            | ?[application install params](aplicaciones.md#application-install-params-object) object                                                   | The default in-app authorization link for the integration                                                                                                                                                 |
| custom\_install\_url?                       | ?string                                                                                                                                   | The default custom authorization link for the integration                                                                                                                                                 |
| integration\_types\_config?                 | map\[integer, ?[application integration type configuration](aplicaciones.md#application-integration-type-configuration-structure) object] | The configuration for each [integration type](aplicaciones.md#application-integration-type) supported by the application                                                                                  |
| discoverability\_state?                     | integer                                                                                                                                   | The current [application directory discoverability state](aplicaciones.md#application-discoverability-state) of the application (only `NOT_DISCOVERABLE` and `DISCOVERABLE` is supported)                 |
| monetization\_state?                        | integer                                                                                                                                   | The current [application monetization state](aplicaciones.md#application-monetization-state) of the application (only `NONE` and `ENABLED` is supported)                                                  |
| max\_participants?                          | ?integer                                                                                                                                  | The maximum possible participants in the application's embedded activity (-1 for no limit)                                                                                                                |

<sup>1</sup> The sending of Gateway events over the interactions endpoint requires [interactions version 2](aplicaciones.md#application-interactions-version).

#### [Modify Current Application](aplicaciones.md#modify-current-application) <a href="#modify-current-application" id="modify-current-application"></a>

`PATCH/applications/@me`

Modifies the requestor's application information. Returns the updated [application](aplicaciones.md#application-object) object on success.

[**JSON Params**](aplicaciones.md#json-params)

| Field                                    | Type                                                                                                                                      | Description                                                                                                                                                                          |
| ---------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| description?                             | ?string                                                                                                                                   | The description of the application                                                                                                                                                   |
| icon?                                    | ?[image data](https://docs.discord.food/reference#cdn-data)                                                                               | The application's icon                                                                                                                                                               |
| cover\_image?                            | ?[image data](https://docs.discord.food/reference#cdn-data)                                                                               | The application's default rich presence invite cover image                                                                                                                           |
| flags?                                   | integer                                                                                                                                   | The [application's flags](aplicaciones.md#application-flags) (only `GATEWAY_GUILD_MEMBERS_LIMITED`, `GATEWAY_PRESENCE_LIMITED`, and `GATEWAY_MESSAGE_CONTENT_LIMITED` can be set)    |
| rpc\_origins?                            | ?array\[string]                                                                                                                           | The whitelisted RPC origin URLs for the application, if RPC is enabled                                                                                                               |
| deeplink\_uri?                           | ?string                                                                                                                                   | The URL used for deep linking during [OAuth2 authorization](https://docs.discord.food/topics/oauth2) on mobile devices                                                               |
| role\_connections\_verification\_url?    | ?string                                                                                                                                   | The role connection verification entry point of the integration; when configured, this will render the application as a verification method in guild role verification configuration |
| interactions\_endpoint\_url?             | ?string                                                                                                                                   | The URL of the application's [interactions endpoint](https://docs.discord.food/interactions/receiving-and-responding#receiving-an-interaction)                                       |
| interactions\_version?                   | integer                                                                                                                                   | The [version of the application's interactions endpoint implementation](aplicaciones.md#application-interactions-version)                                                            |
| interactions\_event\_types? <sup>1</sup> | ?array\[string]                                                                                                                           | The enabled [Gateway events](https://docs.discord.food/topics/gateway-events) to send to the interaction endpoint                                                                    |
| explicit\_content\_filter?               | integer                                                                                                                                   | [Whether uploaded media content](aplicaciones.md#explicit-content-filter-level) used in application commands is scanned and deleted for explicit content                             |
| tags?                                    | ?array\[string]                                                                                                                           | Tags describing the content and functionality of the application (max 20 characters, max 5)                                                                                          |
| install\_params?                         | ?[application install params](aplicaciones.md#application-install-params-object) object                                                   | The default in-app authorization link for the integration                                                                                                                            |
| custom\_install\_url?                    | ?string                                                                                                                                   | The default custom authorization link for the integration                                                                                                                            |
| integration\_types\_config?              | map\[integer, ?[application integration type configuration](aplicaciones.md#application-integration-type-configuration-structure) object] | The configuration for each [integration type](aplicaciones.md#application-integration-type) supported by the application                                                             |
| max\_participants?                       | ?integer                                                                                                                                  | The maximum possible participants in the application's embedded activity (-1 for no limit)                                                                                           |

#### [Delete Application](aplicaciones.md#delete-application) <a href="#delete-application" id="delete-application"></a>

`POST/applications/{application.id}/delete`

Deletes an application permanently. User must be the owner of the application or current team. Returns a 204 empty response on success.

#### [Transfer Application](aplicaciones.md#transfer-application) <a href="#transfer-application" id="transfer-application"></a>

`POST/applications/{application.id}/transfer`

Transfers ownership of an application to a [team](https://docs.discord.food/resources/team). User must be the owner of the application or current team. Returns an [application](aplicaciones.md#application-object) object on success.

[**JSON Params**](aplicaciones.md#json-params)

| Field    | Type      | Description                                 |
| -------- | --------- | ------------------------------------------- |
| team\_id | snowflake | The ID of the team to transfer ownership to |

#### [Reset Application Secret](aplicaciones.md#reset-application-secret) <a href="#reset-application-secret" id="reset-application-secret"></a>

`POST/applications/{application.id}/reset`

Resets the application's client secret. This revokes all previous secrets and returns a new secret. User must be the owner of the application or developer of the current team.

[**Response Body**](aplicaciones.md#response-body)

| Field  | Type   | Description                              |
| ------ | ------ | ---------------------------------------- |
| secret | string | The client secret key of the application |

[**Example Response**](aplicaciones.md#example-response)

```
{  "secret": "937it3ow87i4ery69876wqire"}
```

#### [Get Application Testers](aplicaciones.md#get-application-testers) <a href="#get-application-testers" id="get-application-testers"></a>

`GET/oauth2/applications/{application.id}/allowlist`

Returns a list of [whitelisted user](aplicaciones.md#whitelisted-user-structure) objects representing the invited testers for the given application ID. User must be the owner of the application or member of the current team.

[**Whitelisted User Structure**](aplicaciones.md#whitelisted-user-structure)

| Field | Type                                                                        | Description                                                                       |
| ----- | --------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| user  | partial [user](https://docs.discord.food/resources/user#user-object) object | The user that is whitelisted for the application                                  |
| state | integer                                                                     | The [state of the whitelisted user](aplicaciones.md#application-membership-state) |

[**Application Membership State**](aplicaciones.md#application-membership-state)

| Value | Name     | Description                                                                |
| ----- | -------- | -------------------------------------------------------------------------- |
| 1     | INVITED  | The user has been invited to the application but has not yet accepted      |
| 2     | ACCEPTED | The user has accepted the invitation to the application and is whitelisted |

#### [Add Application Tester](aplicaciones.md#add-application-tester) <a href="#add-application-tester" id="add-application-tester"></a>

`POST/oauth2/applications/{application.id}/allowlist`

Adds a user to the application's list of testers. User must be the owner of the application or developer of the current team. Returns a [whitelisted user](aplicaciones.md#whitelisted-user-structure) object on success.

[**JSON Params**](aplicaciones.md#json-params)

| Field                       | Type    | Description                          |
| --------------------------- | ------- | ------------------------------------ |
| username                    | string  | The username of the user to add      |
| discriminator? <sup>1</sup> | ?string | The discriminator of the user to add |

<sup>1</sup> Not applicable for migrated users. See the [section on Discord's new username system](https://docs.discord.food/resources/user#unique-usernames) for more information.

#### [Accept Application Tester Invitation](aplicaciones.md#accept-application-tester-invitation) <a href="#accept-application-tester-invitation" id="accept-application-tester-invitation"></a>

`POST/oauth2/allowlist/accept`

Accepts an application tester invitation received via email. Invited users will receive an email with a link that redirects to the official Discord client with a verification token present in the URL's query (e.g. ). Returns a 204 empty response on success.

[**Query String Params**](aplicaciones.md#query-string-params)

| Field | Type   | Description                         |
| ----- | ------ | ----------------------------------- |
| token | string | The verification token from the URL |

#### [Remove Application Tester](aplicaciones.md#remove-application-tester) <a href="#remove-application-tester" id="remove-application-tester"></a>

`DELETE/oauth2/applications/{application.id}/allowlist/{user.id}`

Removes a user from the application's list of testers. User must be the owner of the application or developer of the current team. Returns a 204 empty response on success.

#### [Create Application Bot](aplicaciones.md#create-application-bot) <a href="#create-application-bot" id="create-application-bot"></a>

`POST/applications/{application.id}/bot`

Creates and attaches a bot to the given application ID. User must be the owner of the application or developer of the current team.

[**Response Body**](aplicaciones.md#response-body)

| Field | Type    | Description                                      |
| ----- | ------- | ------------------------------------------------ |
| token | ?string | The token of the bot, if a bot was newly created |

[**Example Response**](aplicaciones.md#example-response)

```
{  "token": "NzIyNDUwMzAzOTE5NTg3NDA5.GRj2Bt.cPbrvvjxglZXK4dTcIPDMvfq0LxJcilsIYW01A"}
```

#### [Modify Application Bot](aplicaciones.md#modify-application-bot) <a href="#modify-application-bot" id="modify-application-bot"></a>

`PATCH/applications/{application.id}/bot`

Modifies the application's bot. User must be the owner of the application or developer of the current team. Returns the updated [user](https://docs.discord.food/resources/user#user-object) object on success.

[**JSON Params**](aplicaciones.md#json-params)

| Field     | Type                                                        | Description                           |
| --------- | ----------------------------------------------------------- | ------------------------------------- |
| username? | string                                                      | The user's username (2-32 characters) |
| avatar?   | ?[image data](https://docs.discord.food/reference#cdn-data) | The user's avatar                     |
| banner?   | ?[image data](https://docs.discord.food/reference#cdn-data) | The user's banner                     |

#### [Reset Application Bot Token](aplicaciones.md#reset-application-bot-token) <a href="#reset-application-bot-token" id="reset-application-bot-token"></a>

`POST/applications/{application.id}/bot/reset`

Resets the application's bot token. This revokes all previous tokens and returns a new token. User must be the owner of the application or developer of the current team.

[**Response Body**](aplicaciones.md#response-body)

| Field | Type   | Description          |
| ----- | ------ | -------------------- |
| token | string | The token of the bot |

#### [Request Application Gateway Intents](aplicaciones.md#request-application-gateway-intents) <a href="#request-application-gateway-intents" id="request-application-gateway-intents"></a>

`POST/applications/{application.id}/request-additional-intents`

Submits a request for Gateway intents for a verified bot. User must be the owner of the application or developer of the current team. Returns a 204 empty response on success.

[**JSON Params**](aplicaciones.md#json-params)

| Field                                                                                            | Type     | Description                                                                                                                                                                                           |
| ------------------------------------------------------------------------------------------------ | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| application\_description                                                                         | string   | The description of the application (50-2000 characters)                                                                                                                                               |
| intents\_flags\_requested?                                                                       | integer  | The [application flags](aplicaciones.md#application-flags) representing the requested Gateway intents (only `GATEWAY_PRESENCE`, `GATEWAY_GUILD_MEMBERS`, and `GATEWAY_MESSAGE_CONTENT` are supported) |
| intents\_gateway\_presence\_use\_case\_description? <sup>1</sup>                                 | ?string  | The use case for requesting the presence intent (50-2000 characters)                                                                                                                                  |
| intents\_gateway\_presence\_use\_case\_supplemental\_material\_description? <sup>1</sup>         | ?string  | The supplemental material for the requested Gateway presence intent (5-2000 characters)                                                                                                               |
| intents\_gateway\_presence\_store\_off\_platform? <sup>1</sup>                                   | ?boolean | Whether the application stores presence data off-platform                                                                                                                                             |
| intents\_gateway\_presence\_retention?                                                           | ?boolean | Whether the application retains presence data for 30 days or less                                                                                                                                     |
| intents\_gateway\_presence\_encrypted?                                                           | ?boolean | Whether the application encrypts stored presence data at rest                                                                                                                                         |
| intents\_gateway\_presence\_opt\_out\_stored?                                                    | ?boolean | Whether application users can opt out of having their presence data stored                                                                                                                            |
| intents\_gateway\_presence\_contact\_deletion? <sup>1</sup>                                      | ?string  | How application users can request the deletion of their presence data (25-2000 characters)                                                                                                            |
| intents\_gateway\_guild\_members\_use\_case\_description? <sup>1</sup>                           | ?string  | The use case for requesting the guild members intent (50-2000 characters)                                                                                                                             |
| intents\_gateway\_guild\_members\_use\_case\_supplemental\_material\_description? <sup>1</sup>   | ?string  | The supplemental material for the requested Gateway guild members intent (5-2000 characters)                                                                                                          |
| intents\_gateway\_guild\_members\_store\_off\_platform? <sup>1</sup>                             | ?boolean | Whether the application stores guild member data off-platform                                                                                                                                         |
| intents\_gateway\_guild\_members\_retention?                                                     | ?boolean | Whether the application retains guild member datafor 30 days or less                                                                                                                                  |
| intents\_gateway\_guild\_members\_encrypted?                                                     | ?boolean | Whether the application encrypts stored guild member data at rest                                                                                                                                     |
| intents\_gateway\_guild\_members\_contact\_deletion?                                             | ?string  | How application users can request the deletion of their guild member data (25-2000 characters)                                                                                                        |
| intents\_gateway\_message\_content\_use\_case\_description? <sup>1</sup>                         | ?string  | The use case for requesting the message content intent (50-2000 characters)                                                                                                                           |
| intents\_gateway\_message\_content\_use\_case\_supplemental\_material\_description? <sup>1</sup> | ?string  | The supplemental material for the requested Gateway message content intent (5-2000 characters)                                                                                                        |
| intents\_gateway\_message\_content\_store\_off\_platform? <sup>1</sup>                           | ?boolean | Whether the application stores message content data off-platform                                                                                                                                      |
| intents\_gateway\_message\_content\_retention?                                                   | ?boolean | Whether the application retains message content data for 30 days or less                                                                                                                              |
| intents\_gateway\_message\_content\_encrypted?                                                   | ?boolean | Whether the application encrypts stored message content data at rest                                                                                                                                  |
| intents\_gateway\_message\_content\_opt\_out\_stored?                                            | ?boolean | Whether application users can opt out of having their message content data stored                                                                                                                     |
| intents\_gateway\_message\_content\_ai\_training?                                                | ?boolean | Whether the application uses message content data for AI training                                                                                                                                     |
| intents\_gateway\_message\_content\_privacy\_policy\_public?                                     | ?boolean | Whether the application has a public privacy policy detailing how message content data is used                                                                                                        |
| intents\_gateway\_message\_content\_privacy\_policy\_location?                                   | ?string  | Where the application's privacy policy can be found (25-2000 characters)                                                                                                                              |
| intents\_gateway\_message\_content\_privacy\_policy\_example?                                    | ?string  | A link to or screenshots of the application's privacy policy (25-2000 characters)                                                                                                                     |
| intents\_gateway\_message\_content\_contact\_deletion?                                           | ?string  | How application users can request the deletion of their message content data (25-2000 characters)                                                                                                     |

<sup>1</sup> Required if the corresponding intent is requested.

#### [Get Application Discoverability State](aplicaciones.md#get-application-discoverability-state) <a href="#get-application-discoverability-state" id="get-application-discoverability-state"></a>

`GET/applications/{application.id}/discoverability-state`

Returns information about the application's eligibility for application directory. User must be the owner of the application or member of the current team.

[**Response Body**](aplicaciones.md#response-body)

| Field                         | Type                                                                                                                         | Description                                                                                                                        |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| discoverability\_state        | integer                                                                                                                      | The current application directory [discoverability state](aplicaciones.md#application-discoverability-state) of the application    |
| discovery\_eligibility\_flags | integer                                                                                                                      | The current application directory [eligibility flags](aplicaciones.md#application-discovery-eligibility-flags) for the application |
| bad\_commands                 | array\[[application command](https://docs.discord.food/interactions/application-commands#application-command-object) object] | Not safe for work commands that are not allowed in the application directory                                                       |

#### [Query Application Test Mode](aplicaciones.md#query-application-test-mode) <a href="#query-application-test-mode" id="query-application-test-mode"></a>

`GET/activities/{application.id}/test-mode`

Queries whether the user can use test mode for the application. Test mode allows completing purchases without payment. User must be the owner of the application or developer of the current team. Returns a 204 empty response on success.

#### [Get Embedded Activities](aplicaciones.md#get-embedded-activities) <a href="#get-embedded-activities" id="get-embedded-activities"></a>

`GET/activities/shelf`

Returns the embedded activities available globally or in a particular guild.

[**Query String Params**](aplicaciones.md#query-string-params)

| Field      | Type      | Description                              |
| ---------- | --------- | ---------------------------------------- |
| guild\_id? | snowflake | The ID to return embedded activities for |

[**Response Body**](aplicaciones.md#response-body)

| Field        | Type                                                                                       | Description                                                 |
| ------------ | ------------------------------------------------------------------------------------------ | ----------------------------------------------------------- |
| activities   | array\[[embedded activity config](aplicaciones.md#embedded-activity-config-object) object] | The available embedded activities                           |
| applications | array\[partial [application](aplicaciones.md#application-object) object]                   | Applications representing the available embedded activities |
| assets       | map\[snowflake, array\[[application asset](aplicaciones.md#application-asset-object)]]     | The assets for each application                             |

#### [Set Application Embeddability](aplicaciones.md#set-application-embeddability) <a href="#set-application-embeddability" id="set-application-embeddability"></a>

`POST/applications/{application.id}/set-embedded`

Modifies whether the application is an embedded activity or not (determined by the [flag](aplicaciones.md#application-flags)). User must be the owner of the application or developer of the current team. Returns a 204 empty response on success.

[**JSON Params**](aplicaciones.md#json-params)

| Field    | Type    | Description                         |
| -------- | ------- | ----------------------------------- |
| embedded | boolean | Whether the application is embedded |

#### [Get Application Embedded Activity Config](aplicaciones.md#get-application-embedded-activity-config) <a href="#get-application-embedded-activity-config" id="get-application-embedded-activity-config"></a>

`GET/applications/{application.id}/embedded-activity-config`

Returns the [embedded activity config](aplicaciones.md#embedded-activity-config-object) object for the given application ID. User must be the owner of the application or member of the current team.

#### [Modify Application Embedded Activity Config](aplicaciones.md#modify-application-embedded-activity-config) <a href="#modify-application-embedded-activity-config" id="modify-application-embedded-activity-config"></a>

`PATCH/applications/{application.id}/embedded-activity-config`

Modifies the embedded activity config for the given application ID. User must be the owner of the application or developer of the current team. Returns the updated [embedded activity config](aplicaciones.md#embedded-activity-config-object) object on success.

[**JSON Params**](aplicaciones.md#json-params)

| Field                                      | Type                                                                                                                  | Description                                                                                                                     |
| ------------------------------------------ | --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| activity\_preview\_video\_asset\_id?       | ?snowflake                                                                                                            | The ID of the application asset to preview the activity with                                                                    |
| supported\_platforms?                      | ?array\[string]                                                                                                       | The [platforms this activity is supported on](aplicaciones.md#embedded-activity-platform-type)                                  |
| default\_orientation\_lock\_state?         | integer                                                                                                               | The default [orientation lock state](aplicaciones.md#embedded-activity-orientation-lock-state-type) for the activity on mobile  |
| tablet\_default\_orientation\_lock\_state? | integer                                                                                                               | The default [orientation lock state](aplicaciones.md#embedded-activity-orientation-lock-state-type) for the activity on tablets |
| requires\_age\_gate?                       | boolean                                                                                                               | Whether the activity is age gated                                                                                               |
| free\_period\_starts\_at? **(deprecated)** | ?ISO8601 timestamp                                                                                                    | When the current free period for the activity starts, if any                                                                    |
| free\_period\_ends\_at? **(deprecated)**   | ?ISO8601 timestamp                                                                                                    | When the current free period for the activity ends, if any                                                                      |
| client\_platform\_config?                  | map\[string, [embedded activity platform config](aplicaciones.md#embedded-activity-platform-config-structure) object] | The release configuration for the activity on each [platform](aplicaciones.md#embedded-activity-platform-type)                  |
| shelf\_rank?                               | integer                                                                                                               | The rank of the activity in the activity shelf sort order                                                                       |

#### [Get Application Proxy Config](aplicaciones.md#get-application-proxy-config) <a href="#get-application-proxy-config" id="get-application-proxy-config"></a>

`GET/applications/{application.id}/proxy-config`

Returns the application's [activity proxy config](aplicaciones.md#application-proxy-config-object) object for the given application ID. User must be the owner of the application or member of the current team.

#### [Modify Application Proxy Config](aplicaciones.md#modify-application-proxy-config) <a href="#modify-application-proxy-config" id="modify-application-proxy-config"></a>

`POST/applications/{application.id}/proxy-config`

Replaces the activity proxy config for the given application ID. User must be the owner of the application or developer of the current team. Returns the updated [application proxy config](aplicaciones.md#application-proxy-config-object) object on success.

Notes:

* URL mappings can utilize any protocol, so the protocol should be omitted from the field.
* Parameter matching is supported in both the and fields. For example, you can map to .
* Because of how URL globbing works, the order of the mappings is important. The most specific mappings should be at the top of the list as the first match is used. For example, if you have and , you must place the URL before or else the mapping for will never be reached.

[**JSON Params**](aplicaciones.md#json-params)

| Field    | Type                                                                                            | Description                  |
| -------- | ----------------------------------------------------------------------------------------------- | ---------------------------- |
| url\_map | array\[[application proxy mapping](aplicaciones.md#application-proxy-mapping-structure) object] | The URLs mapped to the proxy |

#### [Get Application Assets](aplicaciones.md#get-application-assets) <a href="#get-application-assets" id="get-application-assets"></a>

`GET/oauth2/applications/{application.id}/assets`

Returns a list of [application assets](aplicaciones.md#application-asset-object) for the given application ID.

[**Query String Params**](aplicaciones.md#query-string-params)

| Field    | Type    | Description                                                             |
| -------- | ------- | ----------------------------------------------------------------------- |
| nocache? | boolean | Whether to bypass the Cloudflare cache for the response (default false) |

#### [Create Application Asset](aplicaciones.md#create-application-asset) <a href="#create-application-asset" id="create-application-asset"></a>

`POST/oauth2/applications/{application.id}/assets`

Creates a new application asset for the given application ID. User must be the owner of the application or developer of the current team. Returns an [application asset](aplicaciones.md#application-asset-object) object on success.

[**JSON Params**](aplicaciones.md#json-params)

| Field | Type                                                       | Description                                                     |
| ----- | ---------------------------------------------------------- | --------------------------------------------------------------- |
| name  | string                                                     | The name of the asset                                           |
| type  | integer                                                    | The [type of the asset](aplicaciones.md#application-asset-type) |
| image | [image data](https://docs.discord.food/reference#cdn-data) | The asset's image                                               |

#### [Delete Application Asset](aplicaciones.md#delete-application-asset) <a href="#delete-application-asset" id="delete-application-asset"></a>

`DELETE/oauth2/applications/{application.id}/assets/{asset.id}`

Deletes an application asset permanently. User must be the owner of the application or developer of the current team. Returns a 204 empty response on success.

#### [Proxy Application Assets](aplicaciones.md#proxy-application-assets) <a href="#proxy-application-assets" id="proxy-application-assets"></a>

`POST/applications/{application.id}/external-assets`

Proxies a list of URLs for the given application ID. Returns a list of [external asset](aplicaciones.md#external-asset-structure) objects on success.

[**JSON Params**](aplicaciones.md#json-params)

| Field | Type           | Description                                               |
| ----- | -------------- | --------------------------------------------------------- |
| urls  | array\[string] | The URLs of the assets to proxy (max 256 characters, 1-2) |

[**External Asset Structure**](aplicaciones.md#external-asset-structure)

| Field                 | Type   | Description                                                                |
| --------------------- | ------ | -------------------------------------------------------------------------- |
| url                   | string | The URL of the asset                                                       |
| external\_asset\_path | string | The path to the asset on the media proxy (`https://media.discordapp.net/`) |

[**Example External Asset**](aplicaciones.md#example-external-asset)

```
[  {    "url": "https://google.com/favicon.ico",    "external_asset_path": "external/OCZzr1eoglei1yFsfSMClt6B95EI9W-dOhq7fbnn5aY/https/google.com/favicon.ico"  }]
```

#### [Create Application Attachment](aplicaciones.md#create-application-attachment) <a href="#create-application-attachment" id="create-application-attachment"></a>

`POST/applications/{application.id}/attachment`

Uploads an ephemeral attachment to the application. Must be a body. Requires the [application flag](aplicaciones.md#application-flags).

[**Form Params**](aplicaciones.md#form-params)

| Field | Type          | Description                                                |
| ----- | ------------- | ---------------------------------------------------------- |
| file  | file contents | The image file to upload, must be a JPEG, PNG, or GIF file |

[**Response Body**](aplicaciones.md#response-body)

| Field      | Type                                                                               | Description                      |
| ---------- | ---------------------------------------------------------------------------------- | -------------------------------- |
| attachment | [attachment](https://docs.discord.food/resources/message#attachment-object) object | The created ephemeral attachment |

#### [Get Detectable Applications](aplicaciones.md#get-detectable-applications) <a href="#get-detectable-applications" id="get-detectable-applications"></a>

`GET/applications/detectable`

Returns a list of [detectable application](aplicaciones.md#detectable-application-structure) objects representing games that can be detected by Discord for rich presence.

[**Detectable Application Structure**](aplicaciones.md#detectable-application-structure)

| Field                        | Type                                                                                   | Description                                                                                                                                                                     |
| ---------------------------- | -------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id                           | snowflake                                                                              | The ID of the application                                                                                                                                                       |
| name                         | string                                                                                 | The name of the application                                                                                                                                                     |
| aliases                      | array\[string]                                                                         | Other names the application's game is associated with                                                                                                                           |
| executables                  | array\[[application executable](aplicaciones.md#application-executable-object) object] | The unique executables of the application's game                                                                                                                                |
| themes                       | array\[string]                                                                         | The themes of the application's game                                                                                                                                            |
| hook                         | boolean                                                                                | Whether the Discord client is allowed to hook into the application's game directly                                                                                              |
| overlay                      | boolean                                                                                | Whether the application's game supports the [Discord overlay](https://support.discord.com/hc/en-us/articles/217659737-Game-Overlay-101) (default false)                         |
| overlay\_methods             | ?integer                                                                               | The [methods of overlaying](aplicaciones.md#overlay-method-flags) that the application's game supports                                                                          |
| overlay\_warn                | boolean                                                                                | Whether the [Discord overlay](https://support.discord.com/hc/en-us/articles/217659737-Game-Overlay-101) is known to be problematic with this application's game (default false) |
| overlay\_compatibility\_hook | boolean                                                                                | Whether to use the compatibility hook for the overlay (default false)                                                                                                           |

[**Example Detectable Application**](aplicaciones.md#example-detectable-application)

```
{  "aliases": ["PUBG: BATTLEGROUNDS", "PUBG"],  "executables": [    {      "is_launcher": false,      "name": "win64/tslgame_be.exe",      "os": "win32"    },    {      "is_launcher": false,      "name": "win64/tslgame.exe",      "os": "win32"    },    {      "is_launcher": false,      "name": "tslgame.exe",      "os": "win32"    },    {      "is_launcher": false,      "name": "win64/tslgame_uc.exe",      "os": "win32"    },    {      "is_launcher": false,      "name": "tslgame_be.exe",      "os": "win32"    }  ],  "hook": true,  "id": "356873622985506820",  "name": "PLAYERUNKNOWN'S BATTLEGROUNDS",  "overlay": true,  "overlay_compatibility_hook": true,  "overlay_methods": null,  "overlay_warn": false,  "themes": ["Action", "Warfare"]}
```

#### [Get Partial Applications](aplicaciones.md#get-partial-applications) <a href="#get-partial-applications" id="get-partial-applications"></a>

`GET/applications/public`

Returns a list of partial [application](aplicaciones.md#application-object) objects for the given IDs.

[**Query String Params**](aplicaciones.md#query-string-params)

| Field            | Type              | Description                                                   |
| ---------------- | ----------------- | ------------------------------------------------------------- |
| application\_ids | array\[snowflake] | The IDs of the applications to fetch; unknown IDs are ignored |

#### [Get Partial Application](aplicaciones.md#get-partial-application) <a href="#get-partial-application" id="get-partial-application"></a>

`GET/applications/{application.id}/public`

Returns a partial [application](aplicaciones.md#application-object) object for the given ID with all public application fields.

[**Query String Params**](aplicaciones.md#query-string-params)

| Field        | Type    | Description                                                                                      |
| ------------ | ------- | ------------------------------------------------------------------------------------------------ |
| with\_guild? | boolean | Whether to include the guild object in the response if the guild is discoverable (default false) |

#### [Get Rich Presence Application](aplicaciones.md#get-rich-presence-application) <a href="#get-rich-presence-application" id="get-rich-presence-application"></a>

`GET/applications/{application.id}/rpc`

Returns a partial [application](aplicaciones.md#application-object) object for the given ID with rich presence-related fields.

#### [Get Application Disclosures](aplicaciones.md#get-application-disclosures) <a href="#get-application-disclosures" id="get-application-disclosures"></a>

`GET/applications/{application.id}/disclosures`

Returns an object representing additional safety disclosures for the application.

[**Response Body**](aplicaciones.md#response-body)

| Field              | Type            | Description                                                                                            |
| ------------------ | --------------- | ------------------------------------------------------------------------------------------------------ |
| disclosures        | array\[integer] | The [disclosures](aplicaciones.md#application-disclosure-type) of the application                      |
| acked\_disclosures | array\[integer] | The [disclosures](aplicaciones.md#application-disclosure-type) that have been acknowledged by the user |
| all\_acked         | boolean         | Whether all disclosures have been acknowledged by the user                                             |

[**Application Disclosure Type**](aplicaciones.md#application-disclosure-type)

| Value | Name                                 | Description                                                                                                  |
| ----- | ------------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| 0     | UNSPECIFIED\_DISCLOSURE              | Unspecified disclosure                                                                                       |
| 1     | IP\_LOCATION                         | Application may access the user's IP address                                                                 |
| 2     | DISPLAYS\_ADVERTISEMENTS             | Application may display advertisements                                                                       |
| 3     | PARTNER\_SDK\_DATA\_SHARING\_MESSAGE | Application's game uses the social layer SDK's messaging features, which surface in-game messages on Discord |

[**Example Response**](aplicaciones.md#example-response)

```
{  "disclosures": [1, 2],  "acked_disclosures": [1, 2],  "all_acked": true}
```

#### [Acknowledge Application Disclosures](aplicaciones.md#acknowledge-application-disclosures) <a href="#acknowledge-application-disclosures" id="acknowledge-application-disclosures"></a>

`POST/applications/{application.id}/disclosures`

Acknowledges a list of disclosures for the application.

[**JSON Params**](aplicaciones.md#json-params)

| Field       | Type            | Description                                                                                |
| ----------- | --------------- | ------------------------------------------------------------------------------------------ |
| disclosures | array\[integer] | The [disclosures](aplicaciones.md#application-disclosure-type) to acknowledge for the user |

[**Response Body**](aplicaciones.md#response-body)

| Field       | Type            | Description                                                                                            |
| ----------- | --------------- | ------------------------------------------------------------------------------------------------------ |
| disclosures | array\[integer] | The [disclosures](aplicaciones.md#application-disclosure-type) that have been acknowledged by the user |

#### [Get Guild Applications](aplicaciones.md#get-guild-applications) <a href="#get-guild-applications" id="get-guild-applications"></a>

`GET/guilds/{guild.id}/applications`

Returns a list of [application](aplicaciones.md#application-object) objects attached to the given guild ID. Requires the permission.

[**Query String Params**](aplicaciones.md#query-string-params)

| Field                       | Type      | Description                                                                |
| --------------------------- | --------- | -------------------------------------------------------------------------- |
| type?                       | integer   | The [type of applications](aplicaciones.md#application-type) to return     |
| include\_team? <sup>1</sup> | boolean   | Whether to include team information for owned applications (default false) |
| channel\_id?                | snowflake | The ID of the channel to filter by (TODO: what the fuck does this do)      |

<sup>1</sup> You must own the application or be a member of the owning team to receive this information.

#### [Report Unverified Application](aplicaciones.md#report-unverified-application) <a href="#report-unverified-application" id="report-unverified-application"></a>

`POST/unverified-applications`

Reports a game not detected and tracked to Discord. Returns an unverified application object on success.

[**JSON Params**](aplicaciones.md#json-params)

| Field                     | Type                                                                                | Description                                                                          |
| ------------------------- | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| report\_version?          | integer                                                                             | The version of the report (currently 3)                                              |
| name                      | string                                                                              | The name of the application (2-100 characters)                                       |
| icon                      | string                                                                              | The MD5 hash of the application's icon (32 characters)                               |
| os                        | string                                                                              | The [operating system](aplicaciones.md#operating-system) the application is found on |
| executable?               | string                                                                              | The executable of the application (max 1024 characters)                              |
| publisher?                | string                                                                              | The publisher of the application (2-100 characters)                                  |
| distributor\_application? | [application distributor](aplicaciones.md#application-distributor-structure) object | The distributor of the application SKU                                               |

[**Application Distributor Structure**](aplicaciones.md#application-distributor-structure)

| Field       | Type   | Description                                                     |
| ----------- | ------ | --------------------------------------------------------------- |
| distributor | string | The [application distributor](aplicaciones.md#distributor-type) |
| sku?        | string | The SKU of the application (max 256 characters)                 |

[**Operating System**](aplicaciones.md#operating-system)

| Value  | Description |
| ------ | ----------- |
| win32  | Windows     |
| darwin | macOS       |
| linux  | Linux       |

[**Response Body**](aplicaciones.md#response-body)

| Field         | Type           | Description                                                                           |
| ------------- | -------------- | ------------------------------------------------------------------------------------- |
| name          | string         | The name of the application                                                           |
| hash          | string         | The unique hash of the application                                                    |
| missing\_data | array\[string] | The [missing data](aplicaciones.md#application-missing-data-type) for the application |

[**Application Missing Data Type**](aplicaciones.md#application-missing-data-type)

| Value | Description                                                                                                                                                                 |
| ----- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| icon  | The application's icon hash is not found and should be uploaded using the [Upload Unverified Application Icon](aplicaciones.md#upload-unverified-application-icon) endpoint |

[**Example Response**](aplicaciones.md#example-response)

```
{  "name": "Alien Simulator",  "hash": "0312ce2c94e1fa8257fefbade4587fb3",  "missing_data": ["icon"]}
```

#### [Upload Unverified Application Icon](aplicaciones.md#upload-unverified-application-icon) <a href="#upload-unverified-application-icon" id="upload-unverified-application-icon"></a>

`POST/unverified-applications/icons`

Uploads an unverified application's icon to Discord. Returns a 204 empty response on success.

[**JSON Params**](aplicaciones.md#json-params)

| Field             | Type                                                       | Description                        |
| ----------------- | ---------------------------------------------------------- | ---------------------------------- |
| application\_name | string                                                     | The name of the application        |
| application\_hash | string                                                     | The unique hash of the application |
| icon              | [image data](https://docs.discord.food/reference#cdn-data) | The application's icon             |

#### [Get User Application Role Connections](aplicaciones.md#get-user-application-role-connections) <a href="#get-user-application-role-connections" id="get-user-application-role-connections"></a>

`GET/users/@me/applications/role-connections`

Returns a list of [application role connection](aplicaciones.md#application-role-connection-object) objects for the user.

#### [Get User Application Role Connection](aplicaciones.md#get-user-application-role-connection) <a href="#get-user-application-role-connection" id="get-user-application-role-connection"></a>

`GET/users/@me/applications/{application.id}/role-connection`

Returns an [application role connection](aplicaciones.md#application-role-connection-object) object for the user, without optional fields.

#### [Modify User Application Role Connection](aplicaciones.md#modify-user-application-role-connection) <a href="#modify-user-application-role-connection" id="modify-user-application-role-connection"></a>

`PUT/users/@me/applications/{application.id}/role-connection`

Updates an application's role connection for the user. Returns the updated [application role connection](aplicaciones.md#application-role-connection-object) object on success.

[**JSON Params**](aplicaciones.md#json-params)

| Field               | Type                 | Description                                                                                                                                                                                                                 |
| ------------------- | -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| platform\_name?     | string               | The vanity name of the platform a bot has connected (max 50 characters)                                                                                                                                                     |
| platform\_username? | string               | The username on the platform a bot has connected (max 100 characters)                                                                                                                                                       |
| metadata?           | map\[string, string] | Object mapping [application role connection metadata](aplicaciones.md#application-role-connection-metadata-object) keys to their `string`-ified value (max 100 characters) for the user on the platform a bot has connected |

#### [Get Application Managed Links](aplicaciones.md#get-application-managed-links) <a href="#get-application-managed-links" id="get-application-managed-links"></a>

`GET/applications/{application.id}/managed-links/`

Returns a list of [activity link](aplicaciones.md#activity-link-object) objects for the given application ID. User must be the owner of the application or developer of the current team.

#### [Create Application Managed Link](aplicaciones.md#create-application-managed-link) <a href="#create-application-managed-link" id="create-application-managed-link"></a>

`POST/applications/{application.id}/managed-links/`

Creates a new activity managed link. User must be the owner of the application or developer of the current team. Returns an [activity link](aplicaciones.md#activity-link-object) object on success.

[**JSON Params**](aplicaciones.md#json-params)

| Field       | Type                                                       | Description                                            |
| ----------- | ---------------------------------------------------------- | ------------------------------------------------------ |
| custom\_id? | ?string                                                    | A custom id for the activity link (1-256 characters)   |
| description | string                                                     | The description of the activity link (1-64 characters) |
| image       | [image data](https://docs.discord.food/reference#cdn-data) | The activity link asset                                |
| title       | string                                                     | The title of the activity link (1-32 characters)       |

#### [Get Application Managed Link](aplicaciones.md#get-application-managed-link) <a href="#get-application-managed-link" id="get-application-managed-link"></a>

`GET/applications/{application.id}/managed-links/{link_id}`

Returns an [activity link](aplicaciones.md#activity-link-object) object for the given ID.

#### [Update Application Managed Link](aplicaciones.md#update-application-managed-link) <a href="#update-application-managed-link" id="update-application-managed-link"></a>

`PATCH/applications/{application.id}/managed-links/{link_id}`

Updates the specified activity link for the given application ID. User must be the owner of the application or developer of the current team. Returns an [activity link](aplicaciones.md#activity-link-object) object on success.

#### [Delete Application Managed Link](aplicaciones.md#delete-application-managed-link) <a href="#delete-application-managed-link" id="delete-application-managed-link"></a>

`DELETE/applications/{application.id}/managed-links/{link_id}`

Deletes the specified activity link for the given application ID. User must be the owner of the application or developer of the current team. Returns a 204 empty response on success.

#### [Create Application Quick Link](aplicaciones.md#create-application-quick-link) <a href="#create-application-quick-link" id="create-application-quick-link"></a>

`POST/applications/{application.id}/quick-links/`

Creates a new activity quick link. Returns an [activity link](aplicaciones.md#activity-link-object) object on success.

[**JSON Params**](aplicaciones.md#json-params)

| Field       | Type                                                       | Description                                            |
| ----------- | ---------------------------------------------------------- | ------------------------------------------------------ |
| custom\_id? | ?string                                                    | A custom id for the activity link (1-256 characters)   |
| description | string                                                     | The description of the activity link (1-64 characters) |
| image       | [image data](https://docs.discord.food/reference#cdn-data) | The activity link asset                                |
| title       | string                                                     | The title of the activity link (1-32 characters)       |

#### [Get Application Quick Link](aplicaciones.md#get-application-quick-link) <a href="#get-application-quick-link" id="get-application-quick-link"></a>

`GET/applications/{application.id}/quick-links/{link_id}`

Returns an [activity link](aplicaciones.md#activity-link-object) object for the given ID.

#### [Get Application Verification Eligibility](aplicaciones.md#get-application-verification-eligibility) <a href="#get-application-verification-eligibility" id="get-application-verification-eligibility"></a>

`GET/applications/{application.id}/verification`

Checks if an application is eligible to apply for verification. Returns an empty 204 response on success.

#### [Verify Application](aplicaciones.md#verify-application) <a href="#verify-application" id="verify-application"></a>

`POST/applications/{application.id}/auto-verification`

Verifies an application and allows it to scale past 100 servers. Returns a 204 empty response on success. User must be the owner of the current team. The application must meet the following criteria to be eligible for verification:

* It must belong to a team
* It must not contain any harmful or bad language in its name, description, commands or role connection metadata
* It must have links to its Terms of Service and Privacy Policy
* It must have an install link
* All its team members must have a verified email and MFA set up, with the team owner additionally having to undergo identity verification

---
icon: scale-balanced
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

# Derechos

### [Entitlements](derechos.md#entitlements) <a href="#entitlements" id="entitlements"></a>

Entitlements in Discord represent a user or guild's access to a specific SKU. Entitlements can represent purchases, subscriptions, or gifts, and are used to power many different features in Discord.

#### [Entitlement Object](derechos.md#entitlement-object) <a href="#entitlement-object" id="entitlement-object"></a>

[**Entitlement Structure**](derechos.md#entitlement-structure)

| Field                  | Type                                                            | Description                                                                                    |
| ---------------------- | --------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| id                     | snowflake                                                       | The ID of the entitlement                                                                      |
| type                   | integer                                                         | The [type of entitlement](derechos.md#entitlement-type)                                        |
| sku\_id                | snowflake                                                       | The ID of the SKU granted                                                                      |
| application\_id        | snowflake                                                       | The ID of the application that owns the SKU                                                    |
| user\_id               | snowflake                                                       | The ID of the user that is granted access to the SKU                                           |
| guild\_id?             | snowflake                                                       | The ID of the guild that is granted access to the SKU                                          |
| parent\_id?            | snowflake                                                       | The ID of the parent entitlement                                                               |
| deleted                | boolean                                                         | Whether the entitlement is deleted                                                             |
| consumed?              | boolean                                                         | For consumable items, whether the entitlement has been consumed                                |
| branches?              | array\[snowflake]                                               | The IDs of the application branches granted                                                    |
| starts\_at             | ?ISO8601 timestamp                                              | When the entitlement validity period starts                                                    |
| ends\_at               | ?ISO8601 timestamp                                              | When the entitlement validity period ends                                                      |
| promotion\_id          | ?snowflake                                                      | The ID of the promotion the entitlement is from                                                |
| subscription\_id?      | snowflake                                                       | The ID of the subscription the entitlement is from                                             |
| gift\_code\_flags      | integer                                                         | The [flags for the gift code](derechos.md#gift-code-flags) the entitlement is attached to      |
| gift\_code\_batch\_id? | snowflake                                                       | The ID of the batch the gift code attached to the entitlement is from                          |
| gifter\_user\_id?      | snowflake                                                       | The ID of the user that gifted the entitlement                                                 |
| gift\_style?           | integer                                                         | The [style of the gift](derechos.md#gift-style) attached to the entitlement                    |
| fulfillment\_status?   | integer                                                         | The [tenant fulfillment status](derechos.md#entitlement-fulfillment-status) of the entitlement |
| fulfilled\_at?         | ISO8601 timestamp                                               | When the entitlement was fulfilled                                                             |
| source\_type?          | integer                                                         | The [special source type](derechos.md#entitlement-source-type) of the entitlement              |
| tenant\_metadata?      | [tenant metadata](derechos.md#tenant-metadata-structure) object | Tenant metadata for the entitlement                                                            |
| sku?                   | SKU object                                                      | The SKU granted                                                                                |
| subscription\_plan?    | partial subscription plan object                                | The subscription plan granted                                                                  |

[**Tenant Metadata Structure**](derechos.md#tenant-metadata-structure)

| Field          | Type                                                                          | Description                                                 |
| -------------- | ----------------------------------------------------------------------------- | ----------------------------------------------------------- |
| quest\_rewards | [quest rewards metadata](derechos.md#quest-rewards-metadata-structure) object | Metadata about the quest rewards granted by the entitlement |

[**Quest Rewards Metadata Structure**](derechos.md#quest-rewards-metadata-structure)

| Field         | Type                                                                                            | Description                                                                                        |
| ------------- | ----------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| tag           | integer                                                                                         | The [reward type](https://docs.discord.food/resources/quests#quest-reward-type) of the entitlement |
| reward\_code? | [quest reward code](https://docs.discord.food/resources/quests#quest-reward-code-object) object | The reward granted by the entitlement                                                              |

[**Entitlement Type**](derechos.md#entitlement-type)

| Value | Name                          | Description                                                         |
| ----- | ----------------------------- | ------------------------------------------------------------------- |
| 1     | PURCHASE                      | Entitlement was purchased by a user                                 |
| 2     | PREMIUM\_SUBSCRIPTION         | Entitlement is for a premium (Nitro) subscription                   |
| 3     | DEVELOPER\_GIFT               | Entitlement was gifted by a developer                               |
| 4     | TEST\_MODE\_PURCHASE          | Entitlement was purchased by a developer in application test mode   |
| 5     | FREE\_PURCHASE                | Entitlement was granted when the SKU was free                       |
| 6     | USER\_GIFT                    | Entitlement was gifted by another user                              |
| 7     | PREMIUM\_PURCHASE             | Entitlement was claimed for free through a premium subscription     |
| 8     | APPLICATION\_SUBSCRIPTION     | Entitlement is for an application subscription                      |
| 9     | FREE\_STAFF\_PURCHASE         | Entitlement was claimed for free by a Discord employee              |
| 10    | QUEST\_REWARD                 | Entitlement was granted as a reward for completing a quest          |
| 11    | FRACTIONAL\_REDEMPTION        | Entitlement is for a fractional premium subscription                |
| 12    | VIRTUAL\_CURRENCY\_REDEMPTION | Entitlement was purchased with virtual currency (Orbs)              |
| 13    | GUILD\_POWERUP                | Entitlement was purchased with premium guild subscriptions (boosts) |

[**Entitlement Fulfillment Status**](derechos.md#entitlement-fulfillment-status)

| Value | Name                     | Description                                    |
| ----- | ------------------------ | ---------------------------------------------- |
| 0     | UNKNOWN                  | Unknown fulfillment status                     |
| 1     | FULFILLMENT\_NOT\_NEEDED | Fulfillment is not needed for this entitlement |
| 2     | FULFILLMENT\_NEEDED      | Fulfillment is needed for this entitlement     |
| 3     | FULFILLED                | Entitlement has been fulfilled                 |
| 4     | FULFILLMENT\_FAILED      | Fulfillment of the entitlement has failed      |
| 5     | UNFULFILLMENT\_NEEDED    | Unfulfillment is needed for this entitlement   |
| 6     | UNFULFILLED              | Entitlement has been unfulfilled               |
| 7     | UNFULFILLMENT\_FAILED    | Unfulfillment of the entitlement has failed    |

[**Entitlement Source Type**](derechos.md#entitlement-source-type)

| Value | Name                          | Description                                                |
| ----- | ----------------------------- | ---------------------------------------------------------- |
| 1     | QUEST\_REWARD                 | Entitlement was granted as a reward for completing a quest |
| 2     | DEVELOPER\_GIFT               | Entitlement was gifted by a developer                      |
| 3     | INVOICE                       | Entitlement was granted via an invoice                     |
| 4     | REVERSE\_TRIAL                | Entitlement was granted as part of a reverse trial         |
| 5     | USER\_GIFT                    | Entitlement was gifted by another user                     |
| 6     | GUILD\_POWERUP                | Entitlement was granted via the guild powerups feature     |
| 7     | HOLIDAY\_PROMOTION            | Entitlement was granted as part of a first-party promotion |
| 8     | FRACTIONAL\_PREMIUM\_GIVEBACK | Unknown                                                    |

[**Example Entitlement**](derechos.md#example-entitlement)

```
{  "id": "1014639973498097686",  "sku_id": "557494559257526272",  "application_id": "557494559257526272",  "user_id": "852892297661906993",  "promotion_id": null,  "type": 3,  "deleted": false,  "gift_code_flags": 0,  "starts_at": null,  "ends_at": null,  "branches": ["557494559257526272"],  "gift_code_batch_id": "916443614618464296"}
```

#### [Gift Code Object](derechos.md#gift-code-object) <a href="#gift-code-object" id="gift-code-object"></a>

A gift from one user to another, which can be redeemed for an entitlement.

[**Gift Code Structure**](derechos.md#gift-code-structure)

| Field                   | Type                                                                        | Description                                                  |
| ----------------------- | --------------------------------------------------------------------------- | ------------------------------------------------------------ |
| code                    | string                                                                      | The gift code                                                |
| sku\_id                 | snowflake                                                                   | The ID of the SKU that the gift code grants                  |
| application\_id         | snowflake                                                                   | The ID of the application that owns the SKU                  |
| flags?                  | integer                                                                     | The [flags for the gift code](derechos.md#gift-code-flags)   |
| uses                    | integer                                                                     | The number of times the gift code has been used              |
| max\_uses               | integer                                                                     | The maximum number of times the gift code can be used        |
| redeemed                | boolean                                                                     | Whether the gift code has been redeemed by the current user  |
| expires\_at             | ?ISO8601 timestamp                                                          | When the gift code expires                                   |
| batch\_id?              | snowflake                                                                   | The ID of the batch the gift code is from                    |
| entitlement\_branches?  | array\[snowflake]                                                           | The IDs of the application branches granted by the gift code |
| gift\_style?            | ?integer                                                                    | The [style of the gift code](derechos.md#gift-style)         |
| user?                   | partial [user](https://docs.discord.food/resources/user#user-object) object | The user that created the gift code                          |
| store\_listing?         | store listing object                                                        | The store listing for the SKU the gift code grants           |
| subscription\_plan\_id? | snowflake                                                                   | The ID of the subscription plan the gift code grants         |
| subscription\_plan?     | subscription plan object                                                    | The subscription plan the gift code grants                   |
| subscription\_trial?    | subscription trial object                                                   | The subscription trial the gift code is from                 |
| promotion?              | promotion object                                                            | The promotion the gift code is from                          |

[**Gift Code Flags**](derechos.md#gift-code-flags)

| Value  | Name                               | Description                                                          |
| ------ | ---------------------------------- | -------------------------------------------------------------------- |
| 1 << 0 | PAYMENT\_SOURCE\_REQUIRED          | Gift requires a payment source to redeem                             |
| 1 << 1 | EXISTING\_SUBSCRIPTION\_DISALLOWED | Gift cannot be redeemed by users with existing premium subscriptions |
| 1 << 2 | NOT\_SELF\_REDEEMABLE              | Gift cannot be redeemed by the gifter                                |
| 1 << 3 | PROMOTION                          | Gift is from a promotion                                             |

[**Gift Style**](derechos.md#gift-style)

| Value | Name                    | Description                           |
| ----- | ----------------------- | ------------------------------------- |
| 1     | SNOWGLOBE               | Snowglobe style gift code             |
| 2     | BOX                     | Box style gift code                   |
| 3     | CUP                     | Cup style gift code                   |
| 4     | STANDARD\_BOX           | Standard box style gift code          |
| 5     | CAKE                    | Cake style gift code                  |
| 6     | CHEST                   | Chest style gift code                 |
| 7     | COFFEE                  | Coffee style gift code                |
| 8     | SEASONAL\_STANDARD\_BOX | Seasonal standard box style gift code |
| 9     | SEASONAL\_CAKE          | Seasonal cake style gift code         |
| 10    | SEASONAL\_CHEST         | Seasonal chest style gift code        |
| 11    | SEASONAL\_COFFEE        | Seasonal coffee style gift code       |
| 12    | NITROWEEN\_STANDARD     | Nitroween standard style gift code    |

[**Example Gift Code**](derechos.md#example-gift-code)

```
{  "code": "2CG6SV9QtRxerJTgCYNDnU7M",  "sku_id": "521847234246082599",  "application_id": "521842831262875670",  "uses": 1,  "max_uses": 1,  "expires_at": null,  "redeemed": false,  "batch_id": "1215710455985610833",  "store_listing": {    "id": "521848044908576803",    "summary": " ",    "sku": {      "id": "521847234246082599",      "type": 5,      "product_line": 1,      "dependent_sku_id": null,      "application_id": "521842831262875670",      "manifest_labels": null,      "access_type": 1,      "name": "Nitro",      "features": [],      "release_date": null,      "premium": false,      "slug": "nitro",      "flags": 68,      "show_age_gate": false    },    "thumbnail": {      "id": "971526227435323423",      "size": 227396,      "mime_type": "image/png",      "width": 834,      "height": 474    },    "benefits": []  },  "subscription_plan_id": "642251038925127690"}
```

### [Endpoints](derechos.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get User Entitlements](derechos.md#get-user-entitlements) <a href="#get-user-entitlements" id="get-user-entitlements"></a>

`GET/users/@me/entitlements`

Returns a list of [entitlement](derechos.md#entitlement-object) objects granted to the current user, both active and expired.

[**Query String Params**](derechos.md#query-string-params)

| Field              | Type    | Description                                                          |
| ------------------ | ------- | -------------------------------------------------------------------- |
| with\_sku?         | boolean | Whether to include SKU objects in the response (default false)       |
| with\_application? | boolean | Whether to include application objects in the SKUs (default false)   |
| exclude\_ended?    | boolean | Whether ended entitlements should be omitted (default false)         |
| entitlement\_type? | integer | The [type of entitlement](derechos.md#entitlement-type) to filter by |

#### [Get User Giftable Entitlements](derechos.md#get-user-giftable-entitlements) <a href="#get-user-giftable-entitlements" id="get-user-giftable-entitlements"></a>

`GET/users/@me/entitlements/gifts`

Returns a list of [entitlement](derechos.md#entitlement-object) objects that the current user can gift.

[**Query String Params**](derechos.md#query-string-params)

| Field          | Type   | Description                     |
| -------------- | ------ | ------------------------------- |
| country\_code? | string | The user's billing country code |

#### [Get Guild Entitlements](derechos.md#get-guild-entitlements) <a href="#get-guild-entitlements" id="get-guild-entitlements"></a>

`GET/guilds/{guild.id}/entitlements`

Returns a list of [entitlement](derechos.md#entitlement-object) objects granted to the given guild, both active and expired.

[**Query String Params**](derechos.md#query-string-params)

| Field              | Type    | Description                                                          |
| ------------------ | ------- | -------------------------------------------------------------------- |
| with\_sku?         | boolean | Whether to include SKU objects in the response (default false)       |
| with\_application? | boolean | Whether to include application objects in the SKUs (default false)   |
| exclude\_ended?    | boolean | Whether ended entitlements should be omitted (default false)         |
| exclude\_deleted?  | boolean | Whether deleted entitlements should be omitted (default true)        |
| entitlement\_type? | integer | The [type of entitlement](derechos.md#entitlement-type) to filter by |

#### [Get Application Entitlements](derechos.md#get-application-entitlements) <a href="#get-application-entitlements" id="get-application-entitlements"></a>

`GET/applications/{application.id}/entitlements`

Returns a list of [entitlement](derechos.md#entitlement-object) objects for the given application, both active and expired.

[**Query String Params**](derechos.md#query-string-params)

| Field             | Type              | Description                                                   |
| ----------------- | ----------------- | ------------------------------------------------------------- |
| user\_id?         | snowflake         | The ID of the user to look up entitlements for                |
| sku\_ids?         | array\[snowflake] | The IDs of the SKUs to look up entitlements for               |
| guild\_id?        | snowflake         | The ID of the guild to look up entitlements for               |
| exclude\_ended?   | boolean           | Whether ended entitlements should be omitted (default false)  |
| exclude\_deleted? | boolean           | Whether deleted entitlements should be omitted (default true) |
| before?           | snowflake         | Get entitlements before this entitlement ID                   |
| after?            | snowflake         | Get entitlements after this entitlement ID                    |
| limit?            | integer           | Max number of entitlements to return (1-100, default 100)     |

#### [Get User Application Entitlements](derechos.md#get-user-application-entitlements) <a href="#get-user-application-entitlements" id="get-user-application-entitlements"></a>

`GET/users/@me/applications/{application.id}/entitlements`

Returns a list of [entitlement](derechos.md#entitlement-object) objects granted to the current user for the given application.

[**Query String Params**](derechos.md#query-string-params)

| Field             | Type              | Description                                                    |
| ----------------- | ----------------- | -------------------------------------------------------------- |
| sku\_ids?         | array\[snowflake] | The IDs of the SKUs to look up entitlements for                |
| exclude\_consumed | boolean           | Whether consumed entitlements should be omitted (default true) |

#### [Get Application Entitlement](derechos.md#get-application-entitlement) <a href="#get-application-entitlement" id="get-application-entitlement"></a>

`GET/applications/{application.id}/entitlements/{entitlement.id}`

Returns an [entitlement](derechos.md#entitlement-object) object for the given application and entitlement ID.

#### [Create Application Entitlement](derechos.md#create-application-entitlement) <a href="#create-application-entitlement" id="create-application-entitlement"></a>

`POST/applications/{application.id}/entitlements`

Creates a test entitlement to a given subscription SKU for a given guild or user. Returns an [entitlement](derechos.md#entitlement-object) object on success. Fires an [Entitlement Create](https://docs.discord.food/topics/gateway-events#entitlement-create) Gateway event.

[**JSON Params**](derechos.md#json-params)

| Field       | Type    | Description                                                                |
| ----------- | ------- | -------------------------------------------------------------------------- |
| sku\_id     | string  | The ID of the SKU to grant the entitlement to                              |
| owner\_id   | string  | The ID of the guild or user to grant the entitlement to                    |
| owner\_type | integer | The [type of owner](derechos.md#entitlement-owner-type) of the entitlement |

[**Entitlement Owner Type**](derechos.md#entitlement-owner-type)

| Value | Name  | Description                |
| ----- | ----- | -------------------------- |
| 1     | GUILD | Entitlement is for a guild |
| 2     | USER  | Entitlement is for a user  |

#### [Consume Application Entitlement](derechos.md#consume-application-entitlement) <a href="#consume-application-entitlement" id="consume-application-entitlement"></a>

`POST/applications/{application.id}/entitlements/{entitlement.id}/consume`

For one-time purchase consumable SKUs, marks a given entitlement for the user as consumed. Returns a 204 empty response on success. Fires an [Entitlement Update](https://docs.discord.food/topics/gateway-events#entitlement-update) Gateway event.

#### [Delete Application Entitlement](derechos.md#delete-application-entitlement) <a href="#delete-application-entitlement" id="delete-application-entitlement"></a>

`DELETE/applications/{application.id}/entitlements/{entitlement.id}`

Deletes a currently-active test entitlement. Returns a 204 empty response on success. Fires an [Entitlement Delete](https://docs.discord.food/topics/gateway-events#entitlement-delete) Gateway event.

#### [Get Gift Code](derechos.md#get-gift-code) <a href="#get-gift-code" id="get-gift-code"></a>

`GET/entitlements/gift-codes/{gift_code.code}`

Returns a [gift code](derechos.md#gift-code-object) object for the given code.

[**Query String Params**](derechos.md#query-string-params)

| Field                     | Type    | Description                                                                |
| ------------------------- | ------- | -------------------------------------------------------------------------- |
| with\_application?        | boolean | Whether to include the application object in the SKU (default false)       |
| with\_subscription\_plan? | boolean | Whether to include the subscription plan object in the SKU (default false) |

#### [Redeem Gift Code](derechos.md#redeem-gift-code) <a href="#redeem-gift-code" id="redeem-gift-code"></a>

`POST/entitlements/gift-codes/{gift_code.code}/redeem`

Redeems a gift code for the current user. Returns an [entitlement](derechos.md#entitlement-object) object on success. Fires an [Entitlement Create](https://docs.discord.food/topics/gateway-events#entitlement-create) and [Gift Code Update](https://docs.discord.food/topics/gateway-events#gift-code-update) Gateway event.

[**JSON Params**](derechos.md#json-params)

| Field                       | Type                                                                               | Description                                                      |
| --------------------------- | ---------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| payment\_source\_id?        | ?string                                                                            | The ID of the payment source to use for the gift code redemption |
| channel\_id?                | ?snowflake                                                                         | The ID of the channel the gift code is being redeemed in         |
| gateway\_checkout\_context? | ?[gateway checkout context](derechos.md#gateway-checkout-context-structure) object | The context for the gateway checkout, if applicable              |

[**Gateway Checkout Context Structure**](derechos.md#gateway-checkout-context-structure)

| Field                    | Type    | Description                                         |
| ------------------------ | ------- | --------------------------------------------------- |
| braintree\_device\_data? | ?string | The Braintree device data collected during checkout |

#### [Get User Gift Codes](derechos.md#get-user-gift-codes) <a href="#get-user-gift-codes" id="get-user-gift-codes"></a>

`GET/users/@me/entitlements/gift-codes`

Returns a list of [gift code](derechos.md#gift-code-object) objects that the current user has created.

[**Query String Params**](derechos.md#query-string-params)

| Field                   | Type              | Description                                  |
| ----------------------- | ----------------- | -------------------------------------------- |
| sku\_ids?               | array\[snowflake] | The IDs of the SKUs to filter by             |
| subscription\_plan\_id? | snowflake         | The ID of the subscription plan to filter by |

#### [Create User Gift Code](derechos.md#create-user-gift-code) <a href="#create-user-gift-code" id="create-user-gift-code"></a>

`POST/users/@me/entitlements/gift-codes`

Creates a gift code. Requires an eligible giftable entitlement. Returns a [gift code](derechos.md#gift-code-object) object on success. Fires a [Gift Code Create](https://docs.discord.food/topics/gateway-events#gift-code-create) Gateway event.

[**JSON Params**](derechos.md#json-params)

| Field                   | Type      | Description                                               |
| ----------------------- | --------- | --------------------------------------------------------- |
| sku\_id                 | string    | The ID of the SKU to create a gift code for               |
| subscription\_plan\_id? | snowflake | The ID of the subscription plan to create a gift code for |
| gift\_style?            | integer   | The [style of the gift](derechos.md#gift-style) created   |

#### [Revoke User Gift Code](derechos.md#revoke-user-gift-code) <a href="#revoke-user-gift-code" id="revoke-user-gift-code"></a>

`DELETE/users/@me/entitlements/gift-codes/{gift_code.code}`

Revokes a gift code created by the current user. Returns a 204 empty response on success.

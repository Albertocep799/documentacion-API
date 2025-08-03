---
icon: filter-circle-dollar
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

# Referidos premium

### [Premium Referrals](referidos-premium.md#premium-referrals) <a href="#premium-referrals" id="premium-referrals"></a>

Premium referrals, or Nitro trials, allow Nitro subscribers to share up to three 2-week Nitro subscriptions with friends who haven't had an active Nitro subscription in the past 12 months.

#### [Premium Referral Object](referidos-premium.md#premium-referral-object) <a href="#premium-referral-object" id="premium-referral-object"></a>

[**Premium Referral Structure**](referidos-premium.md#premium-referral-structure)

| Field               | Type                                                                        | Description                                         |
| ------------------- | --------------------------------------------------------------------------- | --------------------------------------------------- |
| id                  | snowflake                                                                   | The ID of the referral                              |
| user\_id            | snowflake                                                                   | The user who the referral was sent to               |
| trial\_id           | snowflake                                                                   | The trial ID associated with the referral           |
| subscription\_trial | subscription trial object (todo)                                            | The subscription trial associated with the referral |
| expires\_at         | ISO8601 timestamp                                                           | The expiry date of the referral link                |
| referrer\_id        | snowflake                                                                   | The ID of the user who created the referral         |
| referrer            | partial [user](https://docs.discord.food/resources/user#user-object) object | The user who created the referral                   |
| redeemed\_at?       | ISO8601 timestamp                                                           | When the referral was redeemed                      |

[**Example Premium Referral**](referidos-premium.md#example-premium-referral)

```
{  "id": "1107800271637200936",  "user_id": "563434444321587202",  "trial_id": "1073698058383917056",  "subscription_trial": {    "id": "1073698058383917056",    "interval": 3,    "interval_count": 14,    "sku_id": "521847234246082599"  },  "expires_at": "2023-05-17T22:42:46.690847+00:00",  "referrer_id": "1044657759066525777",  "referer": {    "id": "1044657759066525777",    "username": "hackermon",    "global_name": "daniel",    "avatar": null,    "avatar_decoration_data": null,    "discriminator": "0",    "public_flags": 16384,    "primary_guild": null  },  "redeemed_at": "2023-05-16T18:22:21.777456+00:00"}
```

#### [Premium Referral Eligibility Object](referidos-premium.md#premium-referral-eligibility-object) <a href="#premium-referral-eligibility-object" id="premium-referral-eligibility-object"></a>

[**Premium Referral Eligibility Structure**](referidos-premium.md#premium-referral-eligibility-structure)

| Field                         | Type                     | Description                                                                                              |
| ----------------------------- | ------------------------ | -------------------------------------------------------------------------------------------------------- |
| referrals\_remaining          | integer                  | The amount of referrals remaining                                                                        |
| sent\_user\_ids               | array\[snowflake]        | The IDs of users that have been referred                                                                 |
| refresh\_at                   | ?ISO8601 timestamp       | When the referral count will refresh                                                                     |
| has\_eligible\_friends        | boolean                  | Whether the user has friends that are eligible for a referral                                            |
| recipient\_status             | map\[snowflake, integer] | The [redemption status](referidos-premium.md#premium-referral-recipient-status) of each referred user ID |
| is\_eligible\_for\_incentive  | boolean                  | Whether the user is eligible for a personal discount upon referral redemption                            |
| is\_qualified\_for\_incentive | boolean                  | Whether the user will receive an incentivized discount on their next Nitro purchase                      |
| referral\_incentive\_status   | integer                  | The [incentive status](referidos-premium.md#premium-referral-incentive-status) of the user               |

[**Premium Referral Recipient Status**](referidos-premium.md#premium-referral-recipient-status)

| Value | Name     | Description                                  |
| ----- | -------- | -------------------------------------------- |
| 1     | REDEEMED | The recipient has redeemed the referral      |
| 2     | PENDING  | The recipient has yet to redeem the referral |

[**Premium Referral Incentive Status**](referidos-premium.md#premium-referral-incentive-status)

| Value | Name          | Description                                                                 |
| ----- | ------------- | --------------------------------------------------------------------------- |
| 0     | NOT\_ELIGIBLE | The user is not eligible for an incentive                                   |
| 1     | ELIGIBLE      | The user is eligible for an incentive                                       |
| 2     | QUALIFIED     | The user will receive an incentivized discount on their next Nitro purchase |
| 3     | COOLDOWN      | The user is on cooldown and cannot receive an incentive                     |
| 4     | UNAPPLIED     | The user has not applied their incentive yet                                |

[**Example Premium Referral Eligibility**](referidos-premium.md#example-premium-referral-eligibility)

```
{  "referrals_remaining": 0,  "sent_user_ids": ["159985870458322944", "563434444321587202", "296776625432035328"],  "refresh_at": null,  "has_eligible_friends": true,  "recipient_status": {    "159985870458322944": 2,    "563434444321587202": 2,    "296776625432035328": 2  },  "is_eligible_for_incentive": false,  "is_qualified_for_incentive": false,  "referral_incentive_status": 0}
```

### [Endpoints](referidos-premium.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get Premium Referral](referidos-premium.md#get-premium-referral) <a href="#get-premium-referral" id="get-premium-referral"></a>

`GET/referrals/{referral.id}`

Returns a [premium referral](referidos-premium.md#premium-referral-object) object for the given referral ID.

#### [Get Premium Referral Eligibility](referidos-premium.md#get-premium-referral-eligibility) <a href="#get-premium-referral-eligibility" id="get-premium-referral-eligibility"></a>

`GET/users/@me/referrals/eligibility`

Returns a [premium referral eligibility](referidos-premium.md#premium-referral-eligibility-object) object for the user.

#### [Get Premium Referral Incentive Eligibility](referidos-premium.md#get-premium-referral-incentive-eligibility) <a href="#get-premium-referral-incentive-eligibility" id="get-premium-referral-incentive-eligibility"></a>

`GET/users/@me/referrals/incentive-eligibility`

Returns a subset of the [premium referral eligibility](referidos-premium.md#premium-referral-eligibility-object) object for the user with their eligibility for a personal discount upon referral redemption.

[**Response Body**](referidos-premium.md#response-body)

| Field                        | Type    | Description                                                                   |
| ---------------------------- | ------- | ----------------------------------------------------------------------------- |
| is\_eligible\_for\_incentive | boolean | Whether the user is eligible for a personal discount upon referral redemption |

#### [Get Premium Referral Eligible Users](referidos-premium.md#get-premium-referral-eligible-users) <a href="#get-premium-referral-eligible-users" id="get-premium-referral-eligible-users"></a>

`POST/users/@me/referrals/eligible-users`

Returns an object containing a list of users who are eligible to receive a referral from the user.

[**JSON Params**](referidos-premium.md#json-params)

| Field          | Type    | Description                                            |
| -------------- | ------- | ------------------------------------------------------ |
| index          | integer | The current search index (0-1000)                      |
| limit?         | integer | Max number of users to return (max 50, default 30)     |
| search\_query? | string  | Query to match usernames against (max 1024 characters) |

[**Response Body**](referidos-premium.md#response-body)

| Field        | Type                                                                                | Description                     |
| ------------ | ----------------------------------------------------------------------------------- | ------------------------------- |
| users        | array\[partial [user](https://docs.discord.food/resources/user#user-object) object] | The eligible users              |
| next\_index? | integer                                                                             | The next index to paginate from |

#### [Create Premium Referral](referidos-premium.md#create-premium-referral) <a href="#create-premium-referral" id="create-premium-referral"></a>

`POST/users/@me/referrals/{user.id}`

Creates a new premium referral. Returns a [premium referral](referidos-premium.md#premium-referral-object) object. Fires a [Message Create](https://docs.discord.food/topics/gateway-events#message-create) Gateway event. The referral is sent to the user specified. Referrals expire after 48 hours.

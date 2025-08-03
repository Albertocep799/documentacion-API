---
icon: user
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

# Usuarios

### [Users](usuarios.md#users) <a href="#users" id="users"></a>

Users in Discord are generally considered the base entity. Users can spawn across the entire platform, be members of guilds, participate in text and voice chat, and much more. Users are separated by a distinction of "bot" vs "normal". Although they are similar, bot users are automated users that are attached to an application, "owned" by another user. Unlike normal users, bot users do _not_ have a limitation on the number of guilds they can be a part of.

#### [Usernames and Nicknames](usuarios.md#usernames-and-nicknames) <a href="#usernames-and-nicknames" id="usernames-and-nicknames"></a>

Discord enforces the following restrictions for usernames, display names, and nicknames:

1. Names can contain most valid unicode characters. We limit some zero-width and non-rendering characters.
2. Usernames must be between 2 and 32 characters long.
3. Display names and nicknames must be between 1 and 32 characters long.
4. Webhook names must be between 1 and 80 characters long.
5. Names are sanitized and trimmed of leading, trailing, and excessive internal whitespace.

The following restrictions are additionally enforced for usernames and display names:

1. Usernames cannot contain the following substrings: '@', '#', ':', '\`\`\`'.
2. Usernames and display names cannot be: 'everyone', 'here', 'system message', or contain 'discord'.

The following restrictions are additionally enforced for webhook names:

1. Webhook names cannot contain the following substrings: 'clyde'.

[Migrated usernames](usuarios.md#unique-usernames) are subject to a new set of restrictions in addition to the above:

1. Migrated usernames can only contain lowercase alphanumeric characters, underscores (), and periods (). Uppercase characters, spaces, dashes (), and other special characters are not allowed.
2. Migrated usernames cannot have two or more consecutive periods ().
3. Migrated usernames are unique to each user, and no two users can share the same username.

There are other rules and restrictions not shared here for the sake of spam and abuse mitigation, but the majority of users won't encounter them. It's important to properly handle all error messages returned by Discord when editing or updating names.

#### [Unique Usernames](usuarios.md#unique-usernames) <a href="#unique-usernames" id="unique-usernames"></a>

Discord's username system is changing. Discriminators are being removed and new, unique usernames () and display names are being introduced. Internally, this migration is referred to as "pomelo". You can read more details about how the changes to the username system affect user accounts in the [general Help Center article](https://dis.gd/usernames). To learn how it impacts bots specifically, you can read the [Developer Help Center article](https://dis.gd/app-usernames).

A user's legacy tag will still be usable to [send friend requests](https://docs.discord.food/resources/relationships#send-friend-request), and will be available as a [profile badge](usuarios.md#profile-badge-structure) for migrated users.

[**Identifying Migrated Users**](usuarios.md#identifying-migrated-users)

The value of a single zero () in the [field on the user object](usuarios.md#user-object) indicates that the user has ~~been pommeled~~ migrated to the new username system. Note that the discriminator for migrated users will _not_ be 4-digits like a standard discriminator (it is , not ). The value of the field will become the migrated user's unique username.

[**Migrating**](usuarios.md#migrating)

~~Users can only migrate their account to pomelo if they are in the rollout. A user is in the rollout if they have in the~~ [~~field on the Ready Supplemental event~~](https://docs.discord.food/topics/gateway-events#ready-supplemental) ~~and are~~ [~~in the user experiment~~](https://docs.discord.food/topics/experiments#user-experiments)~~.~~ Migration is now complete and all non-migrated users have been automatically assigned a unique username.

To migrate, users should first [check if the username they want is available](usuarios.md#get-unique-username-eligibility), and then [migrate their account to that username](usuarios.md#create-unique-username). If the username they want is not available, they can [get a list of suggested usernames](usuarios.md#get-unique-username-suggestions) to choose from.

[**Display Names**](usuarios.md#display-names)

As part of unique usernames, user accounts can define a non-unique display name. This value is a new nullable field with a max length of 32 characters.

[**Default Avatars**](usuarios.md#default-avatars)

For users with migrated accounts, default avatar URLs will be based on the user ID instead of the discriminator. The URL can be calculated using . For non-migrated accounts, the URL can be calculated using .

#### [User Object](usuarios.md#user-object) <a href="#user-object" id="user-object"></a>

[**User Structure**](usuarios.md#user-structure)

| Field                                   | Type                                                                                               | Description                                                                                                      |
| --------------------------------------- | -------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| id                                      | snowflake                                                                                          | The ID of the user                                                                                               |
| username <sup>5</sup>                   | string                                                                                             | The user's username, may be unique across the platform (2-32 characters)                                         |
| discriminator <sup>5</sup>              | string                                                                                             | The user's stringified 4-digit Discord tag                                                                       |
| global\_name <sup>5</sup>               | ?string                                                                                            | The user's display name (1-32 characters)                                                                        |
| avatar                                  | ?string                                                                                            | The user's [avatar hash](https://docs.discord.food/reference#cdn-formatting)                                     |
| avatar\_decoration\_data                | ?[avatar decoration data](usuarios.md#avatar-decoration-data-object) object                        | The user's [avatar decoration](https://support.discord.com/hc/en-us/articles/13410113109911-Avatar-Decorations)  |
| collectibles?                           | ?[collectibles](usuarios.md#collectibles-object) object                                            | The user's equipped collectibles                                                                                 |
| display\_name\_styles?                  | ?[display name style](usuarios.md#display-name-style-structure) object                             | The user's display name style                                                                                    |
| primary\_guild?                         | ?[primary guild](usuarios.md#primary-guild-structure) object                                       | The primary guild of the user                                                                                    |
| linked\_users <sup>1</sup> <sup>3</sup> | array\[[linked user](https://docs.discord.food/resources/family-center#linked-user-object) object] | The linked users connected to the account via [Family Center](https://docs.discord.food/resources/family-center) |
| bot?                                    | boolean                                                                                            | Whether the user is a bot account                                                                                |
| system?                                 | boolean                                                                                            | Whether the user is an official Discord System user (part of the urgent message system)                          |
| mfa\_enabled                            | boolean                                                                                            | Whether the user has multi-factor authentication enabled on their account                                        |
| nsfw\_allowed? <sup>1</sup>             | ?boolean                                                                                           | Whether the user is allowed to see NSFW content, `null` if not yet known                                         |
| age\_verification\_status <sup>1</sup>  | integer                                                                                            | The [age verification status](usuarios.md#age-verification-status) of the user                                   |
| pronouns? <sup>1</sup> <sup>4</sup>     | string                                                                                             | The user's pronouns (max 40 characters)                                                                          |
| bio <sup>1</sup>                        | string                                                                                             | The user's bio (max 190 characters)                                                                              |
| banner                                  | ?string                                                                                            | The user's [banner hash](https://docs.discord.food/reference#cdn-formatting)                                     |
| accent\_color                           | ?integer                                                                                           | The user's banner color encoded as an integer representation of a hexadecimal color code                         |
| locale? <sup>3</sup>                    | string                                                                                             | The [language option](https://docs.discord.food/reference#locales) chosen by the user                            |
| verified <sup>2</sup>                   | boolean                                                                                            | Whether the email on this account has been verified                                                              |
| email <sup>2</sup>                      | ?string                                                                                            | The user's email address                                                                                         |
| phone? <sup>1</sup>                     | ?string                                                                                            | The user's E.164-formatted phone number                                                                          |
| premium **(deprecated)** <sup>4</sup>   | boolean                                                                                            | Whether the user is subscribed to Nitro                                                                          |
| premium\_type                           | integer                                                                                            | The [type of premium (Nitro) subscription](usuarios.md#premium-type) on a user's account                         |
| personal\_connection\_id?               | snowflake                                                                                          | The ID of the user's personal, non-employee user account                                                         |
| flags <sup>1</sup>                      | integer                                                                                            | The [flags](usuarios.md#user-flags) on a user's account                                                          |
| public\_flags                           | integer                                                                                            | The public [flags](usuarios.md#user-flags) on a user's account                                                   |
| purchased\_flags? <sup>1</sup>          | integer                                                                                            | The [purchased flags](usuarios.md#purchased-flags) on a user's account                                           |
| premium\_usage\_flags? <sup>1</sup>     | integer                                                                                            | The [premium usage flags](usuarios.md#premium-usage-flags) on a user's account                                   |
| desktop? <sup>1</sup> <sup>4</sup>      | boolean                                                                                            | Whether the user has used the desktop client before                                                              |
| mobile? <sup>1</sup> <sup>4</sup>       | boolean                                                                                            | Whether the user has used the mobile client before                                                               |
| has\_bounced\_email? <sup>1</sup>       | boolean                                                                                            | Whether the user's email has failed to deliver and is no longer valid                                            |
| authenticator\_types? <sup>3</sup>      | array\[integer]                                                                                    | The [types of multi-factor authenticators](usuarios.md#authenticator-type) the user has enabled                  |

<sup>1</sup> Not included when fetching a user via OAuth2.

<sup>2</sup> Not included when fetching a user via OAuth2 without the scope.

<sup>3</sup> Not included in the user object returned in the [Ready event](https://docs.discord.food/topics/gateway-events#ready).

<sup>4</sup> Only included in the user object returned in the [Ready event](https://docs.discord.food/topics/gateway-events#ready).

<sup>5</sup> See the [section on Discord's new username system](usuarios.md#unique-usernames) for more information.

[**Partial User Structure**](usuarios.md#partial-user-structure)

| Field                       | Type                                                                        | Description                                                                                                     |
| --------------------------- | --------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| id                          | snowflake                                                                   | The ID of the user                                                                                              |
| username <sup>1</sup>       | string                                                                      | The user's username, may be unique across the platform (2-32 characters)                                        |
| discriminator <sup>1</sup>  | string                                                                      | The user's stringified 4-digit Discord tag                                                                      |
| global\_name? <sup>1</sup>  | ?string                                                                     | The user's display name (1-32 characters)                                                                       |
| avatar                      | ?string                                                                     | The user's [avatar hash](https://docs.discord.food/reference#cdn-formatting)                                    |
| avatar\_decoration\_data?   | ?[avatar decoration data](usuarios.md#avatar-decoration-data-object) object | The user's [avatar decoration](https://support.discord.com/hc/en-us/articles/13410113109911-Avatar-Decorations) |
| collectibles?               | ?[collectibles](usuarios.md#collectibles-object) object                     | The user's equipped collectibles                                                                                |
| display\_name\_styles?      | ?[display name style](usuarios.md#display-name-style-structure) object      | The user's display name style                                                                                   |
| primary\_guild?             | ?[primary guild](usuarios.md#primary-guild-structure) object                | The primary guild of the user                                                                                   |
| bot?                        | boolean                                                                     | Whether the user is a bot account                                                                               |
| system?                     | boolean                                                                     | Whether the user is an official Discord System user (part of the urgent message system)                         |
| banner? <sup>2</sup>        | ?string                                                                     | The user's [banner hash](https://docs.discord.food/reference#cdn-formatting)                                    |
| accent\_color? <sup>2</sup> | ?integer                                                                    | The user's banner color encoded as an integer representation of a hexadecimal color code                        |
| public\_flags?              | integer                                                                     | The public [flags](usuarios.md#user-flags) on a user's account                                                  |

<sup>1</sup> See the [section on Discord's new username system](usuarios.md#unique-usernames) for more information.

<sup>2</sup> Only guaranteed to be included when fetched through the [Get User](usuarios.md#get-user) and [Get User Profile](usuarios.md#get-user-profile) endpoints. May be included in data received through other API endpoints.

<sup>3</sup> Only guaranteed to be included when fetched through the [Get User](usuarios.md#get-user) endpoint or the [field on the message object](https://docs.discord.food/resources/message#message-object). May be included in data received through other API endpoints.

[**Primary Guild Structure**](usuarios.md#primary-guild-structure)

| Field                            | Type       | Description                                                                    |
| -------------------------------- | ---------- | ------------------------------------------------------------------------------ |
| identity\_enabled <sup>1</sup>   | ?boolean   | Whether the user is displaying their guild tag                                 |
| identity\_guild\_id <sup>2</sup> | ?snowflake | The ID of the guild                                                            |
| tag <sup>1</sup>                 | ?string    | The user's guild tag (max 4 characters)                                        |
| badge <sup>1</sup>               | ?string    | The [guild tag badge hash](https://docs.discord.food/reference#cdn-formatting) |

<sup>1</sup> This field is when a user has not reaffirmed their identity after a tag change.

<sup>2</sup> Only populated for users with not set to .

[**User Flags**](usuarios.md#user-flags)

| Value       | Name                                     | Description                                                                                                                              | Public          |
| ----------- | ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | --------------- |
| 1 << 0      | STAFF                                    | Discord Staff                                                                                                                            | Yes             |
| 1 << 1      | PARTNER                                  | Partnered Server Owner                                                                                                                   | Yes             |
| 1 << 2      | HYPESQUAD                                | HypeSquad Events                                                                                                                         | Yes             |
| 1 << 3      | BUG\_HUNTER\_LEVEL\_1                    | Level 1 Discord Bug Hunter                                                                                                               | Yes             |
| 1 << 4      | MFA\_SMS                                 | SMS enabled as a multi-factor authentication backup                                                                                      | No              |
| 1 << 5      | PREMIUM\_PROMO\_DISMISSED                | User has dismissed the current premium (Nitro) promotion                                                                                 | No              |
| 1 << 6      | HYPESQUAD\_ONLINE\_HOUSE\_1              | HypeSquad Bravery                                                                                                                        | Yes             |
| 1 << 7      | HYPESQUAD\_ONLINE\_HOUSE\_2              | HypeSquad Brilliance                                                                                                                     | Yes             |
| 1 << 8      | HYPESQUAD\_ONLINE\_HOUSE\_3              | HypeSquad Balance                                                                                                                        | Yes             |
| 1 << 9      | PREMIUM\_EARLY\_SUPPORTER                | Early Premium (Nitro) Supporter                                                                                                          | Yes             |
| 1 << 10     | TEAM\_PSEUDO\_USER                       | User is a [Team](https://docs.discord.food/resources/team)                                                                               | Yes             |
| 1 << 11     | IS\_HUBSPOT\_CONTACT                     | User is registered on Discord's [HubSpot](https://www.hubspot.com/) customer platform, used for official Discord programs (e.g. partner) | No <sup>1</sup> |
| ~~1 << 12~~ | ~~SYSTEM~~                               | ~~User is a system user (i.e. official Discord account)~~                                                                                | ~~Yes~~         |
| 1 << 13     | HAS\_UNREAD\_URGENT\_MESSAGES            | User has unread urgent system messages; an urgent message is one sent from Trust and Safety                                              | No              |
| 1 << 14     | BUG\_HUNTER\_LEVEL\_2                    | Level 2 Discord Bug Hunter                                                                                                               | Yes             |
| 1 << 15     | UNDERAGE\_DELETED                        | User is scheduled for deletion for being under the minimum required age                                                                  | No <sup>1</sup> |
| 1 << 16     | VERIFIED\_BOT                            | Verified Bot                                                                                                                             | Yes             |
| 1 << 17     | VERIFIED\_DEVELOPER                      | Early Verified Bot Developer                                                                                                             | Yes             |
| 1 << 18     | CERTIFIED\_MODERATOR                     | Moderator Programs Alumni                                                                                                                | Yes             |
| 1 << 19     | BOT\_HTTP\_INTERACTIONS                  | Bot uses only HTTP interactions and is shown in the online member list                                                                   | Yes             |
| 1 << 20     | SPAMMER                                  | User is marked as a spammer and has their messages collapsed in the UI                                                                   | Yes             |
| ~~1 << 21~~ | ~~DISABLE\_PREMIUM~~                     | ~~User has manually disabled premium (Nitro) features~~                                                                                  | ~~No~~          |
| 1 << 22     | ACTIVE\_DEVELOPER                        | [Active Developer](https://support-dev.discord.com/hc/articles/10113997751447)                                                           | Yes             |
| 1 << 23     | PROVISIONAL\_ACCOUNT                     | User is a provisional account used with the social layer integration                                                                     | Yes             |
| 1 << 33     | HIGH\_GLOBAL\_RATE\_LIMIT                | User has their global ratelimit raised to 1,200 requests per second                                                                      | No <sup>1</sup> |
| 1 << 34     | DELETED                                  | User's account is deleted                                                                                                                | No <sup>1</sup> |
| 1 << 35     | DISABLED\_SUSPICIOUS\_ACTIVITY           | User's account is disabled for suspicious activity and must reset their password to regain access                                        | No <sup>1</sup> |
| 1 << 36     | SELF\_DELETED                            | User deleted their own account                                                                                                           | No <sup>1</sup> |
| 1 << 37     | PREMIUM\_DISCRIMINATOR                   | User has a premium (Nitro) custom discriminator                                                                                          | No <sup>1</sup> |
| 1 << 38     | USED\_DESKTOP\_CLIENT                    | User has used the desktop client                                                                                                         | No <sup>1</sup> |
| 1 << 39     | USED\_WEB\_CLIENT                        | User has used the web client                                                                                                             | No <sup>1</sup> |
| 1 << 40     | USED\_MOBILE\_CLIENT                     | User has used the mobile client                                                                                                          | No <sup>1</sup> |
| 1 << 41     | DISABLED                                 | User's account is disabled                                                                                                               | No <sup>1</sup> |
| 1 << 43     | HAS\_SESSION\_STARTED                    | User has started at least one Gateway session and is now eligible to send messages                                                       | No <sup>1</sup> |
| 1 << 44     | QUARANTINED                              | User is quarantined and cannot create DMs or accept invites                                                                              | No              |
| 1 << 47     | PREMIUM\_ELIGIBLE\_FOR\_UNIQUE\_USERNAME | User is eligible for early access to [unique usernames](usuarios.md#create-unique-username)                                              | No <sup>1</sup> |
| 1 << 50     | COLLABORATOR                             | User is a collaborator and is considered staff                                                                                           | No              |
| 1 << 51     | RESTRICTED\_COLLABORATOR                 | User is a restricted collaborator and is considered staff                                                                                | No              |

<sup>1</sup> Not exposed to the API, can only be found in [user data harvests](usuarios.md#harvest-object).

[**Purchased Flags**](usuarios.md#purchased-flags)

Purchased flags denote what premium items a user has ever purchased. Visit the [Nitro](https://discord.com/nitro) page to learn more about the premium plans currently offered.

| Value  | Name               | Description                      |
| ------ | ------------------ | -------------------------------- |
| 1 << 0 | NITRO\_CLASSIC     | User has purchased Nitro classic |
| 1 << 1 | NITRO              | User has purchased regular Nitro |
| 1 << 2 | GUILD\_BOOST       | User has purchased a guild boost |
| 1 << 3 | NITRO\_BASIC       | User has purchased Nitro basic   |
| 1 << 4 | ON\_REVERSE\_TRIAL | User has a reverse trial active  |

[**Premium Usage Flags**](usuarios.md#premium-usage-flags)

Premium usage flags denote what premium (Nitro) features a user has utilized.

| Value  | Name                   | Description                              |
| ------ | ---------------------- | ---------------------------------------- |
| 1 << 0 | PREMIUM\_DISCRIMINATOR | User has utilized premium discriminators |
| 1 << 1 | ANIMATED\_AVATAR       | User has utilized animated avatars       |
| 1 << 2 | PROFILE\_BANNER        | User has utilized profile banners        |

[**Premium Type**](usuarios.md#premium-type)

Premium types denote the level of premium a user has. Visit the [Nitro](https://discord.com/nitro) page to learn more about the premium plans currently offered.

| Value | Name                  | Description   |
| ----- | --------------------- | ------------- |
| 0     | NONE **(deprecated)** | No Nitro      |
| 1     | TIER\_1               | Nitro Classic |
| 2     | TIER\_2               | Nitro         |
| 3     | TIER\_3               | Nitro Basic   |

[**Age Verification Status**](usuarios.md#age-verification-status)

| Value | Name            | Description                     |
| ----- | --------------- | ------------------------------- |
| 1     | UNVERIFIED      | User has not verified their age |
| 2     | VERIFIED\_TEEN  | User is a verified teenager     |
| 3     | VERIFIED\_ADULT | User is a verified adult        |

[**Required Action Type**](usuarios.md#required-action-type)

Denotes an action Discord requires the user to take before they can continue using the platform. In some cases, multiple actions may be required, and the user must complete all of them before they can continue using Discord.

| Value                                             | Description                                                                                                                                 | Action                                                                                                                                                                                                                               |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| AGREEMENTS                                        | The user must re-indicate their agreement of Discord's terms of service and privacy policy; this does not limit the user from using Discord | [Reaffirm agreements](usuarios.md#modify-user-agreements)                                                                                                                                                                            |
| ~~REQUIRE\_CAPTCHA~~                              | ~~The user must complete a reCAPTCHA challenge~~                                                                                            | [~~Complete a reCAPTCHA challenge~~](usuarios.md#verify-user-captcha)                                                                                                                                                                |
| REQUIRE\_VERIFIED\_EMAIL                          | The user must add and verify an email address to their account                                                                              | [Add an email address](https://docs.discord.food/topics/email-verification#adding-an-email-address)                                                                                                                                  |
| REQUIRE\_REVERIFIED\_EMAIL                        | The user must reverify their existing email address                                                                                         | [Reverify your email address](https://docs.discord.food/topics/email-verification#reverifying-your-email-address)                                                                                                                    |
| REQUIRE\_VERIFIED\_PHONE                          | The user must add a phone number to their account                                                                                           | [Add a phone number](https://docs.discord.food/topics/phone-verification#adding-a-phone-number)                                                                                                                                      |
| REQUIRE\_REVERIFIED\_PHONE                        | The user must reverify their existing phone number                                                                                          | [Reverify your phone number](https://docs.discord.food/topics/phone-verification#reverifying-your-phone-number)                                                                                                                      |
| ~~REQUIRE\_VERIFIED\_PHONE\_THEN\_EMAIL~~         | ~~The user must add a phone number to their account and then add and verify an email address to their account~~                             | [~~Add a phone number~~](https://docs.discord.food/topics/phone-verification#adding-a-phone-number) ~~and~~ [~~add an email address~~](https://docs.discord.food/topics/email-verification#adding-an-email-address)                  |
| REQUIRE\_VERIFIED\_EMAIL\_OR\_VERIFIED\_PHONE     | The user must add and verify an email address to their account or add a phone number to their account                                       | [Add an email address](https://docs.discord.food/topics/email-verification#adding-an-email-address) or [add a phone number](https://docs.discord.food/topics/phone-verification#adding-a-phone-number)                               |
| REQUIRE\_REVERIFIED\_EMAIL\_OR\_VERIFIED\_PHONE   | The user must reverify their existing email address or add a phone number to their account                                                  | [Reverify your email address](https://docs.discord.food/topics/email-verification#reverifying-your-email-address) or [add a phone number](https://docs.discord.food/topics/phone-verification#adding-a-phone-number)                 |
| REQUIRE\_VERIFIED\_EMAIL\_OR\_REVERIFIED\_PHONE   | The user must add and verify an email address to their account or reverify their existing phone number                                      | [Add an email address](https://docs.discord.food/topics/email-verification#adding-an-email-address) or [reverify your phone number](https://docs.discord.food/topics/phone-verification#reverifying-your-phone-number)               |
| REQUIRE\_REVERIFIED\_EMAIL\_OR\_REVERIFIED\_PHONE | The user must reverify their existing email address or reverify their existing phone number                                                 | [Reverify your email address](https://docs.discord.food/topics/email-verification#reverifying-your-email-address) or [reverify your phone number](https://docs.discord.food/topics/phone-verification#reverifying-your-phone-number) |

[**Example User**](usuarios.md#example-user)

```
{  "id": "80351110224678912",  "username": "nelly",  "global_name": "Nelly",  "avatar": "8342729096ea3675442027381ff50dfe",  "discriminator": "0",  "public_flags": 64,  "flags": 96,  "purchased_flags": 10,  "premium_usage_flags": 4,  "banner": "06c16474723fe537c283b8efa61a30c8",  "accent_color": null,  "bio": "I'm a bot!",  "locale": "en-US",  "nsfw_allowed": true,  "mfa_enabled": true,  "premium_type": 2,  "avatar_decoration_data": {    "sku_id": "1144058844004233369",    "asset": "a_fed43ab12698df65902ba06727e20c0e",    "expires_at": null  },  "email": "nelly@discord.com",  "verified": true,  "phone": "+18885940085",  "authenticator_types": [1, 2, 3],  "primary_guild": {    "identity_guild_id": "80351110224678913",    "identity_enabled": true,    "tag": "MEOW",    "badge": "7d1734ae5a615e82bc7a4033b98fade8"  }}
```

[**Example Partial User**](usuarios.md#example-partial-user)

```
{  "id": "80351110224678912",  "username": "nelly",  "avatar": "8342729096ea3675442027381ff50dfe",  "discriminator": "0",  "public_flags": 64,  "banner": "06c16474723fe537c283b8efa61a30c8",  "accent_color": 16711680,  "global_name": "Nelly",  "avatar_decoration_data": {    "sku_id": "1144058844004233369",    "asset": "a_fed43ab12698df65902ba06727e20c0e",    "expires_at": null  },  "primary_guild": {    "identity_guild_id": "80351110224678913",    "identity_enabled": true,    "tag": "MEOW",    "badge": "7d1734ae5a615e82bc7a4033b98fade8"  }}
```

#### [Avatar Decoration Data Object](usuarios.md#avatar-decoration-data-object) <a href="#avatar-decoration-data-object" id="avatar-decoration-data-object"></a>

A user's active avatar decoration.

[**Avatar Decoration Data Structure**](usuarios.md#avatar-decoration-data-structure)

| Field       | Type      | Description                                                                      |
| ----------- | --------- | -------------------------------------------------------------------------------- |
| asset       | string    | The [avatar decoration hash](https://docs.discord.food/reference#cdn-formatting) |
| sku\_id     | snowflake | The ID of the avatar decoration's SKU                                            |
| expires\_at | ?integer  | Unix timestamp of when the current avatar decoration expires                     |

[**Example Avatar Decoration Data**](usuarios.md#example-avatar-decoration-data)

```
{  "sku_id": "1144058844004233369",  "asset": "a_fed43ab12698df65902ba06727e20c0e",  "expires_at": 1740124800}
```

#### [Collectibles Object](usuarios.md#collectibles-object) <a href="#collectibles-object" id="collectibles-object"></a>

A user's equipped collectibles, excluding avatar decorations and profile effects.

[**Collectibles Structure**](usuarios.md#collectibles-structure)

| Field     | Type                                                           | Description                                                                                         |
| --------- | -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| nameplate | ?[nameplate data](usuarios.md#nameplate-data-structure) object | The user's [nameplate](https://support.discord.com/hc/en-us/articles/30408457944215-Nameplates-FAQ) |

[**Nameplate Data Structure**](usuarios.md#nameplate-data-structure)

| Field       | Type      | Description                                                                    |
| ----------- | --------- | ------------------------------------------------------------------------------ |
| asset       | string    | The [nameplate asset path](https://docs.discord.food/reference#cdn-formatting) |
| sku\_id     | snowflake | The ID of the nameplate's SKU                                                  |
| label       | string    | The nameplate's accessibility description                                      |
| palette     | string    | The nameplate's [color palette](usuarios.md#nameplate-color-palette)           |
| expires\_at | ?integer  | Unix timestamp of when the current nameplate expires                           |

[**Nameplate Color Palette**](usuarios.md#nameplate-color-palette)

| Value       | Name      |
| ----------- | --------- |
| none        | None      |
| crimson     | Crimson   |
| berry       | Berry     |
| sky         | Sky       |
| teal        | Teal      |
| forest      | Forest    |
| bubble\_gum | BubbleGum |
| violet      | Violet    |
| cobalt      | Cobalt    |
| clover      | Clover    |

[**Example Collectibles Object**](usuarios.md#example-collectibles-object)

```
{  "nameplate": {    "asset": "nameplates/nameplatetest/angel/",    "palette": "bubble_gum",    "label": "COLLECTIBLES_NAMEPLATETEST_ANGEL_A11Y",    "sku_id": "1344802364934062152",    "expires_at": null  }}
```

#### [Display Name Style Object](usuarios.md#display-name-style-object) <a href="#display-name-style-object" id="display-name-style-object"></a>

How a user's name gets displayed, such as font, colors, gradient, glow.

[**Display Name Style Structure**](usuarios.md#display-name-style-structure)

| Field      | Type            | Description                                                                                    |
| ---------- | --------------- | ---------------------------------------------------------------------------------------------- |
| font\_id   | integer         | The [font](usuarios.md#display-name-font) to use                                               |
| effect\_id | integer         | The [effect](usuarios.md#display-name-effect) to use                                           |
| colors     | array\[integer] | The colors to use encoded as an array of integers representing hexadecimal color codes (max 2) |

[**Display Name Font**](usuarios.md#display-name-font)

| Value | Name           | Description                                                          |
| ----- | -------------- | -------------------------------------------------------------------- |
| 11    | DEFAULT        | Default font                                                         |
| 1     | BANGERS        | [Bangers](https://fonts.google.com/specimen/Bangers)                 |
| 2     | BIO\_RHYME     | [BioRhyme](https://fonts.google.com/specimen/BioRhyme)               |
| 3     | CHERRY\_BOMB   | [Cherry Bomb One](https://fonts.google.com/specimen/Cherry+Bomb+One) |
| 4     | CHICLE         | [Chicle](https://fonts.google.com/specimen/Chicle)                   |
| 5     | COMPAGNON      | [Compagnon](https://velvetyne.fr/fonts/compagnon/)                   |
| 6     | MUSEO\_MODERNO | [MuseoModerno](https://fonts.google.com/specimen/MuseoModerno)       |
| 7     | NEO\_CASTEL    | [Néo-Castel](https://maxlilllo.gumroad.com/l/neo-castel)             |
| 8     | PIXELIFY       | [Pixelify Sans](https://fonts.google.com/specimen/Pixelify+Sans)     |
| 9     | RIBES          | [Ribes](https://www.collletttivo.it/typefaces/ribes)                 |
| 10    | SINISTRE       | [Sinistre](https://www.collletttivo.it/typefaces/sinistre)           |

[**Display Name Effect**](usuarios.md#display-name-effect)

| Value | Name     | Description                         |
| ----- | -------- | ----------------------------------- |
| 1     | SOLID    | Displays the first color provided   |
| 2     | GRADIENT | Two color gradient                  |
| 3     | NEON     | Glow around the name                |
| 4     | TOON     | Subtle vertical gradient and stroke |
| 5     | POP      | Colored dropshadow                  |

#### [Profile Metadata Object](usuarios.md#profile-metadata-object) <a href="#profile-metadata-object" id="profile-metadata-object"></a>

A user's profile metadata.

[**Profile Metadata Structure**](usuarios.md#profile-metadata-structure)

| Field                              | Type                                                                    | Description                                                                                               |
| ---------------------------------- | ----------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| guild\_id?                         | snowflake                                                               | The guild ID this profile applies to, if it is a guild profile                                            |
| pronouns                           | string                                                                  | The user's pronouns (max 40 characters)                                                                   |
| bio?                               | string                                                                  | The user's bio (max 190 characters)                                                                       |
| banner?                            | ?string                                                                 | The user's [banner hash](https://docs.discord.food/reference#cdn-formatting)                              |
| accent\_color? <sup>1</sup>        | ?integer                                                                | The user's banner color encoded as an integer representation of a hexadecimal color code                  |
| theme\_colors?                     | ?array\[integer, integer]                                               | The user's two theme colors encoded as an array of integers representing hexadecimal color codes          |
| popout\_animation\_particle\_type? | ?snowflake                                                              | The user's profile popout animation particle type                                                         |
| emoji?                             | ?[emoji](https://docs.discord.food/resources/emoji#emoji-object) object | The user's profile emoji                                                                                  |
| profile\_effect?                   | ?[profile effect](usuarios.md#profile-effect-structure) object          | The user's [profile effect](https://support.discord.com/hc/en-us/articles/17828465914263-Profile-Effects) |

<sup>1</sup> Not respected on guild profiles.

[**Profile Effect Structure**](usuarios.md#profile-effect-structure)

| Field       | Type      | Description                                               |
| ----------- | --------- | --------------------------------------------------------- |
| id          | snowflake | The ID of the profile effect                              |
| expires\_at | ?integer  | Unix timestamp of when the current profile effect expires |

[**Example Profile Metadata**](usuarios.md#example-profile-metadata)

```
{  "guild_id": "80351110224678913",  "pronouns": "gnarp/gnap",  "bio": "👽 Professional alien",  "banner": null,  "accent_color": null,  "theme_colors": [1, 1],  "popout_animation_particle_type": null,  "emoji": {    "name": "meowlien",    "roles": [],    "id": "1090395834966880336",    "require_colons": true,    "managed": false,    "animated": false,    "available": true  },  "profile_effect": {    "id": "1139323097930027068",    "expires_at": 1740124800  }}
```

#### [Authenticator Object](usuarios.md#authenticator-object) <a href="#authenticator-object" id="authenticator-object"></a>

[**Authenticator Structure**](usuarios.md#authenticator-structure)

| Field | Type   | Description                                                 |
| ----- | ------ | ----------------------------------------------------------- |
| id    | string | The ID of the authenticator                                 |
| type  | string | The [type of authenticator](usuarios.md#authenticator-type) |
| name  | string | The name of the authenticator                               |

[**Authenticator Type**](usuarios.md#authenticator-type)

Authenticator types represent enabled multi-factor authentication methods. See the [MFA verification documentation](https://docs.discord.food/authentication#mfa-verification) for more information.

| Value | Name     | Description                       |
| ----- | -------- | --------------------------------- |
| 1     | WEBAUTHN | WebAuthn credentials              |
| 2     | TOTP     | Time-based One-Time Password code |
| 3     | SMS      | SMS code                          |

[**Example Authenticator**](usuarios.md#example-authenticator)

```
{  "id": "1219430671865610261",  "type": 1,  "name": "AlienKey"}
```

#### [Backup Code Object](usuarios.md#backup-code-object) <a href="#backup-code-object" id="backup-code-object"></a>

A multi-factor authentication backup code.

[**Backup Code Structure**](usuarios.md#backup-code-structure)

| Field    | Type      | Description                           |
| -------- | --------- | ------------------------------------- |
| user\_id | snowflake | The ID of the user                    |
| code     | string    | The backup code                       |
| consumed | boolean   | Whether the backup code has been used |

[**Example Backup Code**](usuarios.md#example-backup-code)

```
{  "user_id": "852892297661906993",  "code": "zqs8oqxk",  "consumed": false}
```

#### [Harvest Object](usuarios.md#harvest-object) <a href="#harvest-object" id="harvest-object"></a>

A user's data harvest.

[**Harvest Structure**](usuarios.md#harvest-structure)

| Field             | Type                                                              | Description                                                                                                                 |
| ----------------- | ----------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| harvest\_id       | snowflake                                                         | The ID of the harvest                                                                                                       |
| user\_id          | snowflake                                                         | The ID of the user being harvested                                                                                          |
| email             | string                                                            | The email the harvest will be sent to                                                                                       |
| state             | string                                                            | The [state](usuarios.md#harvest-state) of the harvest                                                                       |
| status            | integer                                                           | The [status](usuarios.md#harvest-status) of the harvest                                                                     |
| created\_at       | ISO8601 timestamp                                                 | When the harvest was created                                                                                                |
| completed\_at     | ?ISO8601 timestamp                                                | When the harvest was completed                                                                                              |
| polled\_at        | ?ISO8601 timestamp                                                | When the harvest was last polled                                                                                            |
| backends          | map\[string, string]                                              | The [state](usuarios.md#harvest-backend-state) of each [backend](usuarios.md#harvest-backend-internal-type) being harvested |
| updated\_at       | ISO8601 timestamp                                                 | When the harvest was last updated                                                                                           |
| shadow\_run       | boolean                                                           | Whether the harvest is a shadow run                                                                                         |
| harvest\_metadata | [harvest metadata](usuarios.md#harvest-metadata-structure) object | Additional metadata about the harvest                                                                                       |

[**Example Harvest**](usuarios.md#example-harvest)

```
{  "harvest_id": "1319498748052639754",  "user_id": "852892297661906993",  "email": "alien@dolfi.es",  "state": "DELIVERED",  "status": 3,  "created_at": "2024-12-20T02:56:56.639579+00:00",  "completed_at": "2024-12-21T11:05:41.462828+00:00",  "polled_at": "2024-12-21T11:05:41.462828+00:00",  "backends": {    "ads": "EXTRACTED",    "users": "EXTRACTED",    "guilds": "EXTRACTED",    "hubspot": "EXTRACTED",    "messages": "EXTRACTED",    "analytics": "EXTRACTED",    "activities_e": "EXTRACTED",    "activities_w": "EXTRACTED"  },  "updated_at": "2024-12-21T11:05:41.462828+00:00",  "shadow_run": false,  "harvest_metadata": {    "user_is_staff": false,    "sla_email_sent": false,    "bypass_cooldown": false,    "is_provisional": false  }}
```

[**Harvest Metadata Structure**](usuarios.md#harvest-metadata-structure)

| Field            | Type    | Description                                                                                       |
| ---------------- | ------- | ------------------------------------------------------------------------------------------------- |
| user\_is\_staff  | boolean | Whether the user being harvested is a Discord employee                                            |
| sla\_email\_sent | boolean | Whether an email has been sent informing the user that the archive is taking longer than expected |
| bypass\_cooldown | boolean | Whether the harvest bypasses the cooldown period for requesting harvests                          |
| is\_provisional  | boolean | Whether the user being harvested is a provisional account                                         |

[**Harvest State**](usuarios.md#harvest-state)

| Value      | Description                                |
| ---------- | ------------------------------------------ |
| INCOMPLETE | The harvest is not yet complete            |
| DELIVERED  | The harvest has been delivered to the user |
| CANCELLED  | The harvest has been cancelled             |

[**Harvest Status**](usuarios.md#harvest-status)

| Value | Name      | Description                                    |
| ----- | --------- | ---------------------------------------------- |
| 0     | QUEUED    | The harvest is queued and has not been started |
| 1     | RUNNING   | The harvest is currently running               |
| 2     | FAILED    | The harvest has failed                         |
| 3     | COMPLETED | The harvest has completed successfully         |
| 4     | CANCELLED | The harvest has been cancelled                 |

[**Harvest Backend Internal Type**](usuarios.md#harvest-backend-internal-type)

| Value         | Description                                                                                                   |
| ------------- | ------------------------------------------------------------------------------------------------------------- |
| users         | All account information                                                                                       |
| analytics     | Actions the user has taken in Discord                                                                         |
| activities\_e | First-party embedded activity information                                                                     |
| activities\_w | First-party embedded activity information                                                                     |
| messages      | All user messages                                                                                             |
| hubspot       | Discord's [HubSpot](https://www.hubspot.com/) contact data, used for official Discord programs (e.g. partner) |
| guilds        | All guilds the user is currently a member of                                                                  |
| ads           | Quest data                                                                                                    |

[**Harvest Backend State**](usuarios.md#harvest-backend-state)

| Value     | Description                         |
| --------- | ----------------------------------- |
| INITIAL   | The backend has not been processed  |
| RUNNING   | The backend is currently processing |
| EXTRACTED | The backend has been processed      |

#### [User Survey Object](usuarios.md#user-survey-object) <a href="#user-survey-object" id="user-survey-object"></a>

A user survey.

[**User Survey Structure**](usuarios.md#user-survey-structure)

| Field               | Type                       | Description                                                                                      |
| ------------------- | -------------------------- | ------------------------------------------------------------------------------------------------ |
| id                  | snowflake                  | The ID of the survey                                                                             |
| key                 | snowflake                  | The ID of the survey                                                                             |
| prompt              | string                     | The title of the survey                                                                          |
| cta                 | string                     | The call-to-action text                                                                          |
| url                 | string                     | The URL to the survey                                                                            |
| guild\_requirements | array\[string]             | [User requirements](usuarios.md#survey-requirement-type) for the survey to be shown              |
| guild\_size         | array\[?integer, ?integer] | The guild member count requirements (min, max)                                                   |
| guild\_permissions  | array\[string]             | The [guild permissions bitwise value](https://docs.discord.food/topics/permissions) requirements |

[**Survey Requirement Type**](usuarios.md#survey-requirement-type)

| Value              | Description                                                                                              | Field               |
| ------------------ | -------------------------------------------------------------------------------------------------------- | ------------------- |
| IS\_OWNER          | The user must be the owner of a guild                                                                    | -                   |
| IS\_ADMIN          | The user must have the `ADMINISTRATOR` permission in any guild                                           | -                   |
| IS\_COMMUNITY      | The user must be in a guild with the [feature](https://docs.discord.food/resources/guild#guild-features) | -                   |
| GUILD\_SIZE        | The user must be in a guild with a member count in a given range                                         | `guild_size`        |
| GUILD\_SIZE\_ALL   | All guilds the user is in must have a member count in a given range                                      | `guild_size`        |
| IS\_HUB            | The user must be in a guild with the [feature](https://docs.discord.food/resources/guild#guild-features) | -                   |
| IS\_VIEWING        | The user must be currently viewing a guild                                                               | -                   |
| GUILD\_PERMISSIONS | The user must have the given permissions in any guild                                                    | `guild_permissions` |

[**Example User Survey**](usuarios.md#example-user-survey)

```
{  "id": "1301267751645483122",  "key": "1301267751645483122",  "prompt": "Share your experience with Discord",  "cta": "Take the survey!",  "url": "https://discord.sjc1.qualtrics.com/jfe/form/SV_123456",  "guild_requirements": [],  "guild_size": [null, null],  "guild_permissions": []}
```

#### [User Identity Verification Object](usuarios.md#user-identity-verification-object) <a href="#user-identity-verification-object" id="user-identity-verification-object"></a>

An Identity Verification used for application verification.

[**User Identity Verification Structure**](usuarios.md#user-identity-verification-structure)

| Field                      | Type      | Description                                                                                      |
| -------------------------- | --------- | ------------------------------------------------------------------------------------------------ |
| id                         | snowflake | The ID for the team verification                                                                 |
| status                     | number    | The current [status of the identity verification](usuarios.md#user-identity-verification-status) |
| last\_error                | ?number   | The [error code of the last verification attempt](usuarios.md#user-identity-verification-error)  |
| redirect\_url <sup>1</sup> | string    | The Stripe identity verification URL to redirect the user to                                     |

<sup>1</sup> Only returned on newly-created identity verification attempts.

[**User Identity Verification Status**](usuarios.md#user-identity-verification-status)

| Value | Name                     | Description                                                         |
| ----- | ------------------------ | ------------------------------------------------------------------- |
| 1     | REQUIRES\_ACTION         | User action is required to complete verification                    |
| 2     | PROCESSING               | The verification is currently in progress                           |
| 3     | CANCELED                 | The verification was cancelled before completion                    |
| 4     | SUCCEEDED                | The verification succeeded                                          |
| 5     | MANUALLY\_SUCCEEDED      | The verification was manually approved by staff                     |
| 6     | DELETED                  | The verification was deleted                                        |
| 7     | SUCCEEDED\_GRACE\_PERIOD | The verification is currently in a grace period before finalization |

[**User Identity Verification Error**](usuarios.md#user-identity-verification-error)

| Value | Name                                         | Description                                                                        |
| ----- | -------------------------------------------- | ---------------------------------------------------------------------------------- |
| 1     | CONSENT\_DECLINED                            | User declined consent at the beginning of verification                             |
| 2     | UNVERIFIED                                   | Verification was aborted or Stripe failed to verify the user's identity            |
| 3     | DEVICE\_UNSUPPORTED                          | The device used for verification is unsupported                                    |
| 4     | VERIFICATION\_DOCUMENT\_EXPIRED              | The submitted document has expired                                                 |
| 5     | VERIFICATION\_DOCUMENT\_INVALID              | The submitted document is invalid                                                  |
| 6     | VERIFICATION\_UNEXPECTED\_DOCUMENT\_COUNTRY  | The submitted document has an unexpected issuing country                           |
| 7     | VERIFICATION\_UNEXPECTED\_DOCUMENT\_TYPE     | The submitted document has an unexpected document type                             |
| 8     | VERIFICATION\_SCAN\_NOT\_READABLE            | The scan is not readable by the verification system                                |
| 9     | VERIFICATION\_SCAN\_MISSING\_BACK            | The scan is missing the back side of the document                                  |
| 10    | VERIFICATION\_SCAN\_ID\_TYPE\_NOT\_SUPPORTED | The scan contains a document type that is not supported by the verification system |
| 11    | VERIFICATION\_SCAN\_CORRUPT                  | The scan is incomplete or corrupted                                                |
| 12    | VERIFICATION\_SCAN\_FAILED\_COPY             | The verification system determined the scan is a copy of the original document     |
| 13    | VERIFICATION\_SCAN\_MANIPULATED\_DOCUMENT    | The verification system determined the document was manipulated or damaged with    |
| 14    | VERIFICATION\_SCAN\_FAILED\_GRAYSCALE        | The scan failed due to the uploaded document having been uploaded in grayscale     |
| 15    | VERIFICATION\_UNDER\_SUPPORTED\_AGE          | The user is under the minimum supported age for verification                       |

### [Endpoints](usuarios.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get Current User](usuarios.md#get-current-user) <a href="#get-current-user" id="get-current-user"></a>

`GET/users/@me`

Returns the [user](usuarios.md#user-object) object of the requester's account.

#### [Modify Current User](usuarios.md#modify-current-user) <a href="#modify-current-user" id="modify-current-user"></a>

`PATCH/users/@me`

Modifies the requester's user account settings. Returns a [user](usuarios.md#user-object) object with an extra field representing the user's new authorization token on success. Fires a [User Update](https://docs.discord.food/topics/gateway-events#user-update) Gateway event.

[**JSON Params**](usuarios.md#json-params)

| Field                              | Type                                                        | Description                                                                                                                                                               |
| ---------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| username? <sup>5</sup>             | string                                                      | The user's username (2-32 characters)                                                                                                                                     |
| discriminator? <sup>5</sup>        | string                                                      | The user's stringified 4-digit Discord tag; can only be changed for users with an applicable premium (Nitro) plan, which triggers a reroll after the subscription expires |
| global\_name? <sup>5</sup>         | ?string                                                     | The user's display name (1-32 characters)                                                                                                                                 |
| avatar?                            | ?[image data](https://docs.discord.food/reference#cdn-data) | The user's avatar; can be animated when the user has an applicable premium (Nitro) plan                                                                                   |
| avatar\_description?               | ?string                                                     | The description of the new user avatar, usually in the format "{filename}, added {date}" (max 1024 characters)                                                            |
| avatar\_id?                        | string                                                      | The ID of the recent avatar to use                                                                                                                                        |
| avatar\_decoration\_id?            | ?snowflake                                                  | The ID of the user's avatar decoration                                                                                                                                    |
| avatar\_decoration\_sku\_id?       | ?snowflake                                                  | The SKU ID of the user's avatar decoration                                                                                                                                |
| nameplate\_id?                     | ?snowflake                                                  | The ID of the user's nameplate                                                                                                                                            |
| nameplate\_sku\_id?                | ?snowflake                                                  | The SKU ID of the user's nameplate                                                                                                                                        |
| display\_name\_font\_id?           | ?integer                                                    | The [display name font](usuarios.md#display-name-font) to use                                                                                                             |
| display\_name\_effect\_id?         | ?integer                                                    | The [display name effect](usuarios.md#display-name-effect) to use                                                                                                         |
| display\_name\_colors?             | ?array\[integer]                                            | The display name colors to use encoded as an array of integers representing hexadecimal color codes (max 2)                                                               |
| email?                             | string                                                      | The user's email address; if changing from a verified email, `email_token` must be provided                                                                               |
| email\_token? <sup>4</sup>         | string                                                      | The user's email token from their previous email                                                                                                                          |
| pronouns?                          | ?string                                                     | The user's pronouns (max 40 characters)                                                                                                                                   |
| bio?                               | ?string                                                     | The user's bio (max 190 characters)                                                                                                                                       |
| banner?                            | ?[image data](https://docs.discord.food/reference#cdn-data) | The user's banner; can only be changed for premium (Nitro) users                                                                                                          |
| accent\_color?                     | ?integer                                                    | The user's banner color encoded as an integer representation of a hexadecimal color code                                                                                  |
| flags?                             | integer                                                     | The user's [flags](usuarios.md#user-flags) (only `PREMIUM_PROMO_DISMISSED` and `HAS_UNREAD_URGENT_MESSAGES` can be set)                                                   |
| date\_of\_birth? <sup>2</sup>      | ISO8601 timestamp                                           | The user's date of birth; can only be set once                                                                                                                            |
| password? <sup>1</sup>             | string                                                      | The user's current password; if the account does not have a password, this sets it                                                                                        |
| new\_password? <sup>3</sup>        | string                                                      | The user's new password (8-72 characters)                                                                                                                                 |
| push\_provider                     | string                                                      | The [push notification provider](https://docs.discord.food/topics/push-notifications#push-notification-provider) of the device                                            |
| push\_token                        | string                                                      | The push notification token to register                                                                                                                                   |
| push\_voip\_provider? <sup>6</sup> | string                                                      | The VOIP [push notification provider](https://docs.discord.food/topics/push-notifications#push-notification-provider) of the device                                       |
| push\_voip\_token? <sup>6</sup>    | string                                                      | The VOIP push notification token to register                                                                                                                              |

<sup>1</sup> Required for changing , , , , or .

<sup>2</sup> Setting this defines the field of the user based on whether they are over 18.

<sup>3</sup> Changing the account password invalidates all active tokens. Don't fret though, as the key in the response will be valid.

<sup>4</sup> This value can be obtained by requesting a verification code as outlined in the [email verification documentation](https://docs.discord.food/topics/email-verification#changing-your-email-address).

<sup>5</sup> If using unique usernames, the field must be unique across Discord, and cannot be changed. Else, the and fields must be unique across Discord, and changing the username may cause the discriminator to be randomized. See the [section on Discord's new username system](usuarios.md#unique-usernames) for more information. See the [Usernames and Nicknames section](usuarios.md#usernames-and-nicknames) for information on username restrictions.

<sup>6</sup> VOIP-specific push notification tokens are only used with PushKit on iOS.

#### [Modify Current User Account](usuarios.md#modify-current-user-account) <a href="#modify-current-user-account" id="modify-current-user-account"></a>

`PATCH/users/@me/account`

Modifies the requester's user account settings. Returns a partial [user](usuarios.md#user-object) object on success. Fires a [User Update](https://docs.discord.food/topics/gateway-events#user-update) Gateway event.

[**JSON Params**](usuarios.md#json-params)

| Field         | Type    | Description                               |
| ------------- | ------- | ----------------------------------------- |
| global\_name? | ?string | The user's display name (1-32 characters) |

#### [Get Recent Avatars](usuarios.md#get-recent-avatars) <a href="#get-recent-avatars" id="get-recent-avatars"></a>

`GET/users/@me/avatars`

Returns the user's recent avatars. For premium users, the six most recent avatars will be returned. Otherwise, only two will be returned.

[**Response Body**](usuarios.md#response-body)

| Field   | Type                                                  | Description                     |
| ------- | ----------------------------------------------------- | ------------------------------- |
| avatars | array\[[avatar](usuarios.md#avatar-structure) object] | The recent avatars for the user |

[**Avatar Structure**](usuarios.md#avatar-structure)

| Field         | Type    | Description                                                           |
| ------------- | ------- | --------------------------------------------------------------------- |
| id            | string  | The avatar ID                                                         |
| storage\_hash | string  | The [avatar hash](https://docs.discord.food/reference#cdn-formatting) |
| description   | ?string | The description specified when the avatar was uploaded                |

[**Example Response**](usuarios.md#example-response)

```
{  "avatars": [    {      "id": "1357011390585507910",      "storage_hash": "212aed0ac14cf7804051218f99624a9f",      "description": "alien, added April 2, 2025 at 5:09 PM"    }  ]}
```

#### [Delete Recent Avatar](usuarios.md#delete-recent-avatar) <a href="#delete-recent-avatar" id="delete-recent-avatar"></a>

`DELETE/users/@me/avatars/{avatar.id}`

Deletes a recent avatar for the user. Returns a 204 empty response on success.

#### [Get User](usuarios.md#get-user) <a href="#get-user" id="get-user"></a>

`GET/users/{user.id}`

Returns a partial [user](usuarios.md#user-object) object for a given user ID.

#### [Get User Profile](usuarios.md#get-user-profile) <a href="#get-user-profile" id="get-user-profile"></a>

`GET/users/{user.id}/profile`

Returns a user profile object for a given user ID.

[**Query String Params**](usuarios.md#query-string-params)

| Field                         | Type      | Description                                                                                        |
| ----------------------------- | --------- | -------------------------------------------------------------------------------------------------- |
| with\_mutual\_guilds?         | boolean   | Whether to include the mutual guilds of the user with the current user (default true)              |
| with\_mutual\_friends?        | boolean   | Whether to include mutual friends the user has with the current user (default false)               |
| with\_mutual\_friends\_count? | boolean   | Whether to include the number of mutual friends the user has with the current user (default false) |
| guild\_id?                    | snowflake | The guild ID to get the user's member profile in                                                   |
| connections\_role\_id?        | snowflake | The role ID to get the user's application role connection metadata in                              |
| join\_request\_id?            | snowflake | The join request ID to use for the request                                                         |

[**Response Body**](usuarios.md#response-body)

| Field                                             | Type                                                                                                                             | Description                                                                                  |
| ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| application?                                      | [profile application](usuarios.md#profile-application-structure) object                                                          | The bot's application profile                                                                |
| user                                              | partial [user](usuarios.md#user-object) object                                                                                   | The user object, with an extra `bio` key denoting the user's bio                             |
| user\_profile <sup>1</sup>                        | [profile metadata](usuarios.md#profile-metadata-object) object                                                                   | The user's profile metadata                                                                  |
| badges <sup>1</sup>                               | array\[[profile badge](usuarios.md#profile-badge-structure) object]                                                              | The user's profile badges                                                                    |
| guild\_member? <sup>1</sup>                       | [guild member](https://docs.discord.food/resources/guild#guild-member-object) object                                             | The guild member in the guild specified, with an extra `bio` denoting the guild member's bio |
| guild\_member\_profile? <sup>1</sup>              | [profile metadata](usuarios.md#profile-metadata-object) object                                                                   | The guild member's profile in the guild specified                                            |
| guild\_badges <sup>1</sup>                        | array\[[profile badge](usuarios.md#profile-badge-structure) objcet]                                                              | The guild member's guild-specific profile badges                                             |
| legacy\_username? <sup>1</sup> <sup>2</sup>       | ?string                                                                                                                          | The user's pre-migration `username#discriminator`, if applicable and shown                   |
| mutual\_guilds? <sup>1</sup>                      | array\[[mutual guild](usuarios.md#mutual-guild-structure) object]                                                                | The mutual guilds of the user with the current user                                          |
| mutual\_friends? <sup>1</sup> <sup>3</sup>        | array\[partial [user](usuarios.md#user-object) object]                                                                           | The mutual friends the user has with the current user                                        |
| mutual\_friends\_count? <sup>1</sup> <sup>3</sup> | integer                                                                                                                          | The number of mutual friends the user has with the current user                              |
| connected\_accounts                               | array\[partial [connection](https://docs.discord.food/resources/connected-accounts#connection-object) object]                    | The user's public connected accounts                                                         |
| application\_role\_connections?                   | array\[[application role connection](https://docs.discord.food/resources/application#application-role-connection-object) object] | The user's application role connections for the role specified                               |
| premium\_type <sup>1</sup>                        | ?integer                                                                                                                         | The [type of premium (Nitro) subscription](usuarios.md#premium-type) on a user's account     |
| premium\_since <sup>1</sup>                       | ?ISO8601 timestamp                                                                                                               | The date the user's premium (Nitro) subscription started                                     |
| premium\_guild\_since <sup>1</sup>                | ?ISO8601 timestamp                                                                                                               | The date the user's premium guild (boosting) subscription started                            |

<sup>1</sup> These fields are unexpectedly missing or if the user has blocked the current user.

<sup>2</sup> See the [section on Discord's new username system](usuarios.md#unique-usernames) for more information.

<sup>3</sup> This will always be an empty list / zero value for bots, even if the user has mutual friends with it.

[**Profile Application Structure**](usuarios.md#profile-application-structure)

| Field                               | Type                                                                                                                                                                      | Description                                                                                                                                              |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id                                  | snowflake                                                                                                                                                                 | The ID of the application                                                                                                                                |
| flags                               | integer                                                                                                                                                                   | The [application's flags](https://docs.discord.food/resources/application#application-flags)                                                             |
| verified                            | boolean                                                                                                                                                                   | Whether the application is verified                                                                                                                      |
| storefront\_available               | boolean                                                                                                                                                                   | Whether the application has monetization enabled (i.e. subscriptions or products available for purchase)                                                 |
| primary\_sku\_id?                   | snowflake                                                                                                                                                                 | The ID of the application's primary SKU (game, application subscription, etc.)                                                                           |
| install\_params?                    | [application install params](https://docs.discord.food/resources/application#application-install-params-object) object                                                    | The default in-app authorization link for the integration                                                                                                |
| integration\_types\_config?         | map\[integer, ?[application integration type configuration](https://docs.discord.food/resources/application#application-integration-type-configuration-structure) object] | The configuration for each [integration type](https://docs.discord.food/resources/application#application-integration-type) supported by the application |
| popular\_application\_command\_ids? | array\[snowflake]                                                                                                                                                         | The IDs of the application's most popular application commands (max 5)                                                                                   |
| custom\_install\_url?               | string                                                                                                                                                                    | The default custom authorization link for the integration                                                                                                |

[**Profile Badge Structure**](usuarios.md#profile-badge-structure)

For a list of known profile badges, refer to [this Gist](https://gist.github.com/XYZenix/c45156b7c883b5301c9028e39d71b479).

| Field       | Type   | Description                                                                 |
| ----------- | ------ | --------------------------------------------------------------------------- |
| id          | string | The reference ID of the badge                                               |
| description | string | A description of the badge                                                  |
| icon        | string | The badge's [icon hash](https://docs.discord.food/reference#cdn-formatting) |
| link?       | string | A link representing the badge                                               |

[**Mutual Guild Structure**](usuarios.md#mutual-guild-structure)

| Field | Type      | Description                      |
| ----- | --------- | -------------------------------- |
| id    | snowflake | The guild ID                     |
| nick  | ?string   | The user's nickname in the guild |

[**Example Response**](usuarios.md#example-response)

```
{  "user": {    "id": "852892297661906993",    "username": "alien",    "global_name": "Alien",    "avatar": "9d52298a3ad006da31ac66a86230d9f2",    "avatar_decoration_data": null,    "discriminator": "0",    "public_flags": 64,    "flags": 64,    "banner": "a_17a0757cf6121ccc07546de9bff3edb2",    "accent_color": null,    "bio": "👽 Professional smoothbrain",    "avatar_decoration_data": null,    "primary_guild": null  },  "connected_accounts": [    {      "type": "twitter",      "id": "123456",      "name": "discord",      "verified": true,      "metadata": {        "verified": "1",        "followers_count": "100000",        "statuses_count": "100000",        "created_at": "2016-01-01T00:00:00"      }    }  ],  "premium_since": "2016-01-01T00:00:00.00+00:00",  "premium_type": 2,  "premium_guild_since": "2016-01-01T00:00:00.00+00:00",  "mutual_friends_count": 100,  "mutual_guilds": [    {      "id": "80351110224678913",      "nick": "Liena"    }  ],  "guild_member": {    "avatar": null,    "communication_disabled_until": null,    "unusual_dm_activity_until": null,    "flags": 0,    "joined_at": "2016-01-01T00:00:00.00+00:00",    "nick": null,    "pending": false,    "premium_since": "2016-01-01T00:00:00.00+00:00",    "roles": [],    "user": {      "id": "852892297661906993",      "username": "alien",      "global_name": "Alien",      "avatar": "9d52298a3ad006da31ac66a86230d9f2",      "discriminator": "0",      "public_flags": 4194368,      "avatar_decoration_data": null,      "primary_guild": null    },    "bio": "👽 Professional alien",    "banner": null,    "mute": false,    "deaf": false  },  "application_role_connections": [    {      "platform_name": "Aliens United",      "platform_username": "Alien",      "metadata": {        "real": "1",        "certified": "1"      },      "application": {        "id": "891436233903964161",        "name": "Lightbulb",        "icon": "4d47160ec8c45f22e2bdbe75ac3e1bbd",        "description": "<:support_icon:853084466016288828> Imagine a bot.",        "summary": "",        "type": null,        "bot": {          "id": "891436233903964161",          "username": "lightbulb",          "global_name": "Lightbulb",          "avatar": "59fb354bf144ed784aa8bdef88d135bb",          "avatar_decoration_data": null,          "discriminator": "0",          "public_flags": 0,          "bot": true        }      },      "application_metadata": {        "real": {          "type": 7,          "key": "real",          "name": "Real",          "description": "Are you real alier?"        },        "certified": {          "type": 7,          "key": "certified",          "name": "Certified",          "description": "Are you certified alier?"        }      }    }  ],  "user_profile": {    "bio": "👽 Professional smoothbrain",    "accent_color": null,    "pronouns": "gnarp/gnap",    "banner": "a_17a0757cf6121ccc07546de9bff3edb2",    "theme_colors": [1, 1],    "popout_animation_particle_type": 100000,    "emoji": null,    "profile_effect": {      "id": "1139323097930027068",      "expires_at": null    }  },  "guild_member_profile": {    "guild_id": "80351110224678913",    "pronouns": "",    "bio": "👽 Professional alien",    "banner": null,    "accent_color": null,    "theme_colors": [1, 1],    "popout_animation_particle_type": null,    "emoji": null,    "profile_effect": {      "id": "1139323097930027068",      "expires_at": null    }  }}
```

#### [Modify User Profile](usuarios.md#modify-user-profile) <a href="#modify-user-profile" id="modify-user-profile"></a>

`PATCH/users/@me/profile`

Modifies the current user's profile. Returns the updated [profile metadata](usuarios.md#profile-metadata-object) object on success. Fires a [User Update](https://docs.discord.food/topics/gateway-events#user-update) Gateway event.

[**JSON Params**](usuarios.md#json-params)

| Field                              | Type                                                        | Description                                                                                      |
| ---------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| pronouns?                          | ?string                                                     | The user's pronouns (max 40 characters)                                                          |
| bio?                               | ?string                                                     | The user's bio (max 190 characters)                                                              |
| banner?                            | ?[image data](https://docs.discord.food/reference#cdn-data) | The user's banner; can only be changed for premium (Nitro) users                                 |
| accent\_color?                     | ?integer                                                    | The user's banner color encoded as an integer representation of a hexadecimal color code         |
| theme\_colors?                     | ?array\[integer, integer]                                   | The user's two theme colors encoded as an array of integers representing hexadecimal color codes |
| popout\_animation\_particle\_type? | ?snowflake                                                  | The user's profile popout animation particle type                                                |
| emoji\_id?                         | ?snowflake                                                  | The user's profile emoji ID                                                                      |
| profile\_effect\_id?               | ?snowflake                                                  | The user's profile effect ID                                                                     |

#### [Get Mutual Relationships](usuarios.md#get-mutual-relationships) <a href="#get-mutual-relationships" id="get-mutual-relationships"></a>

`GET/users/{user.id}/relationships`

Returns a list of partial [user](usuarios.md#user-object) objects that are friends with the user and current user.

#### [Enable TOTP MFA](usuarios.md#enable-totp-mfa) <a href="#enable-totp-mfa" id="enable-totp-mfa"></a>

`POST/users/@me/mfa/totp/enable`

Enables TOTP multi-factor authentication for the current user. Fires a [User Update](https://docs.discord.food/topics/gateway-events#user-update) Gateway event.

[**JSON Params**](usuarios.md#json-params)

| Field    | Type   | Description                                       |
| -------- | ------ | ------------------------------------------------- |
| password | string | The user's password                               |
| secret?  | string | The generated TOTP secret (32 characters)         |
| code?    | string | The TOTP code to verify the secret (6 characters) |

[**Response Body**](usuarios.md#response-body)

| Field         | Type                                                         | Description                                 |
| ------------- | ------------------------------------------------------------ | ------------------------------------------- |
| token         | string                                                       | The new authorization token for the session |
| backup\_codes | array\[[backup code](usuarios.md#backup-code-object) object] | MFA backup codes                            |

#### [Disable TOTP MFA](usuarios.md#disable-totp-mfa) <a href="#disable-totp-mfa" id="disable-totp-mfa"></a>

`POST/users/@me/mfa/totp/disable`

Disables TOTP multi-factor authentication for the current user. Fires a [User Update](https://docs.discord.food/topics/gateway-events#user-update) Gateway event.

[**Response Body**](usuarios.md#response-body)

| Field | Type   | Description                                 |
| ----- | ------ | ------------------------------------------- |
| token | string | The new authorization token for the session |

#### [Enable SMS MFA](usuarios.md#enable-sms-mfa) <a href="#enable-sms-mfa" id="enable-sms-mfa"></a>

`POST/users/@me/mfa/sms/enable`

Enables SMS multi-factor authentication for the current user. Requires that TOTP-based MFA is already enabled and the user has a verified phone number. Returns a 204 empty response on success. Fires a [User Update](https://docs.discord.food/topics/gateway-events#user-update) Gateway event.

[**JSON Params**](usuarios.md#json-params)

| Field    | Type   | Description         |
| -------- | ------ | ------------------- |
| password | string | The user's password |

#### [Disable SMS MFA](usuarios.md#disable-sms-mfa) <a href="#disable-sms-mfa" id="disable-sms-mfa"></a>

`POST/users/@me/mfa/sms/disable`

Disables SMS multi-factor authentication for the current user. Returns a 204 empty response on success. Fires a [User Update](https://docs.discord.food/topics/gateway-events#user-update) Gateway event.

[**JSON Params**](usuarios.md#json-params)

| Field    | Type   | Description         |
| -------- | ------ | ------------------- |
| password | string | The user's password |

#### [Get WebAuthn Authenticators](usuarios.md#get-webauthn-authenticators) <a href="#get-webauthn-authenticators" id="get-webauthn-authenticators"></a>

`GET/users/@me/mfa/webauthn/credentials`

Returns a list of [WebAuthn authenticator](usuarios.md#authenticator-object) objects for the current user.

#### [Create WebAuthn Authenticator](usuarios.md#create-webauthn-authenticator) <a href="#create-webauthn-authenticator" id="create-webauthn-authenticator"></a>

`POST/users/@me/mfa/webauthn/credentials`

Creates a WebAuthn authenticator for the current user. Fires [User Update](https://docs.discord.food/topics/gateway-events#user-update) and [Authenticator Create](https://docs.discord.food/topics/gateway-events#authenticator-create) Gateway events once the authenticator is created.

[**JSON Params**](usuarios.md#json-params)

| Field       | Type   | Description                                                                                                                                    |
| ----------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| name?       | string | The name of the authenticator (1-32 characters)                                                                                                |
| ticket?     | string | The MFA ticket returned from the same endpoint                                                                                                 |
| credential? | string | A stringified JSON object of the [public key credential response](https://developer.mozilla.org/en-US/docs/Web/API/PublicKeyCredential/toJSON) |

[**Response Body**](usuarios.md#response-body)

| Field                      | Type                                                         | Description                                                                                                                                                              |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| ticket <sup>1</sup>        | string                                                       | The MFA ticket                                                                                                                                                           |
| challenge <sup>1</sup>     | string                                                       | The stringified JSON [public key credential request options](https://developer.mozilla.org/en-US/docs/Web/API/CredentialsContainer/get#web_authentication_api) challenge |
| id <sup>2</sup>            | string                                                       | The ID of the authenticator                                                                                                                                              |
| type <sup>2</sup>          | string                                                       | The [type of authenticator](usuarios.md#authenticator-type) (always `WEBAUTHN`)                                                                                          |
| name <sup>2</sup>          | string                                                       | The name of the authenticator                                                                                                                                            |
| backup\_codes <sup>2</sup> | array\[[backup code](usuarios.md#backup-code-object) object] | MFA backup codes                                                                                                                                                         |

<sup>1</sup> Only returned when no parameters are provided.

<sup>2</sup> Only returned when parameters are provided.

[**Example Response (Ticket)**](usuarios.md#example-response-\(ticket\))

```
{  "ticket": "ODUyODkyMjk3NjYxOTA2OTkz.H2Rpq0.WrhGhYEhM3lHUPN61xF6JcQKwVutk8fBvcoHjo",  "challenge": "{\"publicKey\":{\"challenge\":\"a8a1cHP7_zYheggFG68zKUkl8DwnEqfKvPE-GOMvhss\",\"timeout\":60000,\"rpId\":\"discord.com\",\"allowCredentials\":[{\"type\":\"public-key\",\"id\":\"izrvF80ogrfg9dC3RmWWwW1VxBVBG0TzJVXKOJl__6FvMa555dH4Trt2Ub8AdHxNLkQsc0unAGcn4-hrJHDKSO\"}],\"userVerification\":\"preferred\"}}"}
```

[**Example Response (Authenticator)**](usuarios.md#example-response-\(authenticator\))

```
{  "id": "1219430671865610261",  "type": 1,  "name": "AlienKey",  "backup_codes": [    {      "user_id": "852892297661906993",      "code": "zqs8oqxk",      "consumed": false    }  ]}
```

#### [Modify WebAuthn Authenticator](usuarios.md#modify-webauthn-authenticator) <a href="#modify-webauthn-authenticator" id="modify-webauthn-authenticator"></a>

`PATCH/users/@me/mfa/webauthn/credentials/{authenticator.id}`

Modifies the given WebAuthn authenticator. Returns the updated [authenticator](usuarios.md#authenticator-object) object on success. Fires an [Authenticator Update](https://docs.discord.food/topics/gateway-events#authenticator-update) Gateway event.

[**JSON Params**](usuarios.md#json-params)

| Field | Type   | Description                                     |
| ----- | ------ | ----------------------------------------------- |
| name? | string | The name of the authenticator (1-32 characters) |

#### [Delete WebAuthn Authenticator](usuarios.md#delete-webauthn-authenticator) <a href="#delete-webauthn-authenticator" id="delete-webauthn-authenticator"></a>

`DELETE/users/@me/mfa/webauthn/credentials/{authenticator.id}`

Deletes the given WebAuthn authenticator. Returns a 204 empty response on success. Fires [User Update](https://docs.discord.food/topics/gateway-events#user-update) and [Authenticator Delete](https://docs.discord.food/topics/gateway-events#authenticator-delete) Gateway events.

#### [Send Backup Codes Challenge](usuarios.md#send-backup-codes-challenge) <a href="#send-backup-codes-challenge" id="send-backup-codes-challenge"></a>

`POST/auth/verify/view-backup-codes-challenge`

Sends an email to the current user with a verification code that allows them to view their backup codes. Returns a 204 empty response on success.

[**JSON Params**](usuarios.md#json-params)

| Field    | Type   | Description         |
| -------- | ------ | ------------------- |
| password | string | The user's password |

[**Response Body**](usuarios.md#response-body)

| Field             | Type   | Description                                                         |
| ----------------- | ------ | ------------------------------------------------------------------- |
| nonce             | string | The one-time verification nonce used to view the backup codes       |
| regenerate\_nonce | string | The one-time verification nonce used to regenerate the backup codes |

#### [Get Backup Codes](usuarios.md#get-backup-codes) <a href="#get-backup-codes" id="get-backup-codes"></a>

`POST/users/@me/mfa/codes-verification`

Returns the user's MFA backup codes.

[**JSON Params**](usuarios.md#json-params)

| Field                           | Type    | Description                                                              |
| ------------------------------- | ------- | ------------------------------------------------------------------------ |
| key <sup>1</sup>                | string  | The backup code verification key received in the email                   |
| nonce <sup>1</sup> <sup>2</sup> | string  | The one-time verification nonce used to view/regenerate the backup codes |
| regenerate <sup>2</sup>         | boolean | Whether to regenerate the backup codes                                   |

<sup>1</sup> This value can be obtained by requesting a verification code with the [Send Backup Codes Challenge](usuarios.md#send-backup-codes-challenge) endpoint.

<sup>2</sup> The nonce used must correspond to the action being performed. Each action can only be performed once.

[**Response Body**](usuarios.md#response-body)

| Field         | Type                                                         | Description      |
| ------------- | ------------------------------------------------------------ | ---------------- |
| backup\_codes | array\[[backup code](usuarios.md#backup-code-object) object] | MFA backup codes |

[**Example Response**](usuarios.md#example-response)

```
{  "backup_codes": [    {      "user_id": "852892297661906993",      "code": "zqs8oqxk",      "consumed": false    }  ]}
```

#### [Disable User Account](usuarios.md#disable-user-account) <a href="#disable-user-account" id="disable-user-account"></a>

`POST/users/@me/disable`

Disables the current user's account. Invalidates all active tokens. Returns a 204 empty response on success.

[**JSON Params**](usuarios.md#json-params)

| Field    | Type   | Description         |
| -------- | ------ | ------------------- |
| password | string | The user's password |

#### [Delete User Account](usuarios.md#delete-user-account) <a href="#delete-user-account" id="delete-user-account"></a>

`POST/users/@me/delete`

Marks the current user's account for deletion. Invalidates all active tokens. Returns a 204 empty response on success.

[**JSON Params**](usuarios.md#json-params)

| Field    | Type    | Description                 |
| -------- | ------- | --------------------------- |
| password | ?string | The user's password, if any |

#### [Verify User Captcha](usuarios.md#verify-user-captcha) <a href="#verify-user-captcha" id="verify-user-captcha"></a>

`POST/users/@me/captcha/verify`

Verifies a reCAPTCHA solution when needed by the [required action](usuarios.md#required-action-type). Returns a 204 empty response on success. Fires a [User Required Action Update](https://docs.discord.food/topics/gateway-events#user-required-action-update) Gateway event.

[**reCAPTCHA Site Key**](usuarios.md#recaptcha-site-key)

```
6Lef5iQTAAAAAKeIvIY-DeexoO3gj7ryl9rLMEnn
```

[**JSON Params**](usuarios.md#json-params)

| Field        | Type   | Description            |
| ------------ | ------ | ---------------------- |
| captcha\_key | string | The reCAPTCHA solution |

#### [Modify User Agreements](usuarios.md#modify-user-agreements) <a href="#modify-user-agreements" id="modify-user-agreements"></a>

`patch/users/@me/agreements`

Reaffirms the user's agreements to Discord's [Terms of Service](https://discord.com/terms) and [Privacy Policy](https://discord.com/privacy) when needed by the [required action](usuarios.md#required-action-type), which is assigned when a policy change occurs. Returns a 204 empty response on success. Fires a [User Required Action Update](https://docs.discord.food/topics/gateway-events#user-required-action-update) Gateway event.

[**JSON Params**](usuarios.md#json-params)

| Field    | Type    | Description                                     |
| -------- | ------- | ----------------------------------------------- |
| terms?   | boolean | Whether the user agrees to the Terms of Service |
| privacy? | boolean | Whether the user agrees to the Privacy Policy   |

#### [Get Unique Username Suggestions](usuarios.md#get-unique-username-suggestions) <a href="#get-unique-username-suggestions" id="get-unique-username-suggestions"></a>

`GET/users/@me/pomelo-suggestions`

Returns a suggested unique username string based on the current user's username.

[**Response Body**](usuarios.md#response-body)

| Field    | Type   | Description            |
| -------- | ------ | ---------------------- |
| username | string | The suggested username |

[**Example Response**](usuarios.md#example-response)

```
{ "username": "gnarp.gnap" }
```

#### [Get Unique Username Eligibility](usuarios.md#get-unique-username-eligibility) <a href="#get-unique-username-eligibility" id="get-unique-username-eligibility"></a>

`POST/users/@me/pomelo-attempt`

Checks whether a unique username is available for the user to claim.

[**JSON Params**](usuarios.md#json-params)

| Field    | Type   | Description           |
| -------- | ------ | --------------------- |
| username | string | The username to check |

[**Response Body**](usuarios.md#response-body)

| Field | Type     | Description                   |
| ----- | -------- | ----------------------------- |
| taken | ?boolean | Whether the username is taken |

[**Example Response**](usuarios.md#example-response)

```
{ "taken": true }
```

#### [Create Unique Username](usuarios.md#create-unique-username) <a href="#create-unique-username" id="create-unique-username"></a>

`POST/users/@me/pomelo`

Claims a unique username for the user. Returns the updated [user](usuarios.md#user-object) object on success. Fires a [User Update](https://docs.discord.food/topics/gateway-events#user-update) Gateway event.

[**JSON Params**](usuarios.md#json-params)

| Field    | Type   | Description           |
| -------- | ------ | --------------------- |
| username | string | The username to claim |

#### [Set Guild Identity](usuarios.md#set-guild-identity) <a href="#set-guild-identity" id="set-guild-identity"></a>

`PUT/users/@me/clan`

Sets the current user's primary guild. Returns a [user](usuarios.md#user-object) object on success. Fires a [User Update](https://docs.discord.food/topics/gateway-events#user-update) Gateway event.

[**JSON Params**](usuarios.md#json-params)

| Field                | Type       | Description                                         |
| -------------------- | ---------- | --------------------------------------------------- |
| identity\_enabled?   | ?boolean   | Whether the user has enabled the feature            |
| identity\_guild\_id? | ?snowflake | The ID of the guild whose identity is being adopted |

#### [Get Recent Mentions](usuarios.md#get-recent-mentions) <a href="#get-recent-mentions" id="get-recent-mentions"></a>

`GET/users/@me/mentions`

Returns a list of [message](https://docs.discord.food/resources/message#message-object) objects that the current user has been mentioned in during the past 7 days.

[**Query String Params**](usuarios.md#query-string-params)

| Field      | Type      | Description                                                    |
| ---------- | --------- | -------------------------------------------------------------- |
| before?    | snowflake | Get messages before this message ID                            |
| limit?     | integer   | Max number of messages to return (1-100, default 25)           |
| guild\_id? | snowflake | The guild to limit returned messages by                        |
| roles?     | boolean   | Whether to include role mentions (default true)                |
| everyone?  | boolean   | Whether to include @everyone and @here mentions (default true) |

#### [Delete Recent Mention](usuarios.md#delete-recent-mention) <a href="#delete-recent-mention" id="delete-recent-mention"></a>

`DELETE/users/@me/mentions/{message.id}`

Acknowledges a message the current user has been mentioned in. Returns a 204 empty response on success. Fires a [Recent Mention Delete](https://docs.discord.food/topics/gateway-events#recent-mention-delete) Gateway event.

#### [Get User Harvest](usuarios.md#get-user-harvest) <a href="#get-user-harvest" id="get-user-harvest"></a>

`GET/users/@me/harvest`

If it exists, returns a [harvest](usuarios.md#harvest-object) object representing the current user's most recent user data harvest request. Otherwise, returns a 204 empty response.

#### [Create User Harvest](usuarios.md#create-user-harvest) <a href="#create-user-harvest" id="create-user-harvest"></a>

`POST/users/@me/harvest`

Creates a user data harvest request for the current user. Returns a [harvest](usuarios.md#harvest-object) object on success.

[**JSON Params**](usuarios.md#json-params)

| Field              | Type            | Description                                         |
| ------------------ | --------------- | --------------------------------------------------- |
| backends?          | ?array\[string] | The types of user data being requested <sup>1</sup> |
| email <sup>2</sup> | string          | The email address to send the harvest to            |

<sup>1</sup> Invalid options are ignored. If the array contains no valid values, all data types are requested.

<sup>2</sup> Only applicable in OAuth2 contexts.

[**Harvest Backend Type**](usuarios.md#harvest-backend-type)

See [the official support page](https://support.discord.com/hc/en-us/articles/360004957991) for more information.

| Value      | Description                                  |
| ---------- | -------------------------------------------- |
| Accounts   | All account information                      |
| Ads        | Quest data                                   |
| Analytics  | Actions the user has taken in Discord        |
| Activities | First-party embedded activity information    |
| Messages   | All user messages                            |
| Programs   | Official Discord programs (e.g. partner)     |
| Servers    | All guilds the user is currently a member of |

#### [Get User Survey](usuarios.md#get-user-survey) <a href="#get-user-survey" id="get-user-survey"></a>

`GET/users/@me/survey`

Returns the current user's active survey.

[**Query String Params**](usuarios.md#query-string-params)

| Field                          | Type      | Description                                                                 |
| ------------------------------ | --------- | --------------------------------------------------------------------------- |
| disable\_auto\_seen?           | boolean   | Whether to prevent automatically marking the survey as seen (default false) |
| survey\_override? <sup>1</sup> | snowflake | The ID of the survey to return                                              |

<sup>1</sup> Only usable by Discord employees.

[**Response Body**](usuarios.md#response-body)

| Field  | Type                                                  | Description                      |
| ------ | ----------------------------------------------------- | -------------------------------- |
| survey | ?[user survey](usuarios.md#user-survey-object) object | The user's active survey, if any |

#### [Acknowledge User Survey](usuarios.md#acknowledge-user-survey) <a href="#acknowledge-user-survey" id="acknowledge-user-survey"></a>

`POST/users/@me/survey/{survey.id}/seen`

Marks a user survey as seen. Returns a 204 empty response on success.

#### [Get User Notes](usuarios.md#get-user-notes) <a href="#get-user-notes" id="get-user-notes"></a>

`GET/users/@me/notes`

Returns a mapping of user IDs to notes for the current user.

[**Example Response**](usuarios.md#example-response)

```
{  "852892297661906993": "This is a note",  "787017887877169173": "This is another note"}
```

#### [Get User Note](usuarios.md#get-user-note) <a href="#get-user-note" id="get-user-note"></a>

`GET/users/@me/notes/{user.id}`

Returns the note for the given user.

[**Response Body**](usuarios.md#response-body)

| Field          | Type      | Description                                                       |
| -------------- | --------- | ----------------------------------------------------------------- |
| note           | string    | The note (max 256 characters)                                     |
| note\_user\_id | snowflake | The ID of the user the note is on                                 |
| user\_id       | snowflake | The ID of the user who created the note (always the current user) |

[**Example Response**](usuarios.md#example-response)

```
{  "note": "This is a note",  "note_user_id": "787017887877169173",  "user_id": "852892297661906993"}
```

#### [Modify User Note](usuarios.md#modify-user-note) <a href="#modify-user-note" id="modify-user-note"></a>

`PUT/users/@me/notes/{user.id}`

Sets the note for the given user. Returns a 204 empty response on success. Fires a [User Note Update](https://docs.discord.food/topics/gateway-events#user-note-update) Gateway event.

[**JSON Params**](usuarios.md#json-params)

| Field | Type    | Description                   |
| ----- | ------- | ----------------------------- |
| note  | ?string | The note (max 256 characters) |

#### [Get User Affinities](usuarios.md#get-user-affinities) <a href="#get-user-affinities" id="get-user-affinities"></a>

`GET/users/@me/affinities/users`

Returns the current user's affinity scores for other users. Affinity scores are a measure of how likely a user is to be friends with another user.

[**Response Body**](usuarios.md#response-body)

| Field            | Type                                                                | Description                                |
| ---------------- | ------------------------------------------------------------------- | ------------------------------------------ |
| user\_affinities | array\[[user affinity](usuarios.md#user-affinity-structure) object] | The user's affinity scores for other users |

[**User Affinity Structure**](usuarios.md#user-affinity-structure)

| Field    | Type      | Description        |
| -------- | --------- | ------------------ |
| user\_id | snowflake | The user's ID      |
| affinity | float     | The affinity score |

#### [Get User Affinities v2](usuarios.md#get-user-affinities-v2) <a href="#get-user-affinities-v2" id="get-user-affinities-v2"></a>

`GET/users/@me/affinities/v2/users`

Returns more detailed user affinity scores for the current user.

[**Response Body**](usuarios.md#response-body)

| Field            | Type                                                                      | Description                                |
| ---------------- | ------------------------------------------------------------------------- | ------------------------------------------ |
| user\_affinities | array\[[user affinity v2](usuarios.md#user-affinity-v2-structure) object] | The user's affinity scores for other users |

[**User Affinity v2 Structure**](usuarios.md#user-affinity-v2-structure)

| Field                        | Type      | Description                                                            |
| ---------------------------- | --------- | ---------------------------------------------------------------------- |
| other\_user\_id              | snowflake | The user's ID                                                          |
| user\_segment                | string    | The [usage segment](usuarios.md#user-segment-type) of the current user |
| other\_user\_segment         | string    | The [usage segment](usuarios.md#user-segment-type) of the user         |
| is\_friend                   | boolean   | Whether the user is a friend                                           |
| dm\_probability              | float     | The affinity score for direct messaging                                |
| dm\_rank                     | integer   | The rank of the direct message affinity                                |
| vc\_probability              | float     | The affinity score for voice calling                                   |
| vc\_rank                     | integer   | The rank of the voice call affinity                                    |
| server\_message\_probability | float     | The affinity score for guild messaging                                 |
| server\_message\_rank        | integer   | The rank of the guild message affinity                                 |
| communication\_probability   | float     | The overall communication affinity score                               |
| communication\_rank          | integer   | The rank of the overall communication affinity                         |

[**User Segment Type**](usuarios.md#user-segment-type)

| Value         | Description                                  |
| ------------- | -------------------------------------------- |
| HFU\_MAU      | High Frequency User, Monthly Active User     |
| NON\_HFU\_MAU | Non-High Frequency User, Monthly Active User |
| NON\_MAU      | Non-Monthly Active User                      |

[**Example User Affinities v2**](usuarios.md#example-user-affinities-v2)

```
{  "other_user_id": "1001086404203389018",  "user_segment": "HFU_MAU",  "other_user_segment": "HFU_MAU",  "is_friend": true,  "dm_probability": 0.869776725769043,  "dm_rank": 1,  "vc_probability": 0.004896213300526142,  "vc_rank": 4,  "server_message_probability": 0.846949577331543,  "server_message_rank": 6,  "communication_probability": 0.573874172133704,  "communication_rank": 1}
```

#### [Get Guild Affinities](usuarios.md#get-guild-affinities) <a href="#get-guild-affinities" id="get-guild-affinities"></a>

`GET/users/@me/affinities/guilds`

Returns the current user's affinity scores for their joined guilds. Affinity scores are a measure of how likely a user is to interact with a guild.

[**Response Body**](usuarios.md#response-body)

| Field             | Type                                                                  | Description                                 |
| ----------------- | --------------------------------------------------------------------- | ------------------------------------------- |
| guild\_affinities | array\[[guild affinity](usuarios.md#guild-affinity-structure) object] | The user's affinity scores for their guilds |

[**Guild Affinity Structure**](usuarios.md#guild-affinity-structure)

| Field     | Type      | Description        |
| --------- | --------- | ------------------ |
| guild\_id | snowflake | The guild's ID     |
| affinity  | float     | The affinity score |

#### [Get Channel Affinities](usuarios.md#get-channel-affinities) <a href="#get-channel-affinities" id="get-channel-affinities"></a>

`GET/users/@me/affinities/channels`

Returns the current user's affinity scores for their participated channels. Affinity scores are a measure of how likely a user is to interact with a channel.

[**Response Body**](usuarios.md#response-body)

| Field               | Type                                                                      | Description                                   |
| ------------------- | ------------------------------------------------------------------------- | --------------------------------------------- |
| channel\_affinities | array\[[channel affinity](usuarios.md#channel-affinity-structure) object] | The user's affinity scores for their channels |

[**Channel Affinity Structure**](usuarios.md#channel-affinity-structure)

| Field       | Type      | Description        |
| ----------- | --------- | ------------------ |
| channel\_id | snowflake | The channel's ID   |
| affinity    | float     | The affinity score |

#### [Get Tutorial](usuarios.md#get-tutorial) <a href="#get-tutorial" id="get-tutorial"></a>

`GET/tutorial`

Returns the current user's [tutorial](https://docs.discord.food/topics/gateway-events#tutorial-structure) object, which contains information about the user's tutorial progress. If no tutorial is available, returns a 204 empty response instead.

#### [Confirm Tutorial Indicator](usuarios.md#confirm-tutorial-indicator) <a href="#confirm-tutorial-indicator" id="confirm-tutorial-indicator"></a>

`PUT/tutorial/indicators/{indicator}`

Confirms the given [tutorial](https://docs.discord.food/topics/gateway-events#tutorial-structure) indicator. Returns a 204 empty response on success.

#### [Suppress Tutorial](usuarios.md#suppress-tutorial) <a href="#suppress-tutorial" id="suppress-tutorial"></a>

`POST/tutorial/indicators/suppress`

Suppresses all [tutorial](https://docs.discord.food/topics/gateway-events#tutorial-structure) indicators. Returns a 204 empty response on success.

#### [Join HypeSquad Online](usuarios.md#join-hypesquad-online) <a href="#join-hypesquad-online" id="join-hypesquad-online"></a>

`POST/hypesquad/online`

Joins a HypeSquad house and applies the relevant [user flag](usuarios.md#user-flags) to the current user. Returns a 204 empty response on success. Fires a [User Update](https://docs.discord.food/topics/gateway-events#user-update) Gateway event.

[**JSON Params**](usuarios.md#json-params)

| Field     | Type    | Description                                                |
| --------- | ------- | ---------------------------------------------------------- |
| house\_id | integer | The [HypeSquad house](usuarios.md#hypesquad-house) to join |

[**HypeSquad House**](usuarios.md#hypesquad-house)

| Value | Description          |
| ----- | -------------------- |
| 1     | HypeSquad Bravery    |
| 2     | HypeSquad Brilliance |
| 3     | HypeSquad Balance    |

#### [Leave HypeSquad Online](usuarios.md#leave-hypesquad-online) <a href="#leave-hypesquad-online" id="leave-hypesquad-online"></a>

`DELETE/hypesquad/online`

Leaves the current user's HypeSquad house and removes the relevant [user flag](usuarios.md#user-flags). Returns a 204 empty response on success. Fires a [User Update](https://docs.discord.food/topics/gateway-events#user-update) Gateway event.

#### [Join Active Developer Program](usuarios.md#join-active-developer-program) <a href="#join-active-developer-program" id="join-active-developer-program"></a>

`POST/developers/active-program`

Joins the active developer program and applies the [user flag](usuarios.md#user-flags) to the current user. Requires the permission in the target channel. Returns a 204 empty response on success. Fires a [User Update](https://docs.discord.food/topics/gateway-events#user-update) and [Webhooks Update](https://docs.discord.food/topics/gateway-events#webhooks-update) Gateway event.

[**JSON Params**](usuarios.md#json-params)

| Field                        | Type                                                  | Description                                        |
| ---------------------------- | ----------------------------------------------------- | -------------------------------------------------- |
| application\_id <sup>1</sup> | snowflake                                             | The ID of the application to join the program with |
| channel\_id <sup>2</sup>     | The ID of the channel to create a follower webhook in |                                                    |

<sup>1</sup> User must be the owner of the application or member of the current team. The application must have the [application flag](https://docs.discord.food/resources/application#application-flags).

<sup>2</sup> This webhook will point to the official Discord Developers #developer-news channel and will be used to send updates about developing on Discord.

[**Response Body**](usuarios.md#response-body)

| Field    | Type                                                                                           | Description          |
| -------- | ---------------------------------------------------------------------------------------------- | -------------------- |
| follower | [followed channel](https://docs.discord.food/resources/channel#followed-channel-object) object | The followed channel |

#### [Leave Active Developer Program](usuarios.md#leave-active-developer-program) <a href="#leave-active-developer-program" id="leave-active-developer-program"></a>

`DELETE/developers/active-program`

Leaves the active developer program and removes the [user flag](usuarios.md#user-flags) from the current user. Returns a 204 empty response on success. Fires a [User Update](https://docs.discord.food/topics/gateway-events#user-update) Gateway event.

#### [Submit Developer Portal CSAT Survey](usuarios.md#submit-developer-portal-csat-survey) <a href="#submit-developer-portal-csat-survey" id="submit-developer-portal-csat-survey"></a>

`POST/dev-portal-csat-survey-response`

Submits a customer satisfaction survey response for the development experience on Discord. Returns a 204 empty response on success.

[**JSON Params**](usuarios.md#json-params)

| Field          | Type      | Description                        |
| -------------- | --------- | ---------------------------------- |
| user\_id       | snowflake | The ID of the client user          |
| csat\_response | integer   | The rating given by the user (1-5) |

#### [Get User Premium Usage](usuarios.md#get-user-premium-usage) <a href="#get-user-premium-usage" id="get-user-premium-usage"></a>

`GET/users/@me/premium-usage`

Returns the current user's premium usage for various perks. Only available for users with Nitro.

[**Response Body**](usuarios.md#response-body)

| Field                   | Type                                                        | Description                                     |
| ----------------------- | ----------------------------------------------------------- | ----------------------------------------------- |
| nitro\_sticker\_sends   | [premium usage](usuarios.md#premium-usage-structure) object | The number of Nitro sticker the user has sent   |
| total\_animated\_emojis | [premium usage](usuarios.md#premium-usage-structure) object | The number of animated emojis the user has sent |
| total\_global\_emojis   | [premium usage](usuarios.md#premium-usage-structure) object | The number of global emojis the user has sent   |
| total\_large\_uploads   | [premium usage](usuarios.md#premium-usage-structure) object | The number of large uploads the user has made   |
| total\_hd\_streams      | [premium usage](usuarios.md#premium-usage-structure) object | The number of streams the user has made in HD   |
| hd\_hours\_streamed     | [premium usage](usuarios.md#premium-usage-structure) object | The number of hours the user has streamed in HD |

[**Premium Usage Structure**](usuarios.md#premium-usage-structure)

| Field | Type    | Description                            |
| ----- | ------- | -------------------------------------- |
| value | integer | The total number of uses for this perk |

[**Example Response**](usuarios.md#example-response)

```
{  "total_large_uploads": {    "value": 50  },  "total_global_emojis": {    "value": 967  },  "total_animated_emojis": {    "value": 217  },  "nitro_sticker_sends": {    "value": 303  },  "hd_hours_streamed": {    "value": 100  },  "total_hd_streams": {    "value": 50  }}
```

#### [Get User Profile Effects](usuarios.md#get-user-profile-effects) <a href="#get-user-profile-effects" id="get-user-profile-effects"></a>

`GET/user-profile-effects`

Returns the list of [profile effects](https://support.discord.com/hc/en-us/articles/17828465914263-Profile-Effects) available to display.

[**Response Body**](usuarios.md#response-body)

| Field                    | Type                                                                                | Description                                  |
| ------------------------ | ----------------------------------------------------------------------------------- | -------------------------------------------- |
| profile\_effect\_configs | array\[[profile effect config](usuarios.md#profile-effect-config-structure) object] | The list of profile effects available to use |

[**Profile Effect Config Structure**](usuarios.md#profile-effect-config-structure)

| Field               | Type                                                                                      | Description                                                                                   |
| ------------------- | ----------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| id                  | string                                                                                    | The ID of the profile effect                                                                  |
| type                | integer                                                                                   | The [type of collectible](usuarios.md#collectible-type) (always `PROFILE_EFFECT`)             |
| sku\_id             | string                                                                                    | The ID of the profile effect SKU                                                              |
| title               | string                                                                                    | The title of the profile effect                                                               |
| description         | string                                                                                    | The description of the profile effect                                                         |
| accessibilityLabel  | string                                                                                    | An accessible description of the profile effect                                               |
| animationType       | integer                                                                                   | The [type of animation](usuarios.md#profile-effect-animation-type) used by the profile effect |
| thumbnailPreviewSrc | string                                                                                    | The URL of the profile effect's thumbnail preview image (in APNG format)                      |
| reducedMotionSrc    | string                                                                                    | A URL of the profile effect with reduced motion (in APNG format)                              |
| effects             | array\[[profile effect animation](usuarios.md#profile-effect-animation-structure) object] | The animation frames for the profile effect                                                   |

[**Profile Effect Animation Structure**](usuarios.md#profile-effect-animation-structure)

| Field             | Type                                                                                | Description                                                      |
| ----------------- | ----------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| src               | string                                                                              | The URL of the animation image (in APNG format)                  |
| loop              | boolean                                                                             | Whether the animation frame should loop                          |
| height            | integer                                                                             | The height of the animation image                                |
| width             | integer                                                                             | The width of the animation image                                 |
| duration          | integer                                                                             | The duration of the animation frame (in milliseconds)            |
| start             | integer                                                                             | The start time of the animation frame (in milliseconds)          |
| loopDelay         | integer                                                                             | The delay between loops of the animation frame (in milliseconds) |
| position          | [profile effect position](usuarios.md#profile-effect-position-structure) object     | The position of the animation frame                              |
| zIndex            | integer                                                                             | The z-index of the animation frame                               |
| randomizedSources | array\[[profile effect source](usuarios.md#profile-effect-source-structure) object] | The sources to randomize the `src` from                          |

[**Profile Effect Position Structure**](usuarios.md#profile-effect-position-structure)

| Field | Type    | Description                             |
| ----- | ------- | --------------------------------------- |
| x     | integer | The x-coordinate of the animation frame |
| y     | integer | The y-coordinate of the animation frame |

[**Profile Effect Source Structure**](usuarios.md#profile-effect-source-structure)

| Field | Type   | Description                                     |
| ----- | ------ | ----------------------------------------------- |
| src   | string | The URL of the animation image (in APNG format) |

[**Collectible Type**](usuarios.md#collectible-type)

| Value | Name               | Description                     |
| ----- | ------------------ | ------------------------------- |
| 0     | AVATAR\_DECORATION | An avatar decoration            |
| 1     | PROFILE\_EFFECT    | A profile effect animation      |
| 2     | NAMEPLATE          | A nameplate                     |
| 1000  | BUNDLE             | A bundle of collectibles        |
| 2000  | VARIANTS\_GROUP    | A group of collectible variants |
| 3000  | EXTERNAL\_SKU      | An external SKU                 |

[**Profile Effect Animation Type**](usuarios.md#profile-effect-animation-type)

| Value | Name         | Description                        |
| ----- | ------------ | ---------------------------------- |
| 0     | UNSPECIFIED  | The animation type is unspecified  |
| 1     | PERSISTENT   | The animation type is persistent   |
| 2     | INTERMITTENT | The animation type is intermittent |

[**Example Profile Effect Config**](usuarios.md#example-profile-effect-config)

```
{  "type": 1,  "id": "1139323075519852625",  "sku_id": "1139323092645183591",  "title": "Hydro Blast",  "description": "Time to make a splash.",  "accessibilityLabel": "A powerful stream of water swirls and twirls in the air. Wait, where's it headed off to?!",  "animationType": 2,  "thumbnailPreviewSrc": "https://cdn.discordapp.com/assets/profile_effects/effects/b17d139f2e9/splash/thumbnail.png",  "reducedMotionSrc": "https://cdn.discordapp.com/assets/profile_effects/effects/b17d139f2e9/splash/reducedMotion.png",  "effects": [    {      "src": "https://cdn.discordapp.com/assets/profile_effects/effects/b17d139f2e9/splash/intro.png",      "loop": false,      "height": 880,      "width": 450,      "duration": 2880,      "start": 0,      "loopDelay": 0,      "position": {        "x": 0,        "y": 0      },      "zIndex": 100,      "randomizedSources": []    },    {      "src": "https://cdn.discordapp.com/assets/profile_effects/effects/b17d139f2e9/splash/loop.png",      "loop": true,      "height": 880,      "width": 450,      "duration": 2880,      "start": 5760,      "loopDelay": 5760,      "position": {        "x": 0,        "y": 0      },      "zIndex": 100,      "randomizedSources": []    }  ]}
```

#### [Get Confetti Potions](usuarios.md#get-confetti-potions) <a href="#get-confetti-potions" id="get-confetti-potions"></a>

`GET/users/@me/consumable/confetti`

Returns the number of confetti potions left to use.

[**Response Body**](usuarios.md#response-body)

| Field        | Type                                                                                      | Description                                    |
| ------------ | ----------------------------------------------------------------------------------------- | ---------------------------------------------- |
| entitlement  | ?[entitlement](https://docs.discord.food/resources/entitlement#entitlement-object) object | The entitlement to which the potions belong to |
| num\_potions | integer                                                                                   | Number of unused potions left                  |

#### [Apply Confetti Potion](usuarios.md#apply-confetti-potion) <a href="#apply-confetti-potion" id="apply-confetti-potion"></a>

`POST/users/@me/consumable/confetti`

Applies a confetti potion to a message. To use custom emoji, you must encode it in the format with the emoji name and emoji ID. Returns a 204 empty response on success.

[**JSON Params**](usuarios.md#json-params)

| Field       | Type      | Description                                           |
| ----------- | --------- | ----------------------------------------------------- |
| channel\_id | snowflake | The ID of the channel the message is in               |
| message\_id | snowflake | The ID of the message to apply the confetti potion to |
| emoji\_name | string    | Unicode emoji or custom emoji name and ID             |

#### [Get Saved Messages](usuarios.md#get-saved-messages) <a href="#get-saved-messages" id="get-saved-messages"></a>

`GET/users/@me/saved-messages`

Returns message bookmarks and reminders for the current user.

[**Response Body**](usuarios.md#response-body)

| Field   | Type                                                                | Description                |
| ------- | ------------------------------------------------------------------- | -------------------------- |
| results | array\[[saved message](usuarios.md#saved-message-structure) object] | The list of saved messages |

[**Saved Message Structure**](usuarios.md#saved-message-structure)

| Field      | Type                                                                          | Description                   |
| ---------- | ----------------------------------------------------------------------------- | ----------------------------- |
| message    | ?[message](https://docs.discord.food/resources/message#message-object) object | The saved message             |
| save\_data | [save data](usuarios.md#save-data-structure) object                           | The save data for the message |

[**Save Data Structure**](usuarios.md#save-data-structure)

| Field            | Type               | Description                              |
| ---------------- | ------------------ | ---------------------------------------- |
| channel\_id      | snowflake          | The ID of the channel                    |
| message\_id      | snowflake          | The ID of the message                    |
| guild\_id?       | snowflake          | The ID of the guild                      |
| saved\_at        | ISO8601 timestamp  | The timestamp when the message was saved |
| author\_summary  | string             | Unknown                                  |
| channel\_summary | string             | Unknown                                  |
| message\_summary | string             | Unknown                                  |
| notes            | string             | Unknown                                  |
| due\_at          | ?ISO8601 timestamp | When the reminder is due                 |

#### [Save Message](usuarios.md#save-message) <a href="#save-message" id="save-message"></a>

`PUT/users/@me/saved-messages/{channel.id}/{message.id}`

Saves a message for the current user. Returns a [saved message](usuarios.md#saved-message-structure) object on success. Fires a [Saved Message Create](https://docs.discord.food/topics/gateway-events#saved-message-create) Gateway event.

[**JSON Params**](usuarios.md#json-params)

| Field    | Type               | Description              |
| -------- | ------------------ | ------------------------ |
| due\_at? | ?ISO8601 timestamp | When the reminder is due |

#### [Unsave Message](usuarios.md#unsave-message) <a href="#unsave-message" id="unsave-message"></a>

`DELETE/users/@me/saved-messages/{channel.id}/{message.id}`

Unsaves a message for the current user. Returns a 204 empty response on success. Fires a [Saved Message Delete](https://docs.discord.food/topics/gateway-events#saved-message-delete) Gateway event.

#### [Verify Age](usuarios.md#verify-age) <a href="#verify-age" id="verify-age"></a>

`POST/age-verification/verify`

Starts the age verification process using a third-party age verification provider. After the process is complete, a [age verification system message](https://docs.discord.food/resources/message#age-verification-system-message) is sent to the user.

[**Response Body**](usuarios.md#response-body)

| Field                      | Type   | Description                                                                |
| -------------------------- | ------ | -------------------------------------------------------------------------- |
| verification\_request\_id  | string | UUID generated by the server to track the current age verification request |
| verification\_vendor\_name | string | The third party age verification provider (currently always `K_ID`)        |
| verification\_webview\_url | string | The webview URL to iframe into the client                                  |

#### [Create User Identity Verification](usuarios.md#create-user-identity-verification) <a href="#create-user-identity-verification" id="create-user-identity-verification"></a>

`POST/users/@me/identity/verification`

Creates a new verification attempt for the user. Returns a [user identity verification](usuarios.md#user-identity-verification-object) object on success.

[**JSON Params**](usuarios.md#json-params)

| Field       | Type   | Description                                               |
| ----------- | ------ | --------------------------------------------------------- |
| return\_url | string | The URL to redirect to after Stripe verification succeeds |

#### [Get User Identity Verification](usuarios.md#get-user-identity-verification) <a href="#get-user-identity-verification" id="get-user-identity-verification"></a>

`GET/users/@me/identity/verification`

Returns a [user identity verification](usuarios.md#user-identity-verification-object) object representing the most recent verification attempt.

---
icon: screen-users
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

# Equipos

Teams are groups of developers on Discord who want to collaborate on apps. On other platforms, these may be referred to as "organizations", "companies", or "teams". Discord went with the name Teams because it best encompassed all the awesome conglomerates of devs that work together to make awesome things on Discord. Also, none of you ever got picked for kickball in gym class, so now you get to be on a team.

Teams allow you and other Discord users to share access to apps. No more sharing login credentials in order to reset the token on a bot that your friend owns but you work on, or other such cases.

For game developers, this means that you can get your engineers access to your app for credentials they may need, your marketing folks access to store page management, and your finance people access to sales and performance metrics.

### Team Object

**Team Structure**

\| Field | Type | Description | | ------------------------------ | ------------------------------------------------------------------- | ------------------------------------------------------------------------------ | | id | snowflake | The ID of the team | | name | string | The name of the team | | icon ^1^ | ?string | The team's icon hash | | owner\_user\_id | snowflake | The ID of the team's owner | | members? ^2^ | array\[[team member](equipos.md#team-member-object) object] | The members in the team | | payout\_account\_status? ^3^ | ?integer | The [status of the team's primary payout account](equipos.md#team-payout-account-status) | | payout\_account\_statuses? ^3^ | array\[[team payout account](equipos.md#team-payout-account-structure) object] | The statuses of the team's payout accounts | | stripe\_connect\_account\_id? ^4^ | string | The ID of the team's Stripe Connect account |

^1^ The default team icon uses the same images as default avatars and can be calculated using `team_id % 5`.

^2^ Only provided in the application object.

^3^ Only included when fetched from [Get Team](equipos.md#get-team) or [Get Teams](equipos.md#get-teams) with `include_payout_account_status` set to `true`.

^4^ Only included when fetched from [Get Team](equipos.md#get-team).

**Team Payout Account Structure**

\| Field | Type | Description | | ------- | ------- | --------------------------------------------------------------- | | gateway | integer | The [payout gateway](equipos.md#team-payout-gateway) used | | status | integer | The [status of the payout account](equipos.md#team-payout-account-status) |

**Team Payout Gateway**

\| Value | Name | Description | | ----- | -------------- | ------------- | | 1 | STRIPE\_TOPUP | Stripe Top-Up | | 2 | TIPALTI | Tipalti | | 3 | STRIPE\_PRIMARY | Stripe |

**Team Payout Account Status**

\| Value | Name | Description | | ----- | --------------- | ------------------------------------------------------------- | | 1 | UNSUBMITTED | Team has not submitted a payout account application | | 2 | PENDING | Team's payout account application is pending approval | | 3 | ACTION\_REQUIRED | Team's payout account requires action to receive payouts | | 4 | ACTIVE | Team's payout account is active and can receive payouts | | 5 | BLOCKED | Team's payout account is blocked and cannot receive payouts | | 6 | SUSPENDED | Team's payout account is suspended and cannot receive payouts |

**Example Team**

\`json

```
```

````
```json

````

```json
{

```

```
```

```
```

```
" "status": 1 }]
}
`

### Team Member Object

###### Team Member Structure

| Field            | Type                                               | Description                                                |
| ---------------- | -------------------------------------------------- | ---------------------------------------------------------- |
| user             | partial [user](/resources/user#user-object) object | The user this team member represents                       |
| team_id          | snowflake                                          | The ID of the team the user is a member of                 |
| membership_state | integer                                            | The user's [team membership state](#team-membership-state) |
| role             | string                                             | The user's [role](#team-member-roles) on the team          |

###### Team Membership State

| Value | Name     | Description                      |
| ----- | -------- | -------------------------------- |
| 1     | INVITED  | The user is invited              |
| 2     | ACCEPTED | The user has accepted the invite |

## Team Member Roles

Team members can be one of four roles (owner, admin, developer, and read-only), and each role inherits the access of those below it. Roles for team members can be configured under **Team Members** in a team's settings.

###### Team Member Role Types

| Value     | Description                                                                                                                                                                                                                                                                                                                                       |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| admin     | Admins have similar access to owners, except they cannot take destructive actions on the team or team-owned apps.                                                                                                                                                                                                                                 |
| developer | Developers can access information about team-owned apps, like the client secret or public key. They can also take limited actions on team-owned apps, like configuring interaction endpoints or resetting the bot token. Members with the Developer role _cannot_ manage the team or its members, or take destructive actions on team-owned apps. |
| read_only | Read-only members can access information about a team and any team-owned apps. Some examples include getting the IDs of applications and exporting payout records.                                                                                                                                                                                |

##### Example Team Member

`json

```

```
```

```json
```

```json
{
  "user": {

```

```
```

```
```

```
```

"primary\_guild": null },

```
```

"role": "admin" } \`

### Team Payout Object

**Team Payout Structure**

\| Field | Type | Description | | ----------------------------------- | ------------------ | ----------------------------------------------- | | id | snowflake | The ID of the payout | | user\_id | snowflake | The ID of the user who receives the payout | | amount | integer | The amount of the payout | | status | integer | The [status of the payout](equipos.md#team-payout-status) | | period\_start | ISO8601 timestamp | When the payout period started | | period\_end | ?ISO8601 timestamp | When the payout period ended | | payout\_date | ?ISO8601 timestamp | When the payout was made | | latest\_tipalti\_submission\_response? | object | The latest response from Tipalti |

**Team Payout Status**

\| Value | Name | Description | | ----- | ----------------- | -------------------------------------------- | | 1 | OPEN | The payout is open | | 2 | PAID | The payout has been paid out | | 3 | PENDING | The payout is pending completion | | 4 | MANUAL | The payout has been manually made | | 5 | CANCELLED | The payout has been cancelled | | 6 | DEFERRED | The payout has been deferred | | 7 | DEFERRED\_INTERNAL | The payout has been deferred internally | | 8 | PROCESSING | The payout is processing | | 9 | ERROR | The payout has errored | | 10 | REJECTED | The payout has been rejected | | 11 | RISK\_REVIEW | The payout is under risk review | | 12 | SUBMITTED | The payout has been submitted for completion | | 13 | PENDING\_FUNDS | The payout is pending sufficient funds |

**Example Team Payout**

\`json

```
```

````
```json

````

```json
{

```

```
```

```
```

```

"payout_date": null
}
`

### Company Object

A development/publishing company working on a game on Discord.

###### Company Structure

| Field | Type      | Description             |
| ----- | --------- | ----------------------- |
| id    | snowflake | The ID of the company   |
| name  | string    | The name of the company |

###### Example Company

`json

```

```
```

```json
```

```json
{

```

"name": "AlienTec" } \`

## Endpoints

> 📋 HEADER: Get Teams

Returns a list of [team](equipos.md#team-object) objects that the current user is a member of.

**Query String Params**

\| Field | Type | Description | | ------------------------------ | ------- | ------------------------------------------------------------------------------------------------------------ | | include\_payout\_account\_status? | boolean | Whether to include team [payout account status](equipos.md#team-payout-account-status) in the response (default false) |

> 📋 HEADER: Create Team

Creates a new team. Returns a [team](equipos.md#team-object) object on success. Users can join a maximum of 30 teams.

> ⚠️ ALERTA:

This action requires the user to have MFA enabled.

**JSON Params**

\| Field | Type | Description | | ----- | ------ | -------------------- | | name | string | The name of the team |

> 📋 HEADER: Get Team

Returns a [team](equipos.md#team-object) object for the given team ID.

> 📋 HEADER: Modify Team

Modifies a team. User must be an admin of the team. Returns the updated [team](equipos.md#team-object) object on success.

**JSON Params**

\| Field | Type | Description | | -------------- | ---------------------------------- | ------------------------------------------------------ | | name? | string | The name of the team | | icon? | ?image data | The team's icon | | owner\_user\_id? | snowflake | The ID of the team's owner (must be the current owner) |

> 📋 HEADER: Delete Team

Deletes a team permanently. User must be the owner of the team. Returns a 204 empty response on success.

> 📋 HEADER: Accept Team Invite

Accepts an invite to join a team. Returns a [team](equipos.md#team-object) object on success. Users can join a maximum of 30 teams.

> ⚠️ ALERTA:

This action requires the user to have MFA enabled.

**JSON Params**

\| Field | Type | Description | | --------- | ------ | --------------------- | | token ^1^ | string | The team invite token |

^1^ This token can be retrieved by visiting the emailed `https://click.discord.com/` link and extracting the `#token` URI fragment from the redirect URL.

> 📋 HEADER: Get Team Members

Returns a list of [team member](equipos.md#team-member-object) objects for the given team ID.

> 📋 HEADER: Add Team Member

Invites a user to the team. User must be an admin of the team. Returns a [team member](equipos.md#team-member-object) object on success.

> ⚠️ ALERTA:

You must be friends with the user you are inviting.

**JSON Params**

\| Field | Type | Description | | ------------------ | ------- | ------------------------------------------------- | | username | string | The username of the user to invite | | discriminator? ^1^ | ?string | The discriminator of the user to invite | | role | string | The user's [role](equipos.md#team-member-roles) on the team |

^1^ `null` for migrated users. See the section on Discord's new username system for more information.

> 📋 HEADER: Modify Team Member

Modifies a team member. User must be an admin of the team. Returns the updated [team member](equipos.md#team-member-object) object on success.

> ⚠️ ALERTA:

The team owner cannot be modified.

**JSON Params**

\| Field | Type | Description | | ----- | ------ | ------------------------------------------------- | | role? | string | The user's [role](equipos.md#team-member-roles) on the team |

> 📋 HEADER: Remove Team Member

Removes a team member. User must be the an admin of the team unless removing themselves. Returns a 204 empty response on success.

> 📋 HEADER: Get Team Applications

Returns a list of application objects for the given team ID.

> 📋 HEADER: Get Team Stripe Connect URL

Returns a link that can be used to access the team's Stripe Connect payout account dashboard.

> ⚠️ ALERTA:

Stripe Connect can only be used for payouts to US bank accounts.

**JSON Params**

\| Field | Type | Description | | ------------- | ------ | ----------------------------------------------------------------------------------------------------------- | | country\_code? | string | The [ISO 3166-1 alpha-2 country code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code to use |

**Response Body**

\| Field | Type | Description | | --------------------------- | ------ | ------------------------------- | | stripe\_connect\_redirect\_url | string | The Stripe Connect redirect URL |

**Example Response**

\`json

```
```

````
```json

````

```json
{
  "stripe_connect_redirect_url": "https://connect.stripe.com/setup/e/acct_123456/789abcd"
}
`

> 📋 HEADER: 
  Get Team Payout Onboarding


Returns a link that can be embedded in an IFrame to allow the user to access the team's Tipalti payout account dashboard. User must be the owner of the team.

> ⚠️ ALERTA: 

Tipalti can be used for payouts to international bank accounts.


###### Response Body

| Field | Type   | Description             |
| ----- | ------ | ----------------------- |
| url   | string | The payee dashboard URL |

###### Example Response

`json

```

```
```

```json
```

```json
{
  "url": "https://ui2.tipalti.com/payeedashboard/home?ts=12345&idap=10418817887227111107389984538707326773&payer=Discord&hashkey=123456abcd"
}
`

> 📋 HEADER: 
  Get Team Payouts


Returns a list of [team payout](#team-payout-object) objects for the given team ID.

###### Query String Params

| Field  | Type      | Description                                        |
| ------ | --------- | -------------------------------------------------- |
| limit? | number    | Max number of payouts to return (1-96, default 96) |
| after? | snowflake | Return payouts after this ID                       |

> 📋 HEADER: 
  Get Team Payout Report


Returns a CSV file containing the payout report for the given payout ID.

###### Query String Params

| Field | Type   | Description                                                |
| ----- | ------ | ---------------------------------------------------------- |
| type  | string | The [type of report](#team-payout-report-type) to generate |

###### Team Payout Report Type

| Value       | Description           |
| ----------- | --------------------- |
| sku         | Report by SKU         |
| transaction | Report by transaction |

> 📋 HEADER: 
  Search Companies


Returns a list of [company](#company-object) objects that match the given query. If no query is provided, returns a 204 empty response.

###### Query String Params

| Field | Type   | Description                          |
| ----- | ------ | ------------------------------------ |
| name? | string | Query to match company names against |

> 📋 HEADER: 
  Get Company


Returns a [company](#company-object) object for the given company ID.

> 📋 HEADER: 
  Create Company


Creates a new company under this team. Returns a [company](#company-object) object on success.

###### JSON Params

| Field | Type   | Description             |
| ----- | ------ | ----------------------- |
| name  | string | The name of the company |

> 📋 HEADER: 
  Create Team Identity Verification


Creates a new verification attempt for the team. Returns a [user identity verification](/resources/user#user-identity-verification-object) object on success.
Initiating an identity verification permanently locks the team out of manually transferring team ownership, Discord support must be contacted instead.
User must be the owner of the team.

###### JSON Params

| Field      | Type   | Description                                               |
| ---------- | ------ | --------------------------------------------------------- |
| return_url | string | The URL to redirect to after Stripe verification succeeds |

> 📋 HEADER: 
  Get Team Identity Verification


Returns a [user identity verification](/resources/user#user-identity-verification-object) object representing the most recent verification attempt.

```

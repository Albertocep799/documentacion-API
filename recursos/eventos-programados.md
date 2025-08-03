---
icon: calendar-days
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

# Eventos programados

### [Guild Scheduled Events](eventos-programados.md#guild-scheduled-events) <a href="#guild-scheduled-events" id="guild-scheduled-events"></a>

Scheduled events are a way to plan and organize events in a guild. They can be associated with a stage channel, voice channel, or an external location.

#### [Guild Scheduled Event Object](eventos-programados.md#guild-scheduled-event-object) <a href="#guild-scheduled-event-object" id="guild-scheduled-event-object"></a>

[**Guild Scheduled Event Structure**](eventos-programados.md#guild-scheduled-event-structure)

| Field                               | Type                                                                                           | Description                                                                                         |
| ----------------------------------- | ---------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| id                                  | snowflake                                                                                      | The ID of the scheduled event                                                                       |
| guild\_id                           | snowflake                                                                                      | The ID of the guild the scheduled event belongs to                                                  |
| channel\_id <sup>2</sup>            | ?snowflake                                                                                     | The ID of the channel in which the scheduled event will be hosted                                   |
| creator\_id? <sup>1</sup>           | ?snowflake                                                                                     | The ID of the user that created the scheduled event                                                 |
| creator?                            | partial [user](https://docs.discord.food/resources/user#user-object) object                    | The user that created the scheduled event                                                           |
| name                                | string                                                                                         | The name of the scheduled event (1-100 characters)                                                  |
| description?                        | ?string                                                                                        | The description for the scheduled event (1-1000 characters)                                         |
| scheduled\_start\_time              | ISO8601 timestamp                                                                              | When the scheduled event will start                                                                 |
| scheduled\_end\_time <sup>2</sup>   | ?ISO8601 timestamp                                                                             | When the scheduled event will end                                                                   |
| auto\_start? <sup>3</sup>           | boolean                                                                                        | Whether the event should automatically start at the scheduled start time                            |
| privacy\_level                      | integer                                                                                        | The [privacy level](https://docs.discord.food/resources/guild#privacy-level) of the scheduled event |
| status                              | integer                                                                                        | The [status](eventos-programados.md#guild-scheduled-event-status) of the scheduled event            |
| entity\_type                        | integer                                                                                        | The [type](eventos-programados.md#guild-scheduled-event-entity-type) of scheduled event             |
| entity\_id                          | ?snowflake                                                                                     | The ID of an entity associated with the scheduled event                                             |
| entity\_metadata <sup>2</sup>       | ?[entity metadata](eventos-programados.md#guild-scheduled-event-entity-metadata) object        | Additional metadata for the scheduled event                                                         |
| user\_count? <sup>4</sup>           | integer                                                                                        | The number of users subscribed to the scheduled event                                               |
| image?                              | ?string                                                                                        | The [cover image hash](https://docs.discord.food/reference#cdn-formatting) for the scheduled event  |
| recurrence\_rule                    | ?[recurrence rule](eventos-programados.md#guild-scheduled-event-recurrence-rule-object) object | The definition for how often this event should recur                                                |
| guild\_scheduled\_event\_exceptions | array\[[exception](eventos-programados.md#guild-scheduled-event-exception-object) object]      | Exceptions to the recurrence rule for this event                                                    |

<sup>1</sup> will be null and will not be included for events created before October 25th, 2021, when the concept of was introduced and tracked.

<sup>2</sup> See [field requirements by entity type](eventos-programados.md#guild-scheduled-event-entity-type-validation) to understand the relationship between and the following fields: , , and .

<sup>3</sup> Only included in [Gateway events](https://docs.discord.food/topics/gateway-events#guild-scheduled-events). See [the automations section](eventos-programados.md#guild-scheduled-event-status-update-automation) for more info on how to determine this manually.

<sup>4</sup> Only included when fetched from the [Get Guild Scheduled Events](eventos-programados.md#get-guild-scheduled-events) or [Get Guild Scheduled Event](eventos-programados.md#get-guild-scheduled-event) endpoints with set to .

[**Example Guild Scheduled Event**](eventos-programados.md#example-guild-scheduled-event)

```
{  "id": "1059954443799498922",  "guild_id": "1046920999469330512",  "name": "Alien meetup",  "description": "Aliens only!",  "channel_id": null,  "creator_id": "787017887877169173",  "creator": {    "id": "787017887877169173",    "username": "dziurwa",    "avatar": "cff3479a14360e4223f151eb8ad63dec",    "discriminator": "0",    "public_flags": 4194560,    "banner": null,    "accent_color": null,    "global_name": "Dziurwa",    "avatar_decoration_data": null,    "primary_guild": null  },  "image": "4b07ee3046773e8f2c8be856a70bd1a7",  "scheduled_start_time": "2023-12-31T23:00:00+00:00",  "scheduled_end_time": "2024-01-01T23:00:00+00:00",  "status": 1,  "entity_type": 3,  "entity_id": null,  "recurrence_rule": null,  "user_count": 7,  "privacy_level": 2,  "sku_ids": [],  "guild_scheduled_event_exceptions": [],  "entity_metadata": {    "location": "somwhere in ocean"  }}
```

[**Guild Scheduled Event Entity Metadata**](eventos-programados.md#guild-scheduled-event-entity-metadata)

| Field                  | Type   | Description                              |
| ---------------------- | ------ | ---------------------------------------- |
| location? <sup>1</sup> | string | Location of the event (1-100 characters) |

<sup>1</sup> Required for events with an of .

[**Guild Scheduled Event Status**](eventos-programados.md#guild-scheduled-event-status)

| Value | Name                   | Description                             |
| ----- | ---------------------- | --------------------------------------- |
| 1     | SCHEDULED              | The scheduled event has not started yet |
| 2     | ACTIVE                 | The scheduled event is currently active |
| 3     | COMPLETED <sup>1</sup> | The scheduled event has ended           |
| 4     | CANCELED <sup>1</sup>  | The scheduled event was canceled        |

<sup>1</sup> Once is set to or , the can no longer be updated.

[**Guild Scheduled Event Entity Type**](eventos-programados.md#guild-scheduled-event-entity-type)

| Value | Name            | Description                                                        |
| ----- | --------------- | ------------------------------------------------------------------ |
| 1     | STAGE\_INSTANCE | The scheduled event is in a stage channel                          |
| 2     | VOICE           | The scheduled event is in a voice channel                          |
| 3     | EXTERNAL        | The scheduled event is somewhere else™ not associated with Discord |
| 4     | PRIME\_TIME     | The scheduled event is a prime time event                          |

[**Guild Scheduled Event Entity Type Validation**](eventos-programados.md#guild-scheduled-event-entity-type-validation)

The following table shows field requirements based on current entity type.

| Entity Type     | channel\_id | entity\_metadata         | scheduled\_end\_time |
| --------------- | ----------- | ------------------------ | -------------------- |
| STAGE\_INSTANCE | required    | null                     | -                    |
| VOICE           | required    | null                     | -                    |
| EXTERNAL        | null        | required with `location` | required             |

#### [Guild Scheduled Event Recurrence Rule Object](eventos-programados.md#guild-scheduled-event-recurrence-rule-object) <a href="#guild-scheduled-event-recurrence-rule-object" id="guild-scheduled-event-recurrence-rule-object"></a>

Discord's recurrence rule is a subset of the behaviors [defined in the iCalendar RFC](https://datatracker.ietf.org/doc/html/rfc5545) and implemented using [Python's dateutil rrule](https://dateutil.readthedocs.io/en/stable/rrule.html).

[**Guild Scheduled Event Recurrence Rule Structure**](eventos-programados.md#guild-scheduled-event-recurrence-rule-structure)

| Field                      | Type                                                                                                                               | Description                                                                                                                                       |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| start                      | ISO8601 timestamp                                                                                                                  | Starting time of the recurrence interval                                                                                                          |
| end <sup>1</sup>           | ?ISO8601 timestamp                                                                                                                 | Ending time of the recurrence interval                                                                                                            |
| frequency                  | integer                                                                                                                            | [How often the event occurs](eventos-programados.md#guild-scheduled-event-recurrence-rule---frequency)                                            |
| interval                   | integer                                                                                                                            | The spacing between the events, defined by `frequency`; for example, `frequency` of `WEEKLY` and an `interval` of `2` would be "every other week" |
| by\_weekday                | ?array\[integer]                                                                                                                   | [Specific days within a week](eventos-programados.md#guild-scheduled-event-recurrence-rule---weekday) for the event to recur on                   |
| by\_n\_weekday             | ?array\[[recurrence rule - n\_weekday](eventos-programados.md#guild-scheduled-event-recurrence-rule---n_weekday-structure) object] | Specific days within a specific week (1-5) to recur on                                                                                            |
| by\_month                  | ?array\[integer]                                                                                                                   | [Specific months](eventos-programados.md#guild-scheduled-event-recurrence-rule---month) to recur on                                               |
| by\_month\_day             | ?array\[integer]                                                                                                                   | Specific dates within a month to recur on                                                                                                         |
| by\_year\_day <sup>1</sup> | ?array\[integer]                                                                                                                   | Specific days within a year to recur on (1-364)                                                                                                   |
| count <sup>1</sup>         | ?integer                                                                                                                           | The total amount of times that the event is allowed to recur before stopping                                                                      |

<sup>1</sup> Cannot currently be set externally.

<details>

<summary>System limitationsRecurrence rules are powerful, but they have some specific restrictions</summary>

The current system limitations are present due to how reoccurring event data needs to be displayed in the client. In the future, we would like to open the system up to have fewer / none of these restrictions.

[**The following fields cannot be set by the client**](eventos-programados.md#the-following-fields-cannot-be-set-by-the-client)

[**The following combinations are mutually exclusive**](eventos-programados.md#the-following-combinations-are-mutually-exclusive)

*
*
* \+



* Only valid for daily and weekly events ( of or )
* when used in a daily event ( is )
  * The values present in the event must be a "known set" of weekdays.
  * The following are current allowed "sets"
    * Monday - Friday ()
    * Tuesday - Saturday ()
    * Sunday - Thursday ()
    * Friday & Saturday ()
    * Saturday & Sunday ()
    * Sunday & Monday ()
* when used in a weekly event ( is )
  * array currently can only be have length of
    * i.e: You can only select a single day within a week to have a recurring event on
    * If you wish to have multiple days within a week have a recurring event, please use a of
  * Also, see below for "every-other" week information



* Only valid for monthly events ( of )
* array currently can only have a length of
  * i.e: You can only select a single day within a month to have a recurring event on

[**and**](eventos-programados.md#by_month-and-by_month_day)

* Only valid for annual event ( is )
* both and must be provided
* both and arrays must have a length of
  * (i.e. you can only set a single date for annual events)

[**can only be set to a value other than when is set to**](eventos-programados.md#interval-can-only-be-set-to-a-value-other-than-1-when-frequency-is-set-to-weekly)

* In this situation, interval can be set to
* This allowance enables "every-other week" events
* Due to the limitations placed on this means that if you wish to use "every-other week" functionality you can only do so for a single day.

</details>

<details>

<summary>ExamplesSimple examples of some common Recurrent Rules</summary>

**Every weekday**

```
frequency = 3; // Frequency.DAILYinterval = 1;by_weekday = [0, 1, 2, 3, 4]; // [Weekday.MONDAY, ..., Weekday.FRIDAY]
```

**Every Wednesday**

```
frequency = 2; // Frequency.WEEKLYinterval = 1;by_weekday = [2]; // [Weekday.WEDNESDAY]
```

**Every other Wednesday**

```
frequency = 2; // Frequency.WEEKLYinterval = 2;by_weekday = [2]; // [Weekday.WEDNESDAY]
```

**Monthly on the fourth Wednesday**

```
frequency = 1; // Frequency.MONTHLYinterval = 1;by_n_weekday = [  {    n: 4,    day: 2, // Weekday.WEDNESDAY  },];
```

**Annually on July 24**

```
frequency = 0; // Frequency.YEARLYinterval = 1;by_month = [7]; // [Month.JULY]by_month_day = [24];
```

</details>

[**Guild Scheduled Event Recurrence Rule - Frequency**](eventos-programados.md#guild-scheduled-event-recurrence-rule---frequency)

| Value | Name    |
| ----- | ------- |
| 0     | YEARLY  |
| 1     | MONTHLY |
| 2     | WEEKLY  |
| 3     | DAILY   |

[**Guild Scheduled Event Recurrence Rule - Weekday**](eventos-programados.md#guild-scheduled-event-recurrence-rule---weekday)

| Value | Name      |
| ----- | --------- |
| 0     | MONDAY    |
| 1     | TUESDAY   |
| 2     | WEDNESDAY |
| 3     | THURSDAY  |
| 4     | FRIDAY    |
| 5     | SATURDAY  |
| 6     | SUNDAY    |

[**Guild Scheduled Event Recurrence Rule - N\_Weekday Structure**](eventos-programados.md#guild-scheduled-event-recurrence-rule---n_weekday-structure)

| Field | Type    | Description                                                                                                     |
| ----- | ------- | --------------------------------------------------------------------------------------------------------------- |
| n     | integer | The week to reoccur on (1-5)                                                                                    |
| day   | integer | The [day within the week](eventos-programados.md#guild-scheduled-event-recurrence-rule---weekday) to reoccur on |

[**Guild Scheduled Event Recurrence Rule - Month**](eventos-programados.md#guild-scheduled-event-recurrence-rule---month)

| Value | Name      |
| ----- | --------- |
| 1     | JANUARY   |
| 2     | FEBRUARY  |
| 3     | MARCH     |
| 4     | APRIL     |
| 5     | MAY       |
| 6     | JUNE      |
| 7     | JULY      |
| 8     | AUGUST    |
| 9     | SEPTEMBER |
| 10    | OCTOBER   |
| 11    | NOVEMBER  |
| 12    | DECEMBER  |

#### [Guild Scheduled Event Exception Object](eventos-programados.md#guild-scheduled-event-exception-object) <a href="#guild-scheduled-event-exception-object" id="guild-scheduled-event-exception-object"></a>

Represents an exception to the recurrence rule for a guild scheduled event.

[**Guild Scheduled Event Exception Structure**](eventos-programados.md#guild-scheduled-event-exception-structure)

| Field                  | Type               | Description                                                                                |
| ---------------------- | ------------------ | ------------------------------------------------------------------------------------------ |
| event\_id              | snowflake          | The ID of the scheduled event the exception is for                                         |
| event\_exception\_id   | snowflake          | A snowflake representing when the scheduled event would have started without the exception |
| is\_canceled           | boolean            | Whether the scheduled event will be skipped on this recurrence                             |
| scheduled\_start\_time | ?ISO8601 timestamp | The scheduled event's modified start time for this recurrence                              |
| scheduled\_end\_time   | ?ISO8601 timestamp | The scheduled event's modified end time for this recurrence                                |

[**Example Guild Scheduled Event Exception**](eventos-programados.md#example-guild-scheduled-event-exception)

```
{  "event_id": "1341289071875461170",  "event_exception_id": "2117779587072000000",  "scheduled_start_time": "2030-02-18T00:01:00+00:00",  "scheduled_end_time": null,  "is_canceled": false}
```

#### [Guild Scheduled Event User Object](eventos-programados.md#guild-scheduled-event-user-object) <a href="#guild-scheduled-event-user-object" id="guild-scheduled-event-user-object"></a>

Represents a user's subscription to a guild scheduled event or override for a specific exception.

[**Guild Scheduled Event User Structure**](eventos-programados.md#guild-scheduled-event-user-structure)

| Field                                   | Type                                                                                 | Description                                                                                              |
| --------------------------------------- | ------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------- |
| guild\_scheduled\_event\_id             | snowflake                                                                            | The ID of the scheduled event the user subscribed to                                                     |
| guild\_scheduled\_event\_exception\_id? | snowflake                                                                            | The ID of the specific exception this subscription is for, if any                                        |
| response                                | integer                                                                              | The user's [response](eventos-programados.md#guild-scheduled-event-user-response) to the scheduled event |
| user\_id                                | snowflake                                                                            | The ID of the user that subscribed to the scheduled event                                                |
| user? <sup>1</sup>                      | partial [user](https://docs.discord.food/resources/user#user-object) object          | The user that subscribed to the scheduled event                                                          |
| member? <sup>1</sup>                    | [guild member](https://docs.discord.food/resources/guild#guild-member-object) object | Guild member data for the user in the scheduled event's guild, if any                                    |

<sup>1</sup> Only included when fetched from the [Get Guild Scheduled Event Users](eventos-programados.md#get-guild-scheduled-event-users) endpoint.

[**Guild Scheduled Event User Response**](eventos-programados.md#guild-scheduled-event-user-response)

| Value | Name         | Description                                   |
| ----- | ------------ | --------------------------------------------- |
| 0     | UNINTERESTED | User is not interested in the occurrence      |
| 1     | INTERESTED   | User is interested in the event or occurrence |

### [Endpoints](eventos-programados.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get User Guild Scheduled Events](eventos-programados.md#get-user-guild-scheduled-events) <a href="#get-user-guild-scheduled-events" id="get-user-guild-scheduled-events"></a>

`GET/users/@me/scheduled-events`

Returns an array of [guild scheduled event user](eventos-programados.md#guild-scheduled-event-user-object) objects for the current user for a given guild.

[**Query String Params**](eventos-programados.md#query-string-params)

| Field      | Type      | Description                                             |
| ---------- | --------- | ------------------------------------------------------- |
| guild\_ids | snowflake | The guild ID to get the subscribed scheduled events for |

#### [Get Guild Scheduled Events](eventos-programados.md#get-guild-scheduled-events) <a href="#get-guild-scheduled-events" id="get-guild-scheduled-events"></a>

`GET/guilds/{guild.id}/scheduled-events`

Returns a list of and [guild scheduled event](eventos-programados.md#guild-scheduled-event-object) objects for the given guild.

[**Query String Params**](eventos-programados.md#query-string-params)

| Field              | Type    | Description                                                              |
| ------------------ | ------- | ------------------------------------------------------------------------ |
| with\_user\_count? | boolean | Whether to include the of users subscribed to each event (default false) |

#### [Get Guild Scheduled Event](eventos-programados.md#get-guild-scheduled-event) <a href="#get-guild-scheduled-event" id="get-guild-scheduled-event"></a>

`GET/guilds/{guild.id}/scheduled-events/{guild_scheduled_event.id}`

Gets a guild scheduled event. Returns a [guild scheduled event](eventos-programados.md#guild-scheduled-event-object) object.

[**Query String Params**](eventos-programados.md#query-string-params)

| Field              | Type    | Description                                                              |
| ------------------ | ------- | ------------------------------------------------------------------------ |
| with\_user\_count? | boolean | Whether to include the of users subscribed to each event (default false) |

#### [Create Guild Scheduled Event](eventos-programados.md#create-guild-scheduled-event) <a href="#create-guild-scheduled-event" id="create-guild-scheduled-event"></a>

`POST/guilds/{guild.id}/scheduled-events`

Creates a guild scheduled event in the guild. Returns a [guild scheduled event](eventos-programados.md#guild-scheduled-event-object) object on success. Fires a [Guild Scheduled Event Create](https://docs.discord.food/topics/gateway-events#guild-scheduled-event-create) Gateway event.

[**JSON Params**](eventos-programados.md#json-params)

| Field                              | Type                                                                                           | Description                                                                                         |
| ---------------------------------- | ---------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| channel\_id? <sup>1</sup>          | snowflake                                                                                      | The ID of the channel in which the scheduled event will be hosted                                   |
| entity\_metadata? <sup>2</sup>     | [entity metadata](eventos-programados.md#guild-scheduled-event-entity-metadata)                | Additional metadata for the scheduled event                                                         |
| name                               | string                                                                                         | the name of the scheduled event                                                                     |
| privacy\_level                     | integer                                                                                        | The [privacy level](https://docs.discord.food/resources/guild#privacy-level) of the scheduled event |
| scheduled\_start\_time             | ISO8601 timestamp                                                                              | When the scheduled event will start                                                                 |
| scheduled\_end\_time? <sup>2</sup> | ISO8601 timestamp                                                                              | When the scheduled event will end                                                                   |
| description?                       | string                                                                                         | the description of the scheduled event                                                              |
| entity\_type                       | integer                                                                                        | The [entity type](eventos-programados.md#guild-scheduled-event-entity-type) of the scheduled event  |
| image?                             | [image data](https://docs.discord.food/reference#cdn-data)                                     | The cover image for the scheduled event                                                             |
| recurrence\_rule?                  | ?[recurrence rule](eventos-programados.md#guild-scheduled-event-recurrence-rule-object) object | The definition for how often this event should recur                                                |

<sup>1</sup> Optional for events with an of .

<sup>2</sup> Required for events with an of .

#### [Modify Guild Scheduled Event](eventos-programados.md#modify-guild-scheduled-event) <a href="#modify-guild-scheduled-event" id="modify-guild-scheduled-event"></a>

`PATCH/guilds/{guild.id}/scheduled-events/{guild_scheduled_event.id}`

Modifies a guild scheduled event. Returns the modified [guild scheduled event](eventos-programados.md#guild-scheduled-event-object) object on success. Fires a [Guild Scheduled Event Update](https://docs.discord.food/topics/gateway-events#guild-scheduled-event-update) and optionally a [Stage Instance Create](https://docs.discord.food/topics/gateway-events#stage-instance-create) Gateway event.

[**JSON Params**](eventos-programados.md#json-params)

| Field                              | Type                                                                             | Description                                                                                         |
| ---------------------------------- | -------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| channel\_id? <sup>1</sup>          | ?snowflake                                                                       | The ID of the channel in which the scheduled event will be hosted                                   |
| entity\_metadata?                  | ?[entity metadata](eventos-programados.md#guild-scheduled-event-entity-metadata) | Additional metadata for the scheduled event                                                         |
| name?                              | string                                                                           | The name of the scheduled event                                                                     |
| privacy\_level?                    | integer                                                                          | The [privacy level](https://docs.discord.food/resources/guild#privacy-level) of the scheduled event |
| scheduled\_start\_time?            | ISO8601 timestamp                                                                | When the scheduled event will start                                                                 |
| scheduled\_end\_time? <sup>1</sup> | ISO8601 timestamp                                                                | When the scheduled event will end                                                                   |
| description?                       | ?string                                                                          | the description of the scheduled event                                                              |
| entity\_type? <sup>1</sup>         | integer                                                                          | The [entity type](eventos-programados.md#guild-scheduled-event-entity-type) of the scheduled event  |
| status? <sup>2</sup>               | integer                                                                          | The [status](eventos-programados.md#guild-scheduled-event-status) of the scheduled event            |
| image?                             | [image data](https://docs.discord.food/reference#cdn-data)                       | The cover image for the scheduled event                                                             |

<sup>1</sup> If updating to :

* is required and must be set to
* with a field must be provided
* must be provided

<sup>2</sup> Only the following are valid status changes:

* SCHEDULED --> ACTIVE
* ACTIVE --> COMPLETED
* SCHEDULED --> CANCELED

#### [Delete Guild Scheduled Event](eventos-programados.md#delete-guild-scheduled-event) <a href="#delete-guild-scheduled-event" id="delete-guild-scheduled-event"></a>

`DELETE/guilds/{guild.id}/scheduled-events/{guild_scheduled_event.id}`

Deletes a guild scheduled event. Returns a a 204 empty response on success. Fires a [Guild Scheduled Event Delete](https://docs.discord.food/topics/gateway-events#guild-scheduled-event-delete) Gateway event.

#### [Create Guild Scheduled Event Exception](eventos-programados.md#create-guild-scheduled-event-exception) <a href="#create-guild-scheduled-event-exception" id="create-guild-scheduled-event-exception"></a>

`POST/guilds/{guild.id}/scheduled-events/{guild_scheduled_event.id}/exceptions`

Creates an exception to the recurrence rule for a guild scheduled event. Returns a [guild scheduled event exception](eventos-programados.md#guild-scheduled-event-exception-object) object on success. Fires a [Guild Scheduled Event Exception Create](https://docs.discord.food/topics/gateway-events#guild-scheduled-event-exception-create) Gateway event.

[**JSON Params**](eventos-programados.md#json-params)

| Field                            | Type               | Description                                                       |
| -------------------------------- | ------------------ | ----------------------------------------------------------------- |
| original\_scheduled\_start\_time | ISO8601 timestamp  | When the scheduled event would have started without the exception |
| is\_canceled?                    | boolean            | Whether the scheduled event will be skipped on this recurrence    |
| scheduled\_start\_time?          | ?ISO8601 timestamp | The scheduled event's modified start time for this recurrence     |
| scheduled\_end\_time?            | ?ISO8601 timestamp | The scheduled event's modified end time for this recurrence       |

#### [Modify Guild Scheduled Event Exception](eventos-programados.md#modify-guild-scheduled-event-exception) <a href="#modify-guild-scheduled-event-exception" id="modify-guild-scheduled-event-exception"></a>

`PATCH/guilds/{guild.id}/scheduled-events/{guild_scheduled_event.id}/{guild_scheduled_event_exception.id}`

Modifies an exception to the recurrence rule for a guild scheduled event. Returns the modified [guild scheduled event exception](eventos-programados.md#guild-scheduled-event-exception-object) object on success. Fires a [Guild Scheduled Event Exception Create](https://docs.discord.food/topics/gateway-events#guild-scheduled-event-exception-create) Gateway event.

[**JSON Params**](eventos-programados.md#json-params)

| Field                   | Type               | Description                                                    |
| ----------------------- | ------------------ | -------------------------------------------------------------- |
| is\_canceled?           | boolean            | Whether the scheduled event will be skipped on this recurrence |
| scheduled\_start\_time? | ?ISO8601 timestamp | The scheduled event's modified start time for this recurrence  |
| scheduled\_end\_time?   | ?ISO8601 timestamp | The scheduled event's modified end time for this recurrence    |

#### [Delete Guild Scheduled Event Exception](eventos-programados.md#delete-guild-scheduled-event-exception) <a href="#delete-guild-scheduled-event-exception" id="delete-guild-scheduled-event-exception"></a>

`DELETE/guilds/{guild.id}/scheduled-events/{guild_scheduled_event.id}/{guild_scheduled_event_exception.id}`

Deletes an exception to the recurrence rule for a guild scheduled event. Returns a 204 empty response on success. Fires a [Guild Scheduled Event Exception Delete](https://docs.discord.food/topics/gateway-events#guild-scheduled-event-exception-delete) Gateway event.

#### [Get Guild Scheduled Event User Count](eventos-programados.md#get-guild-scheduled-event-user-count) <a href="#get-guild-scheduled-event-user-count" id="get-guild-scheduled-event-user-count"></a>

`GET/guilds/{guild.id}/scheduled-events/{guild_scheduled_event.id}/users/count`

Returns the number of users subscribed to a guild scheduled event.

[**Query String Params**](eventos-programados.md#query-string-params)

| Field                                    | Type              | Description                                             |
| ---------------------------------------- | ----------------- | ------------------------------------------------------- |
| guild\_scheduled\_event\_exception\_ids? | array\[snowflake] | The IDs of the exceptions to return counts for (max 10) |

[**Response Body**](eventos-programados.md#response-body)

| Field                                      | Type                     | Description                                                 |
| ------------------------------------------ | ------------------------ | ----------------------------------------------------------- |
| guild\_scheduled\_event\_count             | integer                  | The number of users subscribed to the guild scheduled event |
| guild\_scheduled\_event\_exception\_counts | map\[snowflake, integer] | The number of users subscribed to each exception            |

[**Example Response**](eventos-programados.md#example-response)

```
{  "guild_scheduled_event_count": 18,  "guild_scheduled_event_exception_counts": {    "1456059344486400000": 18  }}
```

#### [Get Guild Scheduled Event Users](eventos-programados.md#get-guild-scheduled-event-users) <a href="#get-guild-scheduled-event-users" id="get-guild-scheduled-event-users"></a>

`GET/guilds/{guild.id}/scheduled-events/{guild_scheduled_event.id}/users`

Returns a list of users subscribed to a guild scheduled event.

[**Query String Params**](eventos-programados.md#query-string-params)

| Field                                 | Type      | Description                                                    |
| ------------------------------------- | --------- | -------------------------------------------------------------- |
| before?                               | snowflake | Get users before this user ID                                  |
| after?                                | snowflake | Get users after this user ID                                   |
| limit?                                | number    | Max number of users to return (1-100, default 100)             |
| with\_member?                         | boolean   | Whether to include possible guild member data (default false)  |
| upgrade\_response\_type? <sup>1</sup> | boolean   | Whether to return the new response body format (default false) |

<sup>1</sup> This parameter is ignored for bots as they always receive the new response body format.

[**Response Body**](eventos-programados.md#response-body)

When is set to , the response is a list of [guild scheduled event user](eventos-programados.md#guild-scheduled-event-user-object) objects.

Otherwise, the response looks like this:

| Field | Type                                                                                | Description                                                                                     |
| ----- | ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| users | array\[partial [user](https://docs.discord.food/resources/user#user-object) object] | The users subscribed to the scheduled event, with an extracontaining optional guild member data |

#### [Create Guild Scheduled Event User](eventos-programados.md#create-guild-scheduled-event-user) <a href="#create-guild-scheduled-event-user" id="create-guild-scheduled-event-user"></a>

`PUT/guilds/{guild.id}/scheduled-events/{guild_scheduled_event.id}/users/@me`

Subscribes the current user to a guild scheduled event. Returns a [guild scheduled event user](eventos-programados.md#guild-scheduled-event-user-object) object on success. Fires a [Guild Scheduled Event User Add](https://docs.discord.food/topics/gateway-events#guild-scheduled-event-user-add) Gateway event.

#### [Delete Guild Scheduled Event User](eventos-programados.md#delete-guild-scheduled-event-user) <a href="#delete-guild-scheduled-event-user" id="delete-guild-scheduled-event-user"></a>

`DELETE/guilds/{guild.id}/scheduled-events/{guild_scheduled_event.id}/users/@me`

Unsubscribes the current user from a guild scheduled event. Returns a 204 empty response on success. Fires a [Guild Scheduled Event User Remove](https://docs.discord.food/topics/gateway-events#guild-scheduled-event-user-remove) Gateway event.

#### [Get Guild Scheduled Event Exception Users](eventos-programados.md#get-guild-scheduled-event-exception-users) <a href="#get-guild-scheduled-event-exception-users" id="get-guild-scheduled-event-exception-users"></a>

`GET/guilds/{guild.id}/scheduled-events/{guild_scheduled_event.id}/{guild_scheduled_event_exception.id}/users`

Returns a list of [guild scheduled event user](eventos-programados.md#guild-scheduled-event-user-object) objects subscribed to a specific guild scheduled event exception.

[**Query String Params**](eventos-programados.md#query-string-params)

| Field         | Type      | Description                                                   |
| ------------- | --------- | ------------------------------------------------------------- |
| before?       | snowflake | Get users before this user ID                                 |
| after?        | snowflake | Get users after this user ID                                  |
| limit?        | number    | Max number of users to return (1-100, default 100)            |
| with\_member? | boolean   | Whether to include possible guild member data (default false) |

#### [Create Guild Scheduled Event Exception User](eventos-programados.md#create-guild-scheduled-event-exception-user) <a href="#create-guild-scheduled-event-exception-user" id="create-guild-scheduled-event-exception-user"></a>

`PUT/guilds/{guild.id}/scheduled-events/{guild_scheduled_event.id}/{guild_scheduled_event_exception.id}/users/@me`

Overrides the user's subscription to a guild scheduled event for a specific exception. Returns a [subscribed guild scheduled event user](eventos-programados.md#guild-scheduled-event-user-object) object on success. Fires a [Guild Scheduled Event User Add](https://docs.discord.food/topics/gateway-events#guild-scheduled-event-user-add) Gateway event.

[**JSON Params**](eventos-programados.md#json-params)

| Field    | Type    | Description                                                                                                        |
| -------- | ------- | ------------------------------------------------------------------------------------------------------------------ |
| response | integer | The user's [response](eventos-programados.md#guild-scheduled-event-user-response) to the scheduled event exception |

#### [Delete Guild Scheduled Event Exception User](eventos-programados.md#delete-guild-scheduled-event-exception-user) <a href="#delete-guild-scheduled-event-exception-user" id="delete-guild-scheduled-event-exception-user"></a>

`DELETE/guilds/{guild.id}/scheduled-events/{guild_scheduled_event.id}/{guild_scheduled_event_exception.id}/users/@me`

Removes the user's subscription override for a specific exception. Returns a 204 empty response on success. Fires a [Guild Scheduled Event User Remove](https://docs.discord.food/topics/gateway-events#guild-scheduled-event-user-remove) Gateway event.

### [Guild Scheduled Event Status Update Automation](eventos-programados.md#guild-scheduled-event-status-update-automation) <a href="#guild-scheduled-event-status-update-automation" id="guild-scheduled-event-status-update-automation"></a>

[**An active scheduled event for a stage channel where all users have left the stage channel will automatically end a few minutes after the last user leaves the channel**](eventos-programados.md#an-active-scheduled-event-for-a-stage-channel-where-all-users-have-left-the-stage-channel-will-automatically-end-a-few-minutes-after-the-last-user-leaves-the-channel)

When an event with a of and of has no users connected to the stage channel for a certain period of time (on the order of minutes), the event will be automatically set to .

[**An active scheduled event for a voice channel where all users have left the voice channel will automatically end a few minutes after the last user leaves the channel**](eventos-programados.md#an-active-scheduled-event-for-a-voice-channel-where-all-users-have-left-the-voice-channel-will-automatically-end-a-few-minutes-after-the-last-user-leaves-the-channel)

When an event with a of and of has no users connected to the voice channel for a certain period of time (on the order of minutes), the event will be automatically set to .

[**An external event will automatically begin at its scheduled start time**](eventos-programados.md#an-external-event-will-automatically-begin-at-its-scheduled-start-time)

An event with an of at its will automatically have set to .

[**An external event will automatically end at its scheduled end time**](eventos-programados.md#an-external-event-will-automatically-end-at-its-scheduled-end-time)

An event with an of at its will automatically have set to .

[**Any scheduled event which has not begun after its scheduled start time will be automatically cancelled after a few hours**](eventos-programados.md#any-scheduled-event-which-has-not-begun-after-its-scheduled-start-time-will-be-automatically-cancelled-after-a-few-hours)

Any event with a of after a certain time interval (on the order of hours) beyond its will have its automatically set to .

### [Guild Scheduled Event Permissions Requirements](eventos-programados.md#guild-scheduled-event-permissions-requirements) <a href="#guild-scheduled-event-permissions-requirements" id="guild-scheduled-event-permissions-requirements"></a>

[**Permissions to create an event with entity\_type:**](eventos-programados.md#permissions-to-create-an-event-with-entity_type:-stage_instance)

[**Write Permissions (CREATE / UPDATE)**](eventos-programados.md#write-permissions-\(create-/-update\))

* at the guild level or at least for the associated with the event
*
*
*

[**Read Permissions (GET)**](eventos-programados.md#read-permissions-\(get\))

* for associated with the event

[**Permissions to create an event with entity\_type:**](eventos-programados.md#permissions-to-create-an-event-with-entity_type:-voice)

[**Write Permissions (CREATE / UPDATE)**](eventos-programados.md#write-permissions-\(create-/-update\))

* at the guild level or for the associated with the event
* for associated with event
* for associated with event

[**Read Permissions (GET)**](eventos-programados.md#read-permissions-\(get\))

* for associated with the event

[**Permissions to create an event with entity\_type:**](eventos-programados.md#permissions-to-create-an-event-with-entity_type:-external)

[**Write Permissions (CREATE / UPDATE)**](eventos-programados.md#write-permissions-\(create-/-update\))

* at the guild level

[**Read Permissions (GET)**](eventos-programados.md#read-permissions-\(get\))

* _No other permissions required_

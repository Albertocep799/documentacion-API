---
icon: chart-line-up
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

# Estadísticas

Insights are a data monitoring and analytics tool that Discord provides to help community managers understand their guild's performance and how their members are interacting with it.

**Aggregation Interval**

\| Value | Name | Description | | ----- | ------- | ----------- | | 1 | DAILY | Daily | | 2 | WEEKLY | Weekly | | 3 | MONTHLY | Monthly |

## Endpoints

> 📋 HEADER: Get Guild Growth Activation Overview

Returns a list of [growth activation overview](estadisticas.md#growth-activation-overview-structure) objects representing the amount of users who joined the guild, and the number of users who chatted in the guild.

**Query String Params**

\| Field | Type | Description | | -------- | ------------ | ----------------------------------------------------------- | | start | ISO8601 date | The start date for the insights data | | end | ISO8601 date | The end date for the insights data | | interval | integer | [The interval for the insights data](estadisticas.md#aggregation-interval) | | use\_t2? | boolean | Unknown |

**Growth Activation Overview Structure**

\| Field | Type | Description | | ----------------- | ------------ | ------------------------------------------------------- | | day\_pt | ISO8601 date | When the data was captured | | new\_members | integer | The number of new members who joined the guild | | new\_communicators | integer | The number of new members who communicated in the guild |

> 📋 HEADER: Get Guild Growth Activation Joins

Returns a list of [growth activation joins](estadisticas.md#growth-activation-joins-structure) objects that represents the number of users who joined the guild on a given date.

**Query String Params**

\| Field | Type | Description | | -------- | ------------ | ----------------------------------------------------------- | | start | ISO8601 date | The start date for the insights data | | end | ISO8601 date | The end date for the insights data | | interval | integer | [The interval for the insights data](estadisticas.md#aggregation-interval) | | use\_t2? | boolean | Unknown |

**Growth Activation Joins Structure**

\| Field | Type | Description | | ------ | ------------ | ----------------------------------------------------- | | day\_pt | ISO8601 date | When the data was captured | | joins | integer | The number of users who joined the guild on this date |

> 📋 HEADER: Get Guild Growth Activation Joins by Invite

Returns a list of [growth activation joins](estadisticas.md#growth-activation-joins-by-invite-structure) objects that represents the number of users who joined the guild using an invite link.

**Query String Params**

\| Field | Type | Description | | -------- | ------------ | ----------------------------------------------------------- | | start | ISO8601 date | The start date for the insights data | | end | ISO8601 date | The end date for the insights data | | interval | integer | [The interval for the insights data](estadisticas.md#aggregation-interval) | | use\_t2? | boolean | Unknown |

**Growth Activation Joins by Invite Structure**

\| Field | Type | Description | | ----------- | ------------ | --------------------------------------------------------------- | | day\_pt | ISO8601 date | The date the invite link was created | | invite\_link | string | The invite link used to join the guild | | joins | integer | The number of users who joined the guild using this invite link |

> 📋 HEADER: Get Guild Growth Activation Joins by Referrer

Returns a list of [growth activation joins by referrer](estadisticas.md#growth-activation-joins-by-referrer-structure) objects representing the number of users who joined the guild through website referrals like Google, Reddit, or other websites.

**Query String Params**

\| Field | Type | Description | | -------- | ------------ | ----------------------------------------------------------- | | start | ISO8601 date | The start date for the insights data | | end | ISO8601 date | The end date for the insights data | | interval | integer | [The interval for the insights data](estadisticas.md#aggregation-interval) | | use\_t2? | boolean | Unknown |

**Growth Activation Joins by Referrer Structure**

\| Field | Type | Description | | -------------------- | ------------ | ----------------------------------------------------------------- | | day\_pt | ISO8601 date | The date the referral was made | | referring\_domain ^1^ | string | The referring domain that led to the referral link being clicked | | joins | integer | The number of users who joined the guild using this referral link |

^1^ The `referring_domain` field represents what the `Referer` header contained when they joined the guild.

> 📋 HEADER: Get Guild Growth Activation Joins by Sources

Returns a list of [growth activations joins by sources](estadisticas.md#growth-activation-joins-by-sources-structure) objects representing the number of users who joined the guild by source, such as invites, integration, or other means.

**Query String Params**

\| Field | Type | Description | | -------- | ------------ | ----------------------------------------------------------- | | start | ISO8601 date | The start date for the insights data | | end | ISO8601 date | The end date for the insights data | | interval | integer | [The interval for the insights data](estadisticas.md#aggregation-interval) | | use\_t2? | boolean | Unknown |

**Growth Activation Joins by Sources Structure**

\| Field | Type | Description | | ----------------- | ------------ | -------------------------------------------------------------------------------------------------------------------------------------- | | day\_pt | ISO8601 date | The date the source was recorded | | discovery\_joins | integer | The total number of users who joined the guild through guild discovery | | invites | integer | The total number of users who joined the guild through an invite | | vanity\_joins | integer | The total number of users who joined the guild through a vanity URL | | hubs\_joins | integer | The total number of users who joined the guild through a student hub | | bot\_joins | integer | The total number of users who were added to the guild by a bot using the guilds.join OAuth2 scope | | integration\_joins | integer | The total number of users who were added to the guild by an integration (e.g. Twitch) | | other\_joins | integer | The total number of users who joined the guild through an unknown source | | total\_joins | integer | The total number of users who joined the guild |

> 📋 HEADER: Get Guild Growth Activation Leavers

Returns a list of [growth activation leavers](estadisticas.md#growth-activation-leavers-structure) objects representing the number of users who left the guild on a given date, along with the number of days they were in the guild before leaving.

**Query String Params**

\| Field | Type | Description | | -------- | ------------ | ----------------------------------------------------------- | | start | ISO8601 date | The start date for the insights data | | end | ISO8601 date | The end date for the insights data | | interval | integer | [The interval for the insights data](estadisticas.md#aggregation-interval) | | use\_t2? | boolean | Unknown |

**Growth Activation Leavers Structure**

\| Field | Type | Description | | ------------- | ------------ | ----------------------------------------------------------- | | day\_pt | ISO8601 date | When the data was captured | | days\_in\_guild | integer | The number of days the user was in the guild before leaving | | leavers | integer | The number of users who left the guild on this date |

> 📋 HEADER: Get Guild Growth Activation Percentages

Returns a list of [growth activation percentages](estadisticas.md#growth-activation-percentages-structure) objects representing the number of new members who joined the guild, the percentage of those members who communicated in the guild, and the percentage of those members who visited or talked.

**Query String Params**

\| Field | Type | Description | | -------- | ------------ | ----------------------------------------------------------- | | start | ISO8601 date | The start date for the insights data | | end | ISO8601 date | The end date for the insights data | | interval | integer | [The interval for the insights data](estadisticas.md#aggregation-interval) | | use\_t2? | boolean | Unknown |

**Growth Activation Percentages Structure**

\| Field | Type | Description | | ------------------- | ------------ | ----------------------------------------------------------------------------------------------- | | day\_pt | ISO8601 date | When the data was captured | | new\_members | integer | The number of new members who joined the guild on this date | | pct\_communicated | float | The percentage of new members who sent 3 or more message in the guild or talked in a voice chat | | pct\_opened\_channels | float | The percentage of new members who opened 3 or more channels in the guild |

> 📋 HEADER: Get Guild Growth Activation Retention

Returns a list of [growth activation retention](estadisticas.md#growth-activation-retention-structure) objects representing the number of new members who joined the guild on a given date, along with the general percentage of those members who stayed in the guild.

**Query String Params**

\| Field | Type | Description | | -------- | ------------ | ----------------------------------------------------------- | | start | ISO8601 date | The start date for the insights data | | end | ISO8601 date | The end date for the insights data | | interval | integer | [The interval for the insights data](estadisticas.md#aggregation-interval) | | use\_t2? | boolean | Unknown |

**Growth Activation Retention Structure**

\| Field | Type | Description | | ------------ | ------------ | ----------------------------------------------------------- | | day\_pt | ISO8601 date | When the data was captured | | new\_members | integer | The number of new members who joined the guild on this date | | pct\_retained | float | The percentage of new members who stayed in the guild |

> 📋 HEADER: Get Guild Growth Activation Membership

Returns a list of [growth activation membership](estadisticas.md#growth-activation-membership-structure) objects representing the total number of members in the guild on the date provided.

**Query String Params**

\| Field | Type | Description | | -------- | ------------ | ----------------------------------------------------------- | | start | ISO8601 date | The start date for the insights data | | end | ISO8601 date | The end date for the insights data | | interval | integer | [The interval for the insights data](estadisticas.md#aggregation-interval) | | use\_t2? | boolean | Unknown |

**Growth Activation Membership Structure**

\| Field | Type | Description | | ---------------- | ------------ | ----------------------------------------------------- | | day\_pt | ISO8601 date | When the data was captured | | total\_membership | integer | The total number of members in the guild on this date |

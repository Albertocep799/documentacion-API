---
icon: box-circle-check
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

# Entradas del directorio

A directory in Discord is a special type of channel that contains a list of directory entries, which are guilds and scheduled events that have been added and made discoverable by the community. Any user that can access a directory can view the entries and join the guilds.

Directories are most commonly found in [student hubs](https://support.discord.com/hc/en-us/articles/4406046651927).

### Directory Entry Object

**Directory Entry Structure**

\| Field | Type | Description | | -------------------------- | ------------------------------------------------------------------------------------ | -------------------------------------------------------- | | type | integer | The [type of directory entry](entradas-del-directorio.md#directory-entry-type) | | directory\_channel\_id | snowflake | The ID of the directory channel that the entry is in | | entity\_id | snowflake | The ID of the guild or scheduled event | | created\_at | string | When the entry was created | | primary\_category\_id? | integer | The [primary category](entradas-del-directorio.md#directory-category) of the entry | | description | ?string | The description of the entry | | author\_id | snowflake | The ID of the user that created the entry | | guild? ^1^ | [directory guild](entradas-del-directorio.md#directory-guild-structure) object | The guild entry | | guild\_scheduled\_event? ^1^ | [directory guild scheduled event](entradas-del-directorio.md#directory-guild-scheduled-event-structure) object | The guild scheduled event entry |

^1^ Not included when fetched from [Get Partial Directory Entries](entradas-del-directorio.md#get-partial-directory-entries).

**Directory Guild Structure**

This object is a partial guild object with the following additional fields:

\| Field | Type | Description | | ----------------------- | ------- | ------------------------------------------------------------- | | featurable\_in\_directory | boolean | Whether the guild is eligible to be featured in the directory |

**Directory Guild Scheduled Event Structure**

This object is a guild scheduled event object with the following additional fields:

\| Field | Type | Description | | ---------- | ----------------------------------------------------- | ---------------------------------------- | | guild | partial guild object | The guild that the event is for | | user\_rsvp? | boolean | Whether the user has RSVP'd to the event |

**Directory Entry Type**

\| Value | Name | Description | | ----- | --------------------- | ----------------- | | 0 | GUILD | A guild | | 1 | GUILD\_SCHEDULED\_EVENT | A scheduled event |

**Directory Category**

\| Value | Name | Description | | ----- | ----------------- | --------------------------- | | 0 | UNCATEGORIZED | Uncategorized entry | | 1 | SCHOOL\_CLUB | School club or organization | | 2 | CLASS | Class or subject | | 3 | STUDY\_SOCIAL | Study or social group | | \~\~4\~\~ | \~\~SUBJECT\_MAJOR\~\~ | \~\~For a subject or major\~\~ | | 5 | MISC | Miscellaneous entry |

## Endpoints

> 📋 HEADER: Get Directory Counts

Returns a mapping of [directory categories](entradas-del-directorio.md#directory-category) to their entry count in the given directory channel. Requires the `VIEW_CHANNEL` permission.

> 📋 HEADER: Get Directory Entries

Returns a list of [directory entry](entradas-del-directorio.md#directory-entry-object) objects in the given directory channel. Requires the `VIEW_CHANNEL` permission.

**Query String Parameters**

\| Field | Type | Description | | ------------ | ------- | ----------------------------------------------------------------- | | type? | integer | The [type of directory entry](entradas-del-directorio.md#directory-entry-type) to filter by | | category\_id? | integer | The [primary category](entradas-del-directorio.md#directory-category) to filter by |

> 📋 HEADER: Get Partial Directory Entries

Returns a list of partial [directory entry](entradas-del-directorio.md#directory-entry-object) objects in the given directory channel. Requires the `VIEW_CHANNEL` permission.

**Query String Parameters**

\| Field | Type | Description | | ----------- | ---------------- | ------------------------------------------------------ | | entity\_ids? | array\[snowflake] | The IDs of the directory entries to retrieve (max 100) |

> 📋 HEADER: Search Directory Entries

Returns a list of [directory entry](entradas-del-directorio.md#directory-entry-object) objects in the given directory channel that match the query. Requires the `VIEW_CHANNEL` permission.

**Query String Parameters**

\| Field | Type | Description | | ------------ | ------- | ----------------------------------------------------------------- | | query | string | The query to search for (1-100 characters) | | type? | integer | The [type of directory entry](entradas-del-directorio.md#directory-entry-type) to filter by | | category\_id? | integer | The [primary category](entradas-del-directorio.md#directory-category) to filter by |

> 📋 HEADER: Get Directory Entry

Returns a [directory entry](entradas-del-directorio.md#directory-entry-object) object for the given entity ID in the directory channel. Requires the `VIEW_CHANNEL` permission.

> 📋 HEADER: Create Directory Entry

Creates a new [directory entry](entradas-del-directorio.md#directory-entry-object) in the given directory channel. Requires the `VIEW_CHANNEL` permission and the `MANAGE_GUILD` permission on the entity being added. Returns the new [directory entry](entradas-del-directorio.md#directory-entry-object) object on success. Fires a Guild Directory Entry Create Gateway event.

**JSON Params**

\| Field | Type | Description | | -------------------- | ------- | ---------------------------------------------------------------------------------- | | type? | integer | The [type of directory entry](entradas-del-directorio.md#directory-entry-type) to create (default `GUILD`) | | primary\_category\_id? | integer | The [primary category](entradas-del-directorio.md#directory-category) of the entry (default `UNCATEGORIZED`) | | description? | ?string | The description of the entry (max 200 characters) |

> 📋 HEADER: Modify Directory Entry

Modifies an existing directory entry in the given directory channel. Requires the `VIEW_CHANNEL` permission and the `MANAGE_GUILD` permission on the entity being modified. Returns the updated [directory entry](entradas-del-directorio.md#directory-entry-object) object on success. Fires a Guild Directory Entry Update Gateway event.

**JSON Params**

\| Field | Type | Description | | -------------------- | ------- | -------------------------------------------------------- | | primary\_category\_id? | integer | The [primary category](entradas-del-directorio.md#directory-category) of the entry | | description? | string | The description of the entry (max 200 characters) |

> 📋 HEADER: Delete Directory Entry

Deletes a directory entry in the given directory channel. Requires the `VIEW_CHANNEL` permission and the `MANAGE_GUILD` permission on the entity being deleted. Returns a 204 empty response on success. Fires a Guild Directory Entry Delete Gateway event.

> 📋 HEADER: Get Directory Broadcast Info

Returns the broadcast information for the given guild and directory entry type. User must be a member of the guild.

**Query String Parameters**

\| Field | Type | Description | | ---------- | ------- | ------------------------------------------------------------------------------ | | type | integer | The [type of directory entry](entradas-del-directorio.md#directory-entry-type) to get broadcast info for | | entity\_id? | integer | The ID of the directory entry to get broadcast info for |

**Response Body**

\| Field | Type | Description | | ------------------ | ------- | ----------------------------------------------------------------- | | can\_broadcast | boolean | Whether the user can broadcast in any linked directory channels | | has\_broadcast? ^1^ | boolean | Whether the entity has been broadcasted in any directory channels |

^1^ Only included when `entity_id` is provided.

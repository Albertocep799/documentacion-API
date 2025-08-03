---
icon: waveform
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

# Panel de sonido

Soundboard sounds are short audio clips that can be played in voice channels. There is a set of [default sounds](panel-de-sonido.md#get-default-soundboard-sounds) available to all users. Soundboard sounds can also be [created in a guild](panel-de-sonido.md#create-guild-soundboard-sound); users will be able to use them in the guild, and premium (Nitro) subscribers can use them in all guilds.

Custom soundboard sounds in a can be retrieved over the Gateway using Request Soundboard Sounds.

### Soundboard Sound Object

**Soundboard Sound Structure**

\| Field | Type | Description | | ------------ | -------------------------------------------------- | ------------------------------------------------------------------------------------------------ | | sound\_id | snowflake | The ID of the soundboard sound | | name | string | The name of the soundboard sound (2-32 characters) | | volume | float | The volume of the soundboard sound (represented as a float from 0 to 1) | | emoji\_id | ?snowflake | The ID of the sound's custom emoji | | emoji\_name | ?string | The unicode character of the sound's emoji | | guild\_id? | snowflake | The ID of the source guild | | available | boolean | Whether this guild sound can be used; may be false due to loss of premium subscriptions (boosts) | | user? ^1^ | partial user object | The user who created this sound | | user\_id? ^2^ | snowflake | The ID of the user who created this sound |

^1^ Only included for sounds in contexts where the sound is created or updated, as well as when fetched through the [Get Guild Soundboard Sounds](panel-de-sonido.md#get-guild-soundboard-sounds) or [Get Guild Soundboard Sound](panel-de-sonido.md#get-guild-soundboard-sound) endpoints by a user with the `MANAGE_EXPRESSIONS` permission.

^2^ Only included in Gateway events related to the soundboard.

## Endpoints

> 📋 HEADER: Get Default Soundboard Sounds

Returns a list of [soundboard sound](panel-de-sonido.md#soundboard-sound-object) objects that can be used by all users.

> 📋 HEADER: Get Guild Soundboard Sounds

Returns an object containing a list of [soundboard sound](panel-de-sonido.md#soundboard-sound-object) objects for the given guild. Includes the `user` field if the user has the `CREATE_EXPRESSIONS` or `MANAGE_EXPRESSIONS` permission.

**Response Body**

\| Field | Type | Description | | ----- | ---------------------------------------------------------- | ---------------------------------- | | items | array\[[soundboard sound](panel-de-sonido.md#soundboard-sound-object) object] | The soundboard sounds in the guild |

> 📋 HEADER: Get Guild Soundboard Sound

Returns a [soundboard sound](panel-de-sonido.md#soundboard-sound-object) object for the given guild and sound ID. Includes the `user` field if the user has the `CREATE_EXPRESSIONS` or `MANAGE_EXPRESSIONS` permission.

> 📋 HEADER: Create Guild Soundboard Sound

Creates a new soundboard sound for the guild. Requires the `CREATE_EXPRESSIONS` permission. Returns the new [soundboard sound](panel-de-sonido.md#soundboard-sound-object) object on success. Fires a Guild Soundboard Sound Create Gateway event.

> ⚠️ ALERTA:

Soundboard sounds have a maximum file size of **512 KiB** and a maximum duration of 5.2 seconds. Attempting to upload a sound larger than this limit will fail with a 400 bad request.

> ⬇️ COLAPSABLE:

Soundboard sound limits are applied to the total amount of sounds in the guild, making them a lot simpler than emoji limits. The default sound limit is 8. The real limit depends on the guild's [premium tier](https://support.discord.com/hc/en-us/articles/360028038352) and features.

These limits are summarized in the following table by premium tier. Note that if the guild has the MORE\_SOUNDBOARD feature, the applied limit is instead 96.

\| Premium Tier | Sound Limit | | ------------ | ----------- | | `NONE` | 8 | | `TIER_1` | 24 | | `TIER_2` | 36 | | `TIER_3` | 48 |

**JSON Params**

\| Field | Type | Description | | ----------- | --------------------------------- | ---------------------------------------------------------------------------------- | | name | string | The name of the soundboard sound (2-32 characters) | | sound | sound data | The sound file to upload | | volume? | ?float | The volume of the soundboard sound (represented as a float from 0 to 1, default 1) | | emoji\_id? | ?snowflake | The ID of the sound's custom emoji | | emoji\_name? | ?string | The unicode character of the sound's emoji |

> 📋 HEADER: Modify Guild Soundboard Sound

Modifies the given soundboard sound. For sounds created by the current user, requires either the `CREATE_EXPRESSIONS` or MANAGE\_EXPRESSIONS permission. For other sounds, requires the `MANAGE_EXPRESSIONS` permission. Returns the updated [soundboard sound](panel-de-sonido.md#soundboard-sound-object) object on success. Fires a Guild Soundboard Sound Update Gateway event.

**JSON Params**

\| Field | Type | Description | | ----------- | ---------- | ---------------------------------------------------------------------------------- | | name? | string | The name of the soundboard sound (2-32 characters) | | volume? | ?float | The volume of the soundboard sound (represented as a float from 0 to 1, default 1) | | emoji\_id? | ?snowflake | The ID of the sound's custom emoji | | emoji\_name? | ?string | The unicode character of the sound's emoji |

> 📋 HEADER: Delete Guild Soundboard Sound

For sounds created by the current user, requires either the `CREATE_EXPRESSIONS` or `MANAGE_EXPRESSIONS` permission. For other sounds, requires the `MANAGE_EXPRESSIONS` permission. Returns a 204 empty response on success. Fires a Guild Soundboard Sound Delete Gateway event.

> 📋 HEADER: Get Soundboard Sound Guild

Returns a discoverable guild object for the guild that owns the given sound. This endpoint requires the guild to be discoverable, not be auto-removed, and have guild expression discoverability enabled.

> 📋 HEADER: Send Soundboard Sound

Sends a soundboard sound to a voice channel. Returns a 204 empty response on success. Fires a Voice Channel Effect Send Gateway event.

> ⚠️ ALERTA:

Sending a soundboard sound requires the current user to be connected to the voice channel. The user cannot be server muted, deafened, or suppressed.

**JSON Params**

\| Field | Type | Description | | ---------------- | ---------- | ---------------------------------------------------------------- | | sound\_id | snowflake | The ID of the soundboard sound to send | | source\_guild\_id? | ?snowflake | The ID of the sound's source guild, if applicable (not required) |

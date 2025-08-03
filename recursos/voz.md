---
icon: phone-volume
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

# Voz

### [Voice](voz.md#voice) <a href="#voice" id="voice"></a>

Voice resources are used to interact with voice in Discord. For more information on connecting to voice, see the [Voice Connections topic](https://docs.discord.food/topics/voice-connections).

#### [Voice State Object](voz.md#voice-state-object) <a href="#voice-state-object" id="voice-state-object"></a>

Used to represent a user's voice connection status.

[**Voice State Structure**](voz.md#voice-state-structure)

| Field                         | Type                                                                                 | Description                                        |
| ----------------------------- | ------------------------------------------------------------------------------------ | -------------------------------------------------- |
| guild\_id? <sup>1</sup>       | ?snowflake                                                                           | The guild ID this voice state is for               |
| channel\_id                   | ?snowflake                                                                           | The channel ID this user is connected to           |
| lobby\_id?                    | snowflake                                                                            | The ID of the lobby this user is connected to      |
| user\_id                      | snowflake                                                                            | The user ID this voice state is for                |
| member? <sup>1</sup>          | [guild member](https://docs.discord.food/resources/guild#guild-member-object) object | The guild member this voice state is for           |
| session\_id                   | string                                                                               | The session ID this voice state is from            |
| deaf                          | boolean                                                                              | Whether this user is deafened by the guild, if any |
| mute                          | boolean                                                                              | Whether this user is muted by the guild, if any    |
| self\_deaf                    | boolean                                                                              | Whether this user is locally deafened              |
| self\_mute                    | boolean                                                                              | Whether this user is locally muted                 |
| self\_stream?                 | boolean                                                                              | Whether this user is streaming using "Go Live"     |
| self\_video                   | boolean                                                                              | Whether this user's camera is enabled              |
| suppress                      | boolean                                                                              | Whether this user's permission to speak is denied  |
| request\_to\_speak\_timestamp | ?ISO8601 timestamp                                                                   | When which the user requested to speak             |
| user\_volume? <sup>2</sup>    | float                                                                                | Volume level of the user (0-100)                   |

<sup>1</sup> Omitted in the [Gateway guild](https://docs.discord.food/topics/gateway-events#gateway-guild-object) object.

<sup>2</sup> Only available in [OAuth2 contexts](https://docs.discord.food/topics/gateway#oauth2-and-the-gateway). For regular connections, user volumes are available in [audio settings](https://docs.discord.food/resources/user-settings-proto#audio-settings-structure).

[**Example Voice State**](voz.md#example-voice-state)

```
{  "channel_id": "157733188964188161",  "user_id": "80351110224678912",  "session_id": "90326bd25d71d39b9ef95b299e3872ff",  "deaf": false,  "mute": false,  "self_deaf": false,  "self_mute": true,  "suppress": false,  "request_to_speak_timestamp": "2021-03-31T18:45:31.297561+00:00"}
```

#### [Voice Region Object](voz.md#voice-region-object) <a href="#voice-region-object" id="voice-region-object"></a>

[**Voice Region Structure**](voz.md#voice-region-structure)

| Field      | Type    | Description                                                          |
| ---------- | ------- | -------------------------------------------------------------------- |
| id         | string  | The unique ID for the region                                         |
| name       | string  | The name of the region                                               |
| optimal    | boolean | Whether this is the closest to the current user's client             |
| deprecated | boolean | Whether this is a deprecated voice region (avoid switching to these) |
| custom     | boolean | Whether this is a custom voice region (used for events, etc.)        |

### [Endpoints](voz.md#endpoints) <a href="#endpoints" id="endpoints"></a>

#### [Get Voice Regions](voz.md#get-voice-regions) <a href="#get-voice-regions" id="get-voice-regions"></a>

`GET/voice/regions`

Returns an array of [voice region](voz.md#voice-region-object) objects that can be used when setting a [voice channel's ](https://docs.discord.food/resources/channel#channel-object).

#### [Get Guild Voice Regions](voz.md#get-guild-voice-regions) <a href="#get-guild-voice-regions" id="get-guild-voice-regions"></a>

`GET/guilds/{guild.id}/regions`

Returns a list of [voice region](voz.md#voice-region-object) objects that can be used when setting a [voice channel's ](https://docs.discord.food/resources/channel#channel-object). Unlike the similar [Get Voice Regions](voz.md#get-voice-regions) route, this returns VIP servers when the guild is VIP-enabled.

#### [Upload Voice Public Key](voz.md#upload-voice-public-key) <a href="#upload-voice-public-key" id="upload-voice-public-key"></a>

`PUT/voice/public-keys`

Uploads a persistent public key used for voice encryption. Returns a 204 empty response on success.

<details>

<summary>More InformationHow do I generate and use this key?</summary>

This key is used for persistent keypair signatures in the DAVE protocol. For further details, see the [whitepaper](https://daveprotocol.com/#persistent-signature-keypairs).

Generating a keypair and signature can be done with the following Python pseudocode:

```
import base64import requestsfrom cryptography.hazmat.primitives.asymmetric import ecfrom cryptography.hazmat.primitives import hashesfrom cryptography.hazmat.primitives import serializationAPI_ENDPOINT = 'https://discord.com/api/v9'private_key = ec.generate_private_key(ec.SECP256R1())public_key = private_key.public_key().public_bytes(serialization.Encoding.X962, serialization.PublicFormat.CompressedPoint)# https://datatracker.ietf.org/doc/html/rfc9000#name-variable-length-integer-encdef encode_quic_varint(data: bytes) -> bytes:    length = len(data)    if length < 2**6:        return bytes([length]) + data    elif length < 2**14:        return bytes([(1 << 6) | (length >> 8), length & 0xFF]) + data    elif length < 2**30:        return bytes([(2 << 6) | (length >> 24), (length >> 16) & 0xFF, (length >> 8) & 0xFF, length & 0xFF]) + data    else:        return bytes([(3 << 6) | (length >> 56), (length >> 48) & 0xFF, (length >> 40) & 0xFF, (length >> 32) & 0xFF, (length >> 24) & 0xFF, (length >> 16) & 0xFF, (length >> 8) & 0xFF, length & 0xFF]) + data# https://datatracker.ietf.org/doc/html/rfc9420/#name-signingdef sign_with_label(private_key: ec.EllipticCurvePrivateKey, label: str, content: bytes) -> bytes:    label_bytes = b'MLS 1.0 ' + label.encode('ascii')    sign_content = encode_quic_varint(label_bytes) + encode_quic_varint(content)    signature = private_key.sign(sign_content, ec.ECDSA(hashes.SHA256()))    return signature# The session ID is the static_client_session_id value from the READY payloadsession_id = '00000000-0000-0000-0000-000000000000'.encode('ascii')signature = sign_with_label(private_key, "DiscordSelfSignature", session_id + b':' + public_key)data = {    'key_version': 1,    'public_key': 'data:application/octet-stream;base64,' + base64.b64encode(public_key).decode('utf-8'),    'signature': 'data:application/octet-stream;base64,' + base64.b64encode(signature).decode('utf-8'),}headers = {    'authorization': 'token',    # Rest of headers here}r = requests.put('%s/voice/public-keys' % API_ENDPOINT, json=data, headers=headers)r.raise_for_status()
```

</details>

[**JSON Params**](voz.md#json-params)

| Field        | Type                                                     | Description                                                                                                                                        |
| ------------ | -------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| key\_version | integer                                                  | The version of the persistent key protocol (currently 1)                                                                                           |
| public\_key  | [cdn data](https://docs.discord.food/reference#cdn-data) | The X9.62 P256 public key data                                                                                                                     |
| signature    | [cdn data](https://docs.discord.food/reference#cdn-data) | An MLS style self-signature of the public key data with application label `DiscordSelfSignature` and content `static_client_session_id:public_key` |

#### [Verify Voice Public Key](voz.md#verify-voice-public-key) <a href="#verify-voice-public-key" id="verify-voice-public-key"></a>

`POST/voice/{user.id}/match-public-key`

Verifies a user's persistent public key for voice encryption against their uploaded ones.

<details>

<summary>More InformationWhere do I get this key?</summary>

This key is used by another user to encrypt voice data. For further details, see the [whitepaper](https://daveprotocol.com/#persistent-verification).

</details>

[**JSON Params**](voz.md#json-params)

| Field        | Type                                                     | Description                                              |
| ------------ | -------------------------------------------------------- | -------------------------------------------------------- |
| key\_version | integer                                                  | The version of the persistent key protocol (currently 1) |
| public\_key  | [cdn data](https://docs.discord.food/reference#cdn-data) | The X9.62 P256 public key data                           |

[**Response Body**](voz.md#response-body)

| Field     | Type    | Description                                                                      |
| --------- | ------- | -------------------------------------------------------------------------------- |
| is\_match | boolean | Whether the public key matches one of the user's uploaded persistent public keys |

#### [Get Voice Filters Catalog](voz.md#get-voice-filters-catalog) <a href="#get-voice-filters-catalog" id="get-voice-filters-catalog"></a>

`GET/voice-filters/catalog`

Returns the voice filters that can be used by the client.

[**Query String Params**](voz.md#query-string-params)

| Field        | Type           | Description                                                                                           |
| ------------ | -------------- | ----------------------------------------------------------------------------------------------------- |
| vfm\_version | integer        | The version of the [voice filter native module](https://docs.discord.food/topics/client-distribution) |
| models?      | array\[string] | The IDs of the ONNX models to return in the response                                                  |

[**Response Body**](voz.md#response-body)

| Field                 | Type                                                                        | Description                                                |
| --------------------- | --------------------------------------------------------------------------- | ---------------------------------------------------------- |
| limited\_time\_voices | [limited time voices](voz.md#limited-time-voices-structure) object          | The current sets of limited time voice filters available   |
| models? <sup>1</sup>  | map\[str, [voice filter model](voz.md#voice-filter-model-structure) object] | The ONNX machine learning models used by the voice filters |
| voices?               | array\[[voice filter](voz.md#voice-filter-structure) object]                | The voice filters available to the client                  |

<sup>1</sup> This field is empty if no models were provided in the query parameter in voice filter module versions 6 and above.

[**Limited Time Voices Structure**](voz.md#limited-time-voices-structure)

| Field               | Type              | Description                                     |
| ------------------- | ----------------- | ----------------------------------------------- |
| current\_set        | array\[string]    | The currently free voice filters                |
| current\_set\_end   | ISO8601 timestamp | When the current set will no longer be free     |
| current\_set\_start | ISO8601 timestamp | When the current set will start being free      |
| next\_set           | array\[string]    | The next set of voice filters that will be free |
| next\_set\_end      | ISO8601 timestamp | When the next set ends                          |
| next\_set\_start    | ISO8601 timestamp | When the next set starts                        |

[**Voice Filter Model Structure**](voz.md#voice-filter-model-structure)

| Field | Type   | Description                   |
| ----- | ------ | ----------------------------- |
| url   | string | The CDN URL to the ONNX model |

[**Voice Filter Structure**](voz.md#voice-filter-structure)

| Field                        | Type              | Description                                                      |
| ---------------------------- | ----------------- | ---------------------------------------------------------------- |
| id                           | string            | The ID of the voice filter                                       |
| models? <sup>1</sup>         | array\[string]    | The IDs of the ONNX models used by the voice filter              |
| requires\_premium            | boolean           | Whether the voice filter requires a premium (Nitro) subscription |
| limited\_time\_free\_ends?   | ISO8601 timestamp | When the voice filter will no longer be free to use              |
| limited\_time\_free\_starts? | ISO8601 timestamp | When the voice filter will start being free to use               |
| available                    | boolean           | Whether the voice filter is available to use                     |

<sup>1</sup> This field is no longer used and is serialized as an empty object in voice filter module versions 6 and above.

[**Example Response**](voz.md#example-response)

```
{  "limited_time_voices": {    "current_set": ["solara", "robot"],    "current_set_start": "2025-05-22T00:00:00+00:00",    "current_set_end": "2025-05-22T23:59:59+00:00",    "next_set": ["tunes", "robot"],    "next_set_start": "2025-05-23T00:00:00+00:00",    "next_set_end": "2025-05-23T23:59:59+00:00"  },  "models": {    "vocoder_large_1": {      "url": "https://cdn.discordapp.com/assets/content/XXX.onnx"    },    "asr_large": {      "url": "https://cdn.discordapp.com/assets/content/XXX.onnx"    },    "pitch_small_3": {      "url": "https://cdn.discordapp.com/assets/content/XXX.onnx"    }  },  "voices": [    {      "id": "skye",      "models": ["vocoder_large_1", "asr_large", "pitch_small_3"],      "requires_premium": true,      "limited_time_free_ends": "2025-05-22T23:59:59+00:00",      "limited_time_free_starts": "2025-05-22T00:00:00+00:00",      "available": true    }  ]}
```

#### [Get Current User Voice State](voz.md#get-current-user-voice-state) <a href="#get-current-user-voice-state" id="get-current-user-voice-state"></a>

`GET/guilds/{guild.id}/voice-states/@me`

Returns the current user's [voice state](voz.md#voice-state-object) object in the guild.

#### [Get User Voice State](voz.md#get-user-voice-state) <a href="#get-user-voice-state" id="get-user-voice-state"></a>

`GET/guilds/{guild.id}/voice-states/{user.id}`

Returns the specified user's [voice state](voz.md#voice-state-object) object in the guild.

#### [Modify Current User Voice State](voz.md#modify-current-user-voice-state) <a href="#modify-current-user-voice-state" id="modify-current-user-voice-state"></a>

`PATCH/guilds/{guild.id}/voice-states/@me`

Updates the current user's voice state in the given guild ID. Returns a 204 empty response on success. Fires a [Voice State Update](https://docs.discord.food/topics/gateway-events#voice-state-update) Gateway event.

<details>

<summary>CaveatsModifying voice states is subject to some restrictions</summary>

There are currently several caveats for this endpoint:

* must point to a stage channel
* Current user must already have joined
* You must have the permission to unsuppress yourself; you can always suppress yourself
* You must have the permission to request to speak; you can always clear your own request to speak
* You can only set to the present or a future time

</details>

[**JSON Params**](voz.md#json-params)

| Field                          | Type               | Description                                    |
| ------------------------------ | ------------------ | ---------------------------------------------- |
| channel\_id?                   | snowflake          | The ID of the channel the user is currently in |
| suppress?                      | boolean            | Whether the user is suppressed in the channel  |
| request\_to\_speak\_timestamp? | ?ISO8601 timestamp | When the user requested to speak               |

#### [Modify User Voice State](voz.md#modify-user-voice-state) <a href="#modify-user-voice-state" id="modify-user-voice-state"></a>

`PATCH/guilds/{guild.id}/voice-states/{user.id}`

Updates another user's voice state in the given guild ID. Returns a 204 empty response on success. Fires a [Voice State Update](https://docs.discord.food/topics/gateway-events#voice-state-update) Gateway event.

<details>

<summary>CaveatsModifying voice states is subject to some restrictions</summary>

There are currently several caveats for this endpoint:

* must point to a stage channel
* Target user must already have joined
* You must have the permission
* When unsuppressed, user accounts will have their set to the current time; bot users will not
* When suppressed, the user will have their removed

</details>

[**JSON Params**](voz.md#json-params)

| Field       | Type      | Description                                    |
| ----------- | --------- | ---------------------------------------------- |
| channel\_id | snowflake | The ID of the channel the user is currently in |
| suppress?   | boolean   | Whether the user is suppressed in the channel  |

#### [Send Voice Channel Effect](voz.md#send-voice-channel-effect) <a href="#send-voice-channel-effect" id="send-voice-channel-effect"></a>

`POST/channels/{channel.id}/voice-channel-effects`

Sends a voice channel effect to a voice channel. Returns a 204 empty response on success. Fires a [Voice Channel Effect Send](https://docs.discord.food/topics/gateway-events#voice-channel-effect-send) Gateway event.

[**JSON Params**](voz.md#json-params)

| Field            | Type       | Description                                                                                                |
| ---------------- | ---------- | ---------------------------------------------------------------------------------------------------------- |
| animation\_type? | ?integer   | The [type of emoji animation](voz.md#voice-channel-effect-animation-type), if applicable (default `BASIC`) |
| animation\_id?   | ?integer   | The ID of the emoji animation (0-20, default 0)                                                            |
| emoji\_id?       | ?snowflake | The ID of the custom emoji to send                                                                         |
| emoji\_name?     | ?string    | The emoji name or unicode character of the emoji to send                                                   |

[**Voice Channel Effect Animation Type**](voz.md#voice-channel-effect-animation-type)

| Value | Name    | Description                                              |
| ----- | ------- | -------------------------------------------------------- |
| 0     | PREMIUM | A fun animation, requires a premium (Nitro) subscription |
| 1     | BASIC   | The standard animation                                   |

#### [Modify Stream](voz.md#modify-stream) <a href="#modify-stream" id="modify-stream"></a>

`PATCH/streams/{stream_key}/stream`

Modifies the stream. User must be the owner of the stream. Returns a 204 empty response on success. Fires a [Stream Update](https://docs.discord.food/topics/gateway-events#stream-update) Gateway event.

[**JSON Params**](voz.md#json-params)

| Field   | Type   | Description                                                      |
| ------- | ------ | ---------------------------------------------------------------- |
| region? | string | The [voice region](voz.md#voice-region-object) ID for the stream |

#### [Get Stream Preview](voz.md#get-stream-preview) <a href="#get-stream-preview" id="get-stream-preview"></a>

`GET/streams/{stream_key}/preview`

Returns a URL to a stream preview for the given [stream key](https://docs.discord.food/topics/gateway-events#stream-key). Requires the permission in the stream's channel.

[**Response Body**](voz.md#response-body)

| Field | Type   | Description                       |
| ----- | ------ | --------------------------------- |
| url   | string | The CDN URL to the stream preview |

#### [Upload Stream Preview](voz.md#upload-stream-preview) <a href="#upload-stream-preview" id="upload-stream-preview"></a>

`POST/streams/{stream_key}/preview`

Uploads a stream preview for the given [stream key](https://docs.discord.food/topics/gateway-events#stream-key). User must be the owner of the stream. Returns a 204 empty response on success.

[**JSON Params**](voz.md#json-params)

| Field     | Type                                                       | Description              |
| --------- | ---------------------------------------------------------- | ------------------------ |
| thumbnail | [image data](https://docs.discord.food/reference#cdn-data) | The stream preview image |

#### [Upload Video Stream Preview](voz.md#upload-video-stream-preview) <a href="#upload-video-stream-preview" id="upload-video-stream-preview"></a>

`POST/streams/{stream_key}/preview/video`

Uploads a stream preview video for the given [stream key](https://docs.discord.food/topics/gateway-events#stream-key). User must be the owner of the stream. Returns a 204 empty response on success.

[**Form Params**](voz.md#form-params)

| Field | Type          | Description                        |
| ----- | ------------- | ---------------------------------- |
| file  | file contents | The stream preview video to upload |

#### [Broadcast Stream Notification](voz.md#broadcast-stream-notification) <a href="#broadcast-stream-notification" id="broadcast-stream-notification"></a>

`POST/streams/{stream_key}/notify`

Broadcasts a stream notification to all friends of the current user that are in the same guild as the stream and have stream notifications enabled. User must be the owner of the stream and must be streaming in a guild. Returns a 204 empty response on success.

---
icon: laptop-mobile
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

# Conexiones de voz

### [Voice Connections](conexiones-de-voz.md#voice-connections) <a href="#voice-connections" id="voice-connections"></a>

Voice connections operate in a similar fashion to the [Gateway](https://docs.discord.food/topics/gateway#connections) connection. However, they use a different set of payloads and a separate UDP-based connection for RTC data transmission. Because UDP is generally used for both receiving and transmitting RTC data, your client _must_ be able to receive UDP packets, even through a firewall or NAT (see [UDP Hole Punching](https://en.wikipedia.org/wiki/UDP_hole_punching) for more information). The Discord voice servers implement functionality (see [IP Discovery](conexiones-de-voz.md#ip-discovery)) for discovering the local machine's remote UDP IP/Port, which can assist in some network configurations. If you cannot support a UDP connection, you may implement a [WebRTC connection](conexiones-de-voz.md#webrtc-connections) instead.

Audio and video from a "Go Live" stream require a [separate connection to another voice server](conexiones-de-voz.md#streams). Only microphone and camera data are sent over the normal connection.

### [Voice Gateway](conexiones-de-voz.md#voice-gateway) <a href="#voice-gateway" id="voice-gateway"></a>

To ensure that you have the most up-to-date information, please use [version 8](conexiones-de-voz.md#gateway-versions). Otherwise, the events and commands documented here may not reflect what you receive over the socket. Video is only fully supported on Gateway v5 and above.

[**Gateway Versions**](conexiones-de-voz.md#gateway-versions)

| Version | Status      | Change                                                                                                                                          |
| ------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| 9       | Recommended | Added `channel_id` to [Opcode 0 Identify](conexiones-de-voz.md#identify-structure) and [Opcode 7 Resume](conexiones-de-voz.md#resume-structure) |
| 8       | Recommended | Added buffered resuming                                                                                                                         |
| 7       | Available   | Added [Opcode 17 Channel Options Update](conexiones-de-voz.md#gateway-events)                                                                   |
| 6       | Available   | Added [Opcode 16 Voice Backend Version](conexiones-de-voz.md#gateway-events)                                                                    |
| 5       | Available   | Added [Opcode 15 Media Sink Wants](conexiones-de-voz.md#gateway-events)                                                                         |
| 4       | Available   | Changed [speaking status](conexiones-de-voz.md#speaking-flags) from boolean to bitmask                                                          |
| 3       | Deprecated  | Added video functionality, consolidated [Opcode 1 Hello](conexiones-de-voz.md#gateway-events) payload                                           |
| 2       | Deprecated  | Changed Gateway heartbeat reply to [Opcode 6 Heartbeak ACK](conexiones-de-voz.md#gateway-events)                                                |
| 1       | Deprecated  | Initial version                                                                                                                                 |

[**Gateway Commands**](conexiones-de-voz.md#gateway-commands)

| Name                                                                          | Description                               |
| ----------------------------------------------------------------------------- | ----------------------------------------- |
| [Identify](conexiones-de-voz.md#identify-structure)                           | Start a new voice connection              |
| [Resume](conexiones-de-voz.md#resume-structure)                               | Resume a dropped connection               |
| [Heartbeat](conexiones-de-voz.md#example-heartbeat)                           | Maintain an active WebSocket connection   |
| [Media Sink Wants](conexiones-de-voz.md#simulcasting)                         | Indicate the desired media stream quality |
| [Select Protocol](conexiones-de-voz.md#select-protocol-structure)             | Select the voice protocol and mode        |
| [Session Update](conexiones-de-voz.md#session-update-structure-\(send\))      | Indicate the client's supported codecs    |
| [Speaking](conexiones-de-voz.md#speaking-structure)                           | Indicate the user's speaking state        |
| [Voice Backend Version](conexiones-de-voz.md#voice-backend-version-structure) | Request the current voice backend version |

[**Gateway Events**](conexiones-de-voz.md#gateway-events)

| Name                                                                                    | Description                                                                                                          |
| --------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| [Hello](conexiones-de-voz.md#hello-structure)                                           | Defines the heartbeat interval                                                                                       |
| [Heartbeat ACK](conexiones-de-voz.md#example-heartbeat-ack)                             | Acknowledges a received client heartbeat                                                                             |
| [Client Connect](conexiones-de-voz.md#client-connections)                               | A user connected to voice, also sent on initial connection to inform the client of existing users                    |
| [Client Flags](conexiones-de-voz.md#client-connections)                                 | Contains the flags of a user that connected to voice, also sent on initial connection for each existing user         |
| [Client Platform](conexiones-de-voz.md#client-connections)                              | Contains the platform type of a user that connected to voice, also sent on initial connection for each existing user |
| [Client Disconnect](conexiones-de-voz.md#client-disconnections)                         | A user disconnected from voice                                                                                       |
| [Media Sink Wants](conexiones-de-voz.md#simulcasting)                                   | Requested media stream quality updated                                                                               |
| [Ready](conexiones-de-voz.md#ready-structure)                                           | Contains SSRC, IP/Port, experiment, and encryption mode information                                                  |
| [Resumed](conexiones-de-voz.md#example-resumed)                                         | Acknowledges a successful connection resume                                                                          |
| [Session Description](conexiones-de-voz.md#session-description-structure)               | Acknowledges a successful protocol selection and contains the information needed to send/receive RTC data            |
| [Session Update](conexiones-de-voz.md#session-update-structure-\(receive\))             | Client session description changed                                                                                   |
| [Speaking](conexiones-de-voz.md#speaking-structure)                                     | User speaking state updated                                                                                          |
| [Voice Backend Version](conexiones-de-voz.md#example-voice-backend-version-\(receive\)) | Current voice backend version information, as requested by the client                                                |

### [Connecting to Voice](conexiones-de-voz.md#connecting-to-voice) <a href="#connecting-to-voice" id="connecting-to-voice"></a>

#### [Retrieving Voice Server Information](conexiones-de-voz.md#retrieving-voice-server-information) <a href="#retrieving-voice-server-information" id="retrieving-voice-server-information"></a>

The first step in connecting to a voice server (and in turn, a guild's voice channel or private channel) is formulating a request that can be sent to the [Gateway](https://docs.discord.food/topics/gateway), which will return information about the voice server we will connect to. Because Discord's voice platform is widely distributed, users **should never** cache or save the results of this call. To inform the Gateway of our intent to establish voice connectivity, we first send an [Update Voice State](https://docs.discord.food/topics/gateway-events#update-voice-state) payload.

If our request succeeded, the Gateway will respond with _two_ events—a [Voice State Update](https://docs.discord.food/topics/gateway-events#voice-state-update) event and a [Voice Server Update](https://docs.discord.food/topics/gateway-events#voice-server-update) event—meaning you must properly wait for both events before continuing. The first will contain a new key, , and the second will provide voice server information we can use to establish a new voice connection.

With this information, we can move on to establishing a voice WebSocket connection.

When changing channels within the same guild, it is possible to receive a [Voice Server Update](https://docs.discord.food/topics/gateway-events#voice-server-update) with the same as the existing session. However, the will be changed and you cannot re-use the previous session during a channel change, even if the endpoint remains the same.

#### [Establishing a Voice WebSocket Connection](conexiones-de-voz.md#establishing-a-voice-websocket-connection) <a href="#establishing-a-voice-websocket-connection" id="establishing-a-voice-websocket-connection"></a>

Once we retrieve a , , and information, we can connect and handshake with the voice server over another secure WebSocket. Unlike the Gateway endpoint we receive in a [Get Gateway](https://docs.discord.food/topics/gateway#get-gateway) request, the endpoint received from our [Voice Server Update](https://docs.discord.food/topics/gateway-events#voice-server-update) payload does not contain a URL protocol, so some libraries may require manually prepending it with before connecting. Once connected to the voice WebSocket endpoint, we can immediately send an [Opcode 0 Identify](conexiones-de-voz.md#gateway-commands) payload:

[**Identify Structure**](conexiones-de-voz.md#identify-structure)

| Field                    | Type                                                           | Description                                                               |
| ------------------------ | -------------------------------------------------------------- | ------------------------------------------------------------------------- |
| server\_id               | snowflake                                                      | The ID of the guild, private channel, stream, or lobby being connected to |
| channel\_id <sup>1</sup> | snowflake                                                      | The ID of the channel being connected to                                  |
| user\_id                 | snowflake                                                      | The ID of the current user                                                |
| session\_id              | string                                                         | The session ID of the current session                                     |
| token                    | string                                                         | The voice token for the current session                                   |
| video?                   | boolean                                                        | Whether this connection supports video (default false)                    |
| streams?                 | array\[[stream](conexiones-de-voz.md#stream-structure) object] | [Simulcast](conexiones-de-voz.md#simulcasting) streams to send            |

<sup>1</sup> Only required for Gateway v9 and above.

[**Stream Structure**](conexiones-de-voz.md#stream-structure)

| Field             | Type                                                                         | Description                                                         |
| ----------------- | ---------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| type <sup>1</sup> | string                                                                       | The [type of media stream](conexiones-de-voz.md#media-type) to send |
| rid               | string                                                                       | The RTP stream ID                                                   |
| quality?          | integer                                                                      | The media quality to send (0-100, default 0)                        |
| active?           | boolean                                                                      | Whether the stream is active (default false)                        |
| max\_bitrate?     | integer                                                                      | The maximum bitrate to send in bps                                  |
| max\_framerate?   | integer                                                                      | The maximum framerate to send in fps                                |
| max\_resolution?  | [stream resolution](conexiones-de-voz.md#stream-resolution-structure) object | The maximum resolution to send                                      |
| ssrc?             | integer                                                                      | The SSRC of the stream                                              |
| rtx\_ssrc?        | integer                                                                      | The SSRC of the retransmission stream                               |

<sup>1</sup> Currently, this field is ignored and always set to .

[**Media Type**](conexiones-de-voz.md#media-type)

| Value  | Description |
| ------ | ----------- |
| audio  | Audio       |
| video  | Video       |
| screen | Screenshare |
| test   | Speed test  |

[**Stream Resolution Structure**](conexiones-de-voz.md#stream-resolution-structure)

| Field  | Type   | Description                                                        |
| ------ | ------ | ------------------------------------------------------------------ |
| type   | string | The [resolution type](conexiones-de-voz.md#resolution-type) to use |
| width  | number | The fixed resolution width, or 0 for source                        |
| height | number | The fixed resolution height, or 0 for source                       |

[**Resolution Type**](conexiones-de-voz.md#resolution-type)

| Value  | Description       |
| ------ | ----------------- |
| fixed  | Fixed resolution  |
| source | Source resolution |

[**Example Identify**](conexiones-de-voz.md#example-identify)

```
{  "op": 0,  "d": {    "server_id": "41771983423143937",    "user_id": "104694319306248192",    "session_id": "30f32c5d54ae86130fc4a215c7474263",    "token": "66d29164ee8cd919",    "video": true,    "streams": [      { "type": "video", "rid": "100", "quality": 100 },      { "type": "video", "rid": "50", "quality": 50 }    ]  }}
```

The voice server should respond with an [Opcode 2 Ready](conexiones-de-voz.md#gateway-events) payload, which informs us of the SSRC, connection IP/port, supported encryption modes, and experiments the voice server supports:

[**Ready Structure**](conexiones-de-voz.md#ready-structure)

| Field       | Type                                                           | Description                                                              |
| ----------- | -------------------------------------------------------------- | ------------------------------------------------------------------------ |
| ssrc        | integer                                                        | The SSRC of the user's voice connection                                  |
| ip          | string                                                         | The IP address of the voice server                                       |
| port        | integer                                                        | The port of the voice server                                             |
| modes       | array\[string]                                                 | Supported [voice encryption modes](conexiones-de-voz.md#encryption-mode) |
| experiments | array\[string]                                                 | Available voice experiments                                              |
| streams     | array\[[stream](conexiones-de-voz.md#stream-structure) object] | Populated simulcast streams                                              |

[**Example Ready**](conexiones-de-voz.md#example-ready)

```
{  "op": 2,  "d": {    "ssrc": 12871,    "ip": "127.0.0.1",    "port": 1234,    "modes": [      "aead_aes256_gcm_rtpsize",      "aead_aes256_gcm",      "aead_xchacha20_poly1305_rtpsize",      "xsalsa20_poly1305_lite_rtpsize",      "xsalsa20_poly1305_lite",      "xsalsa20_poly1305_suffix",      "xsalsa20_poly1305"    ],    "experiments": ["fixed_keyframe_interval"],    "streams": [      {        "type": "video",        "ssrc": 12872,        "rtx_ssrc": 12873,        "rid": "50",        "quality": 50,        "active": false      },      {        "type": "video",        "ssrc": 12874,        "rtx_ssrc": 12875,        "rid": "100",        "quality": 100,        "active": false      }    ]  }}
```

#### [Establishing a Voice Connection](conexiones-de-voz.md#establishing-a-voice-connection) <a href="#establishing-a-voice-connection" id="establishing-a-voice-connection"></a>

Once we receive the properties of a voice server from our [Ready](conexiones-de-voz.md#ready-structure) payload, we can proceed to the final step of voice connections, which entails establishing and handshaking a connection for RTC data. First, we establish either a [UDP connection](conexiones-de-voz.md#udp-connections) using the [Ready](conexiones-de-voz.md#ready-structure) payload data, or prepare a [WebRTC](conexiones-de-voz.md#webrtc-connections) SDP. We then send an [Opcode 1 Select Protocol](conexiones-de-voz.md#gateway-events) with details about our connection:

[**Select Protocol Structure**](conexiones-de-voz.md#select-protocol-structure)

| Field                | Type                                                                     | Description                                                                      |
| -------------------- | ------------------------------------------------------------------------ | -------------------------------------------------------------------------------- |
| protocol             | string                                                                   | The [voice protocol](conexiones-de-voz.md#protocol-type) to use                  |
| data                 | ?[protocol data](conexiones-de-voz.md#protocol-data-structure) \| string | The voice connection data or WebRTC SDP                                          |
| rtc\_connection\_id? | string                                                                   | The UUID RTC connection ID, used for analytics                                   |
| codecs?              | array\[[codec](conexiones-de-voz.md#codec-structure) object]             | The supported audio/video codecs                                                 |
| experiments?         | array\[string]                                                           | The [received voice experiments](conexiones-de-voz.md#ready-structure) to enable |

[**Protocol Type**](conexiones-de-voz.md#protocol-type)

| Value          | Description                                                           |
| -------------- | --------------------------------------------------------------------- |
| udp            | [Standard UDP voice connection](conexiones-de-voz.md#udp-connections) |
| webrtc         | [WebRTC voice connection](conexiones-de-voz.md#webrtc-connections)    |
| ~~webrtc-p2p~~ | ~~WebRTC peer-to-peer voice connection~~                              |

[**Protocol Data Structure**](conexiones-de-voz.md#protocol-data-structure)

| Field                | Type    | Description                                                        |
| -------------------- | ------- | ------------------------------------------------------------------ |
| address <sup>1</sup> | string  | The discovered IP address of the client                            |
| port <sup>1</sup>    | integer | The discovered UDP port of the client                              |
| mode                 | string  | The [encryption mode](conexiones-de-voz.md#encryption-mode) to use |

<sup>1</sup> These fields are only used to receive RTC data. If you only wish to send frames and do not care about receiving, you can randomize these values.

[**Codec Structure**](conexiones-de-voz.md#codec-structure)

| Field                      | Type    | Description                                                                 |
| -------------------------- | ------- | --------------------------------------------------------------------------- |
| name                       | string  | The [name of the codec](conexiones-de-voz.md#supported-codecs)              |
| type                       | string  | The [type of codec](conexiones-de-voz.md#supported-codecs)                  |
| priority <sup>1</sup>      | integer | The preferred priority of the codec as a multiple of 1000 (unique per type) |
| payload\_type <sup>2</sup> | integer | The dynamic RTP payload type of the codec                                   |
| rtx\_payload\_type?        | integer | The dynamic RTP payload type of the retransmission codec (video-only)       |
| encode?                    | boolean | Whether the client supports encoding this codec (default `true`)            |
| decode?                    | boolean | Whether the client supports decoding this codec (default `true`)            |

<sup>1</sup> For audio, Opus is the only available codec and should be priority .

<sup>2</sup> No payload type should be set to , as it is reserved for probe packets.

[**Supported Codecs**](conexiones-de-voz.md#supported-codecs)

Providing codecs is optional due to backwards compatibility with old clients and bots that do not handle video. If the client does not provide any codecs, the server assumes an Opus audio codec with a payload type of and no specific video codec. If no clients with specified video codecs are connected, the server defaults to H264.

| Type  | Name | Status    |
| ----- | ---- | --------- |
| audio | opus | Required  |
| video | AV1  | Preferred |
| video | H265 | Preferred |
| video | H264 | Default   |
| video | VP8  | Available |
| video | VP9  | Available |

[**Example Select Protocol**](conexiones-de-voz.md#example-select-protocol)

```
{  "op": 1,  "d": {    "protocol": "udp",    "data": {      "address": "127.0.0.1",      "port": 1337,      "mode": "aead_aes256_gcm_rtpsize"    },    "codecs": [      {        "name": "opus",        "type": "audio",        "priority": 1000,        "payload_type": 120      },      {        "name": "AV1",        "type": "video",        "priority": 1000,        "payload_type": 101,        "rtx_payload_type": 102,        "encode": false,        "decode": true      },      {        "name": "H264",        "type": "video",        "priority": 2000,        "payload_type": 103,        "rtx_payload_type": 104,        "encode": true,        "decode": true      }    ],    "rtc_connection_id": "d6b92f64-40df-48eb-8bce-7facb043149a",    "experiments": ["fixed_keyframe_interval"]  }}
```

[**Encryption Mode**](conexiones-de-voz.md#encryption-mode)

The RTP size variants determine the unencrypted size of the RTP header in [the same way as SRTP](https://tools.ietf.org/html/rfc3711#section-3.1), which considers CSRCs and (optionally) the extension preamble to be part of the unencrypted header. The deprecated variants use a fixed size unencrypted header for RTP.

The Gateway will report what encryption modes are available in [Opcode 2 Ready](conexiones-de-voz.md#gateway-events). Compatible modes will always include but may not include depending on the underlying hardware. You must support . You should prefer to use when it is available.

| Value                              | Name                               | Nonce                                                 | Status     |
| ---------------------------------- | ---------------------------------- | ----------------------------------------------------- | ---------- |
| aead\_aes256\_gcm\_rtpsize         | AEAD AES256 GCM (RTP Size)         | 32-bit incremental integer value appended to payload  | Preferred  |
| aead\_xchacha20\_poly1305\_rtpsize | AEAD XChaCha20 Poly1305 (RTP Size) | 32-bit incremental integer value appended to payload  | Required   |
| xsalsa20\_poly1305\_lite\_rtpsize  | XSalsa20 Poly1305 Lite (RTP Size)  | 32-bit incremental integer value appended to payload  | Deprecated |
| aead\_aes256\_gcm                  | AEAD AES256-GCM                    | 32-bit incremental integer value appended to payload  | Deprecated |
| xsalsa20\_poly1305                 | XSalsa20 Poly1305                  | Copy of RTP header                                    | Deprecated |
| xsalsa20\_poly1305\_suffix         | XSalsa20 Poly1305 (Suffix)         | 24 random bytes                                       | Deprecated |
| xsalsa20\_poly1305\_lite           | XSalsa20 Poly1305 (Lite)           | 32-bit incremental integer value, appended to payload | Deprecated |

Finally, the voice server will respond with an [Opcode 4 Session Description](conexiones-de-voz.md#gateway-events) that includes the and , a 32 byte array used for [sending and receiving](conexiones-de-voz.md#sending-and-receiving-voice) RTC data:

[**Session Description Structure**](conexiones-de-voz.md#session-description-structure)

| Field               | Type            | Description                                                                                  |
| ------------------- | --------------- | -------------------------------------------------------------------------------------------- |
| audio\_codec        | string          | The audio codec to use                                                                       |
| video\_codec        | string          | The video codec to use                                                                       |
| media\_session\_id  | string          | The media session ID, used for analytics                                                     |
| mode?               | string          | The [encryption mode](conexiones-de-voz.md#encryption-mode) to use, not applicable to WebRTC |
| secret\_key?        | array\[integer] | The 32 byte secret key used for encryption, not applicable to WebRTC                         |
| sdp?                | string          | The WebRTC session description protocol                                                      |
| keyframe\_interval? | integer         | The keyframe interval in milliseconds                                                        |

[**Example Session Description**](conexiones-de-voz.md#example-session-description)

```
{  "op": 4,  "d": {    "audio_codec": "opus",    "media_session_id": "89f1d62f166b948746f7646713d39dbb",    "mode": "aead_aes256_gcm_rtpsize",    "secret_key": [ ... ],    "video_codec": "H264"  }}
```

We can now start sending and receiving RTC data over the previously established [UDP](conexiones-de-voz.md#udp-connections) or [WebRTC](conexiones-de-voz.md#webrtc-connections) connection.

#### [Session Updates](conexiones-de-voz.md#session-updates) <a href="#session-updates" id="session-updates"></a>

At any time, the client may update the they support using an [Opcode 14 Session Update](conexiones-de-voz.md#gateway-events). If a user joins that does not support the current codecs, or a user indicates that they no longer support the current codecs, the voice server will send an [Opcode 14 Session Update](conexiones-de-voz.md#gateway-events):

This may also be sent to update the current or .

[**Session Update Structure (Send)**](conexiones-de-voz.md#session-update-structure-\(send\))

| Field  | Type                                                         | Description                      |
| ------ | ------------------------------------------------------------ | -------------------------------- |
| codecs | array\[[codec](conexiones-de-voz.md#codec-structure) object] | The supported audio/video codecs |

[**Session Update Structure (Receive)**](conexiones-de-voz.md#session-update-structure-\(receive\))

| Field               | Type    | Description                                  |
| ------------------- | ------- | -------------------------------------------- |
| audio\_codec?       | string  | The new audio codec to use                   |
| video\_codec?       | string  | The new video codec to use                   |
| media\_session\_id? | string  | The new media session ID, used for analytics |
| keyframe\_interval? | integer | The keyframe interval in milliseconds        |

### [Heartbeating](conexiones-de-voz.md#heartbeating) <a href="#heartbeating" id="heartbeating"></a>

In order to maintain your WebSocket connection, you need to continuously send heartbeats at the interval determined in [Opcode 8 Hello](conexiones-de-voz.md#gateway-events).

This is sent at the start of the connection. Be warned that the [Opcode 8 Hello](conexiones-de-voz.md#gateway-events) structure differs by Gateway version. Versions below v3 follow a flat structure without or fields, including only a single field. Be sure to expect this different format based on your version.

This heartbeat interval is the minimum interval you should heartbeat at. You can heartbeat at a faster interval if you wish. For example, the web client uses a heartbeat interval of if the Gateway version is v4 or above, and otherwise. The desktop client uses the provided heartbeat interval if the Gateway version is v4 or above, and otherwise.

[**Hello Structure**](conexiones-de-voz.md#hello-structure)

| Field               | Type    | Description                                                           |
| ------------------- | ------- | --------------------------------------------------------------------- |
| v                   | integer | The [voice server version](conexiones-de-voz.md#gateway-versions)     |
| heartbeat\_interval | integer | The minimum interval (in milliseconds) the client should heartbeat at |

[**Example Hello**](conexiones-de-voz.md#example-hello)

```
{  "op": 8,  "d": {    "v": 8,    "heartbeat_interval": 41250  }}
```

The Gateway may request a heartbeat from the client in some situations by sending an [Opcode 3 Heartbeat](conexiones-de-voz.md#gateway-events). When this occurs, the client should immediately send an [Opcode 3 Heartbeat](conexiones-de-voz.md#gateway-events) without waiting the remainder of the current interval.

After receiving [Opcode 8 Hello](conexiones-de-voz.md#gateway-events), you should send [Opcode 3 Heartbeat](conexiones-de-voz.md#gateway-events)—which contains an integer nonce—every elapsed interval:

[**Heartbeat Structure**](conexiones-de-voz.md#heartbeat-structure)

| Field     | Type    | Description                                              |
| --------- | ------- | -------------------------------------------------------- |
| t         | integer | A unique integer nonce (e.g. the current unix timestamp) |
| seq\_ack? | integer | The last received sequence number                        |

[**Example Heartbeat**](conexiones-de-voz.md#example-heartbeat)

```
{  "op": 3,  "d": {    "t": 1501184119561,    "seq_ack": 10  }}
```

Since Gateway v8, heartbeat messages must include which contains the sequence number of the last numbered message received from the gateway. See [Buffered Resume](conexiones-de-voz.md#buffered-resume) for more information. Previous versions follow a flat structure, with the field representing the field in both the [Heartbeat](conexiones-de-voz.md#example-heartbeat) and [Heartbeat ACK](conexiones-de-voz.md#example-heartbeat-ack) structure.

In return, you will be sent back an [Opcode 6 Heartbeat ACK](conexiones-de-voz.md#gateway-events) that contains the previously sent nonce:

[**Example Heartbeat ACK**](conexiones-de-voz.md#example-heartbeat-ack)

```
{  "op": 6,  "d": {    "t": 1501184119561  }}
```

### [UDP Connections](conexiones-de-voz.md#udp-connections) <a href="#udp-connections" id="udp-connections"></a>

UDP is the most likely protocol that clients will use. First, we open a UDP connection to the IP and port provided in the Ready payload. If required, we can now perform an [IP Discovery](conexiones-de-voz.md#ip-discovery) using this connection. Once we've fully discovered our external IP and UDP port, we can then tell the voice WebSocket what it is by sending a [Select Protocol](conexiones-de-voz.md#select-protocol-structure) as outlined above, and receive our [Session Description](conexiones-de-voz.md#session-description-structure) to begin sending/receiving RTC data.

#### [IP Discovery](conexiones-de-voz.md#ip-discovery) <a href="#ip-discovery" id="ip-discovery"></a>

Generally routers on the Internet mask or obfuscate UDP ports through a process called NAT. Most users who implement voice will want to utilize IP discovery to find their external IP and port which will then be used for receiving voice communications. To retrieve your external IP and port, send the following UDP packet to your voice port (all numeric are big endian):

| Field   | Type                          | Description                                                    | Size     |
| ------- | ----------------------------- | -------------------------------------------------------------- | -------- |
| Type    | Unsigned short (big endian)   | Values 0x1 and 0x2 indicate request and response, respectively | 2 bytes  |
| Length  | Unsigned short (big endian)   | Message length excluding Type and Length fields (value `70`)   | 2 bytes  |
| SSRC    | Unsigned integer (big endian) | The [SSRC](conexiones-de-voz.md#ready-structure) of the user   | 4 bytes  |
| Address | Null-terminated string        | The external IP address of the user                            | 64 bytes |
| Port    | Unsigned short (big endian)   | The external port number of the user                           | 2 bytes  |

#### [Sending and Receiving Voice](conexiones-de-voz.md#sending-and-receiving-voice) <a href="#sending-and-receiving-voice" id="sending-and-receiving-voice"></a>

Voice data sent to and received from Discord should be encoded or decoded with [Opus](https://www.opus-codec.org/), using two channels (stereo) and a sample rate of 48kHz. Video data should be encoded or decoded using the RFCs relevant to the [codec being used](conexiones-de-voz.md#session-description-structure). Data is sent using a [RTP Header](https://www.rfcreader.com/#rfc3550_line548), followed by encrypted Opus audio data or video data. Encryption uses the key passed in [Session Description](conexiones-de-voz.md#session-description-structure) and the nonce formed with the 12 byte header appended with 12 bytes, if required. Discord encrypts with the [libsodium](https://download.libsodium.org/doc/) encryption library.

When receiving data, the user who sent the packet is identified by caching the SSRC and user IDs received from [Speaking](conexiones-de-voz.md#speaking) events. At least one [Speaking](conexiones-de-voz.md#speaking) event for the user is received before any frames are received, so the user ID should always be available.

[**RTP Packet Structure**](conexiones-de-voz.md#rtp-packet-structure)

| Field                        | Type                          | Description                                                                                              | Size    |
| ---------------------------- | ----------------------------- | -------------------------------------------------------------------------------------------------------- | ------- |
| Version + Flags <sup>1</sup> | Unsigned byte                 | The RTP version and flags (always `0x80` for voice)                                                      | 1 byte  |
| Payload Type <sup>2</sup>    | Unsigned byte                 | The [type of payload](conexiones-de-voz.md#codec-structure) (`0x78` with the default Opus configuration) | 1 byte  |
| Sequence                     | Unsigned short (big endian)   | The sequence number of the packet                                                                        | 2 bytes |
| Timestamp                    | Unsigned integer (big endian) | The RTC timestamp of the packet                                                                          | 4 bytes |
| SSRC                         | Unsigned integer (big endian) | The [SSRC](conexiones-de-voz.md#ready-structure) of the user                                             | 4 bytes |
| Payload                      | Binary data                   | Encrypted audio/video data                                                                               | n bytes |

<sup>1</sup> If sending an RTP header extension, the flags should have the extension bit () set (e.g. becomes ).

<sup>2</sup> When sending a final video frame, the payload type should have the M bit () set (e.g. becomes ).

#### [Quality of Service](conexiones-de-voz.md#quality-of-service) <a href="#quality-of-service" id="quality-of-service"></a>

Discord utilizes [RTCP](http://www.rfcreader.com/#rfc3550_line855) packets to monitor the quality of the connection. Sending and parsing these packets is not required, but is recommended to aid in monitoring the connection and synchronizing audio and video streams. The client should send an [RTCP Sender Report](http://www.rfcreader.com/#rfc3550_line1614) roughly every 5 seconds (without padding or reception report blocks) to inform the server of the current state of the connection. Likewise, Discord will send [RTCP Receiver Reports](http://www.rfcreader.com/#rfc3550_line1879) to the client to provide feedback on the quality of the connection.

### [WebRTC Connections](conexiones-de-voz.md#webrtc-connections) <a href="#webrtc-connections" id="webrtc-connections"></a>

WebRTC allows for direct peer-to-peer voice connections, and is most commonly used in browsers. To use WebRTC, you must first send a [Select Protocol](conexiones-de-voz.md#establishing-a-voice-connection) payload as outlined above, with the field set to , and set to the client's WebRTC SDP. The voice server will respond with a [Session Description](conexiones-de-voz.md#session-description-structure) payload, with the field set to the server's WebRTC SDP. The client can then use this SDP to establish a WebRTC connection.

### [Speaking](conexiones-de-voz.md#speaking) <a href="#speaking" id="speaking"></a>

To notify the voice server that you are speaking or have stopped speaking, send an [Opcode 5 Speaking](conexiones-de-voz.md#gateway-commands) payload:

[**Speaking Structure**](conexiones-de-voz.md#speaking-structure)

| Field                 | Type      | Description                                               |
| --------------------- | --------- | --------------------------------------------------------- |
| speaking <sup>1</sup> | integer   | The [speaking flags](conexiones-de-voz.md#speaking-flags) |
| ssrc                  | integer   | The SSRC of the speaking user                             |
| user\_id <sup>2</sup> | snowflake | The user ID of the speaking user                          |
| delay? <sup>3</sup>   | integer   | The speaking packet delay                                 |

<sup>1</sup> For Gateway v3 and below, this field is a boolean.

<sup>2</sup> Only sent by the voice server.

<sup>3</sup> Not sent by the voice server.

[**Speaking Flags**](conexiones-de-voz.md#speaking-flags)

| Value  | Name       | Description                                                    |
| ------ | ---------- | -------------------------------------------------------------- |
| 1 << 0 | VOICE      | Normal transmission of voice audio                             |
| 1 << 1 | SOUNDSHARE | Transmission of context audio for video, no speaking indicator |
| 1 << 2 | PRIORITY   | Priority speaker, lowering audio of other speakers             |

[**Example Speaking (Send)**](conexiones-de-voz.md#example-speaking-\(send\))

```
{  "op": 5,  "d": {    "speaking": 5,    "delay": 0,    "ssrc": 1  }}
```

When a different user's speaking state is updated, and for each user with a speaking state at connection start, the voice server will send an [Opcode 5 Speaking](conexiones-de-voz.md#gateway-events) payload:

[**Example Speaking (Receive)**](conexiones-de-voz.md#example-speaking-\(receive\))

```
{  "op": 5,  "d": {    "speaking": 5,    "ssrc": 2,    "user_id": "852892297661906993"  }}
```

### [Video](conexiones-de-voz.md#video) <a href="#video" id="video"></a>

To notify the voice server that you are sending video, send an [Opcode 12 Video](conexiones-de-voz.md#gateway-commands) payload:

[**Video Structure**](conexiones-de-voz.md#video-structure)

| Field                  | Type                                                           | Description                                                                    |
| ---------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| audio\_ssrc            | integer                                                        | The [SSRC](conexiones-de-voz.md#ready-structure) of the audio stream           |
| video\_ssrc            | integer                                                        | The [SSRC](conexiones-de-voz.md#stream-structure) of the video stream          |
| rtx\_ssrc <sup>1</sup> | integer                                                        | The [SSRC](conexiones-de-voz.md#stream-structure) of the retransmission stream |
| streams                | array\[[stream](conexiones-de-voz.md#stream-structure) object] | Simulcast streams to send                                                      |
| user\_id <sup>2</sup>  | snowflake                                                      | The user ID of the video user                                                  |

<sup>1</sup> Not sent by the voice server.

<sup>2</sup> Only sent by the voice server.

[**Example Video (Send)**](conexiones-de-voz.md#example-video-\(send\))

```
{  "op": 12,  "d": {    "audio_ssrc": 13959,    "video_ssrc": 13960,    "rtx_ssrc": 13961,    "streams": [      {        "type": "video",        "rid": "100",        "ssrc": 13960,        "active": true,        "quality": 100,        "rtx_ssrc": 13961,        "max_bitrate": 9000000,        "max_framerate": 60,        "max_resolution": {          "type": "source",          "width": 0,          "height": 0        }      }    ]  }}
```

When a different user's video state is updated, and for each user with a video state at connection start, the voice server will send an [Opcode 12 Video](conexiones-de-voz.md#gateway-events) payload:

[**Example Video (Receive)**](conexiones-de-voz.md#example-video-\(receive\))

```
{  "op": 12,  "d": {    "user_id": "852892297661906993",    "audio_ssrc": 13959,    "video_ssrc": 13960,    "streams": [      {        "ssrc": 13960,        "rtx_ssrc": 13961,        "rid": "100",        "quality": 100,        "max_resolution": {          "width": 0,          "type": "source",          "height": 0        },        "max_framerate": 60,        "active": true      }    ]  }}
```

#### [Voice Data Interpolation](conexiones-de-voz.md#voice-data-interpolation) <a href="#voice-data-interpolation" id="voice-data-interpolation"></a>

When there's a break in the sent data, the packet transmission shouldn't simply stop. Instead, send five frames of silence () before stopping to avoid unintended Opus interpolation with subsequent transmissions.

Likewise, when you receive these five frames of silence, you know that the user has stopped speaking.

### [Resuming Voice Connection](conexiones-de-voz.md#resuming-voice-connection) <a href="#resuming-voice-connection" id="resuming-voice-connection"></a>

When your client detects that its connection has been severed, it should open a new WebSocket connection. Once the new connection has been opened, your client should send an [Opcode 7 Resume](conexiones-de-voz.md#gateway-commands) payload:

[**Resume Structure**](conexiones-de-voz.md#resume-structure)

| Field                    | Type      | Description                                               |
| ------------------------ | --------- | --------------------------------------------------------- |
| server\_id               | snowflake | The ID of the guild or private channel being connected to |
| channel\_id <sup>2</sup> | snowflake | The ID of the channel being connected to                  |
| session\_id              | string    | The session ID of the current session                     |
| token                    | string    | The voice token for the current session                   |
| seq\_ack? <sup>1</sup>   | integer   | The last received sequence number                         |

<sup>1</sup> Only available on Gateway v8 and above.

<sup>2</sup> Only required for Gateway v9 and above.

[**Example Resume**](conexiones-de-voz.md#example-resume)

```
{  "op": 7,  "d": {    "server_id": "41771983423143937",    "session_id": "30f32c5d54ae86130fc4a215c7474263",    "token": "66d29164ee8cd919"  }}
```

If successful, the voice server will respond with an [Opcode 9 Resumed](conexiones-de-voz.md#gateway-commands) to signal that your client is now resumed:

[**Example Resumed**](conexiones-de-voz.md#example-resumed)

```
{  "op": 9,  "d": null}
```

If the resume is unsuccessful—for example, due to an invalid session—the WebSocket connection will close with the appropriate [close code](https://docs.discord.food/topics/opcodes-and-status-codes#voice-close-event-codes). You should then follow the [Connecting](conexiones-de-voz.md#connecting-to-voice) flow to reconnect.

#### [Buffered Resume](conexiones-de-voz.md#buffered-resume) <a href="#buffered-resume" id="buffered-resume"></a>

Since version 8, the Gateway can resend buffered messages that have been lost upon resume. To support this, the Gateway includes a sequence number with all messages that may need to be re-sent.

[**Example Message With Sequence Number**](conexiones-de-voz.md#example-message-with-sequence-number)

```
{  "op": 5,  "d": {    "speaking": 0,    "delay": 0,    "ssrc": 110  },  "seq": 10}
```

A client using Gateway v8 must include the last sequence number they received under the data key as in both the [Opcode 3 Heartbeat](conexiones-de-voz.md#gateway-commands) and [Opcode 7 Resume](conexiones-de-voz.md#gateway-commands) payloads. If no sequence numbered messages have been received, can be omitted or included with a value of -1.

The Gateway uses a fixed bit length sequence number and handles wrapping the sequence number around. Since Gateway messages will always arrive in order, a client only needs to retain the last sequence number they have seen.

If the session is successfully resumed, the Gateway will respond with an [Opcode 9 Resumed](conexiones-de-voz.md#gateway-events) and will re-send any messages that the client did not receive.

The resume may be unsuccessful if the buffer for the session no longer contains a message that has been missed. In this case the session will be closed and you should then follow the [Connecting](conexiones-de-voz.md#connecting-to-voice) flow to reconnect.

### [Connected Clients](conexiones-de-voz.md#connected-clients) <a href="#connected-clients" id="connected-clients"></a>

#### [Client Connections](conexiones-de-voz.md#client-connections) <a href="#client-connections" id="client-connections"></a>

At connection start, and when a client thereafter connects to voice, the voice server will send a series of events, including an [Opcode 11 Client Connect](conexiones-de-voz.md#gateway-events), and [Opcode 18 Client Flags](conexiones-de-voz.md#gateway-events) and [Opcode 20 Client Platform](conexiones-de-voz.md#gateway-events) for every joined user.

These events are meant to inform a new client of all existing clients and their flags/platform, and inform existing clients of a newly-connected client.

[**Client Connect Structure**](conexiones-de-voz.md#client-connect-structure)

| Field     | Type      | Description                         |
| --------- | --------- | ----------------------------------- |
| user\_ids | snowflake | The IDs of the users that connected |

[**Example Client Connect**](conexiones-de-voz.md#example-client-connect)

```
{  "op": 11,  "d": {    "user_ids": ["852892297661906993"]  }}
```

[**Client Flags Structure**](conexiones-de-voz.md#client-flags-structure)

| Field    | Type      | Description                                                |
| -------- | --------- | ---------------------------------------------------------- |
| user\_id | snowflake | The ID of the user that connected                          |
| flags    | ?integer  | The [user's voice flags](conexiones-de-voz.md#voice-flags) |

[**Voice Flags**](conexiones-de-voz.md#voice-flags)

| Value  | Name                      | Description                                                                                  |
| ------ | ------------------------- | -------------------------------------------------------------------------------------------- |
| 1 << 0 | CLIPS\_ENABLED            | User has [clips](https://support.discord.com/hc/en-us/articles/16861982215703-Clips) enabled |
| 1 << 1 | ALLOW\_VOICE\_RECORDING   | User has allowed their voice to be recorded in another user's clips                          |
| 1 << 2 | ALLOW\_ANY\_VIEWER\_CLIPS | User has allowed stream viewers to clip them                                                 |

[**Example Client Flags**](conexiones-de-voz.md#example-client-flags)

```
{  "op": 18,  "d": {    "user_id": "852892297661906993",    "flags": 3  }}
```

[**Client Platform Structure**](conexiones-de-voz.md#client-platform-structure)

| Field    | Type      | Description                                                      |
| -------- | --------- | ---------------------------------------------------------------- |
| user\_id | snowflake | The ID of the user that connected                                |
| platform | ?integer  | The [user's voice platform](conexiones-de-voz.md#voice-platform) |

[**Voice Platform**](conexiones-de-voz.md#voice-platform)

| Value | Name        | Description             |
| ----- | ----------- | ----------------------- |
| 0     | DESKTOP     | Desktop-based client    |
| 1     | MOBILE      | Mobile client           |
| 2     | XBOX        | Xbox integration        |
| 3     | PLAYSTATION | PlayStation integration |

[**Example Client Platform**](conexiones-de-voz.md#example-client-platform)

```
{  "op": 20,  "d": {    "user_id": "852892297661906993",    "platform": 0  }}
```

#### [Client Disconnections](conexiones-de-voz.md#client-disconnections) <a href="#client-disconnections" id="client-disconnections"></a>

When a user disconnects from voice, the voice server will send an [Opcode 13 Client Disconnect](conexiones-de-voz.md#gateway-events):

When received, the SSRC of the user should be discarded.

[**Client Disconnect Structure**](conexiones-de-voz.md#client-disconnect-structure)

| Field    | Type      | Description                          |
| -------- | --------- | ------------------------------------ |
| user\_id | snowflake | The ID of the user that disconnected |

[**Example Client Disconnect**](conexiones-de-voz.md#example-client-disconnect)

```
{  "op": 13,  "d": {    "user_id": "852892297661906993"  }}
```

### [Simulcasting](conexiones-de-voz.md#simulcasting) <a href="#simulcasting" id="simulcasting"></a>

The voice server supports simulcasting, allowing clients to send multiple video streams of different qualities and adjust the quality of the video stream they receive to fit bandwidth constraints. This can be used to lower the quality of a received video stream when the user is not in focus, or to disable the transmission of a voice or video stream entirely when a user is off-screen or a client has muted them.

A media stream specified by a given SSRC can be requested at a quality level between 0 and 100, with 0 disabling it entirely and 100 being the highest quality. Additionally, if the user offers multiple streams for a given media type, the client can request a specific stream by setting its quality level to 100 and the others to 0. A special SSRC value of can be used to request a quality level for all streams.

Clients may request the media quality they want per SSRC by sending an [Opcode 15 Media Sink Wants](conexiones-de-voz.md#gateway-commands) payload with a mapping of SSRCs to quality levels. Clients may also specify a field to indicate the preferred resolution of the video stream for each SSRC, which can be used by the voice server to determine the best quality level to send based on the client's capabilities and preferences.

Likewise, the voice server may send a [Opcode 15 Media Sink Wants](conexiones-de-voz.md#gateway-events) payload to inform the client of the quality levels it should be sending for each SSRC.

[**Example Media Sink Wants**](conexiones-de-voz.md#example-media-sink-wants)

```
{  "op": 15,  "d": {    "8964": 100,    "pixelCounts": {      "8964": 1189844.5769597634    }  }}
```

### [Voice Backend Version](conexiones-de-voz.md#voice-backend-version) <a href="#voice-backend-version" id="voice-backend-version"></a>

For analytics, the client may want to receive information about the voice backend's current version. To do so, send an [Opcode 16 Voice Backend Version](conexiones-de-voz.md#gateway-commands) with an empty payload:

[**Voice Backend Version Structure**](conexiones-de-voz.md#voice-backend-version-structure)

| Field       | Type   | Description                 |
| ----------- | ------ | --------------------------- |
| voice       | string | The voice backend's version |
| rtc\_worker | string | The WebRTC worker's version |

[**Example Voice Backend Version (Send)**](conexiones-de-voz.md#example-voice-backend-version-\(send\))

```
{  "op": 16,  "d": {}}
```

In response, the voice server will send an [Opcode 16 Voice Backend Version](conexiones-de-voz.md#gateway-commands) payload with the versions:

[**Example Voice Backend Version (Receive)**](conexiones-de-voz.md#example-voice-backend-version-\(receive\))

```
{  "op": 16,  "d": {    "voice": "0.9.1",    "rtc_worker": "0.3.35"  }}
```

### [Streams](conexiones-de-voz.md#streams) <a href="#streams" id="streams"></a>

Stream connections operate in a similar fashion to regular voice connections. In fact, on the protocol side, they are identical and use all of the payloads and processes described above. The main differences are within the [Gateway](https://docs.discord.food/topics/gateway) protocol, as streams are started and joined differently to regular voice connections.

#### [Connecting to Streams](conexiones-de-voz.md#connecting-to-streams) <a href="#connecting-to-streams" id="connecting-to-streams"></a>

To start or join a stream, the client must first be connected to the voice instance that the stream is hosted on. Then, send a [Create Stream](https://docs.discord.food/topics/gateway-events#create-stream) or [Watch Stream](https://docs.discord.food/topics/gateway-events#watch-stream) payload to the Gateway.

If our request succeeded, as with voice, you must wait for the Gateway to respond with _two_ events—a [Stream Create](https://docs.discord.food/topics/gateway-events#stream-create) event and a [Stream Server Update](https://docs.discord.food/topics/gateway-events#stream-server-update). You can then use the information provided in these events to establish a connection to the stream server as outlined in [Connecting to Voice](conexiones-de-voz.md#connecting-to-voice). Note that the used when identifying will be provided in the [Stream Create](https://docs.discord.food/topics/gateway-events#stream-create) event.

Note that if joining a stream fails, the Gateway will instead respond with a [Stream Delete](https://docs.discord.food/topics/gateway-events#stream-delete) event which will contain the reason for the failure.

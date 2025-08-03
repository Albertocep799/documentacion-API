---
icon: diamonds-4
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

# Componentes

### [Components](componentes.md#components) <a href="#components" id="components"></a>

This document serves as a comprehensive reference for all available components. It covers three main categories:

* **Layout Components** - For organizing and structuring content (Action Rows, Sections, Containers)
* **Content Components** - For displaying static text, images, and files (Text Display, Media Gallery, Thumbnails)
* **Interactive Components** - For user interactions (Buttons, Select Menus, Text Input)

To use these components, you need to send the [message flag](https://docs.discord.food/resources/message#message-flags), which can be sent on a per-message basis. Once a message has been sent with this flag, it can't be removed from the message. This enables the new components system with the following changes:

* The and fields will no longer work but you'll be able to use [Text Display](componentes.md#text-display) and [Container](componentes.md#container) as replacements
* Attachments won't show by default—they must be exposed through components
* The and fields are disabled
* Messages allow up to 40 total components

### [What is a Component](componentes.md#what-is-a-component) <a href="#what-is-a-component" id="what-is-a-component"></a>

Components allow you to style and structure your messages, modals, and interactions. They are interactive elements that can create rich user experiences in your Discord applications.

Components are a field on the [message object](https://docs.discord.food/resources/message#message-object) and in modals. You can use them when creating messages or responding to an interaction, like an [application command](https://docs.discord.food/interactions/application-commands).

### [Legacy Message Component Behavior](componentes.md#legacy-message-component-behavior) <a href="#legacy-message-component-behavior" id="legacy-message-component-behavior"></a>

Before the introduction of the flag, message components were sent in conjunction with message content. This means that you could send a message using a subset of the available components without setting the flag, and the components would be included in the message content along with and .

Additionally, components of messages preceding components V2 will contain an of .

### [Component Object](componentes.md#component-object) <a href="#component-object" id="component-object"></a>

[**Component Type**](componentes.md#component-type)

| Value | Name                                                                             | Description                                                | Style       | Usage          |
| ----- | -------------------------------------------------------------------------------- | ---------------------------------------------------------- | ----------- | -------------- |
| 1     | [ACTION\_ROW](componentes.md#action-row)                                         | A container for other components                           | Layout      | Message, Modal |
| 2     | [BUTTON](componentes.md#button)                                                  | A button object                                            | Interactive | Message        |
| 3     | [STRING\_SELECT](componentes.md#string-select)                                   | Select menu for picking from defined text options          | Interactive | Message        |
| 4     | [TEXT\_INPUT](componentes.md#text-input)                                         | Text input object                                          | Interactive | Modal          |
| 5     | [USER\_SELECT](componentes.md#user-select)                                       | Select menu for users                                      | Interactive | Message        |
| 6     | [ROLE\_SELECT](componentes.md#role-select)                                       | Select menu for roles                                      | Interactive | Message        |
| 7     | [MENTIONABLE\_SELECT](componentes.md#mentionable-select)                         | Select menu for mentionables (users _and_ roles)           | Interactive | Message        |
| 8     | [CHANNEL\_SELECT](componentes.md#channel-select)                                 | Select menu for channels                                   | Interactive | Message        |
| 9     | [SECTION](componentes.md#section) <sup>1</sup>                                   | Container to display text alongside an accessory component | Layout      | Message        |
| 10    | [TEXT\_DISPLAY](componentes.md#text-display) <sup>1</sup>                        | Markdown text                                              | Content     | Message        |
| 11    | [THUMBNAIL](componentes.md#thumbnail) <sup>1</sup>                               | Small image that can be used as an accessory               | Content     | Message        |
| 12    | [MEDIA\_GALLERY](componentes.md#media-gallery) <sup>1</sup>                      | Display images and other media                             | Content     | Message        |
| 13    | [FILE](componentes.md#file) <sup>1</sup>                                         | Displays an attached file                                  | Content     | Message        |
| 14    | [SEPARATOR](componentes.md#separator) <sup>1</sup>                               | Component to add vertical padding between other components | Layout      | Message        |
| 16    | [CONTENT\_INVENTORY\_ENTRY](componentes.md#content-inventory-entry) <sup>2</sup> | Displays an activity feed entry                            | Content     | Message        |
| 17    | [CONTAINER](componentes.md#container) <sup>1</sup>                               | Container that visually groups a set of components         | Layout      | Message        |

<sup>1</sup> Requires the [message flag](https://docs.discord.food/resources/message#message-flags).

<sup>2</sup> Not usable by bots.

[**Anatomy of a Component**](componentes.md#anatomy-of-a-component)

All components have the following fields:

| Field | Type    | Description                                                 |
| ----- | ------- | ----------------------------------------------------------- |
| type  | integer | The [type](componentes.md#component-type) of the component  |
| id?   | integer | 32 bit integer used as an optional identifier for component |

The field is optional and is used to identify components in the response from an interaction that aren't interactive components. The must be unique within the message and is generated sequentially if left empty. Generation of s won't use another that exists in the message if you have one defined for another component. Sending components with an of is allowed but will be treated as empty and replaced by the API.

[**Custom ID**](componentes.md#custom-id)

Additionally, interactive components like buttons and selects must have a field. The developer defines this field when sending the component payload, and it is returned in the interaction payload sent when a user interacts with the component. For example, if you set on a button, you'll receive an interaction containing when a user clicks that button.

is only available on interactive components and must be unique per component. Multiple components on the same message must not share the same . This field is a string of a maximum of 100 characters and can be used flexibly to maintain state or pass through other important data.

| Field      | Type   | Description                                       |
| ---------- | ------ | ------------------------------------------------- |
| custom\_id | string | Developer-defined identifier (max 100 characters) |

#### [Action Row](componentes.md#action-row) <a href="#action-row" id="action-row"></a>

An Action Row is a top-level layout component used in messages and modals.

Action Rows can contain:

* Up to 5 contextually grouped [buttons](componentes.md#button)
* A single [text input](componentes.md#text-input)
* A single select component ([string select](componentes.md#string-select), [user select](componentes.md#user-select), [role select](componentes.md#role-select), [mentionable select](componentes.md#mentionable-select), or [channel select](componentes.md#channel-select))

[**Action Row Structure**](componentes.md#action-row-structure)

| Field      | Type                                                        | Description                                                                                                     |
| ---------- | ----------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| type       | integer                                                     | Always                                                                                                          |
| id?        | integer                                                     | An optional identifier for the component                                                                        |
| components | array\[[component](componentes.md#component-object) object] | Up to 5 [button](componentes.md#button) components or a single [select](componentes.md#string-select) component |

[**Example**](componentes.md#example)

```
{  "type": 1,  "components": [    {      "type": 2,      "label": "Accept",      "style": 1,      "custom_id": "click_yes"    },    {      "type": 2,      "label": "Learn More",      "style": 5,      "url": "http://watchanimeattheoffice.com/"    },    {      "type": 2,      "label": "Decline",      "style": 4,      "custom_id": "click_no"    }  ]}
```

#### [Button](componentes.md#button) <a href="#button" id="button"></a>

A Button is an interactive component that can only be used in messages. It creates clickable elements that users can interact with, sending an [interaction](https://docs.discord.food/interactions/receiving-and-responding#interaction-object) to your app when clicked.

Buttons must be placed inside an [Action Row](componentes.md#action-row) or a [Section](componentes.md#section)'s field.

[**Button Structure**](componentes.md#button-structure)

| Field      | Type                                                                           | Description                                                                       |
| ---------- | ------------------------------------------------------------------------------ | --------------------------------------------------------------------------------- |
| type       | integer                                                                        | Always                                                                            |
| id?        | integer                                                                        | An optional identifier for the component                                          |
| style      | integer                                                                        | A [button style](componentes.md#button-style)                                     |
| label?     | string                                                                         | Text that appears on the button (max 80 characters)                               |
| emoji?     | partial [emoji](https://docs.discord.food/resources/emoji#emoji-object) object | The emoji that appears on the button                                              |
| custom\_id | string                                                                         | Developer-defined identifier for the button (max 100 characters)                  |
| sku\_id?   | snowflake                                                                      | Identifier for a purchasable SKU, only available when using premium-style buttons |
| url?       | string                                                                         | URL for link-style buttons (max 512 characters)                                   |
| disabled?  | boolean                                                                        | Whether the button is disabled (default false)                                    |

Buttons come in various styles to convey different types of actions. These styles also define what fields are valid for a button.

* Non-link and non-premium buttons **must** have a , and cannot have a or a .
* Link buttons **must** have a , and cannot have a
* Link buttons do not send an [interaction](https://docs.discord.food/interactions/receiving-and-responding#interaction-object) to your app when clicked
* Premium buttons **must** contain a , and cannot have a , , , or .
* Premium buttons do not send an [interaction](https://docs.discord.food/interactions/receiving-and-responding#interaction-object) to your app when clicked

[**Button Style**](componentes.md#button-style)

| Value | Name      | Action                                                         | Required Field |
| ----- | --------- | -------------------------------------------------------------- | -------------- |
| 1     | PRIMARY   | The most important or recommended action in a group of options | `custom_id`    |
| 2     | SECONDARY | Alternative or supporting actions                              | `custom_id`    |
| 3     | SUCCESS   | Positive confirmation or completion actions                    | `custom_id`    |
| 4     | DANGER    | An action with irreversible consequences                       | `custom_id`    |
| 5     | LINK      | Navigates to a URL                                             | `url`          |
| 6     | PREMIUM   | Purchase                                                       | `sku_id`       |

[**Example Button**](componentes.md#example-button)

```
{  "type": 1,  "components": [    {      "type": 2,      "label": "Click me!",      "style": 1,      "custom_id": "clicked_me"    }  ]}
```

[**Button Design Guidelines**](componentes.md#button-design-guidelines)

[**General Button Content**](componentes.md#general-button-content)

* 34 characters max with icon or emoji.
* 38 characters max without icon or emoji.

[**Premium Buttons**](componentes.md#premium-buttons)

Premium buttons will automatically have the following:

* Shop icon
* SKU name
* SKU price

#### [String Select](componentes.md#string-select) <a href="#string-select" id="string-select"></a>

A String Select is an interactive component that allows users to select one or more provided in a message.

String Selects can be configured for both single-select and multi-select behavior. When a user finishes making their choice(s) your app receives an [interaction](https://docs.discord.food/interactions/receiving-and-responding#interaction-structure).

String Selects must be placed inside an [Action Row](componentes.md#action-row) and are only available in messages. An Action Row can contain only one select menu and cannot contain buttons if it has a select menu.

[**String Select Structure**](componentes.md#string-select-structure)

| Field        | Type                                                                   | Description                                                           |
| ------------ | ---------------------------------------------------------------------- | --------------------------------------------------------------------- |
| type         | integer                                                                | Always                                                                |
| id?          | integer                                                                | An optional identifier for the component                              |
| custom\_id   | string                                                                 | Developer-defined identifier for the select menu (max 100 characters) |
| options      | array\[[select option](componentes.md#select-option-structure) object] | Specified choices in a select menu (max 25)                           |
| placeholder? | string                                                                 | Placeholder text if nothing is selected (max 150 characters)          |
| min\_values? | integer                                                                | Minimum number of items that must be chosen (max 25, default 1)       |
| max\_values? | integer                                                                | Maximum number of items that can be chosen (max 25, default 1)        |
| disabled?    | boolean                                                                | Whether the select menu is disabled (default false)                   |

[**Select Option Structure**](componentes.md#select-option-structure)

| Field        | Type                                                                           | Description                                                        |
| ------------ | ------------------------------------------------------------------------------ | ------------------------------------------------------------------ |
| label        | string                                                                         | User-facing name of the option (max 100 characters)                |
| value        | string                                                                         | Developer-defined value of the option (max 100 characters)         |
| description? | string                                                                         | Additional description of the option (max 100 characters)          |
| emoji?       | partial [emoji](https://docs.discord.food/resources/emoji#emoji-object) object | Emoji to show next to the name                                     |
| default?     | boolean                                                                        | Whether to show this option as selected by default (default false) |

[**Example String Select**](componentes.md#example-string-select)

```
{  "type": 3,  "custom_id": "string_select",  "placeholder": "Favorite bug?",  "options": [    {      "label": "Ant",      "value": "ant",      "description": "(best option)",      "emoji": { "name": "🐜" }    },    {      "label": "Butterfly",      "value": "butterfly",      "emoji": { "name": "🦋" }    },    {      "label": "Catarpillar",      "value": "caterpillar",      "emoji": { "name": "🐛" }    }  ]}
```

The object is included in interaction payloads for user, role, mentionable, and channel select menu components. contains a nested object with additional details about the selected options.

| Field     | Type                                                                                                          | Description       |
| --------- | ------------------------------------------------------------------------------------------------------------- | ----------------- |
| channels? | map\[snowflake, partial [channel](https://docs.discord.food/resources/channel#channel-object) object]         | Selected channels |
| roles?    | map\[snowflake, partial [role](https://docs.discord.food/resources/guild#role-object) object]                 | Selected roles    |
| users?    | map\[snowflake, partial [user](https://docs.discord.food/resources/user#user-object) object]                  | Selected users    |
| members?  | map\[snowflake, partial [guild member](https://docs.discord.food/resources/guild#guild-member-object) object] | Selected members  |

```
{  "channels": {    "986678954901234567": {      "id": "986678954901234567",      "name": "general",      "permissions": "4398046511103",      "type": 0    }  }}
```

#### [Text Input](componentes.md#text-input) <a href="#text-input" id="text-input"></a>

Text Input is an interactive component that allows users to enter free-form text responses in modals. It supports both short, single-line inputs and longer, multi-line paragraph inputs.

Text Inputs can only be used within modals and must be placed inside an [Action Row](componentes.md#action-row).

When defining a text input component, you can set attributes to customize the behavior and appearance of it. However, not all attributes will be returned in the interaction payload.

[**Text Input Structure**](componentes.md#text-input-structure)

| Field        | Type    | Description                                                        |
| ------------ | ------- | ------------------------------------------------------------------ |
| type         | integer | Always                                                             |
| id?          | integer | An optional identifier for the component                           |
| custom\_id   | string  | Developer-defined identifier for the input (max 100 characters)    |
| style        | integer | The [text input style](componentes.md#text-input-style)            |
| label        | string  | Label for this component (max 45 characters)                       |
| min\_length? | integer | Minimum input length for a text input (max 4000)                   |
| max\_length? | integer | Maximum input length for a text input (1-4000)                     |
| required?    | boolean | Whether this component is required to be filled (default true)     |
| value?       | string  | Prefilled value for the component (max 4000 characters)            |
| placeholder? | string  | Custom placeholder text if the input is empty (max 100 characters) |

[**Text Input Style**](componentes.md#text-input-style)

| Value | Name      | Description       |
| ----- | --------- | ----------------- |
| 1     | SMALL     | Single-line input |
| 2     | PARAGRAPH | Multi-line input  |

[**Example Text Input**](componentes.md#example-text-input)

```
{  "type": 1,  "components": [    {      "type": 4,      "custom_id": "name",      "label": "Name",      "style": 1,      "min_length": 1,      "max_length": 4000,      "placeholder": "John",      "required": true    }  ]}
```

#### [User Select](componentes.md#user-select) <a href="#user-select" id="user-select"></a>

A User Select is an interactive component that allows users to select one or more users in a message. Options are automatically populated based on the guild's available users.

User Selects can be configured for both single-select and multi-select behavior. When a user finishes making their choice(s) your app receives an [interaction](https://docs.discord.food/interactions/receiving-and-responding#interaction-structure).

User Selects must be placed inside an [Action Row](componentes.md#action-row) and are only available in messages. An Action Row can contain only one select menu and cannot contain buttons if it has a select menu.

[**User Select Structure**](componentes.md#user-select-structure)

| Field            | Type                                                                          | Description                                                           |
| ---------------- | ----------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| type             | integer                                                                       | Always                                                                |
| id?              | integer                                                                       | An optional identifier for the component                              |
| custom\_id       | string                                                                        | Developer-defined identifier for the select menu (max 100 characters) |
| placeholder?     | string                                                                        | Placeholder text if nothing is selected (max 150 characters)          |
| default\_values? | array\[[default value](componentes.md#select-default-value-structure) object] | Default values for auto-populated select menu components (max 25)     |
| min\_values?     | integer                                                                       | Minimum number of items that must be chosen (max 25, default 1)       |
| max\_values?     | integer                                                                       | Maximum number of items that can be chosen (max 25, default 1)        |
| disabled?        | boolean                                                                       | Whether the select menu is disabled (default false)                   |

[**Select Default Value Structure**](componentes.md#select-default-value-structure)

| Field | Type      | Description                          |
| ----- | --------- | ------------------------------------ |
| id    | snowflake | ID of a user, role, or channel       |
| type  | string    | Type of value associated with the ID |

[**Example User Select**](componentes.md#example-user-select)

```
{  "type": 1,  "components": [    {      "type": 5,      "custom_id": "user_select",      "placeholder": "Select a user"    }  ]}
```

#### [Role Select](componentes.md#role-select) <a href="#role-select" id="role-select"></a>

A Role Select is an interactive component that allows users to select one or more roles in a message. Options are automatically populated based on the guild's available roles.

Role Selects can be configured for both single-select and multi-select behavior. When a user finishes making their choice(s) your app receives an [interaction](https://docs.discord.food/interactions/receiving-and-responding#interaction-structure).

Role Selects must be placed inside an [Action Row](componentes.md#action-row) and are only available in messages. An Action Row can contain only one select menu and cannot contain buttons if it has a select menu.

[**Role Select Structure**](componentes.md#role-select-structure)

| Field            | Type                                                                          | Description                                                           |
| ---------------- | ----------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| type             | integer                                                                       | Always                                                                |
| id?              | integer                                                                       | An optional identifier for the component                              |
| custom\_id       | string                                                                        | Developer-defined identifier for the select menu (max 100 characters) |
| placeholder?     | string                                                                        | Placeholder text if nothing is selected (max 150 characters)          |
| default\_values? | array\[[default value](componentes.md#select-default-value-structure) object] | Default values for auto-populated select menu components (max 25)     |
| min\_values?     | integer                                                                       | Minimum number of items that must be chosen (max 25, default 1)       |
| max\_values?     | integer                                                                       | Maximum number of items that can be chosen (max 25, default 1)        |
| disabled?        | boolean                                                                       | Whether the select menu is disabled (default false)                   |

[**Example Role Select**](componentes.md#example-role-select)

```
{  "type": 1,  "components": [    {      "type": 6,      "custom_id": "role_select",      "placeholder": "Which roles?",      "min_values": 1,      "max_values": 3    }  ]}
```

#### [Mentionable Select](componentes.md#mentionable-select) <a href="#mentionable-select" id="mentionable-select"></a>

A Mentionable Select is an interactive component that allows users to select one or more mentionables in a message. Options are automatically populated based on available mentionables in the guild.

Mentionable Selects can be configured for both single-select and multi-select behavior. When a user finishes making their choice(s), your app receives an [interaction](https://docs.discord.food/interactions/receiving-and-responding#interaction-structure).

Mentionable Selects must be placed inside an [Action Row](componentes.md#action-row) and are only available in messages. An Action Row can contain only one select menu and cannot contain buttons if it has a select menu.

[**Mentionable Select Structure**](componentes.md#mentionable-select-structure)

| Field            | Type                                                                          | Description                                                           |
| ---------------- | ----------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| type             | integer                                                                       | Always                                                                |
| id?              | integer                                                                       | An optional identifier for the component                              |
| custom\_id       | string                                                                        | Developer-defined identifier for the select menu (max 100 characters) |
| placeholder?     | string                                                                        | Placeholder text if nothing is selected (max 150 characters)          |
| default\_values? | array\[[default value](componentes.md#select-default-value-structure) object] | Default values for auto-populated select menu components (max 25)     |
| min\_values?     | integer                                                                       | Minimum number of items that must be chosen (max 25, default 1)       |
| max\_values?     | integer                                                                       | Maximum number of items that can be chosen (max 25, default 1)        |
| disabled?        | boolean                                                                       | Whether the select menu is disabled (default false)                   |

[**Example Mentionable Select**](componentes.md#example-mentionable-select)

```
{  "type": 1,  "components": [    {      "type": 7,      "custom_id": "mentionable_select",      "placeholder": "Who?"    }  ]}
```

#### [Channel Select](componentes.md#channel-select) <a href="#channel-select" id="channel-select"></a>

A Channel Select is an interactive component that allows users to select one or more channels in a message. Options are automatically populated based on available channels in the guild and can be filtered by channel types.

Channel Selects can be configured for both single-select and multi-select behavior. When a user finishes making their choice(s) your app receives an [interaction](https://docs.discord.food/interactions/receiving-and-responding#interaction-structure).

Channel Selects must be placed inside an [Action Row](componentes.md#action-row) and are only available in messages. An Action Row can contain only one select menu and cannot contain buttons if it has a select menu.

[**Channel Select Structure**](componentes.md#channel-select-structure)

| Field            | Type                                                                          | Description                                                                                                          |
| ---------------- | ----------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| type             | integer                                                                       | Always                                                                                                               |
| id?              | integer                                                                       | An optional identifier for the component                                                                             |
| custom\_id       | string                                                                        | Developer-defined identifier for the select menu (max 100 characters)                                                |
| channel\_types?  | array\[integer]                                                               | [Channel types](https://docs.discord.food/resources/channel#channel-type) to include in the channel select component |
| placeholder?     | string                                                                        | Placeholder text if nothing is selected (max 150 characters)                                                         |
| default\_values? | array\[[default value](componentes.md#select-default-value-structure) object] | Default values for auto-populated select menu components (max 25)                                                    |
| min\_values?     | integer                                                                       | Minimum number of items that must be chosen (max 25, default 1)                                                      |
| max\_values?     | integer                                                                       | Maximum number of items that can be chosen (max 25, default 1)                                                       |
| disabled?        | boolean                                                                       | Whether the select menu is disabled (default false)                                                                  |

[**Example Channel Select**](componentes.md#example-channel-select)

```
{  "type": 1,  "components": [    {      "type": 8,      "custom_id": "channel_select",      "channel_types": [0],      "placeholder": "Which text channel?"    }  ]}
```

#### [Section](componentes.md#section) <a href="#section" id="section"></a>

A Section is a top-level layout component that allows you to join text contextually with an accessory.

Sections are only available in messages.

[**Section Structure**](componentes.md#section-structure)

| Field                   | Type                                                                                   | Description                              |
| ----------------------- | -------------------------------------------------------------------------------------- | ---------------------------------------- |
| type                    | integer                                                                                | Always                                   |
| id?                     | integer                                                                                | An optional identifier for the component |
| components <sup>1</sup> | array\[[text display component](componentes.md#text-display) object]                   | Text components to display (1-3)         |
| accessory <sup>1</sup>  | [thumbnail](componentes.md#thumbnail) object \| [button](componentes.md#button) object | A thumbnail or a button component        |

<sup>1</sup> May be expanded to include other component types in the future.

[**Example Section**](componentes.md#example-section)

```
{  "type": 9,  "components": [    {      "type": 10,      "content": "# Real Game v7.3"    },    {      "type": 10,      "content": "Hope you're excited, the update is finally here! Here are some of the changes:\n- Fixed a bug where certain treasure chests wouldn't open properly\n- Improved server stability during peak hours\n- Added a new type of gravity that will randomly apply when the moon is visible in-game\n- Every third thursday the furniture will scream your darkest secrets to nearby npcs"    },    {      "type": 10,      "content": "-# That last one wasn't real, but don't use voice chat near furniture just in case..."    }  ],  "accessory": {    "type": 11,    "media": {      "url": "https://websitewithopensourceimages/gamepreview.png"    }  }}
```

#### [Text Display](componentes.md#text-display) <a href="#text-display" id="text-display"></a>

A Text Display is a top-level content component that allows you to add text to your message formatted with markdown and mention users and roles. This is similar to the field of a message, but allows you to add multiple text components, controlling the layout of your message.

Text Displays are only available in messages.

[**Text Display Structure**](componentes.md#text-display-structure)

| Field   | Type    | Description                                                          |
| ------- | ------- | -------------------------------------------------------------------- |
| type    | integer | Always                                                               |
| id?     | integer | An optional identifier for the component                             |
| content | string  | Text that will be displayed similar to a message (1-4000 characters) |

#### [Thumbnail](componentes.md#thumbnail) <a href="#thumbnail" id="thumbnail"></a>

A Thumbnail is a content component that is a small image only usable as an accessory in a [section](componentes.md#section). The preview comes from an url or attachment through the [unfurled media item](componentes.md#unfurled-media-item-object) structure.

Thumbnails are only available in messages as an accessory in a [section](componentes.md#section).

[**Thumbnail Structure**](componentes.md#thumbnail-structure)

| Field        | Type                                                                    | Description                                               |
| ------------ | ----------------------------------------------------------------------- | --------------------------------------------------------- |
| type         | integer                                                                 | Always                                                    |
| id?          | integer                                                                 | An optional identifier for the component                  |
| media        | [unfurled media item](componentes.md#unfurled-media-item-object) object | A URL or attachment                                       |
| description? | string                                                                  | Alt text for the media (max 1024 characters)              |
| spoiler?     | boolean                                                                 | Whether the thumbnail should be spoilered (default false) |

#### [Media Gallery](componentes.md#media-gallery) <a href="#media-gallery" id="media-gallery"></a>

A Media Gallery is a top-level content component that allows you to display 1-10 media attachments in an organized gallery format. Each item can have optional descriptions and can be marked as spoilers.

Media Galleries are only available in messages.

[**Media Gallery Structure**](componentes.md#media-gallery-structure)

| Field | Type                                                                             | Description                            |
| ----- | -------------------------------------------------------------------------------- | -------------------------------------- |
| type  | integer                                                                          | Always                                 |
| id?   | integer                                                                          | Optional identifier for component      |
| items | array\[[media gallery item](componentes.md#media-gallery-item-structure) object] | Items to display in the gallery (1-10) |

[**Media Gallery Item Structure**](componentes.md#media-gallery-item-structure)

| Field        | Type                                                                    | Description                                             |
| ------------ | ----------------------------------------------------------------------- | ------------------------------------------------------- |
| media        | [unfurled media item](componentes.md#unfurled-media-item-object) object | A URL or attachment                                     |
| description? | string                                                                  | Alt text for the media (max 1024 characters)            |
| spoiler?     | boolean                                                                 | Whether the media should be a spoilered (default false) |

[**Example**](componentes.md#example)

```
{  "type": 12,  "items": [    {      "media": { "url": "https://livevideofeedconvertedtoimage/webcam1.png" },      "description": "An aerial view looking down on older industrial complex buildings. The main building is white with many windows and pipes running up the walls."    },    {      "media": { "url": "https://livevideofeedconvertedtoimage/webcam2.png" },      "description": "An aerial view of old broken buildings. Nature has begun to take root in the rooftops. A portion of the middle building's roof has collapsed inward. In the distant haze you can make out a far away city."    },    {      "media": { "url": "https://livevideofeedconvertedtoimage/webcam3.png" },      "description": "A street view of a downtown city. Prominently in photo are skyscrapers and a domed building"    }  ]}
```

#### [File](componentes.md#file) <a href="#file" id="file"></a>

A File is a top-level component that allows you to display an uploaded file as an attachment to the message and reference it in the component. Each file component can only display 1 attached file, but you can upload multiple files and add them to different file components within your payload.

Files are only available in messages.

[**File Structure**](componentes.md#file-structure)

| Field             | Type                                                                    | Description                                           |
| ----------------- | ----------------------------------------------------------------------- | ----------------------------------------------------- |
| type              | integer                                                                 | Always                                                |
| id?               | integer                                                                 | An optional identifier for the component              |
| file              | [unfurled media item](componentes.md#unfurled-media-item-object) object | The file attachment (does not support URLs)           |
| spoiler?          | boolean                                                                 | Whether the media should be a spoiler (default false) |
| name <sup>1</sup> | string                                                                  | The name of the file                                  |
| size <sup>1</sup> | integer                                                                 | The size of the file in bytes                         |

<sup>1</sup> This field is received only and cannot be set.

[**Example**](componentes.md#example)

```
{  "type": 13,  "file": {    "url": "attachment://game.zip"  }}
```

#### [Separator](componentes.md#separator) <a href="#separator" id="separator"></a>

A Separator is a top-level layout component that adds vertical padding and visual division between other components.

Separators are only available in messages.

[**Separator Structure**](componentes.md#separator-structure)

| Field    | Type    | Description                                                                          |
| -------- | ------- | ------------------------------------------------------------------------------------ |
| type     | integer | Always                                                                               |
| id?      | integer | An optional identifier for the component                                             |
| divider? | boolean | Whether a visual divider should be displayed in the component (default true)         |
| spacing? | integer | [Size of separator padding](componentes.md#separator-spacing-type) (default `SMALL`) |

[**Separator Spacing Type**](componentes.md#separator-spacing-type)

| Value | Name  | Description                                         |
| ----- | ----- | --------------------------------------------------- |
| 1     | SMALL | 8px gap between elements, 16px with divider hidden  |
| 2     | LARGE | 16px gap between elements, 32px with divider hidden |

[**Example**](componentes.md#example)

```
{  "type": 14,  "divider": true,  "spacing": 1}
```

#### [Content Inventory Entry](componentes.md#content-inventory-entry) <a href="#content-inventory-entry" id="content-inventory-entry"></a>

A Content Inventory Entry is a top-level component that displays an activity feed entry.

Content Inventory Entries cannot be sent directly.

[**Content Inventory Entry Structure**](componentes.md#content-inventory-entry-structure)

| Field                     | Type                                                                            | Description                              |
| ------------------------- | ------------------------------------------------------------------------------- | ---------------------------------------- |
| type                      | integer                                                                         | Always                                   |
| id?                       | integer                                                                         | An optional identifier for the component |
| content\_inventory\_entry | [content inventory entry](componentes.md#content-inventory-entry-object) object | Content inventory entry data             |

[**Content Inventory Entry Object**](componentes.md#content-inventory-entry-object)

| Field         | Type                                                                                    | Description                                                                                |
| ------------- | --------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| id            | string                                                                                  | The ID of the entry as a snowflake when sent in guild or a string of numbers when DMed     |
| author\_id    | snowflake                                                                               | The ID of the user this entry is for                                                       |
| author\_type  | integer                                                                                 | The [type of author](componentes.md#content-inventory-author-type) that created this entry |
| content\_type | integer                                                                                 | \[The type of content]]\(#content-inventory-content-type)                                  |
| traits        | array\[[content inventory trait](componentes.md#content-inventory-trait-object) object] | Contains info such as streak, marathon, and time                                           |
| extra         | [content inventory metadata](componentes.md#content-inventory-metadata-object) object   | Metadata, such as a game or song                                                           |
| participants? | array\[snowflake]                                                                       | The IDs of all users involved with this entry                                              |
| expires\_at?  | ISO8601 timestamp                                                                       | When this entry expires                                                                    |
| ended\_at?    | ISO8601 timestamp                                                                       | When this entry ended                                                                      |
| started\_at?  | ISO8601 timestamp                                                                       | When this entry started                                                                    |
| original\_id? | snowflake                                                                               | ID of the entry                                                                            |
| guild\_id?    | snowflake                                                                               | Guild ID this entry happened in                                                            |
| channel\_id?  | snowflake                                                                               | Channel ID this entry happened in                                                          |
| session\_id?  | snowflake                                                                               | Session ID of this entry                                                                   |
| signature     | [content inventory signature](componentes.md#content-inventory-signature-object) object | Signature metadata for validation                                                          |

[**Content Inventory Author Type**](componentes.md#content-inventory-author-type)

| Value | Name                      | Description              |
| ----- | ------------------------- | ------------------------ |
| 0     | AUTHOR\_TYPE\_UNSPECIFIED | No author type specified |
| 1     | USER                      | A Discord user           |

[**Content Inventory Content Type**](componentes.md#content-inventory-content-type)

| Value | Name                       | Description                                         |
| ----- | -------------------------- | --------------------------------------------------- |
| 0     | CONTENT\_TYPE\_UNSPECIFIED | No content type specified                           |
| 1     | PLAYED\_GAME               | A game that was played                              |
| 2     | WATCHED\_MEDIA             | Media that was watched (e.g. Crunchyroll)           |
| 3     | TOP\_GAME                  | Top played game in the guild                        |
| 4     | LISTENED\_MEDIA            | Media that was listened to (e.g. Spotify)           |
| 5     | LISTENED\_SESSION          | Media listening session (e.g. Spotify Listen Along) |
| 6     | TOP\_ARTIST                | Top listened artist in the guild                    |
| 7     | CUSTOM\_STATUS             | Custom status activity                              |
| 8     | LAUNCHED\_ACTIVITY         | Embedded activity                                   |
| 9     | LEADERBOARD                | Leaderboard entry (e.g. League of Legends)          |

[**Content Inventory Trait Object**](componentes.md#content-inventory-trait-object)

| Field                      | Type              | Description                                                                                   |
| -------------------------- | ----------------- | --------------------------------------------------------------------------------------------- |
| type                       | integer           | The [type of trait](componentes.md#content-inventory-trait-type)                              |
| first\_time?               | boolean           | Shows the "New player" text (only `FIRST_TIME`)                                               |
| duration\_seconds?         | integer           | Total time elapsed during the entry (only `DURATION_SECONDS`)                                 |
| is\_live?                  | boolean           | Whether the entry is still ongoing (only `IS_LIVE`)                                           |
| range?                     | integer           | [Time range](componentes.md#content-inventory-aggregate-range-type) (only `AGGREGATE_RANGE`)  |
| resurrected\_last\_played? | ISO8601 timestamp | When the game for the entry was last played (only `RESURRECTED`)                              |
| marathon?                  | boolean           | Shows the "#h marathon" text (only `MARATHON`)                                                |
| streak\_count\_days?       | integer           | Number of days for the streak text (only `STREAK_DAYS`)                                       |
| trending?                  | integer           | The [trending type](componentes.md#content-inventory-trending-type) (only `TRENDING_CONTENT`) |
| count?                     | integer           | Total count (only `TOP_ITEM_TOTAL_COUNT`, `TOP_PARENT_ITEM_TOTAL_COUNT`, `AGGREGATE_COUNT`)   |

[**Content Inventory Trait Type**](componentes.md#content-inventory-trait-type)

| Value | Name                            | Description                                                                     |
| ----- | ------------------------------- | ------------------------------------------------------------------------------- |
| 0     | TRAIT\_TYPE\_UNSPECIFIED        | No trait type specified                                                         |
| 1     | FIRST\_TIME                     | First time the content was played                                               |
| 2     | DURATION\_SECONDS               | Total duration in seconds                                                       |
| 3     | IS\_LIVE                        | Whether the content is currently live                                           |
| 4     | AGGREGATE\_RANGE                | Time range for the aggregated data                                              |
| 5     | RESURRECTED                     | Whether the user is returning to content previously interacted with (e.g. game) |
| 6     | MARATHON                        | Whether the content is part of a marathon                                       |
| 7     | NEW\_RELEASE                    | Whether the content is a new release                                            |
| 8     | STREAK\_DAYS                    | Number of days in the streak                                                    |
| 9     | TRENDING\_CONTENT               | Whether the content is trending                                                 |
| 10    | TOP\_ITEM\_TOTAL\_COUNT         | Total count of the top item in the guild                                        |
| 11    | TOP\_PARENT\_ITEM\_TOTAL\_COUNT | Total count of the top parent item in the guild                                 |
| 12    | AGGREGATE\_COUNT                | Aggregate count of the content in the guild                                     |

[**Content Inventory Aggregate Range Type**](componentes.md#content-inventory-aggregate-range-type)

| Value | Name                          | Description                  |
| ----- | ----------------------------- | ---------------------------- |
| 0     | AGGREGATE\_RANGE\_UNSPECIFIED | No aggregate range specified |
| 1     | WEEK                          | Last 7 days of data          |

[**Content Inventory Trending Type**](componentes.md#content-inventory-trending-type)

| Value | Name                        | Description                |
| ----- | --------------------------- | -------------------------- |
| 0     | TRENDING\_TYPE\_UNSPECIFIED | No trending type specified |
| 1     | GLOBAL                      | Global trending content    |

[**Content Inventory Metadata Object**](componentes.md#content-inventory-metadata-object)

| Field                        | Type                                                                                    | Description                                                                                                        |
| ---------------------------- | --------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| type                         | string                                                                                  | [Metadata type](componentes.md#content-inventory-metadata-type)                                                    |
| game\_name?                  | string                                                                                  | The name of the game (`played_game_extra` only)                                                                    |
| application\_id?             | snowflake                                                                               | The ID of the associated application                                                                               |
| platform?                    | integer                                                                                 | The [type of platform](componentes.md#content-inventory-platform-type) (only `played_game_extra`)                  |
| last\_update?                | ISO8601 timestamp                                                                       | When the entry was last updated                                                                                    |
| entries?                     | array\[[metadata entry](componentes.md#content-inventory-metadata-entry-object) object] | Entries (only 1 for the component)                                                                                 |
| media?                       | [metadata entry](componentes.md#content-inventory-metadata-entry-object) object         | Metadata object of type `listened_media_extra`                                                                     |
| provider?                    | integer                                                                                 | The [provider](componentes.md#content-inventory-provider-type) of the media (only `listened_media_extra`)          |
| media\_type?                 | integer                                                                                 | The [type of media](componentes.md#content-inventory-media-type) (only `listened_media_extra`)                     |
| parent\_title?               | string                                                                                  | The title of the media's parent (album title when `media_type` is `TRACK`) (only `listened_media_extra`)           |
| title?                       | string                                                                                  | The title of the media (only `listened_media_extra`)                                                               |
| image\_url?                  | string                                                                                  | Image for the media (e.g. album art) (only `listened_media_extra`)                                                 |
| artist?                      | [artist](componentes.md#content-inventory-artist-object) object                         | Top listened artist (only `top_artist_extra`)                                                                      |
| artists?                     | array\[[artist](componentes.md#content-inventory-artist-object) object]                 | Artists for the media (empty when `top_artist_extra`) (only `listened_media_extra`)                                |
| external\_id?                | string                                                                                  | The external platform ID for the media (e.g. Spotify track ID) (only `listened_media_extra`)                       |
| external\_parent\_id?        | string                                                                                  | The external platform ID for the media's parent (e.g. Spotify album ID) (only `listened_media_extra`)              |
| media\_assets\_large\_image? | string                                                                                  | The large image for the media (e.g. movie poster) (only `watched_media_extra`)                                     |
| media\_assets\_large\_text?  | string                                                                                  | Text displayed when hovering over the large image (e.g. season and episode of a show) (only `watched_media_extra`) |
| media\_assets\_small\_image? | string                                                                                  | Small image for the media (e.g. platform logo) (only `watched_media_extra`)                                        |
| media\_assets\_small\_text?  | string                                                                                  | Text displayed when hovering over the small image (e.g. platform name) (only `watched_media_extra`)                |
| media\_title?                | string                                                                                  | The title of the media (only `watched_media_extra`)                                                                |
| media\_subtitle?             | string                                                                                  | The subtitle of the media (only `watched_media_extra`)                                                             |
| url?                         | string                                                                                  | The URL of the media (only `watched_media_extra`)                                                                  |
| activity\_name?              | string                                                                                  | The name of the activity (only `launched_activity_extra`)                                                          |

[**Content Inventory Metadata Type**](componentes.md#content-inventory-metadata-type)

| Value                     | Description                         |
| ------------------------- | ----------------------------------- |
| played\_game\_extra       | Game                                |
| listened\_session\_extra  | Listened session (e.g. Spotify)     |
| listened\_media\_extra    | Listened media (e.g. Spotify track) |
| top\_artist\_extra        | Top artist (e.g. Spotify)           |
| watched\_media\_extra     | Watched media (e.g. Crunchyroll)    |
| launched\_activity\_extra | Embedded activity                   |

[**Content Inventory Provider Type**](componentes.md#content-inventory-provider-type)

| Value | Name                  | Description           |
| ----- | --------------------- | --------------------- |
| 0     | PROVIDER\_UNSPECIFIED | No provider specified |
| 1     | SPOTIFY               | Spotify               |

[**Content Inventory Media Type**](componentes.md#content-inventory-media-type)

| Value | Name        | Description                              |
| ----- | ----------- | ---------------------------------------- |
| 0     | TOP\_ARTIST | Top artist in the guild                  |
| 1     | TRACK       | Track in a media provider (e.g. Spotify) |

[**Content Inventory Metadata Entry Object**](componentes.md#content-inventory-metadata-entry-object)

| Field                | Type                                                                | Description                                         |
| -------------------- | ------------------------------------------------------------------- | --------------------------------------------------- |
| media?               | [metadata](componentes.md#content-inventory-metadata-object) object | Metadata object of type `listened_media_extra`      |
| verification\_state? | integer                                                             |                                                     |
| repeat\_count?       | integer                                                             | How many times this track has been played on repeat |

[**Content Inventory Artist Object**](componentes.md#content-inventory-artist-object)

| Field        | Type   | Description                                                       |
| ------------ | ------ | ----------------------------------------------------------------- |
| external\_id | string | The external platform ID for this artist (e.g. Spotify artist ID) |
| name         | string | The ame of the artist                                             |

[**Content Inventory Platform Type**](componentes.md#content-inventory-platform-type)

| Value | Name        | Description             |
| ----- | ----------- | ----------------------- |
| 0     | DESKTOP     | Desktop                 |
| 1     | XBOX        | Xbox integration        |
| 2     | PLAYSTATION | PlayStation integration |
| 3     | IOS         | iOS                     |
| 4     | ANDROID     | Android                 |
| 5     | NINTENDO    | Nintendo integration    |
| 6     | LINUX       | Linux                   |
| 7     | MACOS       | macOS                   |

[**Content Inventory Signature Object**](componentes.md#content-inventory-signature-object)

| Field     | Type    | Description                     |
| --------- | ------- | ------------------------------- |
| signature | string  | SHA256 hash of the entry        |
| kid       | string  | Key ID for the signature        |
| version   | integer | Signature version (currently 1) |

[**Example**](componentes.md#example)

```
{  "type": 16,  "id": "0",  "content_inventory_entry": {    "author_id": "150745989836308480",    "author_type": 1,    "content_type": 1,    "ended_at": "2025-04-23T02:13:18.123000+00:00",    "extra": {      "application_id": "356879032584896512",      "game_name": "Garry's Mod",      "platform": 0,      "type": "played_game_extra"    },    "id": "1364423860543557746",    "participants": ["150745989836308480"],    "signature": {      "kid": "AtDT4Kx25Wmu5cfllPxAiwZKgPbmLsaeHitpx/duvPY=",      "signature": "580e54d406bc466a936cbd9a1b8f19954997187d56638c84871cc5f3cd9c245b",      "version": 1    },    "started_at": "2025-04-23T02:11:24.123000+00:00",    "traits": [      {        "duration_seconds": 114,        "type": 2      },      {        "streak_count_days": 3,        "type": 8      }    ]  }}
```

#### [Container](componentes.md#container) <a href="#container" id="container"></a>

A Container is a top-level layout component that holds up to 10 components. Containers are visually distinct from surrounding components and have an optional customizable color bar.

Containers are only available in messages.

[**Container Structure**](componentes.md#container-structure)

| Field          | Type                                                        | Description                                                                                                                                                                                                                                                                 |
| -------------- | ----------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type           | integer                                                     | Always                                                                                                                                                                                                                                                                      |
| id?            | integer                                                     | An optional identifier for the component                                                                                                                                                                                                                                    |
| components     | array\[[component](componentes.md#component-object) object] | Components of the type [action row](componentes.md#action-row), [text display](componentes.md#text-display), [section](componentes.md#section), [media gallery](componentes.md#media-gallery), [separator](componentes.md#separator), or [file](componentes.md#file) (1-10) |
| accent\_color? | ?integer                                                    | Color for the accent on the container encoded as an integer representation of a hexadecimal color code                                                                                                                                                                      |
| spoiler?       | boolean                                                     | Whether the container should be spoilered (default false)                                                                                                                                                                                                                   |

[**Example**](componentes.md#example)

```
{  "type": 17,  "accent_color": 703487,  "components": [    {      "type": 10,      "content": "# You have encountered a wild coyote!"    },    {      "type": 12,      "items": [        {          "media": { "url": "https://websitewithopensourceimages/coyote.png" }        }      ]    },    {      "type": 10,      "content": "What would you like to do?"    },    {      "type": 1,      "components": [        {          "type": 2,          "custom_id": "pet_coyote",          "label": "Pet it!",          "style": 1        },        {          "type": 2,          "custom_id": "feed_coyote",          "label": "Attempt to feed it",          "style": 2        },        {          "type": 2,          "custom_id": "run_away",          "label": "Run away!",          "style": 4        }      ]    }  ]}
```

### [Unfurled Media Item Object](componentes.md#unfurled-media-item-object) <a href="#unfurled-media-item-object" id="unfurled-media-item-object"></a>

[**Unfurled Media Item Object Structure**](componentes.md#unfurled-media-item-object-structure)

| Field                                 | Type                                                                                                        | Description                                                                                                   |
| ------------------------------------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| url                                   | string                                                                                                      | The URL of the media, supports arbitrary URLs and `attachment://<filename>` references (max 2048 characters)  |
| proxy\_url? <sup>1</sup>              | string                                                                                                      | The proxied URL of the media item                                                                             |
| height? <sup>1</sup>                  | ?integer                                                                                                    | The height of the media item                                                                                  |
| width? <sup>1</sup>                   | ?integer                                                                                                    | The width of the media item                                                                                   |
| flags? <sup>1</sup>                   | integer                                                                                                     | The [media's attachment flags](https://docs.discord.food/resources/message#attachment-flags)                  |
| content\_type? <sup>1</sup>           | string                                                                                                      | The [media type](https://en.wikipedia.org/wiki/Media_type) of the content                                     |
| content\_scan\_metadata? <sup>1</sup> | [content scan metadata](https://docs.discord.food/resources/message#content-scan-metadata-structure) object | The content scan metadata for the media                                                                       |
| placeholder\_version? <sup>1</sup>    | integer                                                                                                     | The attachment placeholder protocol version (currently 1)                                                     |
| placeholder? <sup>1</sup>             | string                                                                                                      | A low-resolution [thumbhash](https://github.com/evanw/thumbhash) of the media, to display before it is loaded |
| loading\_state? <sup>1</sup>          | integer                                                                                                     | The [loading state](componentes.md#unfurled-media-item-loading-state) of the media item                       |
| attachment\_id? <sup>1</sup>          | snowflake                                                                                                   | The ID of the uploaded attachment, if any                                                                     |

<sup>1</sup> This field is received only and cannot be set.

[**Unfurled Media Item Loading State**](componentes.md#unfurled-media-item-loading-state)

| Value | Name               | Description                             |
| ----- | ------------------ | --------------------------------------- |
| 0     | UNKNOWN            | Loading state is unknown                |
| 1     | LOADING            | Media item is currently loading         |
| 2     | LOADED\_SUCCESS    | Media item has loaded successfully      |
| 3     | LOADED\_NOT\_FOUND | Media item has loaded but was not found |

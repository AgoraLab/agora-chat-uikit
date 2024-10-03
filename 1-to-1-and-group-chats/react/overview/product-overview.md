# Product overview

Agora UIKit for one-to-one chats and group chats is an instant messaging UI component library developed based on 
Agora Chat SDK. It provides various components to implement features such as a conversation list, chat interface, 
contact list and interface, and others. This helps you to quickly build instant messaging applications based 
on the particular business needs.

To access the source code, [click here](https://github.com/easemob/react-native-chat-library).

## UIKit basic project structure

UIKit is a Chat SDK product with additional UI pages and components. It provides common tools such as themes and internationalization.

The project structure is as follows:

```
├── CHANGELOG.md // The update log.
├── CODE_OF_CONDUCT.md // The Code of Conduct.
├── CONTRIBUTING.md // The Contribution Guide.
├── LICENSE // The Open Source Agreement.
├── README.md // The project introduction.
├── README.zh.md // The project introduction.
├── lib // The compiled files and type definitions.
│ ├── commonjs // commonjs
│ ├── module // esm
│ └── typescript // typescript
├── node_modules // The dependencies
├── package.json // The project configuration.
├── src // The source code.
│ ├── assets // The static resources.
│ ├── biz // The business code.
│ ├── chat // The Chat SDK encapsulation, provides basic services.
│ ├── config // The configuration components.
│ ├── config.local.ts // The local configuration.
│ ├── const.tsx // The constants.
│ ├── container // The entry component container.
│ ├── error // The error handling.
│ ├── hook // The custom hook.
│ ├── i18n // The internationalization.
│ ├── index.tsx // The source code entry.
│ ├── services // The service components.
│ ├── theme // The theme components.
│ ├── types.tsx // The type definitions.
│ ├── ui // The basic UI components, provide basic services for business components.
│ ├── utils // The tool functions.
│ └── version.ts // The Chat UIKit SDK version.
├── tsconfig.build.json // The compilation config.
└── tsconfig.json // The compilation config.
```

## Features

UIKit provides the following main features: Themes, internationalization, multimedia processing, contact page, conversation list, conversation details, error handling, and others.

| Component collection name | Description |
| -------------------------- | ------------------------------------ -------------------------------------------------- ---------------------------------- |
| `Container` | The entry component, used at the application entry, sets global configuration and initializes the UI component library. |
| `Theme` | The theme component, composed of `Palette` and `Theme`, can configure the color and style of UI components. |
| `i18n` | The internationalization component that provides the international content for UI components by default. Supports content changes and custom target languages. |
| `biz` | A collection of page-level business components. Includes `ConversationList`, `ContactList`, `GroupList`, `GroupParticipantList`, `ConversationDetail`, and others. |
| `chat` | The message service component for all non-page message processing. |
| `config` | The service configuration component. Contains global settings. |
| `dispatch` | The event dispatch component. This component can communicate with other components. |
| `error` | The error object. Error objects are defined here. |
| `hook` | The custom hook component that serves other components. |

| Page-level component name | Description |
| -------------------- | ------------------------------- -------------------------------------------------- ----------------------------------------------- |
| `ConversationList` | The conversation list component, provides display and management of conversation lists. |
| `ContactList` | The contact list component, provides display and management of contact lists. Reused in the contact list, create a conversation, create a group, add group members, share a business card, and forward a message pages. |
| `ConversationDetail` | The message page component that can send and receive messages and load message history in one-to-one and group chats. Reused in the chat, search, thread, and create a thread pages. |
| `GroupList` | The group list component, provides display and management of group lists. |
| `GroupParticipantList` | The group member list component, provides display and management of the group member list. Reused in adding members, deleting members, modifying the group owner, and multi-person audio and video pages. |
| `NewRequests` | The new notification list component, receives and processes friend requests. |
| `CreateGroup` | The create a group component. |
| `ContactInfo` | The contact details component. |
| `GroupInfo` | The group details component. |

The business-related UI controls are mainly included in the following three components:

- `ConversationDetail` provides a view of all chats.

    ![Group chat](../../assets/images/group_chat.png)

- `ConversationList` provides a list of conversations.

    ![Conversation list](../../assets/images/conversation_list.png)

- `ContactList` provides contacts, groups, and their details.

    ![Contacts](../../assets/images/contacts.png)
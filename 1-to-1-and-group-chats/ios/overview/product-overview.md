# Product overview

Agora UIKit for one-to-one and group chats is an instant messaging UI component library developed based on 
Agora Chat SDK. It provides various components to implement features such as a conversation list, chat interface, 
contact list and interface, and others. This helps you to quickly build instant messaging applications, including UI, based 
on the particular business needs.

To access the source code, [click here](https://github.com/AgoraIO-Usecase/AgoraChat-UIKit-ios/blob/SwiftUIKit).

## UIKit basic project structure

```
Classes
├─ Service // The basic service component.
│ ├─ Client // The main user initialization, login, cache, and other usage APIs. // TODO：Involving APIs for main initialization, login, and caching.
│ ├─ Protocol // The business protocol.
│ │ ├─ ConversationService // The conversation protocol, including conversation processing actions.
│ │ ├─ ContactService // The contact protocol, including contact adding and deletion.
│ │ ├─ ChatService // The chat protocol, including message processing actions.
│ │ ├─ UserService // The user login protocol, including user login and socket connection status change.
│ │ ├─ MultiService // The multi-device notification protocol.
│ │ └─ GroupService // The group management implementation protocol, including joining and leaving the group, modifying 
group information, and others.
│ └─ Implement // The implementation component of the corresponding protocol.
│
└─ UI // The basic UI components without business logic.
├─ Resource // The images and localization files.
├─ Component // The UI modules containing specific business logic, i.e., some functional UI components.
│ ├─ Chat // The container for all chat views.
│ ├─ Contact // The container for contacts, groups, and their details.
│ └─ Conversation // The container for the conversation list.
└─ Core
├─ UIKit // The common UIKit components, custom components, and some UI-related tool classes.
├─ Foundation // The logs and audio conversion tool classes.
├─ Theme // The theme-related components, including colors, fonts, skin protocols, and their components.
└─ Extension // The system class extensions.
```

## Features

The business-related UI controls are mainly included in the following modules:

- The chat module provides a container for all chat views.

    ![Group chat](../../assets/images/group_chat.png) 

- The conversation module provides a conversation list container.

    ![Conversation list](../../assets/images/conversation_list.png)

- The contact module provides a container for contacts, groups, and their details.

    ![Contacts](../../assets/images/contacts.png)
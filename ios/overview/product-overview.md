Agora UIKit for one-to-one chats and group chats is an instant messaging UI component library developed based on 
Agora Chat SDK. It provides various components to implement features such as a conversation list, chat interface, 
contact list and interface, and others. This helps you to quickly build instant messaging applications based 
on the particular business needs.

To access the source code, [click here](https://github.com/easemob/chatuikit-ios).

## UIKit basic project structure

```
Classes
├─ Service // Basic service component.
│ ├─ Client // Main user initialization, login, cache, and other usage APIs
│ ├─ Protocol // Business protocol
│ │ ├─ ConversationService // Conversation protocol, including various processing operations on the conversation
│ │ ├─ ContactService // Contact protocol, including contact addition and deletion operations
│ │ ├─ ChatService // Chat protocol, including various processing operations on messages
│ │ ├─ UserService // User login protocol, including user login and socket connection status change
│ │ ├─ MultiService // Multi-device notification protocol
│ │ └─ GroupService // Implement group chat management protocol, including joining and leaving the group, modifying 
group information, and others
│ └─ Implement // Implementation component of the corresponding protocol
│
└─ UI // Basic UI components without business logic
├─ Resource // Images or localization files
├─ Component // UI modules containing specific business logic. Some functional UI components
│ ├─ Chat // Container for all chat views
│ ├─ Contact // Container for contacts, groups, and their details
│ └─ Conversation // Container for the conversation list
└─ Core
├─ UIKit // Common UIKit components, custom components, and some UI-related tool classes
├─ Foundation // Logs and audio conversion tool classes
├─ Theme // Theme-related components, including colors, fonts, skin protocols, and their components
└─ Extension // System class extensions
```

## Functions

The business-related UI controls are mainly included in the following three modules:

- Chat module provides a container for all chat views.
- Conversation module provides a conversation list container.
- Contact module provides a container for contacts, groups, and their details.
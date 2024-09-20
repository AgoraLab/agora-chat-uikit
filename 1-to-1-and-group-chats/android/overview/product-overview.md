Agora UIKit for one-to-one chats and group chats is an instant messaging UI component library developed based on 
Agora Chat SDK. It provides various components to implement features such as a conversation list, chat interface, 
contact list and interface, and others. This helps you to quickly build instant messaging applications based 
on the particular business needs.

To access the source code, [click here](https://github.com/easemob/chatuikit-android).

## UIKit basic project structure

```
└── easeui
    ├── EaseIM // UIKit SDK entrance
    ├── EaseIMConfig // UIKit SDK configuration class
    ├── feature // UIKit function module
    │ ├── chat // Chat function module
    │ │ ├── activities // Activity folder of the chat function module
    │ │ │ └── EaseChatActivity // UIKit built-in chat interface
    │ │ ├── adapter // Adapter folder of the chat function module
    │ │ │ └── EaseMessagesAdapter // Message list adapter for the chat function module
    │ │ ├── reply // Related to the reply function of the chat function module
    │ │ ├── report // Related to the reporting message function of the chat function module
    │ │ ├── viewholders // Message type of the chat function module ViewHolder
    │ │ ├── widgets // Custom View of the chat function module
    │ │ └── EaseChatFragment // Chat Fragment provided in UIKit
    │ ├── conversation // Conversation list function module
    │ │ ├── adapter // Adapter folder of the conversation list function module
    │ │ │ └── EaseConversationListAdapter // Conversation list adapter of the conversation list function module
    │ │ ├── viewholders // Conversation type ViewHolder of the conversation list function module
    │ │ ├── widgets // Custom View of the conversation list function module
    │ │ └── EaseConversationListFragment // Conversation list Fragment provided in UIKit
    │ ├── contact // Contact list function module
    │ │ ├── adapter // Adapter folder of the contact list function module
    │ │ │ └── EaseContactListAdapter // Contact list adapter of the contact list function module
    │ │ ├── viewholders // Contact-related ViewHolder of the contact list function module
    │ │ ├── widgets // Custom View of the contact list function module
    │ │ └── EaseContactsListFragment // Contact list Fragment provided in UIKit
    │ └── group // Group function module
    ├── repository // UIKit SDK data warehouse
    ├── viewmodel // UIKit SDK ViewModel
    ├── provider // UIKit SDK provider
    ├── common // UIKit SDK public class
    ├── interfaces // UIKit SDK interface class
    └── widget // UIKit SDK custom View
```

## Features

The business-related UI controls are mainly included in the following three fragments:

- Chat fragment provides a container for all chat views.
- Conversation fragment provides a conversation list container.
- Contact fragment provides a container for contacts, groups, and their details.
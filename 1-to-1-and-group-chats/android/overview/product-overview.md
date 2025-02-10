# Product overview

Agora UIKit for one-to-one and group chats is an instant messaging UI component library developed based on 
Agora Chat SDK. It provides various components to implement features such as a conversation list, chat UI, and 
contact list. This helps you to quickly build instant messaging applications based 
on the particular business needs.

To access the source code, [click here](https://github.com/AgoraIO-Usecase/AgoraChat-UIKit-android/tree/dev-kotlin).

## Basic project structure of chat_uikit

```
└── uikit
    ├── ChatUIKitClient                                   // UIKit SDK entry
    ├── ChatUIKitConfig                             // UIKit SDK configuration class
    ├── feature                                  // UIKit function module
    │   ├── chat                                   // Chat module
    │   │   ├── activities                            // Activity folder
    │   │   │   └── UIKitChatActivity                  // Chat page built in the UIKit
    │   │   ├── adapter                               // Adapter folder of the chat module
    │   │   │   └── ChatUIKitMessagesAdapter          // Message list adapter
    │   │   ├── controllers                           // Controller of all functions of the chat module
    │   │   ├── pin                                   // Message pinning
    │   │   ├── urlpreview                            // URL preview
    │   │   ├── reply                                 // Message reply
    │   │   ├── report                                // Message reporting
    │   │   ├── chathistory                           // Chat history
    │   │   ├── forward                               // Message forwarding
    │   │   ├── reaction                              // Message reaction
    │   │   ├── search                                // Message search
    │   │   ├── translation                           // Message translation
    │   │   ├── viewholders                           // Message type ViewHolder
    │   │   ├── widgets                               // Custom view of the chat module
    │   │   └── UIKitChatFragment                      // Chat fragment built in the UIKit
    │   ├── conversation                           // Conversation list module
    │   │   ├── adapter                               // Adapter folder
    │   │   │   └── ChatUIKitConversationListAdapter       // Conversation list adapter
    │   │   ├── viewholders                           // Conversation ViewHolder
    │   │   ├── widgets                               // Custom view of the conversation list module
    │   │   └── ChatUIKitConversationListFragment          // Conversation list fragment built in the UIKit
    │   ├── thread                                 // Message thread module
    │   │   ├── adapter                               // Adapter folder
    │   │   │   └── ChatUIKitThreadListAdapter         // Message thread list adapter
    │   │   ├── viewholder                            // Message thread ViewHolder 
    │   │   ├── widgets                               // Custom view of the message thread module
    │   │   └── ChatUIKitThreadActivity               // Thread chat page within the UIKit
    │   ├── contact                               // Contact list module
    │   │   ├── adapter                               // Contact list adapter folder 
    │   │   │   └── ChatUIKitContactListAdapter            // Contact list adapter
    │   │   ├── viewholders                           // Contact ViewHolder
    │   │   ├── widgets                               // Custom view of the contact list module
    │   │   └── ChatUIKitContactsListFragment              // Contact list fragment built in the UIKit
    │   └── group                                 // Group module
    │       ├── fragments                             // Group fragment
    │       ├── adapter                               // Adapter folder 
    │       │   └── ChatUIKitGroupListAdapter                // Group list adapter
    │       ├── viewholders                           // ViewHolder   Message ViewHolder
    │       └── ChatUIKitGroupListActivity                 // Group list UI built in the UIKit
    ├── repository                               // UIKit SDK data repository
    ├── viewmodel                                // UIKit SDK ViewModel
    ├── provider                                 // UIKit SDK Provider
    ├── common                                   // Public class of UIKit SDK
    ├── interfaces                               // API class of UIKit SDK
    └── widget                                   // Custom view of UIKit SDK
```

## Features

The business-related UI controls are mainly included in the following three fragments:

- The chat fragment provides a container for all chat views.

    ![Group chat](../../assets/images/group_chat.png)

- The conversation fragment provides a conversation list container.

    ![Conversation list](../../assets/images/conversation_list.png)

- The contact fragment provides a container for contacts, groups, and their details.

    ![Contacts](../../assets/images/contacts.png)
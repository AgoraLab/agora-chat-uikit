Agora UIKit for one-to-one chats and chat groups is an instant messaging UI component library developed based on 
Agora Chat SDK. It provides various components to implement features such as a conversation list, chat interface, 
contact list and interface, and others. This helps you to quickly build instant messaging applications based 
on the particular business needs.

To access the source code, [click here](https://github.com/easemob/chatuikit-android).

## UIKit basic project structure

```
└── uikit
    ├── ChatIM                                   // UIKit SDK entry
    ├── ChatIMConfig                             // UIKit SDK configuration class
    ├── feature                                  // UIKit function module
    │   ├── chat                                   // Chat module
    │   │   ├── activities                            // Activity folder
    │   │   │   └── AgoraChatActivity                  // Chat page built in the UIKit
    │   │   ├── adapter                               // Adapter folder of the chat module
    │   │   │   └── ChatMessagesAdapter               // Message list adapter
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
    │   │   └── AgoraChatFragment                      // Chat fragment built in the UIKit
    │   ├── conversation                           // Conversation list module
    │   │   ├── adapter                               // Adapter folder
    │   │   │   └── ChatConversationListAdapter       // Conversation list adapter
    │   │   ├── viewholders                           // Conversation ViewHolder
    │   │   ├── widgets                               // Custom view of the conversation list module
    │   │   └── ChatConversationListFragment          // Conversation list fragment built in the UIKit
    │   ├── thread                                 // Message thread module
    │   │   ├── adapter                               // Adapter folder
    │   │   │   └── AgoraChatThreadListAdapter         // Message thread list adapter
    │   │   ├── viewholder                            // Message thread ViewHolder 
    │   │   ├── widgets                               // Custom view of the message thread module
    │   │   └── AgoraChatThreadActivity               // Thread chat page within the UIKit
    │   ├── contact                               // Contact list module
    │   │   ├── adapter                               // Contact list adapter folder 
    │   │   │   └── ChatContactListAdapter            // Contact list adapter
    │   │   ├── viewholders                           // Contact ViewHolder
    │   │   ├── widgets                               // Custom view of the contact list module
    │   │   └── ChatContactsListFragment              // Contact list fragment built in the UIKit
    │   └── group                                 // Group module
    │       ├── fragments                             // Group fragment
    │       ├── adapter                               // Adapter folder 
    │       │   └── ChatGroupListAdapter                // Group list adapter
    │       ├── viewholders                           // ViewHolder Message ViewHolder
    │       └── ChatGroupListActivity                 // Group list UI built in the UIKit
    ├── repository                               // UIKit SDK data repository
    ├── viewmodel                                // UIKit SDK ViewModel
    ├── provider                                 // UIKit SDK Provider
    ├── common                                   // Public class of UIKit SDK
    ├── interfaces                               // API class of UIKit SDK
    └── widget                                   // Custom view of UIKit SDK
```

## Features

The business-related UI controls are mainly included in the following three fragments:

- Chat fragment provides a container for all chat views.
- Conversation fragment provides a conversation list container.
- Contact fragment provides a container for contacts, groups, and their details.
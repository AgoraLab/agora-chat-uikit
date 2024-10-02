# Product overview

Agora UIKit for one-to-one and group chats is an instant messaging UI component library developed based on 
Agora Chat SDK. It provides various components to implement features such as a conversation list, chat interface, 
contact list and interface, and others. This helps you to quickly build instant messaging applications based 
on the particular business needs.

To access the source code, [click here](https://github.com/easemob/chatuikit-android).

## UIKit basic project structure

```
└── easeui
    ├── EaseIM // The UIKit SDK entrance.
    ├── EaseIMConfig // The UIKit SDK configuration class.
    ├── feature // The UIKit function modules.
    │ ├── chat // The chat function module.
    │ │ ├── activities // The Activity folder.
    │ │ │ └── EaseChatActivity // The built-in chat interface.
    │ │ ├── adapter // The adapter folder.
    │ │ │ └── EaseMessagesAdapter // The message list adapter.
    │ │ ├── reply // The components related to the reply feature.
    │ │ ├── report // The components related to the message reporting feature.
    │ │ ├── viewholders // The message type ViewHolders.
    │ │ ├── widgets // The custom views.
    │ │ └── EaseChatFragment // The chat fragment provided in UIKit.
    │ ├── conversation // The conversation list function module.
    │ │ ├── adapter // The adapter folder.
    │ │ │ └── EaseConversationListAdapter // The conversation list adapter.
    │ │ ├── viewholders // The conversation type ViewHolders.
    │ │ ├── widgets // The custom views.
    │ │ └── EaseConversationListFragment // The conversation list fragment.
    │ ├── contact // The contact list function module.
    │ │ ├── adapter // The adapter folder.
    │ │ │ └── EaseContactListAdapter // The contact list adapter.
    │ │ ├── viewholders // The contact-related ViewHolders.
    │ │ ├── widgets // The custom views.
    │ │ └── EaseContactsListFragment // The contact list fragment provided in UIKit.
    │ └── group // The group function module.
    ├── repository // The UIKit SDK data warehouse.
    ├── viewmodel // The UIKit SDK ViewModel.
    ├── provider // The UIKit SDK provider.
    ├── common // The UIKit SDK public class.
    ├── interfaces // The UIKit SDK interface classes.
    └── widget // The UIKit SDK custom view.
```

## Features

The business-related UI controls are mainly included in the following three fragments:

- The chat fragment provides a container for all chat views.

    ![Group chat](../../assets/images/group_chat.png)

- The conversation fragment provides a conversation list container.

    ![Conversation list](../../assets/images/conversation_list.png)

- The contact fragment provides a container for contacts, groups, and their details.

    ![Contacts](../../assets/images/contacts.png)
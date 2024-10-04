# Product overview

Agora UIKit for one-to-one chats and group chats is an instant messaging UI component library developed based on 
Agora Chat SDK. It provides various components to implement features such as a conversation list, chat interface, 
contact list and interface, and others. This helps you to quickly build instant messaging applications based 
on the particular business needs.

To access the source code, [click here](https://github.com/easemob/chatuikit-flutter).

## UIKit basic project structure

```
├── chat_uikit.dart // The main class.
├── chat_uikit_context.dart // The data cache.
├── chat_uikit_localizations.dart // The internationalization.
├── chat_uikit_settings.dart // The general configuration.
├── chat_uikit_time_formatter.dart // The time formatter class.
├── provider
│   └── chat_uikit_provider.dart // The user data provider class.
├── sdk_wrapper // The SDK wrapper class.
│   ├── actions
│   │   ├── chat_actions.dart
│   │   ├── contact_actions.dart
│   │   ├── group_actions.dart
│   │   ├── notification_actions.dart
│   │   └── presence_actions.dart
│   ├── chat_sdk_wrapper.dart
│   ├── chat_sdk_wrapper_action_events.dart
│   ├── observers
│   │   ├── action_event_observer.dart
│   │   ├── chat_observer.dart
│   │   ├── connect_observer.dart
│   │   ├── contact_observer.dart
│   │   ├── group_observer.dart
│   │   ├── message_observer.dart
│   │   ├── multi_observer.dart
│   │   └── presence_observer.dart
│   ├── sdk_wrapper_tools.dart
│   ├── typedef_define.dart
│   └── wrappers
│       ├── chat_wrapper.dart
│       ├── connect_wrapper.dart
│       ├── contact_wrapper.dart
│       ├── group_wrapper.dart
│       ├── message_wrapper.dart
│       ├── multi_wrapper.dart
│       ├── notification_wrapper.dart
│       └── presence_wrapper.dart
├── service // The secondary packaging class for SDK packaging.
│   ├── actions
│   │   ├── chat_uikit_chat_actions.dart
│   │   ├── chat_uikit_contact_actions.dart
│   │   ├── chat_uikit_events_actions.dart
│   │   └── chat_uikit_notification_actions.dart
│   ├── chat_uikit_action_events.dart
│   ├── chat_uikit_service.dart
│   ├── observers
│   │   ├── chat_uikit_contact_observers.dart
│   │   └── chat_uikit_events_observers.dart
│   └── protocols
│       └── chat_uikit_profile.dart
├── tools // The tools.
│   ├── chat_uikit_conversation_helper.dart
│   ├── chat_uikit_file_size_tool.dart
│   ├── chat_uikit_helper.dart
│   ├── chat_uikit_highlight_tool.dart
│   ├── chat_uikit_image_loader.dart
│   ├── chat_uikit_insert_message_tool.dart
│   ├── chat_uikit_message_helper.dart
│   └── chat_uikit_time_tool.dart
├── ui // The UI components.
│   ├── components // The list-related components.
│   │   ├── contact_list_view.dart
│   │   ├── conversation_list_view.dart
│   │   ├── group_list_view.dart
│   │   ├── group_member_list_view.dart
│   │   ├── message_list_view.dart
│   │   └── new_requests_list_view.dart
│   ├── controllers // The controller for list-related components.
│   │   ├── chat_uikit_list_view_controller_base.dart
│   │   ├── contact_list_view_controller.dart
│   │   ├── conversation_list_view_controller.dart
│   │   ├── group_list_view_controller.dart
│   │   ├── group_member_list_view_controller.dart
│   │   ├── message_list_view_controller.dart
│   │   └── new_request_list_view_controller.dart
│   ├── custom // The custom component.
│   │   ├── chat_uikit_emoji_data.dart
│   │   ├── custom_text_editing_controller.dart
│   │   └── message_list_share_user_data.dart
│   ├── models // The required model for UI components.
│   │   ├── alphabetical_item_model.dart
│   │   ├── chat_uikit_list_item_model_base.dart
│   │   ├── contact_item_model.dart
│   │   ├── conversation_model.dart
│   │   ├── group_item_model.dart
│   │   ├── new_request_item_model.dart
│   │   └── quote_mode.dart
│   ├── route // The custom routing directory.
│   │   ├── chat_uikit_route.dart
│   │   ├── chat_uikit_route_names.dart
│   │   └── view_arguments
│   │       ├── change_info_view_arguments.dart
│   │       ├── contact_details_view_arguments.dart
│   │       ├── contacts_view_arguments.dart
│   │       ├── conversations_view_arguments.dart
│   │       ├── create_group_view_arguments.dart
│   │       ├── current_user_info_view_arguments.dart
│   │       ├── group_add_members_view_arguments.dart
│   │       ├── group_change_owner_view_arguments.dart
│   │       ├── group_delete_members_view_arguments.dart
│   │       ├── group_details_view_arguments.dart
│   │       ├── group_members_view_arguments.dart
│   │       ├── group_mention_view_arguments.dart
│   │       ├── groups_view_arguments.dart
│   │       ├── messages_view_arguments.dart
│   │       ├── new_request_details_view_arguments.dart
│   │       ├── new_requests_view_arguments.dart
│   │       ├── report_message_view_arguments.dart
│   │       ├── search_group_members_view_arguments.dart
│   │       ├── search_users_view_arguments.dart
│   │       ├── select_contact_view_arguments.dart
│   │       ├── show_image_view_arguments.dart
│   │       ├── show_video_view_arguments.dart
│   │       └── view_arguments_base.dart
│   ├── views // The view-level components.
│   │   ├── change_info_view.dart // The data modification component.
│   │   ├── contact_details_view.dart // The contact details component.
│   │   ├── contacts_view.dart // The contact list component.
│   │   ├── conversations_view.dart // The conversation list component.
│   │   ├── create_group_view.dart // The group creation component.
│   │   ├── current_user_info_view.dart // The current user details component.
│   │   ├── group_add_members_view.dart // The add a group member component.
│   │   ├── group_change_owner_view.dart // The modify the group owner component.
│   │   ├── group_delete_members_view.dart // The delete a group member component.
│   │   ├── group_details_view.dart // The group details component.
│   │   ├── group_members_view.dart // The group member list component.
│   │   ├── group_mention_view.dart // The group mention component.
│   │   ├── groups_view.dart // The group list component.
│   │   ├── messages_view.dart // The message component.
│   │   ├── new_request_details_view.dart // The friend request details component.
│   │   ├── new_requests_view.dart // The friend request list component.
│   │   ├── report_message_view.dart // The message data reporting component.
│   │   ├── search_group_members_view.dart // The search group member component.
│   │   ├── search_users_view.dart // The search a friend component.
│   │   ├── select_contact_view.dart  // The contact multi-select component.
│   │   ├── show_image_view.dart // The display image component.
│   │   └── show_video_view.dart // The display video component.
│   └── widgets // Widget-level components.
│       ├── chat_uikit_alphabetical_widget.dart // The alphabetical index component.
│       ├── chat_uikit_app_bar.dart // The appBar component.
│       ├── chat_uikit_avatar.dart // The avatar component.
│       ├── chat_uikit_badge.dart // The badge component.
│       ├── chat_uikit_bottom_sheet.dart // The bottom sheet component.
│       ├── chat_uikit_button.dart // The button component.
│       ├── chat_uikit_dialog.dart // The dialog component.
│       ├── chat_uikit_downloads_helper_widget.dart // The download attachment component.
│       ├── chat_uikit_input_bar.dart // The message input component.
│       ├── chat_uikit_input_emoji_bar.dart // The emoji component.
│       ├── chat_uikit_list_view.dart // The list component.
│       ├── chat_uikit_message_sliver.dart // The message list component.
│       ├── chat_uikit_message_status_widget.dart // The message status component.
│       ├── chat_uikit_quote_widget.dart // The message quote component.
│       ├── chat_uikit_record_bar.dart // The recording component.
│       ├── chat_uikit_reply_bar.dart // The message reply component.
│       ├── chat_uikit_search_widget.dart // The search component.
│       ├── chat_uikit_show_image_widget.dart // The display a large image component.
│       ├── chat_uikit_show_video_widget.dart // The display a video component.
│       ├── chat_uikit_water_ripple.dart // The water ripple component.
│       └── list_view_items // The list items.
│           ├── chat_uikit_alphabetical_list_view_item.dart // The first letter item component.
│           ├── chat_uikit_contact_list_view_item.dart // The contact item component.
│           ├── chat_uikit_conversation_list_view_item.dart // The conversation list item component.
│           ├── chat_uikit_details_list_view_item.dart // The item component of the details page list component.
│           ├── chat_uikit_group_list_view_item.dart // The group item list component.
│           ├── chat_uikit_list_view_more_item.dart // The item component used to display the data before or after the list in the list 
                component (for example, the friend application and the group list parts in the contact list)
│           ├── chat_uikit_new_request_list_view_item.dart // The item component of the friend request list component.
│           ├── chat_uikit_search_list_view_item.dart // The item component of the search component.
│           └── message_list_view_items // The message list component items.
│               ├── chat_uikit_message_list_view_alert_item.dart // The alert message item component.
│               ├── chat_uikit_message_list_view_bubble.dart // The message bubble component.
│               ├── chat_uikit_message_list_view_message_item.dart  // The message item component.
│               └── message_widget // The message content component.
│                   ├── chat_uikit_card_message_widget.dart // The card message component.
│                   ├── chat_uikit_file_message_widget.dart // The file message component.
│                   ├── chat_uikit_image_message_widget.dart // The image message component.
│                   ├── chat_uikit_nonsupport_message_widget.dart // The unsupported message component.
│                   ├── chat_uikit_text_message_widget.dart // The text message component.
│                   ├── chat_uikit_video_message_widget.dart // The video message component.
│                   └── chat_uikit_voice_message_widget.dart // The audio message component.
└── universal
    ├── chat_uikit_action_model.dart // The event model.
    └── defines.dart // The internal use key.
```

## Features

The business-related UI controls are mainly included in the following three Views:

- `MessagesView` provides a view of all chats.

    ![Group chat](../../assets/images/group_chat.png)

- `ConversationsView` provides a list of conversations.

    ![Conversation list](../../assets/images/conversation_list.png)

- `ContactsView` provides contacts, groups, and their details.

    ![Contacts](../../assets/images/contacts.png)
Agora UIKit for one-to-one chats and group chats is an instant messaging UI component library developed based on 
Agora Chat SDK. It provides various components to implement features such as a conversation list, chat interface, 
contact list and interface, and others. This helps you to quickly build instant messaging applications based 
on the particular business needs.

To access the source code, [click here](https://github.com/easemob/chatuikit-flutter).

## UIKit basic project structure

```
├── chat_uikit.dart // Main class
├── chat_uikit_context.dart // Data cache
├── chat_uikit_localizations.dart // Internationalization
├── chat_uikit_settings.dart // General configuration
├── chat_uikit_time_formatter.dart // Time formatter class
├── provider
│   └── chat_uikit_provider.dart // User data provider class
├── sdk_wrapper // SDK wrapper class
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
├── service // Secondary packaging class for SDK packaging
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
├── tools // 工具类
│   ├── chat_uikit_conversation_helper.dart
│   ├── chat_uikit_file_size_tool.dart
│   ├── chat_uikit_helper.dart
│   ├── chat_uikit_highlight_tool.dart
│   ├── chat_uikit_image_loader.dart
│   ├── chat_uikit_insert_message_tool.dart
│   ├── chat_uikit_message_helper.dart
│   └── chat_uikit_time_tool.dart
├── ui // ui components
│   ├── components // List-related components
│   │   ├── contact_list_view.dart
│   │   ├── conversation_list_view.dart
│   │   ├── group_list_view.dart
│   │   ├── group_member_list_view.dart
│   │   ├── message_list_view.dart
│   │   └── new_requests_list_view.dart
│   ├── controllers // Controller for list-related components
│   │   ├── chat_uikit_list_view_controller_base.dart
│   │   ├── contact_list_view_controller.dart
│   │   ├── conversation_list_view_controller.dart
│   │   ├── group_list_view_controller.dart
│   │   ├── group_member_list_view_controller.dart
│   │   ├── message_list_view_controller.dart
│   │   └── new_request_list_view_controller.dart
│   ├── custom // Custom component
│   │   ├── chat_uikit_emoji_data.dart
│   │   ├── custom_text_editing_controller.dart
│   │   └── message_list_share_user_data.dart
│   ├── models // Required model for UI components
│   │   ├── alphabetical_item_model.dart
│   │   ├── chat_uikit_list_item_model_base.dart
│   │   ├── contact_item_model.dart
│   │   ├── conversation_model.dart
│   │   ├── group_item_model.dart
│   │   ├── new_request_item_model.dart
│   │   └── quote_mode.dart
│   ├── route // Custom routing directory
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
│   ├── views // View-level components
│   │   ├── change_info_view.dart // Modify data component
│   │   ├── contact_details_view.dart // Contact details component
│   │   ├── contacts_view.dart // Contact list component
│   │   ├── conversations_view.dart // Conversation list component
│   │   ├── create_group_view.dart // Create group component
│   │   ├── current_user_info_view.dart // Current user details component
│   │   ├── group_add_members_view.dart // Add group member component
│   │   ├── group_change_owner_view.dart // Modify group owner component
│   │   ├── group_delete_members_view.dart // Delete group member component
│   │   ├── group_details_view.dart // Group details component
│   │   ├── group_members_view.dart // Group member list component
│   │   ├── group_mention_view.dart // Group mention component
│   │   ├── groups_view.dart // Group list component
│   │   ├── messages_view.dart // Message component
│   │   ├── new_request_details_view.dart // Friend request details component
│   │   ├── new_requests_view.dart // Friend request list component
│   │   ├── report_message_view.dart // Message data reporting component
│   │   ├── search_group_members_view.dart // Search group member component
│   │   ├── search_users_view.dart // Search friend component
│   │   ├── select_contact_view.dart  // Contact multiple selection component
│   │   ├── show_image_view.dart // Display image component
│   │   └── show_video_view.dart // Display video component
│   └── widgets // Widget-level components
│       ├── chat_uikit_alphabetical_widget.dart // Alphabetical index component
│       ├── chat_uikit_app_bar.dart // appBar component
│       ├── chat_uikit_avatar.dart // Avatar component
│       ├── chat_uikit_badge.dart // Badge component
│       ├── chat_uikit_bottom_sheet.dart // Bottom sheet component
│       ├── chat_uikit_button.dart // Button component
│       ├── chat_uikit_dialog.dart // Dialog component
│       ├── chat_uikit_downloads_helper_widget.dart // Download attachment component
│       ├── chat_uikit_input_bar.dart // Message input component
│       ├── chat_uikit_input_emoji_bar.dart // Emoji component
│       ├── chat_uikit_list_view.dart // List component
│       ├── chat_uikit_message_sliver.dart // Message list component
│       ├── chat_uikit_message_status_widget.dart // Message status component
│       ├── chat_uikit_quote_widget.dart // Message quote component
│       ├── chat_uikit_record_bar.dart // Recording component
│       ├── chat_uikit_reply_bar.dart // Message reply component
│       ├── chat_uikit_search_widget.dart // Search component
│       ├── chat_uikit_show_image_widget.dart // Display large image component
│       ├── chat_uikit_show_video_widget.dart // Display video component
│       ├── chat_uikit_water_ripple.dart // Water ripple component
│       └── list_view_items // List items
│           ├── chat_uikit_alphabetical_list_view_item.dart // Item component of the first letter component
│           ├── chat_uikit_contact_list_view_item.dart // Item component of the contact component
│           ├── chat_uikit_conversation_list_view_item.dart // Item component of the conversation list component
│           ├── chat_uikit_details_list_view_item.dart // Item component of the details page list component
│           ├── chat_uikit_group_list_view_item.dart // Item component of the group list component
│           ├── chat_uikit_list_view_more_item.dart // Item component used to display the data before or after the list in the list 
                component (for example, the friend application and group list part in the contact list)
│           ├── chat_uikit_new_request_list_view_item.dart // Item component of the friend request list component
│           ├── chat_uikit_search_list_view_item.dart // Item component of the search component
│           └── message_list_view_items // Message list component items
│               ├── chat_uikit_message_list_view_alert_item.dart // Alert message item component
│               ├── chat_uikit_message_list_view_bubble.dart // Message bubble component
│               ├── chat_uikit_message_list_view_message_item.dart  // Message item component
│               └── message_widget // Message content component
│                   ├── chat_uikit_card_message_widget.dart // Card message component
│                   ├── chat_uikit_file_message_widget.dart // File message component
│                   ├── chat_uikit_image_message_widget.dart // Image message component
│                   ├── chat_uikit_nonsupport_message_widget.dart // Unsupported message component 
│                   ├── chat_uikit_text_message_widget.dart // Text message component
│                   ├── chat_uikit_video_message_widget.dart // Video message component
│                   └── chat_uikit_voice_message_widget.dart // Audio message component
└── universal
    ├── chat_uikit_action_model.dart // Event model
    └── defines.dart // UIKit internal use key
```

## Functions

The business-related UI controls are mainly included in the following three Views:

- `MessagesView` provides a view of all chats.
- `ConversationsView` provides a list of conversations.
- `ContactsView` provides contacts, groups, and their details.
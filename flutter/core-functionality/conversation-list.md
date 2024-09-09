The conversation list `ConversationsView` is the main component provided by `ChatUIKit`, which is used to display all the conversations of the current user, including one-to-one chat and chat group. It provides conversation search, deletion, pinning, and do not disturb features. A single conversation displays the user's conversation list, including the conversation name, the last message, the time of the last message, and the pinning and mute status.

It can be used directly or through (routing)[../advanced-features-and-customizations/advanced-usage.md] `ConversationsView`.

For one-to-one chat and chat group, the name displayed in the conversation is the nickname set in the profile. If the nickname is not obtained, the ID is displayed. The conversation avatar is the avatar set in the profile. If no avatar is set, the default avatar is used.

## Add a conversation list to the page

When adding a conversation list, add `ConversationsView` to the page.

```dart
@override
Widget build(BuildContext context) {
  return const ConversationsView();
}
```

## Customize the conversation list

To customize the conversation list, modify the following parameters:

| Parameter | Description |
|:---:|:---:|
| `final ConversationListViewController? controller` | Conversation list controller. If not passed, the default controller will be used internally. |
| `final ChatUIKitAppBar? appBar` | Customize the conversation list `appBar`. If not set, the default will be used. |
| `final bool enableAppBar` | Whether to enable `appBar`. Enabled by default. If disabled, it will no longer be displayed and the input `appBar` will no longer take effect. |
| `final String? title` | The default `appBar` title display content. If a custom one is passed in `appBar`, this setting will not take effect. |
| `final AppBarMoreActionsBuilder? appBarMoreActionsBuilder` | By default, the callback for clicking the `appBar` more button in the upper right corner will provide a default operation list and return a new operation list. |
| `final void Function(List<ConversationModel> data)? onSearchTap` | The event callback of the conversation list search click. After clicking, all current conversations will be called back. If not set, there will be a default implementation. |
| `final List<Widget>? beforeWidgets` | A widget displayed in front of the conversation list. |
| `final List<Widget>? afterWidgets` | A widget displayed after the conversation list. |
| `final ConversationItemBuilder? listViewItemBuilder` | Conversation list item builder. If you need to rewrite the conversation list, do it here. |
| `final void Function(BuildContext context, ConversationModel info)? onTap` | Callback for clicking on the conversation list. If not implemented, the default page will be routed to the message page. |
| `final ConversationsViewItemLongPressHandler? onLongPressHandler `| Long-press event in the conversation list. A default action list will be provided and a new action list will be returned. |
| `final String? searchBarHideText` | The default text displayed in the search box. |
| `final bool enableSearchBar` | Whether to use search. Used by default. |
| `final Widget? listViewBackground` | The background image to display when the list is empty. |
| `final String? attributes` | The extended parameters that will be passed to the next page. |

## More

- Get the number of unread messages (excluding the conversations set to Do Not Disturb), which is generally used to display the total number of unread messages on the conversation page.

    ```dart
    try {
        // withoutIds: The list of IDs to be excluded, which can be group ID or user ID. If not passed, the total number of unread messages for all non-DND conversations will be obtained.
        // unreadCount: The number of unread messages returned, except for the total number of unread messages in other non-DND conversations with 'withoutIds`.
      int unreadCount = await ChatUIKit.instance.getUnreadMessageCount(
        withoutIds: [
          'userId1',
          'groupId1',
        ],
      );
    } catch (e) {
      // error
    }
    ```

- Get the number of unread messages in a specified conversation (including conversations set to Do Not Disturb). This feature is generally used to display the total number of unread messages in aggregated conversations.

    ```dart
    try {
      // appointIds: Conversation ID.
      // unreadCount: The total number of unread messages for the conversation ID in `appointIds`, including the number of unread messages for conversations with Do Not Disturb settings.
      int unreadCount = await ChatUIKit.instance.getAppointUnreadCount(
        appointIds: [
          'userId1',
          'groupId1',
        ],
      );
    } catch (e) {
      // error
    }
    ```

- Gets the number of conversations with unread messages in the specified conversation, generally used to display the number of conversation with new messages in the aggregated conversation.

```dart
try {
   // appointIds: Specifies the conversation range to be retrieved to see if it contains new messages.
   // hasUnreadMessagesConversationCount: Returns the number of conversations with new messages in the passed `appointIds`. This return includes conversations set to do not disturb.
  int hasUnreadMessagesConversationCount =
      await ChatUIKit.instance.appointNewMessageConversationCount(
    appointIds: [
      'conversationId1',
      'conversationId2',
      'conversationId3',
    ],
  );
} catch (e) {
  // error
}
```
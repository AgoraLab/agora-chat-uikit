# Advanced usage

## Implement custom pages through routing jumps

Internally, the `Navigator.of(context).pushNamed` method is used for redirects, and each available View and `routeName` are provided. When you need to customize the View or intercept the jump page, you can use the routing method to intercept and customize it.

| `routeName` | Corresponding string | Description |
|:---:|:---:|:---:|
| `ChatUIKitRouteNames.changeInfoView` | `/ChangeInfoView` | The edit information page. |
| `ChatUIKitRouteNames.contactDetailsView` | `/ContactDetailsView` | The contact details page. |
| `ChatUIKitRouteNames.contactsView` | `/ContactsView` | The contact list page. |
| `ChatUIKitRouteNames.conversationsView` | `/ConversationsView` | The conversation list page. |
| `ChatUIKitRouteNames.createGroupView` | `/CreateGroupView` | The member selection page when creating a group. |
| `ChatUIKitRouteNames.currentUserInfoView` | `/CurrentUserInfoView` | The current user details page. |
| `ChatUIKitRouteNames.forwardMessageSelectView` | `/forwardMessageSelectView` | The message forwarding selection page. |
| `ChatUIKitRouteNames.forwardMessagesView` | `/forwardMessagesView` | The message forwarding page. |
| `ChatUIKitRouteNames.groupChangeOwnerView` | `/GroupChangeOwnerView` | The modify the group owner page. |
| `ChatUIKitRouteNames.groupDetailsView` | `/GroupDetailsView` | The group details page. |
| `ChatUIKitRouteNames.groupsView` | `/GroupsView` | The group list page. |
| `ChatUIKitRouteNames.groupMembersView` | `/GroupMembersView` | The group member list page. |
| `ChatUIKitRouteNames.groupMentionView` | `/GroupMentionView` | The group and select members page. |
| `ChatUIKitRouteNames.groupDeleteMembersView` | `/GroupDeleteMembersView` | The delete a group member page. |
| `ChatUIKitRouteNames.groupAddMembersView` | `/GroupAddMembersView` | The add group members page. |
| `ChatUIKitRouteNames.messagesView` | `/MessagesView` | The message page. |
| `ChatUIKitRouteNames.newRequestDetailsView` | `/NewRequestDetailsView` | The new friend request details page. |
| `ChatUIKitRouteNames.newRequestsView` | `/NewRequestsView` | The new requests page. |
| `ChatUIKitRouteNames.reportMessageView` | `/ReportMessageView` | The report a message page. |
| `ChatUIKitRouteNames.searchUsersView` | `/SearchUsersView` | The search the contacts page. |
| `ChatUIKitRouteNames.searchGroupMembersView` | `/SearchGroupMembersView` | The search group members page. |
| `ChatUIKitRouteNames.selectContactsView` | `/SelectContactsView` | The select a contact page. |
| `ChatUIKitRouteNames.showImageView` | `/ShowImageView` | The view image page. |
| `ChatUIKitRouteNames.showVideoView` | `/ShowVideoView` | The view video page. |
| `ChatUIKitRouteNames.searchHistoryView` | `/SearchHistoryView` | The search message history page. |
| `ChatUIKitRouteNames.threadMessagesView` | `/ThreadMessagesView` | The message thread page. |
| `ChatUIKitRouteNames.threadMembersView` | `/ThreadMembersView` | The thread members page. |
| `ChatUIKitRouteNames.threadsView` | `/ThreadsView` | The thread list page. |

### Use of routing

When you need to customize page jumps or page styles, you can intercept and customize the route and then pass the customized `RouteSettings` to `ChatUIKitRoute.generateRoute`.

```dart
final ChatUIKitRoute _route = ChatUIKitRoute.instance;
@override
Widget build(BuildContext context) {
  return MaterialApp(
    ...
    onGenerateRoute: (settings) {
      return ChatUIKitRoute().generateRoute(settings) ??
          MaterialPageRoute(
            builder: (context) {
              return ...
            },
          );
    },
  );
}
```

First use `ChatUIKitRoute.generateRoute` to intercept. If it returns `null`, continue to use the default logic in your app.

### Route interception

If you need to intercept the conversation list page to jump to the message page and modify the bubble style, use `settings.name == ChatUIKitRouteNames.messagesView` and reset the intercepted `MessagesViewArguments`.

```dart
@override
Widget build(BuildContext context) {
  return MaterialApp(
    ...
      onGenerateRoute: (settings) {
        RouteSettings newSettings = settings;
        if (newSettings.name == ChatUIKitRouteNames.messagesView) {
          MessagesViewArguments arguments =
              settings.arguments as MessagesViewArguments;
          MessagesViewArguments newArguments = arguments.copyWith(
            bubbleBuilder: (context, child, message) {
              //  Set a new bubble
              return Container(
                padding: const EdgeInsets.all(4),
                color: Colors.red,
                child: child,
              );
            },
          );
          newSettings = RouteSettings(
            name: newSettings.name,
            arguments: newArguments,
          );
        }
        return ChatUIKitRoute().generateRoute(newSettings);
      },
  );
}
```

In addition to `MessagesViewArguments`, each View provides the corresponding `ViewArguments`.

| Type | Corresponding page |
|:---:|:---:|
| `ChangeInfoViewArguments` | The information editing page. |
| `ContactDetailsViewArguments` | The contact details page. |
| `ContactsViewArguments` | The contact list page. |
| `ConversationsViewArguments` | The conversation list page. |
| `CreateGroupViewArguments` | The create a group page. |
| `CurrentUserInfoViewArguments` | The current user details page. |
| `ForwardMessageSelectViewArguments` | The message forwarding selection page. |
| `ForwardMessagesViewArguments` | The message forwarding content page. |
| `GroupAddMembersViewArguments` | The add a group member page. |
| `GroupChangeOwnerViewArguments` | The modify the group owner page. |
| `GroupDeleteMembersViewArguments` | The delete a group member page. |
| `GroupDetailsViewArguments` | The group details page. |
| `GroupMembersViewArguments` | The group member list page. |
| `GroupMentionViewArguments` | The group member mention page. |
| `GroupsViewArguments` | The group list page. |
| `MessagesViewArguments` | The message page. |
| `NewRequestDetailsViewArguments` | The friend request details page. |
| `NewRequestsViewArguments` | The friend request list page. |
| `ReportMessageViewArguments` | The report a message page. |
| `SearchGroupMembersViewArguments` | The search group members page. |
| `SearchHistoryViewArguments` | The search messages page. |
| `SearchViewArguments` | The search users page. |
| `SelectContactViewArguments` | The select a contact page. |
| `ShowImageViewArguments` | The show image page. |
| `ShowVideoViewArguments` | The show video page. |
| `ThreadMembersViewArguments` | The thread members page. |
| `ThreadMessagesViewArguments` | The thread message list page. |
| `ThreadsViewArguments` | The thread list page. |

## Configure the message and conversation time formats

```dart
ChatUIKitTimeFormatter.instance.formatterHandler = (context, type, time) {
  if (type == ChatUIKitTimeType.conversation) { // The time used in the conversation list needs to return the complete time content based on time, such as 'xx month xx day xx:xx'
    return '...';
  } else if (type == ChatUIKitTimeType.message) { // The time format used by the message needs to return the complete time content based on time, such as 'xx month xx day xx:xx'
    return '...';
  }
  return null; // If null is returned, no changes are made.
};
```

## Monitor chat events

Two types of event callbacks are available.

After implementing `ChatSDKEventsObserver`, when a Chat SDK method call starts, it will notify you through the `void onChatSDKEventBegin(ChatSDKEvent event)` callback; when it ends, it will notify you through the `void onChatSDKEventEnd(ChatSDKEvent event, ChatError? error)` method. Errors will be reported through `error`.

After implementing `ChatUIKitEventsObservers`, you will get notified of UIKit-related events though `void onChatUIKitEventsReceived(ChatUIKitEvent event)` callbacks:

```dart
class _ToastPageState extends State<ToastPage> with ChatSDKEventsObserver, ChatUIKitEventsObservers {
  @override
  void initState() {
    super.initState();
    // Register to listen
    ChatUIKit.instance.addObserver(this);
  }

  @override
  void dispose() {
    // Remove listening
    ChatUIKit.instance.removeObserver(this);
    super.dispose();
  }

  // The Chat SDK method call starts
  @override
  void onChatSDKEventBegin(ChatSDKEvent event) {
  }

  // he Chat SDK method call ends
  @override
  void onChatSDKEventEnd(ChatSDKEvent event, ChatError? error) {
  }

  // UIKit events
  @override
  void onChatUIKitEventsReceived(ChatUIKitEvent event) {
  }
}
```

## Other global configuration items

The default configuration items must be configured when the application starts.

### Configure the avatar corner rounding

The default is `CornerRadius.medium`:

```dart
ChatUIKitSettings.avatarRadius = CornerRadius.large;
```

### Configure the search box corner radius

The default is `CornerRadius.small`:

```dart
ChatUIKitSettings.searchBarRadius = CornerRadius.large;
```

### Configure the default avatar

The default is` packages/agora_chat_uikit/assets/images/default_avatar.png`:

```dart
ChatUIKitSettings.avatarPlaceholder = const AssetImage(
  'packages/agora_chat_uikit/assets/images/default_avatar.png',
);
```

### Configure the dialog rounded corners

The default is `ChatUIKitDialogRectangleType.filletCorner`:

```dart
ChatUIKitSettings.dialogRectangleType = ChatUIKitDialogRectangleType.circular;
```

### Configure whether to display avatars in the conversation list

The default is `true`.

```dart
ChatUIKitSettings.showConversationListAvatar = true;
```

### Configure whether to display unread counts in the conversation list

The default is `true`.

```dart
ChatUIKitSettings.showConversationListUnreadCount = true;
```

### Configure the mute icon displayed in the conversation list

The default is `packages/agora_chat_uikit/assets/images/no_disturb.png`:

```dart
ChatUIKitSettings.conversationListMuteImage = const AssetImage(
  'packages/agora_chat_uikit/assets/images/no_disturb.png',
)
```

### Set the order of the long-press message menu

When a user long-presses a message, a menu will pop up. You can modify its default value through `ChatUIKitSettings.msgItemLongPressActions`. The default settings are as follows:

```dart
  static List<MessageLongPressActionType> msgItemLongPressActions = [
    MessageLongPressActionType.reaction,
    MessageLongPressActionType.copy, // Applies to text messages only
    MessageLongPressActionType.reply,
    MessageLongPressActionType.forward,
    MessageLongPressActionType.multiSelect,
    MessageLongPressActionType.translate, // Applies to text messages only
    MessageLongPressActionType.thread, // Applies to text messages only
    MessageLongPressActionType.edit, // Applies to text messages only
    MessageLongPressActionType.report,
    MessageLongPressActionType.recall,
    MessageLongPressActionType.delete,
  ];
```

You can modify the pop-up menu by adjusting the order and content. For example, you can remove the copy option (`MessageLongPressActionType.copy`) in the `ChatUIKitSettings.msgItemLongPressActions` menu. 

### Set whether to open the message thread

Users can create a thread based on a message in a group chat to conduct in-depth discussions and track specific project tasks without affecting other chat content.

The message topic feature is provided as a switch in `ChatUIKitSettings.enableChatThreadMessage`, with a default value of `false`. To enable it, set this parameter to `true`.

The sample code is as follows:

```dart
ChatUIKitSettings.enableMessageThread = true;
```

### Set whether to enable message translation

1. Enable message translation.

   `ChatUIKitSettings` provides a `ChatUIKitSettings.enableMessageTranslation` setting to enable the message translation feature. The default value is `false`. To enable this feature, set this parameter to `true`. The sample code is as follows:

    ```dart
      ChatUIKitSettings.enableMessageTranslation = true;
    ```

1. Set the target language for translation.

    `ChatUIKitSettings` provides a `translateTargetLanguage` property to set the target language:

    ```dart
       ChatUIKitSettings.translateTargetLanguage = 'zh-Hans';
    ```

    If the target language is not set, English is used by default. 

    For more target languages, refer to [Translation Language Support](https://learn.microsoft.com/zh-cn/azure/ai-services/translator/language-support).

### Set whether to enable an emoji reply

Users can reply to messages with emojis to express emotions and attitudes, or to conduct surveys or votes. In UIKit, users can long-press a single message to call the extended menu and select an emoji reply.

`ChatUIKitSettings` provides an `enableMessageReaction` property to enable emoji replies. The default value is `false`. To enable this feature, set it to `true`. The sample code is as follows:

```dart
    ChatUIKitSettings.enableMessageReaction = true;
```

### Set whether to enable message quoting

Users can long-press a message to reply to it, with the original message content quoted in the reply. This feature is enabled by default. To disable, set the `enableMessageReply` property to `false`:

```dart
ChatUIKitSettings.enableMessageReply = false;
```

### Set whether to enable message recall

The sender of a message can recall it within a specified time. This feature is enabled by default. To disable, set the `enableMessageRecall` property to `false`:

```dart
ChatUIKitSettings.enableMessageRecall = false;
```

You can also configure the duration of the recall option display. The default is 120 seconds. Upon expiration, the recall option will no longer be available:

```dart
ChatUIKitSettings.recallExpandTime = 120;
```

### Set whether to enable message editing

Users can edit a message they have already sent. This feature is enabled by default. To disable, set the `enableMessageEdit` property to `false`:

```dart
ChatUIKitSettings.enableMessageEdit = false;
```

### Set whether to enable message reporting

Users can report messages they have sent or received. This feature is enabled by default. To disable, set the `enableMessageReport` property to `false`. 

Set the report content. The report content is a set of key-value pairs (`Map<String, String>`), that is, the label of the reported message and the reason for the report. UIKit provides a way to set the label of reported messages. The corresponding report reason needs to be filled in the internationalization file. The sample code is as follows:

```dart
  /// The label of reported messages can be customized.
  // The reason for reporting needs to be filled in the internationalization file. The key of the reason for reporting in the internationalization file must be consistent with the label of the reported message. Refer to [ChatUIKitLocal.reportTarget1].
  static List<String> reportMessageTags = [
    'tag1',
    'tag2',
    'tag3',
    'tag4',
    'tag5',
    'tag6',
    'tag7',
    'tag8',
    'tag9',
  ];
```

### Set whether to enable multiple message forwarding

Users can combine multiple messages and forward them. This feature is enabled by default. To disable, set the `enableMessageMultiSelect` property to `false`:

```dart
ChatUIKitSettings.enableMessageMultiSelect = false;
```

### Set whether to enable single message forwarding

Users can forward single messages. This feature is enabled by default. To disable, set the `enableMessageForward ` property to `false`:

```dart
ChatUIKitSettings.enableMessageForward = false;
```

### Modify the order of contacts

The default order of contacts is `ABCDEFGHIJKLMNOPQRSTUVWXYZ#`. If you need to modify it, refer to the following code:

```dart
// Move # to the top
ChatUIKitSettings.sortAlphabetical = '#ABCDEFGHIJKLMNOPQRSTUVWXYZ';
```

## Customize AppBar

Customize AppBar through `appBarModel`:

```dart
  /// ChatUIKitAppBarModel constructor
  /// [title] Title
  /// [centerWidget] The middle control has a higher priority than title and subtitle. If centerWidget is set, title and subtitle will not be displayed
  /// [titleTextStyle] Title style
  /// [subtitle] Subtitle
  /// [subTitleTextStyle] Subtitle style
  /// [leadingActions] left control
  /// [leadingActionsBuilder] The left control builder will be called back when a default value exists
  /// [trailingActions] right side controls
  /// [trailingActionsBuilder] The right control builder will be called back when a default value exists
  /// [showBackButton] Whether to display the back button
  /// [onBackButtonPressed] Back button click event, if not set, it will return to the previous page by default
  /// [centerTitle] Whether to display the title in the center
  /// [systemOverlayStyle] Status bar style
  /// [backgroundColor] Status bar style
  /// [bottomLine] Whether to display the bottom dividing line
  /// [bottomLineColor] bottom dividing line color

  ChatUIKitAppBarModel({
    this.title,
    this.centerWidget,
    this.titleTextStyle,
    this.subtitle,
    this.subTitleTextStyle,
    this.leadingActions,
    this.leadingActionsBuilder,
    this.trailingActions,
    this.trailingActionsBuilder,
    this.showBackButton = true,
    this.onBackButtonPressed,
    this.centerTitle = false,
    this.systemOverlayStyle,
    this.backgroundColor,
    this.bottomLine,
    this.bottomLineColor,
  });
```

For example:

```dart
appBarModel: ChatUIKitAppBarModel(
  title: "Chat",
  leadingActions: ["return"].map((e) {
    return ChatUIKitAppBarAction(
      child: Text(
        e,
        style: const TextStyle(fontSize: 18),
      ),
      onTap: (context) {
        Navigator.of(context).pop();
      },
    );
  }).toList(),
  showBackButton: false,
  centerTitle: true,
),
```

## Custom ListItemBuilder

Customize individual `items` of components including lists through `itemBuilder`:

```dart
itemBuilder: (context, model) {
  return ListTile(
    title: Text(model.showName),
    subtitle: const Text('subtitle'),
    onTap: () {
      Navigator.push(
        context,
        MaterialPageRoute
            builder: (context) => MessagesView(profile: model.profile)),
      );
    },
  );
},
```
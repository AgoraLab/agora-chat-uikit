`MessagesView` is the main component provided by UIKit to display messages between users. It can be used directly or through routing.

Currently, the following features are available on the message page:

- Send and receive messages, including text, emoticons, pictures, voice, video, files, and business card messages.
- Copy, quote, recall, delete, edit, resend, and review messages.
- Pull roaming messages from the server.
- Clear local messages.

For details about message-related features, see [Product features](./overview/product-features.md).

## Add a message page

You can directly add a message page to the location you need and pass in the `ChatUIKitProfile` information. `ChatUIKitProfile` is a user information packaging class. See [User information](user-information.md) for details.

At the same time, clicking a conversation in the [conversation list](conversation-list.md) will also bring the user to the message page.

```dart
@override
Widget build(BuildContext context) {
  return MessagesView(
    profile: ChatUIKitProfile.contact(
      id: chatterId,
    ),
  );
}
```

## Customize the message page

If you need to customize the message page, you can modify the following parameters:

| Parameter | Description |
|:---:|:---:|
| `final ChatUIKitProfile profile` | The user information packaging class. Refer to [User information](user-information.md) for details . |
| `final MessageListViewController? controller`| The message list controller. |
| `final ChatUIKitAppBar? appBar` | The custom message page. If `appBar` not set, the default one will be used. |
| `final bool enableAppBar` | Whether to enable `appBar`. It is enabled by default. Will no longer be displayed after disabling, and the `appBar` input will no longer take effect. |
| `final String? title` | The default `appBar` title information. If you use a custom `appBar` or disable it, this parameter will not take effect. |
| `final Widget? inputBar` | A custom input component. If not set, the default `ChatUIKitInputBar` will be used. |
| `final CustomTextEditingController? inputBarTextEditingController` | If you customize the `inputBar` controller, the `inputBar` settings here will not take effect. |
| `final bool showAvatar` | Whether to display the avatar. |
| `final bool showNickname` | Whether to display the nickname. |
| `final MessageItemTapHandler? onItemTap` | The message click event. By default, it will process video, picture, and audio type messages. If you need to intercept the click during customization, return `true`; if not, return `false`. |
| `final MessageItemTapHandler? onItemLongPress` | The message long-press event. By default, a menu will pop up after a long press. When customizing, if you need to intercept the click, return `true`; if not, return `false`. |
| `final MessageItemTapHandler? onDoubleTap` | Message double-click event, not implemented by default. If you need to intercept the click during customization, return `true`; if not, return `false`. |
| `final MessageItemTapHandler? onAvatarTap`| When the avatar click event occurs, the page will jump to the contact details page of the message sender by default. If the sender is not a friend, the page will be redirected to the add friend details page. If you need to intercept the click during customization, return `true`; if you do not want to intercept, return `false`. |
| `final MessageItemTapHandler? onAvatarLongPress` | The avatar long press event, not implemented by default. If you need to intercept the click during customization, return `true`; if you do not want to intercept, return `false`. |
| `final MessageItemTapHandler? onNicknameTap` | The nickname long press event, not implemented by default. If you need to intercept the click during customization, return `true`, if not, return `false`. |
| `final ChatUIKitMessageListViewBubbleStyle bubbleStyle` | The message bubble style. Currently two styles are provided: `ChatUIKitMessageListViewBubbleStyle.arrow` (default) and `ChatUIKitMessageListViewBubbleStyle.noArrow`. |
| `final MessageItemBuilder? itemBuilder` | The custom message item builder. If you need to rewrite the message style (including avatar, nickname, message bubble, message quote, and all other styles), implement it here. |
| `final MessageItemBuilder? alertItemBuilder` | The prompt message of the custom item builder. If you need to rewrite the prompt message style, implement it here. |
| `final FocusNode? focusNode` | The input control focus controller, not recommended to set. If `inputBar` is customized, the setting will not take effect. |
| `final List<ChatUIKitBottomSheetItem>? morePressActions` | More menu button items. If not set, the default menu will be used. This parameter will not take effect after `inputBar` customization. |
| `final MessagesViewMorePressHandler? onMoreActionsItemsHandler` | The callback when clicking the default `inputBar`. You can return a new menu list. If you return null or do not implement it, the content set in `morePressActions` will be used. If not set, the default `morePressActions` will be used. |
| `final List<ChatUIKitBottomSheetItem>? longPressActions` | The message long-press menu. If not set, the default menu will be used. |
| `final MessagesViewItemLongPressHandler? onItemLongPressHandler` | The callback for long-pressing the menu item, which can return a new menu list. If it returns null or is not implemented, the content set in `longPressActions` will be used. If not set, the default `longPressActions` will be used. |
| `final bool? forceLeft` | Whether to flush all messages to the left. |
| `final widget? emojiWidget` | The emoji widget. If not set, the default one will be used. |
| `final MessageItemBuilder? replyBarBuilder` | The custom `replyBar` component, used to temporarily display the message content above the input box when the message is quoted. If not set, the default `ChatUIKitReplyBar` will be used. |
| `final Widget Function(BuildContext context, QuoteModel model)? quoteBuilder` | Customize the style of message references when displayed. If not set, the default style is used. |
| `final bool Function(BuildContext context, Message message)? onErrorTapHandler;` | The callback when clicking the red dot of message sending failure. If not set, it will trigger the message to be re-sent. |
| `final MessageItemBubbleBuilder? bubbleBuilder` | The message bubble. If you need to customize the message bubble, implement it here. If you don't set it, the default `ChatUIKitMessageListViewBubble` will be used. |
| `final MessageBubbleContentBuilder? bubbleContentBuilder` | The message bubble content. If you need to customize the bubble content, implement it here. If you do not set it, the default value will be used. |
| `final String? attributes` | The extended parameters that will be passed to the next page. |
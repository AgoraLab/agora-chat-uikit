# Chat messages

`MessagesView` is the main component provided by UIKit to display messages between users. It can be used directly or through routing.

The following features are available:

- Send and receive messages, including text, emojis, pictures, voice, video, files, business card, and combined messages.
- Copy, quote, reply with emoji, recall, delete, pin, edit, resend, and review messages.
- Pull roaming messages from the server.
- Clear local messages.

For details about message-related features, see [Product features](./flutter/overview/product-features.md).

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

If you need to customize the message page, you can modify the following properties:

| Property | Description |
|:---:|:---:|
| `final ChatUIKitProfile profile` | The user information packaging class. Refer to [User information](user-information.md) for details. |
| `final MessageListViewController? controller`| The message list controller. |
| `final ChatUIKitAppBarModel? appBarModel` | The custom message page. If `appBarModel` is not set, the default one will be used. |
| `final bool enableAppBar` | Whether to enable `appBar`. It is enabled by default. Will no longer be displayed after disabling, and the `appBar` input will no longer take effect. |
| `final String? title` | The default `appBar` title information. If you use a custom `appBar` or disable it, this parameter will not take effect. |
| `final Widget? inputBar` | The custom input component. If not set, the default `ChatUIKitInputBar` will be used. |
| `final CustomTextEditingController? inputBarTextEditingController` | If you customize the `inputBar` controller, the `inputBar` settings here will not take effect. |
| `final bool showAvatar` | Whether to display the avatar. |
| `final bool showNickname` | Whether to display the nickname. |
| `final MessageItemTapHandler? onItemTap` | The message click event. By default, it will process video, image, and voice messages. If you need to intercept the click during customization, return `true`; if not, return `false`. |
| `final MessageItemTapHandler? onItemLongPress` | The message long-press event. By default, a menu will pop up after a long press. When customizing, if you need to intercept the click, return `true`; if not, return `false`. | 
| `final MessageItemTapHandler? onDoubleTap` | The message double-click event, not implemented by default. If you need to intercept the click during customization, return `true`; if not, return `false`. |
| `final MessageItemTapHandler? onAvatarTap`| When the avatar click event occurs, the page will jump to the contact details page of the message sender by default. If the sender is not a friend, the page will be redirected to the friend addition details page. If you need to intercept the click during customization, return `true`; if not, return `false`. |
| `final MessageItemTapHandler? onAvatarLongPress` | The avatar long-press event, not implemented by default. If you need to intercept the click during customization, return `true`; if not, return `false`. |
| `final MessageItemTapHandler? onNicknameTap` | The nickname long-press event, not implemented by default. If you need to intercept the click during customization, return `true`; if not, return `false`. |
| `final ChatUIKitMessageListViewBubbleStyle bubbleStyle` | The message bubble style. Currently two styles are provided: `ChatUIKitMessageListViewBubbleStyle.arrow` (default) and `ChatUIKitMessageListViewBubbleStyle.noArrow`. |  
| `final MessageItemBuilder? itemBuilder` | The builder for message item customization. If you need to rewrite the message style (including avatar, nickname, message bubble, message quote, and all other styles), implement it here. | 
| `final MessageItemBuilder? alertItemBuilder` | The builder for item customization for the prompt message. Yan can rewrite the prompt message styles with the builder. | 
| `final FocusNode? focusNode` | The input control focus controller, not recommended to set. If `inputBar` is customized, this setting will not take effect. |
| `final List<ChatUIKitBottomSheetItem>? morePressActions` | More menu button items. If not set, the default menu will be used. This parameter will not take effect after `inputBar` customization. |
| `final MessagesViewMorePressHandler? onMoreActionsItemsHandler` | The callback when clicking the default `inputBar`. You can return a new menu list. If you return `null` or do not implement it, the content set in `morePressActions` will be used. If not set, the default `morePressActions` will be used. |
| `final List<ChatUIKitBottomSheetItem>? longPressActions` | The message long-press menu. If not set, the default menu will be used. |
| `final MessagesViewItemLongPressPositionHandler? onItemLongPressHandler` | The callback for long-pressing the menu item, which can return a new menu list. If it returns `null` or is not implemented, the content set in `longPressActions` will be used. If not set, the default `longPressActions` will be used. |
| `final bool? forceLeft` | Whether to flush all messages to the left. |
| `final widget? emojiWidget` | The emoji widget. If not set, the default one will be used. |
| `final MessageItemBuilder? replyBarBuilder` | The custom `replyBar` component, used to temporarily display the message content above the input box when the message is quoted. If not set, the default `ChatUIKitReplyBar` will be used. |
| `final Widget Function(BuildContext context, QuoteModel model)? quoteBuilder` | Customize the style of message references when displayed. If not set, the default style is used. |
| `final bool Function(BuildContext context, Message message)? onErrorTapHandler;` | The callback when clicking the red dot of the message sending failure. If not set, it will trigger the message to be re-sent. |
| `final MessageItemBubbleBuilder? bubbleBuilder` | The message bubble. You can customize the message bubble with this parameter. If you don't set it, the default `ChatUIKitMessageListViewBubble` will be used. |
| `final MessageBubbleContentBuilder? bubbleContentBuilder` | The message bubble content. You can customize the message bubble content with this parameter. If you do not set it, the default value will be used. |
| `final String? attributes` | The extended parameters that will be passed to the next page. | 

## Customize AppBar

Set whether to display AppBar through `enableAppBar` and customize it through `appBarModel`:

```dart
MessagesView(
  appBarModel: ChatUIKitAppBarModel(
    title: 'Title',
    subtitle: 'Subtitle',
  ),
);
```

For a detailed description of AppBar customization, see [Advanced usage](advanced-usage.md).

## Message list

The message page can be customized in two ways:

- Customize directly when you need to jump to the `MessagesView` page.
- Customize `RouteSettings` when redirecting via routing. For details, see [Advanced usage](advanced-usage.md).

### Avatar and nickname in the message list

The message list display follows the `ChatUIKitProvider` principle. For setting the avatar and nickname, refer to the [User information](user-information.md). You can configure whether the avatar and nickname are displayed by using `showMessageItemAvatar` and `showMessageItemNickname` respectively.

For more information about avatar rounded corners, default avatar, and other settings, see [Advanced usage](advanced-usage.md).

```dart
MessagesView(
  profile: ChatUIKitProfile.contact(id: 'userId', nickname: 'nickname'),
  showMessageItemAvatar: (model) {
    return true;
  },
  showMessageItemNickname: (model) {
    return true;
  },
  ...
);
```

### Custom message bubbles

Customize the message bubble through `bubbleBuilder`, including the bubble color and corner radius:

```dart
MessagesView(
  profile: ChatUIKitProfile.contact(id: 'userId', nickname: 'nickname'),
  ...
  bubbleBuilder: (context, child, model) {
    // Custom bubbles
    return Container(
      padding: const EdgeInsets.all(8),
      decoration: BoxDecoration(
        color: Colors.white,
        borderRadius: BorderRadius.circular(8),
      ),
      child: child,
    );
  },
);
```

### Custom list component

To customize the message list item, use `itemBuilder`. If `null` is returned, it means that the list item will not be modified and the default implementation will be used:

```dart
MessagesView(
  profile: ChatUIKitProfile.contact(id: 'userId', nickname: 'nickname'),
  ...
  itemBuilder: (context, model) {
    if (model.message.bodyType == MessageType.TXT) {
      return Text(model.message.textContent);
    } else {
      return null;
    }
  },
);
```

### Custom message timestamp

The conversation list items, message list items, and pinned message list items have message times. The default rule is to display the specific time of the day instead of date. If this does not meet your needs, customize it through `ChatUIKitTimeFormatter`:

```dart
ChatUIKitTimeFormatter.instance.formatterHandler = (ctx, type, time) {
  if (type == ChatUIKitTimeType.message) {
    return 'message time';
  }
  return null;
};
```

The message timestamp is determined by the two parameters - `type` and `time` - and the formatted time style is returned based on them.

- `type` is an enumeration type, including conversation, message, and pinned message:

    ```dart
    enum ChatUIKitTimeType { conversation, message, messagePinTime }
    ```

- `time` is a timestamp.
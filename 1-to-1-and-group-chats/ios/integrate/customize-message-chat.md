# Customize the message chat

The UIKit message chat page provides the following features:

- Send and receive messages, including text, emojis, images, voice, video, files, business card messages and combined messages.
- Copy, quote, recall, delete, edit, resend, and report messages.
- Pull roaming messages from the server.
- Clear local messages.

For details about message-related functions, see [Product features](../overview/product-features.md).

You can configure the chat page navigation bar, message list items, input box, jump events, and other elements. See [MessageListController.swift](https://github.com/easemob/easemob-uikit-ios/blob/main/Sources/EaseChatUIKit/Classes/UI/Components/Chat/Controllers/MessageListController.swift) for details.

## Customize the navigation bar

The navigation bars of the conversation list page, chat page, contact list page, group details page, and contact details page use `EaseChatNavigationBar`. If the navigation bar does not meet your needs, you can customize it by overriding the method and passing in the customized navigation class. For details about the title, background color, button image, and avatar, see [Customize the conversation list](customize-conversation-list.md).

## Customize the message list cells

To customize the contents of list items in the message table, that is, cells of various message types, take the following steps:

1. Inherit the corresponding message cell class and register a new custom class.
1. Override the corresponding method.

Each message type cell has an initialization method, a `createContent` and `refresh` methods that can be overridden, and other UI creation methods for various small modules.

If you need to reuse the existing logic and add new logic, override the corresponding method and call it `super.xxx`, for example:

```swift
    override open func refresh(entity: MessageEntity) {
       super.refresh(entity:entity)
       //Continue with your new logic
    }
```

If you need to modify the previous logic, copy the code in the previous `refresh` method and modify it without calling it `super.xxxx`.

| Message cell class | Description | Properties to register and override |
| ------------------------ | ------------------ | -------- -------------------------------------------------- -- |
| `TextMessageCell` | Text message | `ComponentsRegister.shared.ChatTextMessageCell = YourTextMessageCell.self` |
| `ImageMessageCell` | Image message | `ComponentsRegister.shared.ChatImageMessageCell = YourImageMessageCell.self` |
| `AudioMessageCell` | Audio message | `ComponentsRegister.shared.ChatAudioMessageCell = YourAudioMessageCell.self` |
| `VideoMessageCell` | Video message | `ComponentsRegister.shared.ChatVideoMessageCell = YourVideoMessageCell.self` |
| `FileMessageCell` | File message | `ComponentsRegister.shared.ChatFileMessageCell = YourFileMessageCell.self` |
| `ContactCardCell` | Contact card message | `ComponentsRegister.shared.ChatContactMessageCell = YourContactCardCell.self` |
| `LocationMessageCell` | Location message | `ComponentsRegister.shared.ChatLocationCell = YourLocationMessageCell.self` |
| `CombineMessageCell` | Combined messages | `ComponentsRegister.shared.ChatCombineCell = YourCombineMessageCell.self` |
| `AlertMessageCell` | Prompt message | `ComponentsRegister.shared.ChatAlertCell = YourAlertMessageCell.self` |
| `CustomMessageCell` | Custom message | `ComponentsRegister.shared.ChatCustomMessageCell = YourCustomMessageCell.self` |


### Set the style of the message bubble

You can set the style of the message bubble through `Appearance.chat.bubbleStyle`: With an arrow or with multiple rounded corners.

### Set the content in the message bubble

You can set the configurable array of content displayed in the chat page message through `Appearance.chat.contentStyle = [.withReply,.withAvatar,.withNickName,.withDateAndTime]`. By default, the reply message bubble, the sender's avatar, the sender's nickname, and the date and time of the message are displayed.

You can remove unnecessary features, and you can also add emoji replies (`MessageReaction`) and threads (`MessageThread`). 

```swift
//Whether to display the message thread.
if self.messageThread {
    Appearance.chat.contentStyle.append(.withMessageThread)
}
//Whether to display an emoji reply
if self.messageReaction {
    Appearance.chat.contentStyle.append(.withMessageReaction)
}
```

### Set the message text color

Set the message text color of the receiver/sender on the chat page with `Appearance.chat.receiveTextColor = value`/`Appearance.chat.sendTextColor = value`.

### Set the message date

- `Appearance.chat.dateFormatToday = "HH:mm"`: Set the message time format for the current day.
- `Appearance.chat.dateFormatOtherDay = "MM-dd HH:mm"`: Set the message date format for days other than today.

### Set the attachment message

#### Set the UIActionSheet style

Create a `UIActionSheet` style pop-up that appears after long-pressing a message as follows:

```swift 
Appearance.chat.messageAttachmentMenuStyle = .actionSheet
```

#### Set configuration items related to attachment messages

- Set the maximum duration of a voice message recording.

    You can set the maximum duration of an audio message recording in the chat page via `Appearance.chat.audioDuration = value`. The default is 60 seconds.

- Set an animation for when a voice message is played.

    - `Appearance.chat.receiveAudioAnimationImages = value`: Set the animation image for when the recipient's voice message is played on the chat page.
    - `Appearance.chat.sendAudioAnimationImages = value`: Set the animation image for when the sender's voice message is played on the chat page.

- Set rounded corners for image messages.

    You can set the chat page image message corner radius with `Appearance.chat.imageMessageCorner = value`. The default value is 4.

- Set a placeholder for an image or video message.

  - `Appearance.chat.imagePlaceHolder = value`: The image message placeholder.
  - `Appearance.chat.videoPlaceHolder = value`: The video message placeholder.

### Set the message recall time

You can set the time for recalling messages on the chat page through `Appearance.chat.recallExpiredTime = value`. The default value is 120 seconds.

### Set the path of the audio file to play when a new message is received

Set the path of the audio file that plays when the chat page receives a new message through `Appearance.chat.newMessageSoundPath = value`.

### Set message translation

- `Appearance.chat.enableTranslation = value`: Whether to enable the text message feature when long pressing a message. The default value is `false`, which means that the feature is disabled by default. 
- `Appearance.chat.targetLanguage= .English`: The target language for translation. After long-pressing a text message, the **Translation** menu appears. Click **Translate** to set the target language for translation. Before using, set `Appearance.chat.enableTranslation` to `true`.
- `Appearance.chat.receiveTranslationColor = value`: The translation text color of the message receiver.
- `Appearance.chat.sendTranslationColor = value`: The translation text color of the message sender.

### Set the action displayed after long-pressing a message

Set the `ActionSheet` menu items after long-pressing a message on the chat page through `Appearance.chat.messageLongPressedActions = value`. You can inherit `MessageListController` and override the methods in the page to remove or add actions in the menu.

```swift
override func filterMessageActions(message: MessageEntity) -> [ActionSheetItemProtocol] {
        if let ext = message.message.ext,let value = ext[callIdentifier] as? String,value == callValue {
            return [
                ActionSheetItem(title: "barrage_long_press_menu_delete".chat.localize, type: .normal,tag: "Delete",image: UIImage(named: "message_action_delete", in: .chatBundle, with: nil)),
                ActionSheetItem(title: "barrage_long_press_menu_multi_select".chat.localize, type: .normal,tag: "MultiSelect",image: UIImage(named: "message_action_multi_select", in: .chatBundle, with: nil)),
                ActionSheetItem(title: "barrage_long_press_menu_forward".chat.localize, type: .normal,tag: "Forward",image: UIImage(named: "message_action_forward", in: .chatBundle, with: nil))
            ]
        } else {
            return super.filterMessageActions(message: message)
        }
    }
```

Get the click event of an action:

```swift
        Appearance.chat.messageLongPressedActions.first { $0.tag == "xxx" }?.action = { [weak self ] in
           //action handler
        }
```
#### Set the UIActionSheet style popup

Create a `UIActionSheet` style pop-up that appears after long-pressing a message as follows:

```swift 
    Appearance.chat.messageLongPressMenuStyle = .actionSheet
```

### Set the input box of the chat page 

- `Appearance.chat.maxInputHeight = value`: The maximum input height of the input box of the chat page.

- `Appearance.chat.inputPlaceHolder = value`: The default placeholder for input box of the chat page.

- `Appearance.chat.inputBarCorner = value`: The rounded corners of the input box of the chat page.

- `Appearance.chat.inputExtendActions = value`: The `ActionSheet` menu items that appear after clicking the **+** button on the right.
    
Get the click event of an action:

```swift
        Appearance.chat.inputExtendActions.first { $0.tag == "xxx" }?.action = { [weak self ] in
           //action handler
        }
```

![img](/images/uikit/chatuikit/ios/configurationitem/chat/Appearance_chat_input.png)

### Set up the message reporting feature

Use `Appearance.chat.reportSelectionTags` and `Appearance.chat.reportSelectionReasons` to set the report tag array and the corresponding reason array of the message reporting feature, in the key-value format, with one-to-one correspondence between the two.

## Set the jump event

For custom redirect events, check the overridable methods marked as open in the chat page and override them accordingly. The following are the commonly used APIs for overriding:
  
| Method | Description | Overridable (Y/N) |
| --------------------------- | --------------------------- ------ | ---------- |
| `messageBubbleClicked` | The message bubble click. | Yes |
| `messageAvatarClick` | The message avatar click. | Yes |
| `audioDialog` | The input box audio button click. | Yes |
| `attachmentDialog` | The input box attachment click. | Yes |

## Intercept the original component click event

You need to fully implement the business and UI refresh logic after interception. It is recommended that you register new events by inheriting the original ones.

The message list events are as follows:

- `replyClicked`: The quoted message click.

- `bubbleClicked`: The message bubble click.

- `bubbleLongPressed`: The message bubble long-press.

- `avatarClicked`: The avatar click.

- `avatarLongPressed`: The avatar long-press.

## Other settings

All other methods marked as **open** can be overridden to implement a custom business logic. For more configurable items, see [General configurable items](general_configurable_items.md). 

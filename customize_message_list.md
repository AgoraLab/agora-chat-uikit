![Customize Message List](./assets/images/customize-message-list.png)

## 1. Customize the navigation bar UI in the message list page

- In the demo, inherit the `EaseChatNavigationBar` class in `EaseChatUIKit` to create your own page navigation. In this example, it is called `CustomConversationNavigationBar`.

- Override the `createNavigation()` method and return the object you created using `CustomConversationNavigationBar`. The sample code is as follows:

    ``` swift
        override func createNavigationBar() -> EaseChatNavigationBar {
            CustomConversationNavigationBar(showLeftItem: false,rightImages: [UIImage(named: "add", in: .chatBundle, with: nil,hiddenAvatar: false)
        }
    ```

- To customize the look of the button on the right side of the navigation bar, set `rightImages` in the above sample code to return the required image. Note that the order is 0, 1, 2. Control whether to display the avatar on the left side of the navigation bar with the `hiddenAvatar` parameter.

- To customize navigation and listen to the original navigation click event, override the `navigationClick` method in the conversation list page, and then perform the processing according to the corresponding click area. The sample code is as follows:

    ```
        override func navigationClick(type: EaseChatNavigationBarClickEvent, indexPath: IndexPath?) {
            switch type {
            case .back: self.backAction()
            case .avatar: self.avatarAction()
            case .title: self.titleAction()
            case .subtitle: self.subtitleAction()
            case .rightItems: self.rightItemsAction(indexPath: indexPath)
            default:
                break
            }
        }
    ```

- Enable the editing mode of the navigation bar by setting `editMode = true`. This means that both the **back** button and the three buttons on the right side will be hidden, and a **cancel** button will appear on the right side.

- Set `self.navigation.title = "Chats".chat.localize` and `self.navigation.subtitle = "xxx"` to change the navigation title and subtitle, respectively. If present, set the subtitle first to update the corresponding layout position inside.

- Change the navigation avatar with `self.navigation.avatarURL = "https://xxx.xxx.xxx"`.

- Set the navigation background color with `self.navigation.backgroudColor = .red`. The internal components of the navigation can also support this method of modification provided that the theme is not switched. If the theme is switched, it will use the theme's default color.

- To customize the redirect event, check for the methods marked as **open** in the message list page and override them to jump to your own business page. Here are some examples of APIs that can be overridden:

    | Method name            | Usage         | Can be overridden |
    |------------------------|-------------------|-------------------|
    | `messageBubbleClicked` | Message bubble click | Yes               |
    | `messageAvatarClick`   | Message avatar click | Yes               |
    | `audioDialog`          | Input box audio button click | Yes               |
    | `attachmentDialog`     | Input box send attachment message  | Yes               |

## 2. Customize the list items, such as the cell for each message type

Customize the content of the list items in the message table, that is, `TextMessageCell`. Inherit the cell of the message type, register and override some services in the subscript in `EaseChatUIKit`, and then set the following code:

| Cell class name       | Usage                     | Swift code to register the corresponding override property                     |
|-----------------------|---------------------------|--------------------------------------------------------------------------------|
| `TextMessageCell `    | Text type message         | `ComponentsRegister.shared.ChatTextMessageCell = YourTextMessageCell.self`     |
| `ImageMessageCell`    | Picture type message      | `ComponentsRegister.shared.ChatImageMessageCell = YourImageMessageCell.self`   |
| `AudioMessageCell`    | Audio type message        | `ComponentsRegister.shared.ChatAudioMessageCell = YourAudioMessageCell.self`   |
| `VideoMessageCell`    | Video type message        | `ComponentsRegister.shared.ChatVideoMessageCell = YourVideoMessageCell.self`   |
| `FileMessageCell`     | File type message         | `ComponentsRegister.shared.ChatFileMessageCell = YourFileMessageCell.self`     |
| `ContactCardCell`     | Contact card type message | `ComponentsRegister.shared.ChatContactMessageCell = YourContactCardCell.self`  |
| `LocationMessageCell` | Location type message     | `ComponentsRegister.shared.ChatLocationCell = YourLocationMessageCell.self`    |
| `CombineMessageCell`  | Combined type message                 | `ComponentsRegister.shared.ChatCombineCell = YourCombineMessageCell.self`      |
| `AlertMessageCell`    | Prompt type message                   | `ComponentsRegister.shared.ChatAlertCell = YourAlertMessageCell.self`          |
| `CustomMessageCell`   | Custom type message                   | `ComponentsRegister.shared.ChatCustomMessageCell = YourCustomMessageCell.self` |

Then, in your custom class, override the corresponding method. If you need to reuse the existing logic and add new logic on this basis, override the method and call `super.xxx` in it. For example:

``` swift
    override open func refresh(entity: MessageEntity） {
       super.refresh(entity:entity)
       // Continue with your new logic
    }
```

If you need to make changes to the previous logic, copy the code from the previous `refresh` method and make changes without calling `super.xxxx`. Each corresponding message type cell has an initialization method, content `createContent`, and `refresh` method in the bubble that can be overridden, as well as other methods for small modules.

## 3. Other customizable methods in the message list page

All other methods marked as **open** can be overridden to implement a custom business logic.

## 4. Configurable items in the message list module

The `ChatAppearance` class is a configurable container class. For simple configuration items, look at the default values of the properties of the container class.

- `Appearance.chat.bubbleStyle = .withArrow`: This property is the style type of the message bubble. It has two enumeration values. The default is a bubble with sharp corners and the other is a message bubble with polygonal rounded corners.

- `Appearance.chat.contentStyle = [.withReply,.withAvatar,.withNickName,.withDateAndTime]`: This property is the message list item, which is the content displayed in the cell. By default, it displays the reply message bubble, the message sender's avatar, the message sender's nickname, and the date and time of the message. The value-added functions include the expression response to the message (`MessageReaction`) and the opening of a topic discussion group in the group based on the current message (`MessageThread`). Make sure to enable these two features in the Console before adding them.

    ``` swift
        //Whether to show the message topic or not.
        if self.messageThread {
            Appearance.chat.contentStyle.append(.withMessageThread)
        }
        //Whether to show the message reaction or not.
        if self.messageReaction {
            Appearance.chat.contentStyle.append(.withMessageReaction)
        }
    ```

- `Appearance.chat.messageLongPressedActions`: The data source array of the `ActionSheet` that pops up after long pressing a message bubble. Users can filter or add some actions according to their own `MessageListController` inheritance triggered after long pressing a message in the sample code:

  ![Message Menu](/images/customize-message-list-message-menu.png)

    ``` swift
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

    Get an action of the click event, sample code:
    
    ``` swift
        Appearance.chat.messageLongPressedActions.first { $0.tag == "xxx" }?.action = { [weak self ] in 
            //action handler
        }
    ```

- `Appearance.chat.targetLanguage= .Chinese`: After long pressing a text message, a translation menu will appear. After clicking **Translate**, set the target language to be translated to. The prerequisite is to apply for the translation function in Agora Console and set `Appearance.chat.enableTranslation` to `true`.

- `Appearance.chat.reportSelectionTags` & `Appearance.chat.reportSelectionReasons`: The message reporting feature has an array of report tags and a corresponding array of reasons.

- `Appearance.chat.inputExtendActions`: Click on the right side of the message input box to pop up the `ActionSheet` data source array. For usage, refer to `Appearance.chat.messageLongPressedActions` above.

- `Appearance.chat.dateFormatToday = "HH:mm"` & `Appearance.chat.dateFormatOtherDay = "yyyy-MM-dd HH:mm"`: Time format of the message.

- `Appearance.chat.audioDuration`:  Maximum recording duration of an audio message. The default is 60 seconds.

- `Appearance.chat.recallExpiredTime`: The default maximum time limit for withdrawing a message is 2 minutes.

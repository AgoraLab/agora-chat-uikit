# Customize the conversation list

You can configure the head navigation bar and conversation list items on the conversation list page. See [ConversationListController.swift](https://github.com/easemob/easemob-uikit-ios/blob/main/Sources/EaseChatUIKit/Classes/UI/Components/Conversation/Controllers/ConversationListController.swift) for details.

![Customize Conversation List](../../assets/images/conversation_list_highlighted.jpg)

## Customize the header navigation bar 

The navigation bars of the conversation list page, chat page, contact list page, group details page, and contact details page use `EaseChatNavigationBar`. If the navigation bar does not meet your needs, you can customize it by overriding the method and passing in the customized navigation class.

1. In the demo, inherit the `EaseChatNavigationBar` class in `EaseChatUIKit` to create your own page navigation. For example, `CustomConversationNavigationBar`.

1. Override the `createNavigation()` method and return the object you have created using `CustomConversationNavigationBar`. The sample code is as follows:

    ```swift
    override func createNavigationBar() -> EaseChatNavigationBar {
                CustomConversationNavigationBar(showLeftItem: false,rightImages: [UIImage(named: "add", in: .chatBundle, with: nil,hiddenAvatar: false)
            }
    ```

The header navigation bar consists of three areas: left, center, and right. This section describes how to configure them.

### Set the navigation bar editing mode

Set `editMode` to `true` to hide the back button on the left and the three buttons on the right, but display the cancel button.

### Set the background color

Set the navigation background color with `self.navigation.backgroundColor = .red`. You can also modify the internal components in this way, but after switching the theme, it will switch to the default color of the theme.

### Set the avatar

Set the `hiddenAvatar` parameter to determine whether to display the avatar on the left side of the navigation bar. Modify the navigation avatar with `self.navigation.avatarURL = "https://xxx.xxx.xxx"`.

### Set the title

Set the navigation title with `self.navigation.title = "Chats".chat.localize` and the subtitle with `self.navigation.subtitle = "xxx"`. If both need to be modified, set the subtitle first in order to update the corresponding layout position in the navigation.

### Customize the button image

Set the `rightImages` parameters to customize the look of the button on the right. The order is `0`, `1`, and `2`.

### Customize the menu displayed when clicking the button at the top

Use `Appearance.conversation.listMoreActions = value` to customize the menu called by clicking the **+** button at the top right. You can add or delete menu items. The sample code is as follows:

```swift
   //Add a new menu item
        Appearance.conversation.listMoreActions.append(ActionSheetItem(title: "new list item", type: .destructive, tag: "custom"))
        //Remove a menu item
        Appearance.conversation.listMoreActions.removeAll { $0. tag == "you want remove" }
```

### Set up click listener events

To customize navigation and listen to the original navigation click event, override the `navigationClick` method in the conversation list page, and then perform the processing according to the corresponding click area. The sample code is as follows:

```swift
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

## Customize the conversation list

To customize the conversation list `TableView`, override the `createList` method in the conversation list page and return the `ConversationList` class object that you inherited from `EaseChatUIKit`. Find and take a closer look at the `ConversationList.swift` class to implement the business logic. The sample code is as follows:

```swift
override open func createList() -> ConversationList {
            CustomConversationList(frame: CGRect(x: 0, y: self.search.frame.maxY+5, width: self.view.frame.width, height: self.view.frame.height-NavigationHeight-BottomBarHeight-(self.tabBarController?.tabBar.frame.height ?? 49)), style: .plain)
        }
```

## Customize the conversation list item

To customize the contents of the list items, take the following steps:
 
1. Create a new custom class `CustomConversationCell` by inheriting the `ConversationCell` class in `EaseChatUIKit` and set it up with the following code:

    ```swift
    ComponentsRegister.shared.ConversationCell = CustomConversationCell.self
    ```

1. In the `CustomConversationCell` class, override the corresponding method. If you need to reuse the existing logic and add new logic on this basis, override the method and call `super.xxx` in it. For example:

    ```swift
    override open func refresh(info: ConversationInfo) {
                       super.refresh(info:info)
                       // Continue with your new logic
                    }
    ```

    If you need to make changes to the previous logic, copy the code from the previous `refresh` method and make changes without calling `super.xxxx`. Initialization methods and some UI creation methods can be overridden.

### Set the height of the conversation list item

Use `Appearance.conversation.rowHeight = value` to set the height of the conversation list item (cell).

### Set a chat avatar

1. Set the rounded corners of the chat avatar with `Appearance.avatarRadius = value`. There are four levels of avatar corner rounding: Very small, small, medium, and large. 

1. Set `Appearance.conversation.singlePlaceHolder = value` to display avatar placeholders for one-to-one and group chats.
   
### Set the left and right swipe menu items

Set left and right swipe menus for conversation list items with `Appearance.conversation.swipeLeftActions = value` and `Appearance.conversation.swipeRightActions = value`, respectively. 

By default, the left-swipe menu includes muting, pinning, and deleting conversations, and the right-swipe menu includes marking conversations as read and calling up more menus. As an enumeration array, it only supports deleting menu items, not adding them.

```swift
     //Remove
     Appearance.conversation.swipeLeftActions.removeAll { $0 == .more }
```

### Set more conversation actions

Use `Appearance.conversation.moreActions = value` to set the menu that appears after a user swipes right and clicks **...**. You can add or delete menu items. The sample code is as follows:

```swift
//Add menu item
Appearance.conversation.moreActions.append(ActionSheetItem(title: "new list item", type: .destructive, tag: "custom"))
//Delete menu item
Appearance.conversation.moreActions.removeAll { $0. tag == "you want remove" }
```

Get the click event of an item in the array:

```swift
        if let item = Appearance.conversation.listMoreActions.first(where: { $0.tag == "xxx" }) {
            item.actionClosure = { [weak self] _ in
                //do something
            }
        }
        if let item = Appearance.conversation.listMoreActions.first(where: { $0.tag == "xxx" }) {
            item.actionClosure = { [weak self] _ in
                //do something
            }
        }
```

### Set the conversation time

Set the conversation time using two parameters:

- `Appearance.conversation.dateFormatToday = value`: Set the conversation time for the day in the `HH:mm` format.
- `Appearance.conversation.dateFormatOtherDay = value`: Set the date other than today in the `MM:dd` format.

### Intercept the original component click event

You need to fully implement the business and UI refresh logic after interception. It is recommended to use registration inheritance.

The conversation list events are as follows:

- `swipeAction`: Slide event.
- `longPressed`: Long-press event.
- `didSelected`: Click event.

The following sample code is a conversation long-press event:

```swift
ComponentViewsActionHooker.shared.conversation.longPressed = { [weak self] indexPath,info in 
    //Process your business logic.
}
```

## Other settings

All other methods marked as **open** can be overridden to implement a custom business logic. For more configurable items, see [General configurable items](general_configurable_items.md). 


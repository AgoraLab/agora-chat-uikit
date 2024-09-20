You can customize some controls in UIKit, add your own business logic, and implement personalized business needs.

All inheritable components are in `ComponentsRegister.swift`, and you can replace the original components after inheritance.

## Customize the message cell style

You can customize the style of the message cells provided by UIKit, such as adding or hiding them.

The following takes the custom location message cell as an example:

```swift
class CustomLocationMessageCell: LocationMessageCell {
    // Create and return the view you want to display, and the bubble will wrap your view.
    @objc open override func createContent() -> UIView {
        UIView(frame: .zero).backgroundColor(.clear).tag(bubbleTag)
    }
}
// Register a custom class that inherits the original class in EaseChatUIKit to replace the original class.
// Call this method before creating a message page or using other UI components.
ComponentsRegister.shared.ChatLocationCell = CustomLocationMessageCell.self
```

If you want to customize the style of a message cell, you can inherit the basic message cell and customize the style according to your own business.

```swift
ComponentsRegister.shared.registerCustomizeCellClass(cellType: YourMessageCell.self)
class YourMessageCell: MessageCell {
    override func createAvatar() -> ImageView {
        ImageView(frame: .zero)
    }
}
```

## Intercept the original component click event

You need to fully implement the business logic and UI refresh logic after interception. It is recommended to use registration inheritance to fulfill the requirements more quickly.

### Conversation list

- `swipeAction`: The sliding event.
- `longPressed`: The long-press event.
- `didSelected`: The click event.

The following sample code is a conversation long-press event:

```swift
ComponentViewsActionHooker.shared.conversation.longPressed = { [weak self] indexPath,info in 
    //Process you business logic.
}
```

### Contact list

- `didSelectedContact: Click on the contact.
- `groupWithSelected: Click to add group members or create a group and select members.

### Message list

- `replyClicked`: The message bubble clicked in a message.
- `bubbleClicked`: The message bubble is clicked.
- `bubbleLongPressed`: The message bubble is long-pressed.
- `avatarClicked`: The avatar is clicked.
- `avatarLongPressed`: The avatar is long-pressed.
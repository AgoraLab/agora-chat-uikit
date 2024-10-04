# Customize the contact list

You can customize the navigation bar, header, contact list, and contact list items. See [ContactViewController](https://github.com/easemob/easemob-uikit-ios/tree/main/Documentation/EaseChatUIKit.doccarchive/documentation/easechatuikit/contactviewcontroller) for details.

## Customize the header navigation bar

The navigation bars of the contact list page, chat page, conversation list page, group details page, and contact details page use `EaseChatNavigationBar`. If the navigation bar of the contact list page (`ContactViewController.swift`) does not meet your requirements, customize it and pass in the customized navigation class by overriding the method. For details about the title, background color, button image, and avatar, see [Customize the conversation list](customize-conversation-list.md).

## Customize the contact list header

### Set the data source

You can set the header list data source via `Appearance.contact.listHeaderExtensionActions = value`.
   
The following sample code shows how to add or delete data items:

```swift
    //Add data items
    Appearance.contact.listHeaderExtensionActions.append(ContactListHeaderItem(featureIdentify: "New", featureName: "NewFeature", featureIcon: UIImage(named: "NewFeature")))
    //Delete data items
    Appearance.contact.listHeaderExtensionActions.removeAll { $0.featureIdentify == "you want remove" }
```

Get the click event of a single item in the array:

```swift
        if let item = Appearance.contact.listHeaderExtensionActions.first(where: { $0.featureIdentify == "NewFriendRequest" }) {
            item.actionClosure = { [weak self] _ in
                //do something
            }
        }
        if let item = Appearance.contact.listHeaderExtensionActions.first(where: { $0.featureIdentify == "GroupChats" }) {
            item.actionClosure = { [weak self] _ in
                //do something
            }
        }
```

### Set the height of the contact list header cell

Set the height via `Appearance.contact.headerRowHeight = value`.

## Customize the contact list

To customize the contact list `TableView`, override the `createContactList` method in the contact list page and return the class object that inherits the `ContactView` in `EaseChatUIKit`. For details on implementing business logic in the navigation bar, see the `ContactView.swift` class. The sample code is as follows:

```swift
    override open func createContactList() -> ContactView {
        CustomContactView(frame: CGRect(x: 0, y: self.search.frame.maxY+5, width: self.view.frame.width, height: self.view.frame.height-NavigationHeight-BottomBarHeight-(self.tabBarController ?.tabBar.frame.height ?? 49)), style: .plain)
    }
```

## Customize contact list items

To customize the content of a contact cell item in the contact list, take the following steps:

1. Inherit the `ContactCell` class in `EaseChatUIKit` to create a new custom class `CustomContactCell`, then set it with the following code:

```swift
    ComponentsRegister.shared.ContactsCell = CustomContactCell.self
```

1. In the `CustomContactCell` class, override the corresponding method.
   
   If you need to reuse existing logic and add new logic, override the corresponding method and call it `super.xxx`, for example:

```swift
    override open func refresh(profile: EaseProfileProtocol) {
       super.refresh(profile: profile)
       //Continue with your new logic
    }
```

If you want to modify the previous logic, copy the code of the previous `refresh` method and modify it without calling it `super.xxxx`. The initialization method and some UI creation methods can also be overridden.

### Set the height of the contact list cell

Set the height of the contact list item with `Appearance.contact.rowHeight = value`.

### Set the contact avatar

Set the contact avatar radius with `Appearance.avatarRadius = value`. This can be one of four levels: Very small, small, medium, and large.

## Intercept the original component click event

You need to fully implement the business and UI refresh logic after interception. It is recommended to use registration inheritance.

The contact list events include:

- `didSelectedContact`: The contact click.

- `groupWithSelected`: The click to add group members or create a group and select members.

## Other settings

All other methods marked as **open** can be overridden to implement a custom business logic. For more configurable items, see [General configurable items](general_configurable_items.md). 

![Customize Your Contact List](/images/customize-your-contact-list.png)

## 1. Customize the header navigation bar of the Conversation List page (ContactViewController.swift)

- In the demo, inherit the `EaseChatNavigationBar` class in `EaseChatUIKit` to create your own conversation list page navigation. This example is named `CustomConversationNavigationBar`.

- Override the `createNavigation()` method and return the object you created using `CustomConversationNavigationBar`. The sample code is as follows:

```Swift
    override func createNavigationBar() -> EaseChatNavigationBar {
        CustomConversationNavigationBar(showLeftItem: false,rightImages: [UIImage(named: "add", in: .chatBundle, with: nil,hiddenAvatar: false)
    }
```

- To customize the right side of the navigation bar button to display images, set `rightImages` in the above code to return the picture you want. Note that the order is 0, 1, 2. Whether to display the avatar on the left side of the navigation bar can be controlled by the `hiddenAvatar` parameter in the above code.

- To customize navigation and listen to the original navigation click event, you need to overload the `navigationClick` method in the session list page, and then perform corresponding processing according to the corresponding click area. The sample code is as follows:

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

- The edit mode of the navigation bar can be enabled by setting `editMode = true`, which means that both the **back** button and the three buttons on the right side will be hidden, and a **cancel** button will appear on the right side.

- Changing the navigation title content can be achieved by `self.navigation.title = "Chats".chat.localize`, and the implementation of the navigation subtitle  `self.navigation.subtitle = "xxx"` is similar, but it should be noted that you need to set up the subtitle before setting up the title. But if there is no subtitle, then the title can be set directly. The reason for setting the subtitle first and then the title is to update the corresponding layout position inside (if both are present).

- Changing the navigation avatar can be accomplished with `self.navigation.avatarURL = "https://xxx.xxx.xxx"`.

- Setting the navigation and background color can be achieved through `self.navigation.backgroudColor = .red`. The internal components of the navigation can also support this method of modification provided that the theme is not switched. If the theme is switched, it will switch to the theme's default color.

## 2. Customize the list and list items in the contact list page

- To customize the conversation list `TableView`, you need to overload the `createContactList` method in the conversation list page and return the `ContactView` class object that you inherited from `EaseChatUIKit`. You need to find and take a closer look at the `ContactView.swift` class to implement the business logic. The sample code is as follows:

```Swift
    override open func createContactList() -> ContactView {
        CustomContactView(frame: CGRect(x: 0, y: self.search.frame.maxY+5, width: self.view.frame.width, height: self.view.frame.height-NavigationHeight-BottomBarHeight-(self.tabBarController?.tabBar.frame.height ?? 49)), style: .plain)
    }
```

- To customize the contents of the list items in the `ContactCell` contact list, you need to create a new custom class `CustomContactCell` by inheriting the `ContactCell` class in `EaseChatUIKit`, and then set it up with the following code:

```Swift
    ComponentsRegister.shared.ContactsCell = CustomContactCell.self
```

Then in the `CustomContactCell` class, overload the corresponding overloadable method. If you need to reuse the existing logic and add new logic on this basis, you only need to overload a method and call `super.xxx` in it. For example:

```Swift
    override open func refresh(profile: EaseProfileProtocol) {
       super.refresh(profile: profile)
       // Continue with your new logic
    }
```

If you need to make changes to the previous logic, you need to copy the code from the previous `refresh` method and make changes without calling `super.xxxx`. Initialization methods and some of the UI creation methods can be overloaded.

## 3. Other customizable methods in the contact list page

All other methods marked as **open** are overloadable methods. If users need to, they can overload the corresponding methods to implement their own business logic.

## 4. Configurable items in the contacts module

- The following example shows how to add or remove data source items in the contact list header using `Appearance.contact.listHeaderExtensionActions`:

```Swift
    //Add
    Appearance.contact.listHeaderExtensionActions.append(ContactListHeaderItem(featureIdentify: "New", featureName: "NewFeature", featureIcon: UIImage(named: "NewFeature")))
    //Remove
    Appearance.contact.listHeaderExtensionActions.removeAll { $0.featureIdentify == "you want remove" }
```

Get the click event for a single item in this array, for example:

```Swift
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

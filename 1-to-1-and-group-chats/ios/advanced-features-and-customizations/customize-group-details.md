# Customize the group details page

You can configure the navigation bar, group actions, group details, and other items. For details, see [GroupInfoViewController](https://github.com/easemob/easemob-uikit-ios/tree/main/Documentation/EaseChatUIKit.doccarchive/documentation/easechatuikit/groupinfoviewcontroller).

## Customize the navigation bar

The navigation bars of the contact list page, chat page, conversation list page, group details page, and contact details page use `EaseChatNavigationBar`. If the navigation bar of the contact list page (`ContactViewController.swift`) does not meet your requirements, customize it and pass in the customized navigation class by overriding the method. For details about the title, background color, button image, and avatar, see [Customize the conversation list](customize-conversation-list.md).

### Set the group actions

A user can click the **...** button in the upper right corner of the group details page to pop up the `ActionSheet` menu. The data source can be configured with `Appearance.contact.moreActions`. You can add or delete menu items. The sample code is as follows:

```swift
     //Add menu items
     Appearance.contact.moreActions.append(ActionSheetItem(title: "new list item", type: .destructive, tag: "contact_custom"))
     //Delete menu item
     Appearance.contact.moreActions.removeAll { $0. tag == "you want remove" }
```

Get the click event of a single item in the array:

```Swift
        if let item = Appearance.contact.moreActions.first(where: { $0.tag == "xxx" }) {
            item.actionClosure = { [weak self] _ in
                //do something
            }
        }
        if let item = Appearance.contact.moreActions.first(where: { $0.tag == "xxx" }) {
            item.actionClosure = { [weak self] _ in
                //do something
            }
        }
```

## Custom buttons

Configure the data source of the header button with `Appearance.contact.detailExtensionActionItems`. The main features include chatting, audio and video calls, and others. To customize, inherit the group details page, register it into `EaseChatUIKit`, and add configurable items. The example is as follows:

```swift
final class MineGroupDetailViewController: GroupInfoViewController {
    
    override func cleanHistoryMessages() {
        super.cleanHistoryMessages()
        self.showToast(toast: "Clean successful!".localized())
    }

    override func viewDidLoad() {
        Appearance.contact.detailExtensionActionItems = [ContactListHeaderItem(featureIdentify: "Chat", featureName: "Chat".chat.localize, featureIcon: UIImage(named: "chatTo", in: .chatBundle, with: nil)),ContactListHeaderItem(featureIdentify: "AudioCall", featureName: "AudioCall".chat.localize, featureIcon: UIImage(named: "voice_call", in: .chatBundle, with: nil)),ContactListHeaderItem(featureIdentify: "VideoCall", featureName: "VideoCall".chat .localize, featureIcon: UIImage(named: "video_call", in: .chatBundle, with: nil)),ContactListHeaderItem(featureIdentify: "SearchMessages", featureName: "SearchMessages".chat.localize, featureIcon: UIImage(named: "search_history_messages", in: .chatBundle, with: nil))]
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        self.header.status.isHidden = true
    }
    

    override func headerActions() {
        if let chat = Appearance.contact.detailExtensionActionItems.first(where: { $0.featureIdentify == "Chat" }) {
            chat.actionClosure = { [weak self] in
                //do something
            }
        }
        if let search = Appearance.contact.detailExtensionActionItems.first(where: { $0.featureIdentify == "SearchMessages" }) {
            search.actionClosure = { [weak self] in
                //do something
            }
        }
        if let audioCall = Appearance.contact.detailExtensionActionItems.first(where: { $0.featureIdentify == "AudioCall" }) {
            audioCall.actionClosure = { [weak self] in
                //do something
            }
        }
        if let videoCall = Appearance.contact.detailExtensionActionItems.first(where: { $0.featureIdentify == "VideoCall" }) {
            videoCall.actionClosure = { [weak self] in
                //do something
            }
        }
    }
    
    
}
```


 
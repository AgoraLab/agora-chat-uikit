# Customize the contact details

You can customize the navigation bar, actions, contact items, and other elements. See [ContactInfoViewController](https://github.com/easemob/easemob-uikit-ios/tree/main/Documentation/EaseChatUIKit.doccarchive/documentation/easechatuikit/contactinfoviewcontroller) for details.

## Customize the navigation bar

The navigation bars of the contact list page, chat page, conversation list page, group details page, and contact details page use `EaseChatNavigationBar`. If the navigation bar of the contact list page (`ContactViewController.swift`) does not meet your requirements, customize it and pass in the customized navigation class by overriding the method. For details about the title, background color, button image, and avatar, see [Customize the conversation list](customize-conversation-list.md).

### Set the contact actions

A user can click the **...** button in the upper right corner of the contact details page to pop up the `ActionSheet` menu. The data source can be configured with `Appearance.contact.moreActions`. You can add or delete menu items. The sample code is as follows:

```swift
     //Add menu items
     Appearance.contact.moreActions.append(ActionSheetItem(title: "new list item", type: .destructive, tag: "contact_custom"))
     //Delete menu item
     Appearance.contact.moreActions.removeAll { $0. tag == "you want remove" }
```

Get the click event of a single item in the array:

```Swift
        if let item = Appearance.contact.moreActions.first(where: { $0.tag == "xxx" }) {
            item.actionClosure = { [weak self] item,object in
                //do something
            }
        }
        if let item = Appearance.contact.moreActions.first(where: { $0.tag == "xxx" }) {
            item.actionClosure = { [weak self] item,object in
                //do something
            }
        }
```

## Custom buttons

Configure the data source of the header button with `Appearance.contact.detailExtensionActionItems`. The main features include chatting, audio and video calls, and others. To customize, inherit the contact details page, register the inherited page into `EaseChatUIKit`, and add configurable items. The example is as follows:

```swift
final class MineContactDetailViewController: ContactInfoViewController {
    
    override func createHeader() -> DetailInfoHeader {
        super.createHeader()
    }
    
    override func dataSource() -> [DetailInfo] {
        let json: [String : Any] = ["title":"contact_details_button_remark".localized(),"detail":"","withSwitch": false,"switchValue":false]
        let info = self.dictionaryMapToInfo(json: json)
        var infos = super.dataSource()
        infos.insert(info, at: 0)
        return infos
    }

    override func viewDidLoad() {
        Appearance.contact.detailExtensionActionItems = [
            ContactListHeaderItem(featureIdentify: "Chat", featureName: "Chat".chat.localize, featureIcon: UIImage(named: "chatTo", in: .chatBundle, with: nil)),
            ContactListHeaderItem(featureIdentify: "AudioCall", featureName: "AudioCall".chat.localize, featureIcon: UIImage(named: "voice_call", in: .chatBundle, with: nil)),
            ContactListHeaderItem(featureIdentify: "VideoCall", featureName: "VideoCall".chat.localize, featureIcon: UIImage(named: "video_call", in: .chatBundle, with: nil)),
            ContactListHeaderItem(featureIdentify: "SearchMessages", featureName: "SearchMessages".chat.localize, featureIcon: UIImage(named: "search_history_messages", in: .chatBundle, with: nil))
        ]
        super.viewDidLoad()
        self.requestInfo()
        self.header.status.isHidden = true
        // Do any additional setup after loading the view.
    }
    
    
}
```


 

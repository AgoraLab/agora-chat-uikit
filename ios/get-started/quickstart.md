With UIKit, you can easily implement messaging in one-to-one chats and chat groups. This page explains how do this for a one-to-one chat.

## Prerequisites

Before you start, make sure your development environment meets the following conditions:

- The latest version of Xcode (recommended)
- An iOS simulator or Apple device with iOS 13.0 or above 
- CocoaPods installed and integrated

If your network environment has a firewall deployed, contact [Agora technical support](mailto:support@agora.io) to set up a whitelist.

## Implementation

1. [Create a new project](https://developer.apple.com/cn/documentation/xcode/creating_an_xcode_project_for_an_app/) using Xcode. In the **Choose options for your new project** dialog box, make the following settings: 

   - **Product Name**: Fill in **AgoraChatUIKitQuickStart**.
   - **Organization Identifier**: Set to your ID.
   - **User Interface**: Select **Storyboard**.
   - **Language**: Select the development language you commonly use.

1. Initialize the app

   You can initialize the app when it loads or before using UIKit. During initialization, you will need to pass the app key you have obtained in Agora Console.

    ```
   import EaseChatUIKit
       
   @UIApplicationMain
   class AppDelegate：UIResponder，UIApplicationDelegate {
   
        var window：UIWindow?
   
   
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
            let error = EaseChatUIKitClient.shared.setup(appkey: "Appkey")
        }
   }
    ```

1. Log in

    Log in to `EaseChatUIKit` with a user ID and token. If you have integrated Chat SDK, all user IDs can be used to log in with the UIKit.

    In a development environment, you need to create a Chat user in Agora Console and obtain a user token from your app server. 

   ```
   public final class YourAppUser: NSObject, EaseProfileProtocol {
   
               public func toJsonObject() -> Dictionary<String, Any>? {
           ["ease_chat_uikit_user_info":["nickname":self.nickname,"avatarURL":self.avatarURL,"userId":self.id]]
       }
       
       
       public var id: String = ""
           
       public var nickname: String = ""
           
       public var selected: Bool = false
       
       public override func setValue(_ value: Any?, forUndefinedKey key: String) {
           
       }
   
       public var avatarURL: String = "https://accktvpic.oss-cn-beijing.aliyuncs.com/pic/sample_avatar/sample_avatar_1.png"
   
   }
   // Log in to EaseChatUIKit using the user information of the current user object that conforms to the `EaseProfileProtocol` protocol
    EaseChatUIKitClient.shared.login(user: YourAppUser(), token: ExampleRequiredConfig.chatToken) { error in 
    }
    ```

1. Create a chat page

   1. Turn off the friend check function, which means you can chat without adding friends.
   1. Call the `init` method and pass the user ID of the user created in the Console into the `conversationId` parameter to send a message to the user.

    ```swift
   let vc = ComponentsRegister.shared.MessageViewController.init(conversationId: <#Create user's id#>, chatType: .chat)
   // Either push or present can be used
    ControllerStack.toDestination(vc: vc)
    ```
   
1. Send the first message

    Type your message at the bottom of the chat page and click **Send** to send the message.
# Quickstart

With UIKit, you can easily implement messaging in one-to-one chats and group chats. This page explains how do this for a one-to-one chat.

## Prerequisites

Before you start, make sure your development environment meets the following conditions:

- The latest version of Xcode (recommended);
- An iOS simulator or Apple device with iOS 13.0 or above;
- CocoaPods installed and integrated;
- A valid Agora project with users and tokens generated. See [Enable and configure Chat](https://docs.agora.io/en/agora-chat/get-started/enable) and [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication) for details. 

If your network environment has a firewall deployed, contact [Agora technical support](mailto:support@agora.io) to set up a allow list.

## Implementation

1. [Create a new project](https://developer.apple.com/cn/documentation/xcode/creating_an_xcode_project_for_an_app/) using Xcode. In the **Choose options for your new project** dialog box, configure the following settings: 

   - **Product Name**: Fill in **ChatUIKitQuickStart**.
   - **Organization Identifier**: Set to your ID.
   - **User Interface**: Select **Storyboard**.
   - **Language**: Select the development language you commonly use.

1. Initialize UIKit.

   You can initialize UIKit when the app loads before UIKit is used. During initialization, pass the app key you have obtained in Agora Console:

    ```
    import chat_uikit
        
    @UIApplicationMain
    class AppDelegate：UIResponder，UIApplicationDelegate {
    
         var window：UIWindow?
    
    
         func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
             let error = ChatUIKitClient.shared.setup(appkey: "Appkey")
         }
    }
    ```

2. Log in to UIKit.

    Log in to UIKit with a user ID and token. If you have integrated Chat SDK, all user IDs can be used to log in to UIKit:

    ```
    public final class YourAppUser: NSObject, ChatUserProfileProtocol {
   
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
    // Log in to chat_uikit using the user information of the current user object that conforms to the `ChatUserProfileProtocol` protocol
    ChatUIKitClient.shared.login(user: YourAppUser(), token: ExampleRequiredConfig.chatToken) { error in 
    }
    ```

3. Create a chat page,

   1. Turn off the friend check feature, which means you can chat with strangers without adding them to the contact list.
   2. Call the `init` method and pass the user ID of the user created in the Console into the `conversationId` parameter to send a message to the user:

    ```swift
    let vc = ComponentsRegister.shared.MessageViewController.init(conversationId: <#Create user's id#>, chatType: .chat)
    // Either push or present can be used
     ControllerStack.toDestination(vc: vc)
    ```
   
4. Send the first message

    Type your message at the bottom of the chat page and click **Send** to send the message.
# Quickstart

With UIKit, you can easily implement user interaction in a chat room. This page describes how to implement sending chat room messages.

## Prerequisites

- Xcode (latest version recommended);
- An iOS simulator or Apple device with iOS 13.0 or later installed;
- The `ChatroomUIKit` dependency added using CocoaPods;
- A valid Agora project with users and tokens generated. See [Enable and configure Chat](https://docs.agora.io/en/agora-chat/get-started/enable) and [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication) for details. 

## Implementation

Take the following steps to implement message sending:

1. Initialize `AgoraChatroomUIKit`:

    You can initialize `ChatroomUIKit` when your app loads or before use. During initialization, you need to pass in the app key.

    ```swift
    import AgoraChatroomUIKit
        
    @UIApplicationMain
    class AppDelegate：UIResponder，UIApplicationDelegate {
         var window：UIWindow？
         func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
             let error = ChatroomUIKitClient.shared.setup(appKey: "Appkey")
         }
    }
    ```
   
1. Log in to `ChatroomUIKit`:

   Log in to UIKit with user ID and user token. If you have integrated tChat SDK, all user IDs in the SDK can be used to log in to UIKit.

   ```swift
   ChatroomUIKitClient.shared.login(userId: "user id", token: "token", completion: <#T##(ChatError?) -> Void#>)
   ```

1. Log in to `AgoraChatroomUIKit`.

   If you have integrated Chat SDK, the corresponding user IDs can be used to log into UIKit. 
  
   ```kotlin
         ChatroomUIKitClient.getInstance().login("userId", "token")
   ```

1. Create a chat room view

   To create a chat room view, follow these steps:

   1. Get the chat room list and join the specified chat room.

   1. Create the chat room view `ChatroomView`, passing in the chat room ID, layout parameters, and the user ID of the chat room owner. 
    
    It is recommended to initialize the width of `ChatroomView` to the width of the screen, and the height to at least the value of the following formula: screen height x 1/5 + gift bubble row height x 2 + 54 (the height of the bottom toolbar). For models with notch screens, the height of ChatroomView is the value of the above formula plus the height of the bottom safe area.

    ```swift
    let roomView = ChatroomUIKitClient.shared.launchRoomView(roomId: "Chat room ID",frame: CGRect, ownerId: "Chatroom owner ID")       
    ```

   1. Add the chat room view to the target area.

      When calling `ChatroomUIKitClient.shared.launchRoomView(roomId: self.roomId, frame: CGRect(x: 0, y: ScreenHeight/2.0, width: ScreenWidth, height: ScreenHeight/2.0), ownerId: "OwnerID")`, remember to add `ChatroomView` to the existing view to facilitate interception and pass-through of click events. For example, if you have a view that plays a video stream, add it above the video view. The passed in `frame` is the available area, the message screen area, the bottom toolbar area, and the response height of the input box event after the keyboard pops up.

1. Send a message

   Enter the message content at the bottom of the screen and click **Send** to send the message.

   ![Send a message](../assets/images/click_chat.png)

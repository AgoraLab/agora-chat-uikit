# Best practices

## Initialize ChatroomUIKit

Initialization is a necessary step to use `ChatroomUIKit` and must be completed before calling any interface methods.

When initializing `ChatroomUIKit`, you can pass in the `option` parameters to set various options.

```swift
let option = ChatroomUIKitInitialOptions.ChatOptions()
option.enableConsoleLog = true
option.autoLogin = true
let error = ChatroomUIKitClient.shared.setup(appKey,option: option)
```

## Log in to ChatroomUIKit

You can log in to `ChatroomUIKit` by using the user object in the project and complying with the `UserInfoProtocol` protocol. The sample code is as follows:

```swift
public final class YourAppUser: NSObject,UserInfoProtocol {
        
        public func toJsonObject() -> Dictionary<String, Any>? {
            ["userId":self.userId,"nickName":self.nickname,"avatarURL":self.avatarURL,"identity":self.identity,"gender":self.gender]
        }
        
        public var identity: String = ""//用户级别图像的 URL
        
        public var userId: String = <#T##String#>
        
        public var nickname: String = "Jack"
        
        public var avatarURL: String = "https://accktvpic.oss-cn-beijing.aliyuncs.com/pic/sample_avatar/sample_avatar_1.png"
        
        public var gender: Int = 1
        
    }
ChatroomUIKitClient.shared.login(user: YourAppUser(), token: "token", completion: <#T##(ChatError?) -> Void#>)
```

## Initialize the chat room view

1. Get the chat room list and join the specified chat room.

1. Create the chat room view `ChatroomView`, passing in the chat room ID, layout parameters, the user ID of the chat room owner, and optional parameters.

    It is recommended to initialize the width of `ChatroomView` to the width of the screen, and the height to at least the value of the following formula: screen height x 1/5 + gift bubble row height x 2 + 54 (the height of the bottom toolbar). For models with notch screens, the height of ChatroomView is the value of the above formula plus the height of the bottom safe area.

1. Add a view.

```swift
let options  = ChatroomUIKitInitialOptions.UIOptions()
options.bottomDataSource = self.bottomBarDatas()
// Implement the gift message area.
options.showGiftMessageArea = true
// Whether to display gift information in the chat area.
options.chatAreaShowGift = false
let roomView = ChatroomUIKitClient.shared.launchRoomView(roomId: self.roomId, frame: CGRect(x: 0, y: ScreenHeight/2.0, width: ScreenWidth, height: ScreenHeight/2.0), ownerId: "Chatroom's owner user id", options: options)
addSubView(roomView)
```
It is recommended that you add this view above your business view to facilitate `ChatroomView` to intercept and pass through click events. For example, if you have a view that plays a video, you need to add `ChatroomView` above that view.

## Listen to ChatroomUIKit events and errors

You can call the `registerRoomEventsListener` method to add listeners for `ChatroomUIKit` events and errors.

```swift
ChatroomUIKitClient.shared.registerRoomEventsListener(self)
```

To learn more about the best practices above, click [here](https://github.com/easemob/ChatroomDemo/tree/dev/iOS/ChatroomDemo). 
# Best practices

## Initialize ChatroomUIKit

Initialization is a necessary step to use `ChatroomUIKit` and must be completed before calling any interface methods.

When initializing `ChatroomUIKit`, you can pass in `optionparameters` to set various options.

```kotlin
 		val chatroomUIKitOptions = ChatroomUIKitOptions(
            chatOptions = ChatSDKOptions(enableDebug = true),
            uiOptions = UiOptions(
                targetLanguageList = listOf(currentLanguage),
                useGiftsInList = false,
            )
        )
```

## Log in to ChatroomUIKit

You can log in to `ChatroomUIKit` by using the user object in the project and complying with the `UserInfoProtocol` protocol. The sample code is as follows:

```kotlin
class YourAppUser: UserInfoProtocol {
    var userId: String = "your application user id"
            
    var nickName: String = "you user nick name"
            
    var avatarURL: String = "you user avatar url"
            
    var gender: Int = 1
            
    var identity: String =  "you user level symbol url"
            
}
ChatroomUIKitClient.getInstance().login(YourAppUser, token, onSuccess = {}, onError = {code,error ->})
```

## Initialize the chat room view

1. Get the chat room list and join the specified chat room.

1. Load the chat room view `ComposeChatroom`, passing in the chat room ID, layout parameters, the user ID of the chat room owner, and optional parameters.

```kotlin
class ChatroomActivity : ComponentActivity(){
	override fun onCreate(savedInstanceState: Bundle?) {
		super.onCreate(savedInstanceState)
		setContent {
			ComposeChatroom(roomId = roomId,roomOwner = ownerInfo)
		}
	}
}
```

## Listen to ChatroomUIKit events and errors

You can call the `registerRoomResultListener` method to add listeners for `ChatroomUIKit` events and errors.

```kotlin
ChatroomUIKitClient.getInstance().registerRoomResultListener(this)
```

To learn more about the best practices above, click [here](https://github.com/AgoraIO-Usecase/AgoraChat-UIKit-Chatroom-android/tree/main/app). 
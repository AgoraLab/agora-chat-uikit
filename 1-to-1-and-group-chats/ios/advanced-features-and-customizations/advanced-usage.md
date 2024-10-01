The following are examples of advanced usage of UIKit. The conversation list, message list page, and contact list can all be used separately.

## Initialization

Compared to the initialization in the quickstart, additional `ChatOptions` parameter is added here, including switches for whether to print logs in the SDK, to log in automatically, and to use user attributes by default.

```swift
let error = EaseChatUIKitClient.shared.setup(option: ChatOptions(appkey: appKey))
```

## Login

Use the information of the current user object that conforms to `EaseProfileProtocol` to log in to UIKIt. Pass in the user ID into the `userId` field in the following code: 

```swift
public final class YourAppUser: NSObject, EaseProfileProtocol {

    public func toJsonObject() -> Dictionary<String, Any>? {
        ["ease_chat_uikit_user_info":["nickname":self.nickname,"avatarURL":self.avatarURL,"userId":self.id]]
    }

    public var userId: String = <#T##String#>

    public var nickname: String = "Jack"

    public var avatarURL: String = "https://accktvpic.oss-cn-beijing.aliyuncs.com/pic/sample_avatar/sample_avatar_1.png"

}
 EaseChatUIKitClient.shared.login(user: YourAppUser(), token: ExampleRequiredConfig.chatToken) { error in 
 }
```

If you have integrated Chat SDK, all user IDs can be used to log in to UIKit. 

## Provider in EaseChatUIKitContext

<Admonition type="tip" title="Note">Provider is only used for the conversation list and contact list. If you enter the chat page through quickstart, you do not need to implement the Provider.</Admonition>

1. Set the Provider implementation class.

    - Use coroutines to asynchronously return information about the conversation list. This is limited to Swift:

    ```swift
        //userProfileProvider is the provider of user data. Coroutine implementation and userProfileProviderOC cannot coexist at the same time. userProfileProviderOC is implemented using closures.
        EaseChatUIKitContext.shared?.userProfileProvider = self
        EaseChatUIKitContext.shared?.userProfileProviderOC = nil
        //The principle of groupProvider is the same as above
        EaseChatUIKitContext.shared?.groupProfileProvider = self
        EaseChatUIKitContext.shared?.groupProfileProviderOC = nil
    ```

    - Use closure to return information about the conversation list. This can be used in both Swift and OC:

    ```swift
           //userProfileProvider is the provider of user data. Coroutine implementation and userProfileProviderOC cannot coexist at the same time. userProfileProviderOC is implemented using closures.
           EaseChatUIKitContext.shared?.userProfileProvider = nil
           EaseChatUIKitContext.shared?.userProfileProviderOC = self
           //The principle of groupProvider is the same as above
           EaseChatUIKitContext.shared?.groupProfileProvider = nil
           EaseChatUIKitContext.shared?.groupProfileProviderOC = self
    ```
   
1. Implement the conversation list Provider.

    For Objective-C, implement `EaseProfileProviderOC`. The following sample code implements a Swift-specific provider with the coroutine functionality.

    ```swift
       //MARK: - EaseProfileProvider for conversations&contacts usage.
       //For example, using conversations controller as follows:
       extension MainViewController: EaseProfileProvider,EaseGroupProfileProvider {
           //MARK: - EaseProfileProvider
           func fetchProfiles(profileIds: [String]) async -> [any EaseChatUIKit.EaseProfileProtocol] {
               return await withTaskGroup(of: [EaseChatUIKit.EaseProfileProtocol].self, returning: [EaseChatUIKit.EaseProfileProtocol].self) { group in
                   var resultProfiles: [EaseChatUIKit.EaseProfileProtocol] = []
                   group.addTask {
                       var resultProfiles: [EaseChatUIKit.EaseProfileProtocol] = []
                       let result = await self.requestUserInfos(profileIds: profileIds)
                       if let infos = result {
                           resultProfiles.append(contentsOf: infos)
                       }
                       return resultProfiles
                   }
                   //Await all task were executed.Return values.
                   for await result in group {
                       resultProfiles.append(contentsOf: result)
                   }
                   return resultProfiles
               }
           }
           //MARK: - EaseGroupProfileProvider
           func fetchGroupProfiles(profileIds: [String]) async -> [any EaseChatUIKit.EaseProfileProtocol] {
               
               return await withTaskGroup(of: [EaseChatUIKit.EaseProfileProtocol].self, returning: [EaseChatUIKit.EaseProfileProtocol].self) { group in
                   var resultProfiles: [EaseChatUIKit.EaseProfileProtocol] = []
                   group.addTask {
                       var resultProfiles: [EaseChatUIKit.EaseProfileProtocol] = []
                       let result = await self.requestGroupsInfo(groupIds: profileIds)
                       if let infos = result {
                           resultProfiles.append(contentsOf: infos)
                       }
                       return resultProfiles
                   }
                   //Await all task were executed.Return values.
                   for await result in group {
                       resultProfiles.append(contentsOf: result)
                   }
                   return resultProfiles
               }
           }
           
           private func requestUserInfos(profileIds: [String]) async -> [EaseProfileProtocol]? {
               var unknownIds = [String]()
               var resultProfiles = [EaseProfileProtocol]()
               for profileId in profileIds {
                   if let profile = EaseChatUIKitContext.shared?.userCache?[profileId] {
                       if profile.nickname.isEmpty {
                           unknownIds.append(profile.id)
                       } else {
                           resultProfiles.append(profile)
                       }
                   } else {
                       unknownIds.append(profileId)
                   }
               }
               if unknownIds.isEmpty {
                   return resultProfiles
               }
               let result = await ChatClient.shared().userInfoManager?.fetchUserInfo(byId: unknownIds)
               if result?.1 == nil,let infoMap = result?.0 {
                   for (userId,info) in infoMap {
                       let profile = EaseChatProfile()
                       let nickname = info.nickname ?? ""
                       profile.id = userId
                       profile.nickname = nickname
                       if let remark = ChatClient.shared().contactManager?.getContact(userId)?.remark {
                           profile.remark = remark
                       }
                       profile.avatarURL = info.avatarUrl ?? ""
                       resultProfiles.append(profile)
                       if (EaseChatUIKitContext.shared?.userCache?[userId]) != nil {
                           profile.updateFFDB()
                       } else {
                           profile.insert()
                       }
                       EaseChatUIKitContext.shared?.userCache?[userId] = profile
                   }
                   return resultProfiles
               }
               return []
           }
           
           private func requestGroupsInfo(groupIds: [String]) async -> [EaseProfileProtocol]? {
               var resultProfiles = [EaseProfileProtocol]()
               let groups = ChatClient.shared().groupManager?.getJoinedGroups() ?? []
               for groupId in groupIds {
                   if let group = groups.first(where: { $0.groupId == groupId }) {
                       let profile = EaseChatProfile()
                       profile.id = groupId
                       profile.nickname = group.groupName
                       profile.avatarURL = group.settings.ext
                       resultProfiles.append(profile)
                       EaseChatUIKitContext.shared?.groupCache?[groupId] = profile
                   }
       
               }
               return resultProfiles
           }
       }
    ```

## Conversation list

1. Create a conversation list:

    ```swift
    let vc = EaseChatUIKit.ComponentsRegister.shared.ConversationsController.init()
    vc.tabBarItem.tag = 0
    ```
   
1. Listen for conversation list events:

    ```swift
    vc.viewModel?.registerEventsListener(listener: self)
    ```

## Contact list

1. Create a contact list page. 

    A custom class that inherits the contact list page class can call ViewModel's methods to listen to related events after registering it with `ContactViewController().viewModel.registerEventsListener`:

    ```swift
    let vc = EaseChatUIKit.ComponentsRegister.shared.ContactsController.init(headerStyle: .contact)
    ```

1.  Listen for contact list page events:

    ```swift
    vc.viewModel?.registerEventsListener(listener: self)
    ```

## Initialize the chat page

Most of the message and page processing logic in the chat page can be overridden, including ViewModel:

```swift
//Create a new user in the Agora Console, pass the user ID into the following construction method parameters, and jump to the chat page.
let vc = ComponentsRegister.shared.MessageViewController.init(conversationId: <#ID of the user just created#>, chatType: .chat)
// Custom classes after inheritance and registration can also call the registerEventsListener method of ViewModel to listen for chat message-related events, such as message reception, long press, click, etc. 
// Either push or present can be used
ControllerStack.toDestination(vc: vc)
```

## Monitor user and server connection events

Call `registerUserStateListener` to listen to the events and errors related to the user and the connection status changes between the server and UIKit:

```swift
EaseChatUIKitClient.shared.registerUserStateListener(self)
```

When the listener is not used, call `unregisterUserStateListener` to remove it:

```swift
EaseChatUIKitClient.shared.unregisterUserStateListener(self)
```

## More

For more advanced usage, refer to the [demo](https://github.com/easemob/easemob-demo-ios/tree/SwiftDemo).
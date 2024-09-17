The following are examples of advanced usage of UIKit. The conversation list page, message list page, and contact list page can all be used separately.

## Initialization

Compared to the initialization in the quickstart, additional `ChatOptions` parameters are added, including the switch configuration of whether to print logs in the SDK, whether to log in automatically, and whether to use user attributes by default.

```swift
let error = AgoraChatUIKitClient.shared.setup(option: ChatOptions(appkey: appKey))
```

## Login

Use the user information of the current user object that conforms to `AgoraProfileProtocol` to log in to `AgoraChatUIKit`.

Create a user on Agora Console and pass in the user ID into the `userId` field in the following code. 

If you have integrated Chat SDK, all user IDs can be used to log in with the UIKit. 

```swift
public final class YourAppUser: NSObject, ChatProfileProtocol {

    public func toJsonObject() -> Dictionary<String, Any>? {
        ["agora_chat_uikit_user_info":["nickname":self.nickname,"avatarURL":self.avatarURL,"userId":self.id]]
    }

    public var userId: String = <#T##String#>

    public var nickname: String = "Jack"

    public var avatarURL: String = "https://accktvpic.oss-cn-beijing.aliyuncs.com/pic/sample_avatar/sample_avatar_1.png"

}
 AgoraChatUIKitClient.shared.login(user: YourAppUser(), token: ExampleRequiredConfig.chatToken) { error in 
 }
```

## Provider in AgoraChatUIKitContext

<Admonition type="tip" title="Note">Provider is only used for the conversation list and contact list. If you only enter the chat page through quickstart, you do not need to implement the Provider.</Admonition>

1. Set the Provider implementation class

    - Use coroutines to asynchronously return information about the conversation list. This is limited to Swift.

    ```swift
       
        //userProfileProvider is the provider of user data. Coroutine implementation and userProfileProviderOC cannot coexist at the same time. userProfileProviderOC is implemented using closures.
        AgoraChatUIKitContext.shared?.userProfileProvider = self
        AgoraChatUIKitContext.shared?.userProfileProviderOC = nil
        //The principle of groupProvider is the same as above
        AgoraChatUIKitContext.shared?.groupProfileProvider = self
        AgoraChatUIKitContext.shared?.groupProfileProviderOC = nil
    ```

    - Use closure to return information about the conversation list. It can be used in both Swift and OC.

    ```swift
           //userProfileProvider is the provider of user data. Coroutine implementation and userProfileProviderOC cannot coexist at the same time. userProfileProviderOC is implemented using closures.
           AgoraChatUIKitContext.shared?.userProfileProvider = nil
           AgoraChatUIKitContext.shared?.userProfileProviderOC = self
           //The principle of groupProvider is the same as above
           AgoraChatUIKitContext.shared?.groupProfileProvider = nil
           AgoraChatUIKitContext.shared?.groupProfileProviderOC = self
    ```
   
1. Implement the conversation list Provider.

    For Objective-C, implement `ChatProfileProviderOC`. The following sample code implements a Swift-specific provider with the coroutine functionality.

    ```
       //MARK: - ChatProfileProvider for conversations&contacts usage.
       //For example, using conversations controller as follows:
       extension MainViewController: ChatProfileProvider,ChatGroupProfileProvider {
           //MARK: - ChatProfileProvider
           func fetchProfiles(profileIds: [String]) async -> [any AgoraChatUIKit.ChatProfileProtocol] {
               return await withTaskGroup(of: [AgoraChatUIKit.ChatProfileProtocol].self, returning: [AgoraChatUIKit.ChatProfileProtocol].self) { group in
                   var resultProfiles: [AgoraChatUIKit.ChatProfileProtocol] = []
                   group.addTask {
                       var resultProfiles: [AgoraChatUIKit.ChatProfileProtocol] = []
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
           //MARK: - ChatGroupProfileProvider
           func fetchGroupProfiles(profileIds: [String]) async -> [any AgoraChatUIKit.ChatProfileProtocol] {
               
               return await withTaskGroup(of: [AgoraChatUIKit.ChatProfileProtocol].self, returning: [AgoraChatUIKit.ChatProfileProtocol].self) { group in
                   var resultProfiles: [AgoraChatUIKit.ChatProfileProtocol] = []
                   group.addTask {
                       var resultProfiles: [AgoraChatUIKit.ChatProfileProtocol] = []
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
           
           private func requestUserInfos(profileIds: [String]) async -> [ChatProfileProtocol]? {
               var unknownIds = [String]()
               var resultProfiles = [ChatProfileProtocol]()
               for profileId in profileIds {
                   if let profile = AgoraChatUIKitContext.shared?.userCache?[profileId] {
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
                       let profile = AgoraChatProfile()
                       let nickname = info.nickname ?? ""
                       profile.id = userId
                       profile.nickname = nickname
                       if let remark = ChatClient.shared().contactManager?.getContact(userId)?.remark {
                           profile.remark = remark
                       }
                       profile.avatarURL = info.avatarUrl ?? ""
                       resultProfiles.append(profile)
                       if (AgoraChatUIKitContext.shared?.userCache?[userId]) != nil {
                           profile.updateFFDB()
                       } else {
                           profile.insert()
                       }
                       AgoraChatUIKitContext.shared?.userCache?[userId] = profile
                   }
                   return resultProfiles
               }
               return []
           }
           
           private func requestGroupsInfo(groupIds: [String]) async -> [ChatProfileProtocol]? {
               var resultProfiles = [ChatProfileProtocol]()
               let groups = ChatClient.shared().groupManager?.getJoinedGroups() ?? []
               for groupId in groupIds {
                   if let group = groups.first(where: { $0.groupId == groupId }) {
                       let profile = AgoraChatProfile()
                       profile.id = groupId
                       profile.nickname = group.groupName
                       profile.avatarURL = group.settings.ext
                       resultProfiles.append(profile)
                       AgoraChatUIKitContext.shared?.groupCache?[groupId] = profile
                   }
       
               }
               return resultProfiles
           }
       }
    ```

## Conversation list

1. Create a conversation list 

    ```swift
       
        let vc = AgoraChatUIKit.ComponentsRegister.shared.ConversationsController.init()
        vc.tabBarItem.tag = 0
    ```
   
1. Listen for events on the conversation list page

    ```swift
           
        vc.viewModel?.registerEventsListener(listener: self)
    ```


## Contact list

1. Create a contact list page

    The custom class that inherits the contact list page class provided by UIKit can call ViewModel's methods to listen to related events after registering `ContactViewController().viewModel.registerEventsListener`.

    ```swift
           let vc = AgoraChatUIKit.ComponentsRegister.shared.ContactsController.init(headerStyle: .contact)
    ```

1.  Listen for contact list page events

    ```swift
            vc.viewModel?.registerEventsListener(listener: self)
    ```

## Initialize chat page

Most of the message processing and page processing logic in the chat page can be overridden, including ViewModel.

```
//Create a new user in the Agora Console, pass the user ID into the following construction method parameters, and jump to the chat page.
let vc = ComponentsRegister.shared.MessageViewController.init(conversationId: <#ID of the user just created#>, chatType: .chat)
// Custom classes after inheritance and registration can also call the registerEventsListener method of ViewModel to listen for chat message-related events, such as message reception, long press, click, etc. 
// Either push or present can be used
ControllerStack.toDestination(vc: vc)
```

## Monitor user and server connection events

You can call `registerUserStateListener` to listen to the events and errors related to the user and the connection status changes between the server and AgoraChatUIKit.

```
AgoraChatUIKitClient.shared.registerUserStateListener(self)
```

When the listener is not used, you can call `unregisterUserStateListener` to remove it:

```
AgoraChatUIKitClient.shared.unregisterUserStateListener(self)
```

## More

For more advanced usage, refer to the [demo](https://github.com/easemob/easemob-demo-ios/tree/SwiftDemo).
# User-defined information

You must provide user information that is used across UIKit. This page describes how to do it.

## Logged-in user information

When a user calls the `ChatUIKitClient.login` method to log in, they need to pass in an `ChatUIKitProfile` object, which contains three properties: `id`, `name`, and `avatar`. `id` is a required parameter. `name` and `avatar` display the logged-in user's nickname and avatar. When sending a message, set the `name` and `avatar` properties to facilitate the display to other users.

If the `name` and `avatar` properties are not passed in during login, you can call the `ChatUIKitClient.updateCurrentUser` method to update the user information after login.

```kotlin
ChatUIKitClient.login(
    user = ChatUIKitProfile(
        id = "",
        name = "",
        avatar = ""
    ),
    token = "", 
    onSuccess = {
                        
    }, 
    onError = {code,error ->
                
    }
)
```

## User information

UIKit offers an `ChatUIKitClient.setUserProfileProvider` interface to provide user information, including contact and group member information. The `ChatUIKitUserProfileProvider` interface is as follows:

```kotlin
interface ChatUIKitUserProfileProvider {
    // Get user information synchronously
    fun getUser(userId: String?): ChatUIKitProfile?

    // Get user information asynchronously
    fun fetchUsers(userIds: List<String>, onValueSuccess: OnValueSuccess<List<ChatUIKitProfile>>)
}
```

The usage example is as follows:

```kotlin
ChatUIKitClient.setUserProfileProvider(object : ChatUIKitUserProfileProvider {
    override fun getUser(userId: String?): ChatUIKitProfile? {
        return getLocalUserInfo(userId)
    }

    override fun fetchUsers(
        userIds: List<String>,
        onValueSuccess: OnValueSuccess<List<ChatUIKitProfile>>
    ) {
        fetchUserInfoFromServer(idsMap, onValueSuccess)
    }

})
```

## Group information

UIKit provides an `ChatUIKitClient.setGroupProfileProvider` interface to provide group information. The sample code is as follows:

```kotlin
interface ChatUIKitGroupProfileProvider {
// Get group information synchronously
fun getGroup(id: String?): ChatUIKitGroupProfile?

// Get group information asynchronously
fun fetchGroups(groupIds: List<String>, onValueSuccess: OnValueSuccess<List<ChatUIKitGroupProfile>>)
}
```

The usage example is as follows:

```kotlin
ChatUIKitClient.setGroupProfileProvider(object : ChatUIKitGroupProfileProvider {
    override fun getGroup(id: String?): ChatUIKitGroupProfile? {
        ChatClient.getInstance().groupManager().getGroup(id)?.let {
            return ChatUIKitGroupProfile(it.groupId, it.groupName, it.extension)
        }
        return null
    }

    override fun fetchGroups(
            groupIds: List<String>,
            onValueSuccess: OnValueSuccess<List<ChatUIKitGroupProfile>>
    ) {
// Get group-related information based on the groupId list, return it through onValueSuccess(), and update the cache information.
    }
})
```

## Set the chat avatar and nickname


```kotlin

 // Call setUserProfileProvider to set the user attributes for a one-to-one chat, including user avatar and nickname.
 ChatUIKitClient.setUserProfileProvider(object : ChatUIKitUserProfileProvider {
     override fun getUser(userId: String?): ChatUIKitProfile? {
         // Return the local user attributes corresponding to userId
         return DemoHelper.getInstance().getDataModel().getAllContacts()[userId]?.toProfile()
     }

     override fun fetchUsers(
         userIds: List<String>,
         onValueSuccess: OnValueSuccess<List<ChatUIKitProfile>>
     ) {
         // Provider. Users can obtain the Profile information of the corresponding ID from their own server according to the userId list and return it through onValueSuccess().
         // At the same time, the acquired information can be updated to the cache through ChatUIKitClient.updateUsersInfo(). When obtaining the Profile, UIKit will query from the cache first.
     }
 })
 // Set group information through setGroupProfileProvider.
 ChatUIKitClient.setGroupProfileProvider(object : ChatUIKitGroupProfileProvider {

    override fun getGroup(id: String?): ChatUIKitGroupProfile? {
        ChatClient.getInstance().groupManager().getGroup(id)?.let {
            return ChatUIKitGroupProfile(it.groupId, it.groupName, it.extension)
        }
        return null
    }

    override fun fetchGroups(
        groupIds: List<String>,
        onValueSuccess: OnValueSuccess<List<ChatUIKitGroupProfile>>
    ) {
        // Get group-related information based on the groupId list through onValueSuccess() and update the cached information.
    }
})
```

## Information processing logic

1. If the information has been cached in memory, when the page needs to display information, UIKit will first obtain the cached data from memory and render the page.

1. If there is no cached data, you can get the data from the app's local database or memory, build an `ChatUIKitProfile` object, and use the `getUser` method to return it. This way, UIKit will use the `ChatUIKitProfile` object to update the information in the UI.

1. If the data is not obtained through the `getUser` method, the UIKit provider will get the data from your server through the `fetchUsers` method: When the list page stops sliding, UIKit will first get the data from the cache, provide a list of user IDs for which there is no cached data, and query the data of these users from the server. You can construct a `List <ChatUIKitProfile> ` object, and when calling the `fetchUsers` method, the data will be returned through `onValueSuccess(List<ChatUIKitProfile>)`.

## Update cached information for UIKit

You can update the cached information using the `update` method provided by UIKit:

```kotlin
// Update current user information
ChatUIKitClient.updateCurrentUser(currentUserProfile)
// Update one-to-one chat user/group member information
ChatUIKitClient.updateUsersInfo(userProfileList)
```
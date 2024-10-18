# User-defined information

You must provide user information that is used across UIKit. This page describes how to do it.

## Logged-in user information

When a user calls the `EaseIM.login` method to log in, they need to pass in an `EaseProfile` object, which contains three properties: `id`, `name`, and `avatar`. `id` is a required parameter. `name` and `avatar` display the logged-in user's nickname and avatar. When sending a message, set the `name` and `avatar` properties to facilitate the display to other users.

If the `name` and `avatar` properties are not passed in during login, you can call the `EaseIM.updateCurrentUser` method to update the user information after login.

```kotlin
EaseIM.login(
    user = EaseProfile(
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

UIKit offers an `EaseIM.setUserProfileProvider` interface to provide user information, including contact and group member information. The `EaseUserProfileProvider` interface is as follows:

```kotlin
interface EaseUserProfileProvider {
    // Get user information synchronously
    fun getUser(userId: String?): EaseProfile?

    // Get user information asynchronously
    fun fetchUsers(userIds: List<String>, onValueSuccess: OnValueSuccess<List<EaseProfile>>)
}
```

The usage example is as follows:

```kotlin
EaseIM.setUserProfileProvider(object : EaseUserProfileProvider {
    override fun getUser(userId: String?): EaseProfile? {
        return getLocalUserInfo(userId)
    }

    override fun fetchUsers(
        userIds: List<String>,
        onValueSuccess: OnValueSuccess<List<EaseProfile>>
    ) {
        fetchUserInfoFromServer(idsMap, onValueSuccess)
    }

})
```

## Group information

UIKit provides an `EaseIM.setGroupProfileProvider` interface to provide group information. The sample code is as follows:

```kotlin
interface EaseGroupProfileProvider {
// Get group information synchronously
fun getGroup(id: String?): EaseGroupProfile?

// Get group information asynchronously
fun fetchGroups(groupIds: List<String>, onValueSuccess: OnValueSuccess<List<EaseGroupProfile>>)
}
```

The usage example is as follows:

```kotlin
EaseIM.setGroupProfileProvider(object : EaseGroupProfileProvider {
    override fun getGroup(id: String?): EaseGroupProfile? {
        ChatClient.getInstance().groupManager().getGroup(id)?.let {
            return EaseGroupProfile(it.groupId, it.groupName, it.extension)
        }
        return null
    }

    override fun fetchGroups(
            groupIds: List<String>,
            onValueSuccess: OnValueSuccess<List<EaseGroupProfile>>
    ) {
// Get group-related information based on the groupId list, return it through onValueSuccess(), and update the cache information.
    }
})
```

## Set the chat avatar and nickname


```kotlin

 // Use setUserProfileProvider to set profile for users in one-to-one chats, including the user avatar and nickname.
 EaseIM.setUserProfileProvider(object : EaseUserProfileProvider {
     override fun getUser(userId: String?): EaseProfile? {
         // TODO: Return the local user profile of the userId
         return DemoHelper.getInstance().getDataModel().getAllContacts()[userId]?.toProfile()
     }

     override fun fetchUsers(
         userIds: List<String>,
         onValueSuccess: OnValueSuccess<List<EaseProfile>>
     ) {
         // Provider. Users can obtain the Profile information of the corresponding ID from their own server according to the userId list and return it through onValueSuccess().
         // At the same time, the acquired information can be updated to the cache through EaseIM.updateUsersInfo(). When obtaining the Profile, UIKit will query from the cache first.
     }
 })
 // Use setGroupProfileProvider to set group profile.
 EaseIM.setGroupProfileProvider(object : EaseGroupProfileProvider {

    override fun getGroup(id: String?): EaseGroupProfile? {
        ChatClient.getInstance().groupManager().getGroup(id)?.let {
            return EaseGroupProfile(it.groupId, it.groupName, it.extension)
        }
        return null
    }

    override fun fetchGroups(
        groupIds: List<String>,
        onValueSuccess: OnValueSuccess<List<EaseGroupProfile>>
    ) {
        // Get group-related information based on the groupId list through onValueSuccess() and update the cached information.
    }
})
```

## Information processing logic

1. If the information has been cached in memory, when the page needs to display it, UIKit gets the cached data and render it. 

If no data is cached, you can obtain data from the local database or memory of the app, build the `EaseProfile` object and use the `getUser` method to return the `EaseProfile` object. Then the UIKit will use the `EaseProfile` object to update the information on the UI.   

1. If no data is obtained via the `getUser` method, the UIKit provider will get data from your server using the `fetchUsers` method:
   
   When the list page stops sliding, UIKit will first obtain user data from the cache, provide a list of user IDs with no cached data, and then query user information for such users from the server. You can build the `List<EaseProfile>` object. When the `fetchUsers` method is called, it will return data via `onValueSuccess(List<EaseProfile>)`. 

## Update cached information for UIKit

You can update the cached information using the `update` method provided by UIKit:

```kotlin
// Update current user information
EaseIM.updateCurrentUser(currentUserProfile)
// Update one-to-one chat user/group member information
EaseIM.updateUsersInfo(userProfileList)
```
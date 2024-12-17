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

 // Call setUserProfileProvider to set the user attributes for a one-to-one chat, including user avatar and nickname.
 EaseIM.setUserProfileProvider(object : EaseUserProfileProvider {
     override fun getUser(userId: String?): EaseProfile? {
         // Return the local user attributes corresponding to userId
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
 // Set group information through setGroupProfileProvider.
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

1. If the information has been cached in memory, when the page needs to display information, UIKit will first obtain the cached data from memory and render the page.

1. If there is no cached data, you can get the data from the app's local database or memory, build an `EaseProfile` object, and use the `getUser` method to return it. This way, UIKit will use the `EaseProfile` object to update the information in the UI.

1. If the data is not obtained through the `getUser` method, the UIKit provider will get the data from your server through the `fetchUsers` method: When the list page stops sliding, UIKit will first get the data from the cache, provide a list of user IDs for which there is no cached data, and query the data of these users from the server. You can construct a `List <EaseProfile> ` object, and when calling the `fetchUsers` method, the data will be returned through `onValueSuccess(List<EaseProfile>)`.

## Update cached information for UIKit

You can update the cached information using the `update` method provided by UIKit:

```kotlin
// Update current user information
EaseIM.updateCurrentUser(currentUserProfile)
// Update one-to-one chat user/group member information
EaseIM.updateUsersInfo(userProfileList)
```
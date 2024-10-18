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
         // Return the information corresponding to userId from the local query  // TODO：本地指本地数据库？
         // TODO: Return the user profile matching the userId from the local database
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
 // Use setGroupProfileProvider to set profile for users in group chats, including the user avatar and nickname.
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

1. If the cache is not there, UIKit calls the synchronization method to obtain information from the local app. You can obtain and provide the corresponding information from the local database or app memory. Once UIKit obtains the information, it renders the page and caches the information.

// TODO: 上面的一段改为这样？开发者从应用本地的数据库或者内存中获取数据，UIKit 通过 provider 的同步方法获取开发者提供的信息，并将信息缓存？If no data is cached, you can obtain data from the local database or memory of the app and provide the data to the UIKit. Once obtaining the data using the synchronous method of the provider, the UIKit renders the data on the page and caches it.   
// TODO：provider 同步方法，synchronous method of the provider.

1. If the data obtained by the synchronous method is empty, when the list page stops sliding, UIKit will do the following:

   1. Exclude the data provided by the cache and synchronous methods. 
   2. Return the data required for the items visible on the current page through the asynchronous method.

// TODO： If the data obtained with the synchronous method is empty, when the list page stops sliding, the UIKit returns the data required for the items visible on the current page through the asynchronous method of the provider.

   Once you obtain the corresponding information from the server, you can provide it to UIKit through `onValueSuccess`, enabling the UIKit to refresh and update the list.

## Update cached information for UIKit

You can update the cached information using the `update` method provided by UIKit:

```kotlin
// Update current user information
EaseIM.updateCurrentUser(currentUserProfile)
// Update one-to-one chat user/group member information
EaseIM.updateUsersInfo(userProfileList)
```
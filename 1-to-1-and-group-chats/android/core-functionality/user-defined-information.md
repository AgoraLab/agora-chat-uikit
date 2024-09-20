User information is used across UIKit and needs to be provided by developers. This page describes how developers can provide user information to UIKit.

## Logged-in user information

When a user calls the `EaseIM.login` method to log in, they need to pass in an `EaseProfile` object, which contains three properties: `id`, `name`, and `avatar`. `id` is a required parameter used to display the logged-in user's nickname and avatar. When sending a message, set the `name` and `avatar` properties to facilitate the display to other users.

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

UIKit provides an `EaseIM.setUserProfileProvider` interface to provide user information, including contact and group member information. The `EaseUserProfileProvider` interface is as follows:

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

UIKit provides an `EaseIM.setGroupProfileProvider` interface to provide group information. The `EaseGroupProfileProvider` interface is as follows:

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

## Information processing logic

1. If the information has been cached in memory, when the page needs to display it, UIKit gets the cached data. 

1. If the cache is not there, UIKit calls the synchronization method to obtain information from the local app. You can obtain and provide the corresponding information from the local database or app memory. Once UIKit obtains the information, it renders the page and caches the information.

1. If the data obtained by the synchronous method is empty, when the list page stops sliding, UIKit will do the following:

   1. Exclude the data provided by the cache and synchronous methods.
   1. Return the data required for the items visible on the current page through the asynchronous method.

   Once you obtain the corresponding information from the server, you can provide it to UIKit through `onValueSuccess`. Once UIKit receives the data, it refreshes and updates the list.

## Update UIKit cache information

You can update the cached information through the methods provided by UIKit:

```kotlin
// Update current user information
EaseIM.updateCurrentUser(currentUserProfile)
// Update one-to-one chat user/group member information
EaseIM.updateUsersInfo(userProfileList)
```
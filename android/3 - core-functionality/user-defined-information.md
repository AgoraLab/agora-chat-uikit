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

The usage is as follows:

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

UIKit provides an `EaseIM.setGroupProfileProvider`  interface to provide group information. 

## information processing logic

## Update UIKit cache information
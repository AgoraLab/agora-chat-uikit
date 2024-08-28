Before using UIKit, you need to integrate it into your app. This page explains the necessary steps. 

## Prerequisites

Before you start, make sure your development environment meets the following conditions:

- Android Studio 4.0 and above
- Gradle 4.10.x and above
- targetVersion 26 and above
- Android SDK API 21 and above
- JDK 11 and above

## Integrate UIKIt

Take the following steps to integrate UIKit:

1. Add remote dependency

   Add the following dependencies in the app project build.gradle.kts:

   ```kotlin
   implementation("io.hyphenate:ease-chat-kit:4.7.0")
   ```

1. Add local dependencies

   Download UIKit from the [GitHub repository](https://github.com/easemob/chatuikit-android) and integrate it as follows:

   1. Add the following code to the `/Gradle Scripts/settings.gradle.kts` file:

   ```kotlin
   include(":ease-im-kit")
   project(":ease-im-kit").projectDir = File("../chatuikit-android/ease-im-kit")
   ```
   
   1. Add the following code to the `/Gradle Scripts/build.gradle` file:

   ```kotlin
   //chatuikit-android
   implementation(project(mapOf("path" to ":ease-im-kit")))
   ```

1. Prevent code obfuscation

   Add the following line to `app/proguard-rules.pro`:

   ```kotlin
   -keep class com.hyphenate.** {*;}
   -dontwarn  com.hyphenate.**
   ```


## Build a page

### Create a chat page

Use either of the following: 

- The `EaseChatActivity#actionStart` method of the `EaseChatActivity` page. The sample code is as follows:

   ```kotlin
   // conversationId: Peer user ID for a one-to-one conversation and group ID for a chat group
   // chatType: EaseChatType#SINGLE_CHAT for one-to-one chat and EaseChatType#GROUP_CHAT for chat group
   EaseChatActivity.actionStart(mContext, conversationId, chatType)
   ```

   The `EaseChatActivity` page mainly requests permissions, such as camera permissions, voice permissions, and other.

- `useEaseChatFragment`. The sample code is as follows:

   ```kotlin
   class ChatActivity: AppCompactActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_chat)
        // conversationID: Peer user ID for a one-to-one chat and group ID for a chat group
        // chatType can be EaseChatType#SINGLE_CHAT or EaseChatType#GROUP_CHAT
        EaseChatFragment.Builder(conversationId, chatType)
                        .build()?.let { fragment ->
                            supportFragmentManager.beginTransaction()
                                .replace(R.id.fl_fragment, fragment).commit()
                        }
       }
   }
   ```

### Create a conversation list page

UIKit provides `EaseConversationListFragment` that can be used by adding it to Activity. The sample code is as follows:

```kotlin
class ConversationListActivity: AppCompactActivity() {
   override fun onCreate(savedInstanceState: Bundle?) {
      super.onCreate(savedInstanceState)
      setContentView(R.layout.activity_conversation_list)

      EaseConversationListFragment.Builder()
              .build()?.let { fragment ->
                 supportFragmentManager.beginTransaction()
                         .replace(R.id.fl_fragment, fragment).commit()
              }
   }
}
```


### Create a contact list page

UIKit provides `EaseContactsListFragment` that can be used by adding it to Activity. The sample code is as follows:

```kotlin
class ContactListActivity: AppCompactActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_contact_list)

        EaseContactsListFragment.Builder()
                        .build()?.let { fragment ->
                            supportFragmentManager.beginTransaction()
                                .replace(R.id.fl_fragment, fragment).commit()
                        }
    }
}
```



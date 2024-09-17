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
   implementation("io.hyphenate:agora-chat-kit:4.7.0")
   ```

1. Add local dependencies

   Download UIKit from the [GitHub repository](https://github.com/easemob/chatuikit-android) and integrate it as follows:

   1. Add the following code to the `/Gradle Scripts/settings.gradle.kts` file:

   ```kotlin
   include(":chat-im-kit")
   project(":chat-im-kit").projectDir = File("../chatuikit-android/chat-im-kit")
   ```
   
   1. Add the following code to the `/Gradle Scripts/build.gradle` file:

   ```kotlin
   //chatuikit-android
   implementation(project(mapOf("path" to ":chat-im-kit")))
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

- The `AgoraChatActivity#actionStart` method of the `AgoraChatActivity` page. The sample code is as follows:

   ```kotlin
   // conversationId: Peer user ID for a one-to-one conversation and group ID for a group chat
   // chatType: AgoraChatType#SINGLE_CHAT for one-to-one chat and AgoraChatType#GROUP_CHAT for group chat
   AgoraChatActivity.actionStart(mContext, conversationId, chatType)
   ```

   The `AgoraChatActivity` page mainly requests permissions, such as camera permissions, voice permissions, and other.

- `useAgoraChatFragment`. The sample code is as follows:

   ```kotlin
   class ChatActivity: AppCompactActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_chat)
        // conversationID: Peer user ID for a one-to-one chat and group ID for a group chat
        // chatType can be AgoraChatType#SINGLE_CHAT or AgoraChatType#GROUP_CHAT
        AgoraChatFragment.Builder(conversationId, chatType)
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



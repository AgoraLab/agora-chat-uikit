# Integrate UIKit

Before using UIKit, you need to integrate it into your app. This page explains the necessary steps. 

## Development Environment

- Android Studio Flamingo | 2022.2.1 or later
- Gradle 8.0 or later
- TargetVersion 26 or later
- Android SDK API 21 or later
- JDK 17 or later

## Installation

The UIKit can be integrated with Gradle and module source code.

### Integrate with Gradle

#### Gradle before 7.0

Add the Maven remote repository in `build.gradle` or `build.gradle.kts` in the root directory of the project.

```kotlin
buildscript {
    repositories {
        ...
        mavenCentral()
    }
}
allprojects {
    repositories {
        ...
        mavenCentral()
    }
}
```

#### Gradle later than 7.0

Add the Maven remote repository in `settings.gradle` or `settings.gradle.kts` in the root directory of the project.

```kotlin
pluginManagement {
    repositories {
        ...
        mavenCentral()
    }
}
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        ...
        mavenCentral()
    }
}
```

### Module remote dependency

Add the following dependency to `build.gradle.kts` of the app project,where `x.y.z` indicates the [latest version](https://central.sonatype.com/artifact/io.agora.rtc/chat-uikit/versions):

```kotlin

implementation("io.agora.rtc:chat-uikit:x.y.z")

```

### Integrate with the Module source code

Acquire the Chat UIKit source code from the [GitHub repository](https://github.com/AgoraIO-Usecase/AgoraChat-UIKit-android/tree/dev-kotlin) and integrate it in the following way:

1. Add the following code in the `settings.gradle.kts` file Project/settings.gradle.kts(Project Settings) in the root directory.

```kotlin
include(":chat-uikit")
project(":chat-uikit").projectDir = File("../AgoraChat-UIKit-android/ease-im-kit")
```

2. Add the following code in `build.gradle.kts` app/build.gradle(Module: app).

```kotlin
//chat-uikit
implementation(project(mapOf("path" to ":chat-uikit")))
```

### Prevent code obfuscation

Add the following lines to `app/proguard-rules.pro` to prevent code obfuscation.

```kotlin
-keep class io.agora.** {*;}
-dontwarn  io.agora.**
```
   
## Build a page

Take the following steps to build a page.

### Create a chat page

Use either of the following: 

- The `UIKitChatActivity#actionStart` method of the `UIKitChatActivity` page. The sample code is as follows:

   ```kotlin
   // conversationId: Peer user ID for a one-to-one conversation and group ID for a group chat
   // chatType: ChatUIKitType#SINGLE_CHAT for one-to-one chat and ChatUIKitType#GROUP_CHAT for group chat
   UIKitChatActivity.actionStart(mContext, conversationId, chatType)
   ```

   The `UIKitChatActivity` page requests permissions, such as camera permissions, voice permissions, and other.

- The `UIKitChatFragment`. The sample code is as follows:

   ```kotlin
   class ChatActivity: AppCompactActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_chat)
        // conversationID: Peer user ID for a one-to-one chat and group ID for a group chat
        // chatType: ChatUIKitType#SINGLE_CHAT or ChatUIKitType#GROUP_CHAT
        UIKitChatFragment.Builder(conversationId, chatType)
                        .build()?.let { fragment ->
                            supportFragmentManager.beginTransaction()
                                .replace(R.id.fl_fragment, fragment).commit()
                        }
       }
   }
   ```

### Create a conversation list page

UIKit provides `ChatUIKitConversationListFragment` that can be added to Activity. The sample code is as follows:

```kotlin
class ConversationListActivity: AppCompactActivity() {
   override fun onCreate(savedInstanceState: Bundle?) {
      super.onCreate(savedInstanceState)
      setContentView(R.layout.activity_conversation_list)

      ChatUIKitConversationListFragment.Builder()
              .build()?.let { fragment ->
                 supportFragmentManager.beginTransaction()
                         .replace(R.id.fl_fragment, fragment).commit()
              }
   }
}
```

### Create a contact list page

UIKit provides `ChatUIKitContactsListFragment` that can be used by adding it to Activity. The sample code is as follows:

```kotlin
class ContactListActivity: AppCompactActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_contact_list)

        ChatUIKitContactsListFragment.Builder()
                        .build()?.let { fragment ->
                            supportFragmentManager.beginTransaction()
                                .replace(R.id.fl_fragment, fragment).commit()
                        }
    }
}
```



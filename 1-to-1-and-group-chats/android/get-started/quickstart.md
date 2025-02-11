# Quickstart

With UIKit, you can easily implement messaging in one-to-one and group chats. This page explains how do this.

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

## Implementation

Take the following steps to send a message to a one-to-one or group chat.

1. Create a quick start page. 

   1. Open the `app/res/values/strings.xml` file and replace the content with the following:

      ```xml
      <resources>
         <string name="app_name">quickstart</string>
       
         <string name="app_key">[Your app key]</string>
      </resources>
      ```

      Replace `app_key` with your app key.

   2. Open the `app/res/layout/activity_main.xml` file and replace the content with the following:

      ```xml
      <?xml version="1.0" encoding="utf-8"?>
      <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:tools="http://schemas.android.com/tools"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:orientation="vertical"
      tools:context=".MainActivity">
     
          <EditText
              android:id="@+id/et_userId"
              android:layout_width="match_parent"
              android:layout_height="50dp"
              android:layout_margin="20dp"
              android:hint="UserId"/>
     
          <EditText
              android:id="@+id/et_password"
              android:layout_width="match_parent"
              android:layout_height="50dp"
              android:layout_margin="20dp"
              android:hint="Password"/>
     
          <Button
              android:id="@+id/btn_login"
              android:layout_width="match_parent"
              android:layout_height="50dp"
              android:layout_margin="20dp"
              android:onClick="login"
              android:text="Login"/>
     
          <Button
              android:id="@+id/btn_logout"
              android:layout_width="match_parent"
              android:layout_height="50dp"
              android:layout_margin="20dp"
              android:onClick="logout"
              android:text="Logout"/>
     
          <EditText
              android:id="@+id/et_peerId"
              android:layout_width="match_parent"
              android:layout_height="50dp"
              android:layout_margin="20dp"
              android:hint="PeerId"/>
     
          <Button
              android:id="@+id/btn_chat"
              android:layout_width="match_parent"
              android:layout_height="50dp"
              android:layout_margin="20dp"
              android:onClick="startChat"
              android:text="Start Chat"/>
     
      </LinearLayout>
     ```

2. Implement the logic.

   1. Implement the login and logout pages.

      If you have integrated Chat SDK, all user IDs can be used to log into the UIKit. Open the `MainActivity` file and replace the code with the following:

       ```kotlin
       package io.agora.quickstart

         import androidx.appcompat.app.AppCompatActivity
         import android.os.Bundle
         import android.view.View
         import io.agora.quickstart.databinding.ActivityMainBinding
         import io.agora.chat.uikit.ChatUIKitClient
         import io.agora.chat.uikit.common.ChatLog
         import io.agora.chat.uikit.common.ChatOptions
         import io.agora.chat.uikit.common.extensions.showToast
         import io.agora.chat.uikit.feature.chat.enums.ChatUIKitType
         import io.agora.chat.uikit.feature.chat.activities.UIKitChatActivity
         import io.agora.chat.uikit.interfaces.ChatUIKitConnectionListener
            
         class MainActivity : AppCompatActivity() {
         private val binding: ActivityMainBinding by lazy { ActivityMainBinding.inflate(layoutInflater) }
            
             private val connectListener by lazy {
                 object : ChatUIKitConnectionListener() {
                     override fun onConnected() {}
            
                     override fun onDisconnected(errorCode: Int) {}
            
                     override fun onLogout(errorCode: Int, info: String?) {
                         super.onLogout(errorCode, info)
                         showToast("You have been logged out, please log in again!")
                         ChatLog.e(TAG, "")
                     }
                 }
             }
             override fun onCreate(savedInstanceState: Bundle?) {
                 super.onCreate(savedInstanceState)
                 setContentView(binding.root)
                 initSDK()
                 initListener()
             }
            
             private fun initSDK() {
                 val appkey = getString(R.string.app_key)
                 if (appkey.isEmpty()) {
                     applicationContext.showToast("You should set your AppKey first!")
                     ChatLog.e(TAG, "You should set your AppKey first!")
                     return
                 }
                 ChatOptions().apply {
                     // Set your own appkey here
                     this.appKey = appkey
                     // Set not to log in automatically
                     this.autoLogin = false
                     // Set whether confirmation of delivery is required by the recipient. Default: false
                     this.requireDeliveryAck = true
                 }.let {
                     ChatUIKitClient.init(applicationContext, it)
                 }
             }
            
             private fun initListener() {
                 ChatUIKitClient.addConnectionListener(connectListener)
             }
            
             fun login(view: View) {
                 val username = binding.etUserId.text.toString().trim()
                 val password = binding.etPassword.text.toString().trim()
                 if (username.isEmpty() || password.isEmpty()) {
                     showToast("Username or password cannot be empty!")
                     ChatLog.e(TAG, "Username or password cannot be empty!")
                     return
                 }
                 if (!ChatUIKitClient.isInited()) {
                     showToast("Please init first!")
                     ChatLog.e(TAG, "Please init first!")
                     return
                 }
                 ChatUIKitClient.login(username, password
                     , onSuccess = {
                         showToast("Login successfully!")
                         ChatLog.e(TAG, "Login successfully!")
                     }, onError = { code, message ->
                         showToast("Login failed: $message")
                         ChatLog.e(TAG, "Login failed: $message")
                     }
                 )
             }
            
             fun logout(view: View) {
                 if (!ChatUIKitClient.isInited()) {
                     showToast("Please init first!")
                     ChatLog.e(TAG, "Please init first!")
                     return
                 }
                 ChatUIKitClient.logout(false
                     , onSuccess = {
                         showToast("Logout successfully!")
                         ChatLog.e(TAG, "Logout successfully!")
                     }
                 )
             }
            
             fun startChat(view: View) {
                 val username = binding.etPeerId.text.toString().trim()
                 if (username.isEmpty()) {
                     showToast("Peer id cannot be empty!")
                     ChatLog.e(TAG, "Peer id cannot be empty!")
                     return
                 }
                 if (!ChatUIKitClient.isLoggedIn()) {
                     showToast("Please login first!")
                     ChatLog.e(TAG, "Please login first!")
                     return
                 }
                 UIKitChatActivity.actionStart(this, username, ChatUIKitType.SINGLE_CHAT)
             }
            
             override fun onDestroy() {
                 ChatUIKitClient.removeConnectionListener(connectListener)
                 ChatUIKitClient.releaseGlobalListener()
                 super.onDestroy()
             }
            
             companion object {
                 private const val TAG = "MainActivity"
             }
         }
       ```

      2. Click **Sync Project with Gradle Files**. You can now test your application.

3. Send a message.

   Type your message at the bottom of the chat page and click **Send**.

## Project test

1. In Android Studio, click **Run app** to run the app on your device or emulator.
1. Enter the user ID and password and click **Login**. There will be a `Toast` prompt whether the login was
   successful or failed. You can also view it through Logcat.
1. Log in to another account on another device or simulator.
2. Click **Start Chat** to start chatting.


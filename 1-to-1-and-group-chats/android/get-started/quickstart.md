# Quickstart

With UIKit, you can easily implement messaging in one-to-one chats and group chats. This page explains how do this for a one-to-one chat.

## Prerequisites

Before you start, make sure your development environment meets the following conditions:

- Android Studio 4.0 and above;
- Gradle 4.10.x and above;
- targetVersion 26 and above;
- Android SDK API 21 and above;
- JDK 11 and above;
- A valid Agora project with users and tokens generated. See [Enable and configure Chat](https://docs.agora.io/en/agora-chat/get-started/enable) and [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication) for details. 

## Project setup

Set up your environment in the following way:

1. [Create a new project](https://developer.android.com/studio/projects/create-project) in Android Studio.

    - In the **Phone and Tablet** tab, select the **Empty Views Activity**.
    - For **Minimum SDK**, select **API 21: Android 5.0 (Lollipop)**.
    - Select **Kotlin** for **Language**.

   Once the project is created successfully, make sure it is synchronized.

1. Check whether the project has the **MavenCentral** repository.

    - Before Gradle 7.0:

      In the `/Gradle Scripts/build.gradle.kts(Project: <projectname>)` file, check if there is a **MavenCentral**
      repository:

        ```kotlin
        buildscript {
           repositories {
               mavenCentral()
           }
        }
        ```

    - Gradle 7.0 and later

      In the `/Gradle Scripts/settings.gradle.kts(Project Settings)` file, check if there is a **MavenCentral**
      repository:

        ```kotlin
        dependencyResolutionManagement {
           repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
           repositories {
               mavenCentral()
           }
        }             
        ```

1. Introduce UIKit into the project.

    - Add the remote dependencies in the app project `build.gradle.kts`:

       ```kotlin
       implementation("io.hyphenate:ease-chat-kit:4.7.0")
       ```

    - Add local dependencies:

        1. Get the [UIKit source code](https://github.com/easemob/chatuikit-android).
        1. Add the following code to the `/Gradle Scripts/settings.gradle.kts` file:

           ```kotlin
           include(":chat-im-kit")
           project(":chat-im-kit").projectDir = File("../chatuikit-android/chat-im-kit")
           ```
        1. Add the following code to the `/Gradle Scripts/build.gradle.kts` file:

           ```kotlin
           //chatuikit-android
           implementation(project(mapOf("path" to ":chat-im-kit")))
           ```
1. Prevent code obfuscation.

   Add the following code to the `/Gradle Scripts/proguard-rules.pro` file:

    ```kotlin
    -keep class com.hyphenate.** {*;}
        -dontwarn  com.hyphenate.**
    ```

## Implementation

Take the following steps to send a message to a one-to-one or group chat. // 下面这个是发送单聊消息吧，因此需要删除 "or group"

1. Create a quick start page.// TODO：可以说创建快速开始页面吗？是创建单聊页面用于快速开始？

    1. Open the `app/res/values/strings.xml` file and replace the content with the following:

       ```xml
       <resources>
          <string name="app_name">quickstart</string>
       
          <string name="app_key">[Your app key]</string>
       </resources>
       ```

       Replace `app_key` with your app key.

    1. Open the `app/res/layout/activity_main.xml` file and replace the content with the following:

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

1. Implement the logic.

      1. Implement the login and logout pages.

         If you have integrated Chat SDK, all user IDs can be used to log into the UIKit. Open the `MainActivity` file and replace the code with the following:

          ```kotlin
          package com.easemob.quickstart
   
          import android.content.Context
          import androidx.appcompat.app.AppCompactActivity
          import android.os.Bundle
          import android.view.View
          import android.widget.Toast
          import com.easemob.quickstart.databinding.ActivityMainBinding
          import com.hyphenate.chatui.EaseIM
          import com.hyphenate.chatui.common.ChatConnectionListener
          import com.hyphenate.chatui.common.ChatLog
          import com.hyphenate.chatui.common.ChatOptions
          import com.hyphenate.chatui.feature.messages.EaseChatType
          import com.hyphenate.chatui.feature.messages.activities.EaseChatActivity
          import kotlinx.coroutines.CoroutineScope
          import kotlinx.coroutines.Dispatchers
          import kotlinx.coroutines.launch
   
          class MainActivity : AppCompactActivity(), ChatConnectionListener {
             private val binding: ActivityMainBinding by lazy { ActivityMainBinding.inflate(layoutInflater) }
             override fun onCreate(savedInstanceState: Bundle?) {
                super.onCreate(savedInstanceState)
                setContentView(binding.root)
                initSDK()
                initListener()
             }
   
             private fun initSDK() {
                val appkey = getString(R.string.app_key)
                if (appkey.isNullOrEmpty()) {
                   showToast("You should set your AppKey first!")
                   ChatLog.e(TAG, "You should set your AppKey first!")
                   return
                }
                ChatOptions().apply {
                   //Set your own app key
                   this.appKey = appkey
                   // Set to manual login
                   this.autoLogin = false
                   //Set whether the receiver is required to send a delivery receipt. The default is `false`, which is not required.
                   this.requireDeliveryAck = true
                }.let {
                   EaseIM.init(applicationContext, it)
                }
             }
             private fun initListener() {
                EaseIM.subscribeConnectionDelegates(this)
             }
   
             fun login(view: View) {
                val username = binding.etUserId.text.toString().trim()
                val password = binding.etPassword.text.toString().trim()
                if (username.isEmpty() || password.isEmpty()) {
                   showToast("Username or password cannot be empty!")
                   ChatLog.e(TAG, "Username or password cannot be empty!")
                   return
                }
                if (!EaseIM.isInited()) {
                   showToast("Please init first!")
                   ChatLog.e(TAG, "Please init first!")
                   return
                }
                EaseIM.login(username, password
                        , onSuccess = {
                   showToast("Login successful!")
                   ChatLog.e(TAG, "Login successful!")
                }, onError = { code, message ->
                   showToast("Login failed: $message")
                   ChatLog.e(TAG, "Login failed: $message")
                }
                )
             }
             fun logout(view: View) {
                if (!EaseIM.isInited()) {
                   showToast("Please init first!")
                   ChatLog.e(TAG, "Please init first!")
                   return
                }
                EaseIM.logout(false
                        , onSuccess = {
                   showToast("Logout successful!")
                   ChatLog.e(TAG, "Logout successful!")
                }
                )
             }
          // Jump to the chat page
             fun startChat(view: View) {
                val username = binding.etPeerId.text.toString().trim()
                if (username.isEmpty()) {
                   showToast("Peer id cannot be empty!")
                   ChatLog.e(TAG, "Peer id cannot be empty!")
                   return
                }
                if (!EaseIM.isLoggedIn()) {
                   showToast("Please log in first!")
                   ChatLog.e(TAG, "Please log in first!")
                   return
                }
                EaseChatActivity.actionStart(this, username, EaseChatType.SINGLE_CHAT)
             }
   
             override fun onConnected() {}
   
             override fun onDisconnected(errorCode: Int) {}
   
             override fun onLogout(errorCode: Int, info: String?) {
                super.onLogout(errorCode, info)
                showToast("You have been logged out, please log in again!")
                ChatLog.e(TAG, "")
             }
   
             override fun onDestroy() {
                super.onDestroy()
                EaseIM.unsubscribeConnectionDelegates(this)
             }
   
             companion object {
                private const val TAG = "MainActivity"
             }
          }
   
          fun Context.showToast(msg: String) {
             CoroutineScope(Dispatchers.Main).launch {
                Toast.makeText(this@showToast, msg, Toast.LENGTH_SHORT).show()
             }
          }
          ```

      1. Click **Sync Project with Gradle Files**. You can now test your application.

1. Send a message.

   Type your message at the bottom of the chat page and click **Send**.

## Project test

1. In Android Studio, click **Run app** to run the app on your device or emulator.
1. Enter the user ID and password and click **Login**. There will be a `Toast` prompt whether the login was
   successful or failed. You can also view it through Logcat.
1. Log in to another account on another device or simulator.
1. Click **Start** to start chatting. // TODO：是 Start 还是 Start Chat?


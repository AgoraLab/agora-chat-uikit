# Quickstart

With UIKit, you can easily implement user interaction in a chat room. This page describes how to implement sending chat room messages.

## Prerequisites

- Android Studio Arctic Fox (2020.3.1) or above;
- Android API level 21 or above;
- Developed using Kotlin language, version 1.5.21 or above;
- JDK 1.8 or above;
- A valid Agora account;
- An app key and a user token generated on your app server. See [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication).

## Implementation

Take the following steps to implement message sending:

1. Create a project and import the `ChatroomUIKit` module

    1. Open your project in Android Studio.
    1. Select **File** > **Module import**.
    1. Add local module dependencies.

   Find the downloaded `ChatroomUIKit` module and add it as a local dependency. Then import the `ChatroomService` module into the project:

    ```kotlin
    // settings.gradle
    include ':ChatroomUIKit'
    include ':ChatroomService'
    project(':ChatroomUIKit').projectDir = new File('../ChatroomUIKit/ChatroomUIKit')
    project(':ChatroomService').projectDir = new File('../ChatroomUIKit/ChatroomService')
    
    // app/build.gradle
    dependencies {
      implementation(project(mapOf("path" to ":ChatroomUIKit")))
    }
    ```
   
1. Initialize `ChatroomUIKit`.

   You can initialize it when your app loads or before using UIKit.

   ```kotlin
         class ChatroomApplication : Application() {
           
              override fun onCreate() {
                super.onCreate()
           
                ChatroomUIKitClient.getInstance().setUp(this, "Your AppKey")
           
              }
            }
   ```

1. Log in to `ChatroomUIKit`.

   If you have integrated Chat SDK, the corresponding user IDs can be used to log into UIKit. 
  
   ```kotlin
         ChatroomUIKitClient.getInstance().login("userId", "token")
   ```

1. Create a chat room view

   To create a chat room view, follow these steps:

   1. Get the chat room list and join the specified chat room.

   1. Load the chat room view `ComposeChatroom`, passing in the chat room ID and the user ID of the chat room owner.

   1. [Add chat room members](https://docs.agora.io/en/agora-chat/restful-api/chatroom-management/manage-chatroom-members#adding-a-chat-room-member).

1. Send a message

   Enter the message content at the bottom of the screen and click **Send** to send the message.

   ![Send a message](../assets/images/click_chat.png)
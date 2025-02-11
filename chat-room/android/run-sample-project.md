# Run a sample project

Agora provides an open-source sample project, which demonstrates the use of UIKit for building a chat room and implementing the logic. This page explains how to compile and run it. 

## Prerequisites

Before you start, make sure your development environment meets the following requirements:

- Android Studio Arctic Fox (2020.3.1) or above;
- Android API level 21 or above;
- Developed using Kotlin language, version 1.5.21 or above;
- JDK 1.8 or above;
- Gradle 7.0.0 or above;
- A valid Agora project with users and tokens generated. See [Enable and configure Chat](https://docs.agora.io/en/agora-chat/get-started/enable) and [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication) for details. 

## Implementation

Take the following steps to compile and run the sample project:

1. Download [the sample code](https://github.com/AgoraIO-Usecase/AgoraChat-UIKit-Chatroom-android).

1. Add UIKit module dependency:

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
   
1. Compile the project

   1. Initialize `ChatroomUIKit`:

      ```kotlin
      class ChatroomApplication : Application() {
        
           override fun onCreate() {
             super.onCreate()
        
             ChatroomUIKitClient.getInstance().setUp(this, "Your AppKey")
        
           }
         }
      ```

   1. Log in to `ChatroomUIKit`:

      ```kotlin
      ChatroomUIKitClient.getInstance().login("userId", "token")
      ```
      

   1. Load the `ComposeChatroom` view, passing in the `roomId` and the room owner's `UserEntity` object.

      ```kotlin
      class ChatroomActivity : ComponentActivity(){
      	override fun onCreate(savedInstanceState: Bundle?) {
      		super.onCreate(savedInstanceState)
      		setContent {
      			ComposeChatroom(roomId = roomId,roomOwner = ownerInfo)
      		}
      	}
      }
      ```
      
1. Run and test the project

   Note that the sample project is only used to quickly launch the chat room. Multi-member interaction testing is currently not available.
# Integrate UIKit

Before using UIKit, you need to integrate it into your app. This page explains the necessary steps. 

## Prerequisites

Before you start, make sure your development environment meets the following conditions:

- Flutter 3.19.0 or above;
- iOS 12 or above or Android 23 or above;
- A valid Agora project with users and tokens generated. See [Enable and configure Chat](https://docs.agora.io/en/agora-chat/get-started/enable) and [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication) for details. 

## Integrate UIKIt

Take the following steps to integrate UIKit:

1. Install UIKit:

   ```
   flutter pub add agora_chat_uikit
   ```

2. Add permissions.

   Because UIKit uses the feature of sending images and audio, you need to enable the permissions to record, use camera, and access the image library.

   - iOS: Add the following permissions in `<project root>/ios/Runner/Info.plist`:

     ```xml
     NSPhotoLibraryUsageDescription
     NSCameraUsageDescription
     NSMicrophoneUsageDescription
     ```
   
   - Android: The following permissions have been added in `AndroidManifest.xml`, you don't need to add them again:

     ```xml
       <uses-permission android:name="android.permission.CAMERA" />
       <uses-permission android:name="android.permission.RECORD_AUDIO" />
       <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
     ```
   
3. Introduce the header file:

    ```dart
    // Import the header file
    import 'package:agora_chat_uikit/chat_uikit.dart';
    ```

4. Initialize UIKit.

   Initialize Chat SDK when the app starts:

   ```dart
   void main() {
     ChatUIKit.instance
         .init(options: Options(appKey: appkey, autoLogin: false))
         .then((value) {
       runApp(const MyApp());
     });
   }
   ```

5. Log in with the user ID and token:

      ```dart
      ChatUIKit.instance.loginWithToken(userId: userId, token: token);
      ```

6. Build the interface.

   UIKit provides components such as a conversation list, a chat, group settings, and a contact list. You can use these components to build the interface. These components support customization. For details, refer to the documentation of each corresponding component.
   
   The following is an example of an interface consisting of a conversation list and a chat component:

   ```dart
   import 'package:agora_chat_uikit/chat_uikit.dart';
   import 'package:flutter/material.dart';
   
   class Conversations extends StatefulWidget {
     const Conversations({super.key});
   
     @override
     State<Conversations> createState() => _ConversationsState();
   }
   
   class _ConversationsState extends State<Conversations> {
     @override
     Widget build(BuildContext context) {
       return const ConversationsView();
     }
   }
   ```
   
7. Package the app.

   When packaging for Android, add the following code to the `project/android/app/proguard-rules.pro` file. If this file does not exist, create it with the same name:

    ```dart
    -keep class com.hyphenate.** {*;}
    -dontwarn  com.hyphenate.**
    ```
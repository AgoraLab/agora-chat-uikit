# Quickstart

With UIKit, you can easily implement messaging in one-to-one chats and group chats. This page explains how do this for a one-to-one chat.

UIKit for Flutter supports iOS and Android. 

## Prerequisites

Before you start, make sure your development environment meets the following conditions:

- Flutter v3.19.0 and above;
- Permissions granted in the following way:

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

- A valid Agora project with users and tokens generated. See [Enable and configure Chat](https://docs.agora.io/en/agora-chat/get-started/enable) and [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication) for details. 

## Implementation

1. Create a new project:

    ```
    flutter create chat_uikit_demo --platforms=android,ios
    ```

1. Add dependencies.

    Enter the project directory and add the latest `agora_chat_uikit` version:

    ```
    cd chat_uikit_demo
    flutter pub add agora_chat_uikit
    flutter pub get
    ```
   

2. Initialize UIKit.

   Initialize UIKit, replace `appkey` with your own app key:

   ```dart
    // Import header file
    import 'package:agora_chat_uikit/chat_uikit.dart';
    ...
    
    void main() {
      ChatUIKit.instance
          .init(options: Options(appKey: appkey, autoLogin: false))
          .then((value) {
        runApp(const MyApp());
      });
    }
   ```

2. Log in with the user ID and token:

      ```dart
      ChatUIKit.instance.loginWithToken(userId: userId, token: token);
      ```

3. Add a chat page.

   UIKit provides `MessagesView` to display the chat page after a successful login:

    ```dart
      @override
      Widget build(BuildContext context) {
        /// userId: Receiver's user ID
        return MessagesView(profile: ChatUIKitProfile.contact(id: userId));
      }
    ```

4.  Send the first message.

    Type your message at the bottom of the chat page and click **Send**.

## Reference

The complete code to quickly start the entire process is as follows: 

```dart
import 'package:agora_chat_uikit/chat_uikit.dart';
import 'package:flutter/material.dart';

const appkey = '';
const currentUserId = '';
const currentUserToken = '';
const chatterId = '';
void main() {
  ChatUIKit.instance
      .init(options: Options(appKey: appkey, autoLogin: false))
      .then((value) {
    runApp(const MyApp());
  });
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
      ),
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            TextButton(
              onPressed: () {
                if (ChatUIKit.instance.isLogged()) {
                  ChatUIKit.instance.logout().then((value) => setState(() {}));
                } else {
                  ChatUIKit.instance
                      .loginWithToken(
                          userId: currentUserId, token: currentUserToken)
                      .then((value) => setState(() {}));
                }
              },
              child: ChatUIKit.instance.isLogged()
                  ? const Text('Logout')
                  : const Text('Login'),
            ),
            if (ChatUIKit.instance.isLogged())
              const Expanded(child: ChatPage()),
          ],
        ),
      ),
    );
  }
}

class ChatPage extends StatefulWidget {
  const ChatPage({super.key});

  @override
  State<ChatPage> createState() => _ChatPageState();
}

class _ChatPageState extends State<ChatPage> {
  @override
  Widget build(BuildContext context) {
    return MessagesView(profile: ChatUIKitProfile.contact(id: chatterId));
  }
}
```
# Run a sample project

Agora provides an open-source sample project, which demonstrates the use of UIKit for building a chat room and implementing the logic. This page explains how to compile and run it. 

## Prerequisites

Before you start, make sure your development environment meets the following requirements:

- Agora Chat SDK (included);
- Flutter 3.3.0 or above;
- For iOS: Xcode 13 or above, iOS 11 or above. For Android: minSDKVersion 21;
- A valid Agora project with users and tokens generated. See [Enable and configure Chat](https://docs.agora.io/en/agora-chat/get-started/enable) and [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication) for details. 

## Implementation

You can use the `example` project for demonstration. The `example` folder contains sample projects. You can download the source code, compile it, and then run it to experience it.

Take the following steps to compile and run the sample project:

1. [Download the sample code](https://github.com/AgoraIO-Usecase/AgoraChat-UIKit-Chatroom-flutter/archive/refs/heads/main.zip) or run the following command:

   ```
   git clone git@github.com:AgoraIO-Usecase/AgoraChat-UIKit-Chatroom-flutter.git
   ```

1. Initialize the project:

    1. Run the `flutter pub get` command in the `example` directory.
    1. Modify `example/lib/main.dart` with your app key.
   
1. Run the project in VScode or another IDE. 


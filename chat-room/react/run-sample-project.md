# Run a sample project

Agora provides an open-source sample project, which demonstrates the use of UIKit for building a chat room and implementing the logic. This page explains how to compile and run it. 

## Prerequisites

Before you start, make sure your development environment meets the following requirements:

- MacOS 12 or above;
- React-Native 0.66 or above;
- NodeJs 16.18 or above;
- iOS apps: Xcode 13 or above, and its dependencies.
- Android apps: Android Studio 2021 or above, and its dependent tools.
- A valid Agora project with users and tokens generated. See [Enable and configure Chat](https://docs.agora.io/en/agora-chat/get-started/enable) and [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication) for details. 

## Implementation

Take the following steps to compile and run the sample project:

1. Install UIKit:

   ```typescript
   npm install react-native-chat-room 
   // or     
   yarn add react-native-chat-room
   // or
   npx expo install react-native-chat-room
   ``` 

1. Download the sample project.

   The `example` folder contains the sample project. You can download, compile, and run it.

   - Download the source code repository:

     ```
     git clone https://github.com/agora/rncr/react-native-chat-room
     ```
     
   - Alternatively download the source zip file:

     ```
     curl -L -o file.zip  https://github.com/AsteriskZuo/react-native-chat-room/archive/refs/heads/main.zip
     ```
   
1. Initialize the project

   1. Run `yarn & yarn env` in the project root directory. 
   
   1. In the generated `example/src/env.ts` file, modify the necessary configuration items.
   
      - For iOS: Run `pod install`.
      - For Android: Run `gradle sync`.
      
1. Run the project

  Run the following command in the `react-native-chat-room/tree/main/example` directory:

  ```
  yarn run ios
  // or
  yarn run android
  ```
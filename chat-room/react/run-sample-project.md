# Run a sample project

Agora provides an open-source sample project, which demonstrates the use of UIKit for building a chat room and implementing the logic. This page explains how to compile and run it. 

## Prerequisites

Before you start, make sure your development environment meets the following requirements:

- MacOS 12 or above;
- React-Native 0.66 or above;
- NodeJs 18.16 or above;
- iOS apps: Xcode 13 or above, and its dependencies.
- Android apps: Android Studio 2021 or above, and its dependent tools.
- A valid Agora project with users and tokens generated. See [Enable and configure Chat](https://docs.agora.io/en/agora-chat/get-started/enable) and [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication) for details. 

## Implementation

Take the following steps to compile and run the sample project:

1. Download the sample project.

   The `examples/room-example` folder contains the sample project. You can download, compile, and run it.

   - Download the source code repository:

     ```
     git clone https://github.com/AgoraIO-Usecase/AgoraChat-UIKit-rn
     ```
   
2. Initialize the project

   1. Run `yarn & yarn-prepack` in the project root directory. 
   
   2. In the file `examples/room-example/src/env.ts`, set `appKey` parameters.
   
      - For iOS: Run `pod install`.
      - For Android: Run `gradle sync`.
      
3. Run the project

  Run the following command in the `examples/room-example` directory:

  ```
  yarn run ios
  // or
  yarn run android
  ```
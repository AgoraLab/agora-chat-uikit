# Integrate UIKit 

Before using UIKit for chat rooms, you need to integrate it into your application.

## Prerequisites

Before you start, make sure your development environment meets the following requirements:

- MacOS 12 or above;
- React-Native 0.66 or above;
- NodeJs 16.18 or above;
- iOS apps: Xcode 13 or above, and its dependencies.
- Android apps: Android Studio 2021 or above, and its dependent tools.
- A valid Agora project with users and tokens generated. See [Enable and configure Chat](https://docs.agora.io/en/agora-chat/get-started/enable) and [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication) for details.

## Implementation

1. Create a project or use an existing one.

    Create with the following command: 

    ```
    npx react-native init MyApp --version 0.71.11
    ```

1. Install UIKit into your project.

   Enter the project and run the following command:

   ```
   npm install react-native-chat-room
   # or
   yarn add react-native-chat-room
   # or
   npx expo install react-native-chat-room
   ```
